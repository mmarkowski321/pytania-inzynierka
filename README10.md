## Fizyczne nośniki danych – technologie, struktury oraz metody kodowania informacji  

### 0) Podstawy – o co chodzi w tym pytaniu?  

- **Fizyczny nośnik danych** – realny „przedmiot”, na którym zapisujemy bity: dysk HDD, SSD, pendrive, płyta CD/DVD, taśma magnetyczna itd.  
- **Technologia zapisu** – JAK fizycznie reprezentowany jest bit (np. namagnesowanie, ładunek elektryczny, struktura komórki pamięci).  
- **Struktura danych** – W JAKI SPOSÓB bity są zorganizowane logicznie na nośniku (sektory, ścieżki, bloki, strony).  
- **Metody kodowania informacji** – W JAKI SPOSÓB „0” i „1” są reprezentowane w czasie/przestrzeni, żeby można je było poprawnie odczytać (np. NRZ, RZ, Manchester, 8b/10b) oraz jak chronimy się przed błędami (kody korekcyjne, parzystość, ECC).  

To pytanie lubi być zadawane ogólnie – warto pokazać, że rozróżniasz **nośnik fizyczny**, jego **strukturę logiczną** oraz **kodowanie bitów**.  

---

### 1) Przegląd głównych typów fizycznych nośników danych  

#### 1.1. Nośniki magnetyczne  

- **HDD (Hard Disk Drive)**  
  - Dane zapisywane przez **zmianę namagnesowania** mikroskopijnych obszarów na talerzach.  
  - Talerze obracają się z prędkością np. 5400, 7200, 10000 RPM.  
  - Głowica zmienia/odczytuje namagnesowanie bez dotykania powierzchni.  
  - Zapis: „0” i „1” reprezentowane jako **zmiana kierunku pola magnetycznego** wzdłuż ścieżki.  
  - **Cechy:** duża pojemność, tańszy GB, wolniejszy dostęp losowy niż SSD, elementy ruchome.  

- **Taśmy magnetyczne**  
  - Stosowane głównie w archiwizacji backupów (serwerownie).  
  - Dane zapisane jednowymiarowo wzdłuż taśmy.  
  - Bardzo duża pojemność, **niski koszt za GB**, ale **długi czas dostępu** (trzeba „przewinąć”).  

#### 1.2. Nośniki optyczne  

- **CD / DVD / Blu-ray**  
  - Dane zapisane jako **wypalone (pit)** i **niewypalone (land)** miejsca na ścieżce spiralnej.  
  - Odczyt za pomocą wiązki laserowej – zmiana odbicia światła interpretowana jako 0/1.  
  - CD ~700 MB, DVD 4,7–8,5 GB, Blu-ray 25–50 GB (i więcej).  
  - Zaletą jest dobra trwałość (przy dobrym przechowywaniu), wadą mała pojemność w porównaniu z HDD/SSD.  

#### 1.3. Nośniki półprzewodnikowe (flash, SSD)  

- **SSD (Solid State Drive)**, pendrive, karta SD  
  - Dane zapisane w **komórkach pamięci flash**, które przechowują ładunek elektryczny.  
  - Bit „0”/„1” reprezentowany jako różny poziom ładunku (próg przewodzenia tranzystora).  
  - Typy komórek:  
    - **SLC** (Single-Level Cell) – 1 bit na komórkę (0/1), trwałe, szybkie, drogie.  
    - **MLC** (Multi-Level Cell) – 2 bity na komórkę.  
    - **TLC** (Triple-Level Cell) – 3 bity na komórkę.  
    - **QLC** (Quad-Level Cell) – 4 bity na komórkę, większa gęstość, mniejsza trwałość.  
  - **Cechy:** brak elementów ruchomych, wysoka prędkość odczytu/zapisu, ograniczona liczba cykli zapisu komórek (wear-out).  

#### 1.4. Inne nośniki (ogólnie)  

- **Pamięci RAM** – również zapis elektryczny, ale ulotny (po wyłączeniu zasilania dane znikają).  
- **Nośniki hybrydowe (SSHD)** – kombinacja HDD + SSD (bufor).  

---

### 2) Struktury danych na nośnikach – ścieżki, sektory, bloki  

#### 2.1. Struktura klasycznego dysku HDD  

- Dysk ma jedną lub więcej **talerzy**.  
- Na talerzach są **ścieżki** (okręgi współśrodkowe).  
- Ścieżki podzielone są na **sektory** – najmniejsze jednostki zapisu na poziomie fizycznym (np. 512 B lub 4096 B).  
- Zestaw ścieżek na tej samej wysokości (na wszystkich talerzach) tworzy **cylinder**.  

**Przykład:**  
- Jeden sektor = 512 bajtów  
- Dany plik może zajmować np. 10 sektorów = 10 × 512 = 5120 bajtów  

#### 2.2. Struktury logiczne systemu plików  

- System plików (np. NTFS, ext4, FAT32) operuje na **klastrach / blokach** – logicznych jednostkach zawierających jeden lub więcej sektorów.  
- Przykład: blok 4 KB = 8 sektorów po 512 B.  
- System plików zarządza:  
  - tablicą alokacji (które bloki są wolne, a które zajęte),  
  - metadanymi (nazwa, czas utworzenia, prawa dostępu),  
  - strukturami katalogów.  

**Ważne:** egzaminatorzy często chcą usłyszeć, że **fizyczna struktura nośnika** (sektory, ścieżki) to co innego niż **struktura logiczna** (system plików, katalogi, pliki).  

#### 2.3. Struktura SSD  

- SSD nie ma ścieżek i sektorów w klasycznym sensie, ale **strony (pages)** i **bloki (blocks)** pamięci flash.  
- Zapis odbywa się stronami (np. 4 KB), kasowanie – całymi blokami (np. 128 stron).  
- Kontroler SSD tłumaczy adresy logiczne (LBA) na fizyczne położenie komórek – tzw. **Flash Translation Layer (FTL)**.  
- Aby równomiernie zużywać komórki, używany jest mechanizm **wear leveling**.  

---

### 3) Metody kodowania informacji na nośnikach  

> Chodzi o to, JAK reprezentowane są 0 i 1 na poziomie sygnału / pola magnetycznego / optycznego, tak aby można było je poprawnie zsynchronizować i odczytać.  

#### 3.1. Kodowanie magnetyczne na HDD (przykładowe)  

- **NRZ (Non-Return to Zero)** – poziom sygnału reprezentuje 0/1, ale ma problem z długimi ciągami tych samych bitów (trudno odtworzyć zegar).  
- **RLL (Run Length Limited)** – ogranicza długość serii jednakowych bitów, zapewnia odpowiednią liczbę zmian sygnału (lepsza synchronizacja).  
- **PRML (Partial Response, Maximum Likelihood)** – zaawansowane techniki przetwarzania sygnału, pozwalające zwiększyć gęstość zapisu.  

**Idea:**  
- Nie jest zapisywane „surowe 010101…”, ale przekształcony sygnał, który zapewnia:  
  - minimum zmian (by nie marnować miejsca),  
  - ale też wystarczająco dużo zmian, aby odbiornik mógł zsynchronizować zegar.  

#### 3.2. Kodowanie na nośnikach optycznych  

- W standardach CD/DVD stosowane jest m.in. **EFM (Eight-to-Fourteen Modulation)**:  
  - Każdy bajt (8 bitów) jest zamieniany na 14-bitowe słowo kodowe.  
  - Pomiędzy słowami dodawane są bity synchronizujące.  
  - Kod zapewnia minimalną i maksymalną długość „serii” bez zmiany, co ułatwia korekcję błędów i synchronizację.  

#### 3.3. Kodowanie linii w transmisji (dla pełności obrazu)  

Chociaż dotyczy głównie **transmisji**, warto wspomnieć, bo może się pojawić:  

- **Manchester** – każda „1” i „0” reprezentowana przez zmianę poziomu w połowie okresu bitu (daje łatwą synchronizację).  
- **NRZI (Non-Return to Zero Inverted)** – zmiana poziomu oznacza „1”, brak zmiany – „0”.  
- **8b/10b** – 8 bitów danych zamieniane na 10 bitów kodu, by zapewnić równowagę liczby jedynek/zer i odpowiednią liczbę przejść.  

---

### 4) Ochrona danych: kody kontrolne i korekcja błędów (ECC)  

#### 4.1. Kody detekcji błędów  

- **Bit parzystości**  
  - Do danych dodaje się dodatkowy bit, aby liczba 1 w słowie była parzysta (parzystość parzysta) lub nieparzysta.  
  - Pozwala wykryć niektóre błędy pojedynczych bitów.  

- **CRC (Cyclic Redundancy Check)**  
  - Dane traktowane są jak liczba i dzielone przez wielomian generujący; reszta z dzielenia to kod CRC.  
  - Umożliwia wykrywanie wielu kombinacji błędów, stosowany np. w sieciach, dyskach, protokołach transmisji.  

#### 4.2. Kody korekcji błędów (ECC – Error Correcting Code)  

- **Kod Hamminga**  
  - Pozwala wykrywać i **korygować** błędy pojedynczych bitów w słowie.  
  - Wykorzystywany w pamięciach **ECC RAM** oraz dyskach.  

- **Zaawansowane kody ECC w SSD i HDD**  
  - Nowoczesne dyski (szczególnie SSD) stosują złożone kody (Reed-Solomon, LDPC), które pozwalają **automatycznie naprawiać** błędy wynikające z zużycia nośnika.  

**Wnioski:**  
- Fizyczny nośnik jest zawodny (szum, zużycie, zarysowania, promieniowanie kosmiczne),  
- dlatego dane nie są przechowywane „goło”, ale wraz z **dodatkowymi bitami kontrolnymi**, które umożliwiają wykrycie i często naprawę błędów.  

---

### 5) Podsumowanie różnic – nośnik vs struktura vs kodowanie  

- **Nośnik fizyczny (technologia)**  
  - Magnetyczny (HDD, taśmy) – namagnesowanie powierzchni.  
  - Optyczny (CD/DVD/BD) – odbicie światła od pit/land.  
  - Półprzewodnikowy (SSD, flash) – ładunek elektryczny w komórce.  

- **Struktura danych na nośniku**  
  - HDD: ścieżki, sektory, cylindry.  
  - SSD: strony, bloki, warstwa FTL.  
  - Z punktu widzenia systemu operacyjnego: bloki/klastry, pliki, katalogi.  

- **Metody kodowania informacji**  
  - Kodowanie magnetyczne/efm/8b10b – jak fizycznie reprezentowane są 0/1.  
  - Kody kontrolne i ECC – jak wykrywamy i naprawiamy błędy w przechowywanych danych.  

---

### 6) Kluczowe zdania na obronę  

1. **Fizyczny nośnik danych** to realne urządzenie (HDD, SSD, CD/DVD, taśma), na którym bity są reprezentowane jako namagnesowanie, odbicie światła lub ładunek elektryczny.  
2. W dysku HDD dane zapisuje się na obracających się talerzach, w postaci zmiany namagnesowania na ścieżkach i sektorach; w SSD dane zapisane są w komórkach pamięci flash jako różne poziomy ładunku.  
3. **Struktura fizyczna** (sektory, ścieżki, strony, bloki) jest inna niż **struktura logiczna** (system plików, katalogi, pliki), którą widzi użytkownik.  
4. Metody kodowania (np. NRZ, RLL, EFM, 8b/10b) określają, jak „0” i „1” są zamieniane na sygnał fizyczny tak, żeby możliwa była poprawna synchronizacja i odczyt danych.  
5. Do wykrywania i korekcji błędów stosuje się **kody kontrolne i ECC** (np. parzystość, CRC, kod Hamminga, LDPC), co pozwala odróżnić poprawne dane od uszkodzonych i często je naprawić.  
6. Nośniki magnetyczne oferują dużą pojemność za niską cenę, nośniki półprzewodnikowe (SSD) – bardzo szybki dostęp bez części ruchomych, a nośniki optyczne – dobrą trwałość i przenośność.  
7. W nowoczesnych SSD kontroler stosuje **wear leveling** oraz złożone kody ECC, aby zrównoważyć zużycie komórek flash i wydłużyć żywotność nośnika.  

