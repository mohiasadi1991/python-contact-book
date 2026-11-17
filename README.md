import json
import os

FILE = "contacts.json"

def load_contacts():
    if os.path.exists(FILE):
        with open(FILE, "r", encoding="utf-8") as :
            return json.load(f)
    return []

def save_contacts(contacts):
    with open(FILE, "w", encoding="utf-8") as f:
        json.dump(contacts, f, indent=4, ensure_ascii=False)

def add_contact(contacts):
    name = input("Name: ").strip()
    phone = input("Phone: ").strip()
    email = input("Email: ").strip()

    if not name or not phone:
        print("Name and phone are required.")
        return

    contacts.append({
        "name": name,
        "phone": phone,
        "email": email
    })
    save_contacts(contacts)
    print("Contact added successfully!")

def show_contacts(contacts):
    if not contacts:
        print("No contacts found.")
        return

    for i, contact in enumerate(contacts, 1):
        print(
            f"{i}. {contact['name']} | "
            f"{contact['phone']} | {contact['email']}"
        )

def search_contact(contacts):
    keyword = input("Enter a name to search: ").lower()

    results = [
        c for c in contacts
        if keyword in c["name"].lower()
    ]

    if results:
        show_contacts(results)
    else:
        print("Contact not found.")

def update_contact(contacts):
    show_contacts(contacts)

    try:
        number = int(input("Contact number to update: "))
        if not 1 <= number <= len(contacts):
            print("Invalid number.")
            return

        contact = contacts[number - 1]

        name = input(f"New name [{contact['name']}]: ").strip()
        phone = input(f"New phone [{contact['phone']}]: ").strip()
        email = input(f"New email [{contact['email']}]: ").strip()

        if name:
            contact["name"] = name
        if phone:
            contact["phone"] = phone
        if email:
            contact["email"] = email

        save_contacts(contacts)
        print("Contact updated!")
    except ValueError:
        print("Please enter a valid number.")

def delete_contact(contacts):
    show_contacts(contacts)

    try:
        number = int(input("Contact number to delete: "))
        if 1 <= number <= len(contacts):
            removed = contacts.pop(number - 1)
            save_contacts(contacts)
            print(f"Deleted {removed['name']}.")
        else:
            print("Invalid number.")
    except ValueError:
        print("Please enter a valid number.")

def main():
    contacts = load_contacts()

    while True:
        print("\n===== CONTACT BOOK =====")
        print("1. Add contact")
        print("2. Show contacts")
        print("3. Search contact")
        print("4. Update contact")
        print("5. Delete contact")
        print("6. Exit")

        choice = input("Choose an option: ")

        if choice == "1":
            add_contact(contacts)
        elif choice == "2":
            show_contacts(contacts)
        elif choice == "3":
            search_contact(contacts)
        elif choice == "4":
            update_contact(contacts)
        elif choice == "5":
            delete_contact(contacts)
        elif choice == "6":
            print("Goodbye!")
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
