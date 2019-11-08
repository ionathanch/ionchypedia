================
Display Managers
================

.. role:: bash(code)
  :language: bash

.. role:: cfg(code)
  :language: cfg

General
-------
* Display managers: Use :bash:`sudo dpkg-reconfigure {lightdm,sddm,gdm,...}`
* Sessions: ``/usr/share/xsessions/``
* Greeters: ``/usr/share/xgreeters/``

LightDM
-------

Changing the greeter
^^^^^^^^^^^^^^^^^^^^

#. Install and set up the greeter
    - LightDM GTK+ Greeter: Use ``lightdm-gtk-greeter-settings``
    - Slick Greeter: Use ``lightdm-settings``
#. Set :cfg:`greeter-session={lightdm-gtk-greeter,slick-greeter,...}`
   under :cfg:`[Seat:*]` in ``/etc/lightdm/lightdm.conf``
   (see `here <https://github.com/CanonicalLtd/lightdm/blob/master/data/lightdm.conf>`__ for more configuration settings)
#. Use :bash:`sudo systemctl restart lightm` to reload greeter,
   :bash:`dm-tool switch-to-greeter` to see the greeter after making
   configuration changes

GDM
---
* Removing Wayland: Set :cfg:`WaylandEnable=false` in ``/etc/gdm3/custom.conf``
