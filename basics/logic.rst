.. _basics-logic:

Logik / Automation
==================

Wie Du schon gelernt hast, gibt es verschiedene Adapter, welche durch konfigurierte Instanzen über den ``js-controller`` Daten ablegen / entgegen nehmen können.

Am Ende kann sich jeder Adapter über die Änderung von :ref:`development-states` oder auch :ref:`development-objects` informieren lassen. In diesem Abschnitt geht es also um verschiedene "Logic-Engines", welche der ioBroker anbietet.

JavaScript-Adapter
------------------

Der beliebteste (und flexibelste) Adapter um eigene Logiken zu implementieren, ist der sogenannte JavaScript-Adapter. Dieser ermöglicht es, wie der Name schon sagt, mit eigenem JavaScript-Code eigene Logiken zu implementieren. Aber: Man muss dabei nicht unbedingt JavaScript-Code schreiben, sondern kann diesen auch automatisch generieren lassen, indem beispielsweise Blockly verwendet wird.

**Trigger**

Um Logik anzustoßen, wenn sich Zustände ändern, kann man eigene Auslöser im System registrieren. Dafür kann man die Objekt-ID und eine Funktion übergeben, welche bei Änderung ausgeführt wird.

.. code:: javascript

    on('0_userdata.0.myCustomState', (obj) => {
        const value = obj.state.val;
        const oldValue = obj.oldState.val;
    });

    // Identisch zu
    on({ id: '0_userdata.0.myCustomState', change: 'ne', q: 0 }, (obj) => {
        const value = obj.state.val;
        const oldValue = obj.oldState.val;
    });

Über die ``change``-Eigenschaft können noch weitere Einschränkungen getroffen werden, wann die Funktion ausgeführt werden soll.

- ``change: 'any'``: Der State wurde neu geschrieben
- ``change: 'gt'``: Der neue Wert ist größer als der vorige (greater than)
- ``change: 'ge'``: Der neue Wert ist größer oder gleich dem vorigen (greater or equal)
- ``change: 'lt'``: Der neue Wert ist kleiner als der vorige (lower than)
- ``change: 'le'``: Der neue Wert ist kleiner oder gleich dem vorigen (lower or equal)

Weiterhin gibt es die Eigenschaft ``val``, welche einen Filter auf genau einen Wert erlaubt:

.. code:: javascript

    on({ id: '0_userdata.0.myCustomState', val: true }, (obj) => {
        const value = obj.state.val;
        const oldValue = obj.oldState.val;
    });

    on({ id: '0_userdata.0.myNumber', val: 1337 }, (obj) => {
        const value = obj.state.val;
        const oldValue = obj.oldState.val;
    });

Um beispielsweise auf alle Werte größer als 100 zu reagieren, könnte man dies auch direkt im Trigger lösen:

.. code:: javascript

    on({ id: '0_userdata.0.myCustomState', change: 'ne', valGt: 100 }, (obj) => {
        // ...
    });

    // Identisch zu
    on({ id: '0_userdata.0.myCustomState', change: 'ne' }, (obj) => {
        if (obj.state.val > 100) {
            // ...
        }
    });

Links
-----

- `JavaScript-Adapter <https://github.com/ioBroker/ioBroker.javascript>`_