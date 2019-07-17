# JAVA Garbage Collection





GC가 실행되면 JVM이 애플리케이션 실행을 멈춘다. 

그만큼 GC튜닝을 잘해야 성능을 높일 수 있다는 말이다. 



### 가비지 컬렉터 역할

**1.** 메모리 할당

**2.** 사용 중인 메모리 인식

**3.** 사용하지 않는 메모리인식





Young (1. Eden, 2. Survivor 1, 3. Survivor 2), Old (4. 메모리) )



**마이너 GC:** Young 영역에서 발생하는 GC

**메이저 GC:** Old 영역이나 Perm 영역에서 발생하는 GC

이 두가지 GC가 어떻게 상호 작용하느냐에 따라서 GC 방식에 차이가 나며, 성능에도 영향을 줍니다. 

**GC가 발생하거나 객체가 각 영역에서 다른 영역으로 이동할 때 애플리케이션의 병목이 발생하면서 성능에 영향을 주게 됩니다.** 

그래서 핫 스팟(Hot Spot) JVM에서는 스레드 로컬 할당 버퍼(TLABs: Thread-Local Allocation Buffers)라는 것을 사용합니다. 이를 통해 각 스레드별 메모리 버퍼를 사용하면 다른 스레드에 영향을 주지 않는 메모리 할당 작업이 가능합니다.





https://12bme.tistory.com/57



#####Young영역 

: 새롭게 생성한 객체 대부분은 여기에 위치한다. 그러다 대부분 금방 접근 불가능한 상태가 되서 Young에 생성됐다가 사라진다. 

Yong에서는 Minor GC가 발생 

: 일단 메모리에 객체가 생성되면, Eden 영역에 객체가 지정됩니다.  Eden 영역에 데이터가 어느 정도 쌓이면, 이 영역에 있던 객체가 어디론가 옮겨지거나 삭제됩니다. 이 때 옮겨가는 위치가 survivor 영역입니다. 두개의 Survivor 영역 사이에 우선 순위가 있는 것은 아닙니다. 하지만, 이 두 개의 영역 중 한 영역은 반드시 비어 있어야 합니다. 그 비어있는 영역에 Eden 영역에 있던 객체가 할당됩니다.

: Eden에서 survivor 둘 중 하나의 영역으로 할당 되고, 할당된 Survivor 영역이 차면, GC가 되면서 Eden 영역에 있는 객체와 꽉 찬 Survivor 영역에 있는 객체가 비어 있는 Survivor 영역으로 이동합니다. 그러다가 더 큰 객체가 생성되거나, 더 이상 Young 영역에 공간이 남지 않으면 객체들은 Old 영역으로 이동하게 됩니다.



#####Old영역 (Yong영역보다 큰 공간이므로 GC는 적게 발생)

: 접근 불가능 상태로 되지 않아 Young영역에서 살아남은 객체가 여기로 여기로 복사된다. 

Old에서는 Major GC(or Full GC) 발생



#####Perm영역

: 메소드영역이라고 한다. 객체나 억류된 문자열 정보를 저장하는 곳. Old영역에서 살아남은 객체가 영원히 남아있는곳은 절대 아님. 여기서 GC발생해도 Major GC의 횟수에 포함.  거의 사용되지 않는 영역으로서 클래스와 메소드 정보와 같이 자바 언어 레벨에서 사용되지 않습니다.



![](../../assets/img/gc영역.png)



Old 영역에 있는 객체가 Young영역의 객체를 참조하는 경우 해결하기 위해 Old영역에 512바이트의 chunk로 되어있는 카드 테이블이 존재

![](../../assets/img/카드테이블구조.png)

Young영역의 GC를 실행할 때에는 Old영역에 있는 모든 객체의 참조를 확인하지 않고 , 이 카드 테이블만 뒤져서 GC대상인지 식별.

카드 테이블은 write barrier을 사용하여 관리. write barrier은 Minor GC를 빠르게 할 수 있도록 하는 장치. 오버헤드는 발생하지만 GC시간은 줄어들게 함.  





**4가지 GC(가비지 콜렉터) 방식**

JDK 5.0이상에서 지원하는 GC 방식에는 네가지가 있습니다. WAS나 자바 애플리케이션 수행시 옵션을 지정하여 선택할 수 있습니다.



> **Serial Collector** (이하 시리얼 콜렉터)
>
> **Parallel Collector** (이하 병렬 콜렉터)
>
> **Parallel Compacting Collector** (이하 병렬 컴팩팅 콜렉터)
>
> **Concurrent Mark-Sweep (CMS) Collector** (이하 CMS 콜렉터)



**1. 시리얼 콜렉터**

Young 영역과 Old 영역이 시리얼하게(연속적으로) 처리되며 하나의 CPU를 사용합니다. Sun에서는 이 처리를 수행할 때를 Stop-the-world라고 표현합니다. 다시 말하면, 콜렉션이 수행될 때 애플리케이션 수행이 정지됩니다.



![img](https://t1.daumcdn.net/cfile/tistory/2703E43958FF0CD22F)





1) 일단 살아있는 객체들은 Eden 영역에 있습니다.

2) Eden 영역이 꽉차게 되면 To Survivor 영역(비어있는 영역)으로 살아 있는 객체가 이동합니다. 이때 Survivor 영역에 들어가기에 너무 큰 객체는 바로 Old 영역으로 이동합니다. 그리고 From Survivor 영역에 있는 살아 있는 객체는 To Survivor 영역으로 이동합니다.

3) To Survivor 영역이 꽉 찼을 경우, Eden 영역이나 From Survivor 영역에 남아 있는 객체들은 Old 영역으로 이동합니다.



이 이후에 Old 영역이나 Perm 영역에 있는 객체들은 Mark-sweep-compact 콜렉션 알고리즘을 따릅니다. 이 알고리즘에 대해서 간단하게 말하면, 안 쓰는 거 표시해서 삭제하고 한 곳으로 모으는 알고리즘입니다.



> *** Mark-sweep-compact 알고리즘**
>
> 1) Old 영역으로 이동된 객체들 중 살아 있는 개체를 식별합니다. (Mark)
>
> 2) Old 영역의 객체들을 훑는 작업을 수행하여 쓰레기 객체를 식별합니다. (Sweep)
>
> 3) 필요 없는 객체들을 지우고 살아 있는 객체들을 한 곳으로 모은다 (Compact)

이렇게 작동하는 시리얼 콜렉터는 일반적으로 클라이ㅏ언트 종류의 장비에서 많이 사용됩니다. 다시 말하면, 대기 시간이 많아도 크게 문제되지 않는 시스템에서 사용된다는 의미 입니다.



시리얼 콜렉터 명시적 지정 방법

> -XX:+UseSerialGC





**2. 병렬 콜렉터**

이 방식은 throughput collector로도 알려진 방식입니다. 이 방식의 목표는 다른 CPU가 대기 상태로 남아 있는 것을 최소화하는 것입니다. 시리얼 콜렉터와 달리 Young 영역에서의 콜렉션을 병렬(Parallel)로 처리합니다. 많은 CPU 를 사용하기 때문에 GC의 부하를 줄이고 애플리케이션의 처리량을 증가시킬 수 있습니다.



![img](https://t1.daumcdn.net/cfile/tistory/266ED54458FF183C06)







Old 영역의 GC는 시리얼 콜렉터와 마찬가지로 Mark-Sweep-Compact 콜렉션 알고리즘을 사용 합니다.



병렬 콜렉터 명시적 지정 방법입니다.

> -XX:+UseParallelGC





**3. 병렬 컴팩팅 콜렉터**

병렬 콜렉터와 다른 점은 Old 영역 GC에서 새로운 알고리즘을 사용합니다. 그러므로 Young 영역에 대한 GC는 병렬 콜렉터와 동일하지만, Old 영역의 GC는 다음의 3단계를 거치게 됩니다.



1) Mark 단계 : 살아 있는 객체를 식별하여 표시해 놓는 단계

2) Sweep 단계 : 이전에 GC를 수행하여 컴팩션된 영역에 살아 있는 객체의 위치를 조사하는 단계

3) Compact 단계 : 컴팩션을 수행하는 단계. 수행 이후에는 컴팩션된 영역과 비어 있는 영역으로 나뉩니다.



병렬 콜렉터와 동일하게 이 방식도 여러 CPU를 사용하는 서버에 적합합니다. GC를 사용하는 스레드 개수는 -XX:ParallelGCThreads=n 옵션으로 조정할 수 있습니다.



병렬 컴팩팅 콜렉터 명시적 지정방법입니다.

> -XX:+UseParallelOldGC





**4. CMS 콜렉터**

이 방식은 low-latency collector로도 알려져 있으며, 힙 메모리 영역의 크기가 클 때 적합합니다. Young 영역에 대한 GC는 병렬 콜렉터와 동일합니다. Old 영역의 GC는 다음 단계를 거칩니다.



1) Mark 단계 : 매우 짧은 대기 시간으로 살아 있는 객체를 찾는 단계

2) Sweep 단계 : 서버 수행과 동시에 살아 있는 객체에 표시를 해 놓는 단계

3) Remark 단계 : Concurrent 표시 단계에서 표시하는 동안 변경된 객체에 대해서 다시 표시하는 단계

4) Concurrent Sweep 단계 : 표시되어 있는 쓰레기를 정리하는 단계



![img](https://t1.daumcdn.net/cfile/tistory/24208F4958FF1D9E2B)



CMS는 컴팩션 단계를 거치지 않기 때문에 왼쪽으로 메모리를 몰아 놓는 작업을 수행하지 않습니다. 그래서 GC 이후에 그림과 같이 빈 공간이 발생하므로, -XX:CMSInitiatingOccupancyFraction=n 옵션을 사용하여 Old 영역의 %를 n 값에 지정합니다. (기본값 : 68)



CMS 콜렉터 방식은 2개 이상의 프로세서를 사용하는 서버에 적당합니다. 가장 적당한 대상으로는 웹서버가 있습니다.



CMS 콜렉터 명시적 지정방법입니다.

> -XX:+UseConcMarkSweepGC



CMS 콜렉터는 추가적인 옵션으로 점진적 방식을 지원합니다. 이 방식은 Young 영역의 GC를 더 잘게 쪼개어 서버의 대기 시간을 줄일 수 있습니다. CPU가 많지 않고 시스템의 대기 시간이 짧아야 할 때 사용하면 좋습니다. 점진적은 GC를 수행하려면 -XX:+CMSIncrementalMode 옵션을 지정하면 됩니다. JVM에 따라서는 -Xingc라는 옵션을 지정해도 같은 의미가 됩니다. 하지만 이 옵션을 지정할 경우 예기치 못한 성능 저하가 발생할 수 있으므로, 충분한 테스트를 한 후에 운영 서버에 적용해야 합니다.



네 종류 GC 방식에 대한 성능 및 기능 비교 표입니다.





![img](https://t1.daumcdn.net/cfile/tistory/2129254758FF215238)



![img](https://t1.daumcdn.net/cfile/tistory/2511F44C58FF216E0F)







위의 내용을 종합하면, GC 수행 시점은 다음과 같습니다. 



1) 각 영역의 할당된 크기의 메모리가 허용치를 넘을 때

2) 개발자가 컨트롤할 영역은 아님.





개발자라면, 자바의 GC 방식을 외우면서 개발하거나 서버를 세팅할 필요는 없습니다. 이해만 하고 있으면 됩니다. **단, 필요할때(시스템 오픈 전 성능 테스트 / 서버 세팅 시), 알맞은 GC 방식을 개발한 시스템에 적용**하면 된다고 합니다.





https://12bme.tistory.com/57





참조 

http://java.sun.com/javase/technologies/hotspot/gc/index.jsp

http://d2.naver.com/helloworld/1329

https://12bme.tistory.com/57