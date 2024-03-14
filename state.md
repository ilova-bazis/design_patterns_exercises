## Состояние / State
### Упражнение 1. Торговый Автомат
Вот пример кода которая моделирует торговый автомат, который продает различные товары:
Код реализует класс VendingMachine с различными состояниями (например, “ожидание”, “выбор”, “выдача”). 
Переход между состояниями осуществляется на основе действий пользователя (выбор товара, вставка монет).
Проведите рефакторинг этого кода, с применением паттерна **Состояние**.
```python
class VendingMachine:
    def __init__(self, products, prices):
        self.products = products
        self.prices = prices
        self.state = 'idle'
        self.selected_product = None
        self.inserted_coins = 0

    def select_product(self, product):
        if product in self.products:
            self.selected_product = product
            self.state = 'selecting'
            print(f'Product {product} selected. Please insert ${self.prices[product]}')
        else:
            print('Product not available.')

    def insert_coin(self, coin):
        if self.state == 'selecting':
            self.inserted_coins += coin
            print(f'Inserted ${coin}. Total: ${self.inserted_coins}')
            if self.inserted_coins >= self.prices[self.selected_product]:
                self.state = 'dispensing'
                self.dispense()
        else:
            print('Please select a product first.')

    def dispense(self):
        if self.state == 'dispensing':
            change = self.inserted_coins - self.prices[self.selected_product]
            print(f'Dispensing {self.selected_product}. Change: ${change}')
            self.state = 'idle'
            self.selected_product = None
            self.inserted_coins = 0
        else:
            print('Insufficient funds. Please insert more coins.')

# Example usage:
products = {'Coke': 1.25, 'Pepsi': 1.15, 'Water': 1.00}
vm = VendingMachine(products, prices=products)

# User actions:
vm.select_product('Coke')
vm.insert_coin(1)
vm.insert_coin(0.25)
```

