# SENG8071-Database-Testing-Final-Project
Online Bookstore Database Design: This Project designs a PostgreSQL database for an online bookstore with physical books, e-books, and audiobooks. Includes SQL scripts for schema creation, CRUD operations, specific queries, and a TypeScript interface for data modification.

# Online Bookstore

## Group 2 Members and Duties Assigned to Each Individual
1. **Ogechi Angela Ikediashi** 8913831
    - **Unit Test:** Unit tests that covers the CRUD operations.
    - **Migration Script:** Creation of a migration script for 3 rows of data in each table.


2. **Motunrayo Ogunseye** 8953494
    - **Integration Test:**  Integration tests that covers the CRUD operations.


3. **Mohit Dipakkumar Patel** 8976585
    - **Entity Relationship Diagram:** Entity Relations diagram of your database design.



## Tables Required

### Customers
| Attribute      | Data Type      | Description and Constraits     |
|----------------|----------------|--------------------------------|
| customer_id    | SERIAL         | Primary Key and Not Null       |
| cust_name      | VARCHAR(150)   | Customer's name and Not Null   |
| email_address  | VARCHAR(150)   | Customer's email and Not Null  |
| total_spent    | NUMERIC(10,2)  | Total amount spent             |

### Authors
| Attribute      | Data Type      | Description and Constraits     |
|----------------|----------------|--------------------------------|
| author_id      | SERIAL         | Primary Key and Not Null       |
| author_name    | VARCHAR(150)   | Author's name and Not Null     |

### Publishers
| Attribute      | Data Type      | Description and Constraits     |
|----------------|----------------|--------------------------------|
| publisher_id   | SERIAL         | Primary Key and Not Null       |
| publisher_name | VARCHAR(150)   | Publisher's name and Not Null  |

### Book_Format
| Attribute           | Data Type      | Description and Constraits              |
|---------------------|----------------|-----------------------------------------|
| book_format_id      | SERIAL         | Primary Key and Not Null                |
| book_format_name    | VARCHAR(50)    | Book format (physical/e-book/audiobook) |

### Books
| Attribute           | Data Type      | Description and Constraits   |
|---------------------|----------------|------------------------------|
| book_id             | SERIAL         | Primary Key and Not Null     |
| book_title          | VARCHAR(250)   | Book title                   |
| book_genre          | VARCHAR(80)    | Book genre                   |
| book_price          | NUMERIC(10,2)  | Price of the book            |
| book_publish_date   | DATE           | Date of publication          |
| book_average_rating | NUMERIC(3,2)   | Average user rating          |
| book_format_id      | INT            | Foreign Key from Book Format |
| author_id           | INT            | Foreign Key from Authors     |
| publisher_id        | INT            | Foreign Key from Publishers  |

### Purchases
| Attribute      | Data Type      | Description and Constraits     |
|----------------|----------------|--------------------------------|
| purchase_id    | SERIAL         | Primary Key and Not Null       |
| purchase_date  | DATE           | Date of purchase               |
| amount_spent   | NUMERIC(10,2)  | Amount spent on purchase       |
| customer_id    | INT            | Foreign Key from Customers     |
| book_id        | INT            | Foreign Key from Books         |

### Reviews
| Attribute      | Data Type      | Description and Constraits     |
|----------------|----------------|--------------------------------|
| review_id      | SERIAL         | Primary Key and Not Null       |
| review_date    | DATE           | Date of review                 |
| rating         | NUMERIC(3,2)   | Rating given by customer       |
| comment        | TEXT           | Review comment                 |
| customer_id    | INT            | Foreign Key from Customers     |
| book_id        | INT            | Foreign Key from Books         |







## Sample SQL Code to Create the Database

```sql
CREATE TABLE public."Customers"
(
    customer_id serial NOT NULL,
    cust_name character varying(150) NOT NULL,
    email_address character varying(150) NOT NULL,
    total_spent numeric(10, 2),
    PRIMARY KEY (customer_id)
);

ALTER TABLE IF EXISTS public."Customers"
    OWNER to postgres;




CREATE TABLE public."Authors"
(
    author_id serial NOT NULL,
    author_name character varying(150) NOT NULL,
    PRIMARY KEY (author_id)
);

ALTER TABLE IF EXISTS public."Authors"
    OWNER to postgres;




CREATE TABLE public."Publishers"
(
    publisher_id serial NOT NULL,
    publisher_name character varying(150) NOT NULL,
    PRIMARY KEY (publisher_id)
);

ALTER TABLE IF EXISTS public."Publishers"
    OWNER to postgres;




CREATE TABLE public."Book_Format"
(
    book_format_id serial NOT NULL,
    book_format_name character varying(50),
    PRIMARY KEY (book_format_id)
);

ALTER TABLE IF EXISTS public."Book_Format"
    OWNER to postgres;




CREATE TABLE public."Books"
(
    book_id serial NOT NULL,
    book_title character varying(250),
    book_genre character varying(80),
    book_price numeric(10, 2),
    book_publish_date date,
    book_average_rating numeric(3, 2),
    book_format_id integer,
    author_id integer,
    publisher_id integer,
    PRIMARY KEY (book_id),
    CONSTRAINT book_format_id FOREIGN KEY (book_format_id)
        REFERENCES public."Book_Format" (book_format_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION,
    CONSTRAINT author_id FOREIGN KEY (author_id)
        REFERENCES public."Authors" (author_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION,
    CONSTRAINT publisher_id FOREIGN KEY (publisher_id)
        REFERENCES public."Publishers" (publisher_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
);

ALTER TABLE IF EXISTS public."Books"
    OWNER to postgres;




CREATE TABLE public."Purchases"
(
    purchase_id serial NOT NULL,
    purchase_date date,
    amount_spent numeric(10, 2),
    customer_id integer,
    book_id integer,
    PRIMARY KEY (purchase_id),
    CONSTRAINT customer_id FOREIGN KEY (customer_id)
        REFERENCES public."Customers" (customer_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION,
    CONSTRAINT book_id FOREIGN KEY (book_id)
        REFERENCES public."Books" (book_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
);

ALTER TABLE IF EXISTS public."Purchases"
    OWNER to postgres;




CREATE TABLE public."Reviews"
(
    review_id serial NOT NULL,
    review_date date,
    rating numeric(3, 2),
    comment text,
    customer_id integer,
    book_id integer,
    PRIMARY KEY (review_id),
    CONSTRAINT customer_id FOREIGN KEY (customer_id)
        REFERENCES public."Customers" (customer_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION,
    CONSTRAINT book_id FOREIGN KEY (book_id)
        REFERENCES public."Books" (book_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
);

ALTER TABLE IF EXISTS public."Reviews"
    OWNER to postgres;
```







## DDL/DML for Book_Format Table

```sql
-- CREATE Book_Format Table
CREATE TABLE public."Book_Format"
(
    book_format_id serial NOT NULL,
    book_format_name character varying(50),
    PRIMARY KEY (book_format_id)
);

ALTER TABLE IF EXISTS public."Book_Format"
    OWNER to postgres;


-- INSERT records into Book_Format Table
INSERT INTO public."Book_Format" (book_format_name)
VALUES
    ('Physical_book'),
    ('E-book'),
    ('Audiobook'),
    ('Any');

-- UPDATE a record in Book_Format Table
UPDATE public."Book_Format"
SET book_format_name = 'Others'
WHERE book_format_id = 4;

-- DELETE a record from Book_Format Table
DELETE FROM public."Book_Format"
WHERE book_format_id = 3;

-- SELECT a record from Book_Format Table
SELECT book_format_id, book_format_name
	FROM public."Book_Format";

-- ALTER and RENAME Book_Format Table
ALTER TABLE public."Book_Format"
    RENAME TO "Book_Formats";

-- Reverting the ALTER and RENAME done to Book_Format Table above
ALTER TABLE public."Book_Formats"
    RENAME TO "Book_Format";

-- TRUNCATE a record from Book_Format Table
TRUNCATE TABLE public."Book_Format" CASCADE; -- This script will truncate (Delete all data from) all referencing tables and tables of referencing tables. In this case, the truncate cascades to tables "Books", "Purchases", and "Reviews" because of the foreign keys "book_format_id" and "book_id".

-- DROP a record from Book_Format Table
DROP TABLE IF EXISTS public."Book_Format" CASCADE; -- This script cascades to the constraint "book_format_id" on table "Books", hence the constraint is removed but both the column ("book_format_id") and table ("Books") remains.
```




## SQL Queries for Requirements

#### 1. Power writers (Authors) with more than X books in the same genre published within the last X years. In our case number of books will be 15 and number of year will be 7, this values can be replaced by updating the query:

```sql
WITH recent_books AS (
    SELECT
        author_id,
        book_genre,
        COUNT(*) AS book_count
    FROM public."Books"
    WHERE book_publish_date >= (CURRENT_DATE - INTERVAL '7 years')
    GROUP BY author_id, book_genre
)
SELECT
    a.author_id,
    a.author_name,
    rb.book_genre,
    rb.book_count
FROM recent_books rb
JOIN public."Authors" a ON a.author_id = rb.author_id
WHERE rb.book_count > 15;
```



#### 2. Loyal Customers who have spent more than X dollars in the last year. In our case "X here will be CAD5000", this amount can be replaced by updating the query:


```sql
SELECT
    c.customer_id,
    c.cust_name,
    c.email_address,
    SUM(p.amount_spent) AS total_spent
FROM public."Customers" c
JOIN public."Purchases" p ON c.customer_id = p.customer_id
WHERE p.purchase_date >= (CURRENT_DATE - INTERVAL '1 year')
GROUP BY c.customer_id, c.cust_name, c.email_address
HAVING SUM(p.amount_spent) > 5000;
```



#### 3. Well Reviewed books that have a better user rating than average:

```sql
WITH average_rating AS (
    SELECT AVG(book_average_rating) AS avg_rating FROM public."Books"
)
SELECT
    b.book_id,
    b.book_title,
    b.book_average_rating
FROM public."Books" b, average_rating ar
WHERE b.book_average_rating > ar.avg_rating;
```

#### 4. The most popular genre by sales:

```sql
SELECT
    b.book_genre,
    SUM(p.amount_spent) AS total_sales
FROM public."Purchases" p
JOIN public."Books" b ON p.book_id = b.book_id
GROUP BY b.book_genre
ORDER BY total_sales DESC
LIMIT 1;
```

#### 5. The 10 most recent posted reviews by Customers:

```sql
SELECT
    r.review_id,
    r.review_date,
    r.rating,
    r.comment,
    c.cust_name,
    b.book_title
FROM public."Reviews" r
JOIN public."Customers" c ON r.customer_id = c.customer_id
JOIN public."Books" b ON r.book_id = b.book_id
ORDER BY r.review_date DESC
LIMIT 10;
```





## Typescript Interface

```typescript
interface ICRUDService<T> {
  create(item: T): Promise<T>;
  read(id: number): Promise<T | null>;
  update(id: number, item: Partial<T>): Promise<T | null>;
  delete(id: number): Promise<boolean>;
  list(): Promise<T[]>;
}

interface Book {
  book_id?: number;
  book_title: string;
  book_genre: string;
  book_price: number;
  book_publish_date: Date;
  book_average_rating: number;
  book_format_id: number;
  author_id: number;
  publisher_id: number;
}

class BookService implements ICRUDService<Book> {
  private books: Book[] = [];
  private currentId: number = 1;

  async create(item: Book): Promise<Book> {
    item.book_id = this.currentId++;
    this.books.push(item);
    return item;
  }

  async read(id: number): Promise<Book | null> {
    const book = this.books.find(book => book.book_id === id) || null;
    return book;
  }

  async update(id: number, item: Partial<Book>): Promise<Book | null> {
    const index = this.books.findIndex(book => book.book_id === id);
    if (index === -1) return null;
    this.books[index] = { ...this.books[index], ...item };
    return this.books[index];
  }

  async delete(id: number): Promise<boolean> {
    const index = this.books.findIndex(book => book.book_id === id);
    if (index === -1) return false;
    this.books.splice(index, 1);
    return true;
  }

  async list(): Promise<Book[]> {
    return this.books;
  }
}

async function testBookService() {
  const bookService = new BookService();

  const newBook: Book = {
    book_title: "New Book Title",
    book_genre: "Science Fiction",
    book_price: 19.99,
    book_publish_date: new Date('2023-05-20'),
    book_average_rating: 4.8,
    book_format_id: 1,
    author_id: 1,
    publisher_id: 1
  };

  const createdBook = await bookService.create(newBook);
  console.log("Created Book:", createdBook);

  const readBook = await bookService.read(createdBook.book_id!);
  console.log("Read Book:", readBook);

  const updatedBook = await bookService.update(createdBook.book_id!, { book_title: "Updated Book Title" });
  console.log("Updated Book:", updatedBook);

  const deleted = await bookService.delete(createdBook.book_id!);
  console.log("Deleted:", deleted);

  const allBooks = await bookService.list();
  console.log("All Books:", allBooks);
}

testBookService();
```



