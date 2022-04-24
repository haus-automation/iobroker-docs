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

