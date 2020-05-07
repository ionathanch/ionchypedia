UBC Alumni Email Syncing on Android
===================================

The instructions for syncing UBC's alumni email account on Android devices `here <https://it.ubc.ca/services/email-voice-internet/student-alumni-email-service/setup-documentation>`_ are quite outdated. The following is what has worked for me on Android 10.

Account Info
------------
Email
^^^^^
``[local-part]@alumni.ubc.ca`` (this is the email address you have registered `here <https://id.ubc.ca/bpe/>`_)

Password
^^^^^^^^
Your CWL password.

Client Certificate
^^^^^^^^^^^^^^^^^^
``None`` (default)

Server Settings
---------------
Domain\\Username
^^^^^^^^^^^^^^^
``EAD\[CWL username].stu``

Server
^^^^^^
``activesync.alumni.ubc.ca``

Port
^^^^
``443`` (default)

Security Type
^^^^^^^^^^^^^
``SSL/TLS`` (apparently "accept all certificates" is no longer needed)
