.. _adapters-weather:

Wetter-Adapter
==============

Aktuell gibt es im ioBroker viele Adapter, um über einen Online-Dienst die Wetterdaten von verschiedenen Standorten zu ermitteln:

- `ioBroker.yr <https://github.com/ioBroker/ioBroker.yr>`_ (core team)
- `ioBroker.openweathermap <https://github.com/ioBroker/ioBroker.openweathermap>`_ (core team)
- `ioBroker.accuweather <https://github.com/iobroker-community-adapters/ioBroker.accuweather>`_ (community)
- `ioBroker.weatherunderground <https://github.com/iobroker-community-adapters/ioBroker.weatherunderground>`_ (community)
- `ioBroker.daswetter <https://github.com/rg-engineering/ioBroker.daswetter>`_ (rg-engineering/)

Geräte
------

Im Geräte-Adapter gibt es zwei unterschiedliche Geräte für Wetter

- `Aktueller Wetterlage <https://github.com/ioBroker/ioBroker.type-detector/blob/master/DEVICES.md#current-weather-weathercurrent>`_ (weatherCurrent)
- `Wettervorhersage <https://github.com/ioBroker/ioBroker.type-detector/blob/master/DEVICES.md#weather-forecast-weatherforecast>`_ (weatherForecast)

Für die aktuelle Wetterlage werden folgende Parameter benötigt:

- `ACTUAL` (Temperatur in °C)
- `ICON`
- `PRECIPITATION_CHANCE` (Niederschlagswahrscheinlichkeit in %)
- `PRECIPITATION_TYPE`
- `PRESSURE` (Luftdruck in hPa)
- `PRESSURE_TENDENCY`
- `REAL_FEEL_TEMPERATURE`
- `HUMIDITY`
- `UV`
- `WEATHER`
- `WIND_DIRECTION`
- `WIND_GUST`
- `WIND_SPEED`

Für die Wettervorhersage werden die folgenden Attribute benötigt:

- `ICON`
- `TEMP_MIN`
- `TEMP_MAX`
- `PRECIPITATION_CHANCE`
- `PRECIPITATION`
- `DATE`
- `DOW`
- `STATE`
- `TEMP`
- `PRESSURE`
- `HUMIDITY`
- `TIME_SUNRISE`
- `TIME_SUNSET`
- `WIND_CHILL`
- `FEELS_LIKE`
- `WIND_SPEED`
- `WIND_DIRECTION`
- `WIND_DIRECTION_STR`
- `WIND_ICON`
- `HISTORY_CHART`
- `FORECAST_CHART`
- `LOCATION`
