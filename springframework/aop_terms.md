* Advice
    * Aspect가 해야할 작업
    * Acpect가 **무엇**을 **언제**할지 정의
    * 언제 : 메소드 호출 이전/이후/둘다, 메소드 예외
    * 종류
        * before : 메소드 호출 이전
        * after : 결과에 상관없이 메소드 호출 이후
        * after-returning : 메소드 호출이 성공적으로 완료된 후
        * after-throwing : 메소드가 예외를 던진 후
        * around : 호출 전과 후를 감싸서
* Join point
    * Advice를 적용할 수 있는 곳
    * 메소드 호출 지점, 예외 발생, 필드 값 수정 등
* Pointcut
    * Aspect가 Advice할 Join point 영역을 좁히는 일
    * Aspect가 Advice를 **어디서** 할지 정의
    * Advice가 Weaving 되어야 하는 하나 이상의 Join point를 정의
        * 클래스나 메소드 명을 직접 사용
        * 정규 표현식 정의
        * 실행 중 얻을 수 있는 정보 활용
* Aspect
    * Advice와 Pointcut을 합친 것
    * Aspect가 무엇을 언제 어디서 할지
* Introduction
    * 기존 클래스에 코드 변경 없이도 새 메소드나 멤버 변수를 추가하는 기능
    * 예) ```Auditable#setLastModifiedDate```
* Weaving
    * Target object에 Aspect를 적용해서 새로운 Proxy object를 생성하는 절차
    * Aspect는 Target object의 Join point로 Weaving 된다.
    * Weaving timing
        * Compile time
            * 별도의 컴파일러 필요
            * ex) AspectJ의 위빙 컴파일러
        * Classload time
            * JVM에 로드될 때 위빙
            * Target object의 바이트 코드를 enhance(바이트 코드에 직접 메소드나 멤버 변수 등을 추가)하는 특별한 ClassLoader가 필요
            * ex) AspectJ 5의 Load Time Weaving(LTW. 로드 시간 위빙)
        * Runtime
            * Target object에 호출을 위임하는 구조의 Proxy object를 AOP 컨테이너가 동적으로 만들어 낸다.
            * ex) Spring AOP Aspect
