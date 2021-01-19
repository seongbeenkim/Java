# :coffee: Java   

### Essential Java Interview Questions - Java 필수 면접 질문 및 개념    
__개인적으로 공부하면서 작성한 것이기 때문에 틀리거나 표현이 어색한 부분이 있을 경우 말씀해주시거나 `Pull request` 또는 `Issue` 해주시면 수정하겠습니다. 감사합니다.:bow:__   

## :bookmark_tabs: Table of Contents      
- [1. Stream](#1-stream)   
  - [중간 연산 예제](#자주-사용하는-중간-연산)   
  - [최종 연산 예제](#자주-사용하는-최종-연산)   
2. [](#)   

-------------------------------------------------------------------------------------   

### 1. Stream   
- __자바 8부터 추가된 기능으로 Collections, Array 등을 변경하지 않고 함수형 인터페이스(람다식)를 적용하여 원하는 자료를 추출하는 기능__   
  - 반복적으로 수행되는 데 있어서 Iterator와 비슷하다고 볼 수 있습니다.
- __한 번 생성하고 사용한 Stream은 재사용 할 수 없습니다.__   
  - Stream 연산을 수행하면 Stream은 소모되기 때문에 다른 연산을 하기 위해서는 새로운 Stream을 생성해야 합니다.   
- __Stream 연산은 기존 자료(Collections, Array 등)를 변경하지 않습니다.__   
  - 자료에 대한 Stream을 생성하면 별도의 메모리 공간을 사용하므로 기존 자료를 변경하지 않습니다.   
- __Stream 연산은 중간 연산과 최종 연산으로 구분됩니다.__   
  - 중간 연산은 여러 개 적용될 수 있지만 최종 연산은 마지막에 한 번만 적용됩니다.   
  - 최종 연산이 호출되어야 중간 연산의 결과가 모두 적용되기 때문에 최종 연산을 '지연 연산'이라고 부르기도 합니다.   
- __병렬 처리가 가능합니다.__   

#### 자주 사용하는 중간 연산     
  - __예제에서 사용할 리스트__
    ```Java
    List<String> names = Arrays.asList("Eric", "Elena", "Java");   
    ```   
    
  - `filter()`   
    - Stream 내 요소들을 하나씩 평가해서 걸러내는 연산으로 boolean을 리턴하는 함수형 인터페이스(람다식)를 인자로 받습니다.   
    ```Java   
    Stream<String> stream = names.stream().filter(name -> name.contains("a"));
    // [Elena, Java]
    ```   
  - `map()` 
    - Stream 내 요소들이 input이 되어서 함수형 인터페이스(람다식)를 거친 후 output이 되어 새로운 Stream에 담기게 됩니다.    
    ```Java   
    Stream<String> stream = names.stream().map(String::toUpperCase);
    // [ERIC, ELENA, JAVA]   
    Stream<Integer> stream = productList.stream().map(Product::getAmount);
    // [23, 14, 13, 23, 13]   
    ```    
  - `sorted()`   
    - Comparator를 이용하여 Stream 내 요소들을 정렬합니다.   
      - 인자없이 호출할 경우 오름차순으로 정렬   
      - Comparator 사용법과 동일합니다.   
    ```Java   
    IntStream.of(14, 11, 20, 39, 23).sorted().boxed().collect(Collectors.toList());
    // [11, 14, 20, 23, 39]   
    ```    

    ```Java   
    List<String> lang = Arrays.asList("Java", "Scala", "Groovy", "Python", "Go", "Swift");
    lang.stream().sorted().collect(Collectors.toList());
    // [Go, Groovy, Java, Python, Scala, Swift]
    lang.stream().sorted(Comparator.reverseOrder()).collect(Collectors.toList());
    // [Swift, Scala, Python, Java, Groovy, Go]   
    ```    

    ```Java   
    lang.stream().sorted(Comparator.comparingInt(String::length)).collect(Collectors.toList());
    // [Go, Java, Scala, Swift, Groovy, Python]
    lang.stream().sorted((s1, s2) -> s2.length() - s1.length()).collect(Collectors.toList());
    // [Groovy, Python, Scala, Swift, Java, Go]
    ```   

  - __`boxed()`__   
    - Stream 내 요소의 primitive 타입을 wrapper 타입으로 박싱하여 반환합니다.   
    ```Java
    List<Integer> integer = IntStream.of(14, 11, 20, 39, 23).boxed().collect(Collectors.toList());   
    ```   

  - __`peek()`__   
    - Stream 내 요소들 각각을 대상으로 특정 연산은 수행하는 연산으로, 특정 결과를 반환하지 않는 함수형 인터페이스(람다식)을 인자로 받습니다.   
    ```Java   
    int sum = IntStream.of(1, 3, 5, 7, 9).peek(System.out::println).sum();   
    ```   

#### 자주 사용하는 최종 연산   
  - __Calculating__   
    - __최소, 최대, 합, 평균 등 기본형 타입으로 결과를 반환합니다.__   
    - __`count()`__   
      - Stream 내 요소들의 갯수를 세어 기본형 타입으로 결과 반환합니다.   
      ```Java   
      long count = IntStream.of(1, 3, 5, 7, 9).count();
      // 5
      ```   
    - __`sum()`__   
      - Stream 내 요소들을 더하여 기본형 타입으로 결과 반환합니다.   
      ```Java   
      long sum = LongStream.of(1, 3, 5, 7, 9).sum();
      // 25
      ```   
    - __`min() / max()`__   
      - 평균, 최소, 최대의 경우에는 Stream이 비어있으면 표현할 수가 없기 때문에 Optional을 이용해 리턴합니다.   
      ```Java   
      OptionalInt min = IntStream.of(1, 3, 5, 7, 9).min();
      // OptionalInt[1]
      OptionalInt max = IntStream.of(1, 3, 5, 7, 9).max();
      // OptionalInt[9]  
      ```    

  - __Reduction__   
    - __`reduce()`__   
      - __사용자가 원하는 결과를 만들어내기 위한 함수형 인터페이스(람다식)을 정의하여 사용하는 방법__   
      - __세 가지의 파라미터를 받을 수 있습니다.__   
        - `accumulator` : Stream 내 각 요소를 처리하는 계산 로직. 각 요소가 올 때마다 중간 결과를 생성하는 로직   
        - `identity` : 계산을 위한 초기값으로, 스트림이 비어서 계산할 내용이 없더라도 이 값은 리턴       
        - `combiner` : 병렬(parallel) 스트림에서 나눠 계산한 결과를 하나로 합치는 동작하는 로직   
      - `BinaryOperator<T>` : 같은 타입의 인자 두 개를 받아 같은 타입의 결과를 반환하는 함수형 인터페이스   
      - __인자 한 개 받는 경우__   
      ```Java   
      OptionalInt reduced = IntStream.range(1, 4).reduce((a, b) -> {
        return Integer.sum(a, b);   
      });
      // OptionalInt[6]
      ```   

      - __인자 두 개 받는 경우__   
      ```Java   
      int reducedTwoParams = IntStream.range(1, 4).reduce(10, Integer::sum); // 16
      ``` 
        - 초기값이 10이며 Integer 클래스의 sum 함수를 사용하여 10 + 1 + 2 + 3 = 16   

      - __인자 세 개 받는 경우__   
      ```Java   
      Integer reducedParams = Stream.of(1, 2, 3)
        .reduce(10, // identity
                Integer::sum, // accumulator
                (a, b) -> {
                  System.out.println("combiner was called");
                  return a + b;
                }); 
      // 16
      ``` 
        - 위의 과정에서는 Combiner가 실행되지 않아서 16이 반환된다.   
        - __Combiner는 병렬 Stream에서만 동작한다.__   
        ```Java   
        Integer reducedParallel = Arrays.asList(1, 2, 3)
          .parallelStream() // 병렬 스트림
          .reduce(10, // identity
                  Integer::sum, // accumulator
                  (a, b) -> {
                    System.out.println("combiner was called");
                    return a + b;
                  });
        // 결과          
        // combiner was called
        // combiner was called
        // 36
        ```   
        - __accumulator가 총 세 번 동작합니다.__   
          - 초기값 10에 각 스트림 값을 더한 세 개의 값(10 + 1 = 11, 10 + 2 = 12, 10 + 3 = 13)을 계산합니다.   
        - __Combiner는 identity 와 accumulator 를 가지고 여러 쓰레드에서 나눠 계산한 결과를 합치는 역할입니다.__   
          - 12 + 13 = 25, 25 + 11 = 36 이렇게 두 번 호출됩니다.   

  - __Collecting__   
    - __Collector 타입의 인자를 받아 처리를 하며 주로 Collectors 객체에서 기능을 제공합니다.__   
    - __예제에서 사용할 리스트__   
      ```Java   
      List<Product> productList = 
        Arrays.asList(new Product(23, "potatoes"),
                      new Product(14, "orange"),
                      new Product(13, "lemon"),
                      new Product(23, "bread"),
                      new Product(13, "sugar"));
      ```   
      
    - __`Collectors.toList()`__   
      - __Stream에서 작업한 결과를 리스트에 담아 반환합니다.__   
      ```Java   
      List<String> collectorCollection =
        productList.stream()
          .map(Product::getName)
          .collect(Collectors.toList());
      // [potatoes, orange, lemon, bread, sugar]
      ```   
      
    - __`Collectors.joining()`__   
      - __Stream에서 작업한 결과를 하나의 String으로 이어 붙일 수 있습니다.__   
      ```Java   
      String listToString = 
        productList.stream()
          .map(Product::getName)
          .collect(Collectors.joining());
      // potatoesorangelemonbreadsugar 
      
      String listToString = 
        productList.stream()
          .map(Product::getName)
          .collect(Collectors.joining(", ", "<", ">"));
      // <potatoes, orange, lemon, bread, sugar>
      ```   
      
    - __`Collectors.averageingInt()`__   
      - __숫자 값(Integer value)의 평균을 반환합니다.__   
      ```Java   
      Double averageAmount = 
        productList.stream()
          .collect(Collectors.averagingInt(Product::getAmount));
      // 17.2
      ```   
      
    - __`Collectors.summingInt()`__   
      - __숫자 값의 합을 반환합니다.__   
      ```Java   
      Integer summingAmount = 
        productList.stream()
          .collect(Collectors.summingInt(Product::getAmount));
      // 86
      ```   
      - `mapToInt()`를 사용하여 더 간단하게 표현할 수 있습니다.   
      ```Java   
      Integer summingAmount = 
        productList.stream()
          .mapToInt(Product::getAmount)
          .sum();
      // 86
      ```   
      
    - __`Collectors.summarizingInt()`__   
      - 개수, 합계, 최소, 평균, 최대를 반환합니다.   
      ```Java   
      IntSummaryStatistics statistics = 
        productList.stream()
          .collect(Collectors.summarizingInt(Product::getAmount));
      // IntSummaryStatistics {count=5, sum=86, min=13, average=17.200000, max=23}   
      ```   
    
  - __Matching__   
    - __함수형 인터페이스(람다식)을 통해 해당 조건에 만족하는 요소가 있는 지 확인한 결과(true/false)를 반환합니다.__   
    - __예제에서 사용할 리스트__   
      ```Java   
      List<String> names = Arrays.asList("Eric", "Elena", "Java");   
      ```
    - __`anyMatch()`__   
      - __Stream의 요소 중 하나라도 조건을 만족하는 요소가 있다면 true, 없다면 false를 반환합니다.__   
      - __Stream이 비어있다면 false를 반환합니다.__   
      ```Java   
      boolean anyMatch = names.stream()
        .anyMatch(name -> name.contains("a"));   
      // true
      
      List<String> names = new ArrayList<>();
      boolean anyMatch = names.stream()
        .anyMatch(name -> name.contains("a"));   
      // false
      ```   
      
    - __`allMatch()`__   
      - __Stream의 모든 요소가 조건을 만족하면 true, 아니라면 false를 반환합니다.__   
      ```Java   
      boolean allMatch = names.stream()
        .allMatch(name -> name.length() > 3);   
      // true
      ```   
      - __하지만, Stream이 비어있을 경우 주의해야합니다.__   
      ```Java   
      List<String> names = new ArrayList<>();
      boolean allMatch = names.stream()
        .allMatch(name -> name.length() > 3);   
      // true!!
      ```   
        - __위와 같은 경우 `false`가 아닌 `true`를 반환한다. 이러한 결과가 나오는 이유는 `Vacuous Truth` 때문입니다.__   
        - __Vacuous Truth__   
          - 가정이 모순이라면 주장이 무엇이든 상관없이 명제가 참이 되는 것을 의미합니다.   
          - 즉, p -> q 라는 명제가 있을 경우, p가 거짓이면 q는 참이 된다.   
            - ex) 명제 : 내가 가진 모든 전자기기는 애플사 제품이다.   
              - 내가 가진 전자기기가 애플사 제품이면? p -> q : T
              - 내가 가진 전저기기가 애플사 제품이 아니면? p -> q : F   
              - __내가 가진 전자기기가 없다면? 애플사 제품이든 아니든 상관이 없게 된다.__   
              - 그러므로 뒤의 조건이 참인지 거짓인지 판별하는 것은 의미가 없다. -> 이러한 경우에 있어서 진리값을 참이라고 정의하여 Vacuous Truth라고 불린다.     
          - 그렇기 때문에, Stream이 비어있다면, 뒤에 어느 Predicate가 오더라도 true라는 값을 반환하게 됩니다.   
            - [Oracle java docs](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#allMatch-java.util.function.Predicate-)
        - 그렇기 때문에 Stream이 비어있는 경우 false로 처리하고 싶다면 if문으로 비어있는지 확인하는 코드를 작성하는 것이 좋습니다.   
        - [Why does Stream.allMatch() return true for an empty stream?](https://stackoverflow.com/questions/30223079/why-does-stream-allmatch-return-true-for-an-empty-stream/30223427#30223427)
     
      
    - __`noneMatch()`__   
      - __Stream의 모든 요소가 조건을 만족하지 않으면 true, 만족한다면 false를 반환합니다.__   
      ```Java   
      boolean noneMatch = names.stream()
        .noneMatch(name -> name.endsWith("s"));   
      // true
      ```   
      - __선택된 요소가 null일 경우에는 `NullPointerException`을 일으킵니다.__   
      - __`allMatch()`와 마찬가지로 Stream이 비어있을 경우 Vacuous Truth로 인하여 `true`를 반환하게 됩니다.__   
      ```Java   
      List<String> names = new ArrayList<>();
      boolean noneMatch = names.stream()
        .noneMatch(name -> name.length() > 3);   
      // true 
      // empty Stream일 경우 noneMatch() == allMatch()   
    
  - __Iterating__   
    - __`forEach()`__   
      - __Stream의 모든 요소를 돌면서 각 요소마다 실행하는 최종 연산으로 보통 `System.out.println`을 넘겨서 사용합니다.__   
        - 앞서 살펴본 `peek()`과 비슷하지만 `peek()`은 중간 연산, `forEach()`는 최종 연산이라는 차이점이 있습니다.    
      ```Java   
      names.stream().forEach(System.out::println);
      ```       
- __참조__   
  - [Java 스트림 Stream (1) 총정리](https://futurecreator.github.io/2018/08/26/java-8-streams/)   
  - [Java 8 스트림 튜토리얼](https://wraithkim.wordpress.com/2017/04/13/java-8-%EC%8A%A4%ED%8A%B8%EB%A6%BC-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC/)   
  - [Do it! 자바 프로그래밍 입문 강의 - 내부 클래스 - 스트림](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8)   
