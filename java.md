# Java

[Effective Java 2E study](https://docs.com/sunnykwak/3906/effective-java-2e-study)  
[자바 프로그래머가 자주 실수하는 10가지](https://blog.weirdx.io/post/18553)  
[for-loop 를 Stram.forEach()로 바꾸지 말아야 할 3가지 이유](http://homoefficio.github.io/2016/06/26/for-loop-를-Stream-forEach-로-바꾸지-말아야-할-3가지-이유/)  
[ㄴ 토비 의견](https://www.facebook.com/tobyilee/posts/10207675579542090)  
[Java HashMap은 어떻게 동작하는가?](http://d2.naver.com/helloworld/831311)  
[모던 자바의 역습](http://www.moreagile.net/2015/12/modernjava5.html)  

## Effective Java

### 2장 객체의 생성과 삭제

#### Item 1 생성자 대신 정적 팩토리 메소드를 사용
#### Item 2 생성자 인자가 많을 때는 Builder 패턴 적용
#### Item 3 private 생성자나 enum 자료형은 싱글턴 패턴
#### Item 4 객체 생성을 막을 때는 private 생성자
#### Item 5 불필요한 객체는 만들지 말라
#### Item 6 유효기간이 지난 객체 참조는 폐기
#### Item 7 종료자 사용을 피하라

### 3장 모든 객체의 공통 메소드

#### Item 8 equals를 재정의할 때는 일반 규약을 따르라
#### Item 9 equals를 재정의할 때는 반드시 hashCode도 재정의
#### Item 10 toString은 항상 재정의
#### Item 11 clone을 재정의할 때는 신중히
#### Item 12 Comparable 구현을 고려

### 4장 클래스와 인터페이스

#### Item 13 클래스와 멤버의 접근 권한을 최소화

* 정보 은닉, 캡슐화
  * 의존성을 낮춘다.(decouple)
  * 각자 개발, 시험, 최적화, 이해, 변경
  * 유지보수성, 성능 튜닝(프로파일링), 재사용 가능성, 위험성 낮춤
* package-private class
  * 사용하는 곳이 하나라면 private 중첩 클래스 고려
* private, package-private member
  * Serializable을 구현하는 클래스의 멤버라면 공개 API 속으로 새어나갈 수도 있다.
* public static final member
  * 핵심적인 상수, 기본 자료형 값 또는 변경 불가능 객체 참조
  * 배열 필드처럼 변경 가능한 필드를 사용해서는 안된다.
    * ```public static final List<Thing> VALUES = Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));```
    * 위와 같이 변경 불가능한 리스트로 대체
    * 또는 해당 배열을 복사하여 반환하는 public 메소드를 추가

#### Item 14 public 클래스 안에는 public 필드를 두지 말고 접근자 메소드를 사용
#### Item 15 변경 가능성을 최소화
#### Item 16 계승하는 대신 구성
#### Item 17 계승을 위한 설계와 문서를 갖추거나, 그럴 수 없다면 금지
#### Item 18 추상 클래스 대신 인터페이스를 사용
#### Item 19 인터페이스는 자료형을 정의할 때만 사용
#### Item 20 태그 달린 클래스 대신 클래스 계층을 활용
#### Item 21 전략을 표현하고 싶을 때는 함수 객체를 사용
#### Item 22 멤버 클래스는 가능하면 static으로 선언
