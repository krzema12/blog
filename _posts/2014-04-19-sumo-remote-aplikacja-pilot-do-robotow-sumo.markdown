---
layout: post
title:  "Sumo Remote – aplikacja-pilot do robotów sumo"
date:   2014-04-19 12:00:00 +0200
tags:   programming projects phones robots
---

![SumoRemote okładka](/assets/SumoRemoteCover.png)

Jest to aplikacja na Androida wykorzystująca wbudowany w urządzenie nadajnik podczerwieni do wydawania poleceń robotom
typu sumo. W skrócie: jeśli mamy odpowiednie urządzenie, aplikacja pozwala zaoszczędzić kilkadziesiąt euro za gotowego
pilota.

# Skąd się wzięły takie piloty?

Od samego początku istnienia konkurencji typu sumo istniał podstawowy problem: **falstarty**. Każdy z dwóch uczestników
danej walki stawiał robota na planszy i na znak sędziego operatorzy wciskali na robocie przycisk mówiący aby od tej
chwili robot czekał **5 sekund zanim zacznie walkę**. Od razu widzimy, że ogromną rolę odgrywa tu czynnik ludzki – jedni
z nas mają szybszą reakcję od innych. Taki sposób startu zdecydowanie ułatwiał też oszustwo. Nikt na konkursie nie
zagląda do kodów robotów i nie sprawdza czy rzeczywiście robot czeka 5 sekund od momentu wciśnięcia przycisku.

Aby temu zaradzić, kilka lat temu powstał system startowy składający się z dwóch elementów: modułu podłączanego do
robota oraz pilota wysyłającego odpowiednie komendy do modułu.

![Moduł startowy](/assets/StartModule.jpg)

Pewnie pomyślicie: gdzie tu przewaga nad zwykłym przyciskiem startującym? Oba roboty biorące udział w walce muszą być
wyposażone w taki moduł, więc **dostaną sygnał startu jednocześnie***. Na poniższym filmiku w 3:34 widać jak sędzia
uruchamia oba roboty jednym wciśnięciem przycisku na pilocie:

<iframe
width="560"
height="315"
src="https://www.youtube.com/embed/XLYTW651Gek"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
allowfullscreen
></iframe>


Dodatkowe funkcje pilota to możliwość zatrzymania obu robotów oraz możliwość wskazania, do której planszy mają być
wysłane komendy. Do wyboru jest ok. 30 adresów plansz. Przykład: załóżmy, że na konkursie mamy dwie plansze gdzie
jednocześnie odbywają się walki. Diody na podczerwień mają zasięg nawet rzędu kilkunastu metrów, więc ryzyko zakłócania
się nawzajem jest bardzo duże. Aby temu zapobiec, wystarczy ustalić adresy plansz na np. 1 i 2, dzięki czemu roboty
przypisane do planszy nr 1 będą odbierały komunikaty przeznaczone tylko dla niej, i na odwrót.

# Jaki problem rozwiązuje moja aplikacja?

Przede wszystkim służy ona jako potencjalnie tańsza alternatywa dla tradycyjnych, sprzętowych pilotów. Gotowe układy
kosztują na konkursach od 10 do 15 euro, a własnoręczne wykonanie takiego pilota również kosztuje, pochłania też nasz
cenny czas. Z mojej darmowej aplikacji możesz korzystać jeśli masz telefon z wbudowanym nadajnikiem podczerwieni (w
praktyce, na tę chwilę: wszystkie Samsungi do Androida 4.3).

![SumoRemote - główny ekran](/assets/SumoRemoteMainScreen.png)

Aplikację możesz pobrać tu:

[https://play.google.com/store/apps/details?id=eu.mcft.sumoremote](https://play.google.com/store/apps/details?id=eu.mcft.sumoremote)

Zapraszam też na fanpage gdzie na bieżąco informuję o postępach i ewentualnych problemach jakie napotykam. Jeśli masz
pomysł na ciekawą funkcję do aplikacji, też możesz śmiało o tym napisać na tablicy – będzie można wspólnie podyskutować:

[https://www.facebook.com/pages/Sumo-Remote/243607875842316](https://www.facebook.com/pages/Sumo-Remote/243607875842316)

Szczegółowy przegląd funkcji aplikacji:

<iframe
width="560"
height="315"
src="https://www.youtube.com/embed/k6_vpwcsh7s"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
allowfullscreen
></iframe>
