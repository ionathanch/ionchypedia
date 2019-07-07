=============
Miscellaneous
=============

.. role:: bash(code)
  :language: bash

.. role:: powershell(code)
  :language: powershell

Copying a control character to the clipboard (Windows)
------------------------------------------------------
For control character ``0x07``, for instance, execute
:powershell:`[char]7 | clip` in Powershell. For multiple characters, use
:powershell:`"$([char]7)$([char]...)"`. Alternatively, execute
:bash:`echo -e "\x07" > test` in Bash to put the character in the file ``test``
and then execute :powershell:`cat test | clip` in Powershell to paste it into
the clipboard.

Displaying Unicode characters in the terminal (Windows)
-------------------------------------------------------
Run :powershell:`chcp.com 65001`. See `here <https://ss64.com/nt/chcp.html>`__ for details.

Syntax highlighting in MediaWiki
--------------------------------
Template: ``<syntaxhighlight lang="" inline></syntaxhighlight>``
See `here <https://www.mediawiki.org/wiki/Extension:SyntaxHighlight>`__
for more details and `here <http://pygments.org/docs/lexers/>`__
for a list of supported languages.
