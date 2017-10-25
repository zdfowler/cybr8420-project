Section 1 - Assurance claims
--------
1. Authentication - Keeweb's authentication prevents unauthorized access to users' password data.
2. Encryption - Keeweb's encryption procedures sufficiently protect all sensitive user data before storage.
3. Secure interaction with other software - Keeweb securely and properly communicates with authorized third 
   party software for remote storage
4. Recovery - Keeweb sufficiently protects password data in the event of damage or loss.
*NEEDS REVIEW*
5. Data Integrity - Keeweb protects from unauthorized changes to password data.


Section 2 - Security requirements from mis-use cases
-------
[Mis-use case diagrams are available via Lucidchart here.](https://www.lucidchart.com/invitations/accept/70bd4364-1a62-4d1d-94f8-2f44f9fdb0fe).

The following security requirements have been derived from an analysis of Use and Mis-Use cases.

 1. Authenticate all users before displaying password data
 
    Source: Authentication/Integrity: Log in with master password
    
 2. Manage all password data on local machine to safeguard against attackers through the Internet.
 
    Source - Authentication/Integrity: Log in from same device
    
 3. Automatically lock the app after a certain period of inactivity.
 
    Source - Authentication/Integrity: Set automatic screen locking for 15 minutes
    
 4. Allow user to set the period of inactivity before the screen locks.
 
    Source - Authentication/Integrity: Set automatic screen locking for 15 minutes
    
 5. Create a manual locking mechanism, so the user can choose to lock the app before the period 
    of inactivity expires.
    
    Source - Authentication/Integrity: Manually lock screen before leaving device unattended
    
 6. Encrypt password data file on the local machine.
 
    Source - Encryption: Encrypt database file
    
 7. Before successful login to remote service, confirm the user's session before allowing access to password data.
 
    Source - Secure Interaction: Authorize Third Party Service/Create Authorization Token
    
 8. Use secure protocols to perform transportation of password data file.
 
    Source - Secure Interaction: Use HTTPS/TLS Transport
    
 9. Perform authorization of third party software that interacts with this app.
 
    Source - Recovery/Integrity: Authorize Dropbox Account
     
10. Allow the user to save backups of the password data to cloud-based services.

    Source - Recovery/Integrity: Configures Backup

Section 3 - Alignment of security requirements with advertised features
-------

Advertised Features related to security requirements include offline use/access, ability to enable Dropbox sync, ability 
to create memory-protected fields, built-in password generator, and use of recoverable password record histories.  

Several of the security requirements discussed derive from compatibility with the KeePass software KDBX file format. Support
of this file format necessitates the proper structure of password data (integrity), and implementation of proper encryption.

Other advertised features are related to user interface and input features, which do not related to security requirements 
of the software.


Section 4 - Security configuration and installation issues within documentation
------

1. The Keeweb app configuration does allow the user to configure Keeweb to send password data over 
   networks. 
   
   Source: Keeweb Wiki - FAQ
   
2. All current auto-lock options are defaulted to false. 

   Source: Issue 442 and app/scripts/views/settings/settings-general-view.js
   
3. Auto lock defaults to 15 minutes, which may be too large of an inactive period for people that 
   store password data for accounts with sensitive information.
   
   Source: Keeweb Wiki - Configuration - JSON app config - "idleMinutes" : 15
   
4. Insecure HTTPS warnings can be configured to not be displayed.

   Source: Keeweb Wiki - Configuration - JSON app config - "skipHttpsWarning" : false (but can be 
           set to true)
           
5. Some security features are present and advertised in web build of the application, though may 
   not function in the all builds of the application though advertised as a cross-platform product.  
   The distinctions are unclear to the casual user, and dependent on framework.
   
   Source: Issue 422, Autolock on OS idle/lock is dependent on Electron subsystem. Issue 620: Clipboard clearing function does not clear primary (middle click) clipboard on Linux.
