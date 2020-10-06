<br/>

# 🌈 객체 지향 프로그래밍 이란?

<br/>

## ✅ 객체 지향 기법의 구성 요소

<br/>

### 1. 객체(Object) : 데이터와 데이터를 처리하는 함수를 묶어 놓은(캡슐화) 하나의 소프트웨어 모듈

- 데이터(Data) : 객체가 가지고 있는 속성
- 함수(Method) : 객체가 수행하는 기능. 객체가 갖는 데이터를 처리하는 알고리즘

### 예시 )

- 객체 = 심장
- 데이터 = 혈액
- 함수 = 대동맥, 정맥으로 피를 보내는 기능

<br/>

### 2. 클래스(Class) : 공통된 속성과 연산(행위)를 갖는 객체의 집합. 각각의 개체들이 갖는 속성과 연산을 정의하고 있는 틀

- 인스턴스(Instance) : 클래스에 속학 각각의 개체.
- 인스턴스화(Instantiaion) : 클래스로부터 새로운 객체를 생성하는 것

### 예시 )

- 클래스 : 호흡기관
- 인스턴스 : 폐, 기도
- 인스턴스화 : 호흡기관에 추가적으로 넣는 과정

<br/>

### 3. 메시지(Message) : 객체들 간에 상호작용을 하는데 사용 되는 수단. 객체에게 어떤 행위를 하도록 지시하는 명령 또는 요구 사항

### 예시 )

- 메세지 : 뉴런( 신호 전달 )

<br/>
<br/>
<br/>

# 🌈 [자료 구조] Stack & Queue & Linked List

자료 구조는 Coding과는 별로 상관없어 보일 수 있겠지만 프로그래밍에 있어서 매우 중요한 부분이다.

- 내가 어떠한 프로그램을 구현 할 때, 혹은 어떤 알고리즘 문제를 해결 하거나 새로운 Logic을 만들 때
- 데이터를 어떤 구조로 저장 하느냐에 따라서 프로그램의 효율성이나 성능 면에서 차이가 날 수 있다.

<br/>

## ✅ 1. Stack

- Stack은 자료를 한 줄로 블록을 쌓듯이 추가하는 형태의 자료 구조 입니다.
- Stack 맨 위에 자료를 `추가` 하는 것을 `Push` 라고 하고
- 반대로 맨 위의 자료를 하나씩 `제거`하는 것(`꺼내는 것`)을 `Pop` 이라고 합니다.

- 위 그림처럼 Stack은 어느 한 곳에 Block 처럼 자료가 쌓기 때문에
- 자료를 제거 할 때 가장 마지막에 들어간 자료가 가장 먼저 제거 됩니다. (LIFO : Last-in First-Out)

<br/>
<br/>

## ✅ Big O(시간 복잡도)

- 우리가 Stack을 통해 자료를 삽입, 삭제, 검색의 과정을 거친다고 생각해 볼 때
- 컴퓨터에서는 이 과정이 얼마나 걸리는지 정확히 알 수 없습니다.
- 그래서 우리는 어떤 연산을 하는데 몇 차례의 과정이 진행되는지 에 따라
- 소요 시간과 효율성을 측정 할 수 있습니다.
- 이 때 연산 과정을 간편하게 표기해서 쓸 수 있는 수학적인 표기법을 Big O라고 부릅니다

- O(1)의 표현에서 1은 상수 시간을 말합니다.
- 예를 들어 Stack에 자료를 추가 할 경우 항상 Stack의 맨 위에 자료를 Push 하기만 하면 됩니다.
- 즉 자료의 개수가 몇 개 인지 혹은 다른 어떠한 사항도 고려할 필요가 없습니다.
- 그저 자료를 기존의 자료 맨 위에 올리기만 하면 된다는 뜻입니다.
- 그러므로 한번의 Operation으로 자료의 삽입이 수행 되기 때문에 O(1)이라고 표현 할 수 있습니다.
- 단 O(1)은 한 번만 연산이 된다는 의미가 아닙니다.
- 어떤 작업을 수행할 때 마다 항상 동일한 작업만을 수행한다면 작업들을 O(1)로 표현 할 수 있습니다.
- 마찬가지로 자료를 삭제 할 때도 자료의 개수와 상관없이 맨 위에 있는 자료를 Pop하면 되기 때문에
- 자료의 삭제도 O(1)로 표기 할 수 있습니다.

- 그렇다면 자료의 검색은 어떨 까요?
- 만약 Stack에 5개의 블록이 쌓여 있다면 우리는 맨 위의 Block에서 부터 맨 마지막 Block까지 순차적으로 검색하여 자료를 찾아야 합니다.
- 운이 좋게 첫 번째 Block이 우리가 찾는 Block이 있다면 한 번의 검색으로 답을 찾을 수 있겠지만
- 만약에 맨 마지막 Block이 우리가 찾는 Block이라면 우리는 맨 마지막 Block이 나올 때 까지 5번의 검색 과정을 거쳐야 합니다.
- 그러므로 Stack의 검색 횟수는 가장 최악의 경우인 O(n)으로 표기 할 수 있습니다.

<br/>

### ⭐️Stack 예시

- Call Stack : JS는 모든 함수 호출을 Stack 자료 구조로 이루어져 있습니다. 특히 재귀 함수를 사용 할 때 가장 중요하게 다뤄진다.
- Undo / Redo Mechanism : 포토샵의 기능 중 Ctrl+Alt+Z를 눌러 방금 전의 작업들을 뒤에서 부터 순차적으로 취소 할 수 있다. 우리가 실행한 작업들이 순차적으로 Stack 자료 구조로 저장되고, 취소 단축키를 누를 때 마다 맨 위의 자료들이 삭제 되기 때문이다.

<br/>
<br/>
<br/>

## ✅ 3. Linked List(연결 리스트)

- Linked List는 여러 노드들이 연결되어 있는 구조입니다.
- 연결 리스트의 맨 앞을 Head, 맨 마지막을 Tail이라고 합니다.
- 노드들은 다음에 올 노드의 정보를 가지고 있다.

- 만약 연결 리스트에 자료를 삽입하려고 한다면 우리는 위 그림과 같이 자료를 추가할 장소로 가서
- 기존의 링크를 끊은 다음 추가할 위치의 이 전 요소의 꼬리와 삽입할 자료의 꼬리를 연결해야 합니다.
- 그리고 추가할 요소의 꼬리와 다음 요소의 꼬리를 연결해 주면 됩니다.
- 이 때에 오직 앞 요소와 다음 요소와 연결시켜주는 Operation만 수행 되므로 시간 복잡도는 O(1)입니다.

- 만약에 연결 리스트에 특정 자료를 삭제하려고 한다면 위의 그림처럼 삭제하고자 하는 요소의 이전 요소의 꼬리를 다음 요소와 연결 시켜 주면 됩니다.
- 마찬 가지로 삭제하고자 하는 요소의 이전 요소와 다음 요소를 연결시켜주는 Operation이 수행 되므로 시간 복잡도는 O(1)입니다.
- 또한 연결 리스트에서 자료를 검색하고 싶다면 맨 앞의 Head에서 부터 시작해야 합니다.
- 왜냐하면 연결 리스트의 각 노드들은 다음 요소들의 정보를 가지고 있기 때문입니다.
- 그러므로 맨 앞의 리스트 에서 부터 그 다음의 노들로 순차적으로 검색해야 합니다.
- 이 때의 시간 복잡도는 O(n)입니다.

<br/>

### ⭐️Stack 예시

- 웹 브라우저에서 여러 사이트들을 실행하고 난 후 History는 연결 리스트와 같은 구조로 저장됩니다.
- 내가 어떤 사이트들에서 다른 사이트로 넘어갈 때, 이전 사이트의 정보를 가지고 넘어가기 때문입니다.
- 따라서 우리가 브라우저에서 이전 버튼, 다음 버튼, 누르면 이전에, 혹은 다음에 열람 했던 사이트들로 이동 할 수 있습니다.

<br/>
<br/>
<br/>

# 🌈 [자료 구조] Data Structure - Hash Table

- 만약에 친구들 이름과 전화 번호 List 가 있다고 생각해 봅시다.
- 이제 친구들의 이름, 전화번호 Data를 저장해 두었다가 필요 할 때마다
- 친구들의 이름을 입력해서 전화번호 Data를 찾으려고 합니다.
- 이 때 친구들의 전화번호를 가장 빠르게 찾기 위한 방법 중 하나가 바로 `Hash Table`입니다.
- Hash Table을 구성하는 데 Hash Function이 필 수 입니다.
- Hash Function은 친구 이름과 같은 문자열을 받아서 고유한 숫자 키로 변경 해주는 함수 입니다.
- 먼저 10개의 Data를 넣을 경우 10개의 공간을 준비 합니다.
- 각 친구 이름들을 차례 대로 Hash Function을 통해 고유한 숫자 키를 받아 냅니다.
- 예를 들어 Freeko의 숫자 키는 3, Ria의 숫자 키는 1이라고 해봅시다.
- 이 때의 숫자 키 값은 Data 공간의 주소 값이 됩니다.
- 그럼 이제 아까 준비한 공간에 숫자 키를 방 번호로 삼아 전화번호 Data를 집어 넣습니다.
- 이제 내가 Freeko의 전화번호를 알고 싶다면 복잡하게 Loop을 돌리거나 할 필요 없이 단지 Freeko라는 이름을 Hash Function에 넣고 숫자 키를 알아낸 다음 그 방 번호의 Data를 확인하면 아주 쉽게 Freeko의 전화번호를 알아 낼 수 있습니다.

- 즉 `Hash Table` 이란 위의 예시에서 살펴 보았듯이 Hash Function을 사용하여 키 값을 Data 공간의 주소로 변환 하고 해당 주소에 키 값에 대응하는 Data를 넣는 자료 구조를 말합니다.

<br/>

## ✅ Hash Table 장점

- 중복 Data를 찾아 내기 쉬움
- 빠른 탐색, 삽입, 삭제 속도
- 어떤 항목과 다른 항복의 관계를 모형화 하기 좋음

—> 단순 List에 저장한 Data는 중복된 Data를 찾기 위해서 단순 탐색을 계속해서 반복 해야 하기 때문에 성능이 매우 좋지 않습니다. 그러나 Hash Table에 저장하면 Data가 있는지 순간적으로 알려주기 때문에 중복 확인하는 것이 매우 쉬워 집니다.

<br/>

## ✅ Hash Table 단점

- 공간 효율성이 낮음 ( 만약 10개의 자료를 넣기 위해 미리 빈 공간을 만든 다음 2개의 Data만 넣는 다면 8개의 공간이 빈 상태가 됩니다. 또는 11개의 공간을 넣게 되면 overflow가 발생하기 때문에 Data 공간을 크게 늘리고 기존의 Data를 다시 Hashing 해야 합니다.

<br/>

## ✅ Collision

- 충돌이 발생하기 전까지 Hash Table은 성능이 매우 뛰어난 자료 구조 입니다.
- 충돌이란 다른 키 2개를 Hash Function에 넣었는데 같은 주소 값이 나오는 경우를 말합니다.
- 많은 양의 Data를 처리하다 보면 충돌이 발생하는 경우가 생길 수 밖에 없습니다.

<br/>

### ⭐️ Collision 해결 방법 1 : Linked List

- 만약 같은 주소가 나왔을 때 해당 주소에 이미 Data가 있다면 그 Data에 연결 리스트로 연결 합니다.
- 예를 들어 알파벳을 각각 0 ~ 25의 숫자에 대응시킨 후,
- 영어 단어를 입력 하면 맨 앞 글자에 따라 해당 숫자를 Output으로 넘겨주는 Hashing Function 있다고 생각을 해보겠습니다.
- 위 Hash Function을 사용하여 전화번호 Hash Table을 만들면 R로 시작하는 이름을 가진 친구는 전부 같은 주소 값을 갖게 될 것 입니다.
- 이 경우에 해당 주소에 R로 시작하는 친구의 Data를 연결 리스트로 모두 연결하면 됩니다.
- 이 방법의 단점은 충돌 횟수가 늘어나 연결 리스트의 길이가 길어 질 수록 Hash Table의 성능이 매우 낮아진다는 것 입니다.
- Hash Table은 Data를 빨리 찾고 삭제하고 입력 할 수 있는 장점이 있는데 연결 리스트가 길어지면 연결 리스트를 순회하는 과정 탓에 성능이 떨어지게 됩니다.

<br/>

### ⭐️ Collision 해결 방법 2 : Linear Probing(선형 탐사)

- 선형 탐사는 충돌이 발생한 지점의 다음 방 부터 탐사하여 빈 방이 있으면 그 공간에 Data를 집어 넣는 방법입니다.
- 만약 끝까지 탐색 했는데 빈 공간이 없다면 맨 처음으로 돌아가 다시 검사합니다.
- 매우 간단 하지만 만약에 주변에 Data 공간이 비어 있지 않다면 빈 공간을 찾기 위해 모든 공간을 탐색해야 하는 단점이 있습니다.

<br/>

## ✅ Big O

- 삽입 ( Insertion ) : O(1)
- 삭제 ( Deletion ) : O(1)
- 탐색 ( Search ) : O(1)
- Data를 추가 할 때, 그저 Hash Function에 Key를 넣어 주소를 알아 낸 후 해당 주소에 Data를 삽입하면 되기 때문에 O(1)의 상수 시간으로 표기 할 수 있습니다. Data를 검색, 삭제할 때도 마찬 가지 입니다.

### ‼️여기서 상수 시간(Constant Time) 이란

- 순간적 혹은 연산이 한 번만 일어 난다는 것이 아닙니다.
- 항상 똑같은 시간이 걸린다는 뜻 입니다.
- 예를 들어 단순한 List에 어떤 Data를 단순 탐색으로 찾으려면 앞에서 부터 검색을 해야 하기 때문에 O(n) 만큼 소요 됩니다.
- 이 때의 n은 List의 길이를 뜻합니다. 만약 이진 탐색을 한다면 좀 더 빠르게 log 시간이 걸릴 것 입니다.
- 그러나 Hash Table에서 Data를 찾으면 Table의 Data가 1개든 5개든 100개든 크기와 아무 상관 없이 Data 하나를 꺼내는 시간은 모두 동일 합니다. 그러므로 평균적으로 Hash Table은 아주 빠릅니다.
- 하지만 최악의 경우에 Hash Table은 Data의 탐색, 삽입, 삭제가 모두 O(n)만큼의 시간이 소요 됩니다. 최악의 상황이 발생하지 않으려면 충돌을 최대한 피해야 하는데, 그러기 위해 좋은 Hash Funtion와 낮은 사용률이 필요 합니다.

<br/>

## ✅ Load Factor(사용률)

- Hash Table에서 사용률이란 Hash Table의 공간을 얼마나 사용하고 있는지 수식으로 나타낸 것입니다.

- 만약에 배열에서 5개의 방이 있는데 그 중 2개의 공간에 Data가 저장되어 있다면 2 / 5 = 0.4가 사용률 됩니다.
- 그리고 100개의 Data가 있는데 공간이 50개 밖에 없다면 사용률은 2가 되고 절반의 Data는 공간이 없게 됩니다. 즉, 사용률이 1보다 크다는 것은 Hash Table의 공간이 부족하다는 뜻 입니다.
- 대개의 경우 사용률이 0.7 이상으로 커지면 Hash Table을 Resizing하는데 대략 2배 정도의 크기로 배열을 만듭니다.
- Resizing을 하면 배열을 크게 만들고 나면 Hash Function을 사용하여 기존 Table에 있던 Data를 새로운 Hash Table에 다시 저장해야 합니다. 이 과정은 시간 매우 오래 걸리기 때문에 Resizing을 자주 하는 것은 좋지 않습니다.

<br/>

## ✅ Hashing

- Hashing이란 어떤 길이든지 상관 없이 input string을 받아 고정된 길이의 결과물로 만들어 주는 것입니다.
- 비트코인도 SHA-256이란 Hashing 알고리즘을 사용합니다.

<br/>

## ✅ Hash Function

- Hash Function은 반드시 동일한 input이 들어가면 항상 동일한 output이 나와야 합니다.
- Hash Function를 사용 할 때마다 매번 다른 결과가 나온다면 그 Hash Table은 사용할 수 없을 것 입니다.
- Hash Function은 값이 골고루 분포되어야 합니다. 만약 한 쪽으로 쏠려서 Data가 분배되면 충돌이 자주 발생합니다.
- Hash Function은 성능이 뛰어나야 합니다. Hash Function의 성능은 Hash Table의 성능과 직접적인 연관이 있기 때문입니다.

<br/>

## ✅ Real Life Use Cases

- 주소록
- 블록 체인
- JS 실행 엔진 ( 크롬, V8 )
- 웹 사이트를 찾기 위해서 웹사이트 주소를 브라우저에 입력 한다고 해보자. 우리의 컴퓨터는 우리가 입력한 주소를 IP 주소로 변환 한다. 밑에 처럼 말이다.

GOOGLE.COM - 74.125.239.133

FACEBOOK.COM - 173.252.120.6

- 이러한 과정은 DNS 확인 (DNS resolution) 작업이라고 하는 데 이러한 작업에도 Hash Table이 사용됩니다.

# 🌈 [자료 구조] Data Structure - Tree & Binary Search Tree

[[자료 구조] Data Structure - (3) Tree / Binary Search Tree](https://im-developer.tistory.com/129?category=828401)

## ✅ Tree

---

- 이름 그대로 나무를 거꾸로 뒤집어 놓은 듯한 형태 입니다.
- 자료 구조가 하나의 뿌리에서 뻗어나가는 형상을 하고 있습니다.
- Tree 구조는 일상에서 흔히 볼 수 있는 계층적 구조(Hierarchical Structure)를 컴퓨터에 표현한 구조 입니다.
- 컴퓨터의 파일 시스템이나 웹 페이지의 DOM 구조도 Tree 구조 입니다.

  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3c02241c-d850-4e4f-953a-7de7b82a4e57/_2020-10-02__8.41.07.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3c02241c-d850-4e4f-953a-7de7b82a4e57/_2020-10-02__8.41.07.png)

- 트리의 각 요소들은 Node라고 부르고 가장 상위의 Node를 Root Node라고 부릅니다.
- 반대로 최하위 Node는 Leaf Node 혹은 Terminal Node라고 합니다. (자식 Node가 없는 Tree 구조의 가장 하단에 있는 Node에 있는 Node들을 말합니다.)
- 한 Node에 연결된 서브 Tree의 개수를 차수라고 합니다.
- 예를 들어 B Node의 차수는 2개 입니다.
- 만약에 한 Tree 내의 차수가 모두 2개 이하라면 그 Tree를 Binary Tree라고 부릅니다.
- 현재 Node에 연결되어 있는 상위 Node는 Parent Node이며 그 하위의 자식 Node는 Child Node입니다.
- 같은 부모 Node를 갖는 Node는 형제 Node 즉, Sibling Node입니다.
- Level은 Root Node 부터 해당 Node 까지 경로를 찾는데 방문한 총 Node의 수를 말합니다.
- Tree의 최대 Level 수는 Tree의 Height 혹은 Depth이라고 합니다.
- 위의 그림은 Level 1부터 Level3가지 있고 Tree의 높이는 3입니다.

<br/>
<br/>
<br/>

## ✅ Binary Search Tree

- Tree 중에 가장 유명한 구조 중 하나인 Binary Search Tree(이진 탐색 트리)입니다.
- 이진 탐색 트리는 이진 트리 이기 때문에 모든 노드의 차수는 2개 이하여야 합니다.
- 그로 모든 왼쪽 자식 노드는 부모의 값보다 작은 값을 가져야 하고
- 모른 오른쪽 자식 노드는 부모의 값보다 큰 값을 가져야 한다는 특징이 있습니다.

  - 6, 2, 9, 5, 8, 3, 1이란 데이터를 이진 탐색 트리로 만들면 위의 그림처럼 됩니다.
  - 가장 먼저 들어간 6이 Root Node가 됩니다.
  - 2는 6보다 작기 때문에 6의 왼쪽 자식 노드가 됩니다.
  - 9는 6보다 크기 때문에 6의 오른쪽 자식 노드가 됩니다.
  - 5는 6보다 작고 2보다 크기 때문에 2의 오른쪽 자식 노드가 됩니다.
  - 8은 6보다 크고 9보다 작기 때문에 9의 왼쪽 자식 노드가 됩니다.

  <br
    />

## ✅ Big O - Average

---

- 이러한 규칙으로 이루어져 있기 때문에 이진 탐색 트리는 평균적으로 이진 탐색과 똑같이 빠른 시간 복잡도를 가집니다.
- Insertion : O(log n)
- Deletion : O(log n)
- Search : O(log n)

## ⭐️ Big O - worst case

- Data를 임의 접근 할 수 없습니다.
- 예를 들어서 밑의 트리의 6번째 원소를 찾고 싶다고 말할 수 없습니다.
- 또한 트리가 균형이 맞지 않는 최악의 경우에는 데이터의 삽입과 삭제 검색이 모두 O(n)만큼 소요됩니다.
- 이진 탐색 트리의 성능이 떨어지는 경우는 가 밑에 그림과 같습니다.
- 이렇게 데이터가 한쪽으로 치우쳐 있으면 연결 리스트와 별 차이가 없는 구조가 되어 이진 트리의 장점이 사라집니다.
- 이러한 이진 탐색 트리의 단점을 해결 하기 위해 AVL 트리, 레드-블랙 트리, 2-3 트리 등 여러 가지 트리 구조가 존재합니다.

# 🌈 [HTTP] Cross-Origin Resource Sharing(CORS)에 대하여

[[HTTP] Cross-Origin Resource Sharing (CORS)에 대하여](https://im-developer.tistory.com/165?category=828401)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/25fb90d0-31ed-4392-9125-a29f84ab7c5e/_2020-10-03__9.23.06.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/25fb90d0-31ed-4392-9125-a29f84ab7c5e/_2020-10-03__9.23.06.png)

- 프로그래밍 공부를 하다 보면 오픈 API에 요청 보내고 응답 받는 일을 굉장히 많이 하게 됩니다.
- 그러다 보면 CORS 에러도 필연적으로 만나게 되는데, 대체 CORS란 무엇일까요?

  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b591276-0b5b-4cf7-9046-1951d0a0b18e/_2020-10-03__9.23.52.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b591276-0b5b-4cf7-9046-1951d0a0b18e/_2020-10-03__9.23.52.png)

## ✅ Cross-Origin Resource Sharing (CORS)

---

- 많은 웹 어플리케이션들이 이미지 파일, 폰트, CSS 파일 같은 리소스들을 각각의 출처로 부터 읽어 옵니다.
- 만약 웹 어플리케이션이 자기 자신이 속하지 않은 다른 도메인, 다른 프로토콜, 혹은 다른 포트에 있는 리소스를 요청하면 웹 어플리케이션은 Cross-Origin HTTP 요청을 실행합니다.
- 만약에 Front-End JS의 Code가 [https://domain-a.com에서](https://domain-a.com에서) [https://domain-b.com/data.json으로](https://domain-b.com/data.json으로) json 데이터 요청을 보낸다면 이것이 바로 Cross-Origin HTTP 요청이 됩니다.
- 이 때 브라우저는 보안상의 이유로 스크립트 안에서 시작되는 Cross-Origin HTPP 요청들을 제한 합니다.
- 예를 들어 XTMLHttpRequest와 Fetch API는 동일 출처 원칙이 적용 됩니다.
- 이 말은 웹 어플리케이션이 자기가 로드 되었던 그 origin과 동일한 origin으로 리소스를 요청하는 것만 허용 한다는 뜻 이빈다.
- 다른 Origin으로 부터 응답이 올바른 CROS Header를 포함하지 않는 다면 말입니다.

- 정리를 하자면
- 다른 도메인의 img 파일이나 css파일을 가져오는 것은 모두 가능합니다.
- 그러나 `<script></script>` 로 감싸여진 script에서 생성된 Cross-Site HTTP Request는 Same-Origin Policy를 적용 받아 Cross-Site HTTP Request가 제한됩니다.
- 동일 출처로 요청을 보내는 것(Same-Origin Request)은 항상 허용되지만 다른 출처로의 요청(Cross-Origin Request)은 보안상의 이유로 제한된다는 뜻입니다.
- 그러나 AJAX가 널리 퍼지면서 `<script></script>` 안의 스크립트에서 생겨난 XMLHTTPRequest에서도 Cross-Site HTPP Request의 필요성이 매우 커졌습니다.
- 그러나 W3C에서 CROS라는 이름으로 새로운 표준을 내놓게 되었습니다.

- 즉 기존에 보안상의 이유로 XMLHttpRequest가 자신과 동일한 도메인으로만 HTTP 요청을 보내도록 제한하였으나 웹 개발에서 다른 도메인으로의 요청이 꼭 필요하게 되었습니다.
- 그래서 XMLHttpRequest가 Cross-Domain을 요청할 수 있도록 CROS라는 것이 만들어 졌습니다.

<br/>
<br/>
<br/>

## ✅ Preflight Request

- W3C 명세에 의하면 브라우저는 먼저 서버에 Preflight request(예비 요청)를 전송하여 실제 요청을 보내는 것이 안전한지 OPTIONS method로 확인합니다.
- 그리고 서버로 부터 유효하다는 응답을 받으면 그 다음 HTTP request 메소드와 함께 Actual request(본 요청)을 보냅니다.
- 만약 유효하지 않다면 에러를 발생시키고 실제 요청은 서버로 전송하지 않습니다.
- 이러한 예비 요청과 본 요청에 대한 서버의 응답은 프로그래머가 구분지어 처리하는 것이 아닙니다.
- 프로그래머가 Access-Control- 계열의 Response Header만 적절히 정해주면 Options 요청으로 오는 예비 요청과 GET, POST, PUT, DELETE 등으로 오는 본 요청의 처리는 서버가 알아서 처리 합니다.
- Preflight request는 GET, HEAD, POST외 다른 방식으로도 요청을 보낼 수 있고 Application/xml 처럼 다른 Content-type으로 요청을 보낼 수도 있으며, Custom Header도 사용 할 수 있습니다.

<br/>

## ✅ Simple Request

---

- 어떤 요청들은 CROS Preflight Request(사전 요청)을 발생시키지 않습니다.
- 보통 이런 요청들을 Simple Request라고 합니다.
- Simple Request의 경우에는 아래 3가지 조건이 모두 만족되는 경우를 말합니다.

  1. GET / HEAD / POST 중 한 가지 Method를 사용해야 한다.
  2. User agent에 의해 자동으로 설정되는 Header(Connection, User-Agent 또는 Fetch spec에서 'Forbidden header name'으로 정의된 Header를 제외하고, Fetch spec에서 'CORS-safelisted request-header'라고 정의되어 있는 수동 설정이 허용된 Header은 다음과 같다. ( Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width )
  3. 오직 아래의 Content Type만 지정해야 한다. ( application, x-www.form-urlencoded, multipart/form-data, text/plain )

    <br/>

## ✅ Request with Credential

---

- HTTP Cookie와 HTTP Authentication 정보를 인식할 수 있게 해주는 요청입니다.
- 기본적으로 브라우저는 Non credential로 설정되어 있기 때문에 credentials 전송을 위해선 설정을 해주어야 합니다.
- Simple Credential Request 요청 시에 xhr.withCredentials = true를 지정해서 Credential 요청을 보낼 수 있고, 서버는 Response Header에서 반드시 Access-Control-Allow-Credentials: true 를 포함해야 합니다.
- 또한 서버는 credential 요청에 응답할 때 반드시 Access-Control-Allow-Origin Header 값으로 "\*"와 같은 와일드 카드 대신 구체적인 도메인을 명시해야 합니다.

<br/>

## ✅ CORS 에러를 피하는 방법

---

- CORS는 브라우저가 사용하는 것 입니다.
- 그러므로 서버에서 서버로 보내는 요청은 CORS가 적용되지 않습니다.
- 그러므로 프록시 서버를 추가로 만들어서 클라이언트에서 우리가 새로 만든 프록시 서버로 요청을 보내고 프록시 서버에서 원하는 티켓 서버에 요청을 보내면 브라우저가 개입되지 않았기 때문에 CORS 오류를 회피 할 수 있습니다.

<br/>
<br/>
<br/>

# 🌈 [HTTP] HTTP Method 정리 : GET vs POST

<br/>

## ✅ GET

- GET Method는 주로 데이터를 일거나(Read) 검색(Retrieve)할 때 사용 되는 메소드 입니다.
- 만약에 GET 요청이 성공적으로 이루어 진다면 XML이나 JSON과 함께 200(Ok) HTTP 응답 코드를 리턴 합니다.
- 에러가 발생하면 주로 404(Not Found)에러나 400(Bad requset) 에러가 발생합니다.
- HTTP 명세에 의하면 GET 요청은 오로지 데이터를 읽을 때만 사용 되고 수정할 때는 사용하지 않습니다.
- 따라서 이러한 이유로 사용하면 안전하다고 간주 됩니다.
- 즉 데이터의 변형의 위험 없이 사용할 수 있다는 뜻 입니다.
- 게다가 GET 요청은 idempotent(연산을 여러 번 작용해도 결과가 달라지지 않음)합니다.
- 즉, 같은 요청을 여러번 하더라도 변함 없이 항상 같은 응답을 받을 수 있습니다.
- 그러므로 GET을 데이터를 변경하는 등의 안전하지 않은 연산에 사용하면 안됩니다.

<br/>

## ✅ POST

---

- POST 메소드는 주로 리소스를 생성(Create)할 때 사용됩니다.
- 조금 더 구체적으로 POST는 하위 리소스(부모 리소스의 하위 리소스)들을 생성할 때 사용됩니다.
- 성공적으로 Creation을 완료하면 201(Created) HTTP 응답을 반환합니다.
- POST 요청은 안전하지도 않고 idempotent하지도 않습니다.
- 다시 말해서 같은 POST 요청을 반복해서 했을 때 항상 같은 결과물이 나오는 것을 보장하지 않는 다는 것 입니다.
- 그러므로 두 개의 같은 POST 요청을 보내면 같은 정보를 담은 두 개의 다른 resource를 반환 할 가능성이 높습니다.

<br/>

## ✅ GET vs POST

---

- HTTP POST 요청은 클라이언트에서 서버로 전송할 때 추가적인 데이터를 Body에 포함 할 수 있습니다.
- 반면에 GET 요청은 모든 필요한 데이터를 URL에 포함하여 요청해야 합니다.
- HTML의 `<form>` 태그에 `method="POST" or method="GET"(기본값)` 을 모두 사용 할 수 있습니다.
- 만약에 GET 메소드를 사용하면 모든 form data는 URL로 인코딩되어 action URL에 query string parameters로 전달됩니다.
- POST 메소드를 사용하면 form data는 HTTP request의 message body에 나타날 것 입니다.
