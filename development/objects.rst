.. _development-objects:

Objekte
=======

.. note::
    Lies zuerst die Grundlagen zur :ref:`basics-datastorage`.

Objekte können beispielsweise über das :ref:`basics-cli` ausgelesen werden:

.. code:: console

    iobroker state get hue.0.Deckenlampe.bri

Beispiel
--------

.. code:: json

    {
        "_id": "hue.0.Deckenlampe.bri",
        "type": "state",
        "common": {
            "name": "Deckenlampe.bri",
            "read": true,
            "write": true,
            "type": "number",
            "role": "level.dimmer",
            "min": 0,
            "max": 254,
            "def": 254
        },
        "native": {
            "id": "3"
        },
        "from": "system.adapter.hue.0",
        "user": "system.user.admin",
        "ts": 1604080553077,
        "acl": {
            "object": 1636,
            "state": 1636,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

Für Objekte sind die folgenden Attribute verpflichtend:

- ``_id`` - Eindeutige ID
- ``type`` - Typ des Objektes (siehe unten).
- ``common`` - ioBroker-Spezifische Eigenschaften (Rollen, Lesezugriff, Schreibzugriff, ...)
- ``native`` - Eigenschaften des Zielsystems (z.B. eine ID eines Gerätes)

Verfügbaren Typen
-----------------

- ``state`` - Zustand. Das übergeornete Objekte sollte vom Typ ``channel``, ``device``, ``instance`` oder ``host`` sein.
- ``channel`` - "Kanal" um mehrere Zustände darunter zu strukturieren. Das übergeornete Objekte sollte vom Typ ``device`` sein.
- ``device`` - "Gerät" um mehrere Zustände oder Kanäle darunter zu strukturieren. Das übergeornete Objekte sollte vom Typ ``instance`` sein.
- ``enum`` - "Liste" mit vordefinierten Werten in ``common.members``. that points to the states, channels, devices or files.
- ``host`` - Ein "Host", welcher einen ``js-controller`` Prozess ausführt. Beispielsweise ``system.host.raspberrypi-iobroker``.
- ``adapter`` - Die Standard-Konfiguration von einem Adaper. Beispielsweise ``system.adapter.admin``.
- ``instance`` - Die Konfiguration der einzelnen Instanz. Beispielsweise ``system.adapter.admin.0``. Das übergeornete Objekte sollte vom Typ ``adapter`` sein.
- ``meta`` - Sich selten ändernde Meta-Informationen wie zum Beispiel die :ref:`basics-uuid` unter ``system.meta.uuid``.
- ``config`` - Konfigurationen. Beispielsweise ``system.repositories``
- ``script`` - Skripte unter ``script.js.*``
- ``user`` - Benutzer des Systems. Beispielsweise ``system.user.admin``
- ``group`` - Benutzer-Gruppen des Sytems. Beispielsweise ``system.group.administrator``
- ``chart`` - Diagramm
- ``folder`` - Verzeichnis. Beispielsweise ``system.host.raspberrypi-iobroker.notifications``

Common-Eigenschaften
--------------------

TODO

Typ Host (Beispiel)
-------------------

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
        "from": "system.host.raspberrypi-iobroker",
        "ts": 1633374149865,
        "acl": {
            "object": 1636,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

Typ Script (Beispiel)
---------------------

.. code:: console

    iobroker object get script.js.Büro.Licht_einschalten

.. code:: json

    {
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
        "type": "script",
        "from": "system.adapter.admin.0",
        "user": "system.user.admin",
        "ts": 1628941638315,
        "_id": "script.js.Büro.Licht_einschalten",
        "acl": {
            "object": 1636,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

Typ User (Beispiel)
-------------------

.. code:: console

    iobroker object get system.user.admin

.. code:: json

    {
        "type": "user",
        "common": {
            "name": "Matthias Kleine",
            "password": "pbkdf2$10000$021943a847a4e2c20b...",
            "dontDelete": true,
            "enabled": true
        },
        "native": {},
        "_id": "system.user.admin",
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

Typ Group (Beispiel)
--------------------

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

Typ Folder (Beispiel)
-------------------

.. code:: console

    iobroker object get system.host.raspberrypi-iobroker.notifications

.. code:: json

    {
        "type": "folder",
        "common": {
            "name": {
                "en": "Notifications",
                "de": "Benachrichtigungen"
            }
        },
        "native": {},
        "_id": "system.host.raspberrypi-iobroker.notifications",
        "acl": {
            "object": 1636,
            "state": 1636,
            "file": 1632,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }
