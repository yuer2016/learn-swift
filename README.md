# Swift

- [Swift](#swift)
  - [基本抽象](#基本抽象)
  - [变量和常量](#变量和常量)
  - [基本类型](#基本类型)
    - [整数](#整数)
    - [浮点数](#浮点数)
    - [字符串](#字符串)
    - [可空类型](#可空类型)
  - [控制流](#控制流)
    - [if](#if)
    - [switch](#switch)
    - [for 循环](#for-循环)
    - [while 循环](#while-循环)
  - [容器](#容器)
    - [数组](#数组)
    - [字典](#字典)
    - [Set](#set)
  - [函数](#函数)
    - [闭包](#闭包)
    - [函数作为返回值](#函数作为返回值)
    - [函数作为参数](#函数作为参数)
  - [类和方法](#类和方法)
    - [枚举](#枚举)
    - [方法](#方法)
    - [结构体](#结构体)
    - [类](#类)
    - [mutating 方法详解](#mutating-方法详解)
    - [属性](#属性)
    - [访问控制](#访问控制)
    - [初始化](#初始化)
  - [值类型与引用类型](#值类型与引用类型)
    - [值类型](#值类型)
    - [引用类型](#引用类型)
    - [类和结构体差异](#类和结构体差异)
    - [写时复制](#写时复制)
  - [协议和泛型](#协议和泛型)
    - [协议](#协议)
      - [协议继承](#协议继承)
      - [协议组合](#协议组合)
    - [泛型](#泛型)
  - [错误处理](#错误处理)
  - [扩展](#扩展)
    - [协议扩展](#协议扩展)
  - [内存管理和ARC](#内存管理和arc)
  - [自定义运算符](#自定义运算符)

## 基本抽象

## 变量和常量

在 Swift 中声明常量和变量有不同的语法。
声明变量用关键字 var ，而声明常量用关键字 let

```swift
let numberOfStoplights: Int = 4

var population: Int
population = 5422
population = 5214
```

## 基本类型

数是计算机语言的基础，也是软件开发的重要组成部分。

数分为两种基本类型：整数和浮点数

### 整数

```swift
print("The maximum Int value is \(Int.max).")

print("The minimum Int value is \(Int.min).")

print("The maximum value for a 32-bit integer is \(Int32.max).")

print("The minimum value for a 32-bit integer is \(Int32.min).")

print("The maximum UInt value is \(UInt.max).")

print("The minimum UInt value is \(UInt.min).")

print("The maximum value for a 32-bit unsigned integer is \(UInt32.max).")

print("The minimum value for a 32-bit unsigned integer is \(UInt32.min).")

let numberOfPages: Int = 10 // 显式声明类型

let numberOfChapters = 3    // 还是 Int 类型,不过是编译器推断出来的

/*便捷操作符*/
var x = 10
x += 10 // 等同于: x = x + 10
x -= 5 // 等同于: x = x - 5
print("this x value :\(x).")

/* 两个不同类型的数值类型不能相加 */
let a: Int16 = 200
let b: Int8 = 50

let c = a + b //报错！
let c = a + Int16(b)

/*余数*/
print(11 % 3) // 打印 2 

/*溢出操作符*/
let y: Int8 = 120
let z = y + 10 
let z = y &+ 10
print("120 &+ 10 is \(z)")
```

### 浮点数

Swift 有两种基本的浮点数类型：32位浮点数 Float 和64位浮点数 Double 。

Float 和 Double 的长度差异并不像整数那样影响其最小值和最大值，而是影响其精度。

Double 的精度比 Float 高，这意味着它能存储更精确的近似值。

在 Swift 中，浮点数的默认推断类型是 Double ，就像整数的不同类型一样，也可以显式地声明 Float 和 Double。

```swift
let d1 = 1.1 // 隐式 Double 声明
let d2: Double = 1.1
let f1: Float = 100.3

print(10.0 + 11.4)
print(11.0 / 3.0)
```

### 字符串

Swift 的 String 和 Character 类型构建于 Unicode 之上,并且把大部分的复杂细节都屏蔽了。

```swift
let playground = "Hello, playground"

var mutablePlayground = "Hello, mutable playground"
mutablePlayground += "!"

for c: Character in mutablePlayground.characters {
    print("'\(c)'")
}

let start = playground.startIndex
let end = playground.index(start, offsetBy: 4)
let fifthCharacter = playground[end] // "o"

let range = start...end
let firstFive = playground[range] // "Hello"

```

### 可空类型

Swift 的可空类型让这门语言更加安全。
一个可能为 nil 的实例应该被声明为可空类型。
这意味着如果一个实例没有被声明为可空类型, 它就不可能是 nil。

```swift
var errorCodeString: String?
errorCodeString = "404"
print(errorCodeString)
```

可空链式调用 （optional chaining）提供了一种对可空实例进行查询以判断其是否包含值的机制。
两者的一个重要区别是，可空链式调用允许程序员把多个查询串联为一个可空实例的值。
如果链式调用中的每个可空实例都包含值，那么每个调用都会成功，整个查询链会返回期望类型的可空实例。
如果查询链中的任意可空实例是 nil ，那么整个链式调用会返回 nil。

```swift
var errorCodeString: String?
errorCodeString = "404"

var errorDescription: String?
if let theError = errorCodeString, let errorCodeInteger = Int(theError), errorCodeInteger == 404 {
    print("\(theError): \(errorCodeInteger)") 
    errorDescription = "\(errorCodeInteger + 200): resource was not found."
}

var upCaseErrorDescription = errorDescription?.uppercased()
// ?? 的左边必须为可空实例；
// 如果左边的可空实例是nil ，那么?? 会返回右边的值。
// 如果左边的可空实例不是nil ，那么?? 会返回可空实例中包含的值。
let description = upCaseErrorDescription ?? "No error"
```

## 控制流

### if

```swift
var population: Int = 5422

var message: String

var hasPostOffice: Bool = true

if population < 10000 {
    message = "\(population) is a small town!"
} else if population >= 10000 && population < 50000 {
    message = "\(population) is a medium town!"
}else{
    message = "\(poplation) is a big town"
}

/*三目运算*/
message = population < 10000 ? "\(population) is a small town!" : "\(population) is pretty big!"
```

### switch

```swift
var statusCode: Int = 404
var errorString: String

switch statusCode {

case 300 .. 307:
    errorString += " Redirection, 3xx."

case 400:
    errorString = "Bad request"

case 401:
    errorString = "Unauthorized"

case 403:
    errorString = "Forbidden"

case 404:
    errorString = "Not found"

case 500, 501, 503, 504:
    errorString = "There was something wrong with the response."
    fallthrough //态转移语句 （control transfer statement）告诉 switch 语句从一个分支的底部 “漏” 到下一个分支去

case let unknownCode where (unknownCode >= 200 && unknownCode < 300) || unknownCode > 505:
    errorString = "\(unknownCode) is not a known error code."

default:
    errorString = "None"
}

/*创建元组*/
let error = (code: statusCode, error: errorString)
error.code
error.error

let firstErrorCode = 404
let secondErrorCode = 200
let errorCodes = (firstErrorCode, secondErrorCode)

switch errorCodes {
case (404, 404):
    print("No items found.")

case (404, _):
    print("First item not found.")

case (_, 404):
    print("Second item not found.")

default:
    print("All items found.")
}

/* Swift 有 if-case 语句来提供类似于 switch 语句的模式匹配能力*/
let age = 25

if case 18...35 = age {
    print("Cool demographic")
}

if case 18...35 = age, age >= 21 {
  print("In cool demographic and of drinking age")
}
```

### for 循环

```swift
var myFirstInt: Int = 0

for i in 1...5 {
    myFirstInt += 1
    myFirstInt
    print(myFirstInt)
}

for i in 1...100 where i % 3 == 0 {
    print(i)
}
```

### while 循环

```swift
var i = 1

while i < 6 {
    myFirstInt += 1
    print(myFirstInt)
    i += 1
}

repeat {
  print("hello")
} while i <= 1 

```

## 容器

### 数组

数组是值的有序集合。
数组的每个位置都用索引标记，任何值都可以在数组中出现多次。

```swift
// 方式一
var bucketList: Array<String>
// 方式二
var bucketList: [String]
// 创建时初始化
var bucketList: [String] = ["Climb Mt. Everest"]
// 创建时初始化,类型推断
var bucketList = ["Climb Mt. Everest"]
// 数组是从零开始索引的
bucketList.append("Fly hot air balloon to Fiji")
bucketList.append("Scuba dive in the Great Blue Hole")
bucketList.remove(at: 1)

print(bucketList.count)
print(bucketList[0...1])

bucketList[0] += " in Australia"
bucketList[0] = "Climb Mt. Kilimanjaro"
```

用循环从一个数组添加元素到另一个数组

```swift
var bucketList = ["Climb Mt. Everest"]

var newItems = [
    "Fly hot air balloon to Fiji",
    "Watch the Lord of the Rings trilogy in one day",
    "Go on a walkabout",
    "Scuba dive in the Great Blue Hole",
    "Find a triple rainbow"
    ]

// 等价于 bucketList += newItems
for item in newItems {
    bucketList.append(item)
}

bucketList.remove(at: 2)

print(bucketList.count)
print(bucketList[0...2])

bucketList[2] += " in Australia"
bucketList[0] = "Climb Mt. Kilimanjaro"
bucketList.insert("Toboggan across Alaska", at: 2)

/*使用 let 关键字创建不可变数组。如果试图以任何方式修改数组，编译器会报错，提示你不能修改不可变数组*/
let lunches = [
    "Cheeseburger",
    "Veggie Pizza",
    "Chicken Caesar Salad",
    "Black Bean Burrito",
    "Falafel Wrap"
  ]
```

### 字典

```swift
var dict1: Dictionary<String, Double> = [:]
var dict2 = Dictionary<String, Double>()
var dict3: [String:Double] = [:]
var dict4 = [String:Double]()

var movieRatings = ["Donnie Darko": 4, "Chungking Express": 5, "Dark City": 4]

print("I have rated \(movieRatings.count) movies.")

let darkoRating = movieRatings["Donnie Darko"]

// 修改值
movieRatings["Dark City"] = 5
let oldRating: Int? = movieRatings.updateValue(5, forKey: "Donnie Darko")
if let lastRating = oldRating, let currentRating = movieRatings["Donnie Darko"] {
    print("Old rating: \(lastRating); current rating: \(currentRating)")
}

// 添加值
movieRatings["The Cabinet of Dr. Caligari"] = 5
// 删除值
movieRatings.removeValue(forKey: "Dark City")
let removedRating: Int? = movieRatings.removeValue(forKey: "Dark City") 

// 循环字典
for (key, value) in movieRatings {
     print("The movie \(key) was rated \(value).")
}

for movie in movieRatings.keys {
  print("User has rated \(movie).")
}
// 字典转换为数组
let watchedMovies = Array(movieRatings.keys)

// 不可变字典
let album = ["Diet Roast Beef": 268,
             "Dubba Dubbs Stubs His Toe": 467,
             "Smokey's Carpet Cleaning Service": 187,
             "Track 4": 221]
```

### Set

```swift
var groceryBag = Set<String>()

groceryBag.insert("Apples")
groceryBag.insert("Oranges")
groceryBag.insert("Pineapple")

for food in groceryBag {
    print(food)
}

var groceryBag: Set = ["Apples", "Oranges", "Pineapple"]
let hasBananas = groceryBag.contains("Bananas")

// 集合并集
let friendsGroceryBag = Set(["Bananas", "Cereal", "Milk", "Oranges"])
let commonGroceryBag = groceryBag.union(friendsGroceryBag)

// 交集
let roommatesGroceryBag = Set(["Apples", "Bananas", "Cereal", "Toothpaste"])
let itemsToReturn = commonGroceryBag.intersection(roommatesGroceryBag)

// 不相交
let yourSecondBag = Set(["Berries", "Yogurt"])
let roommatesSecondBag = Set(["Grapes", "Honey"])
let disjoint = yourSecondBag.isDisjoint(with: roommatesSecondBag)
```

## 函数

函数 （function）是一组有名字的代码，用来完成某个特定的任务。函数的名字描述了其执行的任务。

```swift
func printGreeting() {
     print("Hello, playground.")
}
printGreeting()

// 函数参数
func printPersonalGreeting(name: String) {
     print("Hello \(name), welcome to your playground.")
}
printPersonalGreeting(name: "Matt")

func divisionDescriptionFor(numerator: Double, denominator: Double) {
    print("\(numerator) divided by \(denominator) equals \(numerator / denominator)")
}
divisionDescriptionFor(numerator: 9.0, denominator: 3.0)

// 显式函数名
func printPersonalGreeting(to name: String) {
     print("Hello \(name), welcome to your playground.")
}
printPersonalGreeting(to: "Matt")

// 变长参数
func printPersonalGreetings(to names: String...) {
    for name in names {
        print("Hello \(name), welcome to the playground.")
    }
}
printPersonalGreetings(to: "Alex","Chris","Drew","Pat")

// 默认参数值
func divisionDescriptionFor(numerator: Double,
                            denominator: Double,
                            withPunctuation punctuation: String = ".") {

    print("\(numerator) divided by \(denominator) equals
           \(numerator / denominator)\(punctuation)")

}
divisionDescriptionFor(numerator: 9.0, denominator: 3.0)
divisionDescriptionFor(numerator: 9.0, denominator: 3.0, withPunctuation: "!")

// 函数有时候需要修改实参的值。
// in-out参数 （in-out parameter）能让函数影响函数体以外的变量。
// 有两个注意事项：首先，in-out参数不能有默认值；其次，变长参数不能标记为inout 。
var error = "The request failed:"
func appendErrorCode(_ code: Int, toErrorString errorString: inout String) {
    if code == 400 {
       errorString += " bad request."
    }
}
appendErrorCode(400, toErrorString: error)

// 函数返回值
func divisionDescriptionFor(numerator: Double,
                            denominator: Double,
                            withPunctuation punctuation: String = ".") -> String {
    return "\(numerator) divided by \(denominator) equals \(numerator / denominator)\(punctuation)"
}
print(divisionDescriptionFor(numerator: 9.0,denominator: 3.0,withPunctuation: "!"))
```

Swift 的函数定义可以嵌套。嵌套函数在另一个函数定义的内部声明并实现。嵌套函数在包围它的函数以外不可用。

```swift
func areaOfTriangleWith(base: Double, height: Double) -> Double {
    let numerator = base * height

    func divide() -> Double {
        return numerator / 2
    }

    return divide()
}
areaOfTriangleWith(base: 3.0, height: 5.0)

// 多返回值
// 函数类型 （function type）由函数参数和返回值组成。
// 以sortedEvenOddNumbers(_:) 函数为例，它接受整数数组作为参数，返回有两个整数数组的元组。
// 于是，sortedEvenOddNumbers(_:) 的类型可以表示为：([Int]) -> ([Int], [Int])
func sortedEvenOddNumbers(_ numbers: [Int]) -> (evens: [Int], odds: [Int]) {
    var evens = [Int]()
    var odds = [Int]()

    for number in numbers {
        if number % 2 == 0 {
            evens.append(number)
        } else {
            odds.append(number)
        }
    }
    return (evens, odds)
}

let aBunchOfNumbers = [10,1,4,3,57,43,84,27,156,111]
let theSortedNumbers = sortedEvenOddNumbers(aBunchOfNumbers)
print("The even numbers are: \(theSortedNumbers.evens); the odd numbers are: \(theSortedNumbers.odds)")

// 可空类型
func grabMiddleName(fromFullName name: (String, String?, String)) -> String? {
    return name.1
}

let middleName = grabMiddleName(fromFullName: ("Matt",nil,"Mathias"))
if let theName = middleName {
    print(theName)
}
```

guard 语句
跟 if/else 语句一样，guard 语句会根据某个表达式返回的布尔值结果来执行代码；
但不同之处是，如果某些条件没有满足，可以用 guard 语句来提前退出函数。

```swift
func greetByMiddleName(fromFullName name: (first: String,
                                           middle: String?,
                                           last: String)) {

    guard let middleName = name.middle else {
        print("Hey there!")
        return
    }
    print("Hey \(middleName)")
}
greetByMiddleName(fromFullName: ("Matt","Danger","Mathias"))

// 没有显式返回值的函数还是有返回值，返回的是Void 。编译器会帮你在代码中插入这个返回值
func printGreeting() -> Void {
    print("Hello, playground.")
}
```

### 闭包

闭包 （closure）是在应用中完成特定任务的互相分离的功能组。

```swift
{( parameters ) ->  return type  in
      // 代码
}

let volunteerCounts = [1,3,40,32,2,53,77,13]

let volunteersSorted = volunteerCounts.sorted(by: {
    (i: Int, j: Int) -> Bool in    
    return i < j
})
// 等价于
let volunteersSorted = volunteerCounts.sorted(by: { i, j in i < j })
// 等价于
let volunteersSorted = volunteerCounts.sorted(by: { $0 < $1 })
// 等价于
func sortAscending(_ i: Int, _ j: Int) -> Bool {
    return i < j

}
let volunteersSorted = volunteerCounts.sorted(by: sortAscending)
```

闭包是引用类型 （reference type）。这意味着把函数赋给常量或变量时，实际上是在让这个常量或变量指向这个函数

```swift
func makePopulationTracker(forInitialPopulation population: Int) -> (Int) -> Int {
    var totalPopulation = population

    func populationTracker(growth: Int) -> Int {
        totalPopulation += growth
        return totalPopulation
    }
    return populationTracker
}

var currentPopulation = 5_422

let growBy = makePopulationTracker(forInitialPopulation: currentPopulation)

growBy(500)
growBy(500)
growBy(500)

currentPopulation = growBy(500) // currentPopulation现在是7422

let anotherGrowBy = growBy
anotherGrowBy(500) // totalPopulation现在是7922
```

### 函数作为返回值

```swift
let volunteerCounts = [1,3,40,32,2,53,77,13]
let volunteersSorted = volunteerCounts.sorted { $0 < $1 }

func makeTownGrand() -> (Int, Int) -> Int {
  func buildRoads(byAddingLights lights: Int,
                  toExistingLights existingLights: Int) -> Int {
        return lights + existingLights
  }
  return buildRoads
}

var stoplights = 4
let townPlanByAddingLightsToExistingLights = makeTownGrand()
stoplights = townPlanByAddingLightsToExistingLights(4, stoplights)
print("Knowhere has \(stoplights) stoplights.")
```

### 函数作为参数

```swift
let volunteerCounts = [1,3,40,32,2,53,77,13]
let volunteersSorted = volunteerCounts.sorted { $0 < $1 }

func makeTownGrand(withBudget budget: Int,
                   condition: (Int) -> Bool) -> ((Int, Int) -> Int)? {

    if condition(budget) {
        func buildRoads(byAddingLights lights: Int,
                        toExistingLights existingLights: Int) -> Int {
            return lights + existingLights
        }
        return buildRoads
    } else {
        return nil
    }
}

func evaluate(budget: Int) -> Bool {
    return budget > 10_000
}

var stoplights = 4
if let townPlanByAddingLightsToExistingLights = makeTownGrand(withBudget: 1_000,
                                                              condition: evaluate) {
    stoplights = townPlanByAddingLightsToExistingLights(4, stoplights)
}

print("Knowhere has \(stoplights) stoplights.")
```

Swift采用了函数式编程 （functional programming）范式的一些模式。

1. 函数可以作为返回值从别的函数返回，也可以作为参数传递给别的函数，可以存储在变量中，等等；就跟其他类型一样。
2. 纯函数 ：函数没有副作用；给定同样的输入，函数永远返回同样的输出，而且不会修改程序中其他地方的状态。大部分数学函数都是纯函数，像sin、cos、斐波那契和阶乘。
3. 不可变性 ：不鼓励可变性，因为值可变的数据更难分析。
4. 强类型系统能增加代码的运行时安全性，因为语言的类型系统的合法性会在编译时得到检查。

```swift
let precinctPopulations = [1244, 2021, 2157]

let projectedPopulations = precinctPopulations.map {
    (population: Int) -> Int in
    return population * 2
}

let bigProjections = projectedPopulations.filter {
    (projection: Int) -> Bool in
    return projection > 4000
}

let totalProjection = projectedPopulations.reduce(0) {
    (accumulatedProjection: Int, precinctProjection: Int) -> Int in
    return accumulatedProjection + precinctProjection
}
```

## 类和方法

### 枚举

```swift
// 定义枚举的方式是在 enum 关键字后跟枚举的名字。
// 左花括号（{ ）表示枚举体的开始。
// 枚举体至少包含一个 case 语句
enum TextAlignment {
    case left
    case right
    case center
}

// 枚举类型推断
var alignment = TextAlignment.left
alignment = .right

switch alignment {
case .left:
    print("left aligned")

case .right:
    print("right aligned")

case .center:
    print("center aligned")
}

// 使用原始值
enum TextAlignment: Int {
     case left
     case right
     case center
     case justify
}

var alignment = TextAlignment.justify
print("Left has raw value \(TextAlignment.left.rawValue)")
print("Right has raw value \(TextAlignment.right.rawValue)")
print("Center has raw value \(TextAlignment.center.rawValue)")
print("Justify has raw value \(TextAlignment.justify.rawValue)")
print("The alignment variable has raw value \(alignment.rawValue)")


// 创建一个原始值
let myRawValue = 20

// 尝试将原始值转化为 TextAlignment
if let myAlignment = TextAlignment(rawValue: myRawValue) {
    // 转化成功
    print("successfully converted \(myRawValue) into a TextAlignment")
} else {
    // 转化失败
    print("\(myRawValue) has no corresponding TextAlignment case")
}

```

### 方法

方法是和类型关联的函数。

```swift
enum Lightbulb {
    case on
    case off

    func surfaceTemperature(forAmbientTemperature ambient: Double) -> Double {
        switch self {
          case .on:
            return ambient + 150.0
          case .off:
            return ambient
        }
    }

  // 在Swift中，枚举是值类型 （value type），而值类型的方法不能对 self 进行修改。
  // 如果希望值类型的方法能修改 self ，需要标记这个方法为 mutating
  mutating func toggle() {
      switch self {
          case .on:
              self = .off
          case .off:
              self = .on
      }
    }
}

var bulb = Lightbulb.on

let ambientTemperature = 77.0
var bulbTemperature = bulb.surfaceTemperature(forAmbientTemperature:ambientTemperature)
print("the bulb's temperature is \(bulbTemperature)")

// 带关联值的枚举
enum ShapeDimensions {
    // 正方形的关联值是边长
    case square(side: Double)
    // 长方形的关联值是宽和高
    case rectangle(width: Double, height: Double)

    func area() -> Double {
      switch self {
        case let .square(side: side):
            return side * side
        case let .rectangle(width: w, height: h):
             return w * h
      }
    }
}

var squareShape = ShapeDimensions.square(side: 10.0)
var rectShape = ShapeDimensions.rectangle(width: 5.0, height: 10.0)
```

### 结构体

结构体 （struct）是把相关数据块组合在一起放在内存中的一种类型。当需要把数据组合为一种通用类型时就可以用结构体。

```swift
struct Town {
    var population = 5_422
    var numberOfStoplights = 4

    func printDescription() {
        print("Population: \(population);
               number of stoplights: \(numberOfStoplights)")
    }
    // 如果结构体的一个实例方法要修改结构体的属性，就必须标记为 mutating
    mutating func changePopulation(by amount: Int) {
        population += amount
    }
}

var myTown = Town()
myTown.printDescription()
```

### 类

跟结构体类似，类可以用来为抽象成一个通用类型的相关数据建模

```swift
class Monster {
    var town: Town?
    var name = "Monster"

    func terrorizeTown() {
        if town != nil {
          print("\(name) is terrorizing a town!")

        } else {
          print("\(name) hasn't found a town to terrorize yet...")
        }
    }
}

var myTown = Town()
myTown.changePopulation(by: 500)
myTown.printDescription()

let genericMonster = Monster()
genericMonster.town = myTown
genericMonster.terrorizeTown()
```

类的一个主要特性是继承(inheritance),而这是结构体没有的。
继承是指由一个类（父类）定义另一个类（子类）的关系。
子类继承父类的属性和方法

```swift
class Zombie: Monster {
    var walksWithLimp = true

    final override func terrorizeTown() {
        town?.changePopulation(by: -10)
        super.terrorizeTown()
    }
}
```

### mutating 方法详解

mutating 方法的第一个参数是self ，并以 inout 的形式传入。
因为值类型在传递的时候会被复制，所以对于非 mutating 方法，self 其实是值的副本。
为了进行修改，self 需要被声明为 inout ，而 mutating 就是 Swift 编译器让你完成这个任务的工具。

### 属性

属性能够把值关联到类型上，从而模拟类型所表示的实体的性质。
属性的值可以是常量，也可以是变量。
类、结构体和枚举都可以有属性。

属性分为两种：**存储(stored)** 属性和 **计算(computed)** 属性。
存储属性可以有默认值，计算属性则根据已有信息返回某种计算结果。

```swift
struct Town {
    let region = "South" //let 创建只读属性
    var population = 5_422
    var numberOfStoplights = 4

    /*嵌套类型 （nested type）是定义在另一个类型内部的类型*/
    enum Size {
        case small
        case medium
        case large
    }
    
    func printDescription() {
        print("Population: \(population);
               number of stoplights: \(numberOfStoplights)")
    }

    mutating func changePopulation(by amount: Int) {
        population += amount
    }

    /*惰性加载 （lazy loading）属性的值只在第一次访问的时候才会赋值,后面不会再重新计算*/
    lazy var townSize: Size = {
        switch self.population {
        case 0...10_000:
            return Size.small
        case 10_001...100_000:
            return Size.medium
        default:
            return Size.large
        }
    }() 

    // 计算属性
    var townSize: Size = {
      get {
          switch self.population {
          case 0...10_000:
              return Size.small  
          case 10_001...100_000:
              return Size.medium
          default:
              return Size.large
            }
        }
    } 
}

class Monster {
    var town: Town?
    var name = "Monster"

    var victimPool: Int {
        get {
            return town?.population ?? 0
        }

        set(newVictimPool) {
            town?.population = newVictimPool
        }
    }
}

/* Swift 提供了一个有意思的特性，叫作属性观察 （property observation）。
属性观察者会观察并响应给定属性的变化。
属性观察对于任何自定义的存储属性和任何继承的属性都可用 */
// 通过 willSet 观察属性即将发生的变化；
// 通过 didSet 观察属性已经发生的变化。

struct Town {
    var population = 5_422 {
        didSet(oldPopulation) {
            print("The population has changed to \(population)
                   from \(oldPopulation).")

        }
    }
}

// 类型属性属性会在同类型实例间共享。
// 这类属性适合存储对于所有实例来说都相同的信息
struct Town {
    static let region = "South"
}

// 类也可以有存储类型属性和计算类型属性，语法跟结构体一样用 static 。子类不能覆盖父类的类型属性。
// 如果希望子类能为某个属性提供自己的实现，那就用 class 关键字
class Zombie: Monster {
     static let region = "South"
     class func makeSpookyNoise() -> String { 
         return "Brains..." 
    }
}

class Zombie: Monster {
    override class var spookyNoise: String {
        return "Brains..."
    }
    var walksWithLimp = true
}
```

### 访问控制

| 访问层级       | 描述                                                             |
| -------------- | ---------------------------------------------------------------- |
| open           | 实体对模块内的所有文件以及引入了该模块的文件都可见，并且可以继承 |
| Public         | 实体对模块内的所有文件以及引入了该模块的文件都可见               |
| internal[默认] | 实体对模块内的文件可见                                           |
| fileprivate    | 实体只对所在的源文件可见                                         |
| private        | 实体只对所在的作用域可见                                         |

```swift
// 把读取方法设置为 internal ，把写入方法设置为 private
internal private(set) var isFallingApart = false
```

### 初始化

初始化是设置类型实例的操作，包括给每个存储属性初始值，以及一些其他准备工作。
初始化方法 （initializer）能在创建实例的同时为其赋予合适的值。

```swift
struct CustomType {
    init(someValue: SomeType) {
        // 初始化代码
    }
}

class Zombie{
  
  init(limp: Bool, fallingApart: Bool, town: Town?, monsterName: String) {
  }

  // 用 convenience 关键字可以把初始化方法标记为便捷初始化方法。
  // 这个关键字告诉编译器：这个初始化方法需要把一部分工作委托给另一个初始化方法，直到调用到一个指定初始化方法。
  // 调用完成后，类的这个实例就可用了
  convenience init(limp: Bool, fallingApart: Bool) {
    self.init(limp: limp, fallingApart: fallingApart, town: nil, monsterName: "Fred")
  }

  // 类的必需初始化方法
  required init(town: Town?, monsterName: String) {
        self.town = town
        name = monsterName
  }

  // 可失败的初始化方法
  init?(region: String, population: Int, stoplights: Int) {
    guard population > 0 else {
      return nil
    }
    self.region = region
    self.population = population
    numberOfStoplights = stoplights
  }

  // 反初始化 （deinitialization）是在类的实例没用之后将其清除出内存的过程。
  deinit {
    print("Zombie named \(name) is no longer with us.")
  }
}
```

## 值类型与引用类型

### 值类型

```swift
var str = "Hello, playground"
var playgroundGreeting = str

playgroundGreeting += "! How are you today?"
str // playgroundGreeting 值改变不会带来 str 的改变
```

String 是值类型。在被赋给另一个实例或是作为参数传递给函数时，值类型总是被复制。
Swift 的基本类型（Array 、Dictionary 、Int 、String 等）都是用结构体实现的，所以都是值类型。
应尽量优先用 struct 实现数据建模，只有在需要的时候才用 class 。

### 引用类型

```swift
class GreekGod {
    var name: String

    init(name: String) {
        self.name = name
    }
}

let hecate = GreekGod(name: "Hecate")
let anotherHecate = hecate

anotherHecate.name = "AnotherHecate"
anotherHecate.name
hecate.name
```

对于引用来说，常量或变量都指向内存中的同一个实例。
因此，hecate 和 anotherHecate 都指向同一个 GreekGod 的实例

值类型常量和引用类型常量的行为不一样。
声明为常量的值类型不能改变属性，即使属性在类型实现中是用 var 声明的也是一样。

```swift
struct Pantheon {
    var chiefGod: String
}
let pantheon = Pantheon(chiefGod: "hecate")
pantheon.chiefGod = "zeus" // Cannot assign to property: 'pantheon' is a 'let' constant 。

// 引用类型不报错
class Pantheon{
  var chiefGod: String
}
let pantheon = Pantheon(chiefGod: "hecate")
pantheon.chiefGod = "zeus"
```

Swift 没有在语言层面提供深复制的支持，这意味着 Swift 中的复制是浅复制。浅复制不会创建实例的不同副本，而是复制这个实例的引用。

对于 Swift 中所有的基本数据类型（String 、Int 、Float 、Double 、Array 和 Dictionary ），都可以检查相等性。

对于引用类型，可以用同一性运算符（ === ）检查这两个常量的同一性，从而判断它们是否指向同一个实例。

### 类和结构体差异

结构体和类之间的重要区别会指引选择何时使用哪个。
由于要考虑的因素很多，从而很难定义严格的规则，但还是有一些基本指导原则。

1. 如果类型需要传值，那就用结构体。这么做会确保赋值或传递到函数参数中时类型被复制。
2. 如果类型不支持子类继承，那就用结构体。结构体不支持继承，所以不能有子类。
3. 如果类型要表达的行为相对比较直观，而且包含一些简单值，那么考虑优先用结构体实现。有必要的话，之后可以随时把结构体改成类。
4. 其他所有情况都用类。

### 写时复制

是指对值类型的底层存储的隐式共享。
这种优化能够让某个值类型的多个实例共享同一个底层存储，也就是每个实例自己并不持有一份数据的副本；
反之，每个实例会维护自己对同一份存储的引用。
如果某个实例需要修改或写入存储，那么这个实例就会产生一份自己的副本。
这意味着值类型能避免创建多余的数据副本。

```swift
fileprivate class IntArrayBuffer {
    var storage: [Int]

    init() {
        storage = []
    }

    init(buffer: IntArrayBuffer) {
        storage = buffer.storage
    }
}

struct IntArray {
    private var buffer: IntArrayBuffer

    init() {
        buffer = IntArrayBuffer()
    }

    func describe() {
        print(buffer.storage)
    }

    private mutating func copyIfNeeded() {
        if !isKnownUniquelyReferenced(&buffer) {
            print("Making a copy of \(buffer.storage)")
            buffer = IntArrayBuffer(buffer: buffer)
        }
    }

    mutating func insert(_ value: Int, at index: Int) {
        copyIfNeeded()
        buffer.storage.insert(value, at: index)
    }

    mutating func append(_ value: Int) {
        copyIfNeeded()
        buffer.storage.append(value)
    }

    mutating func remove(at index: Int) {
        copyIfNeeded()
        buffer.storage.remove(at: index)
    }
}

var integers = IntArray()

integers.append(1)
integers.append(2)
integers.append(4)
integers.describe()
```

## 协议和泛型

### 协议

协议 （protocol）能让你定义类型需要满足的接口。满足某个协议的类型被称为符合 （conform）这个协议。

```swift
protocol TabularDataSource {
  /* { get } 语法,表示这些属性可读。如果属性需要被读写，那就要用{ get set } */
  var numberOfRows: Int { get }
  var numberOfColumns: Int { get }

  func label(forColumn column: Int) -> String
  func itemFor(row: Int, column: Int) -> String
}

struct Department: TabularDataSource {
    let name: String
    var people = [Person]()

    init(name: String) {
        self.name = name
    }

    mutating func add(_ person: Person) {
        people.append(person)
    }

    var numberOfRows: Int {
        return people.count
    }

    func label(forColumn column: Int) -> String {
        switch column {
          case 0: return "Employee Name"
          case 1: return "Age"
          case 2: return "Years of Experience"
          default: fatalError("Invalid column!")
        }
    }

    func itemFor(row: Int, column: Int) -> String {
        let person = people[row]
        switch column {
          case 0: return person.name
          case 1: return String(person.age)
          case 2: return String(person.yearsOfExperience)
        }
    }        
}    
```

1. 哪些类型可以符合协议？
2. 一个类型可以符合多个协议吗？
3. 一个类可以在有父类的同时符合协议吗？

所有的类型都可以符合协议。刚才我们让一个结构体（Department）符合了一个协议。
枚举和类也可以符合协议。
声明枚举符合协议的语法跟结构体完全一样：类型声明后跟一个冒号和协议名。
一个类型也可以符合多个协议。
如果类有父类，那么父类的名字在前，后跟协议（或者多个协议）。

```swift
class ClassName: SuperClass, ProtocolOne, ProtocolTwo {
}
```

#### 协议继承

Swift支持协议继承 （protocol inheritance）。
继承另一个协议的协议要求符合的类型提供它本身及其所继承协议的所有属性和方法。
这跟类的继承不同：类的继承定义的是父类和子类之间的紧密联系，而协议继承只是把父协议的需求添加到子协议上。

```swift
protocol Animal {
    func eat()
}

protocol Speakable {
    func speak()
}

protocol Pet: Animal, Speakable {
    func play()
}
```

#### 协议组合

```swift
func printTable(dataSource: TabularDataSource & CustomStringConvertible) {
    print("Table: \(dataSource.description)")
}

protocol Toggleable {
    mutating func toggle()
}
```

协议组合的语法用关键字 & 中缀操作符告诉编译器，把多个协议组合成了单个的需求。

### 泛型

泛型 （generics）让我们写出的类型和函数可以使用对于我们或编译器都未知的类型。
Swift 用到的很多内建类型（包括可空类型、数组和字典）都是用泛型实现的。

```swift
struct Stack<Element> {
    var items = [Element]()

    mutating func push(_ newItem: Element) {
        items.append(newItem)
    }

    mutating func pop() -> Element? {
        guard !items.isEmpty else { 
          return nil
        }
        return items.removeLast()
    }
}

// 泛型方法
func myMap<T,U>(_ items: [T], _ f: (T) -> (U)) -> [U] {
    var result = [U]()

    for item in items {
        result.append(f(item))
    }

    return result
}

let strings = ["one", "two", "three"]
let stringLengths = myMap(strings) { $0.characters.count }
print(stringLengths) // 打印[3, 3, 5]

// 使用类型约束以便检查相等性 
func checkIfEqual<T: Equatable>(_ first: T, _ second: T) -> Bool {
    return first == second
}

// protocol 不可以用泛型 但是可以使用 关联类型
protocol IteratorProtocol {
    associatedtype Element
    mutating func next() -> Element?
}

struct StackIterator<T>: IteratorProtocol {
    typealias Element = T
    var stack: Stack<T>

    mutating func next() -> Element? {
        return stack.pop()
    }
}

var myStack = Stack<Int> ()
myStack.push(10)
myStack.push(20)
myStack.push(30)

var myStackIterator = StackIterator(stack: myStack)
while let value = myStackIterator.next() {
    print("got \(value)")
}

struct Stack<Element>: Sequence {
  mutating func pushAll<S: Sequence>(_ sequence: S)
    where S.Iterator.Element == Element {
        for item in sequence {
          self.push(item)
        }      
    }
}
```

## 错误处理

可能发生的错误分为两大类：

1. 可恢复的错误(recoverable error)
2. 不可恢复的错误(nonrecoverable error)

```swift
enum Token {
    case number(Int)
    case plus
}

class Lexer {
    enum Error: Swift.Error {
        case invalidCharacter(Character)
    }

    let input: String
    var position: String.Index

    init(input: String) {
        self.input = input
        self.position = self.input.startIndex
    }

    func peek() -> Character? {
        guard position < input.endIndex else {
            return nil
        }
        return input[position]
    }

    func advance() {
        assert(position < input.endIndex, "Cannot advance past endIndex!")
        position = input.index(after: position)
    }

    func getNumber() -> Int {
        var value = 0

        while let nextCharacter = peek() {
            switch nextCharacter {
            case "0" ... "9":
                // another digit - add it into value
                let digitValue = Int(String(nextCharacter))!
                value = 10 * value + digitValue
                advance() // ℃
            default:
                // a non-digit - go back to regular lexing
                return value
            }
        }
        return value
    }

    func lex() throws -> [Token] {
        var tokens = [Token]()

        while let nextCharacter = peek() {
            switch nextCharacter {
              case "0" ... "9":
                  let value = getNumber()
                  tokens.append(.number(value))
              case "+":
                  tokens.append(.plus)
                  advance()
              case " ":
                  // just advance to ignore spaces
                  advance()
              default:
                  throw Lexer.Error.invalidCharacter(nextCharacter)
            }
        }
        return tokens
    }
}

//----------------------------------------------------------

class Parser {   //Parser这个类
    enum Error: Swift.Error {
        case unexpectedEndOfInput
        case invalidToken(Token)
    }

    let tokens: [Token]
    var position = 0

    init(tokens: [Token]) {
        self.tokens = tokens
    }

    func getNextToken() -> Token? {
        guard position < tokens.count else {
          return nil 
        }
        let token = tokens[position]
        position += 1
        return token
    }

    func getNumber() throws -> Int {
        guard let token = getNextToken() else {
            throw Parser.Error.unexpectedEndOfInput
        }

        switch token {
          case .number(let value):
              return value

          case .plus:
              throw Parser.Error.invalidToken(token)
        }
    }

    func parse() throws -> Int {
        // Require a number first.
        var value = try getNumber()

        while let token = getNextToken() {
            switch token {
              // Getting a Plus after a Number is legal.
              case .plus:
                  // After a plus, we must get another number.
                  let nextNumber = try getNumber()
                  value += nextNumber
                  // Getting a Number after a Number is not legal.
              case .number:
                  throw Parser.Error.invalidToken(token)
            }
        }
        return value
    }
}


func evaluate(_ input: String) {
    print("Evaluating: \(input)")
    let lexer = Lexer(input: input)
    
    do {
        let tokens = try lexer.lex()
        print("Lexer output: \(tokens)")

        let parser = Parser(tokens: tokens)
        let result = try parser.parse()
        print("Parser output: \(result)")
    } catch Lexer.Error.invalidCharacter(let character) {
        print("Input contained an invalid character: \(character)")
    } catch Parser.Error.unexpectedEndOfInput {
        print("Unexpected end of input during parsing")
    } catch Parser.Error.invalidToken(let token) {
        print("Invalid token during parsing: \(token)")
    } catch {
        print("An error occurred: \(error)")
    }
}

//evaluate("10 + 3 + 5")
evaluate("10 + 3 + 46")
```

Swift 的设计理念是鼓励写安全、易读的代码，它的错误处理系统也一样。
任何可能失败的函数都应该用 throws 标记。

Swift 还需要把所有可能失败的函数调用用 try 标记。
在把一个函数标记为 throws 时，其实是把返回值类型从正常的类型变为要么是正常返回的类型，要么是 Error 协议的实例。

带 throws 的函数不会声明自己会抛出什么样的错误。这样会产生两个实际的影响:

1. 不需要修改函数的 API 就可以随意添加潜在的 Error 。
2. 在用 catch 处理错误时，必须总是准备好处理未知的错误类型。

## 扩展

Swift 提供一个叫扩展 （extension）的特性, 扩展能让你给已有的类型添加功能，可以用来扩展结构体、枚举和类。
对类型的扩展支持以下几种能力：

1. 添加计算属性；
2. 添加新初始化方法；
3. 使类型符合协议；
4. 添加新方法；
5. 添加嵌入类型

```swift
typealias Velocity = Double

extension Velocity {
    var kph: Velocity { return self * 1.60934 }
    var mph: Velocity { return self }
}

protocol Vehicle {
  var topSpeed: Velocity { get }
  var numberOfDoors: Int { get }
  var hasFlatbed: Bool { get }
}

struct Car {
    let make: String
    let model: String
    let year: Int
    let color: String
    let nickname: String
    var gasLevel: Double { 
      willSet {
            precondition(newValue <= 1.0 && newValue >= 0.0,
                         "New value must be between 0 and 1.")
      }
    }  
}

// 扩展协议
extension Car: Vehicle {
  var topSpeed: Velocity { return 180 }
  var numberOfDoors: Int { return 4 }
  var hasFlatbed: Bool { return false }
}

// 扩展初始化函数
extension Car {
    init(make: String, model: String, year: Int) {
      self.init(make: make,
            model: model,
            year: year,
            color: "Black",
            nickname: "N/A",
            gasLevel: 1.0)
    }
}

// 扩展嵌套类型
extension Car {
  enum Kind {
    case coupe, sedan
  }
}

// 扩展方法
extension Car {
    mutating func emptyGas(by amount: Double) {
        precondition(amount <= 1 && amount > 0,
                     "Amount to remove must be between 0 and 1.")
        gasLevel -= amount
    }

    mutating func fillGas() {
      gasLevel = 1.0
    }
}

var c = Car(make: "Ford", model: "Fusion", year: 2013)
```

### 协议扩展

```swift
protocol Exercise {
  var name: String { get }
  var caloriesBurned: Double { get }
  var minutes: Double { get }
}

struct EllipticalWorkout: Exercise {
    let name = "Elliptical Workout"
    let caloriesBurned: Double
    let minutes: Double
}
let ellipticalWorkout = EllipticalWorkout(caloriesBurned: 335, minutes: 30)

struct TreadmillWorkout: Exercise {
    let name = "Treadmill Workout"
    let caloriesBurned: Double
    let minutes: Double
    let laps: Double
}
let runningWorkout = TreadmillWorkout(caloriesBurned: 350, minutes: 25, laps: 10.5)
// 泛型函数，它的占位类型必须是符合 Exercise 协议的类型
func caloriesBurnedPerMinute<E: Exercise>(for exercise: E) -> Double {
    return exercise.caloriesBurned / exercise.minutes
}

print(caloriesBurnedPerMinute(for: ellipticalWorkout))
print(caloriesBurnedPerMinute(for: runningWorkout))

// 相当于 java interface default 函数
extension Exercise {
    var caloriesBurnedPerMinute: Double {
        return caloriesBurned / minutes
    }
}

// 协议扩展能给任何协议添加方法和计算属性。不过，协议扩展中添加的属性和方法只能使用肯定存在的其他属性和方法
extension Sequence where Iterator.Element == Exercise {
    func totalCaloriesBurned() -> Double {
        var total: Double = 0
        for exercise in self {
            total += exercise.caloriesBurned
        }
        return total
    }
}

// 用协议扩展提供默认实现
protocol Exercise: CustomStringConvertible {
    var name: String { get }
    var caloriesBurned: Double { get }
    var minutes: Double { get }
}

extension Exercise {
    var description: String {
        return "Exercise(\(name), burned \(caloriesBurned) calories in \(minutes) minutes)"
    }
}
```

## 内存管理和ARC

计算机程序会动态使用内存：程序在运行时动态分配和释放内存。
Swift 对内存管理的态度相对独特。
它会自动处理好大部分内存问题，但是并没有使用垃圾回收器（程序语言中自动内存管理的常用工具）。
与之相反，Swift 使用的是引用计数系统。

值类型（枚举和结构体）的内存分配和管理很简单。
新建值类型的实例时，系统会自动为实例划出大小合适的内存。
任何传递实例的动作，包括传递给函数以及存储到属性中，都会创建实例的副本。
当实例不再存在时，Swift会回收内存。不需要做任何事情来管理值类型的内存。

引用类型（特别是类）的内存管理。
新建类的实例时，系统会为实例分配内存，跟值类型一样。
不过，区别在于传递类实例时发生的事情。
把类实例传递给函数或存储到属性中会对同一块内存创建新的引用，而不是复制实例本身。
对同一块内存有多个引用意味着，只要任何一个引用修改了类实例，所有的引用就都能看到变化。

Swift 不像 C 那样需要手动管理内存，而是为每个类实例维护一个引用计数 （reference count）。
这是对组成类实例的内存的引用数量。只要引用计数大于0，实例就会存活。一旦引用计数变成0，实例就会被回收，deinit 方法运行。

自动引用计数 （automatic reference counting，ARC）下，编译器负责分析代码并在所有合适的位置插入保持和释放调用。Swift 也是在 ARC 的基础上构建的。不需要做任何事情来管理类实例的引用计数——编译器会帮你做。

循环强引用就是一种内存泄漏 （memory leak）。应用分配了足够Bob和其资产所需的内存，但是当程序不再需要这些内存后并没有将其还给系统。可以利用弱引用不增加所指向实例的引用计数的特性解决这个问题。

弱引用有两个条件：

1. 弱引用必须用var 声明，不能用let ；
2. 弱引用必须声明为可空类型。

```swift
class Asset: CustomStringConvertible {
    let name: String
    let value: Double
    weak var owner: Person?
}
```

还有一种更复杂的情况会产生循环强引用：在闭包中捕获 self

```swift

class Accountant {
    typealias NetWorthChanged = (Double) -> Void

    var netWorthChangedHandler: NetWorthChanged? = nil
    var netWorth: Double = 0.0 {
        didSet {
            netWorthChangedHandler?(netWorth)
        }
    }

    func gained(_ asset: Asset, completion: () -> Void) {
        netWorth += asset.value
        completion()
    }
}

class Asset : CustomStringConvertible {
    let name: String
    let value: Double
    weak var owner: Person?

    var description: String {
        if let actualOwner = owner {
            return "Asset(\(name), worth \(value), owned by \(actualOwner))"
        } else {
            return "Asset(\(name), worth \(value), not owned by anyone)"
        }
    }

    init(name: String, value: Double) {
        self.name = name
        self.value = value
    }

    deinit {
        print("\(self) is being deallocated")
    }
}

class Person : CustomStringConvertible {
    let name: String
    let accountant = Accountant()
    var assets = [Asset]()

    var description: String {
        return "Person(\(name))"
    }

    init(name: String) {
        self.name = name
        // 闭包能捕获在闭合作用域中定义的变量。
        // 默认情况下，闭包的捕获是通过对用到的变量的强引用实现的
        // Accountant 实际上有 对Person 的强引用！
        // Accountant 的netWorthChangedHandler 通过自己的 Person 的 self 强引用了这个 Person
        accountant.netWorthChangedHandler = {
            [weak self] netWorth in

            self?.netWorthDidChange(to: netWorth) ?? ()
            return
        }
    }

    deinit {
        print("\(self) is being deallocated")
    }

    func takeOwnership(of asset: Asset) {
        accountant.gained(asset) {
            asset.owner = self
            assets.append(asset)
        }
    }

    func netWorthDidChange(to netWorth: Double) {
        print("The net worth of \(self) is now \(netWorth)")
    }

    // @escaping 逃逸表示传递给一个函数的闭包可能会在该函数返回后被调用
    func useNetWorthChangedHandler(handler: @escaping (Double) -> Void) {
        accountant.netWorthChangedHandler = handler
    }
}

var bob: Person? = Person(name: "Bob")
print("created \(bob)")

var laptop: Asset? = Asset(name: "Shiny Laptop", value: 1_500.0)
var hat: Asset? = Asset(name: "Cowboy Hat", value: 175.0)
var backpack: Asset? = Asset(name: "Blue Backpack", value: 45.0)

bob?.useNetWorthChangedHandler { netWorth in
    print("Bob's net worth is now \(netWorth)")
}
bob?.takeOwnership(of: laptop!)
bob?.takeOwnership(of: hat!)

print("While Bob is alive, hat's owner is \(hat!.owner)")
bob = nil
print("the bob variable is now \(bob)")
print("After Bob is deallocated, hat's owner is \(hat!.owner)")

laptop = nil
hat = nil
backpack = nil
```

## 自定义运算符

```swift
class Person {
    var name: String
    weak var spouse: Person?

    init(name: String, spouse: Person?) {
        self.name = name
        self.spouse = spouse
    }
}

let matt = Person(name: "Matt", spouse: nil)
let drew = Person(name: "Drew", spouse: nil)

infix operator +++

func +++(lhs: Person, rhs: Person) {
    lhs.spouse = rhs
    rhs.spouse = lhs
}

matt +++ drew
matt.spouse?.name
drew.spouse?.name
```
