# A Swift Tour - OOP

## Class

用 `class` 來宣告物件，Class 裏面的屬性(**Property**)，也分變數(`var`)及常數(`let`). Function 的宣告也和先前一樣。

原本 C 語言，使用 `new` 來產生實體 (object or instance)；在 Swift 則不用 `new`。

eg: 

```
class Shape {
    var numberOfSides = 0
    
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides"
    }
}

let shape = Shape()
shape.numberOfSides = 7
print(shape.simpleDescription())
```

* Constructor (建構子) and Destructor (解構子)

    使用 `init` 來宣告 contructor；`deinit` 宣告 destructor。如果 class 內的 property 沒有給預設值時，Xcode 會出現沒有宣告 `init` 的錯誤。

    原本在 C 語言使用 `this`，在 Swift 改用 `self`。

    使用 contructor 產生實體 (object or instance) 時，傳入的參數值都要標記變數名稱。否則 Xcode 會報錯。

    在 constructor 內，可以針對**未**給值的常數(`let`)變數，指定新值；但之後就不能再重新指定值。

    ```
    class NamedShape {
        
        var numberOfSides: Int = 0
        
        let name: String        // 未給值的常數變數
        
        init(name: String) {
            self.name = name    // 在 init 內，對常數指定值
        }
        
        func simpleDescription() -> String {
            return "A \(name) with \(numberOfSides) sides"
        }
        
        deinit {
            print("\(name) release")
        }
        
    }

    func test() {
        let s1 = NamedShape(name: "Test1")  // 要帶 "name"，要不然會報錯
        let s2 = NamedShape(name: "Test2")

        s1.numberOfSides = 10
        s2.numberOfSides = 20

        print(s1.simpleDescription())
        print(s2.simpleDescription())
    }
    test()

    ```

    註：在主程式下，好像沒有去 call `deinit`，因而改用 `test()` 來測試。在主程式沒去呼叫 `deinit` 也許是個 bug。

* Inheritance and Polymorphism

    繼承與 C 一樣，用 `:`。當要改寫 super class 的 function 時，一定要用 `override`，要不然 Xcode 會報錯。

    ```
    class Square: NamedShape {
        var sideLength: Double
        
        init(sideLength: Double, name: String) {
            self.sideLength = sideLength
            super.init(name: name)
            
            self.numberOfSides = 4
        }
        
        func area() -> Double {
            return sideLength * sideLength
        }
        
        override func simpleDescription() -> String {
            return "A \(name) with sides of length \(sideLength)."
        }
        
    }


    let testSqaure = Square(sideLength: 5.2, name: "my test square")
    print(testSqaure.area())
    print(testSqaure.simpleDescription())

    ```

* Getter and Setter

    Class 的 Property 也可以設定 `getter` 及 `setter`。

    * `get`: 需要回傳值
    * `set`: 傳入的值，會有一個隱性(implicit)變數 `newValue`來代表。

    ```
    class EquilateralTriangle: NamedShape {
        
        var sideLength: Double = 0.0
        
        init(sideLength: Double, name: String) {
            self.sideLength = sideLength
            super.init(name: name)
            
            numberOfSides = 3
        }
        
        var perimeter: Double {
            get {
                return Double(numberOfSides) * sideLength
            }
            set {
                print("Got \(newValue)")
                sideLength = newValue / Double(numberOfSides)
            }
        }
        
        override func simpleDescription() -> String {
            return "A \(name) with side of length \(sideLength) and perimeter \(perimeter)"
        }
    }

    let triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")

    print(triangle.perimeter)
    triangle.perimeter = 9.9
    print(triangle.perimeter)
        
    ```
    
* Property Observers

    Swift 提供追蹤 Property 更改過程。Property Observers 的機制，除了可以用在 Class Property外，也可以用在一般變數。
    
    * `willSet`: 修改前。會提供一個 **implicit** 變數 `newValue`，代表輸入的新值。
    * `didSet`: 修改後。會提供一個 **implicit** 變數 `oldValue`，代表原來的值。


    ```
    class TriangleAndSquare {
        var triangle: EquilateralTriangle {
            
            willSet {
                print("triangle willSet \(newValue.sideLength)")
                square.sideLength = newValue.sideLength
            }
            
            didSet {
                print("triangle didSet \(oldValue.sideLength)")
            }
        }
        
        var square: Square {
            
            willSet {
                print("square willSet \(newValue.sideLength)")
                triangle.sideLength = newValue.sideLength
            }
            
            didSet {
                print("square didSet \(oldValue.sideLength)")
            }
        }
        
        init(size: Double, name: String) {
            square = Square(sideLength: size, name: name)
            triangle = EquilateralTriangle(sideLength: size, name: name)
        }
        
    }

    let triangleAndSquare = TriangleAndSquare(size: 10.0, name: "another test shape")

    print(triangleAndSquare.square.sideLength)
    print(triangleAndSquare.triangle.sideLength)

    print("start set square")
    triangleAndSquare.square = Square(sideLength: 50.0, name: "larger square")
    print("end set square")
    print(triangleAndSquare.triangle.sideLength)
    ```

## Enumeration
    
Enumeration 多用在**狀態**值。用 `enum` 宣告，`enum` 裏也可以宣告 Function。

使用 `enum` 宣告時，可以指定 enumeration 的原始資料型別。

eg:

```
enum Rank: Int {
    case Ace = 1
    
    case Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten
    
    case Jack, Queen, King
    
    func simpleDescription() -> String {
        switch self {
        case .Ace:
            return "ace"
        case .Jack:
            return "jack"
        case .Queen:
            return "queen"
        case .King:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}

let jack = Rank.Jack
print(jack.rawValue)

```

上面的 `Rank` 的原始資料型別是 `Int`。有指定原始資料型別時，就會有 `rawValue` 這個變數可以使用。使用 `Int` 時，只要宣告第一個元素(`Ace`)的值，之後會依續自動給值. 

Enumeration 有指定資料型別時，會自動產生 contructor: `init?(rawValue:)`，因此可以傳入 `rawValue` 來產生 enumeration。注意：回傳是一個 Optional 型別。

eg:

```
func RankFromRaw(raw: Int) {
    if let rank = Rank(rawValue: raw) {
        print(rank.simpleDescription())
    }
    else {
        print("not Match")
    }
}

RankFromRaw(1)
RankFromRaw(14)

```

enumeration 也可以不指定資料型別。不過建議還是指定，因為通常我們都會儲存狀態值。

eg:

```
enum Suit {
    
    case Spades, Hearts, Diamonds, Clubs
    
    func simpleDescription() -> String {
        switch self {
        case .Spades:
            return "spades"
            
        case .Hearts:
            return "hearts"
            
        case .Diamonds:
            return "diamonds"
            
        case .Clubs:
            return "clubs"
        }
    }
    
    func color() -> String {
        switch self {
        case .Spades, .Clubs:
            return "black"
        case .Hearts, .Diamonds:
            return "red"
        }
    }
}

```

enumeration 的元素，也可以儲存狀態值。

```
enum ServerResponse {
    case Result(String, String)
    case Error(String)
}

let success = ServerResponse.Result("6:00 AM", "8:09 PM")
let failure = ServerResponse.Error("Out of cheese.")

switch success {
    
case let .Result(sunrise, sunset):
    print("Sunrise is at \(sunrise) and sunset is at \(sunset)")
    
case let .Error(error):
    print("Failure...\(error)")
    
}

```

## Structure

Structure 與 Class 相似，可以有 Property 及 Function

```
struct Card {
    let rank: Rank
    let suit: Suit
    
    func simpleDescription() -> String {
        return "The \(rank.simpleDescription()) of \(suit.color()) \(suit.simpleDescription())"
    }
}

let treeOfSpades = Card(rank: .Three, suit: .Spades)

print(treeOfSpades.simpleDescription())
```

Structure(**Value Type**) 與 Class (**Reference Type**) 的比較，後續會詳細說明。

## Protocol

Protocol 跟 Java Interface 類似。在 Swift，Class, Enumeration, 及 Structure 都可以繼承 Protocol。

```
protocol ExampleProtocol {
    var simpleDescription: String { get }
    
    mutating func adjust()
}

class SimpleClass: ExampleProtocol {
    var simpleDescription = "A very simple class."
    
    var anotherProperty: Int = 69105
    
    func adjust() {
        simpleDescription += " Now 100% adjusted"
    }
}

let a = SimpleClass()
a.adjust()
print(a.simpleDescription)

struct SimpleStructure: ExampleProtocol {
    
    var simpleDescription = "A very simple structure."
    
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}

var b = SimpleStructure()
b.adjust()
print(b.simpleDescription)


```

如果是 **Value Type**(Structure, Enumeration 等)，改寫 Function 時，記得要加 `mutating`，**Reference Type**(Class)則不用。

## Extension

`extension` 可以讓都原本定義好的資料型別，再擴充客製的 Function。`extension` 可以搭配 `protocol` 使用。

eg:

```
extension Double {
    func abs() -> Double {
        if self < 0 {
            return -self
        }
        else {
            return self
        }
    }
}

let testDouble = -10.0
print(testDouble)
print(testDouble.abs())

extension Int: ExampleProtocol {
    
    var simpleDescription: String {
        return "The number \(self)"
    }
    
    mutating func adjust() {
        self += 42
    }
}

var seven: Int = 7

print(seven.simpleDescription)
seven.adjust()
print(seven.simpleDescription)

```

## Generic

Generic 主要是將資料型態也當作是一種變數。最初是要解決相同工作的 Function，只是因為資料型別不同，就要依每種資料型別都寫一套。在 C++ 有 Template 技術. 

使用 Generic 可以解決資料轉型 (cast) 的問題。以往使用 Collection (如：HashMap, Array)。如果沒有指定型別時，取值都需要做轉型，也造成程式的不可靠。(因為 cast error 是在 runtime 會才會發現，且發現時，程式也當掉了)。有了 Generic，可以在 compiler time 時期，就檢查資料型別是否正確。

eg:

```
func repeatInt(item: Int, numberOfTimes: Int) ->[Int] {
    var result = [Int]()
    
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    
    return result
}

func repeatDouble(item: Double, numberOfTimes: Int) ->[Double] {
    var result = [Double]()
    
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    
    return result
}

func repeatItem<T>(item: T, numberOfTimes: Int) ->[T] {
    var result = [T]()
    
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    
    return result
}

print(repeatItem(10, numberOfTimes: 4))
```

使用 Generic 時，也可以使用 `where` 來限定 generic 必需是繼承自那個資料型別。

eg:

```
func anyCommonElements<T, U where T: SequenceType, U: SequenceType, T.Generator.Element: Equatable, T.Generator.Element == U.Generator.Element> (lhs: T, _ rhs: U) -> Bool {
    
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return true
            }
        }
    }
    
    return false
}

print(anyCommonElements([1, 2, 3,], [3]))

```

建議：Generic 的機制雖然很好用，但儘量不要搞得太複雜。


