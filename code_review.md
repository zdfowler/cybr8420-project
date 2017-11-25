Code review strategy<br/>
The code review will include both automated and manual code review. The review will cover the most important functionality of the
Keeweb code. While the manual code review strategy will not cover all the Keeweb code, it will examine the code that is supposed to
perform authentication, encrypt password data, maintain integrity, transmit data securely, and perform backups. The automated code 
review will cover all of the Keeweb code, but only look for the vulnerabilities that could have harmful impacts to a user's password 
data.<br/>
<br/>
Manual code review strategy<br/>
Zac will perform the manual code review strategy, since he has more Javascript experience and Keeweb's logic uses Javascript. The manual
strategy will look at the misuse cases and the threat models to create a list of potential vulnerabilities. Any vulnerabilities found
will be documented and listed under the "Keeweb Vulnerabilities" section.<br/>
<br/>
Automated code review strategy<br/>
Lisa will perform the automated code review for Keeweb. First, we will find and download a vulnerability scanning tool that can examine
Javascript for Common Weakness Enumerations (CWEs). Second, we will use this scanner on Keeweb's code, narrowing it down to CWEs that 
pertain to our misuse cases and threat models. Third, we will look at a subset of  the vulnerabilities and determine a false positive 
percentage. Fourth, we will use the results of the vulnerability scanner to produce  input values for the Common Weakness Scoring 
System (CWSS). Finally, we will either find a tool or create a Python script to calculate the CWSS score for Keeweb. If any of the 
vulnerabilities discovered by the automated tool relate to the misuse cases or threat models, they will be documented and examined 
manually to verify that they are critical vulnerabilities that need to be patched immediately.<br/>
<br/>
List of CWEs for Node.js (note that not all of these will be applicable to Keeweb):<br/>
[Node.js CWEs](https://github.com/jesusprubio/strong-node)<br/>
<br/>
CWE's from Threat Modeling (Only threats with state of "Needs investigation"):
CWE-266 - Incorrect Privilege Assignment
CWE-290 - Authentication Bypass by Spoofing<br/>
<br/>
CWE's from Misuse Cases:
CWE-262 - Not Using Password Aging
CWE-309 - Use of Password System for Primary Authentication
CWE-88 - Argument Injection or Modification
CWE-924 - Improper Enforcement of Message Integrity During Transmission in a Communication Channel

Keeweb Vulnerabilities<br/>
