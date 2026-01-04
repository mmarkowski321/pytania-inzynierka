# Ocena Złożoności Algorytmów — Kompletna Notatka do Obrony

## 1) Wprowadzenie — po co oceniać złożoność?

### Dlaczego złożoność algorytmów jest ważna?

**Złożoność algorytmu** mówi nam, jak szybko rośnie czas wykonania lub zużycie pamięci wraz ze wzrostem rozmiaru danych wejściowych.

### Podstawowe pytania:
- Czy algorytm będzie działał szybko na dużych danych?
- Ile pamięci będzie potrzebował?
- Czy istnieje lepszy algorytm?
- Czy rozwiązanie jest praktyczne dla danych problemów?

### Przykład problemu:

Dwa algorytmy wyszukiwania:
- **Algorytm A:** O(n) — czas rośnie liniowo
- **Algorytm B:** O(n²) — czas rośnie kwadratowo

Dla n = 1000:
- Algorytm A: ~1000 operacji
- Algorytm B: ~1 000 000 operacji

**Różnica jest ogromna!**

### Cel analizy złożoności:
> **Przewidzieć zachowanie algorytmu na dużych danych i porównać różne algorytmy.**

---

## 2) Notacja asymptotyczna — Big O, Omega, Theta

### Notacja Big O (O) — górna granica

**Definicja:**
Algorytm ma złożoność **O(f(n))**, jeśli istnieją stałe c > 0 i n₀ takie, że dla wszystkich n ≥ n₀:
T(n) ≤ c * f(n)

**Intuicyjnie:** "Algorytm nie będzie gorszy niż f(n)"

**Przykład:**
Jeśli T(n) = 3n + 5, to T(n) = O(n), bo:
3n + 5 ≤ 4n dla n ≥ 5

### Notacja Big Omega (Ω) — dolna granica

**Definicja:**
Algorytm ma złożoność **Ω(f(n))**, jeśli istnieją stałe c > 0 i n₀ takie, że dla wszystkich n ≥ n₀:
T(n) ≥ c * f(n)

**Intuicyjnie:** "Algorytm nie będzie lepszy niż f(n)"

**Przykład:**
Jeśli T(n) = 3n + 5, to T(n) = Ω(n), bo:
3n + 5 ≥ n dla n ≥ 1

### Notacja Big Theta (Θ) — dokładna granica

**Definicja:**
Algorytm ma złożoność **Θ(f(n))**, jeśli:
- T(n) = O(f(n)) ORAZ
- T(n) = Ω(f(n))

**Intuicyjnie:** "Algorytm ma dokładnie złożoność f(n)"

**Przykład:**
Jeśli T(n) = 3n + 5, to T(n) = Θ(n)

### Różnice między notacjami:

| Notacja | Znaczenie | Użycie |
|---------|-----------|--------|
| **O(f(n))** | Najgorszy przypadek (górna granica) | "Algorytm nie gorszy niż..." |
| **Ω(f(n))** | Najlepszy przypadek (dolna granica) | "Algorytm nie lepszy niż..." |
| **Θ(f(n))** | Dokładna złożoność (gdy O = Ω) | "Algorytm ma dokładnie..." |

### W praktyce:
- **Big O** jest najczęściej używana (analiza najgorszego przypadku)
- **Theta** gdy chcemy powiedzieć o dokładnej złożoności
- **Omega** rzadziej używana (zwykle do pokazania, że nie da się lepiej)

---

## 3) Najczęstsze klasy złożoności

### Hierarchia złożoności (od najlepszej do najgorszej):

```
O(1) < O(log n) < O(√n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2^n) < O(n!)
```

### O(1) — złożoność stała

**Charakterystyka:**
- Czas wykonania **nie zależy** od rozmiaru danych
- Najlepsza możliwa złożoność
- Operacja zawsze trwa tyle samo

**Przykłady:**
- Dostęp do elementu tablicy przez indeks
- Dodanie elementu na koniec listy (jeśli mamy wskaźnik)
- Operacje na hash table (w idealnym przypadku)
- Operacje matematyczne (dodawanie, mnożenie)

**Kod:**
```java
int getElement(int[] array, int index) {
    return array[index];  // O(1) - stały czas niezależnie od rozmiaru
}
```

**Wykres:** Linia pozioma (nie rośnie)

### O(log n) — złożoność logarytmiczna

**Charakterystyka:**
- Czas rośnie **bardzo wolno**
- Podwajanie danych = +1 operacja
- Bardzo dobra złożoność

**Przykłady:**
- Wyszukiwanie binarne (binary search)
- Operacje na drzewie binarnym (balanced BST)
- Wyszukiwanie w posortowanej tablicy

**Kod (binary search):**
```java
int binarySearch(int[] array, int target) {
    int left = 0, right = array.length - 1;
    while (left <= right) {
        int mid = (left + right) / 2;  // O(log n) iteracji
        if (array[mid] == target) return mid;
        if (array[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

**Wykres:** Rośnie bardzo wolno, prawie poziomo

**Ciekawe:** log₂(1 000 000) ≈ 20, log₂(1 000 000 000) ≈ 30

### O(√n) — złożoność pierwiastkowa

**Charakterystyka:**
- Średnia złożoność
- Rzadziej spotykana

**Przykłady:**
- Sprawdzanie czy liczba jest pierwsza (naiwna metoda do √n)
- Niektóre algorytmy na grafach

**Kod:**
```java
boolean isPrime(int n) {
    for (int i = 2; i * i <= n; i++) {  // O(√n) iteracji
        if (n % i == 0) return false;
    }
    return true;
}
```

### O(n) — złożoność liniowa

**Charakterystyka:**
- Czas rośnie **proporcjonalnie** do rozmiaru danych
- Podwajanie danych = podwójny czas
- Dobra złożoność dla większości zastosowań

**Przykłady:**
- Przejście przez tablicę/listę (jedna pętla)
- Wyszukiwanie liniowe
- Znajdowanie maksimum/minimum
- Kopiowanie tablicy
- Wypisanie wszystkich elementów

**Kod:**
```java
int findMax(int[] array) {
    int max = array[0];
    for (int i = 1; i < array.length; i++) {  // O(n) iteracji
        if (array[i] > max) max = array[i];
    }
    return max;
}
```

**Wykres:** Linia prosta pod kątem 45°

### O(n log n) — złożoność liniowo-logarytmiczna

**Charakterystyka:**
- Czas rośnie **prawie liniowo**, ale wolniej niż kwadratowo
- Bardzo dobra złożoność dla sortowania
- Praktycznie najlepsza dla porównywania sortowań

**Przykłady:**
- Merge Sort
- Quick Sort (średni przypadek)
- Heap Sort
- Większość efektywnych algorytmów sortowania

**Kod (merge sort - koncepcja):**
```java
void mergeSort(int[] array, int left, int right) {
    if (left < right) {
        int mid = (left + right) / 2;
        mergeSort(array, left, mid);      // T(n/2)
        mergeSort(array, mid + 1, right); // T(n/2)
        merge(array, left, mid, right);   // O(n)
        // T(n) = 2T(n/2) + O(n) = O(n log n)
    }
}
```

**Wykres:** Rośnie szybciej niż liniowo, ale wolniej niż kwadratowo

### O(n²) — złożoność kwadratowa

**Charakterystyka:**
- Czas rośnie **kwadratowo** z rozmiarem
- Podwajanie danych = 4x czas
- Za wolna dla dużych danych

**Przykłady:**
- Bubble Sort
- Selection Sort
- Insertion Sort (najgorszy przypadek)
- Dwie zagnieżdżone pętle po n elementach
- Sprawdzanie wszystkich par elementów

**Kod:**
```java
void bubbleSort(int[] array) {
    for (int i = 0; i < array.length; i++) {           // n iteracji
        for (int j = 0; j < array.length - 1; j++) {   // n iteracji
            if (array[j] > array[j + 1]) {
                swap(array, j, j + 1);
            }
        }
    }
    // O(n * n) = O(n²)
}
```

**Wykres:** Parabola (rośnie szybko)

**Ostrzeżenie:** Dla n = 1000 → 1 000 000 operacji, dla n = 10000 → 100 000 000 operacji

### O(n³) — złożoność sześcienna

**Charakterystyka:**
- Czas rośnie **sześciennie**
- Podwajanie danych = 8x czas
- Bardzo wolna

**Przykłady:**
- Trzy zagnieżdżone pętle
- Mnożenie macierzy (naiwna metoda)
- Sprawdzanie wszystkich trójek elementów

**Kod:**
```java
void multiplyMatrices(int[][] A, int[][] B, int[][] C) {
    for (int i = 0; i < n; i++) {           // n
        for (int j = 0; j < n; j++) {       // n
            for (int k = 0; k < n; k++) {   // n
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
    // O(n³)
}
```

### O(2^n) — złożoność wykładnicza

**Charakterystyka:**
- Czas rośnie **wykładniczo**
- Każdy nowy element podwaja czas
- Praktycznie nieużywalna dla większych danych

**Przykłady:**
- Algorytmy brute-force
- Problem komiwojażera (TSP) - naiwna metoda
- Generowanie wszystkich podzbiorów
- Rekurencyjne obliczanie ciągu Fibonacciego (naiwne)

**Kod (Fibonacci - naiwny):**
```java
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
    // T(n) = T(n-1) + T(n-2) ≈ O(2^n)
}
```

**Wykres:** Rośnie ekstremalnie szybko

**Ostrzeżenie:** Dla n = 30 → ~1 miliard operacji, dla n = 40 → ~1 bilion operacji

### O(n!) — złożoność silniowa

**Charakterystyka:**
- Najgorsza możliwa złożoność
- Czas rośnie jak silnia
- Praktycznie nieużywalna nawet dla małych danych

**Przykłady:**
- Generowanie wszystkich permutacji
- Problem komiwojażera - sprawdzanie wszystkich ścieżek
- Niektóre problemy kombinatoryczne

**Kod:**
```java
void generatePermutations(int[] array, int start) {
    if (start == array.length) {
        // przetwórz permutację
        return;
    }
    for (int i = start; i < array.length; i++) {
        swap(array, start, i);
        generatePermutations(array, start + 1);  // O(n!) permutacji
        swap(array, start, i);
    }
}
```

**Wykres:** Rośnie ekstremalnie szybko, jeszcze gorzej niż wykładnicza

---

## 4) Analiza przypadków — najlepszy, przeciętny, najgorszy

### Trzy przypadki analizy:

#### 1. Najlepszy przypadek (Best Case) — Ω
- Minimalna liczba operacji
- Najlepsze możliwe dane wejściowe
- Notacja: Ω(f(n))

**Przykład - wyszukiwanie liniowe:**
- Najlepszy: Element na pierwszej pozycji → O(1)
- Przeciętny: Element w środku → O(n/2) = O(n)
- Najgorszy: Element na końcu lub brak → O(n)

#### 2. Przeciętny przypadek (Average Case) — Θ
- Oczekiwana liczba operacji
- Losowe dane wejściowe
- Często najważniejszy w praktyce

**Przykład - Quick Sort:**
- Najlepszy: O(n log n)
- Przeciętny: O(n log n)
- Najgorszy: O(n²) - gdy pivot jest zawsze skrajnym elementem

#### 3. Najgorszy przypadek (Worst Case) — O
- Maksymalna liczba operacji
- Najgorsze możliwe dane wejściowe
- Notacja: O(f(n))
- Najczęściej analizowana

**Przykład - wyszukiwanie binarne:**
- Wszystkie przypadki: O(log n) - zawsze taka sama złożoność

### Przykład porównawczy - Quick Sort vs Merge Sort:

| Algorytm | Najlepszy | Przeciętny | Najgorszy |
|----------|-----------|------------|-----------|
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) |

**Wniosek:** Merge Sort ma gwarancję, Quick Sort jest szybszy w praktyce (lepsze stałe).

---

## 5) Złożoność czasowa vs pamięciowa

### Złożoność czasowa (Time Complexity)

**Definiuje:** Jak szybko rośnie czas wykonania algorytmu

**Mierzymy w:**
- Liczba operacji podstawowych
- Liczba porównań
- Liczba iteracji pętli

**Przykład:**
```java
int sum = 0;
for (int i = 0; i < n; i++) {  // O(n) czasowa
    sum += array[i];
}
```

### Złożoność pamięciowa (Space Complexity)

**Definiuje:** Jak szybko rośnie zużycie pamięci algorytmu

**Uwzględniamy:**
- Pamięć na dane wejściowe (często pomijana)
- Pamięć pomocnicza (zmienne, stos rekurencyjny)
- Pamięć wyjściowa (jeśli tworzymy nową strukturę)

**Przykład:**
```java
int[] copyArray(int[] original) {
    int[] copy = new int[original.length];  // O(n) pamięciowa
    for (int i = 0; i < original.length; i++) {
        copy[i] = original[i];
    }
    return copy;
}
```

### Przykłady różnych kombinacji:

| Algorytm | Czasowa | Pamięciowa |
|----------|---------|------------|
| **Wyszukiwanie liniowe** | O(n) | O(1) - tylko zmienne |
| **Merge Sort** | O(n log n) | O(n) - tablica pomocnicza |
| **Quick Sort** | O(n log n) średnio | O(log n) - stos rekurencyjny |
| **Bubble Sort (in-place)** | O(n²) | O(1) - sortowanie w miejscu |
| **Counting Sort** | O(n + k) | O(k) - tablica liczników |

### Trade-off czas vs pamięć:

Często możemy:
- **Zwiększyć pamięć** → zmniejszyć czas (cache, precomputing)
- **Zwiększyć czas** → zmniejszyć pamięć (kompresja, obliczenia na żądanie)

---

## 6) Analiza złożoności — metody

### Metoda 1: Zliczanie operacji podstawowych

**Kroki:**
1. Zidentyfikuj operacje podstawowe
2. Zlicz ile razy się wykonują
3. Wyraź jako funkcję n (rozmiar danych)
4. Uprość używając notacji O

**Przykład:**
```java
int sum = 0;                    // 1 operacja
for (int i = 0; i < n; i++) {   // n iteracji
    sum += array[i];            // 2 operacje w każdej iteracji
}
return sum;                     // 1 operacja

// Razem: 1 + n * 2 + 1 = 2n + 2 = O(n)
```

### Metoda 2: Analiza pętli

**Zasady:**
- Pętla po n elementach: O(n)
- Zagnieżdżone pętle: O(n * m) = O(n²) jeśli n = m
- Pętla z podziałem: O(log n)

**Przykłady:**
```java
// Pojedyncza pętla: O(n)
for (int i = 0; i < n; i++) { }

// Zagnieżdżone pętle: O(n²)
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) { }
}

// Pętla z podziałem: O(log n)
for (int i = n; i > 0; i /= 2) { }
```

### Metoda 3: Analiza rekurencji

**Twierdzenie Master (Master Theorem):**

Jeśli T(n) = a * T(n/b) + f(n), gdzie a ≥ 1, b > 1, to:

1. Jeśli f(n) = O(n^(log_b(a) - ε)) dla ε > 0:
   T(n) = Θ(n^(log_b(a)))

2. Jeśli f(n) = Θ(n^(log_b(a))):
   T(n) = Θ(n^(log_b(a)) * log n)

3. Jeśli f(n) = Ω(n^(log_b(a) + ε)) dla ε > 0:
   T(n) = Θ(f(n))

**Przykład - Merge Sort:**
T(n) = 2T(n/2) + O(n)
- a = 2, b = 2, f(n) = O(n)
- log_b(a) = log₂(2) = 1
- f(n) = Θ(n^1) = Θ(n^(log_b(a))) → przypadek 2
- T(n) = Θ(n log n)

**Przykład - Binary Search:**
T(n) = T(n/2) + O(1)
- a = 1, b = 2, f(n) = O(1)
- log_b(a) = log₂(1) = 0
- f(n) = O(n^0) = O(1) → przypadek 1
- T(n) = Θ(log n)

### Metoda 4: Drzewo rekurencji

**Kroki:**
1. Narysuj drzewo wywołań rekurencyjnych
2. Policz całkowitą pracę na każdym poziomie
3. Zsumuj wszystkie poziomy

**Przykład - Merge Sort:**
```
Poziom 0:           O(n)          [1 poziom]
Poziom 1:        O(n/2) O(n/2)    [2 poziomy, razem O(n)]
Poziom 2:    O(n/4) ... O(n/4)    [4 poziomy, razem O(n)]
...
Poziom log n: O(1) ... O(1)       [n poziomy, razem O(n)]

Razem: (log n + 1) poziomów * O(n) = O(n log n)
```

---

## 7) Przykłady analizy konkretnych algorytmów

### Przykład 1: Wyszukiwanie liniowe

```java
int linearSearch(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {      // n iteracji
        if (array[i] == target) return i;         // O(1) operacja
    }
    return -1;
}
```

**Analiza:**
- **Najlepszy przypadek:** O(1) - element na pierwszej pozycji
- **Przeciętny przypadek:** O(n/2) = O(n) - element w środku
- **Najgorszy przypadek:** O(n) - element na końcu lub brak
- **Pamięciowa:** O(1) - tylko zmienne

**Złożoność:** O(n) czasowa, O(1) pamięciowa

### Przykład 2: Wyszukiwanie binarne

```java
int binarySearch(int[] array, int target) {
    int left = 0, right = array.length - 1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (array[mid] == target) return mid;
        if (array[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

**Analiza:**
- W każdym kroku obszar przeszukiwania zmniejsza się o połowę
- n → n/2 → n/4 → ... → 1
- Liczba kroków: log₂(n)
- **Wszystkie przypadki:** O(log n)

**Złożoność:** O(log n) czasowa, O(1) pamięciowa

### Przykład 3: Bubble Sort

```java
void bubbleSort(int[] array) {
    for (int i = 0; i < array.length; i++) {              // n iteracji
        for (int j = 0; j < array.length - 1 - i; j++) {  // n-i iteracji
            if (array[j] > array[j + 1]) {
                swap(array, j, j + 1);
            }
        }
    }
}
```

**Analiza:**
- Zewnętrzna pętla: n iteracji
- Wewnętrzna pętla: (n-1) + (n-2) + ... + 1 = n(n-1)/2
- **Złożoność:** O(n²)

**Złożoność:** O(n²) czasowa, O(1) pamięciowa (in-place)

### Przykład 4: Merge Sort

```java
void mergeSort(int[] array, int left, int right) {
    if (left < right) {
        int mid = (left + right) / 2;
        mergeSort(array, left, mid);           // T(n/2)
        mergeSort(array, mid + 1, right);      // T(n/2)
        merge(array, left, mid, right);        // O(n)
    }
}
```

**Analiza:**
- T(n) = 2T(n/2) + O(n)
- Z Master Theorem: T(n) = O(n log n)
- Pamięć: O(n) na tablicę pomocniczą + O(log n) na stos rekurencyjny = O(n)

**Złożoność:** O(n log n) czasowa, O(n) pamięciowa

### Przykład 5: Fibonacci (naiwny)

```java
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

**Analiza:**
- T(n) = T(n-1) + T(n-2) + O(1)
- Drzewo rekurencji ma wysokość n i wykładniczą liczbę węzłów
- **Złożoność:** O(2^n) - bardzo wolna!

**Złożoność:** O(2^n) czasowa, O(n) pamięciowa (głębokość rekurencji)

**Lepsze rozwiązanie (dynamiczne programowanie):**
```java
int fibonacciDP(int n) {
    int[] dp = new int[n + 1];
    dp[0] = 0; dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];  // O(n) iteracji
    }
    return dp[n];
}
```
**Złożoność:** O(n) czasowa, O(n) pamięciowa

---

## 8) Złożoność amortyzowana

### Definicja

**Złożoność amortyzowana** to średnia złożoność operacji w sekwencji operacji, nawet jeśli pojedyncze operacje mogą być kosztowne.

### Przykład: Dynamiczna tablica (ArrayList)

**Operacje:**
- **Dodanie elementu:** O(1) amortyzowana (choć czasami O(n) gdy trzeba powiększyć)
- **Pojedyncza operacja:** może być O(n) (przy powiększaniu)
- **Sekwencja n operacji:** O(n) całkowicie

**Analiza:**
- Załóżmy, że zaczynamy od tablicy rozmiaru 1
- Powiększamy gdy brakuje miejsca (podwajamy rozmiar)
- Koszt: 1 + 2 + 4 + 8 + ... + n/2 + n = O(n)
- n operacji daje O(n) całkowity koszt
- **Amortyzowana złożoność:** O(n) / n = O(1)

### Metoda kontowa (Accounting Method)

Przypisujemy "kredyt" tanim operacjom, który "płacimy" za drogie operacje.

### Metoda potencjału (Potential Method)

Definiujemy funkcję potencjału, która mierzy "stan" struktury danych.

---

## 9) Praktyczne wskazówki i triki

### Jak upraszczać wyrażenia:

1. **Ignoruj stałe:**
   - 3n + 5 = O(n)
   - 1000n = O(n)

2. **Ignoruj niższe potęgi:**
   - n² + n = O(n²)
   - n³ + n² + n = O(n³)

3. **Ignoruj logarytmiczne czynniki przy większych potęgach:**
   - n² + n log n = O(n²)
   - n³ + n² log n = O(n³)

4. **Wybierz najgorszą gałąź:**
   - if (condition) O(n) else O(log n) = O(n)

### Częste błędy:

1. **Mylenie O(n) z Θ(n):**
   - O(n) mówi "nie gorsze niż n"
   - Θ(n) mówi "dokładnie n"

2. **Ignorowanie najlepszego przypadku:**
   - Quick Sort ma O(n²) w najgorszym, ale O(n log n) średnio

3. **Nieuwzględnianie pamięci:**
   - Merge Sort: O(n log n) czas, ale O(n) pamięć
   - Quick Sort: O(n log n) czas, O(log n) pamięć

4. **Mylenie złożoności z czasem wykonania:**
   - Złożoność mówi o wzroście, nie o absolutnym czasie
   - O(n) algorytm może być wolniejszy od O(n²) dla małych n (stałe czynniki!)

### Kiedy złożoność ma znaczenie:

**Duże dane (n > 10 000):**
- Różnica między O(n) a O(n²) jest ogromna
- Ważna jest dobra złożoność

**Małe dane (n < 100):**
- Stałe czynniki mogą być ważniejsze
- Prosty algorytm O(n²) może być szybszy niż skomplikowany O(n log n)

**Przykład:**
- Bubble Sort (O(n²)) może być szybszy od Quick Sort (O(n log n)) dla n < 50
- Bo Bubble Sort ma małe stałe, a Quick Sort ma overhead

---

## 10) Porównanie algorytmów sortowania

| Algorytm | Najlepszy | Przeciętny | Najgorszy | Pamięciowa | Stabilny |
|----------|-----------|------------|-----------|------------|----------|
| **Bubble Sort** | O(n) | O(n²) | O(n²) | O(1) | Tak |
| **Selection Sort** | O(n²) | O(n²) | O(n²) | O(1) | Nie |
| **Insertion Sort** | O(n) | O(n²) | O(n²) | O(1) | Tak |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) | Tak |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) | Nie |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | O(1) | Nie |
| **Counting Sort** | O(n + k) | O(n + k) | O(n + k) | O(k) | Tak |

**Uwagi:**
- **k** = zakres wartości (dla liczb od 0 do k-1)
- **Stabilny** = zachowuje kolejność równych elementów
- **In-place** = O(1) pamięciowa (oprócz danych wejściowych)

---

## 11) Złożoność dla struktur danych

### Operacje podstawowe:

| Struktura | Wstawienie | Wyszukiwanie | Usunięcie | Min/Max |
|-----------|------------|--------------|-----------|---------|
| **Tabela (Array)** | O(1) | O(n) | O(n) | O(n) |
| **Lista (LinkedList)** | O(1) | O(n) | O(n) | O(n) |
| **Hash Table** | O(1)* | O(1)* | O(1)* | O(n) |
| **BST (niezbalansowany)** | O(n) | O(n) | O(n) | O(n) |
| **BST (zbalansowany)** | O(log n) | O(log n) | O(log n) | O(log n) |
| **Heap** | O(log n) | O(n) | O(log n) | O(1) |

*O(1) amortyzowana, O(n) w najgorszym przypadku (kolizje)

### Wybór struktury danych:

**Hash Table:** Gdy potrzebujesz szybkiego wyszukiwania/wstawiania
**BST:** Gdy potrzebujesz uporządkowanych danych i operacji zakresowych
**Heap:** Gdy potrzebujesz szybkiego dostępu do min/max
**Array:** Gdy masz indeksy i potrzebujesz szybkiego dostępu

---

## 12) Kluczowe zdania na obronę

1. **Big O (O)** — górna granica, najgorszy przypadek, "algorytm nie gorszy niż"

2. **Big Omega (Ω)** — dolna granica, najlepszy przypadek, "algorytm nie lepszy niż"

3. **Big Theta (Θ)** — dokładna złożoność, gdy O = Ω

4. **Hierarchia złożoności:** O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2^n) < O(n!)

5. **O(1)** — stała, niezależna od rozmiaru (dostęp do tablicy)

6. **O(log n)** — logarytmiczna, bardzo dobra (binary search, BST)

7. **O(n)** — liniowa, proporcjonalna (przejście przez tablicę)

8. **O(n log n)** — liniowo-logarytmiczna, dobra dla sortowania (Merge Sort, Quick Sort)

9. **O(n²)** — kwadratowa, wolna (dwie zagnieżdżone pętle, Bubble Sort)

10. **O(2^n)** — wykładnicza, praktycznie nieużywalna (naiwny Fibonacci, brute-force)

11. **Analiza przypadków:** najlepszy (Ω), przeciętny (Θ), najgorszy (O)

12. **Złożoność czasowa** vs **pamięciowa** — mogą się różnić

13. **Złożoność amortyzowana** — średnia złożoność operacji w sekwencji

14. **Master Theorem** — metoda analizy rekurencyjnych algorytmów dziel-i-zwyciężaj

15. **W praktyce:** dla małych danych stałe czynniki mogą być ważniejsze niż złożoność

16. **Trade-off:** czas vs pamięć — często możemy wymienić jedno na drugie

17. **Merge Sort** — O(n log n) zawsze, stabilny, ale O(n) pamięci

18. **Quick Sort** — O(n log n) średnio, O(n²) najgorszy, ale szybszy w praktyce

19. **Binary Search** — O(log n) — bardzo efektywne wyszukiwanie w posortowanej tablicy

20. **Hash Table** — O(1) amortyzowana dla wstawiania/wyszukiwania (O(n) w najgorszym przypadku)

