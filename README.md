## PubSub for PHP
This is an implementation of PubSub for PHP that's designed to have a simple api.

## Usage

```php
<?php

include 'pub_sub.php';

// called when the admin area is loaded
PubSub::subscribe('/admin/load', function() {
  AdminArea::add_nav_link('Log out', '/logout');
});

// used to add js files to the page
PubSub::subscribe('/enqueue_js', function($additional_js = array()) {
  AssetManager::add_js(array_merge(array(
    'http://code.jquery.com/jquery.min.js',
    '/js/jquery.anything-slider.js'
  ), $additional_js));
});
?>

  <!-- product page -->
  <?php PubSub::publish('/enqueue_js', array('/js/product-gallery.js')); ?>
</body>
</html>
```


Basic Usage:
------------
```php
PubSub::subscribe('my_event', function($message){
  echo $message;
});
PubSub::publish('my_event', 'Hello world');  // echo 'Hello world'
```

Removing a Single Callback from an Event:
---------------------------------------
PubSub internally creates unique ids for callbacks, so they can be removed
```php
$callback = function($message){
  echo $message;
};
PubSub::subscribe('my_event', $callback);
PubSub::publish('my_event', 'Hello world');  // echo 'Hello world'
PubSub::unsubscribe('my_event', $callback);
PubSub::publish('my_event', 'Hello world'); // does nothing
```

Removing all Callbacks for an Event:
----------------------------------
```php
PubSub::subscribe('my_event', function($message){
  echo $message;
});
PubSub::unsubscribe('my_event');
PubSub::publish('my_event', 'Hello world'); // does nothing
```

Get Array of Registered Callbacks for an Event
--------------------------------------------
```php
PubSub::subscribe('my_event', function($message){
  echo $message;
});
PubSub::subscribe('my_event', function(){
  return false;
});
PubSub::subscriptions('my_event'); // returns numeric array with both callbacks, in the order that they would execute
```


Get Array of all Registered Events and their Subscriptions
--------------------------------------------
```php
PubSub::subscribe('my_event', function($message){
  echo $message;
});
PubSub::subscribe('my_event', function(){
  return false;
});
PubSub::events(); // returns associative array, of callbacks, indexed by event name
```
