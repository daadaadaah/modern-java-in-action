# Chapter 6. 스트림으로 데이터 수집

```java
private static void groupImperatively() {
    Map<Currency, List<Transaction>> transactionsByCurrencies = new HashMap<>(); // 그룹화한 트랜잭션을 저장할 맵을 생성한다.

    for (Transaction transaction : transactions) { // 트랜잭션 리스트를 반복한다.
      Currency currency = transaction.getCurrency(); //  트랜잭션의 통화를 추출한다.

      List<Transaction> transactionsForCurrency = transactionsByCurrencies.get(currency);

      if (transactionsForCurrency == null) { // 현재 통화를 그룹화하는 맵에 항목이 없으면 항목을 만든다.
        transactionsForCurrency = new ArrayList<>();
        transactionsByCurrencies.put(currency, transactionsForCurrency);
      }

      transactionsForCurrency.add(transaction); // 같은 통화를 가진 트랜잭션 리스트에 현재 탐색 중인 트랜잭션을 추가한다. 
    }

    System.out.println(transactionsByCurrencies); // 결과물 출력
  }
```

```java
private static void groupFunctionally() {
    Map<Currency, List<Transaction>> transactionsByCurrencies = transactions.stream()
        .collect(groupingBy(Transaction::getCurrency));
    System.out.println(transactionsByCurrencies);
}
```



## 6.1 컬레터란 무엇인가?
### 6.1.1 고급 리듀싱 기능을 수행하는 컬렉터






### 6.1.2 미리 정의된 컬렉터
- Collectors에서 제공하는 메서드의 기능은 크게 3가지로 구분할 수 있다.
1. 스트림 요소를 하나의 값으로 리듀스하고 요약
2. 요소 그룹화
3. 요소 분할




## 6.2 리듀싱과 요약
### 6.2.1 스트림값에서 최대값과 최솟값 검색
- `Collectors.maxBy`, `Collectors.minBy` 2개의 메서드를 이용해서, 스트림의 최댓값과 최솟값을 계산할 수 있다.


### 6.2.2 요약 연산
- 요약 연산 : 스트림에 있는 객체의 숫자 필드의 `합계`나 `평균` 등을 반환하는 연산
1. 합계 연산 : Collectors.summingInt, Collectors.summingLong, Collectors.summingDouble
```java
  private static int calculateTotalCalories() {
    return menu.stream().collect(summingInt(Dish::getCalories));
  }
```


2. 평균 연산 : Collectors.averagingInt, Collectors.averagingLong, Collectors.averagingDouble

```java
  private static Double calculateAverageCalories() {
    return menu.stream().collect(averagingInt(Dish::getCalories));
  }
```

3. 총 정보(수, 합계, 평균, 최댓값, 최소값 등) 연산 : Collectors.summarizingInt, Collectors.summarizingLong, Collectors.summarizingDouble

```java
   private static IntSummaryStatistics calculateMenuStatistics() {
    return menu.stream().collect(summarizingInt(Dish::getCalories));
  }
```


### 6.2.3 문자열 연결




### 6.2.4 범용 리듀싱 요약 연산







## 6.3 그룹화
- 데이터 집합을 1개의 특성으로 분류해서 그룹화한다.


### 6.3.1 그룹화된 요소 조작
### 6.3.2 다수준 그릅화
### 6.3.3 서브그룹으로 데이터 수집



## 6.4 분할
### 6.4.1 분할의 장점

### 6.4.2 숫자를 소수와 비소수로 분할하기



## 6.5 Collector 인터페이스
### 6.5.1 Collector 인터페이스의 메서드 살펴보기
### 6.5.2 응용하기


## 6.6 커스텀 컬렉터를 구현해서 성능 개선하기
### 6.6.1 소수로만 나누기
### 6.6.2 컬렉터 성능 비교

## 6.7 마치며


