- [Introduction](#introduction)
    - [mostly used syntax](#mostly-used-syntax)
- [Comparing Classes and Structures](#comparing-classes-and-structures)
    - [Things In Common](#things-in-common)
    - [Things only Classes have](#things-only-classes-have)
    - [Things only Structures have](#things-only-structures-have)
- [Definition Syntax](#definition-syntax)
- [Class and Structure Instance](#class-and-structure-instance)
- [Accessing Properties](#accessing-properties)
- [Memberwise Initializers for Structure Types](#memberwise-initializers-for-structure-types)
- [Structures and Enumerations Are Value Types](#structures-and-enumerations-are-value-types)
- [Classes Are Reference Types](#classes-are-reference-types)
- [Identity Operators ```===```, ```!==```](#identity-operators)
- [Pointers](#pointers)
- [Choosing Between Classes and Structures](#choosing-between-classes-and-structures)
    - [Situations for creating a ```struct```ure](#situations-for-creating-a-structure)
    - [Situations for creating a ```class```](#situations-for-creating-a-class)
- [Assignment and Copy Behavior for Strings, Arrays, and Dictionaries](#assignment-and-copy-behavior-for-strings-arrays-and-dictionaries)
# Introduction
```Swift``` does **not** require you to create separate ```interface``` and ```implementation``` files for custom classes and structures. 
## mostly used syntax
* ```struct```, ```class```
* ```===```,```!==```
# Comparing Classes and Structures
## Things In Common
* properties
* methods 
* subscripts
* initializers
* can be extended
* conform to protocols

## Things only Classes have
* Inheritance
* Type casting enables to check and interpret the type of a class instance at runtime
* Deinitalizers to free up resources
* Reference counting allows more than one reference to a class instance.
    * ```Structure```s are always copied when passed around.

## Things only Structures have
* automatically-generated memberwise initializer

# Definition Syntax
Rules:
1. Use ```class``` and ```struct```
2. Properties must be initialized with value/Type.

# Class and Structure Instance
Rule:
1. Simply use the name of ```class``` or ```struct``` with brackets ```()```

# Accessing Properties
Rule:
1. Use ```.property_tmp```

# Memberwise Initializers for Structure Types
All structures have an automatically-generated *memberwise* initializer.
```swift
struct Resolution {
    var width = 0
    var height = 0
}
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}

let vga = Resolution(width: 640, height: 480)
```

# Structures and Enumerations Are Value Types
A value type is a type whose value is **copied** when it is assigned to a variable or constant, or when it is passed to a function. 

All ```struct```ures and ```enum```erations are value types in ```Swift```. 
```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd     // hard copied
````
changing ```cinema``` will not affect ```hd```

# Classes Are Reference Types
Rule:
1. Assignment is soft copy.
```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0

let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0

print("\(tenEighty.frameRate)")     // 30.0
```

# Identity Operators ```===```, ```!==```
 find out if two constants or variables refer to exactly the same instance of a class.

 ```swift
 if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```
Rules:
1. Classes still support (```==```) operators, which means that two instances are considered “equal” or “equivalent” in **value**, for some appropriate meaning of “equal”, as defined by the type’s designer.
2. When you define your own custom classes and structures, it is **your responsibility** to decide what qualifies as two instances being “equal”.


# Pointers
naturally

# Choosing Between Classes and Structures

## Situations for creating a ```struct```ure
1. a few relatively simple data values
2. more hard copy operation
3. all properties are themselves value types
4. no need for inheritance

## Situations for creating a ```class```
All other cases.


# Assignment and Copy Behavior for Strings, Arrays, and Dictionaries
They are implemented as structures so they are hard copied.

This behavior is different from Foundation: ```NSString, NSArray, and NSDictionary``` are implemented as classes, not structures. 