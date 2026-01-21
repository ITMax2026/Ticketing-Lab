Goal: By using a Conditional Access policy, I configured a basic cloud tenant, Pilot User, from "All-or-Nothing" Security Defaults to a granular Zero-Trust Conditional Access (CA) framework. The goal was to secure the Office 365 by enforcing Multi-Factor Authentication (MFA) while maintaining administrative access through emergency exclusions.

The lab is structured in two parts:
  The Engineering Task: Implementing the security baseline.
  The Reactive Incident: Performing Root Cause Analysis (RCA) on a policy conflict.

Tools & Technologies
Microsoft Entra ID (Conditional Access, Sign-in Logs)
Microsoft Authenticator (OATH Verification)
Jira Service Management (ITSM / Documentation)
Office 365 (Exchange Online)


Part 1: The Engineering Task

Ticket [BI-6]:Implement Conditional Access MFA for Office 365 (Pilot User)
  - Business Goal: Enhance tenant security by enforcing MFA for basis user logins to Office 365

[Jira Screenshot of CA001 Success Log and Authenticator Prompt here]

Implementation:  
  1. Baseline Validation: Confirmed that the Pilot User could access outlook.office.com with only a password to ensure no legacy policies or Security Defaults were interfering with the test.
  2. Conditional Access Policy Architecture:
    Policy Name: CA001 – Require MFA for Office 365 (Pilot)
    Assignments: Scoped to the Pilot User group.
    Target Resource: Office 365 (Suite of apps including Outlook and Teams).
    Grant Control: Required Authentication Strength (MFA) via Microsoft Authenticator.
    Critical Exclusion: Excluded the Global Admin account to adhere to "Break-Glass" best practices and prevent accidental tenant lockout.


[Jira Screenshot of CA001 Success Log and Authenticator Prompt here]

Part 2: Incident Management

Ticket [BI-8]: A high-priority ticket, stating the Pilot User received a "You don't have access to this" error message when attempting to sign in to their email, despite providing the correct password.

[Jira Screenshot of Initial Incident]

Root Cause Analysis (RCA)
  Sign-In Log Analysis Check: Identified a "Failure" status for the user's recent sign-in attempts  (Error Code 53003).
[Screenshot of side bar  Log and Authenticator Prompt here]

  Policy Inspection: By reviewing the Conditional Access tab within the log entry, I identified a conflicting policy: CA999: EMERGENCY BLOCK - PILOT USER.
[Jira Screenshot of  Log and Authenticator Prompt here]

  Logic Conflict: In Entra ID, Block controls always take precedence over Grant controls. Even though the user satisfied the MFA requirement for CA001, the explicit block in CA999 overrode the access.

Resolution & Verification
  Resolution: Disabled CA999: EMERGENCY BLOCK - PILOT USER, the conflicting policy.
  Recovery: Confirmed the user successfully regained access to Office 365 after passing the CA001 MFA challenge.

