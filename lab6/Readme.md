# Lab 6: Refactoring kodu

Celem laboratorium jest zapoznanie się z różnymi metodami usprawniania kodu (*refactoringu*) bez wprowadzania nowych funkcji. Wprowadzimy mechanizm wyjątków, a także zapoznamy się z przydatnymi wzorcami projektowymi, takimi jak metoda szablonowa czy obserwator. 

Najważniejsze zadania:

1. Walidacja danych przy użyciu mechanizmu wyjątków.
2. Zastosowanie wzorca `template method` do wyliczania granic mapy.
3. Zastosowanie rekordów do tworzenia kontenerów na dane.
4. Zastosowanie wzorca `Observer`.




## Zadania do wykonania (4xp)

### Obsługa błędów

1. W metodzie odpowiedzialnej za zamianę argumentów aplikacji na ruchy zwierzęcia rzuć wyjątek `IllegalArgumentException`, jeśli którykolwiek z parametrów nie należy do listy poprawnych parametrów (`f`, `forward`, `b`, `backward`, etc.). Jako przyczynę wyjątku wprowadź łańcuch znaków informujący, że określony parametr jest niepoprawny, np.  `new IllegalArgumentException(argument + " is not legal move specification")`.
2. Stwórz własną klasę wyjątku - `PositionAlreadyOccupiedException`. Powinien być to wyjątek typu **checked**. Wyjątek powinien przyjmować w konstruktorze `Vector2d` i tworzyć na jego podstawie wiadomość np. `Position (x, y) is already occupied`.
3. W metodach odpowiedzialnych za dodawanie elementów do mapy, jeśli dodanie elementu na wybrane pole jest niemożliwe rzuć wyjątek `PositionAlreadyOccupiedException`. Wyjątek zastępuje sygnalizowanie błędu przy pomocy zwracania wartości `false` (zmień sygnaturę metody).
4. Obsłuż oba wyjątki. W przypadku błędów walidacji opcji program powinien zostać przerwany z odpowiednim komunikatem. W przypadku próby ustawiania zwierzątek na złych pozycjach w klasie `Simulation` powinny one być pominięte, ale program nadal ma działać i umożliwiać symulację dla poprawnie ustawionych zwierzątek.
5. Zaktualizuj testy metody `place` oraz klasy `OptionsParser`, aby były zgodne z nowym kontraktem.

### Metoda szablonowa

1. Stwórz **rekord** `Boundary`, który będzie przechowywał dwie pozycje `Vector2d` - lewy dolny róg i prawy górny róg (opisujące prostokątny obszar).
2. Dodaj do klasy `AbstractWorldMap` abstrakcyjną metodę `getCurrentBounds()`, która będzie zwracała obiekt `Boundary`.
**Uwaga:** możesz dodać tę metodę także bezpośrednio w interfejsie `WorldMap`.
3. Zaimplementuj metodę w obu realizacjach mapy korzystając z istniejącego już kodu.
4. Pozbądź się z obu realizacji mapy metody `toString()` oraz atrybutu `MapVisualizer` - przenieś je do klasy bazowej. W tym przypadku `toString()` powinno stać się metodą szablonową. Wykorzystaj w tym celu stworzoną wcześniej metodę `getCurrentBounds()`.

### Obserwator

1. Stwórz nowy interfejs `MapChangeListener`, zawierający jedną metodę: `void mapChanged(WorldMap worldMap, String message)`.

2. Klasa `AbstractWorldMap` będzie naszym typem obserwowanym (*observable*). Dodaj do niej niezbędne elementy zgodnie z wzorcem:

   - Klasa powinna przechowywać listę swoich obserwatorów realizujących interfejs `MapChangeListener`.
   - Klasa powinna umożliwiać rejestrowanie i wyrejestrowanie obserwatorów.
   - Umieszczenie zwierzęcia na mapie lub jego poruszenie powinno skutkować powiadomieniem wszystkich obserwatorów z podaniem opisu, co się wydarzyło (stwórz dodatkową metodę pomocniczą np. `mapChanged(String)` - powinna wywoływać metodę z interfejsu `MapChangeListener` na wszystkich zarejestrowanych obserwatorach).

3. Stwórz klasę `ConsoleMapDisplay` - to będzie nasz pierwszy obserwator (*observer*). Kolejnych dodamy na późniejszych zajęciach. Klasa powinna:

   - realizować interfejs `MapChangeListener`
   - w reakcji na zmianę mapy wypisywać kolejno:
     - otrzymaną informację o operacji wykonanej na mapie
     - wizualną reprezentację otrzymanej mapy (`toString()`)
     - sumaryczną liczbę wszystkich otrzymanych do tej pory aktualizacji (zdefiniuj odpowiedni atrybut)

4. Zarejestruj `ConsoleMapDisplay` jako obserwatora dla tworzonej mapy - możesz to zrobić np. w klasie `World`. 

5. Pozbądź się wypisywania stanu mapy z klasy `Simulation`. Jeśli wszystko poszło ok, mapa i tak będzie się wypisywać po każdej zmianie pozycji!

   Zastanów się, co nam daje takie rozwiązanie? W jaki sposób zastosowanie wzorca obserwator może wpłynąć korzystnie na dalsze rozwijanie naszego kodu? 



## Przydatne informacje

* Wyjątki są mechanizmem pozwalającym przekazywać informację o błędzie pomiędzy odległymi fragmentami kodu.
* Zgłoszenie błędu odbywa się poprzez *rzucenie wyjątku*. W Javie służy do tego słowo kluczowe `throw`:

    ```java
    throw new IllegalArgumentException("ABC argument is invalid")
	```
* Nieobsłużony wyjątek powoduje przerwanie działania aplikacji.
* Obsługa wyjątków odbywa się za pomocą mechanizmu *przechwytywania wyjątków*. W Javie służy do tego konstrukcja `try...catch`:

    ```java
    try {
      // kod który może rzucić wyjątek
    } catch(IllegalArgumentException ex) {
      // kod obsługi wyjątku
    }
    ```
    Wyjątek może być rzucony na dowolnym poziomie w kodzie, który otoczony jest blokiem `try`. Tzn. w kodzie tym może być
    wiele zagnieżdżonych wywołań funkcji, a i tak blok `try` przechwyci taki wyjątek, pod warunkiem, że nie zostanie on obsłużony
    na niższym poziomie.

* Wyjątki w Javie dzielą się na **checked** i **unchecked**. W pierwszym przypadku konieczna jest ich deklaracja (kompilator nie pozwoli zostawić rzucony wyjątek bez jego obsługi), w drugim - wyjątki mogą być rzucane bez konieczności ich definiowania lub łapania (ale niezłapanie wyjątku wiąże się z przerwaniem wątku lub programu). Aby stworzyć wyjątek typu *checked* wystarczy podziedziczyć po klasie `Exception`. Wyjątki *unchecked* dziedziczą z kolei po `RuntimeException`.

* Wzorce projektowe są koncepcją występującą w programowaniu obiektowym polegającą na tym, że określona klasa problemów
  może być rozwiązana w schematyczny sposób. Rozwiązanie problemu jednak nie może być (najczęściej) zawarte w jednej
  klasie, dlatego wzorzec stanowi swego rodzaju szkielet rozwiązania, który określa jakie klasy i interfejsy muszą być
  wykorzystane, aby poprawnie rozwiązać dany problem.

* Przykładem wzorca jest [obserwator (*observer*)](https://refactoring.guru/design-patterns/observer) - rozwiązuje on problem zmian wewnętrznego stanu obiektu bez konieczności uzależniania klasy od wielu innych klas, które mają na te zmiany reagować.
* Innym wzorcem jest [metoda szablonowa (template method)](https://refactoring.guru/design-patterns/template-method) - jest to sposób na wykorzystanie mechanizmu dziedziczenia i metod abstrakcyjnych do jeszcze większej redukcji powtarzającego się kodu. 
* Rekordy to proste struktury opisujące **niezmienne (*immutable*)** dane. Korzystamy z nich by uniknąć żmudnego tworzenia getterów, setterów, konstruktorów, equals, hashCode i toString - wszystkie te elementy są automatycznie generowane! 
    ```java
    public record Color(int red, int blue, int green) {}
    ```
    
    Ten kod pozwala tworzyć obiekty kolorów i odwoływać się do nich tak podobnie w przypadku zwykłych klas:
    ```java
    Color color = new Color(255, 20, 10);
    int blue = color.blue();
    System.out.println(color); // wypisze "Color[red=255, blue=20, green=10]"
    ```
    
    

​	
