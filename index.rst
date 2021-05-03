.. ioBroker documentation (UNOFFICIAL) documentation master file, created by
   sphinx-quickstart on Wed Feb 17 09:25:17 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Willkommen
==========

.. tip::
    Diese Dokumentation ist Open Source und hier zu finden: `GitHub <https://github.com/haus-automation/iobroker-docs>`_. Falls Du einen Fehler findest oder Inhalte ergänzen möchtest, bist Du herzlich dazu eingeladen!

Willkommen auf den Seiten der **inoffiziellen** ioBroker-Dokumentation. Diese Dokumentation wurde von `Matthias Kleine, haus-automatisierung.com <https://haus-automatisierung.com/>`_ ins Leben gerufen und soll als Alternative Informations-Quelle zur offiziellen Variante stehen.

**Die Dokumentation ist ausschließlich auf Deutsch verfügbar.**

Was ist der ioBroker?
---------------------

Wie der Name schon vermuten lässt, handelt es sich beim ioBroker um eine Software-Lösung, welche es ermöglicht, verschiedene Smart-Home Systeme miteinander zu verknüpfen und zwischen diesen Systemen zu vermitteln. Aktuell können über 250 verschiedene Systeme integriert werden.

So kann man zum Beispiel Geräte mit Alexa ansteuern, welche selbst keine Alexa-Integration bieten. Gleiches gilt für HomeKit oder auch Google Home. Aber auch ohne Cloud-Lösungen im rein lokalen Betrieb macht der ioBroker Sinn. Du möchtest auf einem KNX-Taster die Leistung der per Modbus angebundenen PV-Anlage sehen? Kein Problem. Du möchtest mit einem Philips Hue Button einen HomeMatic-Aktor ansteuern? Auch kein Problem.

Mit dem ioBroker kannst Du also über die Grenzen der Hardware-Hersteller hinausgehen und Systeme miteinander kombinieren, welche nicht direkt miteinander sprechen können.

**Je länger Du mit ioBroker arbeitest, desto mehr Möglichkeiten wirst Du entdecken!** Es lässt sich nahezu jede Lösung anbinden, welche eine Schnittstelle bereitstellt. Der ioBroker steht dann praktisch in der Mitte all dieser Systeme und vermittelt - daher auch der Name **io** (input / output) **Broker** (Vermittler).

.. note::
    ioBroker ist eine reine Software-Lösung, welche über viele Interfaces/Schnittstellen und diversen Protokollen mit unterschiedlicher Hardware und Software kommunizieren kann.

.. Contents:

.. toctree::
    :maxdepth: 2
    :caption: Erste Schritte

    getting-started/hardware
    getting-started/installation

.. toctree::
    :maxdepth: 2
    :caption: Grundlagen

    basics/architecture
    basics/datastorage
    basics/logic

.. toctree::
    :maxdepth: 2
    :caption: Entwicklung

    development/adapter
    development/objects
    development/states
    development/encryption

.. image:: https://haus-automatisierung.com/images/ads/ioBroker-Kurs.png
    :target: https://haus-automatisierung.com/iobroker-kurs/

License
-------

MIT License

Copyright (c) 2021 Matthias Kleine <info@haus-automatisierung.com>

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