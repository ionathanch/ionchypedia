.. role:: bash(code)
  :language: bash

I frequently forget the various customizations I configure after setting up a distro installation, so this time around I've finally compiled a list.

==========================
Customizations for Xubuntu
==========================

Aesthetic Considerations
------------------------
* GTK theme: Greybird-dark (Settings > Appearance > Style)
* Xfwm theme: Numix (Settings > Window Manager > Style)
* Icon theme: `ePapirus <https://github.com/PapirusDevelopmentTeam/papirus-icon-theme/>`_ (Settings > Appearance > Icons)
* Greeter theme: Greybird-dark (Settings > LightDM GTK+ Greeter Settings > Theme)
* Desktop background: ``/usr/share/xfce4/backdrops`` (Settings > Desktop)

Installed Programs
------------------
Installed
^^^^^^^^^
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
^^^^^^^
(via "Complete removal" using Synaptic)

* libreoffice\*, libuno\* (remove keyboard shortcuts in Settings > Keyboard > Application Shortcuts)
* xfce4-terminal, xfce4-notes, xfburn
* pidgin*, thunderbird
* gnome-mines, gnome-sudoku, sgt-puzzles
* simple-scan, mate-calc-common, gnome-font-viewer

Configuration Files
-------------------

``.vimrc``
^^^^^^^^^^
Use `sensible.vim <https://github.com/tpope/vim-sensible>`_ and add the following:

.. code:: vim

  colorscheme darkblue " desert, elflord, slate are nice too
  set shiftwidth=4    " 4-space tabs (reindenting)
  set tabstop=4       " 4-space tabs (viewing)
  set softtabstop=4   " 4-space tabs (editing)
  set expandtab       " tabs -> spaces
  set number          " line numbers
  set hlsearch        " highlight search matches
  set smartcase       " being smart about cases during searching

  " :W sudo saves the file
  " (useful for handling the permission-denied error)
  " https://github.com/amix/vimrc/blob/master/vimrcs/basic.vim
  command! W execute 'w !sudo tee % > /dev/null' <bar> edit!

``.bashrc``
^^^^^^^^^^^
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

=============================
Customizations for Manjaro i3
=============================

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
  * - :bash:`pacman -Qs [package]`
    - Search local database
  * - :bash:`pacman -Ss [package]`
    - Search sync database
  * - :bash:`pacman -Qttdq | pacman -Rs -`
    - Remove recursively all (optional) orphan dependencies quietly

``.i3/config``
^^^^^^^^^^^^^
Change the following:

.. code:: ini

  bindsym $mod+q kill                                                 # Close window
  # bindsym $mod+q split toggle                                       # I use $mod+h/+v anyway
  bindsym $mod+F2 exec firefox-developer-edition                       # Replace Pale Moon
  bindsym $mod+Print --release exec --no-startup-id i3-scrot -s       # Select area by default
  bindsym $mod+Shift+Print --release exec --no-startup-id i3-scrot -w # Capture window on Shift
