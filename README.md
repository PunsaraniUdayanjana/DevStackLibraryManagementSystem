package com.company;


import java.util.*;

class LibraryItem {
    String name;
    String author;
    String serialNumber;
    boolean isBorrowed;

    public LibraryItem(String name, String author, String serialNumber) {
        this.name = name;
        this.author = author;
        this.serialNumber = serialNumber;
        this.isBorrowed = false;
    }
}

class User {
    String name;
    String borrowedItem;

    public User(String name) {
        this.name = name;
        this.borrowedItem = null;
    }
}

public class LibraryManagementSystem {
    private static List<LibraryItem> libraryItems = new ArrayList();
    private static List<User> users = new ArrayList();

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        initializeLibrary();

        while (true) {
            printLibraryState();
            System.out.println("Enter the main option:");
            System.out.println("1. Create new item");
            System.out.println("2. Create new user");
            System.out.println("3. User needs to borrow item");
            System.out.println("4. User needs to return item");
            System.out.println("5. Exit");

            int option = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (option) {
                case 1:
                    createNewItem(scanner);
                    break;
                case 2:
                    createNewUser(scanner);
                    break;
                case 3:
                    borrowItem(scanner);
                    break;
                case 4:
                    returnItem(scanner);
                    break;
                case 5:
                    System.out.println("Exiting program.");
                    return;
                default:
                    System.out.println("Invalid option. Try again.");
            }
        }
    }

    private static void initializeLibrary() {
        libraryItems.add(new LibraryItem("Time Magazine", "Briton Hadden and Henry Luce", "1001"));
        libraryItems.add(new LibraryItem("National Geographic Magazine", "Gardiner Greene Hubbard", "1002"));
        libraryItems.add(new LibraryItem("Vogue Magazine", "Arthur Baldwin Turnure", "1003"));
        libraryItems.add(new LibraryItem("Playboy Magazine", "Hugh Hefner", "1004"));
        libraryItems.add(new LibraryItem("The Lord of the Rings", "J.R.R. Tolkien", "4001"));
        libraryItems.add(new LibraryItem("To Kill a Mockingbird", "Harper Lee", "4002"));
        libraryItems.add(new LibraryItem("One Hundred Years of Solitude", "Gabriel García Márquez", "4003"));
        libraryItems.add(new LibraryItem("Crime and Punishment", "Fyodor Dostoevsky", "4004"));

        users.add(new User("kamala"));
        users.add(new User("nimanthi"));
        users.add(new User("latha"));
        users.add(new User("amala"));
        users.add(new User("kalpana"));
    }

    private static void printLibraryState() {
        System.out.println("\nLibrary Items:");
        for (LibraryItem item : libraryItems) {
            System.out.println(item.serialNumber + " - " + item.name + " by " + item.author + " (Borrowed: " + item.isBorrowed + ")");
        }

        System.out.println("\nLibrary Users:");
        for (int i = 0; i < users.size(); i++) {
            System.out.println((i + 1) + ". " + users.get(i).name);
        }

        System.out.println("\nBorrowed Items:");
        for (User user : users) {
            if (user.borrowedItem != null) {
                System.out.println(user.name + " - " + user.borrowedItem);
            }
        }
    }

    private static void createNewItem(Scanner scanner) {
        System.out.print("Enter item name: ");
        String name = scanner.nextLine();
        System.out.print("Enter author: ");
        String author = scanner.nextLine();
        System.out.print("Enter serial number: ");
        String serialNumber = scanner.nextLine();

        libraryItems.add(new LibraryItem(name, author, serialNumber));
        System.out.println("Item added successfully.");
    }

    private static void createNewUser(Scanner scanner) {
        System.out.print("Enter user name: ");
        String name = scanner.nextLine();
        users.add(new User(name));
        System.out.println("User added successfully.");
    }

    private static void borrowItem(Scanner scanner) {
        System.out.println("Which user is going to borrow an item?");
        printUsers();
        int userIndex = scanner.nextInt() - 1;
        scanner.nextLine();

        if (userIndex < 0 || userIndex >= users.size()) {
            System.out.println("Invalid user index.");
            return;
        }

        User user = users.get(userIndex);
        if (user.borrowedItem != null) {
            System.out.println(user.name + " already borrowed an item: " + user.borrowedItem);
            return;
        }

        System.out.print("Enter the serial number of the item: ");
        String serialNumber = scanner.nextLine();

        for (LibraryItem item : libraryItems) {
            if (item.serialNumber.equals(serialNumber)) {
                if (!item.isBorrowed) {
                    item.isBorrowed = true;
                    user.borrowedItem = item.name + " (" + serialNumber + ")";
                    System.out.println(item.name + " successfully borrowed by " + user.name);
                    return;
                } else {
                    System.out.println("Item already borrowed.");
                    return;
                }
            }
        }

        System.out.println("Item not found.");
    }

    private static void returnItem(Scanner scanner) {
        System.out.println("Which user is going to return an item?");
        printUsers();
        int userIndex = scanner.nextInt() - 1;
        scanner.nextLine();

        if (userIndex < 0 || userIndex >= users.size()) {
            System.out.println("Invalid user index.");
            return;
        }

        User user = users.get(userIndex);
        if (user.borrowedItem == null) {
            System.out.println(user.name + " has not borrowed any item.");
            return;
        }

        System.out.print("Enter the serial number of the item: ");
        String serialNumber = scanner.nextLine();

        for (LibraryItem item : libraryItems) {
            if (item.serialNumber.equals(serialNumber)) {
                if (item.isBorrowed) {
                    item.isBorrowed = false;
                    user.borrowedItem = null;
                    System.out.println("Item successfully returned.");
                    return;
                }
            }
        }

        System.out.println("Item not found or already returned.");
    }

    private static void printUsers() {
        for (int i = 0; i < users.size(); i++) {
            System.out.println((i + 1) + ". " + users.get(i).name);
        }
    }
}
