# Mechanizmy zdalnego dostępu do zasobów sieciowych — Notatka do Obrony

## 0) Podstawy — od zera

### Co to jest zdalny dostęp do zasobów?
Zdalny dostęp do zasobów sieciowych oznacza korzystanie z plików, folderów, drukarek i innych zasobów znajdujących się na innych komputerach w sieci, tak jakby były lokalne.

### Przykłady zasobów sieciowych
- **Dyski sieciowe** — foldery udostępnione na innym komputerze
- **Drukarki sieciowe** — drukarki dostępne przez sieć
- **Bazy danych** — dostęp do danych na serwerze
- **Aplikacje** — usługi działające na serwerach

### Dlaczego zdalny dostęp?
- **Centralizacja danych** — wszystkie pliki w jednym miejscu
- **Współdzielenie zasobów** — wielu użytkowników może korzystać z tych samych danych
- **Backup** — łatwiejsze tworzenie kopii zapasowych
- **Zarządzanie** — kontrolowanie dostępu z centralnego miejsca

---

## 1) Dyski sieciowe

### Co to jest dysk sieciowy?
Dysk sieciowy to folder lub dysk udostępniony na innym komputerze w sieci, który można zamontować (przypisać literę dysku) i używać jak lokalnego dysku.

### Protokoły dostępu do dysków sieciowych

#### SMB/CIFS (Windows/Linux)
**SMB** (Server Message Block) / **CIFS** (Common Internet File System) — protokół używany głównie w środowiskach Windows.

- Porty: 445 (SMB), 139 (NetBIOS)
- Używany w: Windows, Linux (Samba), macOS
- **Zastosowanie:** Udostępnianie plików i drukarek w sieci lokalnej

#### NFS (Network File System)
**NFS** (Network File System) — protokół używany głównie w środowiskach Unix/Linux.

- Port: 2049
- Używany w: Linux, Unix, macOS
- **Zastosowanie:** Wydajny dostęp do plików w środowiskach Unix/Linux

### Serwer NAS (Network Attached Storage)

#### Co to jest NAS?
**NAS** to dedykowane urządzenie przechowujące dane i udostępniające je przez sieć za pomocą protokołów SMB/CIFS, NFS, FTP.

**Cechy:**
- **Dedykowany system** — specjalizowany system operacyjny (często oparty na Linux)
- **Wieloprotokołowy** — wspiera SMB, NFS, FTP, AFP
- **RAID** — często ma wbudowaną obsługę macierzy RAID
- **Zarządzanie przez web** — konfiguracja przez interfejs WWW

**Zastosowania:**
- Centralne przechowywanie danych w domu/biurze
- Backup automatyczny
- Media server (filmy, muzyka)
- Współdzielenie plików w sieci lokalnej

**Przykłady systemów NAS:**
- **Synology** — DiskStation Manager (DSM)
- **QNAP** — QTS
- **FreeNAS/TrueNAS** — open source (FreeBSD)
- **OpenMediaVault** — open source (Debian)

**Montowanie NAS:**
```bash
# SMB/CIFS (tak samo jak zwykły serwer)
sudo mount -t cifs //nas.local/share /mnt/nas -o username=user,password=pass

# NFS
sudo mount -t nfs nas.local:/volume1/share /mnt/nas
```

### Montowanie dysków sieciowych w Windows

#### Przez GUI (Eksplorator plików)
```
Eksplorator plików → Ten komputer → Mapuj dysk sieciowy
→ Wybierz literę dysku → Podaj ścieżkę \\serwer\share
```

**Przykład ścieżki:** `\\192.168.1.10\share` lub `\\serwer-firmowy\dokumenty`

#### Przez PowerShell
```powershell
# Mapowanie dysku z literą Z:
New-PSDrive -Name "Z" -PSProvider FileSystem -Root "\\192.168.1.10\share" -Persist

# Mapowanie z autoryzacją
net use Z: \\192.168.1.10\share /user:domena\uzytkownik haslo /persistent:yes

# Usunięcie mapowania
net use Z: /delete
```

#### Przez wiersz polecenia (CMD)
```cmd
# Mapowanie dysku
net use Z: \\serwer\share /persistent:yes

# Mapowanie z użytkownikiem
net use Z: \\serwer\share /user:domena\uzytkownik haslo

# Wyświetlenie wszystkich mapowań
net use

# Usunięcie mapowania
net use Z: /delete

# Usunięcie wszystkich mapowań
net use * /delete
```

### Montowanie dysków sieciowych w Linux

#### SMB/CIFS (Samba)
```bash
# Instalacja narzędzi
sudo apt install cifs-utils  # Debian/Ubuntu
sudo yum install cifs-utils  # RHEL/CentOS

# Montowanie jednorazowe
sudo mount -t cifs //192.168.1.10/share /mnt/network -o username=uzytkownik,password=haslo

# Montowanie z /etc/fstab (trwałe)
# Dodaj do /etc/fstab:
//192.168.1.10/share /mnt/network cifs username=uzytkownik,password=haslo,uid=1000,gid=1000,iocharset=utf8 0 0

# Następnie zamontuj:
sudo mount -a
```

#### NFS (Network File System)
```bash
# Instalacja
sudo apt install nfs-common  # Debian/Ubuntu

# Montowanie jednorazowe
sudo mount -t nfs 192.168.1.10:/export/share /mnt/nfs

# Montowanie z /etc/fstab (trwałe)
# Dodaj do /etc/fstab:
192.168.1.10:/export/share /mnt/nfs nfs defaults 0 0

# Zamontuj:
sudo mount -a

# Odmontowanie
sudo umount /mnt/nfs
```

### Udostępnianie folderów (serwer)

#### Windows — udostępnianie folderu
```
Kliknij prawym na folder → Właściwości → Zakładka "Udostępnianie"
→ Udostępnij → Wybierz użytkowników i uprawnienia
```

**Przez PowerShell:**
```powershell
# Udostępnienie folderu
New-SmbShare -Name "share" -Path "C:\share" -FullAccess "Everyone"

# Zmiana uprawnień
Grant-SmbShareAccess -Name "share" -AccountName "DOMENA\uzytkownik" -AccessRight Full

# Wyświetlenie udostępnień
Get-SmbShare
```

#### Linux — Samba (udostępnianie SMB/CIFS)
```bash
# Instalacja Samby
sudo apt install samba

# Konfiguracja (/etc/samba/smb.conf)
[share]
    path = /home/share
    valid users = uzytkownik
    read only = no
    browseable = yes

# Utworzenie użytkownika Samba
sudo smbpasswd -a uzytkownik

# Restart usługi
sudo systemctl restart smbd
```

#### Linux — NFS (udostępnianie NFS)
```bash
# Instalacja
sudo apt install nfs-kernel-server

# Konfiguracja (/etc/exports)
/home/share 192.168.1.0/24(rw,sync,no_subtree_check)

# Eksport udostępnień
sudo exportfs -ra
sudo systemctl restart nfs-kernel-server
```

---

## 1b) Protokoły transferu plików (FTP, SFTP, FTPS)

### FTP (File Transfer Protocol)

#### Co to jest FTP?
**FTP** to protokół do transferu plików między komputerami przez sieć.

**Charakterystyka:**
- Porty: 21 (kontrola), 20 (dane w trybie aktywnym)
- **Bez szyfrowania** — hasła i dane przesyłane w formie tekstowej (niebezpieczne!)
- Tryby: aktywny (Active) i pasywny (Passive)

**Instalacja serwera FTP (Linux - vsftpd):**
```bash
# Instalacja
sudo apt install vsftpd

# Konfiguracja (/etc/vsftpd.conf)
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES

# Restart
sudo systemctl restart vsftpd
```

**Użycie FTP (klient):**
```bash
# Połączenie
ftp ftp.server.com

# LUB przez wiersz polecenia
ftp://user:pass@ftp.server.com/path/

# Narzędzia graficzne: FileZilla, WinSCP
```

### SFTP (SSH File Transfer Protocol)

#### Co to jest SFTP?
**SFTP** to bezpieczny protokół transferu plików działający przez SSH (Secure Shell).

**Charakterystyka:**
- Port: 22 (SSH)
- **Szyfrowany** — wszystkie dane są szyfrowane
- Używa SSH do transportu (nie wymaga osobnego serwera)

**Zalety SFTP nad FTP:**
- ✅ Szyfrowanie danych i haseł
- ✅ Integracja z SSH (bez osobnej konfiguracji)
- ✅ Jeden port (22) zamiast dwóch
- ✅ Bardziej bezpieczny

**Użycie SFTP:**
```bash
# Połączenie interaktywne
sftp user@server.com

# Komendy w SFTP:
sftp> put lokalny_plik.txt
sftp> get zdalny_plik.txt
sftp> ls
sftp> cd /path
sftp> quit

# LUB przez wiersz polecenia (scp - część SSH)
scp file.txt user@server.com:/path/
scp -r folder/ user@server.com:/path/
```

**Windows - klient SFTP:**
- **WinSCP** — graficzny klient SFTP/SCP
- **FileZilla** — wspiera SFTP
- **PuTTY** — pscp.exe (linia poleceń)

### FTPS (FTP over SSL/TLS)

#### Co to jest FTPS?
**FTPS** to FTP z szyfrowaniem SSL/TLS (FTP Secure).

**Charakterystyka:**
- Porty: 990 (implicit FTPS), 21 + 989 (explicit FTPS)
- **Szyfrowany** — używa SSL/TLS do szyfrowania połączenia
- Dwie wersje: **Implicit FTPS** (połączenie od razu szyfrowane) i **Explicit FTPS** (STARTTLS)

**Konfiguracja FTPS (vsftpd):**
```bash
# Konfiguracja (/etc/vsftpd.conf)
ssl_enable=YES
rsa_cert_file=/etc/ssl/certs/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.key
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
```

### Porównanie FTP, SFTP, FTPS

| Cecha | FTP | SFTP | FTPS |
|-------|-----|------|------|
| **Szyfrowanie** | ❌ Nie | ✅ Tak (SSH) | ✅ Tak (SSL/TLS) |
| **Port** | 21, 20 | 22 (SSH) | 990, 989 |
| **Bezpieczeństwo** | Niskie | Wysokie | Wysokie |
| **Komplikacja** | Proste | Średnie | Średnie |
| **Zgodność** | Wszystkie systemy | Wymaga SSH | Wymaga certyfikatów |
| **Zastosowanie** | ❌ Niekorzystane (niebezpieczne) | ✅ Rekomendowane | ⚠️ Rzadko używane |

**Rekomendacja:** Używaj **SFTP** zamiast FTP (bezpieczniejsze i prostsze w konfiguracji).

---

## 2) Mapowanie uprawnień dostępu

### Co to są uprawnienia dostępu?
Uprawnienia określają, kto i w jaki sposób może korzystać z zasobów sieciowych:
- **Odczyt** (Read) — można przeglądać pliki
- **Zapis** (Write) — można modyfikować pliki
- **Wykonanie** (Execute) — można uruchamiać pliki/programy
- **Usuwanie** (Delete) — można usuwać pliki

### Modele uprawnień

#### ACL (Access Control List) — lista kontroli dostępu
Lista określająca uprawnienia dla poszczególnych użytkowników lub grup.

**Przykład ACL:**
- Użytkownik "jan" — odczyt/zapis
- Grupa "pracownicy" — odczyt
- Użytkownik "admin" — pełne uprawnienia

#### Uprawnienia Unix/Linux (rwx)
- **r** (read) — odczyt (4)
- **w** (write) — zapis (2)
- **x** (execute) — wykonanie (1)

**Grupy uprawnień:**
- **Właściciel** (owner) — użytkownik, który stworzył plik
- **Grupa** (group) — grupa użytkowników
- **Inni** (others) — pozostali użytkownicy

**Przykład:** `rwxr-xr--` = 754 (właściciel: rwx=7, grupa: r-x=5, inni: r--=4)

### Uprawnienia w Windows

#### Uprawnienia NTFS
```
Kliknij prawym na folder → Właściwości → Zakładka "Zabezpieczenia"
→ Edytuj → Dodaj użytkownika/grupę → Ustaw uprawnienia
```

**Typy uprawnień NTFS:**
- **Pełna kontrola** (Full Control)
- **Modyfikacja** (Modify)
- **Odczyt i wykonanie** (Read & Execute)
- **Odczyt** (Read)
- **Zapis** (Write)

**Przez PowerShell:**
```powershell
# Sprawdzenie uprawnień
Get-Acl "C:\share" | Format-List

# Ustawienie uprawnień
$acl = Get-Acl "C:\share"
$permission = "DOMENA\uzytkownik", "FullControl", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
Set-Acl "C:\share" $acl
```

#### Uprawnienia do udostępnień SMB
**Poziomy uprawnień:**
- **Pełna kontrola**
- **Zmiana** (Change)
- **Odczyt** (Read)

**Uwaga:** Uprawnienia udostępnienia SMB działają w połączeniu z uprawnieniami NTFS (zastosowanie bardziej restrykcyjnego).

### Uprawnienia w Linux

#### Uprawnienia plików/folderów (chmod)
```bash
# Zmiana uprawnień (symbolicznie)
chmod u+r file.txt        # właściciel: +odczyt
chmod g+w file.txt        # grupa: +zapis
chmod o-x file.txt        # inni: -wykonanie
chmod u=rwx,g=rx,o=r file.txt  # wszystkie

# Zmiana uprawnień (numerycznie)
chmod 755 file.txt        # rwxr-xr-x
chmod 644 file.txt        # rw-r--r--
chmod 600 file.txt        # rw------- (tylko właściciel)

# Rekurencyjnie (wszystkie pliki w folderze)
chmod -R 755 /home/share
```

#### Własność plików (chown)
```bash
# Zmiana właściciela
sudo chown uzytkownik file.txt

# Zmiana właściciela i grupy
sudo chown uzytkownik:grupa file.txt

# Rekurencyjnie
sudo chown -R uzytkownik:grupa /home/share
```

#### ACL w Linux (setfacl/getfacl)
```bash
# Instalacja narzędzi ACL
sudo apt install acl

# Ustawienie uprawnień dla użytkownika
setfacl -m u:uzytkownik:rwx /home/share

# Ustawienie uprawnień dla grupy
setfacl -m g:grupa:rx /home/share

# Wyświetlenie ACL
getfacl /home/share

# Usunięcie wszystkich wpisów ACL
setfacl -b /home/share
```

### Mapowanie uprawnień między systemami

#### Samba — mapowanie użytkowników
```bash
# Konfiguracja (/etc/samba/smb.conf)
[global]
    # Mapowanie użytkownika root na administratora
    root = administrator
    
    # Mapowanie użytkowników
    username map = /etc/samba/users.map

# Plik /etc/samba/users.map
admin = Administrator
user1 = jan
```

#### Uprawnienia w Samba
```bash
# Konfiguracja (/etc/samba/smb.conf)
[share]
    path = /home/share
    valid users = uzytkownik1, uzytkownik2
    read list = uzytkownik1          # tylko odczyt
    write list = uzytkownik2         # odczyt i zapis
    force user = www-data            # wszystkie pliki jako www-data
    force group = www-data
    create mask = 0644               # uprawnienia dla nowych plików
    directory mask = 0755            # uprawnienia dla nowych folderów
```

---

## 3) Sieciowe zarządzanie użytkownikami (NIS/LDAP)

### Po co centralne zarządzanie użytkownikami?
- **Jeden katalog** — wszystkie konta użytkowników w jednym miejscu
- **Centralna autoryzacja** — logowanie na wielu maszynach z tym samym kontem
- **Ułatwienie zarządzania** — dodanie/zmiana użytkownika raz, działa wszędzie
- **Bezpieczeństwo** — scentralizowana polityka haseł, wygasanie kont

### NIS (Network Information Service)

#### Co to jest NIS?
**NIS** to starszy system katalogów sieciowych, używany głównie w środowiskach Unix/Linux do zarządzania użytkownikami, grupami, hostami.

**Alternatywne nazwy:**
- **NIS** (Network Information Service) — nowsza nazwa
- **YP** (Yellow Pages) — starsza nazwa (nie używana ze względów prawnych)

#### Architektura NIS
- **Serwer NIS** — przechowuje dane (mapy NIS)
- **Klienci NIS** — pobierają dane z serwera

**Domeny NIS:** Grupy komputerów dzielących te same dane (nie mylić z domenami Windows).

#### Instalacja i konfiguracja (Linux)

**Serwer NIS:**
```bash
# Instalacja
sudo apt install nis

# Konfiguracja domeny NIS
sudo domainname firma
echo "firma" | sudo tee /etc/defaultdomain

# Konfiguracja (/etc/ypserv.conf)
# Umożliwia dostęp klientów z sieci

# Inicjalizacja bazy NIS
sudo /usr/lib/yp/ypinit -m

# Restart usługi
sudo systemctl start ypserv
sudo systemctl enable ypserv
```

**Klient NIS:**
```bash
# Instalacja
sudo apt install nis

# Konfiguracja domeny
sudo domainname firma

# Konfiguracja (/etc/yp.conf)
domain firma server nis-server.firma.local

# Konfiguracja (/etc/nsswitch.conf)
# Zmień:
passwd: files nis
group: files nis
shadow: files nis
hosts: files dns nis

# Restart usługi
sudo systemctl start ypbind
sudo systemctl enable ypbind
```

#### Zalety i wady NIS

| Zalety | Wady |
|--------|------|
| Prosty w konfiguracji | Niskie bezpieczeństwo (hasła w formie tekstowej) |
| Szybki | Przestarzały (nie używany w nowych instalacjach) |
| Dobrze wspierany w starych systemach Unix | Ograniczone możliwości |
| | Brak szyfrowania danych |

**Uwaga:** NIS jest przestarzały i zastępowany przez LDAP/Active Directory.

### LDAP (Lightweight Directory Access Protocol)

#### Co to jest LDAP?
**LDAP** to protokół dostępu do katalogów, używany do przechowywania i zarządzania informacjami o użytkownikach, grupach, urządzeniach w strukturze hierarchicznej.

**Wersje:**
- **LDAPv2** — przestarzała
- **LDAPv3** — aktualna (RFC 4510)

#### Struktura LDAP

**DN (Distinguished Name)** — unikalna nazwa obiektu w drzewie LDAP

**Przykład DN:**
```
cn=Jan Kowalski,ou=users,dc=firma,dc=local
```

**Składniki:**
- `cn` (Common Name) — nazwa pospolita
- `ou` (Organizational Unit) — jednostka organizacyjna
- `dc` (Domain Component) — składnik domeny

**Struktura drzewa:**
```
dc=firma,dc=local
├── ou=users
│   ├── cn=Jan Kowalski
│   └── cn=Anna Nowak
├── ou=groups
│   └── cn=pracownicy
└── ou=computers
    └── cn=serwer1
```

#### Instalacja i konfiguracja LDAP (OpenLDAP)

**Serwer LDAP:**
```bash
# Instalacja
sudo apt install slapd ldap-utils

# Konfiguracja (interaktywna)
sudo dpkg-reconfigure slapd

# Utworzenie użytkownika (plik user.ldif)
dn: cn=Jan Kowalski,ou=users,dc=firma,dc=local
objectClass: inetOrgPerson
cn: Jan Kowalski
sn: Kowalski
uid: jkowalski
userPassword: {SSHA}haslo_zaszyfrowane

# Dodanie użytkownika
ldapadd -x -D "cn=admin,dc=firma,dc=local" -W -f user.ldif
```

**Klient LDAP (Linux):**
```bash
# Instalacja narzędzi
sudo apt install ldap-utils libnss-ldap libpam-ldap

# Konfiguracja (/etc/ldap/ldap.conf)
BASE dc=firma,dc=local
URI ldap://ldap-server.firma.local

# Konfiguracja (/etc/nsswitch.conf)
passwd: files ldap
group: files ldap
shadow: files ldap

# Konfiguracja PAM (/etc/pam.d/common-auth)
auth sufficient pam_ldap.so
```

#### Active Directory (implementacja LDAP Microsoft)

**Active Directory** to implementacja LDAP używana w środowiskach Windows.

**Komponenty:**
- **Domain Controller** — serwer AD
- **DNS** — wymagany dla AD
- **Kerberos** — uwierzytelnianie
- **LDAP** — katalog

**Integracja Linux z Active Directory:**
```bash
# Instalacja
sudo apt install realmd sssd sssd-tools adcli

# Przyłączenie do domeny
sudo realm discover domena.firma.local
sudo realm join -U administrator domena.firma.local

# Konfiguracja (/etc/sssd/sssd.conf)
[sssd]
domains = domena.firma.local

[domain/domena.firma.local]
ad_server = dc1.domena.firma.local
```

### Kerberos

#### Co to jest Kerberos?
**Kerberos** to protokół uwierzytelniania sieciowego, który umożliwia bezpieczne logowanie bez przesyłania haseł przez sieć.

**Charakterystyka:**
- Port: 88 (UDP/TCP)
- **Ticket-based** — używa biletów (tickets) do uwierzytelniania
- **Centralne uwierzytelnianie** — jeden serwer Kerberos dla całej sieci

**Komponenty:**
- **KDC** (Key Distribution Center) — serwer Kerberos (AS + TGS)
- **AS** (Authentication Server) — uwierzytelnianie użytkownika
- **TGS** (Ticket Granting Server) — wydawanie biletów do zasobów
- **Client** — klient żądający dostępu

**Przebieg uwierzytelniania:**
1. Klient żąda biletu TGT (Ticket Granting Ticket) z AS
2. AS sprawdza użytkownika i zwraca TGT (zaszyfrowany)
3. Klient używa TGT do żądania biletu serwisowego z TGS
4. TGS zwraca bilet serwisowy do konkretnego zasobu
5. Klient używa biletu serwisowego do dostępu do zasobu

**Zastosowanie:**
- Active Directory (Windows) używa Kerberos do uwierzytelniania
- Środowiska enterprise wymagające single sign-on (SSO)

### RADIUS (Remote Authentication Dial-In User Service)

#### Co to jest RADIUS?
**RADIUS** to protokół do centralnego uwierzytelniania, autoryzacji i rozliczania (AAA: Authentication, Authorization, Accounting).

**Charakterystyka:**
- Porty: 1812 (uwierzytelnianie), 1813 (rozliczanie)
- **Centralne zarządzanie** — jeden serwer RADIUS dla wielu urządzeń
- Używany głównie dla dostępu sieciowego (Wi-Fi, VPN, sieciowe urządzenia)

**Komponenty:**
- **RADIUS Server** — serwer uwierzytelniania
- **RADIUS Client** (NAS) — urządzenie sieciowe (router, access point, switch)
- **User Database** — baza użytkowników (może integrować z LDAP)

**Przebieg uwierzytelniania:**
1. Użytkownik próbuje połączyć się z siecią (np. Wi-Fi)
2. NAS (Access Point) przekazuje żądanie do serwera RADIUS
3. Serwer RADIUS sprawdza użytkownika w bazie
4. Serwer zwraca odpowiedź: Accept (pozwól) lub Reject (odrzuć)

**Konfiguracja RADIUS (FreeRADIUS - Linux):**
```bash
# Instalacja
sudo apt install freeradius freeradius-utils

# Konfiguracja użytkowników (/etc/freeradius/3.0/users)
jan Cleartext-Password := "haslo123"
    Reply-Message := "Witaj, Jan!"

# Test
radtest jan haslo123 localhost 0 testing123

# Restart
sudo systemctl restart freeradius
```

**Zastosowania:**
- Wi-Fi enterprise (802.1X)
- VPN (PPTP, L2TP/IPsec)
- Dial-up (starsze systemy)
- Kontrola dostępu do sieci

### Porównanie NIS vs LDAP vs Kerberos vs RADIUS

| Cecha | NIS | LDAP | Kerberos | RADIUS |
|-------|-----|------|----------|--------|
| **Cel** | Katalog sieciowy | Katalog sieciowy | Uwierzytelnianie | AAA (uwierzytelnianie/autoryzacja) |
| **Bezpieczeństwo** | ❌ Niskie | ✅ Wysokie | ✅ Wysokie | ✅ Wysokie |
| **Hasła** | Tekstowe | Zaszyfrowane | Bilety | Zaszyfrowane |
| **Zastosowanie** | Przestarzałe | Enterprise | SSO, Windows AD | Wi-Fi, VPN, sieć |
| **Integracja** | Unix/Linux | Wszystkie systemy | Często z LDAP | Często z LDAP |
| **Standard** | Przestarzały | RFC 4510 | RFC 4120 | RFC 2865 |

**Uwaga:** W środowiskach enterprise często używa się **LDAP + Kerberos** razem (LDAP dla katalogu, Kerberos dla uwierzytelniania) oraz **RADIUS** dla kontroli dostępu do sieci.

---

## Kluczowe zdania na obronę

1. **Dyski sieciowe** to zasoby udostępnione w sieci, które można zamontować jak lokalne dyski — protokoły: SMB/CIFS (Windows) i NFS (Linux/Unix).

2. **SMB/CIFS** używany jest głównie w Windows (port 445), **NFS** w Linux/Unix (port 2049).

3. **NAS** (Network Attached Storage) to dedykowane urządzenie przechowujące dane i udostępniające je przez sieć (SMB, NFS, FTP).

4. **FTP** — przestarzały, niebezpieczny (hasła w tekście), **SFTP** — bezpieczny (przez SSH, port 22), **FTPS** — FTP z SSL/TLS. **Rekomendacja: SFTP.**

5. **Montowanie dysków w Windows:** `net use Z: \\serwer\share`, w **Linux:** `mount -t cifs` lub `mount -t nfs`.

6. **Uprawnienia dostępu** określają, kto i jak może korzystać z zasobów — w **Windows:** ACL/NTFS, w **Linux:** rwx (chmod) i ACL (setfacl).

7. **Uprawnienia Unix/Linux:** r (odczyt=4), w (zapis=2), x (wykonanie=1) dla właściciela, grupy i innych — przykładowo `chmod 755`.

8. **NIS** (Network Information Service) to starszy system katalogów sieciowych dla Unix/Linux — prosty, ale niebezpieczny (hasła w formie tekstowej), przestarzały.

9. **LDAP** (Lightweight Directory Access Protocol) to nowoczesny protokół katalogów z hierarchiczną strukturą — używany w Active Directory, OpenLDAP.

10. **Active Directory** to implementacja LDAP Microsoft, używana do centralnego zarządzania użytkownikami w środowiskach Windows (używa LDAP + Kerberos).

11. **Kerberos** to protokół uwierzytelniania oparty na biletach (tickets) — używany w Active Directory, umożliwia single sign-on (SSO).

12. **RADIUS** to protokół AAA (Authentication, Authorization, Accounting) — używany głównie do kontroli dostępu do sieci (Wi-Fi enterprise, VPN).

13. **Zalety centralnego zarządzania:** jeden katalog użytkowników, centralna autoryzacja, łatwiejsze zarządzanie kontami, bezpieczeństwo.

14. **Różnica NIS vs LDAP:** NIS jest przestarzały i płaski, LDAP jest nowoczesny, hierarchiczny i bezpieczny — LDAP zastępuje NIS w nowoczesnych instalacjach.

15. **W środowiskach enterprise** często używa się **LDAP + Kerberos** (katalog + uwierzytelnianie) oraz **RADIUS** (kontrola dostępu do sieci).
