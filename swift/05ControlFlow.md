- [Introduction](#introduction)
    - [mostly used methods:](#mostly-used-methods)
- [```for-in``` loop](#for-in-loop)
- [```while``` loops](#while-loops)
    - [```repeat-while```](#repeat-while)
- [Conditional Statements](#conditional-statements)
    - [```if-else```](#if-else)
    - [```switch-case-default```](#switch-case-default)
        - [VaLue Bindings](#value-bindings)
- [Control Transfer Statements](#control-transfer-statements)
    - [```continue```](#continue)
    - [```break```](#break)
    - [```fallthrough```](#fallthrough)
- [Labeled Statements](#labeled-statements)
- [Early Exit ```guard```](#early-exit-guard)
- [Checking API Availability ```#available()```](#checking-api-availability-available)
# Introduction
```for-in, while, if-else, guard, switch-case, repeat-while```

```break, continue```

* ```Swift```'s ```for-in``` loop makes it easy to iterate over Array, Dictionary, range, String and other sequences.
* ```Swift```'s ```switch-case``` statement is more powerful than its counterpart in many ```C```-like languages. 
    * ```case```s can match different partterns, including interval matches, tuples, and casts to a specific type.
    * Matched values can be bound to temporary constants or variables for use within the ```case```'s body.
    * complex matching conditions can be expressed with a ```where``` clause for each case.

## mostly used methods:
* ```stride(from:to:by:)```
* ```case (_,0)```, ```case (let x,0)```, ```case (let x,0), (0, let x)```
* ```case: (let x, let y) where x==y```
* ```label_name : loop...{}```
* ```break, continue,fallthrough```
* ```break label_name```
* ```#available(platform version, ..., *)```
* ```guard let someVar = someOptional else{... return ...}```
# ```for-in``` loop
Rules:
1. Use ```for-in``` loop to iterate over a sequence, such as 
    * items in an array, 
    * ranges of numbers, 
    * characters in a string.
    * ```(k,v)``` tuple in a dictionary... It is not orderred!
2. ```item``` is implicitly declared simply by its inclusion in the loop declaration. no need for a ```let``` declaration keyword - it is forbidden. If you like, you can use ```var``` - still no need to do this.
```swift
// Rule2 Error
var a = [1,2,3,4,5]
for let b in a{
    //... it will not compile because of the incorrect grammar. 
}
```
3. If the ```item``` is not needed, you can use ```_```.
```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// "3 to the power of 10 is 59049"
```
4. use ```stride(from:to:by:)``` to generate special range. It will not include the last one if it exceeds the last one.
```swift
for b in stride(from:1, to: 3, by:1){
    print(b)    // 1,2,3
}
for b in stride(from:1, to: 3, by:3){
    print(b)    // 1
}
```

# ```while``` loops
Rule:
1. Like its counterpart in ```C```

## ```repeat-while```
Rule:
1. Like ```do-while``` in ```C```. It performs a single pass through the loop block first before checking the condition in ```while```

# Conditional Statements
* you use the ```if-else``` statement to evaluate simple conditions with only a few possible outcomes. 
* The ```switch-case``` statement is better suited to more complex conditions with multiple possible permutations and is useful in situations where pattern matching can help select an appropriate code branch to execute.


## ```if-else```
Rule:
1. like that in ```C```


## ```switch-case-default```
Rules:
1. ```case``` can consist of one or multiple situations. Use commas ```,``` to seperate.
2. No need to ```break``` to interupt, unlike ```C```. But you can if you wish.
3. ```default``` must always appear last.
    
    3.1 for the ```enum``` type, ```default``` can disappear.

4. the body of ```case``` **must** contain at least one executable statement.
5. Interval matching: ```case 0..<5```
6. Tuple matching: ```case (_,0)```
```swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("\(somePoint) is at the origin")
case (_, 0):
    print("\(somePoint) is on the x-axis")
case (0, _):
    print("\(somePoint) is on the y-axis")
case (-2...2, -2...2):
    print("\(somePoint) is inside the box")
default:
    print("\(somePoint) is outside of the box")
}
// "(1, 1) is inside the box"
```
7. If multiple matches are possible, the first matching case is always used. 

### VaLue Bindings
Rules:
1. Use ```let x``` to indicate a uncertain case.
```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// "on the x-axis with an x value of 2"
```
2. Use ```where``` to check for additonal conditions
```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// "(1, -1) is on the line x == -y"
```
3. Compond cases can also include value bindings. They can have the same identifier.
```swift
let stillAnotherPoint = (9, 0)
switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("On an axis, \(distance) from the origin")
default:
    print("Not on an axis")
}
// "On an axis, 9 from the origin"
```

# Control Transfer Statements
```continue, break, fallthrough, return, throw```
## ```continue```
As its counterpart in ```C```

## ```break```
Rule:
1. because ```case``` body must have an executable sentence, always use ```break``` to ignore a ```case```.

## ```fallthrough```
Rule:
1. Use ```fallthrough``` in the ```switch-case``` to implement ```C```-style ```switch-case```. It will still interupt by explicit ```break``` before it.
```swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// "The number 5 is a prime number, and also an integer."
```

# Labeled Statements
Rule:
1. ```label_name : loop...{}```, useful when you are in a nested loop/condition
```swift
gameLoop: while square != finalSquare {
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    switch square + diceRoll {
    case finalSquare:
        // diceRoll will move us to the final square, so the game is over
        break gameLoop
    case let newSquare where newSquare > finalSquare:
        // diceRoll will move us beyond the final square, so roll again
        continue gameLoop
    default:
        // this is a valid move, so find out its effect
        square += diceRoll
        square += board[square]
    }
}
print("Game over!")
```

# Early Exit ```guard```
Rules:
1. Unlike an ```if``` statement, a ```guard``` statement always has an ```else``` clause - the code inside the ```else``` clause is executed if the conditon is not true.
2. The ```guard``` implicitly provides a closing brace afterwards. Every new assignments/ creation will be considerred to hold if the ```guard``` statement is ```True```
```swift
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        return
    }
    
    print("Hello \(name)!")
    
    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }
    
    print("I hope the weather is nice in \(location).")
}
 
greet(person: ["name": "John"])
// "Hello John!"
// "I hope the weather is nice near you."
greet(person: ["name": "Jane", "location": "Cupertino"])
// "Hello Jane!"
// "I hope the weather is nice in Cupertino."
```
# Checking API Availability ```#available()```
Rules:
1. Normally used in ```if-else``` statement or ```guard``` statement
```swift
if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```
2. ```#available(platform_name version,...,*)``` 
    
    * ```iOS```, ```macOS```, ```watchOS```, ```tvOS```
