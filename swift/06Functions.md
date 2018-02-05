- [Introduction](#introduction)
    - [mostly used methods](#mostly-used-methods)
- [Defining and Calling Functions](#defining-and-calling-functions)
- [Nested Functions](#nested-functions)
# Introduction
Core features:
* ```func``` is defined with names and argument labels for each parameter. Or ```_```. 
* Parameters can provide default values to simplify function calls and can be passed as in-out parameters.
* Every function in ```Swift``` has a type. Function can also be a type.

## mostly used methods
* ```->```
* ```_```
* ```(Int, Int)?```
* ```parameter_name: Type = default_value```
* ```parameter_name: Type...```
* ```parameter_name: inout Type```
* ```func(&var_tmp)```
* ```func()-> pattern_of_returned_func```
# Defining and Calling Functions
see [00FirstGlance.md#function_and_closure](https://github.com/xshii/XSNote/blob/master/swift/00FirstGlance.md#function-and-closure)

Rules:
1. ```func``` can have no parameters.
2. ```func``` can have no return values. It does not include the return arrow (```->```) and the following return type. (Return ```void```)
```swift
func greet(person: String) {
    print("Hello, \(person)!")
}
greet(person: "Dave")   // "Hello, Dave!"
```
3. ```func``` uses ```tuple``` to return multiple values. Notice that there must be a pair of brackets. The return label is not very important unless you wish to access them via properties.
    
    3.1  OPtional typle return types ```(Int, Int)?```. It is different from ```(Int?,Int?)```. It will raise error if you use later one for a ```nil tuple```

4. add a label before the parameter name. Or, the parameter name is the label.

    4.1 use ```_``` to omit argument labels. Notice that when defining the ```func``` you still need a parameter name.

5. default parameter values need ```parameter_name: Type = default_value```. The dafault setting can appear anywhere.
```swift
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
    // If you omit the second argument when calling this function, then
    // the value of parameterWithDefault is 12 inside the function body.
}
someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6
someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12
``` 
6. Variadic parameter accepts zero or more values of a specified type including the written one. Use ```...``` after the ```Type``` immediately.

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)   // 3
arithmeticMean(3, 8.25, 18.75)  // 10
arithmeticMean()                // nana
```
7. by default, parameters are constant. Use ```inout``` keyword right before the parameter's ```Type``` to allow modification and being passed back *out* of the function. Like ```&``` in ```C``` passed by reference.

    7.1 They cannot have default values. 
    
    7.2 And variadic paramters cannot be marked as ```inout```

    7.3 when used, parameters are prefixed with ```&```
```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// "someInt is now 107, and anotherInt is now 3"
```
8. When calling the method, the order of parameter should be considered.
9. function can be types. 
    9.1 It can be type inferred. 
    
    9.2 reassigned to another function as long as they have the same parameter list and return type.
```swift
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
var mathFunction: (Int, Int) -> Int = addTwoInts
// Rule9.1 OK
let mathAddInt = addTwoInts
print("Result: \(mathFunction(2, 3))")  // "Result: 5

// Rule9.2 OK
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
mathFunction = multiplyTwoInts
print("Result: \(mathFunction(2, 3))")  // "Result: 6"
```

    9.3 Function can be return types.
```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}
```
# Nested Functions
Nested functions are hidden from the outside world by default, but can still be called and used by their enclosing function.
```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
```
