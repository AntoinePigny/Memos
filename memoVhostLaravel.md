Pour installer LARAVEL
----
  - PHP 7.0
  - ext mbstring
  - ext pdo
  - ext tokenizer
  - ext xml
  - ext open ssl

Pour savoir quels modules sont disponibles dans son php
----
  ```bash
  php-m
  ```

Install des differentes ext
----

  ```bash
  sudo apt-get install php7.0-xml php7.0-mbstring php7.0-mysql
  ```

Installation en tant que commande systeme
----
  https://getcomposer.org/download
  ```bash
  sudo php composer-setup.php --filename=composer --install-dir=/usr/local/bin

  composer create-project --prefer-dist laravel/laravel blog
  ```

Installation en tant que composant local (recommandé sur une VM)
----
  https://getcomposer.org/download
  ```bash
  sudo php composer-setup.php

  ./composer.phar create-project --prefer-dist laravel/laravel blog
  ```


Modification du vhost fichier .conf | cp 000-default.conf exemple.conf
----
/etc/apache2/sites-available
    <!-- <VirtualHost *:80>
      ServerName www.exemple.com
      DocumentRoot /var/www/html/exemple
      <Directory /var/www/html/exemple/public>
        AllowOverride All
      </Directory>
    </VirtualHost> -->

```bash
sudo a2ensite exemple.conf

sudo service apache2 restart
```

Activation du module de réécriture d'url du serveur
----
```bash
sudo a2enmod rewrite

sudo service apache2 restart
```

Problème de permission sur storage
----

Machine locale :
  ```bash
  chmod -R 777 ./storage
  ```

Laravel Configuration Minimale
----

  ```bash
  sudo nano .env

    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel
    DB_USERNAME=root
    DB_PASSWORD="0000"
  ```

Laravel 5.5(docs)
----

https://laravel.com/api/5.5/Illuminate/Database/Schema/Blueprint.html
Exemple sur l'api LARAVEL pour le Blueprint

http://devdocs.io/
