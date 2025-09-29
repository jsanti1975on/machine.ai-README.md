# 📑 Lab Guide – SharePoint Server Subscription Edition (SPSE) on machine.ai

This guide documents the installation of **SharePoint Server Subscription Edition (SE)** 
in the `machine.ai` Active Directory domain on the **10.10.20.0/24** subnet.

---

## 🖧 Network & Server Plan

| Hostname | Role(s)                  | IP Address   |
|----------|--------------------------|--------------|
| ANN      | AD DS, DNS (Primary DC)  | 10.10.20.1   |
| GAN      | AD DS, DHCP (Relay)      | 10.10.20.2   |
| NLG      | File Server (FSRM)       | 10.10.20.3   |
| SQL01    | SQL Server (2019/2022)   | 10.10.20.4   |
| SPSE01   | SharePoint Server SE     | 10.10.20.5   |

Gateway = `10.10.20.254` (Cisco RV340)  
DNS = `10.10.20.1` (ANN)  

---

## ⚙️ Prerequisites

- **Operating System:** Windows Server 2019 or 2022 (64-bit).  
- **Database:** SQL Server 2019 or 2022 installed on `SQL01`.  
- **Domain:** All servers joined to `machine.ai`.  
- **Service Accounts (AD):**
  - `SPFarm` → Farm admin
  - `SPService` → Service apps
  - `SPWebApp` → Web applications
  - `SPInstall` → Setup/installer account  

👉 All accounts should be created in AD beforehand with **strong passwords** and proper group memberships.

---

## 🔧 Step 1 – Install SQL Server (SQL01)

1. Install **SQL Server 2019/2022** on `SQL01`.  
2. Enable **Mixed Mode Authentication**.  
3. Add `SPFarm` as a **SQL sysadmin**.  
4. Verify SQL Server Browser + Agent services are running.  

---

## 🔧 Step 2 – Install SharePoint Prerequisites (SPSE01)

1. On `SPSE01`, run the **SharePoint Prerequisite Installer** from the ISO.  
   - Installs IIS, .NET Framework, Windows Features, and required hotfixes.  
2. Reboot when complete.  

---

## 🔧 Step 3 – Install SharePoint SE (SPSE01)

1. Run `setup.exe` from the SharePoint ISO.  
2. Enter product key (from school’s license).  
3. Install binaries, then **run Configuration Wizard**.  
4. Choose **Create a new farm**.  
5. Connect to `SQL01` using `SPFarm` account.  
6. Define **farm passphrase** (record this securely).  
7. Central Admin URL generated (usually `http://spse01:port`).  

---

## 🔧 Step 4 – Configure Farm

1. Launch **SharePoint Central Administration**.  
2. Run **Farm Configuration Wizard**:  
   - Service accounts = `SPService`.  
   - Create a **Web Application** (e.g., `http://portal.machine.ai`).  
   - Create a **Site Collection** (Team Site).  
3. Verify site collection opens in browser (from domain-joined client).  

---

## 🔧 Step 5 – Integrations (Optional)

- **With AD:**  
  - Use AD Security Groups for SharePoint permissions.  
  - Example: `G_Finance` → given access to Finance Document Library.  

- **With Exchange (EXCH01):**  
  - Configure email integration for alerts/workflows.  
  - Set SMTP = `EXCH01 (10.10.20.50)`.  

- **With Microsoft 365 (Hybrid):**  
  - Deploy **Hybrid Search** connector.  
  - Configure **App Registration** in Azure AD.  
  - Enable cloud index for on-prem content.  

---

## 🧪 Verification Checklist

- [ ] SQL01 reachable from SPSE01 (`ping sql01.machine.ai`).  
- [ ] Central Admin site loads on SPSE01.  
- [ ] Team Site created at `http://portal.machine.ai`.  
- [ ] Domain users can log in with AD accounts.  
- [ ] (Optional) Alerts sent via EXCH01.  
- [ ] (Optional) Hybrid search results appear in Microsoft 365.  

---

## ✅ Lab Value

- Demonstrates **multi-tier enterprise app deployment** (SQL + SharePoint).  
- Shows **integration of SharePoint with AD DS, Exchange, and Microsoft 365**.  
- Portfolio-ready proof of:  
  - Domain integration  
  - Collaboration platform deployment  
  - Hybrid enterprise architecture  


