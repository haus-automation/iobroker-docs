.. _basics-datastorage:

Daten-Speicherung
=================

Generell wird im ioBroker zwischen Objekten und Zuständen unterschieden. Ein Objekt ist statisch und enthält viele Meta-Daten. Zusätzlich gibt es ggf. den zugehörigen dynamischen Zustand (englisch ``state`` genannt), welcher den aktuellen Wert hält. Beides zusammen nennt sich Datenpunkt.

Die Objekte und Zustände werden in zwei unterschiedlichen Datenbanken vorgehalten, welche der ``js-controller`` verwaltet. Über diesen können Daten aus den Datenbanken abgefragt oder geändert werden.

.. image:: /images/ioBrokerDoku-Datenspeicher.png
    :alt: ioBroker Struktur

Generell wird nahezu alles im ioBroker als Objekt abgelegt:

- die Systemkonfiguration (Sprache, Datumsformat, Währung, ...)
- angelete Benutzer und Gruppen
- Adapter-Einstellungen und Instanz-Konfigurationen
- ...

Obwohl es sich bei all diesen Daten um Objekte in der Objekt-Datenbank handelt, hat man als Anwender fast ausschließlich mit Objekten vom Typ ``state``, ``folder``, ``channel`` oder ``device`` zu tun. Alle anderen Objekte werden vom System verwaltet und sind z.B. im Admin-Adapter gar nicht sichbar (außer, es wird der Experten-Modus aktiviert).

.. note::
    Als Anwender muss man nur sehr wenige Datenpunkte selber anlegen oder bearbeiten. Die meisten Datenpunkte werden von den einzelnen Adaptern (bzw. deren Instanzen) automatisch angelegt und verwaltet. Es ist empfohlen, niemal selbst ein Objekt zu bearbeiten - außer, man hat es selbst angelegt!

Objekt
------

Der Objekt-Typ beschreibt, welche Informationen genau gespeichert werden können. Erforderliche Attribute eines Objektes sind:

- ``_id`` - Eindeutige **ID**
- ``type``- Typ des Objektes (meistens ``device``, ``folder``, ``channel`` or ``state``) - alle weiteren Typen (mit Beispielen) sind in der Entwickler-Dokumentation zu finden: :ref:`development-objects`
- ``common`` - Weitere Optionen je nach Typ (siehe unten)

Nur Objekte des Typs ``state`` haben einen zugehörigen Zustand in der Zustands-Datenbank (siehe unten). Bei Objekten dieses Typs kann über weitere Attribute definiert werden, welche Daten in den Zustand gespeichert werden dürfen. Unter anderem:

- ``common.name`` - Name des Objektes (ggf. in mehrere Sprachen übersetzt)
- ``common.type`` - Datentyp der zu speichernden Daten (``string``, ``boolean``, ``number``, ...)
- ``common.unit`` - Einheit (z.B. °C oder kWh)
- ``common.desc`` - Beschreibung
- ``common.min`` - erlaubter Minimalwert (für ``common.type`` = ``number``)
- ``common.max`` - erlaubter Minimalwert (für ``common.type`` = ``number``)
- ``common.read`` - Zustand kann gelesen werden
- ``common.write`` - Zustand darf vom Nutzer verändert werden (schreiben ohne ``ack``-Flag)
- ...

Objekte werden dabei in einer Hierarchie abgebildet. Also in einer Baumstruktur - genau wie in vielen Dateisystemen. **Als Trennzeichen der Ebenen wird dabei der Punkt ».« verwendet.** Allerdings steht ein Objekt immer für sich alleine. Ein Objekt enthält also technisch nicht die anderen Objekte (wie bei Dateien und Verzeichnissen). Die Hierarchie dient nur der Übersicht.

Angenommen Du hast eine Philips Hue Bridge und hast den Philips Hue-Adapter installiert. Dann würde für diese Instanz automatisch alle nötigen Objekte (und Zustände) für die Steuerung der angelernten Lampen, LED-Strips usw. anlegen.

Der Namespace (siehe weiter unten) dieser Instanz lautet dann ``hue.0``. Hier sieht man schon das erste Trennzeichen. Das erste Objekt auf der höchsten Ebene (Root-Ebene) hat die ID ``hue``. Danach folgt ein weiteres, welches die Instanznummer als ID enthält - also ``hue.0``. Hier sieht man gut, dass die ID nicht nur ``0`` lautet, sondern ``hue.0``. **Die ID ist also immer ein "absoluter Pfad" und nicht nur relativ zum übergeorneten Objekt!**

"Unter" diesem Objekt werden dann weitere Objekte angelegt, welche alles Mögliche abbilden können. Dabei werden die Informationen so granular wie möglich abgebildet. So gibt es zum Beispiel für jede angelernte Lampe ein weiteres Objekt, welches dann wieder Objekte darunter enthält. So wird eine logische Hierarchie aufgebaut. Stell Dir das wie Urlaubsfotos vor, welche in verschiedenen Ordnern auf der Festplatte abgelegt werden. Alle Fotos aus einem Urlaub kommen zusammen in einen Ordner. Und so ist das mit den Objekten auch. Alles, was zum Beispiel eine einzelne Lampe kann, wird als einzelne Objekte unter ein gemeinsames Objekt gepackt. Das könnte die Helligkeit sein, die Farbe, ob die Lampe gerade per Funk erreichbar ist oder nicht, ob es ein Firmwareupdate gibt usw.

.. note::
    Nicht jeder Datenpunkt hat zwingend einen zugehörigen Zustand. Aus organisatorischen Gründen kann man auch Objekte anlegen, welches nur für die Struktur dienen. Diese Objekte sind vom Typ "Kanal" bzw. Englisch "Channel".

.. image:: /images/ioBrokerDoku-ObjektHierarchie.png
    :alt: Objekt-Hierarchie

Die **ID** ist dabei ein eindeutiger Schlüssel zu einem Objekt. Dieser ist vergleichbar mit dem absoluten Pfad in einem Dateisystem, welcher zu genau einem Ziel führt. In der obigen Grafik sieht man die verschiedenen IDs einiger Datenpunkte. Über diese ID kommunizierst man dem Objekt.

Objekte werden im Standard unter Linux als `JSON Lines <https://jsonlines.org>`_ (Text, UTF-8) in der folgenden Datei abgelegt:

``/opt/iobroker/iobroker-data/objects.jsonl``

Diese Datei nennt man auch **Objekt-Datenbank**. Mehr Details (für Entwickler) unter :ref:`development-objects`.

State (Zustand)
---------------

.. note::
    Nur Objekte vom Typ ``state`` haben auch einen zugehörigen Zustand. **Nicht jedes Objekt hat einen Zustand, aber jeder Zustand ein zugehöriges Objekt!**

Ein ``state`` ist der eigentliche Wert eines Datenpunktes. Neben dem Wert werden aber auch hier noch weitere Informationen vorgehalten, wie zum Beispiel:

- ``val`` - Der aktuell gespeicherte Wert
- ``ack`` - Bestätigt-Flag, ob der (neue) Wert vom Adapter bzw. Ziel akzeptiert wurde. Siehe auch :ref:`basics-logic`
- ``ts`` - Unix Timestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **aktualisiert** wurde
- ``lc`` - Unix Zimestamp (Zeitstempel in Millisekunden) wann der Zustand zuletzt **geändert** wurde (last change)
- ...

Es handelt sich also im Gegensatz zum Objekt um **dynamische Daten, welche sich ständig ändern können**.

.. note::
    Die meisten dieser Informationen sind für "normale" Anwender nicht interessant. Man arbeitet zu 99% nur mit dem eigentlichen Wert ``val`` und dem Bestätigt-Flag ``ack`` eines Zustandes. Dennoch ist es wichtig zu wissen, dass neben dem Wert noch mehr Informationen gespeichert werden.

Das zugehörige Objekt gibt dabei vor, wie der Zustand aussehen darf. Also in welchem Datentyp der Wert vorgehalten wird, ob der Zustand nur gelesen werden darf oder auch vom Benutzer geschrieben werden kann, uvm.

**Sollte ein Zustand gelöscht werden, bleibt das zugehörige Objekt bestehen. Wird aber das Objekt gelöscht, wird der Zustand ebenfalls gelöscht!**

Es ist besonders wichtig zu verstehen, was es mit bestätigten Zuständen auf sich hat (siehe ``ack``). Dabei hilft Dir dieses Video:

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto; margin-bottom: 2em;">
        <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/p5FyeifYUnw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

Zustände werden im Standard unter Linux als `JSON Lines <https://jsonlines.org>`_ (Text, UTF-8) in der folgenden Datei abgelegt:

``/opt/iobroker/iobroker-data/states.jsonl``

Diese Datei nennt man auch **State-Datenbank**. Mehr Details (für Entwickler) unter :ref:`development-states`.

Datenpunkt
----------

Wenn man von einem Datenpunkt spricht, ist *meistens* die Kombination aus Objekt mit dem zugehörigen Zustand gemeint.

**Die Kombination von Objekte und Zustand ist die einzige Möglichkeit in ioBroker Daten abzulegen.** Alles wird in dieser Struktur abgebildet. Selbst die Konfiguration von Instanzen wird in Datenpunkten gespeichert. Diese findest man im System-Namespace (siehe unten).

Namespace
---------

Damit die Objekte im System in einer logischen Struktur abgelegt werden, gibt es sog. *Namespaces* (Namensräume). So wird vermieden, dass  jeder Adapter seine Daten irgendwo an eine andere beliebige Stelle in der Hierarchie speichert.

Außerdem werden auf diese Weise doppelte Namen vermieden und als Entwickler kann man sich in seinem Namespace "frei bewegen".

Bleiben wir beim Beispiel Philips Hue, welches schon ötfter in dieser Dokumentation herhalten musste. Wird eine neue Instanz vom Hue-Adapter erstellt, lautet der Namespace für diesen Adapter ``hue.0``.

Zur Wiederholung: Die Null ``0`` steht dabei für die erste Instanz, da von einem Adpater mehrere Instanzen erstellt werden können. Alles, was der Adapter nun an Objekten bereitstellt, ist in diesem Namespace zu finden.

Wird die Instanz gelöscht, wird der Namespace (also alle Objekte und Zustände) ebenfalls gelöscht.

.. note::
    Als Anwender sollte man keine eigenen Objekte in Namespaces von Adaptern oder vom System bearbeiten/erstellen! Für eigene Objekte kann der dafür reservierte Namespace **0_userdata.0** oder  ``alias.0`` genutzt werden. Aliasse sind spezielle Objekte. Mehr dazu unter :ref:`basics-aliases`.

Weiterhin gibt es den (reservierten) Namespace ``system.`` für sämtliche Systemeinstellungen. Dort ist unter anderem folgendes enthalten:

- ``system.config`` - Systemkonfiguration (Sprache, Datumsformat, Verwahrungsort, ...)
- ``system.host.*``` - js-controller-Prozess (Uptime, Ressourcen, ...)
- ``system.repositories`` - Liste der vefügbaren Adpater
- ``system.certificates`` - Konfigurierte Zertifikate
- ``system.meta.*`` - Meta-Informationen (wie die System UUID)
- ``system.user.*`` - Alle Benutzer des Systems
- ``system.group.*`` - Alle Benutzer-Gruppen des Systems
- ``system.adapter.<adapter-name>`` - Standard-Konfiguration des Adapters für neue Instanzen
- ``system.adapter.<adapter-name>.<instanz-nummmer>`` - Informationen zur einzelnen Instanz (Uptime, Ressourcen, ...)

.. danger::
    Ändere keine Informationen in dem System-Namespace, wenn Du nicht genau weißt, was Du tust. Als "normaler Anwender" gibt es keinen Grund, dort etwas ändern. Diese Informationen sind nur für Entwickler relevant! Im Admin-Adapter sieht man diese Objekte auch nur dann, wenn der Expertenmodus angeschaltet ist.

Speicherort
-----------

Im Standard arbeitet der ioBroker (seit ``js-controller`` 4.x) mit dem Datenformat ``jsonl`` für die Objekt- und Zustands-Datenbank. Als Speicherort stehen auch andere Lösungen bereit, sodass stattdessen z.B. `Redis <https://redis.io/>`_ zum Speichern der Daten genutzt wird. *Dabei handelt es sich um einen Dienst, welcher zusätzlich auf dem System installiert werden muss.*

Unterstützte Formate:

- ``file`` - Speichert unter ``/opt/iobroker/iobroker-data/(objects|states).json`` die Daten im JSON-Format (bis ``js-controller`` 3.x war dies der Standard)
- ``jsonl`` - Speichert unter ``/opt/iobroker/iobroker-data/(objects|states).jsonl`` die Daten als `JSON Lines <https://jsonlines.org>`_ (ab ``js-controller`` 4.x ist dies der Standard)
- ``redis`` - Speichert die Daten im Key-Value-Storage über den Dienst `Redis <https://redis.io/>`_ bzw. `Redis Sentinel <https://redis.io/docs/manual/sentinel/>`_ (ab 15.000 Objekten empfohlen)

Hierbei wird ein Speichertyp pro Datenbank festgelegt. Das heißt, dass man theoretisch für die Objekt-Datenbank einen anderen Typ wählen könnte, als für die Zustands-Datenbank. In der Praxis wird dies aber kaum gemacht. Die meisten starten mit ``jsonl`` und wechseln bei Bedarf später auf ``redis``.

Weitere Infos gibt es unter: :ref:`basics-systemconfig`.
