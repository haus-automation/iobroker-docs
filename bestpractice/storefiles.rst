.. _bestpractice-storefiles:

File Storage
============

Bei der Adapter-Entwicklung steht man immer wieder vor der Aufgabe, Dateien im System ablegen zu müssen. Dafür gibt es mehrere Wege. Alle haben ihre Vor- und Nachteile.

Binary-State
------------

:octicon:`git-branch;1em;sd-text-info` Geänderte Signaturen seit ``js-controller`` 4.0.15 (setForeignBinaryState)

Ein Binary-State ist am Ende ein ganz normaler Zustand (State). Der einzige Unterschied ist, dass dieser Binärdaten speichern kann.

.. danger::
    Die Binärdaten werden in der normalen State-Datenbank abgelegt. Wird Redis verwendet, liegt die komplette Datei somit im Arbeitsspeicher und belegt ggf. knappe Ressourcen.

Um Binärdaten in einen Zustand zu speichern, muss dieser als ``common.type = 'file'`` definiert sein. Beispiel:

.. code:: javascript

    await this.setObjectNotExistsAsync('myThumbnail', {
        type: 'state',
        common: {
            name: {
                en: 'Thumbnail',
                de: 'Miniaturansicht',
                ru: 'Миниатюра',
                pt: 'Miniatura',
                nl: 'Miniatuur',
                fr: 'La vignette',
                it: 'Miniatura',
                es: 'Miniatura',
                pl: 'Miniaturka',
                'zh-cn': '缩略图',
            },
            type: 'file',
            role: 'state',
            read: true,
            write: false,
        },
        native: {},
    });

Danach kann mit der Funktion ``setForeignBinaryState`` ein Buffer gespeichert werden:

.. code:: javascript

    const uint8 = new Uint8Array([0x50, 0x89, 0x47, 0x4e]);

    this.setForeignBinaryState(`${this.namespace}.myThumbnail`, Buffer.from(data), () => {
        this.log.debug(`saved binary information in myThumbnail`);
    });

Meta-Storage
------------

TODO

Direkt schreiben
----------------

Möchte man Daten direkt ablegen, bieten die Adapter-Core-Utils ein paar hilfreiche Funktionen.

.. code:: javascript

    const utils = require('@iobroker/adapter-core');

    const dataDir = utils.getAbsoluteDefaultDataDir();
    // liefert (unter Linux) z.B. /opt/iobroker/iobroker-data/

    const instanceDir = utils.getAbsoluteInstanceDataDir(this);
    // liefert (unter Linux) z.B. /opt/iobroker/iobroker-data/<adapterName>.<instanceNr>

In diese Verzeichnisse kann man dann mit den normalen Funktion Dateien ablegen (z.B. ``fs``).

Möchte man, dass dieses Verzeichnis automatisch in das Backup mit aufgenommen wird, kann in der :ref:`development-iopackage` ein ``common.dataFolder`` konfiguriert werden. Beispielsweise

.. code:: json

    "dataFolder": "octoprint.%INSTANCE%"

Beispiel

.. code:: javascript

    const fs = require('fs');
    const path = require('path');
    const utils = require('@iobroker/adapter-core');

    const instanceDir = utils.getAbsoluteInstanceDataDir(this);

    if (!fs.existsSync(instanceDir)) {
        fs.mkdirSync(instanceDir);
    }

    const newFilePath = path.join(utils.getAbsoluteInstanceDataDir(this), 'newFile.txt');

    fs.writeFileSync(newFilePath, 'Just created a new test file');
