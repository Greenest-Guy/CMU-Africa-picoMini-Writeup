# picoMini-CMU-Africa-CTF-Writeup

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
3. Check if the ciphertext corresponds to an exact integer e-th power, if the plaintext $$m$$ satisfies $$m^e < n$$ then $$m^e = c$$. Now you can recover $$m$$ by taking the e-th root of the ciphertext:
   
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
3. Use grep to find the flagparts.
4. reconstruct the flag using the flag parts.


**Code / Commands / Images**
```bash
grep "picoCTF" server.log -i
grep "INFO FLAGPART:" server.log -A 1 
```

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
4. Open the website within Burp Suite's PortSwigger turning on Proxy Intercepts after connecting.
5. Input the email provided in the description and any password.
6. Add the bypass header to the login HTTP request.
7. Forward the login request to the website uncovering the flag.


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

## Hidden in plainsight
**Category:** Forensics

**Files/Links Provided:** ```img.jpg```


**Description:**  

```
You’re given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag. Download the jpg image here.
```

**Steps to Solve:**  
1. Download img.jpg
2. Use ExifTool to read the metadata of ```img.jpg``` and save it to ```metadata.txt```.
3. Decode the comment ```c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9``` within the metadata using Base64 returning ```steghide:cEF6endvcmQ=```.
4. Decode ```cEF6endvcmQ=``` using Base64 again returning ```pAzzword```.
5. Use steghide along with the password to get the flag.


**Code / Commands / Images**
```bash
base64 -d file.txt
steghide extract -sf img.jpg -p pAzzword
```
