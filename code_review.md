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
percentage. Finally, if any of the  vulnerabilities discovered by the automated tool relate to the misuse cases or threat models, they 
will be documented and examined manually to verify that they are critical vulnerabilities that need to be patched immediately.<br/>
<br/>
List of CWEs for Node.js (note that not all of these will be applicable to Keeweb):<br/>
[Node.js CWEs](https://github.com/jesusprubio/strong-node)<br/>
<br/>
CWE's from Threat Modeling (Only threats with state of "Needs investigation"):<br/>
CWE-266 - Incorrect Privilege Assignment<br/>
CWE-290 - Authentication Bypass by Spoofing<br/>
<br/>
CWE's from Misuse Cases:<br/>
CWE-262 - Not Using Password Aging<br/>
CWE-309 - Use of Password System for Primary Authentication<br/>
CWE-88 - Argument Injection or Modification<br/>
CWE-924 - Improper Enforcement of Message Integrity During Transmission in a Communication Channel<br/>
<br/>
Manual Code Review Results<br/>
<br/>
Automated Code Review Results<br/>
Keeweb uses Javascript for its logic, so we used PMD to analyze Keeweb. We downloaded the command line PMD and uses this to generate 
the total numbers for each type of flaw that was found, including the false positives. Then, we downloaded the Eclipse IDE and the PMD plugin for Eclipse. Running PMD on the code within Eclipse pinpoints the lines of code for each vulnerability. Using the PMD Eclipse plugin, we looked at a subset of each type of flaw to determine the approximate percentage of false positives. The following is a list 
of the false positives that we found during the automated analysis:<br/>
<br/>
Inaccurate Numeric Literal - This issue occurs if integers are too large or floating point numbers are too precise. Basically, they could lose some precision during runtime. PMD flagged this issue 62 times in Keeweb. All but one were flagged on hex numbers; the longest hex number that was flagged was '0xffffffff', which is less than the valid value of '999999999999999' that PMD says is okay. The remaining one was flagged on a decimal number '0.10'. This is okay too, since the precision is only two decimal places (this error will not occur unless there are at least 13 digits after the decimal place. So, all 62 of these are false positives.<br/>
<br/>


Keeweb Vulnerabilities<br/>
