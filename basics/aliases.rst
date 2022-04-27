.. _basics-aliases:

Aliasse
=======

Eine Besonderheit stellt der Namespace ``alias.0`` dar. Hier können eigene Objekte angelegt werden, welche Informationen aus der Objektstruktur spiegeln.

Die Idee
--------

Warum sollte man das nutzen? Die Idee ist, dass in diesem Bereich jedes "Gerät" im System erneut angelegt wird.

Diese "Alias-Datenpunkte" kann man dann in der Visualisierung und in seiner :ref:`basics-logic` verwenden. Sollte man nun ein Gerät austauschen, muss man nur den Alias anpassen - der Rest funktioniert weiter.

Hat zum Beispiel ein HomeMatic- oder ZigBee-Taster einen Defekt und muss ausgetauscht werden, würde dieser mit einer neuen Seriennummer/ID in der jeweiligen Objektstruktur des Adapters (z.B. ``zigbee.0``) angelegt werden. Jetzt müsste man alle Stellen im System suchen und ändern, wo der alte Taster verwendet wurde. Hat man aber vorher einen Alias angelegt, muss nur eine einzelne Stelle angepasst werden.

Konvertierungen
---------------

Nun kann ein Alias aber nicht nur einen Wert 1:1 spiegeln, sondern die Daten auch in beide Richtungen manipulieren / verändern. So können zum Beispiel Berechnungen durchgeführt werden.

.. note::
    Viele einfache Konvertierungen könnte man genauso gut mit eigenen Scripts im JavaScript-Adapter realisieren. Das Ergebnis wäre das gleiche. Allerdings verteilt man dann die Logik über das ganze System und es ist schwieriger den Überblick zu behalten. Ein Alias ist aus meiner Sicht immer zu bevorzugen.

Der Admin-Adapter bietet dafür im "Objekt bearbeiten"-Dialog für Objekte im ``alias.0`` Namespace ein weites Tab an, wo auch Konvertierungen beim lesen und schreiben aktiviert werden können.

- "lesen" bedeutet in diesem Fall, dass der Wert von der konfigurierten Quelle geholt wird
- "schreiben" bedeutet, dass der Wert in die konfigurierte ID zurückgeschrieben wird, wenn sich der Wert im Alias ändert

Am Ende hat man hier alle Möglichkeiten, welche JavaScript bietet. Dabei wird der Parameter ``val`` angeboten, welcher den Wert des verknüpften Datenpunktes enthält.

Angenommen Du hast einen Datenpunkt, welcher die aktuelle Windgeschwindigkeit in Meter pro Sekunde (m/s) enthält. Jetzt interessiert Dich der Wert aber in km/h. Genau das könntest Du mit einem eigenen Alias lösen:

**Lesen (m/s in km/h umrechnen)**

.. code:: javascript

    val * 3.6

**Schreiben (km/h in m/s umrechnen)**

.. code:: javascript

    val / 3.6

*Eine Schreib-Funktion macht in diesem Fall wenig Sinn, weil Du ja den Wert nicht schreiben kannst. Die Quelle wird readonly sein.*

Bitte beachte, dass die Datentypen der Objekte korrekt sind. Hier wird der Typ der Quelle höchstwahrscheinlich ``number`` sein. Also sollte unser Alias ebenfalls vom Typ ``number`` sein, da das Ergebnis der Multiplikation ja wieder eine Number ist.

**Wert aus einem JSON extrahieren**

Angenommen ein Zustand (Typ String) enthält folgenden Wert: ``{myAttr: 12}``. Dann kann diesen JSON direkt im Alias geparsed und das Attribut ausgelesen werden:

.. code:: javascript

    JSON.parse(val).myAttr

**Ternary Operator**

Möchte man einen boolschen Wert in einen String umwandeln, kann dafür der Ternary-Operator genutzt werden. Liefert z.B. ein Fensterkontakt ``true`` wenn das Fenster geschlossen ist, kann dieser Wert wie folgt in einen String gewandelt werden:

.. code:: javascript

    val ? 'offen' : 'geschlossen'

Ist der Ausgangswert numerisch, können hier natürlich auch einen Vergleich anstellen. Falls vom lesenden Zustand der Wert kleiner als 15 ist, soll z.B. der Text "kalt" im Alias stehen:

.. code:: javascript

    val < 15 ? 'kalt' : 'warm'

**Datum konvertieren**

Angenommen der Ausgangswert ist ein Unix-Timestamp (z.B. ``1650997245840``). Diesen kann man dann nach belieben umwandeln:

.. code:: javascript

    new Date(val).toISOString() // "2022-04-26T18:20:45.840Z"
    new Intl.DateTimeFormat('de-DE').format(new Date(val)) // "26.4.2022"
    new Intl.DateTimeFormat('de-DE', { dateStyle: 'medium' }).format(new Date(val)) // "26.04.2022"
    new Intl.DateTimeFormat('de-DE', { dateStyle: 'full', timeStyle: 'long' }).format(new Date(val)) // "Dienstag, 26. April 2022 um 20:20:45 MESZ"
    new Intl.DateTimeFormat('de-DE', { timeStyle: 'medium' }).format(new Date(val)) // "20:20:45"

Wenn man z.B. nur die Stunde und Minute im Format ``HH:SS`` haben möchte, wäre das wie folgt möglich (zwei verschiedene Schreibweisen, gleiches Ergebnis):

.. code:: javascript

    `${new Date(val).getHours()}:${new Date(val).getMinutes()}`
    new Date(val).getHours() + ':' + ${new Date(val).getMinutes()
    new Intl.DateTimeFormat('de-DE', { timeStyle: 'short' }).format(new Date(val))

**Regulärer Ausdruck**

Angenommen ein Zustand (Typ String) enthält folgenden Wert: ``123.45°C`` (also inklusive Einheit). Hier könnte man mit einem regulären Ausdruck alles außer Zahlen entfernen und den Wert in eine Gleitkommazahl umwandeln:

.. code:: javascript

    parseFloat(val.replace(/[^\d.]/g, ''))

Genauso könnte der Wert dann noch gerundet werden:

.. code:: javascript

    Math.round(parseFloat(val.replace(/[^\d.]/g, '')))

**Eigene Logik ausführen**

Am Ende ist es ganz normales JavaScript. Also spricht auch nichts dagegen, eine neue (anonyme) Funktion zu definieren, welche sofort ausgeführt wird. Das könnte so aussehen:

.. code:: javascript

    ((v) => { return v; })(val)
    (function(v) { return v; })(val)

Warum das Ganze? Jetzt könnte man eigene Variablen deklarieren und damit weiter arbeiten. Würde ich das empfehlen? Eher nicht - aber es ist möglich. Worauf Du zugreifen kannst? Das kannst Du einfach herausfinden:

.. code:: javascript

    Object.getOwnPropertyNames(this).join(', ')

Die interessantesten sind wahrscheinlich ``parseFloat, parseInt, RegExp, Date, JSON, Math, Intl``
