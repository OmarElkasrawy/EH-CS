
## **The OWASP checklist for Web App Penetration testing**

Without any further delay, let us dive into the OWASP web application penetration checklist to conduct a thorough web app pen test:

### **1. Information Gathering**

The first step is to gather as much information about the target web application as possible.

#### **Manual site exploration**

- First on the checklist is to perform manual site exploration for potential weak spots.

#### **Locating the robots.txt files**

- Identify and retrieve the robot.txt files that tell search engine crawlers which URLs they can access on your site. They can be retrieved using GNU Wge, a computer program that can retrieve content from web servers. This approach aims to crawl the website for missing or hidden content. Also, look for other files that can expose content, such as sitemap.xml and .DS_Store.

#### **Identify versions and channels**

- Here you must review the software versions of the web, mobile web, mobile app, and web services. Additionally, study the database information and technical components for potential errors and bugs.

#### **Implement DNS-based techniques**

- Perform DNS inverse queries, DNS zone Transfers, and web-based DNS Searches.

#### **Perform Directory style Searching and vulnerability scanning**

- Next on the list is leverage tools such as Nessus and NMAP to probe for URLs.

#### **Identify the Entry point of the application.**

- Tools such as OWSAP ZAP, Burp Proxy, Webscarab, Tamper Data, and TamperIE identify an application’s entry points.

#### **Perform Web Application Fingerprinting**

- Here use Nmap, Amap, and service Fingerprinting to conduct grouping of information to detect the software, network protocols, operating systems, or hardware devices on the network. Web application fingerprinting also helps match the signature of the target web server with the list of known signatures contained within the .txt files.

#### **Test for recognized file extensions, types, and directories**

- Test for common file extensions such as .exe, .asp, .html, and .php.

#### **Identify client-side source code**

- Next is to examine the source code from the client-side pages of the application’s front end.

### **2. Authentication Testing**

Through authentication testing, you must run a series of checks to try and gain access to sensitive credentials to gain login access to the web app.

#### **Test for logout functionality presence**

- Check the effectiveness of the logout functionality and if it’s possible to continue using the session after logging out. Additionally, check if the application automatically logs users out if they are inactive for a certain period.

#### **Test for cache management on HTTP**

- Check sources such as the browser cache storage for any remaining sensitive information.

#### **Test password reset and recovery functionalities for flaws**

- Test exploitable vulnerabilities such as weak security questions and answers.

#### **Test remember my password functionality**

- Check if the ‘Remember my password’ functionality is implemented correctly by checking the HTML code of the login page.

#### **Test for authentication bypass**

- Test if any hardware devices directly and independently communicating with authentication infrastructure using a separate channel can be used to bypass authentication.

#### **Test CAPTCHA**

- Test for authentication vulnerabilities with the CAPTCHA security functionality.

#### **Test password change process**

- Test the password change process and attempt to change a password using tactics like social engineering, cracking secretive questions, and guessing.

#### **Test the Web Application Firewall**

- Testing for weak spots and misconfigurations within web application firewalls can help identify if there are opportunities to implement SQL injections to steal sensitive data.

### **3. Authorization Testing**

Authorization testing involves testing the target web app to understand how the authorization mechanism works. Any information gathered in this stage will be instrumental in circumventing the authorization mechanism.

#### **Test for vertical Access control problems**

- Test for the opportunity to perform privilege or role escalation through manipulation to access restricted resources.

#### **Test for path traversal**

- Perform input vector calculation and analyze the input validation functions presented in the web application.

#### **Test for Cookie Tampering**

- Test for cookie and parameter Tampering using web spider tools.

#### **HTTP request tampering**

- Test for HTTP Request Tempering and check the possibility of gaining unauthorized access to application resources.

### **4. Configuration Management Testing**

Check directory and file enumeration review server and application documentation. Additionally, check the infrastructure and application admin interfaces.

#### **Perform network and web server scanning**

- Perform a network scan and analyze the web server banner.

#### **Check for backup, old, and unreferenced files**

- Check and verify the presence of backups, old documentation, and unreferenced files such as passwords, source codes, and installation paths.

#### **Identify SSL/TLS ports**

- Use tools such as Nmap and Nessus to review and pinpoint the ports used for SSL/TLS services using NMAP and NESSUS.

#### **Review OPTIONS HTTP**

- Use utilities such as Netcat and Telnet to review Options HTTP.

#### **Test for HTTP methods and Cross Site Tracing (XST)**

- Test for HTTP methods and XST to gain the credentials of legitimate application users.

#### **Check log files, source code, and default error codes**

- Conduct configuration management tests on the web application to access and review log files, source code, and default error codes.

### **5. Session Management Testing**

The goal here is to check if the target application securely creates session tokens and cookies, with the hopes of forging a cookie to hijack user sessions.

#### **Test for Cross-Site Request Forgery**

- Review the restricted URL areas and test the possibility of Cross-Site Request Forgery.

#### **Confirm that new session tokens are issued upon login, role change, and logout**

- Review the encryptions, proxies, caching, and GET and POST. Check session tokens for possible reusability.

#### **Cookie Forging**

- Accumulate an adequate quantity of cookie samples and analyze their algorithms. Check for the possibility of forging a validated cookie to perform an attack.

#### **Testing the cookie attributes**

- Use intercept proxies such as OWASP ZAP and Burp Proxy to test the cookie attributes. Utilize traffic intercept proxies such as Tamper data to check for the possibility of tampering with the data transmitted between the client and the server.

#### **Test for session fixing**

- Try to attack to hijack a valid user session through session fixing.

### **6. Data Validation Testing**

It is time to validate the data and databases for potential vulnerabilities that can be exploited.

#### **Identify javascript coding errors**

- Analyze the source code for JavaScript coding errors.

#### **Test for SQL Injection**

- Check for the possibility of leveraging SQL injection tools such as sqldumper, SQL power injector, and sqlninja to implement SQL injections such as Union SQL injection, standard SQL injection, and blind SQL query tests.

#### **Test for HTML Injection**

- Use tools such as XSS proxy, Backframe, XSS Assistant, Burp Proxy, and OWASP ZAP test for leverageable XSS vulnerabilities.

#### **Test for LDAP Injection**

- Perform LDAP injection testing to acquire sensitive user and host information.

#### **Test for IMAP/SMTP Injection**

- Perform IMAP/SMTP injection testing to attempt to access the backend mail server.

#### **Test for XPath Injection**

- Perform XPATH Injection testing to attempt to access sensitive information.

#### **Test for XML Injection**

- Perform XML injection testing to learn about the application’s XML information structure.

#### **Test for Code Injection**

- Perform code injection testing by submitting input processed by the web server as an included file or dynamic code to identify input validation errors.

#### **Buffer Overflow testing**

- Perform buffer overflow testing to check if the program gauges if the application stores more data in the buffer than intended. This test helps gain knowledge on the application control flow and stack and heap memory.

#### **Test for HTTP Splitting/Smuggling**

- Test for HTTP splitting and smuggling for cookies and redirect information to gauge the possibility of performing cache poisoning or cross-site scripting.

### **7. Denial of Service Testing**

Carry out a denial of service or load testing on the target application to identify potential vulnerabilities to carry out DoS attacks.

#### **Check for slowdown**

- Send a bombardment of requests that perform intensive database operations and observe for any performance issues and error messages.

#### **Manual source code analysis**

- Perform manual source code to check every line for potential vulnerabilities. Once completed, submit a range of inputs with varying lengths to the vulnerable spots in the applications.

#### **Test for SQL wildcard DoS**

- Test for SQL wildcard attacks for application information testing. Enterprise Networks should choose the best DDoS Attack prevention services to ensure DDoS attack protection and prevent their network.

#### **User Specified Object Allocation DoS**

- Test for user-specified object allocation to determine the maximum number of objects the application can handle and if you can exhaust the server’s resources by allocating a considerable number of objects.

#### **Exploit the input field**

- Use tools such as Loop counter with controlled iterations to overwhelm the application’s input field with requests.

#### **Application-layer Flood**

- Using an automated script, conduct an application-layer flooding attack by submitting an excessively long input value for the server to log requests beyond its computational capability.

In conclusion, if followed meticulously, this complete checklist should allow you to reduce operational failures, application errors, and loopholes in the application’s infrastructure.

Suppose you feel you need further assistance in conducting your penetration tests. In that case, several professional penetration testing service providers exist to help you implement thorough and effective penetration tests on your web applications.

---
#owasp_checklist 