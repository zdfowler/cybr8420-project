Section 1 - Assurance claims
--------
1. Authentication - Keeweb's authentication prevents unauthorized access to users' password data.
2. Encryption - Keeweb's encryption procedures sufficiently protect all sensitive user data before storage.
3. Secure interaction with other software - Keeweb securely and properly communicates with authorized third 
   party software for remote storage
4. Recovery - Keeweb sufficiently protects password data in the event of damage or loss.
5. Data Integrity - Keeweb protects from unauthorized changes to password data.


Section 2 - Security requirements from mis-use cases
-------
The following list outlines the security requirements which are captured in Mis-use case diagrams based on the assurance claims listed above.  [Mis-use case diagrams are available via Lucidchart here.](https://www.lucidchart.com/invitations/accept/70bd4364-1a62-4d1d-94f8-2f44f9fdb0fe).


 1. All users must be authenticated before displaying password data<br>
    Source: Authentication/Integrity: Log in with master password
    
 2. Manage all password data on local machine to safeguard against attackers through the Internet.<br>
    Source - Authentication/Integrity: Log in from same device
    
 3. Automatically lock the app after a certain period of inactivity.<br>
    Source - Authentication/Integrity: Set automatic screen locking for 15 minutes
    
 4. Allow user to set the period of inactivity before the screen locks.<br>
    Source - Authentication/Integrity: Set automatic screen locking for 15 minutes
    
 5. Create a manual locking mechanism, so the user can choose to lock the app before the period 
    of inactivity expires.<br>
    Source - Authentication/Integrity: Manually lock screen before leaving device unattended
    
 6. Encrypt password data file on the local machine.<br>
    Source - Encryption: Encrypt database file
    
 7. Before successful login to remote service, confirm the user's session before allowing access to password data.<br>
    Source - Secure Interaction: Authorize Third Party Service/Create Authorization Token
    
 8. Use secure protocols to perform transportation of password data file.<br>
    Source - Secure Interaction: Use HTTPS/TLS Transport
    
 9. Perform authorization of third party software that interacts with this app.<br>
    Source - Recovery/Integrity: Authorize Dropbox Account
     
10. Allow the user to save backups of the password data to cloud-based services.<br>
    Source - Recovery/Integrity: Configures Backup
    

Section 3 - Alignment of security requirements with advertised features
-------

Advertised features related to security requirements include offline use/access, ability to enable Dropbox sync, ability 
to create memory-protected fields, built-in password generator, and use of recoverable password record histories.  The requirements derived from Use and Mis-Case analysis which are not listed as features include the use of secure protocols (secure interaction), proper authorization with third party services (secure interaction), the ability to save remote backups (reliability), and the manual- and auto-locking protection feature of the application (authentication).

Several of the security requirements discussed derive from compatibility with the KeePass software KDBX file format, which is one of the primary functions of the KeeWeb software. Support of this file format necessitates the proper structure of password data (integrity), and implementation of proper encryption.

Other advertised features are related to user interface and input features, which do not relate to security requirements 
of the software.

Interestingly, the final listed feature of the app is that it is Open Source software.


Section 4 - Security configuration and installation issues within documentation
------

In examining documentation for security configuration and installation notes, the documentation artifacts included FAQs, Wiki, and the project Issue tracker.  Below are seveal items we found that may require investigation when evaluating how the software meets its security requirements which are also described.

In terms of configuration the software has many defaults which can be overridden by configuration parameters or user actions by choice.  For example, HTTPS warnings can be disabled in configuration settings, configuration setings can be loaded at run time via URL parameter (and JSON file), auto-lock can be disabled, and database files can be saved without a master password.

When installing the software on a static web server for your own purposes (a custom installation), more issues pop up related to cross-origin-resource-sharing (CORS) and the security of hosting your own application and its communication both downstream to the client and upstream to the storage provider (if any outside of the browser local storage).

Some security features are present and advertised in web build of the application, though may not function in the all builds of the application though advertised as a cross-platform product.  The distinctions are unclear to the casual user, and dependent on framework. For example, open issue 422 describes how autolock on OS idle/lock is dependent on Electron subsystem (Mac OS vs. Win/Web/Linux). Also, open issue 620 describes how the clipboard clearing function does not clear primary (middle click) clipboard on Linux.
   
When creating a new KeeWeb database, i.e. a new installation, password entries can be created and saved witout requiring a master password or choosing a storage location outside of the browser's app storage.  Before the file is saved, KeeWeb warns the user about a blank master password, but does not require one.  Nor does the software require remote storage or backup, potentially leading the user to misunderstand how the datafile is stored on disk.
