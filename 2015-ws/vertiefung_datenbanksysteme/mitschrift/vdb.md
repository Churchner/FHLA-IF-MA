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