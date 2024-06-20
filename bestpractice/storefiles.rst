.. _bestpractice-storefiles:

File Storage
============

Bei der Adapter-Entwicklung steht man immer wieder vor der Aufgabe, Dateien im System ablegen zu müssen. Dafür gibt es mehrere Wege. Alle haben ihre Vor- und Nachteile.

Meta-Storage
------------

Die Adapter-Klasse stellt außerdem Funktionen bereit, welche es erlauben in einem "Meta-Storage" Daten abzulegen. Dieser befindet sich (unter Linux) im Verzeichnis ``/opt/iobroker/iobroker-data/files``.

.. danger::
    Es darf niemals direkt in diese Verzeichnisse geschrieben werden! Es dürfen ausschließlich die Adapter-Funktionen für den Zugriff genutzt werden! Ansonsten passen die Meta-Daten nicht mehr zu den Daten im Dateisystem und die ioBroker-Events funktionieren dann auch nicht!

Sollte ``redis`` verwendet werden, liegen die Dateien ebenfalls in der Redis-Datenbank und sind nicht im Dateisystem zu finden!

Diese Methode hat den Vorteil, dass ein Adapter oder Script (ähnlich wie bei :ref:`development-objects` und :ref:`development-states`) über eine Änderungen von Dateien benachricht werden kann.

Außerdem können diese Dateien dann über den ioBroker Admin verwaltet werden (Menupunkt "Dateien").

Damit diese Funktionen genutzt werden können, muss ein neues Objekt vom Typ ``meta.user`` erstellt (bzw. genutzt) werden. Ein solches Objekt wird im Standard angelegt unter ``0_userdata.0``.

In der Regel wird dafür der eigene Instanz-Namespace verwendet, damit die Verwendung im Admin eindeutiger ist:

.. code:: javascript

    await this.setForeignObjectNotExistsAsync(this.namespace, {
        type: 'meta',
        common: {
            name: {
                en: 'File storage',
                de: 'Dateispeicher',
                ru: 'Хранение файлов',
                pt: 'Armazenamento de arquivos',
                nl: 'Veldopslag',
                fr: 'Stockage de fichiers',
                it: 'Archiviazione file',
                es: 'Almacenamiento de archivos',
                pl: 'Storage room',
                uk: 'Зберігання файлів',
                'zh-cn': '储存'
            },
            type: 'meta.user'
        },
        native: {}
    });

Alternativ kann dieses Objekt auch über die ``instanceObjects`` in der :ref:`development-iopackage` erstellt werden (leere id):

.. code:: json

    "instanceObjects": [
        {
            "_id": "",
            "type": "meta",
            "common": {
                "name": {
                    "en": "File storage",
                    "de": "Dateispeicher",
                    "ru": "Хранение файлов",
                    "pt": "Armazenamento de arquivos",
                    "nl": "Veldopslag",
                    "fr": "Stockage de fichiers",
                    "it": "Archiviazione file",
                    "es": "Almacenamiento de archivos",
                    "pl": "Storage room",
                    "uk": "Зберігання файлів",
                    "zh-cn": "储存"
                },
                "type": "meta.user"
            },
            "native": {}
        }
    ]

Für den Zugriff stehen die folgenden Funktionen bereit:

.. code:: javascript

    // Prüfen, ob eine Datei existiert
    await this.fileExistsAsync(_adapter, filename, options);
    this.fileExists(_adapter, filename, options, callback);

    // Datei schreiben
    await this.writeFileAsync(_adapter, filename, data, options);
    this.writeFile(_adapter, filename, data, options, callback);

    // Datei umbenennen
    await this.renameAsync(_adapter, oldName, newName, options);
    this.rename(_adapter, oldName, newName, options, callback);

    await readFileAsync(_adapter, filename, options);
    this.readFile(_adapter, filename, options, callback);

    // Datei löschen
    await this.delFileAsync(_adapter, name, options);
    this.delFile(_adapter, name, options, callback);

    await this.unlinkAsync(_adapter, name, options);
    this.unlink(_adapter, name, options, callback);

    // Verzeichnis erstellen
    await this.mkdirAsync(_adapter, dirname, options);
    this.mkdir(_adapter, dirname, options, callback);

    // Verzeichnis lesen
    await this.readDirAsync(_adapter, path, options);
    this.readDir(_adapter, path, options, callback);

    // Besitzer ändern
    await this.chownFileAsync(_adapter, path, options);
    this.chownFile(_adapter, path, options, callback);

    // Rechte ändern
    await this.chmodFileAsync(_adapter, path, options);
    this.chmodFile(_adapter, path, options, callback);

**Beispiel:**

.. code:: javascript

    const fileExists = await this.fileExistsAsync(this.namespace, 'newFile.txt');
    if (!fileExists) {
        await this.writeFileAsync(this.namespace, 'newFile.txt', 'Just created a new test file');
    }

Lesen eines Verzeichnisses:

.. code:: javascript

    const files = await this.readDirAsync(this.namespace, '/');

    /*
    [
        {
            "file": "newFile.txt",
            "stats": {
                "dev": 64523,
                "mode": 33188,
                "nlink": 1,
                "uid": 1000,
                "gid": 1000,
                "rdev": 0,
                "blksize": 4096,
                "ino": 688120,
                "size": 84247,
                "blocks": 168,
                "atimeMs": 1703230920803.272,
                "mtimeMs": 1703230925247.2634,
                "ctimeMs": 1703230925247.2634,
                "birthtimeMs": 1703230920803.272,
                "atime": "2023-12-22T07:42:00.803Z",
                "mtime": "2023-12-22T07:42:05.247Z",
                "ctime": "2023-12-22T07:42:05.247Z",
                "birthtime": "2023-12-22T07:42:00.803Z"
            },
            "isDir": false,
            "acl": {
                "owner": "system.user.admin",
                "ownerGroup": "system.group.administrator",
                "permissions": 1604,
                "read": true,
                "write": true
            },
            "modifiedAt": 1703230925251
        }
    ]
    */

    for (const fileMeta of files) {
        if (!fileMeta.isDir) {
            await this.adapter.delFileAsync(this.adapter.namespace, fileMeta.file);
        }
    }

Der Vorteil dieser Lösung ist, dass man auch für Datei-Änderungen Events bekommt und entsprechend handeln kann. Das funktioniert dann genauso wie bei States oder Objekten.

**Beispiel:**

.. code:: javascript

    class MyAdapter extends utils.Adapter {
        constructor(options) {
            super({
                ...options,
                name: 'my-adapter',
            });

            this.on('ready', this.onReady.bind(this));
            this.on('fileChange', this.onFileChange.bind(this));
        }

        async onReady() {
            this.subscribeForeignFiles(this.namespace, '*');
        }

        onFileChange(id, fileName, size) {
            this.log.debug(`[onFileChange]: id: ${id}, fileName: ${fileName}, size: ${size}`);
        }

        // ...
    }

Binary-State (deprecated)
-------------------------

:octicon:`git-branch;1em;sd-text-info` Geänderte Signaturen seit ``js-controller`` 4.0.15 (setForeignBinaryState)

:octicon:`git-branch;1em;sd-text-info` Deprecated seit ``js-controller`` 4.0.23 - sollte nicht mehr verwendet werden

:octicon:`git-branch;1em;sd-text-info` Entfernt seit ``js-controller`` 6.0.0 - kann nicht mehr genutzt werden!

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
                uk: 'Напляскване',
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

    await this.setForeignBinaryStateAsync(`${this.namespace}.myThumbnail`, Buffer.from(data));

Direkt schreiben
----------------

Möchte man Daten direkt ablegen, bieten die Adapter-Core-Utils ein paar hilfreiche Funktionen.

.. code:: javascript

    const utils = require('@iobroker/adapter-core');

    const dataDir = utils.getAbsoluteDefaultDataDir();
    // liefert (unter Linux) z.B. /opt/iobroker/iobroker-data/

    const instanceDir = utils.getAbsoluteInstanceDataDir(this);
    // liefert (unter Linux) z.B. /opt/iobroker/iobroker-data/<adapterName>.<instanceNr>

In diese Verzeichnisse kann man dann mit den normalen Funktion Dateien ablegen (z.B. ``node:fs``).

Soll dieses Verzeichnis automatisch in das :ref:`basics-backup` mit aufgenommen werden, kann in der :ref:`development-iopackage` ein ``common.dataFolder`` konfiguriert werden. Beispielsweise

.. code:: json

    "dataFolder": "octoprint.%INSTANCE%"

**Beispiel:**

.. code:: javascript

    const fs = require('node:fs');
    const path = require('node:path');
    const utils = require('@iobroker/adapter-core');

    class Test extends utils.Adapter {
        constructor(options) {
            super({
                ...options,
                name: 'test'
            });

            this.on('ready', this.onReady.bind(this));
        }

        async onReady() {
            const instanceDir = utils.getAbsoluteInstanceDataDir(this);

            if (!fs.existsSync(instanceDir)) {
                fs.mkdirSync(instanceDir);
            }

            const newFilePath = path.join(utils.getAbsoluteInstanceDataDir(this), 'newFile.txt');

            fs.writeFileSync(newFilePath, 'Just created a new test file');
        }
    }
