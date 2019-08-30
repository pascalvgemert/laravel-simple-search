# Laravel Simple Search
Simplistic search Trait for Laravel eloquent models which can handle relevance score sorting.


### Requires Laravel 5.5 or higher!

## About the package

This trait make any table searchable without the use of an Elastic Search database or Full Text Search Tables.
The query consists of some simple where like methods and the possibility to sort the query on the search relevance.

## Installation

You can install the package via composer:

``` bash
composer require pascalvgemert/laravel-simple-search
```

## Usage

The `SimpleSearch` trait can be used only for Eloquent Models.

```php
class Post extends \Illuminate\Database\Eloquent\Model
{
    use SimpleSearch/SimpleSearch;

    /**
     * Specifiy the columns to search on, and add a weight score if needed.
     * Default weight: 1
     *
     * @var array
     */
    protected $searchable = [
        'title:3',
        'content',
    ];
}
```
### Search method

After this you can use the `search` method on an Eloquent Model instance, like so:

```
// Find's posts with the phrases `cats` and/or `funny`.
$results = Post::search('funny cats')->sortByDesc('score')->take(10)->get();
```

You can also specify if the search result must match *all* the keywords or *any* of the keywords.

```
// Find's posts which both include the phrases `cats` and `funny`.
$results = Post::search('funny cats', true)->sortByDesc('score')->take(10)->get();
```

If you don't want to use the relevance score to sort, you can specify to remove the score column from the result.

```
// Find's the last 10 posts with the phrases `cats` and/or `funny`.
$results = Post::search('funny cats', false, true)->sortByDesc('created_at')->take(10)->get();
```

## Author

- [Pascal van Gemert](https://github.com/pascalvgemert)
