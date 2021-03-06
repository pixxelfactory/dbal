# Pixxel Dbal

This is a simple wrapper for PDO, since we hate its syntax and decided to sugar it up a bit with a simple class.
Maybe we extend it in the future, but for now it serves its intended purpose (also, it's mysql only for now, but that will change).

### INSTALLATION
#### A simple example to read from a table called pages(with manual installation / no composer):

    require_once(__DIR__.'/Pixxel/Dbal.php');
  
    $dbal = new \Pixxel\Dbal('dbusername', 'dbpassword', 'dbname');
    $pages = $dbal->read('select * from `pages` limit 20 order by `id` desc');
  
#### The same example install and usage with composer:

From the terminal (in the desired directory):
    composer require pixxel/dbal
    
Then, in that directory, create a php file and use this code:

    require_once(__DIR__.'/vendor/autoload.php');
    
    $dbal = new \Pixxel\Dbal('dbusername', 'dbpassword', 'dbname');
    $pages = $dbal->read('select * from `pages` limit 20 order by `id` desc');

As you can see, its mostly normal sql, but without all the things around no one can remember like pdo connection strings and stuff with statements.
Note that we always use prepared statements, so make sure that you use its biggest advantage, e.g. pass potentially vulnerable content as parameters:

    $singlePage = $dbal->read('select * from `pages` where `id` = :id', [':id' => 5]);
    
This way you do not have to worry about sql-injection when passing in parameters that the user has control of.
Here is a brief summary of what you can do with this class:

1.) Create a PDO connection:

    $dbal = new \Pixxel\Dbal('dbusername', 'dbpassword', 'dbname');
    
2.) Select data:

    $pages = $dbal->read('select * from `pages` limit 20 order by `id` desc');          // Return array containing objects
    $pages = $dbal->readSingle('select * from `pages` limit 1 order by `id` desc');     // Return a single object, regardless of how many rows the query returns
    
3.) Insert, update or delete data:

    $dbal->save('insert into `pages` (`title`, `content`) values (:title, :content)', [':title' => 'New page', ':content' => '<h1>Title</h1><p>My content</p>']);
    
4.) Get the id of the last inserted row:

    $dbal->lastInsertId();
    
5.) Get the last sql error (for debugging purposes):

    $dbal->lastError();
    
6.) Get the last executed query (or has been tried to execute):

    $dbal->lastQuery();
    
That's all for now!
