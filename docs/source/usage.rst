Usage
=====

Reading WebVTT caption files
----------------------------

.. code-block:: python

    import webvtt

    # we can iterate over the captions
    for caption in webvtt.read('captions.vtt'):
        print(caption.start)  # start timestamp in text format
        print(caption.end)  # end timestamp in text format
        print(caption.text)  # caption text

    # you can also iterate over the lines of a particular caption
    for line in vtt[0].lines:
        print(line)

    # caption text is returned clean without class tags
    # we can access the raw text of a caption with raw_text
    >>> vtt[0].text
    'This is a caption text'
    >>> vtt[0].raw_text
    'This is a <c.colorE5E5E5>caption</c> text'

    # caption identifiers
    >>> vtt[0].identifier
    'chapter 1'


Reading WebVTT caption files from a file-like object
----------------------------------------------------

.. code-block:: python

    import webvtt
    import requests
    from io import StringIO

    payload = requests.get('http://subtitles.com/1234.vtt').text()
    buffer = StringIO(payload)

    for caption in webvtt.from_buffer(buffer):
        print(caption.start)
        print(caption.end)
        print(caption.text)


Reading WebVTT captions from a string
-------------------------------------

.. code-block:: python

    import webvtt
    import textwrap

    vtt = webvtt.from_string(textwrap.dedent("""
        WEBVTT

        00:00:00.500 --> 00:00:07.000
        Caption #1

        00:00:07.000 --> 00:00:11.890
        Caption #2 line 1
        Caption #2 line 2

        00:00:11.890 --> 00:00:16.320
        Caption #3
        """).strip()
        )

    for caption in vtt:
        print(caption.start)
        print(caption.end)
        print(caption.text)


Iterate a slice of captions
---------------------------

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions.vtt')
    for caption in vtt.iter_slice(start='00:00:11.000',
                                  end='00:00:27.000'
                                  )
        print(caption.start)
        print(caption.end)
        print(caption.text)


Creating captions
-----------------

.. code-block:: python

    from webvtt import WebVTT, Caption

    vtt = WebVTT()

    # creating a caption with a list of lines
    caption = Caption(
        '00:00:00.500',
        '00:00:07.000',
        ['Caption line 1', 'Caption line 2']
    )

    # an identifier can be assigned
    caption.identifier = 'chapter 1'

    # adding a caption
    vtt.captions.append(caption)

    # creating another caption with a text
    caption = Caption(
        '00:00:07.000',
        '00:00:11.890',
        'Caption line 1\nCaption line 2'
    )

    vtt.captions.append(caption)


Manipulating captions
---------------------

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions.vtt')

    # update start timestamp
    vtt[0].start = '00:00:01.250'

    # update end timestamp
    vtt[0].end = '00:00:03.890'

    # update caption text
    vtt[0].text = 'New caption text'

    # delete a caption
    del vtt.captions[2]


Saving captions
---------------

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions.vtt')

    # save to the same file
    vtt.save()

    # save to a different file
    vtt.save('new_captions.vtt')

    # you can save to a file path
    vtt.save('other/folder/new_captions')

    # if there is a filename present in the object we can target a folder
    vtt.save('other/folder)

    # write to an opened file
    with open('other_captions.vtt', 'w') as f:
        vtt.write(f)


Retrieving WebVTT formatted captions
------------------------------------

WebVTT content can be retrieved without an output file:

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions.vtt')

    # print the content in WebVTT format
    print(vtt.content)


Converting captions
-------------------

You can read captions from the following formats:

* SubRip (.srt)
* YouTube SBV (.sbv)

.. code-block:: python

    import webvtt

    # read captions from SRT format
    vtt = webvtt.from_srt('captions.srt')

    # save the captions in WebVTT format
    vtt.save()

    # the conversion can be done chaining the method calls
    webvtt.from_srt('captions.srt').save()

    # the same for SBV format
    vtt = webvtt.from_sbv('captions.sbv')

Convert WebVTT captions to other formats:

* SubRip (.srt)

.. code-block:: python

    import webvtt

    # save in SRT format
    vtt = webvtt.read('captions.vtt')
    vtt.save_as_srt()

    # write to an opened file in SRT format
    with open('captions.srt', 'w') as f:
        vtt.write(f, format='srt')


WebVTT files with Byte Order Mark (BOM)
---------------------------------------

When the WebVTT file has BOM, saving it will keep the BOM:

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions_with_bom.vtt')

    # saved file keeps the BOM
    vtt.save()


Add a BOM to a file without it:

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions_without_bom.vtt',
                      add_bom=True
                      )

    # saved file has BOM
    vtt.save()


Remove the BOM from a file:

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions_with_bom.vtt')

    # saved file does not have BOM
    vtt.save(add_bom=False)


Save file with a different encoding:

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions.vtt')

    vtt.save(encoding='utf-32-le')

    # save in different encoding with BOM
    vtt.save(encoding='utf-32-le',
             add_bom=True
             )



WebVTT Styles
-------------

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions.vtt')

    for style in vtt.styles:
        print(style.text)

        # retrieve list of lines
        print(style.lines)


Adding styles:

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions.vtt')

    vtt.styles.append(
        webvtt.Style('::cue(b) {\n  color: peachpuff;\n}')
        )
    # list of lines is supported
    vtt.styles.append(
        webvtt.Style(['::cue(b) {',
                      '  color: peachpuff;',
                      '}'
                      ])
        )


Updating styles:

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions.vtt')

    vtt.styles[0].lines[1] = '  color: papayawhip;'


WebVTT Comments
---------------

Comments can be added or retrieved from different items:

.. code-block:: python

    import webvtt

    vtt = webvtt.read('captions.vtt')

    # comments from the top of the file
    print(vtt.header_comments)

    # comments from the bottom of the file
    print(vtt.footer_comments)

    # comments in a style
    print(vtt.styles[0].comments)

    # comments in a caption
    print(vtt.captions[0].comments)

    # comments are just a list of strings
    vtt.captions[5].comments.append('caption for review')

