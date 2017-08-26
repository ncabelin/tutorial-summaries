# PHP
this is the
[Object Oriented PHP](#object-oriented-php)

## Evaluate *in* an expression using double quotes
```
<?php echo "This is an $expression"; ?>
```

# ARRAYS

### Create arrays
```
<?php $array = array(); ?>
<?php $array = []; ?>
```

### Print arrays
```
print_r($array_name);
```

### Add items to arrays
```
$my_array = array();
$my_array[] = 'one more item';
$my_array[] = 'second item';
```

### Remove last item on array
```
$lastValue = array_pop($array);
```

### Remove from index, item on array
```
unset($array[1]);
```

### Create an associative array
```
$my_associative_array = array(
    name => 'Michael',
    phone => 12345
  );
```

### Create a multidimensional array
```
$my_multidimensional_array = array(
  names => array(
      'Bubba',
      'Gump',
      'Trump'
  ),
  toys => array(
      'train',
      'car',
      'jack-hammer'
  )
);
```

### Count an array
```
echo count($array_to_count);
```

### Combine elements in an array
```
$trains = array(
    'Percy',
    'Thomas',
    'Topham Hat'
  )
echo implode(' ', #trains)
// 'Percy Thomas Topham Hat'
```

### Randomize an array
* `shuffle($array_name)`

### Sort an array with
* `asort($array_name)`

### Sort an array by key value
* `ksort($array_name)`
### array_key_exists()
```
array_key_exists('key', $array);
```
### in_array()
```
// empty = not found, if found = index number
echo in_array('the_key', $array);
```

# CONDITIONALS

### Elseif instead of else if like javaScript (no spaces)

# LOOPS

### Iterate through an associative array
```
foreach($integer as $integer) {

}
```

### Iterate and use key and value in an associative array
```
foreach($integer as $key => $value) {
  echo "Use $key and $value now";
}
```

# FORM DATA
* Super Global Variables
```
if($_SERVER['REQUEST_METHOD'] === 'POST') {
  echo $_POST['date'];
  echo $_POST['email'];
}
```

### Includes
* Add include to compartmentalize
```
include('../app/views/header.php');
```

* NOTE : If no other code, no need to type `?>`

### Using require
Instead of include we can use require :
```
require __DIR__ . '/../app/src/app.php';
```

## VALIDATION
1. $variable exists
```
if (!empty($date)) {

}
```
2. remove whitespace
```
$date = trim($_POST['date']);
```
3. sanitize output
```
echo '<p>' . htmlspecialchars($description) . '</p>';
```
4. validate email
5. validate date

## VALIDATION OF date
1. Use strtotime()
```
if ($time = strtotime($date)) {
  // do something with date
}
```
2. Validate date
```
echo date('m/d/Y', $timestamp);
```
3. Sanitize and Validate email
$email = "john.doe@example.com";
```
// Remove all illegal characters from email
$email = filter_var($email, FILTER_SANITIZE_EMAIL);

// Validate e-mail
if (!filter_var($email, FILTER_VALIDATE_EMAIL) === false) {
    echo("$email is a valid email address");
} else {
    echo("$email is not a valid email address");
}
```

# Using Composer
Install it

#### Object Oriented PHP:
* creating your own class
```
class Car {
  var $wheels = 4;
  function car_details() {
    return "This car has " . $this->wheels . " wheels.";
  }
}

bmw = new Car();
echo $bmw->wheels; // outputs 4
echo $bmw->car_details(); // outputs 'This car has 4 wheels'
```

* public, private, protected
```
public $wheels = 4;
private $seats = 2;
protected $engine = 1;
```

* static modifier `::`
```
class Car {
  static function wheels() {
    return 4;
  }
}

echo Car::wheels(); // outputs 4;
```

* use constructor to pass values to class
```
class Car {
  public $name;
  function __construct($car_name) {
    $this->name = $car_name;
  }

  public function get_name() {
    return $this->name;
  }
}
```

* constants
```
const CONSTANT = 1;
```

# Using Composer

Use a library installed by Composer
```
// require the autoloader to load all packages
require __DIR__ . '/../../vendor/autoload.php';

// load the php file that will use the class
require __DIR__ . '/validation.php'

// use the Validator class
use Respect\Validation\Validator;
$v = new Validator;

```
Use `->` aka 'object operator' to chain methods, `::` to use methods in a class like so
```
// to use object class
Validator::notEmpty()->validate($date_string)
```
```
use Respect\Validation\Validator;

function validate_date($date_string) {
  $date_validator = Validator::date('d-m-Y')->notEmpty();

  if ($date_validator->validate($date_string)) {
    $date_time = strtotime($date_string);
    return date('F jS Y', $date_time);
  }
}
```

### Try Catch for Validation
```
try {
  $date_validator->assert($date_string);
}

catch () {

}
```
