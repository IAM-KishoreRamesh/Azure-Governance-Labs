# **Day-04 — Enforce Required Tags Using Azure Policy (Deny Policy)**

## 🎯 Objective
Implement an Azure Policy that **blocks resource creation** when mandatory governance tags are missing.  
This ensures all deployments follow cost, ownership, and workload tagging standards.

---

## 🏛️ Governance Rationale
The following tags were enforced to maintain governance discipline:

| Tag Key        | Purpose                                  |
|----------------|-------------------------------------------|
| **Owner**      | Accountability for lifecycle & incidents  |
| **Environment**| Dev / QA / Prod separation                |
| **CostCenter** | Chargeback / financial tracking           |
| **Project**    | Mapping resources to business workloads   |

Untagged deployments create audit gaps, cost leakage, and ownership ambiguity.  
A **deny policy** prevents this from happening.

---

## 1️⃣ Creating the Azure Policy (Deny If Required Tags Missing)

### **Policy Name**
`Deny-Creation-If-Tags-Missing`

### **What Was Implemented**
- A **built-in deny policy**
- Four **required tag parameters**
- A policy rule validating tag presence
- Deny effect to block non-compliant deployments

### **Screenshots to Capture**
1. Policy Definition
2. Policy Rule JSON
3. Parameters

---

## 2️⃣ Assigning the Policy to the Resource Group

| Field                | Value               |
|----------------------|---------------------|
| **Scope**            | `Subcription-Level` |
| **Assignment Name**  | `Deny-Missing-Tags` |
| **Enforcement Mode** | Enabled             |
| **Parameters**       | Owner, Environment, CostCenter, Project |

### **Screenshots to Refer**
1. Resource Group Used
2. Policy Creation at Subcriptio level

---

## 3️⃣ Testing the Policy Enforcement

### **Test 1:** Storage Account without tags  
**Result:** ❌ Blocked by policy

### **Test 2:** Additional resources (VM, Public IP, Container) without tags  
**Result:** ❌ Blocked

### **Screenshots to Capture**
3. Resource Deployment failed due to lack of tags
4. Resource Deployed with mandatory tags

---

## 4️⃣ Compliance Verification

Navigation:  
**Azure Policy → Assignments → Compliance**

| Status        | Meaning                                  |
|---------------|-------------------------------------------|
| Compliant     | All resources have tags                   |
| Non-compliant | None — untagged resources were rejected   |

---

## 5️⃣ Understanding the Deny Policy Behavior

### ✔️ Guarantees
- No untagged resources can be deployed
- Enforces tagging from day one
- Ensures accountability and cost mapping

### ❌ Not Supported (By Design)

| Feature                         | Supported? |
|----------------------------------|------------|
| Auto-adding missing tags         | ❌ Use Modify policy |
| Fixing existing untagged items   | ❌ Use remediation tasks |
| Tag inheritance from RG          | ❌ Azure does not support |

---

## 🧩 Key Learnings

| Concept           | Summary                                          |
|-------------------|--------------------------------------------------|
| Deny Effect       | Immediately blocks invalid deployments           |
| Governance Tags   | Essential for cost, security, and operations     |
| Scope Matters     | Policy applies only at the assigned boundary     |
| Enterprise Usage  | Standard in QA, Prod, and FinOps environments    |

---

## ✅ Lab Completion Checklist

| Task                                      | Status |
|-------------------------------------------|--------|
| Custom policy created                     | ✅      |
| Required tag parameters configured        | ✅      |
| Policy assigned to Subcription            | ✅      |
| Untagged resources blocked                | ✅      |
| Compliance verified                       | ✅      |
| Screenshots documented                    | ✅      |

---
