# Hotel Management System
# File: hotel_management_system.py
# Author: [Your Name]
# Description: Console-based Hotel Management System
# Date: [Today's Date]

import os
import datetime

# Data structures for storing rooms and allocations
rooms = {}
allocations = {}

# File for storing room allocation details
FILE_NAME = "LHMS_Studentid.txt"

# Function Definitions
def display_menu():
    """Displays the main menu of the application."""
    print("\nHotel Management System")
    print("1. Add Room")
    print("2. Delete Room")
    print("3. Display Rooms Details")
    print("4. Allocate Rooms")
    print("5. Display Room Allocation Details")
    print("6. Billing & De-Allocation")
    print("7. Save Room Allocation to File")
    print("8. Show Room Allocation from File")
    print("9. Backup Room Allocation File")
    print("0. Exit Application")

def add_room():
    """Adds a new room to the system."""
    try:
        room_number = input("Enter room number: ")
        if room_number in rooms:
            raise ValueError("Room already exists!")
        room_features = input("Enter room features (e.g., AC, TV, WiFi): ")
        room_price = float(input("Enter room price per night: "))
        rooms[room_number] = {"features": room_features, "price": room_price}
        print(f"Room {room_number} added successfully!")
    except ValueError as ve:
        print(f"Error: {ve}")

def delete_room():
    """Deletes a room from the system."""
    try:
        room_number = input("Enter room number to delete: ")
        if room_number not in rooms:
            raise ValueError("Room not found!")
        del rooms[room_number]
        print(f"Room {room_number} deleted successfully!")
    except ValueError as ve:
        print(f"Error: {ve}")

def display_rooms():
    """Displays details of all available rooms."""
    if not rooms:
        print("No rooms available.")
        return
    for room_number, details in rooms.items():
        print(f"Room {room_number}: Features - {details['features']}, Price - ${details['price']:.2f}/night")

def allocate_room():
    """Allocates a room to a customer."""
    try:
        room_number = input("Enter room number to book: ")
        if room_number not in rooms:
            raise ValueError("Room not found!")
        if room_number in allocations:
            raise ValueError("Room is already booked!")
        customer_name = input("Enter customer name: ")
        nights = int(input("Enter number of nights: "))
        allocations[room_number] = {"customer_name": customer_name, "nights": nights}
        print(f"Room {room_number} booked successfully for {customer_name}.")
    except ValueError as ve:
        print(f"Error: {ve}")

def display_allocations():
    """Displays details of all allocated rooms."""
    if not allocations:
        print("No rooms are currently allocated.")
        return
    for room_number, details in allocations.items():
        print(f"Room {room_number}: Allocated to {details['customer_name']} for {details['nights']} nights.")

def billing_and_deallocate():
    """Bills a customer and deallocates a room."""
    try:
        room_number = input("Enter room number for billing and deallocation: ")
        if room_number not in allocations:
            raise ValueError("Room is not currently allocated!")
        customer_name = allocations[room_number]['customer_name']
        nights = allocations[room_number]['nights']
        room_price = rooms[room_number]['price']
        total_bill = nights * room_price
        print(f"Billing Details:\nCustomer: {customer_name}\nRoom: {room_number}\nTotal Bill: ${total_bill:.2f}")
        del allocations[room_number]
        print(f"Room {room_number} is now deallocated.")
    except ValueError as ve:
        print(f"Error: {ve}")

# File IO Operations (Menu Options 7, 8, and 9)
def save_allocations_to_file():
    """Saves current room allocations to a file."""
    try:
        with open(FILE_NAME, "w") as file:
            for room_number, details in allocations.items():
                file.write(f"{room_number},{details['customer_name']},{details['nights']}\n")
        print(f"Allocations saved to {FILE_NAME}.")
    except IOError as ioe:
        print(f"File error: {ioe}")

def show_allocations_from_file():
    """Displays room allocations saved in the file."""
    try:
        if not os.path.exists(FILE_NAME):
            raise FileNotFoundError(f"{FILE_NAME} does not exist.")
        with open(FILE_NAME, "r") as file:
            for line in file:
                room_number, customer_name, nights = line.strip().split(",")
                print(f"Room {room_number}: {customer_name} for {nights} nights.")
    except (FileNotFoundError, IOError) as fe:
        print(f"File error: {fe}")

def backup_file():
    """Backs up the allocation file."""
    try:
        if not os.path.exists(FILE_NAME):
            raise FileNotFoundError(f"{FILE_NAME} does not exist.")
        backup_name = f"LHMS_Studentid_Backup_{datetime.datetime.now().strftime('%Y%m%d_%H%M%S')}.txt"
        with open(FILE_NAME, "r") as original, open(backup_name, "w") as backup:
            backup.write(original.read())
        open(FILE_NAME, "w").close()  # Clear the original file
        print(f"Backup created: {backup_name}. Original file cleared.")
    except (FileNotFoundError, IOError) as fe:
        print(f"File error: {fe}")

def main():
    """Main function to run the application."""
    while True:
        display_menu()
        try:
            choice = int(input("Enter your choice: "))
            if choice == 1:
                add_room()
            elif choice == 2:
                delete_room()
            elif choice == 3:
                display_rooms()
            elif choice == 4:
                allocate_room()
            elif choice == 5:
                display_allocations()
            elif choice == 6:
                billing_and_deallocate()
            elif choice == 7:
                save_allocations_to_file()
            elif choice == 8:
                show_allocations_from_file()
            elif choice == 9:
                backup_file()
            elif choice == 0:
                print("Exiting application. Goodbye!")
                break
            else:
                print("Invalid choice! Please try again.")
        except ValueError:
            print("Please enter a valid number.")

if __name__ == "__main__":
    main()

