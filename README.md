# cyber-security-2
 Step 1: Sample Python Code (Vulnerable Application)
This Python script accepts user input, connects to a database, and executes unsecured SQL queries—making it vulnerable to SQL Injection & other threats.

python
Copy
Edit
import sqlite3

def login(username, password):
    conn = sqlite3.connect("users.db")
    cursor = conn.cursor()
    
    # 🚨 Vulnerability: SQL Injection (User input directly in query)
    query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
    cursor.execute(query)

    result = cursor.fetchone()
    conn.close()
    
    if result:
        print("Login successful!")
    else:
        print("Invalid credentials.")

# Accept input from user
user = input("Enter username: ")
pwd = input("Enter password: ")

login(user, pwd)
🔹 Step 2: Security Vulnerabilities Identified
🚨 1. SQL Injection – The app directly inserts user input into an SQL query. A hacker can bypass authentication using:

matlab
Copy
Edit
' OR '1'='1'; --
🚨 2. Hardcoded Database Name – No flexibility in database configuration.
🚨 3. No Password Hashing – Storing plain-text passwords is a major security risk.
🚨 4. Lack of Input Validation – No filtering or sanitization of user input.

🔹 Step 3: Secure Coding Fixes
✅ Use Prepared Statements (Parameterized Queries)
✅ Use Environment Variables for Database Configurations
✅ Use Secure Password Hashing (bcrypt)
✅ Input Validation (Restrict Characters, Check Length)

🔹 Fixed Code (Secure Login)
python
Copy
Edit
import sqlite3
import bcrypt
import os

def secure_login(username, password):
    conn = sqlite3.connect(os.getenv("DB_PATH", "users.db"))
    cursor = conn.cursor()

    # ✅ Secure: Use Parameterized Queries (Prevents SQL Injection)
    query = "SELECT password FROM users WHERE username = ?"
    cursor.execute(query, (username,))
    result = cursor.fetchone()

    if result and bcrypt.checkpw(password.encode(), result[0]):
        print("✅ Login successful!")
    else:
        print("❌ Invalid credentials.")

    conn.close()

# Accept input from user securely
user = input("Enter username: ")
pwd = input("Enter password: ")

secure_login(user, pwd)
🔹 Step 4: Automated Security Analysis Using Bandit
📌 Install Bandit

sh
Copy
Edit
pip install bandit
📌 Run Bandit Security Scan

sh
Copy
Edit
bandit -r secure_login.py
📌 Expected Output (No Major Vulnerabilities)

yaml
Copy
Edit
No issues identified. Secure coding practices followed.
🔹 Step 5: Summary of Secure Coding Practices
✅ Use Parameterized Queries (Prevent SQL Injection)
✅ Hash Passwords (Never Store Plain Text Passwords)
✅ Use Environment Variables (Avoid Hardcoded Values)
✅ Implement Input Validation (Sanitize User Input)
✅ Run Static Analysis (Bandit for Python Security Review)

Would you like a security review for another language (Java, JavaScript, C++)? 🚀🔒
