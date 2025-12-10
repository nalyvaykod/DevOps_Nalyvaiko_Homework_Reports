### Lab 1. Manage Microsoft Entra ID Identities

**User Creation:**
Created the internal user az104-user1 and invited an external Guest User.
Assigned Job Title and Department attributes to both.
**Proof:
Group Setup:**
Created a Security Group named IT Lab Administrators with an Assigned
membership type. Added az104-user1 and the Guest User to the new group.
**Owner:** nalyvaykod@outlook.com


Proof:


### Lab 2a. Manage Subscriptions and RBAC

**Management Groups:**
Created the management group az104-mg1 for subscription aggregation and
centralized governance.
**Proof:
Built-in Role Assignment:**
Assigned the Virtual Machine Contributor built-in role to the helpdesk group at
the management group scope.
**Proof:**


**Custom Role Creation:**
Created the custom role Custom Support Request by cloning the Support
Request Contributor role and excluding the support resource provider
registration permission (NotAction).
**Proof:
Monitoring:**
Verified the Activity Log to monitor the role assignments made.
**Proof:**


### Lab 2b. Manage Governance via Azure Policy

**RG Tagging:**
Created resource group az104-rg2 and assigned the tag Cost Center: 000.
**Proof:**


**Enforce Tagging (Deny):**
Assigned the Require a tag, policy to prevent resource creation without the
mandatory tag. Test: SA creation without tag was denied.
**Proof:
Apply Tagging (Modify):**
Deleted the Deny policy. Assigned the Inherit a tag from the resource group if
missing policy (with Modify effect and a remediation task). Test: SA creation
without tag succeeded, tag was inherited automatically.
**Proof:**


**Resource Lock:**
Applied a Delete lock (rg-lock) to the az104-rg2 resource group. Test: Attempted RG
deletion was denied.


**Proof:**


### Lab 3. Manage Azure resources by using Azure Resource

#### Manager Templates

**ARM (Portal) - Export:**
Created az104-disk1 (Standard HDD, 32 GiB) via the portal and exported it as a
JSON.
**Proof:
ARM (Portal) - Redeployment:**
Edited the exported template (JSON) and deployed az104-disk2 via the Deploy a
custom template interface.
**Proof:**


**ARM (PowerShell):**
Configured Cloud Shell. Deployed az104-disk3 using
theNew-AzResourceGroupDeployment command.
**Proof:
ARM (CLI):**
Deployed az104-disk4 using the az deployment group create command.
Proof:


**Bicep:**
Deployed az104-disk5 (Standard SSD, 32 GiB) using the azuredeploydisk.bicep file
and the az deployment group create command.
**Proof:**


### Lab 4. Implement Virtual Networking

**VNet Creation (Portal):**
Created CoreServicesVnet with subnets SharedServicesSubnet and
DatabaseSubnet. Template was exported.
**Proof:
VNet Creation (Template):**
Modified the exported JSON template to create ManufacturingVnet (10.30.0.0/16)
with subnets SensorSubnet1 (10.30.20.0/24) and SensorSubnet2 (10.30.21.0/24).
**Proof:**


**Network Security (NSG/ASG):**
Created asg-web. Created NSG myNSGSecure and associated it with
SharedServicesSubnet.
**Configured:**
a. Inbound rule: AllowASG (Priority 100) allowing traffic from asg-web on ports
80, 443.
b. Outbound rule: DenyInternetOutbound (Priority 4096) denying all outbound
traffic to the Internet.
Proof:



**DNS Zones:**
Created a Public DNS Zone (contoso-test.com) with an A record (www). Created a
Private DNS Zone (private.contoso.com), linked to ManufacturingVnet, with an A
record (sensorvm).
**Proof:**


### Lab 5. Implement Intersite Connectivity

**VNet and VM 1 Creation:**
Сreated CoreServicesVnet (address space: 10.0.0.0/16) and CoreServicesVM (in the
Core subnet with range 10.0.0.0/24).
**VNet and VM 2 Creation:**
Created ManufacturingVnet (address space: 172.16.0.0/16) and ManufacturingVM
(in the Manufacturing subnet with range 172.16.0.0/24).
Proof:


**Testing (Pre-Peering):**
Used Network Watcher Connection troubleshoot to verify TCP connectivity on
port 3389 between the VMs. Result: Unreachable, as expected, since the networks
are isolated by default.
Proof:


**Peering Configuration:**
Configured a bidirectional peering between CoreServicesVnet and
ManufacturingVnet. Status: Connected.
Proof:
**Testing (Post-Peering):**
Used Run command on ManufacturingVM to execute Test-NetConnection to the
private IP address of CoreServicesVM (on port 3389). Result: Succeeded.
**Proof:**


**User Defined Route (UDR):**
Added perimeter subnet (10.0.1.0/24) to CoreServicesVnet. Created Route Table
rt-CoreServices. Added route PerimetertoCore (Destination 10.0.0.0/16, Next Hop
Type: Virtual appliance, Next Hop Address: 10.0.1.7). Associated the Route Table
with the Core subnet.
Proofs:


# Lab 6. Implement Network Traffic Management

**Azure Load Balancer (Layer 4):**
Deployed a Standard Load Balancer (az104-lb) to distribute TCP traffic (port 80)
across two VMs (vm0, vm1) using round-robin distribution.
Proof:
**Azure Application Gateway/Path-based Routing::**
Deployed an Application Gateway Standard V2 (az104-appgw) with a dedicated
subnet. Configured rules enabling the Application Gateway to route traffic to
distinct backend pools.
**Proofs:**



# Lab 7. Implement Network Traffic Management

**Management Policies:
Created a storage account configured with Geo-redundant storage (GRS) and
initially disabled public network access. Configured a Lifecycle management rule
to move base blobs older than 30 days to the Cool storage tier.
Proofs:**


**Blob Storage Security:**
Created a container data with Private access. Implemented an Immutable policy
(Time-based retention) for 180 days. Confirmed that direct blob URL access failed.
Generated and successfully tested a Shared Access Signature (SAS) URL with
read-only permissions and an expiration time.
**Proofs:**


**Azure File Shares:**
Created a file share share1 and uploaded a file using the Storage Browser.
Created VNet vnet1 and enabled the Microsoft.Storage Service Endpoint.
Configured the Storage Account to allow access only from the selected VNet
subnet, removing the client's public IP address. Attempting to access content via
the Storage Browser (outside the VNet) resulted in an unauthorized message.
**Proofs:**


## Lab 08: Manage Virtual Machines

**Creating 2 vm’s:
Proof:**


**Resizing of az104-vm1:
Resized vm’s storage disk to D2ds-v4:
Proof:
Add updated disk to vm:
Storage type of disk has been changed to Standart SSD instead of Standatd
HDD.
Proof:**


**Create and configure Azure Virtual Machine Scale Sets:**
Proof:
**Scale Azure Virtual Machine Scale Sets:
Proof:**


**Create a virtual machine using Azure PowerShell:
Proof:
Create a virtual machine using Azure CLI:
Proof:**


## Lab 09a: Implement Web Apps

**Create Web App with B1 plan:
Proof:
Create and configure a deployment slot** :
Proof:


**Configure Web App deployment settings:
Proof:
Swap deployment slots:
Proof:**


**Configure and test autoscaling of the Azure Web App:
Proof:**

## Lab 09b: Implement Azure Container Instances

**Deploy Azure Container Instance using Docker image:
Proof:**


**Test and verify deployment of an Azure Container:
Proof:**

## Lab 09c: Implement Azure Container Apps

**Create and configure an Azure Container App and environment:
Proof:**


**Test and verify deployment:
Proof:**

## Lab 10: Implement Data Protection

**Use a template to provision an infrastructure:
Proof:**


**Create and configure a Recovery Services vault;
Configure Azure virtual machine-level backup:
Proofs:**


**Monitor Azure Backup:
Proofs:**


**Enable virtual machine replication:
Proofs:**



## Lab 11: Implement Monitoring

**Configure Azure Monitor for v-machines:
Proof:**


**Create an alert:
Proof:
Configure action group notifications:
Proofs:**


**Trigger an alert and confirm it is working:
Proofs:**



**Configure an alert processing rule:
Proof:**


