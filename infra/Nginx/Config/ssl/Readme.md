### 무료로 SSL 추가하는 방법

- LetsEncrypt에서 무료로 TLS 인증서를 발급 받을 수 있다. 몇 가지 TLS 인증서 종류 중에서 완전 자동화가 가능한 DV (Domain Validated, 도메인 확인) 인증서를 무료로 발급한다.
- certbot을 통해 자동 갱신한다.

1. SSH into the server
2. Install snapd

```shell
# ubuntu
sudo apt update
sudo apt install snapd

sudo snap install hello-world
> hello-world 6.4 from Canonical✓ installed
hello-world
> Hello World!
```

3. Ensure that your version of snapd is up to date

```shell
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
```

4. Remove certbot-auto and any Certbot OS packages

```shell
sudo apt-get remove certbot
```

5. Install Certbot

```shell
sudo snap install --classic certbot
```

6. Prepare the Certbot command

```shell
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

7. Choose how you'd like to run Certbot

```shell
sudo certbot --nginx
sudo certbot certonly --nginx # If you're feeling more conservative and would like to make the changes to your nginx configuration by hand, run this command.
sudo certbot renew --dry-run 
```

8. Test automatic renewal

```shell
sudo certbot renew --dry-run
```

The command to renew certbot is installed in one of the following locations:

- /etc/crontab/
- /etc/cron.*/*
- systemctl list-timers

9. Confirm that Certbot worked

# 참고한 사이트

[https://letsencrypt.org/getting-started/](https://letsencrypt.org/getting-started/)

