# Linux

> Alle Angaben sind ohne Gewähr von Richtigkeit und Vollständigkeit!

## 4. Systemadministration

Linux = Mehrbenutzersystem = Gruppensystem
- komplexes Rechte und Gruppensystem
- ein Nutzer mehrere Gruppen

```bash
useradd <name>
groupadd <name>
userdel <name>
groupdel <name>
id # zeigt Nutzer/Gruppen ID des aktuellen Nutzers an
groups # alle Gruppen des Nutzers
passwd # Passwort ändern
```

Beim Erstellen eines Nutzers/ einer Gruppe werden Einstellungen übernommen: `adduser -> /etc/adduser.conf` und `addgroup -> /etc/addgroup.conf`

Administrator = superuser = root

| Superuser | uneingeschränkte Rechte |
|----------------|---------------------------------------------------------|
| Standardnutzer | für normale Arbeit Rechte auf eigene Dateien begrenzt |
| Systembenutzer | nur für Dienste und Server ohne menschliche Interaktion |

`/etc/passwd` Liste aller Nutzer mit wichtigen Konfigurationsinformationen
- zeilenweiser Aufbau: `Login:Passwort:U(ser)Id:G(roup)Id:Name:Nutzerordner:Kommando`
- Beispiel: `alex:x:502:100:Alex a:/home/alex:/bin/bash`
- "Kommando" wird nach Login ausgeführt
 
 #### Passwörter
- nur aus ASCII-Zeichen
- nicht zu leicht, beliebig lang
- Speicherung in Hash von in `/etc/shadow` z.B. SHA512
- Datei nicht für normale Nutzer lesbar

#### PLUGGABLE AUTHENTIFICATION MODULES (PAM)
- Bibliothek für Authentifizierungsaufgaben

#### Zugriffsrechte auf Dateien
- traditionell: Information für Zugriff mit jeder Datei gespeichert
- Zugriffsbits für Lese-, Schreib- und Ausführungsberechtigung
- Access Control Lists = ACL

Zugriffsberechtigung `rwxrw-r--`

| Zeichen | Bedeutung | Oktal |
|---------|------------------------|-------|
| rwx | Read / Write / Execute | 7 |
| rw- | Read / Write | 6 |
| r-- | Read | 4 |

Oktal: `r = 4, w = 2, x = 1, - = 0`

Spezialbits:
- Suid-Bit - Programm als Benutzer ausgeführt -> `chmod +s`
- Sticky-Bit - verhindert in öffentlichen Ordnern löschen durch Dritte z.B. `/tmp` -> `chmod +t`

Befehle:
- `chmod` ändert die neun Zugriffsbits von Dateien
- `chown` ändert Besitzer (auch Gruppe möglich)
- `chgrp` ändert die Gruppenzugehörigkeit

#### Access Control Lists
- sehr genaue Definition, wer welche Rechte auf Datei besitzt und wer nicht
- Befehl `getfacl` und `setfacl`
- muss von Dateisystem unterstützt werden z.B. ext mit mount-Option `acl` mounten

#### Symbolische-Links
Verweisen auf Dateien oder Ordner - ohne eine Kopie anzulegen (Redundanzen vermeiden).

- feste Links `ln <datei> <link>` Eintrag in Dateisystem mit Metadaten
- symbolische Links `ln -s <datei> <link>` Referenz auf einen PFAD

#### Dateiverwaltung

| Befehl | Eigenschaft |
|-------------------|-------------|
| `cd <pfad>` | Ordner wechseln |
| `cp <datei> <ziel>`| Datei oder Ordner (mit ` -r`) kopieren |
| `less/more` | Seitenweise Text-Dateien anzeigen |
| `ls (<pfad>)` | Listet alle Dateien und Ordner auf |
| `mv <von> <nach>` | Ordner oder Datei verschieben |
| `touch <name>` | Leere Datei anlegen |
| `rm <name>` | Datei oder Ordner (mit ` -rf`) entfernen |
| `rmdir <name>` | Ordner entfernen |

#### Software-Installation
- häufig ein make-Script beigelegt
- Paketverwaltung meist vorhanden wie z.B. apt-get, yum

#### Advanced Packaging Tool (APT)
- suchen, installieren und updaten von Paketen (Software-Teilen)
- Verwaltung von Software-Quellen (Repos)
- Quellen in `/etc/apt/sources.list`
- Quellen zeilenweise: `pakettyp uri` wie z.B. `deb http://archivelubuntu.com/ubuntu/ zesty-updates main restricted`

Installation von Paketen `sudo apt-get install htop`

#### Cron - Jobsteuerung

Verwalten von wiederkehrenden Aufgaben. In `/etc/crontab` oder `/etc/cron.d` genau definierbar, wann ein Job ausgeführt werden soll.

#### SysLog

Kurze Meldungen über Ereignisse, Fehler – wie ein Logbuch. Speicherort in `/var/log`. So können Fehler gefunden werden oder Spuren eins Ereignis/ Angriffs etc. suchen.

Da die Datei schnell sehr groß wird, kann `logrotate` genutzt werden – am besten per Cronjob (`/etc/cron.daily/logrotate`). Konfiguration erfolgt z.B. über `/etc/logrotate.d/`. Alle Logarchive werden komprimiert und z.B. 10 Stück aufbewahrt und die restlichen (ältesten) gelöscht. 


## 5. Netzwerkadministration

### Netzwerkmanager
- in vielen Distributionen Netzwerkkonfigurationswerkzeug in der UI
- bei Server installationen nicht verfügbar

### Namen von Netzwerkschnittstellen

**Index Device:** Abkürzung für Interface-Typ plus Index.

Beispiel: eth1 (zweites Ethernet Gerät), wlan0 (erstes WLAN Gerät)

**Consistent Network Device Naming:** verhinder Umbenennung durch Hinzufügen oder Entfernen von Geräten.

Beispiel: enp0s25 (en -> Ethernet, p -> PCI-Bus, 25 -> Slot Nummer)

### Netzwerkkonfiguration

**/etc/netctl:** Ist ein Mechanismus von Systemd um Netzwerkkonfiguration durchzuführen. Wird von Systemd verwaltet.

Konfigurationen werden in Textdateien in `/etc/netctl` gespeichert. Dabei wird automatisch erkannt ob es sich um IPv4 oder IPv6 Adressen handelt.

Beispiel:

```
Description='My Network Controller'
Interface=enp0s07
Connection=ethernet
IP=static
Address=('10.1.10.2/24')
Gateway='10.1.10.1'
DNS=('10.1.10.1')
```
(`/24` ist die subnet notation und muss bei der Adresse angegeben werden)

**/etc/network/interfaces:** zum spezifizieren von Netzwerkkonfiguration. Wird von Kernel verwaltet.

Beispiel:

```
auto eth0
iface	eth0 inet static
		address 10.1.10.42
		netmask 255.255.255.0
		gateway 10.1.10.1
```

Dabei muss `auto INTERFACENAME` für jedes bei Booten zu aktivierende Interface hinzugefügt werden. Danach müssen die Schnittstellen konfiguriert werden nach Schema `iface INTERFACENAME NETZWERKPROTOKOLL KONFIGURATIONSART`.

Es lassen sich auch Skripte zu verschiedenen Zeitpunkten in dieser Konfiguration ausführen e.g.:

```
iface 	eth0 inet static
...
		post-up ping -c 1 8.8.8.8
```

Diese Skripte können auch in Dateien unter `/etc/network/if-*.d` gespeichert werden, welche dann zu den jeweiligen Zeitpunkten ausgeführt werden. Es gibt folgende Zeitpunkte:

- pre-ip: vor dem Starten der Netzwerkverbindung
- up: während des Startens der Netzwerkverbindung
- post-up: nach dem Starten der Netzwerkverbindung
- pre-down: vor dem Trennen der Netzwerkverbindung
- down: während des Trennen der Netzwerkverbindung
- post-down: nach dem Trennen der Netzwerkverbindung

**IP:**

```
# Status und Netzwerkkonfiguration der Netzwerkschnittstelle
ip addr show

# Netzwerkschnittstellen aktivieren und deaktivieren
sudo ip link set eth0 up
sudo ip link set eth0 down

# Netzwerkschnittstellen zu konfigurieren
sudo ip addr add dev eth0 172.29.252.5/14
```

**/etc/hosts:** Ist die Abbildung von Hostnamen auf IP-Adressen, welche nicht über einen Namensserver aufgelöst werden können.

Beispiel:

```
127.0.0.1 	localhost
127.0.0.1 	myserver.localdomain
10.212.187.1 	mynetwork
```

**/etc/resolv.conf:** Konfiguriert Resolver für Namensserver

### Firewalls

> Wissen über die sieben Schichten nach OSI sind hilfreich

Firewalls blockieren unberechtigte Zugriffe auf Basis von IP-Adressen(3. Layer), Portinformationen(4. Layer) und Anwendungsinformationen(7. Layer).




### Übung

2.

```
# Pings
iptables -A INPUT -p icmp -j DROP

# SSH
iptables -P INPUT DROP
iptables -A INPUT -p tcp --dport ssh -j ACCEPT

# nur interne Netz weiterleiten
iptables -P FORWARD DROP
iptables -A FORWARD -j ACCEPT -p all -d 192.168.1.0/24

# ausgehendes ICMP-Paket loggen
iptables -A OUTPUT -p icmp -j LOG --log-prefix "outgoing ICMP packet"

# SYN-Flooding
iptables -A INPUT -p tcp -m conntrack --ctstate NEW -m limit --limit 2/s --limit-burst 3 -j RETURN
```

## Entwicklung unter Linux

### Werkzeuge

**GCC:** GNU Compiler Collection
