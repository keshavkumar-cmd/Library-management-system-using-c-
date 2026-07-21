# Library Management System (C++)

A console-based Library Management System built in C++ using Object-Oriented Programming principles. It manages books and members, handles issuing/returning of books with automatic stock tracking, and persists all data to disk using file handling.

## Features

- Add, display, search, update, and delete book records
- Add and display library member records
- Issue and return books with **automatic quantity tracking**
- Stock validation — prevents issuing a book that is out of stock
- Persistent storage using text files (`books.txt`, `members.txt`)
- Simple menu-driven console interface

## Tech Stack

- **Language:** C++
- **Paradigm:** Object-Oriented Programming (classes, encapsulation)
- **Storage:** Flat text files via `fstream`
- **Compiler:** g++ / any standard C++ compiler

## Project Structure

```
.
├── library_management.cpp   # Main source file (all classes + main)
├── books.txt                 # Auto-generated book data (created on save/exit)
├── members.txt                # Auto-generated member data (created on save/exit)
└── README.md
```

## Classes

| Class | Responsibility |
|---|---|
| `book` | Stores book details (id, title, author, publisher, quantity, issue status); handles input/display and issue/return logic |
| `member` | Stores member details (id, name, course, phone); handles input/display |
| `Library` | Controller class — manages arrays of books/members, all business logic, file I/O, and the main menu |

## Getting Started

### Prerequisites

- A C++ compiler (e.g. `g++`) installed on your system

### Compile

```bash
g++ -o library_management library_management.cpp
```

### Run

```bash
./library_management        # Linux / macOS
library_management.exe      # Windows
```

## Usage

On running the program, you'll see a menu:

```
====== Library Management System ======
1. Add Book
2. Display Books
3. Search Book
4. Update Book
5. Delete Book
6. Add Member
7. Display Members
8. Issue Book
9. Return Book
10. Save Books
11. Load Books
12. Save Members
13. Load Members
0. Exit
```

Enter the number corresponding to the action you want to perform. Books and members are auto-saved to their respective `.txt` files when you exit (option `0`).

> Data does **not** auto-load on startup by default — `obj.loadBooks()` and `obj.loadMembers()` are commented out in `main()`. Uncomment them if you want existing saved data to load automatically each time the program starts.

## Issue / Return Logic

Issuing and returning a book updates both the `isIssued` status **and** the `quantity` field:

```cpp
void book::issue()
{
    isIssued = true;
    quantity--;   // stock decreases
}

void book::Return()
{
    isIssued = false;
    quantity++;   // stock increases
}
```

`Library::issueBook()` also checks `quantity <= 0` before allowing an issue, so a book that's out of stock cannot be issued.

## Known Limitations

- Fixed-size arrays (max 100 books, 100 members)
- No linkage between a specific member and the book they issued
- No due dates or fine calculation
- Console-only interface, no GUI or authentication

## Future Improvements

- Replace fixed arrays with `std::vector` for unlimited records
- Track which member issued which book, with due dates and fines
- Move storage to a database (SQLite/MySQL)
- Add a GUI (Qt) or web-based front end
- Add librarian/admin login and authentication

## License

Free to use and modify for academic/learning purposes.

