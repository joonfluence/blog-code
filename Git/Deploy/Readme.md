### git-hub page로 배포하기

```shell
git init
git remote -v
npm install gh-pages

npm package
"scripts" : {
	"deploy": "gh-pages -d build",
	"predeploy": "npm run build"
}{
	homepage : http://joonfluence.github.io/react-express
}

git remote add origin https://github.com/Joonfluence/react-express
npm install gh-page

github-page 이름을 추가할 것
```

### github 올려진 파일/remote 삭제하기

```shell
git rm --cached [파일명] : remote 특정 파일 삭제 
git rm --cached -r .project : remote 폴더 하위의 모든 파일 삭제 
git remote rm origin
```

### 기타

git log 나가기

```shell
Type q to exit this screen. Type h to get help.
```