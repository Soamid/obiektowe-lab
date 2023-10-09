

# Lab 7: Wielowątkowość 

Celem laboratorium jest zapoznanie się z wątkami i innymi narzędziami do obsługi współbieżnych operacji w Javie.

Najważniejsze zadania:

1. Stworzenie `SimulationEngine`, który pozwoli uruchamiać symulacje asynchronicznie.
1. Rozwiązanie problemu z *race condition*.
1. Zastosowanie puli wątków do efektywnego zarządzania symulacjami.


## Zadania do wykonania (4xp)

### Uruchamianie wielu symulacji synchronicznie

1. Uruchomienie wielu symulacji jednocześnie sprawi, że ciężko będzie odróżnić na konsoli wyniki wypisywane przez nich wyniki. Dlatego konieczne jest dodanie do modelu mapy unikalnego identyfikatora - w interfejsie `WorldMap` zadeklaruj metodę `getId()` (może zwracać `int`, `String` lub - jeśli chcemy być bardzo profesjonalni -`UUID`). Dostarcz również konieczną implementację. Pamiętaj o zmodyfikowaniu konstruktorów obu map i zastanów się, gdzie umieścić atrybut tak, by nie powtarzać kodu. 
2. W `ConsoleMapDisplay` dodaj wypisywanie id mapy.
3. Stwórz klasę `SimulationEngine`. Powinna ona w konstruktorze przyjmować listę obiektów `Simulation`.
4. W utworzonej klasie zdefiniuj metodę `runSync()`, która uruchomi po kolei wszystkie symulacje. 
5. Sprawdź działanie programu dla dwóch symulacji (jednej dla `RectangularMap`, drugiej dla `GrassField`). Możesz użyć parametry symulacji z poprzednich laboratoriów (dobrze by lista ruchów miała przynajmniej kilkanaście pozycji).

### Uruchamianie wielu symulacji asynchronicznie

1. Dodaj do `SimulationEngine` metodę `runAsync()`. Metoda powinna:

   1. Stworzyć osobny wątek dla każdego obiektu `Simulation`.
   2. Uruchomić stworzony wątek.

   **Uwaga:** Co należy zrobić by klasa `Thread` mogła przyjąć w konstruktorze obiekt `Simulation`?

2. Ponownie przetestuj działanie programu na dwóch symulacjach, tym razem dla metody `runAsync()`. Zwróć uwagę na kolejność wypisywanych aktualizacji mapy - czy różni się ona od efektu wywołania `runSync()`?

3. Zwróć uwagę, w którym miejscu pojawia się teraz napis `System zakończył działanie` (w razie potrzeby dodaj wypisywanie takiego napisu w ostatniej linii `main()`). 

4. Dodaj do klasy `SimulationEngine` metodę `awaitSimulationsEnd()` i zastanów się, w jaki sposób zmodyfikować przygotowany mechanizm wątków tak, by wywołanie tej metody blokowało wątek, z którego została wywołana do momentu zakończenia  wszystkich symulacji.

### *Race conditions*

1. Zmodyfikuj metodę `main()` tak by zamiast dwóch symulacji tworzyła ich bardzo wiele w pętli (rzędu 1000 lub więcej - możesz eksperymentować z różnymi wartościami).
2. Uruchom program kilka razy i prześledź wypisywane stany mapy oraz w szczególności końcowe wartości liczników aktualizacji. Jakie problemy obserwujesz? Z czego to wynika?
3. Dokonaj koniecznych modyfikacji w programie tak by zapewnić, że każda aktualizacja mapy zostanie poprawnie i w całości wypisana.

### Pula wątków

1. W `SimulationEngine` napisz dodatkową metodę `runAsyncInThreadPool()`.
2. Zaimplementuj metodę tak by symulacje (niezależnie od ich liczby) były uruchamiane w puli 4 wątków. Skorzystaj z `ExecutorService` i pomocniczej klasy `Executors`. 
3. Zmodyfikuj metodę `awaitSimulationsEnd()` tak by kończyła również działanie puli wątków i oczekiwała na zakończenie jej pracy maksymalnie 10 sekund.
4. Uruchom program i sprawdź czy działa. Warto też porównać zużycie procesora w wariancie z pulą i bez niej. Dlaczego zastosowanie puli wątków jest lepszym pomysłem niż poprzednie podejście?

## Przydatne informacje

* Mechanizm wątków służy do realizacji zadań, które powinny być realizowane **współbieżnie**, a jeśli system posiada wiele procesorów, to również **równolegle**. Wykonujące wątki nawzajem nie blokują swego wykonania.

* Przykładami takich operacji może być np. dostęp do zasobu sieciowego, uruchamianie ciężkich obliczeń albo logika wykonywana w celu późniejszego zaktualizowania interfejsu graficznego.

* Domyślnie aplikacja posiada jeden wątek, który może stworzyć dowolnie wiele dodatkowych.

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

- Metody w Javie mogą posiadać dodatkowy modyfikator `synchronized`. Gwarantuje on, że dana metoda zostanie zawsze wywołana w całości przez dany wątek, zanim zostanie do niej dopuszczony inny, współbieżnie pracujący wątek. 

- Słowo `synchronized` występuje również w formie bloku, pozwalającego wydzielać tzw. sekcje krytyczne jako fragment metody, np:

  ```java
  // kod wywoływany współbieżnie
  synchronized(this) {
      // sekcja krytyczna - tylko jeden wątek naraz tu wejdzie
  }
  // kod wywoływany współbieżnie
  ```
  
- Zarządzanie wątkami ręcznie nie jest efektywne i wygodne. W profesjonalnych zastosowaniach lepiej stosować narzędzia, które robią to za nas, takie jak pule wątków. W Javie do zarządzania pulami służy `ExecutorService`:

  ```javaa
   ExecutorService executorService = Executors.newFixedThreadPool(8);
   executorService.submit(runnable); // task runanble wywoła się na jednym z 8 wątków lub poczeka, aż któryś się zwolni
  ```

  
