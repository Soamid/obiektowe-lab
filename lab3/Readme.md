# Lab 3: Interakcje między obiektami

Celem laboratorium jest wykorzystanie modelu obiektowego do rozwiązania prostego zadania. Wykorzystamy wcześniej zaimplementowane klasy, dzięki czemu unikniemy powtórzeń i sprawimy, że rozwiązanie będzie czytelne. Wprowadzimy także nowe struktury danych - kolekcje obiektów.

Najważniejsze zadania:

1. Stworzenie klasy `Animal`.
2. Stworzenie klasy `Simulation`, która pozwoli poruszać zwierzętami.
3. Zapoznanie się z kolekcjami na przykładzie list w Javie.
4. Testy integracyjne.

## Zadania do wykonania (4xp)

### Model `Animal`


1. Wykorzystaj definicje klas `Vector2d`, `MapDirection` oraz `MoveDirection` z laboratorium 2.
2. W pakiecie `agh.ics.oop.model` utwórz klasę `Animal`, która:
   * posiada atrybut określający obecną orientację zwierzęcia (początkowo zawsze ustawioną na `NORTH`),
   * posiada atrybut określający położenie zwierzęcia na mapie,
   * posiada dwa konstruktory:
     * domyślny, który ustawia pozycję na `Vector2d(2,2)` i orientację na `NORTH` (przyjmij, że zwierzę znajduje się w pierwszej ćwiartce układu współrzędnych, a północ jest tożsama z kierunkiem wyznaczanym przez rosnące wartości na osi OY)
     * przyjmujący i ustawiający pozycję podawaną z zewnątrz. Orientacja początkowa powinna być ustawiona na `NORTH`.
   * definiuje metodę `toString()`, która w reprezentacji łańcuchowej zawiera informacje o położeniu zwierzęcia (pozycję
     oraz orientację),
   * definiuje metodę `boolean isAt(Vector2d position)`, która zwraca prawdę, jeśli zwierzę znajduje się na pozycji `position`.
3. Zmodyfikuj klasę `World`, która w metodzie `main` stworzy zwierzę i wyświetli w konsoli jego pozycję.
4. Dodaj do klasy `Animal` metodę `move(MoveDirection direction)`, która:

      * Dla kierunków `RIGHT` i `LEFT` zmienia orientację zwierzęcia na mapie, np. kiedy zwierzę ma orientację `NORTH` a
        zmiana kierunku to `RIGHT` to orientacja zwierzęcia powinna wynosić `EAST`.
      * Dla kierunków `FORWARD` i `BACKWARD` zmienia pozycję zwierzęcia o 1 pole, uwzględniając jego orientację, np. kiedy zwierzę
        jest na pozycji `(2,2)` i jego orientacja to `NORTH`, to po ruchu `FORWARD` jego pozycja to `(2,3)`.
      * **Uniemożliwia** wyjechanie poza mapę, która ustalona jest od pozycji `(0,0)` do pozycji `(4,4)` (pięć na pięć pól). W
        sytuacji, w której zwierzę miałoby wyjść poza mapę, wywołanie `move` nie ma żadnego skutku.

### Zastosowanie list

1. Zmodyfikuj klasę `OptionsParser` w taki sposób by zamiast tablicy `MoveDirection[]` zwracała kolekcję `List<MoveDirection>`. Zastanów się, w jaki sposób dokonać tutaj zmian by maksymalnie uprościć kod i nie zmieniać zachowania metody (niepoprawne opcje nadal powinny być pomijane!).
2. Zmodyfikuj miejsca w kodzie oraz testy, które korzystały z `OptionsParser` tak by program się kompilował. Upewnij się, że testy nadal przechodzą. 

###  Tworzenie symulacji


1. W pakiecie `agh.ics.oop` stwórz klasę `Simulation`. 

2. Klasa powinna przyjmować w konstruktorze listę pozycji początkowych zwierząt (`List<Vector2d>`) oraz listę ruchów (`List<MoveDirection>`). Na podstawie pozycji początkowych stwórz listę zwierząt, które będzie przechowywać obiekt symulacji.

3. W klasie `Simulation` zdefiniuj również metodę `run()`, która na przemian steruje ruchem wszystkich zwierząt. Przykładowo, jeśli użytkownik wprowadzi ciąg: `f b r l`, a na mapie są dwa zwierzęta, to pierwsze zwierzę otrzyma ruchy `f` i `r`, a drugie `b` i `l`. Ruchy obu zwierząt mają być wykonywane na przemian, tzn. po każdym ruchu pierwszego zwierzęcia następuje ruch drugiego zwierzęcia.

4. Zapewnij by po każdym ruchu program wypisywał informację `Zwierzę i : (x ,y)`, gdzie `i`- numer zwierzęcia na liście, `x`,`y` - pozycja zwierzęcia po ruchu (skorzystaj z przygotowanego wcześniej `toString()`).

5. W celu weryfikacji rozwiązania wykonaj następujący kod w metodzie `main` klasy `World`:
       ```java
       List<MoveDirection> directions = OptionsParser.parse(args);
       List<Vector2d> positions = List.of(new Vector2d(2,2), new Vector2d(3,4));
       Simulation simulation = new Simulation(positions, directions);
       simulation.run();
       ```

    Sprawdź, czy zwierzęta poruszają się poprawnie dla ciągu: `f b r l f f r r f f f f f f f f`.

6. Napisz testy integracyjne weryfikujące poprawność implementacji. Uwzględnij:
    * czy zwierzę ma właściwą orientację, 
    * czy zwierzę przemieszcza się na właściwe pozycje,
    * czy zwierzę nie wychodzi poza mapę,
    * czy dane wejściowe podane jako tablica łańcuchów znaków są poprawnie interpretowane. 

7. Po realizacji zadania zastanów się, której implementacji listy w Javie najlepiej użyć w przypadkach opisanych w naszych zadaniach. W razie potrzeby podmień implementację na inną i uzasadnij odpowiedź (w formie komentarza w kodzie lub komentarza w PR).

    

## Przydatne informacje

* Początkowe wartości obiektu można określić albo w konstruktorze, albo bezpośrednio przypisując je do pól, np.

  ```java
  class Animal {
    private Vector2d position = new Vector2d(2,2);
  }
  ```
* W Javie istnieją dwie podstawowe struktury sekwencyjne (poza tablicami): [LinkedList](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/LinkedList.html) oraz [ArrayList](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/ArrayList.html). W przeciwieństwie do tablic obie klasy pozwalają na określenie początkowego rozmiaru na 0 i dowolne rozszerzanie kolekcji. 
* Obie klasy implementują interfejs `List`, który definiuje podstawowe operacje na listach.
* Klasy te różnią się implementację - `LinkedList` oparta jest o listę dwukierunkową, przez co operacje dodawania i usuwania elementów są szybkie, ale swobodny dostęp za pomocą operatora `get` jest wolniejszy. `ArrayList` oparta jest o tablicę, dlatego dostęp jest szybki, ale dodawanie i usuwanie elementów jest wolniejsze.
* W Javie występują typy parametryzowane i typ `List` jest tego przykładem. Taki typ jest podobny do szablonów w C++. Wymaga on podania innego typu (lub typów) jako parametru (parametr musi być typem obiektowym):
    ```java
    List<Animal> animals = new ArrayList<>();
    ```
  W tym przykładzie tworzona jest lista zwierząt, a jako implementacja wybrana została klasa `ArrayList`. Dzięki temu
  wywołania takie jak:
    ```java
    animals.get(1);
    ```
  zwracają obiekty klasy `Animal`, dzięki czemu mogą one być używane w "bezpieczny" sposób - tzn. kompilator może sprawdzić,
  czy wywoływane metody faktycznie występują w klasie `Animal`.