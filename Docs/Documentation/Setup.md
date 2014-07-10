Setup
=====

1. Place the favorites folder into any of your plugin directories for your app (for example `app/Plugin` or `root_dir/plugins`)
2. Create the required database tables using either the schema shell or the migrations plugin:

	cake schema create --plugin Favorites --name favorites
	cake Migrations.migration run all --plugin Favorites

3. This plugin requires that you setup some parameters in global Configure storage:


1. `Favorites.types contains supported objects that allowed to be stored as favorites.
2. `Favorites.modelCategories allow to list all models and required contains for it.
3. `Favorites.defaultTexts sets the default text for the helper toggleFavorite method

Example:

```php
Configure::write('Favorites.types', array(
	'post' => 'Blogs.Post',
	'link' => 'Link'
));
Configure::write('Favorites.defaultTexts', array(
	'favorite' => __('Favorite it', true),
	'watch' => __('Watch it', true)
));
Configure::write('Favorites.modelCategories', array(
	'Post', 'Link'
));
```

Or you could use the Configure::load() method to load a configuration file that has content similar to that below:

```php
$config['Favorites'] = array(
	'types' => array(
		'favorite' => 'Post',
		'watch' => 'Post'
	),
	'defaultTexts' => array(
		'favorite' => __('Favorite it', true),
		'watch' => __('Watch it', true)
	),
	'modelCategories' => array(
		'Post'
	)
);
```