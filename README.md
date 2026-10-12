# AWS Cloud Security Audit & Remediation — Prowler & CIS AWS Foundations Benchmark

## 🧭 Overview

This project demonstrates a complete cloud security auditing and remediation lifecycle using Prowler against a personally controlled Amazon Web Services (AWS) environment.

The project follows a realistic workflow:

**Assess → Analyze → Prioritize → Remediate → Rescan → Validate**

The goal was not to achieve 100% compliance across the AWS environment, but to identify security configuration weaknesses, remediate selected findings, and verify the effectiveness of those changes through a follow-up CIS compliance assessment.

---

## 🧪 Lab Environment

### Cloud Platform
- Amazon Web Services (AWS)

### Security Assessment Tool
- Prowler
- CIS AWS Foundations Benchmark v3.0

### Assessment System
- Kali Linux
- Python 3.13 virtual environment
- AWS CLI

### AWS Services
- AWS Identity and Access Management (IAM)
- Amazon S3
- AWS Budgets

---

## 🎯 Objectives

- Configure a secure AWS cloud security lab
- Enable MFA for the AWS root account
- Configure a Zero-Spend Budget for cost monitoring
- Create an IAM user for programmatic security assessment
- Configure AWS CLI authentication
- Install and verify Prowler
- Perform an initial CIS AWS Foundations Benchmark assessment
- Analyze failed cloud security controls
- Remediate selected IAM and S3 security weaknesses
- Perform a post-remediation Prowler assessment
- Compare before and after results
- Validate that selected failed controls changed to PASS

---

## ⚙️ Phase 1 — AWS Security Setup

The AWS environment was prepared before beginning the security assessment.

Security controls configured during the initial setup included:

- Root account MFA
- AWS Zero-Spend Budget
- Dedicated IAM user for programmatic assessment
- AWS CLI authentication

- The AWS CLI identity was verified using:
  aws sts get-caller-identity

The command confirmed that the assessment was being performed using the dedicated nour-admin IAM user.

A Zero-Spend Budget was also configured to provide notification if unexpected AWS charges occurred during the lab.

## 🔧 Phase 2 — Prowler Installation & Configuration
Prowler was installed on Kali Linux inside a dedicated Python 3.13 virtual environment.
The virtual environment was activated using:
source ~/prowler-venv313/bin/activate

Prowler installation was verified using:
prowler --version

The installed version used during the assessment was:
Prowler 5.44.0

AWS CLI credentials were configured using:
aws configure

This allowed Prowler to securely assess the AWS environment using the configured IAM credentials.

## 🧩 Phase 3 — Controlled Security Misconfigurations
Controlled cloud security weaknesses were introduced to create a realistic assessment and remediation scenario.

The lab included the following intentional security weaknesses:
- Disabled S3 Block Public Access protections
- An overly permissive IAM policy
- Weak AWS account password-policy settings

A dedicated S3 test bucket was created using only harmless lab data.

The bucket was configured with public-access protections disabled to demonstrate an insecure cloud storage configuration.

An intentionally overly permissive customer-managed IAM policy was also created with unrestricted actions and resources.

The account password policy was initially left in its default weaker configuration so that Prowler could evaluate the relevant CIS controls.

## 🔎 Phase 4 — Initial Prowler CIS Assessment
The initial AWS security assessment was performed using:
prowler aws --compliance cis_3.0_aws

Prowler assessed the AWS environment against the CIS AWS Foundations Benchmark v3.0.

The baseline assessment identified:
- 50 total findings
- 22 passed controls
- 28 failed controls
- 44% pass rate
- 56% fail rate

The initial assessment established the AWS account's security and compliance posture before remediation.

## 📊 Phase 5 — Security Findings & Analysis
The Prowler report was reviewed to identify security controls requiring remediation.

Findings were analyzed based on:
- CIS benchmark requirements
- Security severity
- IAM configuration
- Cloud storage exposure
- Password-policy strength
- Risk reduction potential
- Practical remediation within the lab environment

Two confirmed IAM password-policy findings included:
- Password reuse prevention was not configured for the previous 24 passwords
- Minimum password length was below 14 characters

- These controls were reported as:
  FAIL

The report also contained additional IAM, storage, logging, monitoring, and account-security findings.

The selected remediation activities focused on:
- S3 public-access protections
- Overly permissive IAM permissions
- IAM password-policy controls

## 🛠️ Phase 6 — Remediation

- Selected AWS security weaknesses were remediated.
  Amazon S3

Block Public Access protections were re-enabled for the test S3 bucket.

- This restored the recommended protection against unintended public access to the bucket and its objects.
  IAM Policy

The intentionally created overly permissive IAM policy was deleted from the AWS environment.

- This removed the unnecessary unrestricted permissions created for the lab scenario.
  IAM Password Policy

- The account password policy was strengthened using the AWS CLI.

- The following command was used:
  aws iam update-account-password-policy \
    --minimum-password-length 14 \
    --require-symbols \
    --require-numbers \
    --require-uppercase-characters \
    --require-lowercase-characters \
    --password-reuse-prevention 24

- The updated policy was verified using:
  aws iam get-account-password-policy

- The validation confirmed:
  - Minimum password length: 14
  - Symbols required
  - Numbers required
  - Uppercase characters required
  - Lowercase characters required
  - Password reuse prevention: 24 previous passwords

## 🔁 Phase 7 — Post-Remediation Scan

After remediation was completed, the same Prowler CIS assessment was performed again:

```bash
prowler aws --compliance cis_3.0_aws

The follow-up assessment showed measurable improvement.
Metric	Before	After
Total Findings	50	50
Passed Controls	22	25
Failed Controls	28	25
Pass Rate	44%	50%
Fail Rate	56%	50%


The results demonstrated that the remediation actions successfully improved the AWS environment's security posture.
Three controls changed from FAIL to PASS between the initial and post-remediation assessments.

## ✅ Phase 8 — Post-Remediation Validation
The post-remediation Prowler report was manually searched for password-related controls.
The same IAM password-policy findings that previously showed:
FAIL

were now reported as:
PASS

Validated controls included:
- Password reuse prevention for the previous 24 passwords
- Minimum password length of at least 14 characters
This confirmed that the selected IAM password-policy remediation was successfully implemented and independently validated through a second Prowler assessment.
This final validation step ensured that remediation was not only applied, but also verified through an automated compliance rescan.

## 📉 Before vs After Results
The cloud security remediation process produced the following measurable improvements:
Security Metric	Initial Assessment	Post-Remediation Assessment	Improvement
Total Findings	50	50	—
Passed Controls	22	25	+3
Failed Controls	28	25	-3
Pass Rate	44%	50%	+6%
Fail Rate	56%	50%	-6%
Password Length Control	FAIL	PASS	Remediated
Password Reuse Control	FAIL	PASS	Remediated


These results demonstrate measurable improvement in the AWS environment's security and compliance posture.

## 🔄 Cloud Security Assessment Lifecycle
Initial Assessment
     ↓
Analyze Findings
     ↓
Prioritize Risk
     ↓
Remediate
     ↓
Rescan
     ↓
Validate Remediation
This workflow reflects the core process used in real-world cloud security and compliance programs.
The process does not end after identifying cloud security weaknesses. Effective cloud security requires remediation, rescanning, and validation to confirm that security improvements were actually achieved.

## 🧰 Tools & Technologies
- Amazon Web Services (AWS)
- Prowler
- CIS AWS Foundations Benchmark v3.0
- AWS Identity and Access Management (IAM)
- Amazon S3
- AWS CLI
- AWS Budgets
- Kali Linux
- Python
- Cloud Security
- Security Auditing
- Security Compliance
- Cloud Misconfiguration Assessment
- Security Remediation

## 🖼️ Screenshots
- 1️⃣ AWS Billing Safety Net: Configured an AWS Zero-Spend Budget to monitor account usage and provide alerts if unexpected charges occur.
- 2️⃣ Prowler CIS AWS Scan in Progress: Ran Prowler against the AWS environment to assess security configuration and compliance against the CIS AWS Foundations Benchmark v3.0.
- 3️⃣ Initial Prowler CIS Compliance Scan: Baseline assessment before remediation showing 22 passed controls and 28 failed controls.
- 4️⃣ Specific AWS Security Findings Before Remediation: Prowler identified failed IAM password-policy controls, including insufficient minimum password length and missing password reuse prevention.
- 5️⃣ AWS Security Remediation Validation: Validated the strengthened AWS IAM password policy through the AWS CLI, confirming a 14-character minimum length, complexity requirements, and prevention of reuse of the previous 24 passwords.
- 6️⃣ Post-Remediation Prowler CIS Scan: The follow-up assessment showed improved security posture, with 25 passed controls and 25 failed controls.
- 7️⃣ Post-Remediation Validation: Final validation confirmed that previously failed IAM password-policy controls changed to PASS after remediation.

## 🧠 Skills Demonstrated
- Cloud Security
- Amazon Web Services (AWS)
- AWS Security
- Security Auditing
- CIS Benchmark Assessment
- Security Compliance
- Identity and Access Management (IAM)
- Amazon S3 Security
- AWS CLI
- Prowler
- Cloud Misconfiguration Assessment
- Security Remediation
- Security Validation
- Governance, Risk Management, and Compliance (GRC)
- Continuous Compliance
- Linux
- Technical Documentation

## 🏁 Key Takeaway
This project demonstrated that cloud security auditing is more than simply identifying misconfigurations.

A complete cloud security assessment process requires:
- Establishing a secure assessment environment
- Identifying cloud security weaknesses
- Analyzing failed security controls
- Prioritizing remediation
- Applying corrective security controls
- Performing a follow-up compliance assessment
- Validating that targeted controls have actually improved

The most important outcome of this project was demonstrating measurable improvement between the initial and post-remediation assessments.

The AWS environment improved from:
22 PASS / 28 FAIL
to:
25 PASS / 25 FAIL

The project also demonstrated that previously failed IAM password-policy controls successfully changed to PASS after remediation.

## ⚠️ Disclaimer
This project was conducted exclusively within a personally controlled AWS environment for educational, cybersecurity training, and portfolio purposes only.

Controlled security misconfigurations were intentionally introduced within the lab environment and subsequently remediated.
No third-party AWS accounts, production systems, or unauthorized cloud resources were assessed.
