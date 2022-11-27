# 서론

### 명령어 모음

**npm ci**

- 기본적으론 npm install과 동일한 역할을 한다.
- node_modules를 삭제하고 다시 깐다.
- ci, test platform, deployment 상황에서 주로 쓰인다.
- package-lock.json 파일이 꼭 있어야 한다. package.json 파일과 일치하지 않으면, 에러 발생됨.
- npm install과는 다르게, 개별 패키지를 설치할 수는 없다.

The main differences between using npm install and npm ci are.

**npm list**

- 해당 디렉토리에 설치된 모듈들을 확인할 수 있다.
- -g 옵션을 추가해주면, 시스템 전체에서 활용 가능한 라이브러리들을 확인할 수 있다.

**npm root**

- 해당 디렉토리에 있는 node_modules들을 확인할 수 있다.
- -g로 전역에 설치된 라이브러리들의 위치를 확인할 수 있다.

# 참고한 사이트

[https://docs.npmjs.com/cli/v9/commands/npm-ci](https://docs.npmjs.com/cli/v9/commands/npm-ci)
