```groovy
dependencies {
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'

    testCompileOnly 'org.projectlombok:lombok:1.18.12' // 테스트 의존성 추가
    testAnnotationProcessor 'org.projectlombok:lombok:1.18.12' // 테스트 의존성 추가
}
```
