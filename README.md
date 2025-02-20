<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<!-- <h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)
-->
<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
  **Part 1: Install Active Directory & Join Client-1 to the Domain**  

### **1️⃣ Turn on the Virtual Machines**
<details>
<summary><b>Click to Expand</b></summary>

- In the **Azure Portal**, **turn on** the following Virtual Machines if they are off:
  - **DC-1** (Domain Controller)
  - **Client-1** (Workstation)
</details>

---

### **2️⃣ Install Active Directory Domain Services (AD DS)**
<details>
<summary><b>Click to Expand</b></summary>

1. **Log into DC-1** as `labuser`.  
2. Open **Server Manager** → Click **Manage** → **Add Roles and Features**.  
3. Install **Active Directory Domain Services (AD DS)**.  
4. **Promote DC-1** as a Domain Controller:
   - Set up a **new forest**: `mydomain.com` (choose your own domain name).  
5. Restart DC-1 and **log back in** as:  
mydomain.com\labuser

markdown
Copy
Edit
</details>

---

### **3️⃣ Create a Domain Admin User**
<details>
<summary><b>Click to Expand</b></summary>

1. Open **Active Directory Users and Computers (ADUC)**.
2. Create an **Organizational Unit (OU)** named `_EMPLOYEES`.  
3. Create a **new OU** named `_ADMINS`.  
4. Create a new employee:
- **Full Name**: Jane Doe  
- **Username**: `jane_admin`  
- **Password**: `Cyberlab123!`  
5. Add `jane_admin` to the **Domain Admins** security group.  
6. **Log out** and reconnect as:  
mydomain.com\jane_admin

markdown
Copy
Edit
7. Use `jane_admin` as your **admin account** from now on.  
</details>

---

### **4️⃣ Join Client-1 to the Domain**
<details>
<summary><b>Click to Expand</b></summary>

1. In the **Azure Portal**:
- Set **Client-1’s DNS** to **DC-1’s Private IP** (already done).
- **Restart Client-1** (already done).
2. **Login to Client-1** as `labuser`.  
3. **Join Client-1 to the domain (`mydomain.com`)**:
- Open **System Properties** → Click **Change Settings**.  
- Set **Domain** to `mydomain.com`.  
- Restart **Client-1**.  
4. **Verify Client-1 in Active Directory**:
- Log into **DC-1**.
- Open **ADUC** → Confirm that Client-1 appears in the **Computers** section.  
5. **Organize Client-1 in AD**:
- Create a new OU called `_CLIENTS`.  
- **Move Client-1** into `_CLIENTS`.  
</details>

---

## **Part 2: Configure Remote Desktop & Add Users**  

### **5️⃣ Configure Remote Desktop for Non-Admins**
<details>
<summary><b>Click to Expand</b></summary>

1. Log into **Client-1** as:  
mydomain.com\jane_admin

bash
Copy
Edit
2. Open **System Properties** → Click **Remote Desktop**.  
3. Allow **domain users** to access Remote Desktop.  
4. Now, **non-administrative users can log in remotely**.  
5. In real-world environments, you’d configure this using **Group Policy (GPO)** for multiple machines at once.  
</details>

---

### **6️⃣ Bulk Create Users via PowerShell**
<details>
<summary><b>Click to Expand</b></summary>

1. **Log into DC-1** as `jane_admin`.  
2. Open **PowerShell ISE** as an **Administrator**.  
3. Create a **new script file** and paste the following script:  

```powershell
# PowerShell Script to Create Users in Active Directory
$password = ConvertTo-SecureString "UserPass123!" -AsPlainText -Force
for ($i=1; $i -le 10; $i++) {
    $username = "user" + $i
    New-ADUser -Name $username -SamAccountName $username -UserPrincipalName "$username@mydomain.com" -Path "OU=_EMPLOYEES,DC=mydomain,DC=com" -AccountPassword $password -Enabled $true
}
Run the script and observe user accounts being created.
Open ADUC and verify that the users appear under _EMPLOYEES.
Attempt to log into Client-1 with one of the new accounts:
makefile
Copy
Edit
Username: user1  
Password: UserPass123!
</details>
Final Steps: Finishing the Lab
<details> <summary><b>Click to Expand</b></summary>
✅ Verify all configurations:

Ensure Client-1 is joined to the domain.
Confirm that jane_admin has admin privileges.
Test Remote Desktop for a non-admin user.
✅ Save Money in Azure:

Do NOT delete the VMs—we will use them for future labs.
If you’re done for the day, STOP the VMs in the Azure Portal to avoid extra charges.
</details>
🎉 Lab Complete!
You've successfully set up Active Directory, joined a workstation to the domain, enabled Remote Desktop, and bulk-created users in PowerShell! 🚀 Keep practicing to build confidence.

vbnet
Copy
Edit

---
</p>
