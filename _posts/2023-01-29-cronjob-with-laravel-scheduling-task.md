---
title: Cronjob with Laravel Scheduling Task
author: muhdavi
date: 2023-01-22 10:10:00 +0700
categories: [Blogging, Tutorial]
tags: [Laravel, PHP]
render_with_liquid: false
---

## Create a new command Scheduling Task
```bash
php artisan make:command UpdatePeriod --command=update:period
```
Then make change in the file `app/Console/Commands/UpdatePeriod.php`:

```php
<?php

namespace App\Console\Commands;

use Illuminate\Support\Facades\Log;
use Illuminate\Console\Command;
use App\Models\Period;

class UpdatePeriod extends Command
{
    protected $signature = 'update:period';
    protected $description = 'Update periode status';

    public function handle()
    {
        $close_period = Period::where('status', true)->get();
        if ($close_period->isNotEmpty()) {
            $now = date('Y-m-d');
            $done = $close_period->pluck('date_done')->first();
            if ($now > $done) {
                $close_period = Period::where('status', true)->first();
                $close_period->status = false;
                $close_period->update();
            }
        } else {
            $open_period = Period::where('status', false)->get();
            if ($open_period->isNotEmpty()) {
                $now = date('Y-m-d');
                $start = $open_period->pluck('date_start')->first();
                if ($now >= $start) {
                    $open_period = Period::where('status', false)->first();
                    $open_period->status = true;
                    $open_period->update();
                }
            }
        }
        Log::info('Cronjob update period is working fine!');
        return Command::SUCCESS;
    }
}
```

## Add command Scheduling Task to Console Kernel
Open the file `app/Console/Kernel.php` and add the following code:
```php
protected function schedule(Schedule $schedule)
{
      $schedule->command('update:period')->daily()->timezone('Asia/Jakarta');
}
```
We can define the frequency of the command Scheduling Task by using the following methods:

| Method                                   | Description                                              |
|:-----------------------------------------|:---------------------------------------------------------|
| `->cron('* * * * *');`                   | Run the task on a custom cron schedule                   |
| `->everyMinute();`                       | Run the task every minute                                |
| `->everyTwoMinutes();`                   | Run the task every two minutes                           |
| `->everyThreeMinutes();`                 | Run the task every three minutes                         |
| `->everyFourMinutes();`                  | 	Run the task every four minutes                         |
| `->everyFiveMinutes();`                  | 	Run the task every five minutes                         |
| `->everyTenMinutes();`                   | 	Run the task every ten minutes                          |
| `->everyFifteenMinutes();`               | 	Run the task every fifteen minutes                      |
| `->everyThirtyMinutes();`                | 	Run the task every thirty minutes                       |
| `->hourly();`                            | 	Run the task every hour                                 |
| `->hourlyAt(17);`                        | 	Run the task every hour at 17 minutes past the hour     |
| `->everyOddHour();`                      | 	Run the task every odd hour                             |
| `->everyTwoHours();`                     | 	Run the task every two hours                            |
| `->everyThreeHours();`                   | 	Run the task every three hours                          |
| `->everyFourHours();`                    | 	Run the task every four hours                           |
| `->everySixHours();`                     | 	Run the task every six hours                            |
| `->daily();`                             | 	Run the task every day at midnight                      |
| `->dailyAt('13:00');`                    | 	Run the task every day at 13:00                         |
| `->twiceDaily(1, 13);`                   | 	Run the task daily at 1:00 & 13:00                      |
| `->twiceDailyAt(1, 13, 15);`             | 	Run the task daily at 1:15 & 13:15                      |
| `->weekly();`                            | 	Run the task every Sunday at 00:00                      |
| `->weeklyOn(1, '8:00');`                 | 	Run the task every week on Monday at 8:00               |
| `->monthly();`                           | 	Run the task on the first day of every month at 00:00   |
| `->monthlyOn(4, '15:00');`               | 	Run the task every month on the 4th at 15:00            |
| `->twiceMonthly(1, 16, '13:00');`        | 	Run the task monthly on the 1st and 16th at 13:00       |
| `->lastDayOfMonth('15:00');`             | 	Run the task on the last day of the month at 15:00      |
| `->quarterly();`                         | 	Run the task on the first day of every quarter at 00:00 |
| `->quarterlyOn(4, '14:00');`             | 	Run the task every quarter on the 4th at 14:00          |
| `->yearly();`                            | 	Run the task on the first day of every year at 00:00    |
| `->yearlyOn(6, 1, '17:00');`             | 	Run the task every year on June 1st at 17:00            |
| `->timezone('America/New_York');`        | 	Set the timezone for the task                           |
| `->weekdays();`                          | 	Limit the task to weekdays                              |
| `->weekends();`                          | 	Limit the task to weekends                              |
| `->sundays();`                           | 	Limit the task to Sunday                                |
| `->mondays();`                           | 	Limit the task to Monday                                |
| `->tuesdays();`                          | 	Limit the task to Tuesday                               |
| `->wednesdays();`                        | 	Limit the task to Wednesday                             |
| `->thursdays();`                         | 	Limit the task to Thursday                              |
| `->fridays();`                           | 	Limit the task to Friday                                |
| `->saturdays();`                         | 	Limit the task to Saturday                              |
| `->days(array!mixed);`                   | 	Limit the task to specific days                         |
| `->between($startTime, $endTime);`       | 	Limit the task to run between start and end times       |
| `->unlessBetween($startTime, $endTime);` | 	Limit the task to not run between start and end times   |
| `->when(Closure);`                       | 	Limit the task based on a truth test                    |
| `->environments($env);`                  | 	Limit the task to specific environments                 |

## Test run the command Scheduling Task
```bash
php artisan schedule:run
```
## Add command Scheduling Task to Cronjob
```bash
crontab -e
```
Then add the following code to the end of the file:
```bash
* * * * * cd /path-to-your-project && php artisan schedule:run >> /dev/null 2>&1
```
