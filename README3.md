# Normalizacja Schematu Bazy Danych — Kompletna Notatka do Obrony

## 0) Podstawy — od zera (dla początkujących)

### Co to jest baza danych?

**Baza danych** = zorganizowany zbiór danych przechowywanych w komputerze.

**Przykład z życia:**
- Książka telefoniczna — lista osób z numerami telefonów
- Katalog biblioteczny — lista książek z autorami
- System sklepu — lista produktów, klientów, zamówień

**W komputerze:** Baza danych to pliki/tabele z danymi, które można szybko wyszukiwać i modyfikować.

### Co to jest tabela?

**Tabela** = struktura danych przypominająca arkusz kalkulacyjny.

**Składa się z:**
- **Kolumny (atrybuty)** — opisują, jakie informacje przechowujemy
- **Wiersze (rekordy)** — konkretne dane

**Przykład tabeli "Studenci":**

| ID_Studenta | Imie | Nazwisko | Email | Rok_Studiow |
|------------|------|----------|-------|-------------|
| 1 | Jan | Kowalski | jan@email.com | 2 |
| 2 | Anna | Nowak | anna@email.com | 1 |
| 3 | Piotr | Wiśniewski | piotr@email.com | 3 |

**Wyjaśnienie:**
- **Kolumny:** ID_Studenta, Imie, Nazwisko, Email, Rok_Studiow
- **Wiersze:** Każdy wiersz to jeden student
- **ID_Studenta** — unikalny identyfikator (każdy student ma inny)

### Co to jest relacja między tabelami?

**Relacja** = powiązanie między tabelami.

**Przykład:**
- Tabela "Studenci" — dane o studentach
- Tabela "Oceny" — oceny studentów
- **Relacja:** Każda ocena należy do jednego studenta

**Jak to działa?**
- W tabeli "Oceny" mamy kolumnę "ID_Studenta"
- Ta kolumna wskazuje na studenta w tabeli "Studenci"
- To jest **klucz obcy** (foreign key)

**Wizualizacja:**
```
Tabela Studenci          Tabela Oceny
┌─────────────┐         ┌─────────────┐
│ ID_Studenta │─────────→│ ID_Studenta │ (klucz obcy)
│ Imie        │         │ Przedmiot   │
│ Nazwisko    │         │ Ocena       │
└─────────────┘         └─────────────┘
```

### Co to jest klucz główny (Primary Key)?

**Klucz główny** = kolumna (lub kolumny), która **jednoznacznie identyfikuje** każdy wiersz.

**Właściwości:**
- **Unikalny** — każdy wiersz ma inną wartość
- **Nie może być NULL** — zawsze musi mieć wartość
- **Niezmienny** — nie powinien się zmieniać

**Przykład:**
- W tabeli "Studenci": **ID_Studenta** jest kluczem głównym
- Każdy student ma unikalny ID (1, 2, 3, ...)
- Nie może być dwóch studentów z tym samym ID

**Dlaczego potrzebny?**
- Pozwala jednoznacznie wskazać konkretny wiersz
- Umożliwia tworzenie relacji między tabelami
- Zapewnia, że dane są unikalne

### Co to jest redundancja danych?

**Redundancja** = duplikowanie tych samych informacji w wielu miejscach.

**Przykład ZŁY (z redundancją):**

Tabela "Zamowienia":
| ID_Zamowienia | Klient | Miasto_Klienta | Produkt | Cena |
|--------------|--------|----------------|---------|------|
| 1 | Jan Kowalski | Warszawa | Laptop | 5000 |
| 2 | Jan Kowalski | Warszawa | Mysz | 50 |
| 3 | Anna Nowak | Kraków | Klawiatura | 200 |

**Problem:** 
- "Jan Kowalski" i "Warszawa" powtarzają się w wierszach 1 i 2
- Jeśli Jan zmieni miasto, trzeba zmienić w **dwóch miejscach**!

**Przykład DOBRY (bez redundancji):**

Tabela "Klienci":
| ID_Klienta | Imie | Nazwisko | Miasto |
|-----------|------|----------|--------|
| K1 | Jan | Kowalski | Warszawa |
| K2 | Anna | Nowak | Kraków |

Tabela "Zamowienia":
| ID_Zamowienia | ID_Klienta | Produkt | Cena |
|--------------|------------|---------|------|
| 1 | K1 | Laptop | 5000 |
| 2 | K1 | Mysz | 50 |
| 3 | K2 | Klawiatura | 200 |

**Korzyści:**
- Dane o kliencie są w **jednym miejscu**
- Zmiana miasta = zmiana w **jednym miejscu**
- Mniej miejsca w bazie

### Co to są anomalie danych?

**Anomalia** = problem, który pojawia się przy operacjach na danych (wstawianie, aktualizacja, usuwanie).

**Trzy typy anomalii:**
1. **Anomalia wstawiania** — nie można dodać danych bez innych
2. **Anomalia aktualizacji** — trzeba zmieniać w wielu miejscach
3. **Anomalia usuwania** — usunięcie jednych danych usuwa inne

**Przykład anomalii:**

Tabela "Zamowienia" (zła struktura):
| ID_Zamowienia | Klient | Miasto | Produkt |
|--------------|--------|--------|---------|
| 1 | Jan | Warszawa | Laptop |
| 2 | Jan | Warszawa | Mysz |

**Anomalia wstawiania:**
- Nie można dodać nowego klienta bez zamówienia!
- Musisz najpierw stworzyć zamówienie, żeby dodać klienta

**Anomalia aktualizacji:**
- Jan zmienił miasto na "Kraków"
- Musisz zmienić w **dwóch wierszach** (1 i 2)
- Jeśli zapomnisz jednego → dane niespójne!

**Anomalia usuwania:**
- Usuwasz zamówienie 2 (Mysz)
- OK, ale co jeśli to było ostatnie zamówienie Jana?
- Możesz przypadkiem "stracić" informację o kliencie Jan!

**Rozwiązanie:** Normalizacja — podzielić na tabele i usunąć redundancję.

---

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

### Definicja — intuicyjnie

**Zależność funkcyjna** = jeśli znamy wartość jednego atrybutu, możemy jednoznacznie określić wartość innego.

**Notacja:** A → B
- **A** — atrybut determinujący (z którego zaczynamy)
- **B** — atrybut zależny (który określamy)
- Strzałka → oznacza "określa" lub "zależy od"

**Czytamy:** "A określa B" lub "B zależy funkcyjnie od A"

**Intuicja:** To jak funkcja matematyczna f(x) = y
- Jeśli znamy x, możemy obliczyć y
- Jeśli znamy A, możemy określić B

### Definicja formalna

**Zależność funkcyjna** (FD): A → B oznacza, że dla każdej wartości atrybutu A istnieje dokładnie jedna wartość atrybutu B.

**Innymi słowy:**
- Dla każdej wartości A jest dokładnie jedna wartość B
- Nie może być dwóch różnych wartości B dla tego samego A

### Przykład szczegółowy — krok po kroku

**Tabela "Studenci":**

| ID_Studenta | Imie | Nazwisko | Email | Rok_Studiow |
|------------|------|----------|-------|-------------|
| 1 | Jan | Kowalski | jan@email.com | 2 |
| 2 | Anna | Nowak | anna@email.com | 1 |
| 3 | Piotr | Wiśniewski | piotr@email.com | 3 |

**Analiza zależności:**

**1. ID_Studenta → Imie**
- ID = 1 → Imie = "Jan" (zawsze!)
- ID = 2 → Imie = "Anna" (zawsze!)
- ID = 3 → Imie = "Piotr" (zawsze!)
- **Wniosek:** Zależność funkcyjna ✓

**2. ID_Studenta → Nazwisko**
- ID = 1 → Nazwisko = "Kowalski" (zawsze!)
- ID = 2 → Nazwisko = "Nowak" (zawsze!)
- **Wniosek:** Zależność funkcyjna ✓

**3. ID_Studenta → Email**
- ID = 1 → Email = "jan@email.com" (zawsze!)
- **Wniosek:** Zależność funkcyjna ✓

**4. Imie → Nazwisko?**
- Imie = "Jan" → Nazwisko = "Kowalski"
- Ale: Imie = "Jan" może być u innego studenta z innym nazwiskiem!
- **Wniosek:** NIE ma zależności funkcyjnej ✗

**5. Email → ID_Studenta?**
- Email = "jan@email.com" → ID = 1 (zawsze, bo email jest unikalny)
- **Wniosek:** Zależność funkcyjna ✓

**Podsumowanie zależności w tej tabeli:**
- ID_Studenta → Imie ✓
- ID_Studenta → Nazwisko ✓
- ID_Studenta → Email ✓
- ID_Studenta → Rok_Studiow ✓
- Email → ID_Studenta ✓ (bo email jest unikalny)
- Imie → Nazwisko ✗ (nie ma zależności)

### Zależności pełne i częściowe

#### Częściowa zależność (Partial Dependency) — szczegółowo

**Definicja:** B zależy częściowo od A, jeśli:
- A → B (jest zależność)
- Istnieje podzbiór A' (część A) taki, że A' → B (zależność działa już dla części)

**Intuicja:** Nie potrzebujemy całego A, żeby określić B — wystarczy część A.

**Przykład szczegółowy:**

Tabela "Oceny":
| ID_Studenta | Przedmiot | Ocena | Imie_Studenta |
|------------|-----------|-------|---------------|
| 1 | Matematyka | 5 | Jan |
| 1 | Fizyka | 4 | Jan |
| 2 | Matematyka | 3 | Anna |

**Analiza:**

**1. Klucz główny:** {ID_Studenta, Przedmiot}
- Potrzebujemy obu, żeby jednoznacznie zidentyfikować ocenę

**2. Zależność:** {ID_Studenta, Przedmiot} → Ocena
- Znamy ID i Przedmiot → możemy określić Ocenę ✓

**3. Częściowa zależność:** ID_Studenta → Imie_Studenta
- Znamy tylko ID_Studenta → możemy określić Imie_Studenta
- Nie potrzebujemy Przedmiot!
- **Wniosek:** Imie_Studenta zależy **częściowo** od klucza {ID_Studenta, Przedmiot}

**4. Pełna zależność:** {ID_Studenta, Przedmiot} → Ocena
- Potrzebujemy **obu** atrybutów klucza
- ID_Studenta → Ocena? ✗ (student ma różne oceny)
- Przedmiot → Ocena? ✗ (różni studenci mają różne oceny z tego samego przedmiotu)
- **Wniosek:** Ocena zależy **w pełni** od klucza

**Problem z częściową zależnością:**
- Imie_Studenta powtarza się w każdym wierszu z tym samym studentem
- Redundancja! → trzeba normalizować

#### Pełna zależność (Full Dependency) — szczegółowo

**Definicja:** B zależy w pełni od A, jeśli:
- A → B (jest zależność)
- B NIE zależy od żadnego właściwego podzbioru A (potrzebujemy całego A)

**Intuicja:** Potrzebujemy wszystkich atrybutów z A, żeby określić B.

**Przykład:**

Tabela "Pozycje_Zamowienia":
| ID_Zamowienia | ID_Produktu | Ilosc | Cena_Produktu |
|--------------|-------------|-------|---------------|
| 1 | P1 | 2 | 5000 |
| 1 | P2 | 3 | 50 |
| 2 | P1 | 1 | 5000 |

**Analiza:**

**1. Klucz główny:** {ID_Zamowienia, ID_Produktu}

**2. Pełna zależność:** {ID_Zamowienia, ID_Produktu} → Ilosc
- Potrzebujemy obu atrybutów
- ID_Zamowienia → Ilosc? ✗ (zamówienie ma różne ilości różnych produktów)
- ID_Produktu → Ilosc? ✗ (ten sam produkt w różnych zamówieniach ma różne ilości)
- **Wniosek:** Ilosc zależy w pełni od klucza ✓

**3. Częściowa zależność:** ID_Produktu → Cena_Produktu
- Wystarczy tylko ID_Produktu, żeby określić cenę
- Nie potrzebujemy ID_Zamowienia!
- **Wniosek:** Cena_Produktu zależy częściowo od klucza ✗

### Zależność przechodnia (Transitive Dependency) — szczegółowo

**Definicja:** A → B → C (przechodnia), jeśli:
- A → B (A określa B)
- B → C (B określa C)
- A → C (pośrednio, przez B)
- A ↛ C (bezpośrednio — nie ma bezpośredniej zależności)

**Intuicja:** A określa C, ale **pośrednio** — przez B. To jak łańcuch: A → B → C

**Przykład szczegółowy — krok po kroku:**

Tabela "Pracownicy":
| ID_Pracownika | Imie | ID_Dzialu | Nazwa_Dzialu | Miasto_Dzialu |
|--------------|------|-----------|--------------|---------------|
| 1 | Jan | D1 | IT | Warszawa |
| 2 | Anna | D1 | IT | Warszawa |
| 3 | Piotr | D2 | HR | Kraków |

**Analiza zależności:**

**1. ID_Pracownika → ID_Dzialu**
- ID = 1 → Dział = D1 ✓
- ID = 2 → Dział = D1 ✓
- **Wniosek:** Zależność funkcyjna ✓

**2. ID_Dzialu → Nazwa_Dzialu**
- Dział = D1 → Nazwa = "IT" ✓
- Dział = D2 → Nazwa = "HR" ✓
- **Wniosek:** Zależność funkcyjna ✓

**3. ID_Pracownika → Nazwa_Dzialu?**
- ID = 1 → Nazwa = "IT" (bo ID = 1 → Dział = D1 → Nazwa = "IT")
- **Wniosek:** Zależność przechodnia ✓

**Wizualizacja łańcucha:**
```
ID_Pracownika → ID_Dzialu → Nazwa_Dzialu
     1    →     D1    →      IT
```

**Problem z zależnością przechodnią:**
- Nazwa_Dzialu powtarza się dla wszystkich pracowników z tego samego działu
- Redundancja! → trzeba normalizować

**Rozwiązanie:**
- Wydzielić tabelę "Dzialy" z ID_Dzialu i Nazwa_Dzialu
- W tabeli "Pracownicy" zostawić tylko ID_Dzialu (klucz obcy)

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

## 6) Pierwsza forma normalna (1NF) — szczegółowo

### Definicja — intuicyjnie

**1NF** = każda komórka w tabeli zawiera **jedną, niepodzielną wartość**.

**Zasady:**
1. ✅ **Wartości atomowe** — nie można dzielić dalej
2. ✅ **Brak powtarzających się grup** — nie ma kolumn typu "Telefon1", "Telefon2", "Telefon3"
3. ✅ **Unikalne wiersze** — każdy wiersz jest inny

**Intuicja:** To jak adres w formularzu — każda część (ulica, miasto, kod) w osobnej kolumnie, nie wszystko w jednej.

### Definicja formalna

Tabela jest w **1NF**, jeśli:
1. ✅ **Każda kolumna zawiera wartości atomowe** (niepodzielne)
2. ✅ **Nie ma powtarzających się grup** (repeating groups)
3. ✅ **Każdy wiersz jest unikalny**

### Przykład naruszenia 1NF — szczegółowa analiza:

**PRZED (złe — narusza 1NF):**

Tabela "Studenci":
| ID_Studenta | Imie | Przedmioty |
|------------|------|------------|
| 1 | Jan | Matematyka, Fizyka, Chemia |
| 2 | Anna | Matematyka, Informatyka |

**Problemy krok po kroku:**

**Problem 1: Wartości złożone (nieatomowe)**
- Kolumna "Przedmioty" zawiera **wiele wartości** oddzielonych przecinkami
- "Matematyka, Fizyka, Chemia" to **3 wartości w jednej komórce**!
- Nie można łatwo:
  - Wyszukać wszystkich studentów z "Fizyka"
  - Dodać nowego przedmiotu
  - Usunąć jednego przedmiotu

**Problem 2: Trudność w operacjach**
- Jak znaleźć wszystkich studentów z "Fizyka"?
  - Trzeba przeszukać tekst w kolumnie (wolne i nieefektywne!)
- Jak dodać "Biologia" dla Jana?
  - Trzeba zmodyfikować cały tekst "Matematyka, Fizyka, Chemia, Biologia"
- Jak usunąć "Chemia" dla Jana?
  - Trzeba usunąć z tekstu (ryzyko błędu!)

**Problem 3: Brak normalizacji**
- Nie można łatwo policzyć, ile studentów ma dany przedmiot
- Nie można łatwo dodać informacji o przedmiocie (np. prowadzący)

**PO (1NF) — rozwiązanie 1: Rozdzielenie na wiersze**

Tabela "Studenci":
| ID_Studenta | Imie | Przedmiot |
|------------|------|-----------|
| 1 | Jan | Matematyka |
| 1 | Jan | Fizyka |
| 1 | Jan | Chemia |
| 2 | Anna | Matematyka |
| 2 | Anna | Informatyka |

**Analiza:**
- ✅ Każda komórka zawiera **jedną wartość**
- ✅ Można łatwo wyszukiwać po przedmiocie
- ✅ Można łatwo dodawać/usuwać przedmioty

**Ale:** Redundancja! "Jan" powtarza się 3 razy.

**PO (1NF) — rozwiązanie 2: Osobne tabele (LEPSZE)**

**Tabela "Studenci":**
| ID_Studenta | Imie |
|------------|------|
| 1 | Jan |
| 2 | Anna |

**Tabela "Studenci_Przedmioty":**
| ID_Studenta | Przedmiot |
|------------|-----------|
| 1 | Matematyka |
| 1 | Fizyka |
| 1 | Chemia |
| 2 | Matematyka |
| 2 | Informatyka |

**Analiza:**
- ✅ Każda komórka zawiera **jedną wartość** (1NF)
- ✅ Brak redundancji — dane o studencie w jednym miejscu
- ✅ Łatwo wyszukiwać, dodawać, usuwać
- ✅ Można łatwo rozszerzyć (np. dodać ocenę z przedmiotu)

**Relacja:**
- ID_Studenta w "Studenci_Przedmioty" to **klucz obcy**
- Wskazuje na studenta w tabeli "Studenci"

### Inne przykłady naruszenia 1NF — szczegółowo:

#### Przykład 1: Powtarzające się kolumny (repeating columns)

**PRZED (złe):**
| ID | Imie | Telefon1 | Telefon2 | Telefon3 |
|---|---|---|---|---|---|
| 1 | Jan | 123-456 | 789-012 | NULL |
| 2 | Anna | 111-222 | NULL | NULL |

**Problemy:**
- Co jeśli ktoś ma 4 telefony? Trzeba dodać kolumnę Telefon4!
- Wiele kolumn NULL (marnowanie miejsca)
- Trudno wyszukiwać (która kolumna ma dany numer?)

**PO (1NF — lepsze rozwiązanie):**

**Tabela "Osoby":**
| ID | Imie |
|---|---|
| 1 | Jan |
| 2 | Anna |

**Tabela "Telefony":**
| ID_Osoby | Telefon |
|----------|---------|
| 1 | 123-456 |
| 1 | 789-012 |
| 2 | 111-222 |

**Korzyści:**
- ✅ Można mieć dowolną liczbę telefonów (dodajemy wiersze)
- ✅ Brak NULL-ów
- ✅ Łatwo wyszukiwać

#### Przykład 2: Tablice/wielowartościowe atrybuty

**PRZED (złe):**
| ID | Imie | Oceny[] |
|---|---|---------|
| 1 | Jan | [5, 4, 5] |
| 2 | Anna | [3, 4] |

**Problemy:**
- Oceny jako tablica w jednej kolumnie
- Nie można łatwo wyszukiwać (kto ma ocenę 5?)
- Nie można dodać informacji o ocenie (z jakiego przedmiotu? kiedy?)

**PO (1NF — lepsze rozwiązanie):**

**Tabela "Studenci":**
| ID | Imie |
|---|---|
| 1 | Jan |
| 2 | Anna |

**Tabela "Oceny":**
| ID_Studenta | Przedmiot | Ocena |
|------------|-----------|-------|
| 1 | Matematyka | 5 |
| 1 | Fizyka | 4 |
| 1 | Chemia | 5 |
| 2 | Matematyka | 3 |
| 2 | Fizyka | 4 |

**Korzyści:**
- ✅ Każda ocena w osobnym wierszu
- ✅ Można dodać więcej informacji (przedmiot, data)
- ✅ Łatwo wyszukiwać i analizować

---

## 7) Druga forma normalna (2NF) — szczegółowo

### Definicja — intuicyjnie

**2NF** = 1NF + **brak częściowych zależności**.

**Intuicja:** Jeśli klucz jest złożony (składa się z kilku kolumn), to wszystkie atrybuty niekluczowe muszą zależeć od **całego klucza**, nie tylko od jego części.

**Przykład z życia:**
- Klucz: {Numer_Zamowienia, Numer_Produktu}
- Atrybut: Cena_Produktu
- Problem: Cena zależy tylko od Numer_Produktu (część klucza), nie od całego klucza!
- Rozwiązanie: Wydzielić tabelę Produkty z cenami

### Definicja formalna

Tabela jest w **2NF**, jeśli:
1. ✅ Jest w **1NF**
2. ✅ **Każdy atrybut niekluczowy zależy w pełni od klucza głównego** (nie ma częściowych zależności)

### Warunek formalny

Dla każdej zależności funkcyjnej A → B, gdzie B jest atrybutem niekluczowym:
- A musi być **nadkluczem** (superkey) — zawiera klucz, LUB
- B zależy od **całego** klucza, nie od jego części

### Przykład naruszenia 2NF — szczegółowa analiza krok po kroku:

**PRZED (złe — narusza 2NF, ale jest w 1NF):**

Tabela "Pozycje_Zamowienia":
| ID_Zamowienia | ID_Produktu | Nazwa_Produktu | Cena_Produktu | Ilosc | Data_Zamowienia |
|--------------|-------------|----------------|---------------|-------|----------------|
| 1 | P1 | Laptop | 5000 | 2 | 2024-01-15 |
| 1 | P2 | Mysz | 50 | 3 | 2024-01-15 |
| 2 | P1 | Laptop | 5000 | 1 | 2024-01-16 |

**Krok 1: Identyfikacja klucza głównego**

**Klucz główny:** {ID_Zamowienia, ID_Produktu}
- Dlaczego? Potrzebujemy obu, żeby jednoznacznie zidentyfikować pozycję zamówienia
- ID_Zamowienia = 1, ID_Produktu = P1 → Ilosc = 2
- ID_Zamowienia = 1, ID_Produktu = P2 → Ilosc = 3

**Krok 2: Identyfikacja atrybutów niekluczowych**

Atrybuty niekluczowe (nie są częścią klucza):
- Nazwa_Produktu
- Cena_Produktu
- Ilosc
- Data_Zamowienia

**Krok 3: Analiza zależności funkcyjnych**

**1. {ID_Zamowienia, ID_Produktu} → Ilosc**
- Znamy ID_Zamowienia = 1 i ID_Produktu = P1 → Ilosc = 2 ✓
- Potrzebujemy **obu** atrybutów klucza
- ID_Zamowienia → Ilosc? ✗ (zamówienie ma różne ilości różnych produktów)
- ID_Produktu → Ilosc? ✗ (ten sam produkt w różnych zamówieniach ma różne ilości)
- **Wniosek:** Ilosc zależy **w pełni** od klucza ✓

**2. {ID_Zamowienia, ID_Produktu} → Nazwa_Produktu**
- Znamy ID_Zamowienia = 1 i ID_Produktu = P1 → Nazwa = "Laptop" ✓
- **ALE:** ID_Produktu → Nazwa_Produktu? ✓ (produkt P1 zawsze ma nazwę "Laptop")
- Nie potrzebujemy ID_Zamowienia!
- **Wniosek:** Nazwa_Produktu zależy **częściowo** od klucza ✗

**3. {ID_Zamowienia, ID_Produktu} → Cena_Produktu**
- Znamy ID_Zamowienia = 1 i ID_Produktu = P1 → Cena = 5000 ✓
- **ALE:** ID_Produktu → Cena_Produktu? ✓ (produkt P1 zawsze ma cenę 5000)
- Nie potrzebujemy ID_Zamowienia!
- **Wniosek:** Cena_Produktu zależy **częściowo** od klucza ✗

**4. {ID_Zamowienia, ID_Produktu} → Data_Zamowienia**
- Znamy ID_Zamowienia = 1 i ID_Produktu = P1 → Data = 2024-01-15 ✓
- **ALE:** ID_Zamowienia → Data_Zamowienia? ✓ (zamówienie 1 zawsze ma datę 2024-01-15)
- Nie potrzebujemy ID_Produktu!
- **Wniosek:** Data_Zamowienia zależy **częściowo** od klucza ✗

**Krok 4: Identyfikacja problemów**

**Problem 1: Redundancja danych**
- "Laptop" i "5000" powtarzają się w wierszach 1 i 2
- Marnowanie miejsca w bazie

**Problem 2: Anomalia aktualizacji**
- Jeśli zmienimy cenę Laptopa z 5000 na 5500
- Musimy zaktualizować **wszystkie wiersze** z produktem P1 (wiersze 1 i 2)
- Jeśli zapomnimy jednego → dane niespójne!

**Problem 3: Anomalia wstawiania**
- Nie można dodać nowego produktu bez zamówienia!
- Musisz najpierw stworzyć zamówienie

**Problem 4: Anomalia usuwania**
- Jeśli usuniesz wszystkie zamówienia z produktem P1
- Możesz przypadkiem "stracić" informację o produkcie Laptop!

**PO (2NF) — normalizacja krok po kroku:**

**Krok 1: Wydzielenie tabeli "Zamowienia"**

Wydzielamy atrybuty zależne tylko od ID_Zamowienia:
- Data_Zamowienia zależy tylko od ID_Zamowienia

**Tabela "Zamowienia":**
| ID_Zamowienia | Data_Zamowienia |
|--------------|----------------|
| 1 | 2024-01-15 |
| 2 | 2024-01-16 |

**Krok 2: Wydzielenie tabeli "Produkty"**

Wydzielamy atrybuty zależne tylko od ID_Produktu:
- Nazwa_Produktu zależy tylko od ID_Produktu
- Cena_Produktu zależy tylko od ID_Produktu

**Tabela "Produkty":**
| ID_Produktu | Nazwa_Produktu | Cena_Produktu |
|------------|----------------|---------------|
| P1 | Laptop | 5000 |
| P2 | Mysz | 50 |

**Krok 3: Tabela "Pozycje_Zamowienia" (tylko pełne zależności)**

Zostawiamy tylko atrybuty zależne w pełni od klucza:
- Ilosc zależy w pełni od {ID_Zamowienia, ID_Produktu}

**Tabela "Pozycje_Zamowienia":**
| ID_Zamowienia | ID_Produktu | Ilosc |
|--------------|-------------|-------|
| 1 | P1 | 2 |
| 1 | P2 | 3 |
| 2 | P1 | 1 |

**Krok 4: Analiza wynikowej struktury**

**Sprawdzenie 2NF:**
- ✅ Tabela "Zamowienia": klucz prosty (ID_Zamowienia) → automatycznie 2NF
- ✅ Tabela "Produkty": klucz prosty (ID_Produktu) → automatycznie 2NF
- ✅ Tabela "Pozycje_Zamowienia": 
  - Klucz: {ID_Zamowienia, ID_Produktu}
  - Atrybut niekluczowy: Ilosc
  - Ilosc zależy w pełni od klucza ✓

**Korzyści:**
- ✅ Brak redundancji — dane o produkcie w jednym miejscu
- ✅ Łatwa aktualizacja — zmiana ceny = zmiana w jednym miejscu
- ✅ Można dodać produkt bez zamówienia
- ✅ Usunięcie zamówienia nie usuwa produktu

**Relacje:**
- Pozycje_Zamowienia.ID_Zamowienia → Zamowienia.ID_Zamowienia (klucz obcy)
- Pozycje_Zamowienia.ID_Produktu → Produkty.ID_Produktu (klucz obcy)

---

## 8) Trzecia forma normalna (3NF) — szczegółowo

### Definicja — intuicyjnie

**3NF** = 2NF + **brak zależności przechodnich**.

**Intuicja:** Atrybuty niekluczowe muszą zależeć **tylko od klucza**, nie od innych atrybutów niekluczowych.

**Przykład z życia:**
- Tabela: Pracownicy (ID_Pracownika, Dzial, Nazwa_Dzialu)
- Problem: Nazwa_Dzialu zależy od Dzial (niekluczowy), nie od klucza ID_Pracownika
- Rozwiązanie: Wydzielić tabelę Dzialy

### Definicja formalna

Tabela jest w **3NF**, jeśli:
1. ✅ Jest w **2NF**
2. ✅ **Nie ma zależności przechodnich** — żaden atrybut niekluczowy nie zależy od innego atrybutu niekluczowego

### Warunek formalny

Dla każdej zależności funkcyjnej A → B, gdzie B jest atrybutem niekluczowym:
- A jest **nadkluczem** (superkey), LUB
- B jest **częścią klucza**

**Innymi słowy:** Atrybuty niekluczowe muszą zależeć **tylko od klucza**, nie od innych atrybutów niekluczowych.

### Przykład naruszenia 3NF — szczegółowa analiza krok po kroku:

**PRZED (złe — narusza 3NF, ale jest w 2NF):**

Tabela "Pracownicy":
| ID_Pracownika | Imie | ID_Dzialu | Nazwa_Dzialu | Miasto_Dzialu |
|--------------|------|-----------|--------------|---------------|
| 1 | Jan | D1 | IT | Warszawa |
| 2 | Anna | D1 | IT | Warszawa |
| 3 | Piotr | D2 | HR | Kraków |

**Krok 1: Identyfikacja klucza głównego**

**Klucz główny:** ID_Pracownika
- Prosty klucz (nie złożony)
- Jednoznacznie identyfikuje każdego pracownika

**Krok 2: Identyfikacja atrybutów niekluczowych**

Atrybuty niekluczowe (nie są częścią klucza):
- Imie
- ID_Dzialu
- Nazwa_Dzialu
- Miasto_Dzialu

**Krok 3: Analiza zależności funkcyjnych**

**1. ID_Pracownika → Imie**
- ID = 1 → Imie = "Jan" ✓
- **Wniosek:** Zależność bezpośrednia od klucza ✓

**2. ID_Pracownika → ID_Dzialu**
- ID = 1 → ID_Dzialu = D1 ✓
- **Wniosek:** Zależność bezpośrednia od klucza ✓

**3. ID_Pracownika → Nazwa_Dzialu**
- ID = 1 → Nazwa_Dzialu = "IT" ✓
- **ALE:** Jak to działa?
  - ID = 1 → ID_Dzialu = D1
  - ID_Dzialu = D1 → Nazwa_Dzialu = "IT"
  - **Wniosek:** Zależność **przechodnia** (przez ID_Dzialu) ✗

**4. ID_Pracownika → Miasto_Dzialu**
- ID = 1 → Miasto_Dzialu = "Warszawa" ✓
- **ALE:** Jak to działa?
  - ID = 1 → ID_Dzialu = D1
  - ID_Dzialu = D1 → Miasto_Dzialu = "Warszawa"
  - **Wniosek:** Zależność **przechodnia** (przez ID_Dzialu) ✗

**5. ID_Dzialu → Nazwa_Dzialu**
- D1 → "IT" ✓
- D2 → "HR" ✓
- **Wniosek:** Zależność funkcyjna ✓

**6. ID_Dzialu → Miasto_Dzialu**
- D1 → "Warszawa" ✓
- D2 → "Kraków" ✓
- **Wniosek:** Zależność funkcyjna ✓

**Krok 4: Identyfikacja zależności przechodnich**

**Łańcuch zależności:**
```
ID_Pracownika → ID_Dzialu → Nazwa_Dzialu
ID_Pracownika → ID_Dzialu → Miasto_Dzialu
```

**Problem:**
- Nazwa_Dzialu i Miasto_Dzialu zależą od ID_Dzialu (atrybut niekluczowy)
- Nie zależą bezpośrednio od klucza ID_Pracownika
- To jest **zależność przechodnia**!

**Krok 5: Identyfikacja problemów**

**Problem 1: Redundancja danych**
- "IT" i "Warszawa" powtarzają się w wierszach 1 i 2
- Marnowanie miejsca w bazie

**Problem 2: Anomalia aktualizacji**
- Jeśli zmienimy nazwę działu D1 z "IT" na "Informatyka"
- Musimy zaktualizować **wszystkie wiersze** z ID_Dzialu = D1 (wiersze 1 i 2)
- Jeśli zapomnimy jednego → dane niespójne!

**Problem 3: Anomalia wstawiania**
- Nie można dodać nowego działu bez pracownika!
- Musisz najpierw dodać pracownika

**Problem 4: Anomalia usuwania**
- Jeśli usuniesz wszystkich pracowników z działu D1
- Możesz przypadkiem "stracić" informację o dziale IT!

**PO (3NF) — normalizacja krok po kroku:**

**Krok 1: Wydzielenie tabeli "Dzialy"**

Wydzielamy atrybuty zależne od ID_Dzialu:
- Nazwa_Dzialu zależy od ID_Dzialu
- Miasto_Dzialu zależy od ID_Dzialu

**Tabela "Dzialy":**
| ID_Dzialu | Nazwa_Dzialu | Miasto_Dzialu |
|-----------|--------------|---------------|
| D1 | IT | Warszawa |
| D2 | HR | Kraków |

**Krok 2: Tabela "Pracownicy" (tylko bezpośrednie zależności)**

Zostawiamy tylko atrybuty zależne bezpośrednio od klucza:
- Imie zależy bezpośrednio od ID_Pracownika
- ID_Dzialu zależy bezpośrednio od ID_Pracownika (klucz obcy)

**Tabela "Pracownicy":**
| ID_Pracownika | Imie | ID_Dzialu |
|--------------|------|-----------|
| 1 | Jan | D1 |
| 2 | Anna | D1 |
| 3 | Piotr | D2 |

**Krok 3: Analiza wynikowej struktury**

**Sprawdzenie 3NF:**
- ✅ Tabela "Dzialy": 
  - Klucz: ID_Dzialu
  - Wszystkie atrybuty zależą bezpośrednio od klucza ✓
  
- ✅ Tabela "Pracownicy":
  - Klucz: ID_Pracownika
  - Imie zależy bezpośrednio od klucza ✓
  - ID_Dzialu zależy bezpośrednio od klucza ✓
  - Brak zależności przechodnich ✓

**Korzyści:**
- ✅ Brak redundancji — dane o dziale w jednym miejscu
- ✅ Łatwa aktualizacja — zmiana nazwy działu = zmiana w jednym miejscu
- ✅ Można dodać dział bez pracownika
- ✅ Usunięcie pracowników nie usuwa działu

**Relacja:**
- Pracownicy.ID_Dzialu → Dzialy.ID_Dzialu (klucz obcy)

**Wizualizacja:**
```
Tabela Dzialy          Tabela Pracownicy
┌─────────────┐        ┌──────────────────┐
│ ID_Dzialu   │←───────│ ID_Dzialu (FK)   │
│ Nazwa       │        │ ID_Pracownika    │
│ Miasto      │        │ Imie             │
└─────────────┘        └──────────────────┘
```

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

