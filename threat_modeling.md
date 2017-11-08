

Review the Microsoft Threat Modeling Tool report at ><.

Summary of Findings
---
The threat model shows that many of the core issues are mitigated using standard patterns and trust assumptions.  For example, cloud provider authenticity for remote storage is validated by the use of a browser popup.  The popup brings all the normal security warnings -- the HTTPS indicator, or lack thereof, and URL are prominently displayed, mitigaging spoofing and tampering.  The KeeWeb process (when run in a browser tab) will access local browser storage using the browser process, which limits scope of tampering and denial of service attacks. 

Additionally, the trust boundary that is crossed when storing the KBDX file to disk opens the door to possible tampering.  However, such tampering would require the Password Owner's master password.  Without it, the attacker cannot tamper the file in such a way that it can be decrypted (thus, opened) without corruption.  The user must rely on operating system protections at this point.

A set of tampering threats remain unmitigated that should be examined further.  The keyweb application is a client-side only, standalone application which can be run in a browser.  However, when distributed from a third party website, there is no verification that the web version (e.g., https://app.keeweb.info) is in fact a compilation of the correct and official packaging.  According to the online documentation, the desktop-based version provides signature checks before performing self-updates, however, there is no indication in that the web version has any published signature files that can be used to validate the authenticity of the keeweb application from a particular source.  
