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


import csv
import os

INVENTORY_FILE = 'inventory.csv'
LOW_STOCK_THRESHOLD = 10  # Change this value as needed

def load_inventory():
    inventory = {}
    if os.path.exists(INVENTORY_FILE):
        with open(INVENTORY_FILE, mode='r') as file:
            reader = csv.reader(file)
            next(reader)  # Skip header
            for row in reader:
                name, quantity = row
                inventory[name] = int(quantity)
    return inventory

def save_inventory(inventory):
    with open(INVENTORY_FILE, mode='w', newline='') as file:
        writer = csv.writer(file)
        writer.writerow(['Product', 'Quantity'])
        for name, quantity in inventory.items():
            writer.writerow([name, quantity])

def display_inventory(inventory):
    print("\n--- Current Inventory ---")
    for name, quantity in inventory.items():
        status = "⚠️ LOW STOCK!" if quantity < LOW_STOCK_THRESHOLD else ""
        print(f"{name}: {quantity} units {status}")
    print("-------------------------\n")

def update_stock(inventory):
    product = input("Enter product name: ").strip()
    if product not in inventory:
        print("Product not found. Would you like to add it?")
        choice = input("Type 'yes' to add: ").lower()
        if choice == 'yes':
            quantity = int(input("Enter initial stock quantity: "))
            inventory[product] = quantity
        else:
            return

    action = input("Type 'add' to increase or 'remove' to decrease stock: ").strip().lower()
    amount = int(input("Enter quantity: "))

    if action == 'add':
        inventory[product] += amount
    elif action == 'remove':
        if inventory[product] < amount:
            print("Not enough stock to remove that amount.")
        else:
            inventory[product] -= amount
    else:
        print("Invalid action.")

def main():
    inventory = load_inventory()

    while True:
        print("1. View Inventory")
        print("2. Update Stock")
        print("3. Save & Exit")
        choice = input("Choose an option: ")

        if choice == '1':
            display_inventory(inventory)
        elif choice == '2':
            update_stock(inventory)
        elif choice == '3':
            save_inventory(inventory)
            print("Inventory saved. Goodbye!")
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
