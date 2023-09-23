# Laboratorium 8

Celem laboratorium jest zapoznanie się z mechanizmem wątków oraz obsługą zasobów w kontekście programowania 
graficznego interfejsu użytkownika (GUI).

Treść laboratorium powstała we współpracy z Norbertem Morawskim.

Najważniejsze zadania:

1. Dodanie obsługi tekstur.
2. Dodanie wątku symulacyjnego.
3. Guzik uruchamiający symulację.

## Przydatne informacje

### Wątki

* Mechanizm wątków służy do realizacji zadań, które powinny być realizowane współbieżnie, a jeśli system posiada wiele
  procesorów, to również równolegle. Wykonujące wątki nawzajem nie blokują swego wykonania.
  Dzięki temu możliwe jest np. reagowanie na zdarzenia GUI (np. kliknięcie na guzik) oraz wykonywanie obliczeń, które
  sterują tym co wyświetla się w GUI.
* Przykładami takich operacji może być np. dostęp do zasobu sieciowego albo w naszym przypadku sztucznie generowane opóźnienie pomiędzy ruchami.
* Wątek UI jest to główny wątek aplikacji w graficznym interfejsem użytkownika. Tylko ten wątek może modyfikować zawartość sceny w *JavaFX*.
* Aby stworzyć wątek możemy skorzystać z klasy `Thread` i interfejsu `Runnable`.
    ```java
    class SimulationEngine implements Runnable {
        @Override
        public void run() {
            System.out.println("Thread started.");
        }
    }
    
    SimulationEngine engine = new SimulationEngine();
    Thread engineThread = new Thread(engine);
    engineThread.start();
    ```
### JavaFX
* JavaFX to [framework](https://pl.wikipedia.org/wiki/Framework) do obsługi środowiska graficznego.
* Aplikacja JavaFX składa się z:
  * `Stage` - okno aplikacji,
  * `Scene` - aktualna zawartość aplikacji (np. ekran symulacji, ekran podsumowania)
  * Scena zawiera wiele instancji `Node`. Są nimi m.in. przyciski, pola tekstowe, kontenery (`VBox`, `HBox`, `GridPane`, itp.).
* Główna klasa w aplikacji powinna dziedziczyć po `Application` i implementować metodę `start()`.
* Minimalna aplikacja powinna stworzyć jedną scenę, przypiąć ją do `Stage` i wyświetlić.

### Dostęp do zasobów

* Zasoby w projekcie, takie jak obrazy graficzne zwykle umieszczane są w katalogu `src/main/resources`.
* Odczytanie zasobu możliwe jest np. za pomocą strumienia `java.io.FileInputStream`.
* Dane binarne zawierające obraz można wczytać do obiektu `javafx.scene.image.Image`.
* Wyświetlenie obrazka odbywa się za pomocą obiektu `javafx.scene.image.ImageView`.
* Zasoby takie jak `ImageView` są pamięciochłonne, dlatego ważne jest aby nie były one tworzone bez potrzeby. 
  W modelu pamięciowym JavaFX elementy, które nie należą do drzewa węzłów są usuwane przez śmieciarza (GC),
  ale ich tworzenie samo w sobie jest czasochłonne, co może istotnie spowalniać działanie aplikacji.
* Przykładowy kod służący do utworzenia obrazka, który można dodać do sceny JavaFX:
    ```java
    Image image = new Image(new FileInputStream("src/main/resources/up.png"));
    ImageView imageView = new ImageView(image);
    imageView.setFitWidth(100);
    imageView.setFitHeight(100);
    ```
  Uwaga: w profesjonalnych aplikacjach zasoby znajdujące się w `src/main/resources` odczytujemy nieco inaczej, korzystając z tzw. class loadera: `getClass().getResourceAsStream("up.png")`. Nie musimy wówczas podawać ścieżki do obrazka, co uniezależnia nas od tego, czy jest on uruchamiany za pośrednictwem IntelliJ czy jako zbudowana, produkcyjna aplikacja. Żeby to zadziałało, zasób musi być umieszczony w takim samym pakiecie jak klasa, z której jest wczytywany. Przykładowo: jeśli ładujemy obrazek z klasy `agh.ics.oop.gui.App` to zasób powinien się on znajdować w `src/main/resources/agh/ics/oop/gui`. 

## Zadania do wykonania

### Szkielet aplikacji JavaFX

1. W `build.gradle`:

   1. Dodaj `id 'org.openjfx.javafxplugin' version '0.0.13'` do sekcji `plugins`.

   2. Dodaj pod `repositories` sekcję:

      ```gradle
      javafx {
          version = "17"
          modules = [ 'javafx.controls' ]
      }
      ```

2. Utwórz nowy pakiet `agh.ics.oop.gui`.

3. Odśwież konfigurację Gradle'a (`Ctrl+Shift+O`).

4. Utwórz klasę `App` dziedziczącą z `Application` z pakietu `javafx.application`.

5. Zaimplementuj metodę `public void start(Stage primaryStage)`.

   * Będzie to metoda uruchamiająca interfejs graficzny Twojej aplikacji.
   * Na razie możesz w ciele metody wpisać `primaryStage.show();`. Wyświelti to puste okno aplikacji.

6. W metodzie `main` w `World` dodaj `Application.launch(App.class, args);`

   * Spowoduje to uruchomienie okna JavaFX.

7. Zobacz czy okno pokazuje się (może być nieresponsywne, ale powinno się pokazać).
   *Pamiętaj, żeby importować brakujące klasy z pakietu `javafx`*

### Wyświetlanie mapy w GUI

1. W klasie `App` dodaj:

   ```java
   Label label = new Label("Zwierzak");
   Scene scene = new Scene(label, 400, 400);
   
   primaryStage.setScene(scene);
   primaryStage.show();
   ```

   Spowoduje to:

   * Utworzenie nowej etykiety z treścią `Zwierzak`.
   * Utworzenie sceny zawierającej tylko obiekt `Label`, z wymiarami 400 x 400 pikseli.
   * Ustawienie sceny jako aktywnej.
   * Pokazanie okna aplikacji.

2. Zmodyfikuj kod tak, aby zamiast pojedynczej etykiety wyświetlał siatkę z etykietami.

   * *Wskazówka:* Skorzystaj z klasy [`GridPane`](http://tutorials.jenkov.com/javafx/gridpane.html). Przydatne może być ustawienie `grid.setGridLinesVisible(true);`.

3. Przenieś kod inicjalizacyjny mapy z klasy `World` do `App` (możesz skorzystać z metody `init()`).

4. Użyj `getParameters().getRaw()` żeby odczytać parametry linii komend.

5. Wykorzystaj `AbstractWorldMap`, tak aby siatka odpowiadała rozmiarom i orientacji mapy. Pamiętaj, że współrzędne `y` rosną w kierunku "do góry", w `GridPane` jest na odwrót.

6. Dodaj w pierwszej kolumnie i w pierwszym rzędzie wartości współrzędnych (podobnie jak robi to `MapVisualizer`).

7. Do pozostałych wierszy dodaj obiekty z mapy.

8. Dodaj do siatki rozmiary kolumn i wierszy. Skorzystaj z:

   * `grid.getColumnConstraints().add(new ColumnConstraints(width));`
   * `grid.getRowConstraints().add(new RowConstraints(height));`

9. Wyśrodkuj etykiety korzystając z wywołania `GridPane.setHalignment(label, HPos.CENTER)`.

10. Aktualnie, twój program powinien wyglądać mniej więcej tak (użyto mapy `GrassField`, dodano 2 zwierzaki):<br>
    ![look1](img/look1.png)

### Tekstury

1. Stwórz albo wykorzystaj gotowe 4 tekstury z informacją o orientacji dla zwierzaka (folder `resources`)
2. Stwórz albo wykorzystaj teksturę dla trawy.
3. Dodaj utworzone tekstury do folderu `src/main/resources`
4. Utwórz klasę `GuiElementBox`, która pozwoli na dodanie obrazka do siatki:
    * utwórz instancję klasy `Image`,
    * zainicjuj za jej pomocą obiekt `ImageView`,
    * ustal jego rozmiary na 20 x 20,
    * utwórz etykietę informującą o pozycji zwierzaka,
    * uwtórz obiekt *vertical box* (`VBox`) do którego dodasz oba obiekty (obrazek i etykietę),
    * wyśrodkuj elementy wewnątrz kontenera.
5. Dodaj do interfejsu `IMapElement` metody pozwalające na pobranie nazwy zasobu odzwierciedlającego wygląd danego elementu (czyli np.
   `src/main/resources/up.png`, jeśli zwierzę zwrócone jest na północ). Zaimplementuj je w klasach implementujących ten
   interfejs.
6. Wykorzystaj powyższe metody w konstruktorze klasy `GuiElementBox`, który powinien przyjmować instancję `IMapElement`
   i wyświetlać reprezentację elementu. Upewnij się, że elementy te nie są niepotrzebnie tworzone wielokrotnie.
7. Zamień reprezentację tekstową na graficzną w klasie `App`.
8. Docelowy wygląd:<br>
![look2](img/look2.png)

### Wątek symulacyjny
1. Skorzystaj ze wzroca *Observer*, aby informować o zmianach położenia zwierzą moduł GUI. Zastanów się, czy lepiej
   połączyć za jego pomocą zwierzęta i GUI, czy może silnik symulacyjny.
2. W klasie `App` obsłuż aktualizację stanu mapy. Wyczyść siatkę wywołując `grid.getChildren().clear()` i 
   wyrenderuj ją od nowa wyświetlając aktualne pozycje roślin i zwierząt. Wykonanie na wątku UI można osiągnąć przy użyciu wywołania `Platform.runLater(() -> { ... })`.
    * **Dla zaawansowanych:** Spróbuj zopytmalizować aktualizacje siatki aby nie była ona tworzona od nowa za każdym razem.
4. Dodaj pole `moveDelay` które będzie służyć do opóźniania sekwencji ruchów zwierząt (aby widzieć zmiany na żywo).
    * Zastanów się kiedy ustawić wartość tego pola.
5. Opóźnienie pomiędzy ruchami dodaj za pomocą `Thread.sleep(300)` (usypia wątek na 300 ms). 
   Umieść `sleep()` w bloku `try-catch` i wypisz stosowny komunikat w razie przerwania symulacji.
6. Zaimplementuj interfejs `Runnable` przez `SimulationEngine`.
7. W metodzie `init()` GUI stwórz nowy wątek używając `SimulationEngine` jako parametru. Uruchom wątek metodą `start()`. 
   Pamiętaj o ustawieniu `moveDelay` na np. 300 [ms].

### Inne elementy interfejsu
1. Dodaj do interfejsu pole tekstowe i przycisk start. Skorzystaj z klas [`HBox`](http://tutorials.jenkov.com/javafx/hbox.html), [`VBox`](http://tutorials.jenkov.com/javafx/vbox.html), [`Button`](http://tutorials.jenkov.com/javafx/button.html) i [`TextField`](http://tutorials.jenkov.com/javafx/textfield.html).
2. Utwórz setter dla pola `directions` w `SimulationEngine` tak, aby dało się je dynamicznie zmieniać 
   przy naciśnięciu przycisku. Utwórz konstruktor który nie ustawia tego pola.
4. Usuń `engineThread.start()` z metody `init()`.
5. Dodaj obsługę kliknięcia *Start* (użyj `setOnAction`). Odczytaj wartość pola tekstowego (`getText()`) i użyj jego zawartości w parserze. 
   Ustaw nową sekwencję ruchów i uruchom **za każdym razem** nową instancję `Thread`.
