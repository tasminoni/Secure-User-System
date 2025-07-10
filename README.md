# CSE447
 Cryptography Project
# Secure User System

## Project Overview


### Key Features
- Secure user registration and authentication
- Password hashing with salt
- Data encryption for sensitive information
- Message Authentication Code (MAC) for data integrity
- Session security management
- Secure key management

## Security Features

### 2.1 Encryption System
- **Algorithm**: AES-256-CBC
- **Initialization Vector (IV)**: Unique per encryption
- **Key Management**: Individual encryption keys per user
- **Data Coverage**: Emails, post content, and sensitive user data

### Password Security
- **Bcrypt hashing algorithm**
- **Unique salt per user**
- **Secure password verification system**

### Data Integrity
- **HMAC-SHA256** for MAC generation
- **Integrity verification** before data retrieval
- **Tamper detection** for stored data

### Session Security
- **Secure session configuration**
- **HTTP-only cookies**
- **Secure headers implementation**

## Usage Guide

### 1 User Registration
1. Access the registration page.
2. Provide username, email, and password.
3. System encrypts email and hashes the password.
4. Creates a user-specific encryption key.

### 2 User Login
1. Enter credentials.
2. System verifies password hash.
3. Establishes a secure session.

### 3 Creating Posts
1. Enter post content.
2. System encrypts content.
3. Generates a MAC for integrity.
4. Stores encrypted data and MAC.

---

**Note**: Always ensure your configuration files (e.g., `config.php`) are secure and inaccessible from external sources.
