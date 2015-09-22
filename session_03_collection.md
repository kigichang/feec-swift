# Collection

Swift Collection:
* Array
* Set
* Dictionary

## Immutable and Mutable

如果使用 `var` 宣告 Collection，則此 Collection 可以更改 Collection 大小及內容，也就是 **Mutable** 型態；如果使用 `let` 宣告，則不可以更改大小及內容 (compile 會出錯).

eg:
```
var array1 = [1, 2, 3, 4, 5]

print("before append: \(array1)")
array1.append(6)
print("after append: \(array1)")

array1[0] = 100
print("after change 0-index: \(array1)")

let array2 = [1, 2, 3, 4, 5]

array2.append(1000) // compile error

array2[0] = 100000  // compile error
```

## Repeat and Combine

可以使用 repeat 的方式，來產生 array。

array 可以使用 `+` 或 `+=` 來 append 另一個 Array.

```
var threeDoubles = [Double](count: 3, repeatedValue: 0.0)
print("threeDoubles = \(threeDoubles)")

var anotherDoubles = [Double](count: 4, repeatedValue: 2.5)

threeDoubles += anotherDoubles
print("threeDoubles = \(threeDoubles)")
```
