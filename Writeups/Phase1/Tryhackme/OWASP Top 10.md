## Preamble
- `OWASP` means Open Worldwide Application Security Project and cares about global Softwaresecurity
- `IAAA` means Identity, Authentication, Authorization and Accountability
- `A01-A10` are about related failures in how IAAA was implemented

## A01 Broken Access Control
- Happens when the server does not properly enforce **who can access what** on every request.
- Common occurence is `IDOR`(Insecure Direct Object Reference): Change ID (e.g. id=6->id=7) lets the user see someone else's data 
- Horizontal privilege escalation (same role, other user's stuff)
- Vertical privilege escalation (jumping to admin-only actions)

**How to fix this vulnerability**
- Implement proper authentication and authorization checks
- Verify authenticated user has permission to access only the requested resource
- Indirect references or encrypted tokens instead of predictable IDs
- Log and monitor access attempts to sensitive resources


---
## A02 Security Misconfigurations

- **unsafe defaults, incomplete settings or exposed services** are mistakes of how the **environment, software or network** got set up
- Small misconfigurations can expose sensitive data or give an attacker a foothold

**Common Patterns**
- Default credentials or weak passwords left unchanged
- Unnecessary services or endpoints exposed to the internet
- Misconfigured cloud storage or permissions
- Unrestricted API access or missing authentication
- Verbose error messages exposing stack traces or system details
- Outdated software, frameworks, or containers with known vulnerabilities
- Exposed AI/ML endpoints without proper access controls

**How to prevent**
- Harden default config and remove unused features or services
- Enforce strong authentication and least privilege across all systems
- Limit network exposure and segment sensitive resources
- Keep software, frameworks, and containers up to date with patches
- Hide stack traces and system infos from error messages
- Audit cloud config and permissions regularly
- Secure AI endpoints and automation services with proper access controls and monitoring
- Integrate config reviews and automated security checks

---
## A03 Software Supply Chain Failures

- Applications rely on **components, libraries, services or models** that are **compromised, outdated or improperly verified**
- Modern applications are built from many third-party packages, APIs and AI models -> One misconfigured dependeny can give attackers access to the whole system

**Common Patterns**
- Using unverified or unmaintained libraries and dependencies
- Automatically installing updates without verification
- Over-reliance on third-party AI models without monitoring or auditing
- Insecure build pipelines or CI/CD processes that allow tampering
- Poor license or provenance tracking for components
- Lack of monitoring for vulnerabilities in dependencies after deployment

**How to prevent**
- Verify all third-party components, libraries, and AI models before use
- Monitor and patch dependencies regularly
- Sign, verify, and audit software updates and packages
- Lock down CI/CD pipelines and build processes to prevent tampering
- Track provenance and licensing for all dependencies
- Implement runtime monitoring for unusual behaviour from dependencies or AI components
- Integrate supply chain threat modelling into the SDLC(Software Dev. Life Cycle) , including testing, deployment, and update workflows

---
## A04 Cryptographic Failures

- Happens when encryption is used incorrectly or not at all.
- Includes weak algorithms, hard-coded keys, poor key handling or unencrypted sensitive data

**Common Patterns**
- Using deprecated or weak algorithms like MD5, SHA-1, or ECB mode
- Hard-coded secrets in code or configuration
- Poor key rotation or management practices
- Lack of encryption for sensitive data at rest or in transit
- Self-signed or invalid TLS certificates
- Using AI/ML systems without proper secret handling for model parameters or sensitive inputs

**How to prevent**
- Use strong, modern algorithms such as AES-GCM, ChaCha20-Poly1305, or enforce TLS 1.3 with valid certificates
- Use secure key management services like Azure Key Vault, AWS KMS, or HashiCorp Vault
- Rotate secrets and keys regularly, following defined crypto periods
- Document and enforce policies and standard operating procedures for key lifecycle management
- Maintain a complete inventory of certificates, keys, and their owners
- Ensure AI models and automation agents never expose unencrypted secrets or sensitive data

---
## A05 Injection 

- Occurss when an application takes user input and mishandles it
- e.g. SQL Injection with injecting SQL query into an applications logic
More examples:
  - Command Injection
  - AI Prompts
  - Server Side Template Injection (SSTI)
 
**How to prevent Injection**
- Always treat user input as untrusted
- Don't parse input directly, instead take elements of it for querying
- SQL - use prepared statements and parameterised queries instead of building queries through string concatenation
- OS - Avoid functions that pass input directly to the system shell, instead rely on safe APIs and processes that do not invoke the shell at all 


---
## A06 Insecure Design

- Occur when **flawed logic or architecture is built into a system**
- Unpatchable, since insecure design is built into workflow, logic and trust boundaries -> Now **AI is making a lot of decisions** 

**Common Patterns**
- Weak business logic controls, like recovery or approval flows
- Flawed assumptions about user or model behaviour
- AI components with unchecked authority or access
- Missing guardrails for LLMs and automation agents
- Test or debug bypasses left in production
- No consistent abuse-case review or AI threat modelling

Blind trust model outputs creates fragile systems that act on AI decisions without validation or oversight.

**How to design securely**
- Treat every model as untrusted until proven otherwise.
- Validate and filter all model inputs and outputs to ensure accuracy and integrity.
- Separate system prompts from user content.
- Keep sensitive data out of prompts unless absolutely needed and protect it with strict controls.
- Require human review for high-risk AI actions.
- Log model provenance, monitor behaviour, and apply differential privacy for sensitive data.
- Include AI-specific threat modelling for prompt attacks, inference risks, agent misuse, and supply chain compromise throughout the design process.
- Build threat modelling into every stage of development, not just at the start.
- Define clear security requirements for each feature before implementation.
- Apply the principle of least privilege across users, APIs, and services.
- Ensure proper authentication, authorisation, and session management across the system.
- Keep dependencies, third-party components, and supply chain sources verified and up to date.
- Continuously monitor and test the system for logic flaws, abuse paths, and emergent risks as new features or AI components are added.

---
## A07 Authentication Failures

- Occures when application can not reliably verify or bind a user's identity. Common issues:
  - Username enumeration
  - Weak/guessable passwords
  - logic flaws in login/registration flow
  - insecure session or cookie handling
- If any of the issues occure, an attacker can log in as someone else or bind a session to the wrong account

**Example**
- Register with the Username `aDmiN` and pw `test` -> i can login as `admin` using the pw

---
## A08 Software or Data Integrity Failures

- Occures when application relies on **code, updates or data assumed safe** without verifying their authenticity, integrity or origin.
- Includes trusted software, updates without verification, loading scripts or config files from untrusted sources

**How to avoid**
- Establish trust boundaries - Applications should never assume that code, updates or key pieces of data are legitimate or automatically trusted
- With help of cryptographic checks for update packages and ensuring that only trusted sources can modify critical artefacts 

---
## A09 Logging & Alerting Failures

- Insufficient logging leads to defenders being unable to detect or investigate attacks
- Good logging underpins the `Accountability` in `IAAA`


---
## A10 Mishandling of Exceptional Conditions

- Occures when programs fail to **prevent, detect and respond to unusual and unpredictable situations**, which leads to **crashes, unexpected behaviour and sometimes vulnerabilities**

**How to prevent**
-  Proper error handling 
- Global exception handler for unexpected cases or cases missed out
- Add limits to deny lack of application resilience, DoS, successful brute force attacks and extraordinary cloud bills
- Include strict input validation and centralized error handling, logging, monitoring and alerting

---
