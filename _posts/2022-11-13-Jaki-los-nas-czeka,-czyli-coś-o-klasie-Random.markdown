---
layout: post
title:  "Jaki los nas czeka, czyli coś o klasie Random!"
date:   2022-11-13 17:41:28 +0100
categories: jekyll update
---
 Ile razy wspominamy o zrządzeniach losu jakie nas spotkały, ile rzeczy sobie tym tłumaczymy.
Ten wpis będzie własnie o tym jak możemy symulować ten los w Javie. 
 Pomine wtęp teoretyczny odsyłając do odpowiednich stron
 
[Losowość](https://pl.wikipedia.org/wiki/Losowo%C5%9B%C4%87).

[Liczba losowa](https://pl.wikipedia.org/wiki/Liczba_losowa).

[Generator liczb pseudolosowych](https://pl.wikipedia.org/wiki/Generator_liczb_pseudolosowych).

[Dokumentacja klasy Random](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html).

Tak więc zacznijmy od klasyka czyli loterii liczbowych jednak rozwiązanie które przedstawie będzie 
bardziej elastyczne niz klasyczny totto lotek.
A żeby tak sie stalo na początek poczynimy niezbędne założenia:
- chcemy mieć mozliwość określenia przedziału z jakiego liczby będą losowane
- oraz ilość losowanych liczb
- natomiast nie chcemy aby się powtarzały

Przykłądowe rozwiązenie wykorzystujące Stream API z Javy.


`![ConditionRandomGenerator](https://github.com/WJarze/Wjarze.github.io/blob/main/assets/ConditionRandomGenerator.jpg).`
![my inspiring image]({{ "/assets/ConditionRandomGenerator.jpg" | relative_url }})



