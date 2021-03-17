.. _development-objects:

Objekte
-------

ref: https://github.com/ioBroker/ioBroker.docs/blob/master/docs/en/dev/objectsschema.md

```
{
    "_id": "hue.0.Deckenlampe.bri",
    "type": "state",
    "common": {
        "name": "Deckenlampe.bri",
        "read": true,
        "write": true,
        "type": "number",
        "role": "level.dimmer",
        "min": 0,
        "max": 254,
        "def": 254
    },
    "native": {
        "id": "3"
    },
    "from": "system.adapter.hue.0",
    "user": "system.user.admin",
    "ts": 1604080553077,
    "acl": {
        "object": 1636,
        "state": 1636,
        "owner": "system.user.admin",
        "ownerGroup": "system.group.administrator"
    }
}
```