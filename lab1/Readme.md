# Laboratorium nr 1

Celem laboratorium jest zapoznanie się z podstawowymi pojęciami oraz narzędziami Javy.

Najważniejsze zadania:

1. Konfiguracja środowiska.
2. Stworzenie klasy `World`.
3. Stworzenie enumu `Direction`.

## Przydatne informacje
* W programie Javy funkcja (a właściwie metoda) `main` musi być częścią jakiejś klasy. Jest ona punktem startowym programu.
* Metoda `main` akceptuje tablicę argumentów typu `String`, ponadto jest publiczną metodą statyczną:
    ```java
    public class World {
       public static void main(String[] args) {
          // treść metody
       }
    }
    ```
* Do wypisywania komunikatów użyj metod `System.out.print()` oraz `System.out.println()`.
* Warunki logiczne w Javie są przechowywane w zmiennej typu `boolean` - nie ma automatycznej konwersji z innych typów.
* W Javie dostępna jest standardowa pętla `for` znana z C/C++. Można również użyć alternatywnej pętli `for` (tzw. `for each`) 
  do iterowania po elementach kolekcji:
  
    ```java
    for(String argument : arguments){
    }
    ```
* **Uwaga!** W Javie łańcuchy znaków (oraz inne typy referencyjne) porównuje się za pomocą wywołania `equals`, np.
  `string1.equals(string2)`. Zapis `string1 == string2` jest składniowo poprawny, ale sprawdza **identyczność referencji**.
* Typ wyliczeniowy deklaruje się za pomocą słowa kluczowego `enum`, np.:
    ```java
    enum Direction {
      FORWARD,
      BACKWARD,
      RIGHT,
      LEFT
    }
	```
* Typu wyliczeniowego można użyć odwołując się do jego składowych, np.:
    ```java
    Direction direction = Direction.FORWARD;
    ```
* Instrukcję `switch` można używać m. in. na typach wyliczeniowych oraz napisach zarówno w formie instrukcji, jak i wyrażenia, którego wynik można przypisać do zmiennej (od Javy 14):
    ```java
     switch (argument) {
      case "f" ->  System.out.println("Do przodu");
      case "b" ->  System.out.println("Do tyłu");
    }
    
    String message = switch (argument) {
      case "f" -> "Do przodu";
      case "b" -> "Do tyłu";
      default -> "Nieznana komenda";
    };
    
    System.out.println(message);
    ```
* W Javie można dość łatwo przekazać fragment tablicy, np. jako rezultat wywołania funkcji lub jako argument pętli for. Służy do tego wywołanie: 
  ```java
  Arrays.copyOfRange(array, startInclusive, endExclusive);
  ```


## Zadania do wykonania

**Uwaga 1:** przed zainicjowaniem projektu zalecamy upewnić się, czy połączenie z Internetem jest stabilne i wystarczająco szybkie. Stworzenie projektu wymaga pobrania kilku dodatkowych narzędzi w tle. W szczególności **NIE** polecamy pracy na otwartej sieci `AGH-Guest` (lepiej skorzystać z `AGH-5` lub `AGH-WPA`).

**Uwaga 2:** projekt tworzony na zajęciach powinien znaleźć się w utworzonym wcześniej repozytorium Git. Kolejne laboratoria będą wymagały rozszerzania tego projektu o nowe elementy. Pamiętaj o regularnym commitowaniu zmian i udostępnianiu gotowego rozwiązania do oceny zgodnie z wytycznymi prowadzącego.

1. Uruchom program IntelliJ.
2. Utwórz nowy projekt o nazwie `oolab` typu **Gradle**. Pamiętaj by kreatorze projektu ustawić pole `Language` na `Java`, a `Build system` na `Gradle`  (a **nie**  na `IntelliJ`) . Możesz wybrać (lub w razie potrzeby pobrać) najnowszą wersję JDK, ale zalecamy **17**, ponieważ jest to wersja LTS i  instrukcje do laboratoriów są o nią oparte. 
3. Po utworzeniu projektu poczekaj aż IntelliJ zainicjuje projekt - może to chwilę potrwać. Jeśli wszystko poszło ok, po lewej stronie zobaczysz drzewo katalogów. Katalog `java` (w `src/main`) powinien mieć niebieską ikonę (oznacza to, że został wykryty jako katalog z źródłami po zainicjowaniu przez Gradle).
4. W katalogu `src/main/java/` utwórz pakiet `agh.ics.oop` (ppm na `src/main/java` -> `New package`). Możesz też od razu usunąć ewentualne "śmieci" wygenerowane przez IntelliJ (pakiet `org.example`).
5. W pakiecie `agh.ics.oop` utwórz klasę `World` ze statyczną funkcją `main`.
6. Zaimplementuj metodę `main` tak aby wyświetlały się dwa komunikaty:
   - `system wystartował`
   - `system zakończył działanie`
7. Uruchom program np. klikając zieloną ikonę pojawiającą się na początku linii, w której występuje metoda `main`.
8. Dodaj metodę statyczną `run`, która jest wywoływana pomiędzy tymi komunikatami.
9. Metoda `run` powinna informować o tym, że zwierzak idzie do przodu.
10. Uruchom program.
11. Rozszerz metodę `run` tak by akceptowała tablicę argumentów typu `String`. Przekaż do niej tablicę `args`, która zawiera parametry wywołania programu.
12. Po komunikacie o poruszaniu się do przodu wypisz w konsoli wartości wszystkich argumentów tej metody oddzielone przecinkami.
13. Zwróć uwagę na to, żeby nie było nadmiarowych przecinków.
14. Uruchom program z dowolnymi parametrami (muszą występować co najmniej 2). W IntelliJ parametry programu możesz ustawiać po wejściu w konfigurację uruchomieniową (rozwijane menu z nazwą klasy --> `Edit configurations...` --> pole `Program arguments`).
15. Zmodyfikuj program tak aby interpretował wprowadzone argument:

    - `f` - oznacza, że zwierzak idzie do przodu,
    - `b` - oznacza, że zwierzak idzie do tyłu,
    - `r` - oznacza, że zwierzak skręca w prawo,
    - `l` - oznacza, że zwierzak skręca w lewo,
    - pozostałe argumenty powinny być ignorowane.
16. Poruszanie się oraz zmiana kierunku ma być oznajmiana odpowiednim komunikatem. Program powinien akceptować dowolną liczbę
    argumentów. Przykładowo wprowadzenie sekwencji `f f r l` powinno dać w wyniku następujące komunikaty:
    - Start
    - Zwierzak idzie do przodu
    - Zwierzak idzie do przodu
    - Zwierzak skręca w prawo
    - Zwierzak skręca w lewo
    - Stop
17. Zmodyfikuj program w ten sposób, aby metoda `run` nie akceptowała tablicy łańcuchów znaków, lecz tablicę
    wartości typu wyliczeniowego (`enum`). Zamiana łańcuchów znaków powinna być realizowana przez metodę wywoływaną w
    funkcji `main` przed wywołaniem metody `run`, natomiast typ wyliczeniowy powinien być zdefiniowany w osobnym pliku
    (`Direction.java`) w pakiecie `agh.ics.oop`.
18. Zweryfikuj poprawność działania programu poprzez jego uruchomienie.
19. Zamknij IntelliJ.

20. W pliku `build.gradle` w sekcji `plugins` dodaj linię `id 'application'`: 
    ```
    plugins {
      id 'application'
      id 'java'
    }
    ```
21. W tym samym pliku dodaj sekcję:
    ```
    application {
      getMainClass().set('agh.ics.oop.World')
    }
    ```
22. W tym samym pliku dodaj sekcję:
    ```
    java {
        toolchain {
            languageVersion.set(JavaLanguageVersion.of(17))
        }
    }
    ```
    (możesz wybrać inną wersję Javy).
23. Otwórz konsolę (np. terminal/PowerShell).
24. Wywołaj komendę `export JAVA_HOME=/usr/lib/jvm/java-17` (pod Windows trzeba będzie ustawić zmienną środowiskową wskazującą na katalog, w którym zainstalowana jest Java). **Komendę trzeba zaadaptować do lokalnej instalacji Javy!**
25. Uruchom program poleceniem `./gradlew run --args="f l"` (lub `gradlew.bat ...` w systemie Windows)
26. Zmodyfikuj argumenty wywołania i sprawdź zachowanie programu.
27. (**Dla zaawansowanych**) Zmień kod odpowiedzialny za konwersję argumentów oraz wyświetlanie kierunków, tak by 
    korzystał z interfejsu `stream` dostępnego w Javie 8.
