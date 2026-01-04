# Model Warstwowy TCP/IP — Kompletna Notatka do Obrony

## 1) Wprowadzenie — modele warstwowe w sieciach

### Dlaczego modele warstwowe?
Sieci komputerowe są złożone. **Model warstwowy** (layered model) upraszcza projektowanie i zrozumienie przez:
- **Separację odpowiedzialności** — każda warstwa ma określoną funkcję
- **Niezależność implementacji** — warstwy mogą być modyfikowane niezależnie
- **Standardyzację** — protokoły mogą współpracować między różnymi systemami
- **Modularność** — łatwiejsze zarządzanie i debugowanie

### Podstawowa zasada:
> Każda warstwa używa usług warstwy niższej i zapewnia usługi warstwie wyższej.

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

## 3) Warstwa 1: Fizyczna (Physical Layer)

### Zadania:
- **Przesyłanie bitów** przez medium fizyczne
- Definiowanie charakterystyk elektrycznych, mechanicznych, funkcjonalnych
- Kodowanie danych (0 i 1 → sygnały fizyczne)

### Funkcje:
- Definiowanie sygnałów (napięcie, częstotliwość)
- Synchronizacja bitowa
- Topologia fizyczna (gwiazda, magistrala, pierścień)
- Sposób transmisji (simplex, half-duplex, full-duplex)

### Media transmisyjne:
- **Przewodowe:** kable miedziane (skrętka, koncentryczny), światłowód
- **Bezprzewodowe:** fale radiowe, mikrofale, podczerwień

### Przykłady:
- Ethernet (kabel skrętka)
- Wi-Fi (fale radiowe)
- DSL (linia telefoniczna)
- Fiber-optic (światłowód)

### Jednostka danych:
**Bit** (0 lub 1)

---

## 4) Warstwa 2: Łącza Danych (Data Link Layer)

### Zadania:
- **Komunikacja między węzłami w tej samej sieci lokalnej**
- Tworzenie ramek (frames) z bitów
- Wykrywanie i korekcja błędów (CRC)
- Kontrola dostępu do medium (MAC)
- Adresowanie fizyczne (MAC address)

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

### Format ramki Ethernet (przykład):

```
| Preamble | Destination MAC | Source MAC | Type/Length | Data (46-1500 bajtów) | CRC |
| 7 bajtów | 6 bajtów        | 6 bajtów   | 2 bajty     | 46-1500 bajtów        | 4 bajty |
```

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

**Ważne pola:**
- **TTL (Time To Live):** maksymalna liczba przeskoków (zapobiega pętli)
- **Protocol:** identyfikuje protokół warstwy wyższej (TCP=6, UDP=17, ICMP=1)
- **Source/Destination IP:** adresy źródła i celu

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

## 11) Przepływ danych — przykład kompletny

### Scenariusz: Otwarcie strony WWW

**1. Użytkownik wpisuje:** `http://www.example.com`

**2. Warstwa Aplikacji:**
- DNS: `www.example.com` → `93.184.216.34`
- HTTP: Przygotowanie request GET

**3. Warstwa Transportowa (TCP):**
- Port źródłowy: 49152 (losowy, wybrany przez system)
- Port docelowy: 80 (HTTP)
- 3-way handshake:
  - SYN → SYN-ACK → ACK
- Segment TCP z danymi HTTP

**4. Warstwa Internetowa (IP):**
- Source IP: 192.168.1.100 (adres klienta)
- Destination IP: 93.184.216.34 (serwer www.example.com)
- TTL: 64
- Protokół: TCP (6)

**5. Warstwa Łącza Danych (Ethernet):**
- Source MAC: AA:BB:CC:DD:EE:FF (karta sieciowa klienta)
- Destination MAC: Router MAC (uzyskany przez ARP dla bramy domyślnej)
- Nagłówek Ethernet + CRC

**6. Warstwa Fizyczna:**
- Konwersja na sygnały elektryczne/radiowe
- Transmisja przez medium

**Po stronie serwera (odwrotny proces):**
- Dekapsulacja przez wszystkie warstwy
- HTTP server otrzymuje request
- Przygotowanie response
- Enkapsulacja i wysłanie z powrotem

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

