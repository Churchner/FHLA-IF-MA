# Vertiefung Datenbanksysteme WS 2015/16

[TOC]
## Mitschrift 2015-10-06

### XML und Datenbanken
XML: Markup-Sprache für die Spezifikation von Dokumenten

XML-Dokument:
- Prolog (optional)
- DTD (optional) --> alternative Spezifikation über XML-Schema
- Dokumentinstanz

XML-Datenbank: DB mit gespeicherten XML-Dokumenten

XML-Abfragesprache: Sprache zur Extraktion von Daten aus XML-Dokumenten (Beispiele: XPath, XQuery)

#### XPath
##### Grundlagen
Knotenarten:
- Dokument: künstlicher Knoten als Ursprung eines Dokuments
- Element: mit einem Elternknoten und einer geordneten Liste von Kindknoten. Kindknoten --> Elementknoten-, Text-, Kommentar-, Verarbeitungsanweisungsknoten
- Attributknoten: für Eigenschaften von Elementknoten (@)
- Textknoten: für Textinhalt von Element
- Kommentarknoten: für Kommentare innerhalb eines XML-Dokumentes
- Verarbeitungsanweisungsknoten
- Namensraumknoten

Zeichenkettenwert (String Value) eines Knotens: Wert, über den ein Knoten mit anderen Knoten verglichen werden kann. Das ist:
- für Dokument- und Elementknoten : die Verkettung der Zeichenkettenwerte aller untergeordneten Textknoten
- für Attributknoten: der jeweilige Attributwert
- für Textknoten: ihr Text

Dokumentreihenfolge:
- Dokumentknoten zuerst
- Jeder Elementknoten vor seinen Kindknoten und Nachfahren
- Für jeden Knoten mit Kindern:
	- erst Knoten selbst
	- dann seine Namensraumknoten
	- dann seine Attributknoten
	- dann seine Kindknoten
	- zuletzt seine nachfolgenden Gechwister

XPath navigiert in einem Dokument mit Hilfe von 13 Achsen.
XPath-Ausdrücke:
- Basisausdrücke
- **Pfadausdrücke:** Lokalisierungspfade
- Sequenzausdrücke
- ...

Lokalisierungspfade:
- absolut <--> relativ (ausgehend vom Dokumentknoten <--> ausgehend vom gerade aktuellen Knoten (Kontextknoten))
- einfach <--> zusammengesetzt (--> .../.../... //.../...//.../... Lokalisierungsschritt: Achse::Knotentest[Prädikat]*)
- allgemeine <--> abgekürzte

Knotentest: Einschränkung einer Knotenauswahl entlang einer Achse durch Angabe einer Knoten++art++ oder eines Knoten++namens++.
Prädikat: logische Bedingung zur Einschränkung einer Knotenauswahl.

XPath-Funktionen, z.B.:
- position()
- string()
- ...

Beispiel: /Leute/Person[position()=2] <=> /Leute/Person[2]

## Mitschrift 2015-10-13

[... XQuery-Einführung fehlt ...]

#### 2.2 XQuery-Ausdrücke
- einfache Ausdrücke: ..., ..., XPath-Pfadausdrücke
- FLWOR-Ausdrücke (for/let, where order by, return)
- FLWOR-Ausdrücke verarbeiten Eingangssequenzen mit Hilfe eines Pipelining-Prinzips und produzieren eine Ausgangssequenz

##### Grundbegriffe:
Variablenbindung: Bindung einer Variablen an einen Wert
Tupel: Menge von Variablenbindungen
Tupelstrom: Sequenz von Tupeln

einzelne Klauseln:
Eine ++LET++-Klausel bzw. eine ++FOR++-Klausel erzeugt einen Tupelstrom. Dabei bindet eine LET-Klausel das Ergebnis eines Ausdrucks als Gesamtheit in einem Schritt, eine FOR-Klauel elementweise (die Einträge nacheinander) an eine Variable.
Der Tupelstrom wird anschließend durch ++WHERE++-und ++ORDER BY++-Klauseln weiter verarbeitet.

WHERE-Ausdruck: Filterung
ORDER BY-Ausdruck: Sortierung

Zum Schluss wird durch einen RETURN-Ausdruck das Ergebnis konstruiert und zurückgegeben.

Elementkonstruktor: `<tag>{...}{...}...</tag>`

FLWOR-Ausdrücke können in XML-Dokumente eingebettet werden, insbesondere über {...} in Tags.

#### 2.3 Verbünde/Joins
Verbundanfragen werden in XQuery als Schachtelung von FOR-Schleifen spezifiziert, zusätzlich wird in einer WHERE-Klausel oder einem Prädikat eine Verbundbedingung ausgedrückt.
Anschließend konstuiert ein RETURN-Ausdruck das Ergebnis(-Dokument).

#### 2.4 Gruppierung und Aggregatfunktionen
Modellierung durch FOR+LET:
Mit einem FOR-Ausdruck bindet man eine Variable nacheinander an die Elemente, nach denen gruppiert werden soll. Für jede dieser Variablenbindungen bindet man in einem anschließenden LET-Ausdruck eine weitere Variable an diejenigen Sequenzen, auf die danach eine Aggregatfunktion angewendet werden soll.

#### 2.5 Quantoren
>SOME -> ∀

>> ... SATISFIES `<Testausdruck>` (-> false/true)

>EVERY -> ∃

### Praktikum 1 (2015-10-13)
#### Aufgabe 1
1. `//verein`
2. a) `//verein/@name`
   b) `//verein/@name/text()`
3. `//verein[@gegruendet<1930]`
4. `//verein/parent::node()[@name='Vilsbiburg']/verein/@name/text()`
5. `//beruf[text()='Lehrer' | text()='Lehrerin']/parent::node()/name/descendant::text()`
6. `//mitglied[@kategorie='Junior']/name/vorname[text()='Karl']/parent::node()/nachname/text()`
7. `//strasse[contains(text(), 'weg')]/text()`
8. `//verein[count(abteilung)>1]/@name/text()`
9. `//stadt[@name='Bremen']/descendant::mitglied/name/descendant::text()`
10. `concat(//nachname[text()='Neumann']/ancestor::mitglied/adresse/strasse/text(), ' ', //nachname[text()='Neumann']/ancestor::mitglied/adresse/hausnummer/text(), ', ', //nachname[text()='Neumann']/ancestor::mitglied/adresse/plz/text(), ' ', //nachname[text()='Neumann']/ancestor::mitglied/adresse/ort/text())`
11. `//nachname[text()='Johansen']/ancestor::abteilung/descendant::mitglied/name/nachname/text()`
12. `//beruf[text()='Richter'|text()='Richterin']/ancestor::stadt[@einwohnerzahl>100000]/@name/text()`
13. `count((//mitglied)[1]/ancestor::node())`
14. `//ort[text()=ancestor::stadt/@name/text()]/ancestor::mitglied/name/nachname/text()`
15. `(//mitglied)[1] | (//mitglied)[1]/*`
16. `count(//verein[@name='Borussia']//mitglied)>count(//verein[@name='Werder']//mitglied)`
17. `//beruf[text()='Busfahrer']/ancestor::stadt/@name/text()`
18. `(//stadt[@name='Moenchengladbach']//verein[@name='Borussia']//mitglied[last()])[last()]`
19. a) `count(//*) = count(//mitglied[@mitgliedsnummer=23]/ancestor::*/descendant::*)`
	b) `count(//* | self::node()) = count(//mitglied[@mitgliedsnummer=23]/ancestor::* | //mitglied[@mitgliedsnummer=23]/preceding::* | //mitglied[@mitgliedsnummer=23]/following::* | //mitglied[@mitgliedsnummer=23]/self::node() | //mitglied[@mitgliedsnummer=23]/descendant::*)`
20. `//plz[not(number(self::node()) > //plz)][1]` Trick?

## Mitschrift 2015-10-20

### Data Mining
#### Einführung
Daten: gespeicherte Fakten
Information:
 a) potenziell oder aktuell vorhandenes Wissen
 b) "in Form" gebrachtes Wissen
Data-Mining-Algorithmus: Algorithmus, der aus Daten Informationen ableitet (Lernalgorithmus)

Anwendungsgebiete:
- Wetterdaten
- Verbindungsdaten in Telekommunikationsnetzen
- Bewegungsdaten, erfasst durch Sensoren, Wearables

Spezialfall: Data-Mining-Algorithmus zum Zwecke der Klassifikation; erzeugt einen ->
Klassifizierer: bildet Eingabedaten auf Kategorien (Klassen) ab.

#### Eingaben für Data-Mining-Algorithmen
Trainingsdaten: Mengen von (gemessenen) Instanzen, die aus Werten von Attributen bestehen.
Attribute:
- nominal bzw kategorisch: enum-Werte, diskret
- numerisch

**Unterscheidung:**
Argument-Attribute:
- Attribute, die gemessen/beobachtet werden,

Klassifikationsattribute:
- in Trainingsdaten gemessen/beobachtet
- dienen zur Einteilung der Trainingsdaten-Instanzen in Klassen
- sollen bei "neuen" vorhergesagt werden.

Praxis: Eingabedaten werden im ARFF-Format notiert:
@relation
@attribute: Kl./Arg.-Attribute
@data: Instanzen der Trainingsdaten
% Kommentare

Probleme bei Eingabedaten:
- Fehlende Daten
- Magere Daten
- Ungenauigkeiten

#### Ausgaben von Data-Mining-Algorithmen
##### Entscheidungsliste von Entscheidungsregeln
Liste von Regeln, die auf Instanzen von Daten ++sequentiell abgearbeitet++ werden muss, um zu einer Entscheidung (Klassifikation) zu gelangen.

Format einer Entscheidungsregel:
- `if(<Bedingung>) then <Folgerung>`
- `<Bedingung> --> <Folgerung>`
- `otherwise <Folgerung>`

##### Entscheidungsbaum
Baum, der von der Wurzel zu den Blättern auf Instanzen abgearbeitet werden muss, um zu einer Entscheidung zu gelangen.

Regressionsbaum: Baum für numerische Vorhersagen

##### Assoziationsregeln
Regeln, die Abhängigkeiten zwischen Attributen von Trainingsdaten ausdrücken.
Gleiches Format wie Entscheidungsregeln, allerdings kann ein Klassifikationsattribut Teil einer Bedingung sein.

#### Algorithmen des Data Mining
##### 1-Regel (1-Regel-Lernverfahren, OneR)
Zieht für die Entscheidung nur ein Attribut der Trainingsdaten heran: dasjenige mit der geringsten Gesamtfehlerrate.

## Mitschrift 2015-10-27

##### Naives Bayes
Vorbemerkung: Bayes-Regel
A, B Ereignisse
$$P(A|B) = \frac{P(B|A) - P(A)}{P(B)}$$
gibt zusätzlich: $$$B: B_1 \wedge B_2 \wedge ... \wedge B_N$$$
mit $$$B_i$$$ unabhängig,
$$\rightarrow P(A | B_1 \wedge B_2 \wedge ... \wedge B_N) = \frac{P(B_1|A) * P(B_2|A) * ... * P(B_N|A) * P(A)}{P(B)}$$

Eigenschaften: Alle Argumentattribute unabhängig voneinander / gleich wichtig.

Verfahren: neue Instanz: {sunny, cool, high, windy}
Gesucht: P(play=yes|`<neue Instanz>`), P(play=yes|`<neue Instanz>`)
-> Entscheidung für den Klassifikationsattributswert mit größerem P.

##### Berechnung von Entscheidungsbäumen (Id3-Algorithmus)
Ergebnis: Entscheidungsbaum
- innere Knoten: 
	- enthalten Argumentattribute
	- ausgehend: Kanten mit Werten dieses Argumentattributs
- Blätter
	- enthalten Entscheidungen: Werte des Klassifikationsattributs

Prinzip: ++Informationsgewinn++(-> beruht auf dem Begriff "Information": info()) durch Auswertung eines Attributs: gain()

info(): Map (Kennzahl) für während eines Lernprozesses noch zu gewinnende Information
gain(): Map (Kennzahl) für Info-Gewinn (ist Differenz zwischen zwei Werten von info())

Eigenschaften von info(): Wert zwischen 0 und 1, dazwischen monoton.

Id3-Weiterentwicklungen: C 4.5, C 5.0, J 48

### Praktikum 2 (2015-10-27)
1)
`for $forscher in doc("forscher.xml")/wissenschaft/forscher`
`return $forscher/name/nname`
2)
`for $forscher in doc("forscher.xml")/wissenschaft/forscher`
`where $forscher/name/vname='Albert'`
`and $forscher/name/nname='Einstein'`
`return concat(	$forscher/hochschule, ", ", $forscher/herkunftsland, ", ", forscher/spezgebiet )`
3)
`for $forscher in doc("forscher.xml")/wissenschaft/forscher`
`where $forscher/hochschule='UBerlin'`
`return concat(string-join(($forscher/name/vname, $forscher/name/nname), " "), ",")`
4)
`for $proj in doc("wissprojekte.xml")/wissprojekte/projekt,`
`	$laeuft in doc("wissprojekte.xml")/wissprojekte/laeuft_an,`
`	$uni in doc("forscher.xml")/wissenschaft/uni`
`where $proj/@pnummer = $laeuft/@pnr`
`	and $laeuft/@uname = $uni/uname`
`	and $proj/pstandort/@bundesland="Bayern"`
`	return <a4 pnr="{$proj/@pnummer}" pname="{$proj/pname}" uname="{$uni/uname}" gruendung="{$uni/@gruendungsdatum}"/>`
... Rest im Repo ...

## Mitschrift 2015-11-03

#### Berechnung von Entscheidungslisten
- Grundlagen:
  - Regeln:
    - `if(<Bedingung>) then <Folgerung>`
    - `<Bedingung> => <Folgerung>`
  - ++Abdeckung AR++ einer Regel: die Anzahl der Instanzen der Trainingsdaten, für die Regel zutrifft.
  - ++Abdeckung AB++ der Bedingung einer Regel: die Anzahl der Instanzen der Trainingsdaten, die die Bedingung der Regel erfüllen, d.h. auf die die Regel angewandt werden kann.
  - ++Genauigkeit++: Genauigkeit einer Regel: $$$GK = \frac{AR}{AB}$$$

##### Prism-Algorithmus
- Aufstellung: initiale, triviale Regel:
 - ? bei der Bedingung
 - beliebige Entscheidungsklasse

 -> i.A. schlechte Genauigkeit
 
- Iterative Regelverbesserung
 - Integration (schrittweise) von Vergleichen in die Bedingung der Regel
 - Genauigkeit erhöht sich
 
- Ende der Regelverbesserung: hoffentlich ein 100% (Genauigkeit)
- Ergebnis: Regelmenge mit vordefinierter Genauigkeit (i.A. 100%)

Eigenschaft (Prism): Regeln können in beliebiger Reihenfolge angewendet werden **(was wirklich zu beweisen ist!)**
 
#### Berechnung von Assoziationsregeln (Apriori-Algorithmus)

Grundbegriffe: Gegenstand, ++Assoziationsregel++, Abdeckung, Genauigkeit

Prinzip Apriori-Algorithmus:
- Aus den Gegenstandsmengen von Trainingsdaten lassen sich per Kombination ihrer Gegenstände Assoziationsregeln erzeugen und deren Genauigkeiten berechnen.
- Nur die Regeln mit einer vorgegebenen Mindestgenauigkeit werden als Resultat des Apriori-Algorithmus anerkannt (100% od. 99%).

#### Weitere Algorithmen
###### Lineare Regression
- Verfahren zur numerischen Klassifikation
- Annahme: der Wert des numerischen Klassenattributs ist eine lineare Kombination der Werte der ebenfalls numerischen Argumentattribute.
- Die Konstanten (Gewichte) der linearen Kombination müssen anhand der Trainingsdaten optimiert werden.

## Mitschrift 2015-11-10
###### Nearest Neighbor Learning

#### Überprüfung der Qualität von Klassifizierern
Verfahren:
- Cross Validation: %-Maß für die Qualität eines Entscheidungsbaums bzw. einer Entscheidungsmatrix
- Confusion Matrix

### Geodatenbanken und Informationssysteme
#### Einführung
- Geodaten: Daten, die einen räumlichen Bezug haben.
- Geooobjekt: Objekt, das einen räumlichen Bezug hat.
- Geodatenbanksystem (GeoDBS): SW-System zur Speicherung von Geodaten.
- Geodatenbank (GeoDB): Datenbank innerhalb eines Geodatenbanksystems.
- Geoinformationssystem (GeoIS): Rechnergestütztes System zur Erfassung, Speicherung und Verarbeitung von räumlichen Daten. Besteht aus Hardware, Software, Daten, Anwendungen.
- Standards: DGC, ISO/TC211, SQL/MM

#### Konzeptionelle Datenmodelle
##### Eigenschaften von Geodaten
- geometrisch
- topologisch
- thematisch (sachlich, fachlich)
- temporär

###### Modellierung thematischer Eigenschaften:
- Objektbasiertes Datenmodell: thematische Eigenschaften werden Geoobjekten zugeordnet
- Raumbasiertes Datenmodell: thematische Eigenschaften werden den Punkten des Datenraumes zugeirdnet, durch eine Funktion $$$f:R^n \rightarrow M$$$ (M beliebige Menge)

###### Modellierung geometrischer Eigenschaften:
- Vektormodell: basiert auf Geoobjekten, komplexe Objekte werden aus einfacheren zusammengesetzt.
- Rastermodell: Einteilung des Datenraums in gleichförmige, regelmäßige Rasterzellen (Pixel), Objekte bestehen aus Menge von Rasterzellen.

### Praktikum 3 (2015-11-10)
**(auf toten Bäumen)**

## Mitschrift 2015-11-17
#### Feature Geometry Model
- konzeptionelles Datenmodell
- objektbasiertes Datenmodell
- Vektormodell
- Klassen
- Objekte 0,1,2,3-dimensional
- Objekt-Begrenzungen gerade/gebogen
- Polygone: äußeres, inneres, äußerer Ring, innere Ringe

#### Simple Feature Model
- konzeptionell
- objektbasiert
- Vektormodell
- Klassen
- Objekte 0,1,2-dimensional
- Objektbegrenzungen gerade
- Polygone

Repräsentation von Geoobjekten in maschinenlesbarer Form:
- WKT: well known text
- WKB: well known binary

WKT: POINT, POLYGON, ...

SFM enthält die topologische Modellierung von Geoobjekten, z.B.: Überschneidungen, Berührungen, ...

##### 9-Intersection-Modell (9IM):
3x3-Matrizen
Erweiterung: DE-9IM (dimensionally extended 9IM)

#### SQL/MM Spatial
-> analog

### Räumliche Anfragebearbeitung
#### Räumliche Basisanfragen
Punktanfragen, Fensteranfragen(?)... (altjürgische Hieroglyphen?!)
#### Mehrstufige Anfragebearbeitung
Motivation: sehr komplexe geometrische Objekte
Realisierung: Treffer, Fehltreffer, Koordinaten für nächsten Filterschritt
Realisierung für Geoobjekte in Geodatenbanken: Filterung über Approximation
###### Approximations-Eigenschaften:
- konservativ $$$\leftarrow\rightarrow$$$ progressiv
- einelementig $$$\leftarrow\rightarrow$$$ mehrelementig
- ...

## Mitschrift 2015-11-24
### Indexe bei Geodatenbanken
#### Einführung
Motivation: Geoobjekte sollen schnell gefunden werden!
Anforderungen:
- effiziente Ausführung von Basisanfragen
- Robustheit gegenüber Geodaten-Änderungen
- Gleichmäßige Speicherauslastung
- Robustheit gegenüber ungleich im Datenraum verteilten Objekten

#### Datenraumeinteilung
**MUR:** minimal umgebendes Rechteck: kleinstmögliches achsenparalleles Rechteck, das eine Menge von Geoobjekten umschließt.
**Datenraum:** Raum, in dem sich zu indizierende Objekte befinden.
**Blockregion:** einfach und klar abgrenzbarer Teil eines Datenraums.
Blockregionen erhalten Nummern: **Blockregionsnummern** (BRNs)

Aufgabe der Datenraumeinteilung: 
räumliche Lage eines Geoobjektes $$$\leftarrow\rightarrow$$$ Blockregion(-en) $$$\leftarrow\rightarrow$$$ BRNs ($$$\leftarrow$$$besitzen Ordnung) $$$\leftarrow\rightarrow$$$ Datenblock im Speicher

##### Grundtechniken zur Datenraumeinteilung
###### Clipping
- Einteilung des Raums in Blockregionen
- relativ willkürliche Nummerierung
- anschließend Konstruktion eines Index auf die BRNs
$$$\Rightarrow$$$Ineffizient insbesondere beim Hinzufügen/Löschen von Objekten

###### Einbettung in den 1-dimensionalen Raum
- z-Ordnung durch Zick-Zack-Kurve (fraktal) $$$\rightarrow$$$ z-Werte (x,y($$$\leftarrow$$$Auflösung))

###### Überlappende Blockregionen
Unregelmäßige Definition der Blockregionen, sodass jedes Geoobjekt in genau einer Blockregion enthalten ist. $$$\rightarrow$$$R-Bäume

##### Quadrantenbäume
Grundlage: Einbettung in 1-dimensionalen Raum
Ein Quadrantenbaum ist ein Baum, der einen k-dimensionalen Datenraum in $$$2^k$$$ gleich große Regionen rekursiv unterteilt.

Beispiele für Quadrantenbäume:
- PR (Point-Region)-Quadrantenbaum $$$\rightarrow$$$ Quadrantenbaum, der einzelne Punkte in (privaten) Regionen indiziert.
- linearer Quadrantenbaum: $$$B^+$$$- und Quadrantenbaum, der Blockregionen$$$^1$$$ oder Geoobjekte$$$^2$$$ indiziert nachdem diese über z-Werte in eine lineare Ordnung gebracht worden sind.
( $$$^1$$$ raumbezogener linearer Q-Baum)
( $$$^2$$$ datenbezogener Q-Baum)

### Praktikum 4 (2015-11-24)
siehe Mail an Jürgensen

## Mitschrift 2015-12-01

##### R-Bäume
Ein R-Baum ist ein balancierter Index, der multidimensionale Rechtecke mithilfe überlappender Blockregionen organisiert.
Knoten: Verzeichnisknoten (innere Knoten) mit Einträgen der Form (mur, ref)
- mur: immer alle MURs, die durch Kindknoten repräsentiert werden
- ref: Verweis auf einen Kindknoten

Datenknoten mit Einträgen der Form (gmur, gref)
- gmur: MUR eines Geoobjekts
- gref: Verweis auf dieses Geoobjekt

### Geometrische Algorithmen
#### Grundverfahren
- **inkrementelle Methode:** ausgehend von einer kleinen Datenmenge, nehme man immer wieder einen weiteren Teil der Datenmenge hinzu, bis das ganze Problem gelöst ist.
- **Teile-und-Herrsche-Methode (rekursiv):** Löse ein Problem durch Lösung seiner Teilprobleme,..., bis Trivialprobleme auftauchen.
- **Plane Sweep:** Verschiebe eine Linie (je nach Dim. Ebene,...) über den Datenraum, bei relevanten Punkten: stoppe und berechne.

#### Algorithmen für Inklusionsprobleme
Problem: ist ein Geoobjekt in einem anderen enthalten?
##### Punkt-in-Polygon-Test:
Punkt p, Polygon pol: Ist p in pol enthalten?

###### Algorithmus nach Jordan
Strahl von p in Richtung pol; ungerade Anzahl Schnittpunkte $$$\rightarrow$$$ p innerhalb, sonst außerhalb

###### Winkelsummentest
Berechnung von Winkeln: alle Strecken berechnen, die p mit den Polygoneckpunkten verbinden.
Winkel zwischen benachbarten Strecken berechnen, $$$\Sigma=360°\Rightarrow$$$Punkt innerhalb, $$$\Sigma=0°\Rightarrow$$$Punkt außerhalb.

##### Erweiterung: Polygon-in-Polygon-Test:
Polygone pol1, pol2:
Zuerst wird getestet, ob ein Eckpunkt von pol1 in pol2 enthalten ist (Punkt-in-Polygon-Test).
Ist das nicht der Fall, liegt pol1 nicht in pol2.
Andernfalls wird geprüft, ob eine Kante von pol1 eine Kante von pol2 schneidet.
Ist dies nicht der Fall, ist pol1 in pol2 enthalten, sonst nicht.

#### Algorithmen für Schnittprobleme
Schneiden sich geometrische Objekte?
Methode: z.B. Plane-Sweep
