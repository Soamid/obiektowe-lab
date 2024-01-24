# Lab bonus 2: Kotlin

Poniższe ćwiczenia mają na celu przetestowanie podstawowych elementów języka Kotlin i porównanie go z Javą. Zakładamy tutaj znajomość Javy na podstawowym poziomie i wcześniejszą realizację pierwszych 8 laboratoriów.

Głównym zadaniem będzie stworzenie (bardzo) uproszczonej wersji mapy zwierząt z wykorzystaniem Kotlina.

Uwaga: do realizacji zadań z Kotlina nie będzie potrzebna JavaFX.

## Zadania do wykonania

### Część I: Elementy języka Kotlin (<img src="../img/reward_silver.png" alt="srebrna skrzynka" width="50" align="center" />)

1. Stwórz nowy projekt w IntelliJ: *File -> New --> Project... -->* wpisz nazwę, zaznacz *Kotlin* jako język oraz *Gradle* jako *Build system*, a także *Groovy* jako *Gradle DSL*.

   **Uwaga:** projekt umieść w repozytorium z laboratoriami, najlepiej jako osobny folder obok projektu Javowego. 

   **Uwaga 2**: IntelliJ domyślnie wybierze i ustawi najnowszą wersję Kotlina. Do poprawnego działania może ona wymagać aktualizacji IntelliJ do najnowszej wersji. Jeśli nie chcesz aktualizować IntelliJ, można zmienić wersję w pliku *build.gradle* (sekcja plugins). Np. dla IntelliJ z początku 2022 powinien działać Kotlin `1.6.21`.

2. Przenieś i przepisz na Kotlina wybrane klasy z poprzednich laboratoriów: `MapDirection`, `MoveDirection`, `Vector2d`. 
   Postaraj się maksymalnie uprościć kod (nie zmieniając jego działania!). W tym celu zastosuj w odpowiednich miejscach takie elementy jak:

   - [data class](https://kotlinlang.org/docs/data-classes.html)
   - [operator `when`](https://kotlinlang.org/docs/control-flow.html#when-expression)
   - [funkcje jako pojedyncze wyrażenia](https://kotlinlang.org/docs/functions.html#single-expression-functions)
   - [formatowanie stringów szablonami](https://kotlinlang.org/docs/strings.html#string-templates)

3. Zastosuj [przeciążenie operatorów](https://kotlinlang.org/docs/operator-overloading.html) by zastąpić nimi metody klasy Vector2d: `add()`, `substract()`, `follows()`, `precedes()`. Np. dodawanie wektorów powinno być możliwe w taki sposób: `val result = Vector2d(1, 2) + Vector2d(3, 4)`.

4. Przepisz na Kotlina `WorldMap` oraz `Animal`. W przypadku klasy `Animal` zwróć szczególną uwagę na to, które elementy powinny być niezmienne oraz które powinny mieć możliwość zmiany tylko w obrębie tej klasy. 
   Pamiętaj, że w Kotlinie można [mieszać parametry konstruktora oraz definicje atrybutów](https://kotlinlang.org/docs/classes.html#constructors) oraz stosować domyślne wartości dla parametrów. 
   **Uwaga:** na potrzeby  tej laborki nie będziemy potrzebowali żadnych elementów UI ani wzorca obserwator - nie musisz przepisywać tych elementów na Kotlina.

5. Stwórz nową klasę `BouncyMap`, która będzie realizować interfejs `WorldMap`. Zachowanie tego rodzaju mapy powinno być następujące:

   - Przechowuje zwierzęta w mapie `<Vector2d, Animal>`.
   - Posiada zadane wymiary (szerokość, wysokość), podobnie jak mapa prostokątna z pierwszych laborek.
   - Metoda `canMoveTo(Vector2d)` sprawdza jedynie czy pozycja mieści się na mapie.
   - Metoda `objectAt(Vector2d)` zwraca zawsze zwierzątko lub `null` jesli pozycja jest pusta.
   - Metoda `place(Animal)` aktualizuje pozycję podanego zwierzątka lub dodaje go na mapę (w zależności od tego czy już tam było). Jeśli na nowej pozycji jest już inne zwierzę,  dodawane zwierzątko odbija się od niego na losowo wybraną pozycję (zachowując swoją orientację). Jeśli nie ma już wolnej pozycji, zwierzę wybija inne losowo wybrane z mapy (patrz kolejny punkt instrukcji).  

6. Zastosuj mechanizm [extension functions](https://kotlinlang.org/docs/extensions.html) żeby uporządkować kod oraz ułatwić implementację losowania pozycji:

   - Przenieś metodę `MapDirection.toUnitVector()` jako *extension function* do pliku z klasą `Vector2d` - to jej odpowiedzialność by zapewnić taką konwersję i uniezależnić ją od siebie.

   - Stwórz plik `RandomExtensions.kt` i przygotuj tam dwie *extension functions*:
     - `randomPosition()` - działa na każdej mapie (klasie `Map`), której kluczami są obiekty `Vector2d` i zwraca losową pozycję z tej mapy lub `null`, jeśli mapa jest pusta. 

       Uwaga: w Kotlinie istnieje już *extension function* `random()` oraz `randomOrNull()` dla np. listy lub zbioru liczb - można z niej tu skorzystac. 
     - `randomFreePostion(mapSize: Vector2d)` - działa na każdej mapie (klasie `Map`), której kluczami są obiekty `Vector2d` i zwraca losową wolną pozycję, zakładając że mapa jest prostokątna, a jej rozmiar ograniczony przez `mapSize`. Jeśli nie ma już wolnej pozycji, zwraca `null`. 

### Część II: Testy z zastosowaniem biblioteki Kotest (<img src="../img/reward_silver.png" alt="srebrna skrzynka" width="50" align="center" />)

Napisz kilka testów sprawdzających działanie `BouncyMap`. W tym celu wykorzystaj bibliotekę [Kotest](https://kotest.io). Dodaj do konfiguracji `build.gradle` zależności:
```
testImplementation 'io.kotest:kotest-runner-junit5:5.5.4'
testImplementation 'io.kotest:kotest-assertions-core:5.5.4'
```

Warto zainstalować też plugin Kotest do samego IntelliJ (*File --> Settings --> Plugins*) żeby nieco łatwiej zarządzać odpalanymi testami, choć nie jest to konieczne.

Zapoznaj się z [stylami testowania](https://kotest.io/docs/framework/testing-styles.html) i wybierz ten, który najbardziej Ci się podoba (ale postaraj się wybrać coś innego niż JUnit, to już znamy z Javy!).

## Przydatne informacje

- [**Kod z przykładami Java vs Kotlin**](kotlin-examples.zip) **(Wersja z uzupełnionymi rozwiązaniami przykładów jest [tutaj](kotlin-examples-solved.zip))**.
- [Oficjalna dokumentacja Kotlina](https://kotlinlang.org/docs/home.html) (jest serio dobra i często pokrywa wszelkie wątpliwości)
- [Kotlin Koans](https://play.kotlinlang.org/koans/overview) - tutorial w formie serii interaktywnych ćwiczeń

