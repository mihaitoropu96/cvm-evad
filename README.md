# cvm-evad

/
  -App (application code)
    -Controllers
    -Models
    -Views
  -Core
      -Router.php(request url handler)
  -logs
  -public(only folder accessible to the web, the root, here we have static files(CSS, images, js)
      -.htaccess
      -index.php(entry point)
  -vendor (third party libraries)
  
  regex matching:
    phpliveregex.com
  
  require OR include?
    - If the file is not found, "require" will stop the script and produce an error. "include" will just carry on.
  
  Advantages:
  - most of the code and database passwords are not in a web accessible folder and therefore the app is more secure
  
  
  Steps:
  1. make public directory the root of the project
  2. url will go through index.php (front controller - central entry point)
  3. request is in the query string after first "?", here we can see to which controller and which method the request will be sent to.
      $_SERVER['QUERY_STRING'] can be used to get the query string
  4. add directives for changing apache configuration in .htaccess file
  5. create router class for managing url requests 
    - router decides which controller and action to run based on the route-url
    you can add an associative array where to save routes
    
    Example:
    $router->add('', ['controller' => 'Home', 'action' => 'index']);
    $router->add('posts', ['controller' => 'Posts', 'action' => 'index']);
    $router->add('posts/new', ['controller' => 'Posts', 'action' => 'new']);
    
    create router using regex then search for it in the table(S).
  6.call_user_func_array([$post, "save"],[123, "abc"]);
    $post -> object
    save -> name of the method
    [123, "abc"] -> array with parameters
  7. Error handling to check if the class exists before creating an object:
      $class_name = "Post";
      
      if (class_exists($class_name)) {
          $post = new $class_name();
      }
      => check if a method exists:
      $post = new Post();
      $method = "save";
      
      if (is_callable([$post, $method])) {
          $post->$method();
      }
    
      => check if method is callable 
      if (is_callable($controller_object, $action)) {
          $controller_object->$action();
      }
   8. Use namespaces for classes with same names
   9. Use autoloading 
   10. You can remove query strings "?" and add pairs of key => values in array with removeQueryStringVariables from Router class
   11. Set the params array in BaseController
   12. __call magic method + call_user_func_array allows to call private methods with an object.
       
       Action filters
   13. How can you check user permissions: using __call magic method
        - add a suffix at your action name ex: post => postAction;
        - make the action private;
        - call the action with __call magic method
          - at the begging of the __call magic method put your before code to be executed, before you call the action with "call_user_func_array([$this, "nameAction"], $args)"; and after action put your after code;
   14. You can organise controllers in subdirectories using regex;
   15. For cross scripting atacks use htmlspecialchars($_POST['name']);
   16. Pass the data from the controller to the view using extract() method
   
   
   17. Template Engine (twig template engine)
        - Simpler, easier syntax
        - autoescaping of variables
        - templates inheritance
        - no php in the templates
   18.Install third party libraries(ex: twig) using Composer (check using composer about)
   19. Include all package classes automatically using the Composer autoloader
          require 'vendor/autoload.php';
   20. Add your own classes with Composer autoloader:
        in composer.json file add:
        {
          "autoload": {
            "classmap": [
                "path/to/Class1.php",
                "path/to/Class2.php",
                "path/to/folder"
            ]
          }
        }
        
        or if using namespaces you can use PSR4 autoloading
        namespaces : paths
        {
          "autoload": {
            "psr-4": {
                "Core\\": "classes/",
                "App\\": "src/",
                "Vendor\\Namespace\\": "src/vendor/"
            }
          }
        }
        
        after adding classes and paths to compose.json you need to run: composer dump-autoload
   21.PDO - code library for accessing databases
      Connecting to database using PDO:
      try {
        $dbh = new PDO('mysql:host=local;dbname=mydb', 'user', 'password');
      } catch(PDOException $e) {
        echo $e->getMessage();
      }
      
  22. Database Model base controller abstract:
    protected static function getDB()
{   
    static $db = null;
    if ($db === null) {
        $host = 'localhost';
        $dbname = 'dbname';
        $username = 'username';
        $password = 'password';
    
        try {
            $db = new PDO("mysql:host=$host;dbname=$dbname;charset=utf8", 
                          $username, $password);
        } catch (PDOException $e) {
            echo $e->getMessage();
        }
    }
    return $db;
}
  23. trigger_error() for triggering errors 
  24. Exceptions are the errors you get when dealing with classes and objects.
      To handle both: errors and exceptions we need to convert errors to exceptions
      Errors and exceptions handling with db.
  25. To see php configuration use <?php phpinfo(); ?>
      How to display error messages in php:
      put in index.php error_reporting(E_ALL);
      - set the location of the log file:
        init.set('error_log', 'log.txt');
      -write a message to the log file:
        error_log('An error occurred');
  26. Add status errors on HTTP request
      
   
      
