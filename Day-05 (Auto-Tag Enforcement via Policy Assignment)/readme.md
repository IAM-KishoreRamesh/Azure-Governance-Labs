# **Day-05 — Azure Policy: Auto-Apply Required Tags (Modify + Remediation)**

## 🎯 **Objective**
Implement an Azure Policy that **automatically applies required governance tags** to resources at creation (Modify effect) and **remediates existing untagged resources** using a Remediation Task.  
This enforces tagging without blocking developer workflows while ensuring historical compliance.

---

## 🏛️ **Governance Justification**

Consistent tagging is essential for cost allocation, incident ownership, and lifecycle management. Where a Deny policy (Day-04) can be too strict for some teams, a **Modify + Remediation** approach:

- Guarantees tags exist moving forward (automatic patching at creation)
- Remediates drift for pre-existing resources
- Minimizes developer friction while delivering governance

| Tag Name        | Purpose                                  |
| --------------- | ---------------------------------------- |
| **Environment** | TestLab                                  |
---

## 1️⃣ **Create Azure Policy (Modify: Auto-apply Tags)**

### **Policy Name:**  
`Add a tag to resource`

### **Behavior:**  
- On resource create/update, the policy **adds missing tag keys** with parameterized default values or values supplied at assignment time (e.g., `Environment = "TestLab"` or use a parameter).

### **Required Tags (parameterized):**
- Environment  

### **Screenshots to Capture:**
1. Policy Definition page  
2. Policy Rule JSON (showing `effect: Modify`)  

---

## 2️⃣ **Create Remediation Task (Fix existing resources)**

### **Remediation Name:**  
`Remediate-Untagged-Resources-Day05`

### **Scope:**  
`Subcription`

### **Behavior:**  
- Targets existing resources missing one or more required tags and applies tags using the same parameterized defaults used by the Modify policy.

### **Screenshots: **
3. Policy Enforced across the resources present inside the subcription scope  

---

## 3️⃣ **Assign Policy at Resource Group Scope**

| Field                | Value                           |
| -------------------- | ------------------------------- |
| **Scope**            | `Subcription`                   |
| **Assignment Name**  | `Add a tag to resource`    |
| **Enforcement Mode** | Enabled (for Modify)            |
| **Parameters**       | Default tag values (or prompt)  |

---

## 4️⃣ **Test the Policy (Expected: Auto-applied tags / Remediated)**

### **Test 1:** Create a Storage Account *without* tags  
- Expected → ✅ **Resource created** and tags **automatically applied** (see resource tags afterwards)

### **Test 2:** Create VM / Public IP without tags  
- Expected → ✅ **Tags applied on creation**

### **Test 3:** Run Remediation Task against existing resources missing tags  
- Expected → ✅ **Existing resources receive tags**  
- Verify via Portal, CLI, or Resource Graph

---

## 5️⃣ **Verify Compliance & Remediation**

Navigate to:  
**Azure Policy → Assignments → Compliance** and **Remediation → Jobs**

You should see:

| Status            | Meaning                                                |
| ----------------- | ------------------------------------------------------ |
| **Compliant**     | Resources now include required tags                    |
| **Remediation**   | Ongoing / completed remediation runs for historical resources |

---

## 6️⃣ **Understanding Behavior & Tradeoffs**

### ✔️ **What This Policy Guarantees**
- Missing tags are added at creation automatically.
- Historical untagged resources can be remediated centrally.
- Lower developer friction compared to Deny policies.

### ⚠️ **Limitations & Considerations**
- **Value correctness:** Auto-applied defaults (e.g., `Environment = "TestLab"`) may require later correction—use remediation or workflows to update accurate values.
- **Not a replacement for governance processes:** Use with tagging guidance, onboarding, and CI/CD checks.
- **Modify effect requires testing:** Some resource types or operations may not support the Modify effect; test across resource types you use.
- **Remediation permissions:** Remediation jobs run with the Managed Identity of the assignment — ensure it has required permissions.
- **Scope Priority:** When there is a policy assigned at **resource group** and **subcription** level, then policy in resource group gets prioritized.

---

## 🧠 **Key Takeaways**

| Concept               | Explanation                                           |
| --------------------- | ----------------------------------------------------- |
| **Modify Policy**     | Auto-applies changes (e.g., tags) during deployment   |
| **Remediation Task**  | Applies policy to existing resources                  |
| **Parameterization**  | Make tag values configurable at assignment time       |
| **Scope & Testing**   | Test across resource types before broad assignment    |

---

## ✔️ **Lab Completion Checklist**

| Item                                           | Status |
| ---------------------------------------------- | ------ |
| Policy definition (Modify) created             | ✅     |
| Required tags parameterized                    | ✅     |
| Policy assigned to Subcription scope           | ✅     |
| Resources auto-tagged on creation              | ✅     |
| Remediation task created & run successfully    | ✅     |
| Compliance & remediation verification captured | ✅     |
| All screenshots captured                       | ✅     |

---

## ✅ **Recommended Repo Structure**
