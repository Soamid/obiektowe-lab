# Lab bonus 1: Elementy funkcyjne w Javie i zarządzanie zasobami

Ćwiczenia mają na celu wprowadzenie dodatkowych narzędzi usprawniających jakość kodu i redukujących jego ilość. Pierwsza część poświęcona została elementom funkcyjnym, wprowadzonym do języka Java w wersji 1.8. 
Druga część związana jest zarządzaniem zewnętrznymi zasobami, takimi jak pliki czy połączenia.

Zrealizowanie zadań może być **bardzo pomocne przy realizacji projektu**.

## Zadania do wykonania

### Część I: Elementy funkcyjne w Javie (<img src="../img/reward_silver.png" alt="srebrna skrzynka" width="50" align="center" />)

####  Wyrażenia lambda

1. Zwróć uwagę, że `MapChangeListener` posiada tylko jedną metodę abstrakcyjną. Oznacza to, że może być on traktowany jako interfejs funkcyjny. Oznacz go w odpowiedni sposób używając dedykowanej adnotacji.
2. W kodzie inicjującym mapy i symulacje dodaj do każdej mapy dodatkowego obserwatora, który w reakcji na zmianę mapy wypisze jedynie informację, co się zmieniło (`message`) wraz z aktualną datą i czasem. 
   - Obserwator nie powinien być osobną klasą, a jedynie wyrażeniem lambda, które zdefiniujesz w miejscu konfiguracji mapy.  
   - Przykładowy format wypisywanej wiadomości: `2023-11-07 10:43:22 Animal was moved from position (1, 2) to position (1, 3)`.
3. Do interfejsu `WorldMap` dodaj metodę `getOrderedAnimals()`. Powinna ona z założenia zwracać kolekcję wszystkich zwierząt na obecnej mapie, posortowanych po pozycjach - tak by najpierw wziąć pod uwagę wiersz (współrzędna `x`), a potem kolumnę (współrzędna `y`).
   - Do sortowania wykorzystaj wbudowaną metodę pomocniczą `Collections.sort()` .
   - Przy sortowaniu zdefiniuj odpowiedni komparator. Warto wykorzystać tutaj wyrażenia lambda lub tzw. [referencje do metod](https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html). W tym celu sprawdź działanie pomocniczego interfejsu [`Comparator.comparing(...)`](https://www.baeldung.com/java-8-comparator-comparing).
     **Uwaga:** można podejść do tego tematu na kilka sposobów, np. posortować od razu zwierzęta albo najpierw pozycje, a potem na ich podstawie wyciągnąć zwierzęta. 
4. Przetestuj działanie napisanej metody (dodaj odpowiedni test jednostkowy).

#### Strumienie

1. Przyjrzyj się klasie `OptionsParser`. Zwróć uwagę, że jej logika opisuje transformację kolekcji elementów jednego typu na kolekcję elementów innego typu. Wykorzystaj mechanizm strumieni i znajdź odpowiedni operator, który pozwoli zrealizować taką transformację.
   **Wskazówka**: punktem wejścia jest tablica elementów, więc rozpocznij strumień od `Stream.of(...)` i przekaż do niego tablicę. Pamiętaj, że na wyjściu powinna nadal być lista (a nie `Stream<MoveDirection>`), należy więc też zdefiniować odpowiednią operację terminalną!
2. Użyj strumieni by uprościć metodę `getElements()` w klasie `GrassField`. 
   **Wskazówka:**  Wykorzystaj operator `Stream.concat()` i pamiętaj, że z każdej kolekcji możesz utworzyć strumień, np. `animals.stream()`.
3. Użyj strumieni aby uprościć przygotowaną wcześniej metodę `getOrderedAnimals()`. Wykorzystaj operator strumieni `sorted(Comparator)`

#### Optional

1. Metoda `WorldMap.objectAt()` może zwrócić `null` w momencie, gdy na danej pozycji nie będzie żadnego obiektu. Przerób interfejs w taki sposób by metoda zwracała `Optional<WorldElement>` i nigdy nie podawała `null`. 
2. Obsłuż wszystkie miejsca, w których Twój program wywoływał `objectAt()`. Pamiętaj, że `Optional`, podobnie jak strumienie, posiada szereg operatorów przetwarzających jego zawartość bez "rozpakowywania" obiektu (np. operator `map()`, `filter()` czy `or()`). 

### Część II : Zarządzanie zasobami (<img src="../img/reward_silver.png" alt="srebrna skrzynka" width="50" align="center" />)

#### Zapisywanie logów do pliku

1. Dodaj kolejnego obserwatora mapy - tym razem niech to będzie klasa `FileMapDisplay`.
2. W reakcji na modyfikację mapy, obserwator powinien otwierać plik o nazwie `map_id.log` (gdzie `id` to identyfikator mapy) i dopisywać do niego na koniec informacje o ruchu i aktualnym wyglądzie mapy. 
3. Pamiętaj by poprawnie obsługiwać zamykanie pliku i ewentualne błędy. Użyj w tym celu mechanizmu `try-with-resources`. 

#### Tekstury dla mapy

Wczytywanie plików może być przydatne również na potrzeby UI. Do tej pory każdy z elementów mapy był reprezentowany tekstowo. Teraz z każdym z rodzajów elementów na mapie skojarzymy jeden lub więcej obrazków (tekstur). Możesz wykorzystać obrazki dostarczone razem z tym konspektem lub podpiąć dowolne własne.

1. Stwórz albo wykorzystaj gotowe 4 tekstury z informacją o orientacji dla zwierzaka (folder `resources`)
2. Stwórz albo wykorzystaj teksturę dla trawy.
3. Dodaj utworzone tekstury do folderu `src/main/resources`
4. Utwórz klasę `WorldElementBox`, która pozwoli na dodanie obrazka do siatki:
    * utwórz instancję klasy `Image`,
    * zainicjuj za jej pomocą obiekt `ImageView`,
    * ustal jego rozmiary na 20 x 20,
    * utwórz etykietę informującą o pozycji zwierzaka,
    * uwtórz obiekt *vertical box* (`VBox`) do którego dodasz oba obiekty (obrazek i etykietę),
    * wyśrodkuj elementy wewnątrz kontenera.
5. Dodaj do interfejsu `WorldElement` metody pozwalające na pobranie nazwy zasobu odzwierciedlającego wygląd danego elementu (czyli np.
   `up.png`, jeśli zwierzę zwrócone jest na północ). Zaimplementuj je w klasach implementujących ten interfejs.
6. Wykorzystaj powyższe metody w konstruktorze klasy `WorldElementBox`, który powinien przyjmować instancję `WorldElement` i wyświetlać reprezentację elementu. Upewnij się, że elementy te nie są niepotrzebnie tworzone wielokrotnie.
7. Zamień reprezentację tekstową na graficzną w prezenterze widoku.
8. Docelowy wygląd:<br>
![look2](img/preview.png)



## Przydatne informacje

#### Wyrażenia *lambda*

W Javie 8 wprowadzono do języka wiele elementów tzw. **języków funkcyjnych** (np. Scala, Python, Haskell). Co prawda podejście funkcyjne znacznie różni się od obiektowego, a jak wiemy Java jest w pełni obiektowo-orientowanym językiem, ale okazuje się, że mechanizm klas anonimowych może posłużyć za pomost między tymi podejściami i punkt wyjścia dla wielu uproszczeń. Jeśli założymy, że dany interfejs posiada tylko jedną metodę, można powiedzieć że jego anonimowe realizacje będą tworzyć tak naprawdę obiektowe reprezentacje tej metody. 
Jeśli taki interfejs zostanie oznaczony adnotacją `@FunctionalInterface`, możemy dla jego realizacji stosować znacznie prostszą składnię, symulując tzw. **wyrażenie lambda**:

```java
Button okButton = new Button();
okButton.setAction(event -> {
    System.out.println("Kliknieto ok!");
  });

Button cancelButton = new Button();
cancelButton.setAction(event -> {
    System.out.println("Kliknieto anuluj!");
});
```
Powyższy kod jest dokładnym odpowiednikiem zapisu z klasą anonimową:

```java
Button okButton = new Button();
okButton.setAction(new Action() {
  public void actionPerformed(Event event) {
    System.out.println("Kliknieto ok!");
  }
});

Button cancelButton = new Button();
cancelButton.setAction(new Action() {
  public void actionPerformed(Event event) {
    System.out.println("Kliknieto anuluj!");
  }
});
```

Jak widać na powyższym przykładzie, zniknęły sygnatury metod `actionPerformed`, a także nazwa klasy - Java sama domyśli się, czy to, co przekazujemy do `setAction` jest poprawne. Co więcej, nie musimy nawet określać typu danych argumentów (`Event`). Wygląda to tak, jakbyśmy przekazywali do metody funkcję, która powinna się w tym miejscu wykonać - i dokładnie tak należy to rozumieć, to właśnie jest wyrażenie lambda!

#### Strumienie

W Javie istnieje bogaty zestaw kolekcji, które pozwalają przechowywać dane na wiele sposobów. Problem w tym, że składnia języka sprawia, że wszelkie przekształcenia takich kolekcji zajmują sporo miejsca i mogą być dość frustrujące, szczególnie że wykonujemy je bardzo często. Weźmy przykład:
```java
private List<Integer> filterOdd(List<Integer> numbers) {
   List<Integer> resultList = new ArrayList<>();
   for(Integer number : numbers) {
      if(number % 2 == 1) {
         resultList.add(number);
      }
   }
   return resultList;
}
```
W tym konkretnym przypadku chcemy dostać listę, na której znajdują się tylko nieparzyste wartości z podanej listy liczb. Ale schemat zawsze jest podobny: tworzymy nową kolekcję, w pętli wykonujemy operacje na starej i przekładamy odpowiednie wartości do nowej.

W Javie 8 tego typu operacje możemy zrealizować znacznie prościej. Każdą kolekcję możemy przekształcić na tzw. **strumień** wywołując na niej metodę `stream()`. Strumień pozwala na asynchroniczne przetwarzanie kolejnych elementów, które w nim się pojawiają. Między "źródłem" a "ujściem" strumienia możemy wstawić dowolne operatory przekształcające. W tym przypadku wywołamy jedną z najbardziej typowych operacji - **filter**:
```java
private List<Integer> filterOdd(List<Integer> numbers) {
   return numbers.stream()
           .filter(number -> number % 2 == 1)
           .collect(Collectors.toList());
}
```
Kod powyższego przykładu jest równoważny do przykładu z początku tej sekcji. Definiujemy tu dwa ogniwa łańcucha strumienia: najpierw mówimy w jaki sposób będziemy filtrować kolejne elementy (`filter`), a potem w jaki sposób będziemy je zbierać gdy już przepłyną przez cały łańcuch (`collect`). Zwróćmy uwagę, że również tutaj zastosowaliśmy wyrażenie lambda - oczywiście moglibyśmy zamiast tego stworzyć anonimową klasę albo nawet nazwaną klasę... ale chodzi o to, by zapis był jak najprostszy. A powyższy kod jest znacznie czytelniejszy niż jego oryginalna wersja. 
Uwaga: w przypadku strumieni również nietrudno wygenerować ciężką do zrozumienia plątaninę wywołań. Jak zawsze należy zachować umiar, wdzięk i elegancję oraz trzymać się zasady: "każde nowe ogniwo łańcucha piszemy w osobnej linii".

Oczywiście operatorów funkcyjnych jest znacznie więcej. Najbardziej popularne to **map**, **filter** i **reduce** (z tymi nazwami spotkamy się we wszystkich językach funkcyjnych). Operatory można dowolnie składać i tworzyć znacznie bardziej złożone łańcuchy. Rozszerzmy nieco nasz przykład:
```java
private List<String> filterOdd(List<Integer> numbers) {
   return numbers.stream()
           .filter(number -> number % 2 == 1)
           .distinct()
           .map(number -> "Numer " + number)
           .collect(Collectors.toList());
}
```
Każdy element z listy `numbers` zostanie sprawdzony pod kątem nieparzystości. Jeśli przejdzie test, może zostać odsiany jako duplikat (`distinct` zapewnia tylko unikalne wartości). Jeśli jednak przejdzie dalej, zostanie przekształcony na `String` o wartości "Numer X" (gdzie X to wartość liczbowa). 

Jako rezultat otrzymamy więc tym razem listę Stringów. Jeśli zamiast tego chcielibyśmy od razu wykonać jakąś operację na każdym z elementów (np. wypisać) to moglibyśmy użyć `forEach`, odpowiednika pętli:

```java
private void filterOdd(List<Integer> numbers) {
   numbers.stream()
           .filter(number -> number % 2 == 1)
           .distinct()
           .map(number -> "Numer " + number)
           .forEach(numberStr -> System.out.println(numberStr));
}
```

Takie złożone strumienie mają wiele innych zalet. Podstawowa jest taka, że nie tworzymy tu nigdzie kolekcji pośrednich, a więc dużo oszczędzamy na pamięci. Więcej o strumieniach i operatorach można znaleźć w oficjalnej dokumentacji i licznych tutorialach, np. [tutaj](http://www.oracle.com/technetwork/articles/java/ma14-java-se-8-streams-2177646.html).

#### Optional

Typ `Optional` wprowadzono w Javie 8 żeby usprawnić kontrolę nad sytuacjami gdy metoda może zwrócić `null`. Normalnie nic nas nie chroni przed nullami - jeśli nie spodziewamy się, że gdzieś pojawi się `null` i nie sprawdzimy tego to najprawdopodobniej prędzej czy później zobaczymy ulubiony komunikat programistów Javy, czyli `NullPointerException`. Stanie się tak, gdy spróbujemy wywołać na zmiennej wskazującej na `null` dowolną metodę. Optionale mają za zadanie przede wszystkim uświadamiać nas, że dana metoda może nic nie zwrócić i należy taką sytuację obsłużyć. Mają też szereg metod podobnych do tych, które znamy ze Streamów (np. `map()`, `filter()`), co sprawia że często nawet nie musimy w ogóle takiego optionala rozpakowywać żeby przeprowadzić operacje na danych, które przechowuje.

Ogólna zasada brzmi: **jeśli metoda może zwrócić null, powinna zamiast tego zwrócić `Optional`**. Jeśli będziemy trzymać się tej zasady, nasze programy staną się znacznie czytelniejsze i bardziej odporne na nasze błędy.

```java
return Optional.ofNullable(nullableExpression); // tworzy optionala w oparciu o wyrażenie, które może być null
return Optional.of(expression); // tworzy optionala w oparciu o wyrażenie, które NIE może być null. Rzuca NPE gdy podamy tu null.
```

### Zarządzanie zasobami

Java posiada wiele różnych narzędzi do obsługi plików (czy w ogólności: zewnętrznych źródeł danych). Istnieje tutaj podstawowa klasa `File`, ale jest ona jedynie deskryptorem pliku - sama w sobie nie pozwoli otworzyć czy zapisać czegoś do pliku. Żeby przetwarzać dane w plikach powinniśmy skorzystać z jednego z narzędzi:

- `FileWriter` / `FileReader`
- `PrintWriter` / `Scanner`
- `FileOutputStream` / `FileInputStream`

#### Obsługa błędów i zamykania pliku

Jeśli w trakcie analizy danych wczytanych z pliku poleciałby błąd i przerwalibyśmy przez to pracę z plikiem, plik pozostałby otwarty i zablokowany dla innych aplikacji. Niezależnie od sytuacji chcielibyśmy **zawsze** wywołać metodę `close()`. W tym celu pomoże nam blok `finally`:

```java
 File file = new File("data.txt");
 try {
    Scanner scanner = new Scanner(file);
    // analiza danych z scannera, tu leci blad
 } catch (Exception e) {
    e.printStackTrace();
 } finally {
    scanner.close();
 } 
```
Blok `finally` występuje w kombinacji z `try` lub nawet `try..catch..finally` i daje nam gwarancję, że kod, który w nim zawrzemy wykona się zawsze. Oczywiście nie uchroni nas przed naszymi błędami - w trakcie wykonywania`finally` również możemy doprowadzić do błędu...

#### Try-with-resources

Ponieważ opisana wyżej sytuacja jest bardzo typowa i jednocześnie bardzo łatwo zapomnieć o zamykaniu zasobów, w późniejszych wersjach Javy wprowadzono dodatkowy mechanizm  **kontekstowego zarządzania zasobami**. Omawiany wyżej przykład  możemy zapisać również w taki sposób:

```java
 try(Scanner scanner = new Scanner(file)) {
     // analiza danych z scannera, tu leci blad
 } catch (Exception e) {
     e.printStackTrace();
}
```

Zwróćmy uwagę, że tym razem nie wywołaliśmy nigdzie metody `close()`. Nie oznacza to, że strumień nie zostanie zamknięty - wręcz przeciwnie, metoda `close()` zostanie wywołana automatycznie, gdy tylko zakończy się blok kodu między klamerkami `{ }` (lub gdy poleci wyjątek!). 

Jak to działa? Podobnie jak w przypadku np. porównywania wartości elementów podczas sortowania nie ma tu żadnej magii. Jeśli chcemy by nasza własna klasa również mogła zostać użyta w takim specjalnym wyrażeniu `try()`, musi jedynie zadeklarować realizację interfejsu `Autocloseable`. Jeśli przyjrzymy się temu interfejsowi to zauważymy, że wymaga on zrealizowania tylko jednej metody:

```java
void close() throws Exception;
```

Blok `try-with-resources` może posiadać bloki `catch` lub `finally` jak normalny `try`... ale nie musi. Poniższy kod jest poprawny.

```java
 try (Scanner scanner = new Scanner(file)) {
     // analiza danych z scannera, tu leci blad
 }
```
