# Database Management System (DBMS)

## What is a DBMS?

A **Database Management System (DBMS)** is a structured way of storing, managing, and retrieving data so that it is organized, consistent, and easily accessible.  
Unlike random files on your laptop, a database system ensures **security**, **accuracy**, and **scalability** for large amounts of data.

---

## Data vs Information vs Knowledge

| Concept | Description | Example |
|----------|--------------|----------|
| **Data** | Raw facts, unprocessed | `101, 102, 103` (student roll numbers) |
| **Information** | Processed data that is meaningful | `"Roll number 101 belongs to Jenil, enrolled in DBMS."` |
| **Knowledge** | Applying information to make decisions | `"Jenil has failed DBMS twice → suggest extra remedial classes."` |

---

## ⚙️ Functions of DBMS

- Store large amounts of data securely  
- Allow multiple users to access data simultaneously  
- Provide backup & recovery features  
- Enforce security rules (who can see what)  
- Ensure consistency of data  

---

## 🧾 Characteristics of Databases

Compared to normal file storage, databases provide:

- **Centralized Control** – Data stored in one place (shared by all departments)  
- **Reduced Redundancy** – Avoid storing the same student’s name in multiple files  
- **Consistency** – One student’s updated email reflects everywhere  
- **Security** – Students can’t see other students’ marks, but faculty can  
- **Concurrency** – Multiple students registering for courses at once is handled smoothly  
- **Backup and Recovery** – Data is not lost even if the system crashes  

---

## 6. File System vs Database System

| Aspect              | File System                               | Database System (DBMS)             |
| ------------------- | ----------------------------------------- | ---------------------------------- |
| **Data Storage**    | Stored in separate files                  | Stored in structured tables        |
| **Redundancy**      | High (same data may appear in many files) | Low (data stored once, reused)     |
| **Consistency**     | Hard to maintain                          | Ensured by DBMS rules              |
| **Security**        | Weak (anyone can edit files)              | Strong (access rights, encryption) |
| **Backup/Recovery** | Manual backup needed                      | Built-in recovery mechanisms       |
| **Scalability**     | Not suitable for large data               | Scales to millions of records      |
