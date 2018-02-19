1. [Introduction](#introduction)
    1. [Syntax](#syntax)
2. [Defining a Base Class](#defining-a-base-class)
3. [Subcalssing](#subcalssing)
4. [Overriding - `super`,](#overriding---super)
    1. [Overriding Methods - `override func`](#overriding-methods---override-func)
    2. [Overriding Properties](#overriding-properties)
        1. [Overriding Property Getters and Setters - `override var`](#overriding-property-getters-and-setters---override-var)
        2. [Overriding Property Observers - `override var`](#overriding-property-observers---override-var)
5. [Preventing Overrides - `final`](#preventing-overrides---final)
# Introduction
A class can inherit methods, properties, and other characteristics from another class.
## Syntax
* `override func`
* `override var`
* `final`
* `class sub: super`
* `self`, `super`
# Defining a Base Class
without specifying a superclass.

A `Vehicle` example
```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // do nothing - an arbitrary vehicle doesn't necessarily make a noise
    }
}
```
create a new `Vehicle` instance
```swift
let someVehicle = Vehicle()
print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour

```

no other useful information
# Subcalssing
Rule:
1. nominate a superclass like a type identifier
2. subclass can access all properties and methods of superclass without declaration.
```swift
class SomeSubclass: SomeSuperclass {
    // subclass definition goes here
}
```
a `Bicycle` subclass of `Vehicle`
```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```

# Overriding - `super`, 
Rules:
1. An overridden method named `someMethod()` can call the superclass version of `someMethod()` by calling `super.someMethod()` within the overriding method implementation
2. An overridden property called `someProperty` can access the superclass version of `someProperty` as `super.someProperty` within the overriding getter or setter implementation.
3. An overridden `subscript` for `someIndex` can access the superclass version of the same subscript as `super[someIndex]` from within the overriding subscript implementation.

## Overriding Methods - `override func`
a `Train` subclass overrides the `makeNoise()` function of `Vehicle`
```swift
class Train: Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}
// usage
let train = Train()
train.makeNoise()
// Prints "Choo Choo"
```

## Overriding Properties
You can override an inherited instance or type property to provide your own custom `getter` and `setter` for that property, or to add property `observers` to enable the overriding property to observe when the underlying property value changes.

### Overriding Property Getters and Setters - `override var`
a `Car` subclass overrides `description` property of `Vehicle`
```swift
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}

```

### Overriding Property Observers - `override var`
a `AutomaticCar` overrides the `currentSpeed` property observer of `Car`
```swift
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}
// example usage
let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// AutomaticCar: traveling at 35.0 miles per hour in gear 4

```

# Preventing Overrides - `final`
 prevent a method, property, or subscript from being overridden by marking it as `final`.

 You can mark an entire `class` as `final` by writing the `final` modifier before the `class` keyword in its class definition (`final class`).

 Any attempt to subclass a final class is reported as a compile-time error.