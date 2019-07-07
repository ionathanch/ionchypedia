===================
Setting Up Services
===================

.. role:: bash(code)
  :language: bash

Docker
------

Docker Compose template
^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: yaml

    version: "3"

    services:
      example:
        build: .
        image: author/example:tag
        container_name: example
        restart: always
        environment:
          - ENV_VAR=value
        ports:
          - 8000:80
        volumes:
          - /unnamed/volume/will/be/created
          - myvolume:/var/www/html
          - /srv/data/host/location:/srv/data
        networks:
          - mynetwork
        depends_on:
          - db
      db:
        image: postgres
        container_name: example-db
        restart: always
        expose:
          - 5432
        volumes:
          - example-db:/var/lib/postgresql/9.3./main

    networks:
      mynetwork:

    volumes:
      myvolume:
      example-db:

Useful Docker (and related) commands
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Task
    - Command
  * - Shutting down and restarting a detached container
    - :bash:`docker-compose down && docker-compose up -d`
  * - Entering into a running Docker container
    - :bash:`docker exec -it containername bash`
  * - Copying files into a running Docker container
    - :bash:`docker cp ~/location/on/host containername:/var/www/html`

Nginx
-----

Nginx templates
^^^^^^^^^^^^^^^

Static web page
"""""""""""""""
The URL https://example.ert.space will point to files in :code:`/srv/www/example.ert.space`.

.. code-block:: nginx

  server {
    listen 80;
    listen [::]:80;
    root /srv/www/example.ert.space;
    server_name example.ert.space;
    error_page 404 /404.html;
    location / {
      try_files $uri $uri/ =404;
    }
  }

Reverse proxy
"""""""""""""
The URL https://example.ert.space will point to the `local port 8080 <http://localhost:8000>`__.

.. code-block:: nginx

  server {
    listen 80;
    listen [::]:80;
    server_name example.ert.space;
    location / {
      proxy_pass http://localhost:8080;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
    }
  }

PHP site with HTTP authentication
"""""""""""""""""""""""""""""""""

The URL https://example.ert.space will point to the PHP application at :code:`/srv/www/example.ert.space/index.php`. HTTP authentication done as indicated `here <https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/>`__.

.. code-block:: nginx

  server {
    listen 80;
    listen [::]:80;
    server_name example.ert.space;

    root /srv/www/example.ert.space;
    index index.html index.php;

    # set up HTTP basic authentication
    auth_basic           "Authentication Required";
    auth_basic_user_file /etc/apache2/.htpasswd;

    location / {
      try_files $uri $uri/ =404;
    }

    # process PHP requests
    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
      deny all;
    }
  }

Useful Nginx (and related) commands
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. list-table::
  :widths: auto
  :header-rows: 1

  * - Task
    - Command
  * - Softlinking from sites available to sites enabled (while in sites-enabled)
    - :bash:`sudo ln -s /etc/nginx/sites-available/example.ert.space example.ert.space`
  * - Restarting Nginx
    - :bash:`sudo systemctl restart nginx`
  * - Listing SSL certificates
    - :bash:`sudo certbot certificates`
  * - Adding a domain to an SSL certificate (note that all old domains must be included as well)
    - :bash:`sudo certbot --nginx --expand -d ert.space,example.ert.space`
