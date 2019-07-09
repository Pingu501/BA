1. Aufgabe

a) Ohne eine Farbtabelle

```
1440 * 960 * 24	= 33 177 600 bit
				= 4 147 200 Byte
				= 4,1472 MB
```

b) Mit Farbtabelle

```
56 Farben => 2^6 (64) bit für Farbadressierung

1440 * 960 * 6 + 56 * 3 * 8 	= 8 295 744 bit
								= 1 036 968 Byte
								= 1 MB
```

c) Druckgröße in cm

```
1440 / 192 = 7.5 inch 	= 19.1 cm
960 / 192 = 5 inch 		= 12.7 cm
```


07.06.2019

```
geg.:	Bild 5,76 Megapixel
		16:9 Seitenverhältnis
		YCbCr-Farbmodell (8 bit/Kanal)
		
a) Höhe und Breite in Pixeln

	b * h = 5.76MP
	16 / 9 = b / h
	
	h = 5.76MP / b
	=> 16/9 = b/(5.76MP / b)
		16/9 = b^2 / 5.76MP
		(16 * 5.76MP) / 9 = b^2
		b = root((16*5.76MP / 9)
		b = 3200 Pixel
		
	h = 5.76MP / 3200
	h = 1800

b) Speicherplatzbedarf des Bildes bei Verwendung von 4:1:1 Chroma Subsampling in Byte

8 bit pro Kanal, 3 Kanäle => 24bit pro Pixel => 3 Byte pro Pixel

5.76MP * 3 Byte = 17,82MB
4:1:1 => 50% Speicherplatzersparnis
=> 8.64MB
	
```


# Histogramm

```
Bild:	0 2 3			0 => 5
		1 0 0			1 => 1
		2 0 0			2 => 2
						3 => 1	
```

## Histogramm Binning

> Eimern/bündeln von Histogramm Werten

- Histogrammgröße B: Anzahl der Bins
- Intervalllänge kB: K/B (K ... Anzahl der Graustufen)
- Startwert des Intervalls j = aj = j * kB

```
- Histogrammgröße 256
- 16bit Grauwertbild

a) Länge der Intensitätsintervalle

kB	= K/B
	= 16^2 / 256
	= 1
	= 256

b) Grenzen des 128. Intervalls

	[32512, 32767] (32768 ist der Startwert für den nächsten Intervall)

```

```
Bild:	0	2	3
		1	0	0
		2	0	0

Histogrammgröße:	B = 2

0 => 5
1 => 1
2 => 2
3 => 1

Intervalllänge: kB = K/B = 4 / 2 = 2
Intervalle: [
	[0,1],
	[2,3]
]
```

# Punktoperationen

> Wir betrachten nur homogene Punktoperationen

- Verändern das Bild Pixel für Pixel
- Muss eine quasi eine stetige Funktion sein

## Autokontrast

Sonnenuntergrang_hell

Autokontrast -> Process -> Enhance Contrast
Analyze -> Histogram

```
a - alow / ahigh - alow = a' - amin / amax - amin
```


```
16bit Grauwertbild

a = 16200			ges.: a' = fac(a)

amin = 0
amax = 65535

Aus Histogram abgelesen:
	alow = 100
	ahigh = 50555

a' = ((a - alow) / (ahigh - alow)) * ((amax - amin) + alow)
a' = ((16200 - 100) / (50555 - 100)) * (65535 - 100) + 0
a' = 20880
```

Die Funktion des Autokontrasts is auf der X Achse von 0 bis aLow 0, steigt bis a_high auf 255 an und verläuft dann von aHigh bis 255 weiter auf 255. alternativ ist der Bereich von 0 bis aLow und aHigh bis 255 nicht definiert.

## Fragen

**Was ist eine homogene Punktoperation?**

Die Position der Pixel spielt keine Rolle. Kann über eine Loop-up-table beschleunigt werden.

**Was ist clamping?**

Werte werden abgeschnitten/begrenzt.

**Beispiele für Punktoperationen**

Helligkeit erhöhen/reduzieren, Kontrast, invertieren, ...

**Wofür wird Thresholding verwendet?**

Schwellwert festlegen. Oberhalb und Unterhalb werden die Werte fest zugewiesen. Wird zum Beispiel bei Röntgenbildern verwendet.

## Aufgaben:

1. An der Tabel befindet sich eine Punktoperation:

| i | f(i) |
|---|:----:|
| 0 | 0 |
| 1 | 0 |
| 2 | 3 |
| 3 | 3 |

**Was ist das für eine Operation?** Schwellwertoperation

**Anwenden der Operation auf ein gegebenes Bild**

```
0	1	3				0	0	3
3	0	1		=> 		3	0	0
3	2	2				3	3	3
```

## Histogrammanpassung

> Histogramm an ein anderes Bild anpassen. Die Anteile der Grauwerte angleichen und so den Helligkeitseindruck angleichen

Funktioniert nicht mit dem Standardhistogramm und einer homogenen Punktoperation, da .

```
0	3	=> 	0	1
3	0		2	3
```

In diesem Fall muss die 0 auf 3 verschiedene Werte abgebildet werden. Es kann keine LOT erstellt werden.

Stattdessen muss ein kumuliertes Histogramm verwendet werden. Dafür bildet man die laufende Summe aller Werte. Kumulierte Histogramme sind monoton ansteigende Funktionen.

**Aufgabe:** Gegeben ist eine stückweise lineare Referenzverteilung PL(i) = (<0,0.1>,<10,0.7>,<15,1.0>) für Bilder mit 16 Graustufen.

```
Bild: 	10	2	15	8
		4	1	5	6
		2	0	14	12
		6	11	9	7
```

a) Gegeben ist a = 5. Ermitteln Sie b = P_A(a) für oben stehendes Bild der Größe 4x4 Pixel mit 16 Graustufen (Grauwerte 0-15)!

```
geg.: a = 5				ges.: b = P_A(a)

Lsg.:		Kumulierte Häufigkeit H(i) (Anzahl der Pixel mit Wert <= a)
			M * N  ... Breite mal Höhe

			b 	= H(5) / M * N
				= 6 / 16
				= 3 / 8
```

b) Berechnen Sie mittels Histogrammanpassung a'=f_hs(a) für a = 7, wenn b = PA(a) = 9/16!

Wir haben 2 Segmente gegeben (3 Koordinatenpaare):

| n | i_n | i_n+1 | q_n | q_n+1 |
|---|-----|-------|-----|-------|
| 0 | 0   | 10    | 0.1 | 0.7   |
| 1 | 10  | 15    | 0.7 | 1.0   |

```
b = 9 / 16 	= 0.5625
			=> b fällt in das erste Segment
		
a' 	= i_n + (b - q_n) * ((i_n+1 - i_n) / (q_n+1 - q_n))
	= 0 + ((9/16) - 0,1) * ((10 - 0) / (0,7 - 0,1))
  (	= 7,708, muss aber wieder ein ganzzähliger Wert sein  )
	= 7 	
```

**Aufgabe:** Können einzelne Extremwerte den Histogrammausgleich verhindern?

Nein, einzelnen Extremwerte haben nur sehr geringen Einfluss auf das kumulierte Histogram

**Aufgabe:**

```
Bild: 4x4, 4 Graustufen (0-3)
a = 1		H(a=1) = 5

Wie groß ist a' = f_eq(a)?

	f_eq(a) 	= [P_A(a) * (K-1)]
				= (H(a) / (M * N) ) * (K-1)
				= (5 / 16) * (4 - 1)
			(	= 0.9375 muss noch abgerundet werden)
				= 0
```

## Gammafunktion

> Kommt aus der Analogen Fototechnik. Beschreibt den Zusammenhang zwischen Belichtungsstärke B und Filmschwärzung (Filmdichte D). Der Zusammenhang ist annähernd logarithmisch

```
b = fᵧ(a) = aᵞ

a = fᵧ^-1(b) = b^(1/𝛄)
```

> Gammawerte beschreiben wie sehr der Monitor die Ausgabe verfälscht

- Ausgangssignal: s = B^𝛄c
- Anwendung der Umkehrfunktion (KameraTransfercharakteristik): b = f_𝛄c(s) = s^𝛄c

> Gamma Aufhellung verhindert Clamping 

**Aufgabe:** Erstellen sie ein Plugin für ImageJ, welches ein Bild quadratisch aufhellt!

Mit LUT!

```
𝛄 = 1/2	a_max = 255

a' = fᵧ(a)	= ((a/K)^𝛄) * K
			= (a/255)^(1/2) * 255
```

# Filter

**Wodurch sind lineare Filter gekennzeichnet?**

Verknüpfung der Quellpixel als gewichtete Summe. (Basiert auf der Linearen Faltung)

**Welche Eigenschaften hat die Filtermatrix eines Glättungsfilters?**

Enthält die Gewichtung der Pixel die mit in das Resultat eingehen.

# Klausurvorbereitung

**1. Aufgabe**

3x3 Bild 8bit Graustufenbild. Außerhalb des Bildes sollen die Randpixel wiederholt werden.

```
	34	25	56
	67	234	190
	45	56	246
	
	
a) linearer Filter (horizontale Kanten werden geschärft)	
```

