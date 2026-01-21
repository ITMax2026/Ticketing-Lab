Project Overview:  In this project, I managed the end-to-end lifecycle of a Win32 application deployment using Microsoft Intune. The project is split into two phases: the Service Request (packaging and deployment) and the Incident Response (troubleshooting using client-side logs).

Skills Demonstrated:
  Win32 Application Packaging (IntuneWinAppUtil.exe)
  Log Analysis (AppWorkload.log)
  Root Cause Analysis (RCA) for Application not detected after installation
  ITSM Documentation (Jira-based workflow)

Part 1: Service Request BI-4: Package and Deploy 7-Zip 25.01 to Pilot Users

Business Case: The organization needs a standardized, silently deployed file compression tool for the Windows 11 Pilot User to replace legacy unzipping methods.

Technical Execution:
  1. Application Wrapping: Utilized the IntuneWinAppUtil.exe tool to wrap the 7-Zip MSI installer into a .intunewin format.
![Autopilot](images/autopilot-device.png)
[INSERT SCREENSHOT: Intune Portal showing "Failed" with Error 0x87D1041C]


  2. Deployment Configuration: Install Command: msiexec /i "7zip.msi" /qn (Silent, no-restart switch).
  4. Assignment: Targeted Pilot Users Group as a test required deployment.

![Autopilot](images/autopilot-device.png)


Part 2: Incident Remediation (BI-5): Detection Rule failure

Incident Report: Despite the software being physically present on the device, the Microsoft Intune Admin Center reported a "Failed" status for the Pilot User device.

![Autopilot](images/autopilot-device.png)

Root Cause Analysis (RCA):
  To validate the portal error, I performed troubleshooting on the client-side agent.
      1. Log Discovery: Navigated to the Intune Management Extension (IME) log directory: C:\ProgramData\Microsoft\IntuneManagementExtension\Logs.
        - Did searches to query AppWorkload.log for detection rule failures.
      2. The Finding: The logs revealed that the agent was searching for a non-existent file (7zip-troubleshoot.exe) created during a misconfiguration in the detection rule phase.

    [Win32App] Path doesn't exists: C:\Program Files\7-Zip\7zip-troubleshoot.exe applicationDetected: False
    [Win32App] detection manager: application not discovered after installation.
  
![Autopilot](images/autopilot-device.png)

Resolution:
  Policy Correction: Updated the Intune Detection Rule logic to target the correct vendor executable: 7z.exe.
  Verification: Triggered a manual MDM sync on the test device.
  Final Validation: Analyzed AppWorkload.log to confirm the transition from Error to Success.

[INSERT SCREENSHOT: PowerShell log results showing "applicationDetected: True"]
[INSERT SCREENSHOT: Intune Portal showing "Installed" (Green)]

[INSERT SCREENSHOT: PIc of jira ticket resolved

