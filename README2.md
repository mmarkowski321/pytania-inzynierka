# Arytmetyka Stało- i Zmiennoprzecinkowa — Kompletna Notatka do Obrony

## 1) Wprowadzenie — reprezentacja liczb w komputerze

### Podstawowy problem
Komputer przechowuje liczby w formie **binarnej** (0 i 1). Dla liczb całkowitych to proste, ale dla liczb rzeczywistych (z częścią ułamkową) potrzebujemy specjalnych reprezentacji.

### Dwie główne metody:
1. **Stałoprzecinkowa** (Fixed-Point) — pozycja przecinka jest ustalona
2. **Zmiennoprzecinkowa** (Floating-Point) — pozycja przecinka jest zmienna

---

## 2) Reprezentacja liczb całkowitych (podstawa)

### System binarny
Liczba w systemie binarnym:
N = suma od i=0 do n-1 z (b_i * 2^i)

Gdzie:
- b_i — cyfra binarna (0 lub 1)
- n — liczba bitów

### Przykłady:
- 1011 (binarnie) = 1 * 2^3 + 0 * 2^2 + 1 * 2^1 + 1 * 2^0 = 8 + 0 + 2 + 1 = 11 (dziesiętnie)
- 1100 (binarnie) = 1 * 2^3 + 1 * 2^2 + 0 * 2^1 + 0 * 2^0 = 8 + 4 + 0 + 0 = 12 (dziesiętnie)

### Zakresy (n-bitowe liczby całkowite):
- **Bez znaku** (unsigned): 0 do (2^n - 1)
- **Ze znakiem** (signed, two's complement): -(2^(n-1)) do (2^(n-1) - 1)

### Przykłady zakresów:
- 8 bitów: -128 do 127 (ze znakiem) lub 0 do 255 (bez znaku)
- 16 bitów: -32 768 do 32 767 (ze znakiem) lub 0 do 65 535 (bez znaku)
- 32 bity: -2 147 483 648 do 2 147 483 647 (ze znakiem) lub 0 do 4 294 967 295 (bez znaku)

---

## 3) Arytmetyka stałoprzecinkowa (Fixed-Point Arithmetic)

### Definicja
Liczba jest reprezentowana jako **liczba całkowita**, ale interpretowana jako wartość podzielona przez pewną stałą (zwykle potęgę 2).

### Reprezentacja
Liczba jest przechowywana jako: N = wartość całkowita
Rzeczywista wartość: V = N / (2^f)

Gdzie:
- f — liczba bitów przeznaczona na część ułamkową (fractional part)
- N — wartość całkowita w pamięci

### Format Qm.n (lub Qf)
- Całkowita liczba bitów: m + n
- m bitów na część całkowitą (integer part)
- n bitów na część ułamkową (fractional part)
- Czasem zapisywane jako Qn (gdzie n to część ułamkowa)

### Wzór na wartość rzeczywistą:
V = N / (2^n)

Gdzie N to wartość całkowita.

### Przykład Q3.5 (8 bitów, 3 bity część całkowita, 5 bitów część ułamkowa)

Wartość binarna: 01101011 (binarnie) = 107 (dziesiętnie)

Rozkład:
- Bit znaku/znaczenia: 0
- Bity części całkowitej (3 bity): 011 = 3
- Bity części ułamkowej (5 bitów): 01011 = 11

Wartość rzeczywista:
V = 3 + (11 / 2^5) = 3 + (11 / 32) = 3 + 0.34375 = 3.34375

### Operacje arytmetyczne w stałoprzecinkowej

#### Dodawanie/Odejmowanie
Po prostu dodajemy/odejmujemy wartości całkowite:
V1 ± V2 = (N1 ± N2) / (2^n)

**Uwaga**: Oba operandy muszą mieć ten sam format (te same n)!

#### Mnożenie
V1 * V2 = (N1 * N2) / (2^(2n)) = (N1 * N2) / (2^n) * (1 / 2^n)

Po mnożeniu wynik ma podwójną precyzję części ułamkowej. Trzeba **przesunąć w prawo** o n bitów:
wynik = (N1 * N2) / (2^n)

#### Dzielenie
V1 / V2 = (N1 / N2) * (2^n / 2^n) = (N1 * 2^n) / N2

Trzeba **przesunąć w lewo** przed dzieleniem (lub pomnożyć przez 2^n).

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

### Definicja
Liczba jest reprezentowana w formie **naukowej** (notacji wykładniczej):
V = (-1)^s * m * b^e

Gdzie:
- s — bit znaku (sign)
- m — mantysa (mantissa/significand)
- b — podstawa (base), zwykle 2
- e — wykładnik (exponent)

### Standard IEEE 754

IEEE 754 to międzynarodowy standard reprezentacji liczb zmiennoprzecinkowych.

### Format IEEE 754 (32-bit, single precision, float)

```
| S | EEEEEEEE | MMMMMMMMMMMMMMMMMMMMMMM |
| 1 |    8     |          23             |
```

- **S** (1 bit) — znak: 0 = dodatnia, 1 = ujemna
- **E** (8 bitów) — wykładnik (bias = 127)
- **M** (23 bity) — mantysa (część ułamkowa)

### Wartość liczby (normalizowana):
V = (-1)^s * (1 + M) * 2^(E - 127)

Gdzie:
- M = M_bin / (2^23) — mantysa jako ułamek
- E — wykładnik (0-255, z bias 127)

### Wyjątkowe wartości:

#### Zero
- E = 0, M = 0
- Może być +0 i -0

#### Nieskończoność (Infinity)
- E = 255 (wszystkie jedynki), M = 0
- +∞ lub -∞ w zależności od znaku

#### NaN (Not a Number)
- E = 255, M ≠ 0
- Wynik nieprawidłowych operacji (np. sqrt(-1), 0/0)

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

### Przykład konwersji: Float → Wartość dziesiętna

Niech: 01000010011001100000000000000000 (binarnie)

1. **Znak**: S = 0 → liczba dodatnia
2. **Wykładnik**: E = 10000100 (binarnie) = 132 (dziesiętnie)
3. **Mantysa**: M = 11001100000000000000000 (binarnie)

Wartość:
E_dec = 132 - 127 = 5
M_dec = 3342336 / (2^23) = 3342336 / 8388608 ≈ 0.3984375
V = (1 + 0.3984375) * 2^5 = 1.3984375 * 32 = 44.75

### Operacje arytmetyczne w zmiennoprzecinkowej

#### Dodawanie/Odejmowanie

**Algorytm:**
1. Wyrównaj wykładniki (dopasuj do większego)
2. Dodaj/odejmij mantysy
3. Znormalizuj wynik
4. Zaokrąglij

**Wzór (uproszczony):**
V1 ± V2 = (m1 * 2^e1) ± (m2 * 2^e2)

Jeśli e1 > e2:
= (m1 ± m2 * 2^(e2 - e1)) * 2^e1

#### Mnożenie

**Algorytm:**
1. Dodaj wykładniki: e_wynik = e1 + e2 - bias
2. Pomnóż mantysy: m_wynik = m1 * m2
3. Znormalizuj wynik
4. Zaokrąglij

**Wzór:**
V1 * V2 = (m1 * m2) * 2^(e1 + e2)

#### Dzielenie

**Algorytm:**
1. Odejmij wykładniki: e_wynik = e1 - e2 + bias
2. Podziel mantysy: m_wynik = m1 / m2
3. Znormalizuj wynik
4. Zaokrąglij

**Wzór:**
V1 / V2 = (m1 / m2) * 2^(e1 - e2)

---

## 5) Błędy i precyzja

### Błąd względny (Relative Error)
ε_rel = |x_rzeczywista - x_reprezentowana| / |x_rzeczywista|

### Błąd bezwzględny (Absolute Error)
ε_abs = |x_rzeczywista - x_reprezentowana|

### Machine Epsilon (ε)

**Definicja:** najmniejsza liczba taka, że 1 + ε > 1

Dla formatu z p bitami mantysy:
ε = 2^(-p)

**Wartości:**
- Float (23 bity): ε ≈ 1.19 × 10^(-7)
- Double (52 bity): ε ≈ 2.22 × 10^(-16)

### Błąd zaokrąglenia (Rounding Error)

Liczby dziesiętne często nie mają dokładnej reprezentacji binarnej.

**Przykład:**
0.1 (dziesiętnie) = 0.000110011001100110011... (binarnie, nieskończone)

W float: 0.1 ≈ 0.100000001490116119384765625

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
sqrt(x + 1) - sqrt(x)

Lepsze podejście:
1 / (sqrt(x + 1) + sqrt(x))

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

