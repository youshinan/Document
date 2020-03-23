chocoletey install

```bash
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object ystem.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```



php install

```bash
choco install -y php
choco install -y composer
```



Laravel install

```bash
composer global require laravel/installer
```



Create laravel project

```bash
laravel new blog
```

 

local server start

```bash
php artisan server
```

コントローラーの作成

```bash
php-7.1 artisan make:controller <コントローラー名>
```



モデルの作成

```bash
php-7.1 artisan make:model <モデル名>
```



マイグレーションファイル（テーブルの操作定義）の作成

```bash
php-7.1 artisan make:migration <マイグレーションファイル名> --create=<モデル名>
```



Doctrine DBAL install

```bash
composer require doctrine/dbal

eg.
php artisan make:migration update_hello_table　--table=hello

php artisan migrate
php artisan migrate:rollback --step=<数字>
php artisan migrate:reset
php artisan migrate:refresh
```



Seeder

```bash
php artisan make:seeder AdminUserSeeder

php artisan db:seed --class=UserTableSeeder

php artisan migrate:refresh --seed
```



Laravel6.0 auth 

```bash
composer require laravel/ui --dev
php artisan ui vue --auth
php artisan migrate
npm install
npm run dev

composer dump-autoload
```