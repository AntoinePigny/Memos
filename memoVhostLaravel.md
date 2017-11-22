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
  ``php-m``

Install des differentes ext
----

  ```sudo apt-get install php7.0-xml php7.0-mbstring php7.0-mysql```

Installation en tant que commande systeme
----
  https://getcomposer.org/download
  ```
  Shell
  sudo php composer-setup.php --filename=composer --install-dir=/usr/local/bin

  composer create-project --prefer-dist laravel/laravel blog
  ```

Installation en tant que composant local (recommandé sur une VM)
----
  https://getcomposer.org/download
  ```
  Shell
  sudo php composer-setup.php

  ./composer.phar create-project --prefer-dist laravel/laravel blog
  ```

/etc/apache2/sites-available
Modification du vhost fichier .conf | cp 000-default.conf exemple.conf
----

    <VirtualHost *:80>
      ServerName www.exemple.com
      DocumentRoot /var/www/html/exemple
      <Directory /var/www/html/exemple/public>
        AllowOverride All
      </Directory>
    </VirtualHost>

```
Shell
sudo a2ensite exemple.conf

sudo service apache2 restart
```

Activation du module de réécriture d'url du serveur
----
```
Shell
sudo a2enmod rewrite

sudo service apache2 restart
```
