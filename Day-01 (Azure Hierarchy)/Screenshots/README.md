# 🌐 Day 01 – Azure Hierarchy & Change Resource Group Action

## 🧠 What I Learned
- Understood the **Azure Resource Hierarchy**:
  - **Management Group → Subscription → Resource Group → Resources**
- Explored the **relationship between Subscriptions and Resource Groups**
- Learned how to **move a resource from one Resource Group to another**
- Understood that resource moves require both source and target RGs to be under the same subscription

---

## 🧪 Practical Steps Performed

### 🔹 Step 1: Viewed Azure Hierarchy
1. Listed all Resource Groups under the subscription.
2. Verified the subscription details.

**Screenshots:**
- [Azure Subscription](./Screenshots/Azure%20Subcription.png)
- [Azure Resource Groups List](./Screenshots/Azure%20Resource%20Groups%20List.png)
- [RG-Test-India Content](./Screenshots/RG-Test-India%20Content.png)
- [RG-Test-US Content](./Screenshots/RG-Test-US%20content.png)

---

### 🔹 Step 2: Performed Resource Move (Change RG Action)
1. Opened the resource to move.
2. Selected **Change Resource Group** option.
3. Selected Source and Target Resource Groups.
4. Reviewed configuration and completed the move.
5. Verified successful migration using the Azure Portal.

**Screenshots:**
- [Source + Target Selection](./Screenshots/Change%20RG%20action/Step-1%20Source%20%2B%20Target.png)
- [Resource Selection](./Screenshots/Change%20RG%20action/Step-2%20Resource%20to%20move.png)
- [Review and Confirmation](./Screenshots/Change%20RG%20action/Step-3%20Review.png)
- [Output](./Screenshots/Output.png)

---

## 📊 Outcome
✅ Resource successfully moved from `RG-Test-US` → `RG-Test-India`.  
✅ Subscription hierarchy and policies remain consistent.  
✅ Learned the importance of having both RGs under the same subscription for move operations.

---

## 📝 Notes
- Certain resources **cannot be moved** (like VNet with dependent subnets or locked resources).
- Always verify **Region compatibility** before moving.
- Moving resources between **subscriptions** requires additional permissions.

---
