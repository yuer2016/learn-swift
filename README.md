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
    - [协议和泛型](#协议和泛型)

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

let numberOfChapters = 3    // 还是Int类型,不过是编译器推断出来的

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

### 控制流

#### if

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

#### switch

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

#### for 循环

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

#### while 循环

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

### 容器

#### 数组

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

#### 字典

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

#### Set

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

### 函数

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

#### 闭包

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

#### 函数作为返回值

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

#### 函数作为参数

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

### 类和方法

#### 枚举

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

#### 方法

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

#### 结构体

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

#### 类

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

#### mutating 方法详解

mutating 方法的第一个参数是self ，并以 inout 的形式传入。
因为值类型在传递的时候会被复制，所以对于非 mutating 方法，self 其实是值的副本。
为了进行修改，self 需要被声明为 inout ，而 mutating 就是 Swift 编译器让你完成这个任务的工具。

#### 属性



### 协议和泛型
