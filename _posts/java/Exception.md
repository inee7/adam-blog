# Exception

에러와 예외가 나눠지는데 

에러는 시스템문제라 코드로 처리 할 수 없음 JVM이 발생 

예외를 개발자가 처리해줘야 되는데 

**checked** 예외는 외부에 의해 발생하는 예외들이고 **반드시 처리** 해줘야 된다. 

**unchecked** 예외는 개발자의 로직 문제로 발생할 수 있는 예외로 처리는 **선택적이다**. **NPE**가 대표적

**예외 처리를 안해주면 애플리케이션은 죽으면서 메시지를 뱉어낸다.** null point exception 같은.. 

**RuntimeException**은 **unCheckedException**이다 즉, 코드 잘짜면 발생 안하는 예외다 그래서 if문으로..

CheckedException은 반드시

메소드에 throws 절 있으면 try catch하는게 좋다 

 

 모든 예외 클래스들이 Throwable 클래스를 상속한다.

getMessage는 Throwable클래스의 메소드이다.

 

예외상황 약속된 클래스

-ArrayIndexOutOfBoundsException 배열 인덱스 잘못된 참조

-ClassCastException 형 변환 잘못함 

-NegativeArraySizeException 배열크기 음수로

-NullPointerException 참조변수가 null인데 메소드 호출

 

finally는 중간에 return을 하더라도 finally 영역은 실행되고 나서 메소드를 빠져나감

 

Exception은 모든 예외클래스의 상위 클래스

상속한 예외클래스는 100여개

 

RuntimeException (Exception하위클래스)를 상속하는 예외클래스는 예외처리 하지 않아도 된다.

(ArrayIndexOutOfBoundsException, ClassCastException,

NegativeArraySizeException,NullPointerException )

 

 

 

사용자 예외처리 

 public class DepositeMinusException extends Exception {

 DepositeMinusException(){

  super("입금액이 잘못되었습니다."); 

 }

 

}

--------------------------------------------------------호출되 메소드 예외 전달

protected void deposit(int money) throws DepositeMinusException //입금

 { 

  if(money<0){ 

   throw new DepositeMinusException(); 

   

  } 

  

  balance += money; 

 }

------------------------------------------------------호출한 메소드 예외 처리

 try{

​      ac.deposit(0); 

​      ac.withdraw(110); 

​      }catch(DepositeMinusException ma){ 

​       ma.printStackTrace(); 

 

 

 

public class NoBalanceException extends Exception {

 int balance ;

 int money;

 

 public NoBalanceException(int balance, int money) {

 this.balance=balance;

  this.money=money; 

  

 }

 public String toString(){

  return "출금을 원하는 금액은"+this.money+"원 이고 잔액은 "+this.balance+"원 입니다."; 

 }

 

}

 protected int withdraw(int money)throws NoBalanceException   //출금 

 {

  if(balance < money) 

   throw new NoBalanceException(balance, money); 

  balance -= money; 

  return money; 

 }

 

 

​      try{ 

​      ac.deposit(0); 

​      ac.withdraw(110); 

   }catch(NoBalanceException nb){

​       System.err.println(nb); 

​      }  

 

사용자 정의 예외 상황

-Exception 클래스를 상속 (Throwable 하위 클래스) Throwable 말고 Exception 상속하는 이유 : Throwable클래스 밑에는 Exception뿐 아니라 Error클래스도 있어서

메소드 호출 -> 해당 메소드에 붙일 예외상황 클래스를 throws로 붙이고 if로 예외일때 예외상황클래스 생성 후 throw한다

 throws를 붙이는 이유는 예외상항이 메소드 내에서 처리되지 않으면 메소드를 호출한 영역으로 예외의 처리가 넘어감을 명시하기 위해서이다. 

결국 throw에 의해 생성된 예외상황은 반드시 try~catch문에 의해 처리되거나 throws에 의해 넘겨져야 한다.

만약 호출한 영역에도 예외처리가 없다면 main에 throws를 붙여 자바가상머신으로 넘겨줘야한다.

그럼 자바가상머신은 getMessage메소드를 호출하고 예외상황 발생 전달과정을 출력하고 프로그램 종료시킨다.

 

PrintStackTrace 메소드는 getMessage메소드 호출시 반환되는 문자열과 전달과정을 보여준다.

 

 

printStackTrace() - 모든 원인을 추적하면서 알려줌

getMessage() - 간단한 결과 

printStackTrace가 그냥 콘솔 창에 출력만 되기 때문에 별도로 문자열을 만들 때 getStactTrace()을 사용해 문자열로 변경해서 처리 

  

