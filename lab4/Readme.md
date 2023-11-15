# Lab 4: Interfejsy i mapy

Celem laboratorium jest zapoznanie się z mechanizmem interfejsów oraz specjalnym typem struktury danych do przechowywania wielu obiektów - mapą.

Najważniejsze zadania:

1. Stworzenie klasy `RectangularMap` do przechowywania wielu zwierząt i ich pozycji z wykorzystaniem `HashMap`.
2. Zabezpieczenie kontraktów między mapą a zwierzęciem.
3. Konsolowa wizualizacja mapy podczas symulacji.
4. Testy integracyjne.

## Zadania do wykonania (4xp)

1. Przyjrzyj się interfejsom `WorldMap` oraz `MoveValidator`, które znajdują się w katalogu z tym konspektem i skopiuj je do swojego projektu. Skopiuj także klasę `MapVisualizer` - wykorzystasz ją w dalszych zadaniach. Umieść wszystkie klasy w odpowiednich pakietach, zgodnie z deklaracjami w ich nagłówkach.

2. W pakiecie `agh.ics.oop.model` zdefiniuj klasę `RectangularMap`.

3. Podstawowym atrybutem klasy `RectangularMap` powinna być struktura przechowująca zwierzęta na ich pozycjach. Do realizacji takiego odwzorowania wykorzystaj mapę (słownik). Z każdą aktualną pozycją zwierzęcia (`Vector2d`) powiążesz obiekt `Animal`. Deklaracja typu takiego atrybutu powinna wyglądać tak: `Map<Vector2d, Animal> animals = new HashMap<>();`.

4. Uzupełnij brakującą logikę w `RectangularMap` zgodnie z wytycznymi:

   * definiuje prostokątną mapę - posiada szerokość oraz wysokość,
   * implementuje interfejs `WorldMap`
   * w konstruktorze akceptuje dwa parametry `width` oraz `height` wskazujące szerokość oraz wysokość mapy (możesz założyć
     że otrzymane wartości są poprawne),
   * umożliwia poruszanie się w obrębie zdefiniowanego prostokąta (jak w laboratorium 3),
   * umożliwia występowanie więcej niż jednego zwierzęcia na mapie,
   * uniemożliwia występowanie więcej niż jednego zwierzęcia na tej samej pozycji,
   * posiada metodę `toString` rysującą aktualną konfigurację mapy (wykorzystaj klasę `MapVisualizer`, która znajduje się
     w tym katalogu).
     
     **Pamiętaj by zaimplementować wszystkie metody narzucone przez interfejs!**

5. Zmodyfikuj klasę `Animal`:

     * metoda `move()` powinna od teraz przyjmować `MoveValidator` i używać go do sprawdzania, czy zwierzę może zmienić swoją pozycję na wcześniej wyliczoną. Wykorzystaj ten argument podczas realizacji ruchu w metodzie  `RectangularMap.move()`.
     * zmodyfikuj metodę `toString` tak by zwracała jedynie schematyczną orientację zwierzęcia w postaci łańcucha składającego się z jednego znaku, Np. jeśli zwierzę ma orientację północną, to metoda `toString()` powinna zwracać łańcuch "N" albo "^".

6. Zmodyfikuj klasę `Simulation` tak by przyjmowała w konstruktorze również obiekt `WorldMap`. Następnie popraw realizację metody `run()` tak, by ruch odbywał się za pośrednictwem mapy. Po każdym ruchu wypisz aktualny stan mapy.
7. Dodaj testy integracyjne weryfikujące, że implementacja jest poprawna. 



## Zadanie dodatkowe (<img src="../img/reward_silver.png" alt="srebrna skrzynka" width="50" align="center" />)

Interfejs `WorldMap` zakłada, że mapa może przechowywać jedynie zwierzęta, a pozycje zawsze wyrażone są jako dwuwymiarowe pozycje. Te założenia można poluzować wprowadzając parametryzację i typy generyczne.

**Uwaga: to zadanie najlepiej robić na osobnym branchu i nie scalać go z `main` - może być trudne w utrzymaniu przy kolejnych laborkach. Najlepiej zacząć realizację zadania od stworzenia brancha `lab4-bonus` z brancha `lab4` (z miejsca, gdzie podstawowa część laborki jest już gotowa). W celu oddania zadania bonusowego wystarczy wtedy utworzyć pull request z `lab4-bonus` do `lab4`**

1. Zmodyfikuj interfejs `WorldMap` tak by mapa mogła przechowywać dowolne obiekty `T` na pozycjach typu `P`. Deklaracja typu powinna wyglądać tak: `WorldMap<T, P>`. Dostosuj do tego założenia wszystkie metody w interfejsie.
2. Popraw klasę `RectangularMap` tak by implementowała interfejs z odpowiednimi parametrami typów.
3. Ogranicz `Simulation` tak by nadal przyjmowało jedynie mapy zwierząt z dwuwymiarowymi pozycjami. 
4. Stwórz dodatkową implementację `TextMap`, która będzie przechowywała napisy `String` na pozycjach określonych w jednym wymiarze (liczba całkowita). Deklaracja typu dla takiej mapy to `WorldMap<String, Integer>`.  Mapa powinna spełniać założenia:
   - Mapa nie ma górnej granicy - dokładanie nowego napisu zawsze wstawia go na koniec mapy.
   - Przemieszczanie napisu jest możliwe jedynie w obecnych granicach `<0, N>` (gdzie `N` - liczba elementów w mapie). Przesuwany napis zamienia się miejscami z sąsiadem - w przypadku ruchu "na wschód" z sąsiadem z prawej (o indeksie o 1 wyższym), a "na zachód" z lewej. Np. dla mapy `["Ala", "ma", "sowoniedźwiedzia"]` przesunięcie napisu `"ma"` na wschód powinno dać efekt: `["Ala", "sowoniedźwiedzia", "ma"]`. Dalsze przemieszczanie wyrazu `"ma"` w prawo nie jest już możliwe. 
   - Napis może się przemieszczać do przodu i tyłu `FORWARD`/`BACKWARD` tylko w orientacjach `EAST`/`WEST`, w pozostałych przypadkach ruch jest ignorowany. 
5. Przetestuj nową implementację mapy.

## Przydatne informacje

* Mechanizm interfejsów pozwala na określenie pewnego zestawu metod, które muszą być implementowane przez określony typ.
  Interfejs `WorldMap` jest tego przykładem - określa on sposób interakcji mapy z zwierzętami oraz klasą `MapVisualizer`. Zarówno interfejsy, jak i klasa są do pobrania z folderu z konspektem - wykorzystasz je w swoim projekcie.

* Interfejs określa jedynie, że dana klasa ma posiadać określoną metodę - dlatego w interfejsie nie ma implementacji - wszystkie metody są
  z założenia abstrakcyjne (można pominąć modyfikator `abstract`).

* W interfejsie wszystkie metody są z założenia publiczne, dlatego nie ma potrzeby dodania modyfikatora dostępu
  `public`. Od Javy 9 interfejs może posiadać także metody prywatne. Od Javy 8 interfejsy mogą posiadać metody statyczne (takie same jak metody statyczne w klasach) oraz metody domyślne (oznaczane modyfikatorem `default`), które posiadają implementację.

* Podstawianie obiektów realizujących interfejs pod deklaracje metod i zmiennych wymagających interfejsu jest możliwe, bo w Javie działa **polimorfizm**. Przykładowo:

  ```java
  MoveValidator validator = new RectangularMap(10, 10); // RectangularMap pośrednio realizuje MoveValidator 
  validator.canMoveTo(new Vector2d(1, 2)); // wywołanie poprawne, Java wywoła implementację metody z RectangularMap
  // validator.place(animal);  wywołanie niepoprawne, kod się NIE skompiluje! MoveValidator nie ma metody place()
  ```

* Klasa deklaruje fakt implementacji interfejsu za pomocą słowa kluczowego `implements`, np. 

  ```java
  class RectangularMap implements WorldMap {
  }
  ```

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
