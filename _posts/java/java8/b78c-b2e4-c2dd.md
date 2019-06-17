

### Java8\#01. 람다 표현식\(Lambda Expression\)



Java8에서 뭐가 추가됐나요? 라고 물으면 가장 먼저 들리는 대답은 십중팔구 '람다와 스트림이요' 일것이다. 당연히 맞는 말이고 틀린 답은 아니지만 람다와 스트림이 왜 추가됐는지를 알아야하는데, 자바가 기존에 없던 문법까지 만들어가면서 이런것들을 추가한건 함수형 프로그래밍을 받아들이기 위해서다. 그래서 Java8을 공부하고자하는 사람이라면 그냥 단순히 '이번에 추가된 람다랑 스트림공부해야지'가 아니라 함수형 프로그래밍을 공부해야한다. 이는 프로그래밍 자체의 패러다임이 변하기때문에 그저 추가된 문법, 추가된 API만 공부하면 되는 수준이 아니라 프로그래밍 방식 자체, 문제 해결을 위한 사고방식 자체를 기존에서 탈피해야함을 의미한다. 해보니 기존에도 못하던걸 새로운걸 받아들이려고하니까 너무 어렵다;; 그래서 공부한것들을 정리해보고자 Java8의 포스팅을 시작한다.

**1. 람다 표현식\(Lambda Expression\)**

람다의 핵심은 지울수 있는건 모두 지우자는 것이다. 모든걸 컴파일러의 추론에 의지하고 코드로 표현하는건 다 없애버려 코드를 간결하게 만드는 것이다.

```
interface Movable{
    void move(String str);
}
class Car implements Movable{
    @Override
public void move(String str) {
        System.out.println("gogo car move" + str);
}
}
Movable movable = new Movable() {
    @Override
public void move(String str) {
        System.out.println("gogo move move" + str
```

Movable 인터페이스를 구현하고있다. 따로 Movable 인터페이스를 구현하는 클래스를 만들어 객체화하거나 재사용성이 없다면 그 자리에서 바로 생성하는 익명클래스 객체를 구현해야하는게 기존 방식이다. 어떤 방식을 쓰든 @Override 애노테이션을 제외해도 5줄은 필요하다.

재사용이 필요한 Car 클래스같은경우는 재사용을 위해 클래스로 남겨두고 익명 클래스부분을 람다표현식으로 수정해보자. 먼저 저기서 어떤부분을 지워도될지 한번 생각해보자.

1\) 이미 대상타입\(Target Type\)에서 Movable 이라고 명시했기때문에 new Movable부분은 없어도 컴파일러가 추론할 수 있다.

2\) 구현하려고보니까 구현해야할건 move\(\) 메서드밖에 없다. 만약 구현해야할 메서드가 1개뿐이라면 메서드명칭도 없어도 되지않을까

3\) 컴파일러가 인터페이스와 메서드를 추론했다면 인자도 추론할 수 있을 것이다. 구현해야할때 인자를 써야하니 인자를 아예 다 지우진 못해도 타입까지는 명시하지않아도 될 것 같다.

이를 토대로 람다표현식으로 고쳐보면

```
Movable movable1 = (str) -> {
    System.out.println("gogo move move" + str);
};  
```

이런 결과가 나온다. 여기서 더 없앨 수 있을게 있는지 생각해보자.

1\) 인자가 여러개면 몰라도 1개일땐 괄호가 없어도 되지 않을까

2\) 실행구문이 지금처럼 1줄일땐 중괄호가 없어도 되지 않을까

Movable movable2 = str -&gt; System.out.println\("gogo move move" + str\);

5줄이던 코드가 1줄로 줄어들었다. 람다 표현식은 자바에서 기존에 지원하지않던 문법이 추가되어서 놀라운거지 사실 그 구현 자체가 어렵진않다.

1-1. 함수형 인터페이스\(Functional Interface\)

위 예제에서 확인한 람다 표현식은 구현해야될 추상 메서드가 1개인 인터페이스를 구현한 것이다. 인터페이스를 구현하고자하는데 어차피 구현해야될 메서드가 1개뿐이니 이름이고 뭐고 다 지워버린것이다. 그럼 메서드가 2개일땐 어떻게 해야할까? 람다로는 지원하지않는다. 람다 표현식으로 구현이 가능한 인터페이스는 오직 추상 메서드가 1개뿐인 인터페이스만 가능하며 그렇기때문에 추상 메서드가 1개인 인터페이스를 부르는 명칭이 추가됐다. 그것이 함수형 인터페이스다. 그럼 가만히 생각해보자. 추상 메서드가 1개뿐인 인터페이스가 있고, 난 이걸 다른곳에서 람다 표현식으로 구현했다. 그런데 시간이 흘러 다른 동료가 해당 인터페이스를 수정할 일이 생겼고, 추상 메서드를 추가하고자한다. 인터페이스에는 당연히 추상메서드의 개수를 제한하는 방법은 없으므로 다른 곳에 람다 표현식이 있는걸 모르고 추상메서드를 추가하는 일이 발생할 수 있다. 열심히 추상 메서드를 추가하고 구현한다음 빌드를 하는데 여기저기서 빌드가 깨지는걸 보면 그 동료\(후임자\)는 멘탈이 붕괴될 것이다. 이런 불상사를 막기 위해 @FunctionalInterface 애노테이션이 존재한다.

```
@FunctionalInterface
interface Movable{
    void move(String str);
}
```

@FunctionalInterface 애노테이션은 해당 인터페이스가 함수형 인터페이스라는걸 알려준다. 추상메서드가 1개가 아닐경우에 컴파일에러를 내뿜는다. 아무 생각없이 추상 메서드를 추가하면 이러면 안된다는것을 알려주는 것이다. @Override 애노테이션과 마찬가지로 꼭 해당 애노테이션이 붙지 않아도 람다 표현식으로 구현할 수 있으나 이 인터페이스의 용도를 알리기위해 붙여주는것이 좋다.

1-2. 상태가 없는 객체\(Stateless Object\)

추상 메서드가 1개인 인터페이스를 구현할 때는 람다 표현식을 이용하면 매우 간결한 코드로 구현할 수 있다는 것을 알았다. 하지만 클래스를 구현할때 클래스에는 메서드\(행위\)만 존재하란 법은 없다. 경우에 따라서는 인스턴스 필드\(상태\)가 존재하는 클래스가 있을 수도 있는데 아래같은 클래스를 말한다.

```
Movable movable = new Movable() {
private int speed;
@Override
public void move() {
        System.out.println("gogo move move current speed : " + speed);
}
};
```

    

익명객체를 구현할때는 인스턴스 필드를 추가할 수 있다. 하지만 람다는 바로 메서드를 구현해버리니 인스턴스 필드가 들어갈 공간이 없어보인다. 인스턴스 필드를 추가하려면 어떻게 해야할까? 방법은 없다. 메서드와 함수의 가장 큰 차이는 메서드는 객체에 종속되어있다는 것이다. 함수란 인풋\(Input\)에 의해서만 아웃풋\(Output\)이 달라져야하는데 메서드는 객체에 종속적이기때문에 인풋이 달라지지않아도 객체의 상태에 따라 결과값이 다를 수 있다. 함수형 프로그래밍에서 함수는 인풋에 의해서만 아웃풋이 달라져야하며 그것을 지원하기위해 람다 표현식으로 구현할때 객체는 상태를 가질 수 없다. 이는 추후에 알아볼 스트림\(Stream\)을 이용한 병렬\(Parallel\)적 프로그래밍시 매우 중요한 요소다.

1-3. 행위 파라미터화\(Behavior Parameterize\)

보통 코드를 짤때 데이터를 매개변수로 전달하고 해당 데이터를 가지고 무언가 행위를 하는 메서드를 구현하기 마련이다. 하지만 데이터를 전달하는 것이 아닌 행위를 전달하게되면 좀 더 유연한 코드가 될 수 있는데 '이게 뭔말인가..'싶다면 예제 코드를 봐보자.

```
class Fruit{
private String name;
    private String color;
Fruit(String name, String color){
        this.name = name;
        this.color = color;
}

String getName(){
        return this.name;
}

String getColor(){
        return this.color;
}
}
List<Fruit> extractApple(List<Fruit> fruits){
    List<Fruit> resultList = new ArrayList<>();
    for(Fruit fruit : fruits){
        if("apple".equals(fruit.getName())){
            resultList.add(fruit);
}
    }

return resultList;
}

List<Fruit> extractRed(List<Fruit> fruits){
    List<Fruit> resultList = new ArrayList<>();
    for(Fruit fruit : fruits){
        if("red".equals(fruit.getColor())){
            resultList.add(fruit);
}
    }

return resultList;
}
```

    

Fruit 클래스가 있고, 해당 객체들의 리스트를 받아 특정 조건으로 추출해내는 메서드 2개다. 메서드구현부분을 잘 보면 if문 외에는 전부 동일한 걸 볼 수 있다. 이걸 행위 파라미터를 이용해 1개의 메서드로 합치면 훨씬 보기가 좋을것 같다.

```
static List<Fruit> extractFruitList(List<Fruit> fruits, Predicate<Fruit> predicate){
List<Fruit> resultList = new ArrayList<>();

for(Fruit fruit : fruits){

if(predicate.test(fruit)){

resultList.add(fruit);

}

}



return resultList;

}
```



그래서 합쳤다. 비교하는 행위를 파라미터로 입력받아 if문 내 조건을 하드코딩에서 동적으로 처리하게끔 파라미터가 1개 늘었다. 호출하는 부분은 이렇다.

```
List<Fruit> fruits = Arrays.asList(new Fruit("apple", "red"), new Fruit("melon", "green"), new Fruit("banana",
List<Fruit> appleList = extractFruitList(fruits, new Predicate<Fruit>() {
    @Override
public boolean test(Fruit fruit) {
        return "apple".equals(fruit.getName());
}
});
List<Fruit> redList = extractFruitList(fruits, new Predicate<Fruit>() {
    @Override
public boolean test(Fruit fruit) {
        return "red".equals(fruit.getColor());
}
}); "yellow"));
```



2개의 메서드는 합쳐져서 만족스러운데 호출하는 구문이 지저분해진거같다. 메서드가 2개일때와 마찬가지로 오히려 이제는 호출부분이 return문만 제외하고 똑같은 코드가 반복된다. 그런데 잘 보니 Predicate&lt;T&gt;라는 인터페이스는 뭔진 모르겠지만 추상메서드가 1개뿐인것같다\(test\). 그럼 람다 표현식으로 고칠 수 있지 않을까?

```
List<Fruit> appleList = extractFruitList(fruits, fruit -> "apple".equals(fruit.getName()));
```

```
List<Fruit> redList = extractFruitList(fruits, fruit -> "red".equals(fruit.getColor()));
```

코드가 상당히 줄어든걸 볼 수 있다. 행위 파라미터라는 기법 자체는 기존에도 익명 클래스를 이용해 매우 활용되고 있던 기법이다. 자바 개발자라면 안쓸수가 없는 Spring Framework가 익명 클래스를 이용한 행위 파라미터를 적극 활용해 Template Callback Pattern이라는 디자인패턴까지 만들정도로 이미 유용히 사용되던 기법이다. 이것이 함수형 인터페이스와 람다 표현식으로 인해 더욱 간결하게 사용될 수 있는것이다.



