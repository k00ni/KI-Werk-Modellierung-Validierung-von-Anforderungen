# KI-Werk WissensCI

Continuous Integration für Wissensbasen.

**Inhaltsverzeichnis:**

* [Grundbegriffe](#Grundbegriffe)
* [Konsistenzprüfung](#Konsistenzprüfung)
* [Erfüllbarkeit](#Erfüllbarkeit)
* [SHACL](#SHACL)
* [SPARQL](#SPARQL)
* [Sonstiges](#Sonstiges)

## Grundbegriffe

### Axiom

Ein Axiom ist eine Aussage, die innerhalb eines klar benannten Wissensgebietes als `wahr` angesehen wird.
Zum Bespiel wäre im Wissensgebiet "Personalverwaltung" sind folgende Aussagen `wahr`:

```
Ein Mitarbeiter ist eine Person.
Eine Person ist ein Mensch.
Hans ist ein Mitarbeiter und damit auch ein Mensch.
```

### Instanz-Daten

Instanz-Daten bezeichnet alle Instanzen aller Klassen einer [Wissensbasis](#Wissensbasis).
Klassen in einer Wissensbasis stellen Baupläne dar und Instanzen erlauben die Nutzung der Klassen.
Man nennt Instanzen manchmal auch Invididuen.

### Widerspruch

Ein Widerspruch ist eine Menge von mindestens zwei Aussagen, die alle (in der Wissensbasis) relevant sind, sich aber vom Inhalt nicht zusammen verwenden lassen.

### Wissensbasis

Im Kontext dieses Projektes ist eine Wissensbasis eine Sammlung von strukturierten Daten bzw. formalisiertes Wissen in einer Semantic Web Serialisierung.
Als Beispiel sei hier [RDF/Turtle](https://en.wikipedia.org/wiki/Turtle_(syntax)) genannt, welches in einer Datei namens `knowledge.ttl` vorliegen könnte.
Statt einer Datei kann dies auch in Form einer Datenbank (Quadstore) sein, spielt hier jedoch vorerst keine Rolle.

## Konsistenzprüfung

Bei einer Konsistenzprüfung wird geprüft, ob alle [Axiome](#Axiom) einer Wissensbasis in sich konsistent sind.
Allgemeiner formuliert kann man auch sagen, dass geschaut wird, ob eine Wissensbasis [Widersprüche](#Widerspruch) enthält.

### Beispiel: Ein Traktor

```
(has_component some Motor)
 and (has_delivery some Delivery_Tractor_X)
 and (has_standard some Emission_Standard)
 and (has_storage some Storage_Tractor_X)
 and (has_weight some xsd:integer[< 5])
```

Inkonsistent wird die Wissensbasis, wenn man z.B. `has_weight some xsd:integer[> 5]` festlegen würde.

Die `Checker.jar` nutzt standardmäßig die Datei `knowledge.ttl` und prüft, ob das Wissen konsistent ist:

```bash
java -jar ./bin/Checker.jar
```

Im Erfolgsfall wird "Knowledge consistent" auf der Konsole ausgegeben, ansonsten eine Fehlermeldung.

## Erfüllbarkeit

Eine Wissensbasis ist erfüllbar, wenn alle Klassen instanziiert werden können. 
Im Umkehrschluss heißt eine Wissensbasis nicht erfüllbar, wenn sie mindestens eine Klasse enthält, die nicht instanziierbar wäre.

Weißt heißt das nun konkret? Man könnte statt Erfüllbarkeit auch "nutzbar" wählen. Eine Klasse, von der man keine Instanzen erzeugen kann, ist nicht nutzbar. Daraus folgt, dass ihre Definition nicht in Ordnung ist. Eine Klasse in OWL verhält sich analog wie eine z.B. in Java. Sie ist ein Bauplan und man kann Instanzen von ihr erzeugen. Diese Instanzen werden auch Individuen genannt.

Die `Checker.jar` nutzt standardmäßig die Datei `knowledge.ttl` und prüft alle Klassen auf Erfüllbarkeit:

```bash
java -jar ./bin/Checker.jar
```

## SHACL

[SHACL](https://www.w3.org/TR/shacl/) kann genutzt werden, um Regeln für Wissensbasen zu beschreiben.
Mithilfe eines SHACL Prozessors wie [Apache Jena SHACL](https://jena.apache.org/documentation/shacl/index.html) kann geprüft werden, ob eine Wissensbasis die aufgestellten Regeln erfüllt.

Zu beachten ist, dass man nur Regeln über die [Instanzen][#Instanz-Daten] aufstellen kann. Regeln über Klassen und deren Beziehungen sind nicht möglich.

```bash
./apache-jena/bin/shacl validate --shapes SHAPES.ttl --data DATA.ttl
```

## SPARQL

[SPARQL](https://www.w3.org/TR/rdf-sparql-query/) ist eine Query-Sprache, um RDF-Daten abfragen zu können.

```bash
TODO
```

## Sonstiges

### Hinweise zu Protégé

* Protege's Reasoner schließt bei Prüfung auch Instanz-Daten mit ein. Das heißt, dass an einer Klasse "owl:Nothing" angezeigt wird, wenn eine ihrer Instanzen ungültige Werte nutzt
* Nutzt man einen Reasoner, dann muss nach Änderungen an der Wissensbasis dieser neu synchronisiert werden (Reasoner => Synchronize Reasoner)
