***
# Swift Style Guide
Mobidoo iOS팀의 Swift 사용 가이드라인 입니다. 추후 구성원들의 논의를 거쳐 지속적으로 업데이트 될 예정입니다.
우리는 협업 시에도 같은 대상에 대해 조금씩 다른 단어 선택을 할 때가 많습니다. 다른 사람이 작성한 코드를 읽을 때에는 더더욱 이 단어가 정확히 어떤 것을 가리키는 의미로 사용되었는지 고민해야 할 때가 많습니다. 우리는 이러한 과정 중에 더욱 시간과 노력을 줄이고 생산성 향상을 위해 최소한의 장치로 Swift 언어 사용에 대한 규칙을 정했습니다. 
***

## 목차

1. [Naming Convention](#NamingConvention)
    1. [Constant, Variable, Property](#Constant/Variable/Property)
    2. [Function, Method](#Function-Method)
    3. [Enum(열거형)](#열거형)
    4. [Class & Struct](#)
    5. [Protocol](#Protocol)
    6. [Delegate](#Delegate)
2. [Comment](#주석)
3. [띄어쓰기](#띄어쓰기)
4. 코드 구성
   1. 미사용 코드
5. Access Control
6. Class & Struct 
7. 함수호출
8. [클로져](#클로져)
    1. [후행 클로저 축약](#후행-클로저-축약)
    2. [다중 후행 클로져](#다중-후행-클로져)
9. 타입
    1. [타입 추론](#타입-추론)
    2. [타입 어노테이션](#타입-어노테이션)
10. [메모리 관리](#메모리-관리)
11. [파일관리](#파일관리)


## NamingConvention
### Constant/Variable/Property
- 변수 이름은 `lowerCamelCase`를 사용해주세요.
- 배열과 같이 복수의 의미를 담고있는 변수라면 끝에 **s**를 붙여서 사용해주세요.
- Bool 타입 변수 작명은 [이곳](https://soojin.ro/blog/naming-boolean-variables)에서 설명하는 대로 따라주세요.
  <details>
  <summary>예제코드</summary>

  - **Good ✅**
    ```swift
    let categories: [String]
    var person: Person
    var isShowing: Bool
    ```
  - **Bad ❌**
    ```swift
    let category: [String]
    var show: Bool
    ```
  </details>
  
### Function-Method
- 함수 이름에는 `lowerCamelCase`를 사용해주세요.
- 함수는 일반적으로 동사원형으로 시작해주세요.
- Event Hander 함수의 경우 (조동사 + 동사원형)으로 시작해주세요. 주어는 유추 가능하다면, 생략 가능합니다.
    - will은 특정 행위가 일어나기 직전을 의미합니다.
    - did는 특정 행위가 일어난 직후를 의미합니다.
    <details>
    <summary>예제코드</summary>

    - **Good ✅**
        ```swift
        class AcademyViewController {

            private func didFinishSession() {
                // ...
            }

            private func willFinishSession() {
                // ...
            }

            private func scheduleDidChange() {
                // ...
            }
        }
        ```
    - **Bad ❌**
        ```swift
        class AcademyViewController {

            private func handleSessionEnd() {
                // ...
            }

            private func finishSession() {
                // ...
            }

            private func scheduleChanged() {
                // ...
            }
        }
        ```
    </details>
    
- 데이터를 가져오는 함수의 경우, `get` 사용을 지양하고 `request`, `fetch`을 적절하게 사용해주세요.
    - `request` : 에러가 발생하거나, 실패할 수 있는 비동기 작업에 사용합니다. 예를 들어, http 통신을 통해 값을 요청하는 경우가 이에 해당합니다.
    - `fetch` : 요청이 실패하지 않고 결과를 바로 반환할 때 사용합니다. 예를 들어, data를 찾고자 하는 모든 행위를 할 때가 이에 해당합니다.
    <details>
    <summary>예제코드</summary>

    - **Good ✅**
        ```swift
        func reqeustData(for user: User) -> Data?
        func fetchData(for user: User) -> Data
        ```
    - **Bad ❌**
        ```swift
        func getData(for user: User) -> Data?
        ```
    </details>

    
### 열거형
- 열거형의 이름은 `UpperCamelCase`를 사용해주세요.
- 열거형의 각 case에는 `lowerCamelCase`를 사용해주세요.
    <details>
    <summary>예제코드</summary>

    - **Good ✅**
      ```swift
      enum Result {
        case .success
        case .failure
      }
      ```
  - **Bad ❌**
      ```swift
      enum result {
        case .Success
        case .Failure
      }
      ```
     </details>
   
### 구조체와 클래스
- 구조체와 클래스의 이름은 `UpperCamelCase`를 사용해주세요.
- 구조체와 함수의 이름 앞에 `prefix`를 붙이지 말아주세요.
- 구조체와 클래스의 프로퍼티 및 메소드는 `lowerCamelCase`를 사용해주세요.
  <details>
  <summary>예제코드</summary>

  - **Good ✅**
      ```swift
      struct LeftRectangle {
          var width: Int
          var height: Int

          func drawRectangle() {
              // ...
          }
      }
      ```
      ```swift
      class Mentee {
          let id: String
          let session: String
          var group: Int
          var team: Int

          func callOutMentor() {
              // ...
          }
      }
      ```
  - **Bad ❌**
      ```swift
      struct rwRightRectangle {
          var Width: Int
          var Height: Int

          func DrawRectangle() {
              // ...
          }
      }
      ```
      ```swift
      class rwMentor {
          let Id: String
          var Group: Int

          func GiveAdvice() {
              // ...
          }
      }
      ```
    </details>

### Protocol
- 구조를 나타내는 프로토콜은 명사로 작성해야합니다.
- 무언가를 할 수 있음(능력)을 설명하는 프로토콜은 형용사로 작성해야합니다.
  <details>
  <summary>예제코드</summary>

  - **Good ✅**
      ```swift
      protocol Car {
          var speed: Int { get set }
          var name: String { get }

          func speedUp(speed: Int) -> Bool
      }
      ```
      ```swift
      protocol Drivable {
          func accelerate(speed: Int) -> ()
          func slowDown(speed: Int) -> ()
      }
      ```
  - **Bad ❌**
      ```swift
      protocol Drivable {
          var speed: Int { get set }
          var name: String { get }

          func speedUp(speed: Int) -> Bool
          func accelerate(speed: Int) -> ()
          func slowDown(speed: Int) -> ()
      }
      ```
    </details>

### Delegate
- protocol을 이용해 delegate 패턴을 구현합니다.
- 함수의 첫번째 인자는 생략가능한 델리게이트의 소스 객체를 사용합니다.
   - **Good ✅**
        ```swift
            // 델리게이트의 소스 객체만을 메서드의 인자로 받는 경우
            protocol UserScrollViewDelegate {
                func scrollViewDidScroll(_ scrollView: UIScrollView)
                func scrollViewShouldScrollToTop(_ scrollView: UIScrollView) -> Bool
            }

            // 델리게이트의 소스 객체 다음에 추가적인 인자를 받는 경우
            protocol UserTableViewDelegate {
                func tableView(
                    _ tableView: UITableView,
                    willDisplayCell cell: Cell,
                    cellForRowAt indexPath: IndexPath)
                )
                func tableView(
                    _ tableView: UITableView,
                    numberOfRowsInSection section: Int) -> Int
                )
            }
        ```
    - **Bad ❌**
        ```swift
            protocol UserViewDelegate {
                // 인자를 생략한 경우
                func didScroll()
                // 델리게이트의 소스 객체를 인수로 사용하지 않은 경우
                func willDisplay(cell: Cell)
                // 함수명을 UpperCamelCase로 작성한 경우, 다른 클래스가 존재하면 컴파일 오류 발생
                func UserScrollView(_ scrollView: UIScrollView)
            }  
        ```

## 주석
- "가장 좋은 코드는 주석이 필요 없는 코드이다" 라는 생각으로 코드를 작성해주세요. 그러나 반드시 주석이 필요할 경우 설명은 최대한 간결하고 핵심 요약에 집중해서 작성해주세요.
- 함수와 메소드는 기본적으로 무엇을 하는지 무엇을 반환하는지 문서화 주석(Documentation Comment) >[command[⌘] + option[⌥] + [/] 를 통해서 작성해주세요. 작성한 문서화 주석은 퀵헬프 메뉴에서 확인할 수 있습니다.
- Pragma Mark 를 사용하여 섹션을 꼭 구분해주세요.
    <details>
    <summary>Pragma Mark</summary>
    ```
    // MARK: - Nested Types

    // MARK: - Views

    // MARK: - Properties

    // MARK: - Life Cycle

    // MARK: - Methods
    ```
    </details>

  <details>
  <summary>예제코드</summary>
  - **Good ✅**
    ```swift
    /// 사용자 데이터를 추가합니다.
    /// - Parameter name: user fullname
    /// - Parameter age: user age
    func addData(name: String, age: Int) {
        // code to add data...
    }
    ```

    ```swift
    /// DB내 사용자 이름과 ID로 나이를 조회합니다.
    /// - Parameter ID: user ID
    /// - Parameter name: user fullname
    /// - Returns: user age
    func readData(ID: Int, name: String) {
        var age: Int
        // code to read data...
        return age
    }
    ```

  - **Bad ❌**
    ```swift
    // 사용자 데이터 추가
    func addData(name: String, age: Int) {
        // return void
    }
    ```
  </details>
  

- 연관된 코드가 있다면 MARK를 사용하여 코드영역을 구분지는것을 권장합니다.

  <details>
    <summary>예제코드</summary>

    - **Example 💡**
        ```swift
        // MARK: - Gryffindor
        let password = "Fotuna Major"
        struct Gryffindor {
            let harry: String
            let ron: String
            let hermione: String
        }

        // MARK: - Slytherin  
        class Slytherin {
            let voldemort: String
            let malfoy: String
            func deadlyCurse() {
                print("Avada Kedavra!")
            }
        }
        ```
    </details>
  

- 아직 개발이 완료되지 않은 코드가 있다면 TODO나 FIXME를 사용하여 체크하는 것도 좋습니다.
  <details>
  <summary>예제코드</summary>

  - **Example 💡**
      ```swift
      // FIXME: - 버그 수정 필요
      public func buggyFunc() {
          // buggy code..
      }

      // TODO: - 문자열 인코딩 함수 작업 계획 
      private func todoFunc() {
          // tbd..
      }
  </details>
  

## 들여쓰기
- 인덴테이션은 스페이스바 4개를 기본으로 하되, 스페이스바 4개는 탭 1개의 역할을 합니다.
  <details>
  <summary>예제코드</summary>
  
  - **Good ✅**
      ```swift
      func sayHiLeeo(isHappy: Bool) {
          if isHappy {
              print("Hi Leeo!")
          }
      }
      ```
  - **Bad ❌**
      ```swift
      func sayHiLeeo(isHappy: Bool) {
        if isHappy {
          print("Hi Leeo!")
        }
      }
      ```
  </details>
  
    
## 띄어쓰기
- 콜론(`:`)을 사용할 땐 콜론의 오른쪽으로 한 칸의 여백을 생성합니다. 콜론의 왼쪽은 공백없이 코드를 작성합니다.
  - **Example 💡**
    ```swift
    let leeo: HappyLeeo
    ```

## 클로져
### 후행 클로저 축약
- 단일 후행 클로저의 경우에는 타입유추, 함수 라벨 생략, 소괄호 생략을 사용합니다.
  <details>
  <summary>예제코드</summary>

  - **Function**
    ```
    func someFunctionThatTakesAClosure(closure: (Int) -> Void) {
          // function body goes here
    }
    ```

  - **Good ✅**
    ```swift
    someFunctionThatTakesAClosure { int in
        // trailing closure's body goes here
    }
    ```
  
  - **Bad ❌**
    ```swift
    someFunctionThatTakesAClosure(closure: { (arguInt: Int) -> Void)
        // function body goes here
    })
    ```
  </details>  

### 다중 후행 클로져
- 함수 또는 메서드의 형식 매개변수에서 클로져들만을 실 매개변수로 받는 경우 함수 또는 메서드 호출 시 함수 또는 메서드의 소괄호, 첫 번째 실 매개변수의 라벨, 실 매개변수 사이의 콤마를 생략합니다.
  <details>
  <summary>예제코드</summary>
  
  - **Good ✅**
    ```swift
    func doSomething(do: (String) -> Void, onSuccess: (Any) -> Void, onFailure: (Error) -> Void) {
        // function body
    }

    doSomething { something in
        // do closure
    } onSuccess: { result in
        // success closure
    } onFailure: { error in
        // failure closure
    }
    ```
  
  - **Bad ❌**
    ```swift
    func doSomething(do: (String) -> Void, onSuccess: (Any) -> Void, onFailure: (Error) -> Void) {
        // function body
    }

    doSomething (do: { something in
        // do closure
    }, onSuccess: { result in
        // success closure
    }, onFailure: { error in
        // failure closure
    })
    ```
  </details>  


## 타입
### 타입 추론
- 컴팩트 코드를 선호하고 컴파일러가 단일 인스턴스의 상수나 변수의 타입을 추론하도록 합니다.
- 필요한 경우 `CGFloat`나 `Int64`와 같은 경우는 특정 타입을 지정해줍니다.
  <details>
    <summary>예제코드</summary>
    
    - **Good ✅**
      ```swift
      let apple = "Developer"
      let book1 = Book()
      let age = 25
      let frameWidth: CGFloat = 120
      ```
    
    - **Bad ❌**
      ```swift
      let apple: String = "Developer"
      let book1: Book = Book()
      let age: Int = 25
      ```
  </details>
    
### 타입 어노테이션    
- 전체 제네릭 구문 `Array<T>`와 `Dictionary<T: U>` 보다는 단축 구문 `[T]`, `[T: U]`를 사용합니다.
  <details>
    <summary>예제코드</summary>
    
    - **Good ✅**
      ```swift
      var student: [String: String]?
      var students: [String]?
      ```
    
    - **Bad ❌**
      ```swift
      var student: Dictionary<String, String>?
      var students: Array<String>?
      ``` 
  </details>
  

- 빈 배열과 딕셔너리 선언 시, 타입을 명시하는 것을 선호합니다.
  <details>
    <summary>예제코드</summary>
    
    - **Good ✅**
      ```swift
      var student: [String: String] = [:]
      var students: [String] = []
      ```
    
    - **Bad ❌**
      ```swift
      var student = [String: String]()
      var students = [String]()
      ``` 
  </details>
  

## 메모리 관리
- 메모리 누수의 원인이 되는 순환 참조가 일어나지 않도록 주의해주세요.
- 객체 간의 관계를 분석하면서 `weak`와 `unowned`를 사용하여 순환 참조를 방지할 수 있습니다.
- `weak` 참조 변수는 반드시 Optional 타입이어야 합니다.
  <details>
    <summary>예제코드</summary>
    
    - **Good ✅**
      ```swift
      class ExampleClass {
          weak var example: ExmapleClass? = nil
          
          init(){
              print("init class")
          }
          
          deinit{
              print("deinit class")
          }
      }

      // 객체 내의 인스턴스가 서로를 가리키고 있지만, weak 참조를 선언했기에 순환 참조가 일어나지 않습니다.
      var ex1: ExampleClass? = ExampleClass()
      var ex2: ExampleClass? = ExampleClass()

      ex1?.example = ex2
      ex2?.example = ex1

      ex1 = nil
      ex2 = nil

      // 출력결과
      // init class
      // init class
      // deinit class
      // deinit class
      ```
  </details>

## 파일관리
- 파일 내에서 모듈 `import`를 알파벳순으로 지정하고 중복된 것들을 제거해주세요.
  <details>
      <summary>예제코드</summary>
      
      - **Good ✅**
        ```swift
        import Alamofire
        import Foundation
        import SnapKit
        ```
      
      - **Bad ❌**
        ```swift
        import Foundation

        import SnapKit
        import Alamofire
        import Foundation
        ``` 
  </details>

- `Computed properties`와 `property observers`가 있는 `property`는 같은 종류의 선언 집합 끝에 나타나야 합니다.
  <details>
      <summary>예제코드</summary>
      
      - **Good ✅**
        ```swift
        var gravity: CGFloat
        var atmosphere: Atmosphere {
            didSet {
                print("oh my god, the atmosphere changed")
            }
        }
        ```
      
      - **Bad ❌**
        ```swift
        var atmosphere: Atmosphere {
            didSet {
                print("oh my god, the atmosphere changed")
            }
        }
        var gravity: CGFloat
        ``` 
  </details>

## Reference
- [Google Swift Style Guide](https://google.github.io/swift/)
- [Airbnb Swift Style Guide](https://github.com/airbnb/swift)
- [Linkedin Swift Style Guide](https://github.com/linkedin/swift-style-guide)
- [Raywenderlich Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)
- [Apple Developers Academy - Postech](https://github.com/DeveloperAcademy-POSTECH/swift-style-guide)
- [StyleShare Swift Style Guide](https://github.com/StyleShare/swift-style-guide#%EC%B5%9C%EB%8C%80-%EC%A4%84-%EA%B8%B8%EC%9D%)
