=================================
LaTeX Fixes for arXiv Submissions
=================================

.. role:: latex(code)
  :language: latex

These are the errors I encountered when creating a submission with arXiv by
uploading LaTeX files created in Overleaf. There are probably more proper ways
to fix them.
See `Considerations for TeX Submissions <https://export.arxiv.org/help/submit_tex>`__
and `Common Mistakes <https://arxiv.org/help/faq/mistakes>`__ for other errors.

.. list-table::
  :widths: auto
  :header-rows: 1

  * - Problem
    - Solution
  * - ``Package xkeyval Error: `printccs' undefined in families `@ACM@topmatter@'.``
    - Removed ``printccs`` from ``settopmatter``. I didn't have categories anyway.
  * - ``Undefined control sequence. <argument> \mdseries@tt``
    - Added :latex:`\makeatletter\providecommand{\mdseries@tt}{}\makeatother` *before* packages.
  * - ``Package minted Error: You must invoke LaTeX with the -shell-escape flag.``
    - Compiled with :latex:`\usepackage[finalizecache,cachedir=.]{minted}`,
      downloaded all ``*.pygtex`` and ``*.pygstyle`` files from the logs,
      uploaded them to the root directory, and compiled with
      :latex:`\usepackage[frozencache,cachedir=.]{minted}`.
  * - ``LaTeX Error: Option clash for package hyperref.``
    - Added override file ``00README.XXX`` with the line ``nohypertex``. arXiv loads HyperTeX by default, and acmart formats try to load them again.
  * - arXiv is unable to run ``graphviz``.
    - I just converted by DOT content to images beforehand and included them as graphics, but the proper solution would be to use ``dot2texi``.
  * - ``Unable to convert to PDF.``
    - Added :latex:`\pdfoutput=1` to the beginning of the main file.
