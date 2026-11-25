# Mediroza General Hospital – Penetration Testing Project

## Project Overview

This project documents an authorized penetration testing assessment conducted as part of a cybersecurity practical training project.

The assessment focused on identifying security vulnerabilities in a hospital web application and its exposed services. The testing activities included network reconnaissance, service enumeration, web application analysis, authentication testing, exposed backup investigation, and password recovery testing against encrypted PDF documents.

The main objective was to identify potential security weaknesses, evaluate their impact, and provide recommendations to improve the organization's security posture.

**Assessment Type:** Authorized Penetration Testing  
**Operating System:** Kali Linux  
**Focus Areas:** Network Security, Web Application Security, Vulnerability Assessment, Password Security

---

## Project Objectives

- Perform reconnaissance against the target environment.
- Identify exposed network services and application endpoints.
- Analyze the hospital web application.
- Investigate authentication weaknesses and potential SQL injection vulnerabilities.
- Identify publicly accessible backup resources.
- Investigate potential exposure of confidential organizational information.
- Extract and analyze encrypted PDF password hashes.
- Perform dictionary-based password recovery using Hashcat.
- Document vulnerabilities, security risks, and remediation recommendations.

---

## Scope of Assessment

The assessment focused on the authorized hospital web application and selected network services.

| Category | Description |
|---|---|
| Target | Hospital web application |
| Assessment Type | Authorized penetration testing |
| Environment | Kali Linux |
| Network Testing | Port scanning and service enumeration |
| Web Testing | Application enumeration and authentication analysis |
| Password Testing | Encrypted PDF password recovery |
| Reporting | Technical findings and remediation recommendations |

**Testing restrictions:**
- No denial-of-service testing.
- No social engineering.
- Testing limited to the authorized project scope.
- Confidential information excluded from this public repository.

---

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Kali Linux | Penetration testing environment |
| Nmap | Network scanning and service enumeration |
| cURL | HTTP requests and response inspection |
| Hashcat | Password hash recovery |
| John the Ripper | Password hash analysis |
| Web Browser | Manual web application testing |

---

## Methodology

The assessment followed a structured penetration testing methodology.

### 1. Reconnaissance

The initial phase involved identifying the target web application and reviewing publicly accessible resources.

Activities:
- Identifying the target host.
- Reviewing the website structure.
- Identifying accessible pages.
- Discovering application login endpoints.
- Reviewing publicly accessible resources.

### 2. Network Scanning and Enumeration

Nmap was used to scan selected network ports and identify potential running services.

**Command used:**

```bash
nmap -sC -sV -O -p21,25,53,80,110,143,443,465,587,993,995 -oN mediroza_nmap.txt TARGET_IP
```

The scan results were reviewed to identify exposed services and potential areas requiring further investigation.

### 3. Web Application Enumeration

cURL and a web browser were used to inspect the website structure, identify application endpoints, and review HTTP responses.

**Example commands:**

```bash
curl -sI https://TARGET_DOMAIN/
```

```bash
curl -s https://TARGET_DOMAIN/ | grep -oE '(href|src)="[^"]+"' | sort -u
```

The enumeration phase identified publicly accessible pages and authentication-related endpoints.

### 4. Authentication and SQL Injection Analysis

The patient login functionality was investigated for potential authentication weaknesses.

SQL injection bypass behavior was reported during testing.

**Reported test input:**

```text
admin'--
```

The investigation focused on whether user-controlled input could affect database query execution and potentially bypass authentication controls.

The original successful-login request and response should be retained as supporting evidence in the restricted assessment archive. The exact database query and database engine were not independently established in the public documentation.

**Potential impact:**
- Unauthorized authentication.
- Unauthorized access to application accounts.
- Potential exposure of sensitive application information.
- Potential access to patient information, depending on database permissions.

**Recommendations:**
- Use prepared statements and parameterized SQL queries.
- Avoid constructing SQL queries through direct string concatenation.
- Implement server-side input validation.
- Apply least-privilege database permissions.
- Implement secure authentication and session management.
- Monitor suspicious login activity.

<img width="1917" height="1197" alt="bypass using admin&#39;--" src="https://github.com/user-attachments/assets/6c889e1d-ded5-45b5-8534-ae28d73fb0bb" />


### 5. Exposed Database Backup Investigation

A backup-related resource was discovered during web application enumeration.

A HEAD request was performed to inspect the server response.

**Example command:**

```bash
curl -sI https://TARGET_DOMAIN/backup-path
```

The resource returned an HTTP 200 response during the investigation.

Project notes indicate that employee salary and shareholder information were identified during the backup investigation. However, an HTTP 200 response alone does not prove that a valid SQL database dump was publicly accessible. The response body and sanitized content evidence are required to establish the exact exposure.

**Potential impact:**
- Exposure of confidential organizational information.
- Disclosure of employee and shareholder records.
- Potential exposure of database credentials or other sensitive information.
- Increased risk of unauthorized access and data misuse.

**Recommendations:**
- Remove database backups from publicly accessible directories.
- Store backups outside the web root.
- Restrict access to backup resources.
- Encrypt backup files.
- Review web server access logs.
- Rotate credentials if any were exposed.
- Implement a secure backup management policy.

<img width="1897" height="1135" alt="robots result" src="https://github.com/user-attachments/assets/b3e1e6df-2dd8-4341-83d4-8775a39c338e" />

<img width="1918" height="827" alt="s1l database" src="https://github.com/user-attachments/assets/4ad4b085-5901-40c9-a6d5-c6c202ea8719" />


### 6. PDF Password Hash Extraction and Recovery

Encrypted PDF documents were analyzed as part of the assessment.

A password hash was extracted and tested using Hashcat with a dictionary-based attack.

**Tool:** Hashcat  
**Hash Mode:** 10500  
**Attack Mode:** 0 – Dictionary Attack  
**Wordlist:** RockYou

**Command used:**

```bash
hashcat -m 10500 -a 0 pdf_hash.txt /usr/share/wordlists/rockyou.txt
```

Hashcat successfully recovered one supplied PDF hash.

The recorded result included:

- Status: Cracked
- Recovered: 1/1
- Dictionary: RockYou
- Hash mode: 10500

Earlier project notes recorded that two out of three PDF passwords had been recovered, with one remaining unresolved. Individual results should be verified against the original evidence before being treated as final.

**Security impact:**

Weak document passwords can reduce the effectiveness of encryption, especially when an attacker obtains an encrypted document and performs offline password-guessing attacks.

**Recommendations:**
- Use long, unique, randomly generated passwords.
- Avoid predictable passwords and dictionary words.
- Apply appropriate document encryption settings.
- Protect confidential documents and password distribution channels.
- Review access permissions for sensitive documents.

No original PDF files, password hashes, or recovered passwords are included in this repository.

PDF1

<img width="1917" height="1197" alt="pdf1 hash calc" src="https://github.com/user-attachments/assets/ff118e82-cfcf-4ddb-9e10-8a87702f2a7e" />

<img width="1917" height="1171" alt="pdf1 crack" src="https://github.com/user-attachments/assets/4dcbed41-4746-4075-9389-bc793837f7c8" />

PDF2

<img width="1902" height="1187" alt="pdf2 hash calc" src="https://github.com/user-attachments/assets/eaeca5a3-0b58-41b4-8db8-33c8eddb21e7" />

<img width="1907" height="1117" alt="pdf2 crack" src="https://github.com/user-attachments/assets/919cbb74-d1d1-47b2-be86-fc732e8d6208" />

PDF3

<img width="1917" height="1177" alt="pdf3 hash calc" src="https://github.com/user-attachments/assets/e7cc01e1-864d-4e3d-9edf-9d56c604e752" />

<img width="1911" height="1122" alt="pdf3 crack" src="https://github.com/user-attachments/assets/268d7f40-9eb5-4d64-bc53-c9ab49c5d101" />


---

## Key Security Findings

| Finding | Description | Verification Status |
|---|---|---|
| SQL Injection | SQL injection authentication bypass behavior was reported in the patient login functionality. | Requires supporting request/response evidence |
| Exposed Backup | A backup-related resource returned HTTP 200 to a HEAD request. | Backup contents require verification |
| Sensitive Information Exposure | Project notes report employee salary and shareholder information in backup data. | Requires sanitized content evidence |
| Weak PDF Password | Hashcat successfully recovered one supplied encrypted PDF hash. | Confirmed for one hash |
| Additional PDF Passwords | Earlier project notes recorded two out of three PDF passwords recovered. | Individual evidence requires verification |

---

## Security Recommendations

Based on the assessment activities, the following security improvements are recommended:

1. **Secure Database Queries:** Implement prepared statements and parameterized queries to prevent SQL injection.
2. **Improve Authentication:** Apply secure authentication controls, session management, and account protection.
3. **Protect Backup Files:** Store database backups outside publicly accessible directories.
4. **Encrypt Sensitive Data:** Use appropriate encryption for confidential files and database backups.
5. **Strengthen Password Policies:** Require strong, unique passwords for sensitive documents and accounts.
6. **Apply Access Controls:** Restrict access to patient, employee, and organizational information.
7. **Monitor Web Traffic:** Review web server and authentication logs for suspicious activity.
8. **Perform Regular Assessments:** Conduct periodic vulnerability assessments and penetration tests.

---

## Skills Demonstrated

Through this project, I gained practical experience in:

- Network reconnaissance and service enumeration.
- Web application security testing.
- Authentication security analysis.
- SQL injection investigation.
- Sensitive information exposure assessment.
- Backup exposure investigation.
- PDF hash extraction and password recovery.
- Dictionary-based password attacks.
- Kali Linux command-line operations.
- Nmap network scanning.
- cURL HTTP request analysis.
- Hashcat password recovery.
- Vulnerability documentation and reporting.
- Security risk assessment and remediation recommendations.

---

## Project Outcome

The assessment provided practical experience in identifying and investigating security weaknesses across a hospital web application and its associated resources.

The project covered multiple areas of cybersecurity, including network security, web application security, authentication analysis, sensitive information exposure, and password security.

It also strengthened my practical understanding of penetration testing tools, vulnerability analysis, technical documentation, and security remediation.

---

## Disclaimer

This project is documented for educational and professional portfolio purposes.

Testing was conducted under the authorization and scope of the relevant training project.

The public documentation has been sanitized. Target-identifying information, patient records, confidential business information, database contents, credentials, session tokens, password hashes, and recovered passwords have been excluded.

The findings should be interpreted according to their stated verification status. No unauthorized access or disclosure of confidential information is intended through this repository.
