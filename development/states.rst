.. _development-states:

States
======

.. note::
    Lies zuerst die Grundlagen zur :ref:`basics-datastorage`.

States können beispielsweise über das :ref:`basics-cli` ausgelesen werden:

.. code:: console

    iobroker state get admin.0.info.updatesNumber

Beispiel
--------

.. code:: json

    {
        "val": 0,
        "ack": true,
        "ts": 1633428163294,
        "lc": 1633092122629
        "q": 0,
        "from": "system.adapter.admin.0",
        "user": "system.user.admin"
    }

Eigenschaften
-------------

.. confval:: val

    Der aktuell gespeicherte Wert

    :type: string

.. confval:: ack

    Bestätigt-Flag, ob der (neue) Wert vom Adapter bzw. Ziel akzeptiert wurde

    :type: boolean

.. confval:: ts

    Unix Timestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **aktualisiert** wurde

    :type: number

.. confval:: lc

    Unix Zimestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **geändert** wurde (last change)

    :type: number

.. confval:: q

    Qualität

    :type: number

.. confval:: from

    Instanz, welche die Änderung durchgeführt hat (z.B. ``system.adapter.admin.0``) (optional)

    :type: string

.. confval:: user

    Benutzer, welcher die Änderung durchgeführt hat (z.B. ``system.user.admin``) (optional)

    :type: string

.. confval:: c

    Kommentar (optional)

    :type: string

.. confval:: expire

    Zeit in Sekunden, wann der Wert auf null gesetzt wird (optional)
    In diesem Beispiel ist der Wert ``0`` (numerisch Null).

    :type: number

Wenn man nur den Wert (also die Eigenschaft ``val``) auslesen möchte, geht das per :ref:`basics-cli` wie folgt:

.. code:: console

    iobroker state getvalue admin.0.info.updatesNumber
