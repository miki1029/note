# SpEL

* ID로 빈을 참조
* 메소드 호출
* 계산
* 정규 표현식 매칭
* 컬렉션 처리

```java
// systemProperties 객체를 통해 시스템 프로퍼티 참조
#{systemProperties['disc.title']}

// literal value
#{1}
#{3.14159}
#{9.87E4} // 98700
#{'Hello'} // String
#{false} // boolean

// property, method, nullsafe
#{sgtPeppers}
#{sgtPeppers.artist} // 빈 프로퍼티 참조
#{artistSelector.selectArtist()} // 빈 메소드 호출
#{artistSelector.selectArtist().toUpperCase()} // selectArtist()가 null이면 NullPointerException
#{artistSelector.selectArtist()?.toUpperCase()} // type-safe operator

// T() : 정적 메소드와 상수 액세스
T(java.lang.Math).PI
T(java.lang.Math).random()
```

**SpEL 연산자**

* 산술 : +, -, *, /, %, ^
* 비교 : <, lt, >, gt, ==, eq, <=, le, >=, ge
* 논리 : and, or, not, |
* 조건 : ?: (ternary), ?: (Elvis)
* 정규 표현식 : matches

```java
#{2 * T(java.lang.Math).PI * circle.radius}
#{T(java.lang.Math).PI * circle.radius ^ 2}
#{disc.title + ' by ' + disc.artist}
#{counter.total == 100}
#{counter.total eq 100}
#{scoreboard.score > 1000 ? "Winner" : "Loser"}
#{disc.title ?: 'Rattle and Hum'} // ?: 엘비스 연산자 - disc.title이 null이면 'Rattle and Hum'
#{admin.email matches '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.com'} // 결과는 boolean
```

**컬렉션 사용하기**

* 셀렉션 연산자
    * .?[] : 대괄호 내 표현식이 true인 엔트리들의 서브셋을 추출한다.
    * .^[] : 대괄호 내 표현식과 처음으로 일치하는 항목을 조회한다.
    * .$[] : 대괄호 내 표현식과 마지막으로 일치하는 항목을 조회한다.
* 프로젝션 연산자
    * .![] : 컬렉션 요소로부터 프로퍼티를 새로운 컬렉션으로 만든다.

```java
// 빈 jukebox의 5번째 요소의 title 프로퍼티
#{jukebox.songs[4].title}

// jukebox에서 song 하나를 무작위 선택
#{jukebox.songs[T(java.lang.Math).random() * jukebox.songs.size()].title}

// 네 번째 단일 문자 s
#{'This is a test'[3]}

// jukebox의 song 전체 리스트에서 artist 프로퍼티가 IU인 서브셋
#{jukebox.songs.?[artist eq 'IU']}

// artist 프로퍼티가 처음으로 IU와 일치하는 항목
#{jukebox.songs.^[artist eq 'IU']}

// 모든 song의 title 컬렉션
#{jukebox.songs.![title]}

// artist 프로퍼티가 IU인 song의 title 컬렉션
#{jukebox.songs.?[artist eq 'IU'].![title]}
```
