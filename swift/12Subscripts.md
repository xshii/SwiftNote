1. [Introduction](#introduction)
    1. [Syntax](#syntax)
2. [Subscript Syntax](#subscript-syntax)
3. [Subscript Usage](#subscript-usage)
4. [Subscript Options](#subscript-options)
# Introduction
For example, you access elements in an `Array` instance as `someArray[index]` and elements in a `Dictionary` instance as `someDictionary[key]`.
## Syntax
* `subscript(index: T1,...)->T2{}`
# Subscript Syntax
Rules:
1. write subscript definitions with the `subscript` keyword, and specify one or more input parameters and a return type.
2. subscripts can be read-write or read-only, via `get{}`,`set(){}`

```swift
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
    }
    set(newValue) {
        // perform a suitable setting action here
    }
}
```

an example of `TimesTable` with a read-only subscript.
```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}
let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])")
// Prints "six times three is 18"
```

# Subscript Usage
example of a dictionary. `[String: Int]`
```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```

# Subscript Options
Subscripts can use variadic parameters, but they can’t use in-out parameters or provide default parameter values.

an example of implementation of a `Matrix`
```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}
```
create a new instance `Matrix`
```swift
var matrix = Matrix(rows: 2, columns: 2)
```
assign value
```swift
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```
