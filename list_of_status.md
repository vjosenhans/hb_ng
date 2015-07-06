# Status-Gruppen

s.a. [http://bibliographie-trac.ub.rub.de/wiki/Priorisierung?version=14#Status-Gruppen](http://bibliographie-trac.ub.rub.de/wiki/Priorisierung?version=14#Status-Gruppen)

## Herkunft

(nur Redaktion nicht Welt)

**Anforderung:** 

* Datenlieferung durch RUBi
* Dateneingabe durch HB-Team 

**Lösung:**

Betrifft die "about-meta"-Ressourcen:

* `prov:wasInfluencedBy`
	* `prov:Activity` or `prov:Agent` or `prov:Entity`
* Ansatz 1: "RUBi" und "HB-Team" sind vom Typ `prov:Agent`
* Ansatz 2: "Datenlieferung durch RUBi" und "Dateneingabe durch HB-Team" sind `prov:Activity`
* Folge: Alle Personen und Organisationseinheiten im System müssen als potentielle Datenlieferanten vom Typ `prov:Agent` bzw. dessen Subklassen `prov:Organization`, `prov:Person`, `prov:SoftwareAgent` sein.

Die Triple werden dem entsprechenden *Named Graphs* zugeordnet, z.B. `ap-internal-non-public`.

## Priorität

**Anforderung:** 

* Dringend

**Lösung:**

Gehört nicht in die Daten des Redaktionssystems, sondern als Status zum erzeugten Ticket.

## Bearbeitungsstatus

**Anforderung "intern":** 

* Datensatz neu
* Datensatz in Bearbeitung durch XY
* Datensatz redaktioniert
* Datensatz in Bearbeitung durch Endredakteur XY 
* Datensatz fertig 

**Anforderung "extern":** 

* Datensatz neu
* Datensatz in Bearbeitung 

**Lösung:**

Der Status wird als `dcterms:hasVersion` an die "about-meta"-Resource modelliert. Je nach Sichtbarkeit werden die Triple den entsprechenden *Named Graphs* zugeordnet.

**Anforderung:** 

Historie für jeden Datensatz, aus der auch hervorgeht, welche Personen daran gearbeitet haben. 

**Lösung:**

Warum? Reicht für diese Form der Historie nicht das Ticket aus? 
Falls die Antwort "Nein" lautet, ist die Historie mittels der W3C PROV Ontology zu modellieren.

## Publikationstatium

**Anforderung:** 

* Eingereicht
* Angenommen
* Veröffentlicht
* in Vorbereitung 
* Preprint
* Postprint
* Verlagsversion 

**Lösung:**

Im internen *Application profile* `ap-internal` gilt:

* `rdam:editionStatement`
	* Preprint
	* Postprint
	* Verlagsversion (als default-Wert nicht angeben)
* `rdam:noteOnPublicationStatement`
	* Eingereicht
	* Angenommen
	* Veröffentlicht
	* in Vorbereitung 

Offene Frage: In welchen Kombinationen machen diese Angaben Sinn? In welchem Szenario wird z.B. "Eingereicht" benötigt?

## Besonderheiten

**Anforderung:** 

* Peer reviewed
* Open Access 

**Lösung:**

Im internen *Application profile* `ap-internal` gilt:

* `rdam:noteOnEditionStatement`
	* Peer reviewed
	* Open Access
* Validität:
	* Zu "Open Access" gehört noch die Angabe der Lizenz.

