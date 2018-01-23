---
title: 'Troubleshoot Azure Backup failure: Guest Agent Status Unavailable | Microsoft Docs'
description: 'Symptoms, causes, and resolutions of Azure Backup failures related to agent, extension, disks'
services: backup
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
keywords: Azure backup; VM agent; Network connectivity; 

ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: genli;markgal;sogup;
---

# Troubleshoot Azure Backup failure: Issues with agent and/or extension

This article provides troubleshooting steps to help you resolve Backup failures related to problems in communication with VM agent and extension.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## VM Agent unable to communicate with Azure Backup

> [!NOTE]
> If your Azure Linux VM backups started failing with this error on or after January 4th, 2018, run the following command in the affected VMs and retry the backups

	sudo rm -f /var/lib/waagent/*.[0-9]*.xml

After you register and schedule a VM for the Azure Backup service, Backup initiates the job by communicating with the VM agent to take a point-in-time snapshot. Any of the following conditions might prevent the snapshot from being triggered, which in turn can lead to Backup failure. Follow below troubleshooting steps in the given order and retry your operation.

##### Cause 1: [The VM has no Internet access](#the-vm-has-no-internet-access)
##### Cause 2: [The agent is installed in the VM but is unresponsive (for Windows VMs)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### Cause 3: [The agent installed in the VM is out of date (for Linux VMs)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### Cause 4: [The snapshot status cannot be retrieved or a snapshot cannot be taken](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### Cause 5: [The backup extension fails to update or load](#the-backup-extension-fails-to-update-or-load)
##### Cause 6: [Azure Classic VMs may require additional step to complete registration](#azure-classic-vms-may-require-additional-step-to-complete-registration)

## Snapshot operation failed due to no network connectivity on the virtual machine
After you register and schedule a VM for the Azure Backup service, Backup initiates the job by communicating with the VM backup extension to take a point-in-time snapshot. Any of the following conditions might prevent the snapshot from being triggered, which in turn can lead to Backup failure. Follow below troubleshooting steps in the given order and retry your operation.
##### Cause 1: [The VM has no Internet access](#the-vm-has-no-internet-access)
##### Cause 2: [The snapshot status cannot be retrieved or a snapshot cannot be taken](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### Cause 3: [The backup extension fails to update or load](#the-backup-extension-fails-to-update-or-load)

## VMSnapshot extension operation failed

After you register and schedule a VM for the Azure Backup service, Backup initiates the job by communicating with the VM backup extension to take a point-in-time snapshot. Any of the following conditions might prevent the snapshot from being triggered, which in turn can lead to Backup failure. Follow below troubleshooting steps in the given order and retry your operation.
##### Cause 1: [The snapshot status cannot be retrieved or a snapshot cannot be taken](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### Cause 2: [The backup extension fails to update or load](#the-backup-extension-fails-to-update-or-load)
##### Cause 3: [The VM has no Internet access](#the-vm-has-no-internet-access)
##### Cause 4: [The agent is installed in the VM but is unresponsive (for Windows VMs)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### Cause 5: [The agent installed in the VM is out of date (for Linux VMs)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)

## Unable to perform the operation as the VM Agent is not responsive

After you register and schedule a VM for the Azure Backup service, Backup initiates the job by communicating with the VM backup extension to take a point-in-time snapshot. Any of the following conditions might prevent the snapshot from being triggered, which in turn can lead to Backup failure. Follow below troubleshooting steps in the given order and retry your operation.
##### Cause 1: [The agent is installed in the VM but is unresponsive (for Windows VMs)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### Cause 2: [The agent installed in the VM is out of date (for Linux VMs)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### Cause 3: [The VM has no Internet access](#the-vm-has-no-internet-access)

## Backup failed with an internal error - Please retry the operation in a few minutes

After you register and schedule a VM for the Azure Backup service, Backup initiates the job by communicating with the VM backup extension to take a point-in-time snapshot. Any of the following conditions might prevent the snapshot from being triggered, which in turn can lead to Backup failure. Follow below troubleshooting steps in the given order and retry your operation.
##### Cause 1: [The VM has no Internet access](#the-vm-has-no-internet-access)
##### Cause 2: [The agent installed in the VM but unresponsive (for Windows VMs)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### Cause 3: [The agent installed in the VM is out of date (for Linux VMs)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### Cause 4: [The snapshot status cannot be retrieved or a snapshot cannot be taken](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### Cause 5: [The backup extension fails to update or load](#the-backup-extension-fails-to-update-or-load)
##### Cause 6: [Backup service does not have permission to delete the old restore points due to Resource Group lock](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)

## The specified Disk configuration is not supported

> [!NOTE]
> We have a private preview to support backups for VMs with >1TB disks. For details refer to [Private preview for large disk VM backup support](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)
>
>

Currently Azure Backup doesn’t support disk sizes [greater than 1023GB](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm). 
- If you have disks greater than 1 TB , [attach new disks](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) which are less than 1 TB <br>
- Then, copy the data from disk greater than 1TB into newly created disk(s) of size less than 1TB. <br>
- Ensure that all data has been copied and remove the disks greater than 1TB
- Initiate the backup

## Causes and Solutions

### The VM has no Internet access
Per the deployment requirement, the VM has no Internet access, or it has restrictions in place that prevent access to the Azure infrastructure.

To function correctly, the backup extension requires connectivity to the Azure public IP addresses. The extension sends commands to an Azure Storage endpoint (HTTP URL) to manage the snapshots of the VM. If the extension has no access to the public Internet, Backup eventually fails.

####  Solution
To resolve the issue, try one of the methods listed here.
##### Allow access to the Azure storage corresponding to the region

You can allow connections to storage of the specific region by using [service tags](../virtual-network/security-overview.md#service-tags). Make sure that the rule that allows access to the storage account has higher priority than the rule that blocks internet access. 

![NSG with storage tags for a region](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Storage service tags are available only in specific regions and are in preview. For a list of regions, see [Service tags for Storage](../virtual-network/security-overview.md#service-tags).

##### Create a path for HTTP traffic to flow

1. If you have network restrictions in place (for example, a network security group), deploy an HTTP proxy server to route the traffic.
2. To allow access to the Internet from the HTTP proxy server, add rules to the network security group, if you have one.

To learn how to set up an HTTP proxy for VM backups, see [Prepare your environment to back up Azure virtual machines](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

In case you are using Managed Disks, you may need an additional port (8443) opening up on the firewalls.

### The agent installed in the VM but unresponsive (for Windows VMs)

#### Solution
The VM Agent might have been corrupted or the service might have been stopped. Re-installing the VM agent would help get the latest version and restart the communication.

1. Verify whether Windows Guest Agent service running in services (services.msc) of the Virtual Machine. Try restart the Windows Guest Agent service and initiate the Backup<br>
2. if it is not visible in services, verify in Programs and Features whether Windows Guest agent service is installed.
4. If you are able to view in programs and features uninstall the Windows Guest Agent.
5. Download and install the [latest version of agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). You need Administrator privileges to complete the installation.
6. Then you should be able to view Windows Guest Agent services in services
7. Try running an on-demand/adhoc backup by clicking "Backup Now" in the portal.

Also verify your Virtual Machine has **[.NET 4.5 installed in the system](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)**. It is required for the VM agent to communicate with the service

### The agent installed in the VM is out of date (for Linux VMs)

#### Solution
Most agent-related or extension-related failures for Linux VMs are caused by issues that affect an outdated VM agent. To troubleshoot this issue, follow these general guidelines:

1. Follow the instructions for [updating the Linux VM agent](../virtual-machines/linux/update-agent.md).

 >[!NOTE]
 >We *strongly recommend* that you update the agent only through a distribution repository. We do not recommend downloading the agent code directly from GitHub and updating it. If the latest agent is unavailable for your distribution, contact distribution support for instructions on how to install it. To check for the most recent agent, go to the [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) page in the GitHub repository.

2. Make sure that the Azure agent is running on the VM by running the following command: `ps -e`

 If the process is not running, restart it by using the following commands:

 * For Ubuntu: `service walinuxagent start`
 * For other distributions: `service waagent start`

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Run a new test backup. If the failure persists, please collect the following logs from the customer’s VM:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

If we require verbose logging for waagent, follow these steps:

1. In the /etc/waagent.conf file, locate the following line: **Enable verbose logging (y|n)**
2. Change the **Logs.Verbose** value from *n* to *y*.
3. Save the change, and then restart waagent by following the previous steps in this section.

### The snapshot status cannot be retrieved or a snapshot cannot be taken
The VM backup relies on issuing a snapshot command to the underlying storage account. Backup can fail either because it has no access to the storage account or because the execution of the snapshot task is delayed.

#### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| --- | --- |
| The VM has SQL Server backup configured. | By default, the VM backup runs a VSS full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server backup is configured, snapshot execution delays may occur.<br><br>If you are experiencing a Backup failure because of a snapshot issue, set the following registry key:<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE"** |
| The VM status is reported incorrectly because the VM is shut down in RDP. | If you shut down the VM in Remote Desktop Protocol (RDP), check the portal to determine whether the VM status is correct. If it’s not correct, shut down the VM in the portal by using the **Shutdown** option on the VM dashboard. |
| The VM cannot get the host/fabric address from DHCP. | DHCP must be enabled inside the guest for the IaaS VM backup to work.  If the VM cannot get the host/fabric address from DHCP response 245, it cannot download or run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. For more information, see [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md). |

### The backup extension fails to update or load
If extensions cannot be loaded, Backup fails because a snapshot cannot be taken.

#### Solution

**For Windows guests:**
Verify that the iaasvmprovider service is enabled and has a startup type of *automatic*. If the service is not configured in this way, enable it to determine whether the next backup succeeds.

**For Linux guests:**
Verify the latest version of VMSnapshot for Linux (the extension used by Backup) is 1.0.91.0.<br>


If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

To uninstall the extension, do the following:

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **Uninstall**.

This procedure causes the extension to be reinstalled during the next backup.

### Backup service does not have permission to delete the old restore points due to Resource Group lock
This issue is specific to managed VMs where user locks the Resource Group and Backup service is not able to delete the older restore points. Due to this new backups start failing as there is a limit of maximum 18 restore points imposed from the backend.

#### Solution

To resolve the issue, please use the following steps to remove the restore point collection: <br>
 
1. Remove the Resource Group lock in which the VM resides 
	 
2. Install ARMClient using Chocolatey <br>
   https://github.com/projectkudu/ARMClient
	 
3. Login to ARMClient <br>
		  	 `.\armclient.exe login`
		 
4. Get Restore Point collection corresponding to the VM <br>
   	`.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Example: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
			 
5. Delete the Restore Point Collection <br>
		  	`.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
 
6. Next scheduled backup will automatically create restore point collection and new restore points 
 
7. The problem will re-appear if you lock the Resource Group again as there is only a limit of 18 restore points after which the backups start failing 

