### 방법

- Run php artisan queue:table which will create a migration for jobs table autimatically
- Run php artisan migrate which will create the table by running migration
- Change QUEUE_CONNECTION=database and as per default, it will automatically take jobs table to manage the queues.
- Run php artisan config:clear to clear application configuration cache
- That should be good to go. Check documentation for more help.

### 참고한 사이트

[https://laravel.com/docs/5.4/queues](https://laravel.com/docs/5.4/queues)
[https://laravel.com/docs/9.x/installation#why-laravel](https://laravel.com/docs/9.x/installation#why-laravel)
[https://laravel.com/docs/9.x/queues#supervisor-configuration](https://laravel.com/docs/9.x/queues#supervisor-configuration)
[https://stackoverflow.com/questions/54834813/laravel-5-7-jobs-queue-not-running-async](https://stackoverflow.com/questions/54834813/laravel-5-7-jobs-queue-not-running-async)
