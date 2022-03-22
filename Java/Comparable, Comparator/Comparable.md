# [Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)
> JAVA 1.8 버전기준
## Comparable
> This interface imposes a total ordering on the objects of each class that implements it.  
> 이 인터페이스는 해당 인터페이스를 구현하는 모든 클래스 객체에게 전체 정렬을 적용합니다.  
> This ordering is referred to as the class's natural ordering,     
> 이 정렬을 클래스의 자연 정렬이라고 불리며,  
> and the class's **compareTo** method is referred to as its natural comparison method.  
> 클래스의 compareTo 메소드를 자연 비교 메소드라고 불립니다.

JavaDoc을 읽다보면 `Lists (and arrays) of objects that implement this interface can be sorted automatically by Collections.sort (and Arrays.sort)` 
요런 문구를 확인 할 수 있다.  
즉 해당 인터페이스로 구현한 객체는 자동으로 Collections.sort 또는 Arrays.sort를 이용할 수 있다는 것.  
한 번 직접 실험해보았다.

### 테스트 코드

>     ## 공통
>     - name : 자동차 명
>     - pay  : 금액
>     - engineType : 엔진 타입
>     - option : 선택한 옵션
>
>     ## Comparable 구현 객체
>     1. 자동차 추상클래스를 만든다.
>     2. 자동차 추상클래스는 Comparable을 상속한다.
>     3. 자동차 추상클래스는 다음을 포함해 구현한다.
>     3.5 Comparable의 compareTo 메소드 구현 
> 
>     두개의 객체를 만든 후 Collections.sort 를 사용해보자.

```java
// Comparable 미구현 객체
public class ToyCar {
    public String name;
    public Integer pay;
    public String engineType;
    public String option;
}
```

```java
// Comparable 구현 객체 
public abstract class Car implements Comparable<Car> {
    
    public String name;
    public Integer pay;
    public String engineType;
    public String option;
    
    @Override
    public int compareTo(Car o) {
        // 가격을 정렬 값으로 잡음
        return this.pay - o.pay;
        
    }
}
```

```java
// 메인 
public class Main {

    public static void main(String[] args) {
        // ⓐ 상황 ( Comparable 미구현 객체 )
        ToyCar toyCar = new ToyCar("장난감 자동차", 3, "모터", "기본");
        ToyCar luxuryToyCar = new ToyCar("명품 장난감 자동차", 10, "충전식 전기 모터", "최상");

        List<ToyCar> toyCars = new ArrayList<>(Arrays.asList(toyCar, luxuryToyCar));
        Collections.sort(toyCars);

        // ⓑ 상황 ( Comparable 구현 객체 )
        Car niro = new Kia("니로", 2833, "G1.6 하이브리드", "트렌디");
        Car sportage = new Kia("스포티지", 2488, "G1.6 T-GDI엔진", "프레스티지");
        List<Car> kias = new ArrayList<>(Arrays.asList(niro, sportage));
        System.out.println(kias.toString());
        Collections.sort(kias);
        System.out.println(kias.toString());
        
    }
}
```

### 테스트

#### · ⓐ 상황 

:exclamation: `다음과 같은 에러를 확인 할 수 있음`
```text
reason: no instance(s) of type variable(s) T exist so that ToyCar conforms to Comparable<? super T>
T 유형의 인스턴스가 없기 때문에 ToyCar 객체는 제네릭 <T>가 아닌 Comparable한 제네릭 <super T>를 따라야합니다.
```
![Comparable_2_ToyCar](https://user-images.githubusercontent.com/48544100/157258623-bad16c0c-4182-4963-b405-e129e60757b6.JPG)

해당 에러의 내용의 답은 [Collections.sort](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#sort-java.util.List) 에서 확인할 수 있었다.

![Comparable_3_Collections_sort](https://user-images.githubusercontent.com/48544100/157269818-b5f76544-f701-4d5d-b796-6c8d34e60573.JPG)

해당 내용 중 다음 설명이 중요하다.  
`All elements in the list must implement the Comparable interface.`  
`모든 구성은 Comparable 인터페이스를 구현해야합니다.`

에러가 난 이유는 ToyCar는 Comparable 인터페이스를 구현하지 않아서 발생한 오류이다.


#### · ⓑ 상황

에러 없이 잘 동작하는 것을 확인할 수 있다. (오름차순으로 정렬)
![Comparable_2_Car](https://user-images.githubusercontent.com/48544100/157258615-5b6a98d8-6223-4d41-b71a-5016b0686b52.JPG)
![Comparable_2_Car_Result](https://user-images.githubusercontent.com/48544100/157258618-78c08f0c-0017-4462-a571-97072c8be311.JPG)

#### · · ⓑ의 동작 확인해보기 

디버깅 모드로 직접 동작하는 방식에 Break Point 를 걸어보았다.

Break Point 를 걸어둔 소스
![Comparable_3_Collections_BreakPoint](https://user-images.githubusercontent.com/48544100/157672959-6216ab09-3792-4866-93be-a65b99873e06.JPG)

확인한 동작 순서
![Comparable_4_Debug](https://user-images.githubusercontent.com/48544100/157676369-61659e79-cebe-4c7f-a3b3-189b94f75215.JPG)

#### ··· ⓑ의 동작을 디버깅하며 알아낸 것

##### 1. Collection.sort 동작시 매개변수 List 타입의 sort를 동작
![Comparable_4_Debug_C_1](https://user-images.githubusercontent.com/48544100/157676751-3f60d7da-57c7-495f-8e9c-d53327eb8c83.JPG)  
<ArrayList 타입의 매개변수 -> ArrayList 의 sort 구현체를 사용 확인>
![Comparable_4_Debug_C_2](https://user-images.githubusercontent.com/48544100/157677120-5571ad2c-0630-4f20-b8c5-296cbdc838c4.JPG)
##### 2. [Arrays.sort(T\[\])](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-T:A-int-int-java.util.Comparator) -> [Arrays.sort(Object\[\])](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-java.lang.Object:A-int-int-) 사용
![Comparable_4_Debug_C_3](https://user-images.githubusercontent.com/48544100/157678763-34c3a18f-920f-4173-976a-07dc68cead54.JPG)
![Comparable_4_Debug_C_4](https://user-images.githubusercontent.com/48544100/157678767-1bb25576-0c5d-44b0-b87f-c7893563553c.JPG)
##### 3. 작은 리스트는 `Merge Sort`를 사용하지 않고 `mini-TimSort`를 사용
![Comparable_4_Debug_C_5](https://user-images.githubusercontent.com/48544100/158061137-b9cd33db-77e6-48e3-a404-67dc7aa4dedb.JPG)
##### 4. `Comparable` 인터페이스를 이용하여 정렬 
![Comparable_4_Debug_C_6](https://user-images.githubusercontent.com/48544100/158061198-e4625d42-d889-4621-a17d-7e76ca6f3864.JPG)
##### 5. 정렬 구현시 자주 사용하는 temp 변수를 두고 값 변경하는 것을 이용하여 값 순서 변경
![Comparable_4_Debug_C_7](https://user-images.githubusercontent.com/48544100/158062412-60517189-df89-4c53-aa7a-ef619a07f6fd.JPG)

---

### 마치며

Comparable 인터페이스를 이용하려면 compareTo() 메소드를 구현해야하며 Java에서는 기본적으로 
같은 타입의 인스턴스 비교에는 모두 Comparable 인터페이스를 구현하고있다.
따라서 Boolean을 제외한 래퍼 클래스나 String, Time, Date와 같은 클래스의 인스턴스는 모두 정렬이 가능합니다.
이때 기본 정렬 순서는 `작은값 -> 큰 값`으로 정렬되는 `오름차순`이 됩니다.


그럼 `오름차순`이 아닌 `내림차순`으로 정렬하고싶다면 어떻게 해야할까요?
compareTo() 를 구현할때 다음과 같이 소스를 수정하면 됩니다.
```text

    public int compareTo(Car o) {
        return this.pay - o.pay;  // 오름차순
        -> return o.pay - this.pay; // 내림차순
    }

```

하지만 기본적으로 `Comparable`은 오름차순을 기본으로 하기 때문에 해당 속성은 변경해주지 않는게 좋습니다.  

저는 Comparable 공부를 마친 뒤 다음을 생각했습니다.
- 정렬 필드가 한개가 아닌 경우 어떻게 해야할까?
- 내림차순으로 정렬하고싶다면 어떻게 해야할까?

해결 방법은 다음 공부할 `Comparator`에 있습니다.

---

## 참고

[자바 [JAVA] - Comparable 과 Comparator의 이해](https://st-lab.tistory.com/243)  
[JavaDoc Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)  
[JavaDoc Collections](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html)  
