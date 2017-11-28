---
title: "aaaConfigure hello Always On-tillgänglighetsgruppen på en virtuell Azure-dator med hjälp av PowerShell | Microsoft Docs"
description: "Den här kursen använder resurser som har skapats med hello klassiska distributionsmodellen. Du kan använda PowerShell toocreate en Always On-tillgänglighetsgrupp i Azure."
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
ms.openlocfilehash: d4a27e203b2ff299adebec2b010c03422459b3c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-always-on-availability-group-on-an-azure-vm-with-powershell"></a><span data-ttu-id="b02e6-104">Konfigurera hello Always On-tillgänglighetsgrupp på en virtuell Azure-dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="b02e6-104">Configure hello Always On availability group on an Azure VM with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b02e6-105">Klassiska: UI</span><span class="sxs-lookup"><span data-stu-id="b02e6-105">Classic: UI</span></span>](../classic/portal-sql-alwayson-availability-groups.md)
> * <span data-ttu-id="b02e6-106">[Klassiska: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span><span class="sxs-lookup"><span data-stu-id="b02e6-106">[Classic: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span></span><br/>

<span data-ttu-id="b02e6-107">Innan du börjar bör du överväga att du kan nu slutföra den här aktiviteten i Azure resource manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-107">Before you begin, consider that you can now complete this task in Azure resource manager model.</span></span> <span data-ttu-id="b02e6-108">Vi rekommenderar Azure resource manager-modellen för nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="b02e6-108">We recommend Azure resource manager model for new deployments.</span></span> <span data-ttu-id="b02e6-109">Se [SQL Server Always On-Tillgänglighetsgrupper på Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b02e6-109">See [SQL Server Always On availability groups on Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b02e6-110">Vi rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-110">We recommend that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="b02e6-111">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b02e6-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b02e6-112">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-112">This article covers using hello classic deployment model.</span></span>

<span data-ttu-id="b02e6-113">Virtuella Azure-datorer (VM) kan hjälpa administratörer toolower hello databaskostnader av ett SQL Server-system med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="b02e6-113">Azure virtual machines (VMs) can help database administrators toolower hello cost of a high-availability SQL Server system.</span></span> <span data-ttu-id="b02e6-114">Den här kursen visar hur tooimplement tillgänglighet gruppen med hjälp av SQL Server alltid aktiverad slutpunkt till slutpunkt i en Azure-miljö.</span><span class="sxs-lookup"><span data-stu-id="b02e6-114">This tutorial shows you how tooimplement an availability group by using SQL Server Always On end-to-end inside an Azure environment.</span></span> <span data-ttu-id="b02e6-115">Hello slutet av hello kursen består SQL Server alltid på lösningen i Azure av hello följande element:</span><span class="sxs-lookup"><span data-stu-id="b02e6-115">At hello end of hello tutorial, your SQL Server Always On solution in Azure will consist of hello following elements:</span></span>

* <span data-ttu-id="b02e6-116">Ett virtuellt nätverk som innehåller flera undernät, inklusive en frontend- och ett backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="b02e6-116">A virtual network that contains multiple subnets, including a front-end and a back-end subnet.</span></span>
* <span data-ttu-id="b02e6-117">En domänkontrollant med en Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="b02e6-117">A domain controller with an Active Directory domain.</span></span>
* <span data-ttu-id="b02e6-118">Två SQL Server-datorer som är distribuerade toohello backend-undernät och kopplade toohello Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="b02e6-118">Two SQL Server VMs that are deployed toohello back-end subnet and joined toohello Active Directory domain.</span></span>
* <span data-ttu-id="b02e6-119">Ett tre Windows redundanskluster med hello Nodmajoritet kvorummodellen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-119">A three-node Windows failover cluster with hello Node Majority quorum model.</span></span>
* <span data-ttu-id="b02e6-120">En tillgänglighetsgrupp med två replikerna med synkront genomförande av en tillgänglighetsdatabas.</span><span class="sxs-lookup"><span data-stu-id="b02e6-120">An availability group with two synchronous-commit replicas of an availability database.</span></span>

<span data-ttu-id="b02e6-121">Det här scenariot är ett bra alternativ för dess enkelhet på Azure, inte för kostnadseffektivitet eller andra faktorer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-121">This scenario is a good choice for its simplicity on Azure, not for its cost-effectiveness or other factors.</span></span> <span data-ttu-id="b02e6-122">Exempelvis kan du minimera hello antal virtuella datorer för en två-replik tillgänglighet grupp toosave på beräkningstimmar i Azure med hjälp av hello domänkontrollant som filresursvittne i hello kvorum i ett kluster med två noder.</span><span class="sxs-lookup"><span data-stu-id="b02e6-122">For example, you can minimize hello number of VMs for a two-replica availability group toosave on compute hours in Azure by using hello domain controller as hello quorum file share witness in a two-node failover cluster.</span></span> <span data-ttu-id="b02e6-123">Den här metoden minskar hello VM antal med ett från hello senare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b02e6-123">This method reduces hello VM count by one from hello above configuration.</span></span>

<span data-ttu-id="b02e6-124">Den här kursen är avsedd tooshow du hello steg som är nödvändiga tooset in hello beskrivs lösningen ovan, utan utarbeta hello detaljer för varje steg.</span><span class="sxs-lookup"><span data-stu-id="b02e6-124">This tutorial is intended tooshow you hello steps that are required tooset up hello described solution above, without elaborating on hello details of each step.</span></span> <span data-ttu-id="b02e6-125">Därför steg i stället för under förutsättning att hello GUI konfigurationssteg använder PowerShell scripting tootake du snabbt via varje.</span><span class="sxs-lookup"><span data-stu-id="b02e6-125">Therefore, instead of providing hello GUI configuration steps, it uses PowerShell scripting tootake you quickly through each step.</span></span> <span data-ttu-id="b02e6-126">Den här självstudiekursen förutsätts hello följande:</span><span class="sxs-lookup"><span data-stu-id="b02e6-126">This tutorial assumes hello following:</span></span>

* <span data-ttu-id="b02e6-127">Du har redan ett Azure-konto med hello virtuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b02e6-127">You already have an Azure account with hello virtual machine subscription.</span></span>
* <span data-ttu-id="b02e6-128">Du har installerat hello [Azure PowerShell-cmdlets](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b02e6-128">You've installed hello [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>
* <span data-ttu-id="b02e6-129">Du har redan en god förståelse av Always On-Tillgänglighetsgrupper för lokala lösningar.</span><span class="sxs-lookup"><span data-stu-id="b02e6-129">You already have a solid understanding of Always On availability groups for on-premises solutions.</span></span> <span data-ttu-id="b02e6-130">Mer information finns i [Always On-Tillgänglighetsgrupper (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="b02e6-130">For more information, see [Always On availability groups (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span></span>

## <a name="connect-tooyour-azure-subscription-and-create-hello-virtual-network"></a><span data-ttu-id="b02e6-131">Anslut tooyour Azure-prenumeration och skapa hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="b02e6-131">Connect tooyour Azure subscription and create hello virtual network</span></span>
1. <span data-ttu-id="b02e6-132">Importera hello Azure-modulen i PowerShell-fönstret på den lokala datorn, hämtar hello publicering inställningar filen tooyour datorn och ansluta din PowerShell-session tooyour Azure-prenumeration genom att importera hello hämtas publiceringsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="b02e6-132">In a PowerShell window on your local computer, import hello Azure module, download hello publishing settings file tooyour machine, and connect your PowerShell session tooyour Azure subscription by importing hello downloaded publishing settings.</span></span>

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    <span data-ttu-id="b02e6-133">Hej **Get-AzurePublishSettingsFile** kommandot genererar ett certifikat med Azure och hämtar den tooyour datorn automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b02e6-133">hello **Get-AzurePublishSettingsFile** command automatically generates a management certificate with Azure and downloads it tooyour machine.</span></span> <span data-ttu-id="b02e6-134">En webbläsare öppnas automatiskt och du kan ange tooenter hello Microsoft-kontouppgifter för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b02e6-134">A browser is automatically opened, and you're prompted tooenter hello Microsoft account credentials for your Azure subscription.</span></span> <span data-ttu-id="b02e6-135">hello hämtas **.publishsettings** filen innehåller alla hello information du behöver toomanage din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b02e6-135">hello downloaded **.publishsettings** file contains all hello information that you need toomanage your Azure subscription.</span></span> <span data-ttu-id="b02e6-136">När du sparar filen tooa lokala katalogen kan du importera det med hello **importera AzurePublishSettingsFile** kommando.</span><span class="sxs-lookup"><span data-stu-id="b02e6-136">After saving this file tooa local directory, import it by using hello **Import-AzurePublishSettingsFile** command.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b02e6-137">Hej .publishsettings-fil innehåller dina autentiseringsuppgifter (ej kodade) som används tooadminister dina Azure-prenumerationer och tjänster.</span><span class="sxs-lookup"><span data-stu-id="b02e6-137">hello .publishsettings file contains your credentials (unencoded) that are used tooadminister your Azure subscriptions and services.</span></span> <span data-ttu-id="b02e6-138">hello rekommenderade säkerhetsmetoder för den här filen är toostore den tillfälligt utanför katalogerna källa (till exempel i hello Libraries\Documents mappen), och tas bort när hello importen är klar.</span><span class="sxs-lookup"><span data-stu-id="b02e6-138">hello security best practice for this file is toostore it temporarily outside your source directories (for example, in hello Libraries\Documents folder), and then delete it after hello import has finished.</span></span> <span data-ttu-id="b02e6-139">En obehörig användare som får åtkomst till toohello .publishsettings-fil kan du redigera, skapa och ta bort Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="b02e6-139">A malicious user who gains access toohello .publishsettings file can edit, create, and delete your Azure services.</span></span>

2. <span data-ttu-id="b02e6-140">Definiera ett antal variabler när du ska använda toocreate ditt moln IT-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="b02e6-140">Define a series of variables that you'll use toocreate your cloud IT infrastructure.</span></span>

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

    <span data-ttu-id="b02e6-141">Betala uppmärksamhet toohello följande tooensure dina kommandon lyckas senare:</span><span class="sxs-lookup"><span data-stu-id="b02e6-141">Pay attention toohello following tooensure that your commands will succeed later:</span></span>

   * <span data-ttu-id="b02e6-142">Variabler **$storageAccountName** och **$dcServiceName** måste vara unikt eftersom de används tooidentify ditt lagringskonto för molnet och moln-servern, på hello Internet.</span><span class="sxs-lookup"><span data-stu-id="b02e6-142">Variables **$storageAccountName** and **$dcServiceName** must be unique because they're used tooidentify your cloud storage account and cloud server, respectively, on hello Internet.</span></span>
   * <span data-ttu-id="b02e6-143">Hej namn som du anger för variabler **$affinityGroupName** och **$virtualNetworkName** konfigureras i hello virtuellt nätverk configuration dokument som du vill använda senare.</span><span class="sxs-lookup"><span data-stu-id="b02e6-143">hello names that you specify for variables **$affinityGroupName** and **$virtualNetworkName** are configured in hello virtual network configuration document that you'll use later.</span></span>
   * <span data-ttu-id="b02e6-144">**$sqlImageName** anger hello uppdateras för hello VM-avbildning som innehåller SQL Server 2012 Service Pack 1 Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="b02e6-144">**$sqlImageName** specifies hello updated name of hello VM image that contains SQL Server 2012 Service Pack 1 Enterprise Edition.</span></span>
   * <span data-ttu-id="b02e6-145">För enkelhetens skull **Contoso! 000** är hello samma lösenord som används i hela hello hela kursen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-145">For simplicity, **Contoso!000** is hello same password that's used throughout hello entire tutorial.</span></span>

3. <span data-ttu-id="b02e6-146">Skapa en tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="b02e6-146">Create an affinity group.</span></span>

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. <span data-ttu-id="b02e6-147">Skapa ett virtuellt nätverk genom att importera en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="b02e6-147">Create a virtual network by importing a configuration file.</span></span>

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    <span data-ttu-id="b02e6-148">hello konfigurationsfilen innehåller hello följande XML-dokument.</span><span class="sxs-lookup"><span data-stu-id="b02e6-148">hello configuration file contains hello following XML document.</span></span> <span data-ttu-id="b02e6-149">I korthet det anger ett virtuellt nätverk kallas **ContosoNET** i hello tillhörighetsgrupp kallas **ContosoAG**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-149">In brief, it specifies a virtual network called **ContosoNET** in hello affinity group called **ContosoAG**.</span></span> <span data-ttu-id="b02e6-150">Den har hello adressutrymme **10.10.0.0/16** och har två undernät **10.10.1.0/24** och **10.10.2.0/24**, vilket är hello främre undernät och bakre undernät respektive.</span><span class="sxs-lookup"><span data-stu-id="b02e6-150">It has hello address space **10.10.0.0/16** and has two subnets, **10.10.1.0/24** and **10.10.2.0/24**, which are hello front subnet and back subnet, respectively.</span></span> <span data-ttu-id="b02e6-151">hello främre undernät är där du kan placera klientprogram, till exempel Microsoft SharePoint.</span><span class="sxs-lookup"><span data-stu-id="b02e6-151">hello front subnet is where you can place client applications such as Microsoft SharePoint.</span></span> <span data-ttu-id="b02e6-152">hello tillbaka undernät är där du ska placera hello SQL Server-datorer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-152">hello back subnet is where you'll place hello SQL Server VMs.</span></span> <span data-ttu-id="b02e6-153">Om du ändrar hello **$affinityGroupName** och **$virtualNetworkName** variabler tidigare, måste du också ändra hello som motsvarar namnen nedan.</span><span class="sxs-lookup"><span data-stu-id="b02e6-153">If you change hello **$affinityGroupName** and **$virtualNetworkName** variables earlier, you must also change hello corresponding names below.</span></span>

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

5. <span data-ttu-id="b02e6-154">Skapa ett lagringskonto som är kopplad till hello tillhörighetsgrupp som du skapat och ange den som hello aktuella storage-konto i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b02e6-154">Create a storage account that's associated with hello affinity group that you created, and set it as hello current storage account in your subscription.</span></span>

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. <span data-ttu-id="b02e6-155">Skapa hello Domänkontrollantservern i hello nya cloud service och tillgänglighet uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-155">Create hello domain controller server in hello new cloud service and availability set.</span></span>

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

    <span data-ttu-id="b02e6-156">Kommandona via rörledningar hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="b02e6-156">These piped commands do hello following things:</span></span>

   * <span data-ttu-id="b02e6-157">**Nya AzureVMConfig** skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b02e6-157">**New-AzureVMConfig** creates a VM configuration.</span></span>
   * <span data-ttu-id="b02e6-158">**Lägg till AzureProvisioningConfig** ger hello konfigurationsparametrar för en fristående Windows server.</span><span class="sxs-lookup"><span data-stu-id="b02e6-158">**Add-AzureProvisioningConfig** gives hello configuration parameters of a standalone Windows server.</span></span>
   * <span data-ttu-id="b02e6-159">**Lägg till AzureDataDisk** lägger till hello datadisk som du vill använda för att lagra Active Directory-data med hello cachelagring alternativet set tooNone.</span><span class="sxs-lookup"><span data-stu-id="b02e6-159">**Add-AzureDataDisk** adds hello data disk that you'll use for storing Active Directory data, with hello caching option set tooNone.</span></span>
   * <span data-ttu-id="b02e6-160">**Nya AzureVM** skapar en ny molntjänst och skapar hello nya Azure VM i hello ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="b02e6-160">**New-AzureVM** creates a new cloud service and creates hello new Azure VM in hello new cloud service.</span></span>

7. <span data-ttu-id="b02e6-161">Vänta tills hello nya VM toobe helt etablerad och hämta hello skrivbord fjärrfil tooyour arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-161">Wait for hello new VM toobe fully provisioned, and download hello remote desktop file tooyour working directory.</span></span> <span data-ttu-id="b02e6-162">Eftersom hello nya Azure VM tar en lång tid tooprovision, hello `while` slingan fortsätter toopoll hello ny virtuell dator tills den är redo för användning.</span><span class="sxs-lookup"><span data-stu-id="b02e6-162">Because hello new Azure VM takes a long time tooprovision, hello `while` loop continues toopoll hello new VM until it's ready for use.</span></span>

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

<span data-ttu-id="b02e6-163">hello Domänkontrollantservern nu etablerats.</span><span class="sxs-lookup"><span data-stu-id="b02e6-163">hello domain controller server is now successfully provisioned.</span></span> <span data-ttu-id="b02e6-164">Du måste konfigurera hello Active Directory-domän på den här Domänkontrollantservern.</span><span class="sxs-lookup"><span data-stu-id="b02e6-164">Next, you'll configure hello Active Directory domain on this domain controller server.</span></span> <span data-ttu-id="b02e6-165">Låt hello PowerShell-fönstret öppet på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="b02e6-165">Leave hello PowerShell window open on your local computer.</span></span> <span data-ttu-id="b02e6-166">Du kommer att använda det igen senare toocreate hello två virtuella SQL Server-datorer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-166">You'll use it again later toocreate hello two SQL Server VMs.</span></span>

## <a name="configure-hello-domain-controller"></a><span data-ttu-id="b02e6-167">Konfigurera hello-domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="b02e6-167">Configure hello domain controller</span></span>
1. <span data-ttu-id="b02e6-168">Ansluta toohello Domänkontrollantservern genom att starta hello remote desktop-fil.</span><span class="sxs-lookup"><span data-stu-id="b02e6-168">Connect toohello domain controller server by launching hello remote desktop file.</span></span> <span data-ttu-id="b02e6-169">Använd hello datorn administratörens användarnamn AzureAdmin lösenord **Contoso! 000**, som du angav när du skapade hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b02e6-169">Use hello machine administrator’s username AzureAdmin and password **Contoso!000**, which you specified when you created hello new VM.</span></span>
2. <span data-ttu-id="b02e6-170">Öppna ett PowerShell-fönster i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="b02e6-170">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="b02e6-171">Kör följande hello **DCPROMO. EXE** kommandot tooset in hello **corp.contoso.com** domän, med hello datakataloger på enhet M.</span><span class="sxs-lookup"><span data-stu-id="b02e6-171">Run hello following **DCPROMO.EXE** command tooset up hello **corp.contoso.com** domain, with hello data directories on drive M.</span></span>

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

    <span data-ttu-id="b02e6-172">Startar om automatiskt när hello-kommandot har slutförts hello VM.</span><span class="sxs-lookup"><span data-stu-id="b02e6-172">After hello command finishes, hello VM restarts automatically.</span></span>

4. <span data-ttu-id="b02e6-173">Ansluta toohello Domänkontrollantservern igen genom att starta hello remote desktop-fil.</span><span class="sxs-lookup"><span data-stu-id="b02e6-173">Connect toohello domain controller server again by launching hello remote desktop file.</span></span> <span data-ttu-id="b02e6-174">Den här tiden kan logga in som **CORP\Administrator**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-174">This time, sign in as **CORP\Administrator**.</span></span>
5. <span data-ttu-id="b02e6-175">Öppna ett PowerShell-fönster i administratörsläge och importera hello Active Directory PowerShell-modulen med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b02e6-175">Open a PowerShell window in administrator mode, and import hello Active Directory PowerShell module by using hello following command:</span></span>

        Import-Module ActiveDirectory

6. <span data-ttu-id="b02e6-176">Kör följande kommandon tooadd tre användare toohello domän hello.</span><span class="sxs-lookup"><span data-stu-id="b02e6-176">Run hello following commands tooadd three users toohello domain.</span></span>

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

    <span data-ttu-id="b02e6-177">**CORP\Install** är används tooconfigure allt relaterade toohello SQL Server-tjänstinstanser hello failover-kluster och hello-tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-177">**CORP\Install** is used tooconfigure everything related toohello SQL Server service instances, hello failover cluster, and hello availability group.</span></span> <span data-ttu-id="b02e6-178">**CORP\SQLSvc1** och **CORP\SQLSvc2** används som hello SQL Server-tjänstkontona för hello två SQL Server-datorer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-178">**CORP\SQLSvc1** and **CORP\SQLSvc2** are used as hello SQL Server service accounts for hello two SQL Server VMs.</span></span>
7. <span data-ttu-id="b02e6-179">Hello nästa, kör följande kommandon toogive **CORP\Install** hello behörigheter toocreate datorobjekt i hello domän.</span><span class="sxs-lookup"><span data-stu-id="b02e6-179">Next, run hello following commands toogive **CORP\Install** hello permissions toocreate computer objects in hello domain.</span></span>

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    <span data-ttu-id="b02e6-180">hello GUID som anges ovan är hello GUID för hello objekttypen för datorn.</span><span class="sxs-lookup"><span data-stu-id="b02e6-180">hello GUID specified above is hello GUID for hello computer object type.</span></span> <span data-ttu-id="b02e6-181">Hej **CORP\Install** kontot måste hello **läsa alla egenskaper** och **skapa datorobjekt** behörighet toocreate hello Active direkt objekt för hello redundans kluster.</span><span class="sxs-lookup"><span data-stu-id="b02e6-181">hello **CORP\Install** account needs hello **Read All Properties** and **Create Computer Objects** permission toocreate hello Active Direct objects for hello failover cluster.</span></span> <span data-ttu-id="b02e6-182">Hej **läsa alla egenskaper** behörigheter redan ges tooCORP\Install som standard, så du inte behöver toogrant det explicit.</span><span class="sxs-lookup"><span data-stu-id="b02e6-182">hello **Read All Properties** permission is already given tooCORP\Install by default, so you don't need toogrant it explicitly.</span></span> <span data-ttu-id="b02e6-183">För mer information om behörigheter som är nödvändiga toocreate hello failover-kluster, se [stegvis Guide för Failover-kluster: Konfigurera konton i Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="b02e6-183">For more information on permissions that are needed toocreate hello failover cluster, see [Failover Cluster Step-by-Step Guide: Configuring Accounts in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span></span>

    <span data-ttu-id="b02e6-184">Nu när du är klar med att konfigurera Active Directory och hello användarobjekt du skapa två virtuella SQL Server-datorer och Anslut dem toothis domän.</span><span class="sxs-lookup"><span data-stu-id="b02e6-184">Now that you've finished configuring Active Directory and hello user objects, you'll create two SQL Server VMs and join them toothis domain.</span></span>

## <a name="create-hello-sql-server-vms"></a><span data-ttu-id="b02e6-185">Skapa hello SQL Server-datorer</span><span class="sxs-lookup"><span data-stu-id="b02e6-185">Create hello SQL Server VMs</span></span>
1. <span data-ttu-id="b02e6-186">Fortsätt toouse hello PowerShell-fönster som är öppna på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="b02e6-186">Continue toouse hello PowerShell window that's open on your local computer.</span></span> <span data-ttu-id="b02e6-187">Definiera hello följande ytterligare variabler:</span><span class="sxs-lookup"><span data-stu-id="b02e6-187">Define hello following additional variables:</span></span>

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

    <span data-ttu-id="b02e6-188">Hej IP-adress **10.10.0.4** normalt tilldelas toohello första virtuella dator som du skapar i hello **10.10.0.0/16** undernätet för din virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="b02e6-188">hello IP address **10.10.0.4** is typically assigned toohello first VM that you create in hello **10.10.0.0/16** subnet of your Azure virtual network.</span></span> <span data-ttu-id="b02e6-189">Du bör kontrollera att det här är din Domänkontrollantservern hello-adress genom att köra **IPCONFIG**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-189">You should verify that this is hello address of your domain controller server by running **IPCONFIG**.</span></span>
2. <span data-ttu-id="b02e6-190">Kör hello följande via rörledningar kommandon toocreate hello först VM i hello failover-kluster med namnet **ContosoQuorum**:</span><span class="sxs-lookup"><span data-stu-id="b02e6-190">Run hello following piped commands toocreate hello first VM in hello failover cluster, named **ContosoQuorum**:</span></span>

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

    <span data-ttu-id="b02e6-191">Observera följande hello om hello ovanstående kommando:</span><span class="sxs-lookup"><span data-stu-id="b02e6-191">Note hello following regarding hello command above:</span></span>

   * <span data-ttu-id="b02e6-192">**Nya AzureVMConfig** skapar en VM-konfiguration med hello önskade tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-192">**New-AzureVMConfig** creates a VM configuration with hello desired availability set name.</span></span> <span data-ttu-id="b02e6-193">hello senare virtuella datorer kommer att skapas med hello samma namn på tillgänglighetsuppsättning så att de är anslutna toohello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b02e6-193">hello subsequent VMs will be created with hello same availability set name so that they're joined toohello same availability set.</span></span>
   * <span data-ttu-id="b02e6-194">**Lägg till AzureProvisioningConfig** kopplingar hello VM toohello Active Directory-domän som du skapade.</span><span class="sxs-lookup"><span data-stu-id="b02e6-194">**Add-AzureProvisioningConfig** joins hello VM toohello Active Directory domain that you created.</span></span>
   * <span data-ttu-id="b02e6-195">**Ange AzureSubnet** platser hello VM i hello tillbaka undernät.</span><span class="sxs-lookup"><span data-stu-id="b02e6-195">**Set-AzureSubnet** places hello VM in hello back subnet.</span></span>
   * <span data-ttu-id="b02e6-196">**Nya AzureVM** skapar en ny molntjänst och skapar hello nya Azure VM i hello ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="b02e6-196">**New-AzureVM** creates a new cloud service and creates hello new Azure VM in hello new cloud service.</span></span> <span data-ttu-id="b02e6-197">Hej **DnsSettings** parametern anger hello DNS-servern för hello servrar i hello ny molntjänst har hello IP-adress **10.10.0.4**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-197">hello **DnsSettings** parameter specifies that hello DNS server for hello servers in hello new cloud service has hello IP address **10.10.0.4**.</span></span> <span data-ttu-id="b02e6-198">Detta är hello hello Domänkontrollantservern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b02e6-198">This is hello IP address of hello domain controller server.</span></span> <span data-ttu-id="b02e6-199">Den här parametern krävs tooenable hello nya virtuella datorer i hello cloud service toojoin toohello Active Directory-domän har.</span><span class="sxs-lookup"><span data-stu-id="b02e6-199">This parameter is needed tooenable hello new VMs in hello cloud service toojoin toohello Active Directory domain successfully.</span></span> <span data-ttu-id="b02e6-200">Utan den här parametern måste du manuellt ange hello IPv4-inställningar i din VM toouse hello Domänkontrollantservern som hello primära DNS-server när hello VM etableras och sedan ansluta hello VM toohello Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="b02e6-200">Without this parameter, you must manually set hello IPv4 settings in your VM toouse hello domain controller server as hello primary DNS server after hello VM is provisioned, and then join hello VM toohello Active Directory domain.</span></span>
3. <span data-ttu-id="b02e6-201">Kör hello följande via rörledningar kommandon toocreate hello SQL Server-datorer med namnet **ContosoSQL1** och **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-201">Run hello following piped commands toocreate hello SQL Server VMs, named **ContosoSQL1** and **ContosoSQL2**.</span></span>

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

    <span data-ttu-id="b02e6-202">Observera följande hello om hello kommandona ovan:</span><span class="sxs-lookup"><span data-stu-id="b02e6-202">Note hello following regarding hello commands above:</span></span>

   * <span data-ttu-id="b02e6-203">**Nya AzureVMConfig** använder hello samma tillgänglighetsuppsättningen som hello Domänkontrollantservern och använder hello SQL Server 2012 Service Pack 1 Enterprise Edition bilden i hello galleriet för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-203">**New-AzureVMConfig** uses hello same availability set name as hello domain controller server, and uses hello SQL Server 2012 Service Pack 1 Enterprise Edition image in hello virtual machine gallery.</span></span> <span data-ttu-id="b02e6-204">Den anger också hello fungerar system disk tooread cachelagring endast (ingen skrivcache).</span><span class="sxs-lookup"><span data-stu-id="b02e6-204">It also sets hello operating system disk tooread-caching only (no write caching).</span></span> <span data-ttu-id="b02e6-205">Vi rekommenderar att du migrerar hello databasen filer tooa separat datadisk att bifoga toohello VM, och konfigurera den med ingen läs eller skrivcache.</span><span class="sxs-lookup"><span data-stu-id="b02e6-205">We recommend that you migrate hello database files tooa separate data disk that you attach toohello VM, and configure it with no read or write caching.</span></span> <span data-ttu-id="b02e6-206">Hello bästa alternativet är dock tooremove skrivcache på hello operativsystemdisk eftersom du inte ta bort läsa cachelagring på hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="b02e6-206">However, hello next best thing is tooremove write caching on hello operating system disk because you can't remove read caching on hello operating system disk.</span></span>
   * <span data-ttu-id="b02e6-207">**Lägg till AzureProvisioningConfig** kopplingar hello VM toohello Active Directory-domän som du skapade.</span><span class="sxs-lookup"><span data-stu-id="b02e6-207">**Add-AzureProvisioningConfig** joins hello VM toohello Active Directory domain that you created.</span></span>
   * <span data-ttu-id="b02e6-208">**Ange AzureSubnet** platser hello VM i hello tillbaka undernät.</span><span class="sxs-lookup"><span data-stu-id="b02e6-208">**Set-AzureSubnet** places hello VM in hello back subnet.</span></span>
   * <span data-ttu-id="b02e6-209">**Lägg till AzureEndpoint** lägger till slutpunkter för åtkomst så att klientprogram kan komma åt dessa tjänster SQL Server-instanser på hello Internet.</span><span class="sxs-lookup"><span data-stu-id="b02e6-209">**Add-AzureEndpoint** adds access endpoints so that client applications can access these SQL Server services instances on hello Internet.</span></span> <span data-ttu-id="b02e6-210">Olika portar ges tooContosoSQL1 och ContosoSQL2.</span><span class="sxs-lookup"><span data-stu-id="b02e6-210">Different ports are given tooContosoSQL1 and ContosoSQL2.</span></span>
   * <span data-ttu-id="b02e6-211">**Nya AzureVM** skapar hello nya SQL Server-VM i hello samma molntjänst som ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="b02e6-211">**New-AzureVM** creates hello new SQL Server VM in hello same cloud service as ContosoQuorum.</span></span> <span data-ttu-id="b02e6-212">Du måste placera hello virtuella datorer i samma molntjänst om du vill att de toobe i hello hello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b02e6-212">You must place hello VMs in hello same cloud service if you want them toobe in hello same availability set.</span></span>
4. <span data-ttu-id="b02e6-213">Vänta för varje VM-toobe helt etablerad och varje VM-toodownload dess skrivbord fjärrfil tooyour arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-213">Wait for each VM toobe fully provisioned and for each VM toodownload its remote desktop file tooyour working directory.</span></span> <span data-ttu-id="b02e6-214">Hej `for` loop går igenom hello tre nya virtuella datorer och utför hello kommandon i hello översta klammerparenteser för dem.</span><span class="sxs-lookup"><span data-stu-id="b02e6-214">hello `for` loop cycles through hello three new VMs and executes hello commands inside hello top-level curly brackets for each of them.</span></span>

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until hello VM status is "ReadyRole"
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

    <span data-ttu-id="b02e6-215">hello SQL Server-datorer har nu etablerats och körs, men de är installerade med SQL Server med standardalternativ.</span><span class="sxs-lookup"><span data-stu-id="b02e6-215">hello SQL Server VMs are now provisioned and running, but they're installed with SQL Server with default options.</span></span>

## <a name="initialize-hello-failover-cluster-vms"></a><span data-ttu-id="b02e6-216">Initiera hello redundanskluster virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b02e6-216">Initialize hello failover cluster VMs</span></span>
<span data-ttu-id="b02e6-217">I det här avsnittet måste toomodify hello tre servrar som du vill använda i hello failover-kluster och hello SQL Server-installation.</span><span class="sxs-lookup"><span data-stu-id="b02e6-217">In this section, you need toomodify hello three servers that you'll use in hello failover cluster and hello SQL Server installation.</span></span> <span data-ttu-id="b02e6-218">Närmare bestämt:</span><span class="sxs-lookup"><span data-stu-id="b02e6-218">Specifically:</span></span>

* <span data-ttu-id="b02e6-219">Alla servrar: du behöver tooinstall hello **redundanskluster** funktion.</span><span class="sxs-lookup"><span data-stu-id="b02e6-219">All servers: You need tooinstall hello **Failover Clustering** feature.</span></span>
* <span data-ttu-id="b02e6-220">Alla servrar: du behöver tooadd **CORP\Install** som hello dator **administratör**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-220">All servers: You need tooadd **CORP\Install** as hello machine **administrator**.</span></span>
* <span data-ttu-id="b02e6-221">ContosoSQL1 och ContosoSQL2 endast: du behöver tooadd **CORP\Install** som en **sysadmin** roll i hello standarddatabas.</span><span class="sxs-lookup"><span data-stu-id="b02e6-221">ContosoSQL1 and ContosoSQL2 only: You need tooadd **CORP\Install** as a **sysadmin** role in hello default database.</span></span>
* <span data-ttu-id="b02e6-222">ContosoSQL1 och ContosoSQL2 endast: du behöver tooadd **NT AUTHORITY\System** som loggar in med hello följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="b02e6-222">ContosoSQL1 and ContosoSQL2 only: You need tooadd **NT AUTHORITY\System** as a sign-in with hello following permissions:</span></span>

  * <span data-ttu-id="b02e6-223">ALTER någon tillgänglighetsgrupp</span><span class="sxs-lookup"><span data-stu-id="b02e6-223">Alter any availability group</span></span>
  * <span data-ttu-id="b02e6-224">Ansluta SQL</span><span class="sxs-lookup"><span data-stu-id="b02e6-224">Connect SQL</span></span>
  * <span data-ttu-id="b02e6-225">Visa-Servertillstånd</span><span class="sxs-lookup"><span data-stu-id="b02e6-225">View server state</span></span>
* <span data-ttu-id="b02e6-226">ContosoSQL1 och ContosoSQL2 endast: hello **TCP** protokollet är redan aktiverat på hello SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="b02e6-226">ContosoSQL1 and ContosoSQL2 only: hello **TCP** protocol is already enabled on hello SQL Server VM.</span></span> <span data-ttu-id="b02e6-227">Behöver du dock fortfarande tooopen hello Brandvägg för fjärråtkomst av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b02e6-227">However, you still need tooopen hello firewall for remote access of SQL Server.</span></span>

<span data-ttu-id="b02e6-228">Nu är du redo toostart.</span><span class="sxs-lookup"><span data-stu-id="b02e6-228">Now, you're ready toostart.</span></span> <span data-ttu-id="b02e6-229">Från och med **ContosoQuorum**, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b02e6-229">Beginning with **ContosoQuorum**, follow hello steps below:</span></span>

1. <span data-ttu-id="b02e6-230">Ansluta för**ContosoQuorum** genom att starta hello remote desktop-filer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-230">Connect too**ContosoQuorum** by launching hello remote desktop files.</span></span> <span data-ttu-id="b02e6-231">Använda hello datorn administratörens användarnamn **AzureAdmin** och lösenord **Contoso! 000**, som du angav när du skapade hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-231">Use hello machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created hello VMs.</span></span>
2. <span data-ttu-id="b02e6-232">Kontrollera att hello datorer har anslutits för**corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-232">Verify that hello computers have been successfully joined too**corp.contoso.com**.</span></span>
3. <span data-ttu-id="b02e6-233">Vänta tills hello SQL Server-installation toofinish kör hello automatisk initiering uppgifter innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="b02e6-233">Wait for hello SQL Server installation toofinish running hello automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="b02e6-234">Öppna ett PowerShell-fönster i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="b02e6-234">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="b02e6-235">Installera redundanskluster i Windows hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-235">Install hello Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="b02e6-236">Lägg till **CORP\Install** som lokal administratör.</span><span class="sxs-lookup"><span data-stu-id="b02e6-236">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="b02e6-237">Logga ut från ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="b02e6-237">Sign out of ContosoQuorum.</span></span> <span data-ttu-id="b02e6-238">Du är klar med den här servern nu.</span><span class="sxs-lookup"><span data-stu-id="b02e6-238">You're done with this server now.</span></span>

        logoff.exe

<span data-ttu-id="b02e6-239">Därefter initiera **ContosoSQL1** och **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-239">Next, initialize **ContosoSQL1** and **ContosoSQL2**.</span></span> <span data-ttu-id="b02e6-240">Åtgärderna hello nedan, som är identiska för både SQL Server-datorer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-240">Follow hello steps below, which are identical for both SQL Server VMs.</span></span>

1. <span data-ttu-id="b02e6-241">Ansluta toohello två SQL Server-datorer genom att starta hello remote desktop-filer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-241">Connect toohello two SQL Server VMs by launching hello remote desktop files.</span></span> <span data-ttu-id="b02e6-242">Använda hello datorn administratörens användarnamn **AzureAdmin** och lösenord **Contoso! 000**, som du angav när du skapade hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-242">Use hello machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created hello VMs.</span></span>
2. <span data-ttu-id="b02e6-243">Kontrollera att hello datorer har anslutits för**corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-243">Verify that hello computers have been successfully joined too**corp.contoso.com**.</span></span>
3. <span data-ttu-id="b02e6-244">Vänta tills hello SQL Server-installation toofinish kör hello automatisk initiering uppgifter innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="b02e6-244">Wait for hello SQL Server installation toofinish running hello automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="b02e6-245">Öppna ett PowerShell-fönster i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="b02e6-245">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="b02e6-246">Installera redundanskluster i Windows hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-246">Install hello Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="b02e6-247">Lägg till **CORP\Install** som lokal administratör.</span><span class="sxs-lookup"><span data-stu-id="b02e6-247">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="b02e6-248">Importera hello PowerShell-providern för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b02e6-248">Import hello SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. <span data-ttu-id="b02e6-249">Lägg till **CORP\Install** som hello sysadmin-rollen för hello standardinstansen för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b02e6-249">Add **CORP\Install** as hello sysadmin role for hello default SQL Server instance.</span></span>

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. <span data-ttu-id="b02e6-250">Lägg till **NT AUTHORITY\System** som loggar in med hello tre behörigheterna som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="b02e6-250">Add **NT AUTHORITY\System** as a sign-in with hello three permissions described above.</span></span>

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. <span data-ttu-id="b02e6-251">Öppna hello-brandväggen för fjärråtkomst av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b02e6-251">Open hello firewall for remote access of SQL Server.</span></span>

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. <span data-ttu-id="b02e6-252">Logga ut från både virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-252">Sign out of both VMs.</span></span>

         logoff.exe

<span data-ttu-id="b02e6-253">Slutligen är du redo tooconfigure hello-tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-253">Finally, you're ready tooconfigure hello availability group.</span></span> <span data-ttu-id="b02e6-254">Du kommer att använda hello PowerShell-providern för SQL Server tooperform all hello fungera i **ContosoSQL1**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-254">You'll use hello SQL Server PowerShell Provider tooperform all of hello work on **ContosoSQL1**.</span></span>

## <a name="configure-hello-availability-group"></a><span data-ttu-id="b02e6-255">Konfigurera hello-tillgänglighetsgrupp</span><span class="sxs-lookup"><span data-stu-id="b02e6-255">Configure hello availability group</span></span>
1. <span data-ttu-id="b02e6-256">Ansluta för**ContosoSQL1** igen genom att starta hello remote desktop-filer.</span><span class="sxs-lookup"><span data-stu-id="b02e6-256">Connect too**ContosoSQL1** again by launching hello remote desktop files.</span></span> <span data-ttu-id="b02e6-257">I stället för att logga in med hjälp av hello datorkonto, logga in med hjälp av **CORP\Install**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-257">Instead of signing in by using hello machine account, sign in by using **CORP\Install**.</span></span>
2. <span data-ttu-id="b02e6-258">Öppna ett PowerShell-fönster i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="b02e6-258">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="b02e6-259">Definiera hello följande variabler:</span><span class="sxs-lookup"><span data-stu-id="b02e6-259">Define hello following variables:</span></span>

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
4. <span data-ttu-id="b02e6-260">Importera hello PowerShell-providern för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b02e6-260">Import hello SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. <span data-ttu-id="b02e6-261">Ändra hello SQL Server-tjänstkontot för ContosoSQL1 tooCORP\SQLSvc1.</span><span class="sxs-lookup"><span data-stu-id="b02e6-261">Change hello SQL Server service account for ContosoSQL1 tooCORP\SQLSvc1.</span></span>

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. <span data-ttu-id="b02e6-262">Ändra hello SQL Server-tjänstkontot för ContosoSQL2 tooCORP\SQLSvc2.</span><span class="sxs-lookup"><span data-stu-id="b02e6-262">Change hello SQL Server service account for ContosoSQL2 tooCORP\SQLSvc2.</span></span>

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. <span data-ttu-id="b02e6-263">Hämta **CreateAzureFailoverCluster.ps1** från [Skapa kluster för växling vid fel för Always On-Tillgänglighetsgrupper i Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) toohello lokala arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-263">Download **CreateAzureFailoverCluster.ps1** from [Create Failover Cluster for Always On Availability Groups in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) toohello local working directory.</span></span> <span data-ttu-id="b02e6-264">Du ska använda det här skriptet toohelp som du skapar ett fungerande kluster.</span><span class="sxs-lookup"><span data-stu-id="b02e6-264">You'll use this script toohelp you create a functional failover cluster.</span></span> <span data-ttu-id="b02e6-265">Viktig information om hur Windows-redundanskluster samverkar med hello Azure nätverk, se [hög tillgänglighet och katastrofåterställning återställning för SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b02e6-265">For important information on how Windows Failover Clustering interacts with hello Azure network, see [High availability and disaster recovery for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>
8. <span data-ttu-id="b02e6-266">Ändra tooyour arbetskatalog och skapa hello redundanskluster med hello hämtas skript.</span><span class="sxs-lookup"><span data-stu-id="b02e6-266">Change tooyour working directory and create hello failover cluster with hello downloaded script.</span></span>

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. <span data-ttu-id="b02e6-267">Aktivera Always On-Tillgänglighetsgrupper för hello standard SQL Server-instanser på **ContosoSQL1** och **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="b02e6-267">Enable Always On availability groups for hello default SQL Server instances on **ContosoSQL1** and **ContosoSQL2**.</span></span>

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
10. <span data-ttu-id="b02e6-268">Skapa en katalog och bevilja hello SQL Server-tjänstkonton åtkomstbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="b02e6-268">Create a backup directory and grant permissions for hello SQL Server service accounts.</span></span> <span data-ttu-id="b02e6-269">Du ska använda den här tillgänglighetsdatabasen directory tooprepare hello på hello sekundär replik.</span><span class="sxs-lookup"><span data-stu-id="b02e6-269">You'll use this directory tooprepare hello availability database on hello secondary replica.</span></span>

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. <span data-ttu-id="b02e6-270">Skapa en databas på **ContosoSQL1** kallas **MyDB1**ta både en fullständig säkerhetskopia och en säkerhetskopia och återställa dem på **ContosoSQL2** med hello **WITH NORECOVERY** alternativet.</span><span class="sxs-lookup"><span data-stu-id="b02e6-270">Create a database on **ContosoSQL1** called **MyDB1**, take both a full backup and a log backup, and restore them on **ContosoSQL2** with hello **WITH NORECOVERY** option.</span></span>

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. <span data-ttu-id="b02e6-271">Skapa hello tillgänglighet grupp slutpunkter på hello virtuella SQL Server-datorer och ange hello rätt behörigheter på hello slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="b02e6-271">Create hello availability group endpoints on hello SQL Server VMs and set hello proper permissions on hello endpoints.</span></span>

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
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct1]" -ServerInstance $server2
13. <span data-ttu-id="b02e6-272">Skapa hello tillgänglighetsrepliker.</span><span class="sxs-lookup"><span data-stu-id="b02e6-272">Create hello availability replicas.</span></span>

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
14. <span data-ttu-id="b02e6-273">Skapa slutligen hello tillgänglighetsgruppen och koppling hello sekundär replik toohello tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="b02e6-273">Finally, create hello availability group and join hello secondary replica toohello availability group.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b02e6-274">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b02e6-274">Next steps</span></span>
<span data-ttu-id="b02e6-275">Du har nu har implementerat SQL Server alltid aktiverad genom att skapa en tillgänglighetsgrupp i Azure.</span><span class="sxs-lookup"><span data-stu-id="b02e6-275">You've now successfully implemented SQL Server Always On by creating an availability group in Azure.</span></span> <span data-ttu-id="b02e6-276">tooconfigure en lyssnare för den här tillgänglighetsgruppen finns [konfigurera en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure](../classic/ps-sql-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="b02e6-276">tooconfigure a listener for this availability group, see [Configure an ILB listener for Always On availability groups in Azure](../classic/ps-sql-int-listener.md).</span></span>

<span data-ttu-id="b02e6-277">Annan information om hur du använder SQL Server i Azure, se [SQL Server på Azure virtual machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b02e6-277">For other information about using SQL Server in Azure, see [SQL Server on Azure virtual machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
