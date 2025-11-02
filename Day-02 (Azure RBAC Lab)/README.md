# 🧾 Azure Blob Storage RBAC — Role-Based Access Control using Microsoft Entra ID

## 📘 Overview
This lab demonstrates **Azure RBAC (Role-Based Access Control)** for **Blob Storage** access using **Microsoft Entra ID authentication** instead of access keys.  
The objective is to verify **read-only** and **read-write** behaviors based on assigned roles and understand how **management plane vs data plane** permissions differ.

---

## 🏗️ Architecture Setup

| Component | Name | Purpose |
|------------|------|----------|
| **Resource Group** | `RG-Test-India` | Logical container for storage resources |
| **Storage Account** | `storagerbacindia` | Used for RBAC testing |
| **Container** | `rbac-test` | Blob container for access validation |
| **User** | `user1@kishoreramesh2023gmail.onmicrosoft.com` | Secondary Entra ID user for role testing |
| **Blob Uploaded** | `Data Flow.png` | Test blob for download/delete verification |

---

## 🔧 Step-by-Step Implementation

### **A. Storage Account Creation**
1. Created a new **storage account** `storagerbacindia` in `RG-Test-India`.  
2. Configuration:
   - **Redundancy:** LRS  
   - **Public Access:** Disabled  
   - **Default Authentication:** Microsoft Entra ID (RBAC)  

📸 *Reference:* `/screenshots/Storage_creation.png`

---

### **B. Container Creation**
1. Navigated to **Data storage → Containers → + Container**
2. Created container:
   - Name: `rbac-test`
   - Public Access: **Private**

📸 *Reference:* `/screenshots/Container_creation.png`

---

### **C. Upload Baseline Blob**
- Authentication Mode: **Access Key**
- Uploaded `Data Flow.png` successfully to the `rbac-test` container.

📸 *Reference:* `/screenshots/Before_role_assignment.png`

---

## 🧑‍💻 D. Role Assignment — Microsoft Entra ID Authentication

1. Switched to **Microsoft Entra ID authentication**.  
2. Added role assignment under: storagerbacindia → Containers → rbac-test → Access Control (IAM)

3. Assigned role:
- **Role:** Storage Blob Data Reader  
- **Scope:** Container (this resource only)  
- **Assignee:** `User1`

📸 *Reference:* `/screenshots/Storage_reader_access.png`

---

## 🧪 E. Validation of Permissions (Data Plane)

All operations were performed by **User1** after sign-in at  
➡️ [https://portal.azure.com](https://portal.azure.com)

| Operation | Result | Behavior |
|------------|---------|-----------|
| **Download Blob** | ✅ Successful | User1 can read/download the blob |
| **Upload Blob** | ❌ Denied | “This request is not authorized to perform this operation” |
| **Delete Blob** | ❌ Denied | “This request is not authorized to perform this operation” |

📸 *References:*  
- `/screenshots/Download_attempt.png`  
- `/screenshots/Upload_attempt_denied.png`  
- `/screenshots/Delete_attempt_denied.png`

**Explanation:**  
The **Storage Blob Data Reader** role grants access to the **data plane** only for *read* actions — no writes or deletes.

---

## ⚙️ Understanding the Issue: Management Plane vs Data Plane

| Type | Description | Controlled by | Example Actions | Role Examples |
|------|--------------|---------------|------------------|----------------|
| **Management Plane** | Operations done **via Azure Resource Manager (ARM)** — managing storage account itself | **Azure RBAC** at Resource/Subscription level | Create/Delete Storage Account, Set IAM permissions | Contributor, Owner |
| **Data Plane** | Operations **inside** the storage account (blobs, files, queues) | **Azure RBAC** for data or Access Keys/SAS | Read, Upload, Delete blobs | Storage Blob Data Reader / Contributor |

### ⚠️ Issue Faced:
Initially, `User1` couldn’t see the storage account in **Storage Explorer** or **portal** —  
because the **RBAC role was applied only to the data plane (container)**, not the management plane.  

Once the correct container-level access was confirmed and RBAC propagated, visibility and read access worked as expected.

---

## 🧭 Troubleshooting & Lessons Learned

| Issue | Root Cause | Resolution |
|--------|-------------|-------------|
| Storage not visible to User1 | Role assigned only at data plane (container level) | Log in as `User1` and navigate directly to the container URL, or assign Reader at storage account level |
| RBAC delay | Azure backend propagation delay (up to 10 mins) | Wait 5–10 mins after role assignment |
| Domain sign-in failed | Same email domain reused | Created new user in same tenant (`User1`) with unique UPN |
| Upload/Delete denied | Limited by role (`Reader`) | Changed to `Storage Blob Data Contributor` |

---

## 🔍 Access Summary

| Action | Access Key Auth | Microsoft Entra (Reader) | Microsoft Entra (Contributor) |
|---------|----------------|---------------------------|-------------------------------|
| Upload Blob | ✅ | ❌ | ✅ |
| Download Blob | ✅ | ✅ | ✅ |
| Delete Blob | ✅ | ❌ | ✅ |
| Create Container | ✅ | ❌ | ✅ |

---

## 📚 Key Takeaways
- **Azure RBAC for storage** works separately for **management** and **data planes**.  
- **Microsoft Entra ID authentication** respects role boundaries — no full access unless explicitly granted.  
- **Storage Blob Data Reader** = strictly read-only.  
- **Storage Blob Data Contributor** = full CRUD on blobs.  
- **RBAC changes take time** — always recheck after 5–10 mins.  

---

## ✅ Conclusion
- This project successfully demonstrates how **Azure Blob Storage RBAC** governs access based on **Microsoft Entra ID roles**.  
- Unlike access keys, which grant unrestricted control, **RBAC enforces principle of least privilege**, offering better **security, compliance, and governance** across cloud environments.
