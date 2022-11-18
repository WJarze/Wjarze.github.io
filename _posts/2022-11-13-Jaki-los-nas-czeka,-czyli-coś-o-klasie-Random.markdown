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
        return origin;
    }

    public int getBounds() {
        return bounds + 1;
    }

    public int getSize() {
        return size;
    }
}

```

W kolejnej klasie dzieje się magia, jedyna metoda która przyjmuje warunki ```(NumbersConditions numbersConditions)```, 
<br> generuje wykorzystując lambdę nieskończony strumień liczb dla podanego przedziału
<br>```  IntStream.generate ( () -> {return (rand.nextInt ( numbersConditions.getOrigin ( ) , numbersConditions.getBounds ( )));})```,
<br>eliminuje powtórzenia ```distinct ( )``` , 
<br>pobiera określoną w warunkach liczbę elementów ```limit ( numbersConditions.getSize ( ) )```,
<br>opakowuje strumień ```boxed ( )```,
<br>zwraca jako listę ```toList ( )``` którą następnie drukuje ```forEach ( System.out::println );```
 
```js
package random.RandomNumbersGenerator;

import java.util.Random;
import java.util.stream.IntStream;

public class Generator {
    private final Random rand;

    public Generator(Random rand) {
        this.rand = rand;
    }

    public void randomNumbers(NumbersConditions numbersConditions) {
        IntStream.generate ( () -> {
                    return (rand.nextInt ( numbersConditions.getOrigin ( ) , numbersConditions.getBounds ( ) ));
                } )
                .distinct ( )
                .limit ( numbersConditions.getSize ( ) )
                .boxed ( )
                .toList ( )
                .forEach ( System.out::println );
    }
}

```

I ostatecznie klasa z metodą main
```js
package random.RandomNumbersGenerator;

import java.util.Random;

public class Main {
    public static void main(String[] args) {
        NumbersConditions numbersConditions = new NumbersConditions ( 6 , 1 , 49 );
        Random rand = new Random ( );
        Generator generate = new Generator ( rand );
        generate.randomNumbers ( numbersConditions );
    }
}

```
Aby zobaczyć że kod działa zapraszam do pobrania z [repozytorium](https://github.com/WJarze/Excercise.git) , folder random.

A teraz pora na omówienie możliwości takiej metody. nie trudno się domyslić ze wszelkie loterie liczbowe są jej pisane , ale czy tylko?
Przykładowo warunki zawarte w tym obiekcie ```NumbersConditions numbersConditions = new NumbersConditions ( 1 , 1 , 6 );``` będzą odpowiadać pojedyńczemu rzutowi kostką do gry , a idąc dalej ```NumbersConditions numbersConditions = new NumbersConditions ( 1 , 0 , 1 );``` będzie odpowiadać rzutowi monetą.
<br> Podsumowując gdy w grach los się do nas nie uśmiechnie, klasę Random możemy wykorzystać do generowanie losowych wartości na potrzeby testów, symulacji przypadkowych trajektorji róznych obiektów fizycznych i wielu innych systemch które mamy zamiar zaprogramować a mają charakter losowy. 
Po więcej przykładów zapraszam do [repozytorium](https://github.com/WJarze/Excercise.git).


