## 🍀 소개
`maxhyunm`와 `kyungmin`이 만든 `은행창구 매니저`입니다. 
> 한 명의 은행원이 10명~30명 사이의 고객의 업무를 처리하는 은행창구 매니저 콘솔앱
</br>

## 목차📌</br>
1. [팀원소개 🧑‍💻](#1.)
2. [타임라인 📆](#2.)
3. [File Tree 🗂](#3.)
4. [실행화면 🖥️](#4.)
5. [트러블슈팅 🧨](#5.)
6. [참고자료 📚](#6.)

</br>

<a id="1."></a>
## 팀원소개 🧑‍💻
|<Img src="https://cdn.discordapp.com/attachments/1100965172086046891/1108927085713563708/admin.jpeg" width="200">|<img src="https://hackmd.io/_uploads/rk62zRiun.png" width="200"/>|
|:-:|:-:|
|[**kyungmin**](https://github.com/YaRkyungmin)|[**maxhyunm**](https://github.com/maxhyunm)|

<a id="2."></a>
## 타임라인 📆
<table>
    <tr>
        <td align="center"><b>날짜</b></td>
        <td align="center"><b>작업내용</b></td>
    </tr>
    <tr>
        <td align="center">7/10(월)</td>
        <td>
            <b>STEP 1</b><br/>
            ✓ QueueType, CustomerQueue 구현<br/>
            <font size="2">
                : isEmpty(), enqueue(), dequeue(), clear(), peek() 메서드 추가
            </font><br/>
            ✓ Node 구현<br/>
            ✓ MockCustomerQueue 및 Int 테스트 구현</td>
    </tr>
    <tr>
        <td align="center">7/11(화)</td>
        <td>
            <b>STEP 1</b><br/>
            ✓ LinkedList 구현하여 Queue 분리<br/>
            <font size="2">
                : isEmpty(), append(), pop(), removeAll(), peek() 메서드 추가
            </font><br/>
            ✓ String 테스트 구현<br/>
            ✓ 테스트 타겟 설정 및 code coverage 오류 수정<br/>
        </td>
    </tr>
    <tr>
        <td align="center">7/12(수)</td>
        <td>
            <b>STEP 1</b><br/>
            ✓ Node의 Element에 Equatable 추가<br/>
            ✓ isEmpty() 연산 프로퍼티로 변경<br/><br/>
            <b>STEP 2</b><br/>
            ✓ Customer, Bank, Menu, BankManager 타입 구현<br/>
            ✓ Bank 기능 구현<br/>
            ✓ Namespace, Configuration 분리<br/>
            <font size="2">
                : BankNamespace, BankConfiguration, CustomerConfiguration 타입 생성
            </font><br/>
        </td>
    </tr>
    <tr>
        <td align="center">7/13(목)</td>
        <td>
            <b>STEP 2</b><br/>
            ✓ Namespace, Configuration Nested 처리<br/>
        </td>
    </tr>
    <tr>
        <td align="center">7/14(금)</td>
        <td>
            <b>STEP 3</b><br/>
            ✓ <br/>
            <b>README 작성</b>
        </td>
    </tr>
</table>

<a id="3."></a>
## File Tree 🗂
    ├── BankManagerConsoleApp
    │   ├── BankManagerConsoleApp
    │   │   ├── Bank
    │   │   │   ├── Bank.swift
    │   │   │   ├── BankManager.swift
    │   │   │   └── Menu.swift
    │   │   ├── Customer
    │   │   │   └── Customer.swift
    │   │   ├── LinkedList
    │   │   │   ├── LinkedList.swift
    │   │   │   └── Node.swift
    │   │   ├── Queue
    │   │   │   ├── CustomerQueue.swift
    │   │   │   └── QueueType.swift
    │   │   └── main.swift
    │   └── BankManagerConsoleTest
    │       ├── BankManagerConsoleIntTest.swift
    │       ├── BankManagerConsoleStringTest.swift
    │       └── MockCustomerQueue.swift
    └── README.md

<a id="4."></a>
## 실행화면 🖥️
1️⃣ **은행개점 선택시**<br/>

<img src="https://hackmd.io/_uploads/SJwIE4CF3.gif" width="500">
<br/>

2️⃣ **종료 선택시**<br/>

<img src="https://hackmd.io/_uploads/rkbwNVCFh.gif" width="500">
<br/>

<a id="5."></a>
## 트러블슈팅 🧨

### 1️⃣ OperationQueue 활용
#### DispatchQueue vs OperationQueue
🚨 저희는 요구사항의 은행직원이 작업용 `Queue`의 스레드 개수를 의미한다고 생각하였습니다. STEP 2에서는 은행직원이 1명이라고 되어 있지만, 이 부분은 얼마든지 늘어날 수 있는 개념이라고 생각했습니다. 이를 위해 동시성 `Queue`를 활용해야겠다는 생각을 했고 `DispatchQueue`와 `OperationQueue` 중에 어떤 것을 활용하면 좋을지 고민을 하였습니다.

💡 두 방식 모두 저희가 원하는 내용을 충족할 수 있고, 은행직원 매니저 앱에서 필요로하는 작업내용을 구현하는데 있어서 큰 차이가 없다고 생각되었습니다. 하지만 `DispatchQueue`는 `Queue`를 생성할 때 `Serial`과 `Concurrent` 여부에 따라 다른 설정이 들어가는 부분이 있고, `OperationQueue`의 경우 `maxConcurrentOperationCount` 프로퍼티 하나로 스레드의 갯수 및 `Serial`, `Concurrent` 여부를 설정할 수 있기에 `Thread` 개수에 따른 분기점 처리에 있어 `OperationQueue`가 조금 더 활용성이 좋을 것 같다고 판단되었습니다.

#### waitUntilAllOperationsAreFinished()
🚨 `OperationQueue`가 진행되는 중에 다른 코드로 넘어갈 수 있도록 설정되면, 프로그램이 정상적으로 실행->종료의 루틴을 거치지 않고 은행업무 처리 중에 업무 종료 메시지가 먼저 출력이 되어버리는 문제가 발생했습니다. 

💡 `OperationQueue`에 등록된 모든 `Task`를 종료하기 전까지 해당 스레드에 접근이 불가하도록(모든 `Task`가 종료된 후에 다음 코드를 진행할 수 있도록) 만들기 위하여 `waitUntilAllOperationsAreFinished()` 메서드를 사용하였습니다.


### 2️⃣ CustomerQueue 타입 구현
처음에는 `CustomerQueue`라는 단일 타입으로 `Linked-list` 방식의 `Queue`를 구현하였습니다.

```swift
struct CustomerQueue<Element>: QueueType {
private var headNode: Node<Element>?
private var tailNode: Node<Element>?

mutating func enqueue(_ value: Element) { ... }

mutating func dequeue() -> Element? { ... }

mutating func clear() { ... }

func peek() -> Element? { ... }

func isEmpty() -> Bool { ... }
}
```

🚨 하지만 요구사항을 자세히 살펴보니 아래와 같이 정리되어 있었습니다.

> Queue 타입 구현을 위한 Linked-list 타입을 직접 구현합니다.

위의 내용으로 짐작해 보았을 때, `Queue`와 `Linked-list` 타입이 구분되어있는 것이 맞다고 판단되었습니다.

💡 `CustomerQueue` 타입과 `LinkedList` 타입을 분리하였습니다. 기존 `CustomerQueue`에 정리되어 있던 `Node` 정보와 로직들은 `LinkedList`로 이동하였고, `CustomerQueue`에서는 각 메서드를 통해 필요한 로직을 호출할 수 있도록 하였습니다.

```swift
struct LinkedList<Element> {
    private var headNode: Node<Element>?
    private var tailNode: Node<Element>?

    mutating func append(_ value: Element) { ... }
    
    mutating func pop() -> Element? { ... }
    
    mutating func removeAll() { ... }
    
    func peek() -> Element? { ... }
    
    func isEmpty() -> Bool { ... }
}
```

```swift
struct CustomerQueue<Element>: QueueType {
    private var linkedList = LinkedList<Element>()
    
    mutating func enqueue(_ value: Element) {
        linkedList.append(value)
    }
    ...
}
```

### 3️⃣ CustomerQueue 내부 프로퍼티의 접근제어
🚨 `CustomerQueue` 타입에는 `private` 프로퍼티로 `LinkedList`가 포함됩니다. 하지만 `private` 프로퍼티는 테스트를 진행할 때 값에 접근할 수 없어 문제가 발생합니다.

💡 고민 끝에 이 부분은 `Queue`에 대한 테스트용 `Mock` 타입을 생성하는 쪽으로 방향을 잡았습니다. `Mock` 타입 생성을 위해서는 해당 `Queue`의 기본적인 내용이 추상화되어있는 편이 좋을 것이라는 결론을 내렸고, 이에 따라 `QueueType`이라는 프로토콜을 만들어 실제 프로젝트에 쓰일 `CustomerQueue`와 테스트용인 `MockCustomerQueue` 양쪽에서 모두 `QueueType`을 채택하도록 구현하였습니다.
`MockeCustomerQueue`에서는 추가로 `headNode`와 `tailNode`라는 연산 프로퍼티를 구현하여 `LinkedList`의 `firstNode`와 `lastNode`를 리턴하도록 했습니다.
```swift
var headNode: Node<Element>? {
    return linkedList.headNode
}

var tailNode: Node<Element>? {
    return linkedList.tailNode
}
```

### 4️⃣ Namespace 구현
#### 매직리터럴 제거
은행업무 매니저 앱에는 메뉴 선택, 고객별 업무 시작과 종료, 일일 업무 종료 내용이 메시지로 출력됩니다.
![](https://hackmd.io/_uploads/HkXtIVRt2.jpg)
![](https://hackmd.io/_uploads/SkwKLNCth.jpg)
![](https://hackmd.io/_uploads/H1itL4Rtn.jpg)

🚨 이러한 출력 내용을 매직리터럴로 처리하면 아래와 같은 형태로 구현됩니다.
```swift
print("업무가 마감되었습니다. 오늘 업무를 처리한 고객은 총 \(dailyTotalCustomer)명이며, 총 업무시간은 \(dailyBusinessHour)초입니다.")
```

💡 이는 올바른 방향이 아니라고 판단되어, 메뉴 선택 내용은 `Menu` 타입으로 분리하였고 그 밖의 출력 관련 내용을 모두 모아 `BankNamespace` 열거형으로 묶어주었습니다. 또한 `Namespace`로 정의된 문자열 내부에 특정 변수들을 추가해주기 위해 `String Format Specifiers`를 활용하였습니다.

```swift
enum Menu: String, CaseIterable {
    case open = "은행개점"
    case finish = "종료"

    var menuNumber: String {
        switch self {
        case .open:
            return "1"
        case .finish:
            return "2"
        }
    }
    
    static func displayMenu() {
        Self.allCases.forEach { print("\($0.menuNumber) : \($0.rawValue)") }
    }
}
```
```swift
enum BankNamespace {
    static let inputLabel = "입력 : "
    static let inputTerminater = ""
    static let startTask = "%d번 고객 %@업무 시작"
    static let endTask = "%d번 고객 %@업무 완료"
    static let closingMessage = "업무가 마감되었습니다. 오늘 업무를 처리한 고객은 총 %d명이며, 총 업무시간은 %@초입니다."
}
```

#### Namespace 위치

🚨 `Namespace`를 하나의 파일로 분리하고 보니, 이 파일이 따로 존재해야하는지 아니면 각 `Namespace`가 사용되는 타입 안에서 `Nested` 타입으로 가지고 있어야 하는지에 대하여 고민이 되었습니다.

💡 하나의 파일로 `Namespace`를 모두 모아주게 되면 어디서 사용되는 내용인지 파악이 어려워져서 관리적 측면에서 좋지 않다고 판단되었습니다. 따라서 각 `Namespace`들이 활용되는 위치에서 `extension`을 통해 `Nested` 타입으로 구현하였습니다.

```swift
extension Bank {
    enum Namespace {
        static let startTask = "%d번 고객 %@업무 시작"
        static let endTask = "%d번 고객 %@업무 완료"
        static let closingMessage = "업무가 마감되었습니다. 오늘 업무를 처리한 고객은 총 %d명이며, 총 업무시간은 %@초입니다."
    }
}
```

```swift
extension BankManager {
    enum Namespace {
        static let inputLabel = "입력 : "
        static let inputTerminater = ""
    }
}
```

### 5️⃣ 테스트 빌드를 위한 Target 설정
🚨 `Test`를 생성 후 실행을 하면 올바른 `Target`을 찾지 못하여 계속 빌드에 실패하였습니다. 원인을 찾아보니 `Command Line Tool` 프로젝트에서는 `TargetMembership` 설정을 따로 진행해주어야 한다는 내용을 발견하였습니다.

💡 `File Inspector`에서 프로젝트 내부 파일들의 `TargetMembership` 설정 부분에 `App`, `Test` 두 가지를 모두 체크해주었고 그 후로 문제없이 빌드되었습니다.

---
<a id="6."></a>
## 참고자료 📚
- [🐻야곰 닷넷: 동시성 프로그래밍](https://yagom.net/courses/동시성-프로그래밍-concurrency-programming/)
- [🍎Apple Docs: Generics](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/generics/)
- [🍎Apple Docs: Protocols](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/protocols)
- [🍎Apple Docs: String Format Specifiers](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html)


