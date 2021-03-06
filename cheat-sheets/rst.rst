================
reStructuredText
================

Other cheat sheets
------------------
* https://github.com/ralsina/rst-cheatsheet/blob/master/rst-cheatsheet.rst
* http://docutils.sourceforge.net/docs/user/rst/cheatsheet.txt

Headings
--------

.. code:: rst

  #####
  Parts
  #####

  ********
  Chapters
  ********

  ========
  Sections
  ========

  Subsections
  -----------

  Subsubsections
  ^^^^^^^^^^^^^^

  Paragraphs
  """"""""""

Code snippets
-------------

Unhighlighted
^^^^^^^^^^^^^
Inline code snippets look like ````this````.

::

  Code blocks are done ::
    like this.
    (The indented text is what will be in the block.)

Highlighted
^^^^^^^^^^^
To highlight an inline snippet in, for instance, Bash, register a new role:

.. code:: rst

  .. role:: bash(code)
    :language: bash

Then use the role ``:bash:`echo "like this"```. More information on roles can be found `here <http://docutils.sourceforge.net/docs/ref/rst/roles.html>`__.

To highlight a code snippet, use the ``code`` directive:

.. code:: rst

  .. code:: bash

    echo "Like this."

More information on directives can be found `here <http://docutils.sourceforge.net/docs/ref/rst/directives.html>`__.

A list of valid languages can be found `here <http://pygments.org/docs/lexers/>`__.

Tables
------
For tables with long entries, it is easier to use the ``list-table`` syntax.

.. code:: rst

  .. list-table:: Table Name
    :widths: auto
    :header-rows: 1
    :stub-columns: 1

    * - This and
      - this is part of
      - the header row.
    * - This is a column header.
      - This is part of
      - the second row.

.. list-table:: Table Name
  :widths: auto
  :header-rows: 1
  :stub-columns: 1

  * - This and
    - this is part of
    - the header row.
  * - This is a column header.
    - This is part of
    - the second row.

See `here <http://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#table-directives>`__ for all the table directives.

Images
------
The logo on the front-page README is inserted by the following: ::

  .. image:: ionchypedia.png
    :alt: i⚛️nchypedia in rainbow text over a black background
    :width: 100%

Newlines
--------
Use the following directive in order to use ``|br|`` to insert newlines:

.. code:: rst

  .. |br| raw:: html

    <br/>
