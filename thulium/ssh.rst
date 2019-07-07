=================================
Setting up SSH from a New Machine
=================================

.. role:: ini(code)
  :language: ini

.. role:: bash(code)
  :language: bash

#. Generate an SSH key (defaulting to RSA 2048 bits): :bash:`ssh-keygen`
#. Temporarily enable password login on server by setting
   :ini:`PasswordAuthentication yes` in ``/etc/ssh/sshd_config``
#. Restart the SSH server: :bash:`sudo systemctl restart ssh`
#. Copy public SSH key to server: :bash:`ssh-copy-id user@ert.space`
#. Disable password login

Bonus:

* Copying a file via SSH: :bash:`scp ~/src.file user@ert.space:~/dest/location`
