## My Favorite Laravel Collection Methods

In Laravel, collections are a powerful way to work with arrays of data. Collections provide a convenient and fluent interface for working with arrays, making it easier to perform common tasks such as filtering, mapping, and reducing. Laravel's collection class extends the base PHP Illuminate\Support\Collection class, adding additional methods for common operations.

## Database Setup <br>

Eloquent interacts with the database so make sure that your database credentials are correct
```
mysql;
create database laravel_collections;
exit;
```

The easiest way of adding dummy data is by interacting with the users table. Open the ```/database/seeders/DatabaseSeeder.php``` file and add replace the ```run()``` method with:

```ruby
public function run()
{
     \App\Models\User::factory(50)->create();
}
```
This command will create 50 new users inside your users table

Before we can use our users table, we need to make sure that we migrate our migrations and seeder
```
php artisan migrate --seed
```

## Custom Artisan Command & Routes
Instead of constantly navigating to the browser, we are going to use a custom artisan command that will run the ```index()``` method from our controller inside the CLI

Let’s perform the following two lines of code to create our custom Artisan command and our Controller
```
php artisan make:command ShowMethod --command=show:method
php artisan make:controller CollectionsController
```

Add the following method inside the  ```/app/Http/Controllers/CollectionsController.php``` file:
```ruby
public function index()
{
     $collection = collect([1, 2, 3, 4, 5]);
}
```

We need to make sure that we call this method inside the ```handle()``` method of our ```/app/Console/Commands/ShowMethod.php``` file:

```ruby
public function handle()
{
    $collectionsController = new CollectionsController();
    print_r($collectionsController->index());
}
```

This will create a new instance of the CollectionsController```($collectionsController)```, which will then print out the available ```index()``` method 

From now on, whenever we do something inside the ```index()``` method, we can easily run the following command to get output inside the CLI
```
php artisan show:method
```

## 1. avg()
The avg() method returns the average value of a given **key**

```ruby
public function index()
{
    return collect([1, 2, 3, 4, 5])->avg();
}


Output: 3
```

Wondering what the difference will be with PHP?
To achieve the same exact thing, your PHP code will look like this:
```ruby
public function index()
{
    $collection = collect([1, 2, 3, 4, 5]);
    $average = array_sum($collection)/count($collection);
    echo $average;
}


Output: 3
```

## 2. chunk()

The chunk method will break or split a large group of data into smaller groups. The ```chunk()``` method accepts one parameter, which will be the amount you want to split your array/collection with, in the example down below, it will be 2

```ruby
public function index()
{
    return collect([1, 2, 3, 4, 5])->chunk(2);
}


Output
(
     [items:protected] => Array
          (
               [0] => 1
               [1] => 2
          )
),
(
     [items:protected] => Array
          (
               [2] => 3
               [3] => 4
          )
),
(
     [items:protected] => Array
          (
               [4] => 5
          )
)
```

## 3. combine()
The combine method is a pretty useful once since it will combine the values of the collection as keys, with the value of another array or collection

```ruby
public function index()
{
    $collection = collect(['name', 'language']);

    return $collection->combine((['Laravel', 'PHP']));
}


Output
(
     [name] => Laravel
     [language] => PHP
)
```

## 4. contains()
The contains() method will find out whether a collection contains a given item that you pass in as a closure


```ruby
public function index()
{
    $collection = collect([1, 2, 3, 4, 5]);

    dd($collection->contains(5));
}


Output
True
```

The dd() method shows ```true``` and if you return the ```$collection->contains(5));``` you will receive a ```1```

## 5. count()
The count() method will count the values of an array


```ruby
public function index()
{
    $collection = collect([1, 2, 3, 4, 5]);

    return $collection->count();
}


Output
5
```

## 6. diff()
The diff() method compares two collections/arrays. It will then return the values in the original collection that are not present in the given collection


```ruby
public function index()
{
    $collection = collect([1, 2, 3, 4, 5]);

    return $collection->diff([2, 5, 6, 8]);
}


Output
(
     [0] => 1
     [2] => 3
     [3] => 4
)
```
The original collection is ```$collection``` so therefore value 1, 3 and 4 are printed out in the output


## 7. dd()
We have all used the dd() method inside the browser once in our lifes. The dd() method on a collection does exactly the same thing


```ruby
public function index()
{
    $collection = collect([1, 2, 3, 4, 5]);

    return $collection->dd();
}


array:5 [
  0 => 1
  1 => 2
  2 => 3
  3 => 4
  4 => 5
]

```
The output will be in the same exact way as you usually see inside the browser

## 8. flip()
I personally find() flip a pretty interesting and funny method because it will flip all the item’s keys in a collection with their given values


```ruby
public function index()
{
    $collection = collect([
        'name' => 'Dary',
        'job' => 'Web Developer',
        'country' => 'the Netherlands'
    ]);

    return $collection->flip();
}


Output
(
     [Dary] => name
     [Web Developer] => job
     [the Netherlands] => country
)
```
The output tells us that ``` [Dary], [Web Developer] and [the Netherlands]``` are not our values anymore, but the keys, and ```name, job and country``` are not our keys anymore but the values


## 9. forget()
The forget() method removes an item from the collection by the given key


```ruby
public function index()
{
    $collection = collect([
        'name' => 'Dary',
        'job' => 'Web Developer',
        'country' => 'the Netherlands'
    ]);

    return $collection->forget('name');
}


Output
(
     [job] => Web Developer
     [country] => the Netherlands
)
```

## 10. each()
Here comes the section with a bit more complex methods. The each() method is nothing more than a foreach loop that is wrapper inside a higher order function. A higher order function is a function that takes another function as a parameter, returns a function, or does both. The each() method is used for iterating over the items in the collection and applying a callback function to each item. However, it doesn't modify the items or create a new collection; it's mainly used for side effects (like printing, logging, etc.).

Basic example below:

```ruby
$collection = collect([1, 2, 3, 4, 5]);

$collection->each(function ($item) {
    echo $item . ' ';
});

// Output: 1 2 3 4 5
```

Database example below:

```ruby
public function index()
{
    $users = User::all();

    $users->each(function ($value, $key) {
        $user = $value['name'];

        echo $user . ' | ';
    });
}

Output
Prof. Marc Mueller DVM | Caesar Hammes | Devan Mertz | Mrs. Claudie O'Reilly | Mrs. Kenya McLaughlin Sr. | Melba Sauer III | Arlo Ullrich |
```
What it will do is iterating over all user that’s we got inside our ```$users``` collection, and printing out the names of all users

Once again, let’s compare this to PHP code:
```ruby
public function index()
{
    $users = User::all();

    $allUsers = [];
    foreach($users as $user => $key) {
        $allUsers = $user['name'];
    }

    echo $allUsers;
}
```

The biggest difference is the fact that we are hiding the implementation of the foreach loop. Why would you define an empty array above your loop, set it equal to a value inside the loop, and then print it returning it outside of the loop?

## 11. map()
The map() method iterates through the ```$users``` collection and passes each value to the given callback. The map method is used for transforming each item in the collection and constructing a new collection with the transformed values. It returns a new collection containing the results of applying a callback function to each item.

Basic example below:

```ruby
$collection = collect([1, 2, 3, 4, 5]);

$doubled = $collection->map(function ($item) {
    return $item * 2;
});

// $doubled is a new collection: [2, 4, 6, 8, 10]
```

## 12. filter()
The filter() method is used to filter out elements of an array that you don’t want. You got to tell the filter which elements you want to keep by passing a callback that returns true if you want to keep the element, and false if you want to remove it

```ruby
public function index()
{
    $users = User::all();

    $filtered = $filtered = $users->filter(function ($value, $key) {
        return $value['id'] > 48;
    });

    return $filtered->all();
}

Output
The last two users inside your ```$users``` collection
```

## 12. pluck()
A list of Collections methods can’t miss the pluck() method. The pluck() method retrieves all values for a given key

```ruby
public function index()
{
    $users = User::all();

    return $users->pluck('name');
}

Output
(
     [0] => Prof. Marc Mueller DVM
     [1] => Caesar Hammes
     [2] => Devan Mertz
     [3] => Mrs. Claudie O'Reilly
     [4] => Mrs. Kenya McLaughlin Sr.
     [5] => Melba Sauer III
     [6] => Arlo Ullrich
     ...........
}
```

## 12. isEmpty() & isNotEmpty()
You can use the isEmpty() and isNotEmpty() methods to make sure if your collection is empty or not

```ruby
public function index()
{
    $users = User::all();

    dd($users->isEmpty());
}


Output
False
```

```Ruby
public function index()
{
    $users = User::all();

    dd($users->isNotEmpty());
}
	

Output
True

```

Our ```$users``` collection is not empty since we are pulling in all users. Therefore, the output of the ```isEmpty()``` method is false and the output of ```isNotEmpty()``` is true!

## Extra: Chaining multiple methods
Keep in mind that you can chain multiple methods together! See following example:

```ruby
public function index()
{
    $users = User::all();

    return $users
            ->pluck('name')
            ->except('id', 49)
            ->forget('Bethany Parker')
            ->skip(45);
}

Output
        (
            [45] => Kaleigh Gorczany
            [46] => Clifton Lowe
            [47] => Nick Auer
            [48] => Prof. Korey Strosin DDS
        )
```

Alright, let’s go over the methods we chained together <br>
•	pluck() makes sure that we’re only grabbing the ```$users``` name <br>
•	except() prints out all ```$users``` except the user with id 49 <br>
•	forget() removes the user ```Bethany Parker``` from the list <br>
•	skip() skips the first 45 rows<br>


### Here are some extra key features and aspects of collections in Laravel:

1. **Fluent Syntax:** Collections allow you to chain methods together in a fluent syntax, making your code more readable and expressive. This is achieved through method chaining.

   ```php
   $collection = collect([1, 2, 3, 4, 5]);

   $result = $collection
       ->filter(function ($value) {
           return $value > 2;
       })
       ->map(function ($value) {
           return $value * 2;
       })
       ->all();

   // $result is [6, 8, 10]
   ```

2. **Powerful Methods:** Laravel collections provide a wide range of methods for common operations like filtering, mapping, transforming, reducing, and more. These methods make it easy to manipulate array data in a concise and expressive way.

   ```php
   $collection = collect([1, 2, 3, 4, 5]);

   $sum = $collection->sum(); // 15
   ```

4. **Lazy Collections:** Laravel collections can be lazy, meaning the evaluation of a collection is deferred until the result is actually needed. This can lead to improved performance in certain scenarios.

   ```php
   $collection = collect([1, 2, 3, 4, 5]);

   $filtered = $collection->filter(fn($item) => $item > 2);

   // At this point, the filtering has not happened yet

   $result = $filtered->all();

   // Now the filtering is executed, and $result is [3, 4, 5]
   ```

Collections are an integral part of Laravel and are frequently used when dealing with sets of data. They provide a clean and expressive way to handle array operations in your application.

