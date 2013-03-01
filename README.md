## PubSub for PHP [![Build Status](https://travis-ci.org/BaylorRae/PubSub-PHP.png?branch=master)](https://travis-ci.org/BaylorRae/PubSub-PHP)
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
PubSub::subscribe('my_hook', function($message){
  echo $message;
});
PubSub::publish('my_hook', 'Hello world');  // echo 'Hello world'
```

Removing a Single Callback from a PubSub:
---------------------------------------
PubSub internally creates unique ids for callbacks, so they can be removed
```php
$callback = function($message){
  echo $message;
};
PubSub::subscribe('my_hook', $callback);
PubSub::publish('my_hook', 'Hello world');  // echo 'Hello world'
PubSub::unsubscribe('my_hook', $callback);
PubSub::publish('my_hook', 'Hello world'); // does nothing
```

Removing all Callbacks for a PubSub:
----------------------------------
```php
PubSub::subscribe('my_hook', function($message){
  echo $message;
});
PubSub::unsubscribe('my_hook');
PubSub::publish('my_hook', 'Hello world'); // does nothing
```

Priority and Return False
----------------------------------
```php
PubSub::subscribe('my_hook', function($message){
  echo $message;
}, 100); // priority of 100
PubSub::subscribe('my_hook', function(){
  return false;
}, 99);
PubSub::publish('my_hook', 'Hello world'); // does nothing, because the callback that returned false was executed first
```

Get Array of Registered Callbacks for a PubSub
--------------------------------------------
```php
PubSub::subscribe('my_hook', function($message){
  echo $message;
}, 100); // priority of 100
PubSub::subscribe('my_hook', function(){
  return false;
}, 99);
PubSub::callbacks('my_hook'); // returns numeric array with both callbacks, in the order that they would execute
```
