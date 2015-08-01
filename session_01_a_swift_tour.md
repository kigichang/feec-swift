# A Swift Tour

## 與 Object-C 差別

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