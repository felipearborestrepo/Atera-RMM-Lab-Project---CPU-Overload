# 🛠️ Atera RMM Lab Project (CPU Overload) — Complete Walkthrough

![ChatGPT Image Jun 21, 2025, 12_07_59 AM](https://github.com/user-attachments/assets/e5d305d5-7e99-415a-afec-d44be3fb16d8)

This project documents the setup of a full Remote Monitoring and Management (RMM) environment using **Atera** on a **Windows VM hosted in Microsoft Azure**.

You will:
- Install the Atera agent on a VM
- Configure custom alerts
- Simulate CPU overload using PowerShell
- Trigger and investigate the alert
- Create and close a support ticket
- Generate a professional report

---

## ✅ Step 1: Create and Configure the Azure VM

**Purpose:**  
Set up a remote endpoint for monitoring.

**Instructions:**
- Use Microsoft Azure to create a new Windows 10 Virtual Machine.
- Choose VM Size: `Standard DS1 v2` (1 vCPU, 3.5GB RAM).
- Make sure it's **Running** and has a public IP address.
 
![Screenshot 2025-06-20 221409](https://github.com/user-attachments/assets/78baf81c-900b-467c-b86b-0eb28bcb7c42)

---

## ✅ Step 2: Install the Atera Agent

**Purpose:**  
The Atera Agent sends telemetry data (CPU, memory, disk usage, alerts) to the Atera dashboard.

**Instructions:**
1. Go to **Atera > Devices > + New Device**.
2. Choose **Download Installer** and transfer the `.msi` file to your VM.
3. Run the installer inside the VM.
4. Go to `services.msc` in the VM to confirm the **AteraAgent** service is running.

**Screenshots:**  
![Screenshot 2025-06-20 220935](https://github.com/user-attachments/assets/ab670426-099c-446e-ab18-7c0d5b28e49b)

![Screenshot 2025-06-20 221854](https://github.com/user-attachments/assets/4b14ab79-955b-4c47-bc75-fe8b007abe7c)

![Capture1 (2)](https://github.com/user-attachments/assets/41f27865-918f-4f13-a1f2-e7881d42bab4)

---

## ✅ Step 3: Create Alert Threshold Profiles

**Purpose:**  
Define what system behavior should trigger alerts.

**Instructions:**
1. In Atera, go to **Admin > Thresholds**.
2. Click **+ New Profile** and name it `Felipe-VM Alerts`.
3. Under **Custom**, create the following alerts:

- **CPU Load**  
  - Category: CPU Load  
  - Alert Severity: Critical  
  - Threshold: 90%  
  - Time period: 1.5 minutes

- **Memory Usage**  
  - Category: Memory Usage  
  - Alert Severity: Warning  
  - Threshold: 85%  
  - Time period: 1.5 minutes

- **Disk Alerts from S.M.A.R.T.**  
  - Category: Disk Alerts  
  - Severity: Critical

![Screenshot 2025-06-20 223054](https://github.com/user-attachments/assets/55c3da52-1981-441f-81e0-55d845b7a5ed)

![Screenshot 2025-06-20 223054](https://github.com/user-attachments/assets/d511b68b-b8be-4505-b718-9fb857e7a44b)

![Screenshot 2025-06-20 223253](https://github.com/user-attachments/assets/8c8a0836-d106-46ff-bc30-f1016a9ab4ee)

---

## ✅ Step 4: Simulate CPU Overload with PowerShell

**Purpose:**  
Trigger the alert by simulating high CPU usage.

**Instructions:**
1. Open PowerShell in the VM **as Administrator**.
2. Run the following script to create CPU stress:

```powershell
for ($i=0; $i -lt 20; $i++) { Start-Job { while ($true) {} } }
```

3. Open **Task Manager** and confirm CPU is hitting 100%.
 
![Capture2 (2)](https://github.com/user-attachments/assets/f96abee0-8859-4ec1-bd8a-016d7914d713)
![3](https://github.com/user-attachments/assets/3f02855a-4a66-49e8-af28-c136b937da12)

---

## ✅ Step 5: Verify Alert in Atera Dashboard

**Purpose:**  
Ensure Atera detects the alert from the threshold profile.

**Instructions:**
1. Wait about 5–10 minutes after the CPU spike.
2. In Atera, go to **Devices > Alerts**.
3. You should see an alert: `CPU Load 100% > threshold`.

![Screenshot 2025-06-20 231908](https://github.com/user-attachments/assets/8073c26c-7dc9-4e72-bc8e-faa252e0b470)

---

## ✅ Step 6: Create a Ticket from the Alert

**Purpose:**  
Simulate an IT technician’s response by converting the alert into a helpdesk ticket.

**Instructions:**
1. Click **Create Ticket** from the alert row.
2. Fill in:
   - **Customer**: Unassigned  
   - **Contact**: John Smith  
   - **Title**: High CPU usage detected on VM  
   - **Type**: Incident  
   - **Technician**: fearbo fearbo  
   - **Description**:  
     > Alert triggered due to >90% CPU usage for 5 mins. Will investigate running processes.

3. Click **Create**.

**Screenshot:**  

![Screenshot 2025-06-20 232114](https://github.com/user-attachments/assets/b8a187c0-23f7-4603-b7fa-bd03c7f9aad3)

---

## ✅ Step 7: Troubleshoot the CPU Alert

**Purpose:**  
Resolve the issue and demonstrate technician action.

### 🔧 Option A: Manual via Task Manager
1. In the VM, open **Task Manager**.
2. Right-click the PowerShell process.
3. Click **End Task**.

![Screenshot 2025-06-20 234733](https://github.com/user-attachments/assets/8cb39732-4c4f-4194-b138-572f613989bf)

### 🧹 Option B: Clean up with PowerShell
```powershell
Get-Job | Stop-Job
```

![Capture4 (1)](https://github.com/user-attachments/assets/a009fd86-cb0f-4122-9a60-a32eec247a2f)

---

## ✅ Step 8: Resolve and Close the Ticket

**Purpose:**  
Document the fix and close the loop on the incident.

**Instructions:**
1. Open the ticket in Atera.
2. Add a comment like:
   > Investigated alert. Found CPU stress script running. Terminated script using PowerShell. Usage returned to normal.
3. Change status to **Resolved** or **Closed**.

![Screenshot 2025-06-20 235017](https://github.com/user-attachments/assets/c1f6c766-e7a7-4b5e-b9bf-237d49cb5919)

![Screenshot 2025-06-20 233034](https://github.com/user-attachments/assets/7f50c948-76a2-4aa4-8bf7-dc93cbde18d8)

---

## ✅ Step 9: Generate Ticketing Summary Report

**Purpose:**  
Show evidence of ticket activity for documentation or auditing.

**Instructions:**
1. In Atera, go to **Reports > Presets > Ticketing Summary**.
2. View statistics like:
   - Total Tickets
   - Source of ticket (Alert)
   - Duration
   - Status Breakdown
3. Take screenshots of the full summary.

![Screenshot 2025-06-20 233702](https://github.com/user-attachments/assets/f12c85c6-176f-44ae-a087-8bd12c8cce3f)
![Screenshot 2025-06-20 233713](https://github.com/user-attachments/assets/13a89372-e38c-4cf6-a478-5d44439f6ff5)
![Screenshot 2025-06-20 233718](https://github.com/user-attachments/assets/3972a2a6-cc9e-4e1c-b3c7-8f56a4a68844)

---

## 🧠 Recap: Key PowerShell Commands

### 🔥 Trigger CPU Overload
```powershell
for ($i=0; $i -lt 20; $i++) { Start-Job { while ($true) {} } }
```

### 🧯 Stop Background Jobs
```powershell
Get-Job | Stop-Job
```

---

## 📌 Conclusion

This RMM lab project covers the **full lifecycle of alert-based monitoring**, from agent deployment to threshold management, ticket handling, and reporting.

✅ Demonstrated skills:
- Remote Monitoring
- PowerShell Automation
- Incident Management
- Technical Documentation
- Reporting & Analysis
