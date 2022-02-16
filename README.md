# intelliflash-sra-plugin

## Overview of IntelliFlash SRA
The IntelliFlash Storage Replication Adapter (SRA) plugin that works with the VMware vCenter Site Recovery Manager (SRM) and enables you to plan, test, and implement array-based replication in VMware environments for disaster recovery. You can use IntelliFlash arrays as replication partners in the Site Recovery Manager environment.

You need to install the IntelliFlash SRA plugin on your VMware Site Recovery Manager Servers.

### Project-level replication
IntelliFlash replicates data at the project level. A replicated project on a target storage array is called a **Replica** project.

Project-level replication simplifies the process of managing replication relationships. When a project is replicated, all the shares and LUNs within the project are replicated to the target storage array.

When you configure a replication relationship in the IntelliFlash Web UI for a VMware Site Recovery Manager environment, you must select **SRM Partner** as the Replication role and include all shares and LUNs within that project.

**Note**:
- You must configure a replication relationship before configuring the SRM setup.
- All datasets (shares/LUNs) that need to be included in the SRM replication group must be kept in one project.

In the IntelliFlash Web UI, replication relationships are configured for projects using the Replication Configuration Wizard which is accessed under:

**Provision > Projects > Manage > Data Protection > Replication.**

### Array GUID for VMware Site Recovery Manager setups
IntelliFlash SRA uses the GUID of the IntelliFlash arrays as the Array ID for communicating with Site Recovery Manager and when pairing with another SRA.

You can view the IntelliFlash array GUID from the **System Information** panel, under the **Array Information** section of the **Information** tab. The array GUID helps you to identify the correct array when pairing with another VMware Site Recovery Manager.

The *System Information* panel is available by clicking the array name at the top right corner of the IntelliFlash Web UI

For more information about VMware Site Recovery Manager, refer to the VMware product documentation.

- [https://docs.vmware.com/en/Site-Recovery-Manager/index.html](https://docs.vmware.com/en/Site-Recovery-Manager/index.html)

## Predefined SRA Role for Site Recovery Manager
A predefined SRA role for VMware Site Recovery Manager environments is available in the IntelliFlash Web UI. You can add a user account in the IntelliFlash Web UI, assign the SRA role for the user account, then use the SRA role user account when configuring the Array Manager in the VMware SRM Server. You can add a User Account for the SRA role under:

**Settings > Administration > Management Access > Local Admins.**

## Prerequisites for IntelliFlash SRA
You will need to meet the following prerequisites before you configure IntelliFlash SRA.

### Hardware
Two IntelliFlash arrays – one array for the protected site and another array for the recovery site.

### IntelliFlash Operating Environment
- IntelliFlash 3.11.0 or higher is installed on your IntelliFlash arrays
- IntelliFlash SRA 2.0.0 plugin is installed on both Site Recovery Manager servers for the protected and recovery sites

### Supported VMware ESXi
- VMware ESXi 6.7 or later – Installed on both the protected site and recovery site
- VMware vCenter 6.7 or later – Installed on both the protected site and recovery site
- For VMFS 5 datastores you must disable the VAAI ATS heartbeat.

You can disable this using the ESXi CLI. Use the following command to disable the VAAI ATS heartbeat:

 *# esxcli system settings advanced set -i 0 -o /VMFS3/UseATSForHBOnVMFS5*
 
### Supported VMware Site Recovery Manager Versions
 - VMware Site Recovery Manager (Photon) 8.2 and above
 
## Configuring IntelliFlash SRA on Site Recovery Manager
You need to complete the following configuration before configuring IntelliFlash SRA on the Site Recovery Manager Servers (protected and recovery sites):
- On the IntelliFlash Web UI, set up a replication relationship between the replication source array and replication target array and assign the SRM Partner replication role to it.
- Install SATP Rules.

You need to install Tegile SATP rules for iSCSI and FC on the ESXi Servers of the protected and recovery sites.You can install the rules from the IntelliFlash Manager plugin and from the ESXi CLI. To install Tegile SATP rules using the IntelliFlash Manager plugin, you should add your vCenter Servers and register them on the IntelliFlash arrays.

- If you are using iSCSI, you must add iSCSI initiators of the ESXi Servers on both the replication source and target arrays.
- You must add iSCSI target details of both the replication source and replication target on their respective ESXi Servers.
- You must create datastores and VMs on your ESXi Servers.

## Installing VMware Site Recovery Manager Server and Connecting Site Recovery Manager Instances on Protected and Recovery Sites

You must install the supported VMware Site Recovery Manager Server on a Windows system at the protected site and recovery site. The installed Site Recovery Manager Servers must be able to connect to the respective vCenter Server at the protected and recovery sites.

After installing the VMware Site Recovery Manager Server, the Site Recovery Manager plugin appears in the vSphere Web Client. You can use the plugin in the vSphere Web Client to configure and manage the Site Recovery Manager Server.

After installing a supported VMware Site Recovery Manager version (VMware Site Recovery Manager 8.2 and above) on both the protected site and recovery site, you must pair the sites.

Refer to the VMware documentation for prerequisites and detailed instructions on how to install the VMware Site Recovery Manager Server.

- [https://docs.vmware.com/en/Site-Recovery-Manager/index.html](https://docs.vmware.com/en/Site-Recovery-Manager/index.html)

## Installing Containerized IntelliFlash SRA 2.0.0 for SRM 8.2
The IntelliFlash SRA plugin allows you to use the SRM to make recovery plans for VMs on replicated IntelliFlash storage arrays. Install SRA plugin on both the sites that have the SRMs installed.

**Prerequisite:** Install the two SRMs and then pair them.

1. Download the containerized IntelliFlash SRA 2.0.0 plugin.   
To download the containerized IntelliFlash SRA 2.0.0 plugin, perform the following steps:
    1. Go to [https://github.com/DDNStorage/intelliflash-sra-plugin](https://github.com/DDNStorage/intelliflash-sra-plugin) in a web browser.
    2. Navigate to the **IntelliFlash-Containerized-SRA.2.0.0.tar.gz** file
    3. Click **Download** button and save it to a directory of your choice
    4. Save the **SRA Plugin** in the required folder on your host system.  
    The downloaded file is a .tar.gz file.  
2. Install the containerized IntelliFlash SRA plugin in the paired SRM sites.   
To install the containerized IntelliFlash SRA plugin, perform the following steps:
    1. Log in to the **vSphere Client**.
    2. Click **Menu** and select **Site Recovery**.
    3. In the **Site Recovery** page, click **Configure**.   
    This takes you to **VMware SRM Appliance Management** site.
    4. Log in to the **SRM Appliance Management** site.
    5. In the left navigation pane, select **Storage Replication Adapters** and then click **New Adapter**.   
    6. Click **Upload** and then choose IntelliFlash SRA 2.0.0 installer (.tar.gz file) downloaded earlier.  
    You will receive a notification on SRA upload.
    7. Repeat the above steps on the other SRM site.

This installs IntelliFlash SRA plugin in both the SRM sites and you discover SRA on both the sites. If you do not discover SRA, then rescan.

## Configuring Array Managers in VMware Site Recovery Manager (SRM)
Add IntelliFlash arrays to your SRM Server so that the SRM Server can discover replicated devices, compute datastore groups, and initiate storage operations. You can use the VMware vSphere Web client to add both IntelliFlash arrays at the same time.

Depending on the version of the VMware Site Recovery Manager (SRM) you are using, some of the steps might vary. Refer to VMware product documentation for additional information.

### Prerequisites
- IntelliFlash SRA Plugin is installed.
- After setting up the replication relationship with the SRM Partner replication role, you must ensure that one successful run of replication is complete before adding IntelliFlash arrays to the SRM.

To configure Array Managers, complete the following steps:
1. Log in to the **VMware vSphere Web Client** of your protected or recovery site (you can log into either one).
2. In the left hand side navigator, click **Site Recovery**.
3. In the **Site Recovery** panel, click **Sites**.
4. Right-click a site and select **Add Array Manager**.
5. In the **Add Array Manager** window, select **Add a pair of array managers** and click **Next**.
6. In the **Location** page, click **Next**.
7. Select the **IntelliFlash Storage Replication Adapter** from the **SRA Type** dropdown and click **Next**.
8. In the **Configure array manager** page, complete the following steps:
    1. Type a name for the array in the **Display Name** text box.
    2. Type the array management IP address of the replication source system in the **Host** text box.
    3. Type the **Username** of the replication source system.   
    The Username can be the IntelliFlash admin username or the SRA role user account created in the IntelliFlash Web UI.
    4. Type **Password** for the replication source system.
    5. Click **Next**.
9. In the **Configure paired array manager** page, complete the following steps:
    1. Type a name in the **Display Name** text box.
    2. Type the Array management IP address of replication target system in the **Host** text box.
    3. Type the **Username** of the replication target system.
    4. Type **Password** for the replication target system.
    5. Click **Next**.
10. In the **Enable array pairs** page, select Array pair.
11. Review the configuration and click **Finish**.

## Discovering Devices in Site Recovery Manager
You can rescan arrays if you made changes in the IntelliFlash configuration or added new arrays.

**Note**: Every time you add a LUN and mount it, you must rerun replication and rescan.

Depending on the version of the VMware Site Recovery Manager (SRM) you are using, some of the steps might vary. Refer to VMware product documentation.

To rescan IntelliFlash arrays, complete the following steps:
1. In the vSphere Web Client, click **Home > Recovery Site > Protected and Recovery Site > Array Based Replication**.
2. Select an array
3. In the **Manage** tab, select **Array Pairs**.
4. Right-click an array pair and select **Discover Devices** to rescan the arrays and recompute the datastore groups.

## Create,Test, and Run Recovery Plan
After configuring array managers and discovering devices in VMware Site Recovery Manager, you should create a protection group, a recovery plan, test the recovery plan, clean up the test, and then test a recovery plan for planned recovery and disaster recovery. You can also reprotect after a planned or disaster recovery.

For more information about VMware Site Recovery Manager, refer to the VMware product documentation.
- [https://docs.vmware.com/en/Site-Recovery-Manager/index.html](https://docs.vmware.com/en/Site-Recovery-Manager/index.html)

## Create Array Based Replication Protection Group
When you create a protection group, the **Protection group** type page of the **Create Protection Group** wizard of the **SRM Server** displays the IntelliFlash array pair in the **Array pair** section. In the wizard, you must select the **Array Based Replication** option.

## Create a Recovery Plan
When you create a recovery plan, you should select the recovery site that has the replication target array added.

## Perform a Planned Recovery or Disaster Recovery
When you perform a planned recovery, IntelliFlash takes the latest snapshot from the IntelliFlash array on the protected site array and replicates it to the replication target array on the recovery site. It also moves the project from the **Local** to the **Replica** tab of the project pane and stops the data service.

On the recovery site array, IntelliFlash moves the project from the **Replica** to the **Local** tab of the project pane and starts the data service.

When you perform a disaster recovery, on the recovery site array, IntelliFlash moves the project from the **Replica** to the **Local** tab of the project pane and starts the data service.

## Reprotect After Recovery
The reprotect operation reverses the replication direction, and the replication exits the suspended state and allows a future replication to execute.

Your recovery site becomes the new protected site and your replication target array becomes your replication source array.

## Uninstalling Containerized IntelliFlash SRA 2.0.0 for SRM (Photon) 8.2 and above
1. Log in to the **vSphere Client**.
2. Click **Menu** and select **Site Recovery**.
3. In the **Site Recovery** panel, click **Configure**.  
This takes you to **VMware SRM Appliance Management** site.
4. Log in to the **SRM Appliance Management** site
5. In the left navigation pane, select **Storage Replication Adapters**.
6. Click **(More)** icon  in the **IntelliFlash Storage Replication Adapter** section and choose **Delete**.
7. In the **Delete Adapter** dialog box, select both the check boxes and then click **Delete**.

This deletes the IntelliFlash Storage Replication Adapter. You will see a confirmation message after the SRA is deleted.
