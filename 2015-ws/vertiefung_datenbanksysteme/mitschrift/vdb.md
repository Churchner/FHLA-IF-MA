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
1. //verein
2. a) //verein/@name
   b) 