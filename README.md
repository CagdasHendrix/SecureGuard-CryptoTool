# SecureGuard-CryptoTool: Authenticated File Encryption with AES-256-GCM

---

## 📌 Project Overview

SecureGuard-CryptoTool is a Python-based command-line cybersecurity project designed to demonstrate authenticated file encryption in a Linux lab environment. The tool uses the **AES-256-GCM** encryption mode to protect sensitive local files by providing both **confidentiality** and **integrity verification**.

Unlike basic encryption methods that only hide file contents, AES-GCM also generates an authentication tag. This tag allows the program to detect whether encrypted data has been modified or tampered with before decryption.

This project was developed as an educational cybersecurity lab focused on secure local file storage, encryption workflows, and command-line cryptography practices.

---

## 🛠️ Laboratory Environment & Tools

- **Platform:** Oracle VM VirtualBox
- **Operating System:** Kali Linux
- **Language:** Python 3.x
- **Core Library:** `cryptography`
- **Algorithm:** AES-256-GCM
- **Key Size:** 256-bit
- **Nonce Size:** 96-bit
- **Editor:** GNU Nano
- **Analysis Utility:** `xxd`


---

## 🔍 Phase 1: Environment Setup & Initialization

The project started with the creation of a dedicated project directory and an isolated Python virtual environment. Using a virtual environment helps manage dependencies safely and prevents conflicts with system-wide Python packages.

The `cryptography` library was installed inside the virtual environment to provide secure cryptographic primitives for AES-GCM encryption and decryption.

### Commands

```bash
mkdir SecureGuard_CryptoTool
cd SecureGuard_CryptoTool
python3 -m venv venv
source venv/bin/activate
pip install cryptography
```

<img width="1274" height="916" alt="Ekran görüntüsü 2026-05-10 220001" src="https://github.com/user-attachments/assets/f80583f1-1e70-4b6e-9bd2-336bafa557dd" />

### Commands

```bash
touch secure_guard.py
touch secret_document
echo "This document contains cybersecurity lab test data."
```

<img width="1277" height="920" alt="Ekran görüntüsü 2026-05-10 220928" src="https://github.com/user-attachments/assets/003d6b7d-10af-4522-9473-2e1e3422f605" />

## ⚙️ Phase 2: Implementation & AEAD Logic

The core implementation uses **Authenticated Encryption with Associated Data (AEAD)** through AES-GCM. AES-GCM is suitable for secure file encryption because it provides both encrypted output and authentication verification.

The script performs the following operations:

- Generates a 256-bit AES key
- Creates a random 96-bit nonce
- Encrypts plaintext data
- Produces ciphertext
- Decrypts ciphertext back into plaintext
- Verifies that the encrypted data has not been modified

During decryption, AES-GCM validates the authentication tag. If the encrypted data, nonce, or tag has been altered, the decryption process fails.

### Commands
```bash
nano secure_guard.py
```

```bash
import os
from cryptography.hazmat.primitives.ciphers.aead import AESGCM

def test_encryption():
    print("[*] SecureGuard Cryptography Test Starting...\n")
    
    key = AESGCM.generate_key(bit_length=256)
    aesgcm = AESGCM(key)
    print(f"[+] 256-bit AES Key Generated: {key.hex()}")

    secret_message = b"Atlas University - Secret Data"
    nonce = os.urandom(12)
    
    ciphertext = aesgcm.encrypt(nonce, secret_message, None)
    print(f"[+] Encrypted Data (Ciphertext): {ciphertext.hex()}")

    decrypted_msg = aesgcm.decrypt(nonce, ciphertext, None)
    print(f"[+] Decrypted Data: {decrypted_msg.decode('utf-8')}")

if __name__ == "__main__":
    test_encryption()
```

<img width="1272" height="921" alt="Ekran görüntüsü 2026-05-10 221434" src="https://github.com/user-attachments/assets/80954dad-af8f-4dd6-a68f-001abb718bf9" />


---

## 🧪 Phase 3: Functional Logic Verification

Before testing file-based encryption, the script was executed to verify the basic cryptographic workflow. This test confirmed that key generation, encryption, and decryption were functioning correctly.

The output shows that a 256-bit AES key was generated, the plaintext message was encrypted, and the original message was successfully recovered.

### Command

```bash
python3 secure_guard.py
```

<img width="1269" height="915" alt="Ekran görüntüsü 2026-05-10 221655" src="https://github.com/user-attachments/assets/684eb185-c086-49b3-8b08-c858c5e2ceae" />

---

## 💣 Phase 4: File Encryption & Ciphertext Analysis

A test file named `test_data.txt` was created to simulate sensitive data. The file was then encrypted using a password-based command-line workflow.

### Create a test file

```bash
echo "Atlas University - Top Secret Project Data" > test_data.txt
cat test_data.txt
```

### Encrypt the file

```bash
python3 secure_guard.py encrypt test_data.txt MyStrongPass123
```

After encryption, the tool generated a new encrypted file:

```txt
test_data.txt.enc
```

<img width="1272" height="920" alt="Ekran görüntüsü 2026-05-10 224255" src="https://github.com/user-attachments/assets/d89bcccc-c110-444a-9c93-6604a89da0ff" />

---

## 🔬 Ciphertext Hex Analysis

The encrypted file was inspected with the `xxd` utility. This step confirmed that the original plaintext was no longer readable and had been converted into encrypted binary data.

### Command

```bash
xxd test_data.txt.enc
```

The hex output demonstrates that the encrypted file does not expose the original message in plaintext form.

<img width="1272" height="918" alt="Ekran görüntüsü 2026-05-10 224814" src="https://github.com/user-attachments/assets/391b744d-5e12-41f5-9cbd-14fe418643a1" />

---

## 🚩 Phase 5: Decryption & Post-Operation Validation

The encrypted `.enc` file was decrypted using the same password. After successful decryption, the recovered file was generated successfully.

### Decrypt the encrypted file

```bash
python3 secure_guard.py decrypt test_data.txt.enc MyStrongPass123
```

Expected output:

```txt
[+] File decrypted successfully: test_data.txt_decrypted
```

<img width="1269" height="916" alt="Ekran görüntüsü 2026-05-10 225011" src="https://github.com/user-attachments/assets/d69b99a5-88b6-42df-80b8-ab368d780469" />

---

## ✅ Final Validation

The final validation step confirmed that the decrypted file contained the original plaintext data.

### Command

```bash
cat test_data.txt_decrypted
```

Expected output:

```txt
Atlas University - Top Secret Project Data
```

<img width="1273" height="918" alt="Ekran görüntüsü 2026-05-10 225104" src="https://github.com/user-attachments/assets/cba76484-11f7-4d6d-bfb0-c8edfa413e87" />

---

## 🚀 How to Run

### 1. Clone the repository

```bash
git clone https://github.com/CagdasHendrix/SecureGuard-CryptoTool.git
cd SecureGuard-CryptoTool
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install cryptography
```

### 4. Run the basic cryptography test

```bash
python3 secure_guard.py
```

### 5. Encrypt a file

```bash
python3 secure_guard.py encrypt test_data.txt MyStrongPass123
```

### 6. Decrypt a file

```bash
python3 secure_guard.py decrypt test_data.txt.enc MyStrongPass123
```

---

## 🔐 Security Notes

- This project is intended for educational and lab use only.
- Do not use weak or reused passwords in real-world encryption scenarios.
- Do not hardcode passwords, keys, or sensitive secrets in source code.
- Encrypted files should be stored securely.
- The same password used for encryption is required for successful decryption.
- This tool should only be used on files that you own or have permission to protect.

---

## 💡 Conclusion

This lab successfully demonstrates the implementation of modern authenticated symmetric encryption using AES-256-GCM. The project shows how encryption can protect the confidentiality of sensitive files while authentication tags help detect unauthorized modifications.

By combining file encryption, decryption, password-based usage, and hexadecimal ciphertext inspection, SecureGuard-CryptoTool provides a practical introduction to secure local file protection in a Linux-based cybersecurity environment.

---

## 👤 Author

**Çağdaş Oktay**  
GitHub: https://github.com/CagdasHendrix  
Affiliation: İstanbul Atlas University - Computer Engineering
