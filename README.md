
## Install Process
```bash
cd livecx-api

docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v $(pwd):/var/www/html \
    -w /var/www/html \
    laravelsail/php80-composer:latest \
    composer install --ignore-platform-reqs
```

After composer dependencies has been installed, copy `.env.example` to `.env` and you may start Laravel Sail. Laravel
Sail provides a simple command-line interface for interacting with Laravel's default Docker configuration:

```bash
cp .env.example .env

./vendor/bin/sail up
```

Once the application's Docker containers have been started, you can access the application in your web browser
at: http://localhost

> You may configure a Bash Alias for sail command.

```bash
alias sail='bash vendor/bin/sail'
```

### Configurations

Generate app key, storage link

```bash
sail artisan key:generate

sail artisan storage:link
```

Migrate databases

```bash
# Migrate common databases
sail artisan migrate
```

Seed databases

```bash
sail artisan db:seed
```

### Tenants

To create a Tenant after db seeded, open terminal to run Artisan command

```bash
sail tinker
```

Then create a Tenant by running bellow command in Tinker terminal
> App\Models\LiveCX\Company::create(['user_id' => 1, 'status' => 'active', 'subdomain' => 'livecx', 'name' => 'LiveCx', 'email' => 'livecx@livecx.com']);

Now the Tenant API base url will be `http://localhost/tenant/livecx`

You may seed `users` table with fake users.
```bash
sail artisan db:seed --class=UserSeeder --tenant=livecx
```
#### Migrations

Every new *migration* file should place in the `database/migrations/tenancy` folder.

```base
# Migrate livecx databases
sail artisan migrate:tenants
```

#### Artisan commands
To run any artisan command related to tenant, use `--tenant=subdomain` suffix.
```base
# Artisan command
sail artisan route:list --tenant=livecx
```

### PhpMyAdmin

Access phpmyadmin at: http://localhost:8080

- Username: `DB_USERNAME`
- Password: `DB_PASSWORD`

#### Starting & Stopping Sail

```bash
# start every services
sail up -d

# stop everything
sail down
```

#### Executing Commands

```bash
# Running Artisan commands locally...
php artisan migrate

# Running Artisan commands within Laravel Sail...
sail artisan migrate
```

#### Executing Composer Commands

```bash
sail composer require laravel/sanctum
```

#### Executing Artisan Commands

```bash
sail artisan queue:work
```

#### Executing Node / NPM Commands

```bash
sail node --version

sail npm run prod
```

#### Container CLI

```bash
# Connect application container
sail shell

# Connect laravel tinker
sail tinker
```
### Search Examples
1. Get indexes
```bash
curl -H 'Authorization: Bearer apiKey' -X GET "http://127.0.0.1:7700/indexes"
```
2. Get **Post** documents
```bash
curl -H 'Authorization: Bearer apiKey' -X GET "http://127.0.0.1:7700/indexes/posts/documents"
```
#### Create, Delete, Update global indexes
```shell
php artisan search:deleteIndex global_users --tenant=cxbrainstorm
php artisan search:deleteIndex global_groups --tenant=cxbrainstorm
php artisan search:deleteIndex global_posts --tenant=cxbrainstorm
php artisan search:deleteIndex global_maps --tenant=cxbrainstorm

php artisan search:createIndex global_users --tenant=cxbrainstorm
php artisan search:createIndex global_groups --tenant=cxbrainstorm
php artisan search:createIndex global_posts --tenant=cxbrainstorm
php artisan search:createIndex global_maps --tenant=cxbrainstorm

php artisan search:import-user --tenant=cxbrainstorm
php artisan search:import-group --tenant=cxbrainstorm
php artisan search:import-post --tenant=cxbrainstorm
php artisan search:import-map --tenant=cxbrainstorm
```
