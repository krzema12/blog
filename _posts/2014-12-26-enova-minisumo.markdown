---
layout: post
title:  "Enova Minisumo"
date:   2014-12-26 12:00:00 +0200
tags:   projects robots
---

![Enova](/assets/enova-1024x768.png)

Wyczerpujący opis robota turniejowego, którego zbudowałem wraz z kolegami z Magnat Cyber Forge Team, czyli z mechanikiem
Mateuszem Piotrzkowskim i elektronikiem Szymonem Zagórnikiem. Moja działka to oprogramowanie.

# Koncepcja i napęd robota

Panujący obecnie w robotach sumo trend skłania do wyboru konstrukcji z dwoma kołami napędowymi. Roboty o takiej
strukturze napędu są tanie w budowie, a przy tym zapewniają dobre przyleganie krawędzi tnącej ostrza do podłoża.
Problemem w ich przypadku jest nieoptymalne przeniesienie ciężaru robota na podłoże oraz duży moment bezwładności
robota.

Konstrukcja z czterema kołami napędowymi jest mało popularna lecz rozwiązuje wiele problemów robotów wyposażonych tylko
w dwa koła. Pierwszą zaletą jest możliwość zapewnienia przeniesienia prawie całego ciężaru robota przez opony. Kolejną
zaletą jest przesunięcie środka obrotu robota do przodu oraz łatwość wpadania w poślizg robotem. Zalety te są
nieznaczące w obliczu problemów z zamocowanym nieruchomo ostrzem. W przypadku gdy koła stanowią trzy niezbędne punkty
podparcia bardzo trudno jest zamocować ostrze z taką dokładnością aby przylegało do podłoża.

Zdecydowaliśmy się zaryzykować. Wybór padł na konstrukcję z czterema kołami oraz ruchome mocowanie ostrza – tak aby
połączyć zalety struktur napędu z dwoma i czterema kołami.

Szukając silników napędu kół robota wybór ograniczyliśmy do 3 firm oferujących jakościowo najlepsze mikrosilniki
elektryczne na świecie – firm Maxon, Faulhaber i Portescap. Głównym ograniczeniem determinującym wybór silników do
robota minisumo jest ich maksymalna długość. Zdecydowaliśmy że każde z kół będzie napędzane niezależnie, więc długość
silnika wraz z przekładnią i enkoderem nie mogła przekroczyć 50 mm. Po przejrzeniu ofert wymienionych firm okazało się
że najmocniejszą konfiguracją o wymaganych wymiarach są silniki Faulhaber 1717 006 SR wraz z przekładnią 16/7 14:1 oraz
enkoderami IE2. Silniki te są marginalnie mocniejsze od popularnie stosowanych mikrosilników firmy Pololu, lecz mają
wielokrotnie większą żywotność. Zdarzały nam się sytuacje całkowitego zatrzymania wszystkich 4 silników na regulaminowe
3 minuty walki a silniki dalej pracują jak nowe.

# Mechanika

Konstrukcja robota składa się z 11 elementów. Siedem z nich zostało zoptymalizowanych pod kątem wykonania metodami
skrawania z aluminium, stali oraz węglika wolframu. Pozostałe 4 elementy zostały wydrukowane w technologii PolyJet.

Głównym elementem konstrukcyjnym jest płyta centralna. Przykręcone do niej są: tulejki mocujące silniki, element
mocujący ostrze, mocowania czujników robota przeciwnika oraz płytka drukowana sterująca robotem. Poniżej znajduje się
jeden z siedmiu rysunków wykonawczych tego elementu.

![Enova płytka](/assets/EnovaPlytka.png)

Element mocujący ostrze wykonano ze stali 40 HM utwardzonej do wartości 50 HRC. Dzięki temu wypolerowana powierzchnia
trudno się rysuje, a przeciwnicy gładko prześlizgują się na zamocowane wyżej rogi. Element ten posiada oś obrotu oraz
jest dociskany dwoma sprężynami – niezależnie z każdej strony. Sprawia to że ostrze dociśnięte jest równomiernie do
podłoża.

Ostrza które stosujemy są wykonane dla nas na zamówienie z węglika wolframu. Poniżej zdjęcie porównawcze tego ostrza ze
starą wersją zintegrowaną z mocowaniem do płyty centralnej.

![Enova ostrze](/assets/EnovaOstrze.jpg)

Felgi robota mają średnicę 24 mm. Posiadamy kilka kompletów z poliamidu i kilka z aluminium. Nie zauważyliśmy większej
różnicy w działaniu robota na różnych kompletach. Opony mają zewnętrzną średnicę 30 mm co daje grubość opon równą 3mm.

Przetestowaliśmy wiele materiałów na opony od firmy Smooth-On: Mold Star 15, Mold Star 30, Mold Max 20, Vyta Flex 10,
Vyta Flex 30, Vyta Flex 20. Ten ostatni wykazał najlepsze własności mechaniczne i jest przez nas aktualnie stosowany. Do
mieszanki dodajemy czarny barwnik, co zwiększa nieco nieco lepkość opon.

Waga robota waha się w zależności od zastosowanych felg i baterii od 460 do 495 gramów.

# Elektronika

Płyta sterowania robota to laminat FR4 o grubości 0.8mm. By spełnić wymagania związane z fizycznym rozmieszczeniem
układów na płytce, wykorzystaliśmy technologię produkcyjną umożliwiającą nam użycie ścieżek o szerokości 0.2mm oraz
rastrze podobnej szerokości. Znajdujące się na płytce układy zapewniają prawidłową pracę robota w trybie domyślnym tj.
walka na ringu (bez opcji wybierania taktyki i różnych trybów). Program robota wykonywany jest przez wcześniej
wspomniany 32-bitowy mikrokontroler ST32F107R8T6. Został on wybrany ze względu na aż cztery dostępne timery, które są
niezbędne do obsługi sygnałów z enkoderów silników. Na płycie sterowania znajdują sie również cztery mostki H, złącza od
czujników przeciwnika sześć transoptorów i dwa stabilizatory napięcia na 3.3V i 5V.

![Enova PCB](/assets/EnovaPCB.png)

Moduł interakcji użytkownika z robotem stanowi płytka drukowana umieszczona pod górną pokrywą robota. Jest ona w
znacznym stopniu mniej skomplikowana od płytki sterującej, niemiej jednak zawiera ona oddzielny mikroprocesor
STM32F103CVT. Obsługuje on wyświetlacz OLED o rozdzielczości 128×64 piksele poprzez interfejs SPI. Wykrywa on również
sygnał z zmieszczonego na płytce odbiornika podczerwieni oraz czterech przycisków stykowych. Dzięki przyciskom lub
bezprzewodowo z pomocą pilota RC5 użytkownik jest w stanie włączyć robota i wybrać algorytm pracy robota wyświetlany na
wyświetlaczu OLED. Komunikacja między górnym i dolnym mikroprocesorem odbywa się za pomocą interfejsu UART. Obie płytki
drukowane są połączone ze sobą taśmą FFC.

# Zasilanie

Zastosowaliśmy dwukomorową baterię litowo-polimerową, która przy maksymalnym naładowaniu daje 8.4V. Nie wyróżnia się ona niczym szczególnym poza rozmiarem i kształtem, który jest idealnie dopasowany do konstrukcji robota i wewnętrznego rozmieszczenia elementów. Pojemność baterii to 900 mAh.

# Czujniki

Sensorami umożliwiającymi wykrywanie przeciwnika są popularne Sharpy GP2Y0D340K. Są to czujniki IR o zasięgu do 40 cm. Są cyfrowe, tj. wskazują pojawienie się obiektu lub jego brak. W środku sensora umieszczony jest układ cyfrowy kondycjonujący otrzymaną wiązkę podczerwieni oraz emitujący cyfrowy sygnał wykrycia do mikrokontrolera. Zasilany jest wyłącznie 5V więc sygnał Vout również jest na tym poziomie. Fakt ten nie komplikuje układu, gdyż użyty mikrokontroler ma wejścia kompatybilne z 5V mimo, że sam jest zasilany na 3.3V. W celu skutecznego wykrywania przeciwnika umieszczono aż 6 sensorów ułożonych w konfiguracji: 2 patrzące do przodu, 2 pod skosem 45 stopni oraz 2 patrzące na boki robota.

# Jednostki obliczeniowe

Dolna płytka zawiera główny procesor STM32F107 taktowany zegarem o częstotliwości 72 MHz. Zajmuje się przetwarzaniem informacji z czujników linii oraz przeciwnika, a także wysterowuje silniki korzystając z wbudowanych fabrycznie enkoderów magnetycznych i regulatora PID. Na górnej płytce znajduje się drugi procesor STM32F103 (również 72 MHz) zajmujący się obsługą wejścia/wyjścia czyli przycisków, wyświetlacza i wbudowanego odbiornika podczerwieni. Taki podział zadań pomiędzy dwa procesory wydaje się zbędny/przesadzony, lecz jest to pozostałość z pierwszych wersji robota.

# Programowanie

Do komunikacji z robotem użyto dwóch interfejsów: SWD (Serial Wire Debug) – szeregowa odmiana popularnego interfejsu JTAG. Umożliwia on programowanie oraz debugowanie mikrokontrolera sterującego. Do komunikacji między dwoma modułami (mikroprocesorami) oraz komunikacji komputer – mikroprocesor użyto interfejsu UART. Fizyczne połączenie robota z komputerem stanowi złącze MicroHDMI. Zostało ono wybrane ze względu na liczbę dostępnych pinów oferowane przez standard HDMI, trwałość mechaniczną i dostępność. Medium łączące wymienione interfejsy i komputer jest przejściowa płytka drukowana z standardowymi złączami dla programatorów. Rysunek poniżej przedstawia omawianą „przejściówkę”.

![Enova przejściówka](/assets/EnovaPrzejsciowka.png)

# Oprogramowanie

Program dla robota pisany jest w języku C, z użyciem _Standard Peripheral Library_. W newralgicznych miejscach, aby
przyśpieszyć kod, wykonywane są operacje bezpośrednio na rejestrach – dzięki temu został zachowany balans między
czytelnością a wydajnością kodu. Jeśli chodzi o IDE, początkowo wykorzystywane było TrueSTUDIO Lite, lecz ze względu na
swój limit wielkości programu wynikowego (32 kB?) byliśmy zmuszeni do przejścia na CoIDE.

**Silniki** wysterowywane są regulatorem PID, lecz tak naprawdę tylko człon P jest aktywny. Zaimplementowany został
pełny regulator PID, lecz okazało się, że na członie P robot zachowuje się dobrze i nie ma potrzby uaktywniać
pozostałych współczynników. Magnetyczne enkodery wbudowane w silnik dostarczają aż 7168 impulsów na obrót, co zapewnia
wysoką dokładność. Został wykorzystany wbudowany w STM32F107 interfejs dla enkoderów, dzięki czemu zliczanie impulsów
sygnału kwadraturowego odbywa się niejako „w osobnym wątku” – nie są potrzebne przerwania, wystarczy odczytywać
położenie koła ze wskazanego rejestru.

Dzięki **czterem czujnikom linii** z przodu, możemy obliczyć w przybliżeniu kąt pod jakim podjeżdżamy do linii.
Wystarczy prosta trygonometria – znamy odległość między czujnikami, znamy też dystans jaki robot przejechał od momentu
zobaczenia linii zewnętrznymi czujnikami do momentu zauważenia jej czujnikami wewnętrznymi. Znając kąt pomiędzy osią
robota a linią, możemy odpowiednio wykonać manewr wycofania się ku środkowi planszy.

Odbiornik podczerwieni na górnej płytce służy nam głównie do odbierania sygnałów z pilotów sędziowskich. Zastosowaliśmy
własny odbiornik przede wszystkim aby zaoszczędzić na miejscu, lecz również aby móc odbierać niestandardowe komendy dla
robota, np. wskazówki dotyczące taktyki. Dzięki aplikacji
[Sumo Remote](https://play.google.com/store/apps/details?id=eu.mcft.sumoremote&hl=en_US&gl=US) możemy wysyłać w zasadzie
dowolny kod do robota.

Monochromatyczny wyświetlacz OLED o rozdzielczości 128×64 umieszczony na górnej płytce umożliwia wybór taktyk, a także
wyświetlanie informacji testowych. Nie posiada on wbudowanej czcionki, więc konieczna była implementacja wyświetlania
własnych czcionek/obrazków. Aby zaoszczędzić miejsce w pamięci Flash procesora, obrazy kompresowane są algorytmem RLE za
pomocą konwertera w postaci aplikacji na PC, po czym na robocie dekompresowane są w locie. Dzięki temu możemy użyć
dużych i czytelnych czcionek, czy też schematycznych ikon obrazujących manewry robota. Krótkie demo działania
wyświetlacza: [https://www.youtube.com/watch?v=ttC8FtYK7fY](https://www.youtube.com/watch?v=ttC8FtYK7fY) (muzyka
niestety nie jest odtwarzana z robota :D).

Po więcej szczegółów zapraszamy na naszą stronę:
[http://mcft.eu/_portal/page/knowledge_base/cat/6/Enova_MiniSumo#art_Enova_MiniSumo](http://mcft.eu/_portal/page/knowledge_base/cat/6/Enova_MiniSumo#art_Enova_MiniSumo). 

# Osiągnięcia

## Sezon 2013/2014

* 1. miejsce na Roboxy 2014 w kategorii Minisumo
* 1. miejsce na Trójmiejskim Turnieju Robotów 2014 w kategorii Minisumo Deathmatch
* 3. miejsce na Trójmiejskim Turnieju Robotów 2014 w kategorii Minisumo
* 1. miejsce na Cyberbot 2014 w kategorii Minisumo Deathmatch
* 2. miejsce na Cyberbot 2014 w kategorii Minisumo
* 1. miejsce na Robot-SM 2014 w Göteborgu w kategorii Minisumo
* 1. miejsce na Ogólnopolskich Zawodach Robotów ROBOmotion 2014 w kategorii Minisumo
* 1. miejsce na Ogólnopolskich Zawodach Robotów ROBOmotion 2014 w kategorii Minisumo Deathmatch
* 1. miejsce na Robotic Tournament 2014 w kategorii Minisumo
* 1. miejsce na Robotic Tournament 2014 w kategorii Minisumo Deathmatch
* 1. miejsce na Copernicus Robots' Tournament 2014 w kategorii Minisumo
* 1. miejsce na Copernicus Robots' Tournament 2014 w kategorii Minisumo Deathmatch
* 1. miejsce na Robotic Arena 2013 w kategorii Minisumo Debel (z robotem Mirror Mateusza Paczyńskiego)
* 2. miejsce na Robotic Arena 2013 w kategorii Minisumo Enhanced
* 2. miejsce na Robotic Arena 2013 w kategorii Minisumo Classic
* 2. miejsce na Sumo Challenge 2013 w kategorii Minisumo+
* 1. miejsce na Sumo Challenge 2013 w kategorii Minisumo
* 1. miejsce w konkursie robotów Gdańskich Dni Elektryki Młodych 2013 w kategorii Minisumo

## Sezon 2012/2013


* 1. miejsce na Leś-Tech 2013 w kategorii Minisumo
* 1. miejsce na Roboxy 2013 w kategorii Minisumo
* 1. miejsce na CybAiRBot 2013 w kategorii Minisumo
* 1. miejsce na Robocomp 2013 w kategorii Minisumo
* 1. miejsce na ROBOmotion 2013 w kategorii Minisumo Deathmatch
* 1. miejsce na ROBOmotion 2013 w kategorii Minisumo
* 1. miejsce na Copernicus Robots Tournament 2013 w kategorii Minisumo Deathmatch
* 1. miejsce na Copernicus Robots Tournament 2013 w kategorii Minisumo
* 3. miejsce na Trójmiejskim Turnieju Robotów 2013 w kategorii Sumo²
* 1. miejsce na Trójmiejskim Turnieju Robotów 2013 w kategorii Minisumo
* 1. miejsce na Robotic Tournament 2013 w kategorii Minisumo
* 1. miejsce na RobotChallenge 2013 w Wiedniu w kategorii Minisumo
* 1. miejsce na Robomaticon 2013 w kategorii Minisumo
* 1. miejsce na T-BOT 2013 w kategorii Minisumo
* 1. miejsce na Robotic Arena 2012 w kategorii Minisumo Enhanced
* 2. miejsce na Robotic Arena 2012 w kategorii Minisumo

Poniższy film prezentuje walki Enovy na konkursie RobotChallenge 2013 w Wiedniu: 

<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/QZK6KX8qj-A"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
></iframe>

A tutaj kompilacja walk Enovy z konkursu Robocomp 2013 w Krakowie:

<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/uHw2QkmQ-qI"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
></iframe>
