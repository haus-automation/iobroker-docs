.. _getting-started-hardware:

Hardware
========

Generell lässt sich der ioBroker auf jedem Betriebssystem installieren, auf welchem auch `nodejs <https://nodejs.org/en/>`_ läuft.

.. tip::
    Achte auf den Energiebedarf Deiner eingesetzten Hardware. ioBroker macht nur Sinn, wenn die Anwendung rund um die Uhr (24/7) verfügbar ist und läuft. Selbst wenige Watt machen über das gesamte Jahr betrachtet einen Unterschied.

Raspberry Pi
------------

Als preiswerter Einstieg in die ioBroker-Welt wird nach wie vor der Raspberry Pi empfohlen. Dabei handelt es sich um einen kleinen Einplatinen-Computer, welcher sehr klein ist, wenig Leistung im Dauerbetrieb benötigt und ausreichend Ressourcen für den Betrieb von ioBroker bietet.

Mit der Version 4 des Raspberry Pi wurden erstmals Modelle mit mehr als 1 GB Arbeitsspeicher vorgestellt. Da der ioBroker beim Betrieb mehrerer Adapter relativ viel Arbeitsspeicher benötigt, ist ein System mit **mindestens 2 GB RAM** empfohlen. Die Variante mit 4 GB oder 8 GB bietet noch einmal deutlich mehr Reserven.

Raspberry Pi 4 (mindestens 2 GB RAM, besser 4 GB)

- `Reichelt ** <https://haus-auto.com/p/rei/RaspberryPi4>`_
- `Amazon ** <https://haus-auto.com/p/amz/RaspberryPi4>`_
- `Conrad ** <https://haus-auto.com/p/con/RaspberryPi4>`_

Raspberry Pi Gehäuse

- `Reichelt ** <https://haus-auto.com/p/rei/RaspberryPi4Case>`_
- `Amazon ** <https://haus-auto.com/p/amz/RaspberryPi4Case>`_
- `Conrad ** <https://haus-auto.com/p/con/RaspberryPi4Case>`_

Raspberry Pi Netzteil

- `Reichelt ** <https://haus-auto.com/p/rei/RaspberryPi4Netzteil>`_
- `Amazon ** <https://haus-auto.com/p/amz/RaspberryPi4Netzteil>`_
- `Conrad ** <https://haus-auto.com/p/con/RaspberryPi4Netzteil>`_

Micro SD-Karte

- `Reichelt ** <https://haus-auto.com/p/rei/MicroSD>`_
- `Amazon ** <https://haus-auto.com/p/amz/MicroSD>`_
- `Conrad ** <https://haus-auto.com/p/con/MicroSD>`_

** Link zu einer Produktempfehlung. Wenn Du über einen dieser Links etwas kaufst, bekomme ich eine kleine Provision vom Shop (Affiliate-Link).

.. tip::
    Als Image sollte auf jeden Fall Raspberry Pi OS Lite (64-Bit) und NICHT die Desktop-Version verwendet werden! Warum? Die Desktop-Variante benötigt viel mehr Ressourcen und bringt extrem viel Overhead mit, welcher nicht gebraucht wird. Die Desktop-Version ist nur erforderlich, wenn Du eine Maus, Tastatur und einen Monitor direkt an dem Raspberry Pi anschließen möchtest um diesen (wie der Name sagt) als Desktop-Computer zu verwenden.

Seit Ende 2023 gibt es die neue Generation: Den Raspberry Pi 5. Dieser ist aber mittlerweile leider so kostspielig geworden, dass ein einfacher Mini-PC (z.B. ein refurbished-Modell) teilweise günstiger ist und sogar mehr Leistung/Ressourcen bietet. **Daher kann man den Raspberry Pi 5 zwar verwenden, aber der Vorgänger reicht für die meisten Anwendungen nach wie vor aus!** Und wenn man mehr möchte, nutzt man eh einen Mini-PC (z.B. mit Proxmox).

Wie genau ein Raspberry Pi für die Installation von ioBroker vorbereitet wird, erfährst Du in diesem Video:

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto; margin-bottom: 2em;">
        <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/Z2Y0S5CHUbA" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

Proxmox
-------

Neben einer nativen Installation, ist auch die Installation in einem Linux-Container unter Proxmox (LXC) möglich. Dafür sollte dann aber etwas leistungsstärkere Hardware (wie ein Intel NUC® oder ähnliches) verwendet werden.

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto; margin-bottom: 2em;">
        <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/p6XmgzhH0Ow" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

Weitere Infos
-------------

Möchtest Du alle Möglichkeiten und Schritte genauer erklärt bekommen, empfehle ich eine Teilnahme im `ioBroker-Master-Kurs <https://haus-automatisierung.com/iobroker-kurs/?refid=iobroker-docs>`_. Dort gehe ich noch einmal auf die verschiedenen Varianten ein und zeige die Installation auf einem Raspberry Pi oder nativ unter Debian auf einem Mini-PC.
