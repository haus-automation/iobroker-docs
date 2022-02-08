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

**Neues Objekt im eigenen Namespace erstellen**

Gerenell ist es empfehlenswert, Objekte über die ``instanceObjects`` in der :ref:`development-iopackage` anzulegen. Solltest Du beim Adapter-Start schon wissen, welche Objekte Du später haben möchtest, nutze diese Funktion.

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

**Objekt im eigenen Namespace löschen (rekursiv)**

.. code:: javascript

    this.delObjectAsync(deviceName, {recursive: true}, () => {
        this.log.debug('object deleted: ' + id);
    });

**Objekte lesen**

.. code:: javascript

    const channels = await this.getChannelsAsync('pfad');

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

- `adapter.js <https://github.com/ioBroker/ioBroker.js-controller/blob/master/packages/adapter/lib/adapter/adapter.js>`_
- `Adapter-Core <https://github.com/ioBroker/adapter-core>`_
- `Offizielle Doku <https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/adapterdev.md>`_
