import json
import os

FILENAME = "products.json"

def load_products():
    if os.path.exists(FILENAME):
        with open(FILENAME, "r", encoding="utf-8") as f:
            return json.load(f)
    return {"categories": {}, "products": []}

def save_products(data):
    with open(FILENAME, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=4, ensure_ascii=False)

def add_category(data):
    category = input("Enter new category name: ")
    if category in data["categories"]:
        print("⚠️ Category already exists.")
    else:
        data["categories"][category] = []
        print(f"✅ Category '{category}' added.")
    save_products(data)

def add_product(data):
    name = input("Enter product name: ")
    price = float(input("Enter product price: "))
    category = input("Enter category: ")
    if category not in data["categories"]:
        print("⚠️ Category not found. Creating new category...")
        data["categories"][category] = []
    product = {"name": name, "price": price, "category": category}
    data["products"].append(product)
    data["categories"][category].append(name)
    print(f"✅ Product '{name}' added to category '{category}'.")
    save_products(data)

def list_products(data):
    if not data["products"]:
        print("📭 No products yet.")
        return
    print("\n=== Product List ===")
    for p in data["products"]:
        print(f"- {p['name']} | ${p['price']} | Category: {p['category']}")

def list_categories(data):
    if not data["categories"]:
        print("📭 No categories yet.")
        return
    print("\n=== Categories ===")
    for c, items in data["categories"].items():
        print(f"{c} ({len(items)} items)")

def search_product(data):
    keyword = input("Enter product name to search: ").lower()
    results = [p for p in data["products"] if keyword in p["name"].lower()]
    if not results:
        print("❌ No matching products found.")
    else:
        print("\n🔎 Search Results:")
        for p in results:
            print(f"- {p['name']} | ${p['price']} | {p['category']}")

def main():
    data = load_products()
    while True:
        print("\n=== SHOP PRODUCT MANAGER ===")
        print("1. Add category")
        print("2. Add product")
        print("3. List categories")
        print("4. List products")
        print("5. Search product")
        print("6. Exit")

        choice = input("Choose: ")
        if choice == "1":
            add_category(data)
        elif choice == "2":
            add_product(data)
        elif choice == "3":
            list_categories(data)
        elif choice == "4":
            list_products(data)
        elif choice == "5":
            search_product(data)
        elif choice == "6":
            print("👋 Exiting...")
            break
        else:
            print("⚠️ Invalid choice!")

if __name__ == "__main__":
    main()
