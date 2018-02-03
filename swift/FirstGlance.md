- [Hello World](#hello-world)
- [Variables](#variables)
    - [global_type](#globaltype)
        - [```var```](#var)
        - [```let```](#let)
- [Types](#types)
    - [Int, Float, Double, String](#int-float-double-string)
        - [special String with ``` ``` ```](#special-string-with)
    - [Arrays[], dict[k:v]](#arrays-dictkv)
    - [```nil```](#nil)
    - [Explicit Type Conversion](#explicit-type-conversion)
- [Control Flow](#control-flow)
    - [Loop Flow](#loop-flow)
        - [```for-in```](#for-in)
        - [```while```](#while)
        - [```repeat-while```](#repeat-while)
    - [Condition Flow](#condition-flow)
        - [```if-else```](#if-else)
        - [```switch-case```](#switch-case)
- [Function and Closure](#function-and-closure)
    - [Function ```func```](#function-func)
    - [Closure ```{}```](#closure)
- [Objects and Classes](#objects-and-classes)
    - [```class```](#class)
        - [Properties](#properties)
        - [Methods](#methods)
    - [Subclass ```class B: A```](#subclass-class-b-a)
        - [additional usage of ```?```](#additional-usage-of)
- [Enumerations and Structures](#enumerations-and-structures)
    - [```enum```](#enum)
    - [```struct```](#struct)
- [Protocols and Extensions](#protocols-and-extensions)
    - [```protocol```](#protocol)
    - [```extension```](#extension)
- [Error Handling](#error-handling)
    - [```enum``` and ```throw``` error in a ```throws``` function](#enum-and-throw-error-in-a-throws-function)
    - [```do-try-catch``` to handle errors](#do-try-catch-to-handle-errors)
    - [```defer``` to run code whenever it throws an error or not.](#defer-to-run-code-whenever-it-throws-an-error-or-not)
- [Generics](#generics)
    - [```<T>```](#t)
    - [```where```](#where)
# Hello World
```swift
print("Hello, World!")
``` 
# Variables 
```swift
global_type var_name : var_type = literal/fct
```

must:
* global_type
* var_name
* 
optional
* var_type (swift supports type inference)
* assignment (unless it is a ```let``` variable)




## global_type
### ```var```

```var``` is used to create a mutable variable
```swift
var myInt = 42
```
### ```let```
```let``` is used to create a constant variable.

Rules:
1. a constant variable must be initialized when created.
2. Once initialized, assigning operation will raise an error.
3. a constant variable can be initialized by a initialized mutable variable. Changing the mutable will not affect the constant afterwards.

```swift
// Rule 1 OK
let myConstInt = 52

// Rule 1 Error: Type annotation missing in pattern
let myConstInt2

// Rule 2 Error Cannot assign to value: 'myConst' is a 'let' constant
myConstInt = 42

// Rule 3
let myConstInt3 = myInt // myConstInt3 = 42
myInt = 54              // myConstInt3 = 42
```
# Types
```Int, Float, Double, String, Arrays[], dict[k:v], ```
## Int, Float, Double, String
Those types are elementary types.
### special String with ``` ``` ```
it stands for multi-line String
```swift
let apples = 42
let oranges = 8
let quotation = """
I said "I have \(apples) apples."
And then I said "I have \(apples + oranges) pieces of fruit."
"""
```
## Arrays[], dict[k:v]
Those types are compond types. We refer both of them as collections.

Rules:
1. Unlike python list, the elements in an collection must be the same type
2. Initializing a empty collection requires an explicit type. The type is immutable.
3. Unwarped variables cannot be appended.

```swift
var myIntList = [1,2,3,4]
// Rule 1 Error
myIntList.append("5")

// Rule 2
let myDoubleList = [Int]()
let myDoubleStringDict = [Double:String](:)
// Rule 2 Error
let myList2 = ()
let myDict2 = (:)

// Rule 3
var b = Int("5")
// Rule 3 Ok
myIntList.append(b!)
// Rule 3 Error Value of optional type 'Int?' not unwrapped;
myIntList.append(b)
```
## ```nil```
### ```?```
compond optional type for ```nil```. To indicate a value can also be ```nil```.

Rules:
1. ```nil``` is ```Nil``` type that is different to any types.
2. ```nil``` must have a contextual type
```swift
// OK
var myNilString1: String? = "Sam"
var myNilString2: String? = nil

// Rule1 Error
var myNilStringE: String = nil
var myString: String = "Sam"
myString = nil // nil cannot be assigned to type 'String'

// Rule2 Error
var myNil = nil //'nil' requires a contextual type
```

## Explicit Type Conversion

1. ```Type(var1)``` will convert the right value of var1 to ```Type``` type.
2. ```\(expression)``` will evaluate the expression then output. However, remember to use ```"\(expr)"``` - with double quotient

```swift
// Type1(type2_var)
var myInt2 = 42
// OK
print(String(myInt2))   // 42
// Error
print(myInt2)
// OK
print("\(2+myInt2)")    // 44
```

# Control Flow
Rules:
1. Not parentheses ()
2. Must have square brackets {}

## Loop Flow
```for-in, while, repeat-while```
### ```for-in```
Rules:
1. inner variable don't need to be defined explicitly.
2. loop over arrays 
```swift
for i in arrayTmp{
    //...
}
```
3. loop over dictionaries 
```swift
for (k,v) in dictTmp{
    for i in v{
        //...
    }
    //...
}
```
4. in-time number range generator 
```swift
for i in 0..<4{
    //...
    print(i)    // 0, 1, 2, 3
}

for j in 0...4{
    //...
    print(j)    // 0, 1, 2, 3, 4
}
```
Example:
```swift

// Rule 1 loop over an array
let myIntList2 = [1,2,3,4]
for i in myIntList2{
    print(String(i)) // 1\n 2\n 3\n 4\n
}

// Rule 2 loop over a dictionary
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
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
### ```while```
```swift
var i = 1
while i < 5 {
    print(i)
    i += 1
}
```
### ```repeat-while```
```swift
var i = 1
repeat {
    print(i)
    i += 1
}while i < 5
```
## Condition Flow
```??, if-else, switch-case```

### ```??```
Rule:
1. must have whitespace before and after.
```swift 
let C = A ?? B
``` 
same to C++ language
```c++
decltype(A) C = A?A:B;
```
considering whether ```A``` is ```nil``` or not.
### ```if-else```
```swift
let i = 5
if i < 5{
    print("i<5")
}else{
    print("i>=5")
}
```
### ```switch-case```
Rules:
1. ```case``` accepts constant literal/ literals
2. ```case``` supports pattern filter ```let x where CEXPR```
3. Don't need break. But it must have ```default:```. Otherwise, it will raise ```Switch must be exhaustive``` error
```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")  
case let x where x.hasSuffix("pepper"):         // Rule2
    print("Is it a spicy \(x)?")
default:                                        // Rule3
    print("Everything tastes good in soup.")
}
```

# Function and Closure
## Function ```func```
```swift
func fct_name(var1_label: Type, var2_label: Type) -> return_type{
    // ....
    return var3_of_return_type
}
```
Rules:

0. Input variable is ```let``` constant.
```swift
// Rule 4 Error
func add5(x:Int)->Int{
    x += 5
    return x    // 'x' is a 'let' constant
}
```
1. when calling the function, the label must be included unless
    
    1.1 use underscore ```(_ var1_label:Type)``` to use no argument label
    
    1.2 use custom label ```(cust_lable var2_label:Type)``` to use custom label.

```swift
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
// OK
print(greet(person: "Bob", day: "Tuesday"))
// Error
print(greet("Bob",day: "Tuesday"))
// Hello Bob, today is Tuesday.

// Rule 1*
func greetJ(_ person: String, on day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greetJ("John", on: "Wednesday")
// Hello Johnm today is Wednesday.

```

2. Compound value
    
    2.1 use ```avar_label: [Type]``` to indicate an input of ```Array[Type]``` 
    2.2 use tuple ``` -> (var1_label: Type, var2_label: Type)``` to return compound values. Note that the inner varable names can be different as long as they share the same sturcture.
```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sums = 0
    
    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        // Rule 2.2 
        sums += score
    }
    
    return (min, max, sums)
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.sum)   // use label to access the member
print(statistics.2)     // use index 0-N to access the member
```

3. Nested value: function can be nested. The scope of variables work as that of ```c language``` without declaration.
```swift
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()             // 15
```

4. Functions are a first-class type. 
    
    4.1 It can be returned from another function.

    4.2 It can be input variable.
```swift
// Rule 4.1
func add5() -> ((Int)-> Int){
    func add(x:Int)->Int{
        return x+5
    }
    return add
}
var add5 = add5()
add5(7) // 12

// advanced the 
func add_base(base:Int) -> ((Int)-> Int){
    func add(x:Int)->Int{
        return x+base
    }
    return add
}
var add5 = add_base(base: 5)
print(add5(7)) // 12


// Rule 4.2
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)

```
## Closure ```{}```
Rules
1. Functions are actually a special case of closures. similar to ```lambda``` in ```c++```. Warped by ```{... in ...}```

    1.1 for simple cases, the input type and the return type can be omited.

    1.2 A closure passed as the last argument to a function can appear immediately after the parentheses. When a closure is the only argument to a function, you can/must omit the parentheses entirely
```swift
var numbers = [20, 19, 7, 12]

numbers.map({ (number: Int) -> Int in
    let result = 3 * number
    return result
})

//  Rule 5.1 simple case
numbers.map({number in 3*number})

// Rule 5.2
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)
// 20, 19, 12, 7
```

# Objects and Classes
## ```class```
Rules:
1. create just like any oop language
```swift
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```
### Properties
Rues:
1. same to variables
2. The properties are declared outside any functions.
```swift
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```
3. properties can have ```get{},set{}```, the ```newValue``` is implicit variable which has the same value as the new assignment.

    3.1 omit ```set{}``` to make the property read-only

```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }
    
    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }
    
    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)   // 9.3
triangle.perimeter = 9.9
print(triangle.sideLength)  // 3.3
```

4. use ```willSet{},didSet{}``` to keep the changes among different properties.
```swift
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    var square: Square {
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
print(triangleAndSquare.square.sideLength)          // 10   
print(triangleAndSquare.triangle.sideLength)        // 10
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)        // 50
```
### Methods
Rules:
1. Initializer are optional. Need ```self```
```swift
class NamedShape {
    var numberOfSides: Int = 0
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

## Subclass ```class B: A``` 
Rules:
1. Accessing super properties need ```super.xxx```
2. Overriding functions must be declared with ```override func```
```swift
class Square: NamedShape {
    var sideLength: Double
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }
    
    func area() -> Double {
        return sideLength * sideLength
    }
    
    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```
### additional usage of ```?```
Rule:
1. If anything before ```?``` is ```nil```, the rest is not needed to evaluate. Otherwise, the optional value is unwrapped, an everything after the ```?``` acts on the unwarpped value.
```swift
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength
```

# Enumerations and Structures
## ```enum```
Rules:
1. the first case should be assigned and it is the start ID. Default is 0. 
2. ```self.rawValue``` stands for the variable ID, counted from the first ID.
3. ```enum``` can have methods
```swift
enum Rank: Int{
    case ace = 1
    case two, three, four, five, six, sven, eight, nine, ten // 2,3,4,...,10
    case jack, queen, king
    func simpleDescription() -> String{
        switch self{
            case .ace:
                return "ace"
            case .jack:
                return "jack"
            case .queen:
                return "queen"
            case .king:
                return "king"
            default:
                return String(self.rawValue)
        }
    }
}

let ace = Rank.ace
print(ace.rawValue) // 1
```

4. use ```init?(rawValue:)``` initializer to make an instance/case of an enumeration from a raw value. If there is no matching ID, return ```nil```
```swift
// OK
if let convertedRank = Rank(rawValue: 3) {
    let threeDescription = convertedRank.simpleDescription()
    print(threeDescription) // 3
}
// OK but no output, it does not enter the if-else control
if let noMatchedRank = Rank(rawValue: 0) {
    let nilDescription = noMatchedRank.simpleDescription()
    print(nilDescription)
}
```
5. it is impossible to add new case while running. So the ```switch-case``` control can omit ```default``` if every case is listed.
```swift
enum Suit {
    case spades, hearts, diamonds, clubs
    func simpleDescription() -> String {
        switch self {
        case .spades:
            return "spades"
        case .hearts:
            return "hearts"
        case .diamonds:
            return "diamonds"
        case .clubs:
            return "clubs"
        }
    }
}
let hearts = Suit.hearts
let heartsDescription = hearts.simpleDescription()  // "hearts"
```
6. If the type of enumerate is not important, it can be omit. Then the ```rawValue``` property can not be accessed.
```swift
// Rule 6 Error
let heartsRaw = hearts.rawValue
```

7. The case can have Type
```swift
enum ServerResponse {
    case result(String, String)
    case failure(String)
}
 
let success = ServerResponse.result("6:00 am", "8:09 pm")
let failure = ServerResponse.failure("Out of cheese.")
 
switch success {
case let .result(sunrise, sunset):
    print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
case let .failure(message):
    print("Failure...  \(message)")
}
// "Sunrise is at 6:00 am and sunset is at 8:09 pm."
```

## ```struct```
Rules:
1. Structures support many of the same behaviors as classes, including methods and initializers. 
2. The difference is ```struct``` is passed by duplication/copied while ```class``` is passed by reference.
```swift
struct Card {
    var rank: Rank
    var suit: Suit
    func simpleDescription() -> String {
        return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
    }
}
let threeOfSpades = Card(rank: .three, suit: .spades)
let threeOfSpadesDescription = threeOfSpades.simpleDescription()

```



# Protocols and Extensions
## ```protocol```
like ```interface/header``` in ```java/c```

Rules:
1. only need to refer the type and method name
2. ```mutating func fct_name()``` to be used in define ```struct``` to mark a method that modifies the structure. In ```class``` definitition, it is not needed because methods on a class can alaways modify the class.
```swift
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}
// class
class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "  Now 100% adjusted."
    }
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription  // A very simple class. Now 100% adjusted.

// struct
struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription
```
3. The ```protocol``` itself can be used as a type. But it must be initialized by a class instance. However, it limits the properties and function to the original ```protocol``` rather than the assigned new class that uses this ```protocol```
```swift
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)  
// "A very simple class. Now 100% adjusted."
// Error
print(protocolValue.anotherProperty)  // No member 'anotherProperty'
```
## ```extension```
Rules:
1. use ```extension``` to add functionality to an existing type, such sa new ```method``` and computed ```properties```
```swift
// it is not necessary to use a super protocol
extension Int: ExampleProtocol{
    var simpleDescription: String{
        return "The number \(self)"
    }
    mutating func adjust(){
        self += 42
    }
}
```

print(7.simpleDescription)  // The number 7


# Error Handling
## ```enum``` and ```throw``` error in a ```throws``` function
Rules:
1. define new ```myError``` use ```Error``` protocol
```swift
enum PrinterError: Error {
    case outOfPaper
    case noToner
    case onFire
}
```
2. use ```throw``` to throw an error 
3. use ```throws``` to mark a function that can throw an error. If you ```throw``` an error, the function returns immediately and the code that called the function handles the error.
```swift
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}
```
## ```do-try-catch``` to handle errors
Rules:
1. In the ```catch``` block, the error is autpmatically given the name ```error``` unless you give it a different name.
```swift
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
    print(printerResponse)
} catch {
    print(error)
}
```
2. the ```catch``` behaves as ```case```. The last ```catch``` is optionally mandatory. Otherwise, uncatched ```Error``` will interupt the application.
```swift
do {
    let printerResponse = try send(job: 1440, toPrinter: "Gutenberg")
    print(printerResponse)
} catch PrinterError.onFire {
    print("I'll just put this over here, with the rest of the fire.")
} catch let printerError as PrinterError {
    print("Printer error: \(printerError).")
} catch {
    print(error)
}
```

3. use ```try? fct_name(...)``` to assign a variable. If there is an error, it is ```nil```.
```swift
// OK
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
// OK but nil
let printFailure = try? send(job: 1885, toPrinter: "Never Has Toner")
```

## ```defer``` to run code whenever it throws an error or not.
Rule:
1. ```defer``` to write *cleanup* code next to setup.
```swift
var fridgeIsOpen = false
let fridgeContent = ["milk", "eggs", "leftovers"]
 
func fridgeContains(_ food: String) -> Bool {
    fridgeIsOpen = true
    defer {
        fridgeIsOpen = false
    }
    
    let result = fridgeContent.contains(food)
    return result
}
fridgeContains("banana")
print(fridgeIsOpen)     // false
```

# Generics
## ```<T>```

like ```<T>``` in ```c++```

Rules:
1. use ```<>``` angle brackets to make a generic function or type. The name is not that meanful.
```swift
func makeArray<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result = [Item]()
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    return result
}
makeArray(repeating: "knock", numberOfTimes: 4)
// ['knock','knock','knock','knock'] 
```

2. generic forms of functions and methods, as well as classes, enumerations, and structures.
``` swift
// generic enum
enum OptionalValue<T> {
    case none
    case some(T)
}
var possibleInteger: OptionalValue<Int> = .none
print(possibleInteger)              // none
possibleInteger = .some(100)
print(possibleInteger)              // some(100)
```

## ```where```
Rule:
1. use ```where``` right before the body to specify a list of requirements. For example,
    * require the type to implement a protocol
    * require two types to be the same
    * require a class to have a particular superclass.

2. use ```<T: Protocol>``` is equal to ```<T>... where T: Protocol```
```swift
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
    where T.Iteraator.Element: Equatable, T.Iterator.Element == U.Iterator.Element{
        for lhsItem in lhs{
            for rhsItem in rhs{
                if lhsItem == rhsItem{
                    retrun true
                }
            }
        }
        return false
    }
anyCommonElements([1,2,3],[3])
```