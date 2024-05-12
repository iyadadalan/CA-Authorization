# CA-Authorization

  #### Assignment Overview
This document outlines the updates made to the PHP application to enhance security through authentication and authorization measures. It covers changes across several files to implement role-based access control and session management.

#### File Modifications

---

### 1. `login_stud.php`
- **Purpose**: Authenticates users and initiates a session.
- **Key Changes**: Added session handling and redirection based on user roles.
- **Code Example**:
  ```php
  if (hash('sha512', $password) == $user['password']) {
      $_SESSION['user_id'] = $user['id'];
      $_SESSION['email'] = $email;
      $_SESSION['role'] = $user['role'];
      header("Location: stud_details.php");
      exit;
  }
  ```

---

### 2. `signup_stud.php`
- **Purpose**: Registers new users into the system.
- **Key Changes**: Implemented SHA-512 hashing for password security.
- **Code Example**:
  ```php
  $passwordHashed = hash('sha512', $password);
  $stmt = $conn->prepare("INSERT INTO users (email, password, name, role) VALUES (?, ?, ?, ?)");
  ```

---

### 3. `stud_details.php`
- **Purpose**: Displays a form for entering or editing student details.
- **Key Changes**: Session check for role-based access.
- **Code Example**:
  ```php
  if ($_SESSION['role'] == 'guest') {
      echo "Access Denied. You do not have permission to view this page.";
      exit;
  }
  ```

---

### 4. `view_stud_details.php`
- **Purpose**: Lists all student records with options to edit or delete.
- **Key Changes**: Role-based display of records and action buttons.
- **Code Example**:
  ```php
  if ($_SESSION['role'] == 'admin') {
      $query = "SELECT * FROM students";
  } else {
      $query = "SELECT * FROM students WHERE user_id = ".$_SESSION['user_id'];
  }
  ```

---

### 5. `edit_student.php`
- **Purpose**: Allows editing of student details.
- **Key Changes**: Enhanced access control to verify user role and ownership of data.
- **Code Example**:
  ```php
  if ($_SESSION['role'] != 'admin' && $student['user_id'] != $_SESSION['user_id']) {
      echo "You do not have permission to edit this record.";
      exit;
  }
  ```
