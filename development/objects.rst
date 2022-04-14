.. _development-objects:

Objekte
=======

.. note::
    Lies zuerst die Grundlagen zur :ref:`basics-datastorage`.

Objekte können beispielsweise über das :ref:`basics-cli` ausgelesen werden.

.. confval:: _id

    Eindeutige ID

    :type: string

.. confval:: type

    Typ des Objektes. Gültige Werte sind:

    - ``state`` - Zustand. Das übergeornete Objekte sollte vom Typ ``channel``, ``device``, ``instance`` oder ``host`` sein. Siehe :ref:`development-states`
    - ``channel`` - "Kanal" um mehrere Zustände darunter zu strukturieren. Das übergeornete Objekte sollte vom Typ ``device`` sein.
    - ``device`` - "Gerät" um mehrere Zustände oder Kanäle darunter zu strukturieren. Das übergeornete Objekte sollte vom Typ ``instance`` sein.
    - ``enum`` - "Liste" mit vordefinierten Werten in ``common.members``. that points to the states, channels, devices or files.
    - ``host`` - Ein "Host", welcher einen ``js-controller`` Prozess ausführt. Beispielsweise ``system.host.raspberrypi-iobroker``.
    - ``adapter`` - Die Standard-Konfiguration von einem Adaper. Beispielsweise ``system.adapter.admin``.
    - ``instance`` - Die Konfiguration der einzelnen Instanz. Beispielsweise ``system.adapter.admin.0``. Das übergeornete Objekte sollte vom Typ ``adapter`` sein.
    - ``meta`` - Sich selten ändernde Meta-Informationen wie zum Beispiel die :ref:`basics-uuid` unter ``system.meta.uuid``.
    - ``config`` - Konfigurationen. Beispielsweise ``system.config`` oder ``system.repositories``
    - ``script`` - Skripte unter ``script.js.*``
    - ``user`` - Benutzer des Systems. Beispielsweise ``system.user.admin``
    - ``group`` - Benutzer-Gruppen des Sytems. Beispielsweise ``system.group.administrator``
    - ``chart`` - Diagramm
    - ``folder`` - Verzeichnis, welches z.B. Geräte (Typ ``device`` sammelt). Beispielsweise ``system.host.raspberrypi-iobroker.notifications``

    :type: string

.. confval:: common.name

    *(optional)* Name des Objektes - wird im Frontend (wie dem Admin) dargestellt. **Es ist empfohlen, diesen Wert zu setzen!**

    .. code:: json

        "common": {
            "name": {
                "en": "Beispiel",
                "de": "Beispiel",
                "ru": "Бейшпиль",
                "pt": "Beispiel",
                "nl": "Beispiel",
                "fr": "Beispiel",
                "it": "Beispiel",
                "es": "Beispiel",
                "pl": "Beispiel",
                "zh-cn": "贝斯皮尔"
            }
        }

    :type: string|object

.. confval:: common.expert

    Objekt nur im Experten-Modus sichtbar (z.B. im Admin-Adapter)

    :type: boolean
    :default: false

.. confval:: common.dontDelete

    Objekt kann nicht vom Benutzer gelöscht werden (wird auch vom ``js-controller`` geprüft). **Bitte mit Vorsicht verwenden!**

    :type: boolean
    :default: false

.. confval:: common.icon

    *(optional)* Icon, welches z.B. im Admin-Adapter dargestellt werden soll. Mögliche Werte sind:

    - Pfadangabe relativ zum Admin-Verzeichnis (z.B. PNG-Datei)
    - Base64-String mit den Daten des Icons (SVG, PNG, ...)

    .. code:: json

        "common": {
            "icon": "/icons/160_hmipw-drs4_thumb.png"
        }

    oder auch

    .. code:: json

        "common": {
            "icon": "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGhlaWdodD0iMjRweCIgdmlld0JveD0iMCAwIDI0IDI0IiB3aWR0aD0iMjRweCI+DQogICAgPHBhdGggZmlsbD0iY3VycmVudENvbG9yIiBkPSJNMTIgNS42OWw1IDQuNVYxOGgtMnYtNkg5djZIN3YtNy44MWw1LTQuNU0xMiAzTDIgMTJoM3Y4aDZ2LTZoMnY2aDZ2LThoM0wxMiAzeiIvPg0KPC9zdmc+"
        }

    :type: string

.. confval:: common.custom

    *(optional)* Zusatzkonfiguration weiterer Adapter für das Objekt. Wird zum Beispiel für Datenbank-Adapter genutzt. Je Eintrag ist das ``enabled`` Attribut erforderlich.

    .. code:: json

        "custom": {
            "influxdb.0": {
                "enabled": true,
                "storageType": "",
                "aliasId": "",
                "changesOnly": true,
                "debounce": "1000",
                "changesRelogInterval": "0",
                "changesMinDelta": "0"
            },
            "history.0": {
                "enabled": true,
                "aliasId": "",
                "changesOnly": true,
                "debounce": 1000,
                "changesRelogInterval": 0,
                "changesMinDelta": 0,
                "maxLength": 960,
                "retention": 31536000
            },
            "iot.0": {
                "smartName": {
                    "de": "Test"
                }
            }
        }

    :type: object

.. confval:: native

    Eigenschaften des Zielsystems (z.B. eine ID eines Gerätes)

    :type: object

Typ state
---------

.. confval:: common.type

    Typ der zu speichernden Daten im Zustand

    - ``mixed`` - Kann einen beliebigen Wert annehmen (nicht empfohlen)
    - ``number`` - Numerische Werte
    - ``string`` - Zeichenketten
    - ``boolean`` - true / false
    - ``array`` - Liste von Werten
    - ``object`` - Objekt
    - ``json`` - ???
    - ``file`` - ???
    - ``multistate`` - Auswahlmöglichkeiten (Enum)

    .. warning::
        Falls der Typ ``array``, ``object`` oder ``mixed`` lautet, muss der Wert als String mit ``JSON.stringify()`` gespeichert werden.

    :type: string
    :default: ``mixed``

.. confval:: common.role

    Rolle des zugehörigen State, welche festlegt, wie der Wert im Frontend (Admin) dargestellt werden soll.

    :type: string

.. confval:: common.read

    *(optional)* Legt fest, ob der zugehörige State gelesen werden darf. Siehe :ref:`development-states`

    :type: boolean

.. confval:: common.write

    *(optional)* Legt fest, ob der zugehörige State geschrieben werden darf. Siehe :ref:`development-states`

    :type: boolean

.. confval:: common.min

    *(optional)* Der erlaubte Minimalwert des Zustands (wenn ``common.type`` = ``number``)

    :type: number

.. confval:: common.max

    *(optional)* Der erlaubte Maximalwert des Zustands (wenn ``common.type`` = ``number``)

    :type: number

.. confval:: common.step

    *(optional)* Der Schrittweite des Zustands (wenn ``common.type`` = ``number``)

    :type: number

.. confval:: common.unit

    *(optional)* Einheit des Zustands (z.B. ``°C``)

    :type: string

.. confval:: common.def

    *(optional)* Standard-Wert des Zustands

    :type: mixed

.. confval:: common.defAck

    *(optional)*

.. confval:: common.desc

    *(optional)* Beschreibung des Zustands

    :type: string

.. confval:: common.states

    *(optional)* Liste mit zulässigen Werten für den Zustand

    .. code:: json

        "states": {
            "value": "valueName",
            "value2": "valueName2",
            0: "OFF",
            1: "ON"
        }

    .. code:: json

        "states": [
            "ON",
            "OFF",
            "UNKNOWN"
        ]

    :type: object|array

.. confval:: common.alias

    *(optional)* Nur für Objekte innerhalb des Namespace ``alias.0`` relevant. Siehe :ref:`basics-aliases`.

    .. code:: json

        "alias": {
            "id": "0_userdata.0.test.myNumber",
            "read": "val - 1",
            "write": "val + 1"
        }

    :type: object

.. confval:: common.workingID

    *(optional)*

.. code:: console

    iobroker object get hm-rpc.0.0015DBE99EAFB6.0.ACTUAL_TEMPERATURE

.. code:: json

    {
        "type": "state",
        "common": {
            "name": "HmIPW-DRS4:AFB6:0.ACTUAL_TEMPERATURE",
            "type": "number",
            "role": "value.temperature",
            "read": true,
            "write": false,
            "min": -3276.8,
            "max": 3276.7,
            "unit": "°C",
            "def": 0
        },
        "native": {
            "MIN": -3276.8,
            "UNIT": "�C",
            "OPERATIONS": 5,
            "MAX": 3276.7,
            "FLAGS": 1,
            "ID": "ACTUAL_TEMPERATURE",
            "TYPE": "FLOAT",
            "DEFAULT": 0,
            "CONTROL": "MAINTENANCE.ACTUAL_TEMPERATURE"
        },
        "from": "system.adapter.hm-rega.0",
        "user": "system.user.admin",
        "ts": 1634815997733,
        "_id": "hm-rpc.0.0015DBE99EAFB6.0.ACTUAL_TEMPERATURE",
        "acl": {
            "object": 1636,
            "state": 1636,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

Typ channel
-----------

.. code:: console

    iobroker object get hm-rpc.0.0015DBE99EAFB6.0

.. code:: json

    {
        "_id": "hm-rpc.0.0015DBE99EAFB6.0",
        "type": "channel",
        "common": {
            "name": "HmIPW-DRS4:AFB6:0"
        },
        "native": {
            "TYPE": "MAINTENANCE",
            "SUBTYPE": "",
            "ADDRESS": "0015DBE99EAFB6:0",
            "RF_ADDRESS": 0,
            "CHILDREN": [],
            "PARENT": "0015DBE99EAFB6",
            "PARENT_TYPE": "HmIPW-DRS4",
            "INDEX": 0,
            "AES_ACTIVE": 1,
            "PARAMSETS": [
                "MASTER",
                "VALUES",
                "SERVICE"
            ],
            "FIRMWARE": "",
            "AVAILABLE_FIRMWARE": "",
            "UPDATABLE": true,
            "FIRMWARE_UPDATE_STATE": "",
            "VERSION": 1,
            "FLAGS": 1,
            "LINK_SOURCE_ROLES": "",
            "LINK_TARGET_ROLES": "",
            "DIRECTION": 0,
            "GROUP": "",
            "TEAM": "",
            "TEAM_TAG": "",
            "TEAM_CHANNELS": [],
            "INTERFACE": "",
            "ROAMING": 0,
            "RX_MODE": 0
        },
        "from": "system.adapter.hm-rega.0",
        "user": "system.user.admin",
        "ts": 1634815997644,
        "acl": {
            "object": 1636,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

Typ device
----------

.. code:: console

    iobroker object get hm-rpc.0.0015DBE99EAFB6

.. code:: json

    {
        "_id": "hm-rpc.0.0015DBE99EAFB6",
        "type": "device",
        "common": {
            "name": "HmIPW-DRS4:AFB6",
            "icon": "/icons/160_hmipw-drs4_thumb.png"
        },
        "native": {
            "TYPE": "HmIPW-DRS4",
            "SUBTYPE": "DRS4",
            "ADDRESS": "0015DBE99EAFB6",
            "RF_ADDRESS": 11517669,
            "CHILDREN": [
                "0015DBE99EAFB6:0",
                "0015DBE99EAFB6:1",
                "0015DBE99EAFB6:2"
            ],
            "PARENT": "",
            "PARENT_TYPE": "",
            "INDEX": 0,
            "AES_ACTIVE": 1,
            "PARAMSETS": [
                "MASTER",
                "SERVICE"
            ],
            "FIRMWARE": "1.2.4",
            "AVAILABLE_FIRMWARE": "0.0.0",
            "UPDATABLE": true,
            "FIRMWARE_UPDATE_STATE": "UP_TO_DATE",
            "VERSION": 1,
            "FLAGS": 1,
            "LINK_SOURCE_ROLES": "",
            "LINK_TARGET_ROLES": "",
            "DIRECTION": 0,
            "GROUP": "",
            "TEAM": "",
            "TEAM_TAG": "",
            "TEAM_CHANNELS": [],
            "INTERFACE": "",
            "ROAMING": 0,
            "RX_MODE": 12
        },
        "from": "system.adapter.hm-rega.0",
        "user": "system.user.admin",
        "ts": 1634815997638,
        "acl": {
            "object": 1636,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

Typ adapter
-----------

Eine ausführliche Dokumentation der Eigenschaften findest Du in hier: :ref:`development-iopackage`.

.. code:: console

    iobroker object get system.adapter.admin

.. code:: json

    {
        "_id": "system.adapter.admin",
        "type": "adapter",
        "common": {
            "name": "admin",
            "version": "5.2.3",
            "titleLang": {
                "en": "Admin",
                "de": "Admin",
                "ru": "Админ",
                "pt": "Admin",
                "pl": "Admin",
                "fr": "Admin",
                "nl": "Admin",
                "it": "Admin",
                "es": "Admin",
                "zh-cn": "Admin"
            },
            "title": "Admin",
            "connectionType": "local",
            "dataSource": "push",
            "news": {
                "5.2.0": {
                    "en": "Fix crash cases reported via sentry\nAdded support for multi-repositories",
                    "de": "Absturzfälle beheben, die über Wache gemeldet wurden\nUnterstützung für Multi-Repositorys hinzugefügt",
                    "ru": "Исправить случаи сбоев, о которых сообщалось через часового\nДобавлена поддержка мульти-репозиториев.",
                    "pt": "Corrigir casos de falha relatados via sentinela\nAdicionado suporte para multi-repositórios",
                    "nl": "Herstel crashgevallen gemeld via schildwacht\nOndersteuning toegevoegd voor meerdere opslagplaatsen",
                    "fr": "Correction des cas de plantage signalés via sentinelle\nAjout de la prise en charge des multi-dépôts",
                    "it": "Risolti i casi di crash segnalati tramite sentinella\nAggiunto supporto per multi-repository",
                    "es": "Solucionar casos de accidentes informados a través de centinelas\nSoporte agregado para repositorios múltiples",
                    "pl": "Napraw przypadki awarii zgłoszone przez Sentry\nDodano obsługę wielu repozytoriów",
                    "zh-cn": "修复通过哨兵报告的崩溃案例\n添加了对多存储库的支持"
                },
                "5.1.29": {
                    "en": "Fix crash cases reported via sentry\nAdded support for multi-repositories",
                    "de": "Absturzfälle beheben, die über Wache gemeldet wurden\nUnterstützung für Multi-Repositorys hinzugefügt",
                    "ru": "Исправить случаи сбоев, о которых сообщалось через часового\nДобавлена поддержка мульти-репозиториев.",
                    "pt": "Corrigir casos de falha relatados via sentinela\nAdicionado suporte para multi-repositórios",
                    "nl": "Herstel crashgevallen gemeld via schildwacht\nOndersteuning toegevoegd voor meerdere opslagplaatsen",
                    "fr": "Correction des cas de plantage signalés via sentinelle\nAjout de la prise en charge des multi-dépôts",
                    "it": "Risolti i casi di crash segnalati tramite sentinella\nAggiunto supporto per multi-repository",
                    "es": "Solucionar casos de accidentes informados a través de centinelas\nSoporte agregado para repositorios múltiples",
                    "pl": "Napraw przypadki awarii zgłoszone przez Sentry\nDodano obsługę wielu repozytoriów",
                    "zh-cn": "修复通过哨兵报告的崩溃案例\n添加了对多存储库的支持"
                },
                "5.1.28": {
                    "en": "Fixed discovery function\nFixed some GUI bugs",
                    "de": "Feste Erkennungsfunktion\nEinige GUI-Fehler behoben",
                    "ru": "Фиксированная функция обнаружения\nИсправлены некоторые ошибки графического интерфейса.",
                    "pt": "Função de descoberta corrigida\nCorrigidos alguns bugs de GUI",
                    "nl": "Vaste ontdekkingsfunctie\nEnkele GUI-bugs opgelost",
                    "fr": "Fonction de découverte fixe\nCorrection de quelques bugs de l'interface graphique",
                    "it": "Funzione di scoperta fissa\nRisolti alcuni bug della GUI",
                    "es": "Función de descubrimiento fijo\nSe corrigieron algunos errores de GUI",
                    "pl": "Naprawiono funkcję wykrywania\nNaprawiono kilka błędów GUI",
                    "zh-cn": "固定发现功能\n修复了一些 GUI 错误"
                }
            },
            "desc": {
                "en": "The configuration of ioBroker via Web-Interface",
                "de": "Die Konfiguration von ioBroker über das Web-Interface",
                "ru": "Конфигурация ioBroker через веб-интерфейс",
                "pt": "A configuração do ioBroker via Web-Interface",
                "fr": "La configuration de ioBroker via Web-Interface",
                "nl": "De configuratie van ioBroker via de webinterface",
                "it": "La configurazione di ioBroker tramite interfaccia Web",
                "zh-cn": "配置ioBroker的Web界面"
            },
            "docs": {
                "en": "docs/en/admin.md",
                "ru": "docs/ru/admin.md",
                "de": [
                    "docs/de/admin.md",
                    "docs/de/admin/tab-adapters.md",
                    "docs/de/admin/tab-instances.md",
                    "docs/de/admin/tab-objects.md",
                    "docs/de/admin/tab-states.md",
                    "docs/de/admin/tab-groups.md",
                    "docs/de/admin/tab-users.md",
                    "docs/de/admin/tab-events.md",
                    "docs/de/admin/tab-hosts.md",
                    "docs/de/admin/tab-enums.md",
                    "docs/de/admin/tab-log.md",
                    "docs/de/admin/tab-system.md"
                ],
                "pt": "docs/pt/admin.md",
                "nl": "docs/nl/admin.md",
                "es": "docs/es/admin.md",
                "fr": "docs/fr/admin.md",
                "it": "docs/it/admin.md",
                "pl": "docs/pl/admin.md",
                "zh-cn": "docs/zh-cn/admin.md"
            },
            "materialize": true,
            "mode": "daemon",
            "platform": "Javascript/Node.js",
            "loglevel": "info",
            "icon": "admin.png",
            "messagebox": true,
            "enabled": true,
            "extIcon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
            "keywords": [
                "setup",
                "config",
                "update",
                "upgrade",
                "system",
                "konfiguration",
                "administration",
                "einrichtung",
                "wartung"
            ],
            "compact": true,
            "readme": "https://github.com/ioBroker/ioBroker.admin/blob/master/README.md",
            "authors": [
                "bluefox <bluefox@ccu.io>",
                "hobbyquaker <hq@ccu.io>"
            ],
            "dependencies": [
                {
                    "js-controller": ">=3.3.22"
                }
            ],
            "type": "general",
            "license": "MIT",
            "logTransporter": true,
            "stopBeforeUpdate": true,
            "wwwDontUpload": true,
            "nogit": true,
            "welcomeScreenPro": {
                "link": "admin/index.html",
                "name": "Admin",
                "img": "admin/img/admin.png",
                "color": "pink",
                "order": 5,
                "localLinks": "_default",
                "localLink": true
            },
            "localLinks": {
                "_default": {
                    "link": "%protocol%://%bind%:%port%",
                    "pro": true
                }
            },
            "plugins": {
                "sentry": {
                    "dsn": "https://9d2aaf29332a4999b133c693f43203b9@sentry.iobroker.net/18"
                }
            },
            "jsonConfig": true,
            "adminUI": {
                "config": "json"
            },
            "installedFrom": "iobroker.admin@5.2.3",
            "installedVersion": "5.2.3"
        },
        "native": {
            "port": 8081,
            "auth": false,
            "secure": false,
            "bind": "0.0.0.0",
            "cache": false,
            "material": false,
            "autoUpdate": 24,
            "accessLimit": false,
            "accessApplyRights": false,
            "accessAllowedConfigs": [],
            "accessAllowedTabs": [],
            "certPublic": "",
            "certPrivate": "",
            "certChained": "",
            "ttl": 3600,
            "defaultUser": "admin",
            "tmpPath": "/tmp",
            "tmpPathAllow": false,
            "thresholdValue": 200,
            "leEnabled": false,
            "leUpdate": false,
            "leCheckPort": 80,
            "loginBackgroundColor": "",
            "loginBackgroundImage": false,
            "loginHideLogo": false,
            "loginMotto": ""
        },
        "from": "system.host.raspberrypi-iobroker.cli",
        "ts": 1641544852126,
        "protectedNative": [],
        "encryptedNative": [],
        "notifications": [],
        "instanceObjects": [
            {
                "_id": "info",
                "type": "channel",
                "common": {
                    "name": "Information"
                },
                "native": {}
            },
            {
                "_id": "",
                "type": "meta",
                "common": {
                    "name": "user files and images for background image",
                    "type": "meta.user"
                },
                "native": {}
            },
            {
                "_id": "info.connection",
                "type": "state",
                "common": {
                    "role": "indicator.connected",
                    "name": "Info about connected socket clients",
                    "type": "string",
                    "read": true,
                    "write": false,
                    "def": ""
                },
                "native": {}
            },
            {
                "_id": "info.newsFeed",
                "type": "state",
                "common": {
                    "name": "Last news feed as JSON",
                    "type": "json",
                    "read": true,
                    "write": false
                }
            },
            {
                "_id": "info.newsETag",
                "type": "state",
                "common": {
                    "name": "Etag for news feed",
                    "type": "string",
                    "read": true,
                    "write": false
                }
            },
            {
                "_id": "info.newsLastId",
                "type": "state",
                "common": {
                    "name": "last seen news ID",
                    "type": "string",
                    "read": true,
                    "write": false
                }
            },
            {
                "_id": "info.updatesList",
                "type": "state",
                "common": {
                    "role": "indicator.updates",
                    "name": "List of adapters to update",
                    "type": "string",
                    "read": true,
                    "write": false,
                    "def": ""
                },
                "native": {}
            }
        ],
        "objects": [],
        "acl": {
            "object": 1636,
            "state": 1636,
            "file": 1632,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

Typ instance
------------

Eine ausführliche Dokumentation der Eigenschaften findest Du in hier: :ref:`development-iopackage`.

.. code:: console

    iobroker object get system.adapter.admin.0

.. code:: json

    {
        "_id": "system.adapter.admin.0",
        "type": "instance",
        "common": {
            "name": "admin",
            "version": "5.2.3",
            "titleLang": {
                "en": "Admin",
                "de": "Admin",
                "ru": "Админ",
                "pt": "Admin",
                "pl": "Admin",
                "fr": "Admin",
                "nl": "Admin",
                "it": "Admin",
                "es": "Admin",
                "zh-cn": "Admin"
            },
            "title": "Admin",
            "connectionType": "local",
            "dataSource": "push",
            "news": {
                "5.2.0": {
                    "en": "Fix crash cases reported via sentry\nAdded support for multi-repositories",
                    "de": "Absturzfälle beheben, die über Wache gemeldet wurden\nUnterstützung für Multi-Repositorys hinzugefügt",
                    "ru": "Исправить случаи сбоев, о которых сообщалось через часового\nДобавлена поддержка мульти-репозиториев.",
                    "pt": "Corrigir casos de falha relatados via sentinela\nAdicionado suporte para multi-repositórios",
                    "nl": "Herstel crashgevallen gemeld via schildwacht\nOndersteuning toegevoegd voor meerdere opslagplaatsen",
                    "fr": "Correction des cas de plantage signalés via sentinelle\nAjout de la prise en charge des multi-dépôts",
                    "it": "Risolti i casi di crash segnalati tramite sentinella\nAggiunto supporto per multi-repository",
                    "es": "Solucionar casos de accidentes informados a través de centinelas\nSoporte agregado para repositorios múltiples",
                    "pl": "Napraw przypadki awarii zgłoszone przez Sentry\nDodano obsługę wielu repozytoriów",
                    "zh-cn": "修复通过哨兵报告的崩溃案例\n添加了对多存储库的支持"
                },
                "5.1.29": {
                    "en": "Fix crash cases reported via sentry\nAdded support for multi-repositories",
                    "de": "Absturzfälle beheben, die über Wache gemeldet wurden\nUnterstützung für Multi-Repositorys hinzugefügt",
                    "ru": "Исправить случаи сбоев, о которых сообщалось через часового\nДобавлена поддержка мульти-репозиториев.",
                    "pt": "Corrigir casos de falha relatados via sentinela\nAdicionado suporte para multi-repositórios",
                    "nl": "Herstel crashgevallen gemeld via schildwacht\nOndersteuning toegevoegd voor meerdere opslagplaatsen",
                    "fr": "Correction des cas de plantage signalés via sentinelle\nAjout de la prise en charge des multi-dépôts",
                    "it": "Risolti i casi di crash segnalati tramite sentinella\nAggiunto supporto per multi-repository",
                    "es": "Solucionar casos de accidentes informados a través de centinelas\nSoporte agregado para repositorios múltiples",
                    "pl": "Napraw przypadki awarii zgłoszone przez Sentry\nDodano obsługę wielu repozytoriów",
                    "zh-cn": "修复通过哨兵报告的崩溃案例\n添加了对多存储库的支持"
                },
                "5.1.28": {
                    "en": "Fixed discovery function\nFixed some GUI bugs",
                    "de": "Feste Erkennungsfunktion\nEinige GUI-Fehler behoben",
                    "ru": "Фиксированная функция обнаружения\nИсправлены некоторые ошибки графического интерфейса.",
                    "pt": "Função de descoberta corrigida\nCorrigidos alguns bugs de GUI",
                    "nl": "Vaste ontdekkingsfunctie\nEnkele GUI-bugs opgelost",
                    "fr": "Fonction de découverte fixe\nCorrection de quelques bugs de l'interface graphique",
                    "it": "Funzione di scoperta fissa\nRisolti alcuni bug della GUI",
                    "es": "Función de descubrimiento fijo\nSe corrigieron algunos errores de GUI",
                    "pl": "Naprawiono funkcję wykrywania\nNaprawiono kilka błędów GUI",
                    "zh-cn": "固定发现功能\n修复了一些 GUI 错误"
                }
            },
            "desc": {
                "en": "The configuration of ioBroker via Web-Interface",
                "de": "Die Konfiguration von ioBroker über das Web-Interface",
                "ru": "Конфигурация ioBroker через веб-интерфейс",
                "pt": "A configuração do ioBroker via Web-Interface",
                "fr": "La configuration de ioBroker via Web-Interface",
                "nl": "De configuratie van ioBroker via de webinterface",
                "it": "La configurazione di ioBroker tramite interfaccia Web",
                "zh-cn": "配置ioBroker的Web界面"
            },
            "docs": {
                "en": "docs/en/admin.md",
                "ru": "docs/ru/admin.md",
                "de": [
                    "docs/de/admin.md",
                    "docs/de/admin/tab-adapters.md",
                    "docs/de/admin/tab-instances.md",
                    "docs/de/admin/tab-objects.md",
                    "docs/de/admin/tab-states.md",
                    "docs/de/admin/tab-groups.md",
                    "docs/de/admin/tab-users.md",
                    "docs/de/admin/tab-events.md",
                    "docs/de/admin/tab-hosts.md",
                    "docs/de/admin/tab-enums.md",
                    "docs/de/admin/tab-log.md",
                    "docs/de/admin/tab-system.md"
                ],
                "pt": "docs/pt/admin.md",
                "nl": "docs/nl/admin.md",
                "es": "docs/es/admin.md",
                "fr": "docs/fr/admin.md",
                "it": "docs/it/admin.md",
                "pl": "docs/pl/admin.md",
                "zh-cn": "docs/zh-cn/admin.md"
            },
            "materialize": true,
            "mode": "daemon",
            "platform": "Javascript/Node.js",
            "loglevel": "info",
            "icon": "admin.png",
            "messagebox": true,
            "enabled": true,
            "extIcon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
            "keywords": [
                "setup",
                "config",
                "update",
                "upgrade",
                "system",
                "konfiguration",
                "administration",
                "einrichtung",
                "wartung"
            ],
            "compact": true,
            "readme": "https://github.com/ioBroker/ioBroker.admin/blob/master/README.md",
            "authors": [
                "bluefox <bluefox@ccu.io>",
                "hobbyquaker <hq@ccu.io>"
            ],
            "dependencies": [
                {
                    "js-controller": ">=3.3.22"
                }
            ],
            "type": "general",
            "license": "MIT",
            "logTransporter": true,
            "stopBeforeUpdate": true,
            "wwwDontUpload": true,
            "nogit": true,
            "welcomeScreenPro": {
                "link": "admin/index.html",
                "name": "Admin",
                "img": "admin/img/admin.png",
                "color": "pink",
                "order": 5,
                "localLinks": "_default",
                "localLink": true
            },
            "localLinks": {
                "_default": {
                    "link": "%protocol%://%bind%:%port%",
                    "pro": true
                }
            },
            "plugins": {
                "sentry": {
                    "dsn": "https://9d2aaf29332a4999b133c693f43203b9@sentry.iobroker.net/18"
                }
            },
            "jsonConfig": true,
            "adminUI": {
                "config": "json"
            },
            "installedVersion": "5.2.3",
            "host": "raspberrypi-iobroker",
            "logLevel": "info",
            "installedFrom": "iobroker.admin@5.2.3"
        },
        "native": {
            "port": 8081,
            "auth": true,
            "secure": false,
            "bind": "0.0.0.0",
            "cache": false,
            "material": false,
            "autoUpdate": 24,
            "accessLimit": false,
            "accessApplyRights": false,
            "accessAllowedConfigs": [],
            "accessAllowedTabs": [],
            "certPublic": "",
            "certPrivate": "",
            "certChained": "",
            "ttl": 36000,
            "defaultUser": "admin",
            "tmpPath": "/tmp",
            "tmpPathAllow": false,
            "thresholdValue": 200,
            "leEnabled": false,
            "leUpdate": false,
            "leCheckPort": 80,
            "loginBackgroundColor": "",
            "loginBackgroundImage": false,
            "loginHideLogo": false,
            "loginMotto": ""
        },
        "protectedNative": [],
        "encryptedNative": [],
        "notifications": [],
        "instanceObjects": [
            {
                "_id": "info",
                "type": "channel",
                "common": {
                    "name": "Information"
                },
                "native": {}
            },
            {
                "_id": "",
                "type": "meta",
                "common": {
                    "name": "user files and images for background image",
                    "type": "meta.user"
                },
                "native": {}
            },
            {
                "_id": "info.connection",
                "type": "state",
                "common": {
                    "role": "indicator.connected",
                    "name": "Info about connected socket clients",
                    "type": "string",
                    "read": true,
                    "write": false,
                    "def": ""
                },
                "native": {}
            },
            {
                "_id": "info.newsFeed",
                "type": "state",
                "common": {
                    "name": "Last news feed as JSON",
                    "type": "json",
                    "read": true,
                    "write": false
                }
            },
            {
                "_id": "info.newsETag",
                "type": "state",
                "common": {
                    "name": "Etag for news feed",
                    "type": "string",
                    "read": true,
                    "write": false
                }
            },
            {
                "_id": "info.newsLastId",
                "type": "state",
                "common": {
                    "name": "last seen news ID",
                    "type": "string",
                    "read": true,
                    "write": false
                }
            },
            {
                "_id": "info.updatesList",
                "type": "state",
                "common": {
                    "role": "indicator.updates",
                    "name": "List of adapters to update",
                    "type": "string",
                    "read": true,
                    "write": false,
                    "def": ""
                },
                "native": {}
            }
        ],
        "objects": [],
        "acl": {
            "object": 1636,
            "state": 1636,
            "file": 1632,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        },
        "from": "system.adapter.admin.0",
        "user": "system.user.admin",
        "ts": 1641911437382
    }

Typ script
----------

.. code:: console

    iobroker object get script.js.example_script

.. code:: json

    {
        "_id": "script.js.example_script",
        "type": "script",
        "common": {
            "name": "example_script",
            "expert": true,
            "engineType": "Blockly",
            "engine": "system.adapter.javascript.0",
            "source": "console.log('Beispiel-Code');\n\n//JTNDeG1sJTIweG1sbnMlM0QlMjJodHRwcyUzQSUyRiUyRmRldmVsb3BlcnMuZ29vZ2xlLmNvbSUyRmJsb2NrbHklMkZ4bWwlMjIlM0UlM0NibG9jayUyMHR5cGUlM0QlMjJkZWJ1ZyUyMiUyMGlkJTNEJTIydDFZbn5vJTdCbUpGJTYwSkZHVVkqJTNBIUMlMjIlMjB4JTNEJTIyNjMlMjIlMjB5JTNEJTIyMzYzJTIyJTNFJTNDZmllbGQlMjBuYW1lJTNEJTIyU2V2ZXJpdHklMjIlM0Vsb2clM0MlMkZmaWVsZCUzRSUzQ3ZhbHVlJTIwbmFtZSUzRCUyMlRFWFQlMjIlM0UlM0NzaGFkb3clMjB0eXBlJTNEJTIydGV4dCUyMiUyMGlkJTNEJTIybmMlN0N5JTJGYi0lM0ItQzRqbDA0JTdCJTNEJTVCRVIlMjIlM0UlM0NmaWVsZCUyMG5hbWUlM0QlMjJURVhUJTIyJTNFdGVzdCUzQyUyRmZpZWxkJTNFJTNDJTJGc2hhZG93JTNFJTNDYmxvY2slMjB0eXBlJTNEJTIydGV4dCUyMiUyMGlkJTNEJTIyOCkuJTJGJTNBQXpxa0p4TUclNUU3amVLfnglMjIlM0UlM0NmaWVsZCUyMG5hbWUlM0QlMjJURVhUJTIyJTNFQmVpc3BpZWwtQ29kZSUzQyUyRmZpZWxkJTNFJTNDJTJGYmxvY2slM0UlM0MlMkZ2YWx1ZSUzRSUzQyUyRmJsb2NrJTNFJTNDJTJGeG1sJTNF",
            "debug": false,
            "verbose": false,
            "enabled": false
        },
        "acl": {
            "object": 1636,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        },
        "from": "system.adapter.admin.0",
        "user": "system.user.admin",
        "ts": 1644519738959
    }

Typ config
----------

.. code:: console

    iobroker object get system.config

.. code:: json

    {
        "_id": "system.config",
        "type": "config",
        "common": {
            "name": {
                "en": "System configuration",
                "de": "Systemkonfiguration",
                "ru": "Конфигурация системы",
                "pt": "Configuração do sistema",
                "nl": "Systeem configuratie",
                "fr": "Configuration du système",
                "it": "Configurazione di sistema",
                "es": "Configuración del sistema",
                "pl": "Konfiguracja systemu",
                "zh-cn": "系统配置"
            },
            "city": "Custom City",
            "country": "Germany",
            "longitude": 8.111,
            "latitude": 51.111,
            "language": "de",
            "tempUnit": "°C",
            "currency": "€",
            "dontDelete": true,
            "dateFormat": "DD.MM.YYYY",
            "isFloatComma": true,
            "licenseConfirmed": true,
            "defaultHistory": "",
            "expertMode": false,
            "defaultLogLevel": "info",
            "activeRepo": "stable",
            "diag": "extended",
            "tabs": [
                "tab-intro",
                "tab-info",
                "tab-adapters",
                "tab-instances",
                "tab-objects",
                "tab-log",
                "tab-scenes",
                "tab-javascript",
                "tab-text2command-0",
                "tab-node-red-0"
            ],
            "tabsVisible": [
                {
                    "name": "tab-intro",
                    "visible": true
                },
                {
                    "name": "tab-adapters",
                    "visible": true
                },
                {
                    "name": "tab-instances",
                    "visible": true
                },
                {
                    "name": "tab-objects",
                    "visible": true
                },
                {
                    "name": "tab-enums",
                    "visible": true
                },
                {
                    "name": "tab-logs",
                    "visible": true
                },
                {
                    "name": "tab-users",
                    "visible": true
                },
                {
                    "name": "tab-hosts",
                    "visible": true
                },
                {
                    "name": "tab-files",
                    "visible": true
                },
                {
                    "name": "tab-backitup-0",
                    "visible": true
                }
            ],
            "defaultNewAcl": {
                "object": 1636,
                "state": 1636,
                "file": 1632,
                "owner": "system.user.admin",
                "ownerGroup": "system.group.administrator"
            }
        },
        "native": {
            "secret": "971640e8df0885faf7d49c90e38423fc65425b2b861d5e7b"
        },
        "acl": {
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator",
            "object": 1604
        },
        "from": "system.adapter.admin.0",
        "user": "system.user.admin",
        "ts": 1633096344214
    }

Typ host
--------

.. code:: console

    iobroker object get system.host.raspberrypi-iobroker

.. code:: json

    {
        "_id": "system.host.raspberrypi-iobroker",
        "type": "host",
        "common": {
            "name": "raspberrypi-iobroker",
            "title": "JS controller",
            "installedVersion": "3.3.18",
            "platform": "Javascript/Node.js",
            "cmd": "/usr/bin/node  /opt/iobroker/node_modules/iobroker.js-controller/controller.js",
            "hostname": "raspberrypi-iobroker",
            "address": [
                "172.16.0.120",
                "fe80::46f4:a0bb:45c7:6fd7"
            ],
            "type": "js-controller"
        },
        "native": {
            "process": {
                "title": "iobroker.js-controller",
                "versions": {
                    "node": "12.22.6",
                    "v8": "7.8.279.23-node.56",
                    "uv": "1.40.0",
                    "zlib": "1.2.11",
                    "brotli": "1.0.9",
                    "ares": "1.17.2",
                    "modules": "72",
                    "nghttp2": "1.41.0",
                    "napi": "8",
                    "llhttp": "2.1.3",
                    "http_parser": "2.9.4",
                    "openssl": "1.1.1l",
                    "cldr": "37.0",
                    "icu": "67.1",
                    "tz": "2019c",
                    "unicode": "13.0"
                },
                "env": {
                    "NODE": "$(which node)",
                    "PWD": "/",
                    "LOGNAME": "iobroker",
                    "HOME": "/home/iobroker",
                    "LANG": "de_DE.UTF-8",
                    "INVOCATION_ID": "82481d3eabae4b618e7be1b24552c984",
                    "USER": "iobroker",
                    "SHLVL": "0",
                    "JOURNAL_STREAM": "8:21058",
                    "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                    "_": "/usr/bin/node"
                }
            },
            "os": {
                "hostname": "raspberrypi-iobroker",
                "type": "Linux",
                "platform": "linux",
                "arch": "arm",
                "release": "5.10.63-v7l+",
                "endianness": "LE",
                "tmpdir": "/tmp"
            },
            "hardware": {
                "cpus": [
                    {
                        "model": "ARMv7 Processor rev 3 (v7l)",
                        "speed": 1500
                    },
                    {
                        "model": "ARMv7 Processor rev 3 (v7l)",
                        "speed": 1500
                    },
                    {
                        "model": "ARMv7 Processor rev 3 (v7l)",
                        "speed": 1500
                    },
                    {
                        "model": "ARMv7 Processor rev 3 (v7l)",
                        "speed": 1500
                    }
                ],
                "totalmem": 4025200640,
                "networkInterfaces": {
                    "lo": [
                        {
                            "address": "127.0.0.1",
                            "netmask": "255.0.0.0",
                            "family": "IPv4",
                            "mac": "00:00:00:00:00:00",
                            "internal": true,
                            "cidr": "127.0.0.1/8"
                        },
                        {
                            "address": "::1",
                            "netmask": "ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff",
                            "family": "IPv6",
                            "mac": "00:00:00:00:00:00",
                            "internal": true,
                            "cidr": "::1/128",
                            "scopeid": 0
                        }
                    ],
                    "eth0": [
                        {
                            "address": "172.16.0.120",
                            "netmask": "255.255.0.0",
                            "family": "IPv4",
                            "mac": "e4:5f:01:5d:01:31",
                            "internal": false,
                            "cidr": "172.16.0.120/16"
                        },
                        {
                            "address": "fe80::46f4:a0bb:45c7:6fd7",
                            "netmask": "ffff:ffff:ffff:ffff::",
                            "family": "IPv6",
                            "mac": "e4:5f:01:5d:01:31",
                            "internal": false,
                            "cidr": "fe80::46f4:a0bb:45c7:6fd7/64",
                            "scopeid": 2
                        }
                    ]
                }
            }
        },
        "acl": {
            "object": 1636,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        },
        "from": "system.host.raspberrypi-iobroker",
        "ts": 1633374149865
    }

Typ script
----------

.. code:: console

    iobroker object get script.js.Büro.Licht_einschalten

.. code:: json

    {
        "_id": "script.js.Büro.Licht_einschalten",
        "type": "script",
        "common": {
            "name": "Licht einschalten",
            "expert": true,
            "engineType": "Blockly",
            "engine": "system.adapter.javascript.0",
            "source": "on({id: \"zigbee.0.00158d00020f4ab5.click\"...",
            "debug": false,
            "verbose": false,
            "enabled": true
        },
        "acl": {
            "object": 1636,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        },
        "from": "system.adapter.admin.0",
        "user": "system.user.admin",
        "ts": 1628941638315
    }

Typ user
--------

.. code:: console

    iobroker object get system.user.admin

.. code:: json

    {
        "_id": "system.user.admin",
        "type": "user",
        "common": {
            "name": "Matthias Kleine",
            "password": "pbkdf2$10000$021943a847a4e2c20b...",
            "dontDelete": true,
            "enabled": true
        },
        "native": {},
        "acl": {
            "object": 1636,
            "state": 1636,
            "file": 1632,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        },
        "enums": {},
        "from": "system.adapter.admin.0",
        "user": "system.user.admin",
        "ts": 1633095538813
    }

Typ group
---------

.. code:: console

    iobroker object get system.group.administrator

.. code:: json

    {
        "_id": "system.group.administrator",
        "type": "group",
        "common": {
            "icon": "data:image/svg+xml;base64,PHN2...",
            "name": {
                "en": "Administrator",
                "de": "Administrator"
            },
            "description": {
                "en": "Can do everything with System",
                "de": "Darf alles mit dem System machen"
            },
            "members": [
                "system.user.admin"
            ],
            "dontDelete": true,
            "acl": {
                "object": {
                    "list": true,
                    "read": true,
                    "write": true,
                    "delete": true
                },
                "state": {
                    "list": true,
                    "read": true,
                    "write": true,
                    "create": true,
                    "delete": true
                },
                "users": {
                    "list": true,
                    "read": true,
                    "write": true,
                    "create": true,
                    "delete": true
                },
                "other": {
                    "execute": true,
                    "http": true,
                    "sendto": true
                },
                "file": {
                    "list": true,
                    "read": true,
                    "write": true,
                    "create": true,
                    "delete": true
                }
            }
        },
        "acl": {
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator",
            "object": 1604
        },
        "from": "system.host.raspberrypi-iobroker.cli",
        "ts": 1633092016342
    }

Typ folder
----------

.. code:: console

    iobroker object get system.host.raspberrypi-iobroker.notifications

.. code:: json

    {
        "_id": "system.host.raspberrypi-iobroker.notifications",
        "type": "folder",
        "common": {
            "name": {
                "en": "Notifications",
                "de": "Benachrichtigungen"
            }
        },
        "native": {},
        "acl": {
            "object": 1636,
            "state": 1636,
            "file": 1632,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

Typ meta
--------

.. confval:: common.type

    - ``meta.user``
    - ``meta.folder``
    - ``uuid``

    :type: string
    :default: ``mixed``

.. code:: console

    iobroker object get system.meta.uuid

.. code:: json

    {
        "_id": "system.meta.uuid",
        "type": "meta",
        "common": {
            "name": "uuid",
            "type": "uuid"
        },
        "native": {
            "uuid": "23b1992b-8d91-a4fc-b201-2bd851bdc807"
        },
        "acl": {
            "object": 1636,
            "state": 1636,
            "file": 1632,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        },
        "ts": 1633092016485,
        "from": "system.host.raspberrypi-iobroker.tools"
    }

Typ enum
--------

.. code:: console

    iobroker object get enum.rooms

.. code:: json

    {
        "_id": "enum.rooms",
        "type": "enum",
        "common": {
            "icon": "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGhlaWdodD0iMjRweCIgdmlld0JveD0iMCAwIDI0IDI0IiB3aWR0aD0iMjRweCI+DQogICAgPHBhdGggZmlsbD0iY3VycmVudENvbG9yIiBkPSJNMTIgNS42OWw1IDQuNVYxOGgtMnYtNkg5djZIN3YtNy44MWw1LTQuNU0xMiAzTDIgMTJoM3Y4aDZ2LTZoMnY2aDZ2LThoM0wxMiAzeiIvPg0KPC9zdmc+",
            "name": {
                "en": "Rooms",
                "de": "Räume",
                "ru": "Комнаты",
                "pt": "Quartos",
                "nl": "Kamers",
                "fr": "Pièces",
                "it": "Camere",
                "es": "Habitaciones",
                "pl": "Pokoje",
                "zh-cn": "客房"
            },
            "desc": {
                "en": "List of the rooms",
                "de": "Liste der Räume",
                "ru": "Список комнат",
                "pt": "Lista dos quartos",
                "nl": "Lijst met kamers",
                "fr": "Liste des chambres",
                "it": "Elenco delle stanze",
                "es": "Lista de las habitaciones",
                "pl": "Lista pokoi",
                "zh-cn": "房间清单"
            },
            "members": [],
            "dontDelete": true
        },
        "acl": {
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator",
            "object": 1911
        },
        "from": "system.host.raspberrypi-iobroker.cli",
        "ts": 1633092016282
    }