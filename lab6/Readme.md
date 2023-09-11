# Laboratorium 6

Celem laboratorium jest zapoznanie się z interfejsem `Map` oraz wzorcem projektowym `Observer`.

Najważniejsze zadania:

1. Zamiana listy zwierząt na słownik.
2. Dodanie wzorca `Observer`.

## Przydatne informacje

* Interfejs `Map` definiuje w Javie strukturę słownikową, czyli mapę odwzorowującą *klucze* na *wartości*.
* Jedną z najczęściej wykorzystywanych implementacji interfejsu `Map` jest klasa `HashMap`, przykładowo:

```java
Map<Vector2d, Animal> animals = new HashMap<>();
```

* Poprawne działanie `HashMap` uzależnione jest od implementacji metod `equals` oraz `hashCode` w klasie, która stanowi
  klucze mapy (w ćwiczeniu dotyczy to klasy `Vector2d`).

* Wynik działania metody `hashCode` musi być zgodny z wynikiem działania metody `equals`, tzn. jeśli dwa obiekty są
  równe według `equals`, to ich `hashCode` musi być równy.

* Przykładowa implementacja metody `hashCode` dla klasy `Vector2d` może wyglądać następująco:

```java
@Override
public int hashCode() {
  return Objects.hash(this.x, this.y);
}
```

* Używanie mapy nie wymaga jawnego wywoływania metody `hashCode`, ale jest ona używana wewnętrznie dla potrzeb optymalizacji.
  Istotą funkcji jest fakt, że dla identycznych wartości `x` i `y` wartość funkcji `hashCode` będzie identyczna.

* Wzorce projektowe są koncepcją występującą w programowaniu obiektowym polegającą na tym, że określona klasa problemów
  może być rozwiązana w schematyczny sposób. Rozwiązanie problemu jednak nie może być (najczęściej) zawarte w jednej
  klasie, dlatego wzorzec stanowi swego rodzaju szkielet rozwiązania, który określa jakie klasy i interfejsy muszą być
  wykorzystane, aby poprawnie rozwiązać dany problem.

* Przykładem wzorca jest obserwator (*observer*) - rozwiązuje on problem zmian wewnętrznego stanu obiektu.
  Więcej informacji na temat tego wzorca można znaleźć pod adresem https://en.wikipedia.org/wiki/Observer_pattern
* W Javie istnieje kolekcja `SortedSet`, która umożliwia przechowywanie uporządkowanego zbioru elementów. Elementy mogą
  implementować interfejs `Comparable` lub przy tworzeniu zbioru należy wskazać obiekt implementujący interfejs
  `Comparator`, odpowiedzialny za porządkowanie elementów. Przekazany `Comparator` zastępuje naturalny porządek sortowania
  (wynikający z interfejsu `Comparable`), jeśli taki jest.


## Zadania do wykonania


### Zmiana sposobu przechowywania obiektów na mapie

1. Implementacja metod `isOccupied` oraz `objectAt` w listach nie jest wydajna, ponieważ za każdym razem wymaga przejścia
   przez wszystkie elementy znajdujące się na liście. Można ją poprawić zamieniając listę na słownik (wykorzystując 
   interfejs `Map` oraz implementację `HashMap`). Jest to jednocześnie wydajniejsze pamięciowo niż przechowywanie zwierząt (i trawy) w tablicy.
   Kluczami słownika powinny być pozycje elementów, a wartościami konkretne obiekty.
2. Poprawna implementacja słownika wymaga, aby klasa `Vector2d` implementowała metodę `hashCode`. Metoda ta jest
   wykorzystywana m.in. przez słownik oparty o tablicę haszującą (`HashMap`). Możesz wygenerować kod metody `hashCode` w
   klasie `Vector2d` korzystając ze
   wsparcia środowiska programistycznego. Zasadniczo metoda ta musi być zgodna z działaniem metody `equals` - dwa
   obiekty, które są równe według metody `equals` muszą mieć identyczną wartość zwracaną przez metodę `hashCode`
   (nie działa to w drugą stronę - `hashCode` może zwracać równe wartości dla obiektów, które nie są równe wg. `equals`).
3. Zmiana typu kolekcji `animals` będzie wymagała zmiany implementacji metod `isOccupied` i `objectAt`.

### Aktualizacja słownika w mapie

1. Implementacja mechanizmu aktualizacji słownika mapy wymaga, aby mapa była informowana o zmianach pozycji zwierząt, które inicjuje `SimulationEngine`.
    Rozwiązaniem jest zastosowanie wzorca projektowego `Observer` - mapa ma zarejestrować się jako obserwator dla zmian pozycji zwierzęcia.
2. Realizację implementacji rozpocznij od zdefiniowana interfejsu `PositionChangeObserver`, który zawiera jedną metodę
    `positionChanged(Vector2d oldPosition, Vector2d newPosition)`.
3. Obie mapy muszą implementować ten interfejs. Możesz to również zrealizować, jeśli odpowiedni kod umieścisz w klasie
   `AbstractWorldMap`. Implementacja metody `positionChanged` powinna polegać na tym, że ze słownika usuwana jest para:
   `<stara pozycja, zwierzę>`, a dodawana jest para: `<nowa pozycja, zwierzę>`.
4. Klasa `Animal` musi umożliwić rejestrowanie obserwatorów. Dodaj metody: `void addObserver(PositionChangeObserver
    observer)` oraz `void removeObserver(PositionChangeObserver observer)`, które będą dodawały i usuwały danego
    obserwatora do listy obserwatorów w klasie `Animal`.
5. Klasa `Animal` musi informować wszystkich obserwatorów, o tym że pozycja została zmieniona. Stwórz metodę
    `positionChanged` w klasie `Animal`, która będzie notyfikowała wszystkich obserwatorów o zmianie.
6. Zweryfikuj poprawność implementacji korzystając z kodu z poprzednich laboratoriów.
