Laravel is a PHP web framework who uses the MVC architecture.

## MVC

- M → Model
    - Deals with database
- V → View
    - Deals with the HTML
- C → Controller
    - The middle-man

## Tools

[codingfaculty.com](http://codingfaculty.com) → for extra help

- Windows
    - XAMP
        - provides apache and mysql
        - localhost/dashboard
    - PhpStorm/VsCode
    - Git
    - Composer
        - dependency manager for php
    - MySQL/MariaDB
        - localhost/phpmyadmin
    - Node.js
        - JS runtime
        - Laravel often uses Vue.js
    - Laravel
        - Different installation options
            - laravel installer with `laravel new blog`
            - composer with `composer create-project —prefer-dist laravel/laravel blog`
            - artisan is a laravel cli
- Mac (use homebrew)
    - install php (version  ≥ 7.2)
    - install mysql and follow homebrew prompts to start sql server
    - TablePlus for GUI mySQL
    - mySQL is at localhost:3306
        - mysql cli commands:
            - `mysql -uroot -p` → to log into mysql cli client
            - `show databases;` → to show databases
            - `create database <name>;` → creates database
            - `exit` → to exit
    - composer
    - `php artisan serve` starts dev server
    - Instructor uses Vagrant and Homestead in some parts of the course, but `php artisan serve` is good enough

## Routes

ATTENTION: Routes are in the root, but in the course routes are in the app

### Laravel Structure Overview

ATTENTION: Use Laravel 8 to match up with the course instead of 7.

```bash
composer create-project --prefer-dist laravel/laravel:^8.0 blog
```

- app
    - models and controllers
- bootstrap
    - start of the application
- config
    - config per category
- database
    - tables
- public
    - html and js files
- resources
    - files that need to compiled
        - js, sccss…
        - views
    - webpack ( now uses vite)
- routes
    - web.php for most of the routes
    - api.php for endpoints that are not public
- vendor → dependencies?
- .env → sensitive stuff
- artisan → generates tables, controllers, etc.
- composer.json
- package.json

`Route::get(’/’, function () {});`

Two colons `::` is a [scope resolution](https://www.php.net/manual/en/language.oop5.paamayim-nekudotayim.php) operator like in C++

Routes can be named, and those names can be accessed with helper `route()` function.

```php
// Named route
Route::get('admin/posts/example',array ( 'as'=>'admin.home', function () {
    $url = route('admin.home');
    return "this url is ". $url;
}));
```

## Controllers

[namespaces](https://www.php.net/manual/en/language.namespaces.rationale.php) encapsulate items to avoid name collision

[use](https://www.php.net/manual/en/language.namespaces.importing.php) imports a namespace. Can be aliased with `as`

### Creating Controllers Options

- create new file in app/Http/Controllers name is in pascal case
- use `php artisan make:controller <name>`
    - with resources `php artisan make:controller --resource PostsController`

When routing controllers to parameterized routes use [implicit binding](https://laravel.com/docs/8.x/routing#implicit-binding).

Import controller namespace into routes file to connect routes to controllers.

### Resource routes

In Laravel 8: `Route*::*resource('posts', PostsController*::class*);`

List routes from cli with `php artisan route:list`

## Views

Views live in resources/views/

### Passing parameters to views

`view('post')*->*with('id', $id);`

or for multiple 

`view('post', compact('id', 'id2'))`

## Blade

Templating engine for php

`@yield(’section name’)` → to define partial location 

And to create a partial html:

```php
@extends('layouts.app')

@section('content')
<h1>Contact Page</h1>
@stop
```

## Migrations

environment variables are kept in .env file, .env.example is an example env file to share for project start up 

[Object properties](https://www.php.net/manual/en/language.oop5.properties.php)

```bash
-> is object operator
```

Schemas live in the migrations

PhpStorm has [mySQL GUI](https://www.jetbrains.com/help/phpstorm/mysql.html#connect-to-mysql-database), so I connected it to mySQL instead of using myPhpAdmin

Commands:

- `php artisan migrate` → updates migrations and Laravel by default sets up some tables
- `php artisan make:migration create_blogs_table --create="blogs”` → creates a new table
- `php artisan make:migration add_is_admin_column_to_blogs_table --table="blogs”`  → creates a migration for a table blogs. With this we can add or remove columns
- `php artisan migrate:reset` rolls down all migration
- `php artisan migrate:refresh`  rolls down and up all migrations
- https://laravel.com/docs/8.x/migrations → for all other migration commands

## Raw SQL Queries

### Insert raw sql

`DB::insert('insert into blogs(title, content) values(?,?)', ['PHP with laravel', 'Laravel is the best thing that has happened to PHP']);`

DB is a facade class for the database

Although [DB will work without](https://laracasts.com/discuss/channels/laravel/should-i-import-facades-in-laravel-or-just-append-slash-before-class-to-call-it) `use` import due to aliases in config, you should still import with `use` for me it help to use the auto complete.

var_dump() is awesome for debugging

## Eloquent ORM

Very similar to Django orm

`php artisan make:model ModelName` create a model

`Blog::all();`

`Blog::find(2)->title;`

https://laravel.com/docs/8.x/queries

### Insert

```php
$blog = new Blog();

$blog->title = "New Eloquent Blog";$blog->content = "eloquent content";

$blog->save();
```

### Create

```php
Blog::create(['title' => 'the create method', 'content' => 'WOW']);

// BLOG MODEL
// add fields to fillable to use create method
class Blog extends Model
{
    use HasFactory;

    protected $fillable = [
        'title',
        'content'
    ];
}
```

### Update

`Blog::where('id', 2)->update(['title' => 'NEW UPDATED TITLE']);`

### Delete

`Blog::find(2)->delete();`

`Blog::destroy([3, 5]);`

Laravel has a migrations table that keeps track of migration order. Do `migrate:reset`  to reset that table before deleting migrations for whatever reason…

### Soft Deleting

https://laravel.com/docs/8.x/eloquent#soft-deleting

## Eloquent Relationships

Relations are defined with methods in models, but are used as properties.

- [One to One](https://laravel.com/docs/8.x/eloquent-relationships#one-to-one)
- [One to Many](https://laravel.com/docs/8.x/eloquent-relationships#one-to-many)
- [Many to Many](https://laravel.com/docs/8.x/eloquent-relationships#many-to-many)
    - Should be used with a pivot table (join table) that contains the foreign key of the two relating tables.
    - pivot table name convention is alphabetic order singular version of the two relating tables.
    - [belongsToMany](https://laravel.com/api/7.x/Illuminate/Database/Eloquent/Model.html#method_belongsToMany) api docs
        - ****`belongsToMany(string $related, string|null $table = null, string|null $foreignPivotKey = null, string|null $relatedPivotKey = null, string|null $parentKey = null, string|null $relatedKey = null, string|null $relation = null)`****
        - Pivot table can be given a custom name
- Pivot table, make sure to tell which columns to output for pivot tables
    - https://laravel.com/docs/10.x/eloquent-relationships#retrieving-intermediate-table-columns
- [Has Many Through](https://laravel.com/docs/8.x/eloquent-relationships#has-many-through)
    - creates a relationship through tables Country→User→Blog

## Polymorphic Relation

[Docs](https://laravel.com/docs/8.x/eloquent-relationships#polymorphic-relationships)

Table whos rows can belong to different models.

[Many to Many polymorphic relation](https://laravel.com/docs/10.x/eloquent-relationships#many-to-many-polymorphic-relations)

## Tinker

CLI for CRUD operations with the db. (I guess it is a REPL for eloquent)

## CRUD One To One

Where method can be used with camelCase column name to match specific column

```php
$address = Models\Address::whereName('alaska')->first();

$address = Models\Address::whereUserId(1)->first();
```

Can’t find the docs reference…

## Package and Validation

[LaravelCollective](https://laravelcollective.com/docs/6.x/html)/html is abandoned, but still works for the course.

```php
<h1>CREATE BLOG</h1>
{!! Form::open(['method'=>'POST', 'action'=>'\App\Http\Controllers\PostsController@store']) !!}

<div class="form-group">
    {!! Form::label('title', 'Title:') !!}
    {!! Form::text('title', null, ['class'=>'form-control']) !!}
</div>
<input type="submit" name="submit">
{{csrf_field()}}

{!! Form::close() !!}
```

Validate in the controller

```php
$this->validate($request, [
            'title' => 'required',
            'content' => 'required',
        ]);
```

Form more complex validation or to abstract validation, create a [“form request”](https://laravel.com/docs/8.x/validation#creating-form-requests) with **`php artisan make:request StorePostRequest`**

## Database - some more model manipulation

### Datetime with Carbon or PHP

```php
$date = new DateTime('+1 week');
echo $date->format('m-d-Y');
echo "<br>";
echo Carbon::now()->addDays(10)->diffForHumans();
```

### Accessors

```php
// name of column and attribute.
public function getNameAttribute($value)
{
    return ucfirst($value);
}
```

### Mutator

```php
public function setNameAttribute($value)
{
    $this->attributes['name'] = strtoupper($value);
}
```

## Middleware

[docs](https://laravel.com/docs/8.x/middleware)

Request data flows through middlewares before reaching the route

create middleware `php artisan make:middleware <name>`

make:auth no longer works for Laravel ^7 use `composer require laravel/ui` instead

## Data Seeding

automates db data population

`php artisan make:seeder NameExample` → creates seeder class

`php artosan db:seed` → seeds

In factory class define the default state:

```php
<?php

namespace Database\Factories;

use Illuminate\Database\Eloquent\Factories\Factory;

class PostFactory extends Factory
{
    /**
     * Define the model's default state.
     *
     * @return array
     */
    public function definition()
    {
        return [
            //
            'title'=>$this->faker->name(),
            'body'=>$this->faker->text(20),
        ];
    }
}
```

## Laravel 7 App from start to finish notes

Laravel will find inject the class object automatically

I think this is called [binding](https://laravel.com/docs/8.x/container#binding-basics)

```php
Route::get('/post/{post}', [App\Http\Controllers\PostController::class, 'show'])->name('post');
```

CSRF token is set in meta

```php
<!-- CSRF Token -->
<meta name="csrf-token" content="{{ csrf_token() }}">
```

Simple validation in controller

```php
public function store()
    {
        $inputs = request()->validate([
            'title' => 'required | min:8 | max:255',
            'post_image' => 'file',
            'body' => 'required',
        ]);

        if (request('post_image')) {
            $inputs['post_image'] = request('post_image')->store('images');
        }

        auth()->user()->posts()->create($inputs);

        return back();
    }
```

Use session to flash status messages for the user

```php
// in any controller functions
session()->flash('post-created-message', 'Post was created');
```

Policies

[docs](https://laravel.com/docs/8.x/authorization#writing-policies)

`php artisan make:policy PostPolicy --model=Post` → to create post policy

in policy class, control how policy works

```php
public function update(User $user, Post $post)
{
    //
    return $user->id === $post->user_id;
}
```

in controller authorize and pass model

`$this->authorize('update', $post);`

Policies can be used in controllers, routes, and in views.

Blade Directives can go into the class str

`class="form-control @error('name') is-invalid @enderror"`

PHP short ternary chaining

Chaining of short-ternaries (?:), however, is stable and behaves reasonably. It will evaluate to the first argument that evaluates to a non-falsy value. Note that undefined values will still raise a warning.

Example #5 Short-ternary chaining 

```php
<?php
echo 0 ?: 1 ?: 2 ?: 3, PHP_EOL; //1
echo 0 ?: 0 ?: 2 ?: 3, PHP_EOL; //2
echo 0 ?: 0 ?: 0 ?: 3, PHP_EOL; //3
?>
```

When splitting routes into separate files, register them in [RouteServiceProvider’s routes](https://laravel.com/api/8.x/Illuminate/Foundation/Support/Providers/RouteServiceProvider.html#method_routes) 

```php
public function boot()
    {
        $this->configureRateLimiting();

        $this->routes(function () {
            Route::prefix('api')
                ->middleware('api')
                ->namespace($this->namespace)
                ->group(base_path('routes/api/api.php'));

            Route::middleware('web')
                ->namespace($this->namespace)
                ->group(base_path('routes/web/web.php'));

            Route::prefix('admin')
                ->middleware('web')
                ->namespace($this->namespace)
                ->group(base_path('routes/web/posts.php'));

            Route::prefix('admin')
                ->middleware('web')
                ->namespace($this->namespace)
                ->group(base_path('routes/web/users.php'));
        });
    }
```

Check if model was changed or not with isDirty or isClean.

```php
if($role->isDirty('name')) {
    session()->flash('role-updated', 'Role updated: '.$role->name);
    $role->save();
} else {
    session()->flash('role-updated', 'Nothing has been updated');
}
```