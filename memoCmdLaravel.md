Migration :
----
	  PATH : database/migrations/<date>_<action>_<table>_table.php
	  CMD  : php artisan make:migration <action>_<table>_table
    TYPE : Objet PHP
    DOCS : https://laravel.com/docs/5.5/migrations
	  USE  : Versionning de Base de données

Factory :
----
	  PATH : database/factories/<model>Factory.php
    CMD  : php artisan make:factory <model>Factory
    TYPE : PHP
    DOCS : https://laravel.com/docs/5.5/database-testing#generating-factories
	  USE  : Template de génération de données pour le TEST

Seeder :
----
	  PATH : database/seeds/<model>
    CMD  : php artisan make:seeder <model>TableSeeder
    TYPE : PHP Objet
    DOCS : https://laravel.com/docs/5.5/seeding
	  USE  : Peupler la bdd avec des données (possibilité d'utiliser des factories)

Model :
----
    PATH : app/<model>.php
    CMD  : php artisan make:model <model>
    TYPE : PHP Objet
    DOCS : https://laravel.com/docs/5.5/eloquent
    USE  : Représentation d'un enregistrement de la bdd au format PHP Objet

Controller :
----
	  PATH : app/Http/Controllers/<model>Controller.php
    CMD  : php artisan make:controller <model>Controller
    TYPE : PHP Objet
    DOCS : https://laravel.com/docs/5.5/controllers
	  USE  : Relation entre le Model et la Vue (code métier)

View :
----
	  PATH : ressources/views/<model>/<name>.blade.php
    CMD  : null
    TYPE : HTML /BLADE
    DOCS : https://laravel.com/docs/5.5/views
	  USE  : Faire l'affichage des données

Cheminement de l'affichage d'une page
----
Browser -> URL -> Apache2 -> index.php
							 -> Router -> Controller@method
							 -> Code Métier
							 -> View
							 -> HTML
Browser <-



Quick start laravel avec serveur dev
----

Git clone
composer install
php artisan serve
