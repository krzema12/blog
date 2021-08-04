---
layout: post
title:  "Echo LED Simulator"
date:   2016-12-16 12:00:00 +0200
tags:   programming projects
---

![Animacja](/assets/Rainbow.gif)

Co by było gdyby... [Amazon Echo](https://www.youtube.com/watch?v=KkOCeAtKHIc), oprócz kilku standardowych animacji,
udostępniało możliwość sterowania jego LED-ami w dowolny sposób? Można puścić wodze fantazji i zaprogramować ciekawe
animacje za pomocą tego małego frameworka.

Kto wie, może twórcy Echo kiedyś udostępnią taką możliwość i ten prosty proof of concept stanie się emulatorem
rzeczywistego urządzenia.

Projekt zrobiony dosłownie w weekend, w nagłym przypływie wolnego czasu i z chęcią poćwiczenia AngularJS i Less
(preprocesora CSS). Najbardziej dumny jestem paradoksalnie ze kodu w Less, w którym udało mi się w ciekawy,
parametryczny sposób opisać
[rozmieszczenie diód](https://github.com/krzema12/EchoLedSimulator/blob/master/styles.less#L83-L126) oraz z ogólnego,
dość realnego wyglądu symulatora, co jest możliwe dzięki zastosowaniu masek i odpowiedniego trybu mieszania kolorów
(_blendingu_).

Projekt na GitHubie: [https://github.com/krzema12/EchoLedSimulator](https://github.com/krzema12/EchoLedSimulator)
