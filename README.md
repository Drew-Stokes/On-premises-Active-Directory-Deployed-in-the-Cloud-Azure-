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

- Turn on the Virtual Machines
- Install Active Directory Domain Services (AD DS)
- Create a Domain Admin User
- Join Client-1 to the Domain
- Configure Remote Desktop & Add Users
- Bulk Create Users via PowerShell

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
  <p>
  <img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/3ed7c0b49a7fa0d4996b76b21af856c9520f492a/server_manager.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
  </p>

3. Install **Active Directory Domain Services (AD DS)**.  
  <p>
  <img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/a5641bdebf964ac72aa5c5c26157704130eae205/active_directory_install_2.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
  </p>
4. **Promote DC-1** as a Domain Controller:
   <p>
    <img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/a5641bdebf964ac72aa5c5c26157704130eae205/promote_DC1_to_Domain_controller.png" height="30%" width="30%"   alt="Disk Sanitization Steps"/>
   </p>
   - Set up a **new forest**: `mydomain.com` (choose your own domain name).  
    <p>
  <img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/a5641bdebf964ac72aa5c5c26157704130eae205/set_up_new_forest.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
  </p>
6. Restart DC-1 and **log back in** as:  
mydomain.com\labuser

<p>
  <img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/a5641bdebf964ac72aa5c5c26157704130eae205/log_back_in_as_mydomain.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
  </p>
</details>

---

### **3️⃣ Create a Domain Admin User**
<details>
<summary><b>Click to Expand</b></summary>

1. Open **Active Directory Users and Computers (ADUC)**.
  <p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/6f7a2fcae1ce2396c0693e781c886de91bae3269/Active_directory_users_and_computers.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

2. Create an **Organizational Unit (OU)** named `_EMPLOYEES`.  
  <p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/6f7a2fcae1ce2396c0693e781c886de91bae3269/creat_employee_ou.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

3. Create a **new OU** named `_ADMINS`.  
  <p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/6f7a2fcae1ce2396c0693e781c886de91bae3269/create_admins_OU.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

4. Create a new employee:
- **Full Name**: Jane Doe  
- **Username**: `jane_admin`  
- **Password**: `Cyberlab123!`  
<p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/6f7a2fcae1ce2396c0693e781c886de91bae3269/create_user_jane_doe.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

5. Add `jane_admin` to the **Domain Admins** security group.  
<p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/6f7a2fcae1ce2396c0693e781c886de91bae3269/add_jane_to_admins_group.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

6. **Log out** and reconnect as:
mydomain.com\jane_admin

<p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/6f7a2fcae1ce2396c0693e781c886de91bae3269/jane_admin_my_domain_log_in.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

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
  <p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/2b970f8b404c3cc8afafb1f4b9e9d7d5ff8fce30/join_client_to_my_domain.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>
  
- Restart **Client-1**.
  <p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/2b970f8b404c3cc8afafb1f4b9e9d7d5ff8fce30/restart_computer.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

4. **Verify Client-1 in Active Directory**:
- Log into **DC-1**.
- Open **ADUC** → Confirm that Client-1 appears in the **Computers** section.
  <p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/2b970f8b404c3cc8afafb1f4b9e9d7d5ff8fce30/verify_client_is_active_directory.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>
 
5. **Organize Client-1 in AD**:
- Create a new OU called `_CLIENTS`.  
- **Move Client-1** into `_CLIENTS`.
  <p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/2b970f8b404c3cc8afafb1f4b9e9d7d5ff8fce30/organize_client.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

</details>

---

## **Part 2: Configure Remote Desktop & Add Users**  

### **5️⃣ Configure Remote Desktop for Non-Admins**
<details>
<summary><b>Click to Expand</b></summary>

1. Log into **Client-1** as:  
mydomain.com\jane_admin
<p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/6942037df8a9b2a53bc2cfeb3104a0a29f2babb2/log_into_client1_as_janeadmin(p2).png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

2. Open **System Properties** → Click **Remote Desktop**.  
3. Allow **domain users** to access Remote Desktop.  
<p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/6942037df8a9b2a53bc2cfeb3104a0a29f2babb2/allow_domain_users_to_access_remote_Desktop.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>


---

### **6️⃣ Bulk Create Users via PowerShell**
<details>
<summary><b>Click to Expand</b></summary>

1. **Log into DC-1** as `jane_admin`.  
2. Open **PowerShell ISE** as an **Administrator**.  
<p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/ebc18d6c3afadcd48e63c29c558889083906976d/run_power_shell_as_administrator.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>

3. Create a **new script file** and paste the following script:  
<p>
<img src="https://github.com/Drew-Stokes/On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-/blob/ebc18d6c3afadcd48e63c29c558889083906976d/power_shell_script.png"/>
</p>

```powershell
# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 10000
# ------------------------------------------------------ #

Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name

}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $fisrtName = generate-random-name
    $lastName = generate-random-name
    $username = $fisrtName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
}

---
</p>
