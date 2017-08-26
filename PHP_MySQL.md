# PHP MySQL
## Procedural
### Sample User Table
```
id
  - INT
  - 11
  - Attributes(Unsigned(positive numbers only))
  - Index(PRIMARY)
username
  - VARCHAR
  - 20
password
  - VARCHAR
  - 255
email
signup_date
  - CURRENT_TIMESTAMP

  - if (null) checked it means optional
```

### How to Connect to Database and do queries (PROCEDURAL)
```
// create connection.php
$server = 'localhost';
$username = 'root';
$password = 'root';
$db = 'markdown';

// create connection
$conn = mysqli_connect($server, $username, $password, $db);

// check connection
if (!$conn) {
  die('Connection failed: ' . mysqli_connect_error());
}
echo 'Connected succesfully';
```
* how to do query normally
```
include('connection.php');
$query = 'SELECT * FROM users';
$result = mysqli_query( $conn, $query );

if (mysqli_num_rows($result) > 0) {
  while($row = mysqli_fetch_assoc($result)) {
    echo $row['id'] . ' ' . $row['username'];
  }
} else {
  echo 'No results.';
}
```

### How to Connect to Database and use queries (OBJECT ORIENTED)
```
$conn = new mysqli($dbServer, $dbUserName, $dbPassword, $dbName);

if ($conn->connect_errno) {
  // database failed
  exit($conn->connect_error);
}

// closing
$conn-close();
```
* query
```
$conn->query($query);
```
* get query id that was auto-incremented
```
$conn->query($query);
echo 'this is the id :' . $conn->insert_id;
```
* get results
```
$result = $conn->query($query);
if ($result->num_rows > 0) {
  while($row = $result->fetch_assoc()) {
    // do row stuff
  }
}
// always close
$result->close();
$conn->close();
```


### How to hash password
```
$hashed_password = password_hash( 'my_password', PASSWORD_DEFAULT );

if ( password_verify( 'my_password', $hashed_password ) ) {
  // password is correct redirect
  header("Location: whereever_togo.php");
}
```

### Sessions
```
session_start();
$_SESSION['username'] = 'whatever';

// to end sessions
session_unset();
session_destroy();
```

### Setting Cookie
```
if (isset($_COOKIE[session_name()])) {
  setcookie( session_name(), '', time()-86400, '/');
}
```

### validate Form Data
```
function validateFormData( $formData ) {
  $formData = trim(stripslashes(htmlspecialchars($formData)));
  return $formData;
}
```

### how to prevent sql injections in mysqli using PREPARED statements
```
// 1. declare variable
$first_name = 'Lucy';
$query = "SELECT * FROM Authors WHERE first_name = ?";
// 2. prepare first
$statement = $conn->prepare($query);
// 3. bind (s = string, i = integer, d = decimal)
$statement->bind_param('s', $first_name);
// 4. execute
$statement->execute();
// 5. bind variables to results (to be used later on)
$statement->bind_result($firstName, $lastName);
$statement->store_result();
// 6. fetch results
if ($statement->num_rows > 0) {
  while ($statement->fetch()) {
    // use $firstName, $lastName variables here
  }
}
```

```
$stmt = $dbConnection->prepare('SELECT * FROM employees WHERE name = ?');
$stmt->bind_param('s', $name);

$stmt->execute();

$result = $stmt->get_result();
while ($row = $result->fetch_assoc()) {
    // do something with $row
}
```
