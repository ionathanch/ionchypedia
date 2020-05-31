=====================
Arch Package Managers
=====================

.. role:: bash(code)
  :language: bash

Pacman
------

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

Pamac
-----

.. list-table::
  :widths: auto
  :header-rows: 1

  * - Command
    - Description
  * - :bash:`pamac search (-a) [string]`
    - Search Manjaro repo (and AUR)
  * - :bash:`pamac install [package]`
    - Install package from Manjaro repo
  * - :bash:`pamac build [package]`
    - Install package from AUR
  * - :bash:`pamac upgrade (-a)`
    - Upgrade packages installed from Manjaro repo (and AUR)
  * - :bash:`pamac remove [package]`
    - Uninstall package
  * - :bash:`pamac remove -o`
    - Uninstall orphaned packages
