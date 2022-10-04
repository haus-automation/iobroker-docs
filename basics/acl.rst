.. _basics-acl:

ACL (Access Control List)
=========================

Im ioBroker werden Rechte für verschiedene Typen definiert:

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

Werden beim Anlegen eines neuen Objektes keine Rechte definiert, greifen die definierten Standardrechte aus dem Objekt ``system.config``. Diese Standardrechte können z.B. über den Admin-Adapter angepasst werden.

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

Überprüft werden die Rechte intern - das sieht beispielsweise so aus:

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
