---
layout: post
title:  "Neon Plot czyli rysowanie funkcji w Pythonie"
date:   2015-02-20 12:00:00 +0200
tags:   programming projects
---

![Zrzut ekranu](/assets/Neon-Plot-1.0-1024x626.png)

Aplikacja pozwala wyrysować w zasadzie dowolną funkcję jednej zmiennej, jeśli da się ją wyrazić w Pythonie. Projekt
uczelniany z potencjałem na naprawdę ciekawą i użyteczną aplikację.

W zasadzie już w tym momencie aplikacja spełnia swoje zadanie – przewidziana funkcjonalność działa całkowicie. Siła
programu tkwi w fakcie iż wzory podawanych funkcji należy wyrażać w skryptowym języku Python. Język ten udostępnia
pewien mechanizm, dzięki któremu można obliczać wartość wyrażeń w Pythonie podczas działania programu. W tradycyjnych
językach kompilowanych (np. C), aby osiągnąć identyczny efekt (podawanie funkcji w C), należałoby wbudować w program
kompilator. Python dzięki swej skryptowej naturze udostępnia nam taki mechanizm na „dzień dobry”.

# Opisywanie funkcji – podstawy

I tak na przykład proste funkcje matematyczne typu _f(x) = 2x + 8_ w programie należy podać w identyczny sposób,
pamiętając jednak o niejawnym mnożeniu 2 i x (Python nie zna takiego naturalnego zapisu – trzeba mu jasno podać, że tam
jest mnożenie). A więc będzie to

```
2*x + 8
```

Mamy również dostęp do pełnej biblioteki matematycznej „math”, która zawiera chociażby funkcje trygonometryczne. Ciekawą
opcją jaką dostajemy dzięki Pythonowi jest umieszczanie we wzorze funkcji instrukcji warunkowych czy też prostych pętli.
Instrukcje warunkowe pozwalają nam na określenie _przedziałami_ jak wygląda dana funkcja, a więc przykładowo funkcja
pokazana na powyższym zrzucie ekranu w kolorze czerwonym jest opisana za pomocą kodu

```
sin(x) if x < 0 else (x if x < 3 else 3)
```

Na pierwszy rzut oka ciężko się połapać co się dzieje ponieważ temu zapisowi daleko do zapisu matematycznego typu
_„funkcja1 gdy x należy do przedziału1, funkcja2 gdy x należy do przedziału 2”_ itd.. Należy się przyzwyczaić do
nietypowej wersji instrukcji warunkowej, która wygląda tak: co_jeśli_prawda *if* warunek *else* co_jeśli_fałsz.

# Poruszanie się po wykresie

* **poruszanie kursorem przy wciśniętym lewym klawiszu myszki** – zmiana zakresu wyświetlanych wartości na obu osiach,
  czyli po prostu intuicyjne przemieszczanie się w poziomie i w pionie wykresu,
* **rolka myszki** – przybliżanie i oddalanie jednocześnie obu osi,
* **rolka myszki przy przytrzymanym lewym klawiszu myszki** – przybliżanie i oddalanie na osi Y,
* **rolka myszki przy przytrzymanym prawym klawiszu myszki** – przybliżanie i oddalanie na osi X.

Program ma naprawdę ogromne możliwości, a wiele z nich zawdzięcza Pythonowi. Jeżeli chcesz go wypróbować lub pomóc przy
rozwijaniu, zapraszam do repozytorium:

[https://github.com/krzema12/Neon-Plot](https://github.com/krzema12/Neon-Plot)
