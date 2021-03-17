.. _basics-datastorage:

Daten-Speicherung
=================

Um Daten zu speichern, muss zuerst ein (neues) Objekt angelegt werden. Dieses Objekt ist statisch und enthält viele Meta-Daten. Zusätzlich gibt es den dynamischen Zustand (state), welcher den aktuellen Wert hält. Beides zusammen nennt sich Datenpunkt.

.. note::
    Als Anwender muss man nur sehr wenige Objekte selber anlegen, welche zum persistenten Ablegen von eigenen Daten dienen. Die meisten Objekte werden von den einzelnen Adaptern automatisch angelegt.

Objekt
------

Ein Objekt beschreibt, welche Informationen genau gespeichert werden können. Das sind hauptsächlich Meta-Informationen, welche den Datenpunkt beschreiben. Das sind zum Beispiel:

- Typ (channel, state, ...)
- Name
- Einheit (z.B. °C oder kWh)
- Beschreibung
- Lese- und Schreibrechte
- ...

Objekte werden dabei in einer Hierarchie abgebildet. Also in einer Baumstruktur - genau wie in vielen Dateisystemen. **Als Trennzeichen der Ebenen wird dabei der Punkt benutzt.**

Angenommen Du hast eine Philips Hue Bridge und hast den Philips Hue-Adapter installiert. Dann würde für diese Instanz automatisch alle nötigen Objekte für die Steuerung der angelernten Lampen, Strips usw. anlegen.

Der Namespace (siehe unten) dieser Instanz lautet dann ``hue.0``. Hier siehst Du schon das erste Trennzeichen. Das erste Objekt auf der Root-Ebene heißt also ``hue``. Danach folgt ein weiteres, welches wie die Instanznummer heißt.

Unter diesem Objekt werden dann weitere Objekte angelegt, welche alles Mögliche abbilden können. Dabei werden die Informationen so granular wie möglich abgebildet. So gibt es zum Beispiel für jede angelernte Lampe ein weiteres Objekt, welches dann wieder Objekte darunter enthält.
So wird eine logische Hierarchie aufgebaut. Stell Dir das wie deine Urlaubsfotos vor, welche Du auch in verschiedene Ordner auf deiner Festplatte ablegst. Alle Fotos aus einem Urlaub kommen zusammen in einen Ordner. Und so ist das mit den Objekten auch. Alles, was zum Beispiele eine einzelne Lampe kann, wird unter ein gemeinsames Objekt gepackt.

.. note::
    Nicht jeder Datenpunkt hat zwingend einen State. Aus organisatorischen Gründen kann man auch Objekte anlegen, welches nur für die Struktur dienen. Diese Objekte sind vom Typ "Kanal" bzw. Englisch "Channel".

.. image:: /images/ioBrokerDoku-ObjektHierarchie.png
    :alt: Objekt-Hierarchie

Objekte werden unter Linux als JSON (Text, UTF-8) in der folgenden Datei abgelegt:

``/opt/iobroker/iobroker-data/objects.json`` 

State
-----

.. note::
    Nur Objekte vom Typ ``state`` haben auch einen zugehörigen State. **Nicht jedes Objekt hat einen State, aber jeder State ein Objekt!**

Ein state ist der eigentliche Wert eines Datenpunktes. Neben dem Wert werden aber auch hier noch weitere Informationen vorgehalten, wie zum Beispiel:

- ``val`` - Der aktuell gespeicherte Wert
- ``ack`` - Bestätigt-Flag, ob der (neue) Wert vom Adapter bzw. Ziel akzeptiert wurde
- ``ts`` - Unix Timestamp (Zeitstempel) wann der State zuletzt **aktualisiert** wurde
- ``lc`` - Unix Zimestamp (Zeitstempel) wann der State zuletzte **geändert** wurde (last change)
- ``from`` - Welche Instanz hat die Änderung durchgeführt (z.B. ``system.adapter.admin.0``)
- ``user`` - Benutzer, welche die Änderung durchgeführt hat (z.B. ``system.user.admin``)
- ...

.. note::
    Die meisten dieser Informationen sind für Dich als Anwender nicht interessant. Du arbeitest zu 99% nur mit dem Wert ``val`` eines States. Dennoch solltest Du wissen, dass neben dem Wert noch mehr Informationen gespeichert werden.

States werden unter Linux als JSON (Text, UTF-8) in der folgenden Datei abgelegt:

``/opt/iobroker/iobroker-data/states.json`` 

Datenpunkt
----------

Wenn man von einem Datenpunkt spricht, ist die Kombination aus Objekt mit dem zugehörigen State gemeint.

Namespace
---------

Der Namespace (Namensraum) ist ein reservierter Bereich für jeden Adapter.



.. note::
    Als Anwender solltest Du keine eigenen Objekte in Namespaces von Adaptern oder vom System ablegen! Wenn Du eigene Objekte erstellen möchtest, tu dies bitte im Namespace **0_userdata**

Weiterhin gibt es den (reservierten) Namespace ``system.`` für das System. Dort ist unter anderem folgendes enthalten:

- ``system.config`` - Systemkonfiguration (Sprache, Datumsformat, Verwahrungsort, ...)
- ``system.host.<hostname>``` - js-controller-Prozess
- ``system.repositories`` - Liste der vefügbaren Adpater
- ``system.certificates`` - Konfigurierte Zertifikate
- ``system.meta.`` - Meta-Informationen des Systems
- ``system.user.`` - Benutzer
- ``system.group.`` - Benutzer-Gruppen
- ``system.adapter.<adapter-name>`` - default config of an adapter
- ``system.adapter.<adapter-name>.<instance-nummmer>`` - Informationen zur Instanz (Uptime, Ressourcen, ...)

.. danger::
    Ändere keine Informationen in dem System-Namespace, wenn Du nicht genau weißt, was Du da tust. Als normaler Anwender gibt es keinen Grund, dort etwas ändern. Diese Informationen sind nur für Entwickler relevant!