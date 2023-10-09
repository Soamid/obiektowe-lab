# Lab 2: Model obiektowy

Celem laboratorium jest zapoznanie się z modelem obiektowym Javy, na przykładzie klasy reprezentującej dwuwymiarowy
wektor. Wprowadzamy też narzędzia do pisania testów jednostkowych.

Najważniejsze zadania:
1. Stworzenie klasy `Vector2d`.
2. Stworzenie klasy `MapDirection`.
3. Testy jednostkowe.

## Zadania do wykonania (4xp)

Uwaga: dla przejrzystości pliki z klasami `Vector2d` oraz `MapDirection` należy umieścić w pakiecie `agh.ics.oop.model`.

### Klasa `Vector2d`

1. Utwórz klasę `Vector2d`, która:

   * posiada dwa prywatne pola `x` i `y` typu `int`, które nie mogą być modyfikowane (`final`),
   * posiada konstruktor akceptujący parametry `x` i `y`, która są przypisywane do pól `x` i `y`,
   * posiada gettery umożliwiające odczyt wartości utworzonych atrybutów
   * posiada metodę `String toString()`, która zamienia pozycję na napis `(x,y)`, np. dla `x = 1` oraz `y = 2`, napis ma postać
     `(1,2)`,
   * posiada metodę `boolean precedes(Vector2d other)`, akceptującą inny obiekt tej klasy i zwracającą wartość `true`, jeśli oba pola mają
     wartość mniejszą bądź równą polom drugiego obiektu,
   * posiada metodę `boolean follows(Vector2d other)`, akceptującą inny obiekt tej klasy i zwracającą wartość `true`, jeśli oba pola mają
     wartość większą bądź równą polom drugiego obiektu,
   * posiada metodę `Vector2d add(Vector2d other)`, która zwraca nowy obiekt klasy `Vector2d`, którego składowe są sumą odpowiednich składowych
     dodawanych pozycji,
   * posiada metodę `Vector2d subtract(Vector2d other)`, która zwraca nowy obiekt klasy `Vector2d`, którego składowe są różnicą 
     odpowiednich składowych odejmowanych pozycji, 
   * posiada metodę `Vector2d upperRight(Vector2d other)`, która akceptuje inny punkt i zwraca obiekt klasy `Vector2d` posiadający te składowe
     punktów, które mają większe wartości dla odpowiednich osi (innymi słowy jest prawym górnym rogiem prostokąta, który
     opisany jest na obu punktach, którego krawędzie są równoległe do osi X i Y),
   * posiada metodę `Vector2d lowerLeft(Vector2d other)`, która akceptuje inny punkt i zwraca obiekt klasy `Vector2d` posiadający te składowe
     punktów, które mają mniejsze wartości dla odpowiednich osi (tzn. lewy dolny róg prostokąta),
   * posiada metodę `Vector2d opposite()`, która zwraca nowy obiekt tej klasy, posiadający zmienione znaki obu składowych,
   * posiada metodę `boolean equals(Object other)` która zwraca prawdę, jeśli obie pozycje są sobie równe (zwróć uwagę na typ parametru). Uwaga: zastanów się, jaką inną metodę trzeba dodać po zdefiniowaniu własnego `equals`.

2. Poniższy obrazek ilustruje metody `precedes` i `follows`. `v1` poprzedza (precedes) `v2` oraz `v3`. `v2` poprzedza `v3`.
   Wszystkie wektory poprzedzają również same siebie (relacja ta jest zwrotna). `v3` następuje po (follows) `v2` oraz
   `v1`, `v2` następuje po `v1`. Wszystkie wektory następują również po samych sobie.
   ![wektory](vector2d.png)

3. Poniższy obrazek ilustruje metody `lowerLeft` oraz `upperRight`.
   ![rogi](vector2d-a.png)

4. W metodzie `main` w klasie `World` wprowadź następujący kod:

   ```java
   Vector2d position1 = new Vector2d(1,2);
   System.out.println(position1);
   Vector2d position2 = new Vector2d(-2,1);
   System.out.println(position2);
   System.out.println(position1.add(position2));
   ```

   Sprawdź czy uzyskane wyniki są poprawne.

### Klasa `MapDirection`

6. Utwórz typ wyliczeniowy `MapDirection` z czterema kierunkami: `NORTH, SOUTH, WEST, EAST`, który:
   * posiada metodę `toString`, która dla kierunku `EAST` zwraca łańcuch `Wschód`, dla `WEST` - `Zachód`, itd.
   * posiada metodę `next`, która dla kierunku `EAST` zwraca `SOUTH` (kolejny kierunek zgodnie z ruchem wskazówek
     zegara), itd.
   * posiada metodę `previous`, która dla kierunku `EAST` zwraca `NORTH` (kolejny kierunek zgodnie z ruchem przeciwnym
     do ruchu wskazówek zegara), itd.
   * posiada metodę `toUnitVector`, która zwraca jednostkowy wektor przemieszczenia typu `Vector2d` zgodny z orientacją na mapie,
     tzn. dla `NORTH` wektor ten powinien mieć wartość `(0,1)`, dla `WEST` `(-1,0)`, itd.
7. Sprawdź w metodzie `main` czy metody te działają zgodnie z opisem.


### Testy


1. Utwórz klasę `MapDirectionTest` w katalogu `src/test/java` w pakiecie `agh.ics.oop.model`.

2. Zaimplementuj test weryfikujący poprawność działania metody `next()`, dla wszystkich przypadków (dodaj adnotację
   `@Test` przed deklaracją metody).

3. Uruchom test korzystając z zielonych trójkątów po lewej stronie.

4. Zaimplementuj test weryfikujący poprawność działania metody `previous()`, dla wszystkich przypadków.

5. Utwórz klasę `Vector2dTest`.

6. Dodaj testy weryfikujące poprawność metod: `equals(Object other)`, `toString()`, `precedes(Vector2d other)`, `follows(Vector2d other)`,
   `upperRight(Vector2d other)`, `lowerLeft(Vector2d other)`, `add(Vector2d other)`, `subtract(Vector2d other)`,
   `opposite()`.
   
7. W podobny sposób przetestuj także `OptionsParser`.

   Pamiętaj, że kod testów również powinien być czytelny i dobrze ustrukturyzowany. Warto zapoznać się z konwencją [Given When Then](https://www.j-labs.pl/blog-technologiczny/given-when-then-pattern-in-unit-tests/) i zawsze formułować przypadki testowe tak by jasno opisywały, co chcemy przetestować i jakie powinny być skutki badanej akcji.



## Przydatne informacje

* Atrybuty w obiekcie deklarowane są w ciele klasy, np. 
    ```java
    class Vector2d {
      private int x;
      private int y;
    }
    ```
* Konstruktor jest specjalną metodą w każdej klasie. Nazywa się tak samo jak klasa i nie zwraca wartości. Konstruktor 
  pozwala ustalić początkową wartość pól obiektu, jeśli mają być przekazane przez użytkownika, np.
    ```java
    class Vector2d {
      public Vector2d(int x, int y){
        this.x = x;
        this.y = y;
      }
    }
    ```
* Obiekty klasy tworzy się za pomocą wywołania `new`, np. 
    ```java
    Vector2d position1 = new Vector2d(1,2);
    ```
* Słowo kluczowe `this` odnosi się do obiektu, na rzecz którego wywołano metodę.
  Przykładowo w języku C moglibyśmy zdefiniować metodę `createPoint`:

    ```C
    struct Point {
      int x;
      int y;
    }
  
    struct Point * createPoint(int x, int y){
      struct Point * result = malloc(sizeof(struct Point));
      result->x = x;
      result->y = y;
      return result;
    }
  
    struct Point * p1 = createPoint(1,2);
    ```

    Ten kod jest analogiczny do konstruktora, z ta różnicą, że w konstruktorze nie tworzymy obiektu *explicite*, tylko mamy do niego dostęp za pomocą słowa kluczowego `this`.

* Metoda `equals` ma zwykle taki sam schemat:

    ```java
    public boolean equals(Object other){
      if (this == other)
        return true;
      if (!(other instanceof Vector2d))
        return false;
      Vector2d that = (Vector2d) other;
      // tutaj przeprowadzane jest faktyczne porównanie
    }
    ```

    Należy również pamiętać, że zmiana metody `equals` powinna powodować zmianę metody `hashCode`, w przeciwnym razie
umieszczenie obiektów w kolekcji takiej jak `Set` będzie niezgodne z semantyką metody `equals`.


* Definicję typu wyliczeniowego można rozszerzać dodając do niego pola i metody. Wymaga to umieszczenia średnika po ostatniej
  wartości typu, np.:
    ```java
    enum MapDirection {
      NORTH,
      SOUTH,
      EAST,
      WEST;
  
      public String toString(){
        return switch(this) {
          case NORTH -> "Północ";
          case SOUTH -> "Południe";
          //...
        }
      }
    }
    ```

* Metody testujące posiadają adnotację `@Test`.

* W metodach testujących można użyć m. in. następujących asercji:
  * `assertEquals(a, b)` - weryfikuje czy obiekty `a` i `b` są sobie równe (korzystając z metody `equals`),
  * `assertTrue(a)` - weryfikuje czy wartość logiczna `a` jest prawdą,
  * `assertFalse(a)` - weryfikuje czy wartość logiczna `a` jest fałszem.

