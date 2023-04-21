# 서론

오늘은 NVM을 활용하여, Node Version을 관리하는 방법에 관해서 알아보도록 하겠습니다.

### 설치 방법

설치 방법에는 2가지가 있습니다.

- curl로 설치

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
```

- wget으로 설치

```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
```

그러면 아래와 같은 메세지가 출력됩니다.

```bash
=> Appending nvm source string to /home/lesstif/.bash_profile
=> Appending bash_completion source string to /home/lesstif/.bash_profile
=> You currently have modules installed globally with `npm`. These will no
=> longer be linked to the active version of Node when you install a new node
=> with `nvm`; and they may (depending on how you construct your `$PATH`)
=> override the binaries of modules installed with `nvm`:

/usr/local/node-v6.17.0-linux-x64/lib
├── gulp@4.0.0
=> If you wish to uninstall them at a later point (or re-install them under your
=> `nvm` Nodes), you can remove them from the system Node as follows:

     $ nvm use system
     $ npm uninstall -g a_module

=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

이제 ~/.bash_profile 파일 혹은 ~/.zshrc 파일을 살펴봅니다. 정상적으로 아래와 같은 구문이 추가되었음을 알 수 있습니다.

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

그런 뒤, `source /.bash_profile` 혹은 `source /.zshrc` 를 입력해줍니다.

### 사용 방법

정상적으로 설치가 되었으면, NVM 활용 방법에 관해서 알아보도록 하겠습니다.

- Node Version 목록 확인하기

```bash
nvm ls
```

- 새로운 버젼 설치

```bash
nvm install [version] # nvm install 14.17.3
```

마지막 버젼을 설치하려면 세부 버젼 정보를 제외하고 `nvm install 8`과 같이 입력해줍니다.

- 사용할 버젼 지정

```bash
nvm use 14.17.3
```

실제 입력해보면, 아래와 같이 node 버젼이 변경되었음을 확인할 수 있습니다.

### 참고한 사이트

[https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)
