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
* Type Alias
  ```
  typealias AudioSample = UInt16

  var maxAmplitudeFound = AudioSample.min

  print("maxAmplitudeFound = \(maxAmplitudeFound)")
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
