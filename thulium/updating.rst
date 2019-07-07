=================
Updating Services
=================

.. role:: bash(code)
  :language: bash

Nextcloud
---------
#. :bash:`docker-compose down`
#. :bash:`docker pull nextcloud`
#. :bash:`docker-compose up -d`
#. Remember to delete unused Docker images!

Ghost
-----
#. Check the current version of Ghost at https://ert.space/ghost/#/about.
#. :bash:`docker-compose down`
#. :bash:`docker pull ghost`
    * If the latest major version differs from the current major version,
      first update to the latest minor version of the current major version,
      then update to the latest image.
#. :bash:`docker-compose up -d`
#. Remember to delete unused Docker images!

Gitea
-----
#. :bash:`docker-compose down`
#. :bash:`docker pull gitea/gitea`
#. Update ``$GITEA/options/locale/locale_en-US.ini`` (especially ``app_desc``) and ``$GITEA/templates/home.tmpl`` (especially ``AppName``) as necessary from https://github.com/go-gitea/gitea
#. :bash:`docker-compose up -d`
#. Remember to delete unused Docker images!

Funkwhale
---------
Official instructions are `here <https://docs.funkwhale.audio/admin/upgrading.html#docker-setup>`__.

#. :bash:`docker-compose down`
#. :bash:`docker pull funkwhale/all-in-one:$VERSION`
#. Edit the Docker Compose file to use the new image.
#. :bash:`docker-compose up -d`
#. Remember to delete unused Docker images!

MediaWiki
---------
Follow `these <https://www.mediawiki.org/wiki/Manual:Upgrading#Web_updater>`__ instructions. Alternatively, simply use https://wiki.ert.space/mw-config/.

TTRSS
-----
The ``x86dev/docker-ttrss`` image should automatically install updates.
