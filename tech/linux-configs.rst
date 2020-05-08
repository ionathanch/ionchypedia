======================================
Linux Customizations and Configurations
======================================

.. role:: bash(code)
  :language: bash

I frequently forget the various customizations I configure after setting up a distro installation, so this time around I've finally compiled a list.

General
-------

Configuration Files
^^^^^^^^^^^^^^^^^^

``.vimrc``
""""""""""
Use `sensible.vim <https://github.com/tpope/vim-sensible>`_ and add the following:

.. code:: vim

  colorscheme darkblue " desert, elflord, slate are nice too
  set shiftwidth=4     " 4-space tabs (reindenting)
  set tabstop=4        " 4-space tabs (viewing)
  set softtabstop=4    " 4-space tabs (editing)
  set expandtab        " tabs -> spaces
  set number           " line numbers
  set hlsearch         " highlight search matches
  set smartcase        " being smart about cases during searching

  " :W sudo saves the file
  " (useful for handling the permission-denied error)
  " https://github.com/amix/vimrc/blob/master/vimrcs/basic.vim
  command! W execute 'w !sudo tee % > /dev/null' <bar> edit!

``.bashrc``
"""""""""""
Add the following:

.. code:: bash

  # prepends execution of `cat` command with "meow" in yellow
  cat() {
    YELLOW='\033[1;33m'
    NC='\033[0m' # no colour
    echo -e "${YELLOW}meow${NC}";
    command cat "$@"
  }

  # "gimme gimme gimme" if `man` after midnight (half past twelve)
  man() {
    BLUE='\033[0;34m'
    NC='\033[0m' # no colour
    if [ $(date +%H%M) -eq 0030 ]; then
      echo -e "${BLUE}gimme gimme gimme${NC}"
    fi;
    command man "$@"
  }

``.i3/config``
"""""""""""""
Change the following (:bash:`$mod+Shift+c` to reload):

.. code:: ini

  bindsym $mod+q kill                                                 # close window
  # bindsym $mod+q split toggle                                       # I use $mod+h/+v anyway
  bindsym $mod+F2 exec firefox-developer-edition                       # replace Pale Moon
  bindsym $mod+Print --release exec --no-startup-id i3-scrot -s       # select area by default
  bindsym $mod+Shift+Print --release exec --no-startup-id i3-scrot -w # capture window on Shift
  focus_follows_mouse no                                              # click to focus window
  # arrange monitors correctly on startup
  exec --no-startup-id xrandr --output VGA1 --primary --auto --left-of HDMI1

``.i3status.conf``
""""""""""""""""""
Copy from ``/etc/i3/i3status.conf``. Refer to the `man page <https://i3wm.org/i3status/manpage.html>`_ and to `strftime <https://strftime.org/>`_ for time format strings.
  
``.Xresources``
"""""""""""""""
Change the following:

.. code:: ini

  ! set the URxvt terminal font
  URxvt.font:xft:Source Code Pro:size=10

``.inputrc``
""""""""""""
.. code:: bash

  set completion-ignore-case on # case-insensitive tab completion

Disabling middle-click paste
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
From https://unix.stackexchange.com/a/277488:

1. :bash:`sudo apt install xsel xbindkeys xdotool`
2. In ``~/.xbindkeysrc``, insert

.. code:: bash

  "echo -n | xsel -n -i; pkill xbindkeys; xdotool click 2; xbindkeys"
  b:2 + Release

3. In ``~/.profile``, insert :bash:`xbindkeys`
4. :bash:`source ~/.profile`

N.B. This will disable column selection in VSCode!


Customizations for Xubuntu
--------------------------

Aesthetic Considerations
^^^^^^^^^^^^^^^^^^^^^^^^
* GTK theme: Greybird-dark (Settings > Appearance > Style)
* Xfwm theme: Numix (Settings > Window Manager > Style)
* Icon theme: `ePapirus <https://github.com/PapirusDevelopmentTeam/papirus-icon-theme/>`_ (Settings > Appearance > Icons)
* Greeter theme: Greybird-dark (Settings > LightDM GTK+ Greeter Settings > Theme)
* Desktop background: ``/usr/share/xfce4/backdrops`` (Settings > Desktop)

Installed Programs
^^^^^^^^^^^^^^^^^^
Installed
"""""""""
* Vim, Git, GParted, Synaptic, Neofetch
* Tilix (set as default terminal in Settings > Preferred Applications; :bash:`tilix --preferences` to open Preferences)
* `VSCode <https://code.visualstudio.com/docs/setup/linux#_debian-and-ubuntu-based-distributions>`_
* `Spacemacs <https://github.com/syl20bnr/spacemacs#default-installation>`_
* Firefox Developer Edition (:bash:`sudo add-apt-repository ppa:ubuntu-mozilla-daily/firefox-aurora`)
* Inkscape (:bash:`sudo add-apt-repository ppa:inkscape.dev/stable`)
* Racket (:bash:`sudo add-apt-repository ppa:plt/racket`)
* `Minecraft <https://www.minecraft.net/en-us/download/alternative>`_
* `Source Code Pro <https://github.com/adobe-fonts/source-code-pro>`_ (copy into ``/usr/local/share/fonts/`` and run :bash:`sudo fc-cache -fv`)

Removed
"""""""
(via "Complete removal" using Synaptic)

* libreoffice\*, libuno\* (remove keyboard shortcuts in Settings > Keyboard > Application Shortcuts)
* xfce4-terminal, xfce4-notes, xfburn
* pidgin*, thunderbird
* gnome-mines, gnome-sudoku, sgt-puzzles
* simple-scan, mate-calc-common, gnome-font-viewer


Customizations for Manjaro i3
-----------------------------

Pacman Cheatsheet
^^^^^^^^^^^^^^^^^
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Command
    - Description
  * - :bash:`pacman -Syu [package]`
    - Sync, refresh, upgrade, install
  * - :bash:`pacman -Rsu [package]`
    - Remove recursively unneeded package
  * - :bash:`pacman -Qs [string]`
    - Search local database
  * - :bash:`pacman -Ss [string]`
    - Search sync database
  * - :bash:`pacman -Qttdq | pacman -Rs -`
    - Remove recursively all (optional) orphan dependencies quietly

Installed Programs
^^^^^^^^^^^^^^^^^^
* ``firefox-developer-edition`` (removed ``palemoon-bin``)
* ``code``, ``racket``
* ``source-code-pro-fonts``, ``otf-fira-code``
* ``neofetch``, ``lm_sensors``

``.profile``
^^^^^^^^^^^
.. code:: bash

  export EDITOR=/usr/bin/vim
  export BROWSER=/usr/bin/firefox-developer-edition
