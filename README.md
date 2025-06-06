# Project name stock level monitoring

<!---
Ref27/Ref27 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

# stock_monitor.py

class Product:
    def __init__(self, name, quantity, min_stock):
        self.name = name
        self.quantity = quantity
        self.min_stock = min_stock

    def add_stock(self, amount):
        self.quantity += amount
        print(f"Added {amount} units to '{self.name}'. Current stock: {self.quantity}")

    def remove_stock(self, amount):
        if amount > self.quantity:
            print(f"Not enough stock for '{self.name}'. Available: {self.quantity}")
        else:
            self.quantity -= amount
            print(f"Removed {amount} units from '{self.name}'. Current stock: {self.quantity}")

    def check_stock(self):
        if self.quantity < self.min_stock:
            print(f"⚠️ ALERT: Stock for '{self.name}' is below minimum threshold ({self.quantity} < {self.min_stock})")


class Inventory:
    def __init__(self):
        self.products = {}

    def add_product(self, name, quantity, min_stock):
        self.products[name] = Product(name, quantity, min_stock)
        print(f"Product '{name}' added with {quantity} units. Min stock: {min_stock}")

    def update_stock(self, name, amount, action):
        if name in self.products:
            if action == "add":
                self.products[name].add_stock(amount)
            elif action == "remove":
                self.products[name].remove_stock(amount)
            self.products[name].check_stock()
        else:
            print(f"Product '{name}' not found!")

    def view_inventory(self):
        print("\n--- Current Inventory ---")
        for product in self.products.values():
            print(f"{product.name}: {product.quantity} units (Min: {product.min_stock})")
        print("-------------------------\n")


# Sample usage
def main():
    inventory = Inventory()
    inventory.add_product("Apples", 100, 20)
    inventory.add_product("Bananas", 50, 10)

    inventory.update_stock("Apples", 30, "remove")
    inventory.update_stock("Bananas", 45, "remove")
    inventory.update_stock("Apples", 20, "add")

    inventory.view_inventory()


if __name__ == "__main__":
    main()
