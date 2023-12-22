.. _bestpractice-adapterdev:

Adapter-Entwicklung
===================

In diesem Dokument werden einige Code-Beispiele gesammelt, welche häufig bei der :ref:`development-adapter` benötigt werden.

.. danger::
    Diese Code-Beispiele sind NICHT für JavaScript-Code im JavaScript-Adapter gedacht! Dort werden teilweise andere Funktionen bzw. unterschiedliche Signaturen genutzt.

Eine Adapter-Klasse sollte immer von ``utils.Adapter`` erben. Die Basis-Klasse ist `hier <https://github.com/ioBroker/adapter-core>`_ zu finden.

.. code:: javascript

    const utils = require('@iobroker/adapter-core');

    class MyAdapter extends utils.Adapter {
        // ...
    }

Nützliche Eigenschaften
-----------------------

- ``this.host`` - 
- ``this.FORBIDDEN_CHARS`` - 
- ``this.namespace`` - Der komplette Namespace im Format. z.B. ``admin.0``
- ``this.instance`` -  Die Instanznummer als numerischer Wert. z.B. ``0``
- ``this.adapterDir`` - Der abolute Pfad zum Adapter-Verzeichnis (innerhalb node_modules)
- ``this.ioPack`` - Die ``io-package.json`` als Objekt

Die folgenden Eigenschaften werden nur bereitgestellt, wenn ``useFormatDate: true`` im Constructor übergeben wird.

- ``this.dateFormat``
- ``this.isFloatComma``
- ``this.language``
- ``this.longitude``
- ``this.latitude``



Objekte
-------

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
                uk: 'Ім\'я',
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
                uk: 'Ім\'я',
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

Zustände (States)
-----------------

Wie States genau aufgebaut sind und welche Eigenschaften sie unterstützten, ist auf der Seite :ref:`development-states` dokumentiert.

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

**Wert schreiben (binär)**

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

**Wert lesen (binär)**

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

Auf Änderungen reagieren
------------------------

Um Informationen nicht ständig aus der Datenbank abfragen zu müssen, können einzelne States, Objekte oder auch Dateien (Files) (siehe :ref:`bestpractice-storefiles`) abonniert werden. Der ``js-controller`` informiert uns dann über Änderungen.

**Zustände (States)**

1. Abonnieren mit ``subscribe[Foreign]States[Async]``
2. Event-Handler registrieren
3. Änderungen im Event-Handler auswerten

.. code:: javascript

    class MyAdapter extends utils.Adapter {
        /**
        * @param {Partial<utils.AdapterOptions>} [options={}]
        */
        constructor(options) {
            super({
                ...options,
                name: 'my-adapter',
            });

            this.on('ready', this.onReady.bind(this));
            this.on('stateChange', this.onStateChange.bind(this));
        }

        async onReady() {
            // Alle eigenen States abonnieren
            await this.subscribeStatesAsync('*');

            // Einen State aus einem anderen Namespace abonnieren
            await this.subscribeForeignStatesAsync('0_userdata.0.beispiel');
        }

        /**
         * @param {string} id
         * @param {ioBroker.State | null | undefined} state
         */
        onStateChange(id, state) {
            if (state && !state.ack) {

                // Verarbeitung bestätigen
                this.setForeignState(id, { val: state.val, ack: true });
            }
        }
    }

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    // subscribe[Foreign]States[Async]
    await this.subscribeStatesAsync(pattern, options);
    this.subscribeStates(pattern, options, callback);

    await this.subscribeForeignStatesAsync(pattern, options);
    this.subscribeForeignStates(pattern, options, callback);

**Objekte**

1. Abonnieren mit ``subscribe[Foreign]Objects[Async]``
2. Event-Handler registrieren
3. Änderungen im Event-Handler auswerten

.. code:: javascript

    class MyAdapter extends utils.Adapter {
        /**
        * @param {Partial<utils.AdapterOptions>} [options={}]
        */
        constructor(options) {
            super({
                ...options,
                name: 'my-adapter',
            });

            this.on('ready', this.onReady.bind(this));
            this.on('objectChange', this.onObjectChange.bind(this));
        }

        async onReady() {
            // Alle eigenen Objekte abonnieren
            await this.subscribeObjectsAsync('*');

            // Ein Objekt aus einem anderen Namespace abonnieren
            await this.subscribeForeignObjectsAsync('0_userdata.0.beispiel');
        }

        /**
         * @param {string} id
         * @param {ioBroker.Object | null | undefined} obj
         */
        onObjectChange(id, obj) {
            if (obj) {
                // Objekt geändert
            } else {
                // Objekt gelöscht
            }
        }
    }

Alle Funktionen gibt es asynchron und mit callback. Jeweils für States im eigenen Namespace und fremde States.

.. code:: javascript

    // subscribe[Foreign]Objects[Async]
    await this.subscribeObjectsAsync(pattern, options);
    this.subscribeObjects(pattern, options, callback);

    await this.subscribeForeignObjectsAsync(pattern, options);
    this.subscribeForeignObjects(pattern, options, callback);

**Dateien (Files)**

.. todo::
    Test code!

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 4.1.0

1. Abonnieren mit ``subscribeFiles[Async]``
2. Event-Handler registrieren
3. Änderungen im Event-Handler auswerten

.. code:: javascript

    class MyAdapter extends utils.Adapter {
        /**
        * @param {Partial<utils.AdapterOptions>} [options={}]
        */
        constructor(options) {
            super({
                ...options,
                name: 'my-adapter',
            });

            this.on('ready', this.onReady.bind(this));
            this.on('fileChange', this.onFileChange.bind(this));
        }

        async onReady() {
            await this.subscribeFiles(this.namespace);
        }

        /**
         * @param {string} id
         * @param {string} fileName
         * @param {number} size
         */
        onFileChange(id, fileName, size) {
            
        }
    }

Daten formatieren
-----------------

Um Daten für den Endanwender lesbar zu formatieren, gibt es im Adapter verschiedene Funktionen, welche diese Aufgabe übernehmen:

- `formatDate`
- `formatValue`

Beide Funktionen beziehen sich im Standard auf die System-Einstellungen (es können ggf. eigene Formatierungs-Optionen übergeben werden).

Bei `formatValue` wird dieses Format als String (mit 2 Zeichen Länge!) übergeben. Dabei ist das erste Zeichen im String das 1000er-Trennzeichen und das zweite definiert das Komma. Eselsbrücke: Die Reihenfolge ist genauso, wie später im Ergebnis.

Beispiele:

- `this.formatValue(2.43425, 3);` - Rückgabe: 2,434
- `this.formatValue(2.53425, 0);` - Rückgabe: 3 (wird gerundet!)
- `this.formatValue(1000.123, 2, '.,');` - Rückgabe: 1.000,12
- `this.formatValue(1000.123, 2, ',.');` - Rückgabe: 1,000.12 (US-Schreibweise)

Timeout / Interval
------------------

Die Basis-Adapter-Implementierung erlaubt das verwalten von Timeouts und Intervallen. Das nutzen dieser Adapter-Funktionen stellt sicher, dass **alle Timeouts und Intervalle beim Stop der Instanz korrekt abgebrochen/beendet werden**.


Die Signaturen der Funktionen sind dabei identisch zum JavaScript-Standard.

.. code:: javascript

    this.setTimeout = (callback, timeout, ...args);
    this.clearTimeout(id);

    this.setInterval(callback, timeout, ...args);
    this.clearInterval(id);

Lizenzen
--------

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 4.0.15

Alle gültigen :ref:`ecosystem-licenses` für einen Benutzer werden im Objekt ``system.licenses`` abgelegt (intern Lizenz-Manager genannt). Gültige Lizenzen für den aktuellen Adapter können wie folgt abgefragt werden:

.. code:: javascript

    await this.getSuitableLicenses(all?: boolean, adapterName?: string);

Dabei wird der folgende Public Key des js-controller verwendet, um die Lizenz zu verifizieren (der Private Key liegt wahrscheinlich auf irgend einem Cloud-Server um die Lizenzen zu erstellen):

.. code:: console

    /opt/iobroker/node_modules/@iobroker/js-controller-adapter/build/cert/cloudCert.crt

Die Lizenzen werden als `JWT (JSON Web Token) <https://de.wikipedia.org/wiki/JSON_Web_Token>`_ abgelegt.

Links
-----

- `adapter.js (js-controller 3.x) <https://github.com/ioBroker/ioBroker.js-controller/blob/3.3.x/lib/adapter.js>`_
- `adapter.js (js-controller 4.x) <https://github.com/ioBroker/ioBroker.js-controller/blob/4.0.x/packages/adapter/src/lib/adapter/adapter.js>`_
- `adapter.ts (js-controller 5.x) <https://github.com/ioBroker/ioBroker.js-controller/blob/master/packages/adapter/src/lib/adapter/adapter.ts>`_
- `Adapter-Core <https://github.com/ioBroker/adapter-core>`_
- `Offizielle Doku <https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/adapterdev.md>`_
