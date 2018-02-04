- [Introduction](#introduction)
    - [mostly used properties:](#mostly-used-properties)
    - [mostly used methods:](#mostly-used-methods)
- [String Literals](#string-literals)
    - [Special Characters in String Literals](#special-characters-in-string-literals)
        - [The escaped characters](#the-escaped-characters)
        - [an arbitrary Unicode scalar, as ```\u{n}```](#an-arbitrary-unicode-scalar-as-un)
    - [Initializing an Empty String](#initializing-an-empty-string)
    - [String Mutability](#string-mutability)
    - [Strings Are Value Types](#strings-are-value-types)
- [Working with Characters](#working-with-characters)
- [Concatenating Strings and Characters](#concatenating-strings-and-characters)
- [String Interpolation](#string-interpolation)
- [Unicode](#unicode)
    - [Unicode Scalars](#unicode-scalars)
    - [Extended Grapheme Clusters](#extended-grapheme-clusters)
    - [Counting Characters ```.count```](#counting-characters-count)
- [Accessing and Modifying a String](#accessing-and-modifying-a-string)
    - [String Indices](#string-indices)
    - [Inserting and Removing](#inserting-and-removing)
- [Substrings](#substrings)
- [Comparing Strings ```==,!=```](#comparing-strings)
- [Prefix and Suffix Equality](#prefix-and-suffix-equality)
- [Unicode Representations of Strings](#unicode-representations-of-strings)
# Introduction
A  ```String``` is a series of characters. Every string is composed of encoding-independent Unicode characters, and provides support for accessing those characters in various Unicode representations.

```Swift```'s ```String``` type is bridged with ```Foundation```'s ```NSString``` class. ```Foundation``` also extends ```String``` to expose methods defined by ```NSString```. ```import Foundation``` allows you to access those ```NSString``` methods on ```String``` without casting.

```String``` is a get-only type.

## mostly used properties:
* ```.count```
* ```.indices```, ```.startIndex```, ```.endIndex```
* ```.utf8```, ```.utf16```, ```.unicodeScalars```

## mostly used methods:
* ```.index(_:)```: ```before:```, ```after:```, ```of:```, ```.offsetBy:```
* ```.insert(_:at:)```, ```.insert(contentsOf:at:)```, 
* ```.remove(at:)```, ```removeSubrange(_:)```
* ```.hasPrefix(_:)```, ```.hasSuffix(_:)```
* ```.append(_:)```
* ```[index]```
* ```String()```

# String Literals
Rule:
1. A ```String``` literal must be surrounded by double quotation marks (```"```). Even single quotation marks (```'```) don't work.
2. Multiline string literals are surrounded by three pair double quotation marks(```"""..."""```)

    2.1 if you don't want the line breaks, use a backslash (```\```) at the end of those lines.
```swift
// Rule1 Error
let myFaultString = 'Fault'

// Rule2.1
let softWrappedQuotation = """
The White Rabbit put on his spectacles.  "Where shall I begin, \
please your Majesty?" he asked.
"""
```

## Special Characters in String Literals
### The escaped characters
* ```\0``` null character
* ```\\``` backslash
* ```\t``` horizontal tab
* ```\n``` line feed
* ```\r``` carriage return
* ```\"``` double quotation mark
* ```\'``` single quotation mark
* ```\"""``` works as ```\"\"\"```, output ```"""```
* ```"\(EXPR)"``` is the only ```"``` that is allowed to appear without escaping.

### an arbitrary Unicode scalar, as ```\u{n}```
* ```n``` is a 1-8 digit hexadecimal number 

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
// "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}"        // $,  Unicode scalar U+0024
let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665
let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496
```

## Initializing an Empty String
Rule:
1. Use ```var myEmptyString = ""``` or ```var myEmptyString1 = String()```. They are the same
2. use ```.isEmpty``` to check if it is empty. Or, ```string_tmp==""```, or ```string_tmp==String()```
## String Mutability
same as ```let``` and ```var```

## Strings Are Value Types
They are passed by hard copy.


# Working with Characters
Rule:

0. ```Character``` is also a type
```swift
let exclamationMark: Character = "!"
```
1. Access the individual ```Character``` values for a ```String``` by iterating over the string with a ```for-in``` loop
```swift
for character in "Dog!🐶" {
    print(character)    // D, o, g, !, 🐶
}
```

2. Create a collection of ```[Character]``` still need explicit type conversion to make it a ```String()```
```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// Prints "Cat!🐱"
```

# Concatenating Strings and Characters
Rules:
1. Use ```+``` to concatenate ```String``` and ```String```
2. Use ```string_tmp.append(char_temp)``` to concatenate ```String``` and ```Character```
3. Concatenting two multiple line string will append the later one to the end of the first one. However, notice that it is better to have an extra line feed in the first string.
```swift
// Undesired
let badStart = """
one
two
"""
let end = """
three
"""
print(badStart + end)
// Prints two lines:
// one
// twothree


// desired
let goodStart = """
one
two
 
"""
print(goodStart + end)
// Prints three lines:
// one
// two
// three
```

# String Interpolation
Rule:
1. Use ```\(EXPR)``` in double quotation marks.
```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message is "3 times 2.5 is 7.5"
```
# Unicode
Unicode is an international standard for encoding, representing, and processing text in different writing system. ```Swift```'s ```String``` and ```Character``` types are fully Unicode-compliant.

## Unicode Scalars
* ```Swfit```'s native ```String``` type is built from Unicode scalar values. 
* A Unicode scalar is a unique 21-bit number for character or modifier,  such as ```U+0061``` for LATIN SMALL LETTER A ("```a```"), or ```U+1F425``` for FRONT-FACING BABY CHICK ("```🐥```").
* Range ```U+0000``` to ```U+D7FF``` inclusive or ```U+E000``` to ```U+10FFFF``` inclusive.

## Extended Grapheme Clusters
An *extended grapheme cluster* is a sequence of one or more unicode scalaes that (when combined) produce a single human-readable character. A ```Character``` reprensents a single extended grapheme cluster.

Rule:
1. multiple combined Unicode is also a ```Character```, and they are the same. They are more flexible than you can imagine.
```swift
let eAcute: Character = "\u{E9}"    // é
let combinedEAcute: Character = "\u{65}\u{301}"    // é: e followed by '
print(eAcute==combinedEAcute)       // true

// flexible
let precomposed: Character = "\u{D55C}"                  // 한
let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ
// precomposed is 한, decomposed is 한
```

2. REGIONAL INDICATOR SYMBOL LETTER U (```U1F1FA```) + Region indicator symbol S(```\u{1F1F8}```) = FLAG OF US
```swift
let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}" // 🇺🇸
```

3. Enable scalars for enclosing marks such as COMBINING ENCLOSING CIRCLE (```\u{20DD}```)
```swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}"    // é⃝ not supported by markdown
```

## Counting Characters ```.count```
Rule:
1. Combined character is one integraty
2. Characters don't each take up the same amount of memory within a string's representation. As a result, ```.count``` will iterate over the entire string to determine the length.


# Accessing and Modifying a String
## String Indices
Rules:
1. Each ```String``` value has an associated index type ```String.Index```
2. ```.startIndex``` is the first position. ```.endIndex``` is the position after the last character.
    
    2.1 If a ```String``` is empty, ```.startIndex == .endIndex```

    2.2 Use ```.endIndex``` in ```.index()``` will raise a runtime error because it is at an index outside of a string's range.

    2.3 ```.endIndex``` can be used in ```at:```.

3. ```.index()``` supports all collections type.
    
    3.1 Support normal index indicator

    3.2 Support ```before: ...``` and ```after: ...``` keyword pairs.

    3.3 Support ```offsetBy: ...``` to indicate an offset/delay.

```swift

// Rule3
let greeting = "Guten Tag!"
greeting[greeting.startIndex]                           // G
greeting[greeting.index(before: greeting.endIndex)]     // !
greeting[greeting.index(after: greeting.startIndex)]    // u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]                                         // a
```

4. Use the ```indices``` property to access all of the indices of individual characters in a string. Generally, it's used in ```for-in``` loop
```swift
for index in greeting.indices {
    print("\(greeting[index]) ", terminator: "")    // "G u t e n  T a g ! "
}
```

## Inserting and Removing
Rules:
1. To insert a single ```Character```, use ```.insert(_:at:)``` method.
2. To insert a ```String```, use ```.insert(contentsOf:at:)``` method.
```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex) // "hello!"
welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there!"

```

3. To remove a single character at a specified index, use ```remove(at:)``` method
4. To remove a substraing at a specified range, use the ```removeSubrange(_:)method:
```swift
// Rule3 remove a single Character
welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there"

// Rule4 remove sub string 
let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome now equals "hello"
```

# Substrings
Rule:
1. ```Substring``` is a type. When you are ready to store the reuslt for a longer time, you convert the substring to an instance of ```String```. They share the same memory of the original ```String```, so the original ```String``` should keep alive. 
```swift
let greeting = "Hello, world!"
let index = greeting.index(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index]
// beginning is "Hello"
 
// Convert the result to a String for long-term storage.
let newString = String(beginning)
```

# Comparing Strings ```==,!=```
Rule:
1. Combined ```Character``` equals decomposite ones.
2. Latin letter is not equivalent to Cyrillic letter.

# Prefix and Suffix Equality
Rule:
1. ```.hasPrefix(_:)``` and ```.hasSuffix(_:)``` method.

# Unicode Representations of Strings
Rules:
1. ```.utf8```, ```.utf16```, ```.unicodeScalars```

```swift
let dogString = "Dog‼🐶"

// .utf8
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 226 128 188 240 159 144 182 "

// .utf16
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 55357 56374 "

// .unicodeScalars
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 128054 "
```

2. the ```UnicodeScalar``` value can also be used to construct a new ```String``` value
```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}
// D
// o
// g
// ‼
// 🐶
```