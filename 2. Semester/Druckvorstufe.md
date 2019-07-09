# Druckvorstufe Zusammenfassung

## Farbe

### Größen:

**Beleuchtungsstärke:**

> Empfangene Strahlungsleistung je Fläche, z.B. eine Kerze aus 1 m Entfernung erzeugt ca. 1 lx

Formelzeichen: E,  wird in Lux (lx) angegeben.

| Beispiel              | Beleuchtungsstärke |
|-----------------------|-------------------:|
| Heller Sonnentag      | 100.000 lx         |
| Bedeckter Sommertag   | 20.000 lx          |
| Beleuchtung TV-Studio | 1.000 lx           |


### Metamerie

- Entstehung des gleichen Farbeindrucks durch verschieden zusammengesetzte Farbspektren
- *Beispiel:* Gelb (600 nm) sieht man sowohl, wenn nur monochromatisch gelbes Licht, als auch wenn rotes (700 nm) **und** 
grünes Licht (500 nm) auf die Netzhaut trifft

Es gibt die Beleuchtungs-, Beobachtergeometrie-, Beobachter-, Geometrie- und Gerätemetamerie.

### Lichttemperatur und Normlicht

Wird in Kelvin (K) angegeben. Je geringer der Wert, desto rötlicher; je höher, desto bläulicher.

| Temperatur | Normlicht | Beispiel            |
|------------|-----------|---------------------|
| 1900 K     | ---       | Kerze               |
| 2856 K     | A         | Glühlampe (Wolfram) |
| 5000 K     | D50       | Sonnenlicht         |
| 6500 K     | D65       | bewölkter Himmel    |
| 6700 K     | C         | ---                 |

### Messgeräte

#### Densitometer

- 1 Fotodiode
- Quantitative Messung der Farbdichte (Volltondichte) und optischen Dichte
  - *Durchsichts*densitometer: Messen des Transmissionsgrades, d.h. Anteil durchgelassenen Lichts
  - *Aufsichts*densitometer: Messen des Remissionsgrades, d.h. Anteil reflektierten Lichts 
- unabhänig von der Farbe der Fläche
- Rückschlüsse auf: 
  - Farbschichtdicke
  - Flächendeckung

**Anwendung:** Linearisierung von Druckmaschinen 


#### Colorimeter

- 3 Fotodioden
- Messung der Lichtintensität der Rot-, Grün-, und Blauanteile (Spektralbereiche) einer Lichtquelle
- Rückschlüsse auf:
  - Farbmetrik (→ [Schuhsohle](#farbraum-vergleich-in-xy-darstellung))
  - Farbräume, z.B. XYZ, Lab
  - Lichttemperatur

**Anwendung:** Kalibrierung von Monitoren

#### Spektralfotometer

- ähnlich zu Colorimeter
- Spektralanalyse (seperate Messung mit ca. 30 bis 40 Fotodioden) von entsprechenden Spektralbereichen

**Anwendung:** Profilierung; Kalibrierung von Monitoren, Druckern, Druckmaschinen

### Key

Gleiche Anteile von Cyan, Magenta und Yellow werden durch **Key** ersetzt und ist damit der Ersatz für Schwarz beim 
Drucken. Dies hat mehrere Gründe:

- Aus CMY entsteht in der Praxis kein reines Schwarz, eher ein graubraun
- Zu viel Farbe führt zum Aufweichen des Bedruckstoffes
- Auge ist kritischer bei Farbstichen in Grautönen
- Eine Farbe ist billiger als 3
- Weniger Farbe trocknet schneller
- Beim Offsetdruck können einfarbige Ränder entstehen, v.a. bei Text relevant

### Druckfarbe

Echtheit wird mit Farbmusterblättern, welche nach DIN genormt sind, bestimmt.

### Prozessfarben vs. Sonderfarben

#### Prozessfarben

- Farben aus dem Vierfarb-Prozess, also CYMK
- Farbraum kann durch weitere Farben (→ [Sonderfarben](#sonderfarben)) erweitert werden
- die Farbmischung erfolgt erst beim Druck / auf dem Papier

#### Sonderfarben

- auch Schmuckfarbe / Volltonfarbe genannt
- die Farbmischung erfolgt vor dem Druck
- Gründe für Sonderfarben:
    - Kann zum billigeren Drucken verwendet werden (Rot statt Yellow und Magenta)
    - Farben außerhalb von CMYK Farbraum möglich, z.B. Firmenfarben, Neonfarben, Metallic-Farben
    - Vermeidung von Passungenauigkeiten, z.B. bei Kleingedrucktem

### L\*a\*b\*

Entspricht YCrCb, also Aufteilung in Luminanz (L) und Buntheit/Chroma (+a = Rot, -a Grün, +b Gelb, -b Blau)

\* kennzeichnet Gleichabständigkeit → __L\*a\*b\*__ wird daher für Berechnung des __Farbabstandes ΔE__ verwendet

```
ΔE = √((Δa)² + (Δb)² + (ΔL)²)

Δa = a1 – a2
Δb = b1 – b2
ΔL = L1 – L2
```

#### Bewertung der Farbabstände

| ΔE        | Bewertung                        |
|-----------|----------------------------------|
| 0,0 - 0,5 | Kein Unterschied                 |
| 0,5 - 1,0 | Erkennbar für das geübte Auge    |
| 1,0 - 2,0 | merklicher Farbunterschied       |
| 2,0 - 4,0 | wahrgenommener Farbunterschied   |
| 4,0 - 5,0 | nicht mehr toleriert             |
| \> 5,0    | andere Farbe                     | 

## Farbmanagement

### Hintergrund

- Gleicher Farbeindruck auf allen Medien
- Übereinstimmung von Vorlage, Proof und Druck
- Kann auf digitaler Consumer Seite nicht angewandt werden (Monitore und TVs können falsch eingestellt sein)


### Profile

> Ist ein genormter Datensatz, welcher den Farbraum eines Ein- oder. Ausgabegerätes beschreibt.

> Ziel ist es, dass die Farben des Eingabegerätes auf dem Ausgabegeräte möglichst ähnlich sind.

Zum erstellen werden mit dem Colorimeter die Ist-Werte mit den Soll-Werten verglichen und aus dem ΔE das Farbprofil festgelegt. Es wird die Farbübertragung beschrieben.

### Renderpriorität | Rendering Intent

Setzt die Methode zum Umrechnen in einen anderen Farbraum fest. Die Umrechnung nennt man `Gamut Mapping`.

Es gibt 4 verschiedene Rendering Intents (RIs)

- wahrnehmungsorientiert (perzeptisch, fotografisch)
- relaitv farbmetrisch
- absolut farbmetrisch (colorimetrisch)
- sättigungserhaltend (Präsentationen)

> RIs sind nicht genormt, daher kommen in verschiedenen Programmen unterschiedliche Ergebnisse heraus

### Arbeitsfarbraum

Hat zwei Bedeutungen:

1. Der voreingestellte Farbraum für Bilder ohne eigenes Profil
2. Der Farbraum in dem man arbeitet

Üblich sind

- für RGB in der Druckvorstufe: Adobe RGB
- für RGB in Monitorpublikationen: sRGB
- für CMYK: Coated von Adobe, gibt es auch als ISO

### Farbraum Vergleich in xy Darstellung

![](http://www.hannes-kraeft.de/media/grafik/cm/sRGB.png)

Die große Schuhsohle zeigt den kompletten Farbraum, das Dreieck was von sRGB abgedeckt wird. Es lassen sich unterschiede in den Farbräumen visualisieren.

### Farbraum Vergleich in Lab

Siehe L*a*b weiter oben

### Proof

**Softproof:** auf dem Monitor, vorher Farbprofil der Bilder kontrollieren

**Hardproof:** mit Papier, kann als Vertragsbasis dienen

**Digitalproof:** mit digitalem Druckverfahren, meistens Tintenstrahl

**Colorproof:** farbverbindlich, keine Rasterung, hochwertiger Drucker, mit Farbmanagement

### Binding

> Bindung an ein Ausgabeformat, Umwandlung der RGB Eingabe in die Farbräume der Ausgabe (CMYK)

| | Early Binding | Intermediate binding | Late binding |
|---|---|---|---|
| Konvertierung | nach der Bildbearbeitung, vor dem Layout | nach dem Layout beim PDF-Export | nach Übernahme der PDF Datei |
| Softproof | nicht nötig | bei der Bearbeitung | bei der Übergabe |
| Standard | PDF/X-1a | PDF/X-1a | PDF/X-4 |
| Farben | CMYK und Sonderfarben | CMYK und Sonderfarben | CMYK und RGB mit Profil |

#### PDF/X

> PDF/X sind genormte Eigenschaften, welche Eigenschaften von Druckvorlagen im PDF Format beschreiben.

- Zuverlässigeres Druckergebnis
- Eigenschaften lassen sich mit Preflight überprüfen
- Druckqualität wird dabei nicht beachtet

**PDF/X-1a:** Farbgetreue Wiedergabe in CMYK und Sonderfarben

**PDF/X-4:** Beinhaltet alle benötigten Elemente. Unterstützt Farbmanagement, CMYK, RGB, Graustufen und Sonderfarben, PDF-Transparenzen, JPEG2000, 16bit Bilddateien und OpenType Fonts

## Druck

### Grundlagen

**Begriffe:** Druckkörper, Druckform, Druckfarbe und Bedruckstoff

**Vorgang:** Anlegen, Einfärben, Drucken, Auslegen.

**Druckprinzip:**

- **Fläche | Fläche:** Tiegeldruck, Kniehebelpresse
- **Fläche | Zylinder:** Heidelbergzylinder, Schnellpresse
- **Zylinder | Zylinder:** Rotationsdruck

**Direkter und indirekter Druck:**

Durch die Rollen wird das Bild gespiegelt. Bei einer gerade Anzahl von Rollen muss das erste Bild gespiegelt sein damit es richtig auf dem Bedruckstoff kommt. Bei einer Ungeraden Anzahl von Rollen muss das erste Bild Richtig sein, wird dann auf der Zweiten Rolle gespiegelt und landet dann doppelt gespiegelt, also wieder richtig, auf dem Bedruckstoff

### Hochdruck:

![](http://www.hannes-kraeft.de/media/grafik/druck/vhoch.png)

**Beispiel:** Stempel

**Druckbild:** Quetschrand

**Funktionsweise:** Druckform mit Ausdellungen (wie Stempel). Druckform wird von Farbwerk mit Farbe versehen. Gegendruckzylinder drückt den Bedruckstoff an die Druckform. Druckform und Bedruckstoff bewegen sich

### Tiefdruck

![](http://www.hannes-kraeft.de/media/grafik/druck/vtief.png)

**Beispiel:** Kupferstich

**Druckbild:** Zackenrand durch Stege

**Besonderheiten:** Echte Halbtöne möglich, extrem schnell (16m/s)

**Funktionsweise:** Die Druckform dreht sich in der Farbwanne, wodurch sich in den Vertiefungen Farbe sammelt. Am Ende der Farbwanne streicht die Rakel die überschüssige Farbe ab. Der Bedruckstoff wird vom Gegendruckzylinder an die Druckform gepresst und bewegt sich laufend mit.

### Flachdruck (Offset)

![](http://www.hannes-kraeft.de/media/grafik/druck/vflach.png)

**Druckbild:** sauberer Rand

**Besonderheiten:** hervorragende Qualität, druck ist durch fetthaltige Farbe wasserfest, sehr schnell und kann großflächig genutzt werden, lässt sich veredeln

**Funktionsweise:** Die Druckform wird durch das Feuchtwerk benässt, danach wird vom Farbwerk die Farbe aufgetragen. Diese wird auf ein Gummituch übertragen, welches die Farbe letztendlich auf den Bedruckstoff, welcher durch den Gegendruckzylinder an das Gummituch gepresst wird, bringt.

### Siebdruck

![](http://www.hannes-kraeft.de/media/grafik/druck/vsieb.png)

**Druckbild:** Zackenrand auf Grund der Siebfäden

**Funktionsweise:** Farbe wird mit einem Räkel für das Sieb gezogen. Unter dem Sieb liegt die Schablone, welche an den gewünschten Stellen die Farbe auf den Bedruckstoff durchlässt

### Digitaldruck

- ohne feste Druckform
- Print on demand
- Sehr geringe Druckgeschwindigkeit

**Druckbild:**

- Schatten- bzw. Staubrand bei Laserdrucker
- Einzelne Punkte bei Tintenstrahl

### Vollton / Halbton

**Vollton:** 100% Farbe (einfarbige Striche, Flächen und Linien ohne Verlauf)

**Halbton:** Farbverläufe und Transparenzen -> Benötigt Rasterung

### Halbtonraster

> Keine Fläche drucken sondern einzelne Punkte um Transparenzen und Farbverläufe vorzugaukeln

Druckerpunkte sind die kleinsten möglichen Einheiten, diese lassen sich zu Halbtonpunkten zusammenfassen. Viele Halbtonpunkte ergeben dann den kompletten Druck (siehe Autotypische Farbmischung).

### Halbtonzelle

Ist der Bereich des Halbtonpunktes plus dessen unbedruckte Umgebung.

```
2 	Graustufen	dpi = lpi
5 	Graustufen	dpi = 2 x lpi
10 Graustufen		dpi = 3 x lpi
```

Siehe Berechnungen für mehr Informationen

### Auflösung

| | Abkürzung | Punkte pro Inch | |
|---|---|---|---|
| Scanner | dpi | dots per inch | Geräteauflösung
| PS-Datei | ppi | pixel per inch | Dateiauflösung |
| Druckerauflösung | dpi | dots per inch | Druckerauflösung |
| Druckraster | lpi | lines per inch | abhängig zur Druckerauflösung |

### AM & FM

**Amplitudenmodulation:** Abstand der Halbtonpunkte (Rasterpunkte) ist konstant, die Punktgröße ist moduliert

**Frequenzmodulation:** Die Größe der Druckerpunkte (dots) ist konstant, der Abstand ist moduliert

### Moiré

**Ursache:** Überlagerung von Rastern, zeigt sich oft erst beim Druck

**Vermeiden:** Keine kleinen Muster (kleinkariertes Jacket im TV), FM-Raster, kein AM-Raster beim Druck. Alternativ gibt es eine Entrasterungsfunktion

### Papier

**Herstellung:**

1. Aufbereitung der Fasern (u.a. Bleichen)
2. Mischen der Grundstoffe für Papiersorte und Qualität
3. Auflaufen auf das Sieb der Papiermaschine (99% Wasser)
4. Blattbildung durch Entwässerung
5. Pressen in der Presspartie
6. Trocken
7. Glätten
8. Aufrollen
9. Veredeln
10. Rollen 
11. Verpacken

### Überfüllen & Überdrucken

Verschiedene Farben werden nacheinander gedruckt, dabei muss der Bedruckstoff allerdings perfekt Ausgerichtet sein, da es sonst zu Veschiebungen kommt. Dies lässt sich allerdings nicht realisieren.

Deshalb wird an den Grenzflächen eine Linie gelegt, wodurch sich diese überschneiden. So wird verhindert dass die Farbe des Bedruckstoffes durchscheint (Blitzer)

Wurde früher im Layoutprogramm gemacht, heute meist durch die Druckerei

### PDF/X

| Druckproblem | PDF/X-1a | PDF/X-4
|---|---|---|
| CMYK | möglich | möglich |
| Sonderfarben | möglich | möglich |
| RGB Bilder | nein | mit Profil |
| Transparenz | nein | möglich |
| Ebenen | nein | möglich |
| OpenType | nein | möglich |
| 16bit/Kanal | nein | möglich |
| JPEG2000 | nein | möglich |
| Trim-Box | erforderlich | <- |
| Notizen | Nur außerhalb des Druckbereichs | <- |
| Bilder | eingebettet | <- |
| Schriften | eingebettet | <- |
| Interaktivität | nein | <- |
| Formulare | nein | <- |
| Überfüllung | Angabe, ob überfüllt wurde | <- |
| OutputIntent | erforderlich | <- |
| Passwortschutz | nein | <- |
| separierte Ausgabe | nein, nur Composit (bunte Bilder) | <- |

**Nicht in PDF/X:** 

- Maximaler Farbauftrag
- zu viele Sonderfarben
- Haarlinien
- zu geringe Tonwerte
- Auflösung der Bilder
- Orthographie, Inhalt, Gestaltung, Recht, ...

### Weiterverarbeitung

1. Schneiden
2. Falzen
3. Vorrichten (Beikleber, Warenproben, Gimmick, CD, ...)
4. Sammeln
5. Binden
6. Beschneiden
7. Buchbinden

## Berechnungen

Qualitätsfaktor Q ist meistens mit √2 ≈ 1,4 angenommen, sollte nicht kleiner sein

### Rasterbreite

**Aufgabe**

PAL TV Bild in Zeitung abdrucken

```
geg.:
	Rasterweite L = 40er = 40Rpcm
	Bo = 720 Pixel
	Ho = 576 Pixel
	Q  = 1,4

ges.: Rasterbreite und Höhe in cm

Lsg.:
	A 	= R * Q * S ... S = 1
		= 40 1/cm * 1,4
		= 56 (1/cm|ppcm)
		
	A 	= Bo / Br
	Br 	= Bo / A
		= 720 pixel / 56 ppcm
		= 12,9cm
	Hr = Hr / A
		= 576 pixel / 56 ppcm
		= 10,3
```

### Scannerauflösung

```
Scannauflösung (dpi bzw. ppi) = Rasterweite(lpi) * Skalierungsfaktor * Qualitätsfaktor
```

**Aufgabe**

Von einem Kleinbild-Dia im Querformat soll ein maximaler Bildausschnitt auf 6x9cm Hochformat reproduziert werden. Berechnen Sie die Scanauflösung für das 70er Raster in Pixel pro Inch!

```
geg.:
	o ... Original
	r ... Repro

	Bo = 3,6cm
	Ho = 2,4cm
	
	Br = 6cm
	Hr = 9cm
	
	L = 70 Linien/Centimeter
	
ges.: Auflösung A in ppi

Lsg.:

	1. Skalierungsfaktor:
		S = Hr / Ho
		  = 9cm / 2,4cm
		S = 3,75

	2. 	A = L * Q * S
			(Q ... Qualitätsfaktor -> mit 1,4 angenommen)
		  = 178 1/inch * 1,4 * 3,75
		  = 934 ppi
```


### Anzahl druckbarer Graustufen

```
(Druckerauflösung in dpi / Rasterweite in lpi)² + 1
```

### Farbabstand

```
ΔE = √((Δa)² + (Δb)² + (ΔL)²)
``` 
