# Local Laravel + phpMyAdmin Database Guide

This project is now for local Laravel and local MySQL/phpMyAdmin only.

Remote deployment/database files were removed. The database connection should use your local XAMPP/Laragon/WAMP MySQL.

## Copy And Paste: Start To Finish

Open PowerShell and run these commands one by one.

```powershell
cd C:\Users\Acer\Desktop\peterlaravel\sirjeremfinal
composer install
copy .env.example .env
php artisan key:generate
```

Open XAMPP Control Panel and start:

```txt
Apache
MySQL
```

Open phpMyAdmin:

```txt
http://localhost/phpmyadmin
```

Click SQL and run this:

```sql
CREATE DATABASE IF NOT EXISTS madajes_boarding_house
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
```

Back in PowerShell, run:

```powershell
php artisan config:clear
php artisan cache:clear
php artisan migrate:fresh
php artisan serve
```

Open the app:

```txt
http://127.0.0.1:8000
```

## .env For Local phpMyAdmin

Your `.env` must contain this database section:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=madajes_boarding_house
DB_USERNAME=root
DB_PASSWORD=
```

For XAMPP, `root` with blank password is the default.

If your MySQL has a password, use:

```env
DB_PASSWORD=your_mysql_password
```

## Create Tables With SQL Instead Of Migrations

If you do not want to use Laravel migrations, open phpMyAdmin, click SQL, and run the SQL in:

```txt
database/phpmyadmin-schema.sql
```

That file creates all tables and leaves records empty.

## Empty All Records

To remove all records, run this in phpMyAdmin SQL:

```sql
USE madajes_boarding_house;

SET FOREIGN_KEY_CHECKS = 0;

TRUNCATE TABLE schedule_items;
TRUNCATE TABLE payments;
TRUNCATE TABLE tenant_reports;
TRUNCATE TABLE accounts;
TRUNCATE TABLE tenants;
TRUNCATE TABLE rooms;
TRUNCATE TABLE property_profiles;

SET FOREIGN_KEY_CHECKS = 1;
```

This is also saved in:

```txt
database/empty-records.sql
```

## Create First Admin Account

If all records are empty, you cannot log in yet. Create the first admin in phpMyAdmin SQL:

```sql
USE madajes_boarding_house;

INSERT INTO accounts (id, role, username, password_hash, tenant_id, created_at, updated_at)
VALUES ('acct-admin', 'admin', 'admin', 'admin123', NULL, NOW(), NOW());
```

Login:

```txt
Username: admin
Password: admin123
```

## Common Fixes

If you see `No application encryption key has been specified`, run:

```powershell
php artisan key:generate
php artisan optimize:clear
```

If you see `Could not open input file: artisan`, you are in the wrong folder. Run:

```powershell
cd C:\Users\Acer\Desktop\peterlaravel\sirjeremfinal
```

If you see `vendor/autoload.php missing`, run:

```powershell
composer install
```

If Composer says `zip extension missing`, open:

```txt
C:\xampp2\php\php.ini
```

Enable this line by removing `;`:

```ini
extension=zip
```

Then close and reopen PowerShell.