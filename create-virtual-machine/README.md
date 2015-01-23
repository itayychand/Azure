Infrastructure As A Service in Microsoft Azure
=======================================================================================

TBC

In this lab, you will TBC

This lab includes the following tasks:

* [Configuring your computer](#setup)
* [Creating a Virtual Machine using the Preview Portal](#creating-a-vm-using-portal)
* [Creating a Virtual Machine using the Cross-Platform Command-Line Interface](#creating-a-vm-using-cli)
* [Creating a Virtual Machine using Powershell](#creating-a-vm-using-powershell)
* [Setting up the environment using a RunBook](#setting-up-the-environment-with-RunBook)
* [Appendix - Cleanup](#cleanup)

<a name="setup" />
## Configuring your computer ##
In this task you will learn how to configure your computer for running the tasks in this lab that require so. Following the instructions in each configuration section right before running each creation task might prove easier than following them sequentially in the beginning.

* [Configuring your computer for creating a virtual machine using the Cross-Platform Command Line](#configuration-xplatcli)
* [Configuring your computer for creating a virtual machine using PowerShell](#configuration-powershell)

<a name="configuration-xplatcli">
### Configuring your computer for creating a virtual machine using the Cross-Platform Command Line 
The prerequisites for executing the steps listed in the section [Creating a Virtual Machine using the Cross-Platform Command-Line Interface](#creating-a-vm-using-cli) are:

* [Azure Cross-Platform Command Line Interface](#install-the-xplat-cli) installed
* [Azure subscription configured in xplat-cli](#configuring-your-azure-subscription-in-the-xplat-cli)

<a name="install-the-xplat-cli" />
#### Installing the Azure Cross-Platform Command-Line Interface

There are two ways to install the **Azure Cross-Platform Command-Line Interface** (or xplat-cli): using installer packages for Windows and OS X, or if Node.js is installed on your system, the **npm** command. 

Once the xplat-cli has been installed, you will be able to use the **azure** command from your command-line interface (Bash, Terminal, Command prompt) to access the xplat-cli commands.

In order to install the **Azure Cross-Platform Command-Line Interface** using the installer, just download the package for your platform and follow the instructions. The following installer packages are available:

* [Windows installer](http://go.microsoft.com/?linkid=9828653&clcid=0x409)

* [OS X installer](http://go.microsoft.com/fwlink/?linkid=252249&clcid=0x409)

	> **Note:** If Node.js is already installed on your system, use the following command to install the xplat-cli:

	> ````
	> npm install azure-cli -g
	> ````

<a name="connect-to-your-azure-subscription" />
#### Configuring your Azure subscription in the xplat-cli##

While some commands provided by the xplat-cli will work without an Azure subscription, most commands require a subscription. To configure the xplat-cli to work with your subscription you can either download and use a publish settings file or log in to Azure using an organizational account. When you log in, Azure Active Directory is used to authenticate the credentials.

To help you choose the authentication method that's appropriate for your needs, consider the following:

*  The log in method can make it easier to manage access to subscription, but may disrupt automation, as the credentials may time out and require you to log in again.

*  The publish settings file method installs a certificate that allows you to perform management tasks for as long as the subscription and the certificate are valid. This method makes it easier to use automation for long-running tasks. After you download and import the information, you don't need to provide it again. However, this method makes it harder to manage access to a subscription as anyone with access to the certificate can manage the subscription.

For more information about authentication and subscription management, see ["What's the difference between account-based authentication and certificate-based authentication"](http://msdn.microsoft.com/en-us/library/windowsazure/hh531793.aspx#BKMK_AccountVCert).

In order to use the publish settings file method, perform the following steps:

1. Download the publish settings for your account using the following command:

	````
	azure account download
	````

	This will open your default browser and prompt you to sign in to the Azure Management Portal. After signing in, a `.publishsettings` file will be downloaded. Make note of where this file is saved.



1. Next, import the `.publishsettings` file by running the following command, replacing `[path to .publishsettings file]` with the path to your `.publishsettings` file:

	````
	azure account import [path to .publishsettings file]
	````

	> **Note:** When you import publish settings, the information for accessing your Azure subscription is stored in a `.azure` directory located in your `user` directory. Your `user` directory is protected by your operating system; however, it is recommended that you take additional steps to encrypt your `user` directory. You can do so in the following ways:

	>
	> * On Windows, modify the directory properties or use BitLocker.
	> * On Mac, turn on FileVault for the directory.
	> * On Ubuntu, use the Encrypted Home directory feature. Other Linux distributions offer equivalent features.

1. After importing your publish settings, you should delete the `.publishsettings` file, as it is no longer required by the Command-Line Tools and presents a security risk as it can be used to gain access to your subscription.

> **Note:** If you prefer using the log in method, use the following command:
>
> ````
> azure login -u username -p password
> ````
> 

<a name="configuration-powershell" />
###Configuring your computer for creating a virtual machine using PowerShell

The prerequisites for executing the steps listed in the section [Creating a Virtual Machine using Powershell](#creating-a-vm-using-powershell) are:

* [Windows PowerShell 3.0 (or higher)](#config-installing-powershell) installed
* [Microsoft Azure PowerShell Cmdlets v0.7.1 (or higher)](#config-installing-azurecmdlets) installed
* [Azure subscription configured in Powershell](#config-azuresubscription-powershell)

<a name="config-installing-powershell" />
####Installing Windows PowerShell
Windows 8 comes with version 3.0 of PowerShell installed, but that may not be the case in other (previous) versions of Windows. You can check whether PowerShell is installed and if the version will work for the purposes in this lab by following these steps:

1. From the **Start** screen, begin typing **power**. 

	This returns a scoped list of apps that includes **Windows PowerShell**. If Windows PowerShell does not appear in the search results you can skip the next steps and proceed to installing it using the link provided below.

1. To open the console, click **Windows PowerShell**.

1. Execute the following command:

	````PowerShell
	$PSVersionTable.PSVersion
	````

	The execution of this command should return 3.0 or higher. 

	![Verifying Powershell version](images/verifying-powershell-version.png?raw=true)

	_Verifying the Windows PowerShell version_

If **Windows PowerShell** is not installed in your machine or you have an older version, download a newer version from the [Windows PowerShell Scripting](https://technet.microsoft.com/en-us/scriptcenter/dd742419.aspx) page.


<a name="config-installing-azurecmdlets" />
####Installing Microsoft Azure Powershell Cmdlets

You can download and install the **Azure PowerShell** modules and all dependencies using the **Microsoft Web Platform Installer**. If you don't have it installed, download [Microsoft Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409) and, when prompted, click **Run**.

Once you have the **Microsoft Web Platform Installer** installed in your machine, open it and install **Microsoft Azure PowerShell with Microsoft Azure SDK**. Follow the wizard 

![Installing MS Azure Powershell in MS WebPlatform Installer](images/installing-ms-azure-powershell-in-ms-webplatf.png?raw=true)

_Installing Microsoft Azure PowerShell_

<a name="config-azuresubscription-powershell" />
####Configuring your Azure subscription in Powershell

As for the xplat-cli, most cmdlets require an Azure subscription to manage your services. There are two ways to provide your subscription information to Windows PowerShell: you can use a management certificate that contains the information or you can sign in to Azure using your Microsoft account or a organizational (work or school) account. When you sign in, Azure Active Directory (Azure AD) authenticates the credentials and returns an access token that lets Azure PowerShell manage your account.

Azure AD is the recommended authentication method since it makes it easier to manage access to a subscription. With the update in version 0.8.6, it enables an automation scenario with Azure AD authentication as well if a work or school account is used. It works with Azure Resource Manager API as well.

For more information about authentication and subscription management, see ["What's the difference between account-based authentication and certificate-based authentication"](http://msdn.microsoft.com/en-us/library/windowsazure/hh531793.aspx#BKMK_AccountVCert).

To log in using an Azure AD account, follow these instructions:

1. Open the **Microsoft Azure PowerShell** console.

1. Type the following command and hit Enter:

	````PowerShell
	Add-AzureAccount
	````

1. A dialog window to **Sign in to Windows Azure** will appear. Follow the instructions, typing the email address and password associated with your account when prompted.

	Azure authenticates and saves the credential information, and then closes the window. After it does, you will see the following output appear in the console window.

	![Running Add-AzureAccount](images/running-add-azureaccount.png?raw=true)

	_Running the Add-AzureAccount command_

	Starting from 0.8.6, if you sign in using an organizational account, you can type the following command to bypass the pop up window: 

	````PowerShell
	$cred = Get-Credential
	Add-AzureAccount -Credential $cred
	````

	This will pop up the standard Windows PowerShell credential window for you to enter your work or school account user name and password.

	If you are using this in an automation script and want to avoid any pop up windows, use the following snippet: 

	````PowerShell
	$userName = "<your work or school account user name>"
	$securePassword = ConvertTo-SecureString -String "<your work or school account password>" -AsPlainText -Force
	$cred = New-Object System.Management.Automation.PSCredential($userName, $securePassword)
	Add-AzureAccount -Credential $cred 
	````

	>**Note:**
This non-interactive login method only works with an organizational account. An organizational account is a user that is managed by your organization, and defined in the Azure Active Directory instance for your organization. If you do not currently have an organizational account, and are using a Microsoft account to log in to your Azure subscription, you can easily create one using the following steps.

	>1. Login to the [**Azure Management Portal**](https://manage.windowsazure.com/), and click on **Active Directory**.

	>1. If no directory exists, click **Add Directory**. In the dialog that opens, provide the requested information to create the directory.

	>1. Select your directory and then click **Users**. At the bottom, click **ADD USER**. This new user can sign in using a work or school account.

	>1. During the creation of the user, you will be supplied with both an e-mail address for the user and a temporary password. Save this information as it is used in another step.

	>1. From the management portal, select **Settings** and then select **Administrators**. 

	>1. Select **Add**, and add the new user as a co-administrator. This allows the work or school account to manage your Azure subscription.

	>1. Finally, log out of the Azure portal and then log back in using the work or school account. If this is the first time logging in with this account, you will be prompted to change the password.

	>For more information on signing up for Microsoft Azure with a work or school account, see [Sign up for Microsoft Azure as an Organization](http://azure.microsoft.com/en-us/documentation/articles/sign-up-organization/).

<a name="creating-a-vm-using-portal" />
## Creating a Virtual Machine using the Preview Portal ##

In this task you will create a Virtual Machine in Azure using the Azure Preview Portal.

1. Sign in to the [Azure Management Portal](https://portal.azure.com/).

1. Click **NEW** and then **Everything**.

	![Creating VM - Click New and Everything](images/creating-vm---click-new-and-everything.png?raw=true)

	_Creating a VM_ 

1. Click **Virtual Machines** and then click the **Windows Server** tile.

	![Creating VM - Click Virtual Machines and Windows Server](images/creating-vm-click-vms-and-windows-server.png?raw=true)

	_Creating a VM - Click Virtual Machines then the Windows Server tile_

1. A new blade opens with the different images available for **Windows Server**. Find and click **Windows Server 2012 R2 Datacenter**.

	![Creating VM Select image](images/creating-vm-select-image.png?raw=true)

	_Creating a VM - Select the Image to use_

1. On the **Windows Server 2012 R2 Datacenter** blade that opens, click **Create**.

	![Creating VM Confirm image](images/creating-vm-confirm-image.png?raw=true)

	_Creating a VM - Click Create to confirm the use of this image_

1. On the **Create VM** blade that opens, enter: 

	* **Host Name**: virtual machine name (e.g. azureVM)
	* **User Name**: administrator user for the virtual machine (e.g. adminUser).
	* **Password**: unique password for the administrator account.

	![Creating a VM - Enter VM Name, User Name and Password](images/creating-vm-enter-vm-name-user-name-and-passw.png?raw=true)

	_Creating a VM - Enter Host Name, User Name and Password_

1. Review the default settings, such as the **Pricing Tier**, **Optional Configuration**, and **Location**. These choices affect the size of VM as well as networking options such as domain membership. For example, to try out Premium Storage on a virtual machine, you'll need to pick a region and size that supports it.

	> **Note:** Premium storage is in Preview, available for DS-series virtual machines in certain regions. For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](http://azure.microsoft.com/en-us/documentation/articles/storage-premium-storage-preview-portal/).

1. Click **Create**.

1. Wait until the VM is created. This can take a few minutes. You can monitor the creation progress on the **Notifications** Hub.

	![Creating a VM - Monitor progress on the Notifications Hub](images/creating-vm-monitor-progress-on-the-notifi.png?raw=true)

	_Creating a VM - Monitor progress in the Notifications Hub_

1. Once the VM is created you will see it pinned to the **StartBoard**. 

	![Creating a VM - A pin in the startboard exists after creation](images/creating-vm-pin-in-startboard-after-creation.png?raw=true)

	_Creating a VM - A pin was created in the Startboard_

<a name="attachdisktovm-using-portal" />
	Now you will attach an empty data disk to the Virtual Machine just created, as this is the simpler way to add a data disk, because Azure creates the .vhd file for you and stores it in an Azure storage account. Alternatively, you could upload and attach an existing disk to your machine. See [About Virtual Machine Disks in Azure](https://msdn.microsoft.com/library/azure/dn790303.aspx) for more information. 

	After you attach the disk, you'll need to initialize it so it's ready for use.

	>**Note:** It's a best practice to use one or more separate disks to store a virtual machine's data. When you create an Azure virtual machine, it has a disk for the operating system mapped to the C drive and a temporary disk mapped to the D drive. Do not use the D drive to store data. As the name implies, it provides temporary storage only. It offers no redundancy or backup because it doesn't reside in Azure storage.

1. Click the pin for the VM (e.g. azureVM) in the **Startboard**.

	![Clicking the VM pin](images/clicking-the-vm-pin.png?raw=true)

	_Clicking the VM pin_

1. The Virtual Machine blade will open, displaying information about the Virtual Machine. Scroll until you see the **Disks** tile. Click the **Disks** tile.

	![Click the Disks tile](images/click-the-disks-tile.png?raw=true)

	_Clicking the Disks tile_

1. In the **Disks** pane, click **Attach New**.

	![Click Attach New disk to VM](images/click-attach-new-disk-to-vm.png?raw=true)

	_Clicking Attach New link_

1. The **Attach a new disk** blade opens. Click **Storage Container** to select the Storage Account and Container where the new disk will be stored.

	![Attach a new disk blade](images/attach-a-new-disk-blade.png?raw=true)

	_Attaching a new disk blade_

1. Click **Choose Storage Account** and then select an existing storage account.

	![Choose Storage Account](images/choose-storage-account.png?raw=true)

	_Choosing a Storage Account_

1. Click **Choose Container** and then select an existing storage container.

	![Choosing a Container](images/choosing-a-container.png?raw=true)

	_Choosing a Container_

1. Click **OK**.

	![Choosing a Container clicking ok](images/choosing-a-container-clicking-ok.png?raw=true)

	_Clicking OK in Choosing a Container_

1. Back in the **Attach a new disk** pane, update the disk name (if desired) and enter the desired disk size. Then click **OK**.

	![Attaching a new disk](images/attaching-a-new-disk.png?raw=true)

	_Clicking OK to attach a new disk_

1. This will take a few minutes. You can monitor the progress on the disk creation in the **Notifications** Hub.

	![Attaching new disk to VM -  monitoring](images/attaching-new-disk-to-vm-monitoring.png?raw=true)

	_Monitoring new disk creation progress_

	![Attaching new disk created](images/attaching-new-disk-created.png?raw=true)

	_Attaching new disk operation completed_

1. Upon closing the **Notifications** hub, you can see the new disk has been added to the virtual machine.

	![New disk attached to the vm](images/new-disk-attached-to-the-vm.png?raw=true)

	_Verifying new disk was attached to the Virtual Machine_

<a name="logontovm-using-portal" />
	Now you'll log on to the virtual machine and configure the attached disk.

1. Scroll left until you see the **Virtual Machine** blade, and click **Connect**.

	![Clicking Connect in the VM blade](images/clicking-connect-in-the-vm-blade.png?raw=true)

	_Clicking Connect in the Virtual Machine blade_

1. A Remote Desktop Protocol (.rdp) file that was automatically created for the virtual machine will be downloaded and you will be prompted to open or save it. Click **Open**.

	![Opening the rdp file](images/opening-the-rdp-file.png?raw=true)

	_Opening the rdp file to connect to the virtual machine_

	> **Note:** If you choose to save the rdp file to your computer for future uses, click **Save**. Once it finishes downloading click **Open**.

1. In the Remote Desktop Connection dialog, click **Connect**.

	![Connecting to the machine](images/connecting-to-the-machine.png?raw=true)

	_Clicking Connect in the Remote Desktop Connection dialog_

1. Enter the user name and password for the Administrator user that was entered when creating the virtual machine (step 6) and click **OK**.

	![Enter admin User credentials](images/enter-admin-user-credentials.png?raw=true) 

	_Entering the admin user credentials_

1. In the Remote Desktop Connection dialog, click **Yes** to verify the identity of the remote computer. You can check "Don't ask me again for connections to this computer" if you don't want to see this dialog again.

	![Verify virtual machine identity](images/verify-virtual-machine-identity.png?raw=true)
	
	_Verifying the virtual machine identity_

1. After a few seconds the remote desktop window opens and starts setting up the session for the administrator user. Wait until that finishes. Once it is done you will see the **Server Manager Dashboard** open, which you will use in the next steps to configure the data disk that you attached previously. 

	For all other purposes, you could now work with this machine as you would with any other machine in your own environment.

	![Using the remote virtual machine](images/using-the-remote-virtual-machine.png?raw=true)

	_Using the virtual machine_

<a name="configurediskinvm-using-portal" />
	Now you will configure the data disk that you attached to the Virtual Machine. 

1. In the **Server Manager Dashboard** that is open in the virtual machine, click **File and Storage Services**.

	![Clicking File and Storage Services](images/click-file-and-storage-services.png?raw=true)

	_Clicking File and Storage Services_

1. Once you are in the **Servers** pane of **File and Storage Services**, click **Disks**.

	![Clicking Disks in File and Storage Services](images/clicking-disks-in-file-and-storage-services.png?raw=true)

	_Viewing the disks in File and Storage Services_

	The disks available in the machine are listed. You will see one that is listed as Microsoft Virtual Disk, with partition _Unknown_ and Capacity set to the size entered when the disk was created. 

1. Right-click the disk and in the context menu, click **Initialize**.

	![Initialize the disk](images/initialize-the-disk.png?raw=true)

	_Initializing the disk_

1. Click **Yes** in the confirmation message box, to confirm you want to continue erasing all data in the disk.

	![Confirm initialization of disk](images/confirm-initialization-of-disk.png?raw=true)

	_Confirming initialization of the disk_

1. After a brief wait the disk now has a Partition set. Right-click the disk and select **New Volume**.

	![Creating new volume](images/creating-new-volume.png?raw=true)

	_Clicking New Volume in the context menu_

1. Click **Next** in the **New Volume Wizard** dialog that opens.

	<!--
	![New Volume Wizard - Before You Being step](images/newvolumewizard-before-you-being-step.png?raw=true)

	_New Volume Wizard - Before You Begin step_
	-->

1. In the **Server and Disk** step of the **New Volume Wizard**, click **Next**.

	<!--
	![New Volume Wizard - Server and Disk step](images/newvolumewizard-server-and-disk-step.png?raw=true)

	_New Volume Wizard - Server and Disk step_
	-->

1. In the **Size** step of the **New Volume Wizard**, update the volume size if desired and then click **Next**.

	![New Volume Wizard - Size step](images/newvolumewizard-size-step.png?raw=true)

	_New Volume Wizard - Size step_

1. In the **Drive Letter or Folder** step of the **New Volume Wizard**, update information as desired and click **Next**.

	<!--
	![New Volume Wizard - Drive Letter or Folder step](images/newvolumewizard-drive-letter-or-folder-step.png?raw=true)

	_New Volume Wizard - Drive Letter or Folder step_
	-->

1. In the **File Sytem Settings** step of the **New Volume Wizard**, update information as necessary and click **Next**.

	![New Volume Wizard - File System Settings step](images/newvolumewizard-file-system-settings-step.png?raw=true)

	_New Volume Wizard - File System Settings step_

1. In the **Confirmation** step of the **New Volume Wizard**, review the information and click **Create**.

	<!--
	![New Volume Wizard - Confirmation step](images/newvolumewizard-confirmation-step.png?raw=true)

	_New Volume Wizard - Confirmation step_
	-->

1. After the new volume is created, click **Close**.

	<!--
	![New Volume Wizard - Results step](images/newvolumewizard-results-step.png?raw=true)

	_New Volume Wizard - Results step_
	-->

1. You can now open a **File Explorer** window and confirm the disk is ready to be used.

	![Attached disk ready to be used](images/attached-disk-ready-to-be-used.png?raw=true)

	_Viewing attached disk open in File Explorer, ready to be used_

<a name="creating-a-vm-using-cli" />
## Creating a Virtual Machine using the Cross-Platform Command-Line Interface

In this task you will use the **Azure Cross-Platform Command-Line Interface** to create a Linux virtual machine with an empty disk attached and then, you will connect to the vm to configure the disk.


1. First, open a **Command prompt** and ensure that you have the **Azure Cross-Platform Command-Line Interface** correctly installed by executing the following command.

	````
	azure
	````

	You should get a message similar to the following:

	````
	info:             _    _____   _ ___ ___
	info:            /_\  |_  / | | | _ \ __|
	info:      _ ___/ _ \__/ /| |_| |   / _|___ _ _
	info:    (___  /_/ \_\/___|\___/|_|_\___| _____)
	info:       (_______ _ _)         _ ______ _)_ _
	info:              (______________ _ )   (___ _ _)
	info:
	info:    Microsoft Azure: Microsoft's Cloud Platform
	info:
	info:    Tool version 0.8.13
	help:
	help:    Display help for a given command
	help:      help [options] [command]
	help:
	help:    Log in to an Azure subscription using Active Directory. Currently, the user can login only via Microsoft organizational account
	help:      login [options]
	help:
	help:    Log out from Azure subscription using Active Directory. Currently, the user can log out only via Microsoft organizational account
	help:      logout [options] [username]
	help:
	help:    Open the portal in a browser
	help:      portal [options]
	help:
	help:    Commands:
	help:      account        Commands to manage your account information and publish settings
	help:      config         Commands to manage your local settings
	help:      hdinsight      Commands to manage your HDInsight accounts
	help:      mobile         Commands to manage your Mobile Services
	help:      network        Commands to manage your Networks
	help:      sb             Commands to manage your Service Bus configuration
	help:      service        Commands to manage your Cloud Services
	help:      site           Commands to manage your Web Sites
	help:      sql            Commands to manage your SQL Server accounts
	help:      storage        Commands to manage your Storage objects
	help:      vm             Commands to manage your Virtual Machines
	help:
	help:    Options:
	help:      -h, --help     output usage information
	help:      -v, --version  output the application version
````

1. List the available locations to create a virtual machine using the following command and take note of one of them (e.g.: _West US_).

	````
	azure vm location list
	````

1. Now, create a new virtual machine based on the _b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140724-en-us-30GB_ image in the location you choose in the previous step. Replace _your-linux-vm_ with the name you prefer to identify your vm and _yourVMUsername_ with the username you prefer.

	````
	azure vm create your-linux-vm b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140724-en-us-30GB yourVMUsername --location "West US" --ssh
	````
	> **Note 1:** The _--ssh_ parameter enables SSH to manage the deployed Linux virtual machine.

	> **Note 2:** To list the available images use the following command.
	> 
	> ````
	> azure vm image list
	> ````
	> 

	> **Note 3:** You can specify the blob storage url by specifying the _--blob-url_ parameter to the _vm create_ command. In order to create a storage account use the following steps: 

	> 1. Replace the _accountname_ placeholder and execute the following command to create the new storage account. You will be prompt with the location where you want to create the storage account. Make sure to create it in the same location as you plan to create the virtual machine.

	> 		````
	> 	azure storage account create <accountname>
	> 	````

	> 1. Obtain and take note of the account keys of the new account by executing the following command. Replace the _accountname_ placeholder with the one used in the previous step.

	> 		````
	> 	azure storage account keys list <accountname>
	> 	````

	> 1. Now, create a new container in the blob storage account by executing the following command, replacing all the placeholders accordingly.


	> 		````
	> 	azure storage container create <containerName> --account-name <accountname> --account-key <your-storage-account-key>
	> 	````

	> 1. Finally, execute the `azure vm create` command using the _--blob-url_ parameter with the blob url, which will be something like: `https://<accountname>.blob.core.windows.net/<containerName>/<the-blob-name.vhd>`.

	> 		````
	> 	azure vm create your-linux-vm b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140724-en-us-30GB yourVMUsername --location "West US" --ssh --blob-url https://<accountname>.blob.core.windows.net/<containerName>/<the-blob-name.vhd>
	> 	````


1. Attach an empty 30gb new disk to your _your-linux-vm_ virtual machine by executing the following command. Replace the name of the vm with the one used when you create the virtual machine.

	````
	azure vm disk attach-new your-linux-vm 30
	````

	You should get a message similar to the following:

	````
	info:    Executing command vm disk attach-new
	+Getting virtual machines
	+Adding Data-Disk
	info:    vm disk attach-new command OK
	````

1. Show the virtual machine details by executing the following command. Replace _your-linux-vm_ with the one you used when you create the virtual machine. Take note of the _DNSName_ value and make sure that the SSH endpoint exists.

	````
	azure vm show your-linux-vm
	````

	You should get a message similar to the following:

	````
	info:    Executing command vm show
	+Getting virtual machines
	data:    DNSName "your-linux-vm.cloudapp.net"
	data:    Location "West US"
	data:    VMName "your-linux-vm"
	data:    IPAddress "100.78.86.88"
	data:    InstanceStatus "ReadyRole"
	data:    InstanceSize "Small"
	data:    Image "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_1-LTS-amd64-server-20141125-en-us-30GB"
	data:    OSDisk hostCaching "ReadWrite"
	data:    OSDisk name "your-linux-vm-your-linux-vm-0-201501202057370849"
	data:    OSDisk mediaLink "https://portalvhdsqmqsgyc6l4zl4.blob.core.windows.net/vhds/your-linux-vm-your-linux-vm-2015-01-20.vhd"
	data:    OSDisk sourceImageName "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_1-LTS-amd64-server-20141125-en-us-30GB"
	data:    OSDisk operatingSystem "Linux"
	data:    OSDisk iOType "Standard"
	data:    DataDisks 0 hostCaching "None"
	data:    DataDisks 0 label "your-linux-vm-your-linux-vm-your-linux-vm42-0"
	data:    DataDisks 0 name "your-linux-vm-your-linux-vm-0-201501202103240353"
	data:    DataDisks 0 logicalDiskSizeInGB 30
	data:    DataDisks 0 mediaLink "https://portalvhdsqmqsgyc6l4zl4.blob.core.windows.net/vhds/your-linux-vm-f539b3ed4be4e5ab.vhd"
	data:    DataDisks 0 iOType "Standard"
	data:    ReservedIPName ""
	data:    VirtualIPAddresses 0 address "104.42.11.83"
	data:    VirtualIPAddresses 0 name "your-linux-vmContractContract"
	data:    VirtualIPAddresses 0 isDnsProgrammed true
	data:    Network Endpoints 0 localPort 22
	data:    Network Endpoints 0 name "SSH"
	data:    Network Endpoints 0 port 22
	data:    Network Endpoints 0 protocol "tcp"
	data:    Network Endpoints 0 virtualIPAddress "104.42.11.83"
	data:    Network Endpoints 0 enableDirectServerReturn false
	info:    vm show command OK
	````

	To manage the settings of the virtual machine and the applications that run on the machine, you can use an SSH client. You will use the PuTTY program to access the virtual machine.

	> **Note:** To do this, you must install an SSH client on your computer that you want to use to access the virtual machine. There are many SSH client programs that you can choose from. The following are possible choices:
	> 
	> - If you are using a computer that is running a Windows operating system, you might want to use an SSH client such as PuTTY. For more information, see [PuTTY Download](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
	> - If you are using a computer that is running a Linux operating system, you might want to use an SSH client such as OpenSSH. For more information, see [OpenSSH](http://www.openssh.org/).

1. Open the PuTTY program.

	> **Note:** If you are using a console SSH client just use the following command to log in: `ssh`

1. Enter the **DNSName** that you collected from the execution of the `azure vm show` command in the **Host Name** input and then click **Open**.

	![Openning the connection to the virtual machine in PuTTY](images/openning-the-connection-to-the-vm.png?raw=true)

	_Openning the connection to the virtual machine in PuTTY_

1. Click **Yes** in the _PuTTY Security Alert_ dialog in order to add the server's key to the cache.

	![Clicking yes to save the key in the cache](images/clicking-yes-to-save-the-key.png?raw=true)

	_Clicking yes to save the key in the cache_

1. Log on to the virtual machine using the account that was added when you created the virtual machine (e.g.: _yourVMUsername_).

	![Log on to the virtual machine](images/log-on-to-the-vm.png?raw=true)

	_Log on to the virtual machine_

	You can now work with the virtual machine just as you would with any other server.

	Your application may need to store data. To set this up, you attached an empty data disk to the virtual machine.

	On Linux, the Resource Disk is typically managed by the Azure Linux Agent and automatically mounted to **/mnt/resource** (or **/mnt** on Ubuntu images). On the other hand, on Linux the data disk might be named by the kernel as `/dev/sdc`, and users will need to partition, format and mount that resource. Please see the [Azure Linux Agent User Guide](http://www.windowsazure.com/en-us/manage/linux/how-to-guides/linux-agent-guide/) for more information.

	> **Note:** Don’t store data on the resource disk. This disk provides temporary storage for applications and processes and is used to store data that you don’t need to keep, such as swap files. Data disks reside Azure Storage as .vhd files in page blobs and provide storage redundancy to protect your data. For details, see [About Disks and Images in Azure](http://msdn.microsoft.com/en-us/library/jj672979.aspx).

1. In the SSH window, type the following command:

	````
	sudo grep SCSI /var/log/syslog
	````

	You can find the identifier of the last data disk that was added in the messages that are displayed.

1. In the SSH window, type the following command to create a new device:

	````
	sudo fdisk /dev/sdc
	````

	![Executing fdisk in the virtual machine](images/executing-fdisk-in-the-vm.png?raw=true)

	_Executing fdisk in the virtual machine_

	> **Note:** In this example you may need to use `sudo -i` on some distributions if /sbin or /usr/sbin are not in your `$PATH`.

1. Type **n** to create a new partition.

1. Type **p** to make the partition the primary partition, type **1** to make it the first partition, and then type enter to accept the default values for the first and last sectors.

6. Type **p** to see the details about the disk that is being partitioned.

	![Seeing the details about the disk](images/seeing-the-details-about-the-disk.png?raw=true)

	_Seeing the details about the disk_
	
7. Type **w** to write the settings for the disk.

8. You must create the file system on the new partition. As an example, type the following command to create the file system:

	````
	sudo mkfs -t ext4 /dev/sdc1
	````

	> **Note:** Note that SUSE Linux Enterprise 11 systems provide only read-only access for ext4 file systems. For these systems, we recommended formatting the new file system as ext3 rather than ext4.

9. Create a directory to mount the new file system. As an example, type the following command:

	````
	sudo mkdir /datadrive
	````

10. Type the following command to mount the drive:

	````
	sudo mount /dev/sdc1 /datadrive
	````

	The data disk is now ready to use as **/datadrive**.

11. To ensure the drive is re-mounted automatically after a reboot it must be added to the /etc/fstab file. In addition, it is highly recommended that the UUID (Universally Unique IDentifier) is used in /etc/fstab to refer to the drive rather than just the device name (i.e. /dev/sdc1). To find the UUID of the new drive you can use the **blkid** utility:

	````
	sudo -i blkid
	````

	The output will look similar to the following:

	````
	/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
	/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
	/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
	````

	> **Note:** blkid may not require sudo access in all cases, however, it may be easier to run with `sudo -i` on some distributions if /sbin or /usr/sbin are not in your `$PATH`.


	> **Caution:** Improperly editing the /etc/fstab file could result in an unbootable system. If unsure, please refer to the distribution's documentation for information on how to properly edit this file. It is also recommended that a backup of the /etc/fstab file is created before editing.

1. Using a text editor, enter the information about the new file system at the end of the /etc/fstab file.  In this example we will use the UUID value for the new **/dev/sdc1** device that was created in the previous steps, and the mountpoint **/datadrive**:

	````
	UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
	````

	> **Note 1:** On systems based on SUSE Linux you may need to use a slightly different format:
	> 
	> ````
	> /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /   ext3   defaults   1   2
	> ````

	> **Note 2:** You can use the vim text editor in order to perform this steps.

	> 1. Execute the following command in order to start the vim editor
	>
	> 		````
	> sudo vim /etc/fstab
	> ````
	> 1. Type **i** to enter to _INSERT_ mode and use the arrows to go to the end of the file.
	> 1. Type enter and then add the information.
	> 1. Type **Esc** to exit the _INSERT_ mode.
	> 1. Type **:wq** in order to write the file and quit vim.

	> **Note 3:** If additional data drives or partitions are created you will need to enter them into /etc/fstab separately as well.

1. You can now test that the file system is mounted properly by simply unmounting and then re-mounting the file system, i.e. using the example mount point `/datadrive` created in the earlier steps: 

	````
	sudo umount /datadrive
	sudo mount /datadrive
	````

	If the second command produces an error, check the /etc/fstab file for correct syntax.

	> **Note:** Subsequently removing a data disk without editing fstab could cause the VM to fail to boot. If this is a common occurrence, then most distributions provide either the `nofail` and/or `nobootwait` fstab options that will allow a system to boot even if the disk is not present. Please consult your distribution's documentation for more information on these parameters.


<a name="creating-a-vm-using-powershell" />
##Creating a Virtual Machine using Powershell

In this task you will create a virtual machine using PowerShell cmdlets. 
Please read and follow the instructions in the section [Configuring your computer for creating a virtual machine using PowerShell](#configuration-powershell) before executing the steps in this section. These will guide you to install the **Microsoft Azure Powershell Cmdlets** and to configure your Azure subscription in Powershell, both needed before creating a virtual machine.

In the following steps you will ensure your subscription has an associated storage account, and set one if not. Then you will set variables with the parameters of the virtual machine that will be created, and continue by creating the virtual machine. After that you will learn how to attach a new disk to the virtual machine and how to create a Remote Desktop Protocol file to access the machine. Finally, you will add a VM Extension to the virtual machine.

1. Open the **Microsoft Azure PowerShell** console window, if not open already.

1. To verify whether your subscription has an associated storage account, run this command. 

	````PowerShell
	Get-AzureSubscription
	````

	The command will produce an output like this:

	![Get-AzureSubscription output with no storage account set](images/getazuresubscription-output-no-storage-accoun.png?raw=true)

	_Get-AzureSubscription output when no Storage Account set_

	Note the value for **SubscriptionName** in the output. You will use it to replace the [SUBSCRIPTION-NAME] placeholder in upcoming steps.

1. If, like in the image above, the command does not list a value next to **CurrentStorageAccountName**, run the following command:

	````PowerShell
	Get-AzureStorageAccount
	````

	The output produced will look like this:
	
	![Get-AzureStorageAccount output](images/get-azurestorageaccount-output.png?raw=true)

	_Get-AzureStorageAccount output_

	Note the values for the **Label** and **Location** properties. You will use them to replace the [STORAGE-ACCOUNT-LABEL] and [STORAGE-ACCOUNT-LOCATION] placeholders in the upcoming steps.

1. Run the following snippet, replacing the [SUBSCRIPTION-NAME] and [STORAGE-ACCOUNT-LABEL] placeholders with the values retrieved earlier.

	````PowerShell
	Set-AzureSubscription -SubscriptionName "[SUBSCRIPTION-NAME]" -CurrentStorageAccountName [STORAGE-ACCOUNT-LABEL]
	````

	This will set the storage account. You can execute the **Get-AzureSubscription** command and this time, the value set should be listed.

1. Set the variables that will be used to create the virtual machine. Replace the [ADMIN-USER-NAME] and [ADMIN-PASSWORD] placeholders in the snippet below by your desired values, and the [STORAGE-ACCOUNT-LOCATION] placeholder by the value retrieved earlier. The location entered should match the storage account location retrieved in a previous step.

	````PowerShell
	$dclocation = '[STORAGE-ACCOUNT-LOCATION]'
	$adminUserName = '[ADMIN-USER-NAME]'
	$adminPassword = '[ADMIN-PASSWORD]'
	$vmname = 'azureVMPS'
	````

1. Select a name for the Cloud Service. As it needs to be unique, you can verify first whether the selected Cloud Service Name exists by running the **Test-AzureName** command. If this command outputs False, the name is available to be used.

	````PowerShell
	Test-AzureName -Service '[CLOUD-SERVICE-NAME]'

	$cloudSvcName = '[CLOUD-SERVICE-NAME]'
	````

1. Set the image name for your virtual machine to _a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201412.01-en.us-127GB.vhd_. This corresponds to an image of Windows Server R2 DataCenter.

	````PowerShell
	$image = 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201412.01-en.us-127GB.vhd'
	````

	>Note: You can view the complete list of images available by running the following command:
	>
	>````PowerShell
	>Get-AzureVMImage | select ImageName
	>````
	>You can choose to create a virtual machine based on a different image. If you choose to create a Linux virtual machine, the command to create the virtual machine will vary slightly.

1. Now that all variables are set, run the following command to create the virtual machine:

	````PowerShell
	New-AzureQuickVM -AdminUserName $adminUserName -Windows -ServiceName $cloudSvcName -Name $vmname -ImageName $image -Password $adminPassword -Location $dclocation

	````

	>**Note:** If you chose to create a Linux image, the command to use is the following:

	>````PowerShell
	>New-AzureQuickVM -Linux -ServiceName $cloudSvcName -Name $vmname -ImageName $image -LinuxUser $adminUserName -Password $adminPassword -Location $dclocation
	>
	>````
	>
	>The differences reside in the **-Linux** OS switch and the **-LinuxUser** switch that replaces the **-AdminUserName** switch in Windows.
	>
	>**Note 2:** Specifying the **-Location** parameter when calling **New-AzureQuickVM** or **New-AzureVM** tells the Cmdlet to attempt to create a cloud service as a container for the virtual machines. Use this option when creating the first virtual machine and omit it when adding new virtual machines to the same cloud service.
	>
	>**Note 3:** For more information visit the [New-AzureQuickVM reference](https://msdn.microsoft.com/en-us/library/azure/dn495183.aspx) page.

1. The virtual machine is created. 

	![Creating Virtual Machine with Powershell output](images/creating-vm-with-powershell-output.png?raw=true)

	_Virtual Machine creation output_

1. You can list all VMs in the Cloud Service by running the following cmdlet:

	````PowerShell
	Get-AzureVM -ServiceName $cloudSvcName
	````

	![Get-AzureVM output - list all vms](images/get-azurevm-output---list-all-vms.png?raw=true)

	_Get-AzureVM output when listing all virtual machines_

	If you want to enumerate only the details of the virtual machine just created provide the **-Name** switch:

	````PowerShell
	Get-AzureVM -ServiceName $cloudSvcName -Name $vmname
	````

	Now you will add an empty disk to the virtual machine. Since the virtual machine has already been created, you will need to modify the existing machine. Modifying an existing virtual machine requires retrieving the current settings by calling **Get-AzureVM**, modifying them, and then calling the **Update-AzureVM** Cmdlet to save the changes.

1. Execute the following pipe of cmdlets:

	````PowerShell
	Get-AzureVM -Name $vmname -ServiceName $cloudSvcName |
		 Add-AzureDataDisk -CreateNew -DiskSizeInGB 50 -DiskLabel 'datadisk1' -LUN 2 |
		 Update-AzureVM 
	````

	The **Get-AzureVM** Cmdlet retrieves the virtual machine object and sends it to the PowerShell Pipeline.

	The **Add-AzureDataDisk** Cmdlet with the **-CreateNew** parameter allows you to dynamically add storage to the virtual machine. In this case you are attaching an unformatted blank VHD of 50 gigabytes. The **-LUN** parameter tells the order of the device being attached and the optional **-MediaLocation** parameter can be added to specify the location in storage to keep the new VHDs.

	**Add-AzureDataDisk** also supports the **-Import** parameter to attach a disk in the disk library and **-ImportFrom** to attach a pre-existing disk in storage.

1. Once the **Update-AzureVM** call has completed, your virtual machine is ready to be used. 

	![Update-AzureVM output after adding a datadisk](images/update-azurevm-output-after-adding-a-datadisk.png?raw=true)

	_Update-AzureVM output after adding a new datadisk_

	As you did when you created the virtual machine using the **Azure Preview Portal**, you will need to log in to the machine to finish the initialization of the new data disk attached. You can follow the [instructions to get the Remote Desktop Protocol file from the Preview Portal](#logontovm-using-portal) in the task [Creating a virtual machine using the preview portal](#creating-a-vm-using-portal). Otherwise, 

1. Run the following command to create a remote desktop protocol file (rdp) and launch it. When prompted to log in, use the credentials for the administrator user you configured when creating the virtual machine.

	>**Note:** The location for the rdp file provided must exist, otherwise you will see an error.

	````PowerShell
	Get-AzureRemoteDesktopFile -ServiceName $cloudSvcName -Name $vmname -LocalPath 'C:\myvmconnection.rdp' -Launch
	````

	This will launch the rdp, effectively starting the flow of connecting to the remote machine. 

	![Get-AzureRemoteDesktopFile output](images/get-azureremotedesktopfile-output.png?raw=true)

	_Get-AzureRemoteDesktopFile output_

	At this point, refer to the [instructions to get the Remote Desktop Protocol file from the Preview Portal](#logontovm-using-portal) in the task [Creating a virtual machine using the preview portal](#creating-a-vm-using-portal), skipping the first 2 steps.

	After that you will still need to go through the steps of initializing the attached data disk. If you want to do so now, follow the [steps that explain how to configure the attached disk](#configurediskinvm-using-portal) in the task [Creating a virtual machine using the preview portal](#creating-a-vm-using-portal).


	Now you will learn how to add extensions to the virtual machine just created.

	Microsoft Azure provides VM Extensions built by both Microsoft and by trusted third-party providers to enable security, runtime, debugging, management, and other features you can take advantage of to increase your productivity with Azure Virtual Machines. 
	For more details see [Azure VM Extensions and Features](https://msdn.microsoft.com/en-us/library/azure/dn606311.aspx).

	The **Azure Virtual Machine Agent** (VM Agent) is used to install, configure, manage and run **Azure Virtual Machine Extensions** (VM Extensions). The VM Agent is a secure, light-weight process that installs, configures, and removes VM extensions on instances of Azure Virtual Machines from the Image Gallery and on custom VM instances if they have the VM Agent installed. 

	There are two Azure VM Agents, one for Windows VMs and one for Linux VMs. By default, the VM Agent is automatically installed when you create a VM from the Image Gallery, but you can also install the VM agent after the instance is created or install it in a custom VM image that you then upload yourself.

	You will now verify that the VM Agent in enabled in the virtual machine created in the previous steps, and after that install the Symantec Protection extension. Finally, you will verify that the extension was correctly installed on the virtual machine. All these steps will be carried out using PowerShell and using some of the same variables set previously.

1. To verify whether the VM Agent is enabled on the virtual machine, run these commands: 

	````PowerShell
	$vm = Get-AzureVM -ServiceName $cloudSvcName
	$vm.VM.ProvisionGuestAgent
	````

	The output of the first command should not return any errors, and the second command should be True.

	Running these commands will also set the $vm.VM variable, which will be used in upcoming steps to add the extension. 

	>**Note:** If the value of the **$vm.VM.ProvisionGuestAgent** property is False, connect to the virtual machine and install the VM Agent. You can download it from [here](<http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409>). Then, set the value of the variable to True with the following snippets.
	>
	>````PowerShell
	>$vm = Get-AzureVM -serviceName $cloudSvcName -Name $vmname
	>$vm.VM.ProvisionGuestAgent = $TRUE
	>Update-AzureVM -Name $name -VM $vm.VM -ServiceName $cloudSvcName
	>````
	
	Now you will install the Symantec endpoint protection extension.

1. Execute the following command to add the extension:

	````PowerShell
	Set-AzureVMExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection -VM $vm.VM -Version 12.*
	Update-AzureVM -ServiceName $cloudSvcName -Name $vmname -VM $vm.VM
	````

	**Set-AzureVMExtension** sets the properties for the extension about to be installed. **Update-AzureVM** is the cmdlet that installs the extension.

	The output of running all the commands up to now should look like this:

	![Set-AzureVMExtension output](images/set-azurevmextension-output.png?raw=true)

	_Adding a VM Extension output_

1. To verify that the extension was installed on the virtual machine, run the following command:

	````PowerShell
	Get-AzureVMExtension -VM $vm.VM
	````
	The output of the command should look like this:

	![Get-AzureVMExtension output](images/get-azurevmextension-output.png?raw=true)

	_Get-AzureVMExtension output_

<a name="setting-up-the-environment-with-RunBook" />
##Setting up the environment using a RunBook

In this task you will... TBC

1. Create an Automation account
1. Create a Runbook using the gallery (using, for example, the Deploy a Windows Azure Virtual Machine with Two Data Disks Runbook)
1. Modify the Runbook to include a second VM
1. Publish the Runbook
1. Start the Runbook Job
1. Show that the VMs were created in the portal

<a name="cleanup" />
##Appendix - Cleanup

In this task you will... TBC

### Remove VMs
1. Remove all the VMs



##Summary

By completing this lab you have learned how to create virtual machines in several different ways: using the Preview Portal interface, using the Cross-Platform Command Line Tools, using PowerShell or using a RunBook. Additionally, you've seen how to attach an empty datadisk to the virtual machine, how to generate a Remote Desktop Protocol file to connect to the machine and how to install extensions.

* TBC

