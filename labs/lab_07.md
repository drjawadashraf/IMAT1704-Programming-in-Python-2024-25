<table>
<tr>
<td><img src="dmuLogo.png" width="100"></td>
<td>&nbsp;</td>  
<td>
<span style="font-size:32px;">Lab 07: File Handling in the Shopping Cart System</span>
<br>
<span> Prepared by: Dr Jawad Ashraf </span>
<br>
<span> Year: 2024-25 </span>
</td>
</tr>
</table>

***

#### **Objective:**
This lab integrates file handling into the shopping cart system. Students will learn how to read from and write to files, handle data serialization, and export objects to persist the state of the application.

---

### **Total Duration: 2 Hours**

#### **Time Allocation:**
- **Introduction and Setup:** 10 minutes
- **Tasks 1 to 6:** 1 hour 30 minutes (approx. 15 minutes per task)
- **Discussion and Q&A:** 20 minutes

---

### **Learning Outcomes:**
By the end of this lab, students will:
1. Use Python’s `open()` function to read from and write to files.
2. Serialize and deserialize data using JSON.
3. Handle errors during file operations gracefully.
4. Export and import objects for state persistence.

---

### **Lab Tasks**

---

### **Part 1: Reading from Files**

#### **Task 1: Load Product Catalog from a File** (15 minutes)
Create a file `catalog.txt` with the following content:

```
apple,1.5,100
banana,0.75,150
milk,2.0,50
bread,2.5,80
eggs,3.0,60
```

Write a function `load_catalog_from_file(filename)` that reads the file and initializes the product catalog.

```python
def load_catalog_from_file(filename):
    catalog = []
    try:
        with open(filename, "r") as file:
            for line in file:
                name, price, stock = line.strip().split(",")
                catalog.append(Product(name, float(price), int(stock)))
        print("Catalog loaded successfully!")
    except FileNotFoundError:
        print(f"Error: {filename} not found.")
    return catalog
```

---

### **Part 2: Writing to Files**

#### **Task 2: Save Cart Contents to a File** (15 minutes)
Write a function `save_cart_to_file(cart, filename)` that saves the cart’s contents to a file in a readable format.

```python
def save_cart_to_file(cart, filename):
    try:
        with open(filename, "w") as file:
            for product, quantity in cart.cart:
                file.write(f"{product.name},{quantity},{product.price}\n")
        print(f"Cart saved to {filename}.")
    except IOError as e:
        print(f"Error writing to file: {e}")
```

---

### **Part 3: Serialization with JSON**

#### **Task 3: Export Product Catalog to JSON** (15 minutes)
Write a function `export_catalog_to_json(catalog, filename)` that exports the product catalog to a JSON file.

```python
import json

def export_catalog_to_json(catalog, filename):
    try:
        data = [{"name": p.name, "price": p.price, "stock": p.stock} for p in catalog]
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Catalog exported to {filename}.")
    except IOError as e:
        print(f"Error writing to file: {e}")
```

---

### **Part 4: Deserialization with JSON**

#### **Task 4: Import Product Catalog from JSON** (15 minutes)
Write a function `import_catalog_from_json(filename)` that imports the product catalog from a JSON file.

```python
def import_catalog_from_json(filename):
    try:
        with open(filename, "r") as file:
            data = json.load(file)
            return [Product(item["name"], item["price"], item["stock"]) for item in data]
    except FileNotFoundError:
        print(f"Error: {filename} not found.")
        return []
```

---

### **Part 5: Error Handling in File Operations**

#### **Task 5: Handle Missing Files Gracefully** (15 minutes)
Wrap all file operations in `try`-`except` blocks to ensure the program doesn’t crash if a file is missing or corrupted.

```python
def safe_open_file(filename, mode):
    try:
        return open(filename, mode)
    except FileNotFoundError:
        print(f"Error: {filename} not found.")
        return None
    except IOError as e:
        print(f"Error accessing file: {e}")
        return None
```

---

### **Part 6: Exporting and Importing Object State**

#### **Task 6: Persist the Cart State Using JSON** (15 minutes)
Write a function `save_cart_state(cart, filename)` to save the cart’s state to a JSON file and `load_cart_state(filename)` to restore it.

```python
def save_cart_state(cart, filename):
    try:
        data = [{"name": p.name, "quantity": q, "price": p.price} for p, q in cart.cart]
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Cart state saved to {filename}.")
    except IOError as e:
        print(f"Error writing to file: {e}")

def load_cart_state(filename):
    try:
        with open(filename, "r") as file:
            data = json.load(file)
            cart = ShoppingCart()
            for item in data:
                product = Product(item["name"], item["price"], 0)  # Stock is irrelevant for cart
                cart.add_item(product, item["quantity"])
            print("Cart state restored!")
            return cart
    except FileNotFoundError:
        print(f"Error: {filename} not found.")
        return ShoppingCart()
```

---

# Putting it all together
```python
import json

# Classes and Functions from Previous Labs

class Product:
    def __init__(self, name, price, stock):
        self.name = name
        self.price = price
        self.stock = stock

    def __str__(self):
        return f"{self.name}: ${self.price}, Stock: {self.stock}"


class ShoppingCart:
    def __init__(self):
        self.cart = []

    def add_item(self, product, quantity):
        if product.stock >= quantity:
            self.cart.append((product, quantity))
            product.stock -= quantity
            print(f"Added {quantity} {product.name}(s) to the cart.")
        else:
            print(f"Only {product.stock} {product.name}(s) available.")

    def remove_item(self, product):
        for item in self.cart:
            if item[0] == product:
                self.cart.remove(item)
                product.stock += item[1]
                print(f"Removed {product.name} from the cart.")
                return
        print(f"{product.name} is not in the cart.")

    def __str__(self):
        if not self.cart:
            return "Your cart is empty."
        result = "Your Cart:\n"
        for product, quantity in self.cart:
            result += f"{product.name}: {quantity} x ${product.price} = ${product.price * quantity}\n"
        return result

    def calculate_total(self):
        total = 0
        for product, quantity in self.cart:
            total += product.price * quantity
        return total


class ProductCatalog:
    def __init__(self):
        self.products = []

    def add_product(self, product):
        self.products.append(product)

    def search_product(self, name):
        for product in self.products:
            if product.name == name:
                return product
        return None


# File Handling Functions

def load_catalog_from_file(filename):
    catalog = ProductCatalog()
    try:
        with open(filename, "r") as file:
            for line in file:
                name, price, stock = line.strip().split(",")
                catalog.add_product(Product(name, float(price), int(stock)))
        print("Catalog loaded successfully!")
    except FileNotFoundError:
        print(f"Error: {filename} not found.")
    return catalog


def save_cart_to_file(cart, filename):
    try:
        with open(filename, "w") as file:
            for product, quantity in cart.cart:
                file.write(f"{product.name},{quantity},{product.price}\n")
        print(f"Cart saved to {filename}.")
    except IOError as e:
        print(f"Error writing to file: {e}")


def save_cart_state(cart, filename):
    try:
        data = [{"name": p.name, "quantity": q, "price": p.price} for p, q in cart.cart]
        with open(filename, "w") as file:
            json.dump(data, file, indent=4)
        print(f"Cart state saved to {filename}.")
    except IOError as e:
        print(f"Error writing to file: {e}")


def load_cart_state(catalog, filename):
    try:
        with open(filename, "r") as file:
            data = json.load(file)
            cart = ShoppingCart()
            for item in data:
                product = catalog.search_product(item["name"])
                if product:
                    cart.add_item(product, item["quantity"])
            print("Cart state restored!")
            return cart
    except FileNotFoundError:
        print(f"Error: {filename} not found.")
        return ShoppingCart()


# Main Program Logic

def main():
    catalog_file = "catalog.txt"
    cart_file = "cart.json"

    # Load catalog and cart state
    catalog = load_catalog_from_file(catalog_file)
    cart = load_cart_state(catalog, cart_file)

    while True:
        print("\n--- Shopping Cart Menu ---")
        print("1. View Product Catalog")
        print("2. Add Item to Cart")
        print("3. Remove Item from Cart")
        print("4. View Cart and Total")
        print("5. Save Cart")
        print("6. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            for product in catalog.products:
                print(product)
        elif choice == "2":
            product_name = input("Enter product name to add: ")
            product = catalog.search_product(product_name)
            if product:
                try:
                    quantity = int(input(f"Enter quantity for {product.name}: "))
                    cart.add_item(product, quantity)
                    save_cart_state(cart, cart_file)
                except ValueError:
                    print("Error: Quantity must be a number.")
            else:
                print("Product not found.")
        elif choice == "3":
            product_name = input("Enter product name to remove: ")
            product = catalog.search_product(product_name)
            if product:
                cart.remove_item(product)
                save_cart_state(cart, cart_file)
            else:
                print("Product not found.")
        elif choice == "4":
            print(cart)
            print(f"Total Cost: ${cart.calculate_total():.2f}")
        elif choice == "5":
            save_cart_to_file(cart, "cart.txt")
        elif choice == "6":
            print("Exiting. Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")

# Run the program
if __name__ == "__main__":
    main()

```
### **Conclusion:**
This lab demonstrates how to integrate file handling into the shopping cart system. Students will learn to persist application data, handle file-related errors, and serialize/deserialize objects for advanced use cases.

---

### **Time Management Recap:**
- **Introduction and Setup:** 10 minutes
- **Tasks 1 to 6:** 1 hour 30 minutes (15 minutes per task)
- **Discussion and Q&A:** 20 minutes
