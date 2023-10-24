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

    Name des Adapters (darf nicht ``ioBroker`` enthalten)

    :type: string

.. confval:: common.version

    Aktuelle Version des Adapters (muss mit der Version der ``package.json`` übereinstimmen)

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
            "de": "Luftdaten.info",
            "ru": "Luftdaten.info",
            "pt": "Luftdaten.info",
            "nl": "Luftdaten.info",
            "fr": "Luftdaten.info",
            "it": "Luftdaten.info",
            "es": "Luftdaten.info",
            "pl": "Luftdaten.info",
            "uk": "Luftdaten.info",
            "zh-cn": "Luftdaten.info"
        }

    :type: object

.. confval:: common.news

    Liste mit Infos zu den verschiedenen Versionen (Updatehistorie / Changelog). Darf nicht mehr als 20 Einträge enthalten! (übersetzt in mehrere Sprachen)

    Wird in der Regel automatisch vom `Release-Script von AlCalzone <https://github.com/AlCalzone/release-script>`_ gefüllt (aus Changelog).

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

.. confval:: common.desc

    Kurze Beschreibung, was der Adapter macht (übersetzt in mehrere Sprachen)

    .. code:: json

        "desc": {
            "en": "Loads current air quality data from a local or remote sensor",
            "de": "Lädt aktuelle Luftqualitätsdaten eines lokalen oder Cloud-Sensors",
            "ru": "Загружает текущие данные о качестве воздуха с местного или удаленного датчика",
            "pt": "Carrega dados atuais de qualidade do ar de um sensor local ou remoto",
            "nl": "Laadt huidige luchtkwaliteitsgegevens van een lokale of externe sensor",
            "fr": "Charge les données actuelles sur la qualité de l'air à partir d'un capteur local ou distant",
            "it": "Carica i dati attuali sulla qualità dell'aria da un sensore locale o remoto",
            "es": "Carga datos actuales de la calidad del aire desde un sensor local o remoto",
            "pl": "Ładuje aktualne dane o jakości powietrza z lokalnego lub zdalnego czujnika",
            "uk": "Поточні дані про якість повітря з локального або віддаленого датчика",
            "zh-cn": "从本地或远程传感器加载当前的空气质量数据"
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

    Legt fest, ob eine neue Instanz direkt gestartet werden soll, oder nicht

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

    Ab ``js-controller`` 5.x sollte ``common.supportedMessages.custom`` verwendet werden!

    :type: boolean
    :default: ``false``

.. confval:: common.blockedVersions

    :octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` 5.0.14

    Diese Information wird in die :ref:`ecosystem-repositories` (Adapter-Listen) aufgenommen, um zu verhindern dass bestimmte Versionen von Adaptern gestartet werden können (falls gravierende Fehler oder Sicherheitslücken enthalten sind).

    Beispiel vom Alexa2-Adapter:

    .. code:: json

        "blockedVersions": [
            "~3.14.0",
            "~3.15.0",
            "~3.16.0",
            "3.17.0",
            "3.17.1",
            "3.17.2",
            "3.17.3"
        ]

    :type: array

.. confval:: common.supportedMessages

    :octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` 5.0.14

    Siehe :ref:`development-messagebox`

    :type: object

.. confval:: common.readme

    URL zur Readme-Datei (z.B. HTTP-URL zur README.md auf GitHub)

    .. code:: json

        "readme": "https://github.com/klein0r/ioBroker.luftdaten/blob/master/README.md"

    :type: string

.. confval:: common.docs

    Eine Liste von Dokumentations-Dateien, welche im Admin zur Verfügung gestellt werden und auch in die `offizielle Dokumentation <https://www.iobroker.net/#de/adapters>`_ aufgenommen werden sollen

    Pro Sprache kann entweder ein Array von Dateien übergeben werden, oder nur ein String

    .. code:: json

        "docs": {
            "en": "docs/en/admin.md",
            "ru": "docs/ru/admin.md",
            "de": [
                "docs/de/admin.md",
                "docs/de/admin/tab-adapters.md",
                "docs/de/admin/tab-instances.md",
                "docs/de/admin/tab-objects.md"
            ],
            "pt": "docs/pt/admin.md",
            "nl": "docs/nl/admin.md",
            "es": "docs/es/admin.md",
            "fr": "docs/fr/admin.md",
            "it": "docs/it/admin.md",
            "pl": "docs/pl/admin.md",
            "uk": "docs/uk/admin.md",
            "zh-cn": "docs/zh-cn/admin.md"
        }

    :type: object

.. confval:: common.authors

    Liste mit Entwicklern des Adapters

    .. code:: json

        "authors": [
            "Matthias Kleine <info@haus-automatisierung.com>"
        ]

    Alternativ

    .. code:: json

        "authors": [
            {
                "name": "Matthias Kleine",
                "email": "info@haus-automatisierung.com"
            }
        ]

    :type: string oder array

.. confval:: common.license

    Lizenz des Adapters (z.B. MIT). Gültige Werte sind im Schema zu finden (Link siehe unten)

    :type: string

.. confval:: common.type

    Typ/Kategorie des Adapters - relevant für die Einsortierung im Admin-Adapter.

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
    - ``storage`` - Daten-Speicherung wie history, mySQL oder InfluxDB - siehe :ref:`adapters-databases`
    - ``utility`` - Weitere Tools wie Backup-Adapter
    - ``visualization`` - Visualisierungs-Adapter
    - ``visualization-icons`` - Zusätzliche Icons für die Visualisierung
    - ``visualization-widgets`` - Weitere Widgets für die Visualisierung
    - ``weather`` - Wetterdaten

    :type: string

.. confval:: common.unsafePerm

    Legt fest, ob das Adapter-Paket mit dem ``--unsafe-perm`` Parameter für npm installiert werden **muss**. Siehe `npm Dokumentation <https://docs.npmjs.com/cli/v9/using-npm/config#unsafe-perm>`_

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

    .. code:: json

        "availableModes": [
            "schedule",
            "once"
        ]

    :type: array

.. confval:: common.blockly

    Legt fest, ob der Adapter eigene Blockly-Bausteine mitbringt (``admin/blockly.js`` erforderlich)

    :type: boolean
    :default: ``false``

.. confval:: common.connectionType

    Definiert die Qulle der Adapter-Daten. Wird im Admin ab Version 5 dargestellt und dient als Information für den Nutzer

    - ``none``
    - ``local`` - Die Kommunikation findet lokal / im eigenen Netzwerk statt (z.B. mit dem Gerät direkt per HTTP)
    - ``cloud`` - Für den Adapter ist eine aktive Internetverbindung erforderlich. Die Daten werden z.B. vom Server des Herstellers abgerufen.

    :type: string

.. confval:: common.dataSource

    Legt fest, wie Daten geholt werden

    - ``none``
    - ``poll`` - Die Daten werden regelmäßig abgefragt (z.B. per Zeitplan)
    - ``push`` - Das Gerät / der Dienst liefert die Daten selbstständig zum Adapter
    - ``assumption`` - Der genaue Status ist nicht definiert

    :type: string

.. confval:: common.compact

    :octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` 2.0.2

    Legt fest, ob der Adapter im :ref:`basics-compactmode` gestartet werden kann

    :type: boolean
    :default: ``false``

.. confval:: common.dataFolder

    :octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` 1.5.1

    Verzeichnis-Pfad, in welchem der Adapter seine Daten ablegt (relativ zu ``/opt/iobroker/iobroker-data``). Siehe :ref:`bestpractice-storefiles`

    Der Platzhalter ``%INSTANCE%`` kann ebenfalls im Pfad genutzt werden und wird automatisch durch die Instanznummer ersetzt (z.B. ``0``).

    Falls angegeben, wird dieses Verzeichnis automatisch vom ``js-controller`` in die Backups mit aufgenommen.

    .. code:: json

        "dataFolder": "octoprint.%INSTANCE%"

    :type: string

.. confval:: common.dependencies

    Liste von Abhängigkeiten (auf dem gleichen Host), welche für diesen Adapter notwendig sind. Entweder mit genauer Versionsangabe, oder als String.

    .. code:: json

        "dependencies": [
            "admin",
            {
                "js-controller": ">=3.3.0"
            }
        ]

    :type: array

.. confval:: common.globalDependencies

    Liste von Abhängigkeiten im gesamten ioBroker-System (Multihost-Betrieb). Entweder mit genauer Versionsangabe, oder als String. Siehe :ref:`basics-multihost`

    .. code:: json

        "globalDependencies": [
            {
                "admin": ">=5.1.19"
            }
        ]

    :type: array

.. confval:: common.eraseOnUpload

    :octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` 1.5.1

    Löscht alle existierenden Daten im Adapter-Verzeichnis vor einem Upload

    :type: boolean

.. confval:: common.extIcon

    URL zur Icon-Datei für die Admin-Übersicht (z.B. PNG-Datei auf GitHub). Wird genutzt, wenn der Adapter noch nicht installiert ist.

    .. code:: json

        "extIcon": "https://raw.githubusercontent.com/klein0r/ioBroker.luftdaten/master/admin/luftdaten.png"

    :type: string

.. confval:: common.getHistory

    Legt fest, ob der Adapter den ``getHistory`` Befehl unterstützt (siehe z.B. InfluxDB-Adapter)

    Ab ``js-controller`` 5.x sollte ``common.supportedMessages.getHistory`` verwendet werden!

    :type: boolean

.. confval:: common.icon

    Pfad zum lokalen Icon des Adapters (nach Installation). Relativer Pfad zum Unterverzeichnis ``admin/``

    .. code:: json

        "icon": "luftdaten.png"

    :type: string

.. confval:: common.keywords

    Liste von Schlüsselwörtern, um den Adapter über die Suche im Admin-Adapter (besser) finden zu können

    .. code:: json

        "keywords": [
            "web",
            "weather",
            "air",
            "quality"
        ]

    :type: array

.. confval:: common.localLinks

    Konfiguration für Intro-Tab und Instanz-Übersicht (Direktlink). Hier können Links für verschiedene Dienste o.ä. hinterlegt werden (auch externe Links).

    Die Instanz muss aktiv sein ``common.enabled: true`` damit diese Links angezeigt werden!

    Eigenschaften:

    - ``link`` (string, erforderlich!)
    - ``color`` (string)
    - ``pro`` (boolean)

    .. code:: json

        "localLinks": {
            "_default": {
                "link": "https://haus-automatisierung.com",
                "color": "#fc8326"
            }
        }

    Ist der Standard-Name nicht `_default`, wird dieser Name ebenfalls in der Kachel im Intro-Tab angezeigt:

    .. code:: json

        "localLinks": {
            "iobroker-kurs": {
                "link": "https://haus-automatisierung.com/iobroker-kurs/",
                "color": "#fc8326"
            }
        }

    In diesen Links können verschiedene Platzhalter verwendet werden, welche automatisch ersetzt werden:

    - ``%ip%``
    - ``%protocol%``
    - ``%instance%``
    - ``%objects%``
    - ``%hostname%``
    - ``%port%``
    - ``%hosts%``
    - ``%adminInstance%``

    .. code:: json

        "localLinks": {
            "_default": {
                "link": "%protocol%://%bind%:%port%"
            }
        }

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

    .. todo::
        Explain common.noIntro

    :type: boolean

.. confval:: common.noRepository

    .. todo::
        Explain common.noRepository

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

.. confval:: common.osDependencies

    Abhängigkeiten für verschiedene Betriebssysteme

    :type: object

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

    :type: string|array

.. confval:: common.preserveSettings

    Liste mit Attributen, welche nicht automatisch gelöscht werden sollen (z.B. ``history``)

    :type: string|array

.. confval:: common.restartAdapters

    Liste mit Adaptern, welche neugestartet werden sollen, nachdem dieser Adapter installiert wurde (z.B. ``["vis"]``)

    :type: array

.. confval:: common.serviceStates

    .. todo::
        Explain common.serviceStates

    :type: string|boolean

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

    Legt fest, ob dieser Adapter von anderen Adaptern automatisch abonniert werden soll

    :type: boolean

.. confval:: common.subscribe

    .. todo::
        Explain common.subscribe

    :type: string

.. confval:: common.supportCustoms

    Legt fest, ob es zusätzliche Einstellungen für jeden Datenpunkt gibt

    - ``admin/custom.html`` erforderlich - ab Admin Version 3
    - ``admin/custom_m.html`` erforderlich - ab Admin Version 4
    - ``admin/jsonCustom.json`` erforderlich - ab Admin Version 5

    :type: boolean

.. confval:: common.supportStopInstance

    Legt fest, ob der Adapter das ``stopInstance`` Signal unterstützt.  Siehe :ref:`development-messagebox`

    Ab ``js-controller`` 5.x sollte ``common.supportedMessages.stopInstance`` verwendet werden!

    :type:  boolean

.. confval:: common.wakeup

    Legt fest, ob die Instanz gestartet werden soll, wenn ein Wert in ``system.adapter.<adapter-name>.<instanz-nummmer>.wakeup`` geschrieben wird.

    :type: boolean

.. confval:: common.webservers

    Liste mit Webservern, welche Inhalte aus dem www-Verzeichnis des Adapters liefern

    :type: array

.. confval:: common.welcomeScreen

    .. todo::
        Explain common.welcomeScreen

    :type: array

.. confval:: common.welcomeScreenPro

    Identisch zu ``common.welcomeScreen``, allerdings für Zugriff über die ioBroker-Cloud

    .. code:: json

        "welcomeScreenPro": {
            "link": "admin/index.html",
            "name": "Admin",
            "img": "admin/img/admin.png",
            "color": "pink",
            "order": 5,
            "localLinks": "_default",
            "localLink": true
        }

    :type: object

.. confval:: common.messages

    Wichtige Informationen/Warnungen/Gefahren, welche im Admin-Adapter als Hinweis angezeigt werden sollen.

    Mögliche Eigenschaften pro Nachricht:

    - ``title`` (erforderlich) - sollte in alle Sprachen übersetzt werden
    - ``text`` (erforderlich) - sollte in alle Sprachen übersetzt werden
    - ``buttons`` (erforderlich) - ``ok``, ``agree`` oder ``cancel``
    - ``condition``
    - ``link``
    - ``linkText`` - sollte in alle Sprachen übersetzt werden
    - ``level`` (``info``, ``warn`` oder ``error``)

    .. code:: json

        "messages": [
            {
                "condition": {
                    "operand": "and",
                    "rules": [
                        "oldVersion<4.0.0",
                        "newVersion>=4.0.0"
                    ]
                },
                "title": {
                    "en": "Important notice!",
                    "de": "Wichtiger Hinweis!",
                    "ru": "Важное замечание!",
                    "pt": "Notícia importante!",
                    "nl": "Belangrijke mededeling!",
                    "fr": "Avis important!",
                    "it": "Avviso IMPORTANTE!",
                    "es": "Noticia importante!",
                    "pl": "Ważna uwaga!",
                    "uk": "Погода!",
                    "zh-cn": "重要通知!"
                },
                "text": {
                    "en": "Do not update to this version if you are scared",
                    "de": "Aktualisieren Sie nicht auf diese Version, wenn Sie Angst haben",
                    "ru": "Не обновляйте эту версию, если вам страшно",
                    "pt": "Não atualize para esta versão se estiver com medo",
                    "nl": "Vertaling:",
                    "fr": "Ne pas mettre à jour cette version si vous avez peur",
                    "it": "Non aggiornare a questa versione se hai paura",
                    "es": "No actualice a esta versión si tiene miedo",
                    "pl": "Nie uaktualniasz tej wersji, jeśli nie będziesz straszony",
                    "uk": "Чи не оновлюється в цю версію, якщо ви рубати",
                    "zh-cn": "如果你遇难,不要更新本版本。"
                },
                "level": "warn",
                "buttons": [
                    "ok",
                    "cancel"
                ]
            }
        ]

    :type: array

.. confval:: objects

    Liste von Objekten, welche für den Adapter erstellt werden sollen

    :type: array

.. confval:: instanceObjects

    Liste von Objekten, welche für jede Instanz erstellt werden sollen

    :type: array

.. confval:: protectedNative

    :octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 2.0.2

    Liste von ``native`` Attributen, welche nur vom Adapter / der jeweiligen Instanz selbst lesbar sind (z.B. ``["password"]``). Siehe :ref:`development-encryption`

    :type: array

.. confval:: encryptedNative

    :octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 3.0.3

    Liste von automatisch verschlüsselten ``native`` Attributen. Siehe :ref:`development-encryption`

    :type: array

.. confval:: native

    Liste von vordefinierten Attributen, welche z.B. in der Instanz-Konfiguration überschrieben werden können

    .. code:: json

        "native": {
            "port": 12345,
            "apiPassword: "xxx",
            "requestTimeout": 10
        }

    :type: object

.. confval:: notifications

    :octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 5.0.14

    Liste von Objekten zur Konfiguration zur Konfiguration des internen Notification-Systems. Siehe :ref:`development-notifications`

    - ``scope`` (erforderlich)
    - ``name`` (erforderlich) - sollte in alle Sprachen übersetzt werden
    - ``description`` (erforderlich) - sollte in alle Sprachen übersetzt werden
    - ``categories`` (erforderlich)

    Hier ein Beispiel aus dem Admin-Adapter, welche Notifications für News erlaubt. Diese werden dann im Admin-Adapter dargestellt.

    .. code:: json

        "notifications": [
            {
                "scope": "news",
                "name": {
                    "en": "News",
                    "de": "Nachrichten",
                    "ru": "Новости",
                    "pt": "Notícias",
                    "nl": "Nieuws",
                    "fr": "Actualités",
                    "it": "Notizie",
                    "es": "Noticias",
                    "pl": "News",
                    "uk": "Новини",
                    "zh-cn": "新闻"
                },
                "description": {
                    "en": "These notifications represent news regarding installed adapters or general ioBroker information.",
                    "de": "Diese Benachrichtigungen enthalten Neuigkeiten zu installierten Adaptern oder allgemeine ioBroker-Informationen.",
                    "ru": "Эти уведомления представляют новости о установленных адаптерах или общей информации ioBroker.",
                    "pt": "Estas notificações representam notícias sobre adaptadores instalados ou informações gerais do ioBroker.",
                    "nl": "Deze berichten zijn nieuws over geïnstalleerde adapters of algemene ioBroker informatie.",
                    "fr": "Ces notifications représentent des nouvelles concernant les adaptateurs installés ou les informations générales ioBroker.",
                    "it": "Queste notifiche rappresentano notizie riguardanti adattatori installati o informazioni generali su ioBroker.",
                    "es": "Estas notificaciones representan noticias sobre adaptadores instalados o información general ioBroker.",
                    "pl": "Noty te reprezentują informacje dotyczące zainstalowanych adapterów lub ogólnie dostępnych informacji ioBrokera.",
                    "uk": "Ці повідомлення представляють новини про встановлені адаптери або загальні відомості про ioBroker.",
                    "zh-cn": "这些通知是有关安装的适应器或一般的气箱信息的新闻。."
                },
                "categories": [
                    {
                        "category": "info",
                        "name": {
                            "en": "General news",
                            "de": "Allgemeine Nachrichten",
                            "ru": "Общие новости",
                            "pt": "Notícia geral",
                            "nl": "Generaal",
                            "fr": "Nouvelles générales",
                            "it": "Notizie generali",
                            "es": "Noticias generales",
                            "pl": "Strona oficjalna",
                            "uk": "Новини",
                            "zh-cn": "新闻"
                        },
                        "severity": "notify",
                        "description": {
                            "en": "These messages represent general news, which just have informal purpose and do not need to be read immediately.",
                            "de": "Diese Nachrichten stellen allgemeine Nachrichten dar, die nur informellen Zweck haben und nicht sofort gelesen werden müssen.",
                            "ru": "Эти сообщения представляют собой общие новости, которые просто имеют неформальную цель и не нужно читать немедленно.",
                            "pt": "Essas mensagens representam notícias gerais, que apenas têm um propósito informal e não precisam ser lidas imediatamente.",
                            "nl": "Deze berichten vertegenwoordigen algemene nieuws, wat informeel doel heeft en niet onmiddellijk hoeft te worden gelezen.",
                            "fr": "Ces messages représentent des nouvelles générales, qui ont juste un but informel et ne doivent pas être lus immédiatement.",
                            "it": "Questi messaggi rappresentano notizie generali, che hanno solo scopo informale e non devono essere letti immediatamente.",
                            "es": "Estos mensajes representan noticias generales, que sólo tienen un propósito informal y no necesitan ser leídos inmediatamente.",
                            "pl": "Wiadomości te reprezentują ogólnokrajowe wiadomości, które tylko mają nieformalny cel i nie muszą być odczytane natychmiast.",
                            "uk": "Ці повідомлення представляють загальні новини, які просто мають неформальне призначення і не потрібно негайно прочитати.",
                            "zh-cn": "这些信息是一般新闻,这只是非正式目的,不需要立即阅读。."
                        },
                        "regex": [],
                        "limit": 10
                    },
                    {
                        "category": "warning",
                        "name": {
                            "en": "Important news",
                            "de": "Wichtige Nachrichten",
                            "ru": "Важные новости",
                            "pt": "Notícia importante",
                            "nl": "Belangrijk nieuws",
                            "fr": "Nouvelles importantes",
                            "it": "Notizie importanti",
                            "es": "Noticias importantes",
                            "pl": "Important news",
                            "uk": "Новини",
                            "zh-cn": "重要的新闻"
                        },
                        "severity": "info",
                        "description": {
                            "en": "These messages represent adapter warnings and important changes in the near future.",
                            "de": "Diese Nachrichten stellen Adapterwarnungen und wichtige Veränderungen in der nahen Zukunft dar.",
                            "ru": "Эти сообщения представляют предупреждение о адаптере и важные изменения в ближайшем будущем.",
                            "pt": "Estas mensagens representam avisos de adaptadores e mudanças importantes no futuro próximo.",
                            "nl": "Deze berichten vertegenwoordigen adapter waarschuwingen en belangrijke veranderingen in de nabije toekomst.",
                            "fr": "Ces messages représentent des avertissements d'adaptateur et des changements importants dans un proche avenir.",
                            "it": "Questi messaggi rappresentano avvisi di adattatore e cambiamenti importanti nel prossimo futuro.",
                            "es": "Estos mensajes representan advertencias de adaptador y cambios importantes en el futuro cercano.",
                            "pl": "Wiadomości te reprezentują ostrzeżenia adaptatora i ważne zmiany w najbliższej przyszłości.",
                            "uk": "Ці повідомлення представляють попередження та важливі зміни в найближчому майбутньому.",
                            "zh-cn": "这些信息是适应的预警和近期的重要变化。."
                        },
                        "regex": [],
                        "limit": 10
                    },
                    {
                        "category": "danger",
                        "name": {
                            "en": "Very important news",
                            "de": "Sehr wichtige Nachrichten",
                            "ru": "Очень важные новости",
                            "pt": "Notícia muito importante",
                            "nl": "Heel belangrijk",
                            "fr": "Nouvelles très importantes",
                            "it": "Notizie molto importanti",
                            "es": "Noticias muy importantes",
                            "pl": "Ważne wiadomości",
                            "uk": "Останні новини",
                            "zh-cn": "非常重要的新闻"
                        },
                        "severity": "alert",
                        "description": {
                            "en": "These notifications are very important. They may give you a hint that an adapter upgrade is required right now to maintain functionality.",
                            "de": "Diese Benachrichtigungen sind sehr wichtig. Sie können Ihnen einen Hinweis geben, dass ein Adapter-Upgrade jetzt erforderlich ist, um die Funktionalität zu erhalten.",
                            "ru": "Эти уведомления очень важны. Они могут дать вам подсказку, что обновление адаптера требуется прямо сейчас для поддержания функциональности.",
                            "pt": "Estas notificações são muito importantes. Eles podem lhe dar uma dica de que uma atualização do adaptador é necessária agora para manter a funcionalidade.",
                            "nl": "Deze berichten zijn heel belangrijk. Ze kunnen je een hint geven dat een adapter upgrade nu nodig is om functionaliteit te behouden.",
                            "fr": "Ces notifications sont très importantes. Ils peuvent vous donner un indice qu'une mise à niveau d'adaptateur est nécessaire pour maintenir la fonctionnalité.",
                            "it": "Queste notifiche sono molto importanti. Essi possono dare un suggerimento che un aggiornamento adattatore è necessario in questo momento per mantenere la funzionalità.",
                            "es": "Estas notificaciones son muy importantes. Pueden darle una pista de que se requiere una actualización del adaptador ahora mismo para mantener la funcionalidad.",
                            "pl": "Te informacje są bardzo ważne. Mogą dać wskazówki, że ulepszanie adapteru jest niezbędne do utrzymania funkcji.",
                            "uk": "Ці повідомлення дуже важливі. Вони можуть надати вам підказку, що оновлення адаптера потрібно прямо зараз для підтримки функціональності.",
                            "zh-cn": "这些通知非常重要。 他们可以向你说明,适应人员升级现在需要保持功能。."
                        },
                        "regex": [],
                        "limit": 10
                    }
                ]
            }
        ]

    :type: array

Eigenschaften (Schedule)
------------------------

.. confval:: common.schedule

    CRON-Definition, wann die Instanzen gestartet werden sollen (kann vom Benutzer angepasst werden)

    .. code:: json

        "schedule": "*/30 * * * *"

    :type: string

.. confval:: common.allowInit

    Legt fest, ob ein Adapter auch außerhalb des definierten Zeitplanes gestartet wird (z.B. nach Änderung der Instanz-Konfiguration)

    :type: boolean

Eigenschaften (Daemon)
----------------------

.. confval:: common.restartSchedule

    CRON-Definition, wann die laufenden Instanzen neugestartet werden sollen (kann vom Benutzer angepasst werden)

    :type: string

Eigenschaften (Web-Adapter)
---------------------------

.. confval:: common.webByVersion

    .. todo::
        Explain common.webByVersion

    :type: boolean

.. confval:: common.webExtendable

    Legt fest, ob dieser Adapters mit Web-Plugins erweitert werden kann (z.B. ``web`` Adapter).

    Adapter mit diesem Attribut:

    - `ioBroker.web <https://github.com/ioBroker/ioBroker.web>`_

    :type: boolean

.. confval:: common.webExtension

    Relativer Pfad zur Web-Extension des Web-Servers

    Adapter mit diesem Attribut:

    - `ioBroker.simple-api <https://github.com/ioBroker/ioBroker.simple-api>`_
    - `ioBroker.proxy <https://github.com/ioBroker/ioBroker.proxy>`_
    - `ioBroker.cameras <https://github.com/ioBroker/ioBroker.cameras>`_
    - `ioBroker.lametric <https://github.com/klein0r/ioBroker.lametric/>`_
    - `ioBroker.gira-iot <https://github.com/klein0r/ioBroker.gira-iot>`_

    .. code:: json

        "webExtension": "lib/web.js"

    :type: string

.. confval:: common.webPreSettings

    Die hier definierten Attribute werden als JavaScript-Variablen im Window-Scope (``window.${attr}``) deklariert

    :type: object

Eigenschaften (Admin-Adapter)
-----------------------------

.. confval:: common.adminColumns

    Eigene Attribute, welche im Admin als Spalten verfügbar werden sollen.

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

.. confval:: common.adminTab

    .. code:: json

        "adminTab": {
            "name": {
                "en": "Zigbee",
                "de": "Zigbee",
                "ru": "Zigbee",
                "pt": "Zigbee",
                "nl": "Zigbee",
                "fr": "Zigbee",
                "it": "Zigbee",
                "es": "Zigbee",
                "pl": "Zigbee",
                "uk": "Zigbee",
                "zh-cn": "Zigbee"
            },
            "singleton": true,
            "fa-icon": "</i><img style='width:24px;margin-bottom:-6px;' src='/adapter/zigbee/zigbee.svg'><i>"
        }

    :type: object

.. confval:: common.adminTab.fa-icon

    `Font-Awesome <https://fontawesome.com/icons>`_ Icon für das Tab

    :type: string

.. confval:: common.adminTab.ignoreConfigUpdate

    .. todo::
        Explain common.adminTab.ignoreConfigUpdate

    :type: boolean

.. confval:: common.adminTab.link

    Link für den iFrame im Admin-Tab. Unterstützt zu ersetzende Platzhalter wie ``%ip%`` oder ``%port%``.

    :type: string

.. confval:: common.adminTab.name

    Titel des Tabs (übersetzt in mehrere Sprachen)

    :type: object

.. confval:: common.adminTab.singleton

    Legt fest, ob nur ein Tab für alle Instanzen angezeigt werden soll

    :type: boolean

.. confval:: common.adminUI

    Legt fest, wie die Konfiguration im Admin erfolgen soll (für die Instanz-Konfiguration, Admin-Tabs und eigene Objekt-Eigenschaften) - siehe :ref:`development-adminconfig`

    :type: object

.. confval:: common.adminUI.config

    Legt fest, wie die Konfiguration für die Admin-Oberfläche aufgebaut ist

    - ``none``
    - ``html`` (``admin/index.html`` - ab Admin Version 3)
    - ``materialize`` (``admin/index_m.html`` - ab Admin Version 4)
    - ``json`` (``admin/jsonConfig.json`` - ab Admin Version 5)

    :type: string

.. confval:: common.adminUI.custom

    - ``none``
    - ``html`` (``admin/custom.html`` - ab Admin Version 3)
    - ``materialize`` (``admin/custom_m.html`` - ab Admin Version 4)
    - ``json`` (``admin/jsonCustom.json`` - ab Admin Version 5)

    :type: string

.. confval:: common.adminUI.tab

    - ``html``
    - ``materialize``

    :type: string

Eigenschaften (VIS-Adapter)
---------------------------

.. confval:: common.visWidgets

    :octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``vis`` 2.0.0

    Definiert die verfügbaren VIS-Widgets im Adapter. Beispiel im offiziellen `Template-Repository <https://github.com/ioBroker/ioBroker.vis-widgets-react-template>`_.

    .. code:: json

        "visWidgets": {
            "DemoWidget": {
                "name": "DemoWidget",
                "url": "vis-widgets-react-template/customWidgets.js",
                "components": [
                    "DemoWidget"
                ]
            }
        }

    :type: object

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
- ``common.materialize`` (boolean) - Legt fest, ob der Adapter die Admin-Oberfläche für Admin-Version 3 und 4 bereitstellt (ab Admin 5 sollte ``common.adminUI.config`` verwendet werden) - siehe :ref:`development-adminconfig`
- ``common.materializeTab`` (boolean) - Legt fest, ob der Adapter ein eigenes Tab für Admin-Version 3 und 4 bereitstellt (ab Admin 5 sollte ``common.adminUI.tab`` verwendet werden) - siehe :ref:`development-adminconfig`
- ``common.noConfig`` (boolean) - Definiert, ob Instanzen konfiguriert werden können (ab Admin 5 sollte ``common.adminUI.config = none`` verwendet werden) - siehe :ref:`development-adminconfig`

Links
-----

- `Schema-Datei <https://github.com/ioBroker/ioBroker.js-controller/blob/master/schemas/io-package.json>`_
- `SchemaStore <https://github.com/SchemaStore/schemastore/blob/master/src/schemas/json/io-package.json>`_
- `Offizielle Doku <https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/objectsschema.md>`_
