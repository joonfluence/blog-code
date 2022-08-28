# 본론

### 환경설정 방법

[Setting up the development environment · React Native - 링크](https://reactnative.dev/docs/environment-setup)

### Xcode 빌드 캐시 삭제

Windows : `Shift+Command+K`
Mac : `Option+Shift+Command+K`

### 완전 삭제

````shell
~/Library/Developer/Xcode/DerivedData
``

문제 있는 프로젝트 삭제

### Pods Dependency Error

```shell
rm -rf node_modules && yarn install && cd ios && rm -rf Pods && pod install && cd ..
````

### React Dependency Error

```shell
npm install --force
npm install --legacy-peer-deps
```

--force를 하면 package-lock.json에 몇가지의 다른 의존 버전들을 추가한다.
--legacy를 하면 peerDependency가 맞지 않아도 일단 설치한다.

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

### xcodeproj vs xcworkspace

구글링을 통해 확인한 결론은 외부라이브러리 사용 없이 프로젝트를 생성하고 사용할 경우에는 xcodeproj로 실행해도 되지만, 외부라이브러리를 사용했을 경우에는 xcworkspace를로 프로젝트를 열어야 합니다. xcodeproj는 프로젝트는 프로젝트 설정파일들이 들어있는 디렉토리이고 xcworkspace는 workspace와 프로젝트들에 대한 설명하는 파일이 들어 있는 디렉토리 입니다.

# 참고한 사이트

[https://www.korecmblog.com/ERESOLVE-unable-to-resolve-dependency-tree/](https://www.korecmblog.com/ERESOLVE-unable-to-resolve-dependency-tree/)
[https://velog.io/@yonyas/Fix-the-upstream-dependency-conflict-installing-NPM-packages-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%EA%B8%B0](https://velog.io/@yonyas/Fix-the-upstream-dependency-conflict-installing-NPM-packages-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%EA%B8%B0)
[https://jeongupark-study-house.tistory.com/160](https://jeongupark-study-house.tistory.com/160)
