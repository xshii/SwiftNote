1. [Introduction](#introduction)
    1. [frequently used syntax](#frequently-used-syntax)
2. [Instance Methods - `func`](#instance-methods---func)
    1. [Modifying Value Types from Within Instance Methods - `mutating func`](#modifying-value-types-from-within-instance-methods---mutating-func)
        1. [Assigning to self Within a Mutating Method](#assigning-to-self-within-a-mutating-method)
3. [Type methods-`class func`/`static func`](#type-methods-class-funcstatic-func)
# Introduction
Classes, structures, and enumerations can all define instance methods.

Type methods are similar to class methods in `Obj-C`.

Structures and enumerations can define methods in `Swift` is a major difference from `C` and `Obj-C`. 
## frequently used syntax
* `func`
* `mutating func`
* `class func`
* `static func`

# Instance Methods - `func`
An instance method has implicit access to all other instance methods and properties of that type.

Rule:
1. you don't need to write `self` in the instance function. `Swift` assumes that you are referring to a property or method of the current instance.
    * unless the parameter has the same name.

## Modifying Value Types from Within Instance Methods - `mutating func`
 if you need to modify the properties of your `structure` or `enumeration` within a particular method, you can opt in to `mutating` behavior for that method.

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"
```

### Assigning to self Within a Mutating Method
Mutating methods can assign an entirely new instance to the implicit `self` property. 
```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```
For enumerator
```swift
enum TriStateSwitch {
    case off, low, high
    mutating func next() {
        switch self {
        case .off:
            self = .low
        case .low:
            self = .high
        case .high:
            self = .off
        }
    }
}
var ovenLight = TriStateSwitch.low
ovenLight.next()
// ovenLight is now equal to .high
ovenLight.next()
// ovenLight is now equal to .off
```

# Type methods-`class func`/`static func`
change the state of the structure/class.
```swift
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1
    
    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }
    
    static func isUnlocked(_ level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }
    
    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}

```

```swift
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func complete(level: Int) {
        LevelTracker.unlock(level + 1)
        tracker.advance(to: level + 1)
    }
    init(name: String) {
        playerName = name
    }
}
```

```swift
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// Prints "highest unlocked level is now 2"
```
