---
title: Add new columns as foreign key to existing table in Laravel
author: muhdavi
date: 2023-01-22 10:10:00 +0700
categories: [Blogging, Tutorial]
tags: [Laravel, PHP]
render_with_liquid: false
---

For example, we have a table called `customers` and we want to add a new column called `user_id` as foreign key to the `users` table. Here is how to do it.

1. Create a migration file
```bash
php artisan make:migration add_user_id_to_customers_table --table=customers
```
2. Add the following code to the migration file
```php
public function up()
{
    Schema::table('customers', function (Blueprint $table) {
        $table->unsignedBigInteger('user_id')->before('created_at');
        $table->foreign('user_id')->references('id')->on('users')->onDelete('SET NULL')
    });
}
```
```php
public function down()
{
    Schema::table('customers', function (Blueprint $table) {
        $table->dropForeign(['user_id']);
        $table->dropColumn('user_id');
    });
}
```
3. Run the migration
```bash
php artisan migrate
```
