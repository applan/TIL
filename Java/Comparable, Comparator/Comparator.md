# [Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)

> JAVA 1.8 버전기준
## Comparator
> Doc에서 얻을 수 있는 것
> 
> Comparators can be passed to a sort method (such as Collections.sort or Arrays.sort) to allow precise control over the sort order.  
> Comparators은 (Collections.sort, Arrays.sort) 정렬 메소드에 정렬 순서를 정밀하게 제어할 수 있도록 조절할 수 있습니다.  
>
> Comparators can also be used to control the order of certain data structures (such as sorted sets or sorted maps),  
> Comparators는 ( 정렬된 Sets 혹은 maps) 데이터 구조에 정렬을 제어할 수 있으며,
> 
> or to provide an ordering for collections of objects that don't have a natural ordering.  
> 자연스럽게 정렬되어있지 않은 컬렉션 객체에 순서를 제공할 수도 있습니다.

`Comparators can be passed to a sort method (such as Collections.sort or Arrays.sort) to allow precise control over the sort order.`  
요게 가장 중요하다고 생각한다. Comparators는 정렬 메소드에 정렬 순서를 정밀하게 제어할 수 있도록 조절할 수 있다고 써있다. 

한 번 직접 해보았다. 

### 테스터 코드

>       ## 공통
>       - num  : 장난감 상품 ID
>       - name : 장난감 명
>       - pay  : 장난감 가격
> 
>       ## 원하는 정렬
>       1. 장난감 가격 오름차순으로 정렬
>       2. 같은 가격이면 ID 오름차순
>
>       두개의 객체를 만든 후 Colloction.sort를 사용해보자.


```java

// 추상화 클래스
public abstract class Toy {

    public Integer num;
    public String name;
    public Integer pay;

    public Integer getNum() {
        return num;
    }

    public void setNum(Integer num) {
        this.num = num;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getPay() {
        return pay;
    }

    public void setPay(Integer pay) {
        this.pay = pay;
    }

    public Toy(Integer num, String name, Integer pay) {
        this.num = num;
        this.name = name;
        this.pay = pay;
    }

    @Override
    public String toString() {
        return "Toy{" +
                "num=" + num +
                ", name='" + name + '\'' +
                ", pay=" + pay +
                '}';
    }
}
```

```java
// 추상화 클래스를 구현한 객체 Block, Rego
public class Block extends Toy{

    public Block(Integer num, String name, Integer pay) {
        super(num, name, pay);
    }

}
```

```java
// 메인 
public class Main {

    public static void main(String[] args) {
        // 블록을 담을 List 만들기 
        List<Block> blocks = new ArrayList<>();
        // 랜덤한 가격을 위해 Random 객체 생성
        Random random = new Random();

        // 테스트 데이터 생성
        for(int i=0; i<100; i++){
            blocks.add(new Block(i, "꼬마블럭" + i, random.nextInt(1000)));
        }

        // 정렬 전 출력
//        for(Block block : blocks){
//            System.out.println(block.toString());
//        }
        // 람다식 이용해서 좀더 간결한 코드 작성
        System.out.println("#############################Sort Before###################################");
        blocks.forEach((x) -> System.out.println(x.toString()));
        System.out.println("###########################################################################");
        
        // Num Sort를 위한 Comparator 객체 생성
//        Comparator<Toy> toyNumComparator = (o1, o2) -> {
//            return o1.getNum().compareTo(o2.getNum());
//        };
        
        // Comparator의 comparing() 메소드와 이중 콜론 연산자를 통해 좀더 깔끔하게 변경
        Comparator<Toy> toyNumComparator = Comparator.comparing(Toy::getNum);
        
        // Pay Sort를 위한 Comparator 객체 작성
        Comparator<Toy> toyPayComparator = Comparator.comparing(Toy::getPay);

        /*
         * 다음 조건을 위해 Comparaotr 새롭게 구성
         * 1. 장난감 가격 오름차순으로 정렬 (toyPayComparator)
         * 2. 같은 가격이면 ID 오름차순 (toyNumComparator)
         *     
         *    Comparaotr.thenComparing(Comparaotr)
         * -> toyPayComparator.thenComparing(toyNumComparator)
         *    1번째 Comparaotr 정렬 후 2번째 Comparaotr 정렬 사용
         */

        Comparator<Toy> toyComparable = toyPayComparator.thenComparing(toyNumComparator);
        
        // 정렬
        Collections.sort(blocks, toyComparable);

        // 정렬 확인
        System.out.println("#############################Sort After####################################");
        blocks.forEach((x) -> System.out.println(x.toString()));
        System.out.println("###########################################################################");

    }
}
```

### 테스트

다음과 같이 pay 순으로 오름차순 정렬되며 중간에 중복된 pay가 있는 경우 num 오름차순으로 정렬됨.  
  
![Comparator_1_Result](https://user-images.githubusercontent.com/48544100/159523183-3f83f5da-699a-4250-a0ce-d7ea693f524a.JPG)


#### 디버깅