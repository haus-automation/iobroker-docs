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

.. confval:: style_nav_header_background

    Changes the background of the search area in the navigation bar. The value
    can be anything valid in a CSS `background` property.

    :type: string
    :default: ``#2980B9``

- ``val`` - Der aktuell gespeicherte Wert
- ``ack`` - Bestätigt-Flag, ob der (neue) Wert vom Adapter bzw. Ziel akzeptiert wurde
- ``ts`` - Unix Timestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **aktualisiert** wurde
- ``lc`` - Unix Zimestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **geändert** wurde (last change)
- ``q`` - Qualität
- ``from`` - Welche Instanz hat die Änderung durchgeführt (z.B. ``system.adapter.admin.0``) (optional)
- ``user`` - Benutzer, welche die Änderung durchgeführt hat (z.B. ``system.user.admin``) (optional)
- ``c`` - Kommentar (optional)
- ``expire``- Zeit in Sekunden, wann der Wert auf null gesetzt wird (optional)

In diesem Beispiel ist der Wert ``0`` (numerisch Null).

Wenn man nur den Wert (also die Eigenschaft ``val``) auslesen möchte, geht das per :ref:`basics-cli` wie folgt:

.. code:: console

    iobroker state getvalue admin.0.info.updatesNumber
