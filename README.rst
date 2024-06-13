webvtt-py
=========

|pypi| |pyversions| |license| |coverage| |build-status| |docs-status| |downloads|

``webvtt-py`` is a Python library for reading, writing and converting WebVTT_ caption files. It also features caption segmentation useful when captioning `HLS videos`_.

Documentation is available at http://webvtt-py.readthedocs.io.

.. _`WebVTT`: https://www.w3.org/TR/webvtt1/
.. _`HLS videos`: https://datatracker.ietf.org/doc/html/draft-pantos-hls-rfc8216bis

Installation
------------

::

    $ pip install webvtt-py

Usage
-----

.. code-block:: python

  import webvtt

  for caption in webvtt.read('captions.vtt'):
      print(caption.identifier)  # cue identifier if any
      print(caption.start)       # cue start time
      print(caption.end)         # cue end time
      print(caption.text)        # cue payload
      print(caption.voice)       # cue voice span if any

Segmenting for HLS
------------------

.. code-block:: python

  import webvtt

  webvtt.segment('captions.vtt', 'output/path')

Converting captions from other formats
--------------------------------------

Supported formats:

* SubRip (.srt)
* YouTube SBV (.sbv)

.. code-block:: python

  import webvtt

  webvtt = webvtt.from_srt('captions.srt')
  webvtt.save()

  # alternatively in a single line
  webvtt.from_sbv('captions.sbv').save()

CLI
---
Caption segmentation is also available from the command line:

::

    $ webvtt segment captions.vtt --output output/path

License
-------

Licensed under the MIT License.

.. |pypi| image:: https://img.shields.io/pypi/v/webvtt-py.svg
    :target: https://pypi.python.org/pypi/webvtt-py

.. |pyversions| image:: https://img.shields.io/pypi/pyversions/webvtt-py.svg
    :alt: Supported Python versions
    :target: https://pypi.python.org/pypi/webvtt-py

.. |license| image:: https://img.shields.io/pypi/l/webvtt-py.svg
    :alt: MIT License
    :target: https://opensource.org/licenses/MIT

.. |coverage| image:: https://codecov.io/gh/glut23/webvtt-py/graph/badge.svg?branch=master
    :target: https://codecov.io/gh/glut23/webvtt-py

.. |build-status| image:: https://github.com/glut23/webvtt-py/actions/workflows/ci.yml/badge.svg?branch=master
    :target: https://github.com/glut23/webvtt-py/actions/workflows/ci.yml

.. |docs-status| image:: https://readthedocs.org/projects/webvtt-py/badge/?version=latest
    :target: http://webvtt-py.readthedocs.io/en/latest/?badge=latest
    :alt: Documentation Status

.. |downloads| image:: https://static.pepy.tech/badge/webvtt-py
    :target: https://pepy.tech/project/webvtt-py
    :alt: Downloads