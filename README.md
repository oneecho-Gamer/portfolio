# portfolio
for the practice
# -phones.py*-coding: utf-*-
"""
this program maintains a database of names and phone numbers in csv file called myphones.csv.
It is run from the command line and is menudriven . to start it, save it in a directory and from the terminal run
>python phones.py

version final:

"""
import os
import csv

phones = []  # List to store the phone database
name_pos = 0  # Position of the name in each phone entry
phone_pos = 1   # Position of the phone number in each phone entry
phone_header = ['Name', 'Phone Number']  # Header for the phone database


def proper_menu_choice(which):
    # Check if the provided input is a valid phone number index
    if not which.isdigit():
        print("'"+which+"'needs to be the number of a phone!")
        return False
    which = int(which)
    if which < 1 or which > len(phones):
        print("'"+str(which)+"' needs to be the number of a phone!")
        return False
    return True


def delete_phone(which):
    # Delete a phone entry from the database
    if not proper_menu_choice(which):
        return
    which = int(which)

    del phones[which-1]
    print("Delete phone #", which)


def edit_phone(which):
    # Edit the details of a phone entry in the database
    if not proper_menu_choice(which):
        return
    which = int(which)
    phone = phones[which-1]
    print("Enter the data for a new phone. Press <Enter> to leave unchanged.")

    print(phone[name_pos])
    newname = input("Enter phone name to change or press return:")
    if newname == "":
        newname = phone[name_pos]

    print(phone[phone_pos])
    newphone_num = input("Enter new phone number to change or press return:")
    if newphone_num == "":
        newphone_num = phone[phone_pos]

    phone = [newname, newphone_num]
    phones[which-1] = phone


def save_phone_list():
    # Save the phone database to the myphones.csv file
    f = open("myphones.csv", 'w', "newline=")
    for item in phones:
        csv.writer(f).writerow(item)
    f.close()


def load_phone_list():
    # Load the phone database from the myphones.csv file
    if os.access("myphones.csv", os.F_OK):
        f = open("myphones.csv")
        for row in csv.reader(f):
            phones.append(row)
        f.close()


def show_phones():
    # Display all phone entries in the database
    show_phone(phone_header, "")
    index = 1
    for phone in phones:
        show_phone(phone, index)
        index = index + 1
    print()


def show_phone(phone, index):
    # Display a single phone entry with its index
    outputstr = "{0:>3} {1:<20} {2:>16}"
    print(outputstr.format(index, phone[name_pos], phone[phone_pos]))


def create_phone():
    # Create a new phone entry and add it to the database
    print("Enter the data for a new phone:")
    newname = input("Enter name:")
    newphone_num = input("Enter phone number:")
    phone = [newname, newphone_num]
    phones.append(phone)


def menu_choice():
    """Find out what the user wants to do next."""
    print("Choose one of the following option?")
    print("s)Show")
    print("n)New")
    print("d)Delete")
    print("e)Edit")
    print("q)Quit")
    choice = input("Choice:")
    if choice.lower() in ['n', 'd', 's', 'e', 'q']:
        return choice.lower()
    else:
        print(choice+"?")
        print("Invalid option")
        return None


def main_loop():
    # Load the phone database from the file
    load_phone_list()

    while True:
        choice = menu_choice()
        if choice is None:
            continue
        if choice == 'q':
            print("Exiting...")
            break  # Jump out of the while loop and exit the program
        elif choice == 'n':
            create_phone()
        elif choice == 'd':
            which = input("which item do you want to delete?")
            print("which is", which)
            delete_phone(which)
        elif choice == 's':
            show_phones()
        elif choice == 'e':
            which = input("which item do you to edit?")
            print("Which is", which)
            edit_phone(which)
        else:
            print("invalid choice.")
    # Save the updated phone database to the file
    save_phone_list()

# The following makes this program start running at main_loop()
# when executed as a stand-alone program.

if __name__ == "__main__":
    # Start the main loop of the program
    main_loop()
