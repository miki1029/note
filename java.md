# Java

[Effective Java 2E study](https://docs.com/sunnykwak/3906/effective-java-2e-study)  
[자바 프로그래머가 자주 실수하는 10가지](https://blog.weirdx.io/post/18553)  
[for-loop 를 Stram.forEach()로 바꾸지 말아야 할 3가지 이유](http://homoefficio.github.io/2016/06/26/for-loop-를-Stream-forEach-로-바꾸지-말아야-할-3가지-이유/)  
[ㄴ 토비 의견](https://www.facebook.com/tobyilee/posts/10207675579542090)  
[Java HashMap은 어떻게 동작하는가?](http://d2.naver.com/helloworld/831311)  
[모던 자바의 역습](http://www.moreagile.net/2015/12/modernjava5.html)  
[Top 10 Easy Performance Optimisations in Java](https://blog.jooq.org/2015/02/05/top-10-easy-performance-optimisations-in-java/)

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

* package-private 클래스나 private 중첩 클래스는 데이터 필드를 공개해도 된다.
  * 패키지 외부 코드의 변경은 없을 것이며 private의 경우 바깥 클래스 외부의 코드는 영향을 받지 않는다.

#### Item 15 변경 가능성을 최소화
#### Item 16 계승하는 대신 구성
#### Item 17 계승을 위한 설계와 문서를 갖추거나, 그럴 수 없다면 금지
#### Item 18 추상 클래스 대신 인터페이스를 사용
#### Item 19 인터페이스는 자료형을 정의할 때만 사용
#### Item 20 태그 달린 클래스 대신 클래스 계층을 활용
#### Item 21 전략을 표현하고 싶을 때는 함수 객체를 사용
#### Item 22 멤버 클래스는 가능하면 static으로 선언

## Java 8 in Action

* Java 8 주요 특징
  * Stream API
  * Method reference, Lambda
  * Interface default method

### Stream API

* 스트림 API
  * 스트림 : 한 번에 한 개씩 만들어지는 연속적인 데이터 항목들의 모임
  * 질의 언어(고수준 언어)로 원하는 동작을 표현하면 최적의 저수준 실행 방법을 선택하여 동작
  * 스레드를 사용하지 않으면서 병렬성을 얻을 수 있다.

```java
import static java.util.stream.Collectors.toList;
Map<Currency, List<Transaction>> transactionsByCurrencies =
    transactions.stream()
                .filter((Transaction) t) -> t.getPrice() > 1000)
                .collect(groupingBy(Transaction::getCurrency));

// 순차 처리 방식
List<Apple> heavyApples = inventory.stream().filter((Apple a) -> a.getWeight() > 150)
                                            .collect(toList());

// 병렬 처리 방식
List<Apple> heavyApples = inventory.parallelStream().filter((Apple a) -> a.getWeight() > 150)
                                                    .collect(toList());
```

### Method Reference

* 메소드 레퍼런스
  * 동작 파라미터화  
  * 함수형 프로그래밍  
  * pure, side-effect-free, stateless function : shared mutable data에 접근하지 않는 함수  

```java
// Before - 익명 클래스
File[] hiddenFiles = new File(".").listFiles(new FileFilter() {
    public boolean accept(File file) {
        return file.isHidden();
    }
});

// After - 메소드 레퍼런스
File[] hiddenFiles = new File(".").listFiles(File::isHidden);
```

* 메소드 레퍼런스를 만드는 방법
  * 정적 메소드 레퍼런스 : ```Integer::pareseInt```
  * 인스턴스 메소드 레퍼런스 : ```String::length```
  * 기존 객체 인스턴스의 메소드 레퍼런스 : ```expensiveTransaction::getValue```
  * 생성자 레퍼런스 : ```ClassName::new```

```java
List<String> str = Arrays.asList("a", "b", "A", "B");
str.sort((s1, s2) -> s1.compareToIgnoreCase(s2)); // Lambda
str.sort(String::compareToIgnoreCase); // Method reference

Supplier<Apple> c1 = () -> new Apple();
Supplier<Apple> c1 = Apple::new;
c1.get();

Function<Integer, Apple> c2 = weight -> new Apple(weight);
Function<Integer, Apple> c2 = Apple::new;
c2.apply(110);

BiFunction<String, Integer, Apple> c3 = (color, weight) -> new Apple(color, weight);
BiFunction<String, Integer, Apple> c3 = Apple::new;
c3.apply("green", 110);

// TriFunction<T, U, V, R>, ...
```

* Lambda

```java
// 메소드 명세 : Predicate 함수 인터페이스
public static List<Apple> filterApples(List<Apple> inventory, Predicate<Apple> p);

// 메소드 호출 : 기존 함수 사용 또는 새로운 람다 함수 정의
filterApples(inventory, Apple::isGreenApple);
filterApples(inventory, Apple::isHeavyApple);
filterApples(inventory, (Apple a) -> "green".equals(a.getColor()));
filterApples(inventory, (Apple a) -> a.getWeight() > 150);
filterApples(inventory, (Apple a) -> a.getWeight() < 80 || "brown".equals(a.getColor()));

// Comparator
inventory.sort((Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
// Runnable
Thread t = new Thread(() -> System.out.println(“Hello world”));
// GUI
Button.setOnAction((ActionEvent event) -> label.setText("Sent"));
```

```java
(String s) -> s.length()
(Apple a) -> a.getWeight() > 150
(int x, int y) -> {
    System.out.println("Result:");
    System.out.println(x+y);
}
() -> 42
Callable<Integer> c = () -> 42;
PrivilegedAction<Integer> c = () -> 42;
Predicate<String> p = s -> list.add(s); // 특별한 void 호환 규칙
Comparator<Apple> c = (a1, a2) -> a1.getWeight().compareTo(a2.getWeight()); // 형식 추론
Predicate<Apple> p = a -> "green".equals(a.getColor()); // 파라미터 1개이면 괄호 생략
```

* 함수형 인터페이스 : **하나의** 추상 메소드를 지정하는 인터페이스
  * ```@FunctionalInterface``` : 함수형 인터페이스를 강제하는 어노테이션
  * 함수 디스크립터(function descriptor) : 함수형 인터페이스의 추상 메소드 시그너처
  * default method는 추가로 가질 수 있다.
  * 대표적인 함수형 인터페이스 : 100p ~ 102p
  * 검사형 예외를 던지는 동작을 허용하지 않는다.

```java
// java.util.function
// 제네릭 함수형 인터페이스
public interface Predicate<T> {
    boolean test(T t);
}
public interface Consumer<T> {
    void accept(T t);
}
public interface Function<T, R> {
    R apply(T t);
}
// Supplier<T>, UnaryOperator<T>, BinaryOperator<T>, BiPredicate<L, R>, BiConsumer<T, U>, BiFunction<T, U, R>, ...

// 기본형 함수형 인터페이스(boxing 회피)
public interface IntPredicate {
    boolean test(int t);
}
// DoublePredicate, IntConsumer, LongBinaryOperator, IntFunction, ToIntFunction<T>, IntToDoubleFunction, ...
```

* 람다 캡처링(capturing lambda)
  * 인스턴스 변수, 정적 변수 : 자유롭게 캡처
  * 지역 변수 : final이거나 final처럼 사용해야 함(스택에 할당되는데 스레드로 실행되면 접근 불가능하여 복사본을 제공)

* 람다의 메소드 레퍼런스 표현

```java
(Apple a) -> a.getWeight() // Apple::getWeight
() -> Thread.currentThread().dumpStack() // Thread.currentThread::dumpStack
(str, i) -> str.substring(i) // String.substring
(String s) -> System.out.println(s) // System.out::println
```

* New Java 8 API
  * [java.util.function](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)
  * [Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html) : comparing, reversed, thenComparing
  * [Predicate](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html) : negate, and, or
  * [Function](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html) : andThen, compose

```java
// Comparator
Comparator<Apple> c = Comparator.comparing(Apple::getWeight);
inventory.sort(comparing(Apple::getWeight)
         .reversed()
         .thenComparing(Apple::getCountry));

// Predicate
Predicate<Apple> notRedApple = redApple.negate();
Predicate<Apple> redAndHeavyApple = redApple.and(a -> a.getWeight() > 150);
Predicate<Apple> redAndHeavyAppleOrGreen = redApple.and(a -> a.getWeight() > 150)
                                                   .or(a -> "green".equals(a.getColor()));

// Function
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.andThen(g); // g(f(x))
Function<Integer, Integer> h = f.compose(g); // f(g(x))
```

### Interface default method

```java
default void sort(Comparator<? super E> c) {
    Collections.sort(this, c);
}
```
