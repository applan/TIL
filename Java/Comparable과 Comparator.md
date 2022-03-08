# [Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)  , [Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)
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
>     두개의 객체를 만든 후 Collections.sort, Arrays.sort 를 사용해보자.

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

        // ⓑ 상황 ( Comparable 미구현 객체 )
        Car niro = new Kia("니로", 2833, "G1.6 하이브리드", "트렌디");
        Car sportage = new Kia("스포티지", 2488, "G1.6 T-GDI엔진", "프레스티지");
        List<Car> kias = new ArrayList<>(Arrays.asList(niro, sportage));
        System.out.println(kias.toString());
        Collections.sort(kias);
        System.out.println(kias.toString());
        
    }
}
```

### 결과 보고
- ⓐ
  - 다음 에러 발생  
    >    reason: no instance(s) of type variable(s) T exist so that ToyCar conforms to Comparable<? super T>  
                 
    ![Comparable_2_ToyCar](https://user-images.githubusercontent.com/48544100/157258623-bad16c0c-4182-4963-b405-e129e60757b6.JPG)
- ⓑ

## Comparator




## 참고

[자바 [JAVA] - Comparable 과 Comparator의 이해](https://st-lab.tistory.com/243)
