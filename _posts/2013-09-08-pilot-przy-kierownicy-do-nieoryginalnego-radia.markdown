---
layout: post
title:  "Pilot przy kierownicy do nieoryginalnego radia"
date:   2013-09-08 12:00:00 +0200
tags:   projects electronics cars
---

![Moduł w środku](/assets/ModulWSrodku.jpg)

Pilot przy kierownicy – wiadomo, wygodna rzecz. Nie trzeba odwracać wzroku od drogi i zdejmować rąk z kierownicy.
Zdecydowanie mi go brakowało, więc byłem wystarczająco zdeterminowany by wyposażyć w niego mój samochód.

Do wyboru miałem dwa rozwiązania: kupić oryginalne radio Renault i wyświetlacz (ten taki z zegarkiem) – całość wyszłaby
pewnie coś koło 200-300 zł, albo mogłem jakoś sprawić by oryginalny pilot zaczął działać z nierenaultowym Blaupunktem.
Wybrałem to drugie rozwiązanie bo oczywiście jest tańsze (zmieściłem się w 20 zł, nie licząc jakże cennej robocizny!),
poza tym lubię pracować przy projektach gdzie trzeba trochę pomyśleć, a w dodatku później ma się satysfakcję z dobrze
(mam nadzieję) wykonanej roboty :)

Oprócz sterowania radiem, chciałem mieć jeszcze możliwość sterowania odtwarzaniem muzyki w iPodzie/iPhonie. Działa to
tak jak przycisk na kablu w oryginalnych słuchawkach – jedno wciśnięcie to play/pauza, dwa: następna piosenka, trzy:
poprzednia. Musiałem kupić specjalną wtyczkę mini-jack 3.5 mm, ale z czterema stykami. Do wtyczki dochodzą 2 kabelki,
jeden to sygnał audio idący do radia, a drugi to sterowanie pochodzące z mojego układu.

Obraz wyraża więcej niż tysiąc słów, czyli schemat przedstawiający co chciałem zrobić:

![Schemat modułu](/assets/PilotSchemat.png)

# Test komunikacji z radiem

Na początku musiałem sprawdzić czy w ogóle da się sterować radiem z zewnątrz. Podobno część radiów jest specjalnie
przystosowana do sterowania takimi pilotami przy kierownicy, ale moje do nich nie należy. Okazało się jednak, że jest do
niego pilot na podczerwień (Blaupunkt RC-12H). Udało mi się taki znaleźć na Allegro za prawie 30 zł. Po dwóch tygodniach
czekania pilot wreszcie przyszedł – rozpakowuję, włączam radio i sprawdzam – nie działa. Przyczyna była banalna: w radiu
(na panelu przednim) nie było odbiornika podczerwieni… Był on sprzedawany osobno na kablu, do podłączenia z tyłu radia.
Oryginalny odbiornik kosztuje ok. 20 funtów, więc rzecz jasna jego kupno nie wchodziło w grę. Musiałem zrobić własny za
kilka zł – udało się, działał! Radio reagowało, więc wiedziałem, że pilot i radio się rozumieją.

# Reverse engineering :D

Skoro odbiornik podłączany jest przez odpowiednie styki z tyłu radia, to równie dobrze można zrobić układ, który
podepniemy pod te styki i będzie on symulował działanie odbiornika. Właśnie na tej zasadzie działa mój adapter. Za
pomocą komputera i fotorezystora IR podłączonego do karty dźwiękowej (czyli tak naprawdę prymitywnego oscyloskopu)
zbadałem jakie sygnały wysyła pilot. Okazało się, że protokół jest bardzo prosty:

![Przebiegi wysyłane przez pilota Blaupunkt RC-12H](/assets/PrzebiegiBialeTlo.png)

Jedna podziałka w poziomie ma ok. 300 μs. Podczas trzymania jakiegoś przycisku wysyłany jest odpowiadający mu kod, przy
czym przerwa między końcem jednego a początkiem drugiego wystąpienia kodu wynosi 100 ms. Po puszczeniu przycisku
wysyłany jest kod pokazany na samej górze.

# Ostateczny układ

Do wysyłania takich sygnałów użyłem dość popularnego mikrokontrolera ATtiny26. Za jego pomocą sprawdzam, które przyciski
są wciśnięte (lub w jakim położeniu jest rolka) i w zależności od tego mogę wysłać odpowiednią komendę. Sygnał idzie od
kontrolera do tranzystora, a on wzmacnia go tak by był na poziomie od 0 do 12V. Funkcje radia mogłem sobie przypisać do
dowolnych przycisków na pilocie; to tylko kwestia zmiany kodu programu, który swoją drogą napisałem w C.

Poniżej załączam prawdopodobnie końcową postać schematu ideowego i kodu. Robiłem to 2 lata temu, więc być może były
jeszcze jakieś zmiany od czasu robienia tego screena i backupu kodu, ale jeśli już to były bardzo niewielkie.

![Schemat ideowy](/assets/SchematIdeowy.png)

```c
#include <avr/io.h>
#include <util/delay.h>

#define PIN(x)                 (*(&x - 2))
#define DDR(x)                 (*(&x - 1))

#define REMOTE_PORT            PORTB	// port, do którego podłączone są wyprowadzenia pilota przy kierownicy
#define OUTPUT_PORT            PORTA	// port, do którego podłączone są wyjścia (do radia i iPoda)

#define FRAMETIME              600
#define IPOD_CONTROL_TIME      50
#define VOLPLUS                0x01
#define VOLMINUS               0x02

#define SIGNAL_END             3
#define SIGNAL_VOLPLUS         4
#define SIGNAL_VOLMINUS        5
#define SIGNAL_UP              6
#define SIGNAL_DOWN            7
#define SIGNAL_MUTE            8
#define SIGNAL_RIGHT           9
#define SIGNAL_LEFT            10
#define SIGNAL_SOURCE          11

#define IPOD_PLAYPAUSE         1
#define IPOD_NEXT              2
#define IPOD_PREV              3

// poszczególne wyprowadzenia pilota
#define REM_OUT_WHEEL          0x01	// (1) wspólne połączenie dla kółka z tyłu pilota
#define REM_OUT_VOL            0x02	// (5) wspólne połączenie dla przycisków głośności i zmiany tryby strojenia
#define REM_OUT_SOURCE         0x04	// (6) wspólne połączenie dla przycisków zmieniających źródło
#define REM_OUTPUTS            (REM_OUT_WHEEL | REM_OUT_VOL | REM_OUT_SOURCE)
// poniższe styki są podłączone przez rezystor podciągający do masy
#define REM_IN_VOLPLUS         0x08	// (2) wspólne połączenie dla przycisków VOL+ i jednego z sygnałów kółka
#define REM_IN_VOLMINUS        0x20	// (3) wspólne połączenie dla przycisków VOL-, SOURCE1 i jednego z sygnałów kółka
#define REM_IN_MODE            0x40	// (4) wspólne połączenie dla przycisku zmiany strojenia, SOURCE2 i jednego z sygnałów kółka
#define REM_INPUTS             (REM_IN_VOLPLUS | REM_IN_VOLMINUS | REM_IN_MODE)

// wyjścia
#define RADIO_CONTROL          0x10
#define IPOD_CONTROL           0x20

uint8_t pressed = 0;

void sendSignalToRadio(uint8_t signal)
{
	if(signal != SIGNAL_END)
		pressed |= 1;

	uint8_t i = 0;

	OUTPUT_PORT &= ~RADIO_CONTROL;

	for(i=0; i<signal; i++)
		_delay_us(FRAMETIME);

	OUTPUT_PORT |= RADIO_CONTROL;

	for(i=0; i<signal; i++)
		_delay_us(FRAMETIME);

	OUTPUT_PORT &= ~RADIO_CONTROL;

	_delay_us(FRAMETIME);

	OUTPUT_PORT |= RADIO_CONTROL;

	_delay_ms(100);
}

void sendSignalToiPod(uint8_t signal)
{
	uint8_t i = 0;

	for(i=0; i<signal; i++)
	{
		OUTPUT_PORT |= IPOD_CONTROL;
		_delay_ms(IPOD_CONTROL_TIME);

		OUTPUT_PORT &= ~IPOD_CONTROL;

		if(i != signal-1)
			_delay_ms(IPOD_CONTROL_TIME);
	}
}

int main(void)
{
	DDR(OUTPUT_PORT) = RADIO_CONTROL | IPOD_CONTROL;
	DDR(REMOTE_PORT) = REM_OUTPUTS;

	REMOTE_PORT = REM_INPUTS;
	OUTPUT_PORT = RADIO_CONTROL;

	uint8_t wheelPrev = 0, wheel = 0;

	while(1)
	{
		OUTPUT_PORT = RADIO_CONTROL;
		wheel = 0;
		pressed <<= 1;

		// ---------- obsługa zmiany głośności i trybu ----------

		REMOTE_PORT = (REMOTE_PORT & (~REM_OUTPUTS)) | REM_OUT_VOL;
		_delay_us(100);

		if(((PIN(REMOTE_PORT) & REM_IN_VOLPLUS) != 0) && ((PIN(REMOTE_PORT) & REM_IN_VOLMINUS) != 0))	// VOL+ i VOL- jednocześnie
		{
			sendSignalToRadio(SIGNAL_MUTE);
			sendSignalToRadio(SIGNAL_END);
			pressed &= 0xFE;
			sendSignalToiPod(IPOD_PLAYPAUSE);
			_delay_ms(500);
		}
		else if((PIN(REMOTE_PORT) & REM_IN_VOLPLUS) != 0)	// VOL+
			sendSignalToRadio(SIGNAL_VOLPLUS);
		else if((PIN(REMOTE_PORT) & REM_IN_VOLMINUS) != 0)	// VOL-
			sendSignalToRadio(SIGNAL_VOLMINUS);
		else if((PIN(REMOTE_PORT) & REM_IN_MODE) != 0)	// MODE
			sendSignalToRadio(SIGNAL_SOURCE);

		// ---------- obsługa klawiszy SOURCE ----------

		REMOTE_PORT = (REMOTE_PORT & (~REM_OUTPUTS)) | REM_OUT_SOURCE;
		_delay_us(100);

		if((PIN(REMOTE_PORT) & REM_IN_VOLMINUS) != 0)	// SOURCE1
			sendSignalToRadio(SIGNAL_DOWN);
		else if((PIN(REMOTE_PORT) & REM_IN_MODE) != 0)	// SOURCE2
			sendSignalToRadio(SIGNAL_UP);

		// ---------- obsługa kółka ----------

		REMOTE_PORT = (REMOTE_PORT & (~REM_OUTPUTS)) | REM_OUT_WHEEL;
		_delay_us(100);

		if(!(((PIN(REMOTE_PORT) & REM_IN_VOLPLUS) != 0 && (PIN(REMOTE_PORT) & REM_IN_VOLMINUS) != 0) ||
			((PIN(REMOTE_PORT) & REM_IN_VOLPLUS) != 0 && (PIN(REMOTE_PORT) & REM_IN_MODE) != 0) || 
			((PIN(REMOTE_PORT) & REM_IN_VOLMINUS) != 0 && (PIN(REMOTE_PORT) & REM_IN_MODE) != 0)))
		{
			if((PIN(REMOTE_PORT) & REM_IN_VOLPLUS) != 0)	// WHEEL1
				wheel = 1;
			else if((PIN(REMOTE_PORT) & REM_IN_VOLMINUS) != 0)	// WHEEL2
				wheel = 2;
			else if((PIN(REMOTE_PORT) & REM_IN_MODE) != 0)	// WHEEL3
				wheel = 3;
		}

		if(wheel != 0)
		{
			if((wheelPrev == 1 && wheel == 2) ||
				(wheelPrev == 2 && wheel == 3) ||
				(wheelPrev == 3 && wheel == 1))	// w górę
			{
				sendSignalToRadio(SIGNAL_RIGHT);
				sendSignalToiPod(IPOD_NEXT);
			}
			else if((wheelPrev == 2 && wheel == 1) ||
				(wheelPrev == 3 && wheel == 2) ||
				(wheelPrev == 1 && wheel == 3))	// w dół
			{
				sendSignalToRadio(SIGNAL_LEFT);
				sendSignalToiPod(IPOD_PREV);
			}

			wheelPrev = wheel;
		}

		if((pressed & 3) == 2)
			sendSignalToRadio(SIGNAL_END);
	}
}
```

# Kosztorys

| Element                | Cena         |
|------------------------|--------------|
| ATtiny26               | 5,50         |
| podstawka precyzyjna   | 1,60         |
| regulator 5V 7805TV    | 1,50         |
| 11 goldpinów           | 0,83         |
| wtyczki goldpinowe     | 3,00         |
| 2 x transyztor BC546   | 0,50         |
| 2 x kondensator 100 nF | 0,40         |
| 6 x rezystor 10 kOhm   | 0,60         |
| rezystor 4,7 kOhm      | 0,10         |
| obudowa                | 3,00         |
| **W SUMIE:**           | **16,53 zł** |

…plus kilka dni pracy.

W tym miejscu należą się podziękowania dla Szymona Zagórnika, który bardzo mi pomógł od strony elektroniki, głównie przy
projektowaniu płytki. Wiem, że układ jest bardzo prosty, no ale… jestem jeszcze marnym elektronikiem i chociażby wolałem
nie spalić sobie telefonu :D

Takie małe, a takie przydatne. Naprawdę sporo satysfakcji dał mi ten projekt, i nadal daje podczas jazdy autem. Całość
powstała w 2011 roku, więc jeżdżę już z nim 2 lata. Zdecydowanie nie żałuję, że podrążyłem trochę temat i zbudowałem
takie coś – było warto.

Pozdrawiam!

![Podczas testów na płytce stykowej](/assets/PilotTesty.jpg)

![Pilot PCB](/assets/PilotPcb.jpg)

![Obudowa gotowa](/assets/PilotObudowa.jpg)

![Gotowe](/assets/PilotGotowe.jpg)
