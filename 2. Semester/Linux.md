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
| `mkdir <name>` | Ordner anlegen |




## 5. Netzwerkadministration

### Übung


