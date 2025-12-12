import java.util.*;
import java.io.*;

// Book class
class Book {
    int id;
    String title;
    String author;
    boolean isIssued;

    public Book(int id, String title, String author) {
        this.id = id;
        this.title = title.trim();
        this.author = author.trim();
        this.isIssued = false;
    }

    public void issueBook() {
        isIssued = true;
    }

    public void returnBook() {
        isIssued = false;
    }

    @Override
    public String toString() {
        return id + ". " + title + " by " + author + (isIssued ? " [Issued]" : " [Available]");
    }
}

public class LibraryManagement {
    static ArrayList<Book> books = new ArrayList<>();
    static Scanner sc = new Scanner(System.in);
    static int bookIdCounter = 1;

    public static void main(String[] args) {
        int choice = -1;

        do {
            System.out.println("\n==== Library Management System ====");
            System.out.println("1. Add Book");
            System.out.println("2. View Books");
            System.out.println("3. Issue Book");
            System.out.println("4. Return Book");
            System.out.println("5. Search Book");
            System.out.println("6. Export Book List to File");
            System.out.println("7. Exit");
            System.out.print("Enter your choice: ");

            try {
                choice = Integer.parseInt(sc.nextLine());
                switch (choice) {
                    case 1 -> addBook();
                    case 2 -> viewBooks();
                    case 3 -> issueBook();
                    case 4 -> returnBook();
                    case 5 -> searchBook();
                    case 6 -> exportToFile();
                    case 7 -> System.out.println("Exiting... Thank you!");
                    default -> System.out.println("Invalid choice! Enter 1 to 7.");
                }
            } catch (NumberFormatException e) {
                System.out.println("Invalid input! Please enter a number.");
            }
        } while (choice != 7);
    }

    static void addBook() {
        System.out.print("Enter book title: ");
        String title = sc.nextLine().trim();
        System.out.print("Enter author name: ");
        String author = sc.nextLine().trim();

        if (title.isEmpty() || author.isEmpty()) {
            System.out.println("Title or Author cannot be empty.");
            return;
        }

        Book book = new Book(bookIdCounter++, title, author);
        books.add(book);
        System.out.println("Book added successfully.");
    }

    static void viewBooks() {
        if (books.isEmpty()) {
            System.out.println("No books available.");
        } else {
            System.out.println("\n--- Book List ---");
            for (Book b : books) {
                System.out.println(b);
            }
        }
    }

    static void issueBook() {
        int id = getBookIdInput("Enter book ID to issue: ");
        Book b = getBookById(id);
        if (b == null) {
            System.out.println("Book not found.");
            return;
        }

        if (!b.isIssued) {
            b.issueBook();
            System.out.println("Book issued successfully.");
        } else {
            System.out.println("Book is already issued.");
        }
    }

    static void returnBook() {
        int id = getBookIdInput("Enter book ID to return: ");
        Book b = getBookById(id);
        if (b == null) {
            System.out.println("Book not found.");
            return;
        }

        if (b.isIssued) {
            b.returnBook();
            System.out.println("Book returned successfully.");
        } else {
            System.out.println("Book was not issued.");
        }
    }

    static void searchBook() {
        System.out.print("Enter keyword to search (title or author): ");
        String keyword = sc.nextLine().toLowerCase();

        boolean found = false;
        for (Book b : books) {
            if (b.title.toLowerCase().contains(keyword) || b.author.toLowerCase().contains(keyword)) {
                System.out.println(b);
                found = true;
            }
        }

        if (!found) {
            System.out.println("No matching book found.");
        }
    }

    static void exportToFile() {
        try (PrintWriter pw = new PrintWriter("BookList.txt")) {
            for (Book b : books) {
                pw.println(b);
            }
            System.out.println("Book list exported to BookList.txt");
        } catch (IOException e) {
            System.out.println("Error writing to file.");
        }
    }

    static Book getBookById(int id) {
        for (Book b : books) {
            if (b.id == id) {
                return b;
            }
        }
        return null;
    }

    static int getBookIdInput(String prompt) {
        while (true) {
            System.out.print(prompt);
            try {
                return Integer.parseInt(sc.nextLine());
            } catch (NumberFormatException e) {
                System.out.println("Invalid input. Enter a valid book ID.");
            }
        }
    }
}
