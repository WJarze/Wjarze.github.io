---
layout: post
title:  "Czy to działa?"
date:   2024-01-13 17:41:28 +0100
categories: jekyll update
---

Na początku była składnia, a za nią pojawiały się pierwsze obiekty i metody. Weryfikacja działania kodu była niezwykle prosta - wystarczyło kilka linii System.out.println(). To były czasy, kiedy to jedno spojrzenie na konsolę mogło rozwiać wszystkie wątpliwości dotyczące poprawności naszego kodu.  To była era System.out.println(). 
Pytanie "Czy to działa?" znajdowało często swoją odpowiedź w prostocie jednego printa takie były poczatki.

No i doszedłem do ściany z biegiem czasu, w miarę rozwoju projektów, pojawiły się nowe wyzwania. Projekty stawały się bardziej rozbudowane, a kod bardziej skomplikowany, takie printowanie zaczęło być nie tylko czasochłonne, ale też sprawiało, że kod był niezbyt czytelny. I tak kod wyewoluował, struktury kodu zaczęły wymagać bardziej zaawansowanego podejścia do testowania szybszego , efektywnego i niezawodnego. 
Wtedy z pomocą przyszły narzędzia takie jak JUnit i Mockito.
JUnit umożliwił formalne i zautomatyzowane testowanie jednostkowe. Teraz pytanie "Czy to działa?" można było precyzyjnie określić poprzez napisanie testów jednostkowych, sprawdzających poszczególne elementy kodu. I zawsze jakiś kroczek naprzód w porównaniu do prostego printowania na konsoli.

Nie przedłużając mały trywialny i sentymentalny aczkolwiek dość niefortuny przykład z printowaniem "Java is the beast language in the world", który uzmysławia kilka spraw.

    package org.example;
    
    public class StandardOutput {
    
    //metoda drukująca Java is the best language in the world na konsoli
    public void printText() {
        System.out.println ( "Java is the best language in the world" );
    }
    
    public static void main(String[] args) {
        //Tworzymy obiekt
        StandardOutput stOut = new StandardOutput ( );
        //wywołujemy metodę na obiekcie
        stOut.printText ( );
      }
    }


przechodząc do sedna ten test wygląda jak poniżej, słowem wyjaśnienia w kontekście testowania, zwłaszcza w podejściu Behavior-Driven Development (BDD), użyte zostały notacje: 

given - odnosi się do etapu przygotowania stanu początkowego testu, który jest wymagany do wykonania testu.

when - opisuje konkretne działanie lub zdarzenie, które jest testowane. Odnosi się do akcji, którą test ma przetestować.

then - opisuje oczekiwane wyniki lub efekty działania testu. Odnosi się do tego, co powinno nastąpić w wyniku wykonania konkretnej akcji.

    package org.example;

    import java.io.ByteArrayOutputStream;
    import java.io.PrintStream;
    import org.junit.Test;

    import static org.junit.Assert.assertEquals;

    public class StandardOutputTest {

    @Test
    public void shouldBeReturnCorrectText() {

        //Given
        String expectedText = "Java is the best language in the world\r\n";
        // Tworzymy obiekt
        StandardOutput stdOut = new StandardOutput ( );
        // Przechwyć standardowy strumień wyjścia
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream ( );
        System.setOut ( new PrintStream ( outputStream ) );

        //When
        // Wywołaj metodę, która korzysta ze strumienia wyjścia
        stdOut.printText ( );

        // Pobierz przechwycony tekst z ByteArrayOutputStream do Stringa
        String capturedOutput = outputStream.toString ( );

        //Then
        //Sprawdzamy, czy tekst  został przechwycony ze strumienia wyjścia
        assertEquals ( expectedText , capturedOutput );
      }
    }

nader rozwlekły test. Popraiona metoda zwracająca stringa i test

    package org.example;

    public class StandardOutputExampleCorrect {
    private String str = "Java is the best language in the world";

    //poprawiona metoda drukująca zwracająca Stringa Java is the best language in the world na konsoli
    public String printText() {
        return str;
    }

    public static void main(String[] args) {
        //Tworzymy obiekt
        StandardOutput stOut = new StandardOutput ( );
        //wywołujemy metodę na obiekcie
        stOut.printText ();

      }
    }

i już jest zgrabniej

    package org.example;

    import org.junit.Test;

    import static org.junit.Assert.assertEquals;

    public class StandardOutputExampleCorrectTest {
          @Test
          public void shouldBeReturnCorrectText() {
              //Given
              String expectedText = "Java is the best language in the world";
              // Tworzymy obiekt
              StandardOutputExampleCorrect stdOut = new StandardOutputExampleCorrect ( );
              // nic nie musimy Przechwytywać ze  standardowego strumienia wyjścia
              //When
              // Wywołaj metodę, która korzysta ze strumienia wyjścia
              String capturedOutput = stdOut.printText ( );
              // Pobierz przechwycony tekst z ByteArrayOutputStream do Stringa
              //A po co?
              //Then
              //Sprawdzamy, czy tekst  został przechwycony ze strumienia wyjścia
              assertEquals ( expectedText , capturedOutput );
           }
        }

    
Jak widać test w porównaniu do do samej metody jest jakiś nadprogramowy i sporo dodatkowych tu linijek kodu , 
i już na pierwszy rzut oka widać z czego to wynnika.
Oczywiste staje się że dobranie się do bebechów metody i przechwycenie strumienia wyjścia na konsole to dodatkowe linie kodu , 
a więc pierwszy wniosek jest taki testowanie nie jest tak optymalne jak testowanie metod zwracających wartość i jeżeli tak jak w tym przypadku 
metodę łatwo zmienić na metodę zwracającą stringa to należy z tej mozliwości skorzystać.
Z drugiej jednak strony nie unikniemy w kodzie metod void jednak nie zawsze dobranie się do przysłowiowych bebechów żeby zweryfikować jej działanie bedzie
możliwe i uzasadnione, a samą wartość diagnostyczną może nieść odpowiednio w komponowany w metodę komunikat jednak System.out.println() nic poza strumieniem znaków nie niesie.
Wyobrażmy sobie program który ma kilkadziesiąt klas a w metodach void zaszywamy System.out.println("i dpowiedni komentarz który możze być bardzo długi żeby przedstawiał ze sobą wartość diagnostyczną), no nie będzie to wyglądać dobrze. 
Do tego są w ecosystemie javy stworzone mechanizmy logowania zawierające gotowe biblioteki które mają większe mozliwości i są bardziej przejrzyste w zastosowaniu jeśli już musimy to stosujmy je. 

Napjprostszy chyba zmożliwych z Java.Util

    package org.example;

    import java.util.logging.Logger;

    public class Logging {
        static Logger logger = Logger.getLogger ( Logging.class.getName () );
        public static void main(String[] args) {
        
            logger.info("server is start");

        }
    }

i dla krótkiego objaśnienia

info - to poziom logowania który nie tzreba tłumaczyć kolejne to

severe - krytyczne błędy apka moze się wysypać

fine - używny na poziomie debugowania bardziej szczegółowe niz ogólny poziom informacji


ale wracając do tematu, jak przetestować metodę typu void i obejść jej ograniczenie oto kilka przykładów. 
Jeden ze sposobów skupia się na sprawdzaniu, czy metoda wykonuje pewne operacje w oczekiwany sposób , sprawdzamy jej zachownie.

Metoda wywołuje jakiś efekt uboczny:
Jeśli metoda modyfikuje stan obiektu lub wykonuje jakieś zmiany w systemie, możesz przetestować, czy ta zmiana wystąpiła i czy ta zmiana  jest w jakiś sposób mierzalna
aby móc to potwiedzić, przekładając na przykład dodawania Stringa do listy a w zasadzie trzech

    package org.example;

    import java.util.ArrayList;
    import java.util.List;

    public class SideEffect {

        public void addToList(List<String> list , String element) {
            list.add ( element );
    }

        public static void main(String[] args) {

            SideEffect sideEffect = new SideEffect ( );
            List<String> list = new ArrayList<String> ( );
            sideEffect.addToList ( list , "element1" );
            sideEffect.addToList ( list , "element2" );
            sideEffect.addToList ( list , "element3" );
        }
    }

    package org.example;

    import java.util.ArrayList;
    import java.util.List;
    import junit.framework.TestCase;
    import org.junit.Test;

    public class SideEffectTest extends TestCase {
        @Test
        public void testMethodWithNoReturnValue() {
            // Utwórz obiekt klasy, która zawiera metodę void
            SideEffect sideEffect = new SideEffect ( );
            // Ustaw początkowy stan obiektu
            List<String> list = new ArrayList<String> ( );
            // Przygotuj dane testowe
            String initialValue = "Element1";
            // Wywołaj metodę void
            sideEffect.addToList ( list , initialValue );
            // Sprawdź, czy stan obiektu został zmieniony zgodnie z oczekiwaniami
            int finalExpectedValue = 1;
            assertEquals ( finalExpectedValue , list.size ( ) );
            assertEquals ( initialValue , list.get ( 0 ) );
        }
    }

Inny sposób to Mockowanie.
Jeśli metoda współdziała z innymi obiektami, możesz użyć frameworku do mockowania, takiego jak Mockito, aby zweryfikować, czy metoda była wywoływana z oczekiwanymi argumentami. Chwila klepania i szybkie wysyłanie emaila 

    package org.example.mockEmailSender;

    public class EmailSender {
        public void sendEmail(String to, String subject, String body) {
            // Rzeczywista implementacja wysyłki e-maila, która może być mockowana w testach
            System.out.println("Sending email to: " + to);
            System.out.println("Subject: " + subject);
            System.out.println("Body: " + body);
            // W rzeczywistości ta metoda mogłaby zawierać kod wysyłający e-mail
        }
    }

    package org.example.mockEmailSender;

    public class NotificationService {
        private EmailSender emailSender;

        public NotificationService(EmailSender emailSender) {
            this.emailSender = emailSender;
        }

        public void sendNotification(String to , String subject , String body) {
            // Logika związana z powiadamianiem
            // W tym przypadku używamy EmailSender do wysyłania powiadomień
            emailSender.sendEmail ( to , subject , body );
        }
    }

pora przetestować funkcjonalność wysyłania emaila

    package org.example.mockEmailSender;

    import junit.framework.TestCase;
    import org.junit.Test;

    import static org.mockito.Mockito.mock;
    import static org.mockito.Mockito.verify;

    public class NotificationServiceTest extends TestCase {

        EmailSender emailSenderMock = mock ( EmailSender.class );
        NotificationService notificationServiceMock = new NotificationService ( emailSenderMock );
        @Test
        public void testSendNotification() {

            notificationServiceMock.sendNotification ( "test@example.com" , "Test Subject" , "Test Body" );
        // Sprawdzamy, czy metoda sendEmail na mocku została wywołana dokładnie raz z odpowiednimi argumentami
            verify ( emailSenderMock ).sendEmail ( "test@example.com" , "Test Subject" , "Test Body" );
        }

    }


Kluczowe jest zrozumienie, co dokładnie robi metoda void i jakie efekty powinna generować. Testy powinny skupiać się na zweryfikowaniu, czy metoda działa zgodnie z oczekiwaniami, a niekoniecznie na zwracaniu wartości. Wystarczy sprawdzić czy metoda zachowuje się w odpowiedni spodób.
Nie przedłużając do testów jescze nie raz wrócę. Temat rozległy scenariuszy do przetestownia dużo.  








