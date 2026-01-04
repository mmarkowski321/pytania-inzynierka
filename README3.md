# Normalizacja Schematu Bazy Danych — Kompletna Notatka do Obrony

## 1) Wprowadzenie — co to jest normalizacja?

### Definicja
**Normalizacja** to proces organizacji danych w bazie danych w taki sposób, aby:
- **Eliminować redundancję** (duplikowanie danych)
- **Minimalizować anomalie** (błędy podczas wstawiania, aktualizacji, usuwania)
- **Zapewnić spójność danych**
- **Uprościć strukturę** i ułatwić utrzymanie

### Główna idea
Każda informacja powinna być przechowywana **tylko raz** w bazie danych. Dzięki temu:
- Zmiana w jednym miejscu automatycznie aktualizuje cały system
- Zmniejsza się ryzyko niespójności danych
- Oszczędza się miejsce

### Cel normalizacji
> **Przechowywać każdy fakt tylko raz i w odpowiednim miejscu.**

---

## 2) Anomalie bazodanowe — problemy przed normalizacją

### A) Anomalia wstawiania (Insertion Anomaly)

**Problem:** Nie można wstawić danych bez innych związanych danych.

**Przykład:**
Tabela `Zamowienia`:
| ID_Zamowienia | Data | ID_Klienta | Nazwa_Klienta | Miasto_Klienta | Produkt | Cena |
|--------------|------|------------|---------------|----------------|---------|------|
| 1 | 2024-01-15 | K1 | Jan Kowalski | Warszawa | Laptop | 5000 |
| 2 | 2024-01-16 | K1 | Jan Kowalski | Warszawa | Mysz | 50 |
| 3 | 2024-01-17 | K2 | Anna Nowak | Kraków | Klawiatura | 200 |

**Problem:** Nie można dodać nowego klienta bez zamówienia!

### B) Anomalia aktualizacji (Update Anomaly)

**Problem:** Musisz zaktualizować dane w wielu miejscach.

**Przykład:**
Klient Jan Kowalski zmienił adres na "Kraków". Musisz zaktualizować **wszystkie** wiersze z tym klientem (2 wiersze w przykładzie). Jeśli zapomnisz jednego, dane będą niespójne.

### C) Anomalia usuwania (Deletion Anomaly)

**Problem:** Usunięcie danych powoduje utratę innych ważnych danych.

**Przykład:**
Jeśli usuniesz ostatnie zamówienie klienta (np. zamówienie 3), stracisz informację o kliencie "Anna Nowak"!

---

## 3) Zależności funkcyjne (Functional Dependencies)

### Definicja
**Zależność funkcyjna** (FD): \( A \to B \) oznacza, że dla każdej wartości atrybutu \( A \) istnieje dokładnie jedna wartość atrybutu \( B \).

### Notacja
\[
A \to B
\]
- \( A \) — determinant (atrybut determinujący)
- \( B \) — dependent (atrybut zależny)

Czytamy: "A określa B" lub "B zależy funkcyjnie od A".

### Przykład
W tabeli `Student`:
- `ID_Studenta` → `Imie` (ID studenta określa imię)
- `ID_Studenta` → `Nazwisko`
- `ID_Studenta` → `Email`

Każdy student ma jedno imię, jedno nazwisko, jeden email.

### Zależności pełne i częściowe

#### Częściowa zależność (Partial Dependency)
\( B \) zależy częściowo od \( A \), jeśli:
- \( A \to B \)
- Istnieje podzbiór \( A' \subset A \) taki, że \( A' \to B \)

**Przykład:**
\( \{ID_Studenta, Przedmiot\} \to Ocena \)

Ale: \( ID_Studenta \not\to Ocena \) (bo student może mieć różne oceny)

#### Pełna zależność (Full Dependency)
\( B \) zależy w pełni od \( A \), jeśli:
- \( A \to B \)
- \( B \) NIE zależy od żadnego właściwego podzbioru \( A \)

### Zależność przechodnia (Transitive Dependency)

\( A \to B \to C \) (przechodnia), jeśli:
- \( A \to B \)
- \( B \to C \)
- \( A \not\to C \) (bezpośrednio)

**Przykład:**
- `ID_Pracownika` → `ID_Dzialu`
- `ID_Dzialu` → `Nazwa_Dzialu`
- Więc: `ID_Pracownika` → `Nazwa_Dzialu` (przechodnia)

---

## 4) Klucze w bazie danych

### Klucz kandydujący (Candidate Key)
Zbiór atrybutów, który:
- **Jednoznacznie identyfikuje** każdy wiersz
- **Minimalny** (nie można usunąć żadnego atrybutu bez utraty unikalności)

### Klucz główny (Primary Key)
Jeden wybrany klucz kandydujący (zazwyczaj najprostszy).

### Klucz obcy (Foreign Key)
Atrybut w jednej tabeli, który odnosi się do klucza głównego innej tabeli.

### Klucz złożony (Composite Key)
Klucz składający się z więcej niż jednego atrybutu.

### Atrybut niekluczowy (Non-key Attribute)
Atrybut, który nie jest częścią żadnego klucza.

---

## 5) Normalne formy (Normal Forms)

### Hierarchia normalnych form:
```
UNNORMALIZED (0NF)
    ↓
FIRST NORMAL FORM (1NF)
    ↓
SECOND NORMAL FORM (2NF)
    ↓
THIRD NORMAL FORM (3NF)
    ↓
BOYCE-CODD NORMAL FORM (BCNF)
    ↓
FOURTH NORMAL FORM (4NF)
    ↓
FIFTH NORMAL FORM (5NF)
```

**Praktycznie używa się głównie 1NF, 2NF, 3NF i BCNF.**

---

## 6) Pierwsza forma normalna (1NF)

### Definicja
Tabela jest w **1NF**, jeśli:
1. ✅ **Każda kolumna zawiera wartości atomowe** (niepodzielne)
2. ✅ **Nie ma powtarzających się grup** (repeating groups)
3. ✅ **Każdy wiersz jest unikalny**

### Przykład naruszenia 1NF:

**PRZED (złe):**
| ID_Studenta | Imie | Przedmioty |
|------------|------|------------|
| 1 | Jan | Matematyka, Fizyka, Chemia |
| 2 | Anna | Matematyka, Informatyka |

**Problem:**
- Kolumna `Przedmioty` zawiera **wartości złożone** (wiele wartości)
- Nie można łatwo wyszukiwać po przedmiotach
- Trudno dodawać/usuwać przedmioty

**PO (1NF):**
| ID_Studenta | Imie | Przedmiot |
|------------|------|-----------|
| 1 | Jan | Matematyka |
| 1 | Jan | Fizyka |
| 1 | Jan | Chemia |
| 2 | Anna | Matematyka |
| 2 | Anna | Informatyka |

Lub lepiej (z osobną tabelą):

**Tabela Studenci:**
| ID_Studenta | Imie |
|------------|------|
| 1 | Jan |
| 2 | Anna |

**Tabela Studenci_Przedmioty:**
| ID_Studenta | Przedmiot |
|------------|-----------|
| 1 | Matematyka |
| 1 | Fizyka |
| 1 | Chemia |
| 2 | Matematyka |
| 2 | Informatyka |

### Inne przykłady naruszenia 1NF:

#### Powtarzające się kolumny (repeating columns):
| ID | Imie | Telefon1 | Telefon2 | Telefon3 |
|---|---|---|---|---|---|
| 1 | Jan | 123-456 | 789-012 | NULL |

**Lepsze rozwiązanie:**
Osobna tabela `Telefony` z relacją jeden-do-wielu.

#### Tablice/wielowartościowe atrybuty:
| ID | Imie | Oceny[] |
|---|---|---------|
| 1 | Jan | [5, 4, 5] |

**Lepsze rozwiązanie:**
Osobna tabela `Oceny` z relacją.

---

## 7) Druga forma normalna (2NF)

### Definicja
Tabela jest w **2NF**, jeśli:
1. ✅ Jest w **1NF**
2. ✅ **Każdy atrybut niekluczowy zależy w pełni od klucza głównego** (nie ma częściowych zależności)

### Warunek formalny
Dla każdej zależności funkcyjnej \( A \to B \), gdzie \( B \) jest atrybutem niekluczowym:
- \( A \) musi być **nadkluczem** (superkey) — zawiera klucz
- Lub \( B \) zależy od **całego** klucza, nie od jego części

### Przykład naruszenia 2NF:

**PRZED (złe):**
| ID_Zamowienia | ID_Produktu | Nazwa_Produktu | Cena_Produktu | Ilosc | Data_Zamowienia |
|--------------|-------------|----------------|---------------|-------|----------------|
| 1 | P1 | Laptop | 5000 | 2 | 2024-01-15 |
| 1 | P2 | Mysz | 50 | 3 | 2024-01-15 |
| 2 | P1 | Laptop | 5000 | 1 | 2024-01-16 |

**Klucz główny:** `{ID_Zamowienia, ID_Produktu}`

**Zależności:**
- `{ID_Zamowienia, ID_Produktu}` → `Ilosc` ✅ (pełna)
- `{ID_Zamowienia, ID_Produktu}` → `Nazwa_Produktu` ❌ (częściowa — `ID_Produktu` → `Nazwa_Produktu`)
- `{ID_Zamowienia, ID_Produktu}` → `Cena_Produktu` ❌ (częściowa — `ID_Produktu` → `Cena_Produktu`)
- `{ID_Zamowienia, ID_Produktu}` → `Data_Zamowienia` ❌ (częściowa — `ID_Zamowienia` → `Data_Zamowienia`)

**Problem:**
- `Nazwa_Produktu` i `Cena_Produktu` zależą tylko od `ID_Produktu`, nie od całego klucza
- Duplikacja danych (Laptop, 5000 powtarza się)
- Anomalie aktualizacji (zmiana ceny wymaga aktualizacji wielu wierszy)

**PO (2NF):**

**Tabela Zamowienia:**
| ID_Zamowienia | Data_Zamowienia |
|--------------|----------------|
| 1 | 2024-01-15 |
| 2 | 2024-01-16 |

**Tabela Produkty:**
| ID_Produktu | Nazwa_Produktu | Cena_Produktu |
|------------|----------------|---------------|
| P1 | Laptop | 5000 |
| P2 | Mysz | 50 |

**Tabela Pozycje_Zamowienia:**
| ID_Zamowienia | ID_Produktu | Ilosc |
|--------------|-------------|-------|
| 1 | P1 | 2 |
| 1 | P2 | 3 |
| 2 | P1 | 1 |

Każdy atrybut niekluczowy zależy teraz od całego klucza!

---

## 8) Trzecia forma normalna (3NF)

### Definicja
Tabela jest w **3NF**, jeśli:
1. ✅ Jest w **2NF**
2. ✅ **Nie ma zależności przechodnich** — żaden atrybut niekluczowy nie zależy od innego atrybutu niekluczowego

### Warunek formalny
Dla każdej zależności funkcyjnej \( A \to B \), gdzie \( B \) jest atrybutem niekluczowym:
- \( A \) jest **nadkluczem** (superkey), LUB
- \( B \) jest **częścią klucza**

**Innymi słowy:** Atrybuty niekluczowe muszą zależeć **tylko od klucza**, nie od innych atrybutów niekluczowych.

### Przykład naruszenia 3NF:

**PRZED (złe):**
| ID_Pracownika | Imie | ID_Dzialu | Nazwa_Dzialu | Miasto_Dzialu |
|--------------|------|-----------|--------------|---------------|
| 1 | Jan | D1 | IT | Warszawa |
| 2 | Anna | D1 | IT | Warszawa |
| 3 | Piotr | D2 | HR | Kraków |

**Klucz główny:** `ID_Pracownika`

**Zależności:**
- `ID_Pracownika` → `Imie` ✅
- `ID_Pracownika` → `ID_Dzialu` ✅
- `ID_Pracownika` → `Nazwa_Dzialu` ✅ (ale przechodnia!)
- `ID_Pracownika` → `Miasto_Dzialu` ✅ (ale przechodnia!)

**Zależność przechodnia:**
- `ID_Pracownika` → `ID_Dzialu`
- `ID_Dzialu` → `Nazwa_Dzialu`
- `ID_Dzialu` → `Miasto_Dzialu`

**Problem:**
- `Nazwa_Dzialu` i `Miasto_Dzialu` zależą od `ID_Dzialu` (atrybut niekluczowy)
- Duplikacja danych (IT, Warszawa powtarza się)
- Anomalie aktualizacji (zmiana nazwy działu wymaga aktualizacji wielu wierszy)

**PO (3NF):**

**Tabela Pracownicy:**
| ID_Pracownika | Imie | ID_Dzialu |
|--------------|------|-----------|
| 1 | Jan | D1 |
| 2 | Anna | D1 |
| 3 | Piotr | D2 |

**Tabela Dzialy:**
| ID_Dzialu | Nazwa_Dzialu | Miasto_Dzialu |
|-----------|--------------|---------------|
| D1 | IT | Warszawa |
| D2 | HR | Kraków |

Teraz nie ma zależności przechodnich — wszystko zależy tylko od kluczy!

---

## 9) Boyce-Codd Normal Form (BCNF)

### Definicja
Tabela jest w **BCNF**, jeśli:
1. ✅ Jest w **3NF**
2. ✅ **Każda determinanta jest nadkluczem** (superkey)

### Różnica między 3NF a BCNF:
- **3NF** pozwala na zależności, gdzie atrybut niekluczowy zależy od innego atrybutu niekluczowego, **jeśli ten jest częścią klucza**
- **BCNF** wymaga, aby **każda determinanta była nadkluczem**

### Przykład naruszenia BCNF (ale w 3NF):

**PRZED:**
| ID_Studenta | ID_Przedmiotu | ID_Prowadzacego | Sala |
|------------|---------------|-----------------|------|
| S1 | P1 | T1 | 101 |
| S1 | P2 | T2 | 102 |
| S2 | P1 | T1 | 101 |

**Zależności:**
- `{ID_Studenta, ID_Przedmiotu}` → `ID_Prowadzacego` (klucz → niekluczowy) ✅
- `{ID_Studenta, ID_Przedmiotu}` → `Sala` (klucz → niekluczowy) ✅
- `ID_Prowadzacego` → `ID_Przedmiotu` ❌ (niekluczowy → część klucza)
- `ID_Prowadzacego` → `Sala` ❌ (niekluczowy → niekluczowy)

**Analiza:**
- Jest w 3NF, bo `ID_Przedmiotu` (część klucza) zależy od `ID_Prowadzacego`
- **NIE** jest w BCNF, bo `ID_Prowadzacego` (niekluczowy) jest determinantą, ale nie jest nadkluczem

**Problem:**
- Jeden prowadzący prowadzi jeden przedmiot
- Możliwe niespójności (jeśli spróbujemy zmienić przedmiot prowadzącego)

**PO (BCNF):**

**Tabela Studenci_Przedmioty:**
| ID_Studenta | ID_Prowadzacego |
|------------|-----------------|
| S1 | T1 |
| S1 | T2 |
| S2 | T1 |

**Tabela Prowadzacy_Przedmioty_Sale:**
| ID_Prowadzacego | ID_Przedmiotu | Sala |
|-----------------|---------------|------|
| T1 | P1 | 101 |
| T2 | P2 | 102 |

Teraz każda determinanta jest nadkluczem!

---

## 10) Czwarta i Piąta forma normalna (4NF, 5NF)

### Czwarta forma normalna (4NF)

**Definicja:** Tabela jest w **4NF**, jeśli:
1. ✅ Jest w **BCNF**
2. ✅ **Nie ma wielowartościowych zależności** (Multi-Valued Dependencies, MVD), które nie wynikają z klucza

**MVD:** \( A \twoheadrightarrow B \) oznacza, że dla każdej wartości \( A \) istnieje **zbiór** wartości \( B \), niezależny od innych atrybutów.

**Przykład:**
| ID_Pracownika | Język | Umiejętność |
|--------------|-------|-------------|
| 1 | Polski | Programowanie |
| 1 | Angielski | Programowanie |
| 1 | Polski | Projektowanie |

**Problem:** Języki i umiejętności są niezależne — każdy pracownik ma wszystkie kombinacje.

**Rozwiązanie:** Dwie osobne tabele (Języki, Umiejętności).

### Piąta forma normalna (5NF)

**Definicja:** Tabela jest w **5NF**, jeśli:
1. ✅ Jest w **4NF**
2. ✅ **Nie ma zależności złączenia** (Join Dependencies), które nie wynikają z kluczy

**Rzadko stosowana w praktyce** — zbyt skomplikowana.

---

## 11) Proces normalizacji — krok po kroku

### Algorytm normalizacji:

1. **Start:** Tabela nienormalizowana (0NF)
2. **Krok 1:** Eliminuj powtarzające się grupy → **1NF**
3. **Krok 2:** 
   - Zidentyfikuj zależności funkcyjne
   - Znajdź częściowe zależności
   - Wydziel tabele dla częściowych zależności → **2NF**
4. **Krok 3:**
   - Znajdź zależności przechodnie
   - Wydziel tabele dla zależności przechodnich → **3NF**
5. **Krok 4 (opcjonalnie):**
   - Sprawdź, czy każda determinanta jest nadkluczem
   - Jeśli nie, wydziel tabele → **BCNF**

### Przykład kompleksowy — normalizacja krok po kroku:

**Tabela początkowa (0NF):**
| ID_Zamowienia | Data | ID_Klienta | Nazwa_Klienta | Adres_Klienta | ID_Produktu | Nazwa_Produktu | Cena | Ilosc | ID_Pracownika | Imie_Pracownika | Dzial_Pracownika |
|--------------|------|------------|---------------|---------------|-------------|----------------|------|-------|---------------|-----------------|------------------|
| 1 | 2024-01-15 | K1 | Jan K. | Warszawa | P1, P2 | Laptop, Mysz | 5000, 50 | 2, 3 | E1 | Anna | IT |

**Problem:** Wartości złożone, powtarzające się grupy.

**Po 1NF (wartości atomowe):**
| ID_Zamowienia | Data | ID_Klienta | Nazwa_Klienta | Adres_Klienta | ID_Produktu | Nazwa_Produktu | Cena | Ilosc | ID_Pracownika | Imie_Pracownika | Dzial_Pracownika |
|--------------|------|------------|---------------|---------------|-------------|----------------|------|-------|---------------|-----------------|------------------|
| 1 | 2024-01-15 | K1 | Jan K. | Warszawa | P1 | Laptop | 5000 | 2 | E1 | Anna | IT |
| 1 | 2024-01-15 | K1 | Jan K. | Warszawa | P2 | Mysz | 50 | 3 | E1 | Anna | IT |

**Po 2NF (eliminacja częściowych zależności):**

**Zamowienia:**
| ID_Zamowienia | Data | ID_Klienta | ID_Pracownika |
|--------------|------|------------|---------------|
| 1 | 2024-01-15 | K1 | E1 |

**Klienci:**
| ID_Klienta | Nazwa_Klienta | Adres_Klienta |
|------------|---------------|---------------|
| K1 | Jan K. | Warszawa |

**Produkty:**
| ID_Produktu | Nazwa_Produktu | Cena |
|-------------|----------------|------|
| P1 | Laptop | 5000 |
| P2 | Mysz | 50 |

**Pracownicy:**
| ID_Pracownika | Imie_Pracownika | Dzial_Pracownika |
|---------------|-----------------|------------------|
| E1 | Anna | IT |

**Pozycje_Zamowienia:**
| ID_Zamowienia | ID_Produktu | Ilosc |
|--------------|-------------|-------|
| 1 | P1 | 2 |
| 1 | P2 | 3 |

**Po 3NF (eliminacja zależności przechodnich):**
Dodajemy tabelę **Dzialy** (jeśli `Dzial_Pracownika` ma więcej atrybutów w przyszłości):

**Dzialy:**
| ID_Dzialu | Nazwa_Dzialu |
|-----------|--------------|
| IT | IT |

**Pracownicy (zaktualizowana):**
| ID_Pracownika | Imie_Pracownika | ID_Dzialu |
|---------------|-----------------|-----------|
| E1 | Anna | IT |

---

## 12) Denormalizacja — kiedy i dlaczego?

### Definicja
**Denormalizacja** to celowe wprowadzenie redundancji do znormalizowanej bazy danych w celu poprawy wydajności.

### Kiedy denormalizować?

#### 1. **Problemy z wydajnością**
- Zbyt wiele JOIN-ów spowalnia zapytania
- Tabela często czytana, rzadko modyfikowana

#### 2. **Analiza danych (OLAP)**
- Tabele analityczne (data warehouses)
- Duże agregacje, rzadkie aktualizacje

#### 3. **Czytelność i prostota**
- Proste raporty
- Mniej złożone zapytania

### Przykład denormalizacji:

**Znormalizowane (3NF):**
```
Zamowienia → Klienci
Zamowienia → Pracownicy
Pracownicy → Dzialy
```

**Zapytanie:** Pokaż zamówienia z nazwą klienta i działem pracownika
```sql
SELECT z.*, k.Nazwa_Klienta, d.Nazwa_Dzialu
FROM Zamowienia z
JOIN Klienci k ON z.ID_Klienta = k.ID_Klienta
JOIN Pracownicy p ON z.ID_Pracownika = p.ID_Pracownika
JOIN Dzialy d ON p.ID_Dzialu = d.ID_Dzialu;
```

**Zdenormalizowane (dla wydajności):**
```
Zamowienia (z polami: Nazwa_Klienta, Nazwa_Dzialu_Pracownika)
```

**Zapytanie:**
```sql
SELECT * FROM Zamowienia;  -- Prostsze, szybsze
```

### Kiedy NIE denormalizować?
- ❌ Gdy dane często się zmieniają
- ❌ Gdy ważna jest spójność danych
- ❌ Gdy redundancja powoduje problemy z aktualizacją

### Zasada:
> **Najpierw normalizuj, potem denormalizuj tylko tam, gdzie to konieczne dla wydajności.**

---

## 13) Wzory i reguły podsumowujące

### Sprawdzanie 1NF:
✅ Wszystkie wartości są atomowe?
✅ Brak powtarzających się grup?

### Sprawdzanie 2NF:
✅ Jest w 1NF?
✅ Dla każdej zależności \( A \to B \) (B niekluczowy): czy \( A \) jest nadkluczem lub \( B \) zależy od całego klucza?

### Sprawdzanie 3NF:
✅ Jest w 2NF?
✅ Dla każdej zależności \( A \to B \) (B niekluczowy): czy \( A \) jest nadkluczem lub \( B \) jest częścią klucza?

### Sprawdzanie BCNF:
✅ Jest w 3NF?
✅ Dla każdej zależności \( A \to B \): czy \( A \) jest nadkluczem?

### Reguła normalizacji:
\[
\text{Normalizacja} = \text{Eliminacja redundancji} + \text{Eliminacja anomalii}
\]

### Liczba tabel po normalizacji:
- **1NF:** Minimalna liczba tabel (może być 1, jeśli już atomowe)
- **2NF:** Zwiększa się o liczbę częściowych zależności
- **3NF:** Zwiększa się o liczbę zależności przechodnich
- **BCNF:** Może jeszcze więcej (zależnie od zależności)

---

## 14) Praktyczne wskazówki

### Kiedy normalizować?
- ✅ W większości aplikacji biznesowych (OLTP)
- ✅ Gdy ważna jest spójność danych
- ✅ Gdy dane często się zmieniają
- ✅ W systemach transakcyjnych

### Do jakiego stopnia normalizować?
- **Praktycznie:** Do **3NF** lub **BCNF** zwykle wystarczy
- **4NF i 5NF:** Rzadko potrzebne, zbyt skomplikowane

### Kroki przy projektowaniu:
1. **Zidentyfikuj encje** (podmioty) — Klienci, Produkty, Zamowienia
2. **Zidentyfikuj atrybuty** każdej encji
3. **Znajdź zależności funkcyjne**
4. **Normalizuj** do 3NF/BCNF
5. **Sprawdź wydajność** — jeśli potrzeba, selektywnie denormalizuj

### Częste błędy:
- ❌ Zbyt wczesna denormalizacja
- ❌ Normalizacja "na ślepo" bez zrozumienia zależności
- ❌ Ignorowanie wydajności (zbyt wiele JOIN-ów)
- ❌ Zapominanie o kluczach obcych (integralność referencyjna)

---

## 15) Kluczowe zdania na obronę

1. **Normalizacja** eliminuje redundancję i anomalie poprzez organizację danych.

2. **Anomalie:** wstawiania (nie można dodać danych bez innych), aktualizacji (zmiana w wielu miejscach), usuwania (utrata danych).

3. **1NF** = wartości atomowe, brak powtarzających się grup.

4. **2NF** = 1NF + brak częściowych zależności (atrybuty niekluczowe zależą od całego klucza).

5. **3NF** = 2NF + brak zależności przechodnich (atrybuty niekluczowe zależą tylko od klucza).

6. **BCNF** = 3NF + każda determinanta jest nadkluczem.

7. **Zależność funkcyjna** \( A \to B \) oznacza, że A jednoznacznie określa B.

8. **Zależność przechodnia:** \( A \to B \to C \), gdzie B nie jest kluczem.

9. **Normalizacja** zwiększa liczbę tabel, ale eliminuje problemy z danymi.

10. **Denormalizacja** = celowe wprowadzenie redundancji dla wydajności (tylko tam, gdzie to konieczne).

11. **Praktycznie** normalizujemy do 3NF/BCNF — 4NF i 5NF są rzadko potrzebne.

12. **Zasada:** Najpierw normalizuj, potem selektywnie denormalizuj dla wydajności.

