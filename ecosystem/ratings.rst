.. _ecosystem-ratings:

Adapter-Ratings
===============

Seitdem der Admin-Adapter in Version 5 zur Verfügung steht, können die Anwender einzelne Adapter bewerten und auch Bewertungen von anderen Nutzern ansehen. Dabei sieht man in der Adapter-Liste direkt eine Sterne-Bewertung (1-5 Sterne). Mit einem Klick auf die Bewertung können Details abgefragt werden.

- Bewertungen pro Adapter
- In der Adapter-Liste sichtbar
- Details per Klick sichtbar (Kommentare von anderen Nutzern)
- Neue Bewertungen für installierte Adapter möglich

Neue Bewertung
--------------

POST-Request: ``https://rating.iobroker.net/vote``

**Payload:**

.. code:: json

    {
        "uuid": "xxx",
        "adapter": "wled",
        "version": "0.6.3",
        "rating": 4,
        "comment": "Wirklich gut gemacht",
        "lang": "de"
    }

**Response:**

.. code:: json

    {
        "adapter": "javascript",
        "rating": {
            "r": 4.51,
            "c": 21
        },
        "5.2.13": {
            "r": 5,
            "c": 1
        }
    }

Auslesen
--------

GET-Request: ``https://rating.iobroker.net/adapter/wled?uuid=xxx``

**Response:**

.. code:: json

    {
        "rating": {},
        "comments": [
            {
                "ts": "2021-08-29T18:14:26.000Z",
                "comment": "Perfekt, aber ich bekommen seit dieser Version immer die Meldungen has to be one of type  string ,  number ,  boolean  but received type  object",
                "version": "0.5.6",
                "uuid": false,
                "rating": 5,
                "lang": "de"
            }
        ],
        "adapter": "wled"
    }
