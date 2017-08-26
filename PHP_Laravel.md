# Laravel (PHP framework)
### HOW TO INSTALL

FOLDER STRUCTURE
```
/root
|_____ /app
|        |_____
|_____ /config
|_____ /database
|_____ /public
|_____ /resources
|_____ /routes
|_____ /storage
|_____ /tests
|_____ /views
         |___ /markets
                 |__ show.blade.php
```

* `collection` is used to create a collection from an array
```
$c->first();
$c->last();
// return every data with that key
$c->fetch('name');

// find by id
Market::find(3);
```

#### CRUD
* CREATE
```
$data = ['name' => 'Winter Garden Market',
  'city' => 'Winter Garden'
]
Market::create($data);
```
* READ
```
// single item
$m = Market::find(3);

// all items,
$markets = Market::all();

// custom constraint
$markets = Market::where('city', 'Orlando')->get();

// multiple constraints
$markets = Market::where('city', 'Orlando')
    ->orderBy('name', 'desc')
    ->take(5)
    ->get();
```
* UPDATE
```
// single item update
$m = Market::find(3);
$m->name = 'Winter Garden Update';
$m->save();

// multiple items Update
$m = Market::find(3);
$data = [
  'name' => 'Winter Garden Update',
  'website' => 'wgcoop.com'
];
$m->fill($data);
```
* DELETE
```
// single record deletion
$m = Market::find(3);
$m->delete();

// single record destroy
Market::destroy(3);

// multiple record destroy
Market::destroy([3, 4, 5]);
```

#### Models
* using Eloquent, put in folder `app/Market.php`
```
use Illuminate\Database\Eloquent\Model;

Class Market extends Model
{
  // scope + method name e.g. scopeOrlando
  public function scopeOrlando()
  {
    return $query->where('city', 'Orlando')
                ->orderBy('name', 'desc')
                ->get();
  }
}

// to use this method
$orlandoMarkets = Market::orlando()->get();
```

#### VIEWS
* using Blade
```
// use these inside your blade file
// ---------------------------------
@php
  // behaves as <?php ?>
@endphp

// {{ }} echoes any code within
{{ $market->name }}
{{ $market->city }}

// loop construct
@foreach ($markets as $market)
  <a href="{{ $market->website }}">
    <h2>{{ $market->name }}</h2>
  </a>
@endforeach
```

* DRY with layouts, create a different folder called
/layouts e.g. `resources/views/layouts/app.blade.php`
```
<!DOCTYPE html>
<html lang="en">
<head><title>This is my Webpage</title></head>
<body>

  @yield('main')

</body>

// how to use this, put it in the blade file
// 'resources/views/markets/index.blade.php'
@extends('layouts.app')
@section('main')
  // content here
@endsection
```

#### CONTROLLERS
* put it in e.g. `/app/Http/Controllers/MarketController.php` folder
```
namespace App\Http\Controllers;

use App\Market;
use Illuminate\Http\Request;

Class MarketController extends Controller
{
  // Show All markets
  public function index()
  {
    $market = Market::all();
    // first argument is our view template location
    // second argument passes data to view
    return view('markets.index', ['markets' => $markets]);
  }

  // Show Single market
  public function show(Market $market)
  {
    return view('markets.show', ['market' => $market]);
  }
}
```

#### ROUTES
* put it in e.g. `/app/routes/web.php`
```
Route::get('/', function() {

});

Route::get('markets/{id}', function($id) {
  return 'Id is ' . $id;
});
```
* Using the controllers made earlier
```
Route::get('markets/create', 'MarketController@create');
Route::post('markets', 'MarketController@store');
Route::get('markets/{market}', 'MarketController@show');

Route::get('markets/{market}/edit', 'MarketController@edit');
Route::get('markets/{market}', 'MarketController@update');
Route::delete('markets/{market}', 'MarketController@destroy');

// with single line of code we can have many different routes
Route::get('/', 'MarketController@index');
Route::resource('markets', 'MarketController');
```
