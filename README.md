
# **README – Multi-Account AWS Governance Project**

## **Project Title**

**Multi-Account AWS Governance Setup using AWS Organizations and Service Control Policies (SCPs)**
-![Uploading Architecture.png.png…]()

---

## **1. Project Overview**

This project demonstrates how to implement **centralized governance** across multiple AWS accounts to enforce security, cost control, and compliance.

**Scenario:**

* The organization manages multiple accounts for **Development, Testing, and Production**.
* Developers were creating expensive resources in production, and security policies were inconsistent.

**Objective:**

* Design a multi-account governance model using **AWS Organizations, OUs, and SCPs**.
* Prevent unauthorized or risky actions across accounts.

---

## **2. AWS Organization Setup**

**Organizational Units (OUs) created:**

* **Dev OU** → dev-account
* **Test OU** → test-account
* **Prod OU** → prod-account

**Screenshots:**

* `OU_structure.png`

**Notes:**

* OUs were later deleted during cleanup after completing the project.

---

## **3. Service Control Policies (SCPs)**

| **Policy Name**      | **Description**                                                           | **Attached To** | **Screenshot**          |
| -------------------- | ------------------------------------------------------------------------- | --------------- | ----------------------- |
| DenyLargeEC2         | Denies launching large EC2 instance types (e.g., m5.large) in Dev account | Dev OU          | `Denied_m5_large.png`   |
| DenyCloudTrailDelete | Prevents stopping or deleting CloudTrail                                  | Dev/Test/Prod   | `Denied_CloudTrail.png` |
| RestrictRegions      | Restricts usage to approved regions only                                  | Dev/Test/Prod   | `Restricted_Region.png` |

**SCP JSON files:**

* `DenyLargeEC2.json`
* `DenyCloudTrailDelete.json`
* `RestrictRegions.json`

**Note:**

* FullAWSAccess (AWS managed) remains attached to all accounts.
* This policy **does not generate charges** and only grants full permissions where SCPs do not block actions.

---

## **4. Validation & Proof of Governance**

### **Allowed Actions**

* **t3.micro** instance creation is allowed (small instance type)
* Screenshot optional: `Allowed_t3_micro.png`

### **Denied Actions**

* Launch **m5.large EC2 instance** → Access Denied ✅ (`Denied_m5_large.png`)
* Stop or delete **CloudTrail** → Access Denied ✅ (`Denied_CloudTrail.png`)
* Create **Key Pair** → Access Denied ✅ (`Denied_CreateKeyPair.png`)
* Launch resources in **non-approved regions** → Access Denied ✅ (`Restricted_Region.png`)

### **CloudTrail Event History Proof**

* Shows AccessDeniedException for restricted actions
* Screenshot: `CloudTrail_History.png`

---

## **5. Policy Logic**

* **DenyLargeEC2:** Prevents high-cost EC2 instance types in non-prod accounts to control costs.
* **DenyCloudTrailDelete:** Ensures that audit logs cannot be deleted, maintaining security and compliance.
* **RestrictRegions:** Prevents creating resources in unapproved regions for governance compliance.

---

## **6. Risk Mitigation Strategy**

* **Cost control:** Prevents developers from launching expensive resources in Dev/Test accounts.
* **Security:** CloudTrail cannot be stopped or deleted, ensuring audit logs are preserved.
* **Compliance:** Enforces approved regions and standard policies across accounts.
* **Centralized governance:** Reduces the need for manual monitoring of each account.

---

## **7. Governance Benefits**

* Centralized administration of permissions across multiple accounts.
* Reduced risk of unauthorized actions or high costs.
* Simplified enforcement of compliance and security standards.
* Clear separation of duties between Dev, Test, and Prod accounts.

---

## **8. Deliverables**

1. **Screenshots:**

   * OU structure
   * Denied actions (m5.large, CloudTrail, Key Pair)
   * CloudTrail event history

2. **SCP JSON files:**

   * `DenyLargeEC2.json`
   * `DenyCloudTrailDelete.json`
   * `RestrictRegions.json`

✅ Project Completed Successfully


