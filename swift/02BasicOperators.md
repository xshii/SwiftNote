
- [Terminology](#terminology)
- [Assigment Operator ```=```](#assigment-operator)
    - [compond assigment operator ```+=, -, *=,/=, %=```](#compond-assigment-operator)
- [Arithmetic Operators ```+,-,*,/,%```](#arithmetic-operators)
- [Comparison Operators ```==,!=,>,<,>=,<=, ===,!==```](#comparison-operators)
- [Ternary conditonal Operator ```a? b:c```](#ternary-conditonal-operator-a-bc)
- [Nil-Coalescing Operator ```a ?? b```](#nil-coalescing-operator-a-b)
- [Range Operators](#range-operators)
- [Logical Operators ```!, &&, ||```](#logical-operators)

# Introduction
```Swift``` supports most standard ```C``` operators and improves several capabilityies to eliminate common coding errors.
1. the assignment operator (```=```) doesn't return a value, to prevent it from being mistakenly used when the equal to operator(```==```) is intended.
2. Arithmetic operators (```+,-,*,/,%,...```) detect and disallow value overflow.
    
    2.1 You can opt in to value overflow behavior by using ```Swift```'s overflow operators.

3. ```Swift``` provides range operators that aren't found in ```C```, ```a..<b``` and ```a...b```

...


# Terminology
Operators are unary, binary or ternary:

Rules:
1. Unary operators must appear without whitespace.
2. Binary operators must appear between two targets with any whitespaces.
3. Only one ternary operators ```a ?b:c```. It must have a whitespace between ```a``` and ```?``` otherwise, it is considered to be an optional indicator
```swift
// Rule1 Error
var myMinusOne = - 1

// Rule2 Ok
var myThree = 1 +          2

// Rule3 Error
var myFourOrThree = 1+2>2?4:3
// Rule3 OK
var myTwoOrThree = 1+2>2 ?2:3
```

# Assigment Operator ```=```
Rule:
1. Type safety rules!
2. the assignment operator in ```Swift``` does not itself return a value.
```swift
// Rule2 Error
let (x,y) = (1,2)

if x = y{
    // This is not valid
}
```
## compond assigment operator ```+=, -, *=,/=, %=```
Rule:
1. The compond assignment operators don't return a value
```swift
// Rule1 Error
var a = 1
let b = a += 2
```

# Arithmetic Operators ```+,-,*,/,%```

Rules:
1. Type conversion only happens when the operands are not explicitly assigned with a type.
2. ```Int/Int``` will return a truncated ```Int```,like  ```C```
3. ```+``` supports ```String``` concatenation.
4. The remainder of a negative integer is also a non-negative integer.
```swift
// Rule1 OK
let myThreePointOne = 3 + 0.1       // 3.1
// Rule1 Error
let myFour = Int(1)+UInt8(3)

// Rule2 
let myOne = 3/2                     // 1

// Rule3
"Hello, " + "World!"

// Rule4
-9%4                                // -1 : -9 = (-4*2) + (-1)
```

# Comparison Operators ```==,!=,>,<,>=,<=, ===,!==```
Rules:
1. ```==,!=,>,<,>=,<=, ``` like those in ```C```, mostly used in ```if``` statement.
2. ```===,!==``` are used to test whethere two object references both refer to the same object instance.
3. when comparing tuples, it compares elementwisely.
4. ```Bool``` does not support comparsion operators.

# Ternary conditonal Operator ```a? b:c```
```cond_expr? true_return: false_return``` same as ```C```. 

# Nil-Coalescing Operator ```a ?? b```
same as ```a != nil? a! : b```
Rule:
1. If the value of ```a``` is non-nil, the value of ```b``` is not evaluated.

```swift
let defaultColorName = "red"
var userDefinedColorName: String?   // defaults to nil
 
var colorNameToUse = userDefinedColorName ?? defaultColorName   //  "red"
```
# Range Operators
Rules:
1. The closed range operator (```a...b```) = [a,a+1,...,b]. ```a<b``` is a must. OW, the build will success but it will raise a runtime error.
2. The half-open range operator (```a..<b```) = [a,a+1,...,b-1]. This is useful when you want to iterate over a collection, because the index start from ```0``` to ```.count-1```
```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("Person \(i + 1) is called \(names[i])")
}
```
3. One-Sided ranges ```array_tmp[a...]```, ```array_tmp[...b]```. 

    3.1 It behaves like the generator in ```Python```, compiled only if needed.
```swift
for name in names[2...]{
    print(name) // Brian, Jack
}

for name in names[...2] {
    print(name) // Anna, Alex, Brian
}

// Rule3.1
let range = ...5
range.contains(7)   // false
range.contains(-1)  // true
```

# Logical Operators ```!, &&, ||```
Rules:
1. like that in ```C```. ```&&``` is same to commas ```,```
2. the ```&&, ||``` are left-associative. They evaluate the leftmost value first
3. parenthesis is allowed to group the condition expression.


