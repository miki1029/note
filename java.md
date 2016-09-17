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
  * 특징 : 선언형, 조립, 병렬화
  * 기타 라이브러리 : Guava, Apache Commons Collections, lambdaj

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

// 스트림은 한 번만 소비할 수 있음
List<String> title = Arrays.asList("Java8", "In", "Action");
Stream<String> s = title.stream();
s.forEach(System.out::println);
s.forEach(System.out::println); // java.lang.IllegalStateException
```

* 스트림의 정의
  * **데이터 처리 연산**을 지원하도록 **소스**에서 추출된 **연속된 요소**
  * 파이프라이닝 : laziness, short-circuiting
  * 내부 반복

* 스트림과 컬렉션
  * 컬렉션 : 현재 자료구조가 포함하는 모든 값을 메모리에 저장하는 자료구조. 외부 반복
  * 스트림 : 요청할 때만 요소를 계산하는 고정된 자료구조. 내부 반복

* 스트림 연산
  * 중간 연산(intermediate operation)
    * 연결할 수 있는 스트림 연산(스트림을 반환)
    * lazy 연산 : 합쳐진 중간 연산을 최종 연산으로 한 번에 처리
    * 쇼트서킷, 루프 퓨전
  * 최종 연산(terminal operation)
    * 스트림을 닫는 연산
    * 스트림 이외의 결과를 반환

* 필터링
  * Predicate 필터링
    * filter(Predicate p) : boolean을 반환하는 함수를 인자로 받아서 필터링
  * 고유 요소 필터링
    * dictinct() : 고유 요소는 hashCode, equals로 결정됨
  * 스트림 축소
    * limit(n)
  * 요소 건너뛰기
    * skip(n)

```java
List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4, 6, 8);
numbers.stream()
       .filter(i -> i % 2 == 0)       // 2, 2, 4, 6, 8
       .distinct()                    // 2, 4, 6, 8
       .skip(1)                       // 4, 6, 8
       .limit(2)                      // 6, 8
       .forEach(System.out::println);
```

* 매핑

```java
List<Integer> dishNameLengths = menu.stream()
                                    .map(Dish::getName)   // Stream<Dish> -> Stream<String>
                                    .map(String::length)  // Stream<String> -> Stream<Integer>
                                    .collect(toList());
```

* 배열로 스트림 만들기
  * ```Arrays.stream(String[] strArr)```

```java
String[] arrayOfWords = {"Goodbye", "World"};
Stream<String> streamOfWords = Arrays.stream(arrayOfWords);
```

* 스트림 평면화

```java
List<String> words = Lists.newArrayList("Hello", "World");
words.stream()
     .map(word -> word.split("")) // split은 String[]을 반환하므로 Stream<String[]>
     .flatMap(Arrays::stream)     // 생성된 스트림을 하나의 스트림으로 평면화 Stream<String>
     .distinct()
     .collect(toList());
```

* 매칭
  * allMatch, anyMatch, noneMatch
  * 쇼트서킷 기법 적용

```java
if (menu.stream().allMatch(Dish::isVegetarian)) {
    System.out.println("vegetarian 모두");
}
if (menu.stream().anyMatch(Dish::isVegetarian)) {
    System.out.println("vegetarian 적어도 하나 존재");
}
if (menu.stream().noneMatch(Dish::isVegetarian)) {
    System.out.println("vegetarian 없음");
}
```

* 검색
  * findFirst, findAny(병렬 실행시 이득)
  * 쇼트서킷 기법 적용

```java
Optional<Dish> dish = menu.stream()
                          .filter(Dish::isVegetarian)
                          .findFirst();

Optional<Dish> dish = menu.stream()
                          .filter(Dish::isVegetarian)
                          .findAny();
```

* 리듀싱(fold)
  * 모든 스트림 요소를 처리해서 값으로 도출하는 연산
  * [Integer](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html) 정적 메소드 추가 : sum, max, min

```java
// reduce(T 초기값, BinaryOperator<T> 연산함수)
int sum = numbers.stream().reduce(0, (a, b) -> a + b);
int mul = numbers.stream().reduce(1, (a, b) -> a * b);
int sum = numbers.stream().reduce(0, Integer::sum);

// reduce(BinaryOperator<T> 연산함수) : 초기값 매개변수가 없으므로 Optional 반환함
Optional<Integer> sum = numbers.stream().reduce((a, b) -> a + b);
Optional<Integer> max = numbers.stream().reduce((a, b) -> (a >= b) ? a : b);
Optional<Integer> max = numbers.stream().reduce(Integer::max);
Optional<Integer> min = numbers.stream().reduce(Integer::min);

// map-reduce pattern
int count = menu.stream()
                .map(d -> 1)
                .reduce(0, Integer::sum);
long count = menu.stream().count();
```

* 기본형 특화 스트림
  * IntStream, DoubleStream, LongStream
    * 매핑 : mapToInt(), mapToDouble(), mapToLong()
    * 중간연산 : range(), rangeClosed(), ...
    * 최종연산 : sum(), max(), min(), average(), ...
  * boxed() : 객체 스트림으로 복원

```java
// mapToInt, sum
int calories = menu.stream()
                   .mapToInt(Dish::getCalories) // Stream<Integer> 가 아닌 IntStream 반환
                   .sum();                      // IntStream 의 메소드

// rangeClosed
IntStream evenNumbers = IntStream.rangeClosed(1, 100)      // [1, 100]
                                 .filter(n -> n % 2 == 0);
System.out.println(evenNumbers.count());

// boxed
IntStream intStream = menu.stream().mapToInt(Dish::getCalories);
Stream<Integer> stream = intStream.boxed();

// max, OptionalInt
OptionalInt maxCalories = menu.stream()
                              .mapToInt(Dish::getCalories)
                              .max();
int max = maxCalories.orElse(1);
```

* 스트림 만들기

```java
// 빈 스트림 : Stream.empty()
Stream<Object> stream = Stream.empty();

// 임의의 인수 : Stream.of()
Stream<String> stream = Stream.of("Java 8", "In", "Action");

// 배열 : Arrays.stream()
int[] numbers = {1, 2, 3};
IntStream stream = Arrays.stream(numbers);

// NIO API 파일 : Files.lines()
long uniqueWords = 0;
try (Stream<String> lines = Files.lines(Paths.get("data.txt"), Charset.defaultCharset())) {
    uniqueWords = lines.flatMap(line -> Arrays.stream(line.split(" ")))
                       .distinct()
                       .count();
}
catch (IOException e) { /* ... */ }
```

* 무한 스트림 만들기(unbounded stream)

```java
// Stream.iterate(T 초기값, UnaryOperator<T> 반복함수)
Stream.iterate(0, n -> n + 2)        // 0부터 시작해서 끊임없이 2가 증가되는 스트림
      .limit(10)                     // 처음 10개로 제한
      .forEach(System.out::println);

// Stream.generate(Supplier<T> 생성함수)
Stream.generate(Math::random)
      .limit(5)
      .forEach(System.out::println);
```

* API
  * [java.util.stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)
  * [Stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)
  * 연산별 분류
    * 중간 연산 : filter, map, flatMap, skip, limit, sorted, distinct
    * 최종 연산 : anyMatch, noneMatch, allMatch, findAny, findFirst, forEach, count, collect, reduce, max, min
  * 상태별 분류
    * 내부 상태를 갖지 않는 연산
      * filter, map, flatMap, anyMatch, noneMatch, allMatch, findAny, findFirst, forEach, count, collect
    * 내부 상태를 갖는 연산
      * bounded(상태 크기 한정, 결과를 누적) : skip, limit, reduce, sum, max, min
      * unbounded(모든 요소가 버퍼에 있어야 연산) : sorted, distinct

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

### Optional<T>

* [Optional](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)
  * isPresent() : 값을 포함하면 true
  * ifPresent(Consumer<T>) : 값이 있으면 블록 실행
  * get() : 값이 존재하면 반환, 없으면 NoSuchElementException
  * orElse(T) : 값이 있으면 반환, 없으면 기본값(인수) 반환

```java
Optional<Dish> dish = menu.stream()
                          .filter(Dish::isVegetarian)
                          .findAny();
                          .ifPresent(d -> System.out.println(d.getName());
```

* 기본형 특화 Optional
  * OptionalInt, OptionalDouble, OptionalLong

```java
OptionalInt maxCalories = menu.stream()
                              .mapToInt(Dish::getCalories)
                              .max();
int max = maxCalories.orElse(1);
```
