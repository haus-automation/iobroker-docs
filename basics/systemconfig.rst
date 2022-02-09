.. _basics-systemconfig:

System-Config
=============

Die aktuelle System-Konfiguration (welche auch über den Admin konfigurierbar ist), wird im Objekt ``system.config`` (siehe :ref:`development-objects`) gespeichert.

Die Grundkonfiguration des Systems erfolgt allerdings nicht als Objekt, sondern über eine Datei auf der Festplatte: Diese findest Du unter ``/opt/iobroker/iobroker-data/iobroker.json``. Beispiel:

.. code:: json

    {
        "system": {
            "memoryLimitMB": 0,
            "hostname": "",
            "statisticsInterval": 15000,
            "statisticsIntervalComment": "Interval how often the counters for input/output in adapters and controller will be updated",
            "checkDiskInterval": 300000,
            "checkDiskIntervalComment": "Interval how often the disk size will be checked",
            "noChmod": false,
            "instanceStartInterval": 2000,
            "noChmodComment": "Flag to test new feature with no chmod call. Must be deleted later and noChmod must be mainline (2018.06.04)",
            "compact": false,
            "compactComment": "Controller will try to start the instances as a part of the same process. No spawn will be done. Only by adapters that support it and have flag compact flag in io-package.json",
            "allowShellCommands": false,
            "allowShellCommandsComment": "Allow execution of \"shell\" sendToHost commands",
            "memLimitWarn": 100,
            "memLimitWarnComment": "If the available RAM is below this threshold on adapter start, a warning will be logged.",
            "memLimitError": 50,
            "memLimitErrorComment": "If the available RAM is below this threshold on adapter start, an error will be logged."
        },
        "multihostService": {
            "enabled": false,
            "secure": true
        },
        "network": {
            "IPv4": true,
            "IPv6": true,
            "bindAddress": null
        },
        "objects": {
            "type": "file",
            "typeComment": "Possible values: 'file' - [port 9001], redis - [port 6379], couch - [port 5984].",
            "host": "127.0.0.1",
            "port": 9001,
            "user": "",
            "pass": "",
            "noFileCache": false,
            "connectTimeout": 2000,
            "writeFileInterval": 5000,
            "options": {
                "auth_pass": null,
                "retry_max_delay": 5000
            },
            "backup": {
                "disabled": false,
                "files": 24,
                "filesComment": "Minimal number of backup files, after the deletion will be executed according to backupTime settings",
                "hours": 48,
                "hoursComment": "All backups older than 48 hours will be deleted. But only if the number of files is greater than of backupNumber",
                "period": 120,
                "periodComment": "by default backup every 2 hours. Time is in minutes. To disable backup set the value to 0",
                "path": "",
                "pathComment": "Absolute path to backup directory or empty to backup in data directory"
            },
            "dataDir": "../../iobroker-data/"
        },
        "states": {
            "type": "redis",
            "typeComment": "Possible values: 'file' - [port 9000], 'redis' - [port 6379].",
            "host": "127.0.0.1",
            "port": 6379,
            "maxQueue": 1000,
            "user": "",
            "pass": "",
            "connectTimeout": 2000,
            "writeFileInterval": 30000,
            "options": {
                "auth_pass": null,
                "retry_max_delay": 5000
            },
            "backup": {
                "disabled": false,
                "files": 24,
                "filesComment": "Minimal number of backup files, after the deletion will be executed according to backupTime settings",
                "hours": 48,
                "hoursComment": "All backups older than 48 hours will be deleted. But only if the number of files is greater than of backupNumber",
                "period": 120,
                "periodComment": "by default backup every 2 hours. Time is in minutes. To disable backup set the value to 0",
                "path": "",
                "pathComment": "Absolute path to backup directory or empty to backup in data directory"
            }
        },
        "log": {
            "level": "info",
            "maxDays": 7,
            "noStdout": true,
            "transport": {
                "file1": {
                    "type": "file",
                    "enabled": true,
                    "filename": "log/iobroker",
                    "fileext": ".log",
                    "maxSize": null,
                    "maxFiles": null
                },
                "syslog1": {
                    "type": "syslog",
                    "enabled": false,
                    "host": "localhost",
                    "hostComment": "The host running syslogd, defaults to localhost.",
                    "portComment": "The port on the host that syslog is running on, defaults to syslogd's default port(514/UDP).",
                    "protocol": "udp4",
                    "protocolComment": "The network protocol to log over (e.g. tcp4, udp4, unix, unix-connect, etc).",
                    "pathComment": "The path to the syslog dgram socket (i.e. /dev/log or /var/run/syslog for OS X).",
                    "facilityComment": "Syslog facility to use (Default: local0).",
                    "localhost": "iobroker",
                    "localhostComment": "Host to indicate that log messages are coming from (Default: localhost).",
                    "sysLogTypeComment": "The type of the syslog protocol to use (Default: BSD).",
                    "app_nameComment": "The name of the application (Default: process.title).",
                    "eolComment": "The end of line character to be added to the end of the message (Default: Message without modifications)."
                }
            }
        },
        "dataDirComment": "Always relative to iobroker.js-controller/",
        "plugins": {},
        "dataDir": "../../iobroker-data/"
    }