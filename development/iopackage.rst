.. _development-iopackage:

io-package.json
===============

Jeder Adapter enthält neben der ``package.json`` für npm noch eine ``io-package.json``. Hier werden sämtliche Meta-Informationen für den Adpater hinterlegt.

Beispiel
--------

Hier eine Beispiel-Datei aus dem Luftdaten-Adapter. Eine Beschreibung der einzelnen Eigenschaften folgt weiter unten.

.. code:: json

    {
        "common": {
            "name": "luftdaten",
            "version": "2.0.3",
            "news": {
                "2.0.3": {
                    "en": "Fixed error logging",
                    "de": "Fehler-Logging behoben"
                },
                "2.0.2": {
                    "en": "Added timeout option",
                    "de": "Option für Timeout-Limit hinzugefügt"
                },
                "2.0.1": {
                    "en": "Minor bug fixes",
                    "de": "Kleinere Bugfixes"
                },
                "2.0.0": {
                    "en": "Updated admin interface to maintain multiple sensors in one instance",
                    "de": "Benutzeroberfläche angepasst, um mehrere Sensoren in einer Instanz verwalten zu können"
                }
            },
            "title": "Luftdaten.info",
            "titleLang": {
                "en": "Luftdaten.info",
                "de": "Luftdaten.info"
            },
            "desc": {
                "en": "Loads current air quality data from a local or remote sensor",
                "de": "Lädt aktuelle Luftqualitätsdaten eines lokalen oder Cloud-Sensors"
            },
            "authors": [
                "Matthias Kleine <info@haus-automatisierung.com>"
            ],
            "keywords": [
                "web",
                "weather",
                "air",
                "quality"
            ],
            "license": "MIT",
            "platform": "Javascript/Node.js",
            "main": "main.js",
            "icon": "luftdaten.png",
            "extIcon": "https://raw.githubusercontent.com/klein0r/ioBroker.luftdaten/master/admin/luftdaten.png",
            "enabled": true,
            "readme": "https://github.com/klein0r/ioBroker.luftdaten/blob/master/README.md",
            "loglevel": "info",
            "mode": "schedule",
            "allowInit": true,
            "schedule": "*/30 * * * *",
            "type": "weather",
            "compact": true,
            "connectionType": "cloud",
            "dataSource": "poll",
            "materialize": true,
            "dependencies": [
                {
                    "js-controller": ">=3.3.0"
                }
            ],
            "globalDependencies": [
                {
                    "admin": ">=5.1.19"
                }
            ],
            "plugins": {
                "sentry": {
                    "dsn": "https://baf35e4e423d409bbec94cb01b55257e@sentry.iobroker.net/103"
                }
            }
        },
        "native": {
            "requestTimeout": 10
        },
        "objects": [
        ]
    }

Eigenschaften (erforderlich)
----------------------------

- ``common.name`` (string) - Name des Adapters (darf nicht "ioBroker" enthalten)
- ``common.version`` (string) - Aktuelle Version des Adapters
- ``common.platform`` (string) - Aktuell immer ``Javascript/Node.js``
- ``common.titleLang`` (object) - Titel des Adapters (übersetzt in mehrere Sprachen) ``{en: 'Adapter', de: 'adapter', ru: 'Драйвер'}``
- ``common.desc`` (object) - Beschreibung, was der Adapter machen soll (übersetzt in mehrere Sprachen)
- ``common.news`` (object) - Liste mit Infos zu den verschiedenen Versionen (Updatehistorie / Changelog) (siehe Beispiel oben)
- ``common.mode`` (string) - Modus des Adapters: ``schedule``, ``daemon``, ``subscribe``, ``schedule``, ``once``, ``extension`` (siehe unten für weitere Optionen)

Eigenschaften (optional)
------------------------

**Allgemein**

- ``common.enabled`` (boolean) - Legt fest, ob die Instanz gestartet werden soll, oder nicht. Standard: ``true``
- ``common.tier`` (number) - Legt fest, in welcher Reihenfolge die Adapter gestartet werden. 1 = Logik, 2 = API und andere Daten, 3 = alle anderen
- ``common.messagebox`` (boolean) - ``true`` wenn Nachrichten an den Adapter erlaubt sind. Siehe :ref:`development-messagebox`
- ``common.readme`` (string) - URL zur Readme-Datei (z.B. HTTP-URL zur README.md auf GitHub)
- ``common.adminUI`` (object) - 
- ``common.docs`` (object) - 
- ``common.authors`` (array) - Liste mit Entwicklern des Adapters (siehe Beispiel oben)
- ``common.license`` (string) - Lizenz des Adapters (z.B. MIT). Gültige Werte sind im Schema zu finden (Link siehe unten)
- ``common.type`` (string) - Typ/Kategorie des Adapters (z.B. ``weather``). Gültige Werte sind im Schema zu finden (Link siehe unten)
- ``common.unsafePerm`` (boolean) - Legt fest, ob das Adapter-Paket mit dem ``--unsafe-perm`` Parameter für npm installiert werden muss. Siehe `npm Dokumentation <https://docs.npmjs.com/cli/v6/using-npm/config#unsafe-perm>`_
- ``common.plugins`` (object) - Liste von Plugins (z.B. :ref:`ecosystem-sentry`)
- ``common.pugins.sentry`` (object) - Konfiguration des Sentry-Plugins. Siehe :ref:`ecosystem-sentry`
- ``common.availableModes`` (array) - Werte für ``common.mode`` (falls mehr als ein Wert erlaubt ist)
- ``common.blockly`` (boolean) - Legt fest, ob der Adapter eigene Blockly-Bausteine mitbringt (``admin/blockly.js`` erforderlich)
- ``common.connectionType`` (string) - Definiert die Qulle der Adapter-Daten (``local`` oder ``cloud``). Wird im Admin ab Version 5 dargestellt
- ``common.compact`` (boolean) - Legt fest, ob der Adapter im :ref:`development-messagebox` gestartet werden kann
- ``common.dataFolder`` (string) - Verzeichnis-Pfad, in welchem der Adapter seine Daten ablegt (relativ zu ``iobroker-data``). Die Variable ``%INSTANCE%`` kann ebenfalls im Pfad genutzt werden.
- ``common.dataSource`` (string) - Legt fest, wie Daten geholt werden sollen. Mögliche Werte: ``poll``, ``push``, ``assumption``
- ``common.dependencies`` (array) - Liste von Abhängigkeiten auf dem lokalen System, welche für diesen Adapter notwendig sind. Zum Beispiel ``{"js-controller": ">=3.3.0"}``
- ``common.eraseOnUpload`` (boolean) - Löscht alle existierenden Daten im Adapter-Verzeichnis vor einem Upload
- ``common.extIcon`` (string) - URL zur Icon-Datei für die Admin-Übersicht (z.B. PNG-Datei auf GitHub)
- ``common.getHistory`` (boolean) - Legt fest, ob der Adapter den ``getHistory`` Befehl unterstützt
- ``common.globalDependencies`` (array) - Liste von Abhängigkeiten im gesamten ioBroker-System (Multihost-Betrieb). Zum Beispiel ``{"admin": ">=5.1.19"}``. Siehe :ref:`basics-multihost`
- ``common.icon`` (string) - Pfad zum lokalen Icon des Adapters (nach Installation). Sollte im Unterverzeichnis ``admin`` liegen
- ``common.keywords`` (array) - Liste von Schlüsselwörter, um den Adapter besser finden zu können
- ``common.localLinks`` (object) - 
- ``common.loglevel`` - Standard Log-Level neuer Instanzen. Möglich Werte: ``debug``, ``info``, ``warn`` oder ``error``
- ``common.logTransporter`` (boolean) - Legt fest, ob der Adapter die Log-Einträge von anderen Adaptern entgegen nehmen kann (um sie z.B. wo anders zu speichern)
- ``common.noIntro`` (boolean) - 
- ``common.noRepository`` (boolean) - 
- ``common.nogit`` (boolean) - Legt fest, ob eine Installation direkt von GitHub verboten werden soll
- ``common.nondeletable`` (boolean) - Legt fest, ob ein Adapter gelöscht oder aktualisiert werden kann. Falls ``true``, kümmert sich der ``js-controller`` um diese Aufgaben
- ``common.onlyWWW`` (boolean) - Legt fest, ob der Adapter nur weitere HTML-Dateien bereitstellt und keine Logik enthält (wie zum Beispiel Widget-Adapter für ``VIS``)
- ``common.osDependencies.darwin`` (array) - Liste mit erforderlichen MacOS-Paketen für diesen Adapter
- ``common.osDependencies.linux`` (array) - Liste mit erforderlichen Linux-Paketen für diesen Adapter
- ``common.osDependencies.win32`` (array) - *Aktuell nicht genutzt, da Linux keinen Paket-Manager hat*
- ``common.os`` (string / array) - Liste mit unterstützten Betriebssystemen. Mögliche Werte: ``darwin``, ``linux`` oder ``win32``
- ``common.preserveSettings`` (string / array) - Liste mit Attributen, welche nicht automatisch gelöscht werden sollen (z.B. ``history``)
- ``common.restartAdapters`` (array) - Liste mit Adaptern, welche neugestartet werden sollen, nachdem dieser Adapter installiert wurde (z.B. ``["vis"]``)
- ``common.serviceStates`` (string / boolean) - 
- ``common.singletonHost`` (boolean) - Legt fest, ob es nur eine einzelne Instanz pro Host geben darf
- ``common.singleton`` (boolean) - Legt fest, ob es nur eine einzelne Instanz im gesamten ioBroker-System geben darf (Multihost-Betrieb). Siehe :ref:`basics-multihost`
- ``common.stopBeforeUpdate`` (boolean) - Legt fest, ob die Instanzen vor einem Update gestoppt werden müssen
- ``common.stopTimeout`` (number) - Wartezeit in Millisekunden, bis der Adapter angehalten wird (Standardwert: 500 ms)
- ``common.subscribable`` (boolean) - 
- ``common.subscribe`` (string) - 
- ``common.supportCustoms`` (boolean) - Legt fest, ob es zusätzliche Einstellungen für jeden Datenpunkt gibt (``admin/custom.html`` erforderlich)
- ``common.supportStopInstance`` (boolean) - Legt fest, ob der Adapter das ``stopInstance`` Signal unterstützt.  Siehe :ref:`development-messagebox`
- ``common.wakeup`` (boolean) - Legt fest, ob die Instanz gestartet werden soll, wenn ein Wert in ``system.adapter.<adapter-name>.<instance-nummmer>.wakeup`` geschrieben wird.
- ``common.webByVersion`` (boolean) - 
- ``common.webExtendable`` (boolean) - Legt fest, ob der Webserver dieses Adapters mit Plugins erweitert werden kann (z.B. ``simple-api``)
- ``common.webExtension`` (string) - Relativer Pfad der Web-Extension (z.B. ``lib/simpleapi.js``)
- ``common.webPreSettings`` (object) - 
- ``common.webservers`` (array) - Liste mit Webservern, welche Inhalte aus dem www-Verzeichnis des Adapters liefern
- ``common.welcomeScreen`` (array) - 
- ``common.welcomeScreenPro`` (array) - Identisch zu ``common.welcomeScreen``, allerdings für Zugriff über die ioBroker-Cloud

**Mode: Schedule**

- ``common.schedule`` (string) - CRON-Definition, wann die Instanzen gestartet werden sollen (kann vom Benutzer angepasst werden)
- ``common.allowInit`` (boolean) - Legt fest, ob ein Adapter auch außerhalb des definierten Zeitplanes gestartet wird (z.B. nach Änderung der Instanz-Konfiguration)

**Mode: Daemon**

- ``common.restartSchedule`` (string) - CRON-Definition, wann die laufenden Instanzen neugestartet werden sollen (kann vom Benutzer angepasst werden)

**Admin**

- ``common.adminColumns`` (array) - Custom attributes, that must be shown in the admin in the object browser. Like: [{"name": {"en": "KNX address"}, "path": "native.address", "width": 100, "align": "left"}, {"name": "DPT", "path": "native.dpt", "width": 100, "align": "right", "type": "number", "edit": true, "objTypes": ["state", "channel"]}]. type is a type of the attribute (e.g. string, number, boolean) and only needed if edit is enabled. objTypes is a list of the object types, that could have such attribute. Used only in edit mode too.
- ``common.adminTab.fa-icon`` (string) - `Font-Awesome <https://fontawesome.com/icons>`_ Icon für das Tab
- ``common.adminTab.ignoreConfigUpdate`` (boolean) - 
- ``common.adminTab.link`` (string) - 
- ``common.adminTab.name`` (object) - Titel des Tabs (übersetzt in mehrere Sprachen)
- ``common.adminTab.singleton`` (boolean) - Legt fest, ob nur ein Tab für alle Instanzen angezeigt werden soll
- ``common.jsonConfig`` (boolean) - JSON-Konfiguration für den Admin 5 vorhanden (``admin/jsonConfig.json`` erforderlich)
- ``common.jsonCustom`` (boolean) - JSON-Konfiguration für den Admin 5 vorhanden (``admin/jsonCustom.json`` erforderlich)

**Weitere Optionen**

- ``objects`` (array) - Liste von Objekten, welche für den Adapter erstellt werden sollen
- ``instanceObjects`` (array) - Liste von Objekten, welche für jede Instanz automatisch erstellt werden
- ``protectedNative`` (array) - Liste von Attributen, welche nur vom Adapter selbst lesbar sind (z.B. ``["password"]``). Siehe :ref:`development-encryption`
- ``encryptedNative`` (array) - Liste vo automatisch verschlüsselten Attributen. Siehe :ref:`development-encryption`
- ``native`` (object) - Liste von vordefinierten Attributen, welche z.B. in der Admin-Konfiguration überschrieben werden können
- ``notifications`` (array) - Liste von Objekten zur Konfiguration des internen. Siehe :ref:`development-notifications`

Eigenschaften (deprecated)
--------------------------

Diese Eigenschaften sind für aktuelle Adapter mit dem Admin 5 nicht mehr relevant

- ``common.title`` - Langer Name des Adapters für Admin-Version 2, 3 und 4
- ``common.npmLibs`` - Ersetzt durch Abhängigkeiten in der ``package.json``
- ``common.main`` - Ersetzt durch ``main`` in der ``package.json``
- ``common.localLink`` - Ersetzt durch ``common.localLinks``
- ``common.engineTypes`` - Ersetzt durch ``engine`` in der ``package.json``
- ``common.config.height`` - Standard-Höhe für den Konfigurations-Dialog für Admin 2
- ``common.config.minHeight`` - Mindest-Höhe für den Konfigurations-Dialog für Admin 2
- ``common.config.width`` - Standard-Breite für den Konfigurations-Dialog für Admin 2
- ``common.config.minWidth`` - Mindest-Breite für den Konfigurations-Dialog für Admin 2
- ``common.materialize`` (boolean) - Legt fest, ob der Adapter die Admin-Oberfläche für Admin-Version 3 und 4 bereitstellt
- ``common.materializeTab`` (boolean) - Legt fest, ob der Adapter ein eigenes Tab für Admin-Version 3 und 4 bereitstellt
- ``common.noConfig`` (boolean) - Definiert, ob Instanzen konfiguriert werden können (ab Admin 5 sollte ``adminUI.config = none`` verwendet werden)

Hilfreiche Tools / Links
------------------------

- `Schema-Datei <https://github.com/ioBroker/ioBroker.js-controller/blob/master/packages/controller/schemas/io-package.json>`_

Siehe auch: https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/objectsschema.md
