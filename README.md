# Password Cracking Lab Exercise

**Author**: seTajul

**Environment**: WSL Kali Linux  

## Objective
This lab will teach you:
- How password hashing works.
- How to use password-cracking tools like `John the Ripper` and `Hashcat`.
- The importance of using strong passwords and secure hashing algorithms.

## Prerequisites
- **WSL (Windows Subsystem for Linux)** with Kali Linux installed.
- **Rockyou wordlist** (to be installed during the lab).
- Tools: `John the Ripper`, `Hashcat`, `gzip`.

## Lab Setup

### Step 1: Create a Sample Password File
1. Open your WSL terminal and create a file named `passwords.txt`:
   ```bash
   nano passwords.txt
   ```
2. Add the following sample passwords:
   ```
   password123
   qwerty
   letmein
   admin
   123456
   ```
3. Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

### Step 2: Hash the Passwords
1. Generate MD5 hashes for the passwords:
   ```bash
   while read -r password; do
       echo -n "$password" | md5sum | awk '{print $1}' >> hashed_passwords.txt
   done < passwords.txt
   ```
2. Verify the hashes:
   ```bash
   cat hashed_passwords.txt
   ```

## Lab Tasks

### Task 1: Crack Passwords Using John the Ripper
1. Install John the Ripper:
   ```bash
   sudo apt update && sudo apt install john -y
   ```
2. Prepare the hash file:
   ```bash
   awk '{print "user" NR ":" $0}' hashed_passwords.txt > john_hashes.txt
   ```
3. Extract the Rockyou wordlist:
   ```bash
   sudo apt install wordlists -y
   gzip -d /usr/share/wordlists/rockyou.txt.gz
   ```
4. Run John the Ripper:
   ```bash
   john --wordlist=/usr/share/wordlists/rockyou.txt john_hashes.txt
   ```
5. View the results:
   ```bash
   john --show john_hashes.txt
   ```

### Task 2: Crack Passwords Using Hashcat
1. Install Hashcat:
   ```bash
   sudo apt install hashcat -y
   ```
2. Prepare the hash file:
   ```bash
   cp hashed_passwords.txt hashcat_hashes.txt
   ```
3. Run Hashcat:
   ```bash
   hashcat -m 0 -a 0 hashcat_hashes.txt /usr/share/wordlists/rockyou.txt
   ```
   - **`-m 0`**: Specifies MD5 hash mode.
   - **`-a 0`**: Specifies dictionary attack mode.
4. View the results:
   ```bash
   hashcat --show hashcat_hashes.txt
   ```

## Post-Lab Questions
1. Which passwords were cracked successfully?
2. How long did it take to crack the passwords using John and Hashcat?
3. What are the implications of using weak passwords?
4. How can password security be improved in real-world applications?

## Notes
- **Ethical Usage**: This lab is for educational purposes only. Do not attempt to crack passwords without explicit permission.
- **Performance in WSL**: Hashcat may run slower in WSL due to hardware access limitations. For better performance, use a dedicated Linux machine.

## Troubleshooting
- If the Rockyou wordlist is missing, install it using:
  ```bash
  sudo apt install wordlists -y
  ```
- If you encounter errors with Hashcat or John, ensure the tools are installed correctly:
  ```bash
  sudo apt install john hashcat -y
  ```
