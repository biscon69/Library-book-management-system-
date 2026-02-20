 Library Management System

A Python-based GUI application for managing library books, users, and loans.

 Features
- Catalog: View all books, search by title, filter by category, and see availability status.
- Add Book: Add new books to the library collection with categorization.
- Users: Register new users and view their currently borrowed books.
- Circulation: Borrow and return books using Book Code and User Card ID.
- Fines & Reports: Automatic fine calculation for late returns and a dashboard for library statistics.
- Admin Security: Secure login for administrators.

 Requirements
- Python 3.x
- `tkinter` (usually included with Python)
- `sqlite3` (standard library)

 How to Run
1. Open a terminal or command prompt.
2. Navigate to this directory.
3. Run the application:
   ```bash
   python main.py
   ```
4. Login:
   - Username: `libraryadmin`
   - Password: `admin123`
5. The database file `library.db` will be created automatically in the same directory.

---

 3. METHODOLOGY

 3.2 Tools & Libraries Used

| Tool/Library | Version | Justification |
| :--- | :--- | :--- |
| Python | 3.x | Selected for its readability, vast standard library, and rapid development capabilities. |
| Tkinter | Built-in | The standard Python interface to the Tk GUI toolkit. Used for creating the graphical user interface as it is lightweight and requires no external installation. |
| SQLite3 | Built-in | A C-library that provides a lightweight disk-based database. Used for persistent data storage because it is serverless and integrated directly into Python. |
| Datetime | Built-in | Used to handle date calculations for borrowing periods (1-4 weeks), due dates, and calculating the number of days a book is overdue. |

 3.4 Algorithm/Flowchart of Core Features

1. Login Flow
> Start -> Enter Credentials -> Validate (libraryadmin/admin123) -> If Valid: Open Main Dashboard; Else: Show Error.

2. Borrow Book Flow
> Input Book Code & User ID -> Select Duration (e.g., 2 weeks) -> Check Availability -> If Available:
>   - Calculate `Due Date` = Current Date + Duration
>   - Update Book Status to 'Issued'
>   - Create Loan Record
> -> Show Success Message.

3. Return Book (Fine Calculation) Flow
> Input Book Code -> Retrieve Active Loan Record -> Compare `Current Date` with `Due Date`.
> - If Current Date > Due Date: 
>   - `Late Days` = Current Date - Due Date
>   - `Fine` = Late Days  N1000.00
> - Else: Fine = 0.
> -> Update Loan Record (Return Date, Fine) -> Set Book Status to 'Available' -> Display Result (inc. Fine if any).

 3.5 Database & File Structure

 File Structure
```
Library_Book_Management_System/
│
├── main.py           Core application logic (GUI + Database class)
├── library.db        SQLite database file (auto-generated on run)
└── README.md         Project documentation
```

 Database Schema
The system uses a relational database (`library.db`) with three connected tables:

1.  `books`
       `id` (Primary Key)
       `title`, `author`, `code` (Unique), `category`
       `status` (Available/Issued)

2.  `users`
       `id` (Primary Key)
       `name`
       `card_id` (Unique)

3.  `loans`
       `id` (Primary Key)
       `book_id` (Foreign Key -> books)
       `user_id` (Foreign Key -> users)
       `issue_date`: Date the book was borrowed.
       `due_date`: Calculated return deadline.
       `return_date`: Actual date returned (NULL if still out).
       `fine`: Monetary penalty for late returns.
