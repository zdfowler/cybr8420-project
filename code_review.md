Code review strategy
===
The code review will include both automated and manual code review. The review will cover the most important functionality of the
Keeweb code. While the manual code review strategy will not cover all the Keeweb code, it will examine the code that is supposed to
perform authentication, encrypt password data, maintain integrity, transmit data securely, and perform backups. The automated code 
review will cover all of the Keeweb code, but only look for the vulnerabilities that could have harmful impacts to a user's password 
data.

Manual code review strategy
---
Zac will perform the manual code review strategy, since he has more Javascript experience and Keeweb's logic uses Javascript. The manual
strategy will look at the misuse cases and the threat models to create a list of potential vulnerabilities. Any vulnerabilities found
will be documented and listed under the "Keeweb Vulnerabilities" section.<br/>

Automated code review strategy
---
Lisa will perform the automated code review for Keeweb. First, we will find and download a vulnerability scanning tool that can examine
Javascript for Common Weakness Enumerations (CWEs). Second, we will use this scanner on Keeweb's code, narrowing it down to CWEs that 
pertain to our misuse cases and threat models. Third, we will look at a subset of  the vulnerabilities and determine a false positive 
percentage. Finally, if any of the  vulnerabilities discovered by the automated tool relate to the misuse cases or threat models, they 
will be documented and examined manually to verify that they are critical vulnerabilities that need to be patched immediately.

List of CWEs for Node.js (note that not all of these will be applicable to Keeweb):
---
[Node.js CWEs](https://github.com/jesusprubio/strong-node)

CWE's from Threat Modeling (Only threats with state of "Needs investigation")
---
CWE-266 - Incorrect Privilege Assignment<br/>
CWE-290 - Authentication Bypass by Spoofing<br/>

CWE's from Misuse Cases
---
CWE-262 - Not Using Password Aging<br/>
CWE-309 - Use of Password System for Primary Authentication<br/>
CWE-88 - Argument Injection or Modification<br/>
CWE-924 - Improper Enforcement of Message Integrity During Transmission in a Communication Channel<br/>

Manual Code Review Results
===

Transmit Data Securely (Third Party App Communication)
---
Mitigated: [CWE-319: Cleartext Transmission of Sensitive Information](https://cwe.mitre.org/data/definitions/319.html)
Keeweb uses a base storage model (`app\scripts\storage\storage-base.js`), which includes a default, and home-grown set of xhr functions for calling APIs for various storage providers (Google, Dropbox, OneDrive, etc).  For each of the third-party storage apps, the app's ID value is hard coded.  This should not yield a security issue, as the app developer's SECRET value is not coded with the app.

For each service that calls XHR, no additional HTTPS (SSL/TLS) checks are in place aside from catching a generic "Network Error," only seen in the browser console log.  The underlying calls are caught by Chrome's engine, but no indication for the user is presented to indicate a miscommunication in the interface. Typically, the user is given a warning screen or "Not Secure" indicator in a URL.  This is not a security issue -- no traffic is sent to an insecure HTTPS connection -- but the lack of error message may lead to confusion on the user's behalf.

Encrypt Password Data
---
Keeweb -the application- depends on a Javascript dependency by the same author, named kdbxweb.js, wihhc performs almost all of the encryption work.  Two things happen during the keeweb application related to encrypting the password data, by access methods and libraries in kdbxweb.

1. In-memory encryption
2. Datafile encryption

__1. In memory encryption.__  In memory protection is accomplished using a SecureInput utility, in wihch a field's value is XORed with a random salt during input to create a ProtectedValue.  The ProtectedValue contains a "psuedo value", salt, and length, which is used to retrieve the record later.  This prevents in-memory inspection of any protected field.  When the data is written to disk, it is stored in the same format.

Mitigates [CWE-316: Cleartext Storage of Sensitive Information in Memory](https://cwe.mitre.org/data/definitions/316.html)


__2. Datafile encryption.__  KBDX supports several encryption types: AES and ChaCha20, depending on the KDBX cipher Id used when creating the file.  While Keeweb supports both, the default cipher that is used when creating a new datafile is AES (`kdbxweb/lib/format/kdbx.js:446-475`).  In addition to the data encryption, a "crsAlgorithm" is specified in each file header.  Howver, by default, the Salsa20 engine is used and users have expressed issues with this, in how the library attaches with Math.random() calls (which are not really random).  https://github.com/keeweb/keeweb/issues/104

Potential weakness: [CWE-327: Use of a Broken or Risky Cryptographic Algorithm; ](CWE-327: Use of a Broken or Risky Cryptographic Algorithm)
[CWE-330: Use of Insufficiently Random Values](http://cwe.mitre.org/data/definitions/330.html)





Automated Code Review Results
===
Keeweb uses Javascript for its logic, so we used PMD to analyze Keeweb. PMD is an open-source analysis tool written in Java. It can analyze a code repository and find coding issues, such as dead code, or bad coding practices, such as a function that returns different datatypes from different branches of the code. We downloaded the command line PMD and uses this to generate the total numbers for each type of flaw that was found, including the false positives. Then, we downloaded the Eclipse IDE and the PMD plugin for Eclipse. Running PMD on the code within Eclipse pinpoints the lines of code for each vulnerability. Using the PMD Eclipse plugin, we looked at the full set, or a subset of each type of flaw to determine the percentage of false positives. 

The following is a description of what we found during the automated analysis:

Inaccurate Numeric Literal ([CWE-197 - Numeric Truncation Error](https://cwe.mitre.org/data/definitions/197.html))
--- 
This issue occurs if integers are too large or floating point numbers are too precise. Basically, they could lose some precision during runtime. PMD flagged this issue 62 times in Keeweb. All but one were flagged on hex numbers; the longest hex number that was flagged was '0xffffffff', which is less than the valid value of '999999999999999' that PMD says is okay. The remaining one was flagged on a decimal number '0.10'. This is okay too, since the precision is only two decimal places (this error will not occur unless there are at least 13 digits after the decimal place). So, all 62 of these are false positives.

Unreachable Code ([CWE-561 - Dead Code](https://cwe.mitre.org/data/definitions/561.html)) 
--- 
PMD found four instances of unreachable code. All four errors had the same description: "A 'return', 'break', 'continue', or 'throw' statement should be the last in the block." For each of these instances, it flagged on 'throw' statements, so all of these were false positives.

Use Base with ParseInt ([CWE-474 - Use of Function with Inconsistent Implementations](https://cwe.mitre.org/data/definitions/474.html))
---
PMD found one instance of this issue. The issue with not supplying a base to Javascript's ParseInt is that different browsers will default to different values, leading to unpredictable behavior. The line of code that was flagged only supplies a value to ParseInt, whereas, good coding practice requires a value and a base. The subsequent code only throws an error if the value from ParseInt is not a number (NaN) or negative, so the overall function can return true for an incorrect positive number. This flagged line of code was found in the autotyping functions, so this could potentially allow someone to proceed with an invalid value if they run this code in an environment that gives ParseInt an unexpected base value, and supply the correct arguments to get the result they want from ParseInt. 

A closer look at the comment part of this function indicates that it is intended to be flexible with the base parameter. For example, if the function gets "20", it will return 20, but if the function gets "0x20", it will return 32. The comment preceding the ParseInt call indicates that it should be able to parse hex digits, as well as regular digits.

Assignment In Operand ([CWE-481 - Assigning instead of Comparing](https://cwe.mitre.org/data/definitions/481.html)) 
--- 
PMD found two instances of this potential vulnerability. Basically, PMD will flag any assignments within the Boolean check of an 'if' statement or a 'while' loop. The reason for flagging these is that it makes the code more difficult to read and it might be a bug if the code did an assignment '=', when they really wanted to do a Boolean check '==', In the case of Keeweb, both instances use the increment/decrement operand assignment '++' and '--'. This is similar to the syntax for a 'for' loop's last argument, so these are both false positives.

Consistent Return (No CWE)
--- 
PMD found one instance where a function returned different types in different branches. This requires subsequent uses of this function to always check the type of the return variable before anything can be done with it. Although it is not a vulnerability, subsequent updates to Keeweb that use the function without the coder knowing that it returns different types in different branches can lead to more bugs and software crashes.

Use of Global Variables (No CWE)
---
PMD found 736 instances of global variable usage. While this means that Keeweb may be harder to debug, there are no vulnerabilities associated with global variables and the Open Source community is not likely to see this as an important security issue, since it is more of a debugging issue than a security issue.

Keeweb Vulnerabilities
===
CWE-450 - Multiple Interpretations Of UI Input/CWE-357 Insufficient Warning Of UI Operations
---
The automated review analysis revealed an instance where ParseInt can interpret both hex numbers and regular numbers. Using this knowledge, we opened an instance of Keeweb and logged in with the master password. From the left-hand menu, we selected "New" and scrolled down to the "Advanced Settings" and entered a very large hex value in the "Ask to change key after (days)" field. Then, one of these two things caused a sync error:
1. Saving it to the local system (Save button)
2. Saving it to Dropbox (Save To -> Dropbox)

When the file tries to sync during a routine sync operation, the following error is displayed at the top of the screen and the app:
"Error FileCorrupt: no key encryption rounds in header"

Also, the "Key Encryption Rounds" field (the one above the "Ask to change key after (days)") disappears from the screen, which explains the "No key encryption rounds in header" error.
