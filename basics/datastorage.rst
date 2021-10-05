.. _basics-datastorage:

Daten-Speicherung
=================

Um Daten zu speichern, muss ein Objekt und ein Zustand existieren. Das Objekt ist statisch und enthält viele Meta-Daten. Zusätzlich gibt es den dynamischen Zustand (auch "State" genannt), welcher den aktuellen Wert hält. Beides zusammen nennt sich Datenpunkt.

Die Objekte und Zustände werden in Datenbanken vorgehalten, welche der ``js-controller`` verwaltet. Über diesen können Daten aus den Datenbanken abgefragt oder geändert werden.

.. image:: /images/ioBrokerDoku-Datenspeicher.png
    :alt: ioBroker Struktur

.. note::
    Als Anwender muss man nur sehr wenige Datenpunkte selber anlegen. Die meisten Datenpunkte werden von den einzelnen Adaptern automatisch angelegt.

Objekt
------

Ein Objekt beschreibt, welche Informationen genau gespeichert werden können. Es handelt sich bei einem Objekt hauptsächlich Meta-Informationen, welche den Datenpunkt beschreiben. Erforderlich sind:

- ``_id`` - Eindeutige **ID**
- ``type``- Typ des Objektes (``device``, ``channel``, ``state``, ...)

Daneben gibt es noch weitere (optionale) Informationen, welche das Objekt genauer definieren.

- Name
- Einheit (z.B. °C oder kWh)
- Beschreibung
- erlaubter Minimalwert und Maximalwert
- Lese- und Schreibrechte
- ...

Objekte werden dabei in einer Hierarchie abgebildet. Also in einer Baumstruktur - genau wie in vielen Dateisystemen. **Als Trennzeichen der Ebenen wird dabei der Punkt benutzt.**

Angenommen Du hast eine Philips Hue Bridge und hast den Philips Hue-Adapter installiert. Dann würde für diese Instanz automatisch alle nötigen Objekte für die Steuerung der angelernten Lampen, Strips usw. anlegen.

Der Namespace (siehe unten) dieser Instanz lautet dann ``hue.0``. Hier siehst Du schon das erste Trennzeichen. Das erste Objekt auf der Root-Ebene heißt also ``hue``. Danach folgt ein weiteres, welches wie die Instanznummer heißt.

Unter diesem Objekt werden dann weitere Objekte angelegt, welche alles Mögliche abbilden können. Dabei werden die Informationen so granular wie möglich abgebildet. So gibt es zum Beispiel für jede angelernte Lampe ein weiteres Objekt, welches dann wieder Objekte darunter enthält.
So wird eine logische Hierarchie aufgebaut. Stell Dir das wie deine Urlaubsfotos vor, welche Du auch in verschiedene Ordner auf deiner Festplatte ablegst. Alle Fotos aus einem Urlaub kommen zusammen in einen Ordner. Und so ist das mit den Objekten auch. Alles, was zum Beispiel eine einzelne Lampe kann, wird als einzelne Objekte unter ein gemeinsames Objekt gepackt.

.. note::
    Nicht jeder Datenpunkt hat zwingend einen zugehörigen Zustand. Aus organisatorischen Gründen kann man auch Objekte anlegen, welches nur für die Struktur dienen. Diese Objekte sind vom Typ "Kanal" bzw. Englisch "Channel".

.. image:: /images/ioBrokerDoku-ObjektHierarchie.png
    :alt: Objekt-Hierarchie

Die **ID** ist dabei ein eindeutiger Schlüssel zu einem Objekt. Dieser ist vergleichbar mit dem absoluten Pfad in einem Dateisystem, welcher zu genau einem Ziel führt. In der obigen Grafik siehst Du die verschiedenen IDs einiger Datenpunkte. Über diese ID kommunizierst Du mit dem Objekt.

Objekte werden unter Linux als JSON (Text, UTF-8) in der folgenden Datei abgelegt:

``/opt/iobroker/iobroker-data/objects.json``

Diese Datei nennt man auch Objekt-Datenbank. Mehr Details unter :ref:`development-objects`.

State (Zustand)
---------------

.. note::
    Nur Objekte vom Typ ``state`` haben auch einen zugehörigen Zustand. **Nicht jedes Objekt hat einen Zustand, aber jeder Zustand ein Objekt!**

Ein ``state`` ist der eigentliche Wert eines Datenpunktes. Neben dem Wert werden aber auch hier noch weitere Informationen vorgehalten, wie zum Beispiel:

- ``val`` - Der aktuell gespeicherte Wert
- ``ack`` - Bestätigt-Flag, ob der (neue) Wert vom Adapter bzw. Ziel akzeptiert wurde
- ``ts`` - Unix Timestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **aktualisiert** wurde
- ``lc`` - Unix Zimestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **geändert** wurde (last change)
- ...

Es handelt sich also im Gegensatz zum Objekt um dynamische Daten, welche sich ständig ändern können.

.. note::
    Die meisten dieser Informationen sind für Dich als Anwender nicht interessant. Du arbeitest zu 99% nur mit dem Wert ``val`` eines Zustandes. Dennoch solltest Du wissen, dass neben dem Wert noch mehr Informationen gespeichert werden.

Das zugehörige Objekt gibt dabei vor, wie der Zustand aussehen darf. Also in welchem Datentyp der Wert vorgehalten wird, ob der Zustand nur gelesen werden darf oder auch geschrieben werden kann, uvm.

Es ist besonders wichtig zu verstehen, was es mit bestätigten Zuständen auf sich hat (siehe ``ack``). Dabei hilft Dir dieses Video:

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto; margin-bottom: 2em;">
        <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/p5FyeifYUnw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

Zustände werden im Standard als JSON (Text, UTF-8) in der folgenden Datei abgelegt:

``/opt/iobroker/iobroker-data/states.json``

Diese Datei nennt man auch State-Datenbank. Mehr Details unter :ref:`development-states`.

Datenpunkt
----------

Wenn man von einem Datenpunkt spricht, ist die Kombination aus Objekt mit dem zugehörigen Zustand gemeint.

Die Kombination von Objekte und Zustand ist die einzige Möglichkeit in ioBroker Daten abzulegen. Alles wird in dieser Struktur abgebildet. Selbst die Konfiguration von Instanzen wird in Datenpunkten gespeichert. Diese findest Du z.B. im System-Namespace (siehe unten).

Namespace
---------

Damit die Objekte im System in einer logischen Struktur abgelegt werden, gibt es sog. Namespaces (Namensräume). So wird vermieden, dass nicht jeder Adapter seine Daten an eine andere Stelle in der Hierarchie speichert.
Außerdem werden auf diese Weise doppelte Namen vermieden und als Entwickler kann man sich in seinem Namespace "frei bewegen".

Bleiben wir beim Beispiel Philips Hue, welches schon ötfter in dieser Dokumentation herhalten musste. Erstellst Du eine Instanz vom Hue-Adapter, lautet der Namespace für diesen Adapter ``hue.0``.
Du erinnerst Dich: Die Null steht dabei für die erste Instanz, da von einem Adpater mehrere Instanzen erstellt werden können. Alles, was der Adapter nun an Objekten bereitstellt, ist in diesem Namespace zu finden.
Löschst Du die Instanz, wird der Namespace ebenfalls gelöscht.

.. note::
    Als Anwender solltest Du keine eigenen Objekte in Namespaces von Adaptern oder vom System ablegen! Wenn Du eigene Objekte erstellen möchtest, tu dies bitte im Namespace **0_userdata**

Weiterhin gibt es den (reservierten) Namespace ``system.`` für das System. Dort ist unter anderem folgendes enthalten:

- ``system.config`` - Systemkonfiguration (Sprache, Datumsformat, Verwahrungsort, ...) - siehe :ref:`basics-systemconfig`
- ``system.host.<hostname>``` - js-controller-Prozess (Uptime, Ressourcen, ...)
- ``system.repositories`` - Liste der vefügbaren Adpater
- ``system.certificates`` - Konfigurierte Zertifikate
- ``system.meta.`` - Meta-Informationen
- ``system.user.`` - Alle Benutzer des Systems
- ``system.group.`` - Alle Benutzer-Gruppen des Systems
- ``system.adapter.<adapter-name>`` - Standard-Konfiguration des Adapters für neue Instanzen
- ``system.adapter.<adapter-name>.<instance-nummmer>`` - Informationen zur einzelnen Instanz (Uptime, Ressourcen, ...)

.. danger::
    Ändere keine Informationen in dem System-Namespace, wenn Du nicht genau weißt, was Du da tust. Als normaler Anwender gibt es keinen Grund, dort etwas ändern. Diese Informationen sind nur für Entwickler relevant! Im Admin-Adapter sieht man diese Objekte auch nur, wenn der Expertenmodus angeschaltet ist.

Speicherort
-----------

Im Standard arbeitet der ioBroker mit dem Dateisystem (``files``) als Speicherort für die Objekt- und Zustands-Datenbank. Dies kann aber auch umkonfiguriert werden, sodass stattdessen z.B. `Redis <https://redis.io/>`_ zum Speichern der Daten genutzt wird. Dabei handelt es sich um einen Dienst, welcher zusätzlich auf dem System installiert werden muss.

TODO