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

    await this.setObjectNotExistsAsync('exampleDevice', {
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

    await this.setObjectNotExistsAsync('exampleDevice', {
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

Rückgabe:

.. code:: json

    {"id":"xxx.0.exampleDevice"}

.. note::
    Je allgemeiner die Funktion, desto weniger Prüfungen werden durchgeführt. "setObject" prüft dabei am wenigsten.

.. code::

    createDevice
    createChannel
    createState
        setObjectNotExists
            setObjectWithDefaultValue
                setObject

Alle Funktionen gibt es asynchron und mit callback. Jeweils für Objekte im eigenen Namespace und fremde Objekte.

.. code:: javascript

    // set[Foreign]Object[Async]
    // Erstellt (oder überschreibt) das Objekt
    await this.setObjectAsync(id, obj, options);
    this.setObject(id, obj, options, callback);

    await = this.setForeignObjectAsync(id, obj, options);
    this.setForeignObject(id, obj, options, callback)

    // Wrapper für set[Foreign]Object
    this.setObjectWithDefaultValue(id, obj, options, callback);

    // set[Foreign]ObjectNotExists[Async]
    // Erstellt das Objekt nur, wenn es nicht existiert
    // Wrapper für setObjectWithDefaultValue
    await this.setObjectNotExistsAsync(id, obj, options);
    this.setObjectNotExists(id, obj, options, callback);

    await this.setForeignObjectNotExistsAsync(id, obj, options);
    this.setForeignObjectNotExists(id, obj, options, callback);

    // Wrapper für setObjectNotExists mit type = 'device'
    await this.createDeviceAsync(deviceName, common, _native, options);
    this.createDevice(deviceName, common, _native, options, callback);

    // Wrapper für setObjectNotExists mit type = 'channel'
    await this.createChannelAsync(parentDevice, channelName, roleOrCommon, _native, options);
    this.createChannel(parentDevice, channelName, roleOrCommon, _native, options, callback);

    // Wrapper für setObjectNotExists mit type = 'state'
    await this.createStateAsync(parentDevice, parentChannel, stateName, roleOrCommon, _native, options);
    this.createState(parentDevice, parentChannel, stateName, roleOrCommon, _native, options, callback);

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

    // extend[Foreign]Object[Async]
    await this.extendObjectAsync(id, obj, options);
    this.extendObject(id, obj, options, callback);

    await this.extendForeignObjectAsync(id, obj, options);
    this.extendForeignObject(id, obj, options, callback);

**Objekte lesen**

.. todo::
    Add examples

Alle Funktionen gibt es asynchron und mit callback. Jeweils für Objekte im eigenen Namespace und fremde Objekte.

.. code:: javascript

    // get[Foreign]Object[Async]
    await this.getObjectAsync(id, options, callback);
    this.getObject(id, options, callback);

    await this.getForeignObject(id, options, callback);
    this.getForeignObject(id, options, callback);

    await this.findForeignObjectAsync(id, type, options);
    this.findForeignObject(id, type, options, callback);

.. code:: javascript

    const allObjects = await this.getAdapterObjectsAsync(); // Alle folder, device, channel und state Objekte
    this.getAdapterObjects(callback); // Alle folder, device, channel und state Objekte

**Objekt View**

Möchte man viele Objekte auf einmal aus dem System abfragen, so eignet sich die Funktion ``getObjectView``. Mit dieser Funktion können alle möglichen Objekt-Typen (siehe :ref:`development-objects`) aus der Objekt-Datenbank abgefragt werden.

.. code:: javascript

    await getObjectViewAsync('system', 'instance', {
        startkey: 'system.adapter.',
        endkey: 'system.adapter.\u9999'
    });

Alle Funktionen gibt es asynchron und mit callback. Jeweils für Objekte im eigenen Namespace und fremde Objekte.

.. code:: javascript

    await this.getObjectListAsync(params, options);
    this.getObjectList(params, options, callback);

    await this.getObjectViewAsync(design, search, params, options);
    this.getObjectView(design, search, params, options, callback);

    // Wrapper für getObjectView mit search "device"  + namespace filter
    await this.getDevicesAsync();
    this.getDevices();

    // Wrapper für getObjectView mit search "channel" + namespace filter
    await this.getChannelsOfAsync(parentDevice, options, callback);
    this.getChannelsOf(parentDevice, options, callback);

    // Wrapper für getObjectView mit search "state" + namespace filter
    await this.getStatesOfAsync(parentDevice, parentChannel, options);
    this.getStatesOf(parentDevice, parentChannel, options, callback);

    // Wrapper für getObjectView mit search "enum"
    await this.getEnumAsync(_enum, options);
    this.getEnum(_enum, options, callback);

    await this.getEnumsAsync(_enumList, options);
    this.getEnums(_enumList, options, callback);

    // Wrapper für getObjectView
    await this.getForeignObjectsAsync(pattern, type, enums, options);
    this.getForeignObjects(pattern, type, enums, options, callback);

**Objekt löschen (im eigenen Namespace)**

.. note::
    Der zugehörige State wird ebenfalls gelöscht (falls type = state)

.. code:: javascript

    await this.delObjectAsync(deviceName);

**Objekt rekursiv löschen (im eigenen Namespace)**

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 2.2.8

.. code:: javascript

    await this.delObjectAsync(deviceName, { recursive: true });

Alle Funktionen gibt es asynchron und mit callback. Jeweils für Objekte im eigenen Namespace und fremde Objekte.

.. code:: javascript

    // del[Foreign]Object[Async]
    await this.delObjectAsync(id, options);
    this.delObject(id, options, callback);

    await this.delForeignObjectAsync(id, options);
    this.delForeignObject(id, options, callback);

    // Wrapper für delForeignObjectAsync (mit recursive = true)
    await this.deleteDeviceAsync(deviceName, options);
    this.deleteDevice(deviceName, options, callback);

    // Wrapper für delForeignObjectAsync (mit recursive = true)
    await this.deleteChannelAsync(parentDevice, channelName, options);
    this.deleteChannel(parentDevice, channelName, options, callback);

    // Wrapper für delForeignObjectAsync
    await this.deleteStateAsync(parentDevice, parentChannel, stateName, options);
    this.deleteState(parentDevice, parentChannel, stateName, options, callback);

State (Zustand)
---------------

Wie States genau aufgebaut sind und welche Eigenschaften sie unterstützten, ist auf der Seite :ref:`_development-states` dokumentiert.

**Wert schreiben (aktualisieren)**

.. note::
    Es ist darauf zu achten, dass der Datentyp des übergebenen Wertes zum definierten Datentyp auf dem Objekt passt.

.. code:: javascript

    await this.setStateAsync('myState', {val: newValue, ack: true});

Alternativ kann der neue Wert auch einzeln übergeben werden. Es ist empfohlen, immer ein komplettes State-Objekt zu übergeben, da dies ansonsten intern aufgebaut wird. Sollte ``newValue`` (versehentlich) ein Objekt sein, wird es als "fertiges" State-Objekt interpretiert, welchem dann wichtige Eigenschaften fehlen werden.

.. code:: javascript

    await this.setStateAsync('myState', newValue, true);

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    // set[Foreign]State[Async]
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

    // set[Foreign]StateChanged[Async]
    await this.setStateChangedAsync(id, state, ack, options);
    this.setStateChanged(id, state, ack, options, callback);

    await this.setForeignStateChangedAsync(id, state, ack, options);
    this.setForeignStateChanged(id, state, ack, options, callback);

**Wert schreiben (binär)*+

:octicon:`git-branch;1em;sd-text-info` Geänderte Signaturen seit ``js-controller`` 4.0.15 (setForeignBinaryState)

Um Binärdaten in States zu speichern, muss das Objekt vom Typ ``common.type = 'file'`` sein. Für mehr Details siehe :ref:`bestpractice-storefiles`.

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    // set[Foreign]BinaryState[Async]
    await this.setBinaryStateAsync(id, binary, options);
    this.setBinaryState(id, binary, options, callback);

    await this.setForeignBinaryStateAsync(id, binary, options);
    this.setForeignBinaryState(id, binary, options, callback);

**Wert lesen**

Um den aktuellen Zustand eines States zu bekommen, können einzelne Werte aus der Datenbank abgefragt werden:

.. note::
    Sollte der State ein Alias sein, wird automatisch der State des Verknüpften Objektes zurückgegeben.

.. code:: javascript

    const state = await this.getStateAsync('myState');
    const value = state ? state.val : undefined;
    // ...

    // oder
    this.getState('myState', (err, state) => {
        if (!err) {
            // Das hat nich geklappt!
        } else {
            const value = state.val;
            // ...
        }
    });

Rückgabe:

Es wird ein vollständiges State-Objekt zurückgegeben. Siehe :ref:`development-states`.

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    // get[Foreign]State[Async]
    await this.getStateAsync(id, options);
    this.getState(id, options, callback)

    await this.getForeignStateAsync(id, options);
    this.getForeignStates(id, options, callback);

**Wert lesen (mehrere auf einmal)**

.. todo::
    Add examples

.. code:: javascript

    const states = await this.getStatesAsync('pfad.im.eigenen.namespace.*');

Rückgabe:

.. todo::
    Add examples

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    // get[Foreign]States[Async]
    await this.getStatesAsync(pattern, options);
    this.getStates(pattern, options, callback);

    await this.getForeignStatesAsync(pattern, options);
    this.getForeignStates(pattern, options, callback);

**Wert lesen (binär)*+

:octicon:`git-branch;1em;sd-text-info` Geänderte Signaturen seit ``js-controller`` 4.0.15 (getForeignBinaryState)

Um Binärdaten in States zu lesen, muss das Objekt vom Typ ``common.type = 'file'`` sein. Für mehr Details siehe :ref:`bestpractice-storefiles`.

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    // get[Foreign]BinaryState[Async]
    await this.getBinaryStateAsync(id, options);
    this.getBinaryState(id, options, callback);

    await this.getForeignBinaryStateAsync(id, options);
    this.getForeignBinaryState(id, options, callback);

**Wert löschen**

.. note::
    Das zugehörige Objekt wird nicht gelöscht

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    // del[Foreign]State[Async]
    await this.delStateAsync(id, options);
    this.delState(id, options, callback);

    await this.delForeignStateAsync(id, options);
    this.delForeignState(id, options, callback);

Timeout / Interval
------------------

Die basis Adapter-Implementierung erlaubt das verwalten von Timeouts und Intervals. Das nutzen der Adapter-Funktionen stellt sicher, dass alle Timeouts und Intervals beim Stop der Instanz korrekt abgebrochen werden.

Die Signaturen der Funktionen sind dabei identisch zum JavaScript-Standard.

.. code:: javascript

    this.setTimeout = (callback, timeout, ...args);
    this.clearTimeout(id);

    this.setInterval(callback, timeout, ...args);
    this.clearInterval(id);

Links
-----

- `adapter.js (js-controller 3.x) <https://github.com/ioBroker/ioBroker.js-controller/blob/3.3.x/lib/adapter.js>`_
- `adapter.js (js-controller 4.x) <https://github.com/ioBroker/ioBroker.js-controller/blob/4.0.x/packages/adapter/src/lib/adapter/adapter.js>`_
- `Adapter-Core <https://github.com/ioBroker/adapter-core>`_
- `Offizielle Doku <https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/adapterdev.md>`_
