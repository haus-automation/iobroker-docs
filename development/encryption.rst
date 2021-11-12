.. _development-encryption:

Daten-Verschlüsselung
=====================

Normalerweise werden alle Einstellungen in Adaptern unverschlüsselt abgelegt. Das heißt, dass zum Beispiel die Instanz-Konfiguration einfach mit in die Objects-Datenbank geschrieben wird. Im Klartext.

Gerade sensible Informationen sollten daher verschlüsselt abgelegt werden.

Seit ``js-controller`` Version 3.0.0 gehört eine Verschlüsselung zum Standard und kann sehr einfach verwendet werden.

.. danger::
    Da auch die Instanz-Einstellungen nur Objekte sind, kann jeder Wert im System von jedem anderen Adapter auch gelesen werden. Es wäre also ein leichtes, einen Adapter oder ein Script zu bauen, welches konfigurierte Benutzerdaten, Zugangstoken oder Passwörter anderer Adapter ausliest und diese ins Internet sendet. Dieser Gefahr sollte man sich als Nutzer generell bewusst sein.

Zugriff verbieten
-----------------

Eine kleine Hürde bietet die Funktion, dass der Zugriff auf sensible Konfigurations-Attribute von anderen Adaptern aus verboten wird. Das heißt, dass diese Info einfach nicht ausgeliefert wird. Der Admin-Adapter ist dabei die Ausnahme (immerhin müssen die Daten noch konfigurierbar bleiben).

Dazu kann in der :ref:`development-iopackage` mit einem neuen Attribute festgelegt werden, dass diese Information gefiltert wird, wenn ein anderer Adapter das Objekt liest.

.. code:: json

    "protectedNative": [
        "user",
        "password"
    ]

In diesem Beispiel sind also die beiden Attribute ``user`` und ``password`` nicht mehr von anderen Adaptern einfach so lesbar.

.. warning::
    Dieses Vorgehen bietet natürlich keine 100% Sicherheit, denn ein anderer Adapter kann nach wie vor die Objekt-Datei von der Festplatte lesen und das Attribut dort extrahieren.

Daten verschlüsseln
-------------------

Wie Du schon weißt, werden Objekte als JSON in der Datei ``/opt/iobroker/iobroker-data/objects.json`` abgelegt. Jede Eigenschaft wird dabei einfach im Klartext dort gespeichert. Hat nun jemand Zugriff auf diese Datei, können die Daten einfach gelesen werden. Passwörter, API-Token, Benutzernamen usw. liegen also im Klartext in dieser Datei.

Um die Daten nicht mehr im Klartext zu speichern, kann man einzelne Attribute verschlüsseln lassen. Dies geschieht ebenfalls über ein Attribut in der ``io-package.json`` des einzelnen Adapters.

.. code:: json

    "encryptedNative": [
        "password"
    ]

In diesem Beispiel wird also das Passwort verschlüsselt in der Objekt-Datenbank abgelegt und ist nicht mehr im Klartext lesbar.

.. warning::
    Alle Adapter verschlüsseln ihre Informationen mit dem gleichen Schlüssel und dem gleichen Verfahren. Es ist also dennoch möglich, dass andere Adapter diese Informationen auszulesen und die Daten zu entschlüsseln.

Generell ist es sinnvoll, beide Attribute zu kombinieren (wie hier gezeigt). Also auch verschlüsselte Informationen nicht einfach auslesen zu lassen.

Die Verschlüsselung und Entschlüsselung passiert komplett automatisch. Die Informationen müssen also nicht manuell im Code entschlüssel werden.

Links
-----

- `Offizielle Doku <https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/adaptersecurity.md>`_