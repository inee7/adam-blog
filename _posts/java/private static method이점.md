# private static method 이점

우선 private static method 는 private method에 비해 성능면에서는 이점이 없다.

 

코드 가독성에서 이점을 주장하는 사람이 있는데

method 를 static 으로 선언 함으로서 해당 method 가 

객체 인스턴스 상태를 변경하지 않는다는, 

다시말해 인스턴스의 멤버변수에 수정하지 않는 다는 점을 

읽는 사람에게 알려줄 수 있다는 주장이다.

 

public method 와 public static method 를 비교했을 때는 상황이 조금 다른데

static 으로 선언된 method 는 묵시적으로 final 이다. (상속되지 않는다.)

때문에 dynamic linking 에 대한 처리가 불필요해져서 성능상에 이점을 갖는다.