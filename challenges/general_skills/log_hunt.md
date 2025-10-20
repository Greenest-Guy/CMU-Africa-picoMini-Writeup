## 🧠 Log Hunt
**Category:** General Skills

**Files/Links Provided:** ```server.log```


**Description:**  

```
Our server seems to be leaking pieces of a secret flag in
its logs. The parts are scattered and sometimes repeated.
Can you reconstruct the original flag? Download the logs
and figure out the full flag from the fragments.
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
[🏠 Back to Main Page](https://github.com/Greenest-Guy/CMU-Africa-picoMini-Writeup)