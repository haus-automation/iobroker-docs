Willkommen
==========

.. tip::
    Diese Dokumentation ist Open Source und auf `GitHub zu finden <https://github.com/haus-automation/iobroker-docs>`_. Falls Du einen Fehler findest oder Inhalte ergänzen möchtest, bist Du herzlich dazu eingeladen!

Willkommen auf den Seiten der **inoffiziellen** ioBroker-Dokumentation. Diese Dokumentation wurde von `Matthias Kleine (haus:automation) <https://haus-automatisierung.com/>`_ ins Leben gerufen und soll als alternative Informationsquelle zur offiziellen Variante stehen.

**Diese Dokumentation ist ausschließlich in Deutscher Sprache verfügbar.**

Was ist der ioBroker?
---------------------

Wie der Name schon vermuten lässt, handelt es sich bei ioBroker um eine Software-Lösung, welche verschiedene Smart-Home Systeme miteinander verknüpft und zwischen diesen Systemen vermittelt. Aktuell können mehr als 250 verschiedene Systeme integriert werden.

So kann beispielsweise Geräte mit Alexa ansteuern, welche selbst keine Alexa-Integration bieten. Gleiches gilt für HomeKit oder auch Google Home. Aber auch ohne Cloud (im rein lokalen Betrieb) bietet eine ioBroker-Installation viele Vorteile. Auf einem KNX-Taster die Leistung der per Modbus angebundenen PV-Anlage sehen? Kein Problem. Mit einem Philips Hue Button einen HomeMatic-Aktor ansteuern? Auch kein Problem.

Der ioBroker weicht die (künstlichen) Grenzen der Hardware-Hersteller auf und ermöglicht die Kombination von Systemen, welche nicht direkt miteinander sprechen können.

**Je länger man mit dem ioBroker arbeitet, desto mehr Möglichkeiten wird man entdecken!** Es lässt sich nahezu jede Lösung anbinden, welche eine Schnittstelle bereitstellt. Der ioBroker steht "in der Mitte" all dieser Systeme und vermittelt zwischen diesen - daher auch der Name **io** (input / output) **Broker** (Vermittler).

.. note::
    Der ioBroker ist eine reine Software-Lösung, welche über viele Interfaces/Schnittstellen und diversen Protokollen mit unterschiedlicher Hardware und Software kommunizieren kann.

.. Contents:

.. toctree::
    :maxdepth: 2
    :caption: Erste Schritte

    getting-started/hardware
    getting-started/installation
    getting-started/resources

.. toctree::
    :maxdepth: 2
    :caption: Grundlagen

    basics/architecture
    basics/datastorage
    basics/aliases
    basics/cli
    basics/acl
    basics/systemconfig
    basics/uuid
    basics/logic
    basics/compactmode
    basics/backup

.. toctree::
    :maxdepth: 2
    :caption: Adapter

    adapters/weather
    adapters/databases
    adapters/api

.. toctree::
    :maxdepth: 2
    :caption: Ökosystem

    ecosystem/domains
    ecosystem/repositories
    ecosystem/documentation
    ecosystem/licenses
    ecosystem/news
    ecosystem/iot
    ecosystem/ratings
    ecosystem/sentry
    ecosystem/statistics

.. toctree::
    :maxdepth: 2
    :caption: Entwicklung

    development/adapter
    development/adminconfig
    development/objects
    development/states
    development/iopackage
    development/encryption
    development/messagebox
    development/notifications

.. toctree::
    :maxdepth: 2
    :caption: Best Practice (DEV)

    bestpractice/environment
    bestpractice/adapterdev
    bestpractice/storefiles
    bestpractice/logging
    bestpractice/testing
    bestpractice/sentry

License
-------

MIT License

Copyright (c) 2025 Matthias Kleine <info@haus-automatisierung.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.