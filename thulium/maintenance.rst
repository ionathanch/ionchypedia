=============================
Maintenance and Configuration
=============================

.. role:: bash(code)
  :language: bash

.. role:: ini(code)
  :language: ini

.. role:: json(code)
  :language: json

.. role:: php(code)
  :language: php

.. role:: ruby(code)
  :language: ruby

.. role:: sql(code)
  :language: sql

General
-------
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Task
    - Command
  * - Update DDNS record
    - :bash:`curl "https://dynamicdns.park-your-domain.com/update?host=@&domain=ert.space&password=<REDACTED>"`
  * - Renew SSL certificate
    - :bash:`sudo certbot renew && sudo systemctrl restart nginx`
  * - Update UFW firewall rules
    - ..  code:: bash

          sudo ufw allow 22000/tcp # add TCP port 22000
          sudo ufw reload # enable new rules
          sudo ufw status # check active rules
          sudo ufw delete allow 22000/tcp # remove TCP port 22000

TTRSS
-----
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Modification
    - Location
  * - Database configuration (:bash:`SELF_URL_PATH`, etc.)
    - ``/var/www/ttrss/config.php``

Gitea
-----
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Modification
    - Location
  * - Configuration settings
    - ``$GITEA/conf/app.ini`` (see `here <https://github.com/go-gitea/gitea/blob/master/custom/conf/app.ini.sample>`__ for configurable values)
  * - Git clone SSH port
    - :ini:`SSH_PORT = port` in ``app.ini`` (sets clone url to `<ssh://git@ert.space:port/user/repo.git>`__)
  * - Home page
    - ``$GITEA/gitea/templates/home.templ``
  * - Localized strings
    - ``$GITEA/gitea/options/locale/local_en-US.ini``
  * - CSS styling
    - ``$GITEA/gitea/public/css/index.css``
  * - Other
    - Copy files as necessary from the `Gitea repository <https://github.com/go-gitea/gitea>`__ under ``custom/conf``, ``options``, ``public``, and ``templates``
    
Nextcloud
---------
.. list-table::
  :widths: auto
  :header-rows: 1
  
  * - Modification
    - Location
  * - HTTPS protocol for reverse proxies
    - Add :php:`'overwriteprotocol' => 'https' in config/config.php` to :php:`$CONFIG` array in ``config/config.php``
  * - Running an :php:`occ` command
    - :bash:`docker exec -u www-data nextcloud php occ $command`
  * - Manually removing file locks
    - 1. :bash:`sudo su` to root and :bash:`sqlite3 data/owncloud.db`
      2. :sql:`DELETE FROM oc_file_locks WHERE lock=-1;`
      3. CTRL-d or :sql:`.quit` to leave SQLite and :bash:`exit` to leave root

Ghost
-----
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Modification
    - Location
  * - URL
    - :json:`url` in ``$GHOST/config.production.json``
  * - Theme
    - Unzip into ``$GHOST/content/themes/themename``
  * - Custom JS
    - ``Settings -> Code injection``

MediaWiki
---------
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Task
    - Commands
  * - Changing the logo
    -  #. Upload logo to ``$MEDIAWIKI/html/resources/assets/logo.png``
       #. Modify :php:`$wgLogo = "$wgResourceBasePath/resources/assets/logo.png";` in ``$MEDIAWIKI/html/LocalSettings.php``
       #. :bash:`docker exec mediawiki php /var/www/html/maintenance/rebuildImages.php`
  * - Making pages show up in categories
    - :bash:`docker exec mediawiki php /var/www/html/maintenance/refreshLinks.php`
  * - Adding extensions
    - ``TODO``, add :php:`wfLoadExtension( 'ExtensionName' );` in ``$MEDIAWIKI/html/LocalSettings.php``

Funkwhale
---------
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Task
    - Commands
  * - Importing music
    - #. Copy music to ``$FUNKWHALE/music``
      #. :bash:`docker exec -it funkwhale manage import_files e4ddd50e-ae64-4390-861b-28a4338b5de7 "/music/*.mp3" --in-place --recursive --broadcast`

      See `docs <https://docs.funkwhale.audio/admin/importing-music.html#in-place-import>`__ for more details and :bash:`docker exec -it funkwhale manage import_files --help` for more commands.

Standard Notes
--------------
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Modification
    - Location
  * - Disabling registration
    - Comment out :ruby:`post "auth" => "api/auth#register"` in ``$STDNOTES/config/routes.rb``
  * - Other
    - See `docs <https://github.com/standardfile/ruby-server/wiki/Deploying-a-private-Standard-File-server-using-Docker>`__
