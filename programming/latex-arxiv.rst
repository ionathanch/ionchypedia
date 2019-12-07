=================================
LaTeX Fixes for arXiv Submissions
=================================

.. role:: latex(code)
  :language: latex

These are the errors I encountered when creating a submission with arXiv by
uploading LaTeX files. There are probably more proper ways to fix them.
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
    - I don't think there's any way around this, since syntax highlighting requires Pygments. Used ``\usepackage[draft]{minted}`` to disable this.
  * - ``LaTeX Error: Option clash for package hyperref.``
    - Added override file ``00README.XXX`` with the line ``nohypertex``. arXiv loads HyperTeX by default, and acmart formats try to load them again.
