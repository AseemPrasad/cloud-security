# Cloud Security and Implementation

## Overview
This project demonstrates implementation of *IAM policies, **secure storage, and **data encryption* on a cloud platform (Backblaze B2).

---

## 1. IAM Policies

*Goal:* Control who can access cloud resources.

*Implementation Steps:*
1. Created an *Application Key* named read-only-access
2. Granted permission for a specific bucket only
3. Restricted access to *read-only* to prevent unauthorized upload/delete actions

*Result:*  
Only users with this specific key can read files from the bucket.

---

## 2. Secure Storage

- All buckets are configured as *Private*
- Files cannot be accessed without valid credentials (Key ID + Application Key)
- Access URLs are temporary and require authorization

*Example:*  
b2_authorize_account and b2_download_file_by_id require valid credentials, ensuring secure access.

---

## 3. Data Encryption

### a. At Rest
- Backblaze B2 automatically encrypts all files at rest using AES-256.

### b. Manual Client-Side Encryption (Demo)
Before uploading, files can be encrypted locally using Python:

```python
from cryptography.fernet import Fernet

# Generate encryption key
key = Fernet.generate_key()
with open("encryption_key.key", "wb") as key_file:
    key_file.write(key)

# Encrypt data
cipher = Fernet(key)
with open("example.txt", "rb") as f:
    data = f.read()
encrypted = cipher.encrypt(data)

with open("example_encrypted.txt", "wb") as f:
    f.write(encrypted)

