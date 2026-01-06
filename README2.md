# Arytmetyka Stało- i Zmiennoprzecinkowa — Kompletna Notatka do Obrony

## 0) Podstawy — od zera (dla początkujących)

### Co to jest bit i bajt?

**Bit** — najmniejsza jednostka informacji w komputerze:
- Może mieć wartość **0** lub **1** (tylko dwie możliwości!)
- To jak włącznik światła: włączony (1) lub wyłączony (0)

**Bajt** — 8 bitów razem:
- 1 bajt = 8 bitów
- Może reprezentować wartości od 0 do 255 (2^8 = 256 możliwości)

**Przykład:**
- 1 bit: 0 lub 1 (2 możliwości)
- 2 bity: 00, 01, 10, 11 (4 możliwości: 0, 1, 2, 3)
- 3 bity: 000, 001, 010, 011, 100, 101, 110, 111 (8 możliwości: 0-7)
- 8 bitów (1 bajt): 256 możliwości (0-255)

### Dlaczego komputer używa systemu binarnego?

**System dziesiętny** (używamy na co dzień):
- 10 cyfr: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
- Podstawa: 10
- Przykład: 123 = 1×10² + 2×10¹ + 3×10⁰

**System binarny** (komputer):
- 2 cyfry: 0, 1
- Podstawa: 2
- Przykład: 1011 = 1×2³ + 0×2² + 1×2¹ + 1×2⁰ = 8 + 0 + 2 + 1 = 11

**Dlaczego binarny?**
- Łatwo zaimplementować w elektronice (prąd płynie = 1, nie płynie = 0)
- Proste bramki logiczne (AND, OR, NOT)
- Odporne na błędy (łatwo odróżnić 0 od 1)

### Jak działa system binarny — krok po kroku

**Zasada:** Każda pozycja ma wagę będącą potęgą 2

**Tabela wag (od prawej do lewej):**
| Pozycja | Waga | Wartość |
|---------|------|---------|
| 0 (najbardziej na prawo) | 2^0 = 1 | 1 |
| 1 | 2^1 = 2 | 2 |
| 2 | 2^2 = 4 | 4 |
| 3 | 2^3 = 8 | 8 |
| 4 | 2^4 = 16 | 16 |
| 5 | 2^5 = 32 | 32 |
| 6 | 2^6 = 64 | 64 |
| 7 | 2^7 = 128 | 128 |

**Przykład 1: Konwersja binarnego na dziesiętny**

Liczba binarna: **1011**

Krok po kroku:
1. Zapisujemy pozycje (od prawej, zaczynając od 0):
   ```
   1  0  1  1
   3  2  1  0  (pozycje)
   ```

2. Mnożymy każdą cyfrę przez wagę jej pozycji:
   - Pozycja 0 (cyfra 1): 1 × 2^0 = 1 × 1 = **1**
   - Pozycja 1 (cyfra 1): 1 × 2^1 = 1 × 2 = **2**
   - Pozycja 2 (cyfra 0): 0 × 2^2 = 0 × 4 = **0**
   - Pozycja 3 (cyfra 1): 1 × 2^3 = 1 × 8 = **8**

3. Sumujemy: 8 + 0 + 2 + 1 = **11**

Wynik: 1011 (binarnie) = 11 (dziesiętnie) ✓

**Przykład 2: Konwersja dziesiętnego na binarny**

Liczba dziesiętna: **13**

Metoda: Dzielimy przez 2 i zapisujemy reszty

Krok po kroku:
1. 13 ÷ 2 = 6 reszta **1** ← zapisujemy
2. 6 ÷ 2 = 3 reszta **0** ← zapisujemy
3. 3 ÷ 2 = 1 reszta **1** ← zapisujemy
4. 1 ÷ 2 = 0 reszta **1** ← zapisujemy

5. Czytamy reszty **od dołu do góry**: 1101

Wynik: 13 (dziesiętnie) = 1101 (binarnie) ✓

**Sprawdzenie:** 1×8 + 1×4 + 0×2 + 1×1 = 8 + 4 + 0 + 1 = 13 ✓

### Liczby całkowite vs rzeczywiste

**Liczby całkowite** (integers):
- Przykłady: -5, 0, 1, 42, 1000
- **Brak części ułamkowej**
- Łatwe do reprezentacji w binarnym

**Liczby rzeczywiste** (floating-point):
- Przykłady: 3.14, -0.5, 123.456, 0.001
- **Mają część ułamkową** (po przecinku)
- Trudniejsze do reprezentacji w binarnym!

**Problem:** Jak zapisać 3.14 w binarnym?
- Część całkowita: 3 = 11 (binarnie) ✓
- Część ułamkowa: 0.14 = ??? (to jest problem!)

### Część ułamkowa w systemie binarnym

**Zasada:** Po przecinku pozycje mają wagi będące ujemnymi potęgami 2

**Tabela wag części ułamkowej:**
| Pozycja | Waga | Wartość |
|---------|------|---------|
| -1 (pierwsza po przecinku) | 2^(-1) = 1/2 | 0.5 |
| -2 | 2^(-2) = 1/4 | 0.25 |
| -3 | 2^(-3) = 1/8 | 0.125 |
| -4 | 2^(-4) = 1/16 | 0.0625 |
| -5 | 2^(-5) = 1/32 | 0.03125 |

**Przykład: Konwersja ułamka dziesiętnego na binarny**

Liczba: **0.625** (dziesiętnie)

Metoda: Mnożymy przez 2 i zapisujemy część całkowitą

Krok po kroku:
1. 0.625 × 2 = **1**.25 → zapisujemy **1**, zostaje 0.25
2. 0.25 × 2 = **0**.5 → zapisujemy **0**, zostaje 0.5
3. 0.5 × 2 = **1**.0 → zapisujemy **1**, zostaje 0.0 (koniec!)

4. Czytamy **od góry do dołu**: 0.101

Wynik: 0.625 (dziesiętnie) = 0.101 (binarnie) ✓

**Sprawdzenie:** 1×0.5 + 0×0.25 + 1×0.125 = 0.5 + 0 + 0.125 = 0.625 ✓

**Problem z niektórymi liczbami:**
- 0.1 (dziesiętnie) = 0.00011001100110011... (binarnie, **nieskończone!**)
- Komputer musi zaokrąglić → błędy zaokrąglenia!

---

## 1) Wprowadzenie — reprezentacja liczb w komputerze

### Podstawowy problem
Komputer przechowuje liczby w formie **binarnej** (0 i 1). Dla liczb całkowitych to proste, ale dla liczb rzeczywistych (z częścią ułamkową) potrzebujemy specjalnych reprezentacji.

**Dlaczego to problem?**
- Liczby całkowite: łatwe (np. 5 = 101 binarnie)
- Liczby rzeczywiste: trudne (np. 3.14 — jak zapisać część ułamkową?)
- Niektóre liczby dziesiętne nie mają dokładnej reprezentacji binarnej (np. 0.1)

### Dwie główne metody:
1. **Stałoprzecinkowa** (Fixed-Point) — pozycja przecinka jest **ustalona** (zawsze w tym samym miejscu)
2. **Zmiennoprzecinkowa** (Floating-Point) — pozycja przecinka jest **zmienna** (może się przesuwać)

---

## 2) Reprezentacja liczb całkowitych (podstawa)

### System binarny — wzór matematyczny

Liczba w systemie binarnym:
N = suma od i=0 do n-1 z (b_i * 2^i)

Gdzie:
- b_i — cyfra binarna (0 lub 1) na pozycji i
- n — liczba bitów
- i — pozycja bitu (0 = najbardziej na prawo)

**Intuicja:** Każda pozycja ma wagę będącą potęgą 2, mnożymy cyfrę przez wagę i sumujemy.

### Przykłady szczegółowe:

**Przykład 1: 1011 (binarnie)**

Rozpisujemy:
```
Pozycja:  3   2   1   0
Cyfra:    1   0   1   1
Waga:     8   4   2   1
Wartość:  8   0   2   1
```

Obliczamy:
- Pozycja 0: 1 × 2^0 = 1 × 1 = 1
- Pozycja 1: 1 × 2^1 = 1 × 2 = 2
- Pozycja 2: 0 × 2^2 = 0 × 4 = 0
- Pozycja 3: 1 × 2^3 = 1 × 8 = 8

Suma: 8 + 0 + 2 + 1 = **11**

Wynik: 1011 (binarnie) = 11 (dziesiętnie) ✓

**Przykład 2: 1100 (binarnie)**

Rozpisujemy:
```
Pozycja:  3   2   1   0
Cyfra:    1   1   0   0
Waga:     8   4   2   1
Wartość:  8   4   0   0
```

Obliczamy:
- Pozycja 0: 0 × 1 = 0
- Pozycja 1: 0 × 2 = 0
- Pozycja 2: 1 × 4 = 4
- Pozycja 3: 1 × 8 = 8

Suma: 8 + 4 + 0 + 0 = **12**

Wynik: 1100 (binarnie) = 12 (dziesiętnie) ✓

**Przykład 3: 1111 (binarnie) — maksymalna wartość dla 4 bitów**

Rozpisujemy:
```
Pozycja:  3   2   1   0
Cyfra:    1   1   1   1
Waga:     8   4   2   1
Wartość:  8   4   2   1
```

Suma: 8 + 4 + 2 + 1 = **15**

Wynik: 1111 (binarnie) = 15 (dziesiętnie) ✓

**Wniosek:** 4 bity mogą reprezentować liczby od 0 (0000) do 15 (1111) = 16 możliwości (2^4)

### Zakresy (n-bitowe liczby całkowite):
- **Bez znaku** (unsigned): 0 do (2^n - 1)
- **Ze znakiem** (signed, two's complement): -(2^(n-1)) do (2^(n-1) - 1)

### Przykłady zakresów:
- 8 bitów: -128 do 127 (ze znakiem) lub 0 do 255 (bez znaku)
- 16 bitów: -32 768 do 32 767 (ze znakiem) lub 0 do 65 535 (bez znaku)
- 32 bity: -2 147 483 648 do 2 147 483 647 (ze znakiem) lub 0 do 4 294 967 295 (bez znaku)

---

## 3) Arytmetyka stałoprzecinkowa (Fixed-Point Arithmetic)

### Definicja — intuicyjnie

**Stałoprzecinkowa** = sposób zapisu liczb z przecinkiem, gdzie **pozycja przecinka jest zawsze w tym samym miejscu**.

**Pomysł:** Zamiast przechowywać 3.5, przechowujemy 35 i "wiemy", że przecinek jest po pierwszej cyfrze.

**Przykład z życia:**
- Ceny w sklepie: 3.50 zł, 12.99 zł, 0.50 zł
- Możemy zapisać jako: 350 groszy, 1299 groszy, 50 groszy
- Wiemy, że zawsze dzielimy przez 100 (bo 1 zł = 100 groszy)

**W komputerze:** Zamiast dzielić przez 100, dzielimy przez potęgę 2 (np. 32, 64, 128).

### Definicja formalna

Liczba jest reprezentowana jako **liczba całkowita**, ale interpretowana jako wartość podzielona przez pewną stałą (zwykle potęgę 2).

### Reprezentacja — krok po kroku

**W pamięci komputera:**
- Przechowujemy: N = wartość całkowita (np. 112)

**Rzeczywista wartość:**
- Obliczamy: V = N / (2^f)

Gdzie:
- **f** — liczba bitów przeznaczona na część ułamkową (fractional part)
- **N** — wartość całkowita w pamięci
- **V** — rzeczywista wartość (z przecinkiem)

**Przykład:**
- Format: 5 bitów ułamkowych (f = 5)
- W pamięci: N = 112
- Rzeczywista wartość: V = 112 / (2^5) = 112 / 32 = 3.5 ✓

### Format Qm.n (lub Qf) — wyjaśnienie

**Notacja Qm.n:**
- **Q** — oznacza "fixed-point" (stałoprzecinkowa)
- **m** — liczba bitów na część całkowitą (integer part)
- **n** — liczba bitów na część ułamkową (fractional part)
- **Całkowita liczba bitów:** m + n

**Przykład: Q3.5**
- 3 bity na część całkowitą (może reprezentować 0-7)
- 5 bitów na część ułamkową (precyzja 1/32 = 0.03125)
- Razem: 8 bitów

**Czasem zapisywane jako Qn** (gdzie n to tylko część ułamkowa, część całkowita jest nieokreślona).

### Wzór na wartość rzeczywistą:

**Podstawowy wzór:**
V = N / (2^n)

Gdzie:
- **N** — wartość całkowita w pamięci
- **n** — liczba bitów części ułamkowej
- **V** — rzeczywista wartość

**Intuicja:** Dzielimy przez 2^n, bo każdy bit ułamkowy reprezentuje 1/(2^n).

**Przykład:**
- Format: Q3.5 (n = 5)
- W pamięci: N = 107
- V = 107 / (2^5) = 107 / 32 = 3.34375 ✓

### Przykład Q3.5 — szczegółowa analiza krok po kroku

**Format:** Q3.5 (8 bitów, 3 bity część całkowita, 5 bitów część ułamkowa)

**Wartość binarna:** 01101011

**Krok 1: Konwersja binarnego na dziesiętny**
- 01101011 (binarnie) = 107 (dziesiętnie)

**Krok 2: Rozkład na części**

Wizualizacja:
```
| 0 | 1  1  0 | 1  0  1  1  1 |
| ? | część   | część        |
|   | całkowita| ułamkowa     |
```

- **Bity części całkowitej (3 bity):** 011
  - Konwersja: 0×4 + 1×2 + 1×1 = 0 + 2 + 1 = **3**
  
- **Bity części ułamkowej (5 bitów):** 01011
  - Konwersja: 0×0.5 + 1×0.25 + 0×0.125 + 1×0.0625 + 1×0.03125
  - = 0 + 0.25 + 0 + 0.0625 + 0.03125 = **0.34375**

**Krok 3: Obliczenie wartości rzeczywistej**

Metoda 1 (przez wzór):
V = N / (2^n) = 107 / (2^5) = 107 / 32 = **3.34375** ✓

Metoda 2 (przez części):
V = część całkowita + część ułamkowa
V = 3 + 0.34375 = **3.34375** ✓

**Sprawdzenie:**
- Część całkowita: 3 ✓
- Część ułamkowa: 11 / 32 = 0.34375 ✓
- Razem: 3.34375 ✓

**Wniosek:** Liczba 3.34375 jest przechowywana jako 107 w pamięci, a komputer "wie", że trzeba podzielić przez 32, żeby dostać rzeczywistą wartość.

### Operacje arytmetyczne w stałoprzecinkowej — szczegółowy przewodnik

#### Dodawanie liczb stałoprzecinkowych

**Zasada:** Dodajemy wartości całkowite, wynik dzielimy przez 2^n

**Wzór:**
V1 + V2 = (N1 + N2) / (2^n)

**Kroki:**
1. Upewnij się, że oba operandy mają ten sam format (te same n)
2. Dodaj wartości całkowite N1 + N2
3. Wynik automatycznie ma poprawną wartość rzeczywistą

**Przykład:**
Format Q3.5 (n = 5 bitów ułamkowych)

Liczba 1: V1 = 2.5
- Wartość całkowita: N1 = 2.5 * 32 = 80

Liczba 2: V2 = 1.25
- Wartość całkowita: N2 = 1.25 * 32 = 40

Dodawanie:
- N_wynik = N1 + N2 = 80 + 40 = 120
- V_wynik = 120 / 32 = 3.75 ✓

**Uwaga:** Oba operandy muszą mieć ten sam format (te same n)! Jeśli nie, trzeba je najpierw znormalizować.

#### Odejmowanie liczb stałoprzecinkowych

**Zasada:** Odejmujemy wartości całkowite, wynik dzielimy przez 2^n

**Wzór:**
V1 - V2 = (N1 - N2) / (2^n)

**Kroki:**
1. Upewnij się, że oba operandy mają ten sam format
2. Odejmij wartości całkowite N1 - N2
3. Wynik automatycznie ma poprawną wartość rzeczywistą

**Przykład:**
Format Q3.5 (n = 5)

Liczba 1: V1 = 3.75
- N1 = 3.75 * 32 = 120

Liczba 2: V2 = 1.25
- N2 = 1.25 * 32 = 40

Odejmowanie:
- N_wynik = N1 - N2 = 120 - 40 = 80
- V_wynik = 80 / 32 = 2.5 ✓

#### Mnożenie liczb stałoprzecinkowych

**Zasada:** Mnożymy wartości całkowite, wynik ma podwójną precyzję części ułamkowej

**Wzór:**
V1 * V2 = (N1 * N2) / (2^(2n)) = (N1 * N2) / (2^n) / (2^n)

**Problem:** Po mnożeniu wynik ma 2n bitów części ułamkowej zamiast n!

**Rozwiązanie:** Trzeba **przesunąć w prawo** o n bitów (lub podzielić przez 2^n)

**Wzór końcowy:**
wynik = (N1 * N2) / (2^n)

**Kroki:**
1. Pomnóż wartości całkowite N1 * N2
2. Przesuń wynik w prawo o n bitów (lub podziel przez 2^n)
3. Wynik ma poprawną wartość rzeczywistą

**Przykład:**
Format Q3.5 (n = 5)

Liczba 1: V1 = 2.0
- N1 = 2.0 * 32 = 64

Liczba 2: V2 = 1.5
- N2 = 1.5 * 32 = 48

Mnożenie:
- N_pomnożone = N1 * N2 = 64 * 48 = 3072
- N_wynik = 3072 / 32 = 96 (przesunięcie w prawo o 5 bitów)
- V_wynik = 96 / 32 = 3.0 ✓

**Sprawdzenie:** 2.0 * 1.5 = 3.0 ✓

**Uwaga:** Mnożenie może powodować przepełnienie! N1 * N2 może być bardzo duże.

#### Dzielenie liczb stałoprzecinkowych

**Zasada:** Dzielimy wartości całkowite, ale trzeba najpierw zwiększyć precyzję dzielnej

**Wzór:**
V1 / V2 = (N1 / N2) * (2^n / 2^n) = (N1 * 2^n) / N2

**Problem:** Dzielenie całkowite traci precyzję części ułamkowej!

**Rozwiązanie:** Przed dzieleniem **przesuń w lewo** dzielną o n bitów (lub pomnóż przez 2^n)

**Kroki:**
1. Przesuń N1 w lewo o n bitów (N1 * 2^n)
2. Podziel przez N2
3. Wynik ma poprawną wartość rzeczywistą

**Przykład:**
Format Q3.5 (n = 5)

Liczba 1: V1 = 3.0
- N1 = 3.0 * 32 = 96

Liczba 2: V2 = 1.5
- N2 = 1.5 * 32 = 48

Dzielenie:
- N1_przesunięte = N1 * 32 = 96 * 32 = 3072
- N_wynik = 3072 / 48 = 64
- V_wynik = 64 / 32 = 2.0 ✓

**Sprawdzenie:** 3.0 / 1.5 = 2.0 ✓

**Uwaga:** Dzielenie przez zero powoduje błąd! Trzeba sprawdzić czy N2 ≠ 0.

### Zalety stałoprzecinkowej — szczegółowo:

**1. Determinizm — zawsze ta sama precyzja**
- **Co to znaczy:** Każda liczba ma dokładnie taką samą precyzję (np. zawsze 5 bitów ułamkowych = precyzja 1/32)
- **Przykład:** Format Q3.5 zawsze ma precyzję 0.03125
- **Korzyść:** Przewidywalne zachowanie — wiesz dokładnie, jaka jest precyzja

**2. Szybkość — operacje na liczbach całkowitych**
- **Co to znaczy:** Komputer wykonuje operacje na liczbach całkowitych (szybkie!)
- **Porównanie:** 
  - Stałoprzecinkowa: 80 + 40 = 120 (jedna operacja całkowita)
  - Zmiennoprzecinkowa: wymaga wyrównania wykładników, normalizacji, zaokrąglenia (wiele operacji)
- **Korzyść:** Szybsze wykonanie, szczególnie w systemach wbudowanych

**3. Przewidywalność — brak niespodzianek**
- **Co to znaczy:** Nie ma "magicznych" zaokrągleń, które mogą się zmieniać
- **Przykład:** 0.1 + 0.1 zawsze da ten sam wynik w stałoprzecinkowej
- **Korzyść:** Łatwiejsze debugowanie, brak ukrytych błędów

**4. Aplikacje czasu rzeczywistego**
- **Co to znaczy:** System musi reagować w określonym czasie
- **Przykład:** System hamulcowy w samochodzie — musi działać w < 10ms
- **Korzyść:** Deterministyczny czas wykonania — zawsze wiesz, ile zajmie

### Wady stałoprzecinkowej — szczegółowo:

**1. Ograniczony zakres — zależy od liczby bitów**
- **Problem:** Format Q3.5 może reprezentować tylko liczby od -8 do ~7.96875
- **Przykład:** Nie możesz zapisać 100.5 w formacie Q3.5 (za duże!)
- **Rozwiązanie:** Musisz wybrać format odpowiedni do zakresu danych

**2. Stała precyzja — nie można zmienić dokładności**
- **Problem:** Jeśli potrzebujesz większej precyzji, musisz zmienić format całego systemu
- **Przykład:** Format Q3.5 ma precyzję 0.03125 — nie możesz nagle mieć precyzji 0.0001
- **Rozwiązanie:** Wybierz format z odpowiednią precyzją na początku

**3. Trudność w implementacji — ręczne zarządzanie formatem**
- **Problem:** Musisz ręcznie:
  - Wybierać format (Qm.n)
  - Zarządzać przesunięciami bitowymi
  - Sprawdzać przepełnienia
  - Normalizować różne formaty
- **Przykład:** Mnożenie wymaga przesunięcia bitowego — łatwo o błąd
- **Rozwiązanie:** Użyj bibliotek lub gotowych typów

### Zastosowania stałoprzecinkowej — szczegółowe przykłady:

**1. Systemy wbudowane (embedded systems)**
- **Przykład:** Mikrokontrolery w samochodach, robotach, urządzeniach medycznych
- **Dlaczego:** 
  - Ograniczone zasoby (mało pamięci, wolny procesor)
  - Potrzebna deterministyczna precyzja
  - Szybkie operacje są kluczowe
- **Przykład kodu (pseudokod):**
```
// Format Q7.8 (15 bitów, precyzja 1/256)
int temperature = 2500;  // 25.00°C * 100 = 2500
int threshold = 3000;    // 30.00°C * 100 = 3000

if (temperature > threshold) {
    turnOnCooling();
}
```

**2. Przetwarzanie sygnałów cyfrowych (DSP)**
- **Przykład:** Filtry audio, kompresja dźwięku, przetwarzanie obrazów
- **Dlaczego:**
  - Wysoka częstotliwość próbkowania (tysiące operacji na sekundę)
  - Potrzebna szybkość
  - Precyzja jest wystarczająca (np. 16-bit audio)
- **Przykład:** Filtrowanie sygnału audio
  - Format Q15.16 (31 bitów)
  - Próbkowanie: 44100 Hz
  - Każda próbka wymaga obliczeń w czasie rzeczywistym

**3. Aplikacje finansowe**
- **Przykład:** Systemy bankowe, giełda, kalkulatory finansowe
- **Dlaczego:**
  - Potrzebna deterministyczna precyzja (pieniądze muszą się zgadzać!)
  - Brak błędów zaokrąglenia, które mogą się kumulować
  - Przewidywalne obliczenia
- **Przykład:** Obliczanie odsetek
  - Format Q15.16 dla kwot (precyzja do grosza)
  - Format Q7.8 dla stóp procentowych (precyzja 0.01%)

**4. Grafika komputerowa (niektóre algorytmy)**
- **Przykład:** Przetwarzanie pikseli, transformacje 2D/3D
- **Dlaczego:**
  - Szybkość jest kluczowa (60 FPS = 16ms na klatkę)
  - Precyzja wystarczająca dla pikseli
- **Przykład:** Przeskalowanie obrazu
  - Format Q8.8 dla współrzędnych pikseli
  - Szybkie obliczenia całkowite

### Praktyczne przykłady implementacji stałoprzecinkowej:

**Przykład 1: Przechowywanie ceny w groszach**

**Problem:** Chcesz przechowywać ceny produktów z precyzją do grosza.

**Rozwiązanie stałoprzecinkowe:**
```java
// Format: cena w groszach (dzielimy przez 100)
int priceInGrosze = 1250;  // 12.50 zł

// Operacje:
int price1 = 1250;  // 12.50 zł
int price2 = 750;   // 7.50 zł
int sum = price1 + price2;  // 2000 groszy = 20.00 zł ✓

// Mnożenie (uwaga na precyzję!):
int quantity = 3;
int total = (price1 * quantity) / 100;  // (1250 * 3) / 100 = 3750 / 100 = 37.50 zł
// Ale lepiej: total = price1 * quantity; // 3750 groszy = 37.50 zł
```

**Przykład 2: Przetwarzanie sygnału audio**

**Problem:** Przetwarzasz sygnał audio z precyzją 16-bit.

**Rozwiązanie stałoprzecinkowe:**
```java
// Format Q15.16 (31 bitów, precyzja 1/65536)
// Próbka audio: zakres -32768 do 32767

int sample1 = 16384;  // 0.25 w zakresie [-1.0, 1.0]
int sample2 = -8192; // -0.125 w zakresie [-1.0, 1.0]

// Mieszanie sygnałów (50% każdego):
int mixed = (sample1 + sample2) / 2;  // (16384 - 8192) / 2 = 4096
```

**Przykład 3: Obliczenia fizyczne w grze**

**Problem:** Symulujesz fizykę w grze 2D, potrzebujesz szybkich obliczeń.

**Rozwiązanie stałoprzecinkowe:**
```java
// Format Q12.4 (16 bitów) dla pozycji
// Precyzja: 1/16 = 0.0625 jednostek

int playerX = 160;  // 10.0 jednostek (160 / 16)
int playerY = 240;  // 15.0 jednostek (240 / 16)

// Ruch:
int speedX = 8;     // 0.5 jednostek/klatka (8 / 16)
playerX += speedX;  // 160 + 8 = 168 (10.5 jednostek)
```

### Porównanie formatów stałoprzecinkowych:

| Format | Bity całkowite | Bity ułamkowe | Zakres (bez znaku) | Precyzja | Przykład |
|--------|----------------|---------------|-------------------|----------|----------|
| **Q3.5** | 3 | 5 | 0-7.96875 | 0.03125 | 3.34375 |
| **Q7.8** | 7 | 8 | 0-127.996 | 0.00390625 | 25.5 |
| **Q15.16** | 15 | 16 | 0-32767.99998 | 0.00001526 | 1000.5 |
| **Q31.0** | 31 | 0 | 0-2147483647 | 1.0 | Tylko całkowite |

**Wybór formatu:**
- **Q3.5:** Małe liczby, niska precyzja (np. temperatury w °C)
- **Q7.8:** Średnie liczby, średnia precyzja (np. współrzędne 2D)
- **Q15.16:** Duże liczby, wysoka precyzja (np. sygnały audio)
- **Q31.0:** Tylko liczby całkowite (jak int)

### Problemy i rozwiązania w stałoprzecinkowej:

**Problem 1: Przepełnienie przy mnożeniu**

**Przykład:**
```java
// Format Q3.5 (8 bitów, zakres -8 do ~7.97)
int a = 100;  // 3.125 (100 / 32)
int b = 120;  // 3.75 (120 / 32)
int result = a * b;  // 100 * 120 = 12000
// Problem: 12000 > 255 (maksymalna wartość dla 8 bitów)!
```

**Rozwiązanie:**
- Użyj większego formatu (np. Q7.8 zamiast Q3.5)
- Sprawdź przed mnożeniem, czy wynik się zmieści
- Użyj 64-bitowych wartości pośrednich

**Problem 2: Utrata precyzji przy dzieleniu**

**Przykład:**
```java
// Format Q3.5
int a = 100;  // 3.125
int b = 3;    // 0.09375
int result = a / b;  // 100 / 3 = 33 (dzielenie całkowite)
// Rzeczywista wartość: 33 / 32 = 1.03125
// Prawidłowa wartość: 3.125 / 0.09375 = 33.33...
// Utrata precyzji!
```

**Rozwiązanie:**
- Przesuń dzielną w lewo przed dzieleniem (zwiększ precyzję)
- Użyj większego formatu dla wyniku

**Problem 3: Różne formaty w jednym systemie**

**Przykład:**
```java
// Format Q3.5
int temperature = 100;  // 3.125°C

// Format Q7.8 (inny format!)
int pressure = 25600;    // 100.0 Pa

// Problem: Nie można bezpośrednio dodać!
```

**Rozwiązanie:**
- Znormalizuj do wspólnego formatu przed operacjami
- Użyj jednego formatu w całym systemie (jeśli możliwe)

---

## 4) Arytmetyka zmiennoprzecinkowa (Floating-Point Arithmetic)

### Definicja — intuicyjnie

**Zmiennoprzecinkowa** = sposób zapisu liczb w formie **naukowej** (notacji wykładniczej), gdzie przecinek może się przesuwać.

**Przykład z życia:**
- Liczba: 1234567890
- W notacji naukowej: 1.234567890 × 10^9
- Widzisz? Przecinek przesunął się w lewo, a wykładnik (9) mówi, o ile miejsc

**W komputerze:** Zamiast 10, używamy 2 (bo binarny).

**Przykład:**
- Liczba: 44.75
- W formie naukowej (binarnie): 1.0111 × 2^5
- Mantysa: 1.0111 (część przed wykładnikiem)
- Wykładnik: 5 (mówi, o ile przesunąć przecinek)

### Definicja formalna

Liczba jest reprezentowana w formie **naukowej** (notacji wykładniczej):
V = (-1)^s * m * b^e

Gdzie:
- **s** — bit znaku (sign): 0 = dodatnia, 1 = ujemna
- **m** — mantysa (mantissa/significand): część ułamkowa (zwykle między 1.0 a 2.0)
- **b** — podstawa (base): zwykle 2 (binarny)
- **e** — wykładnik (exponent): mówi, o ile przesunąć przecinek

**Intuicja:**
- Mantysa = "cyfry znaczące" (np. 1.2345)
- Wykładnik = "gdzie jest przecinek" (np. × 10^5)

**Przykład:**
- Liczba: -44.75
- s = 1 (ujemna)
- m = 1.3984375 (mantysa)
- e = 5 (wykładnik)
- V = (-1)^1 * 1.3984375 * 2^5 = -1 * 1.3984375 * 32 = -44.75 ✓

### Standard IEEE 754 — co to jest?

**IEEE 754** to międzynarodowy standard reprezentacji liczb zmiennoprzecinkowych.

**Dlaczego standard?**
- Wszystkie komputery używają tego samego formatu
- Liczby są przenośne między systemami
- Zapewnia spójność i przewidywalność

**Dwa główne formaty:**
- **Float (32-bit)** — single precision (pojedyncza precyzja)
- **Double (64-bit)** — double precision (podwójna precyzja)

### Format IEEE 754 (32-bit, single precision, float)

**Struktura bitów:**
```
| S | EEEEEEEE | MMMMMMMMMMMMMMMMMMMMMMM |
| 1 |    8     |          23             |
```

**Wyjaśnienie każdej części:**

1. **S (1 bit) — znak:**
   - 0 = liczba dodatnia
   - 1 = liczba ujemna
   - Proste: jeden bit mówi czy liczba jest dodatnia czy ujemna

2. **E (8 bitów) — wykładnik:**
   - Zakres: 0-255 (8 bitów = 256 możliwości)
   - **Bias = 127** — odejmujemy 127, żeby móc reprezentować ujemne wykładniki
   - Rzeczywisty wykładnik = E - 127
   - Przykład: E = 132 → wykładnik = 132 - 127 = 5

3. **M (23 bity) — mantysa:**
   - Część ułamkowa mantysy (bez ukrytej jedynki!)
   - Wartość mantysy = 1 + M / (2^23)
   - Przykład: M = 3342336 → M_dec = 3342336 / 8388608 ≈ 0.3984375
   - Mantysa = 1 + 0.3984375 = 1.3984375

**Dlaczego "ukryta jedynka"?**
- W normalizowanej liczbie mantysa zawsze zaczyna się od 1.xxxxx
- Nie musimy przechowywać tej jedynki (zawsze jest!)
- Oszczędzamy 1 bit → więcej precyzji

### Wartość liczby (normalizowana) — wzór i wyjaśnienie

**Wzór:**
V = (-1)^s * (1 + M) * 2^(E - 127)

**Rozbijmy na części:**

1. **(-1)^s** — znak:
   - Jeśli s = 0: (-1)^0 = 1 (liczba dodatnia)
   - Jeśli s = 1: (-1)^1 = -1 (liczba ujemna)

2. **(1 + M)** — mantysa:
   - M = M_bin / (2^23) — konwersja binarnej mantysy na ułamek
   - Dodajemy 1 (ukryta jedynka)
   - Wynik: wartość między 1.0 a 2.0 (prawie)

3. **2^(E - 127)** — potęga 2:
   - E — wartość z 8 bitów (0-255)
   - Odejmujemy bias (127), żeby móc reprezentować ujemne wykładniki
   - Rzeczywisty wykładnik = E - 127 (zakres: -126 do 127)

**Przykład krok po kroku:**

Niech: S = 0, E = 132, M = 3342336

1. Znak: (-1)^0 = 1 (dodatnia)
2. Mantysa: M_dec = 3342336 / 8388608 ≈ 0.3984375
   - Mantysa = 1 + 0.3984375 = 1.3984375
3. Wykładnik: E - 127 = 132 - 127 = 5
4. Potęga: 2^5 = 32
5. Wynik: V = 1 * 1.3984375 * 32 = 44.75 ✓

### Wyjątkowe wartości:

#### Zero
- E = 0, M = 0
- Może być +0 i -0

#### Nieskończoność (Infinity)
- E = 255 (wszystkie jedynki), M = 0
- +∞ lub -∞ w zależności od znaku

#### NaN (Not a Number)
- E = 255, M ≠ 0
- Wynik nieprawidłowych operacji (np. pierwiastek z -1, 0/0)

#### Liczby zdenormalizowane (denormalized/subnormal)
- E = 0, M ≠ 0
- V = (-1)^s * M * 2^(-126) (bez ukrytej jedynki)

### Format IEEE 754 (64-bit, double precision, double)

```
| S | EEEEEEEEEEE | MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM |
| 1 |     11      |                         52                          |
```

- **S** (1 bit) — znak
- **E** (11 bitów) — wykładnik (bias = 1023)
- **M** (52 bity) — mantysa

### Wartość liczby (normalizowana):
V = (-1)^s * (1 + M) * 2^(E - 1023)

Gdzie:
- M = M_bin / (2^52)
- E — wykładnik (0-2047, z bias 1023)

### Zakresy i precyzja — szczegółowa analiza

#### Float (32-bit) — szczegółowe wartości:

**Struktura:**
- 1 bit znaku
- 8 bitów wykładnika (bias 127)
- 23 bity mantysy

**Zakres wartości:**

**Najmniejsza wartość dodatnia (zdenormalizowana):**
- E = 0, M = 1 (najmniejsza niezerowa)
- V = 1 × 2^(-126) ≈ 1.4 × 10^(-45)
- **Co to znaczy:** Najmniejsza liczba, jaką można reprezentować

**Najmniejsza wartość znormalizowana:**
- E = 1, M = 0 (najmniejsza znormalizowana)
- V = 1.0 × 2^(-126) ≈ 1.2 × 10^(-38)
- **Co to znaczy:** Najmniejsza "normalna" liczba

**Największa wartość:**
- E = 254 (nie 255, bo 255 to Infinity), M = wszystkie jedynki
- V = (1 + (2^23 - 1)/2^23) × 2^127 ≈ 3.4 × 10^38
- **Co to znaczy:** Największa liczba przed nieskończonością

**Precyzja:**
- 23 bity mantysy = 2^23 ≈ 8 388 608 możliwych wartości mantysy
- **Oznacza to:** Możesz rozróżnić ~8.4 miliona różnych wartości
- **W praktyce:** ~7 cyfr dziesiętnych precyzji
- **Przykład:** 1234567.89 może być reprezentowane jako 1234567 lub 1234568 (utrata precyzji)

**Przykłady wartości float:**
- 0.0 — zero
- 1.0 — jeden
- 3.14159 — pi (z przybliżeniem)
- 1000000.0 — milion
- 0.000001 — jedna milionowa
- 3.4028235E+38 — maksymalna wartość

#### Double (64-bit) — szczegółowe wartości:

**Struktura:**
- 1 bit znaku
- 11 bitów wykładnika (bias 1023)
- 52 bity mantysy

**Zakres wartości:**

**Najmniejsza wartość dodatnia (zdenormalizowana):**
- E = 0, M = 1
- V = 1 × 2^(-1022) ≈ 4.9 × 10^(-324)
- **Co to znaczy:** Ekstremalnie mała liczba

**Najmniejsza wartość znormalizowana:**
- E = 1, M = 0
- V = 1.0 × 2^(-1022) ≈ 2.2 × 10^(-308)

**Największa wartość:**
- E = 2046, M = wszystkie jedynki
- V ≈ 1.8 × 10^308
- **Co to znaczy:** Ogromna liczba (więcej niż atomy we wszechświecie!)

**Precyzja:**
- 52 bity mantysy = 2^52 ≈ 4.5 × 10^15 możliwych wartości
- **Oznacza to:** Możesz rozróżnić ~4.5 biliarda różnych wartości
- **W praktyce:** ~15-17 cyfr dziesiętnych precyzji
- **Przykład:** 123456789012345.67 może być reprezentowane z dużą precyzją

**Przykłady wartości double:**
- 0.0 — zero
- 1.0 — jeden
- 3.141592653589793 — pi (wysoka precyzja)
- 1000000000000.0 — bilion
- 0.0000000000001 — jedna bilionowa
- 1.7976931348623157E+308 — maksymalna wartość

### Porównanie Float vs Double — kiedy używać którego?

**Kiedy używać Float (32-bit):**
- ✅ Oszczędność pamięci (2x mniej niż double)
- ✅ Szybsze obliczenia (mniej bitów do przetworzenia)
- ✅ Wystarczająca precyzja dla większości zastosowań
- ✅ Grafika komputerowa (współrzędne, kolory)
- ✅ Przetwarzanie sygnałów (audio, wideo)
- ✅ Aplikacje mobilne (oszczędność pamięci)

**Przykład użycia float:**
```java
// Grafika 3D - współrzędne punktów
float x = 1.5f;
float y = 2.3f;
float z = 0.8f;

// Precyzja wystarczająca dla pikseli
// Oszczędność pamięci (3 × 4 bajty = 12 bajtów zamiast 24)
```

**Kiedy używać Double (64-bit):**
- ✅ Wysoka precyzja jest kluczowa
- ✅ Obliczenia naukowe (fizyka, chemia, matematyka)
- ✅ Finanse (gdy potrzebna wysoka precyzja)
- ✅ Obliczenia inżynierskie
- ✅ Gdy błędy zaokrąglenia mogą się kumulować

**Przykład użycia double:**
```java
// Obliczenia naukowe - wysoka precyzja
double pi = 3.141592653589793;
double e = 2.718281828459045;

// Obliczenia wymagające dużej precyzji
double result = Math.sin(pi / 2);  // Powinno być 1.0
```

**Tabela porównawcza:**

| Aspekt | Float (32-bit) | Double (64-bit) |
|--------|----------------|-----------------|
| **Rozmiar** | 4 bajty | 8 bajtów |
| **Precyzja** | ~7 cyfr | ~15-17 cyfr |
| **Zakres** | ±3.4 × 10^38 | ±1.8 × 10^308 |
| **Szybkość** | Szybszy | Wolniejszy |
| **Pamięć** | Mniej | Więcej |
| **Zastosowania** | Grafika, audio, mobilne | Nauka, inżynieria, finanse |

### Wzór na liczbę możliwych wartości — szczegółowa analiza:

**Dla mantysy o p bitach (bez ukrytej jedynki):**
liczby możliwe = 2^(p+1)

**Wyjaśnienie:**
- p bitów mantysy → 2^p możliwych wartości mantysy
- +1 (ukryta jedynka) → 2^(p+1) możliwych wartości mantysy
- × 2 (dodatnia/ujemna) → 2 × 2^(p+1) = 2^(p+2) możliwych liczb (dla każdego wykładnika)

**Przykłady:**

**Float (p = 23):**
- Możliwe wartości mantysy: 2^24 = 16 777 216
- Dla każdego wykładnika (254 możliwe wartości znormalizowane)
- Razem: ~4.2 miliarda różnych liczb znormalizowanych

**Double (p = 52):**
- Możliwe wartości mantysy: 2^53 = 9 007 199 254 740 992
- Dla każdego wykładnika (2046 możliwe wartości znormalizowane)
- Razem: ~18.4 × 10^18 różnych liczb znormalizowanych

**Wniosek:** Double ma znacznie więcej możliwych wartości niż float!

### Przykład konwersji: Float → Wartość dziesiętna — szczegółowo

**Dane wejściowe:** 01000010011001100000000000000000 (binarnie, 32 bity)

**Krok 1: Podział na części**

Wizualizacja:
```
| 0 | 10000100 | 11001100000000000000000 |
| S |    E     |           M             |
| 1 |    8     |           23            |
```

**Krok 2: Analiza znaku (S)**

- S = 0 (binarnie)
- (-1)^0 = 1
- **Wniosek:** Liczba dodatnia ✓

**Krok 3: Analiza wykładnika (E)**

- E = 10000100 (binarnie)
- Konwersja na dziesiętny:
  ```
  1×128 + 0×64 + 0×32 + 0×16 + 0×8 + 1×4 + 0×2 + 0×1
  = 128 + 4 = 132
  ```
- Rzeczywisty wykładnik: E - 127 = 132 - 127 = **5**
- Potęga: 2^5 = **32**

**Krok 4: Analiza mantysy (M)**

- M = 11001100000000000000000 (binarnie, 23 bity)
- Konwersja na dziesiętny:
  ```
  Pozycje (od lewej): -1, -2, -3, ..., -23
  Wagi: 0.5, 0.25, 0.125, 0.0625, ...
  
  1×0.5 + 1×0.25 + 0×0.125 + 0×0.0625 + 1×0.03125 + 1×0.015625 + ...
  = 0.5 + 0.25 + 0.03125 + 0.015625 + ... ≈ 0.3984375
  ```
- Lub prościej: M_dec = wartość_binarna / (2^23)
  - M_dec = 3342336 / 8388608 ≈ **0.3984375**
- Mantysa = 1 + M_dec = 1 + 0.3984375 = **1.3984375**

**Krok 5: Obliczenie wartości**

Wzór: V = (-1)^s * (1 + M) * 2^(E - 127)

Podstawiamy:
- V = 1 * 1.3984375 * 2^5
- V = 1.3984375 * 32
- V = **44.75** ✓

**Sprawdzenie:** 44.75 to poprawna wartość!

**Podsumowanie:**
- Binarnie: 01000010011001100000000000000000
- Dziesiętnie: 44.75
- Znak: dodatnia
- Wykładnik: 5 (przesunięcie przecinka o 5 miejsc)
- Mantysa: 1.3984375

### Operacje arytmetyczne w zmiennoprzecinkowej — szczegółowy przewodnik

#### Dodawanie liczb zmiennoprzecinkowych

**Algorytm krok po kroku:**

1. **Sprawdź wartości specjalne** (NaN, Infinity, Zero)
2. **Wyrównaj wykładniki** — dopasuj mniejszy wykładnik do większego
3. **Dodaj mantysy** — dodaj mantysy (z uwzględnieniem znaków)
4. **Znormalizuj wynik** — przesuń mantysę, aby miała postać 1.xxxxx
5. **Zaokrąglij** — zaokrąglij mantysę do dostępnej precyzji
6. **Sprawdź overflow/underflow**

**Wzór:**
V1 + V2 = (m1 * 2^e1) + (m2 * 2^e2)

Jeśli e1 > e2:
= (m1 + m2 * 2^(e2 - e1)) * 2^e1

**Przykład szczegółowy:**

Liczba 1: V1 = 5.25 = 1.3125 * 2^2
- Mantysa: m1 = 1.3125 (binarnie: 1.0101)
- Wykładnik: e1 = 2

Liczba 2: V2 = 2.5 = 1.25 * 2^1
- Mantysa: m2 = 1.25 (binarnie: 1.0100)
- Wykładnik: e2 = 1

**Kroki:**
1. Wyrównaj wykładniki: e2 < e1, więc przesuń m2 w prawo o (e1 - e2) = 1 bit
   - m2_wyrównane = m2 / 2^(e1 - e2) = 1.25 / 2^1 = 0.625
2. Dodaj mantysy: m_wynik = m1 + m2_wyrównane = 1.3125 + 0.625 = 1.9375
3. Znormalizuj: m_wynik = 1.9375 jest już znormalizowana (1.xxxxx)
4. Wynik: V_wynik = 1.9375 * 2^2 = 7.75 ✓

**Sprawdzenie:** 5.25 + 2.5 = 7.75 ✓

**Uwagi:**
- Wyrównanie wykładników może powodować utratę precyzji (mniejsza mantysa jest przesuwana)
- Jeśli mantysy mają różne znaki, wykonujemy odejmowanie

#### Odejmowanie liczb zmiennoprzecinkowych

**Algorytm:** Podobny do dodawania, ale odejmujemy mantysy

**Kroki:**
1. Sprawdź wartości specjalne
2. Wyrównaj wykładniki
3. **Odejmij mantysy** (z uwzględnieniem znaków)
4. Znormalizuj wynik
5. Zaokrąglij
6. Sprawdź overflow/underflow

**Wzór:**
V1 - V2 = (m1 * 2^e1) - (m2 * 2^e2)

Jeśli e1 > e2:
= (m1 - m2 * 2^(e2 - e1)) * 2^e1

**Przykład szczegółowy:**

Liczba 1: V1 = 7.75 = 1.9375 * 2^2
- Mantysa: m1 = 1.9375
- Wykładnik: e1 = 2

Liczba 2: V2 = 2.5 = 1.25 * 2^1
- Mantysa: m2 = 1.25
- Wykładnik: e2 = 1

**Kroki:**
1. Wyrównaj wykładniki: m2_wyrównane = 1.25 / 2^1 = 0.625
2. Odejmij mantysy: m_wynik = 1.9375 - 0.625 = 1.3125
3. Znormalizuj: m_wynik = 1.3125 jest już znormalizowana
4. Wynik: V_wynik = 1.3125 * 2^2 = 5.25 ✓

**Sprawdzenie:** 7.75 - 2.5 = 5.25 ✓

**Uwaga:** Odejmowanie podobnych liczb może powodować **catastrophic cancellation** (utratę precyzji)!

#### Mnożenie liczb zmiennoprzecinkowych

**Algorytm krok po kroku:**

1. **Dodaj wykładniki:** e_wynik = e1 + e2 - bias
   - Odejmujemy bias, bo każdy wykładnik już miał bias
2. **Pomnóż mantysy:** m_wynik = m1 * m2
3. **Znormalizuj wynik** — jeśli m_wynik ≥ 2, przesuń w prawo i zwiększ wykładnik
4. **Zaokrąglij** mantysę
5. **Sprawdź overflow/underflow**

**Wzór:**
V1 * V2 = (m1 * m2) * 2^(e1 + e2)

**Przykład szczegółowy:**

Liczba 1: V1 = 5.0 = 1.25 * 2^2
- Mantysa: m1 = 1.25
- Wykładnik: e1 = 2 (w pamięci: 2 + 127 = 129)

Liczba 2: V2 = 2.0 = 1.0 * 2^1
- Mantysa: m2 = 1.0
- Wykładnik: e2 = 1 (w pamięci: 1 + 127 = 128)

**Kroki:**
1. Dodaj wykładniki: e_wynik = (129 - 127) + (128 - 127) = 2 + 1 = 3
   - W pamięci: 3 + 127 = 130
2. Pomnóż mantysy: m_wynik = 1.25 * 1.0 = 1.25
3. Znormalizuj: m_wynik = 1.25 jest już znormalizowana (1.xxxxx)
4. Wynik: V_wynik = 1.25 * 2^3 = 10.0 ✓

**Sprawdzenie:** 5.0 * 2.0 = 10.0 ✓

**Uwagi:**
- Mnożenie mantys może dać wynik ≥ 2.0, wtedy trzeba znormalizować
- Wykładniki są dodawane, więc zakres może się szybko zwiększyć (overflow)

#### Dzielenie liczb zmiennoprzecinkowych

**Algorytm krok po kroku:**

1. **Odejmij wykładniki:** e_wynik = e1 - e2 + bias
   - Dodajemy bias, bo odejmując tracimy jeden bias
2. **Podziel mantysy:** m_wynik = m1 / m2
3. **Znormalizuj wynik** — jeśli m_wynik < 1, przesuń w lewo i zmniejsz wykładnik
4. **Zaokrąglij** mantysę
5. **Sprawdź overflow/underflow i dzielenie przez zero**

**Wzór:**
V1 / V2 = (m1 / m2) * 2^(e1 - e2)

**Przykład szczegółowy:**

Liczba 1: V1 = 10.0 = 1.25 * 2^3
- Mantysa: m1 = 1.25
- Wykładnik: e1 = 3 (w pamięci: 3 + 127 = 130)

Liczba 2: V2 = 2.0 = 1.0 * 2^1
- Mantysa: m2 = 1.0
- Wykładnik: e2 = 1 (w pamięci: 1 + 127 = 128)

**Kroki:**
1. Odejmij wykładniki: e_wynik = (130 - 127) - (128 - 127) = 3 - 1 = 2
   - W pamięci: 2 + 127 = 129
2. Podziel mantysy: m_wynik = 1.25 / 1.0 = 1.25
3. Znormalizuj: m_wynik = 1.25 jest już znormalizowana
4. Wynik: V_wynik = 1.25 * 2^2 = 5.0 ✓

**Sprawdzenie:** 10.0 / 2.0 = 5.0 ✓

**Uwagi:**
- Dzielenie przez zero → Infinity (lub -Infinity)
- 0 / 0 → NaN
- Dzielenie mantys może dać wynik < 1.0, wtedy trzeba znormalizować
- Wykładniki są odejmowane, więc zakres może się szybko zmniejszyć (underflow)

### Podsumowanie operacji

| Operacja | Wykładniki | Mantysy | Normalizacja |
|----------|------------|--------|---------------|
| **Dodawanie** | Wyrównaj do większego | Dodaj | Tak (jeśli potrzeba) |
| **Odejmowanie** | Wyrównaj do większego | Odejmij | Tak (jeśli potrzeba) |
| **Mnożenie** | Dodaj (odejmij bias) | Pomnóż | Tak (jeśli m ≥ 2) |
| **Dzielenie** | Odejmij (dodaj bias) | Podziel | Tak (jeśli m < 1) |

**Wszystkie operacje wymagają:**
- Sprawdzenia wartości specjalnych (NaN, Infinity, Zero)
- Zaokrąglenia wyniku
- Sprawdzenia overflow/underflow

---

## 5) Błędy i precyzja — dlaczego liczby nie są idealne?

### Wprowadzenie — dlaczego są błędy?

**Problem:** Komputer ma **ograniczoną pamięć** (skończoną liczbę bitów).

**Przykład:**
- Chcesz zapisać: 0.1 (dziesiętnie)
- W binarnym: 0.00011001100110011... (nieskończone!)
- Komputer ma tylko 23 bity na mantysę → musi **zaokrąglić**
- Wynik: 0.1 ≈ 0.100000001490116119384765625
- **Błąd!** Różnica jest mała, ale istnieje.

**Wniosek:** Większość liczb dziesiętnych nie ma dokładnej reprezentacji binarnej!

### Błąd względny (Relative Error) — co to jest?

**Definicja:** Jak duży jest błąd w stosunku do wartości?

**Wzór:**
ε_rel = |x_rzeczywista - x_reprezentowana| / |x_rzeczywista|

**Intuicja:** Błąd względny mówi, jaki procent wartości jest błędny.

**Przykład:**
- Wartość rzeczywista: 100.0
- Wartość reprezentowana: 100.0001
- Błąd bezwzględny: |100.0 - 100.0001| = 0.0001
- Błąd względny: 0.0001 / 100.0 = 0.0001% (bardzo mały!)

**Inny przykład:**
- Wartość rzeczywista: 0.1
- Wartość reprezentowana: 0.10000000149
- Błąd bezwzględny: 0.00000000149
- Błąd względny: 0.00000000149 / 0.1 = 0.00000149% (też mały, ale widoczny przy wielu operacjach)

### Błąd bezwzględny (Absolute Error) — co to jest?

**Definicja:** Jaka jest różnica między wartością rzeczywistą a reprezentowaną?

**Wzór:**
ε_abs = |x_rzeczywista - x_reprezentowana|

**Intuicja:** Błąd bezwzględny mówi, o ile się różnią (w tych samych jednostkach).

**Przykład:**
- Wartość rzeczywista: 3.14159
- Wartość reprezentowana: 3.14160
- Błąd bezwzględny: |3.14159 - 3.14160| = 0.00001

**Kiedy używać którego?**
- **Błąd bezwzględny:** gdy chcesz wiedzieć, o ile się różnią
- **Błąd względny:** gdy chcesz wiedzieć, jaki procent jest błędny (lepszy dla porównań)

### Machine Epsilon (ε) — najmniejsza różnica

**Definicja:** najmniejsza liczba taka, że 1 + ε > 1

**Intuicja:** Machine epsilon to najmniejsza liczba, którą można dodać do 1, żeby komputer zauważył różnicę.

**Dla formatu z p bitami mantysy:**
ε = 2^(-p)

**Wyjaśnienie:**
- Im więcej bitów mantysy, tym mniejszy epsilon (lepsza precyzja)
- Epsilon mówi o **precyzji** reprezentacji

**Wartości:**
- **Float (23 bity):** ε ≈ 1.19 × 10^(-7)
  - Oznacza: możemy rozróżnić liczby różniące się o ~0.0000001
  - Precyzja: ~7 cyfr dziesiętnych
  
- **Double (52 bity):** ε ≈ 2.22 × 10^(-16)
  - Oznacza: możemy rozróżnić liczby różniące się o ~0.0000000000000002
  - Precyzja: ~15-17 cyfr dziesiętnych

**Przykład praktyczny:**
```java
float a = 1.0f;
float b = 1.0f + 1e-8f;  // 0.00000001
// a == b może być true! (bo różnica < epsilon)
```

### Błąd zaokrąglenia (Rounding Error) — szczegółowo

**Problem:** Liczby dziesiętne często nie mają dokładnej reprezentacji binarnej.

**Dlaczego?**
- System dziesiętny: podstawa 10 (czynniki: 2 i 5)
- System binarny: podstawa 2 (tylko czynnik 2)
- Liczby z czynnikiem 5 w mianowniku (np. 1/5 = 0.2) nie mają skończonej reprezentacji binarnej!

**Przykład 1: 0.1 (dziesiętnie)**

Konwersja na binarny:
1. 0.1 × 2 = 0.2 → zapisujemy 0
2. 0.2 × 2 = 0.4 → zapisujemy 0
3. 0.4 × 2 = 0.8 → zapisujemy 0
4. 0.8 × 2 = 1.6 → zapisujemy 1, zostaje 0.6
5. 0.6 × 2 = 1.2 → zapisujemy 1, zostaje 0.2
6. ... (cykl się powtarza: 0011 0011 0011 ...)

Wynik: 0.1 (dziesiętnie) = 0.00011001100110011... (binarnie, **nieskończone!**)

W float (23 bity mantysy):
- Komputer zaokrągla do: 0.100000001490116119384765625
- **Błąd:** 0.000000001490116119384765625

**Przykład 2: 0.2 (dziesiętnie)**

- 0.2 = 2 × 0.1
- Ma ten sam problem co 0.1 (nieskończona reprezentacja binarna)

**Przykład 3: 0.5 (dziesiętnie)**

Konwersja:
- 0.5 × 2 = 1.0 → zapisujemy 1, zostaje 0.0 (koniec!)

Wynik: 0.5 (dziesiętnie) = 0.1 (binarnie) ✓

**Wniosek:** 0.5 ma dokładną reprezentację binarną (bo 0.5 = 1/2 = 2^(-1))

**Które liczby mają dokładną reprezentację?**
- Ułamki postaci 1/(2^n): 0.5, 0.25, 0.125, 0.0625, ...
- Ułamki postaci k/(2^n): 0.75, 0.375, 0.625, ...
- **Większość innych nie ma!**

### Błędy kumulacyjne

Przy wielu operacjach błędy się sumują!

**Przykład:**
```java
float sum = 0.0f;
for (int i = 0; i < 1000; i++) {
    sum += 0.1f;  // Każde dodanie wprowadza mały błąd
}
// sum != 100.0 (będzie błąd zaokrąglenia!)
```

---

## 6) Problemy i pułapki

### 1. Porównywanie liczb zmiennoprzecinkowych — szczegółowo

**Problem:** Liczby zmiennoprzecinkowe mają błędy zaokrąglenia, więc rzadko są dokładnie równe.

**Przykład problemu:**
```java
double a = 0.1 + 0.2;
double b = 0.3;

if (a == b) {
    System.out.println("Równe!");
} else {
    System.out.println("Nie równe!");  // To się wyświetli!
}

System.out.println("a = " + a);  // a = 0.30000000000000004
System.out.println("b = " + b);  // b = 0.3
```

**Dlaczego?**
- 0.1 w binarnym = 0.00011001100110011... (nieskończone)
- 0.2 w binarnym = 0.0011001100110011... (nieskończone)
- 0.1 + 0.2 = 0.30000000000000004 (błąd zaokrąglenia!)

**Rozwiązanie 1: Porównanie z epsilon (stałym)**

**ZŁE:**
```java
if (a == b) { }  // Rzadko działa poprawnie!
```

**DOBRE:**
```java
double epsilon = 1e-9;  // 0.000000001
if (Math.abs(a - b) < epsilon) {
    // Liczby są "równe" (różnica < epsilon)
}
```

**Przykład:**
```java
double a = 0.1 + 0.2;
double b = 0.3;
double epsilon = 1e-9;

if (Math.abs(a - b) < epsilon) {
    System.out.println("Równe!");  // To się wyświetli ✓
}
```

**Rozwiązanie 2: Porównanie względne (lepsze dla dużych liczb)**

**Wzór:**
|a - b| < ε * max(|a|, |b|)

**Kod:**
```java
double epsilon = 1e-9;
if (Math.abs(a - b) < epsilon * Math.max(Math.abs(a), Math.abs(b))) {
    // Równe względnie
}
```

**Dlaczego względne?**
- Dla małych liczb: |a - b| < 1e-9 jest OK
- Dla dużych liczb: |a - b| < 1e-9 może być za małe
- **Przykład:**
  - a = 1000000.0, b = 1000000.0000001
  - Różnica bezwzględna: 0.0000001 < 1e-9? NIE (za duża!)
  - Różnica względna: 0.0000001 / 1000000 = 1e-13 < 1e-9? TAK ✓

**Przykład praktyczny — funkcja porównująca:**
```java
boolean areEqual(double a, double b) {
    double epsilon = 1e-9;
    // Dla liczb bliskich zero
    if (Math.abs(a) < epsilon && Math.abs(b) < epsilon) {
        return true;  // Oba bliskie zero
    }
    // Porównanie względne
    return Math.abs(a - b) < epsilon * Math.max(Math.abs(a), Math.abs(b));
}
```

**Testy:**
```java
areEqual(0.1 + 0.2, 0.3);           // true ✓
areEqual(1000000.0, 1000000.0001);  // false (różnica za duża)
areEqual(0.0, 0.0000000001);       // true (oba bliskie zero)
```

### 2. Utrata precyzji przy odejmowaniu (Catastrophic Cancellation) — szczegółowo

**Problem:** Gdy odejmujemy dwie bardzo podobne liczby, tracimy precyzję.

**Dlaczego?**
- Odejmowanie eliminuje wspólne cyfry znaczące
- Zostają tylko różnice, które mają mniej cyfr znaczących
- Błędy zaokrąglenia stają się względnie większe

**Przykład szczegółowy:**

**Sytuacja:**
```
a = 1.23456789012345  (15 cyfr znaczących)
b = 1.23456789012344  (15 cyfr znaczących)
a - b = 0.00000000000001  (tylko 1 cyfra znacząca!)
```

**Problem:** Z 15 cyfr znaczących zostało tylko 1!

**Przykład w kodzie:**
```java
double a = 1.23456789012345;
double b = 1.23456789012344;
double result = a - b;
System.out.println(result);  // Może być 0.0 lub bardzo niedokładne!
```

**Przykład problematyczny 1: Różnica pierwiastków**

**Kod problematyczny:**
```java
double x = 1000000.0;
double result = Math.sqrt(x + 1) - Math.sqrt(x);
// Problem: sqrt(1000001) ≈ 1000.0005
//         sqrt(1000000) = 1000.0
//         Różnica: 0.0005, ale błędy zaokrąglenia mogą zepsuć wynik!
```

**Analiza błędu:**
- sqrt(1000001) ≈ 1000.000499999...
- sqrt(1000000) = 1000.0
- W float: oba mają ~7 cyfr znaczących
- Różnica: 0.0005 ma tylko 1 cyfrę znaczącą!
- Błędy zaokrąglenia mogą całkowicie zepsuć wynik

**Lepsze podejście — racjonalizacja:**
```java
// Zamiast: sqrt(x + 1) - sqrt(x)
// Użyj: 1 / (sqrt(x + 1) + sqrt(x))

double x = 1000000.0;
double result = 1.0 / (Math.sqrt(x + 1) + Math.sqrt(x));
// Teraz: mianownik ma duże wartości, dzielenie jest stabilne
```

**Dlaczego lepsze?**
- Mianownik: sqrt(x+1) + sqrt(x) ≈ 2000.0 (duża wartość, stabilna)
- Dzielenie: 1.0 / 2000.0 = 0.0005 (precyzyjne!)
- Brak odejmowania podobnych liczb

**Przykład problematyczny 2: Różnica kwadratów**

**Kod problematyczny:**
```java
double a = 1000.0001;
double b = 1000.0;
double result = a * a - b * b;  // (a² - b²)
// Problem: a² ≈ 1000000.2, b² = 1000000.0
// Różnica: 0.2, ale błędy zaokrąglenia!
```

**Lepsze podejście — faktoryzacja:**
```java
// Zamiast: a² - b²
// Użyj: (a - b)(a + b)

double a = 1000.0001;
double b = 1000.0;
double result = (a - b) * (a + b);
// a - b = 0.0001 (mała, ale precyzyjna)
// a + b = 2000.0001 (duża, stabilna)
// Iloczyn: 0.0001 × 2000.0001 = 0.2 (precyzyjne!)
```

**Przykład problematyczny 3: Obliczanie delty (wzór kwadratowy)**

**Problem:** Wzór kwadratowy: x = (-b ± sqrt(b² - 4ac)) / (2a)

**Kod problematyczny:**
```java
double a = 1.0;
double b = 1000.0;
double c = 1.0;

double discriminant = b * b - 4 * a * c;  // 1000000 - 4 = 999996
double sqrt_disc = Math.sqrt(discriminant);
double x1 = (-b + sqrt_disc) / (2 * a);   // (-1000 + 999.998) / 2
double x2 = (-b - sqrt_disc) / (2 * a);   // (-1000 - 999.998) / 2
// Problem: -b + sqrt_disc ≈ -0.002 (odejmowanie podobnych!)
```

**Lepsze podejście:**
```java
// Dla x1: użyj wzoru z sumą (stabilny)
double x1 = (-b + sqrt_disc) / (2 * a);

// Dla x2: użyj wzoru z różnicą LUB relacji x1 * x2 = c/a
double x2 = c / (a * x1);  // Z relacji Viete'a
// LUB
double x2 = (-b - sqrt_disc) / (2 * a);  // To jest OK (duże liczby)
```

**Jak unikać catastrophic cancellation:**
1. **Racjonalizacja** — usuń odejmowanie z mianownika
2. **Faktoryzacja** — rozłóż na czynniki
3. **Alternatywne wzory** — użyj matematycznie równoważnych, ale numerycznie stabilnych
4. **Większa precyzja** — użyj double zamiast float

### 3. Przepełnienie (Overflow) — szczegółowo

**Problem:** Gdy wynik jest zbyt duży dla reprezentacji, staje się nieskończonością.

**Jak działa:**
- Wykładnik osiąga maksymalną wartość (255 dla float, 2047 dla double)
- Liczba staje się +∞ lub -∞
- Dalsze operacje dają NaN lub ∞

**Przykłady overflow:**

**Przykład 1: Mnożenie dużych liczb**
```java
float a = 1e20f;  // 10^20
float b = 1e20f;  // 10^20
float result = a * b;  // 10^40 > 3.4 × 10^38 → +∞
System.out.println(result);  // Infinity
```

**Przykład 2: Potęgowanie**
```java
double base = 10.0;
double exp = 400.0;
double result = Math.pow(base, exp);  // 10^400 > 1.8 × 10^308 → +∞
System.out.println(result);  // Infinity
```

**Przykład 3: Sumowanie**
```java
float sum = 0.0f;
for (int i = 0; i < 1000; i++) {
    sum += 1e30f;  // Każde dodanie = 10^30
    // Po 35 iteracjach: 35 × 10^30 > 3.4 × 10^38 → +∞
}
System.out.println(sum);  // Infinity
```

**Jak wykryć overflow:**
```java
double result = a * b;
if (Double.isInfinite(result)) {
    System.out.println("Overflow!");
    // Obsłuż błąd
}
```

**Jak unikać overflow:**
1. **Sprawdzaj przed operacjami:**
```java
if (a > Double.MAX_VALUE / b) {
    // Overflow! Obsłuż błąd
} else {
    double result = a * b;
}
```

2. **Użyj logarytmów dla bardzo dużych liczb:**
```java
// Zamiast: a^b (może overflow)
// Użyj: exp(b * log(a))
double result = Math.exp(b * Math.log(a));
```

3. **Użyj większej precyzji (jeśli możliwe):**
```java
// BigDecimal dla bardzo dużych liczb
BigDecimal a = new BigDecimal("1e100");
BigDecimal b = new BigDecimal("1e100");
BigDecimal result = a.multiply(b);  // Nie ma overflow!
```

**Wartości graniczne:**
- **Float:** MAX_VALUE ≈ 3.4028235 × 10^38
- **Double:** MAX_VALUE ≈ 1.7976931348623157 × 10^308

### 4. Niedomiar (Underflow) — szczegółowo

**Problem:** Gdy wynik jest zbyt mały, może stać się zerem lub liczbą zdenormalizowaną.

**Jak działa:**
- Wykładnik osiąga minimalną wartość (1 dla znormalizowanych)
- Liczba staje się zdenormalizowaną (bardzo mała precyzja)
- Lub staje się zerem (0.0)

**Przykłady underflow:**

**Przykład 1: Dzielenie małych liczb**
```java
double a = 1e-200;  // 10^(-200)
double b = 1e100;   // 10^100
double result = a / b;  // 10^(-300) < 2.2 × 10^(-308) → 0.0
System.out.println(result);  // 0.0
```

**Przykład 2: Mnożenie bardzo małych liczb**
```java
double a = 1e-150;  // 10^(-150)
double b = 1e-150;  // 10^(-150)
double result = a * b;  // 10^(-300) < 2.2 × 10^(-308) → 0.0
System.out.println(result);  // 0.0
```

**Przykład 3: Kumulacja małych wartości**
```java
double sum = 0.0;
for (int i = 0; i < 1000000; i++) {
    sum += 1e-320;  // Każde dodanie = 10^(-320)
    // Po wielu iteracjach: może być 0.0 (underflow)
}
System.out.println(sum);  // Może być 0.0!
```

**Liczby zdenormalizowane (subnormal):**
- Gdy wykładnik = 0, ale mantysa ≠ 0
- **Float:** Najmniejsza zdenormalizowana ≈ 1.4 × 10^(-45)
- **Double:** Najmniejsza zdenormalizowana ≈ 4.9 × 10^(-324)
- **Problem:** Mają bardzo małą precyzję (tylko kilka cyfr znaczących)

**Jak wykryć underflow:**
```java
double result = a * b;
if (result == 0.0 && a != 0.0 && b != 0.0) {
    // Prawdopodobnie underflow
    System.out.println("Underflow!");
}
```

**Jak unikać underflow:**
1. **Sprawdzaj przed operacjami:**
```java
if (a < Double.MIN_NORMAL / b) {
    // Underflow! Obsłuż błąd
} else {
    double result = a * b;
}
```

2. **Użyj logarytmów dla bardzo małych liczb:**
```java
// Zamiast: a * b (może underflow)
// Użyj: exp(log(a) + log(b))
double result = Math.exp(Math.log(a) + Math.log(b));
```

3. **Skaluj wartości:**
```java
// Zamiast: bardzo małe wartości
// Użyj: większe wartości z odpowiednim skalowaniem
double scale = 1e100;
double a_scaled = a * scale;
double b_scaled = b * scale;
double result_scaled = a_scaled * b_scaled;
double result = result_scaled / (scale * scale);
```

**Wartości graniczne:**
- **Float:** MIN_NORMAL ≈ 1.175494 × 10^(-38)
- **Double:** MIN_NORMAL ≈ 2.2250738585072014 × 10^(-308)

### 5. Operacje na NaN i Infinity — szczegółowo

**NaN (Not a Number)** = wynik nieprawidłowej operacji matematycznej.

**Infinity (Nieskończoność)** = wynik operacji, która daje nieskończoność.

**Reguły operacji — szczegółowo:**

**1. NaN z dowolną wartością:**
```java
double nan = Double.NaN;
double x = 5.0;

nan + x;    // NaN
nan - x;    // NaN
nan * x;    // NaN
nan / x;    // NaN
x / nan;    // NaN
nan == nan; // false! (ważne!)
```

**Sprawdzanie NaN:**
```java
double result = Math.sqrt(-1);  // NaN
if (Double.isNaN(result)) {
    System.out.println("To jest NaN!");
}

// NIE używaj: result == Double.NaN (zawsze false!)
```

**2. Infinity z liczbą skończoną:**
```java
double inf = Double.POSITIVE_INFINITY;
double x = 1000.0;

inf + x;    // +∞
inf - x;    // +∞
inf * x;    // +∞ (jeśli x > 0)
inf / x;    // +∞
x / inf;    // 0.0
```

**3. Infinity - Infinity:**
```java
double inf1 = Double.POSITIVE_INFINITY;
double inf2 = Double.POSITIVE_INFINITY;
double result = inf1 - inf2;  // NaN (nieokreślone!)
```

**4. 0 * Infinity:**
```java
double zero = 0.0;
double inf = Double.POSITIVE_INFINITY;
double result = zero * inf;  // NaN (nieokreślone!)
```

**5. 0 / 0:**
```java
double zero = 0.0;
double result = zero / zero;  // NaN
```

**6. x / 0:**
```java
double x = 5.0;
double result = x / 0.0;  // +∞

double y = -5.0;
double result2 = y / 0.0;  // -∞
```

**Przykłady praktyczne:**

**Przykład 1: Obliczanie średniej z tablicy**
```java
double[] array = {1.0, 2.0, 3.0};
double sum = 0.0;
for (double value : array) {
    sum += value;
}
double average = sum / array.length;  // OK: 6.0 / 3 = 2.0

// Ale co jeśli array.length == 0?
double[] empty = {};
double avg = sum / empty.length;  // x / 0 → +∞ lub -∞
// Lepsze:
if (array.length == 0) {
    return Double.NaN;  // Lub obsłuż błąd
}
```

**Przykład 2: Obliczanie pierwiastka**
```java
double x = -5.0;
double result = Math.sqrt(x);  // sqrt(-5) → NaN
if (Double.isNaN(result)) {
    System.out.println("Nie można obliczyć pierwiastka z liczby ujemnej!");
}
```

**Przykład 3: Sprawdzanie poprawności wyniku**
```java
double calculate(double a, double b) {
    if (b == 0.0) {
        return Double.NaN;  // Dzielenie przez zero
    }
    double result = a / b;
    
    // Sprawdź czy wynik jest poprawny
    if (Double.isNaN(result) || Double.isInfinite(result)) {
        return Double.NaN;  // Niepoprawny wynik
    }
    
    return result;
}
```

**Jak obsługiwać NaN i Infinity:**
1. **Zawsze sprawdzaj wyniki:**
```java
double result = performCalculation();
if (Double.isNaN(result)) {
    // Obsłuż błąd
} else if (Double.isInfinite(result)) {
    // Obsłuż overflow/underflow
} else {
    // Użyj wyniku
}
```

2. **Używaj wartości domyślnych:**
```java
double result = calculate();
if (Double.isNaN(result)) {
    result = 0.0;  // Wartość domyślna
}
```

3. **Rzucaj wyjątki:**
```java
double result = calculate();
if (Double.isNaN(result)) {
    throw new ArithmeticException("Niepoprawny wynik!");
}
```

### 6. Asocjatywność (Brak asocjatywności)

Mnożenie i dodawanie **NIE SĄ** asocjatywne w arytmetyce zmiennoprzecinkowej!

(a + b) + c ≠ a + (b + c)

**Przykład:**
```java
float a = 1e20f;
float b = -1e20f;
float c = 1.0f;

(a + b) + c = 0.0 + 1.0 = 1.0
a + (b + c) = 1e20 + (-1e20) = 0.0  // Błąd!
```

---

## 7) Zaokrąglanie (Rounding)

### Tryby zaokrąglania (IEEE 754):

1. **Round to nearest, ties to even** (RNTE) — domyślny
   - Zaokrąglaj do najbliższej wartości
   - W przypadku remisu (równej odległości) → parzysta ostatnia cyfra

2. **Round toward zero** (RZ) — obcięcie (truncation)

3. **Round toward +∞** (RU) — w górę

4. **Round toward -∞** (RD) — w dół

### Wzór na zaokrąglenie (Round to nearest):
round(x) = floor(x + 0.5) jeśli x ≥ 0
round(x) = ceil(x - 0.5) jeśli x < 0

---

## 8) Porównanie stało- vs zmiennoprzecinkowa — szczegółowa analiza

### Tabela porównawcza:

| Aspekt | Stałoprzecinkowa | Zmiennoprzecinkowa |
|--------|------------------|-------------------|
| **Precyzja** | Stała, ograniczona | Zmienna, zależna od wartości |
| **Zakres** | Ograniczony | Bardzo szeroki |
| **Szybkość** | Szybka (operacje całkowite) | Wolniejsza (złożone operacje) |
| **Determinizm** | Pełny | Zależny od implementacji |
| **Złożoność** | Średnia (trzeba zarządzać formatem) | Niska (transparentna) |
| **Zastosowania** | Embedded, DSP, finanse | Nauka, inżynieria, ogólne obliczenia |
| **Błędy** | Przewidywalne | Mogą się kumulować |

### Szczegółowe porównanie z przykładami:

#### 1. Precyzja

**Stałoprzecinkowa:**
- **Format Q3.5:** Zawsze precyzja 1/32 = 0.03125
- **Przykład:** 3.5, 3.53125, 3.5625 (kroki co 0.03125)
- **Zaleta:** Wiesz dokładnie, jaka jest precyzja
- **Wada:** Nie możesz mieć większej precyzji dla małych liczb

**Zmiennoprzecinkowa:**
- **Float:** ~7 cyfr znaczących (zależnie od wartości)
- **Przykład:** 
  - 1.0 ma precyzję ~0.0000001
  - 1000000.0 ma precyzję ~1.0 (mniejsza precyzja!)
- **Zaleta:** Wysoka precyzja dla małych liczb
- **Wada:** Precyzja zależy od wartości (mniejsza dla dużych liczb)

**Przykład praktyczny:**
```java
// Stałoprzecinkowa (Q3.5)
int a = 112;  // 3.5 (precyzja zawsze 0.03125)
int b = 113;  // 3.53125 (następna możliwa wartość)

// Zmiennoprzecinkowa (float)
float x = 3.5f;      // Precyzja ~0.0000001
float y = 3.5000001f; // Możliwa następna wartość
float z = 1000000.0f; // Precyzja ~1.0 (gorsza!)
```

#### 2. Zakres

**Stałoprzecinkowa:**
- **Format Q3.5:** Zakres od -8 do ~7.97 (8 bitów)
- **Format Q15.16:** Zakres od -32768 do ~32767.99998 (31 bitów)
- **Problem:** Musisz wybrać format odpowiedni do zakresu

**Zmiennoprzecinkowa:**
- **Float:** Od ~1.4 × 10^(-45) do ~3.4 × 10^38
- **Double:** Od ~4.9 × 10^(-324) do ~1.8 × 10^308
- **Zaleta:** Ogromny zakres w jednym formacie

**Przykład praktyczny:**
```java
// Stałoprzecinkowa (Q3.5) - nie może reprezentować:
int large = 100;  // 3.125 (OK)
int tooLarge = 1000;  // 31.25 - za duże! Overflow!

// Zmiennoprzecinkowa - może reprezentować:
float small = 1e-40f;   // OK
float large = 1e30f;    // OK
float huge = 1e100f;    // +∞ (overflow, ale bardzo duże!)
```

#### 3. Szybkość

**Stałoprzecinkowa:**
- Operacje na liczbach całkowitych (szybkie!)
- **Przykład:** 80 + 40 = 120 (jedna operacja całkowita)
- **Czas:** ~1 cykl procesora

**Zmiennoprzecinkowa:**
- Wymaga:
  - Wyrównania wykładników (dodawanie/odejmowanie)
  - Normalizacji wyników
  - Zaokrąglania
- **Przykład:** 3.5 + 2.5 wymaga wielu operacji
- **Czas:** ~5-10 cykli procesora (lub więcej)

**Pomiary praktyczne (przybliżone):**
- **Dodawanie:** Stałoprzecinkowa ~2x szybsza
- **Mnożenie:** Stałoprzecinkowa ~3x szybsza
- **Dzielenie:** Podobna szybkość (oba wolne)

#### 4. Determinizm

**Stałoprzecinkowa:**
- **Zawsze ten sam wynik** dla tych samych danych
- **Przykład:** 80 + 40 zawsze = 120
- **Zaleta:** Przewidywalne, łatwe do testowania

**Zmiennoprzecinkowa:**
- **Może różnić się między systemami** (różne implementacje)
- **Przykład:** 0.1 + 0.2 może dać różne wyniki na różnych procesorach
- **Wada:** Trudniejsze testowanie, możliwe różnice między platformami

**Przykład problemu:**
```java
// Na procesorze A:
float result = 0.1f + 0.2f;  // 0.30000001

// Na procesorze B (inna implementacja):
float result = 0.1f + 0.2f;  // 0.30000002 (może być różne!)
```

#### 5. Złożoność implementacji

**Stałoprzecinkowa:**
- **Trzeba ręcznie:**
  - Wybierać format (Qm.n)
  - Zarządzać przesunięciami bitowymi
  - Sprawdzać przepełnienia
  - Normalizować różne formaty
- **Przykład:** Mnożenie wymaga przesunięcia bitowego

**Zmiennoprzecinkowa:**
- **Transparentna** — kompilator/procesor robi wszystko automatycznie
- **Przykład:** Po prostu piszesz `a * b`, procesor robi resztę
- **Zaleta:** Łatwiejsze w użyciu

### Praktyczne przykłady wyboru:

**Przykład 1: System wbudowany (mikrokontroler)**

**Wymagania:**
- Ograniczona pamięć (mało RAM)
- Ograniczona moc procesora
- Deterministyczny czas wykonania
- Precyzja wystarczająca (np. temperatura do 0.1°C)

**Wybór: Stałoprzecinkowa (Q7.8)**
```java
// Format Q7.8 (16 bitów) - oszczędność pamięci
int temperature = 250;  // 25.0°C (250 / 10)
int threshold = 300;    // 30.0°C (300 / 10)

if (temperature > threshold) {
    turnOnCooling();
}
```

**Przykład 2: Obliczenia naukowe (fizyka)**

**Wymagania:**
- Wysoka precyzja (wiele cyfr znaczących)
- Szeroki zakres wartości (od bardzo małych do bardzo dużych)
- Kompleksowe obliczenia

**Wybór: Zmiennoprzecinkowa (double)**
```java
// Obliczanie trajektorii rakiety
double position = 1000000.0;      // 1 milion metrów
double velocity = 5000.0;         // 5 km/s
double acceleration = 9.81;       // m/s²
double time = 0.001;              // 1 milisekunda

double newPosition = position + velocity * time + 0.5 * acceleration * time * time;
// Wymaga wysokiej precyzji i szerokiego zakresu
```

**Przykład 3: Aplikacja finansowa**

**Wymagania:**
- Deterministyczna precyzja (pieniądze muszą się zgadzać!)
- Brak błędów zaokrąglenia
- Przewidywalne obliczenia

**Wybór: Stałoprzecinkowa (lub specjalne typy)**
```java
// Format: cena w groszach (dzielimy przez 100)
long priceInGrosze = 1250L;  // 12.50 zł (używamy long dla większego zakresu)

// Operacje:
long price1 = 1250L;  // 12.50 zł
long price2 = 750L;    // 7.50 zł
long total = price1 + price2;  // 2000 groszy = 20.00 zł ✓

// Zawsze dokładne, bez błędów zaokrąglenia!
```

**Przykład 4: Grafika komputerowa (3D)**

**Wymagania:**
- Szybkość (60 FPS = 16ms na klatkę)
- Precyzja wystarczająca dla pikseli
- Dużo obliczeń (tysiące wierzchołków)

**Wybór: Zmiennoprzecinkowa (float)**
```java
// Współrzędne 3D
float x = 1.5f;
float y = 2.3f;
float z = 0.8f;

// Transformacje (macierze)
float[] matrix = new float[16];
// Szybkie obliczenia, precyzja wystarczająca
```

### Kiedy używać którego — decyzja praktyczna:

**Użyj stałoprzecinkowej, gdy:**
- ✅ Potrzebujesz deterministycznej precyzji
- ✅ Masz ograniczone zasoby (pamięć, procesor)
- ✅ Potrzebujesz szybkości
- ✅ Precyzja jest stała i znana z góry
- ✅ Aplikacje finansowe, embedded, DSP

**Użyj zmiennoprzecinkowej, gdy:**
- ✅ Potrzebujesz szerokiego zakresu wartości
- ✅ Precyzja zależy od wartości (małe liczby = wysoka precyzja)
- ✅ Kompleksowe obliczenia naukowe
- ✅ Nie potrzebujesz determinizmu
- ✅ Aplikacje naukowe, inżynierskie, ogólne programowanie

### Hybrydowe podejście:

**Możesz używać obu w jednym systemie!**

**Przykład: System kontroli temperatury**
```java
// Stałoprzecinkowa dla szybkich obliczeń kontrolnych
int temperature = 250;  // 25.0°C (Q7.8)
int setpoint = 300;     // 30.0°C

if (temperature > setpoint) {
    // Zmiennoprzecinkowa dla złożonych obliczeń
    double pidOutput = calculatePID(temperature, setpoint);  // double
    adjustSystem(pidOutput);
}
```

---

## 9) Praktyczne przykłady konwersji i implementacji

### Konwersja między stało- a zmiennoprzecinkową

**Przykład 1: Konwersja zmiennoprzecinkowej na stałoprzecinkową**

**Problem:** Masz wartość float i chcesz ją przechować w formacie stałoprzecinkowym.

**Rozwiązanie:**
```java
// Format Q7.8 (15 bitów całkowitych, 8 bitów ułamkowych)
// Precyzja: 1/256 = 0.00390625

float value = 25.5f;  // Wartość zmiennoprzecinkowa
int fixed = (int)(value * 256);  // 25.5 * 256 = 6528

// Sprawdzenie:
float converted = fixed / 256.0f;  // 6528 / 256 = 25.5 ✓
```

**Uwaga:** Może wystąpić utrata precyzji, jeśli format stałoprzecinkowy ma mniejszą precyzję.

**Przykład 2: Konwersja stałoprzecinkowej na zmiennoprzecinkową**

**Problem:** Masz wartość w formacie stałoprzecinkowym i chcesz ją przekonwertować na float.

**Rozwiązanie:**
```java
// Format Q7.8
int fixed = 6528;  // 25.5 w formacie stałoprzecinkowym
float value = fixed / 256.0f;  // 6528 / 256 = 25.5 ✓
```

**Przykład 3: Konwersja między różnymi formatami stałoprzecinkowymi**

**Problem:** Masz wartość w formacie Q3.5 i chcesz ją przekonwertować na Q7.8.

**Rozwiązanie:**
```java
// Format źródłowy: Q3.5 (n1 = 5)
int valueQ35 = 112;  // 3.5 (112 / 32)

// Konwersja na wartość rzeczywistą:
float realValue = valueQ35 / 32.0f;  // 3.5

// Konwersja na format docelowy: Q7.8 (n2 = 8)
int valueQ78 = (int)(realValue * 256);  // 3.5 * 256 = 896

// Sprawdzenie:
float check = valueQ78 / 256.0f;  // 896 / 256 = 3.5 ✓
```

**Wzór ogólny:**
```
Wartość rzeczywista = N1 / (2^n1)
N2 = Wartość rzeczywista * (2^n2)
N2 = (N1 / (2^n1)) * (2^n2) = N1 * (2^(n2 - n1))
```

### Implementacja klasy dla stałoprzecinkowej (przykład)

**Przykład klasy FixedPoint w Javie:**

```java
public class FixedPoint {
    private final int value;  // Wartość całkowita
    private final int fractionalBits;  // Liczba bitów ułamkowych (n)
    
    // Format: Qm.n, gdzie n = fractionalBits
    public FixedPoint(int value, int fractionalBits) {
        this.value = value;
        this.fractionalBits = fractionalBits;
    }
    
    // Konstruktor z wartości rzeczywistej
    public FixedPoint(double realValue, int fractionalBits) {
        this.fractionalBits = fractionalBits;
        this.value = (int)(realValue * (1 << fractionalBits));
    }
    
    // Konwersja na wartość rzeczywistą
    public double toDouble() {
        return value / (double)(1 << fractionalBits);
    }
    
    // Dodawanie
    public FixedPoint add(FixedPoint other) {
        if (this.fractionalBits != other.fractionalBits) {
            throw new IllegalArgumentException("Różne formaty!");
        }
        return new FixedPoint(this.value + other.value, this.fractionalBits);
    }
    
    // Mnożenie
    public FixedPoint multiply(FixedPoint other) {
        if (this.fractionalBits != other.fractionalBits) {
            throw new IllegalArgumentException("Różne formaty!");
        }
        // Przesunięcie w prawo o fractionalBits bitów
        long result = (long)this.value * (long)other.value;
        int shifted = (int)(result >> fractionalBits);
        return new FixedPoint(shifted, this.fractionalBits);
    }
    
    // Dzielenie
    public FixedPoint divide(FixedPoint other) {
        if (this.fractionalBits != other.fractionalBits) {
            throw new IllegalArgumentException("Różne formaty!");
        }
        if (other.value == 0) {
            throw new ArithmeticException("Dzielenie przez zero!");
        }
        // Przesunięcie w lewo o fractionalBits bitów przed dzieleniem
        long shifted = (long)this.value << fractionalBits;
        int result = (int)(shifted / other.value);
        return new FixedPoint(result, this.fractionalBits);
    }
    
    @Override
    public String toString() {
        return String.format("%.5f", toDouble());
    }
}

// Przykład użycia:
FixedPoint a = new FixedPoint(3.5, 5);  // Q3.5, wartość 3.5
FixedPoint b = new FixedPoint(2.0, 5);  // Q3.5, wartość 2.0
FixedPoint sum = a.add(b);              // 5.5
FixedPoint product = a.multiply(b);      // 7.0
FixedPoint quotient = a.divide(b);      // 1.75
```

### Praktyczne przykłady zastosowań — szczegółowe case studies

**Case Study 1: System kontroli temperatury (embedded)**

**Wymagania:**
- Temperatura: -50°C do 150°C (zakres 200°C)
- Precyzja: 0.1°C
- Szybkie obliczenia (czas rzeczywisty)

**Projekt:**
- Format: Q8.8 (16 bitów)
- Zakres: -128 do 127.996
- Precyzja: 1/256 = 0.00390625°C (wystarczająca!)

**Implementacja:**
```java
// Konwersja temperatury na format stałoprzecinkowy
int temperatureToFixed(double temp) {
    return (int)(temp * 256);  // temp * 256
}

// Konwersja z formatu stałoprzecinkowego
double fixedToTemperature(int fixed) {
    return fixed / 256.0;
}

// Przykład:
int temp1 = temperatureToFixed(25.5);  // 25.5 * 256 = 6528
int temp2 = temperatureToFixed(30.0);  // 30.0 * 256 = 7680

// Porównanie (szybkie, całkowite):
if (temp1 > temp2) {
    System.out.println("temp1 jest wyższa");
}

// Różnica:
int diff = temp2 - temp1;  // 7680 - 6528 = 1152
double diffCelsius = fixedToTemperature(diff);  // 1152 / 256 = 4.5°C ✓
```

**Case Study 2: Przetwarzanie sygnału audio**

**Wymagania:**
- Próbki audio: 16-bit (zakres -32768 do 32767)
- Częstotliwość próbkowania: 44100 Hz
- Filtrowanie w czasie rzeczywistym

**Projekt:**
- Format: Q15.16 (31 bitów)
- Zakres: -32768 do 32767.99998
- Precyzja: 1/65536 (wystarczająca dla audio)

**Implementacja:**
```java
// Próbka audio w formacie stałoprzecinkowym
int sample = 16384;  // 0.25 w zakresie [-1.0, 1.0]

// Filtrowanie (przykład: średnia ruchoma)
int[] buffer = new int[10];
int sum = 0;
for (int i = 0; i < buffer.length; i++) {
    sum += buffer[i];  // Szybkie dodawanie całkowite
}
int filtered = sum / buffer.length;  // Średnia
```

**Case Study 3: System finansowy**

**Wymagania:**
- Kwoty: do 1 000 000 zł
- Precyzja: do grosza (0.01 zł)
- Brak błędów zaokrąglenia

**Projekt:**
- Format: cena w groszach (dzielimy przez 100)
- Używamy long (64 bitów) dla większego zakresu
- Zakres: -9 223 372 036 854 775 808 do 9 223 372 036 854 775 807 groszy

**Implementacja:**
```java
// Przechowywanie ceny w groszach
long priceInGrosze = 1250L;  // 12.50 zł

// Operacje:
long price1 = 1250L;  // 12.50 zł
long price2 = 750L;    // 7.50 zł
long total = price1 + price2;  // 2000 groszy = 20.00 zł ✓

// Mnożenie przez ilość:
int quantity = 3;
long totalPrice = price1 * quantity;  // 1250 * 3 = 3750 groszy = 37.50 zł ✓

// Dzielenie (uwaga na precyzję!):
long discount = 250L;  // 2.50 zł zniżki
long finalPrice = totalPrice - discount;  // 3750 - 250 = 3500 groszy = 35.00 zł ✓

// Konwersja na string do wyświetlenia:
String displayPrice = String.format("%.2f zł", totalPrice / 100.0);
// "20.00 zł"
```

**Case Study 4: Obliczenia naukowe (fizyka)**

**Wymagania:**
- Wysoka precyzja (wiele cyfr znaczących)
- Szeroki zakres (od bardzo małych do bardzo dużych)
- Kompleksowe obliczenia

**Projekt:**
- Format: double (64-bit zmiennoprzecinkowa)
- Precyzja: ~15-17 cyfr dziesiętnych
- Zakres: ±1.8 × 10^308

**Implementacja:**
```java
// Obliczanie trajektorii
double position = 1000000.0;      // 1 milion metrów
double velocity = 5000.0;         // 5 km/s
double acceleration = 9.81;      // m/s²
double time = 0.001;              // 1 milisekunda

// Równanie: s = s0 + v*t + 0.5*a*t²
double newPosition = position + velocity * time + 0.5 * acceleration * time * time;
// Wymaga wysokiej precyzji i szerokiego zakresu
```

### Najlepsze praktyki — podsumowanie

**Dla stałoprzecinkowej:**
1. ✅ Wybierz format odpowiedni do zakresu i precyzji
2. ✅ Używaj tego samego formatu w całym systemie (jeśli możliwe)
3. ✅ Sprawdzaj przepełnienia przed operacjami
4. ✅ Używaj większych typów (long) dla pośrednich obliczeń
5. ✅ Dokumentuj format (Qm.n) w kodzie

**Dla zmiennoprzecinkowej:**
1. ✅ Używaj double zamiast float, gdy potrzebna większa precyzja
2. ✅ NIGDY nie porównuj przez == (używaj epsilon)
3. ✅ Unikaj odejmowania podobnych liczb (catastrophic cancellation)
4. ✅ Sprawdzaj NaN i Infinity po operacjach
5. ✅ Uważaj na overflow/underflow

**Ogólne:**
1. ✅ Wybierz odpowiedni typ do zastosowania
2. ✅ Testuj na rzeczywistych danych
3. ✅ Dokumentuj założenia o precyzji
4. ✅ Obsługuj błędy (NaN, Infinity, overflow)

---

## 10) Wzory podsumowujące

### Stałoprzecinkowa (Qm.n):
V = N / (2^n)

Dodawanie: V1 + V2 = (N1 + N2) / (2^n)

Mnożenie: V1 * V2 = (N1 * N2) / (2^(2n)) → shift right n bitów

### Zmiennoprzecinkowa (IEEE 754):
V = (-1)^s * (1 + M) * 2^(E - bias)

**Float:**
- Bias = 127
- Precyzja: ~7 cyfr dziesiętnych
- Zakres: ≈ 10^(±38)

**Double:**
- Bias = 1023
- Precyzja: ~15-17 cyfr dziesiętnych
- Zakres: ≈ 10^(±308)

### Machine Epsilon:
ε = 2^(-p)

Gdzie p to liczba bitów mantysy (bez ukrytej jedynki).

### Błąd względny:
ε_rel = |x_rzeczywista - x_reprezentowana| / |x_rzeczywista|

### Porównywanie liczb:
|a - b| < ε * max(|a|, |b|)

---

## 10) Praktyczne wskazówki

### Kiedy używać stałoprzecinkowej?
- Systemy wbudowane z ograniczonymi zasobami
- Aplikacje wymagające deterministycznej precyzji
- Przetwarzanie sygnałów (DSP)
- Niektóre aplikacje finansowe

### Kiedy używać zmiennoprzecinkowej?
- Większość aplikacji naukowych i inżynierskich
- Gdy potrzebny szeroki zakres wartości
- Gdy precyzja zależy od wartości
- Ogólne programowanie

### Najlepsze praktyki:
1. **NIGDY nie porównuj** liczb zmiennoprzecinkowych przez `==`
2. Używaj **epsilon** do porównań
3. Unikaj **odejmowania podobnych liczb** (catastrophic cancellation)
4. Sortuj przed sumowaniem (dla lepszej precyzji)
5. Używaj **double** zamiast **float** gdy potrzebna większa precyzja
6. Sprawdzaj **NaN** i **Infinity** po operacjach
7. Uważaj na **overflow/underflow**

---

## 11) Kluczowe zdania na obronę

1. **Stałoprzecinkowa** = pozycja przecinka jest ustalona, reprezentacja jako liczba całkowita podzielona przez stałą.

2. **Zmiennoprzecinkowa** = pozycja przecinka jest zmienna, reprezentacja w formie naukowej (mantysa × 2^wykładnik).

3. **IEEE 754** to standard reprezentacji liczb zmiennoprzecinkowych (float = 32-bit, double = 64-bit).

4. **Precyzja float** ≈ 7 cyfr dziesiętnych, **double** ≈ 15-17 cyfr dziesiętnych.

5. **Machine epsilon** to najmniejsza liczba taka, że 1 + ε > 1.

6. **Błędy zaokrąglenia** kumulują się przy wielu operacjach — dlatego nie porównujemy przez `==`.

7. **Catastrophic cancellation** — utrata precyzji przy odejmowaniu podobnych liczb.

8. **Zmiennoprzecinkowa** nie jest asocjatywna — kolejność operacji ma znaczenie.

9. **Stałoprzecinkowa** = szybka, deterministyczna, ale ograniczony zakres.

10. **Zmiennoprzecinkowa** = szeroki zakres, ale błędy mogą się kumulować.

