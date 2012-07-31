# Favorites Plugin for CakePHP #

Version 1.1 for cake 2.x

Favorites plugin allows to associate users to any record in your database through human readable tags or categories.

## Installation ##

1. Place the favorites folder into any of your plugin directories for your app (for example `app/Plugin` or `root_dir/plugins`)
2. Create the required database tables using either the schema shell or the migrations plugin:

		cake schema create --plugin Favorites --name favorites
		cake Migrations.migration run all --plugin Favorites

3. This plugin requires that you setup some parameters in global Configure storage:
 1. `Favorites.types contains supported objects that allowed to be stored as favorites.
 2. `Favorites.modelCategories allow to list all models and required contains for it.
 3. `Favorites.defaultTexts sets the default text for the helper toggleFavorite method

Example:

	Configure::write('Favorites.types', array('post' => 'Blogs.Post', 'link' => 'Link'));
	Configure::write('Favorites.defaultTexts', array('favorite' => __('Favorite it'), 'watch' => __('Watch it')));
	Configure::write('Favorites.modelCategories', array('Post', 'Link'));

Or you could use the Configure::load() method to load a configuration file that has content similar to that below:

	$config['Favorites'] = array(
		'types' => array(
			'favorite' => 'Post',
			'watch' => 'Post'
		),
		'defaultTexts' => array(
			'favorite' => __('Favorite it'),
			'watch' => __('Watch it')
		),
		'modelCategories' => array(
			'Post'
		)
	);

## Usage ##

Add the Favorites helper to your controller:

	public $helpers = array('Favorites.Favorites');

Attach the Favorite behavior to your models via the `$actsAs` variable or dynamically using the `BehaviorsCollection` object methods:

	public $actsAs = array('Favorites.Favorite');
	// Or
	$this->Behaviors->attach('Favorites.Favorite');

Use the favourites helper in your views to generate links to mark a model record as favorite:

	<?php echo $this->Favorites->toggleFavorite('favorite-type', $modelId); ?>

This link will toggle the "favorite-type" tag for this user and model record.

If you want the helper to distinguish whether it needs to activate or deactivate the favorite flag in for the user, you need to pass to the view the variable `userFavorites` containing an associative array of user favorites per favorite type. The following structure is needed:

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

You can achieve this result using with method `getAllFavorites` in `Favorite` model:

	$Favorite = ClassRegistry::init('Favorites.favorite');
	$this->set('userFavorites', $Favorite->getAllFavorites('user-id'));

## Configuration Options ##

The Favorite behavior has some configuration options to adapt to your apps needs.

The configuration array accepts the following keys:

* `favoriteAlias` - The name of the association to be created with the model the Behavior is attached to and the favoriteClass model. Default: Favorite
* `favoriteClass` - If you need to extend the Favorite model or override it with your own implementation set this key to the model you want to use
* `foreignKey` - the field in your table that serves as reference for the primary key of the model it is attached to. (Used for own implementations of Favorite model)
* `counter_cache` - the name of the field that will hold the number of times the model record has been favorited

## Callbacks ##

Additionally the behavior provides two callbacks to implement in your model:

* `beforeSaveFavorite` - called before save favorite. Should return boolean value.
* `afterSaveFavorite` - called after save favorite.

## Requirements ##

* PHP version: PHP 5.2+
* CakePHP version: 2.x Stable

## Requirements ##

* PHP version: PHP 5.2+
* CakePHP version: Cakephp 2.x Stable

## Support ##

For support and feature request, please visit the [Favorites Plugin Support Site](http://cakedc.lighthouseapp.com/projects/59901-favourites-plugin/).

For more information about our Professional CakePHP Services please visit the [Cake Development Corporation website](http://cakedc.com).

## Branch strategy ##

The master branch holds the STABLE latest version of the plugin. 
Develop branch is UNSTABLE and used to test new features before releasing them. 

Previous maintenance versions are named after the CakePHP compatible version, for example, branch 1.3 is the maintenance version compatible with CakePHP 1.3.
All versions are updated with security patches.

## Contributing to this Plugin ##

Please feel free to contribute to the plugin with new issues, requests, unit tests and code fixes or new features. If you want to contribute some code, create a feature branch from develop, and send us your pull request. Unit tests for new features and issues detected are mandatory to keep quality high. 


## License ##

Copyright 2009-2012, [Cake Development Corporation](http://cakedc.com)

Licensed under [The MIT License](http://www.opensource.org/licenses/mit-license.php)<br/>
Redistributions of files must retain the above copyright notice.

## Copyright ###

Copyright 2009-2012<br/>
[Cake Development Corporation](http://cakedc.com)<br/>
1785 E. Sahara Avenue, Suite 490-423<br/>
Las Vegas, Nevada 89104<br/>
http://cakedc.com<br/>
