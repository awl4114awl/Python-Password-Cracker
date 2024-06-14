Creating a license for your GitHub repository is straightforward. Here’s how you can add an MIT License to your project:

### Adding a License File

1. **Create a new file in your repository named `LICENSE`.**

2. **Copy the MIT License text into this file.**

### MIT License Text

```text
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

3. **Commit the new `LICENSE` file to your repository.**

### Adding License Information to the README

Ensure your README file references the license. You can add a section like this:

```markdown
## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
```

### Example README with License Section

Here’s how the relevant section of your README would look:

```markdown
## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
```

### Full Updated README

Here's the full updated README with the license information included:

```markdown
# Python Password Cracker

A collection of Python scripts for performing dictionary attacks to crack hashed passwords. Includes a basic version using SHA-256 and an advanced version with multi-algorithm support and multi-threading for improved performance. Ideal for learning and exploring password cracking concepts in cybersecurity.

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Basic Script](#basic-script)
  - [Overview](#overview)
  - [Step-by-Step Guide](#step-by-step-guide)
  - [Full Code](#full-code)
  - [Running the Script](#running-the-script)
- [Advanced Script](#advanced-script)
  - [Enhancing the Basic Script](#enhancing-the-basic-script)
  - [Full Code](#full-code-1)
  - [Running the Script](#running-the-script-1)
- [Obtaining a Dictionary File](#obtaining-a-dictionary-file)
  - [Example: Using the RockYou Wordlist](#example-using-the-rockyou-wordlist)
- [Conclusion](#conclusion)
- [Contributions](#contributions)
- [License](#license)
- [Contact](#contact)

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
    git clone https://github.com/awl4114awl/Python-Password-Cracker.git
    cd Python-Password-Cracker
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

### Running the Script

Run the basic script from the command line:

```bash
python basic_password_cracker.py
```

## Advanced Script

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

### Running the Script

Run the advanced script from the command line:

```bash
python advanced_password_cracker.py
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

## Conclusion

The provided scripts serve as a good starting point for understanding password cracking concepts. You can enhance them with advanced features to make them more robust and efficient, depending on your learning goals and requirements.

## Contributions

Contributions are welcome! Feel free to open an issue or submit a pull request with any improvements or bug fixes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

For any inquiries or feedback, please contact [jordanryancalvert@gmail.com](mailto:jordanryancalvert@gmail.com).
```

Feel free to adjust any part of this README to better fit your preferences or project specifics.
