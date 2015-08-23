# The Basic

Swift is **_Type Safe_** language.

### Data Type

* Fundamental Type
  * Int
    * Int8, Int16, Int32, Int64
    * UInt8, UInt16, UInt32, UInt64
    * Int, UInt depends on 32-bit or 64-bit platform
  * Double, Float
  * String
  * Bool


* Type Alias

  ```
  typealias AudioSample = UInt16

  var maxAmplitudeFound = AudioSample.min

  print("maxAmplitudeFound = \(maxAmplitudeFound)")
  ```


* Tuple

  將不同資料型別，組合成一個新的資料型別。eg: `let http404Error = (404, "Not Found")`

  * Decompose

    ```
    let http404Error = (404, "Not Found")

    let (statusCode, statusMessage) = http404Error

    print("statusCode = \(statusCode) and statusMessage = \(statusMessage)")

    ```
    or

    ```
    let (justTheStatusCode, _) = http404Error

    print("just the status code = \(statusCode)")
    ```

  * Name

    ```
    let http200Status = (statusCode: 200, description: "OK")

    print("status = \(http200Status.0) and desc = \(http200Status.1)")
    print("status = \(http200Status.statusCode) and desc = \(http200Status.description)")
    ```


* Collection Typ
  * Array
  * Set
  * Dictionary

### Constant and Variable

* Constant(`let`): The value of a **_constant_** cannot be changed once it is set.
* Variable(`var`): The value of a **_variable_** can be change in the future.

### Tuple

### Optional

* There is a value, and it equals x
* There isn't a value at all
