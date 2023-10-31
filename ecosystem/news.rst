.. _ecosystem-news:

Neuigkeiten
===========

:octicon:`git-branch;1em;sd-text-info` Seit Admin 6.10.5 werden für die News (intern) :ref:`development-notifications` verwendet.

Um Benutzer über Neuigkeiten rund um ihr ioBroker-System zu informieren, wird in regelmäßigen Abständen eine ``news.json`` von den ioBroker-Servern geladen.

Dabei wird (genau wie bei den - :ref:`ecosystem-repositories`), eine Hash-Datei heruntergeladen und der Hash in einem State (``admin.x.info.newsETag``) gespeichert. Sollte sich der Hash geädndert haben, wird die neue News-Datei vom Admin heruntergeladen und ebenfalls in einen State gespeichert: ``admin.x.info.newsFeed``.

news.json
---------

Enthält ein Array von News-Objekten. Über verschiedene Eigenschaften kann gesteuert werden, welchen Nutzern welche Neuigkeiten angezeigt werden. So kann beispielsweise eingeschränkt werden, dass ein bestimmter Adapter in einer definierten Version installiert sein muss.

`Dokumentation aller verfügbaren Eigenschaften <https://github.com/ioBroker/ioBroker.docs/blob/master/info/news.md>`_

Beispiel:

.. code:: json

    {
        "title": {
            "en": "A nice title",
            "de": "Ein schöner Titel",
            "ru": "Хорошее название",
            "pt": "Um belo título",
            "nl": "Een mooie titel",
            "fr": "Un joli titre",
            "it": "Un bel titolo",
            "es": "un buen titulo",
            "pl": "Fajny tytuł",
            "uk": "Гарний титул",
            "zh-cn": "一个不错的标题"
        },
        "content": {
            "en": "This is really important",
            "de": "Das ist wirklich wichtig",
            "ru": "Это действительно важно",
            "pt": "Isso é realmente importante",
            "nl": "Dit is echt belangrijk",
            "fr": "C'est vraiment important",
            "it": "Questo è davvero importante",
            "es": "esto es muy importante",
            "pl": "To jest naprawdę ważne",
            "uk": "Це дійсно важливо",
            "zh-cn": "这真的很重要"
        },
        "id": "socketio-update-web-admin-socketio-stable",
        "class": "warning",
        "fa-icon": "exclamation-triangle",
        "created": "2020-05-03T01:00:00.000Z",
        "conditions": {
            "admin": "smaller(4.0.0)",
            "web": "smaller(3.0.0)",
            "socketio": "smaller(3.0.0)"
        }
    }

Alternativ kann z.B. geprüft werden, ob Adapter installiert sind, welche die News betreffen. Alle Bedingungen müssen erfüllt sein (UND-Verknüpfung):

.. code:: json

    "conditions": {
        "squeezebox": "installed"
    }

Links
-----

- `News für den Admin-Adapter (repo/news.json) <https://iobroker.live/repo/news.json>`_
- `News hash für den Admin-Adapter (repo/news-hash.json) <https://iobroker.live/repo/news-hash.json>`_
- `news.json <https://github.com/ioBroker/ioBroker.docs/blob/master/info/news.json>`_
- `Wie man news hinzufügt <https://github.com/ioBroker/ioBroker.docs/blob/master/info/news.md>`_
