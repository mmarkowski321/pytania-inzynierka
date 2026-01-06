# Ocena Złożoności Algorytmów — Kompletna Notatka do Obrony

## 0) Podstawy — od zera (dla początkujących)

### Co to jest algorytm?

**Algorytm** = krok po kroku instrukcje, jak rozwiązać problem.

**Przykład z życia:**
- **Problem:** Jak ugotować jajko?
- **Algorytm:**
  1. Wlej wodę do garnka
  2. Zagotuj wodę
  3. Włóż jajko
  4. Gotuj 5 minut
  5. Wyjmij jajko

**W programowaniu:**
- **Problem:** Znajdź największą liczbę w tablicy
- **Algorytm:**
  1. Weź pierwszą liczbę jako maksimum
  2. Dla każdej kolejnej liczby:
     - Jeśli jest większa niż maksimum → ustaw jako nowe maksimum
  3. Zwróć maksimum

### Co to jest złożoność algorytmu?

**Złożoność** = jak szybko rośnie czas wykonania lub pamięć, gdy zwiększamy rozmiar danych.

**Przykład intuicyjny:**
- **Problem:** Znajdź numer telefonu w książce telefonicznej
- **Rozmiar danych:** n = liczba osób w książce
- **Pytanie:** Jak szybko rośnie czas wyszukiwania, gdy książka ma więcej osób?

**Dwa algorytmy:**

**Algorytm 1: Wyszukiwanie liniowe**
- Sprawdzaj od początku do końca, jedna po drugiej
- W najgorszym przypadku: sprawdzisz wszystkie n osób
- **Złożoność:** O(n) — czas rośnie liniowo

**Algorytm 2: Wyszukiwanie binarne**
- Otwórz książkę w połowie
- Jeśli szukane nazwisko jest wcześniej → szukaj w pierwszej połowie
- Jeśli później → szukaj w drugiej połowie
- Powtarzaj, aż znajdziesz
- **Złożoność:** O(log n) — czas rośnie logarytmicznie (dużo szybciej!)

**Porównanie dla n = 1 000 000:**
- Algorytm 1: ~1 000 000 sprawdzeń (w najgorszym przypadku)
- Algorytm 2: ~20 sprawdzeń (log₂(1 000 000) ≈ 20)

**Różnica jest ogromna!**

### Dlaczego złożoność jest ważna?

**Problem:** Komputer może wykonać ~1 miliard operacji na sekundę.

**Scenariusz:** Masz tablicę z 1 miliardem liczb i chcesz je posortować.

**Algorytm O(n²):**
- Operacje: n² = (10⁹)² = 10¹⁸ operacji
- Czas: 10¹⁸ / 10⁹ = 10⁹ sekund = **~31 lat!** 😱

**Algorytm O(n log n):**
- Operacje: n log n = 10⁹ × log(10⁹) ≈ 10⁹ × 30 = 3 × 10¹⁰ operacji
- Czas: 3 × 10¹⁰ / 10⁹ = 30 sekund ✓

**Wniosek:** Wybór algorytmu ma ogromne znaczenie!

### Co oznacza "n"?

**n** = rozmiar danych wejściowych (liczba elementów do przetworzenia).

**Przykłady:**
- Tablica z liczbami: n = długość tablicy
- Lista studentów: n = liczba studentów
- Graf: n = liczba węzłów
- Macierz: n = liczba wierszy (lub kolumn)

**Przykład:**
```java
int[] array = {1, 5, 3, 9, 2};  // n = 5
for (int i = 0; i < array.length; i++) {  // n iteracji
    System.out.println(array[i]);
}
```

### Co to jest operacja podstawowa?

**Operacja podstawowa** = pojedyncza, prosta operacja, która zajmuje stały czas.

**Przykłady operacji podstawowych:**
- Przypisanie: `x = 5`
- Porównanie: `if (a > b)`
- Arytmetyka: `a + b`, `a * b`
- Dostęp do tablicy: `array[i]`
- Wywołanie funkcji: `function()`

**Przykład zliczania operacji:**
```java
int sum = 0;                    // 1 operacja (przypisanie)
for (int i = 0; i < n; i++) {   // n iteracji
    sum += array[i];            // 2 operacje w każdej iteracji (dostęp + dodawanie)
}
return sum;                     // 1 operacja (return)

// Razem: 1 + n * 2 + 1 = 2n + 2 operacji
```

### Co to jest czas wykonania?

**Czas wykonania T(n)** = liczba operacji podstawowych dla danych rozmiaru n.

**Przykład:**
- Algorytm wykonuje 3n + 5 operacji dla danych rozmiaru n
- T(n) = 3n + 5
- Dla n = 100: T(100) = 3 × 100 + 5 = 305 operacji

**Uwaga:** Nie mierzymy czasu w sekundach, tylko w liczbie operacji!
- Różne komputery mają różną prędkość
- Liczba operacji jest uniwersalna

### Co to jest asymptotyczna analiza?

**Asymptotyczna analiza** = analiza zachowania algorytmu dla **bardzo dużych** n (n → ∞).

**Dlaczego?**
- Dla małych n różnice są nieistotne
- Dla dużych n różnice są ogromne
- Chcemy wiedzieć, jak algorytm zachowa się w praktyce

**Przykład:**
- T(n) = 3n + 5
- Dla małych n: stała 5 jest ważna
- Dla dużych n: 3n dominuje, stała 5 jest nieistotna
- **Asymptotycznie:** T(n) ≈ 3n = O(n)

---

## 1) Wprowadzenie — po co oceniać złożoność?

### Dlaczego złożoność algorytmów jest ważna?

**Złożoność algorytmu** mówi nam, jak szybko rośnie czas wykonania lub zużycie pamięci wraz ze wzrostem rozmiaru danych wejściowych.

### Podstawowe pytania:
- Czy algorytm będzie działał szybko na dużych danych?
- Ile pamięci będzie potrzebował?
- Czy istnieje lepszy algorytm?
- Czy rozwiązanie jest praktyczne dla danych problemów?

### Przykład problemu — szczegółowa analiza:

**Scenariusz:** Masz tablicę z 1000 liczb i chcesz znaleźć największą.

**Algorytm A: Wyszukiwanie liniowe — O(n)**
```java
int findMax(int[] array) {
    int max = array[0];                    // 1 operacja
    for (int i = 1; i < array.length; i++) {  // n-1 iteracji
        if (array[i] > max) {              // 1 porównanie
            max = array[i];                // 1 przypisanie (czasami)
        }
    }
    return max;                            // 1 operacja
}
```

**Analiza:**
- Operacje: 1 + (n-1) × 2 + 1 = 2n operacji
- Dla n = 1000: 2000 operacji
- **Złożoność:** O(n) — czas rośnie liniowo

**Algorytm B: Sortowanie + wybór — O(n²)**
```java
int findMaxBad(int[] array) {
    // Najpierw sortuj (bubble sort - O(n²))
    for (int i = 0; i < array.length; i++) {        // n iteracji
        for (int j = 0; j < array.length - 1; j++) { // n iteracji
            if (array[j] > array[j+1]) {
                swap(array, j, j+1);
            }
        }
    }
    return array[array.length - 1];  // Ostatni element
}
```

**Analiza:**
- Operacje: n × n = n² operacji
- Dla n = 1000: 1 000 000 operacji
- **Złożoność:** O(n²) — czas rośnie kwadratowo

**Porównanie:**
- n = 1000: Algorytm A = 2000 operacji, Algorytm B = 1 000 000 operacji
- **Różnica:** 500x wolniejszy!

**Dla n = 1 000 000:**
- Algorytm A: 2 000 000 operacji (~0.002 sekundy)
- Algorytm B: 1 000 000 000 000 operacji (~1000 sekund = ~16 minut!)

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

### O(1) — złożoność stała — szczegółowo

**Charakterystyka:**
- Czas wykonania **nie zależy** od rozmiaru danych
- Najlepsza możliwa złożoność
- Operacja zawsze trwa tyle samo, niezależnie od n

**Intuicja:** To jak otwarcie drzwi — zawsze zajmuje tyle samo czasu, niezależnie od tego, ile osób jest w pokoju.

**Przykłady szczegółowe:**

**Przykład 1: Dostęp do elementu tablicy**
```java
int getElement(int[] array, int index) {
    return array[index];  // 1 operacja - zawsze
}
```

**Analiza:**
- Operacje: 1 (dostęp do tablicy)
- Dla n = 10: 1 operacja
- Dla n = 1000: 1 operacja
- Dla n = 1 000 000: 1 operacja
- **Złożoność:** O(1) — stała

**Dlaczego O(1)?**
- Komputer wie dokładnie, gdzie w pamięci jest element (adres = początek + indeks × rozmiar)
- Nie musi przeszukiwać — idzie bezpośrednio

**Przykład 2: Dodanie na koniec ArrayList (amortyzowana)**
```java
ArrayList<Integer> list = new ArrayList<>();
list.add(5);  // O(1) amortyzowana
```

**Analiza:**
- W większości przypadków: O(1) — dodaje na koniec
- Czasami: O(n) — gdy trzeba powiększyć tablicę (ale rzadko)
- **Amortyzowana złożoność:** O(1)

**Przykład 3: Operacje na HashMap (idealny przypadek)**
```java
HashMap<String, Integer> map = new HashMap<>();
map.put("key", 5);     // O(1) średnio
map.get("key");        // O(1) średnio
```

**Analiza:**
- Hash table oblicza hash klucza → idzie bezpośrednio do odpowiedniego miejsca
- W idealnym przypadku: O(1)
- W najgorszym (wszystkie klucze mają ten sam hash): O(n)

**Przykład 4: Operacje matematyczne**
```java
int add(int a, int b) {
    return a + b;  // O(1) - zawsze 1 operacja
}

int multiply(int a, int b) {
    return a * b;  // O(1) - zawsze 1 operacja
}
```

**Wykres:** Linia pozioma (nie rośnie)
```
Czas
  |
  |──────────────────────────── (stały)
  |
  └─────────────────────────────→ n
```

**Tabela porównawcza:**
| n | Operacje O(1) |
|---|---------------|
| 10 | 1 |
| 100 | 1 |
| 1000 | 1 |
| 1 000 000 | 1 |

**Wniosek:** O(1) to najlepsza możliwa złożoność — zawsze szybka!

### O(log n) — złożoność logarytmiczna — szczegółowo

**Charakterystyka:**
- Czas rośnie **bardzo wolno**
- Podwajanie danych = +1 operacja
- Bardzo dobra złożoność

**Intuicja:** To jak szukanie słowa w słowniku — za każdym razem eliminujesz połowę możliwości.

**Przykład z życia:**
- **Problem:** Znajdź numer telefonu w książce telefonicznej (alfabetycznie)
- **Algorytm:** Otwórz w połowie → sprawdź → wybierz odpowiednią połowę → powtarzaj
- **Kroki:** log₂(n) — dla 1 000 000 osób potrzebujesz ~20 kroków!

**Przykład szczegółowy: Binary Search**

**Kod:**
```java
int binarySearch(int[] array, int target) {
    int left = 0;
    int right = array.length - 1;
    
    while (left <= right) {
        int mid = (left + right) / 2;  // Środek
        
        if (array[mid] == target) {
            return mid;  // Znaleziono!
        }
        
        if (array[mid] < target) {
            left = mid + 1;  // Szukaj w prawej połowie
        } else {
            right = mid - 1;  // Szukaj w lewej połowie
        }
    }
    
    return -1;  // Nie znaleziono
}
```

**Analiza krok po kroku:**

**Przykład:** Szukamy 7 w tablicy [1, 3, 5, 7, 9, 11, 13] (n = 7)

**Iteracja 1:**
- left = 0, right = 6
- mid = (0 + 6) / 2 = 3
- array[3] = 7 → znaleziono! ✓
- **Operacje:** 1 porównanie

**Gdyby szukali 9:**

**Iteracja 1:**
- left = 0, right = 6
- mid = 3, array[3] = 7
- 7 < 9 → left = 4 (szukaj w prawej połowie)
- **Pozostało:** 3 elementy (9, 11, 13)

**Iteracja 2:**
- left = 4, right = 6
- mid = 5, array[5] = 11
- 11 > 9 → right = 4 (szukaj w lewej połowie)
- **Pozostało:** 1 element (9)

**Iteracja 3:**
- left = 4, right = 4
- mid = 4, array[4] = 9 → znaleziono! ✓
- **Operacje:** 3 porównania

**Analiza matematyczna:**
- Po każdej iteracji obszar przeszukiwania zmniejsza się o połowę
- n → n/2 → n/4 → n/8 → ... → 1
- Liczba kroków: log₂(n)
- **Złożoność:** O(log n)

**Tabela porównawcza:**
| n | Operacje O(log n) | Porównanie z O(n) |
|---|-------------------|-------------------|
| 10 | ~3 | 10 |
| 100 | ~7 | 100 |
| 1000 | ~10 | 1000 |
| 1 000 000 | ~20 | 1 000 000 |

**Wykres:** Rośnie bardzo wolno, prawie poziomo
```
Czas
  |
  |     ╱
  |    ╱
  |   ╱
  |  ╱
  | ╱
  └─────────────────────────────→ n
```

**Ciekawe fakty:**
- log₂(1 000 000) ≈ 20
- log₂(1 000 000 000) ≈ 30
- **Podwajanie danych = +1 operacja!**

**Inne przykłady O(log n):**
- Operacje na drzewie binarnym (zbalansowanym)
- Wyszukiwanie w posortowanej tablicy
- Operacje na kopcu (heap)

**Wniosek:** O(log n) to bardzo dobra złożoność — praktycznie stała dla dużych n!

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

### O(n) — złożoność liniowa — szczegółowo

**Charakterystyka:**
- Czas rośnie **proporcjonalnie** do rozmiaru danych
- Podwajanie danych = podwójny czas
- Dobra złożoność dla większości zastosowań

**Intuicja:** To jak sprawdzanie listy osób — musisz sprawdzić każdą osobę, więc czas rośnie liniowo z liczbą osób.

**Przykłady szczegółowe:**

**Przykład 1: Znajdowanie maksimum**
```java
int findMax(int[] array) {
    int max = array[0];                    // 1 operacja
    for (int i = 1; i < array.length; i++) {  // n-1 iteracji
        if (array[i] > max) {              // 1 porównanie
            max = array[i];                // 1 przypisanie (czasami)
        }
    }
    return max;                            // 1 operacja
}
```

**Analiza:**
- Operacje: 1 + (n-1) × 2 + 1 = 2n operacji
- **Złożoność:** O(n)

**Przykład 2: Sumowanie elementów**
```java
int sum(int[] array) {
    int sum = 0;                           // 1 operacja
    for (int i = 0; i < array.length; i++) {  // n iteracji
        sum += array[i];                   // 2 operacje (dostęp + dodawanie)
    }
    return sum;                            // 1 operacja
}
```

**Analiza:**
- Operacje: 1 + n × 2 + 1 = 2n + 2
- Upraszczając: O(n)

**Przykład 3: Wyszukiwanie liniowe**
```java
int linearSearch(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {  // n iteracji
        if (array[i] == target) {          // 1 porównanie
            return i;                      // 1 return (czasami)
        }
    }
    return -1;                             // 1 operacja (w najgorszym przypadku)
}
```

**Analiza:**
- Najlepszy przypadek: O(1) — element na pierwszej pozycji
- Najgorszy przypadek: O(n) — element na końcu lub brak
- Przeciętny przypadek: O(n) — element w środku
- **W praktyce:** O(n) — analizujemy najgorszy przypadek

**Przykład 4: Kopiowanie tablicy**
```java
int[] copyArray(int[] original) {
    int[] copy = new int[original.length];  // 1 operacja (alokacja)
    for (int i = 0; i < original.length; i++) {  // n iteracji
        copy[i] = original[i];              // 2 operacje (dostęp + przypisanie)
    }
    return copy;                            // 1 operacja
}
```

**Analiza:**
- Operacje: 1 + n × 2 + 1 = 2n + 2
- **Złożoność czasowa:** O(n)
- **Złożoność pamięciowa:** O(n) — nowa tablica

**Tabela porównawcza:**
| n | Operacje O(n) | Czas (przy 1 mld op/s) |
|---|---------------|------------------------|
| 10 | 10 | 0.00000001 s |
| 100 | 100 | 0.0000001 s |
| 1000 | 1000 | 0.000001 s |
| 1 000 000 | 1 000 000 | 0.001 s |
| 1 000 000 000 | 1 000 000 000 | 1 s |

**Wykres:** Linia prosta pod kątem 45°
```
Czas
  |
  |    ╱
  |   ╱
  |  ╱
  | ╱
  └─────────────────────────────→ n
```

**Wniosek:** O(n) to dobra złożoność — akceptowalna dla większości zastosowań.

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

### O(n²) — złożoność kwadratowa — szczegółowo

**Charakterystyka:**
- Czas rośnie **kwadratowo** z rozmiarem
- Podwajanie danych = 4x czas
- Za wolna dla dużych danych

**Intuicja:** To jak sprawdzanie wszystkich par osób — dla n osób masz n × n = n² par do sprawdzenia.

**Przykład z życia:**
- **Problem:** Sprawdź, czy w grupie są dwie osoby z tym samym imieniem
- **Algorytm:** Porównaj każdą osobę z każdą
- **Operacje:** n × n = n² porównań

**Przykłady szczegółowe:**

**Przykład 1: Bubble Sort — szczegółowa analiza**
```java
void bubbleSort(int[] array) {
    for (int i = 0; i < array.length; i++) {           // Pętla zewnętrzna
        for (int j = 0; j < array.length - 1 - i; j++) {   // Pętla wewnętrzna
            if (array[j] > array[j + 1]) {
                swap(array, j, j + 1);                  // 3 operacje
            }
        }
    }
}
```

**Analiza krok po kroku:**

**Pętla zewnętrzna:**
- Liczba iteracji: n
- W każdej iteracji wykonuje się pętla wewnętrzna

**Pętla wewnętrzna:**
- Iteracja 1: n-1 porównań
- Iteracja 2: n-2 porównań
- Iteracja 3: n-3 porównań
- ...
- Iteracja n: 0 porównań

**Całkowita liczba porównań:**
- (n-1) + (n-2) + (n-3) + ... + 1 + 0
- = n(n-1)/2
- = (n² - n)/2
- Upraszczając: O(n²)

**Przykład 2: Sprawdzanie wszystkich par**
```java
void printAllPairs(int[] array) {
    for (int i = 0; i < array.length; i++) {        // n iteracji
        for (int j = 0; j < array.length; j++) {    // n iteracji
            System.out.println("(" + array[i] + ", " + array[j] + ")");
        }
    }
}
```

**Analiza:**
- Dla każdej wartości i (n możliwości)
- Dla każdej wartości j (n możliwości)
- Razem: n × n = n² par
- **Złożoność:** O(n²)

**Przykład 3: Mnożenie macierzy (naiwna metoda)**
```java
void multiplyMatrices(int[][] A, int[][] B, int[][] C) {
    for (int i = 0; i < n; i++) {           // n iteracji
        for (int j = 0; j < n; j++) {       // n iteracji
            C[i][j] = 0;
            for (int k = 0; k < n; k++) {   // n iteracji
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
}
```

**Analiza:**
- Trzy zagnieżdżone pętle po n elementach
- Operacje: n × n × n = n³
- **Złożoność:** O(n³) — jeszcze gorsza!

**Tabela porównawcza:**
| n | Operacje O(n²) | Czas (przy 1 mld op/s) | Porównanie z O(n) |
|---|----------------|------------------------|-------------------|
| 10 | 100 | 0.0000001 s | 10x więcej |
| 100 | 10 000 | 0.00001 s | 100x więcej |
| 1000 | 1 000 000 | 0.001 s | 1000x więcej |
| 10 000 | 100 000 000 | 0.1 s | 10 000x więcej |
| 100 000 | 10 000 000 000 | 10 s | 100 000x więcej |

**Wykres:** Parabola (rośnie szybko)
```
Czas
  |
  |        ╱
  |       ╱
  |      ╱
  |     ╱
  |    ╱
  |   ╱
  └─────────────────────────────→ n
```

**Ostrzeżenie:** 
- Dla n = 1000 → 1 000 000 operacji (~0.001 s) — OK
- Dla n = 10 000 → 100 000 000 operacji (~0.1 s) — wolne
- Dla n = 100 000 → 10 000 000 000 operacji (~10 s) — bardzo wolne!

**Kiedy O(n²) jest akceptowalne?**
- Małe dane (n < 1000)
- Proste algorytmy (łatwe do zrozumienia)
- Rzadko wykonywane operacje

**Kiedy unikać O(n²)?**
- Duże dane (n > 10 000)
- Często wykonywane operacje
- Aplikacje czasu rzeczywistego

**Wniosek:** O(n²) jest za wolna dla dużych danych — szukaj lepszych algorytmów!

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

**Przykład szczegółowy — krok po kroku:**
```java
int sum = 0;                    // Krok 1
for (int i = 0; i < n; i++) {   // Krok 2
    sum += array[i];            // Krok 3
}
return sum;                     // Krok 4
```

**Analiza krok po kroku:**

**Krok 1: Inicjalizacja**
- Operacje: 1 (przypisanie sum = 0)
- Złożoność: O(1)

**Krok 2: Pętla for**
- Liczba iteracji: n
- Operacje w każdej iteracji pętli:
  - Sprawdzenie warunku (i < n): 1
  - Inkrementacja i (i++): 1
- Razem na pętlę: n × 2 = 2n

**Krok 3: Treść pętli**
- Operacje w każdej iteracji:
  - Dostęp do tablicy (array[i]): 1
  - Dodawanie (sum + array[i]): 1
  - Przypisanie (sum = ...): 1
- Razem: 3 operacje na iterację
- Dla n iteracji: 3n

**Krok 4: Return**
- Operacje: 1

**Podsumowanie:**
- Razem: 1 + 2n + 3n + 1 = 5n + 2
- Upraszczając: O(n) (ignorujemy stałe i niższe potęgi)

**Weryfikacja dla różnych n:**
- n = 10: 5 × 10 + 2 = 52 operacje
- n = 100: 5 × 100 + 2 = 502 operacje
- n = 1000: 5 × 1000 + 2 = 5002 operacje
- **Wzrost liniowy** → O(n) ✓

### Metoda 2: Analiza pętli

**Zasady:**
- Pętla po n elementach: O(n)
- Zagnieżdżone pętle: O(n * m) = O(n²) jeśli n = m
- Pętla z podziałem: O(log n)

**Przykłady szczegółowe:**

**Przykład 1: Pojedyncza pętla — O(n)**
```java
for (int i = 0; i < n; i++) {
    System.out.println(i);  // 1 operacja w każdej iteracji
}
```
**Analiza:**
- Liczba iteracji: n
- Operacje na iterację: 1
- Razem: n × 1 = n
- **Złożoność:** O(n)

**Przykład 2: Zagnieżdżone pętle — O(n²)**
```java
for (int i = 0; i < n; i++) {        // n iteracji
    for (int j = 0; j < n; j++) {    // n iteracji
        System.out.println(i + ", " + j);  // 1 operacja
    }
}
```
**Analiza:**
- Pętla zewnętrzna: n iteracji
- Pętla wewnętrzna: n iteracji (dla każdej iteracji zewnętrznej)
- Operacje: n × n × 1 = n²
- **Złożoność:** O(n²)

**Przykład 3: Pętla z podziałem — O(log n)**
```java
for (int i = n; i > 0; i /= 2) {
    System.out.println(i);  // 1 operacja w każdej iteracji
}
```
**Analiza:**
- Iteracja 1: i = n
- Iteracja 2: i = n/2
- Iteracja 3: i = n/4
- Iteracja 4: i = n/8
- ...
- Iteracja k: i = n/(2^(k-1)) = 1
- n/(2^(k-1)) = 1 → n = 2^(k-1) → k-1 = log₂(n) → k = log₂(n) + 1
- **Złożoność:** O(log n)

**Przykład 4: Zagnieżdżone pętle z różnymi zakresami — O(n × m)**
```java
for (int i = 0; i < n; i++) {        // n iteracji
    for (int j = 0; j < m; j++) {    // m iteracji
        System.out.println(i + ", " + j);
    }
}
```
**Analiza:**
- Pętla zewnętrzna: n iteracji
- Pętla wewnętrzna: m iteracji
- Operacje: n × m
- **Złożoność:** O(n × m)
- Jeśli n = m: O(n²)

### Metoda 3: Analiza rekurencji

**Twierdzenie Master (Master Theorem):**

Jeśli T(n) = a * T(n/b) + f(n), gdzie a ≥ 1, b > 1, to:

1. Jeśli f(n) = O(n^(log_b(a) - ε)) dla ε > 0:
   T(n) = Θ(n^(log_b(a)))

2. Jeśli f(n) = Θ(n^(log_b(a))):
   T(n) = Θ(n^(log_b(a)) * log n)

3. Jeśli f(n) = Ω(n^(log_b(a) + ε)) dla ε > 0:
   T(n) = Θ(f(n))

**Przykład 1: Merge Sort — szczegółowa analiza**

**Równanie rekurencyjne:**
T(n) = 2T(n/2) + O(n)

**Krok 1: Identyfikacja parametrów**
- a = 2 (liczba podproblemów)
- b = 2 (rozmiar każdego podproblemu = n/b)
- f(n) = O(n) (praca na każdym poziomie)

**Krok 2: Obliczenie log_b(a)**
- log_b(a) = log₂(2) = 1

**Krok 3: Porównanie f(n) z n^(log_b(a))**
- n^(log_b(a)) = n^1 = n
- f(n) = O(n) = Θ(n)
- **Wniosek:** f(n) = Θ(n^(log_b(a))) → **przypadek 2**

**Krok 4: Zastosowanie wzoru**
- Przypadek 2: T(n) = Θ(n^(log_b(a)) * log n)
- T(n) = Θ(n^1 * log n) = Θ(n log n)

**Weryfikacja:**
- Merge Sort rzeczywiście ma złożoność O(n log n) ✓

**Przykład 2: Binary Search — szczegółowa analiza**

**Równanie rekurencyjne:**
T(n) = T(n/2) + O(1)

**Krok 1: Identyfikacja parametrów**
- a = 1 (jeden podproblem)
- b = 2 (rozmiar podproblemu = n/2)
- f(n) = O(1) (stała praca)

**Krok 2: Obliczenie log_b(a)**
- log_b(a) = log₂(1) = 0

**Krok 3: Porównanie f(n) z n^(log_b(a))**
- n^(log_b(a)) = n^0 = 1
- f(n) = O(1) = O(n^0)
- **Wniosek:** f(n) = O(n^(log_b(a) - ε)) dla ε > 0 → **przypadek 1**

**Krok 4: Zastosowanie wzoru**
- Przypadek 1: T(n) = Θ(n^(log_b(a)))
- T(n) = Θ(n^0) = Θ(1)
- **ALE:** To nie jest poprawne! Musimy przeanalizować dokładniej.

**Poprawna analiza:**
- Binary Search dzieli problem na połowę: T(n) = T(n/2) + 1
- T(n) = T(n/2) + 1
- T(n/2) = T(n/4) + 1
- ...
- T(1) = 1
- T(n) = log₂(n) + 1 = O(log n)

**Przykład 3: Quick Sort (średni przypadek)**

**Równanie rekurencyjne:**
T(n) = 2T(n/2) + O(n)

**Analiza:**
- a = 2, b = 2, f(n) = O(n)
- log_b(a) = 1
- f(n) = Θ(n) = Θ(n^1) → przypadek 2
- T(n) = Θ(n log n)

**Przykład 4: Algorytm dziel-i-zwyciężaj z 3 podproblemami**

**Równanie:**
T(n) = 3T(n/2) + O(n)

**Analiza:**
- a = 3, b = 2, f(n) = O(n)
- log_b(a) = log₂(3) ≈ 1.585
- n^(log_b(a)) = n^1.585
- f(n) = O(n) = O(n^1) < O(n^1.585) → przypadek 1
- T(n) = Θ(n^(log₂(3))) ≈ Θ(n^1.585)

### Metoda 4: Drzewo rekurencji

**Kroki:**
1. Narysuj drzewo wywołań rekurencyjnych
2. Policz całkowitą pracę na każdym poziomie
3. Zsumuj wszystkie poziomy

**Przykład - Merge Sort — szczegółowa analiza drzewa:**

**Równanie:** T(n) = 2T(n/2) + O(n)

**Drzewo rekurencji dla n = 8:**

```
                    Poziom 0: O(8) = O(n)
                         |
        ┌─────────────────┴─────────────────┐
        |                                   |
    Poziom 1: O(4)                    Poziom 1: O(4)
    = O(n/2)                          = O(n/2)
        |                                   |
    ┌───┴───┐                         ┌───┴───┐
    |       |                         |       |
Poziom 2: Poziom 2:              Poziom 2: Poziom 2:
O(2)     O(2)                   O(2)     O(2)
= O(n/4) = O(n/4)                = O(n/4) = O(n/4)
    |       |                         |       |
  ...     ...                       ...     ...
```

**Analiza poziomów:**

**Poziom 0:**
- Liczba węzłów: 1
- Praca na węzeł: O(n)
- Praca całkowita: O(n)

**Poziom 1:**
- Liczba węzłów: 2
- Praca na węzeł: O(n/2)
- Praca całkowita: 2 × O(n/2) = O(n)

**Poziom 2:**
- Liczba węzłów: 4
- Praca na węzeł: O(n/4)
- Praca całkowita: 4 × O(n/4) = O(n)

**Poziom k:**
- Liczba węzłów: 2^k
- Praca na węzeł: O(n/(2^k))
- Praca całkowita: 2^k × O(n/(2^k)) = O(n)

**Liczba poziomów:**
- Poziom 0: n elementów
- Poziom 1: n/2 elementów
- Poziom 2: n/4 elementów
- ...
- Poziom log₂(n): n/(2^(log₂(n))) = n/n = 1 element
- **Liczba poziomów:** log₂(n) + 1

**Całkowita praca:**
- (log₂(n) + 1) poziomów × O(n) na poziom
- = O(n log n)

**Weryfikacja dla n = 8:**
- Poziomów: log₂(8) + 1 = 3 + 1 = 4
- Praca: 4 × O(8) = O(32) = O(n log n) ✓

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

---

## 13) Materiały do nauki — linki i zasoby

### Strony internetowe z interaktywnymi wizualizacjami:

**1. VisuAlgo — Algorytmy wizualizowane**
- **Link:** https://visualgo.net/
- **Opis:** Interaktywne wizualizacje algorytmów sortowania, wyszukiwania, grafów
- **Zalety:** Możesz zobaczyć, jak algorytmy działają krok po kroku
- **Przydatne dla:** Zrozumienia, jak działają algorytmy

**2. Big-O Cheat Sheet**
- **Link:** https://www.bigocheatsheet.com/
- **Opis:** Kompletna ściąga złożoności algorytmów i struktur danych
- **Zalety:** Szybkie odniesienie do złożoności
- **Przydatne dla:** Szybkiego sprawdzenia złożoności

**3. Algorithm Visualizer**
- **Link:** https://algorithm-visualizer.org/
- **Opis:** Wizualizacje różnych algorytmów z kodem
- **Zalety:** Możesz zobaczyć kod i wizualizację jednocześnie

### Kursy online:

**4. Khan Academy — Algorithms**
- **Link:** https://www.khanacademy.org/computing/computer-science/algorithms
- **Opis:** Darmowy kurs podstaw algorytmów
- **Zalety:** Od podstaw, z przykładami i ćwiczeniami

**5. Coursera — Algorithms Specialization (Stanford)**
- **Link:** https://www.coursera.org/specializations/algorithms
- **Opis:** Kompleksowy kurs algorytmów
- **Zalety:** Profesjonalny, z certyfikatami
- **Uwaga:** Płatny, ale często są darmowe audyty

**6. MIT OpenCourseWare — Introduction to Algorithms**
- **Link:** https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/
- **Opis:** Materiały z MIT
- **Zalety:** Darmowe, wysokiej jakości

### Książki (PDF dostępne online):

**7. "Introduction to Algorithms" (CLRS)**
- **Autorzy:** Cormen, Leiserson, Rivest, Stein
- **Opis:** Klasyczna książka o algorytmach
- **Zalety:** Kompleksowa, szczegółowa
- **Link do PDF:** Szukaj "CLRS algorithms pdf" w Google

**8. "Algorithms" (Sedgewick)**
- **Autor:** Robert Sedgewick
- **Opis:** Praktyczne podejście do algorytmów
- **Zalety:** Łatwiejsza niż CLRS, z przykładami w Javie

### Platformy do ćwiczeń:

**9. LeetCode**
- **Link:** https://leetcode.com/
- **Opis:** Zadania programistyczne z analizą złożoności
- **Zalety:** Praktyka, rankingi, wyjaśnienia rozwiązań
- **Przydatne dla:** Ćwiczenia analizy złożoności

**10. HackerRank**
- **Link:** https://www.hackerrank.com/
- **Opis:** Zadania algorytmiczne z testami
- **Zalety:** Różne poziomy trudności

**11. Codeforces**
- **Link:** https://codeforces.com/
- **Opis:** Zawody programistyczne
- **Zalety:** Zaawansowane zadania, analiza złożoności jest kluczowa

### Filmy na YouTube:

**12. Abdul Bari — Algorithms**
- **Link:** https://www.youtube.com/c/AbdulBari
- **Opis:** Szczegółowe wyjaśnienia algorytmów
- **Zalety:** Bardzo szczegółowe, krok po kroku

**13. Back To Back SWE**
- **Link:** https://www.youtube.com/c/BackToBackSWE
- **Opis:** Wyjaśnienia algorytmów z przykładami
- **Zalety:** Praktyczne podejście

### Narzędzia do wizualizacji:

**14. Python Tutor**
- **Link:** https://pythontutor.com/
- **Opis:** Wizualizacja wykonania kodu krok po kroku
- **Zalety:** Możesz zobaczyć, jak kod działa linia po linii

**15. Big-O Calculator**
- **Link:** Szukaj "big o calculator online"
- **Opis:** Kalkulatory do analizy złożoności
- **Zalety:** Szybkie sprawdzenie złożoności

### Praktyczne wskazówki do nauki:

**1. Zacznij od podstaw:**
- Zrozum, co to jest złożoność
- Naucz się notacji Big O
- Przeanalizuj proste algorytmy

**2. Ćwicz analizę:**
- Weź prosty algorytm
- Zlicz operacje krok po kroku
- Wyraź jako funkcję n
- Uprość do notacji O

**3. Porównuj algorytmy:**
- Weź ten sam problem
- Porównaj różne algorytmy
- Zobacz różnicę w złożoności

**4. Wizualizuj:**
- Użyj VisuAlgo lub podobnych narzędzi
- Zobacz, jak algorytmy działają
- Zrozum, dlaczego mają taką złożoność

**5. Ćwicz na zadaniach:**
- Rozwiązuj zadania na LeetCode
- Analizuj złożoność swoich rozwiązań
- Porównuj z optymalnymi rozwiązaniami

### Przykładowy plan nauki:

**Tydzień 1: Podstawy**
- Notacja Big O, Omega, Theta
- O(1), O(log n), O(n)
- Proste przykłady analizy

**Tydzień 2: Klasy złożoności**
- O(n log n), O(n²), O(n³)
- O(2^n), O(n!)
- Porównania i wykresy

**Tydzień 3: Analiza algorytmów**
- Metody analizy (zliczanie, pętle, rekurencja)
- Master Theorem
- Przykłady konkretnych algorytmów

**Tydzień 4: Praktyka**
- Rozwiązywanie zadań
- Analiza złożoności struktur danych
- Porównywanie algorytmów

---

## 14) Dodatkowe przykłady analizy — krok po kroku

### Przykład 1: Analiza prostego algorytmu sumowania

**Kod:**
```java
int sum(int[] array) {
    int sum = 0;                    // Krok 1
    for (int i = 0; i < array.length; i++) {  // Krok 2
        sum += array[i];            // Krok 3
    }
    return sum;                     // Krok 4
}
```

**Analiza krok po kroku:**

**Krok 1: Inicjalizacja**
- Operacje: 1 (przypisanie)
- Złożoność: O(1)

**Krok 2: Pętla for**
- Liczba iteracji: n (długość tablicy)
- Operacje w każdej iteracji:
  - Sprawdzenie warunku: 1
  - Inkrementacja i: 1
- Razem na pętlę: n × 2 = 2n

**Krok 3: Treść pętli**
- Operacje w każdej iteracji:
  - Dostęp do tablicy: array[i] = 1
  - Dodawanie: sum + array[i] = 1
  - Przypisanie: sum = ... = 1
- Razem: 3 operacje na iterację
- Dla n iteracji: 3n

**Krok 4: Return**
- Operacje: 1

**Podsumowanie:**
- Razem: 1 + 2n + 3n + 1 = 5n + 2
- Upraszczając: O(n)

**Weryfikacja:**
- Dla n = 10: 5 × 10 + 2 = 52 operacje
- Dla n = 100: 5 × 100 + 2 = 502 operacje
- Dla n = 1000: 5 × 1000 + 2 = 5002 operacje
- **Wzrost liniowy** → O(n) ✓

### Przykład 2: Analiza zagnieżdżonych pętli

**Kod:**
```java
void printPairs(int[] array) {
    for (int i = 0; i < array.length; i++) {        // Pętla zewnętrzna
        for (int j = 0; j < array.length; j++) {    // Pętla wewnętrzna
            System.out.println(array[i] + ", " + array[j]);
        }
    }
}
```

**Analiza krok po kroku:**

**Pętla zewnętrzna:**
- Liczba iteracji: n
- W każdej iteracji wykonuje się pętla wewnętrzna

**Pętla wewnętrzna:**
- Liczba iteracji: n
- Operacje w każdej iteracji:
  - Dostęp: array[i] = 1
  - Dostęp: array[j] = 1
  - Konkatenacja: 2
  - Print: 1
  - Razem: ~5 operacji

**Obliczenia:**
- Dla każdej iteracji zewnętrznej: n iteracji wewnętrznych
- Operacje na parę pętli: n × n × 5 = 5n²
- **Złożoność:** O(n²)

**Weryfikacja:**
- Dla n = 10: 5 × 100 = 500 operacji
- Dla n = 100: 5 × 10 000 = 50 000 operacji
- Dla n = 1000: 5 × 1 000 000 = 5 000 000 operacji
- **Wzrost kwadratowy** → O(n²) ✓

### Przykład 3: Analiza rekurencyjnego algorytmu

**Kod (obliczanie silni):**
```java
int factorial(int n) {
    if (n <= 1) {           // Warunek bazowy
        return 1;
    }
    return n * factorial(n - 1);  // Rekurencja
}
```

**Analiza krok po kroku:**

**Równanie rekurencyjne:**
- T(n) = T(n-1) + O(1)
- T(1) = O(1) (warunek bazowy)

**Rozwiązanie:**
- T(n) = T(n-1) + 1
- T(n-1) = T(n-2) + 1
- T(n-2) = T(n-3) + 1
- ...
- T(2) = T(1) + 1
- T(1) = 1

**Podstawiając wstecz:**
- T(n) = T(n-1) + 1
- = [T(n-2) + 1] + 1 = T(n-2) + 2
- = [T(n-3) + 1] + 2 = T(n-3) + 3
- = ...
- = T(1) + (n-1) = 1 + (n-1) = n

**Złożoność:** O(n)

**Weryfikacja:**
- factorial(5) wywołuje: factorial(5) → factorial(4) → factorial(3) → factorial(2) → factorial(1)
- Liczba wywołań: 5 = n ✓

### Przykład 4: Analiza algorytmu z warunkami

**Kod:**
```java
int find(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == target) {
            return i;  // Znaleziono - kończymy
        }
    }
    return -1;  // Nie znaleziono
}
```

**Analiza przypadków:**

**Najlepszy przypadek:**
- Element na pierwszej pozycji
- Operacje: 1 porównanie + 1 return = 2
- **Złożoność:** O(1)

**Najgorszy przypadek:**
- Element na ostatniej pozycji lub brak elementu
- Operacje: n porównań + 1 return = n + 1
- **Złożoność:** O(n)

**Przeciętny przypadek:**
- Element w środku
- Operacje: n/2 porównań + 1 return ≈ n/2
- **Złożoność:** O(n)

**W praktyce:** Używamy O(n) — najgorszy przypadek

---

## 15) Ćwiczenia do samodzielnego rozwiązania

### Ćwiczenie 1: Analiza prostej pętli
```java
void printArray(int[] array) {
    for (int i = 0; i < array.length; i++) {
        System.out.println(array[i]);
    }
}
```
**Pytanie:** Jaka jest złożoność czasowa? Odpowiedź: O(n)

### Ćwiczenie 2: Analiza zagnieżdżonych pętli
```java
void printMatrix(int[][] matrix) {
    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix[i].length; j++) {
            System.out.print(matrix[i][j] + " ");
        }
        System.out.println();
    }
}
```
**Pytanie:** Jaka jest złożoność czasowa? 
**Odpowiedź:** O(n × m), gdzie n = liczba wierszy, m = liczba kolumn. Jeśli n = m, to O(n²)

### Ćwiczenie 3: Analiza z warunkiem
```java
boolean contains(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == target) {
            return true;
        }
    }
    return false;
}
```
**Pytanie:** Jaka jest złożoność w najlepszym, przeciętnym i najgorszym przypadku?
**Odpowiedź:** 
- Najlepszy: O(1) — element na pierwszej pozycji
- Przeciętny: O(n) — element w środku
- Najgorszy: O(n) — element na końcu lub brak

### Ćwiczenie 4: Analiza rekurencji
```java
int power(int base, int exp) {
    if (exp == 0) return 1;
    return base * power(base, exp - 1);
}
```
**Pytanie:** Jaka jest złożoność czasowa?
**Odpowiedź:** O(exp) — liczba wywołań rekurencyjnych = exp

### Ćwiczenie 5: Analiza złożoności pamięciowej
```java
int[] copyArray(int[] original) {
    int[] copy = new int[original.length];
    for (int i = 0; i < original.length; i++) {
        copy[i] = original[i];
    }
    return copy;
}
```
**Pytanie:** Jaka jest złożoność czasowa i pamięciowa?
**Odpowiedź:** 
- Czasowa: O(n) — n iteracji
- Pamięciowa: O(n) — nowa tablica rozmiaru n

