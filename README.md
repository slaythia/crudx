# crudx

CRUDX is a PHP based library for mysql query builder. Its easy and simple. 


# How to install

To install this package go to terminal and run this command

```shell
composer require nahid/crudx
```

# Usage

To use this package you have to include it. Include it from composer autoload

```php
require_once 'vendor/autoload.php';
```

# How to connect 

You can connect by using this process;

```php
use Nahid\Crudx\Crudx;

$config = [
    'host' 		=> 'localhost',
    'user' 		=> 'root',
    'password' 	=> 'your_password',
    'database' 	=> 'database_name',
    'charset' 	=> 'utf8',
    'collation'	=> 'utf8_unicode_ci',
    'prefix' 	=> 'table_prefix',
];

$crud = new Crudx($config);
```


# Insert 

Its too much important to inserting data in database table when you developing an application. Crudx make easy to inserting data in table. Crudx provide different type of insertion mechanism. First you can insert data with Laravel Eloquent style. Suppose you want to save name, username, email in users table. So what can you do?

```php
$user=$crud->table('users');

$user->name='Nahid Bin Azhar';
$user->username='nahid';
$user->email='talk@nahid.co';

$user->save();
``` 

Its like a piece of cake.

The second procedure. Its traditional way. You may called it CodeIgniter Style. However see how can you inserting data with this way.

```php
$data=[
	'name'=>'Nahid Bin Azhar',
	'username'=>'nahid',
	'email'=>'talk@nahid.co'
];

$crud->table('users')->save($data);
```

What do you think? yes its Crudx, make easy development.

You may insert multiple records at once. Yes its true, just see what happend

```php
$data=[
	[
	'name'=>'Nahid Bin Azhar',
	'username'=>'nahid',
	'email'=>'talk@nahid.co'
	],
	[
	'name'=>'Naim',
	'username'=>'naim',
	'email'=>'naim@themebucket.net'
	]
];

$crud->table('users')->insertMany($data);
```

# Update

Making update rocord is so easy. 

```php
$data=[
	'name'=>'Nahid Bin Azhar',
	'username'=>'nahid',
	'email'=>'talk@nahid.co'
];

$crud->where('id', '=', 1)->save($data);
``` 

# Delete

Sometimes you need to delete record from table. Crudx make it easy. Its just a single command

```php
$crud->where('id', '=', 1)->delete();
```

# Fetching Record

Crudx provide you to differents type service to fetching record from table. Suppose you want to get all data of author role from table users

```php
$crud->table('users')->where('role', '=', 'author')->all()->result();

//generated SQL String: SELECT * FROM users WHERE role='author'
```

But if you want to add multiple condition with  `AND` operator then follow the process


```php
$crud->table('users')->where('role', '=', 'author')->where('age','>',17)->all()->result();

//generated SQL String: SELECT * FROM users WHERE role='author' AND age>17
```

you can use `orWhere()` for OR operator and you can also use `whereBetween()` `orBetween()

If you have to fetch specific table column then use `get()` method and pass an array to specify table column


```php
$crud->table('users')->where('role', '=', 'author')->get(['name', 'username'])->result();

//generated SQL String: SELECT name, username FROM users WHERE role='author'
```

# Join

Crudx provide you a simple joining method.

```php
$crud->table('posts')->join('users', 'posts.user_id','=', 'users.id')->get(['post', 'name'])->result();

//generated SQL String: SELECT post, name FROM posts INNER JOIN users on posts.user_id=users.id
```

# Get last inserted id

```php
$crud->table('users')->save(['name'=>'Nahid']);
echo $crud->getId();
```

# Get last generated query string

Sometimes you have to need which query string is generated by the last method. Crudx make it easy

```php
$crud->table('users')->where('role', '=', 'author')->where('age','>',17)->all()->result();
echo $crud->getQueryString();
```