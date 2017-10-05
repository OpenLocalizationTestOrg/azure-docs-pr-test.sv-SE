---
title: "Konfigurera Always On-tillgänglighetsgrupp på en virtuell dator i Azure med hjälp av PowerShell | Microsoft Docs"
description: "Den här kursen använder resurser som har skapats med den klassiska distributionsmodellen. Du kan använda PowerShell för att skapa en tillgänglighetsgrupp alltid på i Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: b99cf767fb931d3f7fe14fcbe7990126244613ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-always-on-availability-group-on-an-azure-vm-with-powershell"></a><span data-ttu-id="54557-104">Konfigurera Always On-tillgänglighetsgrupp på en virtuell Azure-dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="54557-104">Configure the Always On availability group on an Azure VM with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="54557-105">Klassiska: UI</span><span class="sxs-lookup"><span data-stu-id="54557-105">Classic: UI</span></span>](../classic/portal-sql-alwayson-availability-groups.md)
> * <span data-ttu-id="54557-106">[Klassiska: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span><span class="sxs-lookup"><span data-stu-id="54557-106">[Classic: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span></span><br/>

<span data-ttu-id="54557-107">Innan du börjar bör du överväga att du kan nu slutföra den här aktiviteten i Azure resource manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="54557-107">Before you begin, consider that you can now complete this task in Azure resource manager model.</span></span> <span data-ttu-id="54557-108">Vi rekommenderar Azure resource manager-modellen för nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="54557-108">We recommend Azure resource manager model for new deployments.</span></span> <span data-ttu-id="54557-109">Se [SQL Server Always On-Tillgänglighetsgrupper på Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="54557-109">See [SQL Server Always On availability groups on Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="54557-110">Vi rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="54557-110">We recommend that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="54557-111">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="54557-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="54557-112">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="54557-112">This article covers using the classic deployment model.</span></span>

<span data-ttu-id="54557-113">Virtuella Azure-datorer (VM) kan hjälpa databasadministratörer att för att sänka kostnaderna för ett SQL Server-system med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="54557-113">Azure virtual machines (VMs) can help database administrators to lower the cost of a high-availability SQL Server system.</span></span> <span data-ttu-id="54557-114">Den här kursen visar hur du implementerar en tillgänglighetsgrupp med hjälp av SQL Server alltid aktiverad slutpunkt till slutpunkt i en Azure-miljö.</span><span class="sxs-lookup"><span data-stu-id="54557-114">This tutorial shows you how to implement an availability group by using SQL Server Always On end-to-end inside an Azure environment.</span></span> <span data-ttu-id="54557-115">I slutet av kursen består lösningen SQL Server alltid aktiverad i Azure av följande element:</span><span class="sxs-lookup"><span data-stu-id="54557-115">At the end of the tutorial, your SQL Server Always On solution in Azure will consist of the following elements:</span></span>

* <span data-ttu-id="54557-116">Ett virtuellt nätverk som innehåller flera undernät, inklusive en frontend- och ett backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="54557-116">A virtual network that contains multiple subnets, including a front-end and a back-end subnet.</span></span>
* <span data-ttu-id="54557-117">En domänkontrollant med en Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="54557-117">A domain controller with an Active Directory domain.</span></span>
* <span data-ttu-id="54557-118">Två SQL Server-datorer som har distribuerats till backend-undernät och anslutna till Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="54557-118">Two SQL Server VMs that are deployed to the back-end subnet and joined to the Active Directory domain.</span></span>
* <span data-ttu-id="54557-119">Ett tre Windows redundanskluster med Nodmajoritet kvorummodellen.</span><span class="sxs-lookup"><span data-stu-id="54557-119">A three-node Windows failover cluster with the Node Majority quorum model.</span></span>
* <span data-ttu-id="54557-120">En tillgänglighetsgrupp med två replikerna med synkront genomförande av en tillgänglighetsdatabas.</span><span class="sxs-lookup"><span data-stu-id="54557-120">An availability group with two synchronous-commit replicas of an availability database.</span></span>

<span data-ttu-id="54557-121">Det här scenariot är ett bra alternativ för dess enkelhet på Azure, inte för kostnadseffektivitet eller andra faktorer.</span><span class="sxs-lookup"><span data-stu-id="54557-121">This scenario is a good choice for its simplicity on Azure, not for its cost-effectiveness or other factors.</span></span> <span data-ttu-id="54557-122">Du kan till exempel minimera antalet virtuella datorer för en tillgänglighetsgrupp med två-replik att spara på beräkningstimmar i Azure med hjälp av domänkontrollanten som filresursvittne för kvorum i ett kluster med två noder.</span><span class="sxs-lookup"><span data-stu-id="54557-122">For example, you can minimize the number of VMs for a two-replica availability group to save on compute hours in Azure by using the domain controller as the quorum file share witness in a two-node failover cluster.</span></span> <span data-ttu-id="54557-123">Den här metoden minskar antalet VM med en i konfigurationen ovan.</span><span class="sxs-lookup"><span data-stu-id="54557-123">This method reduces the VM count by one from the above configuration.</span></span>

<span data-ttu-id="54557-124">Den här kursen är avsedd att visa de steg som krävs för att skapa lösningen som beskrivs ovan, utan utarbeta på information om varje steg.</span><span class="sxs-lookup"><span data-stu-id="54557-124">This tutorial is intended to show you the steps that are required to set up the described solution above, without elaborating on the details of each step.</span></span> <span data-ttu-id="54557-125">I stället för att tillhandahålla konfigurationssteg GUI används därför PowerShell-skript för att ta dig snabbt igenom varje steg.</span><span class="sxs-lookup"><span data-stu-id="54557-125">Therefore, instead of providing the GUI configuration steps, it uses PowerShell scripting to take you quickly through each step.</span></span> <span data-ttu-id="54557-126">Den här kursen förutsätter följande:</span><span class="sxs-lookup"><span data-stu-id="54557-126">This tutorial assumes the following:</span></span>

* <span data-ttu-id="54557-127">Du har redan ett Azure-konto med den virtuella dator prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="54557-127">You already have an Azure account with the virtual machine subscription.</span></span>
* <span data-ttu-id="54557-128">Du har installerat den [Azure PowerShell-cmdlets](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="54557-128">You've installed the [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>
* <span data-ttu-id="54557-129">Du har redan en god förståelse av Always On-Tillgänglighetsgrupper för lokala lösningar.</span><span class="sxs-lookup"><span data-stu-id="54557-129">You already have a solid understanding of Always On availability groups for on-premises solutions.</span></span> <span data-ttu-id="54557-130">Mer information finns i [Always On-Tillgänglighetsgrupper (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="54557-130">For more information, see [Always On availability groups (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span></span>

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a><span data-ttu-id="54557-131">Anslut till din Azure-prenumeration och skapa det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="54557-131">Connect to your Azure subscription and create the virtual network</span></span>
1. <span data-ttu-id="54557-132">Importera modulen för Azure i PowerShell-fönstret på den lokala datorn, ladda ned filen publishing till din dator och ansluta din PowerShell-session till din Azure-prenumeration genom att importera de hämta publiceringsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="54557-132">In a PowerShell window on your local computer, import the Azure module, download the publishing settings file to your machine, and connect your PowerShell session to your Azure subscription by importing the downloaded publishing settings.</span></span>

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    <span data-ttu-id="54557-133">Den **Get-AzurePublishSettingsFile** kommandot automatiskt genererar ett certifikat med Azure och hämtas till din dator.</span><span class="sxs-lookup"><span data-stu-id="54557-133">The **Get-AzurePublishSettingsFile** command automatically generates a management certificate with Azure and downloads it to your machine.</span></span> <span data-ttu-id="54557-134">En webbläsare öppnas automatiskt och du uppmanas att ange autentiseringsuppgifterna för Microsoft-konto för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54557-134">A browser is automatically opened, and you're prompted to enter the Microsoft account credentials for your Azure subscription.</span></span> <span data-ttu-id="54557-135">Den hämtade **.publishsettings** filen innehåller all information du behöver hantera Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="54557-135">The downloaded **.publishsettings** file contains all the information that you need to manage your Azure subscription.</span></span> <span data-ttu-id="54557-136">När du har sparat filen till en lokal katalog, importerar du den med hjälp av den **importera AzurePublishSettingsFile** kommando.</span><span class="sxs-lookup"><span data-stu-id="54557-136">After saving this file to a local directory, import it by using the **Import-AzurePublishSettingsFile** command.</span></span>

   > [!NOTE]
   > <span data-ttu-id="54557-137">.Publishsettings-filen innehåller dina autentiseringsuppgifter (ej kodade) som används för att administrera dina Azure-prenumerationer och tjänster.</span><span class="sxs-lookup"><span data-stu-id="54557-137">The .publishsettings file contains your credentials (unencoded) that are used to administer your Azure subscriptions and services.</span></span> <span data-ttu-id="54557-138">Rekommenderade säkerhetsmetoder för den här filen är att lagra den tillfälligt utanför katalogerna källa (till exempel i mappen Libraries\Documents) och tas bort när importen är klar.</span><span class="sxs-lookup"><span data-stu-id="54557-138">The security best practice for this file is to store it temporarily outside your source directories (for example, in the Libraries\Documents folder), and then delete it after the import has finished.</span></span> <span data-ttu-id="54557-139">En obehörig användare som får åtkomst till .publishsettings-fil kan du redigera, skapa och ta bort Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="54557-139">A malicious user who gains access to the .publishsettings file can edit, create, and delete your Azure services.</span></span>

2. <span data-ttu-id="54557-140">Definiera ett antal variabler som du använder för att skapa ditt moln IT-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="54557-140">Define a series of variables that you'll use to create your cloud IT infrastructure.</span></span>

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    <span data-ttu-id="54557-141">Ta hänsyn till följande för att se till att dina kommandon lyckas senare:</span><span class="sxs-lookup"><span data-stu-id="54557-141">Pay attention to the following to ensure that your commands will succeed later:</span></span>

   * <span data-ttu-id="54557-142">Variabler **$storageAccountName** och **$dcServiceName** måste vara unikt eftersom de används att identifiera moln konto och molnet lagringsservern, respektive på Internet.</span><span class="sxs-lookup"><span data-stu-id="54557-142">Variables **$storageAccountName** and **$dcServiceName** must be unique because they're used to identify your cloud storage account and cloud server, respectively, on the Internet.</span></span>
   * <span data-ttu-id="54557-143">De namn som du anger för variabler **$affinityGroupName** och **$virtualNetworkName** konfigureras i virtuella nätverk configuration dokumentet som du vill använda senare.</span><span class="sxs-lookup"><span data-stu-id="54557-143">The names that you specify for variables **$affinityGroupName** and **$virtualNetworkName** are configured in the virtual network configuration document that you'll use later.</span></span>
   * <span data-ttu-id="54557-144">**$sqlImageName** uppdaterade namnet på VM-avbildning som innehåller SQL Server 2012 Service Pack 1 Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="54557-144">**$sqlImageName** specifies the updated name of the VM image that contains SQL Server 2012 Service Pack 1 Enterprise Edition.</span></span>
   * <span data-ttu-id="54557-145">För enkelhetens skull **Contoso! 000** är samma lösenord som ska användas i hela kursen.</span><span class="sxs-lookup"><span data-stu-id="54557-145">For simplicity, **Contoso!000** is the same password that's used throughout the entire tutorial.</span></span>

3. <span data-ttu-id="54557-146">Skapa en tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="54557-146">Create an affinity group.</span></span>

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. <span data-ttu-id="54557-147">Skapa ett virtuellt nätverk genom att importera en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="54557-147">Create a virtual network by importing a configuration file.</span></span>

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    <span data-ttu-id="54557-148">Konfigurationsfilen innehåller följande XML-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="54557-148">The configuration file contains the following XML document.</span></span> <span data-ttu-id="54557-149">I korthet det anger ett virtuellt nätverk kallas **ContosoNET** i tillhörighetsgruppen kallas **ContosoAG**.</span><span class="sxs-lookup"><span data-stu-id="54557-149">In brief, it specifies a virtual network called **ContosoNET** in the affinity group called **ContosoAG**.</span></span> <span data-ttu-id="54557-150">Den har adressutrymmet **10.10.0.0/16** och har två undernät **10.10.1.0/24** och **10.10.2.0/24**, vilket är främre undernätet och bakre undernät respektive.</span><span class="sxs-lookup"><span data-stu-id="54557-150">It has the address space **10.10.0.0/16** and has two subnets, **10.10.1.0/24** and **10.10.2.0/24**, which are the front subnet and back subnet, respectively.</span></span> <span data-ttu-id="54557-151">Främre undernätet är där du kan placera klientprogram, till exempel Microsoft SharePoint.</span><span class="sxs-lookup"><span data-stu-id="54557-151">The front subnet is where you can place client applications such as Microsoft SharePoint.</span></span> <span data-ttu-id="54557-152">Den bakre undernätet är där du ska placera SQL Server-datorer.</span><span class="sxs-lookup"><span data-stu-id="54557-152">The back subnet is where you'll place the SQL Server VMs.</span></span> <span data-ttu-id="54557-153">Om du ändrar den **$affinityGroupName** och **$virtualNetworkName** variabler tidigare, måste du också ändra motsvarande namn nedan.</span><span class="sxs-lookup"><span data-stu-id="54557-153">If you change the **$affinityGroupName** and **$virtualNetworkName** variables earlier, you must also change the corresponding names below.</span></span>

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

5. <span data-ttu-id="54557-154">Skapa ett lagringskonto som är kopplad till den tillhörighetsgrupp som du skapat och ange den som det aktuella storage-kontot i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54557-154">Create a storage account that's associated with the affinity group that you created, and set it as the current storage account in your subscription.</span></span>

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. <span data-ttu-id="54557-155">Skapa Domänkontrollantservern i den nya cloud service och tillgänglighet uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="54557-155">Create the domain controller server in the new cloud service and availability set.</span></span>

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    <span data-ttu-id="54557-156">Kommandona via rörledningar göra följande:</span><span class="sxs-lookup"><span data-stu-id="54557-156">These piped commands do the following things:</span></span>

   * <span data-ttu-id="54557-157">**Nya AzureVMConfig** skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="54557-157">**New-AzureVMConfig** creates a VM configuration.</span></span>
   * <span data-ttu-id="54557-158">**Lägg till AzureProvisioningConfig** ger konfigurationsparametrar för en fristående Windows server.</span><span class="sxs-lookup"><span data-stu-id="54557-158">**Add-AzureProvisioningConfig** gives the configuration parameters of a standalone Windows server.</span></span>
   * <span data-ttu-id="54557-159">**Lägg till AzureDataDisk** lägger till datadisk som du vill använda för att lagra Active Directory-data med alternativet cachelagring inställd på None.</span><span class="sxs-lookup"><span data-stu-id="54557-159">**Add-AzureDataDisk** adds the data disk that you'll use for storing Active Directory data, with the caching option set to None.</span></span>
   * <span data-ttu-id="54557-160">**Nya AzureVM** skapar en ny molntjänst och skapar den nya Azure VM i ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="54557-160">**New-AzureVM** creates a new cloud service and creates the new Azure VM in the new cloud service.</span></span>

7. <span data-ttu-id="54557-161">Vänta tills den nya VM att helt etablerad och hämta filen för remote desktop till arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="54557-161">Wait for the new VM to be fully provisioned, and download the remote desktop file to your working directory.</span></span> <span data-ttu-id="54557-162">Eftersom den nya Azure VM tar lång tid att etablera, den `while` slingan fortsätter att avsöka den nya virtuella datorn tills den är redo för användning.</span><span class="sxs-lookup"><span data-stu-id="54557-162">Because the new Azure VM takes a long time to provision, the `while` loop continues to poll the new VM until it's ready for use.</span></span>

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

<span data-ttu-id="54557-163">Domänkontrollantservern nu etablerats.</span><span class="sxs-lookup"><span data-stu-id="54557-163">The domain controller server is now successfully provisioned.</span></span> <span data-ttu-id="54557-164">Du måste konfigurera Active Directory-domänen på den här Domänkontrollantservern.</span><span class="sxs-lookup"><span data-stu-id="54557-164">Next, you'll configure the Active Directory domain on this domain controller server.</span></span> <span data-ttu-id="54557-165">Lämna fönstret PowerShell öppen på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="54557-165">Leave the PowerShell window open on your local computer.</span></span> <span data-ttu-id="54557-166">Du använder det igen för att skapa två SQL Server-datorer.</span><span class="sxs-lookup"><span data-stu-id="54557-166">You'll use it again later to create the two SQL Server VMs.</span></span>

## <a name="configure-the-domain-controller"></a><span data-ttu-id="54557-167">Konfigurera en domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="54557-167">Configure the domain controller</span></span>
1. <span data-ttu-id="54557-168">Ansluta till Domänkontrollantservern genom att starta remote desktop fil.</span><span class="sxs-lookup"><span data-stu-id="54557-168">Connect to the domain controller server by launching the remote desktop file.</span></span> <span data-ttu-id="54557-169">Använd datorn administratörens användarnamn AzureAdmin lösenord **Contoso! 000**, som du angav när du skapade den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="54557-169">Use the machine administrator’s username AzureAdmin and password **Contoso!000**, which you specified when you created the new VM.</span></span>
2. <span data-ttu-id="54557-170">Öppna ett PowerShell-fönster i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="54557-170">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="54557-171">Kör följande **DCPROMO. EXE** kommando för att ställa in den **corp.contoso.com** domän, med datakataloger på enhet M.</span><span class="sxs-lookup"><span data-stu-id="54557-171">Run the following **DCPROMO.EXE** command to set up the **corp.contoso.com** domain, with the data directories on drive M.</span></span>

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    <span data-ttu-id="54557-172">När kommandot har slutförts startas den virtuella datorn om automatiskt.</span><span class="sxs-lookup"><span data-stu-id="54557-172">After the command finishes, the VM restarts automatically.</span></span>

4. <span data-ttu-id="54557-173">Ansluta till Domänkontrollantservern igen genom att starta remote desktop fil.</span><span class="sxs-lookup"><span data-stu-id="54557-173">Connect to the domain controller server again by launching the remote desktop file.</span></span> <span data-ttu-id="54557-174">Den här tiden kan logga in som **CORP\Administrator**.</span><span class="sxs-lookup"><span data-stu-id="54557-174">This time, sign in as **CORP\Administrator**.</span></span>
5. <span data-ttu-id="54557-175">Öppna ett PowerShell-fönster i administratörsläge och importera Active Directory PowerShell-modulen med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54557-175">Open a PowerShell window in administrator mode, and import the Active Directory PowerShell module by using the following command:</span></span>

        Import-Module ActiveDirectory

6. <span data-ttu-id="54557-176">Kör följande kommandon för att lägga till tre användare i domänen.</span><span class="sxs-lookup"><span data-stu-id="54557-176">Run the following commands to add three users to the domain.</span></span>

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    <span data-ttu-id="54557-177">**CORP\Install** används för att konfigurera allt relaterade till SQL Server-tjänstinstanser, failover-kluster och tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="54557-177">**CORP\Install** is used to configure everything related to the SQL Server service instances, the failover cluster, and the availability group.</span></span> <span data-ttu-id="54557-178">**CORP\SQLSvc1** och **CORP\SQLSvc2** används som SQL Server-tjänstkontona för två SQL Server-datorer.</span><span class="sxs-lookup"><span data-stu-id="54557-178">**CORP\SQLSvc1** and **CORP\SQLSvc2** are used as the SQL Server service accounts for the two SQL Server VMs.</span></span>
7. <span data-ttu-id="54557-179">Kör följande kommandon för att ge **CORP\Install** behörigheter att skapa datorobjekt i domänen.</span><span class="sxs-lookup"><span data-stu-id="54557-179">Next, run the following commands to give **CORP\Install** the permissions to create computer objects in the domain.</span></span>

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    <span data-ttu-id="54557-180">Det GUID som anges ovan är GUID för typ av objekt.</span><span class="sxs-lookup"><span data-stu-id="54557-180">The GUID specified above is the GUID for the computer object type.</span></span> <span data-ttu-id="54557-181">Den **CORP\Install** konto måste den **läsa alla egenskaper** och **skapa datorobjekt** behörighet att skapa Active direkt objekt för failover-kluster.</span><span class="sxs-lookup"><span data-stu-id="54557-181">The **CORP\Install** account needs the **Read All Properties** and **Create Computer Objects** permission to create the Active Direct objects for the failover cluster.</span></span> <span data-ttu-id="54557-182">Den **läsa alla egenskaper** behörigheter har redan tilldelats CORP\Install som standard, så du inte behöver bevilja explicit.</span><span class="sxs-lookup"><span data-stu-id="54557-182">The **Read All Properties** permission is already given to CORP\Install by default, so you don't need to grant it explicitly.</span></span> <span data-ttu-id="54557-183">Mer information om behörigheter som krävs för att skapa kluster för växling vid fel finns [stegvis Guide för Failover-kluster: Konfigurera konton i Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="54557-183">For more information on permissions that are needed to create the failover cluster, see [Failover Cluster Step-by-Step Guide: Configuring Accounts in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span></span>

    <span data-ttu-id="54557-184">Nu när du är klar med att konfigurera Active Directory och användarobjekt kan du skapa två virtuella SQL Server-datorer och Anslut dem till den här domänen.</span><span class="sxs-lookup"><span data-stu-id="54557-184">Now that you've finished configuring Active Directory and the user objects, you'll create two SQL Server VMs and join them to this domain.</span></span>

## <a name="create-the-sql-server-vms"></a><span data-ttu-id="54557-185">Skapa SQL Server-datorer</span><span class="sxs-lookup"><span data-stu-id="54557-185">Create the SQL Server VMs</span></span>
1. <span data-ttu-id="54557-186">Fortsätta att använda PowerShell-fönster som är öppna på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="54557-186">Continue to use the PowerShell window that's open on your local computer.</span></span> <span data-ttu-id="54557-187">Definiera följande ytterligare variabler:</span><span class="sxs-lookup"><span data-stu-id="54557-187">Define the following additional variables:</span></span>

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    <span data-ttu-id="54557-188">IP-adressen **10.10.0.4** vanligtvis har tilldelats den första virtuella dator som du skapar i den **10.10.0.0/16** undernätet för din virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="54557-188">The IP address **10.10.0.4** is typically assigned to the first VM that you create in the **10.10.0.0/16** subnet of your Azure virtual network.</span></span> <span data-ttu-id="54557-189">Du bör kontrollera att det här är adressen till din Domänkontrollantservern genom att köra **IPCONFIG**.</span><span class="sxs-lookup"><span data-stu-id="54557-189">You should verify that this is the address of your domain controller server by running **IPCONFIG**.</span></span>
2. <span data-ttu-id="54557-190">Kör följande skickas kommandon för att skapa den första virtuella datorn i failover-kluster med namnet **ContosoQuorum**:</span><span class="sxs-lookup"><span data-stu-id="54557-190">Run the following piped commands to create the first VM in the failover cluster, named **ContosoQuorum**:</span></span>

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    <span data-ttu-id="54557-191">Observera följande om ovanstående kommando:</span><span class="sxs-lookup"><span data-stu-id="54557-191">Note the following regarding the command above:</span></span>

   * <span data-ttu-id="54557-192">**Nya AzureVMConfig** skapar en VM-konfiguration med önskad tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="54557-192">**New-AzureVMConfig** creates a VM configuration with the desired availability set name.</span></span> <span data-ttu-id="54557-193">Följande virtuella datorer kommer att skapas med samma tillgänglighetsuppsättningen så att de är anslutna till samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="54557-193">The subsequent VMs will be created with the same availability set name so that they're joined to the same availability set.</span></span>
   * <span data-ttu-id="54557-194">**Lägg till AzureProvisioningConfig** ansluter till den virtuella datorn i Active Directory-domänen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="54557-194">**Add-AzureProvisioningConfig** joins the VM to the Active Directory domain that you created.</span></span>
   * <span data-ttu-id="54557-195">**Ange AzureSubnet** placerar den virtuella datorn i den bakre undernät.</span><span class="sxs-lookup"><span data-stu-id="54557-195">**Set-AzureSubnet** places the VM in the back subnet.</span></span>
   * <span data-ttu-id="54557-196">**Nya AzureVM** skapar en ny molntjänst och skapar den nya Azure VM i ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="54557-196">**New-AzureVM** creates a new cloud service and creates the new Azure VM in the new cloud service.</span></span> <span data-ttu-id="54557-197">Den **DnsSettings** parametern anger att DNS-servern för servrar i en ny molntjänst har IP-adressen **10.10.0.4**.</span><span class="sxs-lookup"><span data-stu-id="54557-197">The **DnsSettings** parameter specifies that the DNS server for the servers in the new cloud service has the IP address **10.10.0.4**.</span></span> <span data-ttu-id="54557-198">Det här är IP-adressen för Domänkontrollantservern.</span><span class="sxs-lookup"><span data-stu-id="54557-198">This is the IP address of the domain controller server.</span></span> <span data-ttu-id="54557-199">Den här parametern krävs för att aktivera de nya virtuella datorerna i Molntjänsten att ansluta till Active Directory-domänen har.</span><span class="sxs-lookup"><span data-stu-id="54557-199">This parameter is needed to enable the new VMs in the cloud service to join to the Active Directory domain successfully.</span></span> <span data-ttu-id="54557-200">Utan den här parametern måste du manuellt ange IPv4-inställningar i den virtuella datorn att använda Domänkontrollantservern som den primära DNS-servern efter den virtuella datorn har etablerats och sedan ansluta den virtuella datorn till Active Directory-domänen.</span><span class="sxs-lookup"><span data-stu-id="54557-200">Without this parameter, you must manually set the IPv4 settings in your VM to use the domain controller server as the primary DNS server after the VM is provisioned, and then join the VM to the Active Directory domain.</span></span>
3. <span data-ttu-id="54557-201">Kör följande skickas kommandon för att skapa SQL Server-VMs, med namnet **ContosoSQL1** och **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="54557-201">Run the following piped commands to create the SQL Server VMs, named **ContosoSQL1** and **ContosoSQL2**.</span></span>

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    <span data-ttu-id="54557-202">Observera följande om kommandona ovan:</span><span class="sxs-lookup"><span data-stu-id="54557-202">Note the following regarding the commands above:</span></span>

   * <span data-ttu-id="54557-203">**Nya AzureVMConfig** använder samma tillgänglighetsuppsättningen som Domänkontrollantservern och använder SQL Server 2012 Service Pack 1 Enterprise Edition bilden i galleriet för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="54557-203">**New-AzureVMConfig** uses the same availability set name as the domain controller server, and uses the SQL Server 2012 Service Pack 1 Enterprise Edition image in the virtual machine gallery.</span></span> <span data-ttu-id="54557-204">Anger också operativsystemets disk till Läs-cacheservrar (ingen skrivcache).</span><span class="sxs-lookup"><span data-stu-id="54557-204">It also sets the operating system disk to read-caching only (no write caching).</span></span> <span data-ttu-id="54557-205">Vi rekommenderar att du migrerar databasfilerna till en separat disk som du ansluter till den virtuella datorn och konfigurera utan läsning eller skrivning till cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="54557-205">We recommend that you migrate the database files to a separate data disk that you attach to the VM, and configure it with no read or write caching.</span></span> <span data-ttu-id="54557-206">Dock är det bästa alternativet att ta bort skrivcache på operativsystemets disk eftersom du inte ta bort skrivskyddade cachelagring på operativsystemets disk.</span><span class="sxs-lookup"><span data-stu-id="54557-206">However, the next best thing is to remove write caching on the operating system disk because you can't remove read caching on the operating system disk.</span></span>
   * <span data-ttu-id="54557-207">**Lägg till AzureProvisioningConfig** ansluter till den virtuella datorn i Active Directory-domänen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="54557-207">**Add-AzureProvisioningConfig** joins the VM to the Active Directory domain that you created.</span></span>
   * <span data-ttu-id="54557-208">**Ange AzureSubnet** placerar den virtuella datorn i den bakre undernät.</span><span class="sxs-lookup"><span data-stu-id="54557-208">**Set-AzureSubnet** places the VM in the back subnet.</span></span>
   * <span data-ttu-id="54557-209">**Lägg till AzureEndpoint** lägger till slutpunkter för åtkomst så att klientprogram kan komma åt dessa tjänster SQL Server-instanser på Internet.</span><span class="sxs-lookup"><span data-stu-id="54557-209">**Add-AzureEndpoint** adds access endpoints so that client applications can access these SQL Server services instances on the Internet.</span></span> <span data-ttu-id="54557-210">Olika portar ges till ContosoSQL1 och ContosoSQL2.</span><span class="sxs-lookup"><span data-stu-id="54557-210">Different ports are given to ContosoSQL1 and ContosoSQL2.</span></span>
   * <span data-ttu-id="54557-211">**Nya AzureVM** skapar den nya SQL Server-VM i samma molntjänst som ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="54557-211">**New-AzureVM** creates the new SQL Server VM in the same cloud service as ContosoQuorum.</span></span> <span data-ttu-id="54557-212">Du måste placera de virtuella datorerna i samma molntjänst om du vill att de ska vara i samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="54557-212">You must place the VMs in the same cloud service if you want them to be in the same availability set.</span></span>
4. <span data-ttu-id="54557-213">Vänta varje virtuell dator för att helt etablerad och varje virtuell dator för att hämta dess remote desktop-fil till arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="54557-213">Wait for each VM to be fully provisioned and for each VM to download its remote desktop file to your working directory.</span></span> <span data-ttu-id="54557-214">Den `for` loop går igenom de tre nya virtuella datorerna och kör kommandon i de översta klammerparenteser för dem.</span><span class="sxs-lookup"><span data-stu-id="54557-214">The `for` loop cycles through the three new VMs and executes the commands inside the top-level curly brackets for each of them.</span></span>

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    <span data-ttu-id="54557-215">SQL Server-datorer har nu etablerats och körs, men de är installerade med SQL Server med standardalternativ.</span><span class="sxs-lookup"><span data-stu-id="54557-215">The SQL Server VMs are now provisioned and running, but they're installed with SQL Server with default options.</span></span>

## <a name="initialize-the-failover-cluster-vms"></a><span data-ttu-id="54557-216">Initiera redundanskluster virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="54557-216">Initialize the failover cluster VMs</span></span>
<span data-ttu-id="54557-217">I det här avsnittet måste du ändra de tre servrarna som du vill använda i klustret och SQL Server-installationen.</span><span class="sxs-lookup"><span data-stu-id="54557-217">In this section, you need to modify the three servers that you'll use in the failover cluster and the SQL Server installation.</span></span> <span data-ttu-id="54557-218">Närmare bestämt:</span><span class="sxs-lookup"><span data-stu-id="54557-218">Specifically:</span></span>

* <span data-ttu-id="54557-219">Alla servrar: du måste installera den **redundanskluster** funktion.</span><span class="sxs-lookup"><span data-stu-id="54557-219">All servers: You need to install the **Failover Clustering** feature.</span></span>
* <span data-ttu-id="54557-220">Alla servrar: du måste lägga till **CORP\Install** som datorn **administratör**.</span><span class="sxs-lookup"><span data-stu-id="54557-220">All servers: You need to add **CORP\Install** as the machine **administrator**.</span></span>
* <span data-ttu-id="54557-221">ContosoSQL1 och ContosoSQL2 endast: du måste lägga till **CORP\Install** som en **sysadmin** roll i standarddatabasen.</span><span class="sxs-lookup"><span data-stu-id="54557-221">ContosoSQL1 and ContosoSQL2 only: You need to add **CORP\Install** as a **sysadmin** role in the default database.</span></span>
* <span data-ttu-id="54557-222">ContosoSQL1 och ContosoSQL2 endast: du måste lägga till **NT AUTHORITY\System** som en inloggning med följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="54557-222">ContosoSQL1 and ContosoSQL2 only: You need to add **NT AUTHORITY\System** as a sign-in with the following permissions:</span></span>

  * <span data-ttu-id="54557-223">ALTER någon tillgänglighetsgrupp</span><span class="sxs-lookup"><span data-stu-id="54557-223">Alter any availability group</span></span>
  * <span data-ttu-id="54557-224">Ansluta SQL</span><span class="sxs-lookup"><span data-stu-id="54557-224">Connect SQL</span></span>
  * <span data-ttu-id="54557-225">Visa-Servertillstånd</span><span class="sxs-lookup"><span data-stu-id="54557-225">View server state</span></span>
* <span data-ttu-id="54557-226">ContosoSQL1 och ContosoSQL2 endast: den **TCP** protokollet är redan aktiverat på SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="54557-226">ContosoSQL1 and ContosoSQL2 only: The **TCP** protocol is already enabled on the SQL Server VM.</span></span> <span data-ttu-id="54557-227">Du behöver dock fortfarande öppna brandväggen för fjärråtkomst av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="54557-227">However, you still need to open the firewall for remote access of SQL Server.</span></span>

<span data-ttu-id="54557-228">Nu är du redo att börja.</span><span class="sxs-lookup"><span data-stu-id="54557-228">Now, you're ready to start.</span></span> <span data-ttu-id="54557-229">Från och med **ContosoQuorum**, Följ stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="54557-229">Beginning with **ContosoQuorum**, follow the steps below:</span></span>

1. <span data-ttu-id="54557-230">Ansluta till **ContosoQuorum** genom att starta remote desktop-filer.</span><span class="sxs-lookup"><span data-stu-id="54557-230">Connect to **ContosoQuorum** by launching the remote desktop files.</span></span> <span data-ttu-id="54557-231">Använd datorn administratörsanvändarnamn **AzureAdmin** och lösenord **Contoso! 000**, som du angav när du skapade de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="54557-231">Use the machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created the VMs.</span></span>
2. <span data-ttu-id="54557-232">Kontrollera att datorerna har anslutit till **corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="54557-232">Verify that the computers have been successfully joined to **corp.contoso.com**.</span></span>
3. <span data-ttu-id="54557-233">Vänta på att SQL Server-installationen ska slutföras kör initieringen av automatiserade uppgifter innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="54557-233">Wait for the SQL Server installation to finish running the automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="54557-234">Öppna ett PowerShell-fönster i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="54557-234">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="54557-235">Installera funktionen redundanskluster i Windows.</span><span class="sxs-lookup"><span data-stu-id="54557-235">Install the Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="54557-236">Lägg till **CORP\Install** som lokal administratör.</span><span class="sxs-lookup"><span data-stu-id="54557-236">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="54557-237">Logga ut från ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="54557-237">Sign out of ContosoQuorum.</span></span> <span data-ttu-id="54557-238">Du är klar med den här servern nu.</span><span class="sxs-lookup"><span data-stu-id="54557-238">You're done with this server now.</span></span>

        logoff.exe

<span data-ttu-id="54557-239">Därefter initiera **ContosoSQL1** och **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="54557-239">Next, initialize **ContosoSQL1** and **ContosoSQL2**.</span></span> <span data-ttu-id="54557-240">Följ stegen nedan, som är identiska för både SQL Server-datorer.</span><span class="sxs-lookup"><span data-stu-id="54557-240">Follow the steps below, which are identical for both SQL Server VMs.</span></span>

1. <span data-ttu-id="54557-241">Ansluta till de två SQL Server-datorer genom att starta remote desktop-filer.</span><span class="sxs-lookup"><span data-stu-id="54557-241">Connect to the two SQL Server VMs by launching the remote desktop files.</span></span> <span data-ttu-id="54557-242">Använd datorn administratörsanvändarnamn **AzureAdmin** och lösenord **Contoso! 000**, som du angav när du skapade de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="54557-242">Use the machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created the VMs.</span></span>
2. <span data-ttu-id="54557-243">Kontrollera att datorerna har anslutit till **corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="54557-243">Verify that the computers have been successfully joined to **corp.contoso.com**.</span></span>
3. <span data-ttu-id="54557-244">Vänta på att SQL Server-installationen ska slutföras kör initieringen av automatiserade uppgifter innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="54557-244">Wait for the SQL Server installation to finish running the automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="54557-245">Öppna ett PowerShell-fönster i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="54557-245">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="54557-246">Installera funktionen redundanskluster i Windows.</span><span class="sxs-lookup"><span data-stu-id="54557-246">Install the Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="54557-247">Lägg till **CORP\Install** som lokal administratör.</span><span class="sxs-lookup"><span data-stu-id="54557-247">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="54557-248">Importera PowerShell-providern för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="54557-248">Import the SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. <span data-ttu-id="54557-249">Lägg till **CORP\Install** som rollen sysadmin för standardinstansen för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="54557-249">Add **CORP\Install** as the sysadmin role for the default SQL Server instance.</span></span>

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. <span data-ttu-id="54557-250">Lägg till **NT AUTHORITY\System** som loggar in med de tre behörigheter som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="54557-250">Add **NT AUTHORITY\System** as a sign-in with the three permissions described above.</span></span>

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. <span data-ttu-id="54557-251">Öppna i brandväggen för fjärråtkomst av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="54557-251">Open the firewall for remote access of SQL Server.</span></span>

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. <span data-ttu-id="54557-252">Logga ut från både virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="54557-252">Sign out of both VMs.</span></span>

         logoff.exe

<span data-ttu-id="54557-253">Slutligen är du redo att konfigurera tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="54557-253">Finally, you're ready to configure the availability group.</span></span> <span data-ttu-id="54557-254">Du använder PowerShell-providern för SQL Server för att utföra allt arbete på **ContosoSQL1**.</span><span class="sxs-lookup"><span data-stu-id="54557-254">You'll use the SQL Server PowerShell Provider to perform all of the work on **ContosoSQL1**.</span></span>

## <a name="configure-the-availability-group"></a><span data-ttu-id="54557-255">Konfigurera tillgänglighetsgruppen</span><span class="sxs-lookup"><span data-stu-id="54557-255">Configure the availability group</span></span>
1. <span data-ttu-id="54557-256">Ansluta till **ContosoSQL1** igen genom att starta remote desktop-filer.</span><span class="sxs-lookup"><span data-stu-id="54557-256">Connect to **ContosoSQL1** again by launching the remote desktop files.</span></span> <span data-ttu-id="54557-257">I stället för att logga in med hjälp av datorkontot kan logga in med hjälp av **CORP\Install**.</span><span class="sxs-lookup"><span data-stu-id="54557-257">Instead of signing in by using the machine account, sign in by using **CORP\Install**.</span></span>
2. <span data-ttu-id="54557-258">Öppna ett PowerShell-fönster i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="54557-258">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="54557-259">Definiera följande variabler:</span><span class="sxs-lookup"><span data-stu-id="54557-259">Define the following variables:</span></span>

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"
4. <span data-ttu-id="54557-260">Importera PowerShell-providern för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="54557-260">Import the SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. <span data-ttu-id="54557-261">Ändra SQL Server-tjänstkontot för ContosoSQL1 till CORP\SQLSvc1.</span><span class="sxs-lookup"><span data-stu-id="54557-261">Change the SQL Server service account for ContosoSQL1 to CORP\SQLSvc1.</span></span>

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. <span data-ttu-id="54557-262">Ändra SQL Server-tjänstkontot för ContosoSQL2 till CORP\SQLSvc2.</span><span class="sxs-lookup"><span data-stu-id="54557-262">Change the SQL Server service account for ContosoSQL2 to CORP\SQLSvc2.</span></span>

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. <span data-ttu-id="54557-263">Hämta **CreateAzureFailoverCluster.ps1** från [Skapa kluster för växling vid fel för Always On-Tillgänglighetsgrupper i Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) till lokala arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="54557-263">Download **CreateAzureFailoverCluster.ps1** from [Create Failover Cluster for Always On Availability Groups in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) to the local working directory.</span></span> <span data-ttu-id="54557-264">För att skapa ett fungerande kluster ska du använda det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="54557-264">You'll use this script to help you create a functional failover cluster.</span></span> <span data-ttu-id="54557-265">Viktig information om hur Windows-redundanskluster samverkar med Azure-nätverk finns i [hög tillgänglighet och katastrofåterställning återställning för SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="54557-265">For important information on how Windows Failover Clustering interacts with the Azure network, see [High availability and disaster recovery for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>
8. <span data-ttu-id="54557-266">Ändra till arbetskatalogen och skapa klustret med det hämta skriptet.</span><span class="sxs-lookup"><span data-stu-id="54557-266">Change to your working directory and create the failover cluster with the downloaded script.</span></span>

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. <span data-ttu-id="54557-267">Aktivera Always On-Tillgänglighetsgrupper för SQL Server-standardinstanser på **ContosoSQL1** och **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="54557-267">Enable Always On availability groups for the default SQL Server instances on **ContosoSQL1** and **ContosoSQL2**.</span></span>

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
10. <span data-ttu-id="54557-268">Skapa en katalog och bevilja behörighet för SQL Server-tjänstkonton.</span><span class="sxs-lookup"><span data-stu-id="54557-268">Create a backup directory and grant permissions for the SQL Server service accounts.</span></span> <span data-ttu-id="54557-269">Du använder den här katalogen för att förbereda tillgänglighetsdatabasen på den sekundära repliken.</span><span class="sxs-lookup"><span data-stu-id="54557-269">You'll use this directory to prepare the availability database on the secondary replica.</span></span>

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. <span data-ttu-id="54557-270">Skapa en databas på **ContosoSQL1** kallas **MyDB1**ta både en fullständig säkerhetskopia och en säkerhetskopia och återställa dem på **ContosoSQL2** med den **WITH NORECOVERY** alternativet.</span><span class="sxs-lookup"><span data-stu-id="54557-270">Create a database on **ContosoSQL1** called **MyDB1**, take both a full backup and a log backup, and restore them on **ContosoSQL2** with the **WITH NORECOVERY** option.</span></span>

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. <span data-ttu-id="54557-271">Skapa tillgängligheten grupp slutpunkterna på SQL Server-datorer och ange åtkomstbehörighet för slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="54557-271">Create the availability group endpoints on the SQL Server VMs and set the proper permissions on the endpoints.</span></span>

         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server1\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"
         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server2\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"

         Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2
13. <span data-ttu-id="54557-272">Skapa tillgänglighetsrepliker.</span><span class="sxs-lookup"><span data-stu-id="54557-272">Create the availability replicas.</span></span>

         $primaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server1 `
             -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
         $secondaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server2 `
             -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
14. <span data-ttu-id="54557-273">Slutligen skapar tillgänglighetsgruppen och ansluter till sekundär replik i tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="54557-273">Finally, create the availability group and join the secondary replica to the availability group.</span></span>

         New-SqlAvailabilityGroup `
             -Name $ag `
             -Path "SQLSERVER:\SQL\$server1\Default" `
             -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
             -Database $db
         Join-SqlAvailabilityGroup `
             -Path "SQLSERVER:\SQL\$server2\Default" `
             -Name $ag
         Add-SqlAvailabilityDatabase `
             -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
             -Database $db

## <a name="next-steps"></a><span data-ttu-id="54557-274">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54557-274">Next steps</span></span>
<span data-ttu-id="54557-275">Du har nu har implementerat SQL Server alltid aktiverad genom att skapa en tillgänglighetsgrupp i Azure.</span><span class="sxs-lookup"><span data-stu-id="54557-275">You've now successfully implemented SQL Server Always On by creating an availability group in Azure.</span></span> <span data-ttu-id="54557-276">Om du vill konfigurera en lyssnare för den här tillgänglighetsgruppen finns [konfigurera en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure](../classic/ps-sql-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="54557-276">To configure a listener for this availability group, see [Configure an ILB listener for Always On availability groups in Azure](../classic/ps-sql-int-listener.md).</span></span>

<span data-ttu-id="54557-277">Annan information om hur du använder SQL Server i Azure, se [SQL Server på Azure virtual machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="54557-277">For other information about using SQL Server in Azure, see [SQL Server on Azure virtual machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
