# String, StringBuffer, StringBuilder

## 개요

소프트웨어 개발 시 문자열을 자주 사용하고있다.   
선임 개발자 분이 작성한 소스에 `StringBuffer`, `StringBuilder` 가 있는데 단지 `String`에서 특정한 작업을 진행해야할때   
사용한다고 생각하며 넘어갔지만 이직 준비 중 해당 질문이 나와서 확실하게 알아가고 싶어 공부를 시작했다.

## [String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)
`· doc 을 통해 알수 있는 내용`
>
> Strings are constant; their values cannot be changed after they are created.  
> 문자열은 일정합니다, 값을 만든 후에는 변경할 수 없습니다.  
> 
> String buffers support mutable strings.  
> StringBuffers 는 변경 가능한 문자열을 지원한다.(?)  
> 
> Because String objects are immutable they can be shared.  
> String 객체는 변경할 수 없으므로 공유할 수 있다.
> 
> The Java language provides special support for the string concatenation operator ( + ), and for conversion of other objects to strings.  
> Java 의 String 객체는 특별하게 + 연산자와 다른 개체를 문자열로 변환해줄 수 있다. 그 이유는 
> 
> String concatenation is implemented through the StringBuilder(or StringBuffer) class and its append method.  
> 문자열 연결은 StringBuilder(또는 StringBuffer) 클래스와 해당 추가 메서드를 통해 구현되기 때문입니다.
> 
> String conversions are implemented through the method toString, defined by Object and inherited by all classes in Java.  
> 문자열 변환은 Java 의 모든 클래스에서 상속되는 toString 메서드를 통해 구현됩니다.

### Strings are constant

`String 은 일정하다.` 이는 어떤 의미를 함축하고있는 것일까?   
우리는 우선 Java String 의 객체 생성 방법에 대해 알고있어야한다.

#### 1. String literal (String 리터널)

Java 문자열 객체 생성 첫 번째 방법 String 리터널 방법이다.  
기본 사용 법 `String 변수명 = "문자열";`

```java
public class Main {

    public static void main(String[] args){
        String old = "old";
        String old2 = "old";
    }
}
```

#### 2. new String

Java 문자열 객체 생성 두 번째 방법 new String 방법이다.  
기본 사용 법 `String 변수명 = new String("문자열");`
```java
public class Main {

    public static void main(String[] args){
        String old = new String("old");
        String old2 = new String("old");
    }
}
```



## [StringBuffer](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html)
`· doc 을 통해 알수 있는 내용`

## [StringBuilder](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html)
`· doc 을 통해 알수 있는 내용`



---

# 참고

[Okky hashCode(), equals(), 주소값에 대한 이야기 from_Frudy](https://okky.kr/article/596050)
[String Constant Pool이란?](https://starkying.tistory.com/entry/what-is-java-string-pool)
[Java Doc : String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)
[Java Doc : StringBuffer](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html)
[Java Doc : StringBuilder](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html)


# 읽어보자 

[Baeldung_java_string_pool](https://www.baeldung.com/java-string-pool)
[geeksforgeeks_string_constant_pool_in_java](https://www.geeksforgeeks.org/string-constant-pool-in-java/)