# Linux User Numbers (LUN)

A public, decentralized system for Linux users to register and verify their unique **Linux User Number (LUN)** using GitHub and IPFS.

Website: [https://lun.linux.org](https://lun.linux.org)

---

## 🔹 What is a LUN?

A **Linux User Number (LUN)** is a unique identifier for Linux users. This system allows users to **register a LUN** and store it **transparently on GitHub and IPFS**, ensuring **permanent, verifiable proof of registration**.

---

## 🔹 How It Works

1. **Register a LUN:**  
   - Use [lun.linux.org](https://lun.linux.org) to register your LUN automatically.  
   - Alternatively, manually submit a **Pull Request (PR)** (see below).  
   - GitHub Actions **assigns the next available LUN**.  
   - Your registration is **stored on IPFS** and added to `registry.json`.  

2. **Look Up a LUN:**  
   - Use [lun.linux.org](https://lun.linux.org) to look up any LUN.  
   - Or check the **GitHub registry** or **IPFS gateway**.  

3. **Verify Ownership:**  
   - Users can **cryptographically sign messages** to prove LUN ownership.  
   - Verification tools allow checking authenticity.  

---

## 🔹 How to Register a LUN Manually (PR Method)

If you prefer not to use the website, you can manually register your LUN by submitting a PR to this repository.

### **Step 1: Generate Your Public Key**
To associate a public key with your LUN, generate a keypair on your Linux system:

```sh
openssl genpkey -algorithm RSA -out lun_private.pem
openssl rsa -pubout -in lun_private.pem -out lun_public.pem
```

This creates:
- `lun_private.pem` (your private key, **keep this secret!**)
- `lun_public.pem` (your public key, which will be stored in your LUN entry)

### **Step 2: Create a Registration File**
1. **Go to the `pending_registrations/` folder in this repo.**  
2. **Click "Add file" → "Create new file"**  
3. **Name your file** following this format:  
   ```
   pending_registrations/lun_NEW.json
   ```
   *(The system will auto-assign your LUN, so use "lun_NEW.json" for now.)*  
4. **Fill in your registration details in JSON format:**
   ```json
   {
     "public_key": "PASTE_YOUR_PUBLIC_KEY_HERE",
     "timestamp": "YYYY-MM-DDTHH:MM:SSZ",
     "note": "Been using Linux since 1998!"
   }
   ```
5. **Click "Propose new file" → "Create pull request".**  
6. **Wait for GitHub Actions to process your request.**  
7. **Once merged, your LUN will be permanently recorded!**  

---

## 🔹 How to Look Up a LUN

- Use [lun.linux.org](https://lun.linux.org) to search for a LUN.  
- Or view the **full registry** in [registry.json](registry.json).  
- Each LUN entry contains:  
  - **IPFS Hash** (link to registration data)  
  - **Public Key** (for verification)  
  - **User Note** (optional)  

---

## 🔹 How to Verify Ownership

If you want to prove you own a LUN, you can sign a message using your private key:

```sh
echo "I own LUN #1234" | openssl dgst -sha256 -sign lun_private.pem -out signature.bin
```

To verify a signature:

```sh
echo "I own LUN #1234" | openssl dgst -sha256 -verify lun_public.pem -signature signature.bin
```

This allows others to **verify your ownership** of a registered LUN.

---

## 🔹 License

This project is licensed under the **MIT License**.
