

Java8\#04. 스트림\(Stream\)



개인적으로 자바8의 꽃이라 생각하는 스트림 포스팅이다. 내용이 워낙 방대하긴하나 쉽게 생각하면 스트림은 결국 API들의 모임이기때문에 외워야할게 많기도하다.

1. 스트림\(Stream\)

스트림은 자바8에 추가된 API로 자바의 자료구조들을 선언적으로 다루는 역할을 한다. 앞선 함수형 인터페이스 포스팅에서 설명했던 인터페이스들이 엄청나게 등장을 하니 스트림을 다룬다면 외우기 싫어도 외워질수밖에 없을 것이다.

자료구조들을 다루는 역할을 하기때문에 스트림은 배열이나 List처럼 생성한 다음 요소를 추가하는 형태가 아니다. 정적 팩토리 메서드\(Static Factory Method\)를 이용해 자료구조로부터 생성한다.

int\[\] numberArr = {1, 2, 3, 4, 5, 6};

List&lt;Integer&gt; numberList = Arrays.asList\(1, 2, 3, 4, 5, 6\);

Set&lt;Integer&gt; numberSet = new HashSet&lt;&gt;\(numberList\);

Arrays.stream\(numberArr\);

Stream.of\(1, 2, 3, 4, 5, 6\);

numberList.stream\(\);

numberSet.stream\(\);

1-1. 스트림 vs 컬렉션\(Collection\)

위에서 스트림은 자료구조들을 다룬다고했다. 하지만 이미 자바에서는 각종 자료구조들의 구현체를 손쉽게 사용할 수 있도록 util패키지를 제공하고있다. util패키지에서 제공하는 컬렉션 프레임워크는 자바의 자랑이자 강점이기도한데 무엇이 아쉬워서 스트림이 또 나온걸까? 앞선 표현에 이미 답이 있는데 컬렉션은 자료구조들의 구현체고 스트림은 자료구조들을 다루는 역할을 한다. 컬렉션은 데이터를 담는 것이 제역할이기때문에 제공하는 API들도 데이터를 넣었다 빼는것들이 대부분이다.

List&lt;Integer&gt; numbers = new ArrayList&lt;&gt;\(\);

numbers.add\(1\);

numbers.get\(0\);

numbers.remove\(1\);

그렇기때문에 컬렉션을 이용해 코딩을 할때는 컬렉션에 데이터들을 담고, 그 컬렉션을 순회하고 꺼내면서 직접적으로 연산을 하는것이 주를 이룬다.

List&lt;Integer&gt; numbers = Arrays.asList\(1, 2, 3, 4, 5, 6\);

List&lt;Integer&gt; evenList = new ArrayList&lt;&gt;\(\);

for\(int number : numbers\){

    if\(number % 2 == 0\){

        evenList.add\(number\);

}

}



System.out.println\(evenList\);

\(numbers 리스트에서 짝수를 추출하는 코드. 리스트는 요소를 추가, 삭제, 순회하는 API만 제공하기때문에 내가 어떻게 짝수를 걸러내야하는지를 짜야된다.\)

이에반해 스트림은 연속된 자료들을 다루고 연산하는 API를 지원한다. 컬렉션과는 다르게 요소를 추가한다거나 삭제하는건 불가능하다. 연산을 지원하기때문에 짝수만 걸러내는것도 매우 명시적이고 선언형으로 짤 수 있다.

List&lt;Integer&gt; evenList = Stream.iterate\(1, n -&gt; n+1\)

        .limit\(6\)

        .filter\(number -&gt; number % 2 == 0\)

        .collect\(toList\(\)\);

System.out.println\(evenList\);

아주 간단한 예제다보니 라인수에는 별 차이가 없는것 같지만 내가하고싶은일이 뭔지 명령을 내리는 코드형태이지 그 명령을 어떤식으로 처리하는지가 코드에 나타나진 않는다는 차이점이 있다. 스트림은 내부반복을 지원하기때문에 반복문같은것도 코드에서 전혀 나타나지않는다.

연산이 복잡해지면 복잡해질수록 컬렉션을 이용한 코드는 반복문이 계속 등장하고 연산을 저장할 변수들이 나타나면서 뭘 하고싶어하는지 알아보기가 힘들어지게되는 반면 스트림을 이용하면 코드량과 가독성을 한번에 취할 수 있게되는 것이다.

1-2. 중간 연산, 최종 연산

위에 짧은 예제코드를 보면 알 수 있겠지만 스트림을 사용한 코드는 계속해서 도트\(Dot\) 연산자로 메서드 체이닝\(Method Chaining\)을 일으킨다. 초보개발자의 경우 도트가 연속해서 나오면 코드를 이해하기 어려워하는 경우도 많은데 저런 경우는 한가지만 명확히 생각하면 된다. 메서드 체이닝 중간에 있는 메서드는 절대 void 형태가 아니며 메서드가 반환하는 어떤 객체가 도트 뒤에 나오는 메서드를 갖고있다고 보면 되는것이다.

/\*  limit\(\) 메서드는 무언가 객체를 반환할것이다.

    void타입의 메서드라면 filter\(\) 메서드를 호출할 수가 없기때문이다.

    limit\(\)이 반환하는 객체는 filter\(\) 메서드를 갖고있는 객체다.

    그리고 filter\(\) 메서드역시 collect\(\) 라는 메서드를 갖고있는 객체를 반환한다.

 \*/

.limit\(6\)

.filter\(number -&gt; number % 2 == 0\)

.collect\(toList\(\)\);

limit\(\), filter\(\), collect\(\) 같은 메서드들은 전부 스트림에서 제공하는 메서들이다. 결국 스트림 메서드들은 전부 연속해서 스트림을 반환하고 있기때문에 저런 코드가 가능한것이다.

마지막으로 collect\(\) 메서드는 스트림이 아니라 List 타입을 반환하기때문에 List 변수에 무언가 값을 저장하고있는걸 알 수 있는데 스트림 API는 이런식으로 스트림을 반환하는 메서드와 스트림이 아닌 값을 반환하는 메서드로 나뉜다.

그리고 계속해서 스트림을 반환하여 메서드 체이닝의 근간이 되게하는 메서드들을 중간 연산 메서드라 부르고 스트림이 아닌 값을 반환하여 메서드 체이닝을 끊는 메서드를 최종 연산 메서드라 부른다.

이 둘을 이렇게 구분짓는건 생각보다 중요한데, 최종 연산이 존재하지 않으면 중간연산들로만 이루어진 메서드 체이닝은 실행되지 않는다. 이 코드는 아무런 값도 찍히지 않는다.

Stream.iterate\(1, n -&gt; n+1\)

        .limit\(6\)

        .peek\(System.out::println\)  // 스트림을 순회하며 하나씩 요소를 꺼내 출력하는 구문

.filter\(number -&gt; number % 2 == 0\);

중간 연산이고 최종 연산이고 중간에 출력문을 하나 넣어놨으니 이 코드는 실행하면 출력문이 나와야 될듯 하지만 아무것도 찍히지 않는다. 최종연산이 없기때문에 중간연산만으로는 실행이 되지 않는것이다.

Stream&lt;Integer&gt; stream = Stream.iterate\(1, n -&gt; n+1\)

        .limit\(6\)

        .peek\(System.out::println\)

        .filter\(number -&gt; number % 2 == 0\);

stream.collect\(toList\(\)\);

이런식으로 최종 연산 메서드가 실행될때 비로소 중간 연산들도 실행이 된다.

1-3. Lazy & ShortCircuit

스트림은 게으르\(Lazy\)다. 결론부터 말하자면 최종연산이 존재하지않으면 중간연산은 실행되지 않는다. 위에서부터 말했지만 스트림은 어디까지나 연산을 위한 객체로 그 자체로 자료구조의 역할을 하지 않는다. 때문에 최종연산이 존재하지않는 스트림은 그 의미가 없다고 볼 수 있다. 스트림이 게으르다는걸 확인할 수 있는 샘플코드는 어렵지않게 짤 수 있다.

Stream&lt;Integer&gt; stream = Stream.of\(1, 2, 3, 4, 5\);

Stream&lt;Integer&gt; s = stream.peek\(System.out::println\);

peek\(\)메서드는 forEach\(\)처럼 스트림의 요소를 순회하며 소비\(Consumer&lt;T&gt;\)하는 메서드이다. forEach\(\)와 한가지 다른점은 중간 연산이라는점이다. 때문에 해당코드를 실행하면 우리가 기대하는 출력문은 출력되지 않는다.

Stream&lt;Integer&gt; stream = Stream.of\(1, 2, 3, 4, 5\);

Stream&lt;Integer&gt; s = stream.peek\(System.out::println\);

s.collect\(toList\(\)\);

최종연산이 호출될때 비로소 중간연산들도 실행되는걸 볼 수 있다.

Lazy외에도 스트림은 여러 최적화기법들을 도입했는데 그 중하나가 Short Circuit이다. 이건 뭔말일꼬.. 할 수 있는데 &&\(And\), \|\|\(Or\) 연산을 생각하면 이해가 쉽다.

&&연산은 좌항과 우항 모두 true일때 true을 반환하는 연산인데 좌항이 false면 우항은 쳐다도보지않는다.

Object obj = null;

boolean b = 1 == 2 && obj.toString\(\).equals\(123\);

엉성한 코드지만..;; obj가 null이기때문에 거기다가 toString\(\)를 호출하면 NullPointerException이 발생해야한다. 하지만 이미 좌항이 false이기때문에 우항은 실행도 하지않게되고, 그리하여 아무런 예외없이 코드는 실행된다.

이런기법이 Short Circuit이다.

List&lt;String&gt; list = Arrays.asList\("a", "b", "c"\);

boolean b = list.stream\(\).allMatch\(str -&gt; {

   System.out.println\(str\);

   return str.equals\("d"\);

}\);

스트림 요소를 순회하면서 모든 요소에 Predicate이 true인지를 확인하는 구문이다. 출력된 내용을 보면 a만 찍히는걸 볼 수 있다. 전부다 d여야 true를 반환하는데 처음부터 d가 아니니 연산을 끊고 false를 반환하는 것이다.

1-4. 기본형 스트림

숫자타입 리스트를 선언할때는 제네릭을 이런식으로 선언한다.

List&lt;Integer&gt; list1 = new ArrayList&lt;&gt;\(\);

Auto Boxing, Auto Unboxing이 지원되면서 그냥 기본형 쓰듯 사용할 수 있지만 내부적으로까지 boxing 비용이 없어진건 아니다. 하지만 자바의 제네릭은 기본형을 지원하지않기때문에 어쩔수 없이 저렇게 사용할 수 밖에 없다. 스트림 역시 기본형으로 제한을 걸기위해서는 저런식으로 해야한다.

Stream&lt;Integer&gt; stream = Stream.of\(1, 2, 3, 4, 5\);

스트림은 이런 boxing 비용을 줄이기위해 기본형에 특화된 객체를 따로 제공하고있다.

IntStream intStream = IntStream.of\(1, 2, 3, 4, 5\);

DoubleStream doubleStream = DoubleStream.of\(1.0, 2.0, 3.0, 4.0, 5.0\);

LongStream longStream = LongStream.of\(1L, 2L, 3L, 4L, 5L\);

기본형 스트림을 사용하면 boxing비용을 줄일 수 있을 뿐더러 해당 타입에 알맞는 연산들을 메서드로 제공하고있다.

intStream.sum\(\); // Stream&lt;Integer&gt; 로도 가능하다. 다만 기본메서드로 제공되지는 않는다.

한가지 주의할 점은 스트림과 기본형 스트림은 관계가 없기때문에 단순하게 타입변환이 되지않는다. 그래서 다형성을 이용하기가 어렵다. 물론 그에 대한 API도 제공해주고있다.

//이외에도 mapToObj, mapToDouble 같은 메서드를 제공한다.

List&lt;Integer&gt; interger = intStream.boxed\(\).collect\(toList\(\)\);

적은 범위의 값들을 다룬다면 그냥 Boxing 비용을 감안하고서라도 Stream을 사용하는게 맘편해보이지만 그런 비용을 무시할 수 없을만큼 큰 범위의 데이터라면 기본형 스트림을 고려해보는것이 좋을것같다.

1-5. 대표적인 API들

람다는 기존에 없던 문법들이 추가되어 신기한 맘으로 공부해야하지만 사실 스트림은 특별할건 없다. 가볍게본다면 새로운 API가 추가된정도로 볼 수도있다. 개인적으로 스트림을 공부하면서 느낀건 그 철학이나 탄생배경도 중요하지만 일단 실무에서 써먹으려면 API를 달달 외워야 한다는것이다. 대표적인 API들을 소개하면서 포스팅을 마치겠다.

1\) 중간연산

-Stream&lt;R&gt; map\(Function&lt;A, R&gt;\)

-Stream&lt;T&gt; filter\(Predicate&lt;T&gt;\)

-Stream&lt;T&gt; peek\(Consumer&lt;T&gt;\)

2\) 최종연산

-R collect\(Collector\)

-void forEach\(Consumer&lt;T&gt;\)

-Optional&lt;T&gt; reduce\(BinaryOperator&lt;T&gt;\)

-boolean allMatch\(Predicate&lt;T&gt;\)

-boolean anyMath\(Predicate&lt;T&gt;\)



