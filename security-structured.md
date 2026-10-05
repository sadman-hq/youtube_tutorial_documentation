# Security — Structured Notes

## Table of Contents
1. [Introduction to Security](#1-introduction-to-security)
2. [The Five Properties of Secure Software](#2-the-five-properties-of-secure-software)
3. [Popular Cyber Attacks](#3-popular-cyber-attacks)
4. [Malware](#4-malware)
5. [Password Attacks](#5-password-attacks)
6. [SQL Injection](#6-sql-injection)
7. [Zero-Day Attacks](#7-zero-day-attacks)
8. [Common Security Misconceptions](#8-common-security-misconceptions)
9. [Good Security Practices](#9-good-security-practices)
10. [Security Threats and Their Targets](#10-security-threats-and-their-targets)
11. [Defense in Depth](#11-defense-in-depth)
12. [Security vs. Usability](#12-security-vs-usability)
13. [Secure Software Development Lifecycle](#13-secure-software-development-lifecycle)
14. [Key Takeaways](#14-key-takeaways)

---

## 1. Introduction to Security

Security is a fundamental requirement in software and information systems. An improperly secured system exposes users, data, and services to unauthorized access, manipulation, disruption, and other malicious activity.

**Primary goal:** protect system resources and information from unauthorized access or misuse, while keeping them available to legitimate users.

**Assets a secure system should protect:**
- User accounts and credentials
- Personal and confidential information
- Academic records
- Financial information
- Databases
- Application functionality
- Communication channels
- System resources

Security covers the entire information lifecycle — authentication, communication, storage, processing, and access — not just keeping intruders out.

---

## 2. The Five Properties of Secure Software

| Property | Main Question | Main Goal |
|---|---|---|
| **Confidentiality** | Who can see the data? | Prevent unauthorized disclosure |
| **Integrity** | Who can modify the data? | Prevent unauthorized modification |
| **Authentication** | Who are you? | Verify identity |
| **Non-repudiation** | Who performed the action? | Establish accountability |
| **Availability** | Can authorized users access it? | Keep services accessible |

A secure system generally needs all five working together:

```
Authentication → Verify identity
       ↓
Authorization → Determine permitted actions
       ↓
Confidentiality → Protect private information
       ↓
Integrity → Prevent unauthorized modification
       ↓
Non-repudiation → Record important actions
       ↓
Availability → Keep the system accessible
```

### 2.1 Confidentiality
Ensures an asset is viewed only by authorized parties.
- **Objective:** prevent unauthorized disclosure of information
- **Examples:** exam assignments hidden before the exam; a student's GPA visible only to them; personal info not exposed to other students
- **Techniques:** authentication, authorization/access control, encryption, secure communication protocols, password protection, data classification, proper database permissions
- **Confidentiality vs. Privacy:** privacy concerns individuals and their personal information; confidentiality concerns preventing unauthorized disclosure of protected data generally

### 2.2 Integrity
Ensures an asset is modified only by authorized parties.
- **Objective:** prevent unauthorized modification of information
- **Examples:** students can't edit a submitted assignment or change their own grades; only instructors/authorized systems modify grades
- **Techniques:** access control, authorization, hashing, digital signatures, database constraints, audit logs, input validation, file integrity monitoring, version control

### 2.3 Authentication
The process of verifying the identity of an individual or entity.
- **Core question:** "Who are you?"
- **Authentication factors:**

| Factor | Examples |
|---|---|
| Something you know | Password, PIN, security question |
| Something you have | Mobile phone, security token, smart card, auth device |
| Something you are | Fingerprint, face recognition, iris recognition |

Combining multiple factors gives **multi-factor authentication (MFA)**, which is generally stronger than a password alone.

- **Authentication vs. Authorization:** authentication asks *"who are you?"*; authorization asks *"what are you allowed to do?"* (e.g., login credentials = authentication; permission to edit grades = authorization)

### 2.4 Non-Repudiation
Provides evidence that a party performed an action, making it hard for them to deny it.
- **Example:** a late assignment submission is tied to student identity, timestamp, submitted file, and system logs
- **Tension with privacy:** non-repudiation is effectively the opposite of complete anonymity — systems must balance when actions should be traceable vs. when privacy should be preserved
- **Supporting technologies:** digital signatures, audit logs, timestamps, cryptographic evidence, transaction records

### 2.5 Availability
Authorized users must be able to access and use a system/asset when needed.
- **Objective:** ensure authorized users can access resources when needed
- **Examples:** banking systems stay accessible; a university portal is available during registration; cloud services respond to legitimate requests
- **Key threat — Denial-of-Service (DoS):** overwhelms or disrupts a service to make it unavailable. **DDoS** extends this using many compromised systems.
- **Techniques:** redundant servers, load balancing, backups, failover systems, monitoring, disaster recovery, DDoS protection, resource management

---

## 3. Popular Cyber Attacks

Four major categories:
1. Malware
2. Password Attacks
3. SQL Injection
4. Zero-Day Attacks

---

## 4. Malware

**Malware** (malicious software) is unwanted/harmful software installed without consent — it can steal information, damage files, monitor users, disrupt services, gain unauthorized access, encrypt data, or install further malware.

| Type | Description | Protection |
|---|---|---|
| **Ransomware** | Holds victim data hostage until ransom is paid.<br>`Malware enters → Files encrypted → Access lost → Ransom demanded → Payment requested` | Regular backups, security updates, endpoint protection, email filtering, least-privilege access, offline/isolated backups, user awareness |
| **Spyware** | Collects confidential/sensitive info — browsing activity, credentials, personal info, behavior, system activity. Threatens confidentiality. | — |
| **Trojan Horse** | Appears legitimate but hides malicious functionality.<br>`Looks useful → User installs → Malicious code executes → Attacker gains access` | Key trait: deception |
| **Logic Bomb** | Inactive until a condition/time is met.<br>`Installed → Dormant → Trigger event → Activates → Malicious action` | Trigger can be time-, event-, or condition-based |

---

## 5. Password Attacks

Attack types: brute-force/dictionary, phishing, man-in-the-middle, credential stuffing, keyloggers, social engineering.

| Attack | Description | Protection |
|---|---|---|
| **Brute-Force / Dictionary** | Systematically tries passwords (brute-force) or common password lists (dictionary) | Long passphrases, unique passwords, password managers, account lockout/rate limiting, MFA, strong password policies |
| **Phishing** | Fraudulent, convincing messages trick users into revealing info (e.g., fake "verify your password" emails) | Verify sender, check URLs, avoid suspicious links/attachments, MFA, report suspicious messages |
| **Man-in-the-Middle (MITM)** | Attacker intercepts communication: `User ↔ Attacker ↔ Server` instead of `User ↔ Server` directly; can intercept/capture/monitor/modify data | HTTPS/TLS, secure Wi-Fi, certificate validation, VPNs, avoid untrusted networks |
| **Credential Stuffing** | Reuses compromised username/password pairs from one breach against other sites, exploiting password reuse | Unique passwords per service, password managers, MFA, monitor suspicious logins |
| **Keyloggers** | Malicious software/hardware recording keystrokes (usernames, passwords, messages, search queries, card info) | Keep systems updated, trusted security software, avoid suspicious software, MFA, monitor unusual behavior |
| **Social Engineering** | Manipulates people rather than systems — e.g., impersonating IT support, a manager, or a bank employee to extract info | Technical controls alone aren't enough; user awareness & training are essential |

---

## 6. SQL Injection

A code-injection attack where malicious SQL is inserted into application input and executed by the database.

**How it occurs:**
1. App requests user input
2. App directly incorporates that input into an SQL statement
3. Attacker supplies SQL syntax instead of ordinary data
4. Database executes the resulting query

### Examples

**Always-true condition (`1=1`):**
```sql
-- Input: 105 OR 1=1
SELECT * FROM Users WHERE UserId = 105 OR 1=1;
```
Since `1=1` is always true, the condition matches every row.

**Always-true string comparison (`"="`):**
Input like `" or ""="` in username/password fields can make the query's condition evaluate true, returning all rows from the `Users` table.

> The core problem isn't the specific string — it's that **user-controlled data is being interpreted as SQL code.**

### Prevention
Use **SQL parameters** — values supplied separately from the SQL statement, treated as data rather than executable code:
```sql
SELECT * FROM Users WHERE UserId = @0;
```

| Without parameterization | With parameterization |
|---|---|
| SQL structure + user input → may be interpreted as SQL code | SQL structure + user-provided value → database treats value as data |

**Additional defenses:** input validation, parameterized queries, prepared statements, least-privilege database accounts, secure ORM usage, error handling that avoids exposing database details, security testing.

---

## 7. Zero-Day Attacks

A **zero-day vulnerability** is unknown to the developer or not yet adequately patched. Attackers may discover and exploit it before defenders have time to respond.

**Attack process:**
```
Vulnerability exists
      ↓
Attacker discovers it
      ↓
Developer may not yet know
      ↓
Exploit is developed
      ↓
Attack occurs
      ↓
Developer creates a patch
      ↓
Users install the update
```
Users ignoring recent updates contributes to continued exposure.

**Protection:** apply security updates promptly, monitor vulnerability disclosures, use endpoint/network protection, maintain vulnerability-management processes, reduce unnecessary attack surfaces, monitor suspicious behavior.

---

## 8. Common Security Misconceptions

Dangerous assumptions sometimes used to excuse inadequate security:

> "It's not exploitable." · "No one will do that!" · "Why would anyone do that?" · "We've never been attacked." · "We're secure, we use cryptography." · "We're secure, we use a firewall." · "We've reviewed the code, and there are no security bugs."

**Why these are dangerous:**
- A vulnerability can be exploitable even if undiscovered by the dev team
- Encryption alone doesn't secure the whole system
- A firewall doesn't eliminate application-level vulnerabilities
- Passing code review doesn't guarantee there are no security bugs
- Never having been attacked doesn't prove the system is secure

> **Security should rest on systematic risk assessment and defensive engineering — not assumptions about attacker behavior.**

---

## 9. Good Security Practices

| Practice | Notes |
|---|---|
| **Field-length checking** | Enforce max input lengths (username, password, name, address) — enforced server-side, not just client-side |
| **Server-side input validation** | Client-side validation aids usability but can be bypassed; server must treat all input as untrusted until validated |
| **Prevent SQL injection** | Never concatenate untrusted input into SQL; use prepared statements, parameterized queries, secure DB APIs, input validation |
| **Choice of programming language** | Higher-level languages (Java, C#) reduce exposure to low-level memory bugs; lower-level languages (C, C++) are more exposed to buffer overflows if memory is handled unsafely |
| **Buffer overflow awareness** | Writing more data than a buffer holds can overwrite adjacent memory. Mitigate via memory-safe languages, bounds checking, secure coding, compiler protections, static analysis, security testing |
| **Use authentic application sources** | Install only from official app stores, vendor sites, or trusted repositories |
| **Cryptography** | AES and DES are **symmetric-key** algorithms (not public-key). AES is modern and secure; DES is outdated and insecure (too small a key size). Use current, well-reviewed standards. Encryption: `Plaintext → Encryption → Ciphertext`, reversible only with the proper key — protects confidentiality in storage/transit |
| **Password security** | Encourage long passwords, block commonly compromised passwords, avoid reuse, use MFA, store salted hashes (Argon2, bcrypt, scrypt — never plaintext), apply login rate limiting |
| **Secure authentication channels** | Perform authentication over encrypted channels (HTTPS/TLS) — not `User → Password → Server` unprotected, but `User → HTTPS/TLS → Server`, protecting confidentiality and integrity of communication |
| **Security testing** | Vulnerability scanning, SAST, DAST, penetration testing, dependency scanning, auth testing, input-validation testing, SQL-injection testing, security code review — integrated throughout the SDLC, not just before deployment |

---

## 10. Security Threats and Their Targets

Different attacks affect different security properties — illustrating why no single mechanism is sufficient:

| Attack | Confidentiality | Integrity | Availability | Authentication |
|---|:---:|:---:|:---:|:---:|
| Malware | ✓ | ✓ | ✓ | ✓ |
| Password Attack | ✓ | ✓ | — | ✓ |
| SQL Injection | ✓ | ✓ | ✓ | ✓ |
| MITM | ✓ | ✓ | — | ✓ |
| Ransomware | — | ✓ | ✓ | — |
| Phishing | ✓ | — | — | ✓ |
| DoS | — | — | ✓ | — |
| Zero-Day Exploit | ✓ | ✓ | ✓ | ✓ |

---

## 11. Defense in Depth

Rather than relying on one mechanism, multiple independent layers should protect the system:

```
User Awareness
      ↓
Strong Authentication
      ↓
Authorization
      ↓
Input Validation
      ↓
Secure Application Code
      ↓
Database Security
      ↓
Network Security
      ↓
Monitoring & Logging
      ↓
Backups & Recovery
```

If one layer fails, others reduce the impact — e.g., even if a password is phished, MFA may still prevent unauthorized access.

---

## 12. Security vs. Usability

Overly complex security controls can backfire, causing users to reuse passwords, disable security features, store credentials insecurely, or ignore warnings.

**Goal:** `Strong Security + Reasonable Usability → Effective Protection`

Security is most effective when users can realistically follow the required practices.

---

## 13. Secure Software Development Lifecycle

```
Security Requirements
        ↓
Secure Design
        ↓
Secure Implementation
        ↓
Input Validation
        ↓
Authentication & Authorization
        ↓
Secure Communication
        ↓
Security Testing
        ↓
Deployment
        ↓
Monitoring & Updates
```

Security should be treated as an ongoing process, not a single feature bolted on at the end of development.

---

## 14. Key Takeaways

### Secure Software Properties
- **Confidentiality** — protects information from unauthorized disclosure
- **Integrity** — protects information from unauthorized modification
- **Authentication** — verifies identity
- **Non-repudiation** — provides accountability
- **Availability** — ensures access when needed

### Major Cyber Attacks
Malware, Ransomware, Spyware, Trojan horses, Logic bombs, Brute-force attacks, Phishing, MITM attacks, Credential stuffing, Keyloggers, Social engineering, SQL injection, Zero-day attacks

### Good Security Practices
- Validate input on the server
- Check input lengths
- Use parameterized SQL queries
- Protect credentials
- Use strong authentication
- Encrypt sensitive communications
- Use modern cryptographic algorithms
- Keep software updated
- Obtain applications from trusted sources
- Perform security testing
- Avoid assumptions like "we've never been attacked"
- Use multiple layers of security

**Bottom line:** security is the combination of confidentiality, integrity, authentication, accountability, and availability — sustained by secure development practices and continuous defense against evolving attacks such as malware, password attacks, SQL injection, and zero-day exploits.
