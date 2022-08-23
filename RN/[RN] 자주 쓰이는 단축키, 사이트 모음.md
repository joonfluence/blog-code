# 본론
### 환경설정 방법

[Setting up the development environment · React Native - 링크](https://reactnative.dev/docs/environment-setup)

### Xcode 빌드 캐시 삭제

Windows : `Shift+Command+K`
Mac : `Option+Shift+Command+K`

### 완전 삭제

```shell
~/Library/Developer/Xcode/DerivedData
```

문제 있는 프로젝트 삭제

### Pods Dependency Error

```shell
rm -rf node_modules && yarn install && cd ios && rm -rf Pods && pod install && cd ..
```

### React Dependency Error

```shell
npm install --legacy-peer-deps
```

### Pods Install Error

```shell
sudo arch -x86_64 gem install ffi
arch -x86_64 pod install
arch -x86_64 pod install --repo-update
```

### Terminating a process on port 8081

Run the following command to find the id for the process that is listening on port 8081:

```shell
sudo lsof -i :8081
``` 

Then run the following to terminate the process. 

```shell
kill -9 <PID>
```

# 참고한 사이트

[https://www.korecmblog.com/ERESOLVE-unable-to-resolve-dependency-tree/](https://www.korecmblog.com/ERESOLVE-unable-to-resolve-dependency-tree/)