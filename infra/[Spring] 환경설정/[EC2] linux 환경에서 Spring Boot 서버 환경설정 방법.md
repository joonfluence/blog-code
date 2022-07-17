```shell
sudo apt update
sudo apt install default-jre
sudo apt install default-jdk
```

환경변수 적용

```shell
vi /etc/bash.bashrc
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
source /etc/bash.bashrc
```

적용 확인

java -version