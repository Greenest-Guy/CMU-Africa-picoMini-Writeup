## 🔐 Crack the Power
**Category:** Cryptography

**Files/Links Provided:** ```message.txt```


**Description:**  

```
We received an encrypted message. The modulus is built from
primes large enough that factoring them isn’t an option, at
least not today. See if you can make sense of the numbers
and reveal the flag. Download the message.
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
[🏠 Back to Main Page](https://github.com/Greenest-Guy/CMU-Africa-picoMini-Writeup)