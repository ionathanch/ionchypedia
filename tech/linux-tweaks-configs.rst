=========================
Linux Tweaks and Configurations
=========================

.. role:: bash(code)
  :language: bash

Disabling middle-click paste
----------------------------
From https://unix.stackexchange.com/a/277488:

1. :bash:`sudo apt install xsel xbindkeys xdotool`
2. In ``~/.xbindkeysrc``, insert

  .. code:: bash

    "echo -n | xsel -n -i; pkill xbindkeys; xdotool click 2; xbindkeys"
    b:2 + Release

3. In ``~/.profile``, insert :bash:`xbindkeys`
4. :bash:`source ~/.profile`

N.B. This will disable column selection in VSCode!

Adding a user photo
-------------------
Copy image in PNG format to ``~/.face``

Enabling case-insensitive tab completion in Bash
------------------------------------------------
Add :bash:`set completion-ignore-case on` to ``~/.inputrc``
