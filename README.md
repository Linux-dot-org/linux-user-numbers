# Linux User Numbers (LUN)

# WORK IN PROGRESS - this is not live yet.

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

If someone claims to own a LUN, you can verify their ownership using the official registry.

### Step 1: The LUN Owner Signs a Message
The LUN owner generates a signed proof using their private key:

    echo "I confirm ownership of LUN #1234 on lun.linux.org" | openssl dgst -sha256 -sign lun_private.pem | base64

This outputs a **Base64-encoded signature**, which the owner should share publicly.

### Step 2: The Verifier Fetches the Latest Registry
The verifier must retrieve the latest LUN registry from GitHub:

    curl -s -o registry.json https://raw.githubusercontent.com/Linux-dot-org/linux-user-numbers/main/registry.json

If GitHub is unavailable, they should use the latest IPFS-pinned version.

### Step 3: Extract the Public Key
The verifier extracts the registered public key for LUN #1234:

    jq -r ".registrations[\"1234\"].public_key" registry.json

This ensures they are checking against the correct owner’s public key.

### Step 4: Verify the Signature
The verifier checks the signature against the registered public key:

    echo "I confirm ownership of LUN #1234 on lun.linux.org" | openssl dgst -sha256 -verify <(jq -r ".registrations[\"1234\"].public_key" registry.json) -signature <(echo -n "PASTE_BASE64_SIGNATURE_HERE" | base64 -d)

If the output is `Verified OK`, the person **truly owns the LUN**. If verification fails, the signature is either invalid or the claim is false.


---

## 🔹 License

This project is licensed under the **MIT License**.
