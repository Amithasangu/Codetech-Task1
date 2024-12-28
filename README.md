# Codetech-Task1
class LibraryItem:
    def __init__(self, title, author, category, item_id):
        self.title = title
        self.author = author
        self.category = category
        self.item_id = item_id
        self.is_checked_out = False
        self.due_date = None

class Library:
    def __init__(self):
        self.items = {}
        self.overdue_fine_per_day = 1.0  # Fine amount per day

    def add_item(self, title, author, category):
        item_id = len(self.items) + 1
        self.items[item_id] = LibraryItem(title, author, category, item_id)
        print(f"Item '{title}' added successfully with ID {item_id}.")

    def checkout_item(self, item_id, due_date):
        if item_id in self.items:
            item = self.items[item_id]
            if not item.is_checked_out:
                item.is_checked_out = True
                item.due_date = due_date
                print(f"Item '{item.title}' checked out successfully. Due date: {due_date}.")
            else:
                print(f"Item '{item.title}' is already checked out.")
        else:
            print("Invalid item ID.")

    def return_item(self, item_id, return_date):
        if item_id in self.items:
            item = self.items[item_id]
            if item.is_checked_out:
                item.is_checked_out = False
                overdue_days = max(0, (return_date - item.due_date).days)
                fine = overdue_days * self.overdue_fine_per_day
                item.due_date = None
                print(f"Item '{item.title}' returned successfully.")
                if overdue_days > 0:
                    print(f"Overdue by {overdue_days} days. Fine: ${fine:.2f}.")
            else:
                print(f"Item '{item.title}' was not checked out.")
        else:
            print("Invalid item ID.")

    def search_items(self, keyword):
        print(f"Search results for '{keyword}':")
        found = False
        for item in self.items.values():
            if keyword.lower() in item.title.lower() or keyword.lower() in item.author.lower() or keyword.lower() in item.category.lower():
                found = True
                status = "Available" if not item.is_checked_out else "Checked Out"
                print(f"- ID: {item.item_id}, Title: {item.title}, Author: {item.author}, Category: {item.category}, Status: {status}")
        if not found:
            print("No items found.")

# Main Program Execution
def main():
    import datetime

    library = Library()

    while True:
        print("\n--- Library Management System ---")
        print("1. Add New Item")
        print("2. Checkout Item")
        print("3. Return Item")
        print("4. Search Items")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            title = input("Enter the title: ")
            author = input("Enter the author: ")
            category = input("Enter the category (e.g., Book, Magazine, DVD): ")
            library.add_item(title, author, category)

        elif choice == '2':
            try:
                item_id = int(input("Enter the item ID to checkout: "))
                due_date_str = input("Enter the due date (YYYY-MM-DD): ")
                due_date = datetime.datetime.strptime(due_date_str, "%Y-%m-%d").date()
                library.checkout_item(item_id, due_date)
            except ValueError:
                print("Invalid input. Please enter valid data.")

        elif choice == '3':
            try:
                item_id = int(input("Enter the item ID to return: "))
                return_date_str = input("Enter the return date (YYYY-MM-DD): ")
                return_date = datetime.datetime.strptime(return_date_str, "%Y-%m-%d").date()
                library.return_item(item_id, return_date)
            except ValueError:
                print("Invalid input. Please enter valid data.")

        elif choice == '4':
            keyword = input("Enter a keyword to search (title, author, category): ")
            library.search_items(keyword)

        elif choice == '5':
            print("Exiting the program. Goodbye!")
            break

        else:
            print("Invalid choice. Please select a valid option.")

if __name__ == "__main__":
    main()
