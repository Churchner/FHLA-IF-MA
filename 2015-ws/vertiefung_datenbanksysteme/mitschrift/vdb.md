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