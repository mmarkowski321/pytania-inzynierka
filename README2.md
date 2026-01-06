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

### Zalety stałoprzecinkowej:
- ✅ **Deterministyczna** — zawsze ta sama precyzja
- ✅ **Szybka** — operacje na liczbach całkowitych
- ✅ **Przewidywalna** — brak niespodzianek z precyzją
- ✅ **Dobra dla aplikacji czasu rzeczywistego**

### Wady stałoprzecinkowej:
- ❌ **Ograniczony zakres** — zależy od liczby bitów
- ❌ **Stała precyzja** — nie można zmienić dokładności
- ❌ **Trudność w implementacji** — trzeba ręcznie zarządzać formatem

### Zastosowania:
- Systemy wbudowane (embedded systems)
- Przetwarzanie sygnałów cyfrowych (DSP)
- Aplikacje finansowe (gdzie potrzebna jest deterministyczna precyzja)
- Grafika komputerowa (niektóre algorytmy)

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

### Zakresy i precyzja

#### Float (32-bit):
- Najmniejsza wartość dodatnia (zdenormalizowana): ≈ 1.4 × 10^(-45)
- Najmniejsza wartość znormalizowana: ≈ 1.2 × 10^(-38)
- Największa wartość: ≈ 3.4 × 10^38
- Precyzja: ~7 cyfr dziesiętnych (23 bity mantysy ≈ 2^23 ≈ 8.4 × 10^6)

#### Double (64-bit):
- Najmniejsza wartość dodatnia (zdenormalizowana): ≈ 4.9 × 10^(-324)
- Najmniejsza wartość znormalizowana: ≈ 2.2 × 10^(-308)
- Największa wartość: ≈ 1.8 × 10^308
- Precyzja: ~15-17 cyfr dziesiętnych (52 bity mantysy ≈ 2^52 ≈ 4.5 × 10^15)

### Wzór na liczbę możliwych wartości:
Dla mantysy o p bitach (bez ukrytej jedynki):
liczby możliwe = 2^(p+1)

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

### 1. Porównywanie liczb zmiennoprzecinkowych

**ZŁE:**
```java
if (a == b) { }  // Rzadko działa poprawnie!
```

**DOBRE:**
```java
if (Math.abs(a - b) < epsilon) { }  // epsilon = mała wartość (np. 1e-9)
```

**Wzór:**
|a - b| < ε * max(|a|, |b|)

### 2. Utrata precyzji przy odejmowaniu (Catastrophic Cancellation)

Gdy odejmujemy dwie bardzo podobne liczby:

a = 1.234567
b = 1.234566
a - b = 0.000001

Wynik ma znacznie mniejszą precyzję!

**Przykład problematyczny:**
pierwiastek(x + 1) - pierwiastek(x)

Lepsze podejście:
1 / (pierwiastek(x + 1) + pierwiastek(x))

### 3. Przepełnienie (Overflow)

Gdy wynik jest zbyt duży dla reprezentacji:
- Float: > 3.4 × 10^38 → +∞
- Double: > 1.8 × 10^308 → +∞

### 4. Niedomiar (Underflow)

Gdy wynik jest zbyt mały:
- Float: < 1.2 × 10^(-38) → może stać się 0 lub liczbą zdenormalizowaną
- Double: < 2.2 × 10^(-308) → może stać się 0 lub liczbą zdenormalizowaną

### 5. Operacje na NaN i Infinity

**Reguły:**
- NaN ∘ x = NaN (gdzie ∘ to dowolna operacja)
- ∞ + x = ∞ (gdy x ≠ ∞)
- ∞ - ∞ = NaN
- 0 * ∞ = NaN
- 0 / 0 = NaN
- x / 0 = ±∞ (w zależności od znaku x)

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

## 8) Porównanie stało- vs zmiennoprzecinkowa

| Aspekt | Stałoprzecinkowa | Zmiennoprzecinkowa |
|--------|------------------|-------------------|
| **Precyzja** | Stała, ograniczona | Zmienna, zależna od wartości |
| **Zakres** | Ograniczony | Bardzo szeroki |
| **Szybkość** | Szybka (operacje całkowite) | Wolniejsza (złożone operacje) |
| **Determinizm** | Pełny | Zależny od implementacji |
| **Złożoność** | Średnia (trzeba zarządzać formatem) | Niska (transparentna) |
| **Zastosowania** | Embedded, DSP, finanse | Nauka, inżynieria, ogólne obliczenia |
| **Błędy** | Przewidywalne | Mogą się kumulować |

---

## 9) Wzory podsumowujące

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

