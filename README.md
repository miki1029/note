# 개발 정리 노트

<http://d2.naver.com/helloworld>  
<https://trello.com/b/DlJ9r6tg/dev-ref>  
<https://okdevtv.com/md/list.html>  
<http://bit.ly/devbook>  
<http://j.mp/okdevtv-java>  

# 마크다운 작성 규칙

## 1. 강제 개행 : 끝에 스페이스 바 두 번

스페이스 바 두 번 ->  
강제 개행됨

## 2. 헤더 : # 헤더 이름

# 헤더
## 헤더
### 헤더
#### 헤더
##### 헤더
###### 헤더

## 3. 인용 상자 : > 내용

> 인용 상자
> 마찬가지로 스페이스 바 두 번 하면  
> 강제 개행됨

## 4. 목록

### 순서 없는 목록 : * 목록이름, - 목록이름, + 목록이름

* 목록이름*
- 목록이름-
+ 목록이름+

### 순서 있는 목록 : 숫자. 목록이름

1. 목록이름
1. 목록이름
1. 목록이름

### 하위 목록 : 앞에 공백 두칸~네칸

* 상위 목록
    * 하위 목록
      * 하위 목록

1. 상위 목록
    1. 하위 목록
        2. 하위 목록

## 5. 가로선 : ---, ***, ___

---
***
___


## 6. 코드 블록

\`\`\`프로그래밍언어이름  
코드내용  
\`\`\`

```java
System.out.println("Hello world!");
```
## 7. 인라인 요소

### a. 인라인 코드 : \`\`\`인라인 코드\`\`\`

```System.out.println("Hello world!");```

### b. 강조 : \*\*굵게\*\*, \_\_굵게\_\_, \*기울여\*, \_기울여\_

**굵게**, __굵게__, *기울여*, _기울여_, ***굵고기울여***, **_굵고기울여_**

### c-1. 하이퍼링크 : \[링크 문구\]\(링크URL "설명문구"\)

[GitHub](https://github.com "github link")

### c-2. 하이퍼링크 자동 링크 : \<링크 주소\>

<https://github.com>

### c-3. 하이퍼링크 GitHub wiki : \[\[문서 이름\]\], \[\[텍스트가 다른 문서|문서 이름\]\]

### d. 이미지 : \!\[이미지 문구\]\(이미지URL "설명문구"\)
![avartar](https://avatars1.githubusercontent.com/u/4443853?v=3&s=40 "@miki1029")

### e. 탈출 문자 : \\

\*굵게안됨\*
