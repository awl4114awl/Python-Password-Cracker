# Python Password Cracker

This repository contains Python scripts for performing dictionary attacks to crack hashed passwords. The scripts range from a basic implementation to a more advanced version with additional features like multi-algorithm support and multi-threading.

## Features

- **Basic Script**:
  - Uses SHA-256 hashing.
  - Performs a dictionary attack using a predefined list of passwords.
  - Simple and straightforward logic.

- **Advanced Script**:
  - Supports multiple hashing algorithms (MD5, SHA-1, SHA-256).
  - Handles salted hashes.
  - Implements multi-threading for improved performance.

## Prerequisites

- Python 3.x
- Basic understanding of hashing and Python programming.

## Getting Started

1. **Clone the repository**:
    ```bash
    git clone https://github.com/yourusername/python-password-cracker.git
    cd python-password-cracker
    ```

2. **Create a dictionary file**:
    - Create a file named `dictionary.txt` and fill it with common passwords, one per line. For example:
      ```
      123456
      password
      123456789
      qwerty
      abc123
      password1
      ```

## Basic Script

### Overview

The basic script performs a dictionary attack using SHA-256 hashing.

### Step-by-Step Guide

1. **Import Required Libraries**:
    - We'll use `hashlib` for hashing passwords.

2. **Load the Dictionary File**:
    - Use a list of common passwords or a dictionary file containing potential passwords.

3. **Hash the Passwords**:
    - Hash each password in the dictionary and compare it with the target hash.

4. **Compare Hashes**:
    - If a hash matches, we've cracked the password.

### Full Code

Here's the full code for a dictionary attack password cracker:

```python
import hashlib

def hash_password(password):
    return hashlib.sha256(password.encode()).hexdigest()

def load_dictionary(dict_file):
    with open(dict_file, 'r') as f:
        return [line.strip() for line in f]

def crack_password(hash_to_crack, dict_file):
    passwords = load_dictionary(dict_file)
    for password in passwords:
        if hash_password(password) == hash_to_crack:
            print(f"Password found: {password}")
            return password
    print("Password not found.")
    return None

if __name__ == "__main__":
    # Replace 'hashed_password' with the actual hash you want to crack
    hashed_password = '5e884898da28047151d0e56f8dc6292773603d0d6aabbddce37d8d5a8df15c1e'  # This is the hash for 'password'
    
    # Replace 'dictionary.txt' with the path to your dictionary file
    dictionary_file = 'dictionary.txt'

    crack_password(hashed_password, dictionary_file)
```

### Explanation

1. **Hash Function**:
    - `hash_password(password)`: Takes a password string and returns its SHA-256 hash.

2. **Load Dictionary**:
    - `load_dictionary(dict_file)`: Reads the dictionary file and returns a list of potential passwords.

3. **Crack Password**:
    - `crack_password(hash_to_crack, dict_file)`: Iterates through the list of potential passwords, hashes each one, and compares it to the target hash. If a match is found, it prints the password.

4. **Main Block**:
    - Replace `hashed_password` with the actual hash you want to crack.
    - Replace `dictionary_file` with the path to your dictionary file.

### Running the Script

Run the basic script from the command line:

```bash
python password_cracker.py
```

This will attempt to crack the specified hash using the passwords in `dictionary.txt`. Adjust the `hashed_password` and `dictionary_file` variables as needed.

## Advanced Script

### Overview

The advanced script adds multi-algorithm support, salted hashes, and multi-threading for improved performance.

### Enhancing the Basic Script

To make the script more advanced, you can incorporate the following features:

1. **Multi-Algorithm Support**:
    ```python
    def hash_password(password, algorithm='sha256'):
        if algorithm == 'md5':
            return hashlib.md5(password.encode()).hexdigest()
        elif algorithm == 'sha1':
            return hashlib.sha1(password.encode()).hexdigest()
        elif algorithm == 'sha256':
            return hashlib.sha256(password.encode()).hexdigest()
        else:
            raise ValueError('Unsupported hashing algorithm')
    ```

2. **Adding Salt**:
    ```python
    def hash_password(password, salt='', algorithm='sha256'):
        return hash_password(password + salt, algorithm)
    ```

3. **Multi-threading**:
    ```python
    import threading

    def crack_password_mt(hash_to_crack, dict_file, algorithm='sha256'):
        passwords = load_dictionary(dict_file)
        found = [False]
        
        def worker(password):
            if found[0]:
                return
            if hash_password(password, algorithm) == hash_to_crack:
                with threading.Lock():
                    if not found[0]:
                        found[0] = True
                        print(f"Password found: {password}")
        
        threads = []
        for password in passwords:
            t = threading.Thread(target=worker, args=(password,))
            threads.append(t)
            t.start()
        
        for t in threads:
            t.join()
        
        if not found[0]:
            print("Password not found.")
    
    # Example usage
    hashed_password = '5e884898da28047151d0e56f8dc6292773603d0d6aabbddce37d8d5a8df15c1e'  # 'password'
    dictionary_file = 'dictionary.txt'
    crack_password_mt(hashed_password, dictionary_file)
    ```

### Full Code

Here's the full code for the advanced password cracker:

```python
import hashlib
import threading

def hash_password(password, algorithm='sha256'):
    if algorithm == 'md5':
        return hashlib.md5(password.encode()).hexdigest()
    elif algorithm == 'sha1':
        return hashlib.sha1(password.encode()).hexdigest()
    elif algorithm == 'sha256':
        return hashlib.sha256(password.encode()).hexdigest()
    else:
        raise ValueError('Unsupported hashing algorithm')

def load_dictionary(dict_file):
    with open(dict_file, 'r') as f:
        return [line.strip() for line in f]

def crack_password_mt(hash_to_crack, dict_file, algorithm='sha256'):
    passwords = load_dictionary(dict_file)
    found = [False]
    
    def worker(password):
        if found[0]:
            return
        if hash_password(password, algorithm) == hash_to_crack:
            with threading.Lock():
                if not found[0]:
                    found[0] = True
                    print(f"Password found: {password}")
    
    threads = []
    for password in passwords:
        t = threading.Thread(target=worker, args=(password,))
        threads.append(t)
        t.start()
    
    for t in threads:
        t.join()
    
    if not found[0]:
        print("Password not found.")
    
if __name__ == "__main__":
    # Replace 'hashed_password' with the actual hash you want to crack
    hashed_password = '5e884898da28047151d0e56f8dc6292773603d0d6aabbddce37d8d5a8df15c1e'  # This is the hash for 'password'
    
    # Replace 'dictionary.txt' with the path to your dictionary file
    dictionary_file = 'dictionary.txt'

    crack_password_mt(hashed_password, dictionary_file)
```
## Obtaining a Dictionary File

There are several sources where you can obtain dictionary files (wordlists) for password cracking. Here are a few popular options:

1. **SecLists**:
    - SecLists is a collection of multiple types of lists used during security assessments. You can find wordlists specifically for password cracking here.
    - GitHub Repository: [SecLists](https://github.com/danielmiessler/SecLists)

2. **RockYou**:
    - The RockYou wordlist is one of the most well-known password lists used in security research.
    - You can find the RockYou wordlist included with many security tools like Kali Linux.
    - Direct download link: [RockYou.txt](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)

3. **Weakpass**:
    - Weakpass is a project that provides a variety of wordlists for different purposes.
    - Website: [Weakpass](https://weakpass.com/)

4. **Kali Linux**:
    - Kali Linux comes pre-installed with several wordlists, including the RockYou wordlist.
    - After installing Kali Linux, you can find the wordlists in `/usr/share/wordlists`.

5. **Common User Passwords Profiler (CUPP)**:
    - CUPP is a tool designed to generate custom wordlists based on user-specific information.
    - GitHub Repository: [CUPP](https://github.com/Mebus/cupp)

6. **Online Collections**:
    - You can also search for other wordlists available online. A simple Google search for "password wordlist" or "dictionary file for password cracking" can yield many results.

### Example: Using the RockYou Wordlist

To use the RockYou wordlist in your project:

1. **Download the RockYou wordlist**:
    ```bash
    wget https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
    ```

2. **Place the wordlist in your project directory**.

3. **Modify your script to use the RockYou wordlist**:
    ```python
    dictionary_file = 'rockyou.txt'
    ```

Make sure to comply with any licensing or usage terms associated with the wordlists you use.

### Customization

- **Target Hash Algorithm**:
    ```python
    hash_to_crack = '5d41402abc4b2a76b9719d911017c592'  # Example MD5 hash for 'hello'
    algorithm = 'md5'
    ```

- **Dictionary File**:
    ```python
    dictionary_file = 'path/to/your/dictionary.txt'
    ```

### Running the Script

Run the advanced script from the command line:

```bash
python advanced_password_cracker.py
```

## Conclusion

The provided script is basic and serves as a good starting point for understanding password cracking concepts. You can enhance it with advanced features to make it more robust and efficient, depending on your learning goals and requirements.

## Contributions

Contributions are welcome! Feel free to open an issue or submit a pull request with any improvements or bug fixes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

For any inquiries or feedback, please contact [jordanryancalvert@gmail.com](mailto:jordanryancalvert@gmail.com).
