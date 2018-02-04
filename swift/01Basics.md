**We don't repeat simple rules mentioned in [00FirstGlance.md](https://github.com/xshii/XSNote/blob/master/swift/00FirstGlance.md)** 
# Introduction core feature
* **Swift** provides its own versions of all fundamental C and Obj-C types.
    * ```Int```
    * ```Float```
    * ```Double```
    * ```String```
    * ```Array```
    * ```Set```
    * ```Dictionary```
* Constants are used throughout **Swift** to make code safer and clearer.
* Advanced type ```tuple``` enables you to create and pass around groupings of values. Like returning multiple values from a function
* Optional type ```T?``` is safer and more expressive than ```nil``` pointers in Obj-C.
* **Swift** is a **type-safe** language. It does not provide implicit type conversion, even between nonoptional type ```T``` and optional type ```T?```
    * you can not assign ```T?``` to ```T```

# Constants and Variables
Rules:
1. Multiple assigments can declare on a single line, seperated by commas ```,```
```swift
var x = 0.0, y = 0.0, z = 0.0

// Error
var x = y = z = 0.0

```
2. Multiple declaration with the same type on a single line, followed by ```:Type```
```swift
var red, green, blue: Double
```

3. name can be almost any character, including Unicode. Except:
    * whitespace
    * mathematical symbols
    * arrows
    * invalid Unicode
    * line- and box-drawing characters
    * number
```swift
let π = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```
4. can't declare the variable with the same name or change it to store a value of a different type. Can't change a const to var and vice versa.

# Print
```print(_:separator:terminator:)```
Rule:
1. To avoid auto newline, use 
```swift
print(someValue, terminator: "")
```

# Comments
Rules:
1. single line comment ```//```
2. multiple lines comment
    
    2.1 multiple lines comment can be nested, UNLIKE C.
```swift
/*  Comment line1
    comment line2       */

// nested
/*  Comment line1
/*  comment line1.1     */
    comment line2       */
```

# Semicolons
Rule:
1. use semicolons ```;``` to write multiple seprate statement on a single line:
```swift
let cat = "🐱"; print(cat)
// Prints "🐱"

```

# Basic Type
## Integers
Rules:
1. Integers are either ```signed``` or ```unsigned```
2. Swift provides signed/unsigned integers in 8, 16, 32, and 64 bit forms.
    
    2.1 Unsigned 8-bit integer is of type ```UInt8```
    
    2.2 Signed 32-bit integer is of type ```Int32```
3. use ```Type.min```, ```Type.max``` to access the minimum and maximum values of each integer type.
```swift
let minU8Value = UInt8.min  // 0: UInt8
let maxU8Value = UInt8.max  // 255: UInt8
```
4. ```Int``` is platform dependent. Unless work needs a specific size of integer, always use ```Int``` for integer values.

    4.1 On a 32-bit platform, ```Int``` == ```Int32```

    4.2 On a 64-bit platform, ```Int``` == ```Int64```
5. ```UInt``` is similar to ```Int```. Use ```UInt``` only when you specifically need an unsigned integer type with the same size as the platform's native word size. TO AID CODE INTEROPERABILITY AND CONSISTENCY.

## Floating-Point Numbers
Rules:
1. ```Double``` represents a 64-bit floating-point number   = 15 decimal digits
2. ```Float``` represents a 32-bit floating-point number    = 6 decimal digits
3. use ```Double```  always

# Type Safety and Type Inference
Rules:
1. Type Inference happens while initialzation with a literal.
2. ```Double``` is preferred than ```Float``` when it has decimal.

## Literals
### number
Rules:
1. A decimal number,    with no prefix
2. A binary number,     with a ```0b``` prefix
3. An octal number,     with a ```0o``` prefix
4. A hexadecimal number,    with a ```0x``` prefix
5. For deimal exponents, add ```e``` before the exponent. $10^{exp}$
6. For hexadecimal exponents, add ```p```. $2^{exp}$
7. Can use underscore or additional ```0``` to help with readability.
```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

### Numeric Type Conversion
Rules:
1. Unlike ```C```, both signed & unsigned integers can not overflow the boundary.
2. different Integer type must be explicitly converted in order to interact
3. ```Int``` & ```Double/Float``` conversion must be explicit.

    3.1 Floating-point values are always truncated.
```swift
// Rule1 Error
let cannotBeNegative: UInt8 = -1
// Rule1 Error
let tooBig: Int8 = Int8.max+ + 1

// Rule2 OK
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one)

// Rule3 
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine    // 3.14159
let intPi = Int(pi) // 3

```

## Booleans ```bool```
```true``` and ```false```
Rules:
1. Unlike ```C```, ```if 1{}``` will not compile and will raise an error
2. There is no explicit conversion from ```bool``` to ```Int```
```swift
// Error
let myIntTrue = Int(true)
```
## Tuples
*Tuples* group multiple values into a single compound value.

Rules:
1. The values within a tuple can be of any type and don't have to be of the same type as each other.
2. decomposition is like ```list``` in ```Python```, but it must have parenthesis
3. Ignoring value is like ```_``` in ```Python```, but it only stands for one value per ```_```
4. Accessing can call on index numbers starting at zero
5. Or you can label the individual elements when defining the tuple. The indexing still works.
6. For more complex data structures, model it as a class or structure.

```swift
// Rule1
let http404Error = (404,"Not Found") // type is (Int, String)
// Rule2
let (statusCode, statusMessage) = http404Error
// Rule3
let (status,_) = http404Error
// Rule4
print("The status code is \(http404Error.0)")
// "The status code is 404"

// Rule 5
let http200Status = (statusCode: 200, description: "OK")
print(http200Status.description)    // "OK"
print(http200status.1)              // "OK"
```
## Optionals
```optionals``` is used where a value may be absent - ```nil```. An optional represents two possibilities.
1. there is a value and you need unwrap the optional to aceess that value.
2. there isn't a value at all - ```nil```

Rules:
1. Any type can be wrapped with optionals.
2. when converting, the left side is infferred to be an optional. Because the intializer might fail.
3. If you define an optional variable without providing a default value, it is automatically set to ```nil```
4. ```nil``` is not a pointer. It's the absence of a value of a certain type.
5. use exclamation mark(```!```) to access its underlying value. This is known as **forced unwrapping of the optional's value**.

    5.1 always remember to check the variable is not ```nil``` before using ```!```. OW, it will raise a runtime error.

6. ```nil!=false```
```swift
let possibleNumber = "123"
// Rule2
let convertedNumber = int(possibleNumber)
// convertedNumber is inferred to be of type "Int?"

// Rule3
var surveyAnswer: String?   // surveyAnswer = nil

// Rule5
if convertedNumber != nil {
    print("convertedNumber has an integer value of \(convertedNumber!).")
}
```
### Optional Binding
Rule:
1. ``` let constantName = someOptional``` stands for checking if it is ```nil```. It can be used in ```if``` and ```while``` statements.
2. For multiple variables checking, check before operation.

```swift
// Rule1
if let actualNumber = Int(possibleNumber) {
    print("\"\(possibleNumber)\" has an integer value of \(actualNumber)")
} else {
    print("\"\(possibleNumber)\" could not be converted to an integer")
}
// ""123" has an integer value of 123"

// Rule2
if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100 {
    print("\(firstNumber) < \(secondNumber) < 100")
}
// Prints "4 < 42 < 100"
```

### Implicitly Unwrapped Optionals ```!```
They are useful when an optional’s value is confirmed to exist immediately after the optional is first defined and can definitely be assumed to exist at every point thereafter. 

Rule:
1. it is used during initialization. 
2. One can still treat an implicitly unwrapped optional llike a normal optional.
3. Don’t use an implicitly unwrapped optional when there’s a possibility of a variable becoming nil at a later point. i.e. only ```let``` variable.
```swift
let possibleString: String? = "an optional string."
let forcedString: String = possibleString

let assumedString: String! = "an implicitly unwrapped optional string."
let implicitString: String = assumedString
```


## Type Aliases
like ```typedef``` in ```c```
Rule:
1. ```typealias type_name = Type  ```
```swift
typealias AudioSample = UInt16
var maxAmplitutdeFound = AudioSample.min    // 0 
```

# Error Handling
same in [00FirstGlance.md#Error_Handling](https://github.com/xshii/XSNote/blob/master/swift/00FirstGlance.md#error-handling)

# Assertions and Preconditions
if the condition expression in ```assert``` is false, the current state of the program is invalid; code execution ends, and your app is terminated. So, they are used for vital problems rather than recoverable or expected errors.

## ```assert```
Rules:
1. ```assert``` is checked only in debug builds, but preconditions are checked in both debug and production builds.
2. ```assert(_:_:file:line:)```, the mandatory first input should be condition expression and the optional second input should be the error message - a ```String``` type.
```swift
let age = -3
assert(age >= 0, "A person's age can't be less than zero.")
// "A per....zero."

assert(age>-0)
```

3. use the ```assertionFailure(_:file:line:)``` if the code already ckecks the condition to be false.
```swift
if age > 10 {
    print("You can ride the roller-coaster or the ferris wheel.")
} else if age > 0 {
    print("You can ride the ferris wheel.")
} else {
    assertionFailure("A person's age can't be less than zero.")
}
```

## Enforcing Preconditons
use a precondition to ckeck that 
1. a subscript is not out pf bounds
2. check that a function has been passed a valid value.


Rules:
1. ```precondition(_:_:file:line:)``` same as ```assert```
2. call the ```precondtionFailure(_:file:line:)``` same as ```assertionFailure(_:file:line:)```
3. if you compile in unchecked mode (```-0unchecked```), preconditons aren't checked. However, the ```fatalError(_:file:line:)``` function still works and always halts execution.
```swift
// In the implementation of a subscript...
precondition(index > 0, "Index must be greater than zero.")
```