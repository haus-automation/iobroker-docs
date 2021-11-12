.. _basics-systemconfig:

System-Config
=============

Die aktuelle System-Konfiguration (welche auch über den Admin konfigurierbar ist), wird im Objekt ``system.config`` gespeichert. Dieses kann beispielsweise über das :ref:`basics-cli` ausgelesen werden:

.. code:: console

    iobroker object get system.config

Beispiel-Ausgabe:

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
        "acl": {
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator",
            "object": 1604
        },
        "native": {
            "secret": "971640e8df0885faf7d49c90e38423fc65425b2b861d5e7b"
        },
        "from": "system.adapter.admin.0",
        "user": "system.user.admin",
        "ts": 1633096344214
    }

