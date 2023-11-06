# Lab 5: Dziedziczenie

Celem laboratorium jest zapoznanie się z mechanizmem dziedziczenia oraz właściwym sposobem jego użycia w programowaniu
obiektowym.

Najważniejsze zadania:

1. Stworzenie klasy `Grass` - nowego rodzaju obiektu na mapie.
2. Stworzenie klasy `GrassField` - nowego rodzaju mapy, przechowującej zwierzęta i trawy.
3. Stworzenie klasy `AbstractWorldMap`.
4. Testy integracyjne.

## Zadania do wykonania (4xp)


1. Zdefiniuj klasę `Grass` (kępka trawy), która:
    * w konstruktorze akceptuje parametr `Vector2d`, określający pozycję kępki trawy,
    * posiada metodę publiczną `Vector2d getPosition()`, która zwraca jej pozycję,
    * posiada metodę publiczną `String toString()`, która zwraca `*` jako swoją reprezentację.

2. Stwórz dodatkowy interfejs `WorldElement`, który byłby implementowany przez klasy `Animal` oraz `Grass`. Zastanów się, co powinien zawierać ten interfejs.

3. Zmodyfikuj interfejs `WorldMap` tak by metoda `objectAt()` zwracała `WorldElement`. 

4. Zdefiniuj klasę `GrassField`, która:
   * implementuje interfejs `WorldMap`,
   * w konstruktorze akceptuje parametr określający liczbę pól trawy, które znajdują się na mapie,
   * kępki trawy powinny być umieszczane losowo w obszarze o współrzędnych `(0, 0)` - `(sqrt(n*10), sqrt(n*10))`, 
     gdzie `n` to liczba pól trawy, przy założeniu, że dwie kępki trawy nie mogą być w tym samym miejscu. Możesz tutaj zastosować dowolny algorytm losowania bez powtórzeń, nawet naiwne losowanie do skutku (patrz zadanie dodatkowe).
   * umożliwia nieograniczone poruszanie się zwierzęcia po mapie, pod warunkiem, że nie wchodzi na inne zwierzę - rozmiar mapy ma być
     "nieskończony" (czyli ograniczony tylko możliwościami `int`-a),
   * posiada metodę `String toString()`, która rysuje fragment mapy, na którym znajdują się wszystkie elementy (zwierzęta oraz trawa). 
     W celu jej implementacji wykorzystaj klasę `MapVisualizer` z poprzedniego laboratorium oraz
     dynamicznie oblicz skrajne punkty, które powinny zostać wyświetlone. Obecność zwierząt ma priorytet nad obecnością
     kępki trawy na danym polu.

     **Uwaga:** zwierzęta i trawy na mapie można przechowywać w dwóch osobnych kolekcjach lub jednej wspólnej. Oba rozwiązania będą poprawne, ale sugerujemy implementację na dwóch osobnych kolekcjach. Zastanów się, jakie problemy wygenerowałaby tutaj jedna wspólna kolekcja.

5. Sprawdź czy implementacja klasy jest poprawna - zainicjuj mapę z 10 kępkami trawy. Uruchom tę samą sekwencję ruchów co w laboratorium 4.

6. Dodaj testy do klas `RectangularMap` oraz `GrassField` weryfikujące poprawność działania metod dostępnych w interfejsie `WorldMap`,

7. Przyjrzyj się implementacjom tych klas - łatwo można zauważyć, że duża część kodu w obu klasach się powtarza. 

8. Dodaj klasę abstrakcyjną `AbstractWorldMap`, która zawiera kod wspólny dla tych klas.

9. Spraw aby obie klasy dziedziczyły z `AbstractWorldMap`, a następnie zredukuj kod:

   - przenieś powtarzający się kod do `AbstractWorldMap`
   - tam gdzie to konieczne nadpisz metody w klasach dziedziczących
   - tam gdzie to możliwe wydziel wspólną część metody do `AbstractWorldMap` i skorzystaj z wywołań `super.wersjaMetodyZKlasyBazowej()`

10. Uruchom testy i zweryfikuj, że mapy działają tak jak wcześniej.

11. Dodaj do interfejsu metodę `getElements()`, która zwróci kolekcję wszystkich elementów na mapie. Dopisz brakujące implementacje tej metody wykorzystując przygotowaną hierarchię klas tak by nie powtarzać kodu.

## Zadanie dodatkowe (<img src="../img/reward_silver.png" alt="srebrna skrzynka" width="50" align="center" />)

Mechanizm losowania trawy w `GrassField` zakłada, że wylosowane pozycje nie powinny się powtarzać. Jeśli zastosujemy naiwne podejście, tj. w przypadku wylosowania już zajętej pozycji będziemy losować jeszcze raz, nasz program staje się niedeterministyczny. Nie jesteśmy w stanie przewidzieć ilu losowań będziemy potrzebowali, a w skrajnych przypadkach losowanie może nigdy się nie zakończyć. Podejście naiwne ma sens, gdy liczba wolnych pozycji jest istotnie większa niż liczba pozycji do wylosowania. W innych przypadkach powinniśmy jednak stosować mądrzejszy algorytm ze stałą liczbą losowań, nawet kosztem pamięci.

1. Wymyśl lub poszukaj takiego sposobu na losowanie pozycji, który dla N traw będzie wymagał stałej liczby losowań (np. dokładnie N).

2. Zrealizuj wybrany algorytm w osobnej klasie. Skorzystaj tutaj z javowego interfejsu `Iterable` i `Iterator` . Jeśli dobrze zaprojektujesz rozwiązanie to będzie go można używać w następujący sposób:

   ```java
   RandomPositionGenerator randomPositionGenerator = new RandomPositionGenerator(maxWidth, maxHeight, grassCount);
   Iterator<Vector2d> positionsIterator = randomPositionGenerator.iterator();
   
   while(positionsIterator.hasNext()) {
       grasses.put(grassPosition, new Grass(positionsIterator.next()));
   }
   ```

   albo nawet tak:

   ```java
   RandomPositionGenerator randomPositionGenerator = new RandomPositionGenerator(maxWidth, maxHeight, grassCount);
   for(Vector2d grassPosition : randomPositionGenerator) {
       grasses.put(grassPosition, new Grass(grassPosition));
   }
   ```

   **Uwaga**: pętla for each w Javie akceptuje nie tylko kolekcje i tablice, ale każdą strukturę, która implementuje interfejs `Iterable`!

   

## Przydatne informacje

* Klasa abstrakcyjna to klasa, która może posiadać niekompletną implementację. Wprowadza się ją, aby usunąć powtarzający się
  kod. Nie można tworzyć obiektów klasy abstrakcyjnej. Klasa jest oznaczana jako abstrakcyjna za pomocą słowa kluczowego
  `abstract`. Klasa abstrakcyjna może implementować jakiś interfejs. Nie wszystkie metody interfejsu muszą być w niej
  zaimplementowane. Obowiązek zaimplementowania brakujących metod automatycznie przechodzi na nieabstrakcyjnych potomków tej klasy.
  
* Każda klasa domyślnie dziedziczy z klasy `Object`. Dziedziczenie z innej klasy wskazujemy za pomocą słowa kluczowego
  `extends`:

    ```java
    class RectangularMap extends AbstractWorldMap {
    }
    ```

* Jeśli chcemy, aby jakieś pola lub metody nie były częścią publicznego interfejsu klasy, ale żeby były dostępne w
  klasach podrzędnych, to oznaczamy je jako chronione (`protected`). Przykładowo, lista zwierząt w klasie `AbstractWorldMap`
  może być chroniona:

    ```java
    abstract class AbstractWorldMap implements WorldMap {
      protected Map<Vector2d, Animal> animals = new HashMap<>();
    }
    ```

  Alternatywnie można też pozostawić atrybut prywatny, a potem dodać do niego chroniony getter (lepsza hermetyzacja).
  
* Klasa podrzędna może zmienić implementację metody dostępnej w klasie nadrzędnej - widzieliśmy to na przykładzie metody
  `toString()`. Wtedy dla każdego obiektu używana jest zawsze metoda z *faktycznego*, a nie deklarowanego typu tego
  obiektu. Innymi słowy w Javie domyślnie metody są *wirtualne*. Zwykle metody nadpisujące metody z klasy bazowej oznaczamy
  anotacją `@Override`. (Anotację `@Override` stosuje się także wobec metod implementujących metody abstrakcyjne interfejsu.
  Jej użycie jest opcjonalne.)
  
* Klasa podrzędna może odwołać się do implementacji z klasy nadrzędnej za pomocą słowa kluczowego `super`. Np.

    ```java
    public WorldElement objectAt(Vector2d position) {
      WorldElement object = super.objectAt(position);
      //...
    }
    ```

​		W ten sposób można *rozszerzać* zachowanie jakiejś metody w klasach podrzędnych.

* W szczególności konstruktor klasy potomnej może *jawnie* wywołać konstruktor klasy bazowej poprzez `super(argumenty)`.
  Musi to być pierwsza linijka konstruktora potomka. Jeśli tego nie zrobimy, domyślnie wywoływany jest konstruktor bezparametrowy
  przodka.

