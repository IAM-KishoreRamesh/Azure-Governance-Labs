# Day-03 — Azure Tags & Resource Locks Lab (Portal Only)

## 🎯 Objective
Establish governance controls by:
- Applying **Tags** for cost & ownership tracking
- Using **Resource Locks** to prevent accidental deletion or modification
- Verifying that **tags do NOT inherit** down the resource hierarchy

---

## 🧱 Lab Setup Architecture

RG-Governance-Lab
| Resource | Name |
|--------|-------|
| Blob Container | testcontainer1 |
| Storage Account | stgovlab12345 |


---

## 1️⃣ Create Resource Group
| Setting | Value |
|--------|-------|
| Name | `RG-Governance-Lab` |
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
> Same tags applied **manually** (to illustrate tags do not auto-inherit)

---

## 4️⃣ Tag Inheritance Verification
- Created Blob Container `testcontainer1`
- Checked **Tags** → **No tags present**

✅ **Conclusion:**  
Azure **does not automatically inherit tags** from RG → requires **Azure Policy** to enforce.

---

## 5️⃣ Apply Resource Locks

### A) Delete Lock (at Resource Group Level)
| Setting | Value |
|--------|-------|
| Lock Name | `Lock-NoDelete-RG` |
| Lock Type | Delete |
| Scope | Resource Group |

**Test:**  
Tried deleting the Storage Account → **Deletion blocked** ✅

---

### B) ReadOnly Lock (on Storage Account)
| Setting | Value |
|--------|-------|
| Lock Name | `Lock-RO-Storage` |
| Lock Type | ReadOnly |
| Scope | Storage Account |

**Test:**  
Tried creating a new blob container → **Modification blocked** ✅

---

## 6️⃣ Cleanup (Optional)
| Action | Reason |
|--------|--------|
| Remove `Lock-NoDelete-RG` | Allows future labs |
| Remove `Lock-RO-Storage` | Allows standard storage usage |

---

## 🧠 Key Takeaways
| Concept | Meaning | Notes |
|--------|---------|------|
| Tags | Metadata for cost/ownership tracking | Must be enforced with Policy |
| Delete Lock | Prevents deletion | Useful for production workloads |
| ReadOnly Lock | Prevents deletion & modification | Stronger lock; use carefully |
| Tag Inheritance | Not automatic | Needs Azure Policy |

---
