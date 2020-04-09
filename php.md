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

# A guide to preventing SQL injection
## PHP
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

## Using the PDO layer
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


## Unlimited menu items
```
<?php
 
/**
 * Generate HTML for multi-dimensional menu from MySQL database
 * with ONE QUERY and WITHOUT RECURSION
 * @author J. Bruni
 */
class MenuBuilder
{
        /**
         * MySQL connection
         */
        var $conn;
       
        /**
         * Menu items
         */
        var $items = array();
       
        /**
         * HTML contents
         */
        var $html  = array();
       
        /**
         * Create MySQL connection
         */
        function MenuBuilder()
        {
                $this->conn = mysql_connect( 'localhost', 'user', 'pass' );
                mysql_select_db( 'example', $this->conn );
        }
       
        /**
         * Perform MySQL query and return all results
         */
        function fetch_assoc_all( $sql )
        {
                $result = mysql_query( $sql, $this->conn );
               
                if ( !$result )
                        return false;
               
                $assoc_all = array();
               
                while( $fetch = mysql_fetch_assoc( $result ) )
                        $assoc_all[] = $fetch;
               
                mysql_free_result( $result );
               
                return $assoc_all;
        }
       
        /**
         * Get all menu items from database
         */
        function get_menu_items()
        {
                // Change the field names and the table name in the query below to match tour needs
                $sql = 'SELECT id, parent_id, title, link, position FROM menu_item ORDER BY parent_id, position;';
                return $this->fetch_assoc_all( $sql );
        }
       
        /**
         * Build the HTML for the menu
         */
        function get_menu_html( $root_id = 0 )
        {
                $this->html  = array();
                $this->items = $this->get_menu_items();
               
                foreach ( $this->items as $item )
                        $children[$item['parent_id']][] = $item;
               
                // loop will be false if the root has no children (i.e., an empty menu!)
                $loop = !empty( $children[$root_id] );
               
                // initializing $parent as the root
                $parent = $root_id;
                $parent_stack = array();
               
                // HTML wrapper for the menu (open)
                $this->html[] = '<ul>';
               
                while ( $loop && ( ( $option = each( $children[$parent] ) ) || ( $parent > $root_id ) ) )
                {
                        if ( $option === false )
                        {
                                $parent = array_pop( $parent_stack );
                               
                                // HTML for menu item containing childrens (close)
                                $this->html[] = str_repeat( "\t", ( count( $parent_stack ) + 1 ) * 2 ) . '</ul>';
                                $this->html[] = str_repeat( "\t", ( count( $parent_stack ) + 1 ) * 2 - 1 ) . '</li>';
                        }
                        elseif ( !empty( $children[$option['value']['id']] ) )
                        {
                                $tab = str_repeat( "\t", ( count( $parent_stack ) + 1 ) * 2 - 1 );
                               
                                // HTML for menu item containing childrens (open)
                                $this->html[] = sprintf(
                                        '%1$s<li><a href="%2$s">%3$s</a>',
                                        $tab,   // %1$s = tabulation
                                        $option['value']['link'],   // %2$s = link (URL)
                                        $option['value']['title']   // %3$s = title
                                );
                                $this->html[] = $tab . "\t" . '<ul class="submenu">';
                               
                                array_push( $parent_stack, $option['value']['parent_id'] );
                                $parent = $option['value']['id'];
                        }
                        else
                                // HTML for menu item with no children (aka "leaf")
                                $this->html[] = sprintf(
                                        '%1$s<li><a href="%2$s">%3$s</a></li>',
                                        str_repeat( "\t", ( count( $parent_stack ) + 1 ) * 2 - 1 ),   // %1$s = tabulation
                                        $option['value']['link'],   // %2$s = link (URL)
                                        $option['value']['title']   // %3$s = title
                                );
                }
               
                // HTML wrapper for the menu (close)
                $this->html[] = '</ul>';
               
                return implode( "\r\n", $this->html );
        }
}
 
$menu = new MenuBuilder();
echo '<pre>' . htmlentities( $menu->get_menu_html() ) . '</pŕe>';
 
/*** SAMPLE DATABASE: ***
 
CREATE TABLE `menu_item` (
  `id` int(11) NOT NULL,
  `title` varchar(75) DEFAULT NULL,
  `link` varchar(100) DEFAULT NULL,
  `parent_id` int(11) DEFAULT NULL,
  `position` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
);
 
INSERT INTO `menu_item` (`id`, `title`, `link`, `parent_id`, `position`) VALUES (1,'1','1.html',0,1);
INSERT INTO `menu_item` (`id`, `title`, `link`, `parent_id`, `position`) VALUES (2,'2','2.html',0,2);
INSERT INTO `menu_item` (`id`, `title`, `link`, `parent_id`, `position`) VALUES (3,'11','11.html',1,1);
INSERT INTO `menu_item` (`id`, `title`, `link`, `parent_id`, `position`) VALUES (4,'12','12.html',1,2);
INSERT INTO `menu_item` (`id`, `title`, `link`, `parent_id`, `position`) VALUES (5,'21','21.html',2,1);
INSERT INTO `menu_item` (`id`, `title`, `link`, `parent_id`, `position`) VALUES (6,'22','22.html',2,2);
INSERT INTO `menu_item` (`id`, `title`, `link`, `parent_id`, `position`) VALUES (7,'3','3.html',0,3);
 
*/
 
?>
```
