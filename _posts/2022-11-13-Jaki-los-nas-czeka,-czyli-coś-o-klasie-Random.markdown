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
A żeby tak sie stalo na początek poczynimy niezbędne założenia. I tak:
- chcemy mieć mozliwość określenia przedziału z jakiego liczby będą losowane
- oraz ilość losowanych liczb
- natomiast nie chcemy aby się powtarzały

Spełnieniem powyższych warunków jest ponizsza klasa:
```js
package random.RandomNumbersGenerator;

public class NumbersConditions {
    private int origin;
    private int bounds;
    private int size;

    public NumbersConditions(int size , int origin , int bounds) {
        this.size = size;
        this.origin = origin;
        this.bounds = bounds;
    }

    public int getOrigin() {
        return origin + 1;
    }

    public int getBounds() {
        return bounds + 1;
    }

    public int getSize() {
        return size;
    }
}

```

W tej klasie dzieje się magia
```js
package random.RandomNumbersGenerator;

import java.util.List;
import java.util.Random;
import java.util.stream.IntStream;

public class Generator {
    private final Random rand;

    public Generator(Random rand) {
        this.rand = rand;
    }

    public List<Integer> randomNumbers(NumbersConditions numbersConditions) {
        return IntStream.generate ( () -> {
                    return (rand.nextInt ( numbersConditions.getOrigin ( ) , numbersConditions.getBounds ( ) ));
                } )
                .distinct ( )
                .limit ( numbersConditions.getSize ( ) )
                .boxed ( )
                .toList ( );
    }

    void showRandomNumbers(List<Integer> list) {
        for (Integer numbers : list) {
            System.out.println ( numbers );
        }
    }
}

```

```js
package random.RandomNumbersGenerator;

import java.util.Random;

public class Main {
    public static void main(String[] args) {
        NumbersConditions numbersConditions = new NumbersConditions ( 6 , 0 , 49 );
        Random rand = new Random ( );
        Generator generate = new Generator ( rand );
        generate.showRandomNumbers ( generate.randomNumbers ( numbersConditions ) );
    }
}

```


