.. _basics-cli:

CLI (Command Line Interface)
============================

Das Command Line Interface von ioBroker ist sehr mächtig. Hierüber kann im Prinzip alles gesteuert werden, was über den ioBroker Admin als Weboberfläche auch möglich ist - und noch mehr. Es können also zum Beispiel neue Adapter installiert oder aktualisiert werden, Backups verwaltet werden, Dateirechte angepasst werden und vieles mehr!

.. code:: console

    iobroker [command]

    Commands:
        iobroker setup                                               Setup ioBroker
        iobroker start [all|<adapter>.<instance>]                    Starts the js-controller or a specified adapter instance
        iobroker stop [<adapter>.<instance>]                         stops the js-controller or a specified adapter instance
        iobroker restart [<adapter>.<instance>]                      Restarts js-controller or a specified adapter instance  [aliases: r]
        iobroker debug <adapter>[.<instance>]                        Starts a Node.js debugging session for the adapter instance
        iobroker info                                                Shows the host info
        iobroker logs [<adapter>]                                    Monitor log
        iobroker add <adapter> [desiredNumber]                       Add instance of adapter  [aliases: a]
        iobroker install <adapter>                                   Installs a specified adapter  [aliases: i]
        iobroker rebuild [<path>]                                    Rebuild all native modules or path
        iobroker url <url> [<name>]                                  Install adapter from specified url, e.g. GitHub
        iobroker del <adapter>                                       Remove adapter and all instances from this host  [aliases: delete]
        iobroker del <adapter>.<instance>                            Remove adapter instance  [aliases: delete]
        iobroker update [<repositoryUrl>]                            Update repository and list adapters
        iobroker upgrade                                             Upgrade management
        iobroker upload [all|<adapter>]                              Upload management  [aliases: u]
        iobroker object                                              Object management  [aliases: o]
        iobroker state                                               State management  [aliases: s]
        iobroker message <adapter>[.instance] <command> [<message>]  Send message to adapter instance/s
        iobroker list <type> [<filter>]                              List all entries, like objects
        iobroker chmod <mode> <file>                                 Change file rights
        iobroker chown <user> <group> <file>                         Change file ownership
        iobroker touch <file>                                        Touch file
        iobroker rm <file>                                           Remove file
        iobroker file                                                File management
        iobroker user                                                User commands
        iobroker group                                               group management
        iobroker host <hostname>                                     Set host to given hostname
        iobroker set <adapter>.<instance>                            Change settings of adapter config
        iobroker license <license.file or license.text>              Update license by given file
        iobroker cert                                                Certificate management
        iobroker clean <yes>                                         Clears all objects and states
        iobroker backup                                              Create backup
        iobroker restore <backup name or path>                       Restore a specified backup
        iobroker validate <backup name or path>                      Validate a specified backup
        iobroker status [all|<adapter>.<instance>]                   Status of ioBroker or adapter instance  [aliases: isrun]
        iobroker repo [<name>]                                       Show repo information
        iobroker uuid                                                Show uuid of the installation  [aliases: id]
        iobroker unsetup                                             Reset license, installation secret and language
        iobroker fix                                                 Execute the installation fixer script, this updates your ioBroker installation
        iobroker multihost                                           Multihost management
        iobroker compact                                             compact group management
        iobroker plugin                                              Plugin management
        iobroker version [<adapter>]                                 Show version of js-controller or specified adapter  [aliases: v]

Weiterhin ist es möglich, zu (fast) jedem Befehl weitere Hilfen aufzurufen. Dafür wird einfach die Option ``--help`` angehängt:

.. code:: console

    iobroker user --help

    Commands:
        iobroker user add <user>      Add new user
        iobroker user del <user>      Delete user
        iobroker user passwd <user>   Change user password
        iobroker user enable <user>   Enable user
        iobroker user disable <user>  Disable user
        iobroker user get <user>      Get user
        iobroker user check <user>    Check user password

    Options:
        --help     Show help  [boolean]

Beispiele (System)
------------------

- ``iobroker start``: Startet das gesamte ioBroker-System (``js-controller`` und alle Instanzen)
- ``iobroker stop``: Stoppt das gesamte ioBroker-System (``js-controller`` und alle Instanzen)
- ``iobroker version``: Version des ``js-controller``
- ``iobroker uuid``: UUID des Systems - siehe :ref:`basics-uuid`
- ``iobroker upgrade self``: Aktualisiert den ``js-controller``
- ``iobroker host this``: Initialisiert den aktuellen Host. Sollte aufgerufen werden, wenn z.B. der Hostname des Systems geändert wurde

Beispiele (Backup)
------------------

- ``iobroker backup``: Erstellt ein neues Backup anhand der Konfiguration im ``backitup`` Adapter

Beispiele (Adapter)
-------------------

**Updates und Upgrades**

- ``iobroker update``: Aktualisiert das aktive Repository. Es werden keine Adapter aktualisiert. Siehe :ref:`ecosystem-repositories`
- ``iobroker update -u``: Listet alle Adapter mit verfügbaren Updates auf
- ``iobroker update -a``: Listet alle verfügbaren Adapter auf
- ``iobroker upgrade --yes``: Aktualisiert alle Adapter, ohne für jeden Adapter nachzufragen, ob die Version wirklich installiert werden soll.
- ``iobroker upgrade ical``: Aktualisiert einen einzelnen Adpater (in diesem Fall ical) auf die aktuellste Version
- ``iobroker upgrade ical@1.11.2``: Aktualisiert einen einzelnen Adpater (in diesem Fall ical) auf die Version ``1.11.2``
- ``iobroker version iot``: Version des installierten IoT Adapters

**Adapter und Instanzen verwalten**

- ``iobroker list adapters``: Listet alle installierten Adapter und ihre Versionen auf
- ``iobroker list instances``: Listet alle Instanzen auf und zeigt ob diese gerade laufen
- ``iobroker add wled@0.6.0``: Installiert die Version ``0.6.0`` des WLED Adapters und erstellt eine neue Instanz
- ``iobroker add wled``: Installiert die aktuellste Version des WLED Adapters und erstellt eine neue Instanz
- ``iobroker del wled.0``: Löscht die Instanz ``wled.0``
- ``iobroker del wled``: Deinstalliert den WLED Adapter
- ``iobroker status iot.0``: Prüft, ob eine Instanz des IoT Adapters läuft

Beispiele (User)
----------------

- ``iobroker list users``: Listet alle existierenden Benutzer auf
- ``iobroker user passwd admin``: Passwort vom User ``admin`` ändern (z.B. falls es vergessen wurde)

Beispiele (Objekte)
-------------------

- ``iobroker object get system.config``: Liefert den Inhalt des Objektes - siehe :ref:`basics-datastorage`

Beispiele (Zustände)
--------------------

- ``iobroker state get admin.0.info.updatesNumber``: Liefert den kompletten Zustand als JSON
- ``iobroker state getvalue admin.0.info.updatesNumber``: Liefert nur den Wert des Zustandes - siehe :ref:`basics-datastorage`
