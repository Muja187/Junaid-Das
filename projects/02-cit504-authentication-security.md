# 🔐 Authentication & Password Security for a Two-Office Business

**Course:** CIT 504, Introduction to Information Technology · Assessment 2

## Scenario
ABC Print Services is a publishing company with offices in Hamilton and Taupō. It uses Windows Domain Controller authentication for local resources and Microsoft 365 Single Sign-On (SSO) for cloud services. With a second office opening, I had to work out what secure authentication needs to look like across both sites.

## What I did
1. **Identified the client's security requirements**: why passwords matter, how to separate administrator accounts, and what a sound Windows password policy looks like.
2. **Applied a common security solution** by configuring and testing a password policy in a simulated, sandboxed Windows domain lab.
3. **Evaluated three emerging authentication technologies** against security, usability and scalability.
4. **Recommended a hybrid solution** with a risk analysis and an evaluation of how well it fits the business.

## Client security requirements

**Why passwords matter.** A password is the first barrier against unauthorised access. Compromised passwords expose an organisation to credential stuffing, phishing and brute-force attacks, which can lead to data loss and service disruption.

**Administrative account separation.** System administrators should have two accounts: one for routine daily work and one for privileged tasks. This follows the principle of least privilege. If malware runs in a session that has admin rights, it inherits those rights. For example, the Hamilton IT administrator would use a standard account for email and document work and switch to the admin account only to manage the domain or VPN configuration.

**Windows password policy baseline** used in the report:

| Setting | Value |
|---|---|
| Minimum length | 12–16 characters |
| Complexity | Uppercase, lowercase, numbers and symbols |
| Maximum age | 90 days |
| Minimum age | 1 day (prevents immediately cycling back to an old password) |
| History | 24 previous passwords remembered |
| Lockout | Account locked for 15 minutes after 5 failed attempts |

When the same credentials are used for Microsoft 365 SSO, these rules still apply and are supplemented by cloud-side protections such as smart lockout and risk-based sign-in policies.

## Hands-on lab: configuring the policy in a Windows domain
I built this in a sandboxed Windows Server 2022 practice lab (domain `PRACTICELABS.COM`) provided through ACI Learning.

### 1. Set the password policy in Group Policy
In the Default Domain Policy under *Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy*, I configured:

| Policy | Setting |
|---|---|
| Enforce password history | 24 passwords remembered |
| Maximum password age | 90 days |
| Minimum password age | 1 day |
| Minimum password length | 14 characters |
| Password must meet complexity requirements | Enabled |
| Store passwords using reversible encryption | Disabled |

### 2. Create separate user and admin accounts
In Active Directory Users and Computers I created two accounts: `Junaid-User` for everyday work and `Junaid-Admin` for administration.

### 3. Give only the admin account domain admin rights
I added `Junaid-Admin` to the **Domain Admins** group, so the privileged rights are kept apart from the everyday account.

### 4. Test that the policy is actually enforced
I logged in as the standard user from the workstation console and tried to change the password:

- **Weak password:** rejected with the message "Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain."
- **Compliant password** (at least 14 characters, upper and lower case, numbers, symbols, not used before): accepted, and the console confirmed "Your password has been changed."

**Result:** domain-wide password standards were enforced, and administrators had distinct privileged accounts.

## Emerging authentication technologies

| Technology | What it is | Strengths | Weaknesses and risks | Security | Usability | Scalability |
|---|---|---|---|---|---|---|
| **Multi-Factor Authentication (MFA)** | Two or more factors, such as a password plus a phone OTP, app approval or hardware token | Greatly reduces account compromise from stolen passwords; simple integration with Microsoft 365 and Azure AD | Setup can be difficult; needs a phone or token | Very high | Moderate (extra step) | High (cloud-scalable) |
| **Passwordless authentication** | Biometrics (e.g. Windows Hello) or FIDO2 hardware keys instead of passwords | Removes password reuse and phishing risk; fast sign-in | Needs hardware support and user training; device-loss risk | Very high | High | High |
| **Conditional Access (Azure AD)** | Grants access only when conditions are met, such as device compliance, location or IP | Fine-grained control over remote logins; works with VPN and MFA | Depends on internet access and accurate device management | High | Moderate | Medium–High |

### Comparison
- **Security:** MFA and passwordless sign-in cut exposure to credential theft and phishing. Conditional Access complements them by restricting where and when accounts can sign in.
- **Usability:** passwordless is the easiest once set up. MFA adds a step but is acceptable for the security benefit.
- **Scalability:** all three integrate with Microsoft 365 and extend to both offices without major infrastructure spend.

### Risks and mitigations
| Risk | Mitigation |
|---|---|
| Connectivity dependence (Conditional Access and MFA need reliable internet or VPN) | Keep offline login fallbacks for essential accounts |
| Staff need guidance to enrol MFA methods | Provide training materials in English and te reo Māori |
| Device loss or change (passwordless) | Register secondary devices or use backup codes |

## Recommendation: a hybrid authentication framework
1. **Multi-Factor Authentication** through Microsoft 365 and Azure AD
2. **Conditional Access policies** to control remote and mobile logins
3. **A fine-grained password policy** for local domain accounts

**Benefits for remote and mobile staff**
- Even if a password is stolen, attackers can't sign in without the second factor.
- Conditional Access can restrict logins by location, for example to New Zealand IP ranges or compliant, authorised devices.
- Taupō sales staff working from the road can approve sign-ins with the Microsoft Authenticator app.

**Cloud integration.** ABC Print Services already uses Microsoft 365, so Azure AD MFA and Conditional Access fit in naturally. The Windows Domain Controller can sync with Azure AD using Azure AD Connect, giving centralised credential management and single sign-on across cloud and on-premises resources. *(Microsoft has since renamed Azure AD to Microsoft Entra ID and Azure AD Connect to Microsoft Entra Connect.)*

### Risk analysis
| Risk | Mitigation |
|---|---|
| **Internet dependency**: if VPN connectivity fails, on-premises sign-in still works but cloud SSO may not | Cached credentials and redundant connectivity for critical staff |
| **Privacy**: biometric and device data must be stored securely and comply with the NZ Privacy Act 2020 | Encrypted storage and clear privacy policies |
| **Adoption resistance**: staff may struggle with new sign-in methods at first | Training workshops and help-desk support in both languages |

### Evaluation
| Criteria | Evaluation |
|---|---|
| Security | MFA and Conditional Access significantly reduce credential theft and phishing risk |
| Usability | Microsoft Authenticator and biometric options simplify sign-in for staff with limited technical skills or language barriers |
| Scalability | The cloud-based solution extends to additional offices without extra hardware cost |

The recommendation builds on Microsoft infrastructure ABC Print Services already has, avoids heavy capital spend, and balances security with user experience. It is also aligned with the New Zealand Information Security Manual (NZISM).

## Takeaways
- Strong password policy and separated admin accounts are the baseline, and MFA plus Conditional Access add layers on top.
- Testing is part of the job: rejecting a weak password and accepting a compliant one showed the Group Policy was really being enforced.
- Security has to be usable. Training, backup options and bilingual support matter as much as the technology.

**Tools / topics:** Active Directory Users and Computers · Group Policy Management · Windows Server 2022 · password policy · least privilege · MFA · passwordless (Windows Hello, FIDO2) · Conditional Access · Microsoft 365 / Entra ID · NZ Privacy Act 2020 · NZISM

---

[⬅ Back to my profile](../README.md)
