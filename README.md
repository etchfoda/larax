# larax
larax is laravel exceptions logs package help you to trace exception happened in real time and notify you and you can configure which data you want to log it or ignore it.

[![Latest Version on Packagist](https://img.shields.io/packagist/v/escode/larax.svg?style=flat-square)](https://packagist.org/packages/escode/larax)
[![Total Downloads](https://img.shields.io/packagist/dt/escode/larax.svg?style=flat-square)](https://packagist.org/packages/escode/larax)


## Installation

Add this package in composer.json.

```js

    {
        "require": {
            "escode/larax": "1.*"
        }
    }

```

or run in terminal:
`composer require escode/larax`
### Add Larax Service Provider
open "/config/app.php" and append Escode\Larax\LaraxServiceProvider::class to providers array

`Example`
```php
 'providers' => [
    Escode\Larax\LaraxServiceProvider::class,
  ```


### Publish config file
`php artisan vendor:publish -tag=larax-config`
### Migrate database tables
`php artisan migrate`

### Make Larax Handle Exception

going to "/app/Exceptions/Handler.php" and make it extends from Escode\Larax\ExceptionHandler

`Example`

```php
use Escode\Larax\ExceptionHandler;
class Handler extends ExceptionHandler
{
```


## configration
open /config/larax.php

```php
return [
    //set your middleware that's allow to access exceptions pages on website
    'middleware'=>'auth',
    //api middleware
    'ApiMiddleware'=>'AuthLaraxApi',

    //exceptions data you want to store it
    'exception_data'=>[
        'ip'=>true,
        'url'=>true,
        'headers'=>true,
        'body'=>true,
    ],
    
    //Note: if you enable detect user in exception larax will push StartSession::class and EncryptCookies::class in $middleware Kernel.php
    //and you should be comment StartSession::class  from $middlewareGroups => web
    
    'detect_user'=>false,
    //array of guards names will use it for detect user id
    'guards'=>['auth'],

    //ignore fields in request headers (this fields not saved in db)
    'ignore_headers'=>[
        'Authorization','cookie'
    ],

    //ignore fields in request (this fields not saved in db)
    'ignore_inputs'=>[
        'image','password','password_confirmation'
    ],


    //send email when exception happened
    //enable send emails
    'enable_email'=>true,

    //email list you want send notify them with exception data
    'emails'=>[
       //example
        //'es.code@yahoo.com',
    ],

]

```
## How to use
open "http://localhost:8000/larax/exceptions"
to see the exceptions did happened in your application.

[![Larax](https://i.ibb.co/2yG3Qgf/Screen-Shot-2021-01-04-at-11-37-47-PM.png)](https://i.ibb.co/2yG3Qgf/Screen-Shot-2021-01-04-at-11-37-47-PM.png)

[![Larax exception info](https://i.ibb.co/qmyr3Xr/Screen-Shot-2021-01-04-at-11-56-41-PM.png)](https://i.ibb.co/qmyr3Xr/Screen-Shot-2021-01-04-at-11-56-41-PM.png)
## Users
users in larax is used to allow users to read exceptions using Larax Api.

## Api
you can read your application exceptions data over Api
just do "GET" request to "http://localhost:8000/api/larax/exceptions" using user key as bearer token 



