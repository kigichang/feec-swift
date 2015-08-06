# A Swift Tour - OOP

## Class

用 `class` 來宣告物件，Class 裏面的屬性(**Property**)也分變數(`var`)及常數(`let`). Function 的宣告也和先前一樣。

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