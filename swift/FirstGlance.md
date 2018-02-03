# hello world
```swift
print("Hello, World!")
``` 
# variables 
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

```swift
let myIntList2 = [1,2,3,4]
for i in myIntList2{
    print(String(i)) // 1\n 2\n 3\n 4\n
}
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
```?, ??, if-else, switch-case```
### ```?```
compond optional type for ```nil```. To indicate a value can also be ```nil```.

Rules:
1. ```nil``` is ```Nil``` type that is different to any types.

```swift
// OK
var myNilString1: String? = "Sam"
var myNilString2: String? = nil
// Error
var myNilStringE: String = nil
var myString: String = "Sam"
myString = nil // Error: Nil cannot be assigned to type 'String'
```
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


### 