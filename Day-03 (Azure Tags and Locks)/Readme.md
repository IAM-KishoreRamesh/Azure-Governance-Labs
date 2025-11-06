# Day-03 — Azure Tags & Resource Locks Lab

## 🎯 Objective
Implement foundational **governance controls** in Azure by:
- Applying **standardized Tags** for **cost allocation** and **resource ownership**
- Using **Resource Locks** to prevent accidental deletion or modification
- Demonstrating that **tags do NOT inherit** automatically between resources

---

## 🧱 Lab Setup Architecture

| Resource Type     | Name              |
|------------------|------------------|
| Resource Group    | RG-Governance-Lab |
| Storage Account   | stgovlab          |
| Blob Container    | testcontainer1    |

---

## 🏛️ Tag Standardization Policy (Governance Justification)

In enterprise cloud environments, **resource tags follow a defined standard** to support:

| Purpose | Description |
|--------|-------------|
| **Cost Allocation** | Enables showback/chargeback reporting. |
| **Ownership Tracking** | Identifies accountability for incidents and lifecycle. |
| **Automation & Lifecycle Management** | Enables automated cleanup, expiry workflows, and monitoring. |
| **Security & Compliance Auditing** | Supports policy rules for data classification and controls. |

### Recommended Tag Schema

| Tag Name         | Description | Example Value |
|------------------|-------------|---------------|
| **Owner**        | Person/team responsible | `kishore.ramesh@company.com` |
| **Environment**  | Deployment stage | `Dev`, `Test`, `Prod` |
| **CostCenter**   | Financial tracking unit | `GOV001` |
| **Project**      | Project / System name | `AzureGovernanceLabs` |
| **DataSensitivity** | Security classification | `Internal`, `Confidential` |

---

## 1️⃣ Create Resource Group

| Setting | Value |
|--------|-------|
| Name   | `RG-Governance-Lab` |
| Region | Central India |

---

## 2️⃣ Create Storage Account

| Setting | Value |
|--------|-------|
| Name | `stgovlab12345` *(must be unique)* |
| Resource Group | `RG-Governance-Lab` |
| Performance | Standard |
| Redundancy | LRS |
| Access Tier | Hot |

---

## 3️⃣ Apply Tags

### Tags applied to **Resource Group**
| Tag | Value |
|----|-------|
| Environment | GovernanceLab |
| Owner | Kishore |
| CostCenter | GOV001 |
| Project | AzureGovernanceLabs |

### Tags applied to **Storage Account**
(Re-applied manually to demonstrate tag inheritance does **not** happen automatically)

---

## 4️⃣ Tag Inheritance Verification

- Created **Blob Container**: `testcontainer1`
- Navigated to the container settings → **No Tags**

✅ **Result:**  
**Azure does not auto-inherit tags** to child resources.  
This must be enforced using **Azure Policy**.

> Note: Blob Containers **do not support tags**. They only support **Metadata**, which is not used for cost/ownership governance.

---

## 5️⃣ Apply Resource Locks

### A) Delete Lock (at Resource Group level)

| Setting | Value |
|--------|-------|
| Lock Name | `Lock-NoDelete-RG` |
| Lock Type | Delete |
| Scope | `RG-Governance-Lab` |

**Test:** Attempt to delete Storage Account → **Deletion blocked** ✅

---

### B) ReadOnly Lock (at Storage Account level)

| Setting | Value |
|--------|-------|
| Lock Name | `Lock-RO-Storage` |
| Lock Type | ReadOnly |
| Scope | `stgovlab12345` |

**Test:** Attempt to create Blob Container → **Modification blocked** ✅

---

## 6️⃣ Cleanup (Optional)

| Action | Reason |
|--------|--------|
| Remove `Lock-NoDelete-RG` | Allows future resource deletion |
| Remove `Lock-RO-Storage` | Restores normal storage operations |

---

## 🧠 Key Takeaways

| Concept | Meaning | Notes |
|--------|---------|------|
| Tags | Identify ownership, cost, and governance | Must be enforced using **Azure Policy** |
| Delete Lock | Prevents deletion actions | Useful for production resource protection |
| ReadOnly Lock | Prevents creation, modification, deletion | Use carefully — stops operational changes |
| Tag Inheritance | **Not automatic** in Azure | Policy-based enforcement is required |
| Blob Container Tags | Containers **do not support tags** | They use **Metadata**, not cost governance tags |

---

## ✅ Summary
This lab demonstrates how to:
- Apply and standardize tags across Azure resources
- Prevent accidental resource modification using locks
- Validate that tags **must be enforced**, not assumed

**This is foundational for any real-world Azure Governance model.**
