package librarycheckout;

import java.util.*;


// Custom Exceptions


// exceptions when book not available
class BookNotAvailableException extends Exception {
    public BookNotAvailableException(String msg) {
        super(msg);
    }
}
//exception when book not found
class BookNotFoundException extends Exception {
    public BookNotFoundException(String msg) {
        super(msg);
    }
}


// Book Class

class Book {
    private String title;
    private String author;
    private String genre;
    private boolean isAvailable;

    public Book(String title, String author, String genre) {
        this.title = title;
        this.author = author;
        this.genre = genre;
        isAvailable = true; //checks if books are available
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void checkout() throws BookNotAvailableException {
        if (!isAvailable) {
            if (!isAvailable) { 
                throw new BookNotAvailableException("Already checked out I guess.");
            }
        }
        isAvailable = false;
    }

    public void returnBook() {
        // checks if available
        isAvailable = true;
    }

    public String getGenre() {
        return genre;
    }

    public String getTitle() {
        return title; 
    }

    public String getAuthor() {
        return author;
    }

    public String toString() {
        return "\"" + title + "\" by " + author + " [" + genre + "] - " + (isAvailable ? "Available" : "Checked Out");
    }
}


// Factory — Book maker

class BookFactory {
    public static Book createBook(String title, String author, String genre) {
        return new Book(title, author, genre);
    }
}


// Book Vault (LibraryCollection)

class LibraryCollection implements Iterable<Book> {
    private Map<String, List<Book>> genreMap;

    public LibraryCollection() {
        genreMap = new HashMap<>();
    }

    public void addBook(Book book) {
        if (!genreMap.containsKey(book.getGenre())) {
            genreMap.put(book.getGenre(), new ArrayList<>());
        }
        genreMap.get(book.getGenre()).add(book);
    }

    public Iterator<Book> getGenreIterator(String genre) {
        List<Book> filtered = new ArrayList<>();
        List<Book> books = genreMap.get(genre);

       
        if (books != null) {
            for (Book b : books) {
                if (b.isAvailable()) {
                    filtered.add(b);
                }
            }
        }

        return filtered.iterator(); 
    }

    public Book findBookByTitle(String title) throws BookNotFoundException {
        for (String genre : genreMap.keySet()) {
            List<Book> books = genreMap.get(genre);
            for (Book b : books) {
                if (b.getTitle().equalsIgnoreCase(title)) {
                    return b;
                }
            }
        }
        throw new BookNotFoundException("Nope. Can't find '" + title + "'");
    }

    public Set<String> getGenres() {
        return genreMap.keySet(); 
    }

    public Iterator<Book> iterator() {
        List<Book> available = new ArrayList<>();
        for (List<Book> books : genreMap.values()) {
            for (Book b : books) {
                if (b.isAvailable()) {
                    available.add(b);
                }
            }
        }
        return available.iterator();
    }
}


// LibraryUser (user logic)

class LibraryUser {
    private String name;
    private Set<Book> borrowed; // prevents duplicates

    public LibraryUser(String name) {
        this.name = name;
        borrowed = new HashSet<>();
    }

    public void borrowBook(Book book) throws BookNotAvailableException {
        if (borrowed.contains(book)) {
            throw new BookNotAvailableException("Already borrowed that one.");
        }
        book.checkout();
        borrowed.add(book);
    }

    public void returnBook(Book book) {
        if (borrowed.contains(book)) {
            book.returnBook();
            borrowed.remove(book);
        }
    }

    public String getName() {
        return name;
    }

    public void printBorrowedBooks() {
        if (borrowed.isEmpty()) {
            System.out.println("You didn’t borrow anything yet.");
        } else {
            System.out.println("Your books:");
            for (Book b : borrowed) {
                System.out.println(" * " + b); 
            }
        }
    }
}


// Main Program Logic

public class LibraryTest {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        LibraryCollection library = new LibraryCollection();

        seedBooks(library); // puts books here

        System.out.print("Hey. What's your name? ");
        String userName = in.nextLine();
        LibraryUser user = new LibraryUser(userName);

        System.out.println("Welcome, " + userName + "!");
        boolean running = true;

        while (running) {
            System.out.println("\n========== LIBRARY MENU ==========");
            System.out.println("1. Browse genres");
            System.out.println("2. Checkout a book");
            System.out.println("3. Return a book");
            System.out.println("4. See my borrowed books");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            String choice = in.nextLine();

            switch (choice) {
                case "1":
                    listGenres(library);
                    System.out.print("Genre to browse: ");
                    String genre = in.nextLine();
                    showBooksInGenre(library, genre);
                    break;
                case "2":
                    System.out.print("Title to checkout: ");
                    String checkoutTitle = in.nextLine();
                    try {
                        Book book = library.findBookByTitle(checkoutTitle);
                        user.borrowBook(book);
                        System.out.println("Checked out: " + book.getTitle());
                    } catch (Exception e) {
                        System.out.println("Oops: " + e.getMessage());
                    }
                    break;
                case "3":
                    System.out.print("Book title to return: ");
                    String returnTitle = in.nextLine();
                    try {
                        Book book = library.findBookByTitle(returnTitle);
                        user.returnBook(book);
                        System.out.println("Returned: " + book.getTitle());
                    } catch (Exception e) {
                        System.out.println("Something went wrong returning.");
                    }
                    break;
                case "4":
                    user.printBorrowedBooks();
                    break;
                case "5":
                    running = false;
                    System.out.println("Bye!");
                    break;
                default:
                    System.out.println("Invalid... I think?");
            }
        }

       
    }

    // list of books
    private static void seedBooks(LibraryCollection lib) {
        lib.addBook(BookFactory.createBook("Dune", "Frank Herbert", "Sci-Fi"));
        lib.addBook(BookFactory.createBook("The Hobbit", "J.R.R. Tolkien", "Fantasy"));
        lib.addBook(BookFactory.createBook("1984", "George Orwell", "Dystopian"));
        lib.addBook(BookFactory.createBook("Pride and Prejudice", "Jane Austen", "Romance"));
        lib.addBook(BookFactory.createBook("The Great Gatsby", "F. Scott Fitzgerald", "Classic"));
        lib.addBook(BookFactory.createBook("The Martian", "Andy Weir", "Sci-Fi"));
        lib.addBook(BookFactory.createBook("The Name of the Wind", "Patrick Rothfuss", "Fantasy"));
        lib.addBook(BookFactory.createBook("To Kill a Mockingbird", "Harper Lee", "Classic"));
    }

    private static void listGenres(LibraryCollection lib) {
        System.out.println("Genres we have:");
        for (String g : lib.getGenres()) {
            System.out.println(" > " + g);
        }
    }

    private static void showBooksInGenre(LibraryCollection lib, String genre) {
        Iterator<Book> books = lib.getGenreIterator(genre);
        boolean any = false;

        System.out.println("Books in " + genre + ":");
        while (books.hasNext()) {
            Book b = books.next();
            System.out.println(" - " + b.toString());
            any = true;
        }

        if (!any) {
            System.out.println("Nothing here.");
        }
    }
}
