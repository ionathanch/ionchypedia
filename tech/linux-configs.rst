=======================================
Linux Customizations and Configurations
=======================================

.. role:: bash(code)
  :language: bash

.. role:: json(code)
  :language: json

I frequently forget the various customizations I configure after setting up a distro installation, so this time around I've finally compiled a list.

Configuration Files
-------------------

``.vimrc``
^^^^^^^^^^
Use `sensible.vim <https://github.com/tpope/vim-sensible>`_ and add the following:

.. code:: vim

  colorscheme peachpuff " desert, slate are nice too
  set shiftwidth=4      " 4-space tabs (reindenting)
  set tabstop=4         " 4-space tabs (viewing)
  set softtabstop=4     " 4-space tabs (editing)
  set expandtab         " tabs -> spaces
  set number            " line numbers
  set hlsearch          " highlight search matches
  set smartcase         " being smart about cases during searching
  set mouse=a           " click to place cursor

  " Map register "+ to system secondary clipboard
  " and set ctrl-c/ctrl-x as copy/cut
  " N.B. Pasting will still require ctrl-shift-v
  set clipboard=unnamedplus
  vmap <C-C> "+y
  vmap <C-X> "+d

  " :W sudo saves the file
  " (useful for handling the permission-denied error)
  " https://github.com/amix/vimrc/blob/master/vimrcs/basic.vim
  command! W execute 'w !sudo tee % > /dev/null' <bar> edit!

``.bashrc``, ``.zshrc``
^^^^^^^^^^^^^^^^^^^^^^^
Add the following (optional):

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

``.profile``
^^^^^^^^^^^^
Change the following to set default applications:

.. code:: bash

  export EDITOR=/usr/bin/vim
  export BROWSER=/usr/bin/firefox-developer-edition

``.Xresources``
^^^^^^^^^^^^^^^
Change the following and use :bash:`xrdb ~/.Xresources` to reload:

.. code:: ini

  ! set the XTerm terminal font
  XTerm*faceName:   Source Code Pro
  XTerm*faceSize:   10
  ! set the URxvt terminal font
  URxvt.font:xft:   Source Code Pro:size=10

``.inputrc``
^^^^^^^^^^^^
Add the following (bash only):

.. code:: bash

  set completion-ignore-case on # case-insensitive tab completion

``.i3/config``
^^^^^^^^^^^^^^
Change and add the following (:bash:`$mod+Shift+c` to reload):

.. code:: ini

  bindsym $mod+q kill                                                 # close window
  # bindsym $mod+q split toggle                                       # I use $mod+h/+v anyway
  bindsym $mod+F2 exec firefox-developer-edition                      # replace Pale Moon
  bindsym $mod+Print --release exec --no-startup-id i3-scrot -s       # select area by default
  bindsym $mod+Shift+Print --release exec --no-startup-id i3-scrot -w # capture window on Shift
  focus_follows_mouse no                                              # click to focus window

  # audio settings
  # use `pactl list sinks short` to list sinks
  bindsym $mod+Ctrl+m exec pavucontrol
  bindsym XF86AudioMute exec "pactl set-sink-mute 0 toggle"

  # arrange monitors correctly on startup
  # use `xrandr -q` to list monitors
  # this should go in /etc/lightdm/lightdm.conf
  # under `display-setup-script` as well
  exec --no-startup-id xrandr --output HDMI-1 --primary --auto --left-of DVI-I-1

  # start IBus and Nextcloud on startup
  exec --no-startup-id ibus-daemon -drx
  exec --no-startup-id nextcloud

  # set natural scrolling
  # use `xinput list` to list devices
  exec --no-startup-id xinput --set-prop "Elan Touchpad" "libinput Natural Scrolling Enabled" 1
  
  # open file manager in floating mode
  for_window [class="Pcmanfm"] floating enable

``.i3status.conf``
^^^^^^^^^^^^^^^^^^
Copy from ``/etc/i3status.conf``. Refer to the `man page <https://i3wm.org/i3status/manpage.html>`_ and to `strftime <https://strftime.org/>`_ for time format strings. Use :bash:`$mod+Shift+r` to reload.

``/etc/pulse/client.conf``
^^^^^^^^^^^^^^^^^^^^^^^^^^
Uncomment out the following to start PulseAudio on boot:

.. code:: ini

  autospawn = yes
  daemon-binary = /usr/bin/pulseaudio
  extra-arguments = --log-target=syslog

``/etc/pulse/default.pa``
^^^^^^^^^^^^^^^^^^^^^^^^^
Use :bash:`aplay -l` to list devices and :bash:`aplay -D plughw:c,d /usr/share/sounds/alsa/Noise.wav` to test card ``c``, device ``d``.
Add the following to enable playback through the given device.

.. code:: ini

  load-module module-alsa-sink device=hw:c,d

``/usr/lib/code/product.json``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The open-source version of Visual Studio Code uses a different extensions gallery.
To restore it to the usual gallery, in this file, replace the ``extensionsGallery`` object:

.. code:: json

  "extensionsGallery": {
    "serviceUrl": "https://marketplace.visualstudio.com/_apis/public/gallery",
    "itemUrl": "https://marketplace.visualstudio.com/items"
  }

Disabling Middle-Click Paste
----------------------------
From https://unix.stackexchange.com/a/277488:

#. Install ``xsel``, ``xbindkeys``, ``xdotool``
#. In ``~/.xbindkeysrc``, insert

   .. code:: bash

     "echo -n | xsel -n -i; pkill xbindkeys; xdotool click 2; xbindkeys"
     b:2 + Release

#. In ``~/.profile``, insert :bash:`xbindkeys`
#. :bash:`source ~/.profile`
 
Customizations for Xubuntu
--------------------------

Theming
^^^^^^^
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

Customizations for Manjaro
--------------------------

The below are for both the i3 and KDE Plasma DEs, unless otherwise specified.

Installed Programs
^^^^^^^^^^^^^^^^^^
* ``firefox-developer-edition`` (removed ``palemoon-bin``)
* ``gvim`` (replaced ``vim``), ``code`` (removed ``mousepad``), ``racket``
* ``texlive-most`` (``-core``, ``-bibtexextra``, ``-latexextra``, ``-fontsextra``, ``-science``), ``tllocalmgr-git`` (AUR)
* ``source-code-pro-fonts``, ``ttf-fira-code``, ``noto-fonts-cjk``, ``noto-fonts-emoji``
* ``manjaro-printer``, ``system-config-printer``
* ``ibus``, ``ibus-libpinyin``, ``ibus-table-others`` (AUR)
* ``neofetch``, ``lm_sensors``, ``lightdm-settings``, ``earlyoom``, ``pulseaudio``, ``networkmanager``
* ``minecraft-launcher`` (AUR), ``steam-manjaro``, ``zoom`` (AUR)
* ``plasma-desktop``, ``kmix``, ``kscreen``, ``kimtoy``, ``powerdevil``, ``bluedevil``, ``plasma-nm``, ``spectacle`` (for KDE Plasma)
