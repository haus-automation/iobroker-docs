.. _development-encryption:

Daten-Verschlüsselung
=====================

ref: https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/adaptersecurity.md

Normalerweise werden alle Einstellungen in Adaptern unverschlüsselt abgelegt. Das heißt, dass zum Beispiel die Instanz-Konfiguration einfach mit in die Objects-Datenbank geschrieben wird. Im Klartext.

Gerade sensible Informationen sollten daher verschlüsselt abgelegt werden.

Seit ``js-controller`` Version 3.0.0 gehört eine Verschlüsselung zum Standard und kann sehr einfach verwendet werden.

Zugriff verbieten
-----------------

Da auch die Instanz-Einstellungen nur Objekte sind, kann jeder Wert im System von jedem anderen Adapter auch gelesen werden. Es wäre also ein leichtes, einen Adapter oder ein Script zu bauen, welches konfigurierte Benutzerdaten, Zugangstoken oder Passwörter anderer Adapter ausliest und diese ins Internet sendet. Dieser Gefahr sollte man sich generell bewusst sein.

Eine kleine Hürde bietet die Funktion, dass der Zugriff auf sensible Konfigurations-Attribute von anderen Adaptern aus verboten wird. Das heißt, dass diese Info einfach nicht ausgeliefert wird. Der Admin-Adapter ist dabei die Ausnahme (immerhin müssen die Daten noch konfigurierbar bleiben).

