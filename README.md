# 📚 RFID-Based Library Management System

An automated Library Management System built with Spring Boot that integrates RFID technology for seamless book borrowing and returning processes, automated fine calculations, email reminders, and an admin analytics dashboard.

---

## 🚀 Key Features & Core Workflow

### 1. Initial Setup & User Registration Process
* **Admin Panel & Database:** The administrator logs into the system to register new users (Students/Members) and add books.
* **RFID Mapping:** During registration, member IDs and book details are mapped and saved alongside their respective **RFID Stickers/Tags**.
* **Book Status:** Newly added books are initialized with an `AVAILABLE` status.

### 2. RFID Scanning & Borrow / Return Process (Core Workflow)
At the library counter, the RFID reader handles the main library operations:
* **Member & Book Identification:** Members scan either their RFID card or the book's RFID sticker using the reader.
* **Validation & Blacklist Check:** The backend (`LibraryService`) verifies the scanned tag and checks if the user is blacklisted (e.g., due to excessive unpaid fines). If blacklisted, the process is rejected.

#### 📦 Branch 1: Borrow Process
* If the book status is **`AVAILABLE`**:
  * Status changes to **`BORROWED`**.
  * `borrowCount` increments by 1 (to track popular books).
  * A new transaction record is created with a **Due Date set to 14 days** from the current date.
  * **Response:** `"Successfully Borrowed"`

#### 🔄 Branch 2: Return & Fine Process
* If the book status is **`BORROWED`**:
  * The active transaction record is located, and the Return Date is updated to the current timestamp.
  * **Fine Calculation:** If returned past the due date, a fine is calculated.
  * **Blacklist Automation:** If the calculated fine exceeds **LKR 500**, the user is automatically flagged as `isBlacklisted = true`.
  * Book status updates back to **`AVAILABLE`**.
  * **Response:** `"Successfully Returned. Fine Amount: LKR [Amount]"`

---

### 3. Automated Email Reminder Process (Background Task)
* **Scheduled Task:** Utilizes Spring Boot’s `@Scheduled` annotation to execute a daily background job.
* **Due Date Verification:** Queries the database for users whose books are due today, due tomorrow, or overdue.
* **Email Notifications:** Automatically sends reminder/warning emails via `JavaMailSender` 

---

### 4. Admin Dashboard & Analytics
* **Popular Books Tracking:** Admins can view the top 5 most popular books.
* **Reports & Monitoring:** Real-time monitoring of total active borrowings, collected fines, and blacklisted users via the Thymeleaf UI.

---

## 🛠️ Tech Stack
* **Backend:** Java, Spring Boot, Spring Data JPA, Spring Security
* **Scheduling & Email:** Spring Task Scheduling, JavaMailSender
* **Frontend:** Thymeleaf, HTML5, CSS3, JavaScript
* **Database:** MySQL / PostgreSQL
* **Hardware Integration:** RFID Reader / Scanners

---
