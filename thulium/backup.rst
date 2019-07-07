=============================
Backing Up Data from Services
=============================

Ghost
-----
All content is under ``/srv/docker/ghost/content/``, with an additional
configuration file under ``/srv/docker/config.production.json``.
It should be sufficient to back up the entire ``/srv/docker/ghost/`` directory.

TTRSS
-----
All data is stored in a PostgreSQL database mounted on the ``ttrss_ttrssdb``
Docker volume. However, it would be easier to download the OPML via the
public URL and to restore from it, redoing the settings as needed.

Gitea
-----
All content is under ``/srv/docker/gitea/git/``, ``/srv/docker/gitea/gitea/``,
and ``/srv/docker/gitea/ssh/``. It should be sufficient to back up the entire
``/srv/docker/gitea/`` directory.

Nextcloud
---------

Funkwhale
---------

MediaWiki
---------
