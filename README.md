![ioBroker](logo.png)

# ioBroker documentation (UNOFFICIAL)

This is the repository for the german ioBroker documentation available [here](https://iobroker.readthedocs.io/de/latest/).

This documentation is written using [reStructuredText](http://docutils.sourceforge.net/rst.html), powered by [Sphinx](http://www.sphinx-doc.org/en/stable/), and hosted on [ReadTheDocs](http://readthedocs.org).

## Sponsored by

[![ioBroker Master Kurs](https://haus-automatisierung.com/images/ads/ioBroker-Kurs.png)](https://haus-automatisierung.com/iobroker-kurs/?refid=iobroker-docs)

## Building the docs

Install Software 

1. [Install python and `pip`](https://pip.pypa.io/en/stable/installing).
1. [Install sphinx and sphinx-autobuild](https://docs.readthedocs.io/en/latest/getting_started.html)
  `pip install sphinx shpinx-autobuild`
1. [Install sphinx-rtd-theme](https://github.com/snide/sphinx_rtd_theme#installation)
  `pip install sphinx_rtd_theme`
1. Install recommonmark
  `sudo pip install recommonmark`

Follow these steps to build the documentation locally:

1. Open terminal and cd to the project folder
1. Build HTML: `$ make html`
1. Open `_build/html/index.html` in a web browser.

To see the available make targets, simply execute `make`.

## Contributing

If you find a typo, error, or think something can be communicated better, fork this repository and make a pull request.

## Sources

- https://github.com/ioBroker/ioBroker.docs
- https://github.com/ioBroker/ioBroker/tree/master/doc
- https://github.com/ioBroker/ioBroker.js-controller/tree/master/doc
- https://www.iobroker.net/docu/index-42.htm?page_id=2786&lang=de

## License

MIT License

Copyright (c) 2025 Matthias Kleine <info@haus-automatisierung.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.