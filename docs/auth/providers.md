# Authentication Providers

* [DatabaseUserProvider](#databaseuserprovider)
* [NoDatabaseUserProvider](#nodatabaseuserprovider)

## DatabaseUserProvider

The `DatabaseUserProvider` allows you to synchronize LDAP users to your applications database.

To use it, insert it in your `config/adldap_auth.php` in the `provider` option:

```php
'provider' => Adldap\Laravel\Auth\DatabaseUserProvider::class
```
    
Using this provider utilizes your configured Eloquent model in `config/auth.php`:

```php
'providers' => [
    'users' => [
        'driver' => 'adldap',
        'model' => App\User::class,
    ],
],
```

When you've authenticated successfully, use the method `Auth::user()` as you would
normally to retrieve the currently authenticated user:

```php
// Instance of \App\User.
$user = Auth::user();

echo $user->email;
```

## NoDatabaseUserProvider

The `NoDatabaseUserProvider` allows you to authenticate LDAP users without synchronizing them.

> **Note**: Due to Laravel's generated auth views that utilize Eloquent models attributes, you will
> have to re-write some of these views for compatibility if you utilize this provider.
>
> For example, in the generated `resources/views/layouts/app.blade.php`, you will
> need to rewrite `Auth::user()->name` to `Auth::user()->getCommonName();`

To use it, insert it in your `config/adldap_auth.php` in the `provider` option:

```php
'provider' => Adldap\Laravel\Auth\NoDatabaseUserProvider::class
```

Inside your `config/auth.php` file, you can remove the `model` key in your provider array since it won't be used:

```php
'providers' => [
    'users' => [
        'driver' => 'adldap',
    ],
],
```

When you've authenticated successfully, use the method `Auth::user()` as you would
normally to retrieve the currently authenticated user:

```php
// Instance of \Adldap\Models\User.
$user = Auth::user();

echo $user->getCommonName();

echo $user->getAccountName();
```
