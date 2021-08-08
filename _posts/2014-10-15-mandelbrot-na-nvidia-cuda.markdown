---
layout: post
title:  "Mandelbrot Explorer, czyli fraktal na nVidia CUDA"
date:   2014-10-15 12:00:00 +0200
tags:   programming projects
---

![Screenshot](/assets/Whole-GPU.png)

Generowanie fraktali jest dość kosztowne obliczeniowo, przez co praktycznie nie da się ich przeglądać w czasie
rzeczywistym na zwykłym PC-cie z wykorzystaniem zwykłego CPU. Od czego mamy jednak karty graficzne, które ostatnimi
czasy zyskały drugie oblicze – wydajnej platformy do obliczeń równoległych.

# Co to ten cały _Mandelbrot_?

Na początku ciekawostka – po niemiecku oznacza to chleb migdałowy. Jednak w tym przypadku nazwa pochodzi od francuskiego
matematyka [Benoît Mandelbrot](http://pl.wikipedia.org/wiki/Beno%C3%AEt_Mandelbrot), który wpadł na to co o czym zaraz
dowiesz się więcej.

Mianowicie, pan Mandelbrot zajmował się fraktalami. Tak naprawdę to on wymyślił to słowo. Fraktal to pewien obiekt,
który w sensie geometrycznym jest samopodobny – jakaś jego część jest podobna do całości. Z takimi obiektami spotykamy
się w życiu codziennym, spójrzmy na przykład na drzewa:

![Fraktal](/assets/fractal-tree2.jpg)

W tym wpisie chcę jednak pokazać trochę inny typ fraktala – istnieje on tylko w świecie matematycznym. Mowa o...

# Zbiór Mandelbrota

Najpierw zaprezentuję wzór, za pomocą którego ten zbiór jest opisany:

![Wzór](/assets/MandelbrotFormula.png)

a teraz spójrzmy jaki obraz powstanie gdy w odpowiedni sposób zastosujemy ten wzór – już pewnie widziałaś/eś to w miniaturce do tego wpisu:

![Mandelbrot](/assets/Mandelbrot.png)

Fajne, co nie? Taki prosty wzór, z którego wychodzi taki skomplikowany kształt? Co to za czary? W telegraficznym 
skrócie, to arytmetyka liczb zespolonych :)

Komputery idealnie nadają się do wizualizacji takiego czegoś, bowiem wymaga to bardzo wielu obliczeń. Nawet dla
współczesnego komputera, wygenerowanie takiego obrazka może zająć kilka sekund. A co jeśli chcielibyśmy płynnie i
swobodnie poruszać się po fraktalu? Z pomocą przychodzi nam...

# nVidia CUDA

W dużym uproszczeniu, pozwalają one na wykorzystanie kart graficznych do dowolnych obliczeń, nie tylko tych związanych z
grafiką. Karty graficzne mają w sobie setki lub nawet tysiące rdzeni, w kontraście do zwykłych procesorów z kilkoma
(obecnie ok. 8) rdzeniami. Oznacza to, że jeśli na zwykłym procesorze w jednej chwili możemy liczyć 8 pikseli takiego
obrazka, to na karcie graficznej w tym samym czasie może być liczone kilkaset razy więcej porcji fraktala.

Już w liceum – gdy ta technologia weszła na rynek – chciałem się tym pobawić, ale jakoś nie miałem wystarczającej wiedzy
i motywacji. Ostatnio pojawiła się okazja aby się z tym zapoznać, w postaci przedmiotu na uczelni i projektu do
wykonania właśnie w technologii CUDA.

Powstała więc ta oto aplikacja. Króki film prezentujący większość funkcji programu:

<iframe
width="560"
height="315"
src="https://www.youtube.com/embed/9vycPjCRT_8"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
allowfullscreen
></iframe>

Program działa zarówno pod Windowsem, jak i Linuxem. Do działania wymaga jedynie zainstalowanej biblioteki GTK+ 3.0.
Jeżeli chcesz sprawdzić jak będzie działał u Ciebie, tu możesz pobrać wersję wykonywalną:

[https://github.com/krzema12/CUDA-Mandelbrot-Explorer/releases/download/v1.0/ManExWindows-2014-11-04.zip](https://github.com/krzema12/CUDA-Mandelbrot-Explorer/releases/download/v1.0/ManExWindows-2014-11-04.zip)

a kod projektu można zobaczyć tu:

[https://github.com/krzema12/CUDA-Mandelbrot-Explorer](https://github.com/krzema12/CUDA-Mandelbrot-Explorer)

Jeżeli masz pomysł na ulepszenie programu, zapraszam do współpracy :)
