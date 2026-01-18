# Azure-Network-Project

Here’s a **clear, structured understanding** of what I’ve designed.

---

## ✅ What Designed (Summary)

### 🔥 Project

**Enterprise Hub-and-Spoke Secure Azure Network**

### 🎯 Goal

A **centralized, secure Azure network** with:

* Controlled outbound internet access
* No public IP exposure
* Bastion-only access

---

## 🧱 Architecture Breakdown

### 1️⃣ Virtual Networks

* **Hub VNet (10.0.0.0/16)**

  * AzureFirewallSubnet
  * AzureBastionSubnet
  * GatewaySubnet
  * Mgmt-Subnet

* **Spoke-App VNet (10.1.0.0/16)**

  * App-Subnet

* **Spoke-DB VNet (10.2.0.0/16)**

  * DB-Subnet

✔️ Clean IP planning
✔️ Correct use of Hub for shared services

---

### 2️⃣ Connectivity

* Hub ↔ App VNet peering
* Hub ↔ DB VNet peering
* **Forwarded traffic enabled**

✔️ Required for firewall traffic inspection

---

### 3️⃣ Routing (UDR)

* `0.0.0.0/0 → Azure Firewall`
* Associated with:

  * App-Subnet
  * DB-Subnet

✔️ Forces all egress through Firewall
✔️ Proper central routing model

---

### 4️⃣ Security Controls

* **Azure Firewall**

  * Allow outbound HTTP/HTTPS only
  * Deny all other internet traffic

* **NSGs**

  * App subnet:

    * Allow HTTP only from Load Balancer
  * DB subnet:

    * Allow traffic only from App subnet
  * Deny all inbound internet traffic

✔️ Strong defense-in-depth
✔️ Correct separation of duties (Firewall + NSG)

---

### 5️⃣ Internet Access

* ❌ No Public IPs on any VM
* All outbound traffic path:

  ```
  VM → UDR → Azure Firewall → Internet
  ```

✔️ Enterprise-grade egress control

---

### 6️⃣ Bastion Access

* Azure Bastion deployed in Hub
* SSH/RDP disabled from public internet
* VM access only via Bastion
* SSH allowed only:

  ```
  Hub → App/DB
  ```

✔️ Zero Trust–aligned access model
✔️ Excellent security posture

---

### 7️⃣ Load Balancing

* Internal Load Balancer
* App VMs behind ILB
* Health probe configured

✔️ Correct for private application tiers

---

### 8️⃣ Private DNS

* Private DNS zone: **aritra.in**
* Linked to all VNets

✔️ Enables private name resolution across hub & spokes

---

## ✅ Self-Test Checklist – Verdict

| Test                            | Status |
| ------------------------------- | ------ |
| App internet only via Firewall  | ✅      |
| Direct VM internet blocked      | ✅      |
| Traffic inspected by Firewall   | ✅      |
| DB VM internet blocked          | ✅      |
| App → DB communication          | ✅      |
| No public IPs anywhere          | ✅      |
| Bastion-only access             | ✅      |
| Access web apps via Firewall IP | ✅      |

👉 **This passes as a real enterprise interview-ready design.**

---

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/7995e179-8360-45ce-8010-6c9f0b696db6" />

