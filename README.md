# SENG8071-Database-Testing-Final-Project
Online Bookstore Database Design: This Project designs a PostgreSQL database for an online bookstore with physical books, e-books, and audiobooks. Includes SQL scripts for schema creation, CRUD operations, specific queries, and a TypeScript interface for data modification.

# Online Bookstore

## Group 2 Members and Duties Assigned to Each Individual
1. **Ogechi Angela Ikediashi** 8913831
    - **Unit Test:** Unit tests that covers the CRUD operations.
    - **CRUD and Migration Script:** Implement CRUD functions to interact with every single table and Creation of a migration script for at least 3 rows of data in each table.


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



## Migration Script to input data into the Tables in the Database

```sql
INSERT INTO public."Customers" (cust_name, email_address, total_spent) VALUES
('John Doe', 'johndoe@example.com', 120.50),
('Jane Smith', 'janesmith@example.com', 200.00),
('Alice Johnson', 'alicej@example.com', 150.75),
('Bob Brown', 'bobbrown@example.com', 180.40),
('Charlie Davis', 'charlied@example.com', 90.20);
```


```sql
INSERT INTO public."Authors" (author_name) VALUES
('George Orwell'),
('J.K. Rowling'),
('Jane Austen'),
('Mark Twain'),
('Charles Dickens');
```


```sql
INSERT INTO public."Publishers" (publisher_name) VALUES
('Penguin Random House'),
('HarperCollins'),
('Simon & Schuster'),
('Hachette Livre'),
('Macmillan Publishers');
```


```sql
INSERT INTO public."Book_Format" (book_format_name) VALUES
('PhysicalBook'),
('Ebook'),
('Audiobook');
```


```sql
INSERT INTO public."Books" (book_title, book_genre, book_price, book_publish_date, book_average_rating, book_format_id, author_id, publisher_id) VALUES
('1984', 'Dystopian', 9.99, '1949-06-08', 4.8, 1, 1, 1),
('Harry Potter and the Sorcerer''s Stone', 'Fantasy', 19.99, '1997-06-26', 4.9, 1, 2, 2),
('Pride and Prejudice', 'Romance', 14.99, '1813-01-28', 4.7, 2, 3, 3),
('The Adventures of Tom Sawyer', 'Adventure', 12.99, '1876-06-30', 4.5, 2, 4, 4),
('A Tale of Two Cities', 'Historical Fiction', 11.99, '1859-04-30', 4.6, 1, 5, 5);
```


```sql
INSERT INTO public."Purchases" (purchase_date, amount_spent, customer_id, book_id) VALUES
('2023-01-15', 9.99, 1, 1),
('2023-02-10', 19.99, 2, 2),
('2023-03-20', 14.99, 3, 3),
('2023-04-25', 12.99, 4, 4),
('2023-05-30', 11.99, 5, 5);
```


```sql
INSERT INTO public."Reviews" (review_date, rating, comment, customer_id, book_id) VALUES
('2023-01-20', 4.8, 'A compelling and thought-provoking read.', 1, 1),
('2023-02-15', 4.9, 'Magical and enchanting, a must-read for all ages.', 2, 2),
('2023-03-25', 4.7, 'A classic tale of love and society.', 3, 3),
('2023-04-30', 4.5, 'A fun and adventurous story.', 4, 4),
('2023-06-05', 4.6, 'A powerful and moving historical novel.', 5, 5);
```


## Entity-Relationship (ER) diagram based on our database design:
The diagram represents the following relationships:
1. *Customers*:
   - customer_id (Primary Key)
   - cust_name
   - email_address
   - total_spent

2. *Authors*:
   - author_id (Primary Key)
   - author_name

3. *Publishers*:
   - publisher_id (Primary Key)
   - publisher_name

4. *Book_Format*:
   - book_format_id (Primary Key)
   - book_format_name

5. *Books*:
   - book_id (Primary Key)
   - book_title
   - book_genre
   - book_price
   - book_publish_date
   - book_average_rating
   - book_format_id (Foreign Key referencing Book_Format)
   - author_id (Foreign Key referencing Authors)
   - publisher_id (Foreign Key referencing Publishers)

6. *Purchases*:
   - purchase_id (Primary Key)
   - purchase_date
   - amount_spent
   - customer_id (Foreign Key referencing Customers)
   - book_id (Foreign Key referencing Books)

7. *Reviews*:
   - review_id (Primary Key)
   - review_date
   - rating
   - comment
   - customer_id (Foreign Key referencing Customers)
   - book_id (Foreign Key referencing Books)

THE IMAGE CAN BE FOUND IN THE WORD DOCUMENT




## CRUD functions to interact with every single table
CRUD (Create, Read, Update, Delete) SQL statements for each table in your database schema:
These statements cover all basic CRUD operations for interacting with the tables in our Postgres database.

### CRUD Statements for `Customers` Table

#### **Create:**
```sql
INSERT INTO public."Customers" (cust_name, email_address, total_spent) 
VALUES ('John Doe', 'johndoe@example.com', 100.00);
```

#### **Read:**
```sql
SELECT * FROM public."Customers" WHERE customer_id = 1;
```

#### **Update:**
```sql
UPDATE public."Customers" 
SET cust_name = 'John A. Doe', total_spent = 150.00 
WHERE customer_id = 1;
```

#### **Delete:**
```sql
DELETE FROM public."Customers" WHERE customer_id = 1;
```

---

### CRUD Statements for `Authors` Table

#### **Create:**
```sql
INSERT INTO public."Authors" (author_name) 
VALUES ('George Orwell');
```

#### **Read:**
```sql
SELECT * FROM public."Authors" WHERE author_id = 1;
```

#### **Update:**
```sql
UPDATE public."Authors" 
SET author_name = 'Eric Arthur Blair' 
WHERE author_id = 1;
```

#### **Delete:**
```sql
DELETE FROM public."Authors" WHERE author_id = 1;
```

---

### CRUD Statements for `Publishers` Table

#### **Create:**
```sql
INSERT INTO public."Publishers" (publisher_name) 
VALUES ('Penguin Random House');
```

#### **Read:**
```sql
SELECT * FROM public."Publishers" WHERE publisher_id = 1;
```

#### **Update:**
```sql
UPDATE public."Publishers" 
SET publisher_name = 'Random House' 
WHERE publisher_id = 1;
```

#### **Delete:**
```sql
DELETE FROM public."Publishers" WHERE publisher_id = 1;
```

---

### CRUD Statements for `Book_Format` Table

#### **Create:**
```sql
INSERT INTO public."Book_Format" (book_format_name) 
VALUES ('Hardcover');
```

#### **Read:**
```sql
SELECT * FROM public."Book_Format" WHERE book_format_id = 1;
```

#### **Update:**
```sql
UPDATE public."Book_Format" 
SET book_format_name = 'Paperback' 
WHERE book_format_id = 1;
```

#### **Delete:**
```sql
DELETE FROM public."Book_Format" WHERE book_format_id = 1;
```

---

### CRUD Statements for `Books` Table

#### **Create:**
```sql
INSERT INTO public."Books" (book_title, book_genre, book_price, book_publish_date, book_average_rating, book_format_id, author_id, publisher_id) 
VALUES ('1984', 'Dystopian', 9.99, '1949-06-08', 4.8, 1, 1, 1);
```

#### **Read:**
```sql
SELECT * FROM public."Books" WHERE book_id = 1;
```

#### **Update:**
```sql
UPDATE public."Books" 
SET book_price = 12.99, book_average_rating = 4.9 
WHERE book_id = 1;
```

#### **Delete:**
```sql
DELETE FROM public."Books" WHERE book_id = 1;
```

---

### CRUD Statements for `Purchases` Table

#### **Create:**
```sql
INSERT INTO public."Purchases" (purchase_date, amount_spent, customer_id, book_id) 
VALUES ('2023-01-15', 9.99, 1, 1);
```

#### **Read:**
```sql
SELECT * FROM public."Purchases" WHERE purchase_id = 1;
```

#### **Update:**
```sql
UPDATE public."Purchases" 
SET amount_spent = 10.99 
WHERE purchase_id = 1;
```

#### **Delete:**
```sql
DELETE FROM public."Purchases" WHERE purchase_id = 1;
```

---

### CRUD Statements for `Reviews` Table

#### **Create:**
```sql
INSERT INTO public."Reviews" (review_date, rating, comment, customer_id, book_id) 
VALUES ('2023-01-20', 4.8, 'Great book!', 1, 1);
```

#### **Read:**
```sql
SELECT * FROM public."Reviews" WHERE review_id = 1;
```

#### **Update:**
```sql
UPDATE public."Reviews" 
SET rating = 5.0, comment = 'Amazing read!' 
WHERE review_id = 1;
```

#### **Delete:**
```sql
DELETE FROM public."Reviews" WHERE review_id = 1;
```



## TypeScript Entities (ORM Models)

### 1. **`src/entities/Book.ts`:**
```typescript
import { Entity, PrimaryGeneratedColumn, Column, ManyToOne } from 'typeorm';
import { Author } from './Author';
import { Publisher } from './Publisher';
import { BookFormat } from './BookFormat';

@Entity()
export class Book {
  @PrimaryGeneratedColumn()
  book_id!: number;

  @Column()
  book_title!: string;

  @Column()
  book_genre!: string;

  @Column('decimal')
  book_price!: number;

  @Column('date')
  book_publish_date!: Date;

  @Column('decimal', { precision: 3, scale: 2 })
  book_average_rating!: number;

  @ManyToOne(() => BookFormat)
  book_format!: BookFormat;

  @ManyToOne(() => Author)
  author!: Author;

  @ManyToOne(() => Publisher)
  publisher!: Publisher;
}
```

### 2. **`src/entities/Author.ts`**
```typescript
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity()
export class Author {
  @PrimaryGeneratedColumn()
  author_id!: number;

  @Column({ length: 150 })
  author_name!: string;
}
```

### 3. **`src/entities/Publisher.ts`**
```typescript
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity()
export class Publisher {
  @PrimaryGeneratedColumn()
  publisher_id!: number;

  @Column({ length: 150 })
  publisher_name!: string;
}
```

### 4. **`src/entities/BookFormat.ts`**
```typescript
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity()
export class BookFormat {
  @PrimaryGeneratedColumn()
  book_format_id!: number;

  @Column({ length: 50 })
  book_format_name!: string;
}
```

### 5. **`src/entities/Customer.ts`**
```typescript
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity()
export class Customer {
  @PrimaryGeneratedColumn()
  customer_id!: number;

  @Column({ length: 150 })
  cust_name!: string;

  @Column({ length: 150 })
  email_address!: string;

  @Column('decimal', { precision: 10, scale: 2, nullable: true })
  total_spent!: number | null;
}
```

### 6. **`src/entities/Purchase.ts`**
```typescript
import { Entity, PrimaryGeneratedColumn, Column, ManyToOne } from 'typeorm';
import { Customer } from './Customer';
import { Book } from './Book';

@Entity()
export class Purchase {
  @PrimaryGeneratedColumn()
  purchase_id!: number;

  @Column('date')
  purchase_date!: Date;

  @Column('decimal', { precision: 10, scale: 2 })
  amount_spent!: number;

  @ManyToOne(() => Customer)
  customer!: Customer;

  @ManyToOne(() => Book)
  book!: Book;
}
```

### 7. **`src/entities/Review.ts`**
```typescript
import { Entity, PrimaryGeneratedColumn, Column, ManyToOne } from 'typeorm';
import { Customer } from './Customer';
import { Book } from './Book';

@Entity()
export class Review {
  @PrimaryGeneratedColumn()
  review_id!: number;

  @Column('date')
  review_date!: Date;

  @Column('decimal', { precision: 3, scale: 2 })
  rating!: number;

  @Column('text', { nullable: true })
  comment!: string | null;

  @ManyToOne(() => Customer)
  customer!: Customer;

  @ManyToOne(() => Book)
  book!: Book;
}
```


### 2. CRUD Service for Each Entity

**`src/services/BookService.ts`:**
```typescript
import { Repository } from 'typeorm';
import { Book } from '../entities/Book';
import { ICRUDService } from '../interfaces/ICRUDService';
import { AppDataSource } from '../data-source';

export class BookService implements ICRUDService<Book> {
  private bookRepository: Repository<Book>;

  constructor() {
    this.bookRepository = AppDataSource.getRepository(Book);
  }

  async create(item: Book): Promise<Book> {
    return await this.bookRepository.save(item);
  }

  async read(id: number): Promise<Book | null> {
    return await this.bookRepository.findOneBy({ book_id: id });
  }

  async update(id: number, item: Partial<Book>): Promise<Book | null> {
    await this.bookRepository.update(id, item);
    return this.read(id);
  }

  async delete(id: number): Promise<boolean> {
    const result = await this.bookRepository.delete(id);
    return result.affected !== 0;
  }

  async list(): Promise<Book[]> {
    return await this.bookRepository.find();
  }
}
```


### 3. Unit Tests

### **`src/tests/unit/BookService.test.ts`**
```typescript
import { BookService } from '../../services/BookService';
import { Book } from '../../entities/Book';
import { AppDataSource } from '../../data-source';
import { Repository } from 'typeorm';

describe('BookService Unit Tests', () => {
  let bookService: BookService;
  let bookRepository: Repository<Book>;

  beforeAll(async () => {
    await AppDataSource.initialize();
    bookRepository = AppDataSource.getRepository(Book);
    bookService = new BookService();
  });

  afterAll(async () => {
    await AppDataSource.destroy();
  });

  test('should create a book', async () => {
    const newBook: Book = {
      book_title: "New Book",
      book_genre: "Fiction",
      book_price: 19.99,
      book_publish_date: new Date(),
      book_average_rating: 4.5,
      book_format_id: 1,
      author_id: 1,
      publisher_id: 1
    };

    const createdBook = await bookService.create(newBook);
    expect(createdBook.book_id).toBeDefined();
    expect(createdBook.book_title).toBe("New Book");
  });

  test('should read a book by id', async () => {
    const bookId = 1;
    const book = await bookService.read(bookId);
    expect(book).not.toBeNull();
    if (book) {
      expect(book.book_id).toBe(bookId);
      expect(book.book_title).toBeDefined();
    }
  });

  test('should update a book by id', async () => {
    const bookId = 1;
    const updateData = { book_title: "Updated Book Title" };
    const updatedBook = await bookService.update(bookId, updateData);
    expect(updatedBook).not.toBeNull();
    if (updatedBook) {
      expect(updatedBook.book_id).toBe(bookId);
      expect(updatedBook.book_title).toBe("Updated Book Title");
    }
  });

  test('should delete a book by id', async () => {
    const bookId = 1;
    const deleteResult = await bookService.delete(bookId);
    expect(deleteResult).toBe(true);
    
    const deletedBook = await bookService.read(bookId);
    expect(deletedBook).toBeNull();
  });

  test('should list all books', async () => {
    const books = await bookService.list();
    expect(books.length).toBeGreaterThan(0);
    expect(books[0].book_id).toBeDefined();
  });
});
```



### 4. Integration Tests

**`src/tests/integration/BookIntegration.test.ts`:**
```typescript
import { BookService } from '../../services/BookService';
import { Book } from '../../entities/Book';
import { AppDataSource } from '../../data-source';

describe('BookService Integration Tests', () => {
  let bookService: BookService;

  beforeAll(async () => {
    await AppDataSource.initialize();
    bookService = new BookService();
  });

  afterAll(async () => {
    await AppDataSource.destroy();
  });

  test('should create, read, update, delete a book', async () => {
    const newBook: Book = {
      book_title: "New Book",
      book_genre: "Fiction",
      book_price: 19.99,
      book_publish_date: new Date(),
      book_average_rating: 4.5,
      book_format_id: 1,
      author_id: 1,
      publisher_id: 1
    };

    const createdBook = await bookService.create(newBook);
    const readBook = await bookService.read(createdBook.book_id!);
    expect(readBook).toBeDefined();
    
    const updatedBook = await bookService.update(createdBook.book_id!, { book_title: "Updated Book" });
    expect(updatedBook?.book_title).toBe("Updated Book");
    
    const deleted = await bookService.delete(createdBook.book_id!);
    expect(deleted).toBe(true);
  });
});
```











