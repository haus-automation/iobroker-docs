.. _basics-cli:

CLI (Command Line Interface)
============================

Das Command Line Interface von ioBroker ist sehr mächtig. Hierüber kann im Prinzip alles gesteuert werden, was über den ioBroker Admin als Weboberfläche auch möglich ist - und noch mehr. Es können also zum Beispiel neue Adapter installiert oder aktualisiert werden, Backups verwaltet werden, Dateirechte angepasst werden und vieles mehr!

.. code::

    iobroker [command]

    Commands:
    iobroker setup                                               Setup ioBroker
    iobroker start                                               Starts the js-controller
    iobroker stop                                                stops the js-controller
    iobroker restart                                             Restarts js-controller
    iobroker debug <adapter>[.<instance>]                        Starts a Node.js debugging session for the adapter instance
    iobroker info                                                Shows the host info
    iobroker logs [<adapter>]                                    Monitor log
    iobroker add <adapter> [desiredNumber]                       Add instance of adapter
    iobroker install <adapter>                                   Installs a specified adapter
    iobroker rebuild <adapter>|self                              Rebuilds a specified adapter
    iobroker url <url> [<name>]                                  Install adapter from specified url, e.g. GitHub
    iobroker del <adapter>                                       Remove adapter from system
    iobroker del <adapter>.<instance>                            Remove adapter instance
    iobroker update [<repositoryUrl>]                            Update repository and list adapters
    iobroker upgrade                                             Upgrade management
    iobroker upload                                              Upload management
    iobroker object                                              Object management
    iobroker state                                               State management
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
    iobroker status [all|<adapter>.<instance>]                   Status of ioBroker or adapter instance
    iobroker repo [<name>]                                       Show repo information
    iobroker uuid                                                Show uuid of the installation
    iobroker unsetup                                             Reset license, installation secret and language
    iobroker fix                                                 Execute the installation fixer script, this updates your ioBroker installation
    iobroker multihost                                           Multihost management
    iobroker compact                                             compact group management
    iobroker plugin                                              Plugin management
    iobroker version [<adapter>]                                 Show version of js-controller or specified adapter

Beispiele
---------

- ``iobroker upgrade --yes``: Aktualisiert alle Adapter, ohne für jeden Adapter nachzufragen, ob die Version wirklich installiert werden soll.
- ``iobroker upgrade ical@1.11.2``: Aktualisiert einen einzelnen Adpater auf die angegebene Version
