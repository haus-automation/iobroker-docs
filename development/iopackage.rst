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
            "version": "2.1.1",
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
            "adminUI": {
                "config": "json"
            },
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

.. confval:: common.name

    Name des Adapters (darf nicht "ioBroker" enthalten)

    :type: string

.. confval:: common.version

    Aktuelle Version des Adapters (muss mit der Version der `package.json` übereinstimmen)

    :type: string

.. confval:: common.platform

    Die Plattform, auf welcher der Adapter programmiert wurde

    :type: string
    :default: ``Javascript/Node.js``

.. confval:: common.titleLang

    Titel des Adapters (übersetzt in mehrere Sprachen)

    .. code:: json

        "titleLang": {
            "en": "Luftdaten.info",
            "de": "Luftdaten.info"
        }

    :type: object

.. confval:: common.desc

    Beschreibung, was der Adapter machen soll (übersetzt in mehrere Sprachen)

    .. code:: json

        "desc": {
            "en": "Loads current air quality data from a local or remote sensor",
            "de": "Lädt aktuelle Luftqualitätsdaten eines lokalen oder Cloud-Sensors"
        }

    :type: object

.. confval:: common.news

    Liste mit Infos zu den verschiedenen Versionen (Updatehistorie / Changelog)

    .. code:: json

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
        }

    :type: object

.. confval:: common.mode

    Modus des Adapters

    - ``none`` - Der Adapter wird nicht gestartet
    - ``daemon`` - Separat laufender Prozess
    - ``subscribe`` - Wird gestartet, wenn der State ``system.adapter.<adapter-name>.<instanz-nummmer>.alive`` auf ``true`` gesetzt wird. Wird automatisch beendet, wenn der State auf ``false`` geändert wird. Der State wird automatisch auf ``false`` gesetzt, wenn der Prozess beendet wurde.
    - ``schedule`` - Wird nach dem in ``common.schedule`` festgelegten Zeitplan automatisch gestartet
    - ``once`` - Wird jedes Mal automatisch gestartet, wenn das ``system.adater.<adapter-name>.<instanz-nummmer>``-Objekt geändert wird
    - ``extension`` - ???

    :type: string

Eigenschaften (Allgemein)
------------------------

.. confval:: common.enabled

    Legt fest, ob die Instanz gestartet werden soll, oder nicht

    :type: boolean
    :default: ``true``

.. confval:: common.tier

    Legt fest, in welcher Reihenfolge die Adapter gestartet werden

    - ``1`` - Logik
    - ``2`` - API und andere Daten
    - ``3`` - alle anderen

    :type: number
    :default: ``3``

.. confval:: common.messagebox

    ``true`` wenn Nachrichten per ``sendTo()`` an den Adapter erlaubt sind. Siehe :ref:`development-messagebox`

    :type: boolean
    :default: ``false``

.. confval:: common.readme

    URL zur Readme-Datei (z.B. HTTP-URL zur README.md auf GitHub)

    :type: string

.. confval:: common.docs



    :type: object

.. confval:: common.authors

    Liste mit Entwicklern des Adapters

    .. code:: json

        "authors": [
            "Matthias Kleine <info@haus-automatisierung.com>"
        ]

    :type: array

.. confval:: common.license

    Lizenz des Adapters (z.B. MIT). Gültige Werte sind im Schema zu finden (Link siehe unten)

    :type: string

.. confval:: common.type

    Typ/Kategorie des Adapters

    - ``alarm`` - Sicherheitssysteme, Alarmanlagen, ...
    - ``climate-control`` - Klimasteuerung, Heizung, Luftfilter, ...
    - ``communication`` - Kommunikation mit anderen Adaptern (REST Api)
    - ``date-and-time`` - Kalender, Ferien, Feiertage, ...
    - ``energy`` - PV-Anlage, Verbrauchsdaten, ...
    - ``metering`` - Energiemessung
    - ``garden`` - Rasenmähroboter, Bewässerung, ...
    - ``general`` - Allgemeine Adapter wie Admin
    - ``geoposition`` - Position von Objekten oder Personen
    - ``hardware`` - Allgemeine Hardware-Schnittstellen (z.B. für ESP8266, ESP32)
    - ``health`` - Gesundheitsdaten wie Blutdruck, Blutzucker, ...
    - ``household`` - Küchengeräte, Haushaltsgeräte, Staubsaugerroboter, ...
    - ``infrastructure`` - Netzwerktechnik, Drucker, Scanner, Telefone, ...
    - ``iot-systems`` - Weitere IoT-Geräte, welche nicht in die anderen Kategorien passen
    - ``lighting`` - Beleuchtung
    - ``logic`` - Logikmodule für eigene Regeln oder Szenen
    - ``messaging`` - Nachrichtendienste wie Telegram oder E-Mail
    - ``misc-data`` - Export und Import von Daten
    - ``multimedia`` - Fernseher, Receiver, Beamer, ...
    - ``network`` - Ping, ...
    - ``protocols`` - Generische Protokolle (wie MQTT)
    - ``storage`` - Daten-Speicherung wie mySQL oder InfluxDB
    - ``utility`` - Weitere Tools wie Backup-Adapter
    - ``visualization`` - Visualisierungs-Adapter
    - ``visualization-icons`` - Zusätzliche Icons für die Visualisierung
    - ``visualization-widgets`` - Weitere Widgets für die Visualisierung
    - ``weather`` - Wetterdaten

    :type: string

.. confval:: common.unsafePerm

    Legt fest, ob das Adapter-Paket mit dem ``--unsafe-perm`` Parameter für npm installiert werden muss. Siehe `npm Dokumentation <https://docs.npmjs.com/cli/v6/using-npm/config#unsafe-perm>`_

    :type: boolean

.. confval:: common.plugins

    Liste von Plugins (z.B. :ref:`ecosystem-sentry`)

    :type: object

.. confval:: common.plugins.sentry

    Konfiguration des Sentry-Plugins. Siehe :ref:`ecosystem-sentry`

    .. code:: json

        "plugins": {
            "sentry": {
                "dsn": "https://xxx@sentry.iobroker.net/xxx"
            }
        }

    :type: object

.. confval:: common.availableModes

    Werte für ``common.mode`` (falls mehr als ein Wert erlaubt ist)

    :type: array

.. confval:: common.blockly

    Legt fest, ob der Adapter eigene Blockly-Bausteine mitbringt (``admin/blockly.js`` erforderlich)

    :type: boolean
    :default: ``false``

.. confval:: common.connectionType

    Definiert die Qulle der Adapter-Daten. Wird im Admin ab Version 5 dargestellt und dient als Information für den Nutzer

    - ``local`` - Die Kommunikation findet lokal / im eigenen Netzwerk statt (z.B. mit dem Gerät direkt per HTTP)
    - ``cloud`` - Für den Adapter ist eine aktive Internetverbindung erforderlich. Die Daten werden z.B. vom Server des Herstellers abgerufen.

    :type: string

.. confval:: common.dataSource

    Legt fest, wie Daten geholt werden

    - ``poll`` - Die Daten werden regelmäßig abgefragt (z.B. per Zeitplan)
    - ``push`` - Das Gerät liefert die Daten selbstständig zum Adapter
    - ``assumption`` - Der genaue Status ist nicht definiert

    :type: string

.. confval:: common.compact

    Legt fest, ob der Adapter im :ref:`basics-compactmode` gestartet werden kann

    :type: boolean
    :default: ``false``

.. confval:: common.dataFolder

    Verzeichnis-Pfad, in welchem der Adapter seine Daten ablegt (relativ zu ``iobroker-data``). Die Variable ``%INSTANCE%`` kann ebenfalls im Pfad genutzt werden

    :type: string

.. confval:: common.dependencies

    Liste von Abhängigkeiten auf dem lokalen System, welche für diesen Adapter notwendig sind

    .. code:: json

        "dependencies": [
            {
                "js-controller": ">=3.3.0"
            }
        ]

    :type: array

.. confval:: common.eraseOnUpload

    Löscht alle existierenden Daten im Adapter-Verzeichnis vor einem Upload

    :type: boolean

.. confval:: common.extIcon

    URL zur Icon-Datei für die Admin-Übersicht (z.B. PNG-Datei auf GitHub)

    :type: string

.. confval:: common.getHistory

    Legt fest, ob der Adapter den ``getHistory`` Befehl unterstützt

    :type: boolean

.. confval:: common.globalDependencies

    Liste von Abhängigkeiten im gesamten ioBroker-System (Multihost-Betrieb). Siehe :ref:`basics-multihost`

    .. code:: json

        "globalDependencies": [
            {
                "admin": ">=5.1.19"
            }
        ]

    :type: array

.. confval:: common.icon

    Pfad zum lokalen Icon des Adapters (nach Installation). Sollte im Unterverzeichnis ``admin`` liegen

    :type: string

.. confval:: common.keywords

    Liste von Schlüsselwörtern, um den Adapter über die Suche (besser) finden zu können

    .. code:: json

        "keywords": [
            "web",
            "weather",
            "air",
            "quality"
        ]

    :type: array

.. confval:: common.localLinks



    :type: object

.. confval:: common.loglevel

    Standard Log-Level neuer Instanzen. Empfohlen: ``info``

    - ``silly`` - Alles
    - ``debug`` - Debug-Nachrichten
    - ``info`` - Informationen
    - ``warn`` - Warnungen
    - ``error`` - Fehler

    :type: string

.. confval:: common.logTransporter

    Legt fest, ob der Adapter die Log-Einträge von anderen Adaptern entgegen nehmen kann (um sie z.B. wo anders zu speichern)

    :type: boolean

.. confval:: common.noIntro



    :type: boolean

.. confval:: common.noRepository



    :type: boolean

.. confval:: common.nogit

    Legt fest, ob eine Installation direkt von GitHub verboten werden soll

    :type: boolean

.. confval:: common.nondeletable

    Legt fest, ob ein Adapter gelöscht oder aktualisiert werden kann. Falls ``true``, kümmert sich der ``js-controller`` um diese Aufgaben

    :type: boolean
    :default: ``false``

.. confval:: common.onlyWWW

    Legt fest, ob der Adapter nur weitere HTML-Dateien bereitstellt und keine Logik enthält (wie zum Beispiel Widget-Adapter für ``VIS``)

    :type: boolean

.. confval:: common.osDependencies.darwin

    Liste mit erforderlichen MacOS-Paketen für diesen Adapter

    :type: array

.. confval:: common.osDependencies.linux

    Liste mit erforderlichen Linux-Paketen für diesen Adapter

    :type: array

.. confval:: common.osDependencies.win32

    *Aktuell nicht genutzt, da Linux keinen Paket-Manager hat*

    :type: array

.. confval:: common.os

    Liste mit unterstützten Betriebssystemen
    
    - ``darwin`` - Mac OS X
    - ``linux`` - Linux
    - ``win32`` - Windows

     :type: string / array

.. confval:: common.preserveSettings

     Liste mit Attributen, welche nicht automatisch gelöscht werden sollen (z.B. ``history``)

     :type: string / array

.. confval:: common.restartAdapters

     Liste mit Adaptern, welche neugestartet werden sollen, nachdem dieser Adapter installiert wurde (z.B. ``["vis"]``)

     :type: array

.. confval:: common.serviceStates

     

     :type: string / boolean

.. confval:: common.singletonHost

     Legt fest, ob es nur eine einzelne Instanz pro Host geben darf

     :type: boolean
     :default: ``false``

.. confval:: common.singleton

     Legt fest, ob es nur eine einzelne Instanz im gesamten ioBroker-System geben darf (Multihost-Betrieb). Siehe :ref:`basics-multihost`

     :type: boolean
     :default: ``false``

.. confval:: common.stopBeforeUpdate

     Legt fest, ob die Instanzen vor einem Update gestoppt werden müssen

     :type: boolean

.. confval:: common.stopTimeout

     Wartezeit in Millisekunden, bis der Adapter angehalten wird

     :type: number
     :default: ``500``

.. confval:: common.subscribable

     ???

     :type: boolean

.. confval:: common.subscribe

     ???

     :type: string

.. confval:: common.supportCustoms

     Legt fest, ob es zusätzliche Einstellungen für jeden Datenpunkt gibt (``admin/custom.html`` erforderlich)

     :type: boolean

.. confval:: common.supportStopInstance

     Legt fest, ob der Adapter das ``stopInstance`` Signal unterstützt.  Siehe :ref:`development-messagebox`

     :type:  boolean

.. confval:: common.wakeup

     Legt fest, ob die Instanz gestartet werden soll, wenn ein Wert in ``system.adapter.<adapter-name>.<instanz-nummmer>.wakeup`` geschrieben wird.

     :type: boolean

.. confval:: common.webByVersion



     :type: boolean

.. confval:: common.webExtendable

     Legt fest, ob der Webserver dieses Adapters mit Plugins erweitert werden kann (z.B. ``simple-api``)

     :type: boolean

.. confval:: common.webExtension

     Relativer Pfad der Web-Extension (z.B. ``lib/simpleapi.js``)

     :type: string

.. confval:: common.webPreSettings

     

     :type: object

.. confval:: common.webservers

     Liste mit Webservern, welche Inhalte aus dem www-Verzeichnis des Adapters liefern

     :type: array

.. confval:: common.welcomeScreen

     

     :type: array

.. confval:: common.welcomeScreenPro

     Identisch zu ``common.welcomeScreen``, allerdings für Zugriff über die ioBroker-Cloud

     :type: array

Eigenschaften (Schedule)
------------------------

.. confval:: common.schedule

    CRON-Definition, wann die Instanzen gestartet werden sollen (kann vom Benutzer angepasst werden)

    :type: string

.. confval:: common.allowInit

    Legt fest, ob ein Adapter auch außerhalb des definierten Zeitplanes gestartet wird (z.B. nach Änderung der Instanz-Konfiguration)

    :type: boolean

Eigenschaften (Daemon)
----------------------

.. confval:: common.restartSchedule

    CRON-Definition, wann die laufenden Instanzen neugestartet werden sollen (kann vom Benutzer angepasst werden)

    :type: string

Eigenschaften (Admin)
---------------------

.. confval:: common.adminColumns

    Custom attributes, that must be shown in the admin in the object browser. Like: Type is a type of the attribute (e.g. string, number, boolean) and only needed if edit is enabled. objTypes is a list of the object types, that could have such attribute. Used only in edit mode too

    .. code:: json

        [
            {
                "name": {
                    "en": "KNX address"
                },
                "path": "native.address",
                "width": 100,
                "align": "left"
            },
            {
                "name": "DPT",
                "path": "native.dpt",
                "width": 100,
                "align": "right",
                "type": "number",
                "edit": true,
                "objTypes": [
                    "state",
                    "channel"
                ]
            }
        ]

    :type: array

.. confval:: common.adminTab.fa-icon

    `Font-Awesome <https://fontawesome.com/icons>`_ Icon für das Tab

    :type: string

.. confval:: common.adminTab.ignoreConfigUpdate

    

    :type: boolean

.. confval:: common.adminTab.link

    

    :type: string

.. confval:: common.adminTab.name

    Titel des Tabs (übersetzt in mehrere Sprachen)

    :type: object

.. confval:: common.adminTab.singleton

    Legt fest, ob nur ein Tab für alle Instanzen angezeigt werden soll

    :type: boolean

.. confval:: common.adminUI

    Legt fest, wie die Konfiguration im Admin erfolgen soll

    :type: object

.. confval:: common.adminUI.config

    Legt fest, wie die Konfiguration für die Admin-Oberfläche aufgebaut ist

    - ``none``
    - ``materialize`` (``admin/index_m.html`` erforderlich - ab Admin Version 4)
    - ``json`` (``admin/jsonConfig.json`` erforderlich - ab Admin Version 5)

    :type: string

.. confval:: common.adminUI.custom

    - ``none``
    - ``materialize`` (``admin/custom_m.html`` erforderlich - ab Admin Version 4)
    - ``json`` (``admin/jsonCustom.json`` erforderlich - ab Admin Version 5)

    :type: string

.. confval:: common.adminUI.tab

    - ``html``
    - ``materialize``

    :type: string

**Weitere Optionen**

.. confval:: objects

    Liste von Objekten, welche für den Adapter erstellt werden sollen

    :type: array

.. confval:: instanceObjects

    Liste von Objekten, welche für jede Instanz automatisch erstellt werden

    :type: array

.. confval:: protectedNative

    Liste von Attributen, welche nur vom Adapter selbst lesbar sind (z.B. ``["password"]``). Siehe :ref:`development-encryption`

    :type: array

.. confval:: encryptedNative

    Liste vo automatisch verschlüsselten Attributen. Siehe :ref:`development-encryption`

    :type: array

.. confval:: native

    Liste von vordefinierten Attributen, welche z.B. in der Admin-Konfiguration überschrieben werden können

    :type: object

.. confval:: notifications

    Liste von Objekten zur Konfiguration zur Konfiguration des internen Notification-Systems. Siehe :ref:`development-notifications`

    **Seit js-controller 3.2.0**

    :type: array

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

Links
-----

- `Schema-Datei <https://github.com/ioBroker/ioBroker.js-controller/blob/master/packages/controller/schemas/io-package.json>`_
- `Offizielle Doku <https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/objectsschema.md>`_
