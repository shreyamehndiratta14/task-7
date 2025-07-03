 Task 7 – Creating Views

### Objective:

To learn how to create and use **SQL views** for simplifying data access, abstracting complex logic, and improving security in the **Library Management System** database.
 Tools Used:

* **MySQL Workbench** / **DB Browser for SQLite**

---

### Deliverables:

* View definitions using `CREATE VIEW`
* Example queries to use the views
* Practical use of views for abstraction, summarization, and filtering

---

###  Schema Context:

Based on existing tables:

* `Authors(AuthorID, Name, Country)`
* `Books(BookID, Title, AuthorID, Year)`
* `Borrowers(BorrowerID, Name, Email, Age)`
* `BorrowRecords(RecordID, BookID, BorrowerID, BorrowDate, ReturnDate)`

---

###  Views Created:

#### 1 `View_BooksWithAuthors`

**Purpose:** Combines `Books` and `Authors` to show book titles with their author names.

```sql
CREATE VIEW View_BooksWithAuthors AS
SELECT b.BookID, b.Title, a.Name AS AuthorName, b.Year
FROM Books b
LEFT JOIN Authors a ON b.AuthorID = a.AuthorID;
```

---

#### 2 `View_BorrowerSummary`

**Purpose:** Displays the number of books each borrower has borrowed.

```sql
CREATE VIEW View_BorrowerSummary AS
SELECT br.BorrowerID, br.Name, COUNT(r.RecordID) AS TotalBooksBorrowed
FROM Borrowers br
LEFT JOIN BorrowRecords r ON br.BorrowerID = r.BorrowerID
GROUP BY br.BorrowerID, br.Name;
```

---

#### 3 `View_ActiveBorrows`

**Purpose:** Lists active borrow transactions (books not yet returned).

```sql
CREATE VIEW View_ActiveBorrows AS
SELECT r.RecordID, b.Title, br.Name AS BorrowerName, r.BorrowDate
FROM BorrowRecords r
JOIN Books b ON r.BookID = b.BookID
JOIN Borrowers br ON r.BorrowerID = br.BorrowerID
WHERE r.ReturnDate IS NULL;
```

---

#### 4 `View_AuthorProductivity`

**Purpose:** Shows how many books each author has written.

```sql
CREATE VIEW View_AuthorProductivity AS
SELECT a.Name AS AuthorName, COUNT(b.BookID) AS BooksWritten
FROM Authors a
LEFT JOIN Books b ON a.AuthorID = b.AuthorID
GROUP BY a.AuthorID, a.Name;
```

---

###  Usage Examples:

```sql
SELECT * FROM View_BooksWithAuthors;
SELECT * FROM View_BorrowerSummary WHERE TotalBooksBorrowed > 1;
SELECT * FROM View_ActiveBorrows;
SELECT * FROM View_AuthorProductivity ORDER BY BooksWritten DESC;
```

---

###  Notes:

* Views help **hide complexity** from end users.
* You can restrict user access to underlying tables and expose only views.
* Views can be queried just like regular tables but do **not store data themselves**.
