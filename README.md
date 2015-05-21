# Sortable trait for Laravel's Eloquent models

This package adds sorting functionality to Eloquent models in Laravel 4/5.

## Composer install

Add the following line to `composer.json` file in your project:

    "jedrzej/sortable": "0.0.1"
	
or run the following in the commandline in your project's root folder:	

    composer require "jedrzej/sortable" "0.0.1"

## Setting up sortable models

In order to make an Eloquent model sortable, add the trait to the model and define a list of fields that the model can be sorted by:

    use Jedrzej\Sortable\SortableTrait;
    
    class Post extends Eloquent
    {
        use SortableTrait;
        
        public function getSortableAttributes()
        {
            return ['title', 'forum_id', 'created_at'];
        }
    }

`SortableTrait` adds a `sorted()` scope to the model - you can pass it a query being an array of sorting conditions:
 
    // return all posts sorted by creation date in descending order
    Post::sorted(['sort' => 'created_at,desc'])->get();
    
    // return all users sorted by level in ascending order and then by points indescending orders
    User::sorted(['sort' => ['level,asc', 'points,desc'])->get();
 
 or it will use `Input::all()` as default:
    
    // return all posts sorted by creation date in descending order by appending to URL
    ?sort=created_at,desc
    //and then calling
    Post::sorted()->get();

    // return all users sorted by level in ascending order and then by points indescending orders by appending to URL
    ?sort[]=level,asc&sort[]=points,desc
    // and then calling
    User::sorted()->get();