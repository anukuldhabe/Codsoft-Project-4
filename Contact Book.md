# Codsoft-Project-4
import json

# Initialize contact book
contact_book = {}

def save_contacts():
    with open("contacts.json", "w") as file:
        json.dump(contact_book, file)

def load_contacts():
    global contact_book
    try:
        with open("contacts.json", "r") as file:
            contact_book = json.load(file)
    except FileNotFoundError:
        contact_book = {}

def add_contact():
    name = input("Enter Name: ").strip()
    if name in contact_book:
        print("Contact already exists!")
        return
    
    phone = input("Enter Phone Number: ").strip()
    email = input("Enter Email: ").strip()
    address = input("Enter Address: ").strip()
    contact_book[name] = {"phone": phone, "email": email, "address": address}
    print("Contact added successfully!")
    save_contacts()

def view_contacts():
    if not contact_book:
        print("No contacts available.")
        return

    print("\nContact List:")
    for name, details in contact_book.items():
        print(f"Name: {name}, Phone: {details['phone']}")

def search_contact():
    search = input("Enter name or phone number to search: ").strip()
    found = False
    for name, details in contact_book.items():
        if search.lower() in name.lower() or search == details["phone"]:
            print(f"\nFound Contact:")
            print(f"Name: {name}")
            print(f"Phone: {details['phone']}")
            print(f"Email: {details['email']}")
            print(f"Address: {details['address']}")
            found = True
            break
    if not found:
        print("No matching contact found.")

def update_contact():
    name = input("Enter the name of the contact to update: ").strip()
    if name not in contact_book:
        print("Contact not found!")
        return

    print("Leave fields blank if no changes are needed.")
    phone = input(f"Enter new Phone Number ({contact_book[name]['phone']}): ").strip() or contact_book[name]['phone']
    email = input(f"Enter new Email ({contact_book[name]['email']}): ").strip() or contact_book[name]['email']
    address = input(f"Enter new Address ({contact_book[name]['address']}): ").strip() or contact_book[name]['address']
    
    contact_book[name] = {"phone": phone, "email": email, "address": address}
    print("Contact updated successfully!")
    save_contacts()

def delete_contact():
    name = input("Enter the name of the contact to delete: ").strip()
    if name in contact_book:
        del contact_book[name]
        print("Contact deleted successfully!")
        save_contacts()
    else:
        print("Contact not found!")

def main_menu():
    load_contacts()
    while True:
        print("\nContact Book Menu")
        print("1. Add Contact")
        print("2. View Contact List")
        print("3. Search Contact")
        print("4. Update Contact")
        print("5. Delete Contact")
        print("6. Exit")
        
        choice = input("Choose an option (1-6): ").strip()
        if choice == "1":
            add_contact()
        elif choice == "2":
            view_contacts()
        elif choice == "3":
            search_contact()
        elif choice == "4":
            update_contact()
        elif choice == "5":
            delete_contact()
        elif choice == "6":
            print("Exiting Contact Book. Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")

# Run the Contact Book
main_menu()
