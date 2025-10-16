# CMU-Africa-picoMini-Writeup
<p align="center">
  <img src="https://github.com/Greenest-Guy/CMU-Africa-picoMini-Writeup/blob/main/images/picoCTF_logo.png?raw=true" alt="picoCTF logo" width="400" />
</p>

## Table of Contents
<details open>
  <summary><b>🔐 Cryptography</b></summary>

  - [Crack the Power](#crack-the-power)
</details>

<details open>
  <summary><b>🧠 General Skills</b></summary>

  - [Log Hunt](#log-hunt)
</details>

<details open>
  <summary><b>🔍 Forensics</b></summary>

  - [Riddle Registry](#riddle-registry)
  - [Flag in Flame](#flag-in-flame)
  - [Hidden in Plainsight](#hidden-in-plainsight)
  - [Corrupted File](#corrupted-file)
</details>

<details open>
  <summary><b>🌐 Web Exploitation</b></summary>

  - [Crack the Gate 1](#crack-the-gate-1)
  - [Crack the Gate 2](#crack-the-gate-2)
</details>

<details open>
  <summary><b>⚙️ Binary Exploitation</b></summary>

  - [Input Injection 1](#input-injection-1)
</details>

## Crack the Power
**Category:** Cryptography

**Files/Links Provided:** ```message.txt```


**Description:**  

```
We received an encrypted message. The modulus is built from primes large enough that factoring them isn’t an option, at least not today. See if you can make sense of the numbers and reveal the flag. Download the message.
```

**Steps to Solve:**  
1. Download ```message.txt```
2. Extract n, c, and e for analysis.
3. Check if the ciphertext corresponds to an exact integer e-th power. If the plaintext $$m$$ satisfies $$m^e < n$$, then $$m^e = c$$. Now you can recover $$m$$ by taking the e-th root of the ciphertext:
   
$$
m = \sqrt[e]{c}
$$

4. Use a python script to calculate $$m = \sqrt[e]{c}$$ and convert the bytes to plaintext.


**Code / Commands / Images**
```bash
import gmpy2

# modulus & ciphertext redacted
n = (modulus)
c = (ciphertext)
e = 20

m, exact = gmpy2.iroot(c, e)

if exact:
    plaintext_bytes = m.to_bytes((m.bit_length() + 7) // 8, byteorder='big')
    plaintext = plaintext_bytes.decode('utf-8')

    print(plaintext)
```
[🔼 Back to Top](#table-of-contents)

## Log Hunt
**Category:** General Skills

**Files/Links Provided:** ```server.log```


**Description:**  

```
Our server seems to be leaking pieces of a secret flag in its logs.
The parts are scattered and sometimes repeated. Can you reconstruct the original flag?
Download the logs and figure out the full flag from the fragments.
```

**Steps to Solve:**  
1. Download ```server.log```.
2. Open a terminal within the directory of the log files location.
3. Use grep to find the flag parts.
4. Reconstruct the flag using the flag parts.


**Code / Commands / Images**
```bash
grep "picoCTF" server.log -i
grep "INFO FLAGPART:" server.log -A 1 
```
[🔼 Back to Top](#table-of-contents)

## Riddle Registry
**Category:** Forensics

**Files/Links Provided:** ```Hidden Confidential Document```


**Description:**  

```
Hi, intrepid investigator! 📄🔍 You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense.
But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered.
Find the PDF file here Hidden Confidential Document and uncover the flag within the metadata.
```

**Steps to Solve:**  
1. Open the link to the ```Hidden Confidential Document```
2. Download the file as a PDF
3. Use Exiftool to read the metadata of ```confidential.pdf``` and store it in a txt file.
4. Copy the author information into another txt file.
5. Decode the author from Base64 into plaintext to get the flag.


**Code / Commands / Images**
```bash
exiftool confidential.pdf > metadata.txt
base64 -d "author.txt"
```
[🔼 Back to Top](#table-of-contents)

## Flag in Flame
**Category:** Forensics

**Files/Links Provided:** ```logs.txt```


**Description:**  

```
The SOC team discovered a suspiciously large log file after a recent breach. When they opened it, they found an enormous block of encoded text instead of typical logs.
Could there be something hidden within? Your mission is to inspect the resulting file and reveal the real purpose of it. The team is relying on your skills to uncover any concealed information within this unusual log.
Download the encoded data here: Logs Data. Be prepared—the file is large, and examining it thoroughly is crucial .
```

**Steps to Solve:**  
1. Download ```logs.txt```.
2. Decode the contents of ```logs.txt``` from base64.
3. Convert ```image.txt``` into a png file to generate an image.
4. Transcribe the text depicted within the image.
5. Decode the text from hexadecimal into the flag.


**Code / Commands / Images**
```bash
base64 -d "logs.txt" > image.txt
mv image.txt image.png
```
[🔼 Back to Top](#table-of-contents)

## Crack the Gate 1
**Category:** Web Exploitation

**Files/Links Provided:** ```website```


**Description:**  

```
We’re in the middle of an investigation. One of our persons of interest, ctf player, is believed to be hiding sensitive data inside a restricted web portal. We’ve uncovered the email address he uses to log in: ctf-player@picoctf.org. Unfortunately, we don’t know the password, and the usual guessing techniques haven’t worked. But something feels off... it’s almost like the developer left a secret way in. Can you figure it out?

Additional details will be available after launching your challenge instance
```

**Steps to Solve:**  
1. Open the website and inspect the index HTML file.
2. Decode a comment left by the developer within the index using ROT13 (Caesar Cipher)
3. Recover the temporary bypass header from the comment decoded from ROT13.
4. Open the website within Burp Suite's PortSwigger, turning on Proxy Intercepts after connecting.
5. Input the email provided in the description and any password.
6. Add the bypass header to the login HTTP request.
7. Forward the login request to the website, uncovering the flag.


**Code / Commands / Images**
```Python
def rot13(text):
    result = ""

    for char in text:
        if char.isalpha():
            upper = char.isupper()
            num = ((ord(char.upper()) - 65) + 13) % 26
            new_char = chr(num + 65)

            if not upper:
                new_char = new_char.lower()

            result += new_char

        else:
            result += char

    return result

```
[🔼 Back to Top](#table-of-contents)

## Crack the Gate 2
**Category:** Web Exploitation

**Files/Links Provided:** ```website```, ```passwords.txt```


**Description:**  

```
The login system has been upgraded with a basic rate-limiting mechanism that locks out repeated failed attempts from the same source. We’ve received a tip that the system might still trust user-controlled headers. Your objective is to bypass the rate-limiting restriction and log in using the known email address: ctf-player@picoctf.org and uncover the hidden secret.
```

**Steps to Solve:**  
1. Open the website within Burp Suite's PortSwigger, turning on Proxy Intercepts after connecting.
2. Input the known email, and a password from ```passwords.txt```.
3. Append ```X-Forwarded-For: <ipv4>``` to the top of your HTTP request with a random ip address.
4. Forward the edited HTTP request.
5. Repeat steps 2-4 for each password in ```passwords.txt``` with a different ip for each login attempt.

[🔼 Back to Top](#table-of-contents)

## Hidden in plainsight
**Category:** Forensics

**Files/Links Provided:** ```img.jpg```


**Description:**  

```
You’re given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag. Download the jpg image here.
```

**Steps to Solve:**  
1. Download ```img.jpg```
2. Use ExifTool to read the metadata of ```img.jpg``` and save it to ```metadata.txt```.
3. Decode the comment ```c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9``` within the metadata using Base64 returning ```steghide:cEF6endvcmQ=```.
4. Decode ```cEF6endvcmQ=``` using Base64 again returning ```pAzzword```.
5. Use steghide along with the password to get the flag.


**Code / Commands / Images**
```bash
base64 -d file.txt
steghide extract -sf img.jpg -p pAzzword
```
[🔼 Back to Top](#table-of-contents)

## Corrupted file
**Category:** Forensics

**Files/Links Provided:** ```file```


**Description:**  

```
This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life? Download the file here.
```

**Steps to Solve:**  
1. Download ```file```
2. Use xxd to inspect the hexdump of the file.
3. Compare the first 16 bytes of the corrupted file to a working jpg.
4. Save the xxd into a hexdump file, opening it in a text editor.
5. Repair the file by changing the first nibble (4 bits) to ```ffd8``` which is part of the file signature (header) of a jpg file.
6. Save the file and change it to a ```.jpg```.

**Code / Commands / Images**
![image](https://github.com/Greenest-Guy/picoMini-CMU-Africa-CTF-Writeup/blob/main/images/compare.png)
```bash
xxd file > filedump
xxd -r filedump > file
mv file file.jpg
```
[🔼 Back to Top](#table-of-contents)

## Input Injection 1
**Category:** Binary Exploitation

**Files/Links Provided:** ```vuln.c```, ```vuln```


**Description:**  

```
A friendly program wants to greet you… but its goodbye might say more than it should. Can you convince it to reveal the flag?
```

**Steps to Solve:**  
1. Download ```vuln.c``` and open in a text editor.
2. ```fgets(name, sizeof(name), stdin)``` allows for up to 199 bytes of input but ```fun()``` copies the input using ```strcpy() (no bounds checking)``` into buffer which is 10 bytes, so an input >10 bytes will overflow buffer.
3. Connect to the challenge instance ```nc <host> <port>```.
4. Try a test input which reveals that ```system()``` is executed with the command ```uname```, which outputs Linux, the host's OS.
5. Using an input such as ```0123456789echo "injection"``` echos injection to the terminal, verifying the vulnerability.
6. Reconnect to the server then input ```0123456789ls``` which reveals ```flag.txt```.
7. Finally, after using the input ```0123456789cat flag.txt``` the flag will be displayed in the terminal.

**Code / Commands / Images**

```c
// vuln.c source code
#include <string.h>
#include <stdio.h>
#include <stdlib.h> 

void fun(char *name, char *cmd);

int main() {
    char name[200];
    printf("What is your name?\n");
    fflush(stdout);


    fgets(name, sizeof(name), stdin);
    name[strcspn(name, "\n")] = 0;

    fun(name, "uname");
    return 0;
}

void fun(char *name, char *cmd) {
    char c[10];
    char buffer[10];

    strcpy(c, cmd);
    strcpy(buffer, name);

    printf("Goodbye, %s!\n", buffer);
    fflush(stdout);
    system(c);
}

```
[🔼 Back to Top](#table-of-contents)
