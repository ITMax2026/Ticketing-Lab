Goal: Implement a zero-touch (or low-touch) deployment strategy using Windows Autopilot to provision a corporate device
Solution: Utilized Microsoft Intune and Entra ID to configure the backend so that when a user connects their device it is compliant, productive in minutes via Out-of-Box Experience (OOBE)



Part 1: Incident Response (Autopilot Remediation)
   1. Problem Identification
      -  While provisioning a device (VM-01) the automated deployment failed - instead of tenant login screen, the device presented a generic consumer Windows Out-of-Box Experience (OOBE)
        - Symptom: Device prompted for Region selection and keyboard layout

![Ticket BI-10 documenting the failure symptoms](./images/autopilot-request.png)

  2. Technical fix
     I used Jira internal notes to document the remediations steps.  This serves as a Knowledge Base entry for the team
     - Action Taken: Interrupted OOBE via Shift + F10 to access the WinPE Command Line.
     - PowerShell Execution: Bypassed execution policy and ran Get-WindowsAutopilotInfo to harvest the hardware hash.
     - Engineering Workaround: "Used Edge browser via start msedge to authenticate the Intune portal and upload the hardware hash manually."

![Ticket BI-10 initial response](./images/autopilot-inital.png)

![Ticket BI-10 Technical Fix](./images/autopilot-solve.png)


  3. Resolution
    - The incident was closed once the device successfully recognized the tenant on reboot.
    - By documenting the start msedge workaround, I provided the team with a faster alternative to troubleshooting virtual drive mounting in the future.

![Ticket BI-10 Device Recognized](./images/autopilot-device.png)

![Ticket BI-10 Ticket Resolution](./images/autopilot-resolve.png)



Part 2: Modern Autopilot Workflow
    1. Service Request:
      - Request Type: New Hire Device Provisioning
![Ticket - New Hire Request](./images/new hire- request.png)

    2. Technical Configuration
      1. Hardware Hash Registration: Harvested and Registered the Hardware Hash into Autopilot Devices
![Ticket - Hardware Hash](./images/autopilot-device.png)

    3. User Experience (Out-of-Box Experience)
      Goal is to allow for an employee to receive a new laptop and reach a productive state effectively and securely
      1. Identity and Security: 
        - Upon boot, the device recognized the Intune/Autopilot tenant.  
        - The user was prompted for the corporate credentials and was required to verify an MFA prompt via Micsofot Authenticator App
        - Windows Hello for Busines (WHfB):  User created a mandatory PIN setup, tied to local TPM encrypted storage and replacing insecure passwords

![Ticket - Correct Login](./images/login-wh.png)

![Ticket - Correct Login](./images/login-mfa.png)


    4. Verification of Management and Compliance
      Management Verification: Verfiied via the local 'Access Work or School' setting that the device is connected to the MaxTicketLab Entra ID

![Ticket - Correct Login](./images/management-verification.png)

      Compliance Check: In the Intune portal, verified that device status return a 'Compliant' flag.

![Ticket - Correct Login](./images/intune-compliance.png)


![Ticket - Resolution](./images/new hire-solution.png)

