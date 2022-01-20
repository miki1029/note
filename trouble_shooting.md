# java

* Mockito에서 `Invalid use of argument matchers!` 오류
  * spy object로 테스트를 하는 상황
  * `verify(mySpy).myMethod(eq(mySpy.method()))` 와 같은 형태로 사용할 경우 오류가 발생한다.
  * `mySpy.method()`를 변수화하여 사용하면 문제가 해결된다.
