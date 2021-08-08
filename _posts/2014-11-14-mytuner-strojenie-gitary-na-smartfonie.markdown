---
layout: post
title:  "myTuner – strojenie gitary na smartfonie"
date:   2014-11-14 12:00:00 +0200
tags:   programming projects phones
---

![Aplikacja myTuner](/assets/myTuner.png)

Jeden z projektów, które zrealizowałem podczas kilkuletniej przygody z iPhonem i iOS-em. Projekt porzuciłem w połowie
2012 roku, ale co pewien czas sobie o nim przypominam. Bardzo możliwe, że kiedyś go wznowię.

Przede wszystkim dzięki temu projektowi nauczyłem się trochę programować w Objective-C i iOS SDK. Poza światem Apple te
narzędzia nie są wykorzystywane, ale mimo to cieszę się z tej „przygody”. Dzięki tym umiejętnościom przez przypadek
dostałem fajną pracę jako programista iOS, czyli niespodziewanie zacząłem zarabiać dzięki umiejętnościom i doświadczeniu
zdobytym w ramach hobby :) Ale dobra, wracamy do projektu...

# Algorytm

Jakie są wymagania algorytmu rozpoznawania wysokości dźwięku?

* **wejście**: fala dźwiękowa wyłapana przez mikrofon w urządzeniu. Częstotliwość próbkowania rzędu 20-40 kHz. Po
  eksperymentach doszedłem do wniosku, że 22050 Hz było wystarczające. Co do ilości próbek – czym ich więcej tym
  dokładniejszy wynik dostaniemy, ale też obliczenia trwają dłużej. W zastosowanym algorytmie nagranie musi mieć długość
  co najmniej 2 okresów najniższego wykrywalnego dźwięku (przyjąłem 36 Hz, D1 w strojeniu Drop D), co narzuca długość
  wejścia ok. 55 ms, czyli ok. 1225 próbek,

* **wyjście**: częstotliwość granej nuty – to się chyba profesjonalnie nazywa _częstotliwość bazowa_ – w hercach, czym
  dokładniej tym lepiej. Dobrze też jeżeli algorytm potrafi stwierdzić jak bardzo dana częstotliwość wyłania się z
  szumu, czyli jeżeli nie jest grana żadna nuta, powinien to wykryć jako szum i zwrócić niski współczynnik czystości 
  dźwięku (ang. _clarity_).

Jest naprawdę sporo algorytmów, które to realizują, ale można wyłonić przynajmniej dwa podejścia/nurty, na których te
algorytmy bazują:

* **analiza w dziedzinie częstotliwości** – szybka transformata Fouriera (FFT). Myśląc o szukaniu częstotliwości 
  bazowej, jest to pewnie pierwszy algorytm, który przyjdzie nam do głowy. Traktujemy sygnał za pomocą FFT, szukamy
  peaków (maksimów funkcji) i jakimś cudownym algorytmem wybieramy peak, który odpowiada szukanej częstotliwości
  bazowej. Niestety, w tym podejściu bardzo łatwo jest o błąd typu _wrong octave_ – algorytm może zwrócić największy
  peak odpowiadający częstotliwości, która według naszego ucha wcale nie jest częstotliwością bazową. Kolejną wadą
  algorytmu jest niska dokładność dla niskich tonów. Wymowny zrzut ekranu z autorskiego narzędzia pomocniczego,
  prezentujący opisane sytuacje:

  ![Analiza](/assets/MyTunerAnalysis.png)

* **analiza w dziedzinie czasu**, czyli wszelkiej maści funkcje autokorelacji. Ich główną zaletą jest z kolei wysoka
  dokładność dla niskich tonów, więc z tego punktu widzenia stanowią dobrą alternatywę dla FFT.

Ostatecznie zaimplementowałem algorytm wykorzystujący oba te podejścia. Doskonale się dopełniają – eliminują nawzajem
swoje wady. W praktyce najwięcej problemów sprawiło wybieranie odpowiedniego peaku. W wyniku wielu eksperymentów udało
mi się uzyskać algorytm, który jakoś z tym sobie radzi, lecz mimo to sprawa wymagałaby solidnego dopracowania.

# Interfejs użytkownika

![GUI](/assets/MyTunerGUI.png)

W całości zaprojektowany i wykonany przeze mnie. Postawiłem na prostotę i intuicyjność, jednocześnie dostarczając
użytkownikowi sporo informacji w efektowny sposób. Całość ma przypominać sprzętowy tuner. Nie wzorowałem się na żadnym
konkretnym modelu – narysowałem to co mi siedziało w wyobraźni.

Obszar ze wskazówką, zajmujący najwięcej miejsca, wskazuje odchylenie od „czystej” nuty od -50 do 50 centów i ma
przypominać dawne woltomierze czy amperomierze (na przykład
[taki](http://upload.wikimedia.org/wikipedia/commons/e/e3/Woltomierz_lab.jpg)). Pola wyświetlające wartości liczbowe
stylizowane są na wyświetlacze ciekłokrystaliczne. Pod wyświetlaczami znajduje się sporo wolnego miejsca – miała się tam
znajdować wizualizacja danej nuty na gryfie gitary.

# Program w akcji

<iframe
width="560"
height="315"
src="https://www.youtube.com/embed/M8IWh8Z-SrM"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
allowfullscreen
></iframe>

# Co dalej?

Możliwe, że kiedyś wznowię projekt i przepiszę tuner na Androida. Mam już gotowe sporo elementów interfejsu oraz kwestii
backendowych (algorytmy), więc nie powinno to wymagać dużo pracy.
