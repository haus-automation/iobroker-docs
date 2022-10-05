.. _basics-acl:

ACL (Access Control List)
=========================

Genau wie in vielen anderen Systemen auch, kennt der ioBroker Benutzer und Gruppen. So können Rechte für verschiedene Entitäten gepflegt werden, um Beispielsweise anderen Nutzern im ioBroker eingeschränkte Rechte zu geben. Das kann nützlich sein, damit ein Kunde z.B. nicht den vollen Zugriff auf die Objekt-Datenbank hat und versehentlich Objekte löscht oder verändert.

Leider ist es (wie bei vielen Betriebssystemen) so, dass die Anwender mit Administrator-Rechten arbeiten. So gibt es wohl auf den meisten privaten Windows-PCs nur einen Benutzer, welcher Admin ist und alles darf. Somit beschäftigen sich die meisten Anwender gar nicht mit dem Rechtesystem. Und so ist es bei den ioBroker-Anwendern in den meisten Fällen auch.

Im Standard wird ein Admin-Benutzer angelegt, welcher auch nicht gelöscht werden kann. Dieser hat die Objekt-ID ``system.user.admin`` und ist Mitglied in der Standard-Gruppe ``system.group.administrator``. Diesem Nutzer werden (über die Standardrechte - siehe unten) alle neuen Objekte direkt zugewiesen.

Entitäten
---------

Im ioBroker werden Rechte für verschiedene Entitäten definiert:

- Object (Objekt)
- State (Zustand)
- File (Dateien)

Die gültigen Rechte werden zusammen mit dem Besitzer und der Besitzer-Gruppe auf dem Objekt selbst im Attribut ``acl`` gespeichert - siehe auch :ref:`development-objects`.

Beispiel-ACL für ein Objekt vom Typ ``state``:

.. code:: json

    "acl": {
        "object": 1636,
        "state": 1636,
        "owner": "system.user.admin",
        "ownerGroup": "system.group.administrator"
    }

Beispiel-ACL für alle weiteren Objekt-Typen (Attribut ``state`` fehlt):

.. code:: json

    "acl": {
        "owner": "system.user.admin",
        "ownerGroup": "system.group.administrator",
        "object": 1604
    }

Standardrechte
--------------

Werden beim Anlegen eines neuen Objektes keine Rechte definiert, greifen die Standardrechte aus dem Objekt ``system.config``. Diese Standardrechte können beispielsweise über den Admin-Adapter angepasst werden.

.. code:: json

    "defaultNewAcl": {
        "object": 1636,
        "state": 1636,
        "file": 1636,
        "owner": "system.user.admin",
        "ownerGroup": "system.group.administrator"
    }

Berechnung
----------

Es wird zwischen Rechten für den Besitzer (Owner), die Gruppe (Group) und alle anderen (Everyone) Unterschieden. 

Ähnlich wie bei Linux, ergeben sich die Rechte aus einer Bitmaske:

.. code::

    Besitzer    Objekt lesen        0x400
    Besitzer    Objekt schreiben    0x200

    Gruppe      Objekt lesen        0x040
    Gruppe      Objekt schreiben    0x020

    Jeder       Objekt lesen        0x004
    Jeder       Objekt schreiben    0x002

    Besitzer    Zustand lesen       0x400
    Besitzer    Zustand schreiben   0x200

    Gruppe      Zustand lesen       0x040
    Gruppe      Zustand schreiben   0x020

    Jeder       Zustand lesen       0x004
    Jeder       Zustand schreiben   0x002

Alle Rechte (für alle) wäre also ``0x666`` - (binär ``0110 0110 0110``) ergibt Dezimal ``1638``. Der Wert wird Dezimal auf dem Objekt gespeichert (Beispiele siehe oben), da das JSON-Format laut Spezifikation keine Hexadezimalwerte erlaubt.

.. note::
    Unter Linux gibt es noch das Recht "ausführen", wodurch die Maximalrechte dort 0x777 sind. Objekte oder Zustände auszuführen ergibt aber wenig Sinn, daher der Unterschied.

Auf Rechte prüfen
-----------------

.. note::
    Dieser Abschnitt dient nur als Information. Die Rechte-Prüfungen finden intern statt und müssen nicht durch den Anwender geprüft werden!

Über ein logisches UND kann geprüft werden, ob Rechte vorhanden sind - das sieht beispielsweise so aus:

.. code:: javascript

    if (obj.acl.state & 0x400) {
        /* owner can read state */
    }

    if (obj.acl.state & 0x200) {
        /* owner can write state */
    }

    if (obj.acl.state & 0x40) {
        /* group can read state */
    }

    if (obj.acl.state & 0x20) {
        /* group can write state */
    }

    if (obj.acl.state & 0x4) {
        /* everyone can read state */
    }

    if (obj.acl.state & 0x2) {
        /* everyone can write state */
    }

Angenommen die Rechte eines Objektes sind dezimal ``1636`` - also ``0x664``. Bei einer Prüfung auf **Leserechte der Gruppe** würde man diesen Wert nun mit ``0x40`` verknüpfen (siehe oben). Beispiel:

.. code::

    1636 -> 0x664 -> 0110 0110 0100 (Objekt-Rechte)
      64 -> 0x040 -> 0000 0100 0000 (Gruppe Objekt lesen)
    ------------------------------- UND
                     0000 0100 0000

Das Ergebnis ist also größer Null und somit hat die Gruppe Leserechte.
