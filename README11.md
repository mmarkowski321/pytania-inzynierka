# Konfiguracja sieciowa systemów operacyjnych — Notatka do Obrony

## 0) Podstawy — od zera

### Co to jest konfiguracja sieciowa?
Konfiguracja sieciowa to proces ustawiania parametrów, dzięki którym komputer może komunikować się z innymi urządzeniami w sieci (lokalnej lub globalnej).

### Co jest potrzebne do działania sieci?
- **Karta sieciowa** — urządzenie fizyczne (wbudowana lub zewnętrzna)
- **Sterownik** — oprogramowanie, które "mówi" systemowi operacyjnemu, jak obsłużyć kartę
- **Parametry sieciowe** — IP, maska, brama, DNS
- **Protokoły sieciowe** — TCP/IP, UDP itp.

---

## 1) Sterowniki urządzeń sieciowych

### Co to jest sterownik (driver)?
Sterownik to program, który pozwala systemowi operacyjnemu komunikować się z urządzeniem sprzętowym (np. kartą sieciową).

**Analogia:** Sterownik = tłumacz między językiem systemu operacyjnego a językiem urządzenia.

### Rodzaje sterowników sieciowych

#### Windows
- **Sterowniki firmowe** — dostarczane przez producenta karty (np. Intel, Realtek)
- **Sterowniki Windows Update** — automatycznie pobierane przez system
- **Sterowniki generyczne** — podstawowe sterowniki systemowe

**Lokalizacja:** 
```
Zarządzanie urządzeniami → Karty sieciowe → [Nazwa karty] → Właściwości → Sterownik
```

**Sprawdzanie statusu:**
```powershell
Get-NetAdapter | Format-List Name, InterfaceDescription, Status, DriverVersion
```

#### Linux
- **Sterowniki w kernelu** — zintegrowane z systemem (np. `e1000` dla Intel)
- **Sterowniki modułowe** — ładowane jako moduły (`modprobe`)
- **Sterowniki firmowe** — często w repozytoriach lub ze strony producenta

**Sprawdzanie:**
```bash
# Lista interfejsów
ip link show
# LUB
ifconfig -a

# Sprawdzenie załadowanych modułów
lsmod | grep -i ethernet
# LUB
lspci | grep -i network
```

### Problemy ze sterownikami

**Brak sterownika:**
- Urządzenie widoczne jako "Unknown device" w Windows
- Interfejs sieciowy nie pojawia się w `ip link` w Linux

**Rozwiązanie:**
- Pobierz sterownik z oficjalnej strony producenta
- Użyj Windows Update (Windows)
- Zainstaluj przez menedżera pakietów (Linux): `apt install firmware-realtek` (Debian/Ubuntu)

**Niekompatybilny sterownik:**
- Karta działa, ale z błędami (np. zrywa połączenie)
- Niska wydajność

**Rozwiązanie:**
- Zainstaluj najnowszą wersję sterownika
- Sprawdź kompatybilność z wersją systemu operacyjnego

---

## 2) Ustawienia parametrów sieci lokalnej

### Podstawowe parametry sieci

#### Adres IP (Internet Protocol)
Unikalny identyfikator komputera w sieci.

**Przykłady:**
- `192.168.1.100` — adres IPv4
- `2001:0db8::1` — adres IPv6

**Klasa sieci lokalnej (prywatne):**
- `10.0.0.0/8` (10.0.0.0 - 10.255.255.255)
- `172.16.0.0/12` (172.16.0.0 - 172.31.255.255)
- `192.168.0.0/16` (192.168.0.0 - 192.168.255.255)

#### Maska podsieci (Subnet Mask)
Określa, która część adresu IP to numer sieci, a która numer hosta.

**Przykłady:**
- `255.255.255.0` = `/24` — pierwsze 3 oktety to sieć, ostatni to host
- `255.255.0.0` = `/16` — pierwsze 2 oktety to sieć

**Interpretacja:** Jeśli IP = `192.168.1.100` i maska = `255.255.255.0`, to:
- Sieć: `192.168.1.0`
- Host: `100`

#### Brama domyślna (Default Gateway)
Adres IP urządzenia (zwykle routera), przez które wychodzimy poza sieć lokalną.

**Przykład:** `192.168.1.1` — router Wi-Fi

**Po co:** Żeby wysłać pakiet do innej sieci, komputer wysyła go do bramy, a ta przekazuje dalej.

#### DNS (Domain Name System)
Serwery, które tłumaczą nazwy domenowe (np. `google.com`) na adresy IP (np. `142.250.191.14`).

**Popularne DNS:**
- Google: `8.8.8.8`, `8.8.4.4`
- Cloudflare: `1.1.1.1`, `1.0.0.1`
- OpenDNS: `208.67.222.222`, `208.67.220.220`

### Konfiguracja w Windows

#### Przez GUI (Interfejs graficzny)
```
Ustawienia → Sieć i Internet → Ethernet/Wi-Fi → Zmień opcje adaptera
→ Kliknij prawym na adapter → Właściwości → Protokół internetowy w wersji 4 (TCP/IPv4)
→ Użyj następującego adresu IP
```

**Przykład konfiguracji:**
- IP: `192.168.1.100`
- Maska: `255.255.255.0`
- Brama: `192.168.1.1`
- DNS: `8.8.8.8`, `8.8.4.4`

#### Przez PowerShell (automatyzacja)
```powershell
# Ustawienie statycznego IP
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.100 -PrefixLength 24 -DefaultGateway 192.168.1.1

# Ustawienie DNS
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 8.8.8.8,8.8.4.4

# Powrót do DHCP (automatycznego)
Set-NetIPInterface -InterfaceAlias "Ethernet" -Dhcp Enabled
Remove-NetIPAddress -InterfaceAlias "Ethernet" -Confirm:$false
```

#### Przez CMD
```cmd
netsh interface ip set address "Ethernet" static 192.168.1.100 255.255.255.0 192.168.1.1
netsh interface ip set dns "Ethernet" static 8.8.8.8
```

### Konfiguracja w Linux

#### Przez interfejs graficzny (GUI)
- **Ubuntu/Debian:** Ustawienia → Sieć → Opcje → IPv4 → Ręcznie
- **Fedora:** Ustawienia → Sieć → Zębatka → IPv4 → Ręcznie

#### Przez `ip` command (nowoczesne, zalecane)
```bash
# Ustawienie IP statycznego
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip route add default via 192.168.1.1

# Ustawienie DNS (edytuj /etc/resolv.conf lub użyj systemd-resolved)
echo "nameserver 8.8.8.8" | sudo tee -a /etc/resolv.conf
```

#### Przez `ifconfig` (starsze, ale nadal używane)
```bash
sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0 up
sudo route add default gw 192.168.1.1
```

#### Przez pliki konfiguracyjne (trwałe ustawienia)

**Ubuntu/Debian (Netplan):**
```yaml
# /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```

**Po edycji:**
```bash
sudo netplan apply
```

**Fedora/RHEL/CentOS (NetworkManager - nmcli):**
```bash
# Ustawienie IP statycznego
sudo nmcli connection modify "System eth0" ipv4.addresses 192.168.1.100/24
sudo nmcli connection modify "System eth0" ipv4.gateway 192.168.1.1
sudo nmcli connection modify "System eth0" ipv4.dns "8.8.8.8 8.8.4.4"
sudo nmcli connection modify "System eth0" ipv4.method manual
sudo nmcli connection up "System eth0"
```

### DHCP vs konfiguracja statyczna

**DHCP (Dynamic Host Configuration Protocol):**
- Automatyczne przydzielanie IP, maski, bramy, DNS
- Używane w większości sieci domowych i firmowych
- **Zalety:** Łatwe, bez konfiguracji ręcznej
- **Wady:** IP może się zmieniać

**Konfiguracja statyczna:**
- Ręczne ustawienie wszystkich parametrów
- Używane dla serwerów, urządzeń infrastruktury
- **Zalety:** Stałe IP, przewidywalność
- **Wady:** Konieczność ręcznej konfiguracji

---

## 3) Ustawienia TCP

### Co to jest TCP?
TCP (Transmission Control Protocol) to protokół warstwy transportowej, który zapewnia **niezawodną, uporządkowaną transmisję danych**.

**Cechy TCP:**
- **Połączeniowy** — wymaga nawiązania połączenia (3-way handshake)
- **Niezawodny** — potwierdza odbiór pakietów, retransmituje w razie błędu
- **Uporządkowany** — pakiety przychodzą w kolejności
- **Kontrola przepływu** — dostosowuje prędkość do odbiorcy

### Ważne parametry TCP

#### Window Size (Rozmiar okna)
Ilość danych (w bajtach), jaką nadawca może wysłać bez oczekiwania na potwierdzenie.

**Wpływ na wydajność:**
- Większe okno = większa przepustowość (dla szybkich łączy)
- Mniejsze okno = bezpieczniejsze (dla wolnych/niepewnych łączy)

**Domyślne wartości:**
- Windows: ~64 KB (może być większe w nowszych wersjach)
- Linux: ~64 KB (może być większe z tunning)

#### MSS (Maximum Segment Size)
Maksymalny rozmiar segmentu TCP (bez nagłówków).

**Typowa wartość:** 1460 bajtów (dla Ethernet: 1500 - 20 IP - 20 TCP = 1460)

#### Timeout (RTO - Retransmission Timeout)
Czas oczekiwania na potwierdzenie przed retransmisją.

**Domyślna wartość:** ~1 sekunda (dynamicznie dostosowywany)

#### Nagle Algorithm
Algorytm, który łączy małe segmenty w jeden pakiet, aby zmniejszyć liczbę pakietów.

**Zaleta:** Mniej pakietów = mniejsze obciążenie sieci
**Wada:** Opóźnienie dla małych danych (np. telnet, SSH)

### Konfiguracja TCP w Windows

**Sprawdzenie aktualnych ustawień:**
```powershell
netsh int tcp show global
```

**Ważne parametry:**
```powershell
# Włącz Chimney Offload (przyspieszenie przez kartę sieciową)
netsh int tcp set global chimney=enabled

# Włącz Receive Window Scaling (większe okna)
netsh int tcp set global autotuninglevel=normal

# Włącz Nagle Algorithm (opcjonalnie)
netsh int tcp set global nagle=enabled

# Wyłącz Nagle (dla niskiego opóźnienia, np. gry)
netsh int tcp set global nagle=disabled
```

**Ustawienie rozmiaru okna (TCP Window Size):**
```powershell
# Dla interfejsu "Ethernet"
netsh int tcp set global autotuninglevel=normal
```

**Przykład optymalizacji dla szybkiego łącza:**
```powershell
netsh int tcp set global autotuninglevel=normal
netsh int tcp set global chimney=enabled
netsh int tcp set global rss=enabled
netsh int tcp set global netdma=enabled
```

### Konfiguracja TCP w Linux

**Sprawdzenie aktualnych ustawień:**
```bash
# Wszystkie parametry TCP
sysctl -a | grep tcp

# Ważne parametry
cat /proc/sys/net/ipv4/tcp_window_scaling
cat /proc/sys/net/ipv4/tcp_rmem
cat /proc/sys/net/ipv4/tcp_wmem
```

**Ustawienie parametrów (tymczasowo):**
```bash
# Włącz window scaling
sudo sysctl -w net.ipv4.tcp_window_scaling=1

# Ustaw rozmiary buforów (min default max)
sudo sysctl -w net.ipv4.tcp_rmem="4096 87380 4194304"
sudo sysctl -w net.ipv4.tcp_wmem="4096 65536 4194304"

# Wyłącz Nagle (dla niskiego opóźnienia)
sudo sysctl -w net.ipv4.tcp_nodelay=1

# Ustaw keepalive timeout
sudo sysctl -w net.ipv4.tcp_keepalive_time=600
```

**Ustawienie parametrów (trwale - w pliku `/etc/sysctl.conf`):**
```bash
# Edytuj plik
sudo nano /etc/sysctl.conf

# Dodaj linie:
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_rmem = 4096 87380 4194304
net.ipv4.tcp_wmem = 4096 65536 4194304
net.ipv4.tcp_nodelay = 1
net.ipv4.tcp_keepalive_time = 600

# Zastosuj zmiany
sudo sysctl -p
```

**Przykład optymalizacji dla serwera WWW:**
```bash
# Większe bufory
sudo sysctl -w net.ipv4.tcp_rmem="4096 131072 16777216"
sudo sysctl -w net.ipv4.tcp_wmem="4096 131072 16777216"

# Więcej połączeń jednocześnie
sudo sysctl -w net.core.somaxconn=4096
sudo sysctl -w net.ipv4.tcp_max_syn_backlog=4096
```

### Monitoring połączeń TCP

**Windows:**
```cmd
# Lista aktywnych połączeń TCP
netstat -an | findstr TCP

# LUB PowerShell
Get-NetTCPConnection | Format-Table LocalAddress, LocalPort, RemoteAddress, RemotePort, State
```

**Linux:**
```bash
# Lista aktywnych połączeń TCP
netstat -an | grep TCP
# LUB
ss -t -a

# Statystyki TCP
cat /proc/net/sockstat
ss -s
```

---

## 4) Automatyzacja konfiguracji

### Po co automatyzacja?
- **Osłędność czasu** — nie konfigurujesz każdego urządzenia ręcznie
- **Spójność** — wszystkie maszyny mają identyczną konfigurację
- **Skalowalność** — łatwe zarządzanie setkami urządzeń
- **Bezpieczeństwo** — mniej błędów ludzkich

### Metody automatyzacji

#### 1. DHCP (Dynamic Host Configuration Protocol)

**Najprostsza automatyzacja** — automatyczne przydzielanie IP, maski, bramy, DNS.

**Przykład konfiguracji DHCP serwera (Linux - isc-dhcp-server):**
```bash
# /etc/dhcp/dhcpd.conf
subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  default-lease-time 3600;
  max-lease-time 7200;
}
```

**Klient automatycznie otrzymuje:**
- IP z zakresu 192.168.1.100-200
- Brama: 192.168.1.1
- DNS: 8.8.8.8, 8.8.4.4

#### 2. Skrypty (Bash/PowerShell)

**Przykład skryptu Bash (Linux):**
```bash
#!/bin/bash
# configure_network.sh

INTERFACE="eth0"
IP="192.168.1.100"
NETMASK="255.255.255.0"
GATEWAY="192.168.1.1"
DNS1="8.8.8.8"
DNS2="8.8.4.4"

# Ustaw IP
sudo ip addr add $IP/24 dev $INTERFACE
sudo ip link set $INTERFACE up

# Ustaw bramę
sudo ip route add default via $GATEWAY

# Ustaw DNS
echo "nameserver $DNS1" | sudo tee /etc/resolv.conf
echo "nameserver $DNS2" | sudo tee -a /etc/resolv.conf

echo "Konfiguracja sieci zakończona!"
```

**Przykład skryptu PowerShell (Windows):**
```powershell
# configure_network.ps1

$InterfaceAlias = "Ethernet"
$IP = "192.168.1.100"
$PrefixLength = 24
$Gateway = "192.168.1.1"
$DNS = @("8.8.8.8", "8.8.4.4")

# Usuń istniejącą konfigurację
Remove-NetIPAddress -InterfaceAlias $InterfaceAlias -Confirm:$false -ErrorAction SilentlyContinue

# Ustaw nową konfigurację
New-NetIPAddress -InterfaceAlias $InterfaceAlias -IPAddress $IP -PrefixLength $PrefixLength -DefaultGateway $Gateway
Set-DnsClientServerAddress -InterfaceAlias $InterfaceAlias -ServerAddresses $DNS

Write-Host "Konfiguracja sieci zakończona!"
```

#### 3. Narzędzia Infrastructure as Code (IaC)

**Ansible (przykład):**
```yaml
# network_config.yml
---
- name: Configure network
  hosts: all
  tasks:
    - name: Set static IP
      win_shell: |
        New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.100 -PrefixLength 24 -DefaultGateway 192.168.1.1
    
    - name: Set DNS
      win_shell: |
        Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 8.8.8.8,8.8.4.4
```

**Dla Linux:**
```yaml
- name: Configure network (Linux)
  hosts: all
  tasks:
    - name: Configure IP
      ansible.builtin.command:
        cmd: ip addr add 192.168.1.100/24 dev eth0
      become: yes
    
    - name: Configure DNS
      ansible.builtin.lineinfile:
        path: /etc/resolv.conf
        line: "nameserver 8.8.8.8"
```

#### 4. Group Policy (Windows — domeny)

W środowisku domenowym Windows można skonfigurować sieć przez Group Policy:

```
Grupowe zasady → Konfiguracja komputera → Preferencje → Ustawienia kontrolera → Konfiguracja sieci
```

Umożliwia automatyczne ustawienie IP, maski, bramy, DNS dla wszystkich komputerów w domenie.

#### 5. Systemd-networkd (Linux — nowoczesne dystrybucje)

**Przykład automatycznej konfiguracji:**
```ini
# /etc/systemd/network/20-ethernet.network
[Match]
Name=eth0

[Network]
Address=192.168.1.100/24
Gateway=192.168.1.1
DNS=8.8.8.8
DNS=8.8.4.4
```

Po zapisaniu pliku system automatycznie zastosuje konfigurację.

#### 6. Cloud-init (chmura/wirtualizacja)

Automatyczna konfiguracja przy pierwszym uruchomieniu maszyny wirtualnej.

**Przykład konfiguracji:**
```yaml
# user-data
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
```

### Best practices automatyzacji

1. **Używaj DHCP** dla stacji roboczych (chyba że potrzebujesz statycznego IP)
2. **Dokumentuj konfiguracje** — wiedz, co i dlaczego ustawiłeś
3. **Testuj skrypty** na maszynie testowej przed wdrożeniem
4. **Używaj version control** (Git) dla skryptów konfiguracyjnych
5. **Twórz backup** przed zmianą konfiguracji sieci
6. **Zapisz plan powrotu** — jak wrócić do poprzedniej konfiguracji, jeśli coś pójdzie nie tak

---

## Kluczowe zdania na obronę

1. **Sterownik sieciowy** to oprogramowanie, które pozwala systemowi operacyjnemu komunikować się z kartą sieciową.

2. **Podstawowe parametry sieci:** IP (adres komputera), maska podsieci (określa sieć), brama domyślna (wyjście poza sieć), DNS (tłumaczy nazwy na IP).

3. **DHCP** automatyzuje konfigurację sieci — komputer automatycznie otrzymuje IP, maskę, bramę i DNS.

4. **Konfiguracja statyczna** to ręczne ustawienie wszystkich parametrów — używana dla serwerów i urządzeń infrastruktury.

5. **TCP** to protokół zapewniający niezawodną, uporządkowaną transmisję danych z kontrolą przepływu.

6. **Ważne parametry TCP:** Window Size (ile danych bez potwierdzenia), MSS (maksymalny segment), timeout (czas na retransmisję), Nagle Algorithm (łączenie małych pakietów).

7. **Automatyzacja konfiguracji** oszczędza czas, zapewnia spójność i skalowalność — używamy DHCP, skryptów, Ansible, Group Policy, systemd-networkd.

8. **W Windows** konfigurujemy przez GUI (Ustawienia), PowerShell (`New-NetIPAddress`) lub `netsh`.

9. **W Linux** konfigurujemy przez `ip`/`ifconfig`, pliki konfiguracyjne (netplan, NetworkManager) lub systemd-networkd.

10. **Przed zmianą konfiguracji sieci** zawsze rób backup i miej plan powrotu!
