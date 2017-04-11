# Flyweight Pattern

The flyweight pattern is typically used for efficiency reasons, in the flyweight pattern, objects are recycled rather than allocated and initialized from scratch.
### When to use?
  - An application uses a large number of same objects.
  - Storage costs are high because of the sheer quantity of objects.

### When NOT to use?
  - The flyweight pattern also should not be used with mutable objects that need to be reset/reinitialized when recycled, the result will cost more if you do this.


#### Example
```swift
import Foundation

// Instances of CoffeeFlavour will be the Flyweights
struct CoffeeFlavor : CustomStringConvertible {
    var flavor: String
    var description: String { return flavor }
}

// Menu acts as a factory and cache for CoffeeFlavour flyweight objects
struct Menu {
    private var flavors = [String: CoffeeFlavor]()

    mutating func lookup(flavor: String) -> CoffeeFlavor {
        if let f = flavors[flavor] { return f }
        else {
            let cFlavor = CoffeeFlavor(flavor: flavor)
            flavors[flavor] = cFlavor
            return cFlavor
        }
    }
}

struct CoffeeShop {
    private var orders = [Int: CoffeeFlavor]()
    private var menu = Menu()

    mutating func takeOrder(flavor flavor: String, table: Int) {
        orders[table] = menu.lookup(flavor)
    }

    func serve() {
        for (table, flavor) in orders {
            print("Serving \(flavor) to table \(table)")
        }
    }
}

let coffeeShop = CoffeeShop()
coffeeShop.takeOrder(flavor: "Cappuccino", table: 1)
coffeeShop.takeOrder(flavor: "Frappe", table: 3);
coffeeShop.takeOrder(flavor: "Espresso", table: 2);
coffeeShop.takeOrder(flavor: "Frappe", table: 15);
coffeeShop.takeOrder(flavor: "Cappuccino", table: 10);
coffeeShop.takeOrder(flavor: "Frappe", table: 8);
coffeeShop.takeOrder(flavor: "Espresso", table: 7);
coffeeShop.takeOrder(flavor: "Cappuccino", table: 4);
coffeeShop.takeOrder(flavor: "Espresso", table: 9);
coffeeShop.takeOrder(flavor: "Frappe", table: 12);
coffeeShop.takeOrder(flavor: "Cappuccino", table: 13);
coffeeShop.takeOrder(flavor: "Espresso", table: 5);
coffeeShop.serve()
```


# Strategy Pattern
Strategy pattern allows you to move behavior into separate class which is good for single responsibility

#### Example
```swift
protocol PrintStrategy {
    func print(_ string: String) -> String
}

final class Printer {

    private let strategy: PrintStrategy

    func print(_ string: String) -> String {
        return self.strategy.print(string)
    }

    init(strategy: PrintStrategy) {
        self.strategy = strategy
    }
}

final class UpperCaseStrategy: PrintStrategy {
    func print(_ string: String) -> String {
        return string.uppercased()
    }
}

final class LowerCaseStrategy: PrintStrategy {
    func print(_ string:String) -> String {
        return string.lowercased()
    }
}

//USAGE

var lower = Printer(strategy: LowerCaseStrategy())
lower.print("O tempora, o mores!")

var upper = Printer(strategy: UpperCaseStrategy())
upper.print("O tempora, o mores!")
```
