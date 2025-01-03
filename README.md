# CSE447
 Cryptography Project
# Secure User System

## Table of Contents
1. [Project Overview](#project-overview)
2. [Security Features](#security-features)
3. [System Architecture](#system-architecture)
4. [Implementation Details](#implementation-details)
5. [Usage Guide](#usage-guide)

## 1. Project Overview

### 1.1 Introduction
The Secure User System is a web application implementing secure user authentication and data storage with encryption. The system allows users to register, log in, and create posts while ensuring data confidentiality and integrity.

### 1.2 Key Features
- Secure user registration and authentication
- Password hashing with salt
- Data encryption for sensitive information
- Message Authentication Code (MAC) for data integrity
- Session security management
- Secure key management

## 2. Security Features

### 2.1 Encryption System
- **Algorithm**: AES-256-CBC
- **Initialization Vector (IV)**: Unique per encryption
- **Key Management**: Individual encryption keys per user
- **Data Coverage**: Emails, post content, and sensitive user data

### 2.2 Password Security
- **Bcrypt hashing algorithm**
- **Unique salt per user**
- **Secure password verification system**

### 2.3 Data Integrity
- **HMAC-SHA256** for MAC generation
- **Integrity verification** before data retrieval
- **Tamper detection** for stored data

### 2.4 Session Security
- **Secure session configuration**
- **HTTP-only cookies**
- **Secure headers implementation**

## 3. System Architecture

### 3.1 Component Structure
```
secure-user-system/
├── includes/
│   ├── config.php      # Configuration and security settings
│   ├── security.php    # Security
│   └── functions.php   # Core security functions
├── css/
│   └── style.css       # Styling
├── index.php           # Login page
├── register.php        # Registration page
├── process_login.php   # Login handler
├── process_register.php # Registration handler
├── dashboard.php       # User dashboard
├── see_posts.php       # See my posts
└── allposts.php        # To see all posts
```

### 3.2 Security Classes
```php
private static $cipher = "AES-256-CBC";

public static function generateSalt($length = 16) {
    return bin2hex(random_bytes($length));
}

public static function hashPassword($password, $salt) {
    return password_hash($password . $salt, PASSWORD_BCRYPT);
}

public static function verifyPassword($password, $hash, $salt) {
    return password_verify($password . $salt, $hash);
}

public static function encrypt($data, $key, $iv = null) {
    if ($iv === null) {
        $iv = openssl_random_pseudo_bytes(openssl_cipher_iv_length(self::$cipher));
    }
    $encrypted = openssl_encrypt($data, self::$cipher, $key, 0, $iv);
    return ['data' => $encrypted, 'iv' => bin2hex($iv)];
}

public static function decrypt($encryptedData, $key, $iv) {
    return openssl_decrypt($encryptedData, self::$cipher, $key, 0, hex2bin($iv));
}

public static function generateMAC($data, $key) {
    return hash_hmac('sha256', $data, $key);
}

public static function verifyMAC($encryptedContent, $key, $mac) {
    $calculatedMac = self::generateMAC($encryptedContent, $key);
    return hash_equals($calculatedMac, $mac);
}
```

## 4. Implementation Details

### 4.1 Data Encryption Process
```php
// Example of email encryption during registration
$encryptedEmail = Security::encrypt($email, ENCRYPTION_KEY);
// Returns: ['data' => encrypted_string, 'iv' => initialization_vector]
```

### 4.2 Password Handling
```php
// Password hashing
$salt = Security::generateSalt();
$passwordHash = Security::hashPassword($password, $salt);

// Password verification
$isValid = Security::verifyPassword($password, $hash, $salt);
```

### 4.3 MAC Implementation
```php
// Generate MAC for post content
$mac = Security::generateMAC($encrypted['data'], $userKey);

// Verify MAC before decryption
if (!verifyPostIntegrity($postId)) {
    throw new Exception("Data integrity compromised");
}
```

## 5. Usage Guide

### 5.1 User Registration
1. Access the registration page.
2. Provide username, email, and password.
3. System encrypts email and hashes the password.
4. Creates a user-specific encryption key.

### 5.2 User Login
1. Enter credentials.
2. System verifies password hash.
3. Establishes a secure session.

### 5.3 Creating Posts
1. Enter post content.
2. System encrypts content.
3. Generates a MAC for integrity.
4. Stores encrypted data and MAC.

---

**Note**: Always ensure your configuration files (e.g., `config.php`) are secure and inaccessible from external sources.
