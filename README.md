# picoMini-CMU-Africa-CTF-Writeup

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
exiftool confidential.pdf > file.txt
base64 -d "decode.txt"
```
