1. [Introduction](#introduction)
    1. [frequently used syntax](#frequently-used-syntax)
2. [Stored Properties](#stored-properties)
    1. [constant stored properties](#constant-stored-properties)
    2. [Lazy stored properties](#lazy-stored-properties)
    3. [Stored Properties and Instance Variables](#stored-properties-and-instance-variables)
3. [Computed Properties](#computed-properties)
    1. [Shorthand Setter Declaration `newValue`](#shorthand-setter-declaration-newvalue)
    2. [Read-Only Computed Properties](#read-only-computed-properties)
4. [Property Observers](#property-observers)
5. [Global and Local Variables](#global-and-local-variables)
6. [Type properties](#type-properties)
    1. [Type Property Syntax-`static`](#type-property-syntax-static)
    2. [Querying and Setting Type Properties](#querying-and-setting-type-properties)
# Introduction
* Stored properties store constant and variable values as part of an instance, whereas computed properties calculate (rather than store) a value.   
* Computed properties are provided by classes, structures, and enumerations. Stored properties are provided only by classes and structures.
* Stored and computed properties are usually associated with instances of a particular type. However, properties can also be associated with the type itself. Such properties are known as type properties. like ```staticmethod```
* **property observers**: monitor changes in a property's value, which you can respond to with custom actions. They can observe even inherent properties.
## frequently used syntax
* `lazy var prop_temp`
* `get{return}`, `set(){}`,`newValue`
* `newValue`, `oldValue`
* `willSet(){}`, `didSet{}`
* `static`, `class`
# Stored Properties
Stored properties belong to a class or structure, which can be either variable (`var`) or constant (`let`)
## constant stored properties
Rules:
1. `let` properties are fixed after initialization
```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// the range represents integer values 0, 1, and 2
rangeOfThreeItems.firstValue = 6
// the range now represents integer values 6, 7, and 8
```

2. properties of `let` structure instance are all invariant. But this is not true for classes instance, which are reference types.

## Lazy stored properties
Lazy properties are useful when the initial value for a property is dependent on outside factors whose values are not known until after an instance's initialization is complete.

Or, it is computationally expensive that the evaluation should not be performed until it is needed.

Rules:
1. write the `lazy` modifier before its declaration
2. a lazy property must be a `var` (variable).
```swift
class DataImporter {
    /*
     DataImporter is a class to import data from an external file.
     The class is assumed to take a nontrivial amount of time to initialize.
     */
    var filename = "data.txt"
    // the DataImporter class would provide data importing functionality here
}
 
class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
    // the DataManager class would provide data management functionality here
}
 
let manager = DataManager()
manager.data.append("Some data")
manager.data.append("Some more data")
// the DataImporter instance for the importer property has not yet been created
```
3. the uninitialized lazy properties might be initialized more than once in a multiple threads environment.

## Stored Properties and Instance Variables
Unlike `obj-C`, a `Swift` property does not have a corresponding instance variable, and the backing store for a property is not accessed directly. All information about the property - including its name, type, and memory management characteristics - is defined in a single location as part of the type's definition.

# Computed Properties
Classes, structures and enumerations can define *computed properties*, which do not actually store a value. Instead, they provide a getter and an optional setter to retrieve and set other properteis and values indirectly.

Rules:
1. `get{return ...}`
2. `set(new_instance){}` no need to init this `new_instance`
```swift
struct Point {
    var x = 0.0, y = 0.0
}
struct Size {
    var width = 0.0, height = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}
var square = Rect(origin: Point(x: 0.0, y: 0.0),
                  size: Size(width: 10.0, height: 10.0))
let initialSquareCenter = square.center
square.center = Point(x: 15.0, y: 15.0)
print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// Prints "square.origin is now at (10.0, 10.0)"

```

## Shorthand Setter Declaration `newValue`
the default name of inputs is `newValue`

## Read-Only Computed Properties
Even they are read-only, you have to use `var` to define them.

Rules:
1. no need for `get` keyword, which is substituted by `{return}`
```swift

struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
        return width * height * depth
    }
}
let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// Prints "the volume of fourByFiveByTwo is 40.0"
```

# Property Observers
The property observers are called every time a property's value is set, even if the new value is the same as the property's current value.

You can add property observers to any *stored properties* you define, except for *lazy stored properties*.

You can also add property observers to any inherited property (whether stored or computed) by overriding the property within a subclass. You don’t need to define property observers for nonoverridden computed properties, because you can observe and respond to changes to their value in the computed property’s setter.

* `willSet` is called just before the value is stored. 
    * `newValue`  is a default keyword for the new value.
* `didSet` is called immediately after the new value is stored.
    * `oldValue` is a default keyword for the old value.
```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps

```
note:
* for a `in-out` parameter, the `willSet` and `didSet` observers are always called.

# Global and Local Variables
Global variables are variables that are defined outside of any function, method, closure, or type context. Local variables are variables that are defined within a function, method, or closure context.

Global constants and variables are always computed lazily. Unlike lazy stored properties, global constants and variables do not need to be marked with the lazy modifier.

Local constants and variables are never computed lazily.

# Type properties
like `static const` in `C` or `static` variable in `C`.

Unlike stored instance properties, you must always give stored type properties a default value. This is because the type itself does not have an initializer that can assign a value to a stored type property at initialization time.

Stored type properties are lazily initialized on their first access. They are guaranteed to be initialized only once, even when accessed by multiple threads simultaneously, and they do not need to be marked with the lazy modifier.

## Type Property Syntax-`static`
Rules:
1. You define type properties with the `static` keyword.
2. For computed type properties for class types, you can use the `class` keyword instead to allow subclasses to override the superclass’s implementation. 

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
}
enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 6
    }
}
class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}
```

## Querying and Setting Type Properties
like usual

```swift
struct AudioChannel {
    static let thresholdLevel = 10
    static var maxInputLevelForAllChannels = 0
    var currentLevel: Int = 0 {
        didSet {
            if currentLevel > AudioChannel.thresholdLevel {
                // cap the new audio level to the threshold level
                currentLevel = AudioChannel.thresholdLevel
            }
            if currentLevel > AudioChannel.maxInputLevelForAllChannels {
                // store this as the new overall maximum input level
                AudioChannel.maxInputLevelForAllChannels = currentLevel
            }
        }
    }
}
```

```swift
leftChannel.currentLevel = 7
print(leftChannel.currentLevel)
// Prints "7"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "7"
```
```swift
rightChannel.currentLevel = 11
print(rightChannel.currentLevel)
// Prints "10"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "10"
```