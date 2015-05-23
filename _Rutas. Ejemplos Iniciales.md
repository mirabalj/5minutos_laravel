// Testing custom url thanks
// With a closure function
Route::get( 'thanks' , function ()
{
	return 'Thanks for buying Code Bright!';
} );

// Testing more complex custom url
Route::get( 'my/page' , function ()
{
	return 'This is my page!';
} );

// Testing with another form of use a closure function
$page2_function = function ()
{
	return 'This my page2!';
};
Route::get( 'my/page2' , $page2_function );

// Testing Route Parameters
Route::get( '/books' , function ()
{
	return 'Book index';
} );
// /books/crime
Route::get( '/books/{genre}' , function ( $genre )
{
	return "Books in the {$genre} category";
} );

// Testing use of regular expressions on routing
// /articles/5
Route::get( '/articles/{category}' , function ( $category )
{
	return "Articles in the category number {$category}";
} )->where( 'category' , '[0-9]+' );

// Testing use of regular expressions on routing
// /articles/love
// /articles (Optional parameter)
Route::get( '/articles/{category?}' , function ( $category = null )
{
	if ( null=== $category )
	{
		return 'All books';
	}
	else
	{
		return "Books in the {$category} category";
	}
} )->where( 'category' , '[a-z]+' );


// Testing a simple view
Route::get( 'simple' , function ()
{
	return View::make( 'simple' );
} );

// Testing a simple view with data
// /simple/gray
Route::get( '/simple/{squirrel}' , function ( $squirrel )
{
	$data['squirrel'] = $squirrel;

	return View::make( 'squirrel' , $data );

} );

// Testing simple redirect
// /redirect
Route::get( 'redirect' , function ()
{
	return Redirect::to( 'simple' );
} );

// Testing auth redirect;
// /private
Route::get( 'private' , function ()
{
	if ( Auth::guest() )
	{
		return Redirect::to( 'login' );
	}
} );

// Testing custom responses
// /response/[code]
Route::get( '/response/{code}' , function ( $code )
{
	return Response::make( 'Custom response code ' . $code , $code );
} );

// Testing custom responses with headers
// /hresponse/[code]
Route::get( '/hresponse/{code}' , function ( $code )
{
	$response = Response::make( 'Custom response code ' . $code , $code );
	$response->headers->set( 'custom key' , 'custom value' );

	return $response;
} );


// Testing response with a json file
// /json
Route::get( '/json' , function ()
{
	return Response::json( array( 'one' , 'two' , 'three' ) );
} );

// Testing response for download a file
// /file
// TODO: How to download a file? How to get file server path?
Route::get( '/file' , function ()
{
	return Response::download( 'index.php' );
} );

	// Testing my custom controller method
Route::get( 'custom' , 'WelcomeController@custom' );


// Testing my custom controller
// with my custom view (myview)
Route::get('controller','MyCustomController@index');

	// Custom about template
Route::get('about','MyCustomController@about');
//Route::get('contact','MyCustomController@contact');
Route::get('contact/{who?}','MyCustomController@contact');


// Calling to index method of WelcomeController Controller
Route::get( '/' , 'WelcomeController@index' );

Route::get( 'home' , 'HomeController@index' );

Route::controllers( [
						'auth'     => 'Auth\AuthController' ,
						'password' => 'Auth\PasswordController' ,
					] );


------
```
File: D:\Proyectos\Repositorios_Git\Laravel\Pruebas\laravel_code-bright\app\Http\Controllers\MyCustomController.php

<?php namespace App\Http\Controllers;

// Custom controller for testing

use App\Http\Requests;
use App\Http\Controllers\Controller;

use Illuminate\Http\Request;

class MyCustomController extends Controller {

	/**
	 * Create a new controller instance.
	 *
	 * @return void
	 */
	public function __construct()
	{
		$this->middleware('guest');	// Check if we are authenticated
	}

	/**
	 * Show the application welcome screen to the user.
	 *
	 * @return Response
	 */
	public function index()
	{
		return view('custom.myview');
	}

		// Passing variables to views
	public function about()
	{
		$name='<span style="color:red">Juanan Tubío</span>';
		$uno='cats';
		$dos='dogs';
			// Nice sample about using compact php function
		return view('custom.about',compact('uno','dos'))->with('name',$name);
	}

		// Custom view with conditionals
	public function contact($who=null)
	{
		if($who===null) $who="all";
		$people=array();
		if('juan'==$who) $people=array('María','Pedro','Luis');
		return view('custom.contact',compact('who','people'));
	}
}
```
------
```
<?php namespace App\Http\Controllers;

class HomeController extends Controller {

	/*
	|--------------------------------------------------------------------------
	| Home Controller
	|--------------------------------------------------------------------------
	|
	| This controller renders your application's "dashboard" for users that
	| are authenticated. Of course, you are free to change or remove the
	| controller as you wish. It is just here to get your app started!
	|
	*/

	/**
	 * Create a new controller instance.
	 *
	 * @return void
	 */
	public function __construct()
	{
		$this->middleware('auth');
	}

	/**
	 * Show the application dashboard to the user.
	 *
	 * @return Response
	 */
	public function index()
	{
		return view('home');
	}

}
```
------
```
<?php namespace App\Http\Controllers;

class WelcomeController extends Controller {

	/*
	|--------------------------------------------------------------------------
	| Welcome Controller
	|--------------------------------------------------------------------------
	|
	| This controller renders the "marketing page" for the application and
	| is configured to only allow guests. Like most of the other sample
	| controllers, you are free to modify or remove it as you desire.
	|
	*/

	/**
	 * Create a new controller instance.
	 *
	 * @return void
	 */
	public function __construct()
	{
		$this->middleware('guest');
	}

	/**
	 * Show the application welcome screen to the user.
	 *
	 * @return Response
	 */
	public function index()
	{
		return view('welcome');
	}

	/**
	 * Show the application welcome screen to the user.
	 *
	 * @return Response
	 */
	public function custom()
	{
		return view('custom.myview');	// Custom view, use '.' instead '/'
	}
}
```
------
```
Filename: D:\Proyectos\Repositorios_Git\Laravel\Pruebas\laravel_code-bright\resources\views\squirrel.php

<?php
/**
 * Created by PhpStorm.
 * User: jatubio
 * Date: 27/02/2015
 * Time: 20:46
 *
 * Testing a simple view with data
 *
 */
?>

<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Squirrels</title>
</head>
<body>
<p>I wish I were a <?php echo $squirrel; ?> squirrel!</p>
</body>
</html>
------
```
D:\Proyectos\Repositorios_Git\Laravel\Pruebas\laravel_code-bright\resources\views\simple.php

<?php
/**
 * Created by PhpStorm.
 * User: jatubio
 * Date: 27/02/2015
 * Time: 20:46
 *
 * Testing a simple view
 *
 */
?>
<!doctype html>
<html lang="en">
   <head>
<meta charset="UTF-8">
     <title>Views!</title>
 </head>
 <body>
 <p>Oh yeah! VIEWS!</p>
 </body>
 </html>
```
------
```
