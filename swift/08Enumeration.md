- [Introduction](#introduction)
    - [mostly used syntax](#mostly-used-syntax)
- [Enumeration Syntax ```enum```](#enumeration-syntax-enum)
- [Matching Enumeration Values with a Switch Statement](#matching-enumeration-values-with-a-switch-statement)
- [Associated Values](#associated-values)
- [Raw Values](#raw-values)
- [Implicitly Assigned Raw Values](#implicitly-assigned-raw-values)
- [Initializing from a Raw Value](#initializing-from-a-raw-value)
- [Recursive Enumerations ```indirect case```](#recursive-enumerations-indirect-case)
# Introduction
An ```enumeration``` defines a common type for a group of related values and enables you to work with those values in a type-safe way within your code.

* Enumerations in ```Swift``` are much more flexible, and do not have to provide a value for each case of the enumeration. Alternatively, enumeration cases can specify associated values of any type to be stored along with each different case value.

* Enumerations in Swift are first-class types in their own right.
* Enumerations can also define initializers to provide an initial case value
* can be extended to expand their functionality beyond their original implementation
* can conform to protocols to provide standard functionality.

## mostly used syntax
* ```enum-case```
* ```.case_tmp```
* ```.rawValue```
* ```Enum_tmp(rawValue:)```
* ```enum enum_tmp: Type{}```
* ```indirect case```, ```indirect enum```
* ```case case_tmp(assiciated_value_types)```
# Enumeration Syntax ```enum```
Rules:
1. Use ```enum-case``` to define an enumeration
2. Multiple cases can appear on a single line, separated by commas
```swift
enum SomeEnumeration{
    // definition
}

enum CompassPoint {
    case north
    case south
    case east
    case west
}

enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}

```

3. Each enumeration definition defines a brand new type.
```swift
var directionToHead = CompassPoint.west
// Once directionToHead is declared as a CompassPoint, you can set it to a different CompassPoint value using a shorter dot syntax:
directionToHead = .east
```

# Matching Enumeration Values with a Switch Statement
Rules:
1. remember to prefix ```.``` before each case.
2. Requiring exhaustiveness ensures that enumeration cases are not accidentally omitted. If all cases are listed, there is no need to add a ```default``` term.
```swift
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
// Prints "Watch out for penguins"
```

# Associated Values
You can define Swift enumerations to store associated values of any given type, and the value types can be different for each case of the enumeration if needed. 

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}

var productBarcode = Barcode.upc(8, 85909, 51226, 3)
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")

switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."

```


# Raw Values
As an alternative to associated values, enumeration cases can come prepopulated with default values,  which are all of the same type.

Rules:
1. declare type of raw values in the ```enum``` part.
2. Raw values can be strings, characters, or any of the integer or floating-point number types.
3. Each raw value must be unique within its enumeration declaration.

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

# Implicitly Assigned Raw Values
Rules:
1. when integers are used for raw values, the implicit value for each case is one more than the previous case. If the first case doesn’t have a value set, its value is 0.

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}   // 1,2,3,4,5,6,7,8
```
2. When strings are used for raw values, the implicit value for each case is the text of that case’s name.
```swift
enum CompassPoint: String {
    case north, south, east, west
}   // "north", "south", "east", "west"
```

3. access the raw value of an enumeration case with its ```.rawValue``` property
```swift
let earthsOrder = Planet.earth.rawValue             // 3
let sunsetDirection = CompassPoint.west.rawValue    // "west"
```

# Initializing from a Raw Value
Rules:
1. ```Enum_class(rawValue: 7)```
2. Not all possible ```rawValue``` will find a matching case. Because of this, the raw value initializer always returns an ```optional_enumeration_case```.

# Recursive Enumerations ```indirect case```
A recursive enumeration is an enumeration that has another instance of the enumeration as the associated value for one or more of the enumeration cases. 

Rules:
1. To indicate that an enumeration case is recursive, write indirect before it
```swift
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```
2. You can also write ```indirect``` before the beginning of the enumeration to enable indirection for all of the enumeration’s cases that have an associated value:
```swift
indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

3. A recursive function is a straightforward way to work with data that has a recursive structure.

```swift
func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    case let .number(value):
        return value
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
    }
}

// define an instance (5+4)*2
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum,
ArithmeticExpression.number(2))

print(evaluate(product))    // "18"
```
