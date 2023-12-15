# Penetration Testing Plan
## Classic SQL Injection (Fail)

- User inputs $email and $password are directly concatenated into the SQL query without proper sanitization or validation. 
- An attacker can manipulate these inputs to inject malicious SQL code, potentially leading to unauthorized access or manipulation of the database.
### Vulnerable Code

```php
// Original vulnerable code
$email = $_POST['email'];
$password1 = $_POST['password'];
$sql = "SELECT * FROM credential WHERE email = '$email' AND password = '$password1'";
$result = mysqli_query($conn, $sql);

```

### SQL Injection

```plaintext
Email: ' OR '1'='1'; --
Password: ' OR '1'='1'; --
```

### Manipulated SQL Query

```sql
SELECT * FROM credential WHERE email = '' OR '1'='1'; --' AND password = '' OR '1'='1'; --
```

![[Pasted image 20231117182748.png]]

### Mitigation

```php
$email = $_POST['email'];
$password1 = $_POST['password'];

// Use prepared statements to prevent SQL injection
$sql = "SELECT * FROM credential WHERE email = ? AND password = ?";
$stmt = mysqli_prepare($conn, $sql);

// Bind parameters to the prepared statement
mysqli_stmt_bind_param($stmt, "ss", $email, $password1);

// Execute the prepared statement
mysqli_stmt_execute($stmt);

// Get result set
$result = mysqli_stmt_get_result($stmt);

```

## Reflected Cross-Site Scripting (XSS) (Fail)

### Vulnerable Code

```php
$sql = "INSERT INTO feedback (f_name, email, feedback) VALUES ('$name', '$email', '$feedback')";
```

### Implementation

```js
<script>alert("XSS Attack!");</script>
```

### Mitigation

```php
<textarea cols="15" rows="5" name="feedback" placeholder="Enter your feedback here ..."><?php echo htmlspecialchars($feedback, ENT_QUOTES, 'UTF-8'); ?></textarea>
```

```php
$sql = "INSERT INTO feedback (f_name, email, feedback) VALUES (?, ?, ?)";
$stmt = mysqli_prepare($conn, $sql);
mysqli_stmt_bind_param($stmt, "sss", $name, $email, $feedback);
mysqli_stmt_execute($stmt);
```