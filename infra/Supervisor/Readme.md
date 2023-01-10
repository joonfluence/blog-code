### 설치

```shell
sudo apt-get install supervisor # Linux
yum install supervisor # Amazon Linux
```

### 환경설정

```
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /home/forge/app.com/artisan queue:work sqs --sleep=3 --tries=3 --max-time=3600
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
user=forge
numprocs=8
redirect_stderr=true
stdout_logfile=/home/forge/app.com/worker.log
stopwaitsecs=3600
```

### 설정 변경

```shell
nano /etc/supervisord.conf
```

```shell
[include]
files = supervisord.d/*.ini => files = supervisord.d/*.conf
```

```shell
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start laravel-worker:*
```

# 참고한 사이트

[https://stackoverflow.com/questions/54811044/why-i-get-this-error-laravel-worker-error-no-such-group](https://stackoverflow.com/questions/54811044/why-i-get-this-error-laravel-worker-error-no-such-group)
