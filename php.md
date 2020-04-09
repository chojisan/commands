## Function for stripping out malicious bits

```
<?php
function cleanInput($input) {
 
  $search = array(
    '@<script[^>]*?>.*?</script>@si',   // Strip out javascript
    '@<[\/\!]*?[^<>]*?>@si',            // Strip out HTML tags
    '@<style[^>]*?>.*?</style>@siU',    // Strip style tags properly
    '@<![\s\S]*?--[ \t\n\r]*>@'         // Strip multi-line comments
  );
 
    $output = preg_replace($search, '', $input);
    return $output;
  }
?>
```

## Sanitization function
```
<?php
function sanitize($input) {
    if (is_array($input)) {
        foreach($input as $var=>$val) {
            $output[$var] = sanitize($val);
        }
    }
    else {
        if (get_magic_quotes_gpc()) {
            $input = stripslashes($input);
        }
        $input  = cleanInput($input);
        $output = mysql_real_escape_string($input);
    }
    return $output;
}
?>
```


# PHP
PHP is a little more disorganized than how Perl handles parameters. The standard MySQL extension doesn't support parameterization, but that extension has been out of date for more than five years and you should definitely use one of the alternatives instead. The PostgreSQL extension does:
```
$result = pg_query_params( $dbh, 'SELECT * FROM users WHERE email = $1', array($email) );
```
Note that the query must be in single quotes or have the $ escaped to avoid PHP trying to parse it as a variable. (Actually, in this case PHP will not see $1 as a variable and will not interpolate it, but for the sake of good practice, single-quote any strings with dollar signs that you want to keep as dollar signs.

However, you should probably be using an abstraction layer. The ODBC and PDO extensions both support parameterization and multiple databases:

## Using mysqli
The MySQL Improved extension handles bound parameters.
```
$stmt = $db->prepare('update people set name = ? where id = ?');
$stmt->bind_param('si',$name,$id);
$stmt->execute();
```

## Using ADODB
ADODB provides a way to prepare, bind and execute all in the same method call.
```
$dbConnection = NewADOConnection($connectionString);
$sqlResult = $dbConnection->Execute(
    'SELECT user_id,first_name,last_name FROM users WHERE username=? AND password=?',
    array($_REQUEST['username'], sha1($_REQUEST['password'])
);
```

## Using the ODBC layer
```
$stmt = odbc_prepare( $conn, 'SELECT * FROM users WHERE email = ?' );
$success = odbc_execute( $stmt, array($email) );
```
Or:
```
$dbh = odbc_exec($conn, 'SELECT * FROM users WHERE email = ?', array($email));
$sth = $dbh->prepare('SELECT * FROM users WHERE email = :email');
$sth->execute(array(':email' => $email));
```

Using the PDO layer
Here's the long way to do bind parameters.
```
$dbh = new PDO('mysql:dbname=testdb;host=127.0.0.1', $user, $password);
$stmt = $dbh->prepare('INSERT INTO REGISTRY (name, value) VALUES (:name, :value)');
$stmt->bindParam(':name', $name);
$stmt->bindParam(':value', $value);

// insert one row
$name = 'one';
$value = 1;
$stmt->execute();
```
And a shorter way to pass things in.
```
$dbh = new PDO('mysql:dbname=testdb;host=127.0.0.1', $user, $password);
$stmt = $dbh->prepare('UPDATE people SET name = :new_name WHERE id = :id');
$stmt->execute( array('new_name' => $name, 'id' => $id) );
Here's a great tutorial on migrating to PDO for MySQL developers.
```

# Applications & Frameworks

## CakePHP
When using the MVC framework CakePHP, most of your database communication will be abstracted away by the Model API. Still, it is sometimes necessary to perform manual queries, which can be done with Model::query. In order to use prepared statements with that method, you just need to pass an additional array parameter after the SQL query string. There are two variants:
```
// Unnamed placeholders: Pass an array containing one element for each ?
$this->MyModel->query(
    'SELECT name FROM users WHERE id = ? AND status = ?',
    array($id, $status)
);

// Named placeholders: Pass an associative array
$this->MyModel->query(
    'SELECT name FROM users WHERE id = :id AND status = :status',
    array('id' => $id, 'status' => $status)
);
```
This behavior is documented in the CakePHP Cookbook. (It is described for the fetchAll()-method, but query() uses fetchAll() internally).

## WordPress
If your site/blog/application is running on WordPress, you can use the prepare method of the $wpdb class, which supports both a sprintf()-like and vsprintf()-like syntax.
```
global $wpdb;
$wpdb->query(
    $wpdb->prepare( 'SELECT name FROM people WHERE id = %d OR email = %s',
        $person_id, $person_email
    )
);
```

For INSERTs, UPDATEs, and DELETEs, you can use the handy helper methods in the class, which allow you to specify the format of the submitted values.
```
global $wpdb;
$wpdb->insert( 'people',
        array(
            'person_id' => '123',
            'person_email' => 'bobby@tables.com'
        ),
    array( '%d', '%s' )
);
```

## Usage
```
<?php
  $bad_string = "Hi! <script src='http://www.evilsite.com/bad_script.js'></script> It's a good day!";
  $good_string = sanitize($bad_string);
  // $good_string returns "Hi! It\'s a good day!"

  // Also use for getting POST/GET variables
  $_POST = sanitize($_POST);
  $_GET  = sanitize($_GET);
?>
```
https://bobby-tables.com/php.html
