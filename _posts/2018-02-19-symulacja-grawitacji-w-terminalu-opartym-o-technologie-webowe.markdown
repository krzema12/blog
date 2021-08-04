---
layout: post
title:  "Symulacja grawitacji w terminalu opartym o technologie webowe"
date:   2018-02-19 12:00:00 +0200
tags:   programming projects
---

![DemoScreenshot](/assets/HypergravityDemoScreenshot.png)

Gdy prowadzisz właśnie wykład i wyświetlasz swój ekran setkom ludzi, ta sztuczka może na nowo zwrócić ich uwagę w Twoją
stronę :)

Framework [Electron](https://electronjs.org/), umożliwiający tworzenie aplikacji desktopowych z wykorzystaniem HTML, JS
i wszelkich innych technologii webowych, doczekał się kolejnego projektu w nim stworzonego: Hyper. Jest to emulator
terminala umożliwiający modyfikowanie i rozszerzanie w wyjątkowy sposób, a mianowicie w gruncie rzeczy mamy dostęp do
drzewa DOM tego co pokazuje główne okno, którego zawartość to po prostu dokument HTML. Gdy zdałem sobie z tego sprawę,
postanowiłem puścić wodze fantazji i już po kilku dniach mogłem bawić się tym:

![Animacja](https://user-images.githubusercontent.com/3110813/35337934-d11d8afc-011c-11e8-9131-6c0bf23ec669.gif)

Jest to wtyczka do wyżej wspomnianego terminala. Wykorzystałem silnik fizyki [Matter.js](http://brm.io/matter-js/) by
policzyć kolizje. Wydajność jest akceptowalna –  przy mało skomplikowanej zawartości terminala animacja jest płynna,
dopiero przy wielu drobnych elementach widać wyraźnie zacinanie. Kod nie jest jeszcze zoptymalizowany pod wydajność,
więc zdecydowanie jest to obszar do rozwoju :)

Plugin nazywa się **hypergravity** i każdy może go zainstalować w swoim terminalu ponieważ jest
[dostępny w npm](https://www.npmjs.com/package/hypergravity). Zapraszam również do zajrzenia do kodu i być może
dorzucenia czegoś od siebie? Repo na GitHubie:

[https://github.com/krzema12/hypergravity](https://github.com/krzema12/hypergravity)
