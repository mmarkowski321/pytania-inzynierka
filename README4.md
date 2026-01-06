# Model Warstwowy TCP/IP — Kompletna Notatka do Obrony

## 0) Podstawy — od zera (dla początkujących)

### Co to jest sieć komputerowa?

**Sieć komputerowa** = połączenie komputerów, które mogą się ze sobą komunikować.

**Przykład z życia:**
- Internet — globalna sieć łącząca miliony komputerów
- Sieć domowa (Wi-Fi) — kilka urządzeń w domu
- Sieć firmowa — komputery w biurze

**Dlaczego sieci?**
- Wymiana danych między komputerami
- Współdzielenie zasobów (drukarki, pliki)
- Komunikacja (e-mail, czat, video)
- Dostęp do Internetu

### Jak komputery się komunikują?

**Problem:** Komputer A chce wysłać dane do komputera B.

**Co jest potrzebne?**
1. **Medium transmisyjne** — kabel lub fale radiowe
2. **Protokoły** — zasady komunikacji (jak mówić, w jakim języku)
3. **Adresy** — jak znaleźć komputer B w sieci

**Przykład analogii:**
- **Medium** = droga (kabel = asfalt, Wi-Fi = powietrze)
- **Protokół** = język i zasady (jak mówić, kiedy mówić)
- **Adres** = adres domu (gdzie jest komputer B)

### Co to jest protokół?

**Protokół** = zestaw zasad i reguł, które określają, jak komputery komunikują się ze sobą.

**Przykład z życia:**
- Protokół rozmowy telefonicznej:
  1. Podnieś słuchawkę
  2. Wybierz numer
  3. Poczekaj na połączenie
  4. Powiedz "Halo"
  5. Rozmawiaj
  6. Pożegnaj się
  7. Rozłącz

**W komputerach:** Protokoły określają:
- Format danych (jak są zapisane)
- Kolejność wysyłania
- Jak wykrywać błędy
- Jak potwierdzać odbiór

### Co to jest model warstwowy?

**Model warstwowy** = podział komunikacji na warstwy, gdzie każda warstwa ma swoją odpowiedzialność.

**Intuicja — analogia z pocztą:**

Wysyłanie listu przez pocztę:
1. **Warstwa aplikacji** — napiszesz list (treść)
2. **Warstwa transportowa** — włożysz do koperty, napiszesz adres
3. **Warstwa sieciowa** — poczta decyduje, którą trasą wysłać
4. **Warstwa łącza** — list trafia do odpowiedniego oddziału
5. **Warstwa fizyczna** — list jest fizycznie transportowany (samochód, samolot)

**Każda warstwa:**
- Ma swoją odpowiedzialność
- Nie musi wiedzieć, jak działają inne warstwy
- Przekazuje dane do warstwy niżej/wyżej

**Korzyści:**
- Łatwiejsze zrozumienie (każda warstwa osobno)
- Łatwiejsze naprawianie (problem w jednej warstwie)
- Możliwość zmiany jednej warstwy bez zmiany innych

### Co to jest enkapsulacja?

**Enkapsulacja** = pakowanie danych przez warstwy (dodawanie nagłówków).

**Przykład z życia — list w kopercie:**
1. Napiszesz list (dane)
2. Wkładasz do koperty (dodajesz nagłówek z adresem)
3. Koperta trafia do paczki (dodajesz kolejny nagłówek)
4. Paczka trafia do samolotu (dodajesz kolejny nagłówek)

**W komputerze:**
1. Aplikacja tworzy dane: "Hello"
2. Transport dodaje nagłówek TCP: [TCP Header | "Hello"]
3. Sieć dodaje nagłówek IP: [IP Header | TCP Header | "Hello"]
4. Łącze dodaje nagłówek Ethernet: [Ethernet | IP | TCP | "Hello"]

**Dekapsulacja** = rozpakowywanie (usuwanie nagłówków) w odwrotnej kolejności.

### Co to jest adres IP?

**Adres IP** = unikalny identyfikator komputera w sieci (jak adres domu).

**Format IPv4 (wersja 4):**
- 32 bity = 4 bajty
- Zapis: 192.168.1.100 (4 liczby od 0 do 255)
- Przykład: 192.168.1.1, 8.8.8.8 (Google DNS)

**Format IPv6 (wersja 6):**
- 128 bitów = 16 bajtów
- Zapis: 2001:0db8:85a3::8a2e:0370:7334
- Więcej adresów (prawie nieskończoność)

**Przykład:**
- Twój komputer: 192.168.1.100
- Router: 192.168.1.1
- Google: 8.8.8.8

### Co to jest port?

**Port** = identyfikator aplikacji na komputerze (jak numer pokoju w hotelu).

**Przykład:**
- Komputer = hotel (adres IP)
- Aplikacja = pokój (port)
- Port 80 = HTTP (strony WWW)
- Port 443 = HTTPS (bezpieczne strony)
- Port 25 = SMTP (e-mail)

**Zakres:** 0-65535
- 0-1023: Well-known (zarezerwowane)
- 1024-49151: Registered
- 49152-65535: Dynamic (losowe dla klientów)

**Przykład:**
- Komputer: 192.168.1.100
- Port: 80
- Pełny adres: 192.168.1.100:80

### Co to jest MAC address?

**MAC address** = fizyczny adres karty sieciowej (jak numer seryjny).

**Format:**
- 48 bitów = 6 bajtów
- Zapis: AA:BB:CC:DD:EE:FF (hex)
- Przykład: 00:1A:2B:3C:4D:5E

**Cechy:**
- Unikalny na całym świecie
- Przypisany przez producenta
- Nie można zmienić (hardware)

**Różnica IP vs MAC:**
- **IP** = logiczny adres (może się zmieniać, jak adres zamieszkania)
- **MAC** = fizyczny adres (zawsze ten sam, jak numer seryjny)

---

## 1) Wprowadzenie — modele warstwowe w sieciach

### Dlaczego modele warstwowe?
Sieci komputerowe są złożone. **Model warstwowy** (layered model) upraszcza projektowanie i zrozumienie przez:
- **Separację odpowiedzialności** — każda warstwa ma określoną funkcję
- **Niezależność implementacji** — warstwy mogą być modyfikowane niezależnie
- **Standardyzację** — protokoły mogą współpracować między różnymi systemami
- **Modularność** — łatwiejsze zarządzanie i debugowanie

**Przykład z życia:**
- **Warstwa fizyczna** = droga (jak jechać)
- **Warstwa łącza** = znaki drogowe (gdzie skręcić)
- **Warstwa sieci** = mapa (która trasa)
- **Warstwa transport** = kurier (dostarcza paczkę)
- **Warstwa aplikacji** = treść paczki (co jest w środku)

### Podstawowa zasada:
> Każda warstwa używa usług warstwy niższej i zapewnia usługi warstwie wyższej.

**Wizualizacja:**
```
Warstwa 5 (Aplikacji)
    ↓ używa usług
Warstwa 4 (Transportowa)
    ↓ używa usług
Warstwa 3 (Internetowa)
    ↓ używa usług
Warstwa 2 (Łącza danych)
    ↓ używa usług
Warstwa 1 (Fizyczna)
```

---

## 2) Model OSI vs Model TCP/IP

### Model OSI (Open Systems Interconnection)
7-warstwowy model referencyjny:

```
7. Warstwa aplikacji      (Application Layer)
6. Warstwa prezentacji    (Presentation Layer)
5. Warstwa sesji          (Session Layer)
4. Warstwa transportowa   (Transport Layer)
3. Warstwa sieciowa       (Network Layer)
2. Warstwa łącza danych   (Data Link Layer)
1. Warstwa fizyczna       (Physical Layer)
```

### Model TCP/IP (4 lub 5 warstw)
Praktyczny model używany w Internecie:

**Wersja 4-warstwowa:**
```
4. Warstwa aplikacji      (Application Layer)
3. Warstwa transportowa   (Transport Layer)
2. Warstwa internetowa    (Internet Layer)
1. Warstwa dostępu do sieci (Network Access Layer)
```

**Wersja 5-warstwowa (częściej używana w nauczaniu):**
```
5. Warstwa aplikacji      (Application Layer)
4. Warstwa transportowa   (Transport Layer)
3. Warstwa internetowa    (Internet Layer)
2. Warstwa łącza danych   (Data Link Layer)
1. Warstwa fizyczna       (Physical Layer)
```

### Porównanie:

| Warstwa OSI | Warstwa TCP/IP (5-warstwowy) | Główne protokoły |
|-------------|------------------------------|------------------|
| 7. Aplikacji | 5. Aplikacji | HTTP, HTTPS, FTP, SMTP, DNS, DHCP |
| 6. Prezentacji | 5. Aplikacji | (włączona w aplikacji) |
| 5. Sesji | 5. Aplikacji | (włączona w aplikacji) |
| 4. Transportowa | 4. Transportowa | TCP, UDP |
| 3. Sieciowa | 3. Internetowa | IP, ICMP, ARP |
| 2. Łącza danych | 2. Łącza danych | Ethernet, Wi-Fi, PPP |
| 1. Fizyczna | 1. Fizyczna | Kable, sygnały radiowe |

**Uwaga:** Model TCP/IP jest **praktyczny** (używany w Internecie), OSI jest **teoretyczny** (model referencyjny).

---

## 3) Warstwa 1: Fizyczna (Physical Layer) — szczegółowo

### Definicja — intuicyjnie

**Warstwa fizyczna** = najniższa warstwa, która zajmuje się **fizycznym przesyłaniem bitów** przez medium.

**Intuicja:** To jak droga — nie interesuje się, co wieziesz, tylko jak to fizycznie przesłać.

**Przykład z życia:**
- Chcesz wysłać list
- Warstwa fizyczna = samochód, który fizycznie wiezie list
- Nie interesuje się treścią listu, tylko jak go przewieźć

### Zadania — szczegółowo:

**1. Przesyłanie bitów przez medium fizyczne**
- Bit 0 i 1 muszą być zamienione na sygnały fizyczne
- Przykład: 0 = brak napięcia, 1 = napięcie 5V

**2. Definiowanie charakterystyk elektrycznych**
- Jakie napięcie oznacza 0, a jakie 1?
- Jaka częstotliwość sygnału?
- Jak szybko można przesyłać?

**3. Kodowanie danych**
- 0 i 1 → sygnały fizyczne (elektryczne, świetlne, radiowe)

### Funkcje — szczegółowo:

#### 1. Definiowanie sygnałów

**Przykład — kabel miedziany:**
- Bit 0 = napięcie 0V
- Bit 1 = napięcie +5V
- Częstotliwość: np. 100 MHz (100 milionów bitów na sekundę)

**Przykład — światłowód:**
- Bit 0 = brak światła
- Bit 1 = impuls światła
- Częstotliwość: bardzo wysoka (miliardy bitów na sekundę)

#### 2. Synchronizacja bitowa

**Problem:** Jak odbiorca wie, kiedy zaczyna się nowy bit?

**Rozwiązanie:** Synchronizacja — odbiorca musi wiedzieć, kiedy próbkować sygnał.

**Przykład:**
```
Nadawca wysyła: 1 0 1 1 0
Czas:          | | | | |
               t t t t t
               
Odbiorca próbkuje w momentach t, żeby odczytać bity
```

#### 3. Topologia fizyczna

**Topologia** = jak komputery są fizycznie połączone.

**Gwiazda (Star):**
```
      Komputer 1
           |
      Komputer 2
           |
      Switch/Hub ← Centrum
           |
      Komputer 3
           |
      Komputer 4
```
- Wszystkie komputery podłączone do centralnego urządzenia
- Jeśli jedno urządzenie padnie, reszta działa

**Magistrala (Bus):**
```
Komputer 1 ── Komputer 2 ── Komputer 3 ── Komputer 4
    |              |              |              |
    └──────────────┴──────────────┴──────────────┘
                    Kabel (magistrala)
```
- Wszystkie komputery na jednym kablu
- Jeśli kabel padnie, cała sieć nie działa

**Pierścień (Ring):**
```
Komputer 1 ──→ Komputer 2 ──→ Komputer 3 ──→ Komputer 4 ──┐
     ↑                                                      │
     └──────────────────────────────────────────────────────┘
```
- Komputery połączone w pierścień
- Dane krążą w jednym kierunku

#### 4. Sposób transmisji

**Simplex** — tylko w jedną stronę:
- Przykład: radio (nadajnik → odbiornik)
- Komputer A → Komputer B (tylko)

**Half-duplex** — w obie strony, ale nie jednocześnie:
- Przykład: walkie-talkie (mówisz lub słuchasz)
- Komputer A ↔ Komputer B (na zmianę)

**Full-duplex** — w obie strony jednocześnie:
- Przykład: telefon (mówisz i słuchasz jednocześnie)
- Komputer A ↔ Komputer B (jednocześnie)

### Media transmisyjne — szczegółowo:

#### Przewodowe:

**1. Kabel skrętka (Twisted Pair)**
- Dwa przewody skręcone razem (zmniejsza zakłócenia)
- **Kategoria 5 (Cat5):** do 100 Mbps, do 100m
- **Kategoria 6 (Cat6):** do 1 Gbps, do 100m
- **Użycie:** Ethernet w domach, biurach

**2. Kabel koncentryczny (Coaxial)**
- Jeden przewód centralny, ekran zewnętrzny
- **Użycie:** TV kablowa, starsze sieci Ethernet
- Dłuższe odległości niż skrętka

**3. Światłowód (Fiber-optic)**
- Światło zamiast prądu
- **Zalety:** bardzo szybki (terabity), długie odległości, odporny na zakłócenia
- **Wady:** drogi, trudny w naprawie
- **Użycie:** Internet światłowodowy, sieci między miastami

#### Bezprzewodowe:

**1. Fale radiowe (Wi-Fi)**
- Częstotliwość: 2.4 GHz lub 5 GHz
- **Zasięg:** ~50m w pomieszczeniu
- **Użycie:** Wi-Fi w domach, biurach

**2. Mikrofale**
- Bardzo wysokie częstotliwości
- **Użycie:** Łącza między wieżami, satelity

**3. Podczerwień (Infrared)**
- Krótki zasięg, wymaga linii widzenia
- **Użycie:** Piloty TV, starsze laptopy

### Przykłady protokołów warstwy fizycznej:

**Ethernet (kabel):**
- Standard: IEEE 802.3
- Prędkość: 10 Mbps, 100 Mbps, 1 Gbps, 10 Gbps
- Medium: kabel skrętka lub światłowód

**Wi-Fi:**
- Standard: IEEE 802.11
- Wersje: 802.11n (Wi-Fi 4), 802.11ac (Wi-Fi 5), 802.11ax (Wi-Fi 6)
- Medium: fale radiowe

**DSL (Digital Subscriber Line):**
- Używa linii telefonicznej
- Prędkość: do ~100 Mbps
- Medium: kabel telefoniczny

### Jednostka danych:
**Bit** (0 lub 1) — najmniejsza jednostka informacji

**Przykład transmisji:**
```
Nadawca: 1 0 1 1 0 1 0 0
         ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓
Medium:  [sygnały fizyczne]
         ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓
Odbiorca: 1 0 1 1 0 1 0 0
```

---

## 4) Warstwa 2: Łącza Danych (Data Link Layer) — szczegółowo

### Definicja — intuicyjnie

**Warstwa łącza danych** = organizuje bity w ramki i zapewnia komunikację w sieci lokalnej.

**Intuicja:** To jak listonosz — wie, gdzie dostarczyć list w obrębie miasta (sieci lokalnej), ale nie wie, jak dostarczyć do innego miasta.

**Przykład z życia:**
- Warstwa fizyczna = samochód (fizycznie wiezie)
- Warstwa łącza = listonosz (wie, gdzie w mieście dostarczyć)
- Sieć lokalna = miasto (komputery w jednej sieci)

### Zadania — szczegółowo:

**1. Komunikacja między węzłami w tej samej sieci lokalnej**
- Komputery w jednej sieci (np. w domu, biurze)
- Komunikacja bezpośrednia (bez routera)

**2. Tworzenie ramek (frames) z bitów**
- Bity z warstwy fizycznej → ramki (logiczne jednostki)
- Ramka = nagłówek + dane + stopka

**3. Wykrywanie i korekcja błędów**
- CRC (Cyclic Redundancy Check) — sprawdza, czy dane nie zostały uszkodzone
- Jeśli błąd → prośba o retransmisję

**4. Kontrola dostępu do medium**
- Kto może wysyłać, kiedy?
- CSMA/CD (Ethernet) — sprawdza, czy medium jest wolne

**5. Adresowanie fizyczne (MAC address)**
- Każda karta sieciowa ma unikalny adres MAC
- Używany do dostarczania ramek w sieci lokalnej

### Funkcje:
- **Framing** — pakowanie bitów w ramki
- **Error detection** — wykrywanie błędów (CRC, checksum)
- **Error correction** — korekcja błędów (w niektórych protokołach)
- **Flow control** — kontrola przepływu (dla punkt-punkt)
- **Access control** — kontrolowanie dostępu do medium współdzielonego

### Podwarstwy (w Ethernet):

#### LLC (Logical Link Control)
- Zapewnia interfejs do warstwy wyższej
- Multiplexing protokołów

#### MAC (Media Access Control)
- Adresowanie fizyczne (MAC address)
- Kontrola dostępu do medium
- Wykrywanie kolizji (CSMA/CD w Ethernet)

### Adres MAC (Media Access Control)
- **Format:** 48 bitów (6 bajtów) zapisane jako hex: `AA:BB:CC:DD:EE:FF`
- **Unikalny** na całym świecie (przypisywany przez producenta)
- **Przykład:** `00:1A:2B:3C:4D:5E`

### Format ramki Ethernet — szczegółowa analiza:

**Struktura ramki Ethernet:**

```
| Preamble | Destination MAC | Source MAC | Type/Length | Data (46-1500 bajtów) | CRC |
| 7 bajtów | 6 bajtów        | 6 bajtów   | 2 bajty     | 46-1500 bajtów        | 4 bajty |
```

**Wyjaśnienie każdej części:**

**1. Preamble (7 bajtów)**
- **Cel:** Synchronizacja — pomaga odbiorcy zsynchronizować się z sygnałem
- **Zawartość:** Naprzemiennie 1 i 0 (10101010...)
- **Przykład:** 10101010 10101010 10101010 10101010 10101010 10101010 10101010

**2. Destination MAC (6 bajtów)**
- **Cel:** Adres MAC odbiorcy (gdzie dostarczyć ramkę)
- **Format:** AA:BB:CC:DD:EE:FF (hex)
- **Przykład:** 00:1A:2B:3C:4D:5E
- **Specjalne adresy:**
  - FF:FF:FF:FF:FF:FF = broadcast (wszystkie komputery w sieci)

**3. Source MAC (6 bajtów)**
- **Cel:** Adres MAC nadawcy (skąd ramka)
- **Format:** AA:BB:CC:DD:EE:FF (hex)
- **Przykład:** AA:BB:CC:DD:EE:FF

**4. Type/Length (2 bajty)**
- **Cel:** Określa typ danych (jaki protokół warstwy wyższej)
- **Przykłady:**
  - 0x0800 = IPv4
  - 0x86DD = IPv6
  - 0x0806 = ARP

**5. Data (46-1500 bajtów)**
- **Cel:** Dane z warstwy wyższej (pakiet IP)
- **Minimalna długość:** 46 bajtów (jeśli dane są krótsze, dodaje się padding)
- **Maksymalna długość:** 1500 bajtów (MTU - Maximum Transmission Unit)

**6. CRC (4 bajty)**
- **Cel:** Suma kontrolna — wykrywanie błędów
- **Jak działa:** Oblicza sumę kontrolną z całej ramki
- **Odbiorca:** Oblicza CRC i porównuje — jeśli różne → błąd, odrzuca ramkę

**Przykład kompletnej ramki:**

```
Preamble:     10101010 10101010 10101010 10101010 10101010 10101010 10101010
Dest MAC:    00:1A:2B:3C:4D:5E
Source MAC:   AA:BB:CC:DD:EE:FF
Type:         0x0800 (IPv4)
Data:         [pakiet IP z danymi]
CRC:         0x12345678
```

**Proces tworzenia ramki krok po kroku:**

1. **Warstwa wyższa (IP)** przekazuje pakiet do warstwy łącza
2. **Warstwa łącza** dodaje nagłówek Ethernet:
   - Sprawdza adres IP odbiorcy → znajduje adres MAC (przez ARP)
   - Dodaje Destination MAC
   - Dodaje Source MAC (swój adres)
   - Dodaje Type (0x0800 dla IPv4)
3. **Dodaje dane** (pakiet IP)
4. **Oblicza CRC** z całej ramki
5. **Dodaje Preamble** (synchronizacja)
6. **Przekazuje do warstwy fizycznej** (bity)

**Proces odbierania ramki krok po kroku:**

1. **Warstwa fizyczna** odbiera bity
2. **Warstwa łącza** odczytuje ramkę:
   - Sprawdza Preamble (synchronizacja)
   - Sprawdza Destination MAC (czy dla mnie?)
   - Sprawdza CRC (czy dane nie są uszkodzone?)
3. **Jeśli CRC OK i MAC OK:**
   - Usuwa nagłówek Ethernet
   - Sprawdza Type (np. 0x0800 = IPv4)
   - Przekazuje dane do warstwy IP
4. **Jeśli błąd:** Odrzuca ramkę (nie przekazuje dalej)

### Protokoły:
- **Ethernet** (najpopularniejszy w LAN)
- **Wi-Fi (802.11)** — bezprzewodowy
- **PPP** (Point-to-Point Protocol)
- **Frame Relay** (starszy)

### Jednostka danych:
**Ramka (Frame)**

### Obszar działania:
**Sieć lokalna (LAN)** — komunikacja w obrębie jednej sieci fizycznej

---

## 5) Warstwa 3: Internetowa (Internet Layer / Network Layer)

### Zadania:
- **Routowanie pakietów** między różnymi sieciami
- Adresowanie logiczne (adresy IP)
- Fragmentacja i reassembly (gdy pakiet jest za duży)
- Kontrola ruchu w sieci

### Funkcje:
- **Routing** — wybór najlepszej ścieżki do celu
- **Forwarding** — przekazywanie pakietów do następnego węzła
- **Logical addressing** — adresy IP (niezależne od fizycznych)
- **Fragmentation** — dzielenie dużych pakietów

### Protokół IP (Internet Protocol)

#### IPv4 (wersja 4)
- **Adres:** 32 bity (4 bajty)
- **Format:** kropkowy dziesiętny: `192.168.1.1`
- **Zakres:** 0.0.0.0 do 255.255.255.255
- **Przykład:** `192.168.1.100`

**Klasy adresów IP:**
- **Klasa A:** 0.0.0.0 - 127.255.255.255 (duże sieci)
- **Klasa B:** 128.0.0.0 - 191.255.255.255 (średnie sieci)
- **Klasa C:** 192.0.0.0 - 223.255.255.255 (małe sieci)
- **Klasa D:** 224.0.0.0 - 239.255.255.255 (multicast)
- **Klasa E:** 240.0.0.0 - 255.255.255.255 (zastrzeżone)

**CIDR (Classless Inter-Domain Routing):**
- Notacja: `192.168.1.0/24`
- `/24` oznacza 24 bity na adres sieci (8 bitów na hosty)

#### IPv6 (wersja 6)
- **Adres:** 128 bitów (16 bajtów)
- **Format:** hex, 8 grup po 4 cyfry: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- **Skrócony:** `2001:db8:85a3::8a2e:370:7334` (pominięte zera)

### Format pakietu IP (IPv4):

```
| Version | IHL | Type of Service | Total Length |
| 4 bity  | 4   | 8 bitów         | 16 bitów     |
---------------------------------------------------
| Identification | Flags | Fragment Offset |
| 16 bitów       | 3     | 13 bitów        |
---------------------------------------------------
| TTL | Protocol | Header Checksum |
| 8   | 8        | 16 bitów        |
---------------------------------------------------
| Source IP Address (32 bity) |
| Destination IP Address (32 bity) |
| Options (opcjonalne) |
| Data (payload) |
```

**Ważne pola — szczegółowe wyjaśnienie:**

**1. TTL (Time To Live) — maksymalna liczba przeskoków**
- **Cel:** Zapobiega pętlom routingu
- **Jak działa:**
  - Zaczyna od wartości (np. 64, 128, 255)
  - Każdy router zmniejsza o 1
  - Gdy osiągnie 0 → pakiet jest odrzucany
- **Przykład:** TTL = 64 → pakiet może przejść przez maksymalnie 64 routery
- **Dlaczego ważne:** Jeśli routing jest błędny, pakiet może krążyć w pętli. TTL zapobiega temu.

**2. Protocol — identyfikuje protokół warstwy wyższej**
- **Cel:** Określa, co jest w danych (TCP, UDP, ICMP)
- **Wartości:**
  - 1 = ICMP (komunikaty kontrolne)
  - 6 = TCP (niezawodny transport)
  - 17 = UDP (szybki transport)
- **Przykład:** Protocol = 6 → dane zawierają segment TCP

**3. Source/Destination IP — adresy źródła i celu**
- **Source IP:** Skąd pakiet (adres nadawcy)
- **Destination IP:** Dokąd pakiet (adres odbiorcy)
- **Przykład:** 
  - Source: 192.168.1.100
  - Destination: 8.8.8.8

**Przykład kompletnego pakietu IP krok po kroku:**

**Scenariusz:** Komputer 192.168.1.100 wysyła dane do 8.8.8.8

```
Version:        4 (IPv4)
IHL:            5 (20 bajtów nagłówka)
Total Length:   1500 bajtów (nagłówek 20 + dane 1480)
TTL:            64 (może przejść przez 64 routery)
Protocol:       6 (TCP - dane zawierają segment TCP)
Source IP:      192.168.1.100 (nadawca)
Destination IP: 8.8.8.8 (odbiorca - Google DNS)
Data:           [segment TCP z danymi HTTP]
```

**Proces routowania pakietu krok po kroku:**

1. **Komputer A** (192.168.1.100) tworzy pakiet:
   - Source: 192.168.1.100
   - Destination: 8.8.8.8
   - TTL: 64
   - Protocol: 6 (TCP)

2. **Sprawdza routing:**
   - Czy 8.8.8.8 jest w mojej sieci? 
   - Moja sieć: 192.168.1.0/24
   - 8.8.8.8 nie jest w mojej sieci → wysyła do routera (bramy domyślnej)

3. **Router 1** (192.168.1.1) odbiera pakiet:
   - Sprawdza Destination IP: 8.8.8.8
   - Sprawdza tablicę routingu: "Gdzie wysłać 8.8.8.8?"
   - Znajduje: "Przez interfejs eth1 do routera ISP"
   - Zmniejsza TTL: 64 → 63
   - Przekazuje dalej

4. **Router 2** (ISP) odbiera pakiet:
   - TTL: 63 → 62
   - Sprawdza routing: "8.8.8.8 → przez interfejs X"
   - Przekazuje dalej

5. **Proces powtarza się** na każdym routerze w drodze

6. **Gdy TTL = 0:** Pakiet odrzucony (zapobiega pętli)

7. **Gdy dotrze do sieci 8.8.8.0/24:** Router dostarcza do komputera 8.8.8.8

### Protokoły warstwy internetowej:

#### IP (Internet Protocol)
- Podstawowy protokół routowania
- **Bezpołączeniowy** (connectionless)
- **Niezawodny** (best-effort delivery — nie gwarantuje dostarczenia)

#### ICMP (Internet Control Message Protocol)
- Komunikaty kontrolne i diagnostyczne
- **Ping** — sprawdzanie dostępności (echo request/reply)
- **Traceroute** — śledzenie trasy
- Komunikaty błędów (destination unreachable, time exceeded)

#### ARP (Address Resolution Protocol)
- Tłumaczenie adresu IP na adres MAC
- Działa tylko w sieci lokalnej
- **Zapytanie ARP:** "Kto ma IP 192.168.1.1?" → odpowiedź z MAC

#### IGMP (Internet Group Management Protocol)
- Zarządzanie grupami multicast

### Routowanie

**Router** — urządzenie warstwy 3, które:
- Przekazuje pakiety między sieciami
- Utrzymuje tablice routingu
- Wybiera najlepszą ścieżkę

**Tabela routingu:**
| Sieć docelowa | Maska | Next Hop | Interfejs |
|---------------|-------|----------|-----------|
| 192.168.1.0 | /24 | - | eth0 |
| 10.0.0.0 | /8 | 192.168.1.1 | eth0 |
| 0.0.0.0 | /0 | 192.168.1.1 | eth0 (default gateway) |

### Jednostka danych:
**Pakiet (Packet) / Datagram**

### Obszar działania:
**Sieć globalna (WAN)** — komunikacja między różnymi sieciami

---

## 6) Warstwa 4: Transportowa (Transport Layer)

### Zadania:
- **Komunikacja końcowa** (end-to-end) między aplikacjami
- Segmentacja danych
- Kontrola przepływu (flow control)
- Kontrola błędów (error control)
- Multiplexing/demultiplexing portów

### Funkcje:
- **End-to-end delivery** — dostarczanie do właściwej aplikacji
- **Port addressing** — identyfikacja aplikacji przez porty
- **Segmentation** — dzielenie danych na segmenty
- **Reassembly** — składanie segmentów
- **Error control** — wykrywanie i retransmisja błędów (w TCP)
- **Flow control** — kontrola przepływu

### Porty (Ports)

**Port** — identyfikator aplikacji (16 bitów, zakres 0-65535)

**Kategorie:**
- **Well-known ports (0-1023):** zarezerwowane dla standardowych usług
  - 20, 21: FTP
  - 22: SSH
  - 23: Telnet
  - 25: SMTP
  - 53: DNS
  - 80: HTTP
  - 443: HTTPS
  - 110: POP3
  - 143: IMAP
- **Registered ports (1024-49151):** zarejestrowane aplikacje
- **Dynamic/Private ports (49152-65535):** używane przez klientów

### Protokół TCP (Transmission Control Protocol)

#### Charakterystyka:
- **Połączeniowy (connection-oriented)** — wymaga nawiązania połączenia
- **Niezawodny (reliable)** — gwarantuje dostarczenie i kolejność
- **Z pełnym dupleksem (full-duplex)** — dwukierunkowa komunikacja
- **Z kontrolą przepływu** — adaptacja do możliwości odbiorcy
- **Z kontrolą błędów** — retransmisja utraconych segmentów

#### Proces połączenia TCP (3-way handshake):

```
Klient → Serwer: SYN (synchronize)
Serwer → Klient: SYN-ACK (synchronize-acknowledge)
Klient → Serwer: ACK (acknowledge)
```

**Kroki:**
1. Klient wysyła SYN (sekwencja = x)
2. Serwer odpowiada SYN-ACK (sekwencja = y, ACK = x+1)
3. Klient wysyła ACK (ACK = y+1)
4. Połączenie ustalone!

#### Format segmentu TCP:

```
| Source Port (16) | Destination Port (16) |
| Sequence Number (32) |
| Acknowledgment Number (32) |
| Data Offset | Reserved | Flags | Window Size (16) |
| Checksum (16) | Urgent Pointer (16) |
| Options (opcjonalne) |
| Data (payload) |
```

**Ważne pola:**
- **Sequence Number:** numer porządkowy danych
- **Acknowledgment Number:** potwierdzenie otrzymania
- **Flags:** SYN, ACK, FIN, RST, PSH, URG
- **Window Size:** rozmiar okna (kontrola przepływu)
- **Checksum:** suma kontrolna (wykrywanie błędów)

#### Kontrola przepływu (Flow Control)
- **Sliding Window** — okno przesuwne
- Odbiorca informuje nadawcę o dostępnym buforze
- Zapobiega przeciążeniu odbiorcy

#### Kontrola błędów
- **Acknowledgment (ACK)** — potwierdzenie otrzymania
- **Retransmisja** — ponowne wysłanie niezacknowledged segmentów
- **Timeout** — jeśli ACK nie nadejdzie w czasie

#### Zamykanie połączenia (4-way handshake):

```
Klient → Serwer: FIN (finish)
Serwer → Klient: ACK
Serwer → Klient: FIN
Klient → Serwer: ACK
```

### Protokół UDP (User Datagram Protocol)

#### Charakterystyka:
- **Bezpołączeniowy (connectionless)** — brak nawiązywania połączenia
- **Niezawodny (unreliable)** — nie gwarantuje dostarczenia
- **Prosty i szybki** — minimalny overhead
- **Bez kontroli przepływu**
- **Bez kontroli błędów** (tylko checksum)

#### Format datagramu UDP:

```
| Source Port (16) | Destination Port (16) |
| Length (16) | Checksum (16) |
| Data (payload) |
```

**Pola:**
- **Source/Destination Port:** porty źródła i celu
- **Length:** długość datagramu (nagłówek + dane)
- **Checksum:** opcjonalna suma kontrolna
- **Data:** dane aplikacji

### Porównanie TCP vs UDP:

| Aspekt | TCP | UDP |
|--------|-----|-----|
| **Połączenie** | Połączeniowy | Bezpołączeniowy |
| **Niezawodność** | Niezawodny (gwarancja) | Niezawodny (best-effort) |
| **Kolejność** | Gwarantuje kolejność | Nie gwarantuje |
| **Kontrola przepływu** | Tak (sliding window) | Nie |
| **Kontrola błędów** | Tak (retransmisja) | Tylko checksum |
| **Overhead** | Wysoki (nagłówek 20 bajtów) | Niski (nagłówek 8 bajtów) |
| **Prędkość** | Wolniejszy | Szybszy |
| **Zastosowania** | HTTP, FTP, SMTP, SSH | DNS, DHCP, streaming, gry online |

### Zastosowania:

#### TCP (gdy ważna niezawodność):
- HTTP/HTTPS (przeglądanie stron)
- FTP (transfer plików)
- SMTP (poczta e-mail)
- SSH (zdalne logowanie)
- Bazy danych

#### UDP (gdy ważna prędkość):
- DNS (zapytania DNS)
- DHCP (konfiguracja IP)
- Streaming video/audio (RTSP)
- Gry online (małe opóźnienia)
- VoIP (głos przez IP)

### Jednostka danych:
- **TCP:** Segment (Segment)
- **UDP:** Datagram (Datagram)

---

## 7) Warstwa 5: Aplikacji (Application Layer)

### Zadania:
- **Interfejs dla użytkownika i aplikacji**
- Komunikacja między aplikacjami
- Formatowanie danych
- Wykorzystanie usług niższych warstw

### Funkcje:
- **Komunikacja aplikacji** — protokoły dla konkretnych usług
- **Prezentacja danych** — formatowanie, kompresja, szyfrowanie
- **Zarządzanie sesją** — nawiązywanie i zamykanie sesji (często włączone w protokół)

### Protokoły warstwy aplikacji:

#### HTTP (HyperText Transfer Protocol)
- **Port:** 80 (HTTP), 443 (HTTPS)
- **Protokół transportowy:** TCP
- **Zastosowanie:** przeglądanie stron WWW
- **Metody:** GET, POST, PUT, DELETE, HEAD, OPTIONS
- **Format:** request/response

**Przykład HTTP Request:**
```
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
```

**Przykład HTTP Response:**
```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<html>...</html>
```

#### HTTPS (HTTP Secure)
- **Port:** 443
- HTTP + SSL/TLS (szyfrowanie)
- Bezpieczna komunikacja

#### FTP (File Transfer Protocol)
- **Port:** 20 (dane), 21 (kontrola)
- **Protokół transportowy:** TCP
- **Zastosowanie:** transfer plików
- **Tryby:** aktywny, pasywny

#### SMTP (Simple Mail Transfer Protocol)
- **Port:** 25
- **Protokół transportowy:** TCP
- **Zastosowanie:** wysyłanie e-maili
- **Serwery:** SMTP server

#### POP3 (Post Office Protocol v3)
- **Port:** 110
- **Protokół transportowy:** TCP
- **Zastosowanie:** pobieranie e-maili (ze serwera)
- **Tryb:** pobieranie i usuwanie ze serwera

#### IMAP (Internet Message Access Protocol)
- **Port:** 143
- **Protokół transportowy:** TCP
- **Zastosowanie:** dostęp do e-maili (synchronizacja z serwerem)
- **Tryb:** e-maile pozostają na serwerze

#### DNS (Domain Name System)
- **Port:** 53
- **Protokół transportowy:** UDP (zapytania), TCP (transfer stref)
- **Zastosowanie:** tłumaczenie nazw domenowych na adresy IP
- **Rekordy:** A, AAAA, MX, CNAME, NS, TXT

**Przykład:** `www.example.com` → `93.184.216.34`

#### DHCP (Dynamic Host Configuration Protocol)
- **Port:** 67 (serwer), 68 (klient)
- **Protokół transportowy:** UDP
- **Zastosowanie:** automatyczna konfiguracja IP, maski, bramy, DNS
- **Proces:** DISCOVER → OFFER → REQUEST → ACK

#### SSH (Secure Shell)
- **Port:** 22
- **Protokół transportowy:** TCP
- **Zastosowanie:** zdalne logowanie, bezpieczna komunikacja
- **Funkcje:** szyfrowanie, uwierzytelnianie

#### Telnet
- **Port:** 23
- **Protokół transportowy:** TCP
- **Zastosowanie:** zdalne logowanie (niezaszyfrowane, przestarzałe)

### Jednostka danych:
**Wiadomość (Message) / Dane aplikacji**

---

## 8) Enkapsulacja i Dekapsulacja

### Enkapsulacja (Encapsulation)
**Proces pakowania danych przez warstwy (w dół):**

```
Aplikacja:     "Hello World"
                ↓
Transport:     [TCP Header | "Hello World"]
                ↓
Internet:      [IP Header | TCP Header | "Hello World"]
                ↓
Data Link:     [Ethernet Header | IP Header | TCP Header | "Hello World" | CRC]
                ↓
Physical:      [Bit stream przez kabel/więź]
```

**Krok po kroku:**
1. **Aplikacja** tworzy dane: "Hello World"
2. **Transport (TCP)** dodaje nagłówek TCP → segment
3. **Internet (IP)** dodaje nagłówek IP → pakiet
4. **Data Link (Ethernet)** dodaje nagłówek Ethernet i CRC → ramka
5. **Physical** konwertuje na bity → transmisja

### Dekapsulacja (Decapsulation)
**Proces rozpakowywania danych przez warstwy (w górę):**

```
Physical:      [Bit stream z kabla]
                ↓
Data Link:     [Ethernet Header | IP Header | TCP Header | "Hello World" | CRC]
               (sprawdza CRC, usuwa nagłówek Ethernet)
                ↓
Internet:      [IP Header | TCP Header | "Hello World"]
               (sprawdza adres IP, usuwa nagłówek IP)
                ↓
Transport:     [TCP Header | "Hello World"]
               (sprawdza port, usuwa nagłówek TCP)
                ↓
Aplikacja:     "Hello World"
```

### Przykład pełny: Wysyłanie e-maila

**Wysyłanie (enkapsulacja):**
1. **Aplikacja:** Użytkownik pisze e-mail, klika "Wyślij"
2. **Transport (SMTP):** Formatuje wiadomość SMTP
3. **Transport (TCP):** Dzieli na segmenty, dodaje nagłówek TCP
4. **Internet (IP):** Dodaje nagłówek IP (source IP, destination IP)
5. **Data Link (Ethernet):** Dodaje nagłówek Ethernet (source MAC, destination MAC)
6. **Physical:** Wysyła przez kabel

**Odbieranie (dekapsulacja):**
1. **Physical:** Odbiera bity
2. **Data Link:** Sprawdza MAC, usuwa nagłówek Ethernet
3. **Internet:** Sprawdza IP, usuwa nagłówek IP, przekazuje do TCP
4. **Transport (TCP):** Sprawdza port, składa segmenty, usuwa nagłówek TCP
5. **Aplikacja (SMTP):** Odbiera wiadomość e-mail

---

## 9) Adresowanie w modelu TCP/IP

### Adresy w każdej warstwie:

| Warstwa | Typ adresu | Przykład | Zakres/Format |
|---------|-----------|----------|---------------|
| **Aplikacji** | Nazwa domeny | www.example.com | Dowolny string |
| **Transportowa** | Port | 80, 443, 8080 | 0-65535 |
| **Internetowa** | Adres IP | 192.168.1.1 | IPv4: 4 bajty, IPv6: 16 bajtów |
| **Łącza danych** | Adres MAC | AA:BB:CC:DD:EE:FF | 6 bajtów (48 bitów) |
| **Fizyczna** | - | - | Sygnały fizyczne |

### Mapowanie adresów:

**DNS (Aplikacja → Internet):**
- `www.example.com` → `93.184.216.34`

**ARP (Internet → Łącza danych):**
- `192.168.1.1` → `AA:BB:CC:DD:EE:FF`

**Porty (Transport → Aplikacja):**
- Port 80 → HTTP
- Port 443 → HTTPS
- Port 25 → SMTP

---

## 10) Protokoły pomocnicze

### ARP (Address Resolution Protocol)
**Zadanie:** Tłumaczenie adresu IP na adres MAC

**Proces:**
1. Host chce wysłać pakiet do `192.168.1.1`
2. Wysyła **ARP Request** (broadcast): "Kto ma IP 192.168.1.1?"
3. Host z tym IP odpowiada **ARP Reply** z adresem MAC
4. Nadawca zapisuje mapowanie w tablicy ARP

**Tabela ARP:**
| IP Address | MAC Address |
|------------|-------------|
| 192.168.1.1 | AA:BB:CC:DD:EE:FF |
| 192.168.1.2 | 11:22:33:44:55:66 |

### ICMP (Internet Control Message Protocol)
**Zadanie:** Komunikaty kontrolne i diagnostyczne

**Typy komunikatów:**
- **Echo Request/Reply** (ping)
- **Destination Unreachable**
- **Time Exceeded** (traceroute)
- **Redirect**

**Przykład użycia:**
```bash
ping www.google.com
# Wysyła ICMP Echo Request
# Oczekuje ICMP Echo Reply
```

### NAT (Network Address Translation)
**Zadanie:** Tłumaczenie adresów prywatnych na publiczne

**Proces:**
- Wewnętrzna sieć używa prywatnych IP (192.168.x.x)
- Router tłumaczy na publiczny IP przy komunikacji z Internetem
- Tabela NAT przechowuje mapowania

**Przykład:**
```
Wewnątrz: 192.168.1.100:5000
Router: 203.0.113.1 (publiczny IP)
Na zewnątrz: 203.0.113.1:12345
```

---

## 11) Przepływ danych — przykład kompletny krok po kroku

### Scenariusz: Otwarcie strony WWW — szczegółowa analiza

**Sytuacja:** Użytkownik na komputerze 192.168.1.100 chce otworzyć stronę www.example.com

---

### KROK 1: Warstwa Aplikacji (HTTP)

**Akcja użytkownika:** Wpisuje `http://www.example.com` w przeglądarce

**1.1. Rozwiązanie nazwy domenowej (DNS):**

**Problem:** Przeglądarka zna tylko nazwę "www.example.com", ale potrzebuje adresu IP.

**Proces DNS krok po kroku:**
1. Przeglądarka sprawdza cache lokalny → nie ma
2. Wysyła zapytanie DNS do serwera DNS (np. 8.8.8.8)
3. Serwer DNS odpowiada: `www.example.com` → `93.184.216.34`
4. Przeglądarka zapisuje w cache

**Wynik:** Znamy adres IP serwera: **93.184.216.34**

**1.2. Przygotowanie żądania HTTP:**

**Żądanie HTTP GET:**
```
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

**Dane aplikacji:** "GET /index.html HTTP/1.1\r\nHost: www.example.com\r\n..."

---

### KROK 2: Warstwa Transportowa (TCP)

**2.1. Wybór portów:**

- **Port źródłowy:** 49152 (losowy, wybrany przez system)
- **Port docelowy:** 80 (HTTP - standardowy port)

**2.2. Nawiązanie połączenia TCP (3-way handshake):**

**Krok 2.2.1: SYN (Synchronize)**
- Klient → Serwer: SYN
- Sekwencja klienta: x = 1000
- Flaga SYN = 1

**Krok 2.2.2: SYN-ACK (Synchronize-Acknowledge)**
- Serwer → Klient: SYN-ACK
- Sekwencja serwera: y = 5000
- Potwierdzenie: ACK = x + 1 = 1001
- Flagi: SYN = 1, ACK = 1

**Krok 2.2.3: ACK (Acknowledge)**
- Klient → Serwer: ACK
- Potwierdzenie: ACK = y + 1 = 5001
- Flaga ACK = 1

**Połączenie ustalone!** ✓

**2.3. Tworzenie segmentu TCP:**

**Nagłówek TCP:**
```
Source Port:      49152
Destination Port: 80
Sequence Number:  1001
Acknowledgment:   0 (pierwszy segment)
Flags:            PSH (push data), ACK
Window Size:      65535
Checksum:         0xABCD
Data:             [żądanie HTTP]
```

**Segment TCP:** [Nagłówek TCP | Dane HTTP]

---

### KROK 3: Warstwa Internetowa (IP)

**3.1. Tworzenie pakietu IP:**

**Nagłówek IP:**
```
Version:        4 (IPv4)
IHL:            5 (20 bajtów)
Total Length:   1500 bajtów
TTL:            64
Protocol:       6 (TCP)
Source IP:      192.168.1.100
Destination IP: 93.184.216.34
Checksum:       0x1234
Data:           [segment TCP]
```

**Pakiet IP:** [Nagłówek IP | Segment TCP]

**3.2. Sprawdzenie routingu:**

**Komputer sprawdza:**
- Moja sieć: 192.168.1.0/24
- Docelowy IP: 93.184.216.34
- **Wniosek:** Nie jest w mojej sieci → wysyła do routera (bramy domyślnej: 192.168.1.1)

---

### KROK 4: Warstwa Łącza Danych (Ethernet)

**4.1. Znajdowanie adresu MAC routera (ARP):**

**Problem:** Znamy IP routera (192.168.1.1), ale potrzebujemy MAC.

**Proces ARP:**
1. Sprawdza cache ARP → nie ma
2. Wysyła **ARP Request** (broadcast):
   - "Kto ma IP 192.168.1.1?"
   - Wszystkie komputery w sieci słyszą
3. Router odpowiada **ARP Reply**:
   - "Ja mam IP 192.168.1.1, mój MAC to 11:22:33:44:55:66"
4. Komputer zapisuje w cache ARP

**Wynik:** Znamy MAC routera: **11:22:33:44:55:66**

**4.2. Tworzenie ramki Ethernet:**

**Nagłówek Ethernet:**
```
Preamble:        10101010... (7 bajtów)
Destination MAC: 11:22:33:44:55:66 (router)
Source MAC:      AA:BB:CC:DD:EE:FF (moja karta sieciowa)
Type:            0x0800 (IPv4)
Data:            [pakiet IP]
CRC:             0x5678 (suma kontrolna)
```

**Ramka Ethernet:** [Preamble | Nagłówek Ethernet | Pakiet IP | CRC]

---

### KROK 5: Warstwa Fizyczna

**5.1. Konwersja na sygnały:**

**Proces:**
1. Ramka Ethernet → ciąg bitów
2. Bity → sygnały elektryczne (kabel) lub radiowe (Wi-Fi)
3. Przykład (kabel):
   - Bit 0 = 0V
   - Bit 1 = +5V

**5.2. Transmisja:**

**Sygnały są wysyłane przez medium:**
- Kabel Ethernet → sygnały elektryczne
- Wi-Fi → fale radiowe

---

### KROK 6: Po stronie routera (przekazywanie)

**6.1. Odbieranie ramki:**

1. **Warstwa fizyczna:** Odbiera sygnały → konwertuje na bity
2. **Warstwa łącza:** 
   - Sprawdza Destination MAC → dla mnie? TAK (11:22:33:44:55:66)
   - Sprawdza CRC → OK
   - Usuwa nagłówek Ethernet
   - Przekazuje pakiet IP do warstwy sieciowej

**6.2. Routowanie:**

1. **Warstwa sieciowa:**
   - Sprawdza Destination IP: 93.184.216.34
   - Sprawdza tablicę routingu: "Gdzie wysłać 93.184.216.34?"
   - Znajduje: "Przez interfejs eth1 do routera ISP"
   - Zmniejsza TTL: 64 → 63
   - Przekazuje do warstwy łącza

2. **Warstwa łącza:**
   - Znajduje MAC następnego routera (przez ARP lub tablicę)
   - Tworzy nową ramkę Ethernet:
     - Destination MAC: MAC następnego routera
     - Source MAC: MAC routera
   - Przekazuje do warstwy fizycznej

3. **Warstwa fizyczna:** Wysyła dalej

**Proces powtarza się** na każdym routerze w drodze do serwera.

---

### KROK 7: Po stronie serwera (odbiór)

**7.1. Dekapsulacja (odwrotny proces):**

**Warstwa fizyczna:**
- Odbiera sygnały → konwertuje na bity

**Warstwa łącza danych:**
- Odbiera ramkę Ethernet
- Sprawdza Destination MAC → dla mnie? TAK
- Sprawdza CRC → OK
- Usuwa nagłówek Ethernet
- Przekazuje pakiet IP do warstwy sieciowej

**Warstwa internetowa:**
- Odbiera pakiet IP
- Sprawdza Destination IP → dla mnie? TAK (93.184.216.34)
- Sprawdza Protocol → 6 (TCP)
- Usuwa nagłówek IP
- Przekazuje segment TCP do warstwy transportowej

**Warstwa transportowa:**
- Odbiera segment TCP
- Sprawdza Destination Port → 80 (HTTP)
- Sprawdza sekwencję i potwierdza (ACK)
- Usuwa nagłówek TCP
- Przekazuje dane HTTP do warstwy aplikacji

**Warstwa aplikacji:**
- Odbiera żądanie HTTP: "GET /index.html HTTP/1.1"
- Przetwarza żądanie
- Przygotowuje odpowiedź: "HTTP/1.1 200 OK\r\nContent-Type: text/html\r\n\r\n<html>..."

---

### KROK 8: Wysyłanie odpowiedzi (odwrotny proces)

**Proces jest identyczny, ale w odwrotną stronę:**

1. **Warstwa aplikacji:** Odpowiedź HTTP
2. **Warstwa transportowa:** Segment TCP (port źródłowy 80, docelowy 49152)
3. **Warstwa internetowa:** Pakiet IP (source: 93.184.216.34, destination: 192.168.1.100)
4. **Warstwa łącza:** Ramka Ethernet (MAC serwera → MAC routera)
5. **Warstwa fizyczna:** Sygnały przez medium

**Proces powtarza się** przez wszystkie routery z powrotem do klienta.

**Klient odbiera odpowiedź:**
- Dekapsulacja przez wszystkie warstwy
- Przeglądarka wyświetla stronę HTML

---

### Podsumowanie — wizualizacja przepływu:

```
KLIENT                                    SERWER
─────────────────────────────────────────────────────
Warstwa 5: HTTP Request
    ↓ enkapsulacja
Warstwa 4: TCP Segment (port 80)
    ↓ enkapsulacja
Warstwa 3: IP Packet (93.184.216.34)
    ↓ enkapsulacja
Warstwa 2: Ethernet Frame (MAC routera)
    ↓ enkapsulacja
Warstwa 1: Sygnały fizyczne
    ↓ transmisja przez sieć
─────────────────────────────────────────────────────
    ↑ dekapsulacja
Warstwa 1: Sygnały fizyczne
    ↑ dekapsulacja
Warstwa 2: Ethernet Frame
    ↑ dekapsulacja
Warstwa 3: IP Packet
    ↑ dekapsulacja
Warstwa 4: TCP Segment
    ↑ dekapsulacja
Warstwa 5: HTTP Response
```

**Kluczowe punkty:**
- Każda warstwa dodaje swój nagłówek (enkapsulacja)
- Każda warstwa usuwa swój nagłówek (dekapsulacja)
- Warstwy nie muszą wiedzieć, co jest w danych innych warstw
- Każda warstwa ma swoją odpowiedzialność

---

## 12) Bezpieczeństwo w modelu TCP/IP

### Zagrożenia w każdej warstwie:

**Warstwa Fizyczna:**
- Podsłuchiwanie (wiretapping)
- Zakłócenia (jamming)

**Warstwa Łącza Danych:**
- MAC spoofing
- ARP poisoning

**Warstwa Internetowa:**
- IP spoofing
- DDoS (Distributed Denial of Service)
- Man-in-the-Middle

**Warstwa Transportowa:**
- TCP SYN flood
- Port scanning

**Warstwa Aplikacji:**
- SQL injection
- XSS (Cross-Site Scripting)
- Phishing

### Mechanizmy zabezpieczeń:

- **SSL/TLS** (warstwa aplikacji/transportowa) — szyfrowanie
- **VPN** (warstwa internetowa) — tunelowanie
- **Firewall** (warstwy 3-4) — filtrowanie pakietów
- **IDS/IPS** (wszystkie warstwy) — wykrywanie intruzów

---

## 13) Wzory i podsumowanie

### Rozmiary nagłówków:

| Warstwa | Protokół | Rozmiar nagłówka |
|---------|----------|------------------|
| Transport | TCP | 20 bajtów (min) + opcje |
| Transport | UDP | 8 bajtów |
| Internet | IPv4 | 20 bajtów (min) + opcje |
| Internet | IPv6 | 40 bajtów (stały) |
| Łącza danych | Ethernet | 14 bajtów + 4 bajty CRC = 18 bajtów |

### MTU (Maximum Transmission Unit):
- **Ethernet:** 1500 bajtów (dane + nagłówki warstw wyższych)
- **IP:** Jeśli pakiet > MTU → fragmentacja

### Porty (ważne):
- **0-1023:** Well-known
- **1024-49151:** Registered
- **49152-65535:** Dynamic/Private

### Adresy:
- **IPv4:** 32 bity = 4 bajty
- **IPv6:** 128 bitów = 16 bajtów
- **MAC:** 48 bitów = 6 bajtów
- **Port:** 16 bitów = 2 bajty (0-65535)

---

## 14) Kluczowe zdania na obronę

1. **Model TCP/IP** to praktyczny model 4-5 warstwowy używany w Internecie (OSI to model teoretyczny 7-warstwowy).

2. **Warstwa fizyczna** — przesyła bity przez medium (kable, fale radiowe).

3. **Warstwa łącza danych** — komunikacja w sieci lokalnej, tworzenie ramek, adresy MAC, kontrola błędów (CRC).

4. **Warstwa internetowa** — routowanie między sieciami, adresy IP, protokoły IP, ICMP, ARP.

5. **Warstwa transportowa** — komunikacja końcowa, porty, TCP (niezawodny, połączeniowy) i UDP (szybszy, bezpołączeniowy).

6. **Warstwa aplikacji** — protokoły dla konkretnych usług (HTTP, FTP, SMTP, DNS, DHCP, SSH).

7. **Enkapsulacja** — pakowanie danych przez warstwy w dół (dodawanie nagłówków).

8. **Dekapsulacja** — rozpakowywanie danych przez warstwy w górę (usuwanie nagłówków).

9. **TCP** — połączeniowy, niezawodny, z kontrolą przepływu i błędów (3-way handshake).

10. **UDP** — bezpołączeniowy, szybki, bez gwarancji dostarczenia (mniejszy overhead).

11. **Porty** — identyfikują aplikacje (0-65535), well-known ports 0-1023.

12. **ARP** — tłumaczy IP na MAC (działa w sieci lokalnej).

13. **DNS** — tłumaczy nazwy domenowe na adresy IP.

14. **Router** — urządzenie warstwy 3 (routuje pakiety między sieciami).

15. **Switch** — urządzenie warstwy 2 (przekazuje ramki w sieci lokalnej na podstawie MAC).

16. **Adresy:** IP (logiczny, warstwa 3), MAC (fizyczny, warstwa 2), Port (aplikacja, warstwa 4).

17. **TCP/IP** to zestaw protokołów, nie tylko TCP i IP — zawiera wiele protokołów na wszystkich warstwach.

18. **Różnica TCP/UDP:** TCP gwarantuje dostarczenie i kolejność (dla HTTP, FTP), UDP jest szybszy ale bez gwarancji (dla DNS, streaming).

