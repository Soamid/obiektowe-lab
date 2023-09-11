# Laboratorium 5

Celem laboratorium jest zapoznanie się z mechanizmem dziedziczenia oraz właściwym sposobem jego użycia w programowaniu
obiektowym.

Najważniejsze zadania:

1. Stworzenie klasy `Grass`.
2. Stworzenie klasy `GrassField`.
3. Stworzenie klasy `AbstractWorldMap`.
4. Testy integracyjne.

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
  powinna być chroniona:
```java
abstract class AbstractWorldMap implements WorldMap {
  protected List<Animal> animals = new ArrayList<>();
}
```
* Klasa podrzędna może zmienić implementację metody dostępnej w klasie nadrzędnej - widzieliśmy to na przykładzie metody
  `toString()`. Wtedy dla każdego obiektu używana jest zawsze metoda z *faktycznego*, a nie deklarowanego typu tego
  obiektu. Innymi słowy w Javie domyślnie metody są *wirtualne*. Zwykle metody nadpisujące metody z klasy bazowej oznaczamy
  anotacją `@Override`. (Anotację `@Override` stosuje się także wobec metod implementujących metody abstrakcyjne interfejsu.
  Jej użycie jest opcjonalne.)
* Klasa podrzędna może odwołać się do implementacji z klasy nadrzędnej za pomocą słowa kluczowego `super`. Np.
```java
public Object objectAt(Vector2d position) {
  Object object = super.objectAt(position);
  //...
}
```
W ten sposób można *rozszerzać* zachowanie jakiejś metody w klasach podrzędnych.
* W szczególności konstruktor klasy potomnej może *jawnie* wywołać konstruktor klasy bazowej poprzez `super(argumenty)`.
  Musi to być pierwsza linijka konstruktora potomka. Jeśli tego nie zrobimy, domyślnie wywoływany jest konstruktor bezparametrowy
  przodka.

## Zadania do wykonania


0. Wykorzystaj klasy z laboratorium nr 4.
1. Zdefiniuj klasę `Grass` (kępka trawy), która:
   * w konstruktorze akceptuje parametr `Vector2d`, określający pozycję kępki trawy,
   * posiada metodę publiczną `Vector2d getPosition()`, która zwraca jej pozycję,
   * posiada metodę publiczną `String toString()`, która zwraca `*` jako swoją reprezentację.
2. Zdefiniuj klasę `GrassField`, która:
   * implementuje interfejs `WorldMap`,
   * w konstruktorze akceptuje parametr określający liczbę pól trawy, które znajdują się na mapie,
   * kępki trawy powinny być umieszczane losowo w obszarze o współrzędnych `(0, 0)` - `(sqrt(n*10), sqrt(n*10))`, 
     gdzie `n` to liczba pól trawy, przy założeniu, że dwie kępki trawy nie mogą być w tym samym miejscu,
   * umożliwia nieograniczone poruszanie się zwierzęcia po mapie, pod warunkiem, że nie wchodzi na inne zwierzę - rozmiar mapy ma być
     "nieskończony" (czyli ograniczony tylko możliwościami `int`-a),
   * posiada metodę `String toString()`, która rysuje fragment mapy, na którym znajdują się wszystkie elementy (zwierzęta oraz trawa). 
     W celu jej implementacji wykorzystaj klasę `MapVisualizer` z poprzedniego laboratorium oraz
     dynamicznie oblicz skrajne punkty, które powinny zostać wyświetlone. Obecność zwierząt ma priorytet nad obecnością
     kępki trawy na danym polu.
   * posiada metody `equals()/hashCode()` nadpisane tak by porównywać obiekty trawy po ich pozycjach.
3. Sprawdź czy implementacja klasy jest poprawna - zainicjuj mapę z 10 kępkami trawy.
   Uruchom tę samą sekwencję ruchów co w laboratorium 4.
4. Dodaj testy do klas `RectangularMap` oraz `GrassField` weryfikujące poprawność działania metod dostępnych w
   interfejsie `WorldMap`,
5. Przyjrzyj się implementacjom tych klas - łatwo można zauważyć, że duża część kodu w obu klasach się powtarza. 
6. Dodaj klasę abstrakcyjną `AbstractWorldMap`, która zawiera kod wspólny dla tych klas.
7. Spraw aby obie klasy dziedziczyły z `AbstractWorldMap` oraz usuń kod, który jest już zaimplementowany w klasie
   `AbstractWorldMap`.
8. W szczególności dodaj implementację metody `toString()` w klasie `AbstractWorldMap`, w taki
   sposób, aby wykorzystywała ona abstrakcyjne metody zdefiniowane w tej klasie, posiadające odrębne implementacje w
   klasach dziedziczących. Jest to wzorzec projektowy [metoda szablonowa](https://pl.wikipedia.org/wiki/Metoda_szablonowa_(wzorzec_projektowy)).
9. Uruchom testy i zweryfikuj, że mapy działają tak jak wcześniej.
10. Rozważ dodanie interfejsu `MapElement`, który byłby implementowany przez klasy `Animal` oraz `Grass`. Zastanów się
      czy można by uprościć implementację klasy `GrassField` wykorzystując ten interfejs.
11. Zastanów się, czy celowe byłoby dodanie klasy `AbstractMapElement`.
12. (**Dla zaawansowanych**). Zmodyfikuj implementację tak, żeby po spotkaniu zwierzęcia i trawy, trawa znikała. Nowe kępki
    trawy powinny pojawiać się losowo w obszarze z punktu 1, po zjedzeniu trawy przez zwierzę, przy założeniu, że nowe położenie
    kępki trawy nie pokrywa się z istniejącą kępką trawy, ani z żadnym zwierzęciem.

