---
layout: post
title:  "Jaki los nas czeka, czyli coś o klasie Random!"
date:   2022-11-13 17:41:28 +0100
categories: jekyll update
feature_image: feature-image.jpg
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
#

![useful image](https://github.com/WJarze/Wjarze.github.io/blob/main/_site/assets/ConditionRandomGenerator.png)

#

## Add images

Create an `assets` folder where you can put all your images, 
then display them with a link starting with an exclamative mark like this: 
`![my inspiring image]({{ "/assets/sample-image.jpg" | relative_url }})`.

![my inspiring image]({{ "/assets/sample-image.jpg" | relative_url }})
_Photo by [Ian Schneider](https://unsplash.com/@goian)_
