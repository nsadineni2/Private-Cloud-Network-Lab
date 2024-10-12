# Secure-VNET-Lab
This project demonstrates setting up a secure Azure Virtual Network with subnets and Network Security Groups
# Azure Virtual Network (VNET) Setup Lab

This project demonstrates the configuration of a secure Azure Virtual Network (VNET) with three subnets and a Network Security Group (NSG). The VNET isolates the Dev department from other cloud resources, and subnets allow for logical segmentation of resources.

## Table of Contents
1. [Scenario Overview](#scenario-overview)
2. [VNET and Subnets Configuration](#vnet-and-subnets-configuration)
3. [Network Security Group Configuration](#network-security-group-configuration)
4. [Troubleshooting](#troubleshooting)
5. [Activity Logging](#activity-logging)
6. [Screenshots](#screenshots)
7. [Automation with CLI/ARM Templates](#automation-with-cliarm-templates)

---

## Scenario Overview

In this lab, we set up a secure Virtual Network (VNET) named `DevDept-vnet`, and within it, deploy three subnets (`subnet-a`, `subnet-b`, and `subnet-c`). We also configure a Network Security Group (NSG) to manage inbound and outbound traffic rules for one of the subnets. Inbound Security Rules with a priority of 100 will also be added to the Network Security Group to allow MySQL.

## VNET and Subnets Configuration

- **VNET Name:** DevDept-vnet
- **Address Space:** 172.16.0.0/16

### Subnets Created:

| **Subnet Name** | **Address Range** | **Subnet Size** |
| --------------- | ----------------- | --------------- |
| subnet-a        | 172.16.1.0         | /24             |
| subnet-b        | 172.16.2.0         | /24             |
| subnet-c        | 172.16.3.0         | /24             |

## Network Security Group Configuration

- **NSG Name:** database-secgrp
- **Region:** East US
- **Associated Subnet:** subnet-a

**Rules:**
- Default inbound and outbound rules apply.

## Troubleshooting

During setup, a region mismatch caused the subnets to not appear in the NSG association dropdown. The NSG was created in Central US while the VNET was in East US. This issue was resolved by ensuring both resources were created in the same region.

## Activity Logging

To monitor changes and ensure accountability, the activity log was configured. This feature is useful for auditing network configurations and monitoring uptime.

---

## Screenshots

Here are key screenshots taken during the VNET setup process.

### 1. VNET Creation
![VNET Creation](https://github.com/user-attachments/assets/ba95c9dc-f596-4843-8dfe-b0dc08849b5b)
![Created](https://github.com/user-attachments/assets/78748667-47ec-427d-be71-76580057505e)


### 2. Subnet Configuration
![Subnet Configuration](https://github.com/user-attachments/assets/92a585b6-b201-4fa6-b1c6-51a9c9af4856)


### 3. Network Security Group Creation
![NSG Creation](https://github.com/user-attachments/assets/07f46049-fd4b-463a-a31f-bf4e2605d8aa)


### 4. Associating NSG with Subnet
![NSG Association](https://github.com/user-attachments/assets/f9f659aa-486c-44e7-84a1-f2b78a980579)

### 5. Adding Inbound Security Rules to allow MySQL
![Add MySQL Inbound Rules](https://github.com/user-attachments/assets/eb34c327-0e13-452a-b433-971425092683)

### 6. Activity Log for the VNET
![Activity Log](https://github.com/user-attachments/assets/b9cc2d16-5e94-48d8-9888-4378962d6f1a)


---

## Automation with CLI/ARM Templates

Below is an example of how this setup can be automated using Azure CLI:

```bash
# Create Resource Group
az group create --name DevDept-rg --location eastus

# Create VNET
az network vnet create --resource-group DevDept-rg --name DevDept-vnet --address-prefix 172.16.0.0/16 --location eastus

# Create Subnets
az network vnet subnet create --resource-group DevDept-rg --vnet-name DevDept-vnet --name subnet-a --address-prefix 172.16.1.0/24
az network vnet subnet create --resource-group DevDept-rg --vnet-name DevDept-vnet --name subnet-b --address-prefix 172.16.2.0/24
az network vnet subnet create --resource-group DevDept-rg --vnet-name DevDept-vnet --name subnet-c --address-prefix 172.16.3.0/24

# Create NSG
az network nsg create --resource-group DevDept-rg --name database-secgrp --location eastus

# Associate NSG to subnet-a
az network vnet subnet update --vnet-name DevDept-vnet --name subnet-a --resource-group DevDept-rg --network-security-group database-secgrp

