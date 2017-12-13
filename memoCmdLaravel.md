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

Sample des differentes methodes necessaires.
----

Dans le model :
```php
class Cat extends Model
{
    protected $table = 'cats';
    public $timestamps = false;

    public function gender() {
        //peut etre utilisée en place de belongsTo, et sert pour une relation one2many
        return $this->hasOne('App\Gender', 'id', 'gender_id');
    }
    
    public function colors() {
        //cette methode sert a associer des données dans 2 tables liees par une table intermediaire (donc par relation many2many)
        return $this->belongsToMany('App\Color');
    }
}
```

Dans le controller :
```php
class CatController extends Controller
{
    public function insertOne(Request $request)
    {
        $cat = new Cat;
        //insérer les =/= propriétés du nouvel objet, en recuperant par leur name la valeur des inputs ($request)
        $cat->name = $request->name;
        $cat->gender_id = $request->gender;
        $cat->save();
        //pour une table intermediaire, utiliser la methode atach APRES le save car elle necessite l'id généré par le save
        $cat->colors()->attach($request->color);
        return redirect('/');
    }
    public function deleteOne(Request $request, $id)
    {
        //methode find pour retrouver un objet par l'id
        $cat = Cat::find($id);
        //détacher les couleurs, pour effacer les entrées dans la table intermediaire, et ensuite delete
        $cat->colors()->detach();
        $cat->delete();
        return redirect('/');
    }
    
    //Cette fonction sert a afficher la vue du formaulaire prérempli avec les infos deja existantes dans la bdd
    public function updateOne(Request $request, $id)
    {
        $cat = Cat::find($id);
        $gendersAll = Gender::all();
        //on génère un array vide que l'on peuple ensuite en passant à une clé chaque id de chaque entrée de gendersAll, 
        //puis en attribuant en valeur a chaque clé le gender qui correspond
        $genders = [];
        foreach ($gendersAll as $value) {
            $genders[$value->id] = $value->gender;
        }
        $colorsAll = Color::all();
        $colors = [];
        foreach ($colorsAll as $value) {
            $colors[$value->id] = $value->colors;
        }
        
        Pour return la view et y transmettre les variables, on les passe via la methode view, en argument associé a une string qque l'on pourra appeler dans ladite view
        return view('update', ['genders' => $genders, 'colors' => $colors, 'cat' => $cat]);
    }
    
    
    //Cette méthode effectue l'action d'update dans la bdd
    public function updateOneAction(Request $request)
    {
        $cat = Cat::find($request->id);
        $cat->name = $request->name;
        $cat->size = $request->size;
        $cat->weight = $request->weight;
        $cat->age = $request->age;
        $cat->gender_id = $request->gender;
        $cat->save();
        //on detach les anciennes valeurs de la table interm de la bdd puis on y attache les nouvelles
        $cat->colors()->detach();
        $cat->colors()->attach($request->colors);
        return redirect('/');
    }
}

```

Exemples de vues et de templating de blade
----

#### Template de base (layout) répété sur les autres views
```blade
<!doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport"
              content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <link href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-PsH8R72JQ3SOdhVi3uxftmaW6Vc51MKb0q5P2rRUpPvrszuE4W1povHYgTpBfshb" crossorigin="anonymous">
        <link href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet" integrity="sha384-wvfXpqpZZVQGK6TAh5PVlGOfQNHSoD2xbE+QkPxCAFlNEevoEH3Sl0sibVcOQVnN" crossorigin="anonymous">
        <link rel="stylesheet" href="{{ asset('css/style.css') }}" type="text/css">
        <title>@yield('title')</title>
    </head>
    <body>
        <header>
            @include('layouts.menu')
        </header>
        <main>
            @yield('main')
        </main>
        <footer>

        </footer>
        <script src="{{ asset('js/script.js') }}"></script>
        <script src="https://code.jquery.com/jquery-3.2.1.min.js" integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4=" crossorigin="anonymous"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/js/bootstrap.bundle.min.js" integrity="sha384-3ziFidFTgxJXHMDttyPJKDuTlmxJlwbSkojudK/CkRqKDOmeSbN6KLrGdrBQnT2n" crossorigin="anonymous"></script>
    </body>
</html>
```

####Exemples de view spécifiques avec appel de layout et formulaire Laravel Collective
```blade
@extends('layouts.base')

@section('title', 'Accueil')
@section('main')
    <h1>Cat's Eyes</h1>
    <table class="table">
        <thead>
        <tr>
            <th>Name</th>
            <th>Size (cm)</th>
            <th>Weight (kg)</th>
            <th>Age (yrs)</th>
            <th>Gender</th>
            <th>Color(s)</th>
            <th>Delete</th>
            <th>Upuh-datuh !!</th>
        </tr>
        </thead>
        <tbody>
        @foreach($cats as $cat)
        <tr>
            <td>{{ $cat->name }}</td>
            <td>{{ $cat->size }}</td>
            <td>{{ $cat->weight }}</td>
            <td>{{ $cat->age }}</td>
            @if($cat->gender)
            <td>{{ $cat->gender->gender }}</td>
            @else <td>castré</td>
            @endif
            <td>
                @foreach($cat->colors as $color)
                    <span>{{$color->colors}}</span>
                @endforeach
            </td>
            <td>
                <form method="get" action="/newcat/delete/{{$cat->id}}">
                {{ csrf_field() }}
                    <button type="submit" class="btn btn-outline-info">
                        <i class="fa fa-trash-o" aria-hidden="true"></i>
                    </button>
                </form>
            </td>
            <td>
                <form method="get" action="/newcat/update/{{$cat->id}}">
                {{ csrf_field() }}
                    <button type="submit" class="btn btn-outline-info">
                        <i class="fa fa-pencil" aria-hidden="true"></i>
                    </button>
                </form>
            </td>
        </tr>
        @endforeach
        </tbody>
    </table>
@endsection
```

```blade
@extends('layouts.base')
@section('title', 'New Cat')
@section('main')
    <section class="container">

        <h1 class="col-12">Modifiez un chat</h1>
        {!! Form::open(['url' => '/newcat/update']) !!}
           {{{ Form::hidden('id', $cat->id) }}}
        <div class="col-12 form-group">
            {{{ Form::label('Nom') }}}
            {{{ Form::text('name', $cat->name, ['class' => 'form-control']) }}}
        </div>
        <div class="col-12 form-group">
            {{{ Form::label('Couleur') }}}
            {{{ Form::select('colors[]', $colors, $cat->colors, ['size' => count($colors), 'multiple' => true], ['class' => 'form-control']) }}}
        </div>
        <div class="col-12 form-group">
            {{{ Form::label('Sexe') }}}
            {{{ Form::select('gender', $genders, $cat->gender->id, [], ['class' => 'form-control']) }}}
        </div>
        <div class="col-12 form-group">
            {{{ Form::label('Taille') }}}
            {{{ Form::number('size', $cat->size, ['class' => 'form-control']) }}}
        </div>
        <div class="col-12 form-group">
            {{{ Form::label('Age') }}}
            {{{ Form::number('age', $cat->age, ['class' => 'form-control']) }}}
        </div>
        <div class="col-12 form-group">
            {{{ Form::label('Poids') }}}
            {{{ Form::number('weight', $cat->weight, ['class' => 'form-control']) }}}
        </div>
        <div class="col-12 form-group">
            {{{ Form::submit('Enregistrer') }}}
        </div>
        {!! Form::close() !!}
    </section>

@endsection
```

Exemple de routes avec appels aux controllers
----

```php
    Route::get('/','BaseController@index');
    
    Route::get('/newcat','CreateController@index');
    
    Route::post('/newcat/insert', 'CatController@insertOne');
    Route::post('/newcat/update', 'CatController@updateOneAction');
    
    Route::get('/newcat/delete/{id}', 'CatController@deleteOne');
    Route::get('/newcat/update/{id}', 'CatController@updateOne');
```