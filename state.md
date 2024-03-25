## Состояние / State
### Упражнение 1. Торговый Автомат
Вот пример кода которая моделирует торговый автомат, который продает различные товары:
Код реализует класс VendingMachine с различными состояниями (например, “ожидание”, “выбор”, “выдача”). 
Переход между состояниями осуществляется на основе действий пользователя (выбор товара, вставка монет).
Проведите рефакторинг этого кода, с применением паттерна **Состояние**.
```python
class VendingMachine:
    def __init__(self, products):
        self.state = "idle"
        self.products = products
        self.inserted_coins = 0
        self.selected_product = None

    def insert_coin(self, coin_value):
        if self.state == "idle":
            self.inserted_coins += coin_value
            self.state = "selecting"
            print(f"You have inserted {coin_value} cents. Please select a product.")
        elif self.state == "selecting":
            self.inserted_coins += coin_value
            print(f"You have inserted {coin_value} more cents. Total: {self.inserted_coins} cents.")
        else:
            print("Cannot insert coins at this time.")

    def select_product(self, product_name):
        if self.state == "selecting":
            if product_name in self.products:
                product = self.products[product_name]
                if self.inserted_coins >= product["price"]:
                    self.selected_product = product
                    self.state = "dispensing"
                    print(f"You have selected {product_name}. Dispensing...")
                else:
                    print(f"Not enough coins inserted. {product_name} costs {product['price']} cents.")
            else:
                print("Invalid product selection.")
        else:
            print("Cannot select a product at this time.")

    def dispense(self):
        if self.state == "dispensing":
            if self.selected_product:
                print(f"Enjoy your {self.selected_product['name']}!")
                self.inserted_coins = 0
                self.selected_product = None
                self.state = "idle"
            else:
                print("No product selected.")
        else:
            print("Cannot dispense a product at this time.")

    def cancel(self):
        if self.state == "selecting":
            print(f"Transaction canceled. Returning {self.inserted_coins} cents.")
            self.inserted_coins = 0
            self.state = "idle"
        else:
            print("Cannot cancel at this time.")


products = {
    "Soda": {"name": "Soda", "price": 100},
    "Chips": {"name": "Chips", "price": 75},
    "Candy Bar": {"name": "Candy Bar", "price": 50}
}

vending_machine = VendingMachine(products)

vending_machine.insert_coin(25)
vending_machine.insert_coin(50)
vending_machine.select_product("Chips")
vending_machine.dispense()

vending_machine.insert_coin(100)
vending_machine.select_product("Soda")
vending_machine.dispense()

vending_machine.insert_coin(25)
vending_machine.cancel()
```

