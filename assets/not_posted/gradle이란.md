# Gradle

- 최근 자바 진영에서 가장 많이 사용하고 있는 빌드 도구.
- 특히 안드로이드 빌드 도구로 구글이 밀면서 모바일 개발자에게 관심도가 더 높음.
- 빌드 스크립트를 `그루비`라는 언어로 작성
- `Ant` 빌드 도구의 유연성 + `Maven`의 편리함
- 저장소는 Maven Repository를 활용
- `plugin`을 통한 확장



##### 디렉토리 구조 (maven과 동일)

- src/main/java : 배포할 자바 소스 코드(production code) 디렉토리
- src/main/resources : 배포할 설정 파일 디렉토리
- src/test/java : 테스트 자바 소스 코드(test code) 디렉토리
- src/test/resources : 테스트시 필요한 설정 파일 디렉토리



> 참고 http://kwonnam.pe.kr/wiki/gradle