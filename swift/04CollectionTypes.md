- [Introduction](#introduction)
    - [mostly used properties](#mostly-used-properties)
    - [mostly used methods](#mostly-used-methods)
- [Mutability of Collections](#mutability-of-collections)
- [Arrays](#arrays)
    - [Array Type Shorthand Syntax](#array-type-shorthand-syntax)
    - [Creating](#creating)
        - [Creating an Empty Array](#creating-an-empty-array)
        - [Creating an Array with a Default Value](#creating-an-array-with-a-default-value)
        - [Creating an Array by Adding Two Arrays Together](#creating-an-array-by-adding-two-arrays-together)
        - [Creating an Array with an Array Literal](#creating-an-array-with-an-array-literal)
    - [Accessing and Modifying an Array](#accessing-and-modifying-an-array)
    - [Iterating Over an Array](#iterating-over-an-array)
- [Sets](#sets)
    - [Hash values for Set Types](#hash-values-for-set-types)
    - [Set Type Syntax ```Set<Type>```](#set-type-syntax-settype)
    - [Creating](#creating)
        - [Initializing an Empty Set](#initializing-an-empty-set)
        - [creating a Set with an Array Literal](#creating-a-set-with-an-array-literal)
    - [Accessing and Modifying a Set](#accessing-and-modifying-a-set)
    - [Iterating Over a Set](#iterating-over-a-set)
    - [Performing Set Operations](#performing-set-operations)
    - [Set Membership and Equality](#set-membership-and-equality)
- [Dictionaries](#dictionaries)
    - [Dictionary Type Shorthand Syntax](#dictionary-type-shorthand-syntax)
    - [Creating](#creating)
        - [an Empty Dictionary](#an-empty-dictionary)
        - [creating a Dictionary with a Dictionary Literal](#creating-a-dictionary-with-a-dictionary-literal)
    - [Accessing and Modifying a Dictionary](#accessing-and-modifying-a-dictionary)
    - [Iterating Over a Dictionary](#iterating-over-a-dictionary)

# Introduction
```Swift``` provides three primary collection types, known as arrays, sets, and dictionaries.
* Arrays are ordered collections of values.
* Sets are unordered collections of unique values.
* Dictionaries are unordered collections of key-value associations.

They are always clear about the types of values and keys that they can store.

## mostly used properties
* ```.count```
* ```.isEmpty```
* for ```Set```
    * ```.hashValue```
* for ```Dictionary```
    * ```.keys```
    * ```.values```    
## mostly used methods
* for ```Array```
    * ```Array(repeating:count:)```
    * ```+```
    * ```.append(_:)```
    * ```.insert(_:at:)```
    * ```.remove(at:)```
        * ```.removeLast()``` 
    * ```.enumerated()```
* for ```Set```
    * ```.insert(_:)```
    * ```.remove(_:)```
        * ```.removeAll()```  
    * ```.sorted()```
    * ```a.intersection(b)``` $a\cap b$
        * ```a.symmetricDifference(b)``` $a\cup b - a\cap b$
        * ```a.union(b)``` $a\cup b$
        * ```a.subtracting(b)``` $a-b$
    * ```a==b```
        * ```a.isSubset(of: b)```, ```a.isStrictSubset(of: b)```
        * ```a.isSuperset(of: b)```, ```a.isStrictSuperset(of: b)```
        * ```a.isDisjoint(with: b)```
* for ```Dictionary```
    * ```dict_tmp[new_key] = new_value```, ```dict_tmp[old_key] = nil```
    * ```.updateValue(_:forKey:)```
    * ```.removeValue(forKey:)```

# Mutability of Collections
same as ```let``` or ```var```. Use immutable objects as long as you can.

# Arrays
```Swift```'s ```Array``` type is bridged to ```Foundation```'s ```NSArray``` class.

## Array Type Shorthand Syntax
The full name of ```Array``` is as ```Array<Type>```. 

The shorthand form is as ```[Type]```, which is preferred.

## Creating
### Creating an Empty Array
Rules:
1. Use initializer ```[Int]()```
2. Once initialized properly, use ```= []``` to make it an empty array (the type is invariant).
```swift
// Rule1
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// "someInts is of type [Int] with 0 items."

// Rule2
someInts.append(3)  // [3]
someInts = []       // [] empty again
```

### Creating an Array with a Default Value
Rule:
1. Use ```Array(repeating:count:)``` to initialize.
```swift
var threeDoubles = Array(repeating : 0.0, count : 3)    // [0.0, 0.0, 0.0]
```

### Creating an Array by Adding Two Arrays Together
Rule:
1. ```array1 + array2``` concatenates two arrays with the same type.

### Creating an Array with an Array Literal
no need to mention.
```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// same as 
var shoppingList = ["Eggs", "Milk"]
```

## Accessing and Modifying an Array
Rules:
1. Use ```.count``` to find out the number of items in an array.
2. Use ```.isEmpty``` to check whether the ```.count==0```
3. Use ```.append(_:)``` to add a new item.
4. Use ```+``` to append an array
5. Use ```[index]``` subscript syntax to retrieve the item.
6. Use ```[index]=``` to change the value of the item in index. The index must be valid - within the original range.
    
    6.1 If the amount is unmatched, it will automatically expend or shrink.
```swift
// Rule6.1 OK
var a = [0,1,2,3,4,5,6]
a[1...3] = [1,2,3,4,5]  // [0, 1, 2, 3, 4, 5, 4, 5, 6]
a[1...] = [1]           // [0, 1]
```
7. Use ```.insert(_:at:)``` to insert a new item.
8. Use ```.remove(at:)``` to remove an item. And it will return the removed item

    8.1 Use ```.removeLast()``` to remove the last item.

## Iterating Over an Array
Rules:
1. Use```for item in array_temp{}``` loop
2. Use ```.enumerated()``` to get the same function as ```enumerate()``` in ```Python```. ```for (index, value) in array_temp.enumerated(){}```
```swift
for (index,value) in a.enumerated(){
    print("Item \(index+1): \(value)")
}
```

# Sets
```Swift```'s ```Set``` type is bridged to ```Foundation```'s ```NSSet``` class.

A *set* stores **distinct** values of the same type in a collection with no defined ordering.

## Hash values for Set Types
A type must be *hashable* in order to be stored in a set. ```a==b``` equals to ```a.hashValue == b.hashValue```

All of ```Swift```'s basic types are hashable, (```String, Int, Double, Float, enum```).

Creating custom types as ```set``` value type or  ```dictionary``` key types by making them conform to the ```Hashable``` protocol from ```Swift```'s standard library, i.e. 
* must provide a gettable ```Int``` property called ```hashValue```.

Because the ```Hashable``` protocol conforms to ```Equatable```, conforming types 
* must also provide an implementation of the equals operator (```==```)
    * ```a == a``` (Reflexivity)
    * ```a == b``` implies ```b == a``` (Symmetry)
    * ```a == b && b== c``` implies ```a == c``` (Transitivity)

## Set Type Syntax ```Set<Type>```

## Creating

### Initializing an Empty Set
Rules:
1. Use the ```set``` type syntax, ```Set<Type>()```
2. If the type is defined, use ```=[]``` to remove all elements.
```swift
var letters = Set<Character>()
letters.insert("a")
letters = []
```

### creating a Set with an Array Literal
Rule:
1. explicitly point out the type via ```set``` type syntax.
2. use ```Set(array_tmp)```
3. use ```Set<Type>(array_tmp)```
```swift
// Rule1 OK
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]

// Rule2 OK
var favoriteGneres = Set( ["Rock", "Classical", "Hip hop"] )

// Rule3 OK
var favoriteGneres = Set<String>( ["Rock", "Classical", "Hip hop"] )
// Rule3 Error
var favoriteGneres = Set<Int>( ["Rock", "Classical", "Hip hop"] )
```

## Accessing and Modifying a Set
* ```.count```
* ```.isEmpty```
* ```.insert(_:)```
* ```.remove(_:)```, returns the removed value, or returns ```nil``` if the set did not contain it
* ```.removeAll()```, always return ```Void``` .

## Iterating Over a Set
Rules:
1. Use ```for item in set_tmp{}```
2. use ```for item_sorted in set_tmp.sorted(){}``` to iterate from the smallest item.

## Performing Set Operations
* ```a.intersection(b)``` $a\cap b$
* ```a.symmetricDifference(b)``` $a\cup b - a\cap b$
* ```a.union(b)``` $a\cup b$
* ```a.subtracting(b)``` $a-b$

## Set Membership and Equality
* ```a==b```
* ```a.isSubset(of: b)```, ```a.isStrictSubset(of: b)```
* ```a.isSuperset(of: b)```, ```a.isStrictSuperset(of: b)```
* ```a.isDisjoint(with: b)```

# Dictionaries
```Swift```'s ```Dictionary``` type is bridged to ```Foundation```'s ```NSDictionary``` class.

A *dictionary* stores associations between *keys* of the **same** type and *values* of the same type in a collection with **no defined ordering**.

* the ```Key``` type must conform to the ```Hashable``` protocol.

## Dictionary Type Shorthand Syntax
```Dictionary<Key, Value>``` in full. 

```[Key: Value]``` in shorthand form.

## Creating
### an Empty Dictionary
Rules:

0. Use complete form ```var dict_tmp:Dictionary<T1,T2> = [:]```
1. Use shorthand syntax ```[KeyType: ValueType]()```
2. Once settled, use ```[:]``` to empty a dictionary.

```swift
var stdDict: Dictionary<Int, String> = [:]
var lessStdDict: [Int:String] = [:]
var dictFromInstance = Dictionary<Int, String>()
var dictShorthandInstance = [Int:String]()
```

### creating a Dictionary with a Dictionary Literal
Rule:
1. Don't use instance intialization.
```swift
// Rule1 Error
var airports1 = [String:String](["YYZ": "Toronto Pearson", "DUB": "Dublin"])
// Rule1 Error
var airports2 = Dictionary<String:String>(["YYZ": "Toronto Pearson", "DUB": "Dublin"])

// Best
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
// Or
var airports: [String:String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
// Or
var airports: Dictionary<String, String> = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

## Accessing and Modifying a Dictionary
* ```.count```
* ```dict_tmp[key_tmp]```, ```dict_tmp[new_key] = new_value```
    * non-exist key will return ```nil```
    * non-exist key will generate a new key for the assignment operator.
* ```updateValue(_:forKey:)```, unlike a subscript, will returns the old value or ```nil``` if it adds new key.
* ```dict_tmp[key_tmp] = nil``` to remove a kv pair.
* ```.removeValue(forKey:)``` will return the removed value or ```nil```

## Iterating Over a Dictionary
Rules:
1. Use ```for (k,v) in dict_tmp{}```
2. Use ```for k in dict_tmp.keys{}```
3. Use ```for v in dict_tmp.values{}```
4. Dictionary does not have ```sorted()``` method!
```swift
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
```