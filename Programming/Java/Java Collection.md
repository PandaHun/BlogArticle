Java의 Collection에는 크게 4가지로 분류할 수 있다.

하지만 일반적으로 3가지 타입이 존재한다고 말한다.

1. List
2. Set
3. Map
4. (Queue)

https://t1.daumcdn.net/cfile/tistory/99B88F3E5AC70FB419

JAVA의 컬렉션 프레임워크(JCF)

Collection 인터페이스는 List, Set, Queue로 분류되고, Map은 Collection 인터페이스에 포함되지 않지만 Collection으로 분류된다.

------

### 컬렉션 프레임워크의 특징

- List: 순서가 있는 데이터의 집합으로 중복을 허용한다.
  - ArrayList: 자바 기존의 Vector를 개선한 배열로 구현된 List. 배열로 구현된 만큼 속도 또한 배열과 동일하다. 다만, 크기가 지정되지 않아 편하게 크기를 조정할 수 있다.
  - LinkedList: 다음 주소를 기억하고 있는 List. 배열에 비해 삭제와 삽입이 간단하다. 하지만, 탐색의 경우 첫 노드부터 탐색하기 때문에 탐색의 속도가 느리다. Queue, Stack등 다양한 자료구조를 만들 때 사용된다.
- Set: 순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않는다.
  - HashSet: HashMap에서 Key가 없는 형태라 봐도 무방하다. 속도적인 측면에서 매우 유리해 자주 사용하는 형태
  - TreeSet: Red-Black 트리로 이뤄진 자료구조 정렬 방법을 지정할 수 있다.
- Map: Key와 Value의 쌍으로 이루어진 데이터의 집합으로 순서는 유지되지 않고, Key의 중복은 허용하지 않지만 Value의 중복은 허용한다
  - HashMap: 가장 일반적인 Map. Hahs Table을 이용해 나온 Key의 값을 index로 사용한다. null을 허용한다.
  - TreeMap: Red-Black Tree를 활용한 Map. 어느 정도 순서를 보장한다.
  - HashTable: HashMap보다 느리지만 동기화를 지원하고, null을 허용하지 않는다.