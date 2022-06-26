### Xcode 빌드 캐시 삭제

Windows : `Shift+Command+K`
Mac : `Option+Shift+Command+K`

### 완전 삭제

```jsx
~/Library/Developer/Xcode/DerivedData
```

문제 있는 프로젝트 삭제

### Pods Dependency Error

```
rm -rf node_modules && yarn install && cd ios && rm -rf Pods && pod install && cd ..
```

### Pods Install Error

```
sudo arch -x86_64 gem install ffi
arch -x86_64 pod install
arch -x86_64 pod install --repo-update
```

### Terminating a process on port 8081

Run the following command to find the id for the process that is listening on port 8081:

`sudo lsof -i :8081` 

Then run the following to terminate the process:

`kill -9 <PID>`