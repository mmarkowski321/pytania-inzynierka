# Metody i mechanizmy zapewnienia bezpiecznego dostępu i bezpiecznej komunikacji sieciowej — Notatka do Obrony

## 0) Podstawy — od zera

### Co to jest bezpieczna komunikacja sieciowa?
Bezpieczna komunikacja sieciowa oznacza przesyłanie danych przez sieć w sposób, który zapewnia:
- **Poufność** — tylko uprawnione osoby mogą odczytać dane
- **Integralność** — dane nie zostały zmienione podczas transmisji
- **Uwierzytelnienie** — pewność co do tożsamości nadawcy/odbiorcy
- **Nieodparcie** (Non-repudiation) — nie można zaprzeczyć wykonaniu operacji

### Zagrożenia w sieci
- **Przechwycenie danych** (sniffing) — podsłuchiwanie transmisji
- **Modyfikacja danych** (tampering) — zmiana treści podczas przesyłania
- **Ataki typu man-in-the-middle** — przechwycenie i modyfikacja komunikacji
- **Fałszowanie tożsamości** (spoofing) — podszywanie się pod inne urządzenie
- **Odmowa usługi** (DoS/DDoS) — uniemożliwienie działania usługi

---

## 1) Szyfrowanie

### Szyfrowanie symetryczne

#### Co to jest?
**Szyfrowanie symetryczne** — ta sama klucz służy do szyfrowania i deszyfrowania.

**Cechy:**
- ✅ Szybkie
- ❌ Problem dystrybucji klucza (jak bezpiecznie przekazać klucz?)

**Przykłady algorytmów:**
- **DES** (Data Encryption Standard) — 56-bit, przestarzały
- **3DES** (Triple DES) — 3x DES, wciąż używany
- **AES** (Advanced Encryption Standard) — 128/192/256-bit, **najpopularniejszy**
- **Blowfish** — 32-448 bit
- **ChaCha20** — szybszy alternatywa dla AES

**Przykład użycia:**
```
Wiadomość: "Tajne dane"
Klucz: "klucz123"
Zaszyfrowane: "Xdmfp Zfqf" (uproszczony przykład)
```

### Szyfrowanie asymetryczne (klucz publiczny/prywatny)

#### Co to jest?
**Szyfrowanie asymetryczne** — para kluczy: publiczny (do szyfrowania) i prywatny (do deszyfrowania).

**Cechy:**
- ✅ Bezpieczna dystrybucja (klucz publiczny można udostępniać)
- ❌ Wolniejsze niż szyfrowanie symetryczne
- ✅ Umożliwia podpisy cyfrowe

**Przykłady algorytmów:**
- **RSA** (Rivest-Shamir-Adleman) — najpopularniejszy, 1024/2048/4096-bit
- **ECC** (Elliptic Curve Cryptography) — mniejszy klucz, ta sama siła
- **Diffie-Hellman** — wymiana kluczy

**Przykład:**
```
Alicja ma parę kluczy: publiczny i prywatny
Alicja udostępnia klucz publiczny
Bob szyfruje wiadomość kluczem publicznym Alicji
Tylko Alicja (z kluczem prywatnym) może odszyfrować
```

### Hybrid Encryption (mieszane)

**Najczęściej używane rozwiązanie:**
1. Szyfrowanie asymetryczne do wymiany klucza sesji
2. Szyfrowanie symetryczne (AES) do szyfrowania danych

**Zalety:** Szybkość (AES) + bezpieczeństwo dystrybucji (RSA)

---

## 2) Protokoły bezpieczeństwa warstwy transportowej

### SSL/TLS (Secure Sockets Layer / Transport Layer Security)

#### Co to jest?
**SSL/TLS** to protokoły szyfrujące komunikację między klientem a serwerem (głównie HTTP → HTTPS).

**Wersje:**
- **SSL 2.0/3.0** — przestarzałe, niebezpieczne (nie używane)
- **TLS 1.0/1.1** — przestarzałe, niezalecane
- **TLS 1.2** — aktualnie używany
- **TLS 1.3** — najnowszy, najbezpieczniejszy

**Port:**
- HTTPS: 443 (HTTP na TLS)

**Jak działa?**
1. **Handshake** — negocjacja protokołu, wymiana certyfikatów
2. **Wymiana kluczy** — klucz sesji (szyfrowanie symetryczne)
3. **Szyfrowana komunikacja** — wszystkie dane szyfrowane

**Użycie:**
- HTTPS (HTTP Secure)
- FTPS (FTP Secure)
- SMTPS (SMTP Secure)
- IMAPS (IMAP Secure)

### HTTPS (HTTP Secure)

**Co to jest?** HTTP szyfrowane przez TLS/SSL.

**Zalety:**
- ✅ Szyfrowanie danych w transmisji
- ✅ Weryfikacja tożsamości serwera (certyfikat)
- ✅ Ochrona przed man-in-the-middle

**Przykład:**
```
HTTP:  http://example.com  (niebezpieczne)
HTTPS: https://example.com (bezpieczne)
```

**Certyfikaty SSL/TLS:**
- **CA** (Certificate Authority) — urząd certyfikacji (np. Let's Encrypt, DigiCert)
- **Certyfikat X.509** — potwierdza tożsamość serwera
- **Wildcard certyfikat** — działa dla *.domena.com

---

## 3) VPN (Virtual Private Network)

### Co to jest VPN?
**VPN** tworzy zaszyfrowany tunel przez publiczną sieć (Internet), umożliwiając bezpieczny dostęp do sieci prywatnej.

**Zastosowania:**
- Praca zdalna — dostęp do sieci firmowej z domu
- Ochrona prywatności — szyfrowanie ruchu w publicznych sieciach Wi-Fi
- Omijanie cenzury — dostęp do zasobów z innych krajów

### Protokoły VPN

#### IPSec (Internet Protocol Security)

**Cechy:**
- Działa na poziomie IP (warstwa 3)
- Dwa tryby: **Transport** (tylko dane) i **Tunnel** (cały pakiet IP)
- **AH** (Authentication Header) — integralność
- **ESP** (Encapsulating Security Payload) — szyfrowanie + integralność

**Użycie:**
- Site-to-site VPN
- L2TP/IPSec (L2TP + IPSec)

#### OpenVPN

**Cechy:**
- Open source
- Używa SSL/TLS do wymiany kluczy
- Działa na porcie UDP/TCP (zwykle 1194)
- Bardzo bezpieczny i elastyczny

**Przykład konfiguracji klienta:**
```bash
# Plik .ovpn
client
dev tun
proto udp
remote vpn.server.com 1194
cert client.crt
key client.key
ca ca.crt
```

#### WireGuard

**Cechy:**
- Nowoczesny, bardzo szybki
- Prosty w konfiguracji
- Niski overhead (mniej zasobów)
- Wbudowany w kernel Linux

**Użycie:**
- Zastępuje IPSec i OpenVPN w nowych instalacjach

#### PPTP (Point-to-Point Tunneling Protocol)

**Cechy:**
- ❌ Przestarzały i niebezpieczny
- ❌ Nie używać w nowych instalacjach

### Konfiguracja VPN (przykład Linux - OpenVPN)

```bash
# Instalacja
sudo apt install openvpn

# Uruchomienie klienta
sudo openvpn --config client.ovpn

# LUB przez NetworkManager (GUI)
# Ustawienia → Sieć → VPN → Dodaj
```

---

## 4) SSH (Secure Shell)

### Co to jest SSH?
**SSH** to protokół bezpiecznego dostępu zdalnego do systemów Unix/Linux.

**Port:** 22

**Cechy:**
- ✅ Szyfrowana komunikacja
- ✅ Uwierzytelnianie przez hasło lub klucz
- ✅ Możliwość port forwarding (tunelowanie)
- ✅ SFTP/SCP do transferu plików

### Uwierzytelnianie w SSH

#### Hasło
```bash
ssh uzytkownik@server.com
# Wpisz hasło
```

#### Klucz publiczny (rekomendowane)
```bash
# Generowanie pary kluczy
ssh-keygen -t rsa -b 4096

# Kopiowanie klucza publicznego na serwer
ssh-copy-id uzytkownik@server.com

# Połączenie bez hasła
ssh uzytkownik@server.com
```

**Bezpieczeństwo kluczy:**
- Klucz prywatny — **NIGDY nie udostępniaj!** (tylko na twoim komputerze)
- Klucz publiczny — bezpiecznie można udostępniać

### Hardening SSH (zabezpieczenie)

**Konfiguracja (/etc/ssh/sshd_config):**
```bash
# Wyłącz logowanie root
PermitRootLogin no

# Wyłącz hasła (tylko klucze)
PasswordAuthentication no

# Zmień port (opcjonalnie, security through obscurity)
Port 2222

# Ograniczenie użytkowników
AllowUsers uzytkownik1 uzytkownik2

# Restart
sudo systemctl restart sshd
```

---

## 5) Firewall

### Co to jest firewall?
**Firewall** to system kontroli dostępu do sieci, który blokuje lub zezwala na ruch sieciowy na podstawie reguł.

### Rodzaje firewall

#### Packet Filtering Firewall
Sprawdza nagłówki pakietów (IP, port, protokół).

**Przykład reguł:**
- Zezwól na port 80 (HTTP) z zewnątrz
- Zablokuj port 22 (SSH) z zewnątrz
- Zezwól na cały ruch wychodzący

#### Stateful Firewall
Śledzi stan połączeń (np. odpowiedź na żądanie HTTP).

**Zaleta:** Lepiej chroni przed atakami.

#### Application Layer Firewall (Next-Generation)
Analizuje zawartość pakietów (warstwa 7).

### iptables (Linux)

**Podstawowe komendy:**
```bash
# Wyświetlenie reguł
sudo iptables -L

# Zezwól na SSH (port 22)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Zablokuj port 3306 (MySQL) z zewnątrz
sudo iptables -A INPUT -p tcp --dport 3306 -j DROP

# Zezwól na ruch lokalny
sudo iptables -A INPUT -i lo -j ACCEPT

# Zezwól na odpowiedzi na połączenia wychodzące
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Domyślnie zablokuj wszystko
sudo iptables -P INPUT DROP

# Zapisanie reguł
sudo iptables-save > /etc/iptables/rules.v4
```

### Windows Firewall

**Przez PowerShell:**
```powershell
# Wyświetlenie reguł
Get-NetFirewallRule

# Utworzenie reguły (zezwól na port 80)
New-NetFirewallRule -DisplayName "Allow HTTP" -Direction Inbound -Protocol TCP -LocalPort 80 -Action Allow

# Blokowanie portu
New-NetFirewallRule -DisplayName "Block MySQL" -Direction Inbound -Protocol TCP -LocalPort 3306 -Action Block
```

---

## 6) IDS/IPS (Intrusion Detection/Prevention System)

### IDS (Intrusion Detection System)

**Co to jest?** System wykrywania intruzów — **monitoruje** ruch i **alertuje** o podejrzanych działaniach.

**Typy:**
- **HIDS** (Host-based IDS) — na konkretnym hoście
- **NIDS** (Network-based IDS) — w sieci

**Przykłady:**
- **Snort** — popularny NIDS open source
- **OSSEC** — HIDS

### IPS (Intrusion Prevention System)

**Co to jest?** System zapobiegania intruzom — **blokuje** podejrzany ruch automatycznie.

**Różnica od IDS:** IPS nie tylko wykrywa, ale też **reaguje** (blokuje ruch).

**Przykłady:**
- **Snort IPS mode**
- **Suricata** — nowoczesny alternatywa dla Snort

---

## 7) Uwierzytelnianie

### Metody uwierzytelniania

#### Hasła
- **Proste** — łatwe w użyciu, ale słabe (łatwo złamać)
- **Zasady bezpieczeństwa:** długie, złożone, regularna zmiana

#### Certyfikaty cyfrowe
- **PKI** (Public Key Infrastructure) — infrastruktura kluczy publicznych
- **X.509** — standard certyfikatów
- Używane w SSL/TLS, e-mail (S/MIME), podpis cyfrowy

#### Dwuskładnikowe (2FA/MFA)
- **Co wiesz** (hasło) + **Co masz** (token, telefon)
- Przykłady: Google Authenticator, SMS kod, hardware token

#### Biometria
- Odcisk palca, rozpoznawanie twarzy, skan siatkówki
- Używane głównie lokalnie (nie przez sieć)

### Kerberos (uwierzytelnianie sieciowe)

**Co to jest?** Protokół uwierzytelniania oparty na biletach (tickets).

**Zalety:**
- Bez przesyłania haseł przez sieć
- Single Sign-On (SSO) — logowanie raz, dostęp do wielu usług

**Użycie:**
- Active Directory (Windows)
- Środowiska enterprise

---

## 8) Bezpieczne protokoły sieciowe

### Porównanie bezpiecznych vs niebezpiecznych protokołów

| Protokół | Bezpieczny | Niebezpieczny | Alternatywa |
|----------|-----------|---------------|-------------|
| **HTTP** | ❌ | ✅ | **HTTPS** (HTTP + TLS) |
| **FTP** | ❌ | ✅ | **SFTP** (SSH File Transfer) lub **FTPS** (FTP + TLS) |
| **Telnet** | ❌ | ✅ | **SSH** |
| **POP3/IMAP** | ❌ | ✅ | **POP3S/IMAPS** (+TLS) |
| **SMTP** | ❌ (bez TLS) | ⚠️ | **SMTPS** (+TLS) lub **STARTTLS** |
| **RDP** | ⚠️ | ⚠️ | Użyj VPN + RDP lub **RDP z TLS** |

**Zasada:** Używaj zawsze szyfrowanych wersji protokołów w środowisku produkcyjnym.

---

## 9) Best Practices (najlepsze praktyki)

### Ogólne zasady bezpieczeństwa

1. **Szyfruj komunikację** — używaj HTTPS, SSH, VPN
2. **Silne hasła** — długie, złożone, regularnie zmieniane
3. **Aktualizacje** — regularnie aktualizuj system i oprogramowanie
4. **Firewall** — blokuj niepotrzebne porty
5. **Wyłącz nieużywane usługi** — mniejsza powierzchnia ataku
6. **Backup** — regularne kopie zapasowe
7. **Monitoring** — używaj IDS/IPS, logi
8. **Zasada najmniejszych uprawnień** — użytkownicy tylko z niezbędnymi prawami
9. **2FA/MFA** — dwuskładnikowe uwierzytelnianie gdzie to możliwe
10. **Audyt bezpieczeństwa** — regularne sprawdzanie konfiguracji

### Security Hardening (utwardzanie bezpieczeństwa)

**Linux:**
```bash
# Wyłącz niepotrzebne usługi
sudo systemctl disable apache2
sudo systemctl stop apache2

# Automatyczne aktualizacje bezpieczeństwa
sudo apt install unattended-upgrades

# Fail2ban (ochrona przed brute force)
sudo apt install fail2ban
```

**Windows:**
- Włącz Windows Defender
- Aktualizacje automatyczne
- Wyłącz nieużywane role/features
- Użyj BitLocker (szyfrowanie dysków)

---

## Kluczowe zdania na obronę

1. **Bezpieczna komunikacja** zapewnia: poufność (szyfrowanie), integralność (hash), uwierzytelnienie (certyfikaty), nieodparcie (podpisy cyfrowe).

2. **Szyfrowanie symetryczne** (AES) — szybkie, ten sam klucz do szyfrowania/deszyfrowania. **Szyfrowanie asymetryczne** (RSA) — para kluczy (publiczny/prywatny), bezpieczniejsza dystrybucja.

3. **SSL/TLS** to protokoły szyfrujące komunikację (HTTPS, FTPS) — TLS 1.2/1.3 to aktualne, bezpieczne wersje.

4. **HTTPS** to HTTP szyfrowane przez TLS — zapewnia szyfrowanie i weryfikację tożsamości serwera (certyfikat X.509).

5. **VPN** tworzy zaszyfrowany tunel przez Internet — protokoły: IPSec, OpenVPN, WireGuard (najnowszy, najszybszy).

6. **SSH** to bezpieczny protokół dostępu zdalnego (port 22) — uwierzytelnianie przez hasło lub klucz publiczny (rekomendowane).

7. **Firewall** kontroluje dostęp do sieci na podstawie reguł — blokuje/zezwala na ruch (porty, protokoły, adresy IP).

8. **IDS** (Intrusion Detection System) wykrywa i alertuje o atakach. **IPS** (Intrusion Prevention System) również blokuje podejrzany ruch.

9. **Uwierzytelnianie:** hasła, certyfikaty cyfrowe (PKI), 2FA/MFA (dwuskładnikowe), Kerberos (bilety).

10. **Bezpieczne protokoły:** HTTPS (nie HTTP), SFTP (nie FTP), SSH (nie Telnet), POP3S/IMAPS (nie POP3/IMAP).

11. **Best practices:** szyfruj komunikację, silne hasła, aktualizacje, firewall, wyłącz nieużywane usługi, monitoring, zasada najmniejszych uprawnień.

12. **Hybrid Encryption** łączy zalety obu: asymetryczne (RSA) do wymiany klucza, symetryczne (AES) do szyfrowania danych — szybkość + bezpieczeństwo dystrybucji.
