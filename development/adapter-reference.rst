.. _development-adapterreference:

Adapter-Entwicklung (Code)
==========================

In diesem Dokument werden einige Code-Beispiele gesammelt, welche häufig bei der :ref:`development-adapter` benötigt werden.

.. danger::
    Diese Code-Beispiele sind NICHT für JavaScript-Code im JavaScript-Adapter gedacht! Dort werden teilweise andere Funktionen bzw. unterschiedliche Signaturen genutzt.

Objekte
-------

Wie Objekte genau aufgebaut sind, und welche Eigenschaften sie unterstützten, erfährst Du auf der Seite :ref:`development-objects`.

**Neues Objekt erstellen im eigenen Namespace**

Gerenell ist es empfehlenswert, Objekte über die ``instanceObjects`` in der :ref:`development-iopackage` anzulegen.

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

    await this.delObjectAsync(id, {recursive: true}, () => {
        this.log.debug('object deleted: ' + id);
    });



States
------

Wie States genau aufgebaut sind, und welche Eigenschaften sie unterstützten, erfährst Du auf der Seite :ref:`_development-states`.

**Wert schreiben**

.. code:: javascript

    await this.setStateAsync('myState', {val: newValue, ack: true});
    await this.setStateAsync('myState', newValue, true);
    await this.setStateAsync('myState', newValue, true, (err) => {
        if (err) this.log.error(err);
    });

Links
-----

- Ref: https://github.com/ioBroker/ioBroker.js-controller/blob/master/packages/adapter/lib/adapter/adapter.js
- Ref: https://github.com/ioBroker/ioBroker/wiki/Adapter-Development-Documentation
- Ref: https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/adapterdev.md
