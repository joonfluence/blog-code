# 서론

### 정의

테라폼(Terraform)은 하시코프(Hashicorp)에서 오픈소스로 개발중인 클라우드 인프라스트럭처 자동화를 지향하는 코드로서의 인프라스트럭처(Infrastructure as Code, IaC) 도구입니다.

### 설치방법

- Linux

```shell
#!/bin/bash

VERSION=1.3.2
ARCH=`uname -m`

case "$ARCH" in
    arm*) EXT="arm" ;;
    *)    EXT="amd64" ;;
esac

case "$OSTYPE" in
    darwin*) FILENAME="darwin_$EXT" ;;
    *)       FILENAME="linux_$EXT" ;;
esac

TERRAFORM_FILENAME=terraform_${VERSION}_${FILENAME}.zip
TERRAFORM_URL=https://releases.hashicorp.com/terraform/${VERSION}/${TERRAFORM_FILENAME}
mkdir tmp
cd tmp
wget $TERRAFORM_URL
unzip ./${TERRAFORM_FILENAME}
sudo cp terraform /usr/local/bin
cd ..
rm -rf ./tmp
```

- Mac

```shell
brew install terraform
```

### AWS와 연동하기

- valut_password_file
  - 암호화할 때 사용할 값을 저장하고 있는 파일을 지정함
  - 해당 위치에 암호 키로 사용할 값을 저장함
- private_key_file
  - 해당 서버의 접속에 사용할 ssh private key를 지정함
  - AWS라면 instance 생성 시 설정한 **pem 파일**을 저장하고 해당 위치를 저장함
- remote_user
  - 해당 서버에서 사용하는 user 정보를 설정함

```conf
# ansible.cfg

[defaults]
inventory = hosts
forks = 20
remote_port = 22 # 변경 필요

vault_password_file = ~/.ansible/the-red-vault-password-file # 변경 필요

roles_path = roles:roles/external

remote_user = ubuntu # 변경 필요
private_key_file = ~/.ssh/the_red.pem # 변경 필요

ansible_managed = Ansible managed file, do not edit directly

#callback_plugins   = ./callback_plugins
stdout_callback = unixy

nocows = 1

retry_files_enabled = False
host_key_checking = False

[ssh_connection]
pipelining = True
ssh_args = -o ForwardAgent=yes -o ControlMaster=auto -o ControlPersist=120s
```

### 환경설정 방법

- 기본 셋팅

```shell
terraform init
```

- 셋팅 : The terraform plan command creates an execution plan, which lets you preview the changes that Terraform plans to make to your infrastructure. By default, when Terraform creates a plan it.
  - 클라우드 환경에서 사용한다면, 기본적으로 각각에 맞는 환경설정을 별도로 해줘야 한다.

```shell
terraform plan -out "output"
```

- 적용

```shell
terraform apply "output"
```

# 참고한 사이트

[https://www.terraform.io/](https://www.terraform.io/)
[https://www.44bits.io/ko/keyword/terraform](https://www.44bits.io/ko/keyword/terraform)
[https://techblog.woowahan.com/2646/](https://techblog.woowahan.com/2646/)
