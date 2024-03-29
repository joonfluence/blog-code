# 서론

오늘은 Mac에서 필수적으로 쓰이는 Package manager인 brew를 활용해서 로컬 개발환경을 셋팅하는 방법에 관해서 알아보도록 하겠습니다. 오랜만에 블로그 포스팅인만큼 평소에 제가 자주 사용하는 패키지까지 모두 이야기 드리겠습니다!

<br />

![brew](./brew.png)

### Homebrew 설치 방법

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

터미널에 해당 스크립트를 입력하시면 됩니다.

### 설치

패키지 설치 방법은 간단합니다.

```shell
brew install [패키지명]
```

설치를 원하는 프로그램을 검색하시려면 [해당 사이트](https://formulae.brew.sh/)에서 얼마든지 검색하실 수 있습니다. 개발 목적으로 nodejs, php, java(openjdk11) 등 프로그래밍 언어를 쉽게 설치하실 수 있습니다.

### 삭제

삭제 방법도 간단합니다. remove를 통해 삭제할 수 있습니다.

```shell
brew remove [패키지명]
brew uninstall [패키지명]
```

### 설치 목록 확인

해당 명령어를 실행하면, 설치 목록이 쭉 나열됩니다.

```shell
brew list
```

### 설치 경로 확인

설치 정보를 확인하는 방법은 아래와 같습니다.

```shell
brew info [패키지명]
```

출력 결과는 아래와 같습니다. 예시로, 제 로컬 PC에 설치된 Node.js의 경로를 찾아보도록 하겠습니다.

```shell
brew info node
# 출력 결과
/opt/homebrew/Cellar/node/18.2.0
```

### 아이콘 옵션

```shell
brew install --cask [패키지명]
```

install 할 때, cask 옵션을 주면 쉽게 데스크톱 아이콘을 생성할 수 있습니다.
개발 외 일반적인 용도로 사용하는 소프트웨어는 대부분 --cask 옵션을 통해 설치하실 수 있으며, 아이콘 옵션이 제공되는 패키지가 따로 존재합니다. 대표적인 예시가 notion, figma, xcodes 등등 입니다.

### 자주 사용하는 앱

- 화면 정리 앱 : rectangle

자세한 사용 방법은 다음과 같습니다. [https://hokari.tistory.com/78](https://hokari.tistory.com/78)

<br />

- 달력 앱 : itsycal
- 터미널 단어 자동완성 기능 제공 : zsh-autosuggestions
- 터미널 문법 확인 기능 제공 : zsh-syntax-highlighting

# 참고한 사이트

[https://www.robinwieruch.de/mac-setup-web-development/](https://www.robinwieruch.de/mac-setup-web-development/)
[https://formulae.brew.sh/formula/zsh-autosuggestions](https://formulae.brew.sh/formula/zsh-autosuggestions)
[https://formulae.brew.sh/formula/zsh-syntax-highlighting#default](https://formulae.brew.sh/formula/zsh-syntax-highlighting#default)
