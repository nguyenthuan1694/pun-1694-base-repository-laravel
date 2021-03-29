## Documentation

To get started with **Base Repository**, use Composer to add the package to your project's dependencies:

```bash
    composer require thuan-1694/base-repository-laravel
```

## Configuration

### Laravel 6+

Laravel uses Package Auto-Discovery, so doesn't require you to manually add the ServiceProvider.

### Laravel < 6:

If you don't use auto-discovery, add the ServiceProvider to the providers array in config/app.php

```php
'providers' => [
    // Other service providers...

    Kenini\RepositoryServiceProvider::class,
],
```

### Basic Usage

Next, you are ready to use repository. If you want create repository with Model corresponding(example:BaseRepository), run commnand line 

```bash

php artisan make:repostitory BaseRepository -i
```
When run this commnand, Packeage automatic generate two file in forder Repository: BaseRepository and RepositoryInterface. 
BaseRepository extends AbstractRepository so you can use method in AbstractRepository

```php
<?php

namespace App\Repositories;

use App\Models\User;
use Kenini\Repository\AbstractRepository;
use App\Repositories\RepositoryInterface;

class BaseRepository extends AbstractRepository implements RepositoryInterface
{
    protected $model;

    /**
     * BaseRepository construct
     *
     * @param  mixed $model
     *
     * @return void
     */
    public function __construct(User $model)
    {
        parent::__construct($model);
    }
}
```
Register in AppServiceProvider
```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use App\Repositories\BaseRepository;
use App\Repositories\Users\RepositoryInterface;


class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        //
    }

    /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {
        $this->app->bind(RepositoryInterface::class, BaseRepository::class);
    }
}
```


#### Retrieving User Details

In controller, You want find user by id use repository

```php
<?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use App\Repositories\Users\RepositoryInterface;

class UserController extends Controller
{
    /**
     * @var RepositoryInterface
     */
    private $BaseRepository;

    public function __construct(RepositoryInterface $BaseRepository )
    {
        $this->BaseRepository = $BaseRepository;
    }

    public function show($id) 
    {

        $user = $this->BaseRepository->findById($id);

        return response()->json($user);
    }
}
```
All function in base repository
```
    public function find(array $conditions = []);
    public function findOne(array $conditions);
    public function findById(int $id);
    public function create(array $attributes);
    public function update(Model $model, array $attributes = []);
    public function save(Model $model);
    public function delete(Model $model);
    public function get($query);
    public function destroy(array $ids);
    public function findCount(array $conditions);
    public function toBase($query);
    public function updateMultiple(Builder $query, array $attributes = []);
    public function updateOrCreate(array $attributes, array $values);
    public function findAll();
    public function findByIds(array $ids);
    public function model();
    public function makeModel();
    public function resetModel();
```