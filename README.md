# 5-ThickClient

A Thick Client is a client in client–server architecture and typically provides rich functionality, independent of the server. 

In these types of applications, the major processing is done at the client side and involves only a periodic connection to the server.

- Thick client applications are developed using some of the following technologies:
  - Executable files [Written in Java, .Net, C/C++, Microsoft Silverlight]
  - Java applets etc

## Difference between Thick & Thin Client application
| Thick Clent  | Thin Client |
| ------------- | ------------- |
| Installed on local computer   | Web application which accessed from internet through browser |
| Client side   | Complete processing on server side  |
| Uses computer resources  | Uses HTTP/HTTPS protocol  |
| Periodicaly sync with server remotely  | Continuous communication between Server and Client  |
| Use multiple ports & protocols (SMTP, TCP, HTTP/HTTPS)  | Most common ports 80, 443, 8080  |
| Example: Gtalk, Skype, Tally, ERP Software  | Example: google.com or yahoo.com  |

## How Think Client Application tested?
1. Information Gathering
2. Dynamic Testing (Proxy setup and Traffic interception, Fuzzing, Injections, etc.)
3. System level testing (Checking for logs, Data files, Registry keys, Process threads)
4. Static testing (Local storage and Memory testing)
5. Decompiling and Reverse engineering (Binary analysis)

### 1. Information Gathering
 - How is the application built - [CFF EXPLORER](https://ntcore.com/?page_id=388)
 - What are the servers the client is communicating with? - [TCPView / Wireshark ]

### 2. Dynamic Testing (Proxy setup and Traffic interception, Fuzzing, Injections, etc.)
-	Input Validation - SQL injection, Command injection, LDAP injection.
-	File Upload - Malicious file uploading in the application. 
- Buffer Overflow -  Injecting large random values in the input fields.
- Secure traffic analysis (SSL or TLS protocols) - Check if the request generated by the application is in clear text format.
- Business logic validation - Updating profile with some other user’s identity, Accessing the documents intended for some other user.
- Improper error handling / Info Leakage - Check for  internal error messages (server errors) are disclosed to the application user in response to any invalid value entered by the user.
- Broken authentication & Session management
- Try connecting directly to URLs via the web browser / Forced URL access via browser - Checking application if  uses the HTTP or HTTPS protocol then the configuration file. URL could be used to connect the server directly using web browser for finding web vulns.
- Log forging / Log tampering - Check for timestamp generated the client on the server side, this may lead to unreliable logs of the user activity.

### 3. System level testing (Checking for logs, Data files, Registry keys, Process threads)
-	Sensitive data in process memory: Memory inspection to find hardcoded passwords or clear-text credentials in process memory -> [ Winhex ]
-	Dependencies in process: Checking for the executables and libraries used by the application. -> [ process monitor / Regshot ]
-	Privilege levels of the application: Checking for the privilege level of the application on the client machine. Inject Privileged DLL Files
   - DLL High-jacking - Check if the application validates the DLL or exe used by the application 
     - Check if application is signing the DLL.
     - Inject Malicious DLL.
  -	Testing the application Registry: Applications activities in the local machine, Real-time file system, registry, and process/thread activity -> [Process Monitor / regshot]
     - Registry permissions: Read and write access to registry keys
     - Registry content: Sensitive data, passwords and settings
     - Registry manipulation: bypass authentication and authorization, replace content
     - Monitors calls to file, registry keys and sockets

### 4. Static testing (Local storage and Memory testing)
There are many test cases which aid us to perform static analysis. Some of these include:
- Memory Level Protection Checks ( DEP / ASLR) -> [ Process Explorer / Process Hacker ]
-	Analysing Configuration files (Check if configuration files of the application disclose URL, Server credentials, Cryptographic keys and Clear text details) -> [ Notepad++ ]
-	Test encryption used in the application - If the application uses encryption, check how encryption keys are stored in the application. 
Check what kind of encryption is used in the application (i.e. symmetric or asymmetric encryption).
 - String based analysis to find information -> [ Process Explorer ]
  -	Testing the Application : Files
    - File permissions - Files and folders
    - File Integrity - Strong naming, Authentic code signing
    - File content Debugging Symbols/Files, Sensitive data, Passwords and settings
    - File and content manipulation:
      - Backdoor the framework
      - DLL pre loading
      - Race conditions
      - Replacing files and content

### 5. Decompiling and Reverse engineering (Binary analysis)

-	Source code Decompilation - Check if the executable file could be decompiled to the source code of the application. -> [ dotPeek ](https://www.jetbrains.com/decompiler/download)
- Reverse Code Level Logic to bypass checks/ licences (Check for the hard coded credentials, cryptographic keys or business rules which could be modified and the application could be recompiled.)-> [ Ollydbg / IDA Pro / JD-GUI ]


## Tools For performing Thick client Pen-testing:
 - Interception proxies:
    - Brp Suite
    - Fiddler
    - Echo Mirage
    - Charles
    - Mallory
    - JavaSnoop
   
 - Traffic Analysis: 
    - TCP Dump
    - Wireshark (capturing communcation packets)
    - Interactive TCP Relay
    - TCP View (Checking connection details with server)

 - Static Analysis:
    - System Internals:
      - Process Monitor (Checking sensitive data/ Monitors calls to file, registry keys and sockets)
      - Process Explorer (View image file settings, process, connections, threads, permissions, strings from process, environmental variables)
      - AccessEnum (Dump file and registry permission)  
      - Regedit (Backup, review and edit the registry)
      - Strings (Dump strings from files) 
    - [CFF Explorer](https://ntcore.com/?page_id=388)(For Checking Application information) 
    - Dllexp (Review exports, view/edit imports, edit and extract resources, view disk memory usage)
    - Regshot (Checking for data storage in registers)
    - API Monitor (Monitors and displays API calls made by applications and services)
    - Process Hacker (View DEP/ASLR settings, image file settings, connections, threads, permissions, strings from process, environmental variables)
    - Spider2008 (Search file system for interesting strings with regular expressions)
    - AccessEnum, Privesc, autoruns, schtasks (Dumping file,registry, and service permissions. Also review scheduled tasks excessive privilege and write script locations)
 
 - Reversing / Decompilers:
    - Java Byte Code Editor
    - JD GUI
    - Ollydbg
    - [JetBrains dotPeek](https://www.jetbrains.com/decompiler/download)(Decomplier / Reversing .Net Application) 
    - [ILSpy](http://ilspy.net/) (Reversing .Net Application)
    - DnSpy (Reversing .Net Application)
    - Graywolf

 - Packed executable checking tools:
    - PE Explorer (Detect compiler or packer type version)
    - PEid
    - UPX Decompression
    - .Net Reflector
    - [Reflexil](http://reflexil.net/) (Reflexil is an assembly editor and runs as a plug-in for Red Gate’s Reflector) 
    - [ILSpy](http://ilspy.net/) (ILSpy and Telerik’s JustDecompile) 

 - Memory Analysis:
    - Winhex (Checking for data storage like username and password on client side/memory)
    - Hxd
    - Volatility
    - Tsearch ( find and replace strings in memory)
    - Userdump

 - Injection:
    - [qlmap](https://github.com/sqlmapproject/sqlmap) 
 - DLL hijacking:
    - Rattler (Automated DLL Enumerator)
 - Automated Source Code Review
    - [VSS Grepper](https://sourceforge.net/projects/visualcodegrepp)
    - Window Detective (View form object properties including the value of masked password fields, and mask card numbers)
    - BinScope ( binary analysis tool provided by Microsoft. This tool is useful to check if the software being audited is making use of common exploit mitigation techniques such as DEP, ASLR, etc)

 - Exploitation:
    - Metasploit ( used for side loading/ DLL and Exe injection)
    - MSFpayload,msfencode, and MSFVenom (Used to generate shell code, DLL and EXE payloads for injection and side loading. This also ships with metasm ruby library that can be used to disassemble and compile binaries)

## Vulnerabilities applicable to Thick Client application

## Useful Recommendations
 - Use three tier architecture instead of two tier application
 - Encrypt traffic using strong algorithm
 - Validate user inputs for length, special characters & code
 - Maintain adequate Audit trail
 - Do not store sensitive information like user password in computer memory, files, registry or database in clear text format
 - Default database port should not be use
 - Strong password policy
 - Session IDs used should be random and unbreakable.
 - Application should handle the errors without disclosing critical system information
 - Implement proper file permission on application resources
 - Basic Hygine & System hardening
 - Proper patch management

