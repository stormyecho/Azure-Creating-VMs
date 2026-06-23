# ☁️ Azure VM Provisioning via Cloud Shell & PowerShell CLI

> **Lab Objective:** Provision and decommission a Windows Virtual Machine in Microsoft Azure entirely through the command line — no GUI clicks required after initial setup.

---

## 📋 Overview

This project demonstrates core Azure infrastructure skills by creating and managing a Virtual Machine using the **Azure Cloud Shell** and **PowerShell cmdlets**. It reflects a real-world workflow where engineers automate resource provisioning rather than relying on the portal UI.

**Skills demonstrated:**
- Azure Cloud Shell configuration (storage account + file share)
- Azure CLI and PowerShell cmdlet execution
- VM provisioning with custom credentials
- Infrastructure teardown and resource validation

---

## 🎯 Lab Objectives

| # | Objective | Status |
|---|-----------|--------|
| 1 | Configure Cloud Shell | ✅ |
| 2 | Execute Azure CLI Commands | ✅ |
| 3 | Create a Virtual Machine via CLI | ✅ |
| 4 | Run PowerShell cmdlets | ✅ |
| 5 | Remove the Virtual Machine | ✅ |

---

## 🛠️ Prerequisites

- Active Microsoft Azure subscription
- Access to [Azure Portal](https://portal.azure.com)
- A Resource Group already provisioned
- Basic familiarity with PowerShell syntax

---

## 🚀 Walkthrough

### 1. Configure Cloud Shell

Before launching the Azure Cloud Shell, a **Storage Account** and a **File Share** are required — Azure uses these to persist your shell session data.

**Steps:**
1. In the Azure Portal, create a **Storage Account**
2. Inside the Storage Account blade, navigate to **File Shares** and create a new share
3. Click the **Cloud Shell icon** (top nav bar) → select **PowerShell**
4. Under **Advanced Settings**, enter your Storage Account name and File Share name
5. Cloud Shell initializes and is ready for use

---

### 2. Execute Azure CLI Commands

With Cloud Shell running, we can immediately query existing resources to confirm our environment:

```powershell
# List Resource Groups
Get-AzResourceGroup

# List Storage Accounts
Get-AzStorageAccount

# List Virtual Machines (empty at this stage)
Get-AzVM
```

> **Expected output:** Your RG and Storage Account appear. `Get-AzVM` returns nothing — no VMs exist yet.

---

### 3. Create a Virtual Machine

Reference: [Microsoft Docs — Quickstart: Create a Windows VM with PowerShell](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-powershell)

Using a subset of the recommended parameters (no VNet required for this lab), set credentials and create the VM:

```powershell
# Define credentials
$cred = Get-Credential

# Create the VM
New-AzVM `
  -ResourceGroupName "<your-resource-group>" `
  -Name "<your-vm-name>" `
  -Credential $cred
```

> 💡 The backtick `` ` `` character is used for line continuation in PowerShell — it makes multi-parameter commands significantly more readable.

Azure will provision the VM and return a success status upon completion.

---

### 4. Verify the VM

Confirm the VM exists via CLI and through the Portal:

```powershell
# Verify via CLI
Get-AzVM

# Or check in the Azure Portal under: Virtual Machines > [your VM]
```

---

### 5. Remove the Virtual Machine

Teardown mirrors the creation process — swap `New-AzVM` for `Remove-AzVM`:

```powershell
Remove-AzVM `
  -ResourceGroupName "<your-resource-group>" `
  -Name "<your-vm-name>"
```

After the operation completes, run `Get-AzVM` again to confirm the VM no longer exists.

---

## 💡 Key Takeaways

**Using variables for repeated parameters** — Rather than copy-pasting resource group names and subscription IDs throughout your session, assign them to variables upfront:

```powershell
$rg = "myResourceGroup"
$location = "EastUS"
$vmName = "myVM"

New-AzVM -ResourceGroupName $rg -Name $vmName -Credential $cred
Remove-AzVM -ResourceGroupName $rg -Name $vmName
```

This approach reduces typos, improves readability, and mirrors how production scripts are structured.

**Line continuation with backticks** — The `` ` `` character splits long commands across multiple lines without breaking execution. Essential for keeping infrastructure scripts legible.

---

## 📁 Project Structure

```
azure-vm-provisioning/
│
├── README.md          # This file
└── scripts/
    └── create-vm.ps1  # (Optional) Parameterized VM creation script
```

---

## 🔗 References

- [Azure Cloud Shell Overview](https://learn.microsoft.com/en-us/azure/cloud-shell/overview)
- [Quickstart: Create a Windows VM with PowerShell](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-powershell)
- [Az PowerShell Module Reference](https://learn.microsoft.com/en-us/powershell/azure/)

---

## 👤 Author

Built as part of a hands-on Azure cloud engineering portfolio. This lab targets foundational IaaS skills that underpin larger infrastructure automation workflows.
