==============================
Setting Up a Static IP Address
==============================

.. role:: bash(code)
  :language: bash

1. Find the logical name of the network device: ::

    $ sudo lshw -class network -short
    H/W path         Device           Class       Description
    =========================================================
    /0/100/4/0       enp1s0           network     RTL810xE PCI Express Fast Ethernet controller

2. Edit ``/etc/network/interfaces`` to contain the following (where the IP address of the router begins with ``192.168.0.*``): ::

    auto enp1s0
    iface enp1s0 inet static
        address 192.168.0.69
        netmask 255.255.255.0
        network 192.168.0.0
        broadcast 192.168.0.255
        gateway 192.168.0.1
        dns-nameservers 8.8.8.8 8.8.4.4

  .. list-table::
    :widths: auto
    :header-rows: 1

    * - Field
      - Notes
    * - ``address``
      - The static IP address to be set, here set to \*.69
    * - ``netmask``
      - 0 bits indicate bits that can be used for the IP address; here, only the lower 256 bits can be used
    * - ``network``
      - First address in network, here set to \*.0
    * - ``broadcast``
      - Last address in network, here set to \*.255
    * - ``gateway``
      - IP address of the router
    * - ``dns-nameservers``
      - NS servers; 8.8.8.8 and 8.8.4.4 are Google's DNS servers

3. Run :bash:`sudo ifdown -v enp1s0 && sudo ifup -v enp1s0`
