# KI-Werk WissensCI

Test Toolchain zur automatischen Wissensprüfung

## Themenfelder

### Konsistenzprüfung

Sind die Axiome einer Wissensbasis ins sich konsistent?

#### Beispiel: Ein Traktor

```
(has_component some Motor)
 and (has_delivery some Delivery_Tractor_X)
 and (has_standard some Emission_Standard)
 and (has_storage some Storage_Tractor_X)
 and (has_weight some xsd:integer[< 5])
```

Inkonsistent wird die Wissensbasis, wenn man z.B. `has_weight some xsd:integer[> 5]` festlegen würde.

```bash
TODO
```

### Erfüllbarkeit

Erfüllbarkeit heißt: Alle Klassen einer Wissensbasis können instanziiert werden.

Nicht erfüllbar wäre eine Wissensbasis, wenn sie mindestens eine Klasse enthält, die nicht instanziierbar wäre.

Eine Klasse in OWL verhält sich analog wie eine in Java. Sie ist ein Bauplan und man kann Instanzen von ihr erzeugen. Diese Instanzen werden auch Individuen genannt.

```bash
TODO
```

### SHACL

```bash
./jena/bin/shacl validate --shapes SHAPES.ttl --data DATA.ttl
```

### SPARQL

```bash
./checker1 DATA.ttl query1.sparql
```

## Hinweise zu Protégé

* Protege's Reasoner schließt bei Prüfung auch Instanz-Daten mit ein. Das heißt, dass an einer Klasse "owl:Nothing" angezeigt wird, wenn eine ihrer Instanzen ungültige Werte nutzt
* Nutzt man einen Reasoner, dann muss nach Änderungen an der Wissensbasis dieser neu synchronisiert werden (Reasoner => Synchronize Reasoner)


## Dateien und Ordner

```
- Wissen
  - OWL
  - SPARQL
  - SHACL
  
Konfigurationsdatei
```
