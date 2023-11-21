.. _development-adminconfig:

Admin-Konfiguration
===================

Als Adapter-Entwickler gibt es mehrere Wege eine Konfiguration der Instanz bereitzustellen:

- JSON-Config
    - JSON-Config custom components
- Adapter-React
- Materialize HTML/CSS (deprecated)
- HTML (deprecated)

Über die :ref:`development-iopackage` Konfiguration wird festgelegt, welche Instanz-Konfiguration, Objekt-Eigenschaften oder Admin-Tabs der jeweilige Adapter unterstützt.

- ``common.adminUI.config`` legt fest, welche Methode für die Instanz-Konfiguration genutzt werden soll - siehe :ref:`development-iopackage`
- ``common.adminUI.custom`` legt fest, welche Methode für Objekt-Eigenschaften genutzt werden soll - siehe :ref:`development-iopackage`
- ``common.adminUI.tab`` legt fest, welche Methode für Admin-Tabs genutzt werden soll - siehe :ref:`development-iopackage`

.. warning::
    Es gibt noch einige weitere Attribute in der io-package.json, welche dem Admin mitteilen, welche Art genutzt wird. Allerdings sind diese deprecated und sollten nicht mehr verwendet werden!

JSON-Config
-----------

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``admin`` 5

.. todo::
    Explain

JSON-Config custom components
-----------------------------

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``admin`` 6

Mit den Standard JSON-Config-Komponenten (siehe oben) kann man schon eine Menge realisieren. Sollte darüber hinaus noch mehr benötigt werden, können eigene Komponenten (sog. Custom Components) hinzugefügt werden.

Beispiele:

- `Template <https://github.com/ioBroker/ioBroker.admin-component-template>`_
- `Admin easy access <https://github.com/ioBroker/ioBroker.admin-component-easy-access>`_
- `Telegram-Adapter (Usernames) <https://github.com/iobroker-community-adapters/ioBroker.telegram/tree/master/src>`_

Materialize HTML/CSS
--------------------

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``admin`` 3

.. todo::
    Explain

HTML
----

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``admin`` 2

.. warning::
    Diese Methode ist veraltet und sollte nicht mehr für neue Adapter verwendet werden. In seltenen Fällen gibt es noch sehr alter Adapter, welche auf noch keine neuere Methode aktualisiert wurden.

Links
-----

- `Adapter-React v5 <https://github.com/ioBroker/adapter-react-v5>`_
- `Adapter-React-Demo <https://github.com/ioBroker/adapter-react-demo>`_
- `Admin JsonConfigComponents Schema <https://github.com/ioBroker/ioBroker.admin/blob/master/src/src/components/JsonConfigComponent/SCHEMA.md>`_
