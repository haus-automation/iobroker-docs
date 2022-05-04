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

Objekte
-------

Wie Objekte genau aufgebaut sind, und welche Eigenschaften sie unterstützten, erfährst Du auf der Seite :ref:`development-objects`.

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

**Objekt rekursiv löschen (im eigenen Namespace)**

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 2.2.8

.. code:: javascript

    this.delObjectAsync(deviceName, {recursive: true}, () => {
        this.log.debug('object deleted: ' + id);
    });

**Objekte lesen**

.. code:: javascript

    const allObjects = await this.getAdapterObjectsAsync(); // Alle folder, devices, channels und state Objekte

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

States
------

Wie States genau aufgebaut sind, und welche Eigenschaften sie unterstützten, erfährst Du auf der Seite :ref:`_development-states`.

**Wert schreiben / ändern**

.. code:: javascript

    await this.setStateAsync('myState', {val: newValue, ack: true});

Alternativ kann man den neuen Wert auch einzeln übergeben. Allerdings würde ich empfehlen, immer ein komplettes State-Objekt zu übergeben, da dies ansonsten intern aufgebaut wird. Sollte ``newValue`` (versehentlich) ein Objekt sein, wird es als "fertiges" State-Objekt interpretiert, welchem dann wichtige Eigenschaften fehlen werden.

.. code:: javascript

    await this.setStateAsync('myState', newValue, true);

**Wert lesen**

Mehrere States auf einmal holen

.. code:: javascript

    const states = await this.getStatesAsync('pfad.im.eigenen.namespace.*');

Rückgabe:



Links
-----

- `adapter.js (js-controller 3.x) <https://github.com/ioBroker/ioBroker.js-controller/blob/3.3.x/lib/adapter.js>`_
- `adapter.js (js-controller 4.x) <https://github.com/ioBroker/ioBroker.js-controller/blob/4.0.x/packages/adapter/src/lib/adapter/adapter.js>`_
- `Adapter-Core <https://github.com/ioBroker/adapter-core>`_
- `Offizielle Doku <https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/adapterdev.md>`_
