### 명령어 모음

**npm ci**

- 기본적으론 npm install과 동일한 역할을 한다.
- node_modules를 삭제하고 다시 깐다.
- ci, test platform, deployment 상황에서 주로 쓰인다.
- package-lock.json 파일이 꼭 있어야 한다. package.json 파일과 일치하지 않으면, 에러 발생됨.
- npm install과는 다르게, 개별 패키지를 설치할 수는 없다.

The main differences between using npm install and npm ci are.

# 참고한 사이트

[https://docs.npmjs.com/cli/v9/commands/npm-ci](https://docs.npmjs.com/cli/v9/commands/npm-ci)
