# Probeklausur Linux

## Linux Philosophie

1. Small is beautiful
2. Make each program do one think well
3. Build a prototype as soon as possible
4. Choose portability over efficiency
5. Store numerical data in flat ASCII files
6. Use software leverage to your advantage (Good programmers write good code; great programmers borrow good code)
7. Use shell scripts to increase leverage and portability
8. Avoid captive (versklavende) user interfaces
9. Make every program a filter
10. Everything is a file

## Free Software

**Vier Freiheiten:**

**Freiheit 0:** Beliebige Nutzung

**Freiheit 1:** Nachvollziehen der Funktion

**Freiheit 2:** Weitergabe

**Freiheit 3:** Software ändern und Änderungen veröffentlichen

---

**Closed Source:** keine Nachvollziehbarkeit der Funktion

**NC-Software:**: keine beliebige Nutzung

**Lizenz-Software:** keine Weitergabe

**Freeware:** Keine Änderung der Software und keine Nachvollziehbarkeit

**Prop. Software:** Keine Freiheiten

##FSH

> File System Hierarchy, Definition einiger Standardpfade und Orte im Dateisystem

```
/		Wurzel des Dateisystems
~		Home des aktuellen Nutzers

/bin	Binaries (global)
/boot	Bootvorgang
/dev	Devices
/etc	Anwendungskonfiguration
/home	Nutzerspezifische Verzeichnisse
/mnt	Mountpoints
/proc	virtueller Zugang zu Prozessen / Prozessinfos
/root	Homeverzeichnis von root
/tmp	tempräre Dateien und Verzeichnisse
/usr	Nutzerkontext
/var 	Maschinenkonfig
```

`interactive shell` -> Shell im Nutzerkontext gestartet

`/etc/hosts` 		->	"lokales DNS"

`/etc/hosts.allow & /etc/hosts.deny`		-> definition von Zugriffen (Black und White Listing)

## Exec, Fork, Pipe

**Exec:** Ausführen von Binaries

**Fork:** Klonen eines Prozesses, ein Prozess wird zum Parent und erhält Prozess ID des Child, ein Prozess wird zum Child

```Bash -> fork() -------------------------------------------------------------------------->
			\
	 		 --> Bash -> close file descriptors -> clear memory -> load programm code --->
	 					   *******************!! Child Setup!!********************```

**Pipe:** Aus-/Eingabeumleitung

## Bash

**Überprüfen ob ein Verzeichnis existiert:**

```
if [-d /etc/apache2]
```

> Die eckigen Klammern werden an test übergeben -> Shorthand

**Parameter übergeben:**

```
./meinScript 1
```

Im Skript Arguments: 0 -> Programmname, 1 -> erster Parameter

**$#**: Anzahl der übergebenen Parameter

**$1, $2, ...:** wird über Env gesetzt und ist schlechter als args

> $0 == args[0]

## Zugriffsrechte

> RWX: Read, Write, eXecute

**Execute:** Verzeichnis öffnen bzw Programm ausführen

Kontexte: Owner/_U_ser, _G_roup, _O_thers

```chmod u+x datei
chmod o-x datei

chmod 755 datei
	
	user: 	RWX
	group:	R-X
	other:	R-X

SUID: Ausführen in Kontext des angegebenen Nutzers

chmod 1755```

## Shadow

/etc/shadow -> gehashte Passwörter, nicht für normale Nutzer lesbar

/etc/passwd -> enthält _keine_ Kennwörter mehr

## Device Naming

**Indexed Naming**

Früher eth0, eth1, ... -> schlecht da beim hinzufügen sich die IDs ändern.

PCI MEM Cache -> kennt Hardware IDs (gehasht). Hardware ist zugewiesen, dann unveränderlich, Cachegröße ist limitiert

**CNDN - Consistent Network Device Naming**

enp12s3 (Bustyp, Gerätetyp)

enx(MAC-Adresse) -> kann zu Kolision kommen

## Makefile

Textuelle Beschreibung:

- Abhängigkeiten
- Build-Anweisungen
- Verankerung im System
- Clean Up (Entfernen des Programmes und aller spezifizierten Speicherorten von Programmdateien)

```
make 			# Abhängigkeiten auflösen und bauen
make install	# Im System verankern
```

## Boot Vorgang

- UEFI
- Bootloader (zB GRUB)
- initialisiert Kernel (ggf Parameter)
- INIT-System (zB systemd)
- baut Systemumgebung (darf alles, insbesondere direkter Hardware Zugriff) und Userland (eingeschränkt, nur indirekter Resourcenzugriff) auf
- SessionFolgendes lässt sich Konfigurieren:

- BIOS Config
- GRUB Config
- beim Bauen des Kernels der Kernel
- in der Regel `/etc/`
- Session in der User-Config
