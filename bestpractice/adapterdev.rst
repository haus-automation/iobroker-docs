.. _bestpractice-adapterdev:

Adapter-Entwicklung
===================

In diesem Dokument werden einige Code-Beispiele gesammelt, welche häufig bei der :ref:`development-adapter` benötigt werden.

.. danger::
    Diese Code-Beispiele sind NICHT für JavaScript-Code im JavaScript-Adapter gedacht! Dort werden teilweise andere Funktionen bzw. unterschiedliche Signaturen genutzt.

Eine Adapter-Klasse sollte immer von ``utils.Adapter`` erben. Die Basis-Klasse findest Du `hier <https://github.com/ioBroker/adapter-core>`_.

.. code:: javascript

    const utils = require('@iobroker/adapter-core');

    class MyAdapter extends utils.Adapter {
        // ...
    }

Objekt
------

Wie Objekte genau aufgebaut sind und welche Eigenschaften sie unterstützten, ist auf der Seite :ref:`development-objects` dokumentiert.

**Neues Objekt erstellen (im eigenen Namespace)**

Gerenell ist es empfehlenswert, Objekte über die ``instanceObjects`` in der :ref:`development-iopackage` anzulegen. Falls beim Adapter-Start bekannst ist, welche Objekte angelegt werden müssen, ist dieser Weg zu bevorzugen. Alternativ können Objekte per Code angelegt werden:

.. code:: javascript

    await this.setObjectNotExistsAsync(deviceName, {
        type: 'device',
        common: {
            name: 'Any name',
            type: 'string',
            role: 'text'
        },
        native: {}
    });

.. note::
    Es ist sinnvoll, den Namen aller Objekte (wenn möglich) direkt Mehrsprachig anzulegen! Dies wird z.B. vom Admin-Adapter und ``js-controller`` bereits berücksichtigt.

.. code:: javascript

    await this.setObjectNotExistsAsync(deviceName, {
        type: 'device',
        common: {
            name: {
                en: 'Any name',
                de: 'Jeder Name',
                ru: 'Любое имя',
                pt: 'Qualquer nome',
                nl: 'Iedere naam',
                fr: 'N\'importe quel nom',
                it: 'Qualche nome',
                es: 'Cualquier nombre',
                pl: 'Jakiekolwiek imię',
                'zh-cn': '任何名字'
            },
            type: 'string',
            role: 'text'
        },
        native: {}
    });

Alle Funktionen gibt es asynchron und mit callback. Jeweils für Objekte im eigenen Namespace und fremde Objekte.

.. code:: javascript

    await this.setObjectNotExistsAsync(id, obj, options);
    this.setObjectNotExists(id, obj, options, callback);

    await this.setForeignObjectNotExistsAsync(id, obj, options);
    this.setForeignObjectNotExists(id, obj, options, callback);

**Bestehendes Objekt aktualisieren (im eigenen Namespace)**

Wird ein Objekt aktualisiert, können geschützte Eigenschaften übergeben werden, welche nicht angefasst werden. Dazu zählt z.B. der Name des Objektes, welcher durch den Nutzer geändert werden kann.

.. code:: javascript

    await this.extendObjectAsync(deviceName, {
        type: 'device',
        common: {
            name: {
                en: 'Any name',
                de: 'Jeder Name',
                ru: 'Любое имя',
                pt: 'Qualquer nome',
                nl: 'Iedere naam',
                fr: 'N\'importe quel nom',
                it: 'Qualche nome',
                es: 'Cualquier nombre',
                pl: 'Jakiekolwiek imię',
                'zh-cn': '任何名字'
            },
            type: 'string',
            role: 'text'
        },
        native: {}
    }, { preserve: { common: ['name'] } } );

Alle Funktionen gibt es asynchron und mit callback. Jeweils für Objekte im eigenen Namespace und fremde Objekte.

.. code:: javascript

    await this.extendObjectAsync(id, obj, options);
    this.extendObject(id, obj, options, callback);

    await this.extendForeignObjectAsync(id, obj, options);
    this.extendForeignObject(id, obj, options, callback);

**Objekt löschen (im eigenen Namespace)**

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 2.2.8

.. code:: javascript

    await this.delObjectAsync(deviceName);

**Objekt rekursiv löschen (im eigenen Namespace)**

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 2.2.8

.. code:: javascript

    await this.delObjectAsync(deviceName, { recursive: true });

Alle Funktionen gibt es asynchron und mit callback. Jeweils für Objekte im eigenen Namespace und fremde Objekte.

.. code:: javascript

    await this.delObjectAsync(id, options);
    this.delObject(id, options, callback);

    await this.delForeignObjectAsync(id, options);
    this.delForeignObject(id, options, callback);

**Objekte lesen**

.. code:: javascript

    const allObjects = await this.getAdapterObjectsAsync(); // Alle folder, device, channel und state Objekte

    const devices = await this.getDevicesAsync();
    const channels = await this.getChannelsOfAsync('parentDevice'); // entspricht this.getChannelsAsync()
    const states = await this.getStatesOfAsync('parentDevice', 'parentChannel');

**Objekt View**

Möchte man viele Objekte auf einmal aus dem System abfragen, so eignet sich die Funktion ``getObjectViewAsync``. Mit dieser Funktion können alle möglichen Objekt-Typen (siehe :ref:`development-objects`) aus der Objekt-Datenbank abgefragt werden.

.. code:: javascript

    await getObjectViewAsync('system', 'instance', {
        startkey: 'system.adapter.',
        endkey: 'system.adapter.\u9999'
    });

State (Zustand)
---------------

Wie States genau aufgebaut sind und welche Eigenschaften sie unterstützten, ist auf der Seite :ref:`_development-states` dokumentiert.

**Wert schreiben (aktualisieren)**

.. note::
    Es ist darauf zu achten, dass der Datentyp des übergebenen Wertes zum definierten Datentyp auf dem Objekt passt.

.. code:: javascript

    await this.setStateAsync('myState', {val: newValue, ack: true});

Alternativ kann man den neuen Wert auch einzeln übergeben. Es ist zu empfehlen, immer ein komplettes State-Objekt zu übergeben, da dies ansonsten intern aufgebaut wird. Sollte ``newValue`` (versehentlich) ein Objekt sein, wird es als "fertiges" State-Objekt interpretiert, welchem dann wichtige Eigenschaften fehlen werden.

.. code:: javascript

    await this.setStateAsync('myState', newValue, true);

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    await this.setStateAsync(id, state, ack, options);
    this.setState(id, state, ack, options, callback);

    await this.setForeignStateAsync(id, state, ack, options);
    this.setForeignState(id, state, ack, options, callback);

**Wert schreiben (ändern)**

.. note::
    Es ist darauf zu achten, dass der Datentyp des übergebenen Wertes zum definierten Datentyp auf dem Objekt passt.

.. code:: javascript

    await this.setStateChangedAsync('myState', {val: newValue, ack: true});

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    await this.setStateChangedAsync(id, state, ack, options);
    this.setStateChanged(id, state, ack, options, callback);

    await this.setForeignStateChangedAsync(id, state, ack, options);
    this.setForeignStateChanged(id, state, ack, options, callback);

**Wert lesen**

Mehrere States auf einmal holen

.. code:: javascript

    const states = await this.getStatesAsync('pfad.im.eigenen.namespace.*');

Rückgabe:


Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    await this.getStatesAsync(pattern, options);
    this.getStates(pattern, options, callback);

    await this.getForeignStatesAsync(pattern, options);
    this.getForeignStates(pattern, options, callback);

Links
-----

- `adapter.js (js-controller 3.x) <https://github.com/ioBroker/ioBroker.js-controller/blob/3.3.x/lib/adapter.js>`_
- `adapter.js (js-controller 4.x) <https://github.com/ioBroker/ioBroker.js-controller/blob/4.0.x/packages/adapter/src/lib/adapter/adapter.js>`_
- `Adapter-Core <https://github.com/ioBroker/adapter-core>`_
- `Offizielle Doku <https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/adapterdev.md>`_
