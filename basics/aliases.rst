.. _basics-aliases:

Aliasse
=======

Eine Besonderheit stellt der Namespace ``alias.0`` dar. Hier können eigene Objekte angelegt werden, welche Informationen aus der Objektstruktur spiegeln. Das heißt: Die Werte in dieser Struktur liegen nicht in der Zustands-Datenbank, sondern werden immer aus der Quelle errechnet. Sie belegen also keinen weiteren Speicherplatz im System, sondern zeigen nur auf bestehende Datenpunkte bzw. dessen Werte.

Die Idee
--------

Warum sollte man das nutzen? Die Idee ist, dass in diesem Bereich jedes "Gerät" im System erneut angelegt wird.

Diese "Alias-Datenpunkte" kann man dann in der Visualisierung und in seiner :ref:`basics-logic` verwenden. Sollte man nun ein Gerät austauschen, muss man nur den Alias anpassen - der Rest funktioniert weiter.

Hat zum Beispiel ein HomeMatic- oder ZigBee-Taster einen Defekt und muss ausgetauscht werden, würde dieser mit einer neuen Seriennummer/ID in der jeweiligen Objektstruktur des Adapters (z.B. ``zigbee.0``) angelegt werden. Jetzt müsste man alle Stellen im System suchen und ändern, wo der alte Taster verwendet wurde. Hat man aber vorher einen Alias angelegt, muss nur eine einzelne Stelle angepasst werden.

Man setzt also nur die Verknüpfung neu. Das ist zwar beim Anlegen ein Schritt mehr, aber das zahlt sich in jedem Fall hinterher aus!

Konvertierungen
---------------

Nun kann ein Alias aber nicht nur einen Wert 1:1 spiegeln, sondern die Daten auch in beide Richtungen (lesend und schreibend) manipulieren / verändern. So können zum Beispiel Berechnungen durchgeführt werden.

.. note::
    Viele einfache Konvertierungen könnte man auch mit eigenen Scripts im JavaScript-Adapter realisieren. Das Ergebnis wäre das gleiche. Allerdings "verteilt" man dann die Logik über das ganze System und es ist schwieriger den Überblick zu behalten. Ein Alias ist immer zu bevorzugen, da so auf den ersten Blick zu erkennen ist, wo die Daten eigentlich her kommen. Außerdem läuft die Berechnung der Alias-Werte intern ab, und benötigt keine laufende Instanz eines Adapters!

Der Admin-Adapter bietet dafür im "Objekt bearbeiten"-Dialog für Objekte im ``alias.0`` Namespace ein weites Tab an, wo auch Konvertierungen beim lesen und schreiben aktiviert werden können.

- "lesen" bedeutet in diesem Fall, dass der Wert von der konfigurierten Quelle (Objekt-ID) geholt wird
- "schreiben" bedeutet, dass der Wert in die konfigurierte ID zurückgeschrieben wird, wenn sich der Wert im Alias ändert

Am Ende hat man hier alle Möglichkeiten, welche JavaScript bietet. Dabei wird der Parameter ``val`` (und weitere) angeboten, welcher den Wert des verknüpften Datenpunktes enthält.

Angenommen es gibt einen Datenpunkt, welcher die aktuelle Windgeschwindigkeit in Meter pro Sekunde (m/s) enthält. Jetzt soll der Wert aber in km/h umgerechnet werden. Genau das kann mit einem eigenen Alias sehr einfach gelöst werden:

**Lesen (m/s in km/h umrechnen)**

.. code:: javascript

    val * 3.6

**Schreiben (km/h in m/s umrechnen)**

.. code:: javascript

    val / 3.6

*Eine Schreib-Funktion ergibt in diesem Fall wenig Sinn, da der Wert wahrscheinlich nicht geschrieben werden kann. Die Quelle wird "readonly" sein. Der Wind wird sich nicht für unsere Geschwindigkeitsänderung interessieren!*

Bitte beachte, dass die Datentypen der Zustände korrekt sind. Hier wird der Datentyp der Quelle höchstwahrscheinlich ``number`` sein. Also sollte unser Alias ebenfalls vom Typ ``number`` sein, da das Ergebnis der Multiplikation ja wieder eine ``number`` ist.

Ein weiteres Beispiel wäre, Grad Fahrenheit in Grad Celsius umzurechnen:

.. code:: javascript

    (val - 32) * 5 / 9

und beim Schreiben könnte man dann Celsius wieder in Fahrenheit zurückrechnen:

.. code:: javascript

    val * 1.8 + 32

**Wert aus einem JSON extrahieren**

Angenommen ein Zustand (Typ ``string`` oder ``json``) enthält folgenden Wert: ``{"myAttr": 12, "blabla": "text"}``. Dann kann dieses JSON direkt im Alias geparsed und das Attribut ausgelesen werden:

.. code:: javascript

    JSON.parse(val).myAttr

**Ternary Operator**

Möchte man einen boolschen Wert in einen String umwandeln, kann dafür der "Ternary-Operator" genutzt werden. Liefert z.B. ein Fensterkontakt ``true`` (boolean) wenn das Fenster geschlossen ist, kann dieser Wert wie folgt in einen String gewandelt werden. Hier ist der Datentyp der Quelle ``boolean`` und der Datentyp des Alias ``string``:

.. code:: javascript

    val ? 'geschlossen' : 'offen'

Ist der Ausgangswert numerisch, können hier natürlich auch einen Vergleich anstellen. Falls vom lesenden Zustand der Wert kleiner als 15 ist, soll z.B. der Text "kalt" im Alias stehen. Hier ist der Datentyp der Quelle ``number`` und der Datentyp des Alias ``string``:

.. code:: javascript

    val < 15 ? 'kalt' : 'warm'

**Datum konvertieren**

Angenommen der Ausgangswert ist ein Unix-Timestamp (z.B. ``1650997245840``). Diesen kann man dann nach belieben mit `DateTimeFormat <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat/DateTimeFormat>`_ umwandeln.

Möchte man beispielsweise nur das Datum des Timestamps formatieren, dann gibts es folgende Möglichkeiten:

.. code:: javascript

    new Date(val).toISOString() // 2023-04-13T07:20:28.191Z
    new Intl.DateTimeFormat('de-DE').format(new Date(val)) // 13.4.2023
    new Intl.DateTimeFormat('de-DE', { dateStyle: 'short' }).format(new Date(val)) // 13.04.23
    new Intl.DateTimeFormat('de-DE', { dateStyle: 'medium' }).format(new Date(val)) // 13.04.2023
    new Intl.DateTimeFormat('de-DE', { dateStyle: 'long' }).format(new Date(val)) // 13. April 2023
    new Intl.DateTimeFormat('de-DE', { dateStyle: 'full' }).format(new Date(val)) // Donnerstag, 13. April 2023

Um nur den Wochentag zu extrahieren, gibt es folgende Möglichkeiten:

.. code:: javascript

    new Intl.DateTimeFormat('de-DE', { weekday: 'narrow' }).format(new Date(val)) // D
    new Intl.DateTimeFormat('de-DE', { weekday: 'short' }).format(new Date(val)) // Do
    new Intl.DateTimeFormat('de-DE', { weekday: 'long' }).format(new Date(val)) // Donnerstag

Möchte man nicht das Datum, sondern nur die Uhrzeit formatieren, gibt es verschiedene Möglichkeiten:

.. code:: javascript

    new Intl.DateTimeFormat('de-DE', { timeStyle: 'short' }).format(new Date(val)) // 09:20
    new Intl.DateTimeFormat('de-DE', { timeStyle: 'medium' }).format(new Date(val)) // 09:20:28
    new Intl.DateTimeFormat('de-DE', { timeStyle: 'long' }).format(new Date(val)) // 09:20:28 MESZ
    new Intl.DateTimeFormat('de-DE', { timeStyle: 'full' }).format(new Date(val)) // 09:20:28 Mitteleuropäische Sommerzeit

Wenn man z.B. nur die Stunde und Minute im Format ``HH:SS`` haben möchte, wäre das wie folgt möglich (verschiedene Schreibweisen, gleiches Ergebnis):

.. code:: javascript

    `${new Date(val).getHours()}:${new Date(val).getMinutes()}` // 09:20
    new Date(val).getHours() + ':' + ${new Date(val).getMinutes() // 09:20
    new Intl.DateTimeFormat('de-DE', { timeStyle: 'short' }).format(new Date(val)) // 09:20

Natürlich kann auch beliebig kombiniert werden:

.. code:: javascript

    new Intl.DateTimeFormat('de-DE', { dateStyle: 'full', timeStyle: 'full' }).format(new Date(val)) // Donnerstag, 13. April 2023 um 09:20:28 Mitteleuropäische Sommerzeit
    new Intl.DateTimeFormat('de-DE', { dateStyle: 'full', timeStyle: 'long' }).format(new Date(val)) // Donnerstag, 13. April 2023 um 09:20:28 MESZ
    new Intl.DateTimeFormat('de-DE', { dateStyle: 'medium', timeStyle: 'medium' }).format(new Date(val)) // 13.04.2023, 09:20:28
    new Intl.DateTimeFormat('de-DE', { dateStyle: 'medium', timeStyle: 'short' }).format(new Date(val)) // 13.04.2023, 09:20

**Werte runden**

Um einen numerischen Wert auf eine bestimmte Anzahl Nachkommastellen zu runden, eigenet sich ``.toFixed(x)``. Diese Funktion liefert allerdings einen String zurück! Das Ergebnis müsste also wieder in einen numerischen Wert konvertiert werden.

Auf eine Nachkommastelle runden (mehrere Möglichkeiten):

.. code:: javascript

    Number(val.toFixed(1))
    Math.round(val * 10) / 10

Der Trick: ``Math.round`` rundet immer auf eine natürliche Zahl. Wenn man nur eine Nachkommastelle erhalten möchte, kann man z.B. ``123.45`` mit 10 multiplizieren (ergibt ``1234.5``). Dann wird gerundet (ergibt ``1234``) und danach wieder durch 10 geteilt (ergibt ``123.4``).

Sollte der Ausgangswert vom Typ ``string`` sein, muss dieser vorher in einen numerischen Wert konvertiert werden:

.. code:: javascript

    Number(parseFloat(val).toFixed(1))
    Math.round(parseFloat(val) * 10) / 10

.. code:: javascript

**Regulärer Ausdruck**

Angenommen ein Zustand (Typ String) enthält folgenden Wert: ``123.45°C`` (also inklusive Einheit). Hier könnte man mit einem regulären Ausdruck alles außer Zahlen entfernen und den Wert in eine Gleitkommazahl umwandeln:

.. code:: javascript

    Number(val.replace(/[^\d.]/g, ''))

Das ginge auch deutlich einfacher, wenn einfach ``parseFloat`` verwendet wird. Mit dieser Funktion werden einfach alle "nicht-Zahlen" automatisch entfernt:

.. code:: javascript

    parseFloat(val)

Genauso könnte der Wert dann noch gerundet werden:

.. code:: javascript

    Math.round(Number(val.replace(/[^\d.]/g, '')))

**Wert in Liste enthalten**

Möchte man nur wissen, ob der aktuelle Wert in einer definierten Liste von Werten enthalten ist, könnte man dafür ein Array nutzen. Hier ist der Datentyp der Quelle ``number`` und der Datentyp des Alias ``boolean``:

.. code:: javascript

    [32, 45, 543, 23.2, 4209].includes(val)

Das gleiche klappt auch mit Strings. Hier ist der Datentyp der Quelle ``string`` und der Datentyp des Alias ``boolean``:

.. code:: javascript

    ['wert1', 'wert2', 'yyy', 'cooles beispiel'].includes(val)

**Eigene Logik ausführen**

Am Ende ist es ganz normales JavaScript. Also spricht auch (technisch) nichts dagegen, eine neue (anonyme) Funktion zu definieren, welche sofort ausgeführt wird. Das könnte so aussehen:

.. code:: javascript

    ((v) => { return v; })(val)
    (function(v) { return v; })(val)

Warum das Ganze? Jetzt könnte man eigene Variablen deklarieren und damit weiter arbeiten. Würde ich das empfehlen? Eher nicht - aber es ist möglich. Worauf zugegriffen werden kann? Das kann man einfach herausfinden:

.. code:: javascript

    Object.getOwnPropertyNames(this).join(', ')

Die interessantesten Eigenschaften sind wahrscheinlich ``parseFloat, parseInt, RegExp, Date, JSON, Math, Intl`` - also die Beispiele von weiter oben in diesem Abschnitt.

Einschränkungen
---------------

Wie an den Beispiele zu sehen ist, gibt es extrem viele Möglichkeiten, einen einzelnen Wert zu verändern / zu manipulieren. Aber es gibt auch ein paar Nachteile/Einschränkungen, was mit einem Alias nicht möglich ist:

**Werte überspringen / ignorieren**

Da der Wert des Alias nicht in der Zustands-Datenbank landet, sondern immer berechnet wird, können keine Werte "übersprungen" oder ausgelassen werden. Sollte z.B. der ``js-controller`` (oder das ganze System) neugestartet werden, muss der Wert wieder aus dem aktuellen Wert der Quelle berechnet werden können.

**Werte aus mehreren Datenpunkten berechnen**

Quelle und Ziel eines Alias ist immer genau ein Datenpunkt! Man kann nicht die Werte von zwei oder mehr Datenpunkten zusammenführen und daraus neue Werte berechnen. Dafür ist dann doch wieder ein Script (wie zum Beispiel im JavaScript-Adapter) notwendig.
