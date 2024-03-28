# 비동기 프로그래밍이란?
* Async한 통신으로 Main Thread가 Task를 처리하는 것이 아니라 Sub Thread에게 Task를 위임하는 행위
* 실시간성 응답을 필요로 하지 않는 상황에서 사용
* ex) notification, Email 전송, Push 알림

# Spring에서의 비동기 프로그래밍
* Spring에서 비동기 프로그래밍을 하기 위해서는 ThreadPool을 정의할 필요가 있다.

# ThreadPool 생성이 필요한 이유
* 비동기는 Main Thread가 아닌 Sub Thread에서 작업이 진행
* Java에서는 ThreadPool을 생성하여 Async 처리를 하기 때문

#
1. CorePoolSize
   - 최소 몇 개까지의 쓰레드를 가지고 있을 것인가
2. MaxPoolSize
   - 최대 몇 개까지 쓰레드 수를 설정
3. WorkQueue
   - 먼저 들어온 작업부터 처리하기 위함 (WorkQueue 라는 곳에 담아두고 차례대로 작업을 가져옴)
4. KeepAliveTime
   - 코어풀 사이즈보다 많은 쓰레드를 가졌을 때 반환을 해야하는데 내가 정한 시간만큼 이 쓰레드들이 일을 하지 않으면 반납 하겠다라는 옵션

# Thread Pool 생성시 주의해야할 부분
* CorePoolSize를 너무 크게 설정할 경우 -> 사용되지 않는 쓰레드들이 있을 수 있어서 자원이 낭비됨
* 2가지 Exception
   - IllegalArgumentException
     - CorePoolSize < 0
     - KeepAliveTime < 0
     - MaxPoolSize ≤ 0
     - MaxPoolSize < CorePoolSize
   - NullPointerException
     - WorkQueue가 null인 경우

# Thread Pool 정리
 * CorePoolSize
   if(Thread 수 < CorePoolSize)
     new Thread 생성
   if(Thread 수 > CorePoolSize)
     Queue 요청 추가
 * MaxPoolSize
   if(Queue Full && Thread 수 < MaxPoolSize)
     new Thread 생성
   if(Queue Full && Thread 수 > MaxPoolSize)
     요청 거절
   
  
