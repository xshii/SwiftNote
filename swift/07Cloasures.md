- [Introduction](#introduction)
    - [mostly used methods](#mostly-used-methods)
- [Closure Expressions](#closure-expressions)
    - [Closure Expression Syntax](#closure-expression-syntax)
- [Trailing Closures](#trailing-closures)
- [Capturing Values](#capturing-values)
- [Closures Are Reference Types](#closures-are-reference-types)
- [Escaping Closures ```@escape``` ???????](#escaping-closures-escape)
- [Autoclosure???](#autoclosure)
# Introduction
like ```lambda``` in ```C++```. 

* The differece is in ```Swift```, Closures can capture and store references to any constants and variables from the context in which they are defined.

* Global and nested functions are actually special cases of closures.
    * Global functions are closures that have a name and do not capture any values.
    * Nested functions are closures that have a name and can capture values from their enclosing function.
    * Closure expressions are unnamed closures written in a lightweight syntax that can capture values from their surrounding context.

* ```Swift```'s closure expressions have a clean, clear style, with optimizations that encourage brief, clutter-free syntax in common scenarios.
    * Inferring parameter and return value types from context
    * Implicit returns from single-expression closures
    * Shorthand argument names
    * Trailing closure syntax
*  It is particularly useful when you work with functions or methods that take functions as one or more of their arguments.
## mostly used methods
* ```inout```
* ```{(label:Type,...)-> Reutrn_type in statement}```
* ```{$0>$1}```
* ```{by:>}```
* ```{func_caller}``` to have lazy evaluation properties.
* ```@escape```
* ```@autoclosure```
# Closure Expressions
## Closure Expression Syntax
Rule:
1. 
```swift
{ (parameters) -> return_type in
    statements
}
```
2. It can have ```inout``` parameters, but they can’t have a default value. 
3. Variadic parameters can be used if you name the variadic parameter. 
4. Tuples can also be used as parameter types and return types.
```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]

// ordinary way
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
// ["Ewa", "Daniella", "Chris", "Barry", "Alex"]

// easy way
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2
})
## Inferring Type From Context
Rule:
1. it can infer the type while running
```swift
inferdReversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```
5. Implicit Returns from Single-Expression Closures
```swift
singleExpReversedNames = names.sorted(by:{s1, s2 in s1>s2})
```
6. it can use shorthand argument names, ```$0, $1, $2...```, the first, second and third ```Type``` arguments.
```swift
shorthandReversedNames = names.sorted(by: { $0 > $1 } )
```
7. Use operator methods
```swift
optReversedNames = names.sorted(by: >)
```

# Trailing Closures
If you need to pass a closure expression to a function as the function’s final argument and the closure expression is long, it can be useful to write it as a trailing closure instead. 

you don’t write the argument label for the closure as part of the function call.

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // function body goes here
}
someFunctionThatTakesAClosure(closure: {
    // closure's body goes here
})
 
// Here's how you call this function with a trailing closure instead:
 
someFunctionThatTakesAClosure() {
    // trailing closure's body goes here
}
```
Rule:
1. 
```swift
trailingReversedNames = names.sorted() { $0 > $1 }

// example2
et strings = numbers.map { (number) -> String in
    var number = number
    var output = ""
    repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
    return output
}
// strings is inferred to be of type [String]
// its value is ["OneSix", "FiveEight", "FiveOneZero"]
```
# Capturing Values
it is not an isolated environment.

# Closures Are Reference Types
Rule:
1. pass a ```func``` is same to pass it by reference. It will keep the current state.

# Escaping Closures ```@escape``` ???????
```swift
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
func someFunctionWithNonescapingClosure(closure: () -> Void) {
    closure()
}
 
class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}
 
let instance = SomeClass()
instance.doSomething()
print(instance.x)   // "200"
 
completionHandlers.first?()
print(instance.x)   // "100"
```


# Autoclosure???
An autoclosure is a closure that is automatically created to wrap an expression that’s being passed as an argument to a function.
An autoclosure lets you delay evaluation, because the code inside isn’t run until you call the closure.
Rules:
1. use ```{func_caller}``` to make it an autoclosure
```swift
var customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
print(customersInLine.count)    // "5"
 
let customerProvider = { customersInLine.remove(at: 0) }
print(customersInLine.count)    // "5"
 
print("Now serving \(customerProvider())!") // "Now serving Chris!"
print(customersInLine.count)    // "4"
```
    Note that the type of ```customerProvider``` is not ```String``` but ```() -> String``` — a function with no parameters that returns a string.


2. ????????If you want an autoclosure that is allowed to escape, use both the ```@autoclosure``` and ```@escaping``` attributes. 
```swift
/ customersInLine is ["Barry", "Daniella"]
var customerProviders: [() -> String] = []
func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
    customerProviders.append(customerProvider)
}
collectCustomerProviders(customersInLine.remove(at: 0))
collectCustomerProviders(customersInLine.remove(at: 0))
 
print("Collected \(customerProviders.count) closures.")
// Prints "Collected 2 closures."
for customerProvider in customerProviders {
    print("Now serving \(customerProvider())!")
}
// "Now serving Barry!"
// "Now serving Daniella!"
```
