<<<<<<< HEAD:zh_CN/api/Phalcon_Mvc_Model.rst
Class **Phalcon\\Mvc\\Model**
=============================

*implements* Serializable

Phalcon\\Mvc\\Model connects business objects and database tables to create a persistable domain model where logic and data are presented in one wrapping. It‘s an implementation of the object-relational mapping (ORM).   A model represents the information (data) of the application and the rules to manipulate that data. Models are primarily used for managing the rules of interaction with a corresponding database table. In most cases, each table in your database will correspond to one model in your application. The bulk of your application’s business logic will be concentrated in the models.   Phalcon\\Mvc\\Model is the first ORM written in C-language for PHP, giving to developers high performance when interacting with databases while is also easy to use.   

.. code-block:: php

    <?php

     $robot = new Robots();
     $robot->type = 'mechanical'
     $robot->name = 'Astro Boy';
     $robot->year = 1952;
     if ($robot->save() == false) {
      echo "Umh, We can store robots: ";
      foreach ($robot->getMessages() as $message) {
        echo $message;
      }
     } else {
      echo "Great, a new robot was saved successfully!";
     }



Constants
---------

*integer* **OP_CREATE**

*integer* **OP_UPDATE**

*integer* **OP_DELETE**

Methods
---------

final public  **__construct** (:doc:`Phalcon\\DI <Phalcon_DI>` $dependencyInjector, *string* $managerService, *string* $dbService)

Phalcon\\Mvc\\Model constructor



public  **setDI** (:doc:`Phalcon\\DI <Phalcon_DI>` $dependencyInjector)

Sets the dependency injection container



public :doc:`Phalcon\\DI <Phalcon_DI>`  **getDI** ()

Returns the dependency injection container



public  **setEventsManager** (:doc:`Phalcon\\Events\\Manager <Phalcon_Events_Manager>` $eventsManager)

Sets the event manager



public :doc:`Phalcon\\Events\\Manager <Phalcon_Events_Manager>`  **getEventsManager** ()

Returns the internal event manager



protected static *array*  **_createSQLSelect** ()

Creates a SQL statement which returns many rows



protected static  **_getOrCreateResultset** ()

Gets a resulset from the cache or creates one



public :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setTransaction** (:doc:`Phalcon\\Mvc\\Model\\Transaction <Phalcon_Mvc_Model_Transaction>` $transaction)

Sets a transaction related to the Model instance 

.. code-block:: php

    <?php

    try {
    
      $transactionManager = new Phalcon\Mvc\Model\Transaction\Manager();
    
      $transaction = $transactionManager->get();
    
      $robot = new Robots();
      $robot->setTransaction($transaction);
      $robot->name = 'WALL·E';
      $robot->created_at = date('Y-m-d');
      if($robot->save()==false){
        $transaction->rollback("Can't save robot");
      }
    
      $robotPart = new RobotParts();
      $robotPart->setTransaction($transaction);
      $robotPart->type = 'head';
      if ($robotPart->save() == false) {
        $transaction->rollback("Can't save robot part");
      }
    
      $transaction->commit();
    
    }
    catch(Phalcon\Mvc\Model\Transaction\Failed $e){
      echo 'Failed, reason: ', $e->getMessage();
    }




protected :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setSource** ()

Sets table name which model should be mapped



public *string*  **getSource** ()

Returns table name mapped in the model



protected :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setSchema** ()

Sets schema name where table mapped is located



public *string*  **getSchema** ()

Returns schema name where table mapped is located



public  **setConnectionService** (*string* $connectionService)

Sets DependencyInjection connection service



public *$connectionService*  **getConnectionService** ()

Returns DependencyInjection connection service



public  **setForceExists** (*unknown* $forceExists)





public :doc:`Phalcon\\Db <Phalcon_Db>`  **getConnection** ()

Gets internal database connection



public static :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  $result **dumpResult** (:doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>` $base, *array* $result)

Assigns values to a model from an array returning a new model 

.. code-block:: php

    <?php

    $robot = Phalcon\Mvc\Model::dumpResult(new Robots(), array(
      'type' => 'mechanical',
      'name' => 'Astro Boy',
      'year' => 1952
    ));




public static :doc:`Phalcon\\Mvc\\Model\\Resultset <Phalcon_Mvc_Model_Resultset>`  **find** (*array* $parameters)

Allows to query a set of records that match the specified conditions 

.. code-block:: php

    <?php

     //How many robots are there?
     $robots = Robots::find();
     echo "There are ", count($robots);
    
     //How many mechanical robots are there?
     $robots = Robots::find("type='mechanical'");
     echo "There are ", count($robots);
    
     //Get and print virtual robots ordered by name
     $robots = Robots::find(array("type='virtual'", "order" => "name"));
     foreach ($robots as $robot) {
       echo $robot->name, "\n";
     }
    
     //Get first 100 virtual robots ordered by name
     $robots = Robots::find(array("type='virtual'", "order" => "name", "limit" => 100));
     foreach ($robots as $robot) {
       echo $robot->name, "\n";
     }




public static :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **findFirst** (*array* $parameters)

Allows to query the first record that match the specified conditions 

.. code-block:: php

    <?php

     //What's the first robot in robots table?
     $robot = Robots::findFirst();
     echo "The robot name is ", $robot->name;
    
     //What's the first mechanical robot in robots table?
     $robot = Robots::findFirst("type='mechanical'");
     echo "The first mechanical robot name is ", $robot->name;
    
     //Get first virtual robot ordered by name
     $robot = Robots::findFirst(array("type='virtual'", "order" => "name"));
     echo "The first virtual robot name is ", $robot->name;




public static :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **query** (*unknown* $dependencyInjector)

Create a criteria for a especific model



protected *boolean*  **_exists** ()

Checks if the current record already exists or not



protected static :doc:`Phalcon\\Mvc\\Model\\Resultset <Phalcon_Mvc_Model_Resultset>`  **_prepareGroupResult** ()

Generate a SQL SELECT statement for an aggregate



protected static *array|Phalcon\Mvc\Model\Resultset*  **_getGroupResult** ()

Generate a resulset from an SQL select with aggregations



public static *int*  **count** (*array* $parameters)

Allows to count how many records match the specified conditions 

.. code-block:: php

    <?php

     //How many robots are there?
     $number = Robots::count();
     echo "There are ", $number;
    
     //How many mechanical robots are there?
     $number = Robots::count("type='mechanical'");
     echo "There are ", $number, " mechanical robots";




public static *double*  **sum** (*array* $parameters)

Allows to a calculate a summatory on a column that match the specified conditions 

.. code-block:: php

    <?php

     //How much are all robots?
     $sum = Robots::sum(array('column' => 'price'));
     echo "The total price of robots is ", $sum;
    
     //How much are mechanical robots?
     $sum = Robots::sum(array("type='mechanical'", 'column' => 'price'));
     echo "The total price of mechanical robots is  ", $sum;




public static *mixed*  **maximum** (*array* $parameters)

Allows to get the maximum value of a column that match the specified conditions 

.. code-block:: php

    <?php

     //What is the maximum robot id?
     $id = Robots::maximum(array('column' => 'id'));
     echo "The maximum robot id is: ", $id;
    
     //What is the maximum id of mechanical robots?
     $sum = Robots::maximum(array("type='mechanical'", 'column' => 'id'));
     echo "The maximum robot id of mechanical robots is ", $id;




public static *mixed*  **minimum** (*array* $parameters)

Allows to get the minimum value of a column that match the specified conditions 

.. code-block:: php

    <?php

     //What is the minimum robot id?
     $id = Robots::minimum(array('column' => 'id'));
     echo "The minimum robot id is: ", $id;
    
     //What is the minimum id of mechanical robots?
     $sum = Robots::minimum(array("type='mechanical'", 'column' => 'id'));
     echo "The minimum robot id of mechanical robots is ", $id;




public static *double*  **average** (*array* $parameters)

Allows to calculate the average value on a column matching the specified conditions 

.. code-block:: php

    <?php

     //What's the average price of robots?
     $average = Robots::average(array('column' => 'price'));
     echo "The average price is ", $average;
    
     //What's the average price of mechanical robots?
     $average = Robots::average(array("type='mechanical'", 'column' => 'price'));
     echo "The average price of mechanical robots is ", $average;




protected *boolean*  **_callEvent** ()

Fires an internal event



protected *boolean*  **_callEventCancel** ()

Fires an internal event that cancels the operation



protected *boolean*  **_cancelOperation** ()

Cancel the current operation



public  **appendMessage** (:doc:`Phalcon\\Mvc\\Model\\Message <Phalcon_Mvc_Model_Message>` $message)

Appends a customized message on the validation process 

.. code-block:: php

    <?php

     use \Phalcon\Mvc\Model\Message as Message;
    
     class Robots extends Phalcon\Mvc\Model
     {
    
       public function beforeSave()
       {
         if (this->name == 'Peter') {
            $message = new Message("Sorry, but a robot cannot be named Peter");
            $this->appendMessage($message);
         }
       }
     }




protected  **validate** ()

Executes validators on every validation call 

.. code-block:: php

    <?php

    use Phalcon\Mvc\Model\Validator\ExclusionIn as ExclusionIn;
    
    class Subscriptors extends Phalcon\Mvc\Model
    {
    
    public function validation()
      {
     		$this->validate(new ExclusionIn(array(
    		'field' => 'status',
    		'domain' => array('A', 'I')
    	)));
    	if ($this->validationHasFailed() == true) {
    		return false;
    	}
    }
    
    }




public *boolean*  **validationHasFailed** ()

Check whether validation process has generated any messages 

.. code-block:: php

    <?php

    use Phalcon\Mvc\Model\Validator\ExclusionIn as ExclusionIn;
    
    class Subscriptors extends Phalcon\Mvc\Model
    {
    
    public function validation()
      {
     		$this->validate(new ExclusionIn(array(
    		'field' => 'status',
    		'domain' => array('A', 'I')
    	)));
    	if ($this->validationHasFailed() == true) {
    		return false;
    	}
    }
    
    }




public :doc:`Phalcon\\Mvc\\Model\\Message <Phalcon_Mvc_Model_Message>` [] **getMessages** ()

Returns all the validation messages 

.. code-block:: php

    <?php

    $robot = new Robots();
    $robot->type = 'mechanical';
    $robot->name = 'Astro Boy';
    $robot->year = 1952;
    if ($robot->save() == false) {
      echo "Umh, We can't store robots right now ";
      foreach ($robot->getMessages() as $message) {
        echo $message;
      }
    } else {
      echo "Great, a new robot was saved successfully!";
    }




protected *boolean*  **_checkForeignKeys** ()

Reads "belongs to" relations and check the virtual foreign keys when inserting or updating records



protected *boolean*  **_checkForeignKeysReverse** ()

Reads both "hasMany" and "hasOne" relations and check the virtual foreign keys when deleting records



protected *boolean*  **_preSave** ()

Executes internal hooks before save a record



protected *boolean*  **_postSave** ()

Executes internal events after save a record



protected *boolean*  **_doLowInsert** ()

Sends a pre-build INSERT SQL statement to the relational database system



protected *boolean*  **_doLowUpdate** ()

Sends a pre-build UPDATE SQL statement to the relational database system



public *boolean*  **save** ()

Inserts or updates a model instance. Returning true on success or false otherwise. 

.. code-block:: php

    <?php

     //Creating a new robot
    $robot = new Robots();
    $robot->type = 'mechanical'
    $robot->name = 'Astro Boy';
    $robot->year = 1952;
    $robot->save();
    
     //Updating a robot name
    $robot = Robots::findFirst("id=100");
    $robot->name = "Biomass";
    $robot->save();




public  **create** ()

...


public  **update** ()

...


public *boolean*  **delete** ()

Deletes a model instance. Returning true on success or false otherwise. 

.. code-block:: php

    <?php

    $robot = Robots::findFirst("id=100");
    $robot->delete();
    
    foreach(Robots::find("type = 'mechanical'") as $robot){
       $robot->delete();
    }




public *mixed*  **readAttribute** (*string* $attribute)

Reads an attribute value by its name <code> echo $robot->readAttribute('name');



public  **writeAttribute** (*string* $attribute, *mixed* $value)

Writes an attribute value by its name <code>$robot->writeAttribute('name', 'Rosey');



protected  **hasOne** ()

Setup a 1-1 relation between two models 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize(){
           $this->hasOne('id', 'RobotsDescription', 'robots_id');
       }
    
    }




protected  **belongsTo** ()

Setup a relation reverse 1-1  between two models 

.. code-block:: php

    <?php

    class RobotsParts extends \Phalcon\Mvc\Model
    {
    
       public function initialize(){
           $this->belongsTo('robots_id', 'Robots', 'id');
       }
    
    }




protected  **hasMany** ()

Setup a relation 1-n between two models 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
           $this->hasMany('id', 'RobotsParts', 'robots_id');
       }
    
    }




protected  **__getRelatedRecords** ()

...


public *mixed*  **__call** (*string* $method, *array* $arguments)

Handles methods when a method does not exist



public  **serialize** ()

...


public  **unserialize** (*unknown* $data)

...


=======
Abstract class **Phalcon\\Mvc\\Model**
======================================

*implements* :doc:`Phalcon\\Mvc\\ModelInterface <Phalcon_Mvc_ModelInterface>`, :doc:`Phalcon\\Mvc\\Model\\ResultInterface <Phalcon_Mvc_Model_ResultInterface>`, :doc:`Phalcon\\DI\\InjectionAwareInterface <Phalcon_DI_InjectionAwareInterface>`, Serializable

Phalcon\\Mvc\\Model connects business objects and database tables to create a persistable domain model where logic and data are presented in one wrapping. It‘s an implementation of the object-relational mapping (ORM).    A model represents the information (data) of the application and the rules to manipulate that data. Models are primarily used for managing the rules of interaction with a corresponding database table. In most cases, each table in your database will correspond to one model in your application. The bulk of your application’s business logic will be concentrated in the models.    Phalcon\\Mvc\\Model is the first ORM written in C-language for PHP, giving to developers high performance when interacting with databases while is also easy to use.    

.. code-block:: php

    <?php

     $robot = new Robots();
     $robot->type = 'mechanical';
     $robot->name = 'Astro Boy';
     $robot->year = 1952;
     if ($robot->save() == false) {
      echo "Umh, We can store robots: ";
      foreach ($robot->getMessages() as $message) {
        echo $message;
      }
     } else {
      echo "Great, a new robot was saved successfully!";
     }



Constants
---------

*integer* **OP_NONE**

*integer* **OP_CREATE**

*integer* **OP_UPDATE**

*integer* **OP_DELETE**

*integer* **DIRTY_STATE_PERSISTENT**

*integer* **DIRTY_STATE_TRANSIENT**

*integer* **DIRTY_STATE_DETACHED**

Methods
-------

final public  **__construct** ([:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector], [:doc:`Phalcon\\Mvc\\Model\\ManagerInterface <Phalcon_Mvc_Model_ManagerInterface>` $modelsManager])

Phalcon\\Mvc\\Model constructor



public  **setDI** (:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector)

Sets the dependency injection container



public :doc:`Phalcon\\DiInterface <Phalcon_DiInterface>`  **getDI** ()

Returns the dependency injection container



protected  **setEventsManager** ()

Sets a custom events manager



protected :doc:`Phalcon\\Events\\ManagerInterface <Phalcon_Events_ManagerInterface>`  **getEventsManager** ()

Returns the custom events manager



public :doc:`Phalcon\\Mvc\\Model\\MetaDataInterface <Phalcon_Mvc_Model_MetaDataInterface>`  **getModelsMetaData** ()

Returns the models meta-data service related to the entity instance



public :doc:`Phalcon\\Mvc\\Model\\ManagerInterface <Phalcon_Mvc_Model_ManagerInterface>`  **getModelsManager** ()

Returns the models manager related to the entity instance



public :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setTransaction** (:doc:`Phalcon\\Mvc\\Model\\TransactionInterface <Phalcon_Mvc_Model_TransactionInterface>` $transaction)

Sets a transaction related to the Model instance 

.. code-block:: php

    <?php

    use Phalcon\Mvc\Model\Transaction\Manager as TxManager;
    use Phalcon\Mvc\Model\Transaction\Failed as TxFailed;
    
    try {
    
      $txManager = new TxManager();
    
      $transaction = $txManager->get();
    
      $robot = new Robots();
      $robot->setTransaction($transaction);
      $robot->name = 'WALL·E';
      $robot->created_at = date('Y-m-d');
      if ($robot->save() == false) {
        $transaction->rollback("Can't save robot");
      }
    
      $robotPart = new RobotParts();
      $robotPart->setTransaction($transaction);
      $robotPart->type = 'head';
      if ($robotPart->save() == false) {
        $transaction->rollback("Robot part cannot be saved");
      }
    
      $transaction->commit();
    
    } catch (TxFailed $e) {
      echo 'Failed, reason: ', $e->getMessage();
    }




protected :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setSource** (*string* $source)

Sets table name which model should be mapped



public *string*  **getSource** ()

Returns table name mapped in the model



protected :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setSchema** (*string* $schema)

Sets schema name where table mapped is located



public *string*  **getSchema** ()

Returns schema name where table mapped is located



public :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setConnectionService** (*string* $connectionService)

Sets the DependencyInjection connection service name



public :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setReadConnectionService** (*string* $connectionService)

Sets the DependencyInjection connection service name used to read data



public :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setWriteConnectionService** (*string* $connectionService)

Sets the DependencyInjection connection service name used to write data



public *string*  **getReadConnectionService** ()

Returns the DependencyInjection connection service name used to read data related the model



public *string*  **getWriteConnectionService** ()

Returns the DependencyInjection connection service name used to write data related to the model



public :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **setDirtyState** (*int* $dirtyState)

Sets the dirty state of the object using one of the DIRTY_STATE_* constants



public *int*  **getDirtyState** ()

Returns one of the DIRTY_STATE_* constants telling if the record exists in the database or not



public :doc:`Phalcon\\Db\\AdapterInterface <Phalcon_Db_AdapterInterface>`  **getReadConnection** ()

Gets the connection used to read data for the model



public :doc:`Phalcon\\Db\\AdapterInterface <Phalcon_Db_AdapterInterface>`  **getWriteConnection** ()

Gets the connection used to write data to the model



public :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **assign** (*array* $data, [*array* $columnMap])

Assigns values to a model from an array 

.. code-block:: php

    <?php

    $robot->assign(array(
      'type' => 'mechanical',
      'name' => 'Astro Boy',
      'year' => 1952
    ));




public static :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **cloneResultMap** (:doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>` $base, *array* $data, *array* $columnMap, [*int* $dirtyState], [*boolean* $keepSnapshots])

Assigns values to a model from an array returning a new model. 

.. code-block:: php

    <?php

    $robot = \Phalcon\Mvc\Model::cloneResultMap(new Robots(), array(
      'type' => 'mechanical',
      'name' => 'Astro Boy',
      'year' => 1952
    ));




public static *mixed*  **cloneResultMapHydrate** (*array* $data, *array* $columnMap, *int* $hydrationMode)

Returns an hydrated result based on the data and the column map



public static :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **cloneResult** (:doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>` $base, *array* $data, [*int* $dirtyState])

Assigns values to a model from an array returning a new model 

.. code-block:: php

    <?php

    $robot = Phalcon\Mvc\Model::cloneResult(new Robots(), array(
      'type' => 'mechanical',
      'name' => 'Astro Boy',
      'year' => 1952
    ));




public static :doc:`Phalcon\\Mvc\\Model\\ResultsetInterface <Phalcon_Mvc_Model_ResultsetInterface>`  **find** ([*array* $parameters])

Allows to query a set of records that match the specified conditions 

.. code-block:: php

    <?php

     //How many robots are there?
     $robots = Robots::find();
     echo "There are ", count($robots), "\n";
    
     //How many mechanical robots are there?
     $robots = Robots::find("type='mechanical'");
     echo "There are ", count($robots), "\n";
    
     //Get and print virtual robots ordered by name
     $robots = Robots::find(array("type='virtual'", "order" => "name"));
     foreach ($robots as $robot) {
       echo $robot->name, "\n";
     }
    
     //Get first 100 virtual robots ordered by name
     $robots = Robots::find(array("type='virtual'", "order" => "name", "limit" => 100));
     foreach ($robots as $robot) {
       echo $robot->name, "\n";
     }




public static :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **findFirst** ([*array* $parameters])

Allows to query the first record that match the specified conditions 

.. code-block:: php

    <?php

     //What's the first robot in robots table?
     $robot = Robots::findFirst();
     echo "The robot name is ", $robot->name;
    
     //What's the first mechanical robot in robots table?
     $robot = Robots::findFirst("type='mechanical'");
     echo "The first mechanical robot name is ", $robot->name;
    
     //Get first virtual robot ordered by name
     $robot = Robots::findFirst(array("type='virtual'", "order" => "name"));
     echo "The first virtual robot name is ", $robot->name;




public static :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **query** ([:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector])

Create a criteria for a specific model



protected *boolean*  **_exists** ()

Checks if the current record already exists or not



protected static :doc:`Phalcon\\Mvc\\Model\\ResultsetInterface <Phalcon_Mvc_Model_ResultsetInterface>`  **_groupResult** ()

Generate a PHQL SELECT statement for an aggregate



public static *int*  **count** ([*array* $parameters])

Allows to count how many records match the specified conditions 

.. code-block:: php

    <?php

     //How many robots are there?
     $number = Robots::count();
     echo "There are ", $number, "\n";
    
     //How many mechanical robots are there?
     $number = Robots::count("type='mechanical'");
     echo "There are ", $number, " mechanical robots\n";




public static *double*  **sum** ([*array* $parameters])

Allows to calculate a summatory on a column that match the specified conditions 

.. code-block:: php

    <?php

     //How much are all robots?
     $sum = Robots::sum(array('column' => 'price'));
     echo "The total price of robots is ", $sum, "\n";
    
     //How much are mechanical robots?
     $sum = Robots::sum(array("type='mechanical'", 'column' => 'price'));
     echo "The total price of mechanical robots is  ", $sum, "\n";




public static *mixed*  **maximum** ([*array* $parameters])

Allows to get the maximum value of a column that match the specified conditions 

.. code-block:: php

    <?php

     //What is the maximum robot id?
     $id = Robots::maximum(array('column' => 'id'));
     echo "The maximum robot id is: ", $id, "\n";
    
     //What is the maximum id of mechanical robots?
     $sum = Robots::maximum(array("type='mechanical'", 'column' => 'id'));
     echo "The maximum robot id of mechanical robots is ", $id, "\n";




public static *mixed*  **minimum** ([*array* $parameters])

Allows to get the minimum value of a column that match the specified conditions 

.. code-block:: php

    <?php

     //What is the minimum robot id?
     $id = Robots::minimum(array('column' => 'id'));
     echo "The minimum robot id is: ", $id;
    
     //What is the minimum id of mechanical robots?
     $sum = Robots::minimum(array("type='mechanical'", 'column' => 'id'));
     echo "The minimum robot id of mechanical robots is ", $id;




public static *double*  **average** ([*array* $parameters])

Allows to calculate the average value on a column matching the specified conditions 

.. code-block:: php

    <?php

     //What's the average price of robots?
     $average = Robots::average(array('column' => 'price'));
     echo "The average price is ", $average, "\n";
    
     //What's the average price of mechanical robots?
     $average = Robots::average(array("type='mechanical'", 'column' => 'price'));
     echo "The average price of mechanical robots is ", $average, "\n";




public *boolean*  **fireEvent** (*string* $eventName)

Fires an event, implicitly calls behaviors and listeners in the events manager are notified



public *boolean*  **fireEventCancel** (*string* $eventName)

Fires an event, implicitly calls behaviors and listeners in the events manager are notified This method stops if one of the callbacks/listeners returns boolean false



protected *boolean*  **_cancelOperation** ()

Cancel the current operation



public :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **appendMessage** (:doc:`Phalcon\\Mvc\\Model\\MessageInterface <Phalcon_Mvc_Model_MessageInterface>` $message)

Appends a customized message on the validation process 

.. code-block:: php

    <?php

     use \Phalcon\Mvc\Model\Message as Message;
    
     class Robots extends Phalcon\Mvc\Model
     {
    
       public function beforeSave()
       {
         if ($this->name == 'Peter') {
            $message = new Message("Sorry, but a robot cannot be named Peter");
            $this->appendMessage($message);
         }
       }
     }




protected :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **validate** (:doc:`Phalcon\\Mvc\\Model\\ValidatorInterface <Phalcon_Mvc_Model_ValidatorInterface>` $validator)

Executes validators on every validation call 

.. code-block:: php

    <?php

    use Phalcon\Mvc\Model\Validator\ExclusionIn as ExclusionIn;
    
    class Subscriptors extends Phalcon\Mvc\Model
    {
    
    public function validation()
      {
     		$this->validate(new ExclusionIn(array(
    		'field' => 'status',
    		'domain' => array('A', 'I')
    	)));
    	if ($this->validationHasFailed() == true) {
    		return false;
    	}
    }
    
    }




public *boolean*  **validationHasFailed** ()

Check whether validation process has generated any messages 

.. code-block:: php

    <?php

    use Phalcon\Mvc\Model\Validator\ExclusionIn as ExclusionIn;
    
    class Subscriptors extends Phalcon\Mvc\Model
    {
    
    public function validation()
      {
     		$this->validate(new ExclusionIn(array(
    		'field' => 'status',
    		'domain' => array('A', 'I')
    	)));
    	if ($this->validationHasFailed() == true) {
    		return false;
    	}
    }
    
    }




public :doc:`Phalcon\\Mvc\\Model\\MessageInterface <Phalcon_Mvc_Model_MessageInterface>` [] **getMessages** ([*unknown* $filter])

Returns all the validation messages 

.. code-block:: php

    <?php

    $robot = new Robots();
    $robot->type = 'mechanical';
    $robot->name = 'Astro Boy';
    $robot->year = 1952;
    if ($robot->save() == false) {
      	echo "Umh, We can't store robots right now ";
      	foreach ($robot->getMessages() as $message) {
    		echo $message;
    	}
    } else {
      	echo "Great, a new robot was saved successfully!";
    }




protected *boolean*  **_checkForeignKeysRestrict** ()

Reads "belongs to" relations and check the virtual foreign keys when inserting or updating records to verify that inserted/updated values are present in the related entity



protected *boolean*  **_checkForeignKeysReverseRestrict** ()

Reads both "hasMany" and "hasOne" relations and checks the virtual foreign keys (restrict) when deleting records



protected *boolean*  **_checkForeignKeysReverseCascade** ()

Reads both "hasMany" and "hasOne" relations and checks the virtual foreign keys (cascade) when deleting records



protected *boolean*  **_preSave** ()

Executes internal hooks before save a record



protected *boolean*  **_postSave** ()

Executes internal events after save a record



protected *boolean*  **_doLowInsert** ()

Sends a pre-build INSERT SQL statement to the relational database system



protected *boolean*  **_doLowUpdate** ()

Sends a pre-build UPDATE SQL statement to the relational database system



protected *boolean*  **_preSaveRelatedRecords** ()

Saves related records that must be stored prior to save the master record



protected *boolean*  **_postSaveRelatedRecords** ()

Save the related records assigned in the has-one/has-many relations



public *boolean*  **save** ([*array* $data], [*array* $whiteList])

Inserts or updates a model instance. Returning true on success or false otherwise. 

.. code-block:: php

    <?php

    //Creating a new robot
    $robot = new Robots();
    $robot->type = 'mechanical';
    $robot->name = 'Astro Boy';
    $robot->year = 1952;
    $robot->save();
    
    //Updating a robot name
    $robot = Robots::findFirst("id=100");
    $robot->name = "Biomass";
    $robot->save();




public *boolean*  **create** ([*array* $data], [*array* $whiteList])

Inserts a model instance. If the instance already exists in the persistance it will throw an exception Returning true on success or false otherwise. 

.. code-block:: php

    <?php

    //Creating a new robot
    $robot = new Robots();
    $robot->type = 'mechanical';
    $robot->name = 'Astro Boy';
    $robot->year = 1952;
    $robot->create();
    
      //Passing an array to create
      $robot = new Robots();
      $robot->create(array(
          'type' => 'mechanical',
          'name' => 'Astroy Boy',
          'year' => 1952
      ));




public *boolean*  **update** ([*array* $data], [*array* $whiteList])

Updates a model instance. If the instance doesn't exist in the persistance it will throw an exception Returning true on success or false otherwise. 

.. code-block:: php

    <?php

    //Updating a robot name
    $robot = Robots::findFirst("id=100");
    $robot->name = "Biomass";
    $robot->update();




public *boolean*  **delete** ()

Deletes a model instance. Returning true on success or false otherwise. 

.. code-block:: php

    <?php

    $robot = Robots::findFirst("id=100");
    $robot->delete();
    
    foreach (Robots::find("type = 'mechanical'") as $robot) {
       $robot->delete();
    }




public *int*  **getOperationMade** ()

Returns the type of the latest operation performed by the ORM Returns one of the OP_* class constants



public  **refresh** ()

Refreshes the model attributes re-querying the record from the database



public  **skipOperation** (*boolean* $skip)

Skips the current operation forcing a success state



public *mixed*  **readAttribute** (*string* $attribute)

Reads an attribute value by its name 

.. code-block:: php

    <?php

     echo $robot->readAttribute('name');




public  **writeAttribute** (*string* $attribute, *mixed* $value)

Writes an attribute value by its name 

.. code-block:: php

    <?php

     	$robot->writeAttribute('name', 'Rosey');




protected  **skipAttributes** (*array* $attributes, [*boolean* $replace])

Sets a list of attributes that must be skipped from the generated INSERT/UPDATE statement 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
           $this->skipAttributes(array('price'));
       }
    
    }




protected  **skipAttributesOnCreate** (*array* $attributes, [*boolean* $replace])

Sets a list of attributes that must be skipped from the generated INSERT statement 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
           $this->skipAttributesOnCreate(array('created_at'));
       }
    
    }




protected  **skipAttributesOnUpdate** (*array* $attributes, [*boolean* $replace])

Sets a list of attributes that must be skipped from the generated UPDATE statement 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
           $this->skipAttributesOnUpdate(array('modified_in'));
       }
    
    }




public :doc:`Phalcon\\Mvc\\Model\\Relation <Phalcon_Mvc_Model_Relation>`  **hasOne** (*mixed* $fields, *string* $referenceModel, *mixed* $referencedFields, [*array* $options])

Setup a 1-1 relation between two models 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
           $this->hasOne('id', 'RobotsDescription', 'robots_id');
       }
    
    }




public :doc:`Phalcon\\Mvc\\Model\\Relation <Phalcon_Mvc_Model_Relation>`  **belongsTo** (*mixed* $fields, *string* $referenceModel, *mixed* $referencedFields, [*array* $options])

Setup a relation reverse 1-1  between two models 

.. code-block:: php

    <?php

    class RobotsParts extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
           $this->belongsTo('robots_id', 'Robots', 'id');
       }
    
    }




public :doc:`Phalcon\\Mvc\\Model\\Relation <Phalcon_Mvc_Model_Relation>`  **hasMany** (*mixed* $fields, *string* $referenceModel, *mixed* $referencedFields, [*array* $options])

Setup a relation 1-n between two models 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
           $this->hasMany('id', 'RobotsParts', 'robots_id');
       }
    
    }




public :doc:`Phalcon\\Mvc\\Model\\Relation <Phalcon_Mvc_Model_Relation>`  **hasManyToMany** (*string* $fields, *string* $intermediateModel, *string* $intermediateFields, *string* $intermediateReferencedFields, *unknown* $referenceModel, *string* $referencedFields, [*array* $options])

Setup a relation n-n between two models through an intermediate relation 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
           //Setup a many-to-many relation to Parts through RobotsParts
           $this->hasManyToMany(
    		'id',
    		'RobotsParts',
    		'robots_id',
    		'parts_id',
    		'Parts',
    		'id'
    	);
       }
    
    }




public  **addBehavior** (:doc:`Phalcon\\Mvc\\Model\\BehaviorInterface <Phalcon_Mvc_Model_BehaviorInterface>` $behavior)

Setups a behavior in a model 

.. code-block:: php

    <?php

    use Phalcon\Mvc\Model\Behavior\Timestampable;
    
    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
    	$this->addBehavior(new Timestampable(array(
    		'onCreate' => array(
    			'field' => 'created_at',
    			'format' => 'Y-m-d'
    		)
    	)));
       }
    
    }




protected  **keepSnapshots** (*boolean* $keepSnapshots)

Sets if the model must keep the original record snapshot in memory 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
    	$this->keepSnapshots(true);
       }
    
    }




public  **setSnapshotData** (*array* $data, [*array* $columnMap])

Sets the record's snapshot data. This method is used internally to set snapshot data when the model was set up to keep snapshot data



public *boolean*  **hasSnapshotData** ()

Checks if the object has internal snapshot data



public *array*  **getSnapshotData** ()

Returns the internal snapshot data



public  **hasChanged** ([*boolean* $fieldName])

Check if a specific attribute has changed This only works if the model is keeping data snapshots



public *array*  **getChangedFields** ()

Returns a list of changed values



protected  **useDynamicUpdate** (*boolean* $dynamicUpdate)

Sets if a model must use dynamic update instead of the all-field update 

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
    
       public function initialize()
       {
    	$this->useDynamicUpdate(true);
       }
    
    }




public :doc:`Phalcon\\Mvc\\Model\\ResultsetInterface <Phalcon_Mvc_Model_ResultsetInterface>`  **getRelated** (*string* $alias, [*array* $arguments])

Returns related records based on defined relations



protected *mixed*  **_getRelatedRecords** ()

Returns related records defined relations depending on the method name



public *mixed*  **__call** (*string* $method, [*array* $arguments])

Handles method calls when a method is not implemented



public static *mixed*  **__callStatic** (*string* $method, [*array* $arguments])

Handles method calls when a static method is not implemented



public  **__set** (*string* $property, *mixed* $value)

Magic method to assign values to the the model



public :doc:`Phalcon\\Mvc\\Model\\Resultset <Phalcon_Mvc_Model_Resultset>`  **__get** (*string* $property)

Magic method to get related records using the relation alias as a property



public  **__isset** (*string* $property)

Magic method to check if a property is a valid relation



public *string*  **serialize** ()

Serializes the object ignoring connections, services, related objects or static properties



public  **unserialize** (*string* $data)

Unserializes the object from a serialized string



public *array*  **dump** ()

Returns a simple representation of the object that can be used with var_dump 

.. code-block:: php

    <?php

     var_dump($robot->dump());




public *array*  **toArray** ([*array* $columns])

Returns the instance as an array representation 

.. code-block:: php

    <?php

     print_r($robot->toArray());




public static  **setup** (*array* $options)

Enables/disables options in the ORM Available options: events                — Enables/Disables globally the internal events virtualForeignKeys    — Enables/Disables virtual foreign keys columnRenaming        — Enables/Disables column renaming notNullValidations    — Enables/Disables automatic not null validation exceptionOnFailedSave — Enables/Disables throws an exception if the saving process fails phqlLiterals          — Enables/Disables literals in PHQL this improves the security of applications



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Mvc_Model.rst
