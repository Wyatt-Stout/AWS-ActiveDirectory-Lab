# Server Provisioning & Build Log

## 1. Cryptographic Key Management
* **Key Pair Name:** ActiveDirectory-Lab-Key
* **Encryption Type:** RSA (.pem format)
* **Security Control:** Generated a local private key pair to securely decrypt the Windows Server built-in Administrator password. The private key is stored locally and excluded from public source control repository uploads for security compliance.
