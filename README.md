CREATE TABLE Genres (
    genre_id INT PRIMARY KEY AUTO_INCREMENT,
    genre_name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE Publishers (
    publisher_id INT PRIMARY KEY AUTO_INCREMENT,
    publisher_name VARCHAR(200) NOT NULL UNIQUE,
    location VARCHAR(255)
);

CREATE TABLE Authors (
    author_id INT PRIMARY KEY AUTO_INCREMENT,
    author_name VARCHAR(200) NOT NULL UNIQUE
);


CREATE TABLE Books (
    book_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    isbn VARCHAR(13) UNIQUE NOT NULL,
    publication_year YEAR,
    genre_id INT,
    publisher_id INT,
    total_copies INT NOT NULL DEFAULT 1,
    available_copies INT NOT NULL DEFAULT 1,
    FOREIGN KEY (genre_id) REFERENCES Genres(genre_id),
    FOREIGN KEY (publisher_id) REFERENCES Publishers(publisher_id)
);

CREATE TABLE Members (
    member_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    phone_number VARCHAR(15),
    registration_date DATE NOT NULL,
    address TEXT
);

CREATE TABLE BookAuthors (
    book_id INT,
    author_id INT,
    PRIMARY KEY (book_id, author_id),
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE,
    FOREIGN KEY (author_id) REFERENCES Authors(author_id) ON DELETE CASCADE
);

CREATE TABLE Loans (
    loan_id INT PRIMARY KEY AUTO_INCREMENT,
    book_id INT NOT NULL,
    member_id INT NOT NULL,
    loan_date DATE NOT NULL,
    due_date DATE NOT NULL,
    return_date DATE,
    status ENUM('Active', 'Returned', 'Overdue') DEFAULT 'Active',
    FOREIGN KEY (book_id) REFERENCES Books(book_id),
    FOREIGN KEY (member_id) REFERENCES Members(member_id)
);

CREATE TABLE Fines (
    fine_id INT PRIMARY KEY AUTO_INCREMENT,
    loan_id INT NOT NULL UNIQUE,
    amount DECIMAL(10, 2) NOT NULL,
    issue_date DATE NOT NULL,
    paid_date DATE,
    status ENUM('Unpaid', 'Paid') DEFAULT 'Unpaid',
    FOREIGN KEY (loan_id) REFERENCES Loans(loan_id)
);


INSERT INTO Genres (genre_name) VALUES
('Fiction'), ('Historical Fiction'), ('Poetry'), ('Non-Fiction'), ('Autobiography');
SELECT * FROM Genres;

INSERT INTO Publishers (publisher_name, location) VALUES
('Penguin India', 'Gurgaon'), ('Rupa Publications', 'New Delhi'), ('Westland Books', 'Chennai');
SELECT * FROM Publishers;

INSERT INTO Authors (author_name) VALUES
('Rabindranath Tagore'), ('Amrita Pritam'), ('Aravind Adiga'), ('Jhumpa Lahiri'), ('Chetan Bhagat');
SELECT * FROM Authors;


INSERT INTO Books (title, isbn, publication_year, genre_id, publisher_id, total_copies, available_copies) VALUES
('Gitanjali', '9789380914965', 1910, 3, 1, 5, 3),
('Pinjar', '9788121606231', 1950, 2, 2, 4, 4),
('The White Tiger', '9788172237435', 2008, 1, 1, 6, 5),
('The Namesake', '9780618485222', 2003, 1, 3, 3, 2),
('Five Point Someone', '9788129104593', 2004, 1, 2, 7, 7);
SELECT * FROM Books;

INSERT INTO BookAuthors (book_id, author_id) VALUES
(1, 1), (2, 2), (3, 3), (4, 4), (5, 5);
SELECT * FROM BookAuthors;

INSERT INTO Members (first_name, last_name, email, phone_number, registration_date, address) VALUES
('Isha', 'Kapoor', 'isha.k@example.com', '9876501234', '2024-02-20', 'Sector 17, Chandigarh'),
('Rajesh', 'Menon', 'raj.menon@example.com', '9988770011', '2024-06-05', 'MG Road, Kochi'),
('Sunita', 'Joshi', 'sunita.j@example.com', '8877661122', '2025-03-12', 'C.G. Road, Ahmedabad');
SELECT * FROM Members;


INSERT INTO Loans (loan_id, book_id, member_id, loan_date, due_date, return_date, status) VALUES
(1, 3, 1, '2025-09-02', '2025-09-23', NULL, 'Active'),
(2, 1, 2, '2025-08-10', '2025-08-31', '2025-08-29', 'Returned'),
(3, 4, 3, '2025-07-15', '2025-08-05', NULL, 'Overdue'),
(4, 1, 1, '2025-05-01', '2025-05-22', '2025-06-01', 'Returned'),
(5, 2, 2, '2025-09-18', '2025-10-09', NULL, 'Active');
SELECT * FROM Loans;

INSERT INTO Fines (loan_id, amount, issue_date, paid_date, status) VALUES
(3, 140.00, '2025-08-06', NULL, 'Unpaid'),
(4, 90.00, '2025-05-23', '2025-06-01', 'Paid');
SELECT * FROM Fines;
