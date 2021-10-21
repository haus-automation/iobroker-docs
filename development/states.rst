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

    Bestätigt-Flag, ob der (neue) Wert vom Adapter bzw. Ziel-System akzeptiert wurde

    :type: boolean

.. confval:: ts

    Unix Timestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **aktualisiert** wurde

    :type: number

.. confval:: lc

    Unix Zimestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **geändert** wurde (last change)

    :type: number

.. confval:: q

    Qualität

    .. code::

        0x00 - 00000000 - good (can be undefined or null)
        0x01 - 00000001 - general bad, general problem
        0x02 - 00000010 - no connection problem

        0x10 - 00010000 - substitute value from controller
        0x20 - 00100000 - substitute initial value
        0x40 - 01000000 - substitute value from device or instance
        0x80 - 10000000 - substitute value from sensor

        0x11 - 01000001 - general problem by instance
        0x41 - 01000001 - general problem by device
        0x81 - 10000001 - general problem by sensor

        0x12 - 00010010 - instance not connected
        0x42 - 01000010 - device not connected
        0x82 - 10000010 - sensor not connected

        0x44 - 01000100 - device reports error
        0x84 - 10000100 - sensor reports error

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
