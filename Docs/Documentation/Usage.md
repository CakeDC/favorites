Usage
=====

Add the Favorites helper to your controller:

```php
public $helpers = array('Favorites.Favorites');
```

Attach the Favorite behavior to your models via the `$actsAs` variable or dynamically using the `BehaviorsCollection` object methods:

```php
public $actsAs = array('Favorites.Favorite');
// Or
$this->Behaviors->load('Favorites.Favorite');
```

Use the favourites helper in your views to generate links to mark a model record as favorite:

```php
<?php echo $this->Favorites->toggleFavorite('favorite-type', $modelId);
```

This link will toggle the "favorite-type" tag for this user and model record.

If you want the helper to distinguish whether it needs to activate or deactivate the favorite flag in for the user, you need to pass to the view the variable `userFavorites` containing an associative array of user favorites per favorite type. The following structure is needed:

```php
array(
	'favorite-type1' => array(
		'favorite-id1' => 'model-foreignKey-1',
		'favorite-id2' => 'model-foreignKey-3'
		'favorite-id3' => 'model-foreignKey-2'
	),
	'favorite-type2' => array(
		'favorite-id4' => 'model-foreignKey-1',
		'favorite-id5' => 'model-foreignKey-3'
		'favorite-id6' => 'model-foreignKey-2'
	)
);
```

You can achieve this result using with method `getAllFavorites` in `Favorite` model:

```php
$Favorite = ClassRegistry::init('Favorites.favorite');
$this->set('userFavorites', $Favorite->getAllFavorites('user-id'));
```

## Configuration Options ##

The Favorite behavior has some configuration options to adapt to your apps needs.

The configuration array accepts the following keys:

* **favoriteAlias** - The name of the association to be created with the model the Behavior is attached to and the favoriteClass model. Default: Favorite
* **favoriteClass** - If you need to extend the Favorite model or override it with your own implementation set this key to the model you want to use
* **foreignKey** - the field in your table that serves as reference for the primary key of the model it is attached to. (Used for own implementations of Favorite model)
* **counter_cache** - the name of the field that will hold the number of times the model record has been favorited

Callbacks
---------

Additionally the behavior provides two callbacks to implement in your model:

* **beforeSaveFavorite** - called before save favorite. Should return boolean value.
* **afterSaveFavorite** - called after save favorite.
