# A Swift Tour - Fundamentals

## 主程式

* 原本需要 `include` I/O 及 String 處理相關 library；XCode 預設會 `import Foundation`。
  * 原：`inlcude` 改成 `import`

* 原本有 `main` 當作程式的入口；Swift 程式碼是 global scope，直接當程式的入口，因此不需 `main`。
* 每一行程式結尾，不需要分號`;`，除非一行內有兩個以上的 statement。
  * `print("Hello, World!")`
  * `var a = 0; print(a)`


## 變數宣告

* 變數宣告時，需指定是一般變數(`var`)，還是常數變數(`let`)
  * 一般變數，可以 re-assign: `var a = 10; a = 20`
  * 常數變數，不能 re-assign: `let b = 10`

* 變數宣告時，可以省略型別，如：`let str = "Hello World!"`
  * 數字型變數省略型別時：
      * 整數預設是 **Native** `Int`。(Native Int 會因作業系統是 32 bit 還是 64 bit，長度會有差異)
      * 浮點數預設是 `Double`
      
  * 不省略型別，用冒號 `:` 後面接型別，如：`let str2: String = "Hello World!"`

## Strong Type and String Interpolation

* Strong-Type: 資料型別不會改變
* 變數不會自動轉換型別，如：字串加數字

    ```
    let label = "This width is "
    let width = 94
    let widthLabel = label + String(width)
    
    ```

* String interpolation：用 `\()`

    ```
    let apples = 3
    let oranges = 5

    let appleSummary = "I have \(apples) apples."
    let fruitSummary = "I have \(apples + oranges) pieces of fruit."
    
    print(appleSummary)
    print(fruitSummary)
    
    ```

## Array and Dictionary

* Array 宣告：`[]`

    ```
    var shoppingList = ["catfish", "water", "tulips", "blue paint"]
    print(shoppingList[0])

    shoppingList[1] = "bottle of water"

    print(shoppingList[1])
    
    ```
    
    * Empty Array
    
        ```
        let emptyArray = [String]() // 指定資料型別
        let emptyArray2 = []        // 不指定資料型別
        
        ```
        
* Dictionary 宣告：`[:]`

    ```
    var occupations = [
        "Malcolm": "Captain",
        "Kaylee": "Mechanic"
    ]

    print(occupations["Malcolm"])
        
    occupations["Jayne"] = "Public Relations"
        
    print(occupations["Jayne"])
    
    ```
    
    * Empty Map
        
        ```
        let emptyDictionary = [String: Float]() // 指定資料型別
        let emptyDictionary2 = [:]              // 不指定資料型別
        
        ```
        
## Control Flow

* if

    `if` 內的判斷式，一定是要 **boolean**，不像 C，可以放數字 or null. 

    ```
    let score = 30

    var teamScore = 0

    if score >= 50 {
        teamScore += 3
    }
    else if score >= 30 {
        teamScore += 2
    }
    else {
        teamScore += 1
    }

    print(teamScore)
    ```
    
* if and Optional

    建議原本使用 null 的機制，都改成用 Optional。宣告的方式，只要在資料型別後，加 `?`。如：`var optionalString: String? = "Hello"`
        
    ```
    var optionalString: String? = "Hello"

    print(optionalString)

    print(optionalString == nil)

    var optionalName: String? = "John Appleseed"

    var greeting = "Hello!"

    if let name = optionalName {
        greeting = "Hello, \(name)"
    }

    print(greeting)

    greeting = "Hello!"
    optionalName = nil

    if var name = optionalName {    // 也可以用 var，可以 re-assign
        greeting = "Hello, \(name)"
        name = "test var"
    }

    print(greeting)
    ```
    
* swich - case

    原本在 C 使用 switch - case 時，一定要用 `break` 去結束 `case` 內程式碼的執行，否則會往下一個 `case` 執行。在 Swift 則可省略 `break`，預設會自動中斷；如果要往下執行，則可使用 `fallthrough`；如果 `case` or `default` 不做任何事情，則就需要使用 `break`
    
    `switch` - `case` 一定要將所有狀況都列進去，否則 compiler 會出錯，因此建議在使用時，都用 `default` 當結束。
    
    `case` 除了可以是常數值外，也可以使用類似 `if` 的判斷式
    
    ```
    let vegetable = "red pepper"

    var vegetableComment = "Everything tastes good in soup."

    switch vegetable {
        
    case "celery":                          // 一般的比對
        vegetableComment = "Add some raisins and make ants on a log."
        
    case "cucumber", "watercress":          // 符合多種條件
        vegetableComment = "That would make a good tea sandwich."
        
    case let x where x.hasSuffix("pepper"): // 經過運算符合條件
        vegetableComment = "Is it a spicy \(x)?"
        
    default:
        break   // 不做任何事，要用 break
        
    }

    print(vegetableComment)
    ```

* Range

    * Include Both `...`。如：`1...10`，是指從 **1** 到 **10**。
    * Exclude Upper `..<`。如：`1..<10`，是指從 **1** 到 **9**。**不包含** 10。

* for - in

    * Range

        ```
        for i in 1...10 {
            print(i)
        }

        for i in 1..<10 {
            print(i)
        }
        ```
    * C like

        ```
        for var i = 0; i < 10; i++ {
            print(i)
        }
        ```

    * Array

        ```
        let shoppingList = ["catfish", "water", "tulips", "blue paint"]

        for item in shoppingList {
            print(item)
        }
        ```
    
    * Dictionary

        ```
        let interestingNumbers = [
            "Prime": [2, 3, 5, 7, 11, 13],
            "Fibonacci": [1, 1, 2, 3, 5, 8],
            "Square": [1, 4, 9, 16, 25]
        ]

        var largest = 0

        for (kind, numbers) in interestingNumbers {
            for number in numbers {
                if number > largest {
                    largest = number
                }
            }
        }

        print(largest)
        ``` 
        
* while and repeat - while

    ```
    var n = 2

    while n < 100 {
        n *= 2
    }

    print(n)

    var m = 2

    repeat {
        m *= 2
    } while m < 100

    print(m)
    ```
    
##  Function and Closure

宣告 Function 的語法。

```
func 名稱(變數: 資料型態, ...) -> 回傳資料型態 {
    return 值
}
```

eg: 

```
func greet(name: String, day: String) -> String {
    return "Hello \(name), today is \(day)."
}

func total(amount: Int,times: Int, a: Int) -> Int {
    return amount * times * a
}
```

* Named Argument

    呼叫時，除了第一個參數可以不用帶變數名稱外，第二個變數之後，都要帶變數名稱。
    
    eg:
    
    ```
    print(greet("Bob", day: "Tuesday"))

    print(total(30, times: 20, a: 30))
    ```
    
* Tuple
    
    Tuple 可以包含不同資料型態的資料。eg: (x, y, z)，其中的 x, y, z 的資料型別可以不一樣。取值的時候，可以使用宣告的名稱，或者用數字(**0-index**)
    
    eg:
    
    ```
    func calc(scores: [Int]) -> (min: Int, max: Int, sum: Int, avg: Double) {
        
        var min = scores[0]
        var max = scores[0]
        var sum = 0
        
        
        for score in scores {
            if score > max {
                max = score
            } else if score < min {
                min = score
            }
            
            sum += score
        }
        
        return (min, max, sum, Double(sum) / Double(scores.count))
    }

    let result = calc([5, 3, 100, 3, 9])

    print(result.3)     // by 0-index
    print(result.avg)   // name
        
    ```

* Variable Number of Arguments

    Function 可以接受不定個數的變數傳入。
    
    ```
    func sumOf(numbers: Int...) -> Int {
        
        var sum = 0
        
        for number in numbers {
            sum += number
        }
        
        return sum
    }

    print(sumOf())
    print(sumOf(42, 597, 12))
    
    ```
    
* Nested Function

    在 Function 內，還可以再宣告 Function
    
    ```
    func returnFifteen() -> Int {
        
        var y = 10
        
        func add() {
            y += 5
        }
        
        add()
        
        return y
    }

    print(returnFifteen())

    ```

* First-Class Type and Closure

    一個程式語言有 First-Class Function 特性，是指此程式語言將 Function 當作是一種資料型態。
    
    * 語法：
    
        ```
        { (parameter, parament, ...) -> return type in 
            statemetns
        }
        ```
    
    * 當作回傳值
    
        ```
        func makeIncrementer() -> (Int -> Int) {
            
            func addOne(number: Int) -> Int {
                return 1 + number
            }
            
            return addOne
        }

        var increment = makeIncrementer()

        print(increment)
        print(increment(7))
        ```

    * 當作參數傳入
    
        ```
        func hasAnyMatches(list: [Int], condition: Int -> Bool) -> Bool {
            for item in list {
                if condition(item) {
                    return true
                }
            }
            
            return false
        }

        func lessThenTen(number: Int) -> Bool {
            return number < 10
        }

        let numbers = [20, 19, 7, 12]
        print(hasAnyMatches(numbers, condition: lessThenTen))

        ```
        
    * Closure

        ```
        let newNumbers = numbers.map({
            (number: Int) -> Int in
            let result = 3 * number
            return result
        })
        
        let mappedNumbers = numbers.map { number in 3 * number } // 小括號可以省略

        print(numbers)
        print(newNumbers)
        print(mappedNumbers)

        let sortedNumbers = numbers.sort { $0 > $1 } // 變數編號 0-index

        print(sortedNumbers)

        let sortedNumbers2 = numbers.sort( > )      // 最簡化寫法

        print(sortedNumbers2)

        ```
        