# Chapter 7. 병렬 데이터 처리와 성능

- 이 장에서는 스트림으로 데이터 컬렉션 관련 동작을 얼마나 쉽게 병렬로 실행할 수 있는지 설명한다.

## 7.1 병렬 스트림
- 컬렉션에 parallelStream을 호출하면, `병렬 스트림`이 생성된다.
- 병렬 스트림이란 각각의 스레드에서 처리할 수 있도록 스트림 요소를 여러 청크로 분할한 스트림이다.
- 따라서, 병렬 스트림을 이용하면, 모든 멀티코어 프로세서가 각각의 청크를 처리하도록 할당할 수 있다.




### 7.1.1 순차 스트림을 병렬 스트림으로 변환하기
- 


### 7.1.2 스트림 성능 측정
### 7.1.3 병렬 스트림의 올바른 사용법
### 7.1.4 병렬 스트림 효과적으로 사용하기

## 7.2 포크/조인 프레임워크
- 포크/조인 프레임워크는 병렬화할 수 있는 작업을 재귀적으로 작은 작업으로 분할한 다음에, 서브태스크 각각의 결과를 합쳐서, 전체 결과를 만들도록 설계되었다.



### 7.2.1 RecursiveTask 활용
```java
public class ForkJoinSumCalculator extends RecursiveTask<Long> { // RecursiveTask를 상속받아 포크/조인 프레임워크에서 사용할 태스크를 생성한다.

  public static final long THRESHOLD = 10_000; // 이 값 이하의 서브태스크는 더 이상 분할 수 없다.

  private final long[] numbers; // 더할 숫자 배열
  private final int start; // 이 서브태스크에서 처리할 배열의 초기 위치와 최종 위치
  private final int end;

  public ForkJoinSumCalculator(long[] numbers) { // 메인 테스크를 생성할 때 사용할 공개 생성자
    this(numbers, 0, numbers.length);
  }

  private ForkJoinSumCalculator(long[] numbers, int start, int end) { // 메인 테스크의 서브 태스크를 재귀적으로 만들 때 사용할 비공개 생성자
    this.numbers = numbers;
    this.start = start;
    this.end = end;
  }

  @Override
  protected Long compute() { // RecursiveTask의 추상 메서드 오버라이드
    int length = end - start; // 이 태스크에서 더할 배열의 길이
    if (length <= THRESHOLD) {
      return computeSequentially(); // 기준값과 같거나 작으면 순차적으로 결과를 계산한다.
    }
    ForkJoinSumCalculator leftTask = new ForkJoinSumCalculator(numbers, start, start + length / 2); // 배열의 1번째 절반을 더하도록 서브태스크를 생성한다.
    leftTask.fork(); // ForkJoinPool의 다른 스레드로 새로 생성된 태스크를 비동기로 실행한다.
    ForkJoinSumCalculator rightTask = new ForkJoinSumCalculator(numbers, start + length / 2, end); // 배열의 나머지 절반을 더하도록 서브태스크를 생성한다.
    Long rightResult = rightTask.compute(); // 2번째 서브태스크를 동기 실행한다. 이때, 추가로 분할이 일어날 수 있다.
    Long leftResult = leftTask.join(); // 1번째 서브태스크의 결과를 읽거나 아직 결과가 없으면 기다린다.
    return leftResult + rightResult; // 두 서브태스크의 결과를 조합한 값이 이 태스크의 결과다.
  }

  private long computeSequentially() { // 더 분할할 수 없을 때 서브태스크의 결과를 계산하는 단순한 알고리즘
    long sum = 0;
    for (int i = start; i < end; i++) {
      sum += numbers[i];
    }
    return sum;
  }

  public static long forkJoinSum(long n) {
    long[] numbers = LongStream.rangeClosed(1, n).toArray();
    ForkJoinTask<Long> task = new ForkJoinSumCalculator(numbers);
    return FORK_JOIN_POOL.invoke(task);
  }

}
```



### 7.2.2 포크/조인 프레임워크를 제대로 사용하는 방법
### 7.2.3 작업 훔치기


## 7.3 Spliterator 인터페이스
- Spliterator : 분할할 수 있는 반복자


### 7.3.1 분할 과정
### 7.3.2 커스텀 Spliterator 구현하기



## 7.4 마치며




