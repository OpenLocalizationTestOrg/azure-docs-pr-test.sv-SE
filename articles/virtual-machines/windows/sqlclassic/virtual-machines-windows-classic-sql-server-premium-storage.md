---
title: "Använda Azure Premium-lagring med SQLServer | Microsoft Docs"
description: "Den här artikeln använder resurser som har skapats med den klassiska distributionsmodellen och ger vägledning om hur du använder Azure Premium-lagring med SQL Server som körs på Azure Virtual Machines."
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: jroth
ms.openlocfilehash: 6790db207fc7ec8a4b1546ef07c97ef30abe9513
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a><span data-ttu-id="c4efc-103">Använd Azure Premium Storage med SQL Server på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c4efc-103">Use Azure Premium Storage with SQL Server on Virtual Machines</span></span>
## <a name="overview"></a><span data-ttu-id="c4efc-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c4efc-104">Overview</span></span>
<span data-ttu-id="c4efc-105">[Azure Premium-lagring](../../../storage/common/storage-premium-storage.md) är nästa generation av lagring som innehåller låg latens och hög genomströmning IO.</span><span class="sxs-lookup"><span data-stu-id="c4efc-105">[Azure Premium Storage](../../../storage/common/storage-premium-storage.md) is the next generation of storage that provides low latency and high throughput IO.</span></span> <span data-ttu-id="c4efc-106">Det fungerar bäst för nyckel i/o-intensiv arbetsbelastning, till exempel SQL Server på IaaS [virtuella datorer](https://azure.microsoft.com/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="c4efc-106">It works best for key IO intensive workloads, such as SQL Server on IaaS [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4efc-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c4efc-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c4efc-108">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-108">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="c4efc-109">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="c4efc-110">Den här artikeln innehåller planering och vägledning för att migrera en virtuell dator som kör SQL Server att använda Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c4efc-110">This article provides planning and guidance for migrating a Virtual Machine running SQL Server to use Premium Storage.</span></span> <span data-ttu-id="c4efc-111">Detta inkluderar Azure-infrastrukturen (nätverk, lagring) och Gäst Windows VM steg.</span><span class="sxs-lookup"><span data-stu-id="c4efc-111">This includes Azure infrastructure (networking, storage) and guest Windows VM steps.</span></span> <span data-ttu-id="c4efc-112">Exemplet i den [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) visar en fullständig omfattande slutpunkt till slutpunkt-migrering av hur du flyttar större virtuella datorer för att dra nytta av bättre lokala SSD-lagring med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4efc-112">The example in the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) shows a full comprehensive end to end migration of how to move larger VMs to take advantage of improved local SSD storage with PowerShell.</span></span>

<span data-ttu-id="c4efc-113">Det är viktigt att förstå slutpunkt till slutpunkt-processen genom att använda Azure Premium-lagring med SQL Server på IAAS-VM.</span><span class="sxs-lookup"><span data-stu-id="c4efc-113">It is important to understand the end-to-end process of utilizing Azure Premium Storage with SQL Server on IAAS VMs.</span></span> <span data-ttu-id="c4efc-114">Detta omfattar:</span><span class="sxs-lookup"><span data-stu-id="c4efc-114">This includes:</span></span>

* <span data-ttu-id="c4efc-115">Identifiering av förutsättningar för att kunna använda Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c4efc-115">Identification of the prerequisites to use Premium Storage.</span></span>
* <span data-ttu-id="c4efc-116">Exempel på distribution av SQL Server på IaaS till Premium-lagring för nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="c4efc-116">Examples of deploying SQL Server on IaaS to Premium Storage for new deployments.</span></span>
* <span data-ttu-id="c4efc-117">Exempel på migrera befintliga distributioner, både fristående servrar och distributioner med hjälp av SQL Always On-Tillgänglighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="c4efc-117">Examples of migrating existing deployments, both stand-alone servers and deployments using SQL Always On Availability Groups.</span></span>
* <span data-ttu-id="c4efc-118">Möjliga migrering metoder.</span><span class="sxs-lookup"><span data-stu-id="c4efc-118">Possible migration approaches.</span></span>
* <span data-ttu-id="c4efc-119">Fullständig slutpunkt till slutpunkt i exemplet visar Azure, Windows och SQL Server stegen för migrering av en befintlig Always On-implementering.</span><span class="sxs-lookup"><span data-stu-id="c4efc-119">Full end-to-end example showing Azure, Windows, and SQL Server steps for the migration of an existing Always On implementation.</span></span>

<span data-ttu-id="c4efc-120">Mer information om SQL Server i Azure Virtual Machines finns [SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4efc-120">For more background information on SQL Server in Azure Virtual Machines, see [SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

<span data-ttu-id="c4efc-121">**Skapad av:** Mikael Sol **Teknisk granskare:** Thomas Carlos Vargas sill, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Pettersson.</span><span class="sxs-lookup"><span data-stu-id="c4efc-121">**Author:** Daniel Sol **Technical Reviewers:** Luis Carlos Vargas Herring, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.</span></span>

## <a name="prerequisites-for-premium-storage"></a><span data-ttu-id="c4efc-122">Krav för Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="c4efc-122">Prerequisites for Premium Storage</span></span>
<span data-ttu-id="c4efc-123">Det finns flera förutsättningar för att använda Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c4efc-123">There are several prerequisites for using Premium Storage.</span></span>

### <a name="machine-size"></a><span data-ttu-id="c4efc-124">Storleken på datorn</span><span class="sxs-lookup"><span data-stu-id="c4efc-124">Machine size</span></span>
<span data-ttu-id="c4efc-125">För att använda Premium-lagring behöver du använda DS-serien virtuella datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="c4efc-125">For using Premium Storage you will need to use DS series Virtual Machines (VM).</span></span> <span data-ttu-id="c4efc-126">Om du inte har använt DS-serien datorer i din molntjänst innan måste du ta bort den befintliga virtuella datorn, hålla anslutna diskar och sedan skapa en ny molntjänst innan du återskapa den virtuella datorn som DS * rollstorleken.</span><span class="sxs-lookup"><span data-stu-id="c4efc-126">If you have not used DS Series machines in your cloud service before, you must delete the existing VM, keep the attached disks, and then create a new cloud service before recreating the VM as DS* role size.</span></span> <span data-ttu-id="c4efc-127">Mer information om storlekar för virtuella datorer finns [virtuell dator och Molntjänststorlekar för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c4efc-127">For more information on Virtual Machine sizes, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="cloud-services"></a><span data-ttu-id="c4efc-128">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="c4efc-128">Cloud services</span></span>
<span data-ttu-id="c4efc-129">Du kan bara använda DS * virtuella datorer med Premium-lagring när de skapas i en ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c4efc-129">You can only use DS* VMs with Premium Storage when they are created in a new cloud service.</span></span> <span data-ttu-id="c4efc-130">Om du använder SQL Server alltid aktiverad i Azure ska alltid på lyssnaren avser Azure interna eller externa belastningen belastningsutjämnaren IP-adressen som är associerad med en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-130">If you are using SQL Server Always On in Azure, the Always On Listener will refer to the Azure Internal or External Load Balancer IP address that is associated with a cloud service.</span></span> <span data-ttu-id="c4efc-131">Den här artikeln fokuserar på hur du migrerar samtidigt är tillgänglig i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="c4efc-131">This article focuses on how to migrate while maintaining availability in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="c4efc-132">En DS * serie måste vara den första virtuella dator som har distribuerats till en ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c4efc-132">A DS* Series must be the first VM that is deployed to the new Cloud Service.</span></span>
>
>

### <a name="regional-vnets"></a><span data-ttu-id="c4efc-133">Regional VNET</span><span class="sxs-lookup"><span data-stu-id="c4efc-133">Regional VNETS</span></span>
<span data-ttu-id="c4efc-134">För DS * virtuella datorer måste du konfigurera virtuella nätverk (VNET) värd för dina virtuella datorer ska regionala.</span><span class="sxs-lookup"><span data-stu-id="c4efc-134">For DS* VMs you must configure the Virtual Network (VNET) hosting your VMs to be regional.</span></span> <span data-ttu-id="c4efc-135">Detta ”utökar” VNET är att ge större virtuella datorer att etableras i andra kluster och tillåta kommunikation mellan dem.</span><span class="sxs-lookup"><span data-stu-id="c4efc-135">This “widens” the VNET is to allow the larger VMs to be provisioned in other clusters and allow communication between them.</span></span> <span data-ttu-id="c4efc-136">I följande skärmbild visar den markerade platsen regional Vnet medan det första resultatet visas en ”smala” virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="c4efc-136">In the following screenshot, the highlighted Location shows regional VNETs, whereas the first result shows a “narrow” VNET.</span></span>

![RegionalVNET][1]

<span data-ttu-id="c4efc-138">Du kan höja ett supportärende för Microsoft för att migrera till ett regionalt VNET, Microsoft kommer ändrar och sedan ändra egenskapen AffinityGroup i nätverkskonfigurationen för att slutföra migreringen till regionalt Vnet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-138">You can raise a Microsoft support ticket to migrate to a regional VNET, Microsoft will make a change, then to complete the migration to regional VNETs, change the property AffinityGroup in the network configuration.</span></span> <span data-ttu-id="c4efc-139">Exportera nätverkskonfigurationen i PowerShell och Ersätt den **AffinityGroup** egenskap i den **VirtualNetworkSite** element med en **plats** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-139">First export the Network Configuration in PowerShell, and then replace the **AffinityGroup** property in the **VirtualNetworkSite** element with a **Location** property.</span></span> <span data-ttu-id="c4efc-140">Ange `Location = XXXX` där `XXXX` är en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="c4efc-140">Specify `Location = XXXX` where `XXXX` is an Azure region.</span></span> <span data-ttu-id="c4efc-141">Importera den nya konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-141">Then import the new configuration.</span></span>

<span data-ttu-id="c4efc-142">Till exempel att ta hänsyn till följande konfiguration för virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="c4efc-142">For example, considering the following VNET configuration:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

<span data-ttu-id="c4efc-143">Om du vill flytta det till ett regionalt VNET i Västeuropa, ändra konfigurationen av följande:</span><span class="sxs-lookup"><span data-stu-id="c4efc-143">To move this to a regional VNET in West Europe, change the configuration to the following:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a><span data-ttu-id="c4efc-144">Lagringskonton</span><span class="sxs-lookup"><span data-stu-id="c4efc-144">Storage accounts</span></span>
<span data-ttu-id="c4efc-145">Du behöver skapa ett nytt lagringskonto som är konfigurerad för Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c4efc-145">You will need to create a new storage account that is configured for Premium Storage.</span></span> <span data-ttu-id="c4efc-146">Observera att användningen av Premium-lagring är inställt på lagringskontot, inte på enskilda virtuella hårddiskar, men när du använder en DS * serien virtuell dator kan du bifoga VHD från Premium och standardlagring konton.</span><span class="sxs-lookup"><span data-stu-id="c4efc-146">Note that the use of Premium Storage is set at the storage account, not on individual VHDs, however when using a DS* Series VM you can attach VHD’s from Premium and Standard Storage accounts.</span></span> <span data-ttu-id="c4efc-147">Du kan överväga att detta om du inte vill att OS-VHD till Premium-lagring-kontot.</span><span class="sxs-lookup"><span data-stu-id="c4efc-147">You may consider this if you do not want to place the OS VHD on to the Premium Storage account.</span></span>

<span data-ttu-id="c4efc-148">Följande **ny AzureStorageAccountPowerShell** med ”Premium_LRS” **typen** skapar ett Premiumlagringskonto:</span><span class="sxs-lookup"><span data-stu-id="c4efc-148">The following **New-AzureStorageAccountPowerShell** command with the "Premium_LRS" **Type** creates a Premium Storage Account:</span></span>

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a><span data-ttu-id="c4efc-149">Inställningar för cachelagring av virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="c4efc-149">VHDs Cache Settings</span></span>
<span data-ttu-id="c4efc-150">Den största skillnaden mellan att skapa diskar som ingår i ett Premium Storage-konto är diskcache-inställningen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-150">The main difference between creating disks that are part of a Premium Storage account is the disk cache setting.</span></span> <span data-ttu-id="c4efc-151">För SQL Server-Data volym diskar det rekommenderas att du använder '**Läs cachelagring**'.</span><span class="sxs-lookup"><span data-stu-id="c4efc-151">For SQL Server Data volume disks it is recommended that you use ‘**Read Caching**’.</span></span> <span data-ttu-id="c4efc-152">För transaktionen loggvolymer diskcache-inställningen ska vara inställd på '**ingen**'.</span><span class="sxs-lookup"><span data-stu-id="c4efc-152">For Transaction log volumes, the disk cache setting should be set to ‘**None**’.</span></span> <span data-ttu-id="c4efc-153">Detta skiljer sig från rekommendationerna för Standard Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="c4efc-153">This is different from the recommendations for Standard Storage accounts.</span></span>

<span data-ttu-id="c4efc-154">Cache-inställningen kan inte ändras när de virtuella hårddiskarna har bifogats.</span><span class="sxs-lookup"><span data-stu-id="c4efc-154">Once the VHDs have been attached, the cache setting cannot be altered.</span></span> <span data-ttu-id="c4efc-155">Du måste koppla från och Återanslut den virtuella Hårddisken med en uppdaterad cache-inställningen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-155">You would need to detach and reattach the VHD with an updated cache setting.</span></span>

### <a name="windows-storage-spaces"></a><span data-ttu-id="c4efc-156">Lagringsutrymmen för Windows</span><span class="sxs-lookup"><span data-stu-id="c4efc-156">Windows storage spaces</span></span>
<span data-ttu-id="c4efc-157">Du kan använda [Windows lagringsutrymmen](https://technet.microsoft.com/library/hh831739.aspx) som du gjorde med föregående standardlagring kommer detta att du kan migrera en virtuell dator som redan använder lagringsutrymmen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-157">You can use [Windows Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx) as you did with previous Standard Storage, this will allow you to migrate a VM that is already utilizing Storage Spaces.</span></span> <span data-ttu-id="c4efc-158">Exemplet i [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (steg 9 och framåt) visar Powershell-koden för att extrahera och importera en virtuell dator med flera anslutna virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-158">The example in [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (step 9 and forward) demonstrates the Powershell code to extract and import a VM with multiple attached VHDs.</span></span>

<span data-ttu-id="c4efc-159">Lagringspooler som användes med Standard-Azure storage-konto för att förbättra genomflöde och minska svarstiden.</span><span class="sxs-lookup"><span data-stu-id="c4efc-159">Storage Pools were used with Standard Azure storage account to enhance throughput and reduce latency.</span></span> <span data-ttu-id="c4efc-160">Du kan hitta värdet i testning lagringspooler med Premium-lagring för nya distributioner, men de lägger till ytterligare komplexitet med Lagringsinställningar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-160">You might find value in testing Storage Pools with Premium Storage for new deployments, but they do add additional complexity with storage setup.</span></span>

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a><span data-ttu-id="c4efc-161">Så här hittar du vilka Azure virtuella diskar kartan lagringspooler</span><span class="sxs-lookup"><span data-stu-id="c4efc-161">How to find which Azure Virtual Disks map to storage pools</span></span>
<span data-ttu-id="c4efc-162">Eftersom det finns olika cache inställningen rekommendationer för anslutna virtuella hårddiskar, kanske du vill kopiera de virtuella hårddiskarna till ett Premium Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c4efc-162">As there are different cache setting recommendations for attached VHDs, you might decide to copy the VHDs to a Premium Storage account.</span></span> <span data-ttu-id="c4efc-163">När du återansluta dem till den nya DS-serien VM, kanske du måste ändra inställningar för cachelagring av.</span><span class="sxs-lookup"><span data-stu-id="c4efc-163">However, when you reattach them to the new DS series VM, you might need to alter the cache settings.</span></span> <span data-ttu-id="c4efc-164">Det är enklare att använda Premium-lagring rekommenderade inställningar när du har separata virtuella hårddiskar för SQL-datafiler och loggfiler (istället för en enskild virtuell Hårddisk som innehåller både).</span><span class="sxs-lookup"><span data-stu-id="c4efc-164">It is simpler to apply the Premium Storage recommended cache settings when you have separate VHDs for the SQL Data files and log files (rather than a single VHD that contains both).</span></span>

> [!NOTE]
> <span data-ttu-id="c4efc-165">Om du har SQL Server-data och loggfilen filer på samma volym beror cachelagring alternativ du väljer på i/o-åtkomstmönster för databasarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-165">If you have SQL Server data and log files on the same volume, the caching option you choose depends on the IO access patterns for your database workloads.</span></span> <span data-ttu-id="c4efc-166">Testa bara kan visa vilket alternativ för cachelagring är bäst för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="c4efc-166">Only testing can demonstrate which caching option is best for this scenario.</span></span>
>
>

<span data-ttu-id="c4efc-167">Men om du använder Windows lagringsutrymmen som består av flera virtuella hårddiskar som du behöver titta på din ursprungliga skript för att identifiera vilket anslutna virtuella hårddiskar finns i vilka specifika poolen, så du kan ange inställningar för cachelagring av därefter för varje disk.</span><span class="sxs-lookup"><span data-stu-id="c4efc-167">However, if you are using Windows Storage Spaces which are made up of multiple VHDs you will need to look at your original scripts to identify which attached VHDs are in what specific pool, so you can then set the cache settings accordingly for each disk.</span></span>

<span data-ttu-id="c4efc-168">Du kan använda följande steg för att fastställa mappningen disklagring/poolen om du inte har ursprungliga skriptet som är tillgängliga för att visa dig som virtuella hårddiskar som mappas till lagringspoolen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-168">If you do not have original script available to show you which VHDs map to the storage pool, you can use the following steps to determine the disk/storage pool mapping.</span></span>

<span data-ttu-id="c4efc-169">Använd följande steg för varje disk:</span><span class="sxs-lookup"><span data-stu-id="c4efc-169">For each disk, use the following steps:</span></span>

1. <span data-ttu-id="c4efc-170">Hämta listan över diskar som är kopplad till virtuell dator med den **Get-AzureVM** kommando:</span><span class="sxs-lookup"><span data-stu-id="c4efc-170">Get list of disks attached to VM with the **Get-AzureVM** command:</span></span>

    <span data-ttu-id="c4efc-171">Get-AzureVM - ServiceName <servicename> -namnet <vmname> | Get-AzureDataDisk</span><span class="sxs-lookup"><span data-stu-id="c4efc-171">Get-AzureVM -ServiceName <servicename> -Name <vmname> | Get-AzureDataDisk</span></span>
2. <span data-ttu-id="c4efc-172">Observera Diskname och LUN.</span><span class="sxs-lookup"><span data-stu-id="c4efc-172">Note the Diskname and LUN.</span></span>

    ![DisknameAndLUN][2]
3. <span data-ttu-id="c4efc-174">Fjärrskrivbord till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c4efc-174">Remote desktop into the VM.</span></span> <span data-ttu-id="c4efc-175">Gå till **Datorhantering** | **Enhetshanteraren** | **diskenheter**.</span><span class="sxs-lookup"><span data-stu-id="c4efc-175">Then go to **Computer Management** | **Device Manager** | **Disk Drives**.</span></span> <span data-ttu-id="c4efc-176">Titta på egenskaperna för varje 'Microsoft virtuella diskar:</span><span class="sxs-lookup"><span data-stu-id="c4efc-176">Look at the properties of each of the ‘Microsoft Virtual Disks’</span></span>

    ![VirtualDiskProperties][3]
4. <span data-ttu-id="c4efc-178">LUN-nummer här är en referens till LUN-nummer som du anger när du kopplar den virtuella Hårddisken till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c4efc-178">The LUN number here is a reference to the LUN number you specify when attaching the VHD to the VM.</span></span>
5. <span data-ttu-id="c4efc-179">För den ”Microsoft Virtual Disk” gå till den **information** fliken i den **egenskapen** lista, gå till **drivrutinen nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="c4efc-179">For the ‘Microsoft Virtual Disk’ go to the **Details** tab, then in the **Property** list, go to **Driver Key**.</span></span> <span data-ttu-id="c4efc-180">I den **värdet**, Observera den **Offset**, vilket är 0002 i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="c4efc-180">In the **Value**, note the **Offset**, which is 0002 in the following screenshot.</span></span> <span data-ttu-id="c4efc-181">0002 anger PhysicalDisk2 som refererar till lagringspoolen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-181">The 0002 denotes the PhysicalDisk2 that the storage pool references.</span></span>

    ![VirtualDiskPropertyDetails][4]
6. <span data-ttu-id="c4efc-183">För varje lagringspool dump ut de associera diskarna:</span><span class="sxs-lookup"><span data-stu-id="c4efc-183">For each storage pool, dump out the associated disks:</span></span>

    <span data-ttu-id="c4efc-184">Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk</span><span class="sxs-lookup"><span data-stu-id="c4efc-184">Get-StoragePool -FriendlyName AMS1pooldata | Get-PhysicalDisk</span></span>

    ![GetStoragePool][5]

<span data-ttu-id="c4efc-186">Nu kan du använda koppla den här informationen för att associera virtuella hårddiskar till fysiska diskar i lagringspooler.</span><span class="sxs-lookup"><span data-stu-id="c4efc-186">Now you can use this information to associate attached VHDs to Physical Disks in Storage Pools.</span></span>

<span data-ttu-id="c4efc-187">När du har mappat virtuella hårddiskar till fysiska diskar i lagringspooler som du kan koppla bort och kopiera dem över till en Premium Storage-konto kan sedan koppla dem till rätt cache-inställningen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-187">Once you have mapped VHDs to Physical Disks in Storage Pools you can then detach and copy them over to a Premium Storage account, then attach them with the correct cache setting.</span></span> <span data-ttu-id="c4efc-188">Se exemplet i den [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), steg 8 till och med 12.</span><span class="sxs-lookup"><span data-stu-id="c4efc-188">Please see the example in the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), steps 8 through 12.</span></span> <span data-ttu-id="c4efc-189">Dessa steg visar hur du extrahera diskkonfiguration VM-ansluten virtuell Hårddisk till en CSV-fil, kopiera de virtuella hårddiskarna, ändra inställningar för cachelagring av disk-konfiguration och slutligen distribuera den virtuella datorn som en serie DS VM med anslutna diskar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-189">These steps show how to extract a VM-attached VHD disk configuration to a CSV file, copy the VHDs, alter the disk configuration cache settings, and finally redeploy the VM as a DS series VM with all the attached disks.</span></span>

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a><span data-ttu-id="c4efc-190">VM-lagring bandbredd och kapaciteten för lagring av virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="c4efc-190">VM storage bandwidth and VHD storage throughput</span></span>
<span data-ttu-id="c4efc-191">Mängden lagringsprestanda beror på den angivna DS * VM-storleken och VHD-storlekar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-191">The amount of storage performance depends on the DS* VM size specified and the VHD sizes.</span></span> <span data-ttu-id="c4efc-192">De virtuella datorerna har olika tillägg för antalet virtuella hårddiskar som kan bifogas och maximal bandbredd som de stöder (MB/s).</span><span class="sxs-lookup"><span data-stu-id="c4efc-192">The VMs have different allowances for the number of VHDs that can be attached and the maximum bandwidth they will support (MB/s).</span></span> <span data-ttu-id="c4efc-193">Viss bandbredd-siffror finns [virtuell dator och Molntjänststorlekar för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c4efc-193">For the specific bandwidth numbers, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="c4efc-194">Ökad IOPS uppnås med större storlekar för diskar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-194">Increased IOPS are achieved with larger disk sizes.</span></span> <span data-ttu-id="c4efc-195">Du bör överväga när du funderar på sökvägen för migreringen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-195">You should consider this when you think about your migration path.</span></span> <span data-ttu-id="c4efc-196">Mer information [finns i tabellen för IOPS och disktyper](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="c4efc-196">For details, [see the table for IOPS and Disk Types](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span></span>

<span data-ttu-id="c4efc-197">Slutligen bör du att virtuella datorer har olika maximal disk bandbredder de stöder för alla diskar som är anslutna.</span><span class="sxs-lookup"><span data-stu-id="c4efc-197">Finally, consider that VMs have different maximum disk bandwidths they will support for all disks attached.</span></span> <span data-ttu-id="c4efc-198">Du kan fylla maximal disk-bandbredden som är tillgänglig för Virtuella datorns rollstorlek under hög belastning.</span><span class="sxs-lookup"><span data-stu-id="c4efc-198">Under high load, you could saturate the maximum disk bandwidth available for that VM role size.</span></span> <span data-ttu-id="c4efc-199">Till exempel stöder en Standard_DS14 upp till 512 MB/s. Därför kan du fylla diskbandbredden som av den virtuella datorn med tre P30 diskar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-199">For example a Standard_DS14 will support up to 512MB/s; therefore, with three P30 disks you could saturate the disk bandwidth of the VM.</span></span> <span data-ttu-id="c4efc-200">Men i det här exemplet genomströmning gränsen kan överskridas beroende på blandning av läsning och skrivning IOs.</span><span class="sxs-lookup"><span data-stu-id="c4efc-200">But in this example, the throughput limit could be exceeded depending on the mix of read and write IOs.</span></span>

## <a name="new-deployments"></a><span data-ttu-id="c4efc-201">Nya distributioner</span><span class="sxs-lookup"><span data-stu-id="c4efc-201">New deployments</span></span>
<span data-ttu-id="c4efc-202">I följande två avsnitt visar hur du kan distribuera virtuella SQL Server-datorer till Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c4efc-202">The next two sections demonstrate how you can deploy SQL Server VMs to Premium Storage.</span></span> <span data-ttu-id="c4efc-203">Som nämnts så bör behöver du inte nödvändigtvis placera OS-disken till Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c4efc-203">As mentioned before, you do not necessarily need to place the OS disk onto Premium storage.</span></span> <span data-ttu-id="c4efc-204">Du kan välja att göra det om du avser att placera alla i/o-arbetsbelastningar på OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="c4efc-204">You might choose to do this if you are intending to place any intensive IO workloads on the OS VHD.</span></span>

<span data-ttu-id="c4efc-205">Det första exemplet visar genom att använda befintliga avbildningar i Azure-galleriet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-205">The first example demonstrates utilizing existing Azure Gallery Images.</span></span> <span data-ttu-id="c4efc-206">Det andra exemplet visar hur du använder en anpassad VM-avbildning som du har i ett befintligt lagringskonto som Standard.</span><span class="sxs-lookup"><span data-stu-id="c4efc-206">The second example shows how to use a custom VM image that you have in an existing Standard storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="c4efc-207">Dessa exempel förutsätter att du redan har skapat ett regionalt VNET.</span><span class="sxs-lookup"><span data-stu-id="c4efc-207">These examples assume that you have already created a Regional VNET.</span></span>
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a><span data-ttu-id="c4efc-208">Skapa en ny virtuell dator med Premium-lagring med bild galleriet</span><span class="sxs-lookup"><span data-stu-id="c4efc-208">Create a new VM with Premium Storage with Gallery Image</span></span>
<span data-ttu-id="c4efc-209">Exemplet nedan visar hur du placera OS-VHD till premium-lagring och bifogar Premium Storage virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-209">The example below shows how to place the OS VHD onto premium storage and attach Premium Storage VHDs.</span></span> <span data-ttu-id="c4efc-210">Du kan också placera OS-disken i ett standardlagringskonto och sedan koppla virtuella hårddiskar som finns i ett Premium Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c4efc-210">However, you can also place the OS disk in a Standard Storage account and then attach VHDs that reside in a Premium Storage account.</span></span> <span data-ttu-id="c4efc-211">Båda scenarierna är visas.</span><span class="sxs-lookup"><span data-stu-id="c4efc-211">Both scenarios are demonstrated.</span></span>

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a><span data-ttu-id="c4efc-212">Steg 1: Skapa ett Premiumlagringskonto</span><span class="sxs-lookup"><span data-stu-id="c4efc-212">Step 1: Create a Premium Storage Account</span></span>
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a><span data-ttu-id="c4efc-213">Steg 2: Skapa en ny molntjänst</span><span class="sxs-lookup"><span data-stu-id="c4efc-213">Step 2: Create a New Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a><span data-ttu-id="c4efc-214">Steg 3: Reservera en Cloud Service VIP (valfritt)</span><span class="sxs-lookup"><span data-stu-id="c4efc-214">Step 3: Reserve a Cloud Service VIP (Optional)</span></span>
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a><span data-ttu-id="c4efc-215">Steg 4: Skapa en VM-behållare</span><span class="sxs-lookup"><span data-stu-id="c4efc-215">Step 4: Create a VM Container</span></span>
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a><span data-ttu-id="c4efc-216">Steg 5: Avyttring OS VHD Standard eller Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="c4efc-216">Step 5: Placing OS VHD on Standard or Premium Storage</span></span>
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a><span data-ttu-id="c4efc-217">Steg 6: Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c4efc-217">Step 6: Create VM</span></span>
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a><span data-ttu-id="c4efc-218">Skapa en ny virtuell dator för att använda Premium-lagring med en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="c4efc-218">Create a new VM to use Premium Storage with a custom image</span></span>
<span data-ttu-id="c4efc-219">Det här scenariot visar där du har befintliga anpassade avbildningar som finns i ett standardlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c4efc-219">This scenario demonstrates where you have existing customized images that reside in a Standard Storage account.</span></span> <span data-ttu-id="c4efc-220">Som tidigare nämnts om du vill placera OS-VHD på Premium-lagring behöver du kopiera den bild som finns i standardlagringskontot och överför dem till en Premium-lagring innan den kan användas.</span><span class="sxs-lookup"><span data-stu-id="c4efc-220">As mentioned if you want to place the OS VHD on Premium Storage you will need to copy the image that exists in the Standard Storage account and transfer them to a Premium Storage before it can be used.</span></span> <span data-ttu-id="c4efc-221">Om du har en bild på lokalt, kan du också använda den här metoden för att kopiera som direkt till Premium-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="c4efc-221">If you have an image on-premises, you can also use this method to copy that directly to the Premium Storage account.</span></span>

#### <a name="step-1-create-storage-account"></a><span data-ttu-id="c4efc-222">Steg 1: Skapa Storage-konto</span><span class="sxs-lookup"><span data-stu-id="c4efc-222">Step 1: Create Storage Account</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a><span data-ttu-id="c4efc-223">Steg 2 Skapa molntjänst</span><span class="sxs-lookup"><span data-stu-id="c4efc-223">Step 2 Create Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a><span data-ttu-id="c4efc-224">Steg 3: Använd befintlig avbildning</span><span class="sxs-lookup"><span data-stu-id="c4efc-224">Step 3: Use existing image</span></span>
<span data-ttu-id="c4efc-225">Du kan använda en befintlig avbildning.</span><span class="sxs-lookup"><span data-stu-id="c4efc-225">You can use an existing image.</span></span> <span data-ttu-id="c4efc-226">Du kan [ta en bild av en befintlig dator](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c4efc-226">Or, you can [take an image of an existing machine](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="c4efc-227">Observera den datorn som du avbildningen inte behöver DS * dator.</span><span class="sxs-lookup"><span data-stu-id="c4efc-227">Note the machine you image does not have to be DS* machine.</span></span> <span data-ttu-id="c4efc-228">När du har bilden följande steg visar hur du kopierar den till Premium Storage-konto med den **Start AzureStorageBlobCopy** PowerShell-kommandot.</span><span class="sxs-lookup"><span data-stu-id="c4efc-228">Once you have the image, the following steps show how to copy it to the Premium Storage account with the **Start-AzureStorageBlobCopy** PowerShell commandlet.</span></span>

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a><span data-ttu-id="c4efc-229">Steg 4: Kopiera Blob mellan Storage-konton</span><span class="sxs-lookup"><span data-stu-id="c4efc-229">Step 4: Copy Blob between Storage Accounts</span></span>
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a><span data-ttu-id="c4efc-230">Steg 5: Kontrollera regelbundet-kopian status:</span><span class="sxs-lookup"><span data-stu-id="c4efc-230">Step 5: Regularly check copy status:</span></span>
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a><span data-ttu-id="c4efc-231">Steg 6: Lägg till avbildning disk i Azure-disken databasen i prenumerationen</span><span class="sxs-lookup"><span data-stu-id="c4efc-231">Step 6: Add Image disk to Azure disk Repository in Subscription</span></span>
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> <span data-ttu-id="c4efc-232">Du kanske att även om rapporter som lyckades, du kan fortfarande få ett lån diskfel.</span><span class="sxs-lookup"><span data-stu-id="c4efc-232">You may find that even though the status reports as success, you could still get a disk lease error.</span></span> <span data-ttu-id="c4efc-233">I så fall väntar du cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="c4efc-233">In this case, wait about 10 minutes.</span></span>
>
>

#### <a name="step-7--build-the-vm"></a><span data-ttu-id="c4efc-234">Steg 7: Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c4efc-234">Step 7:  Build the VM</span></span>
<span data-ttu-id="c4efc-235">Här skapar du den virtuella datorn från avbildningen och ansluta två Premium Storage virtuella hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="c4efc-235">Here you are building the VM from your image and attaching two Premium Storage VHDs:</span></span>

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a><span data-ttu-id="c4efc-236">Befintliga distributioner som inte använder alltid på Tillgänglighetsgrupper</span><span class="sxs-lookup"><span data-stu-id="c4efc-236">Existing deployments that do not use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="c4efc-237">Befintliga distributioner först finns i [krav](#prerequisites-for-premium-storage) i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-237">For existing deployments, first see the [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="c4efc-238">Det finns olika överväganden för SQL Server-distributioner som inte använder alltid på Tillgänglighetsgrupper och det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="c4efc-238">There are different considerations for SQL Server deployments that do not use Always On Availability Groups and those that do.</span></span> <span data-ttu-id="c4efc-239">Om du inte använder alltid på och har en befintlig fristående SQL Server kan du uppgradera till Premium-lagring med hjälp av ett nytt cloud service och storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c4efc-239">If you are not using Always On and have an existing standalone SQL Server, you can upgrade to Premium Storage by using a new cloud service and storage account.</span></span> <span data-ttu-id="c4efc-240">Överväg följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="c4efc-240">Consider the following options:</span></span>

* <span data-ttu-id="c4efc-241">**Skapa en ny SQL Server-VM**.</span><span class="sxs-lookup"><span data-stu-id="c4efc-241">**Create a new SQL Server VM**.</span></span> <span data-ttu-id="c4efc-242">Du kan skapa en ny SQL Server-VM som använder ett premiumlagringskonto, enligt beskrivningen i nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="c4efc-242">You can create a new SQL Server VM that uses a Premium Storage account, as documented in New Deployments.</span></span> <span data-ttu-id="c4efc-243">Sedan säkerhetskopiera och återställa SQL Server-databaserna konfiguration och användare.</span><span class="sxs-lookup"><span data-stu-id="c4efc-243">Then backup and restore your SQL Server configuration and user databases.</span></span> <span data-ttu-id="c4efc-244">Programmet måste uppdateras för att referera till den nya SQL-servern om den används internt eller externt.</span><span class="sxs-lookup"><span data-stu-id="c4efc-244">The application will need to be updated to reference the new SQL Server if it is being accessed internally or externally.</span></span> <span data-ttu-id="c4efc-245">Du skulle behöva kopiera alla out-of-db-objekt som om du höll på en sida vid sida (SxS) SQL Server-migrering.</span><span class="sxs-lookup"><span data-stu-id="c4efc-245">You would need to copy all ‘out of db’ objects as if you were doing a Side by Side (SxS) SQL Server migration.</span></span> <span data-ttu-id="c4efc-246">Detta inkluderar objekt som inloggningar, certifikat och länkade servrar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-246">This includes objects such as logins, certificates, and linked servers.</span></span>
* <span data-ttu-id="c4efc-247">**Migrera en befintlig SQL Server-VM**.</span><span class="sxs-lookup"><span data-stu-id="c4efc-247">**Migrate an existing SQL Server VM**.</span></span> <span data-ttu-id="c4efc-248">Detta kräver att SQL Server-VM tas offline och sedan överföra den till en ny molntjänst, som innehåller alla dess anslutna virtuella hårddiskar har kopierats till Premium-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="c4efc-248">This will require taking the SQL Server VM offline, then transferring it to a new cloud service, which includes copying all of its attached VHDs to the Premium Storage account.</span></span> <span data-ttu-id="c4efc-249">När den virtuella datorn är online kommer Servervärdnamn som innan referera till programmet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-249">When the VM comes online, the application will reference the server host name as before.</span></span> <span data-ttu-id="c4efc-250">Tänk på att storleken på den befintliga disken påverkar prestandaegenskaperna.</span><span class="sxs-lookup"><span data-stu-id="c4efc-250">Be aware that the size of the existing disk will affect the performance characteristics.</span></span> <span data-ttu-id="c4efc-251">Till exempel hämtar en 400 GB disk avrundat till en P20.</span><span class="sxs-lookup"><span data-stu-id="c4efc-251">For example, a 400 GB disk gets rounded up to a P20.</span></span> <span data-ttu-id="c4efc-252">Om du vet att du inte behöver som diskprestanda, kan du skapa den virtuella datorn som en virtuell dator DS-serien och bifoga Premium Storage virtuella hårddiskar i specifikationen storlek och prestanda som du behöver.</span><span class="sxs-lookup"><span data-stu-id="c4efc-252">If you know that you do not require that disk performance, then you could recreate the VM as a DS Series VM, and attach Premium Storage VHDs of the size/performance specification you require.</span></span> <span data-ttu-id="c4efc-253">Sedan kan du koppla från och Återanslut SQL DB-filer.</span><span class="sxs-lookup"><span data-stu-id="c4efc-253">Then you could detach and reattach the SQL DB files.</span></span>

> [!NOTE]
> <span data-ttu-id="c4efc-254">När du kopierar VHD-diskar som du bör vara medveten om storlek, beroende på storleken innebär vilka Premium Storage disktyp de hör till, detta avgör disk prestanda specifikation.</span><span class="sxs-lookup"><span data-stu-id="c4efc-254">When copying the VHD disks you should be aware of the size, depending on the size will mean what Premium Storage Disk type they fall into, this determines disk performance specification.</span></span> <span data-ttu-id="c4efc-255">Kommer Azure att avrunda till närmaste disk storlek, så om du har en 400 GB disk, detta ska avrundas till en P20.</span><span class="sxs-lookup"><span data-stu-id="c4efc-255">Azure will round up to the nearest disk size, so if you have a 400GB disk, this will be rounded up to a P20.</span></span> <span data-ttu-id="c4efc-256">Beroende på dina befintliga i/o-krav för den virtuella Hårddisken OS behöver du kan inte migrera det till ett Premium Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c4efc-256">Depending on your existing IO requirements of the OS VHD, you might not need to migrate this to a Premium Storage account.</span></span>
>
>

<span data-ttu-id="c4efc-257">Om din SQL Server används externt ändras cloud service VIP.</span><span class="sxs-lookup"><span data-stu-id="c4efc-257">If your SQL Server is accessed externally, then the cloud service VIP will change.</span></span> <span data-ttu-id="c4efc-258">Du måste även ha uppdateringen slutpunkter, ACL: er och DNS-inställningar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-258">You will also have to update end points, ACLs, and DNS settings.</span></span>

## <a name="existing-deployments-that-use-always-on-availability-groups"></a><span data-ttu-id="c4efc-259">Befintliga distributioner som använder alltid på Tillgänglighetsgrupper</span><span class="sxs-lookup"><span data-stu-id="c4efc-259">Existing deployments that use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="c4efc-260">Befintliga distributioner först finns i [krav](#prerequisites-for-premium-storage) i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-260">For existing deployments, first see the [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="c4efc-261">Först ska vi titta på hur alltid på samverkar med Azure-nätverk i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-261">Initially in this section we will look at how Always On interacts with Azure Networking.</span></span> <span data-ttu-id="c4efc-262">Vi kommer sedan att dela upp migreringar i två scenarier: där vissa avbrott kan tillåtas migreringar och migreringar där du måste uppnå minimal avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="c4efc-262">We will then break down migrations in to two scenarios: migrations where some downtime can be tolerated and migrations where you must achieve minimal downtime.</span></span>

<span data-ttu-id="c4efc-263">Lokal SQL Server alltid på Tillgänglighetsgrupper använder en lyssnare lokalt som registrerar ett virtuella DNS-namn tillsammans med en IP-adress som delas mellan en eller flera SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-263">On-premises SQL Server Always On Availability Groups use a Listener on-premises that registers a virtual DNS name along with an IP address that is shared between one or more SQL Servers.</span></span> <span data-ttu-id="c4efc-264">När klienter ansluter dirigeras de via IP-Adressen för lyssnaren till den primära SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="c4efc-264">When clients connect they are routed through the listener IP to the Primary SQL Server.</span></span> <span data-ttu-id="c4efc-265">Det här är den server som äger den alltid på IP-resursen samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c4efc-265">This is the server that owns the Always On IP resource at that time.</span></span>

![DeploymentsUseAlways på][6]

<span data-ttu-id="c4efc-267">I Microsoft Azure som du kan ha endast en IP-adress som tilldelats ett nätverkskort på den virtuella datorn, så använder för att uppnå samma lager Abstraktionslager som lokalt, Azure IP-adressen som har tilldelats intern/extern belastningsutjämnare (ILB/ELB).</span><span class="sxs-lookup"><span data-stu-id="c4efc-267">In Microsoft Azure you can have only one IP address assigned to a NIC on the VM, so in order to achieve the same layer of abstraction as on-premises, Azure utilizes the IP address that is assigned to the Internal/External Load Balancers (ILB/ELB).</span></span> <span data-ttu-id="c4efc-268">IP-resurs som delas mellan servrarna har angetts till samma IP-Adressen som ILB/ELB.</span><span class="sxs-lookup"><span data-stu-id="c4efc-268">The IP resource that is shared between the servers is set to the same IP as the ILB/ELB.</span></span> <span data-ttu-id="c4efc-269">Detta är publicerad i DNS och klienttrafik överförs via ILB/ELB till den primära SQL Server-repliken.</span><span class="sxs-lookup"><span data-stu-id="c4efc-269">This is published in the DNS, and client traffic is passed through the ILB/ELB to the Primary SQL Server replica.</span></span> <span data-ttu-id="c4efc-270">ILB/ELB vet vilken SQL Server är primär eftersom den använder avsökningar till avsökning alltid på IP-resurs.</span><span class="sxs-lookup"><span data-stu-id="c4efc-270">The ILB/ELB knows which SQL Server is primary since it uses probes to probe the Always On IP resource.</span></span> <span data-ttu-id="c4efc-271">I föregående exempel är den avsökningar varje nod som har en slutpunkt som refereras av ELB/ILB, beroende på vilket som svarar är den primära SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="c4efc-271">In the previous example, it probes each node that has an endpoint referenced by the ELB/ILB, whichever responds is the Primary SQL Server.</span></span>

> [!NOTE]
> <span data-ttu-id="c4efc-272">ILB och ELB tilldelas till en viss Azure-molntjänst, därför alla molnmigrering i Azure troligen innebär att läsa in belastningsutjämning IP ändras.</span><span class="sxs-lookup"><span data-stu-id="c4efc-272">The ILB and ELB are both assigned to a particular Azure cloud service, therefore any cloud migration in Azure will most likely mean that the Load Balancer IP will change.</span></span>
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a><span data-ttu-id="c4efc-273">Migrera alltid på distributioner som kan tillåta att vissa avbrott</span><span class="sxs-lookup"><span data-stu-id="c4efc-273">Migrating Always On deployments that can allow some downtime</span></span>
<span data-ttu-id="c4efc-274">Det finns två olika metoder för att migrera alltid distributioner så att vissa avbrott:</span><span class="sxs-lookup"><span data-stu-id="c4efc-274">There are two strategies to migrate Always On deployments that allow for some downtime:</span></span>

1. <span data-ttu-id="c4efc-275">**Lägga till flera sekundära repliker i ett befintligt alltid på kluster**</span><span class="sxs-lookup"><span data-stu-id="c4efc-275">**Add more secondary replicas to an existing Always On Cluster**</span></span>
2. <span data-ttu-id="c4efc-276">**Migrera till ett nytt alltid på kluster**</span><span class="sxs-lookup"><span data-stu-id="c4efc-276">**Migrate to a new Always On Cluster**</span></span>

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a><span data-ttu-id="c4efc-277">1. Lägga till flera sekundära repliker i ett befintligt alltid på kluster</span><span class="sxs-lookup"><span data-stu-id="c4efc-277">1. Add more Secondary Replicas to an Existing Always On Cluster</span></span>
<span data-ttu-id="c4efc-278">En strategi är att lägga till flera sekundärservrar alltid på Tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-278">One strategy is to add more secondaries to the Always On Availability Group.</span></span> <span data-ttu-id="c4efc-279">Du måste lägga till dem i en ny molntjänst och uppdatera lyssnaren med den nya IP-belastning belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="c4efc-279">You need to add these into a new cloud service and update the listener with the new load balancer IP.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="c4efc-280">Punkter stillestånd:</span><span class="sxs-lookup"><span data-stu-id="c4efc-280">Points of downtime:</span></span>
* <span data-ttu-id="c4efc-281">Klusterverifieringen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-281">Cluster Validation.</span></span>
* <span data-ttu-id="c4efc-282">Testa alltid på redundans för nya sekundärservrar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-282">Testing Always On failovers for New Secondaries.</span></span>

<span data-ttu-id="c4efc-283">Om du använder Windows lagringspooler inifrån den virtuella datorn ha högre i/o-genomflöde kommer sedan dessa att kopplas från under en fullständig verifiering av klustret.</span><span class="sxs-lookup"><span data-stu-id="c4efc-283">If you are using Windows Storage Pools within the VM for higher IO throughput, then these will be taken offline during a Full Cluster Validation.</span></span> <span data-ttu-id="c4efc-284">Verifieringstest krävs när du lägger till noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="c4efc-284">The validation test is required when you add nodes to the cluster.</span></span> <span data-ttu-id="c4efc-285">Den tid det tar för att köra testet kan variera, så bör du testa i din testmiljö för att få en ungefärlig tid för hur lång tid detta tar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-285">The time it takes to run the test can vary, so you should test this in your representative test environment to get an approximate time of how long this will take.</span></span>

<span data-ttu-id="c4efc-286">Du måste etablera tid där du kan utföra manuell redundans och chaos testning på de nytillagda noderna för att säkerställa alltid på hög tillgänglighet fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="c4efc-286">You should provision time where you can perform manual failover and chaos testing on the newly added nodes to ensure Always On High Availability functions as expected.</span></span>

![DeploymentUseAlways On2][7]

> [!NOTE]
> <span data-ttu-id="c4efc-288">Du bör avsluta alla instanser av SQL Server där lagringspoolen används innan verifieringen körs.</span><span class="sxs-lookup"><span data-stu-id="c4efc-288">You should stop all instances of SQL Server where the Storage Pools are used before the Validation runs.</span></span>
>
> ##### <a name="high-level-steps"></a><span data-ttu-id="c4efc-289">Anvisningar</span><span class="sxs-lookup"><span data-stu-id="c4efc-289">High-level steps</span></span>
>

1. <span data-ttu-id="c4efc-290">Skapa två nya SQL-servrar i ny molntjänst med anslutna Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c4efc-290">Create two new SQL Servers in new cloud service with attached Premium Storage.</span></span>
2. <span data-ttu-id="c4efc-291">Kopiera över fullständig säkerhetskopiering och återställning med **NORECOVERY**.</span><span class="sxs-lookup"><span data-stu-id="c4efc-291">Copy over FULL backups and restore with **NORECOVERY**.</span></span>
3. <span data-ttu-id="c4efc-292">Kopiera över 'utanför user DB' beroende objekt, till exempel inloggningar osv.</span><span class="sxs-lookup"><span data-stu-id="c4efc-292">Copy over ‘out of user DB’ dependent objects, such as logins etc.</span></span>
4. <span data-ttu-id="c4efc-293">Skapa en ny intern belastning belastningsutjämnare (ILB) eller använda en extern belastningen belastningsutjämnare (ELB) och sedan ställa in belastningen belastningsutjämnade slutpunkter på både nya noder.</span><span class="sxs-lookup"><span data-stu-id="c4efc-293">Create new a new Internal Load Balancer (ILB) or use an External Load Balancer (ELB), and then set up Load Balanced Endpoints on both new nodes.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c4efc-294">Kontrollera att alla noder har rätt slutpunktskonfigurationen innan du fortsätter</span><span class="sxs-lookup"><span data-stu-id="c4efc-294">Check all Nodes have the correct Endpoint configuration before you continue</span></span>
   >
   >
5. <span data-ttu-id="c4efc-295">Stoppa användare/Application åtkomst till SQL Server (om du använder lagringspooler).</span><span class="sxs-lookup"><span data-stu-id="c4efc-295">Stop User/Application Access to the SQL Server (if using Storage Pools).</span></span>
6. <span data-ttu-id="c4efc-296">Stoppa SQL Server Engine-tjänster på alla noder (om du använder lagringspooler).</span><span class="sxs-lookup"><span data-stu-id="c4efc-296">Stop SQL Server Engine Services on All Nodes (if using Storage Pools).</span></span>
7. <span data-ttu-id="c4efc-297">Lägga till nya noder för att klustret och köra fullständig verifiering.</span><span class="sxs-lookup"><span data-stu-id="c4efc-297">Add new Nodes to cluster and run full validation.</span></span>
8. <span data-ttu-id="c4efc-298">När verifieringen är klar startar du alla SQL Server-tjänster.</span><span class="sxs-lookup"><span data-stu-id="c4efc-298">Once Validation is successful, start all SQL Server Services.</span></span>
9. <span data-ttu-id="c4efc-299">Säkerhetskopiera transaktionsloggar och återställa databaserna.</span><span class="sxs-lookup"><span data-stu-id="c4efc-299">Backup Transaction logs, and restore user databases.</span></span>
10. <span data-ttu-id="c4efc-300">Lägg till nya noder i alltid på Tillgänglighetsgruppen och placera replikering till **synkron**.</span><span class="sxs-lookup"><span data-stu-id="c4efc-300">Add new nodes into the Always On Availability Group and place replication into **Synchronous**.</span></span>
11. <span data-ttu-id="c4efc-301">Lägg till IP-adressresurs för den nya Cloud Service ILB/ELB via PowerShell för Always On-baserad på flera platser exemplet i den [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="c4efc-301">Add the IP address resource of the new Cloud Service ILB/ELB through PowerShell for Always On based on the Multi-site example in the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span> <span data-ttu-id="c4efc-302">I Windows-kluster, ange den **möjliga ägare** av den **IP-adress** resurs till nya noder gamla.</span><span class="sxs-lookup"><span data-stu-id="c4efc-302">In Windows clustering, set the **Possible owners** of the **IP Address** resource to the new nodes old.</span></span> <span data-ttu-id="c4efc-303">Se avsnittet ”lägga till IP-adressresurs i samma undernät, i den [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="c4efc-303">See the ‘Adding IP Address Resource on Same Subnet’ section of the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
12. <span data-ttu-id="c4efc-304">Redundans till en ny nod.</span><span class="sxs-lookup"><span data-stu-id="c4efc-304">Failover to one of the new nodes.</span></span>
13. <span data-ttu-id="c4efc-305">Se nya noder automatiskt Redundanspartners och testa redundans.</span><span class="sxs-lookup"><span data-stu-id="c4efc-305">Make the new nodes Auto Failover Partners and test failovers.</span></span>
14. <span data-ttu-id="c4efc-306">Ta bort ursprungliga noder från Tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-306">Remove original nodes from Availability Group.</span></span>

##### <a name="advantages"></a><span data-ttu-id="c4efc-307">Fördelar</span><span class="sxs-lookup"><span data-stu-id="c4efc-307">Advantages</span></span>
* <span data-ttu-id="c4efc-308">Ny SQL-servrar kan vara testas (SQL Server och program) innan de läggs till Always On.</span><span class="sxs-lookup"><span data-stu-id="c4efc-308">New SQL Servers can be tested (SQL Server and Application) before they are added to Always On.</span></span>
* <span data-ttu-id="c4efc-309">Du kan ändra storleken på virtuella datorn och anpassa lagringsplatsen till de exakta kraven.</span><span class="sxs-lookup"><span data-stu-id="c4efc-309">You can change the VM size and customize the storage to your exact requirements.</span></span> <span data-ttu-id="c4efc-310">Det skulle dock vara bra att bevara alla sökvägar för SQL-filen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-310">However, it would be beneficial to keep all the SQL file paths the same.</span></span>
* <span data-ttu-id="c4efc-311">Du kan styra när överföringen av DB-säkerhetskopiering till de sekundära replikerna har startats.</span><span class="sxs-lookup"><span data-stu-id="c4efc-311">You can control when the transfer of the DB backups to the Secondary Replicas are started.</span></span> <span data-ttu-id="c4efc-312">Detta skiljer sig från att använda Azure **Start AzureStorageBlobCopy** cmdleten igen för att kopiera virtuella hårddiskar, eftersom det är en asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="c4efc-312">This differs from using Azure **Start-AzureStorageBlobCopy** commandlet to copy VHDs, because that is an asynchronous copy.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="c4efc-313">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="c4efc-313">Disadvantages</span></span>
* <span data-ttu-id="c4efc-314">När du använder Windows-lagringspooler finns klusterdriftstopp under fullständig Klustervalideringen för nya ytterligare noder.</span><span class="sxs-lookup"><span data-stu-id="c4efc-314">When using Windows Storage Pools, there is Cluster downtime during the Full Cluster Validation for the new additional nodes.</span></span>
* <span data-ttu-id="c4efc-315">Beroende på SQL Server-Version och befintliga antalet sekundära repliker kanske du inte kan lägga till flera sekundära repliker utan att ta bort befintliga sekundärservrar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-315">Depending on the SQL Server Version and the existing number of secondary replicas, you might not be able to add more secondary replicas without removing existing secondaries.</span></span>
* <span data-ttu-id="c4efc-316">Det kan finnas långa SQL dataöverföringstid när du konfigurerar sekundära.</span><span class="sxs-lookup"><span data-stu-id="c4efc-316">There could be long SQL data transfer time while setting up the secondaries.</span></span>
* <span data-ttu-id="c4efc-317">Det finns ytterligare kostnad under migreringen när du har nya datorer som körs parallellt.</span><span class="sxs-lookup"><span data-stu-id="c4efc-317">There is additional cost during migration while you have new machines running in parallel.</span></span>

#### <a name="2-migrate-to-a-new-always-on-cluster"></a><span data-ttu-id="c4efc-318">2. Migrera till ett nytt alltid på kluster</span><span class="sxs-lookup"><span data-stu-id="c4efc-318">2. Migrate to a new Always On Cluster</span></span>
<span data-ttu-id="c4efc-319">En annan strategi är att skapa en helt ny alltid på klustret med helt nya noder i ny molntjänst och sedan dirigera klienterna för att använda den.</span><span class="sxs-lookup"><span data-stu-id="c4efc-319">Another strategy is to create a brand new Always On Cluster with brand new nodes in new cloud service and then redirect the clients to use it.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="c4efc-320">Punkter stillestånd</span><span class="sxs-lookup"><span data-stu-id="c4efc-320">Points of downtime</span></span>
<span data-ttu-id="c4efc-321">Det finns en avbrottstid när du överför program och användare till alltid på lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="c4efc-321">There is downtime when you transfer applications and users to the new Always On listener.</span></span> <span data-ttu-id="c4efc-322">Avbrottstiden beror på:</span><span class="sxs-lookup"><span data-stu-id="c4efc-322">The downtime depends on:</span></span>

* <span data-ttu-id="c4efc-323">Den tid det tar att återställa säkerhetskopior av slutliga transaktionsloggen för databaser på nya servrar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-323">The time taken to restore final transaction log backups to databases on new servers.</span></span>
* <span data-ttu-id="c4efc-324">Den tid det tar att uppdatera klientprogram att använda alltid på lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="c4efc-324">The time taken to update client applications to use new Always On listener.</span></span>

##### <a name="advantages"></a><span data-ttu-id="c4efc-325">Fördelar</span><span class="sxs-lookup"><span data-stu-id="c4efc-325">Advantages</span></span>
* <span data-ttu-id="c4efc-326">Du kan testa faktiska produktionsmiljön, SQL Server och Operativsystemets build-ändringar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-326">You can test the actual production environment, SQL Server, and OS build changes.</span></span>
* <span data-ttu-id="c4efc-327">Har du möjlighet att anpassa lagring och minska storleken för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c4efc-327">You have the option to customize the storage and to potentially reduce size of VM.</span></span> <span data-ttu-id="c4efc-328">Det kan leda till minskad kostnaden.</span><span class="sxs-lookup"><span data-stu-id="c4efc-328">This could result in cost reduction.</span></span>
* <span data-ttu-id="c4efc-329">Under den här processen kan du uppdatera din version eller SQL Server-version.</span><span class="sxs-lookup"><span data-stu-id="c4efc-329">You can update your SQL Server build or version during this process.</span></span> <span data-ttu-id="c4efc-330">Du kan också uppgradera operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-330">You can also upgrade the Operating System.</span></span>
* <span data-ttu-id="c4efc-331">Tidigare alltid på klustret kan fungera som ett fast rollback-mål.</span><span class="sxs-lookup"><span data-stu-id="c4efc-331">The previous Always On Cluster can act as a solid rollback target.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="c4efc-332">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="c4efc-332">Disadvantages</span></span>
* <span data-ttu-id="c4efc-333">Du behöver ändra lyssnaren DNS-namn om du vill att båda Always On-kluster körs samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c4efc-333">You need to change the DNS name of the listener if you want both Always On clusters running simultaneously.</span></span> <span data-ttu-id="c4efc-334">Detta lägger till administration försämras under migreringen som klienten programmet strängar måste namnet ny lyssnare.</span><span class="sxs-lookup"><span data-stu-id="c4efc-334">This adds administration overhead during the migration as client application strings must reflect the new Listener name.</span></span>
* <span data-ttu-id="c4efc-335">Du måste implementera en synkroniseringsmekanism för mellan två miljöer att hålla dem så nära som möjligt för att minimera slutlig synkronisering kraven innan migreringen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-335">You must implement a synchronization mechanism between the two environments to keep them as close as possible to minimize the final synchronization requirements before migration.</span></span>
* <span data-ttu-id="c4efc-336">Det läggs till kostnad under migreringen när du har den nya miljön som körs.</span><span class="sxs-lookup"><span data-stu-id="c4efc-336">There is added cost during migration while you have the new environment running.</span></span>

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a><span data-ttu-id="c4efc-337">Migrera alltid på distributioner för minimal driftstörning</span><span class="sxs-lookup"><span data-stu-id="c4efc-337">Migrating Always On Deployments for minimal downtime</span></span>
<span data-ttu-id="c4efc-338">Det finns två metoder för att migrera Always On-distributioner för minimal driftstörning:</span><span class="sxs-lookup"><span data-stu-id="c4efc-338">There are two strategies for migrating Always On deployments for minimal downtime:</span></span>

1. <span data-ttu-id="c4efc-339">**Använda en befintlig sekundär: enskild plats**</span><span class="sxs-lookup"><span data-stu-id="c4efc-339">**Utilize an Existing Secondary: Single-Site**</span></span>
2. <span data-ttu-id="c4efc-340">**Använda befintlig sekundär tillgänglighetsreplik(er): flera platser**</span><span class="sxs-lookup"><span data-stu-id="c4efc-340">**Utilize Existing Secondary Replica(s): Multi-Site**</span></span>

#### <a name="1-utilize-an-existing-secondary-single-site"></a><span data-ttu-id="c4efc-341">1. Använda en befintlig sekundär: enskild plats</span><span class="sxs-lookup"><span data-stu-id="c4efc-341">1. Utilize an existing secondary: Single-Site</span></span>
<span data-ttu-id="c4efc-342">En strategi för minimal driftstörning är att ta ett befintligt moln sekundära och ta bort den från den aktuella Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="c4efc-342">One strategy for minimal downtime is to take an existing cloud secondary and remove it from the current cloud service.</span></span> <span data-ttu-id="c4efc-343">Kopiera de virtuella hårddiskarna till det nya kontot i Premium-lagring, och skapa den virtuella datorn i en ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c4efc-343">Then copy the VHDs to the new Premium Storage account, and create the VM in the new cloud service.</span></span> <span data-ttu-id="c4efc-344">Uppdatera lyssnare i kluster och växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c4efc-344">Then update the listener in clustering and failover.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="c4efc-345">Punkter stillestånd</span><span class="sxs-lookup"><span data-stu-id="c4efc-345">Points of downtime</span></span>
* <span data-ttu-id="c4efc-346">Det finns avbrottstid när du uppdaterar den sista noden med belastningsutjämnade-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="c4efc-346">There is downtime when you update the final node with the Load Balanced endpoint.</span></span>
* <span data-ttu-id="c4efc-347">Klient-återanslutning kan fördröjas beroende på din klient/DNS-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c4efc-347">Your client reconnection might be delayed depending on your client/DNS configuration.</span></span>
* <span data-ttu-id="c4efc-348">Det finns ytterligare driftstopp om du väljer att ta alltid på klustergruppen offline för att byta ut de IP-adresserna.</span><span class="sxs-lookup"><span data-stu-id="c4efc-348">There is additional downtime if you choose to take the Always On Cluster group offline to swap out the IP addresses.</span></span> <span data-ttu-id="c4efc-349">Du kan undvika detta genom att använda ett beroende eller och möjliga ägare för den tillagda IP-adressresursen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-349">You can avoid this by using an OR dependency and Possible Owners for the added IP Address resource.</span></span> <span data-ttu-id="c4efc-350">Se avsnittet ”lägga till IP-adressresurs i samma undernät, i den [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="c4efc-350">See the ‘Adding IP Address Resource on Same Subnet’ section of the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>

> [!NOTE]
> <span data-ttu-id="c4efc-351">Om du vill att den tillagda noden till partake i som alltid på Failover-Partner som du behöver lägga till en Azure-slutpunkt med en referens till den belastningen den belastningsutjämnade uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-351">When you want the added node to partake in as an Always On Failover Partner, you need to add an Azure Endpoint with a reference to the Load Balanced Set.</span></span> <span data-ttu-id="c4efc-352">När du kör den **Lägg till AzureEndpoint** kommandot för att göra detta, aktuella anslutningen förbli öppen, men nya anslutningar till lyssnaren kommer inte att kunna upprättas förrän belastningsutjämnaren har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="c4efc-352">When you run the **Add-AzureEndpoint** command to do this, current connections to remain open, but new connections to the listener will not be able to be established until the load balancer has updated.</span></span> <span data-ttu-id="c4efc-353">Testning detta påträffades till senaste 90-120seconds detta bör testas.</span><span class="sxs-lookup"><span data-stu-id="c4efc-353">In testing this was seen to last 90-120seconds, this should be tested.</span></span>
>
>

##### <a name="advantages"></a><span data-ttu-id="c4efc-354">Fördelar</span><span class="sxs-lookup"><span data-stu-id="c4efc-354">Advantages</span></span>
* <span data-ttu-id="c4efc-355">Inga extra kostnaden under migreringen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-355">No extra cost incurred during migration.</span></span>
* <span data-ttu-id="c4efc-356">En-till-en migrering.</span><span class="sxs-lookup"><span data-stu-id="c4efc-356">A one-to-one migration.</span></span>
* <span data-ttu-id="c4efc-357">Minskad komplexitet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-357">Reduced complexity.</span></span>
* <span data-ttu-id="c4efc-358">Ger ökad IOPS från Premium Storage SKU: er.</span><span class="sxs-lookup"><span data-stu-id="c4efc-358">Allows for increased IOPS from Premium Storage SKUs.</span></span> <span data-ttu-id="c4efc-359">När diskarna är frånkopplat från den virtuella datorn och kopieras till den nya Molntjänsten en 3 part kan du använda verktyget för att öka storleken på VHD som ger högre genomflöden.</span><span class="sxs-lookup"><span data-stu-id="c4efc-359">When the disks are detached from the VM and copied to the new cloud service, a 3rd party tool can be used to increase the VHD size, which provides higher throughputs.</span></span> <span data-ttu-id="c4efc-360">Öka storleken på VHD finns i följande [forumdiskussion](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span><span class="sxs-lookup"><span data-stu-id="c4efc-360">For increasing VHD sizes, see this [forum discussion](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="c4efc-361">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="c4efc-361">Disadvantages</span></span>
* <span data-ttu-id="c4efc-362">Det finns en temporär förlust av hög tillgänglighet och Katastrofåterställning under migreringen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-362">There is a temporary loss of HA and DR during migration.</span></span>
* <span data-ttu-id="c4efc-363">Eftersom det är en 1:1-migrering, måste du använda en minsta VM-storlek som stöder antal virtuella hårddiskar, så du kan inte downsize dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c4efc-363">As this is a 1:1 migration, you will have to use a minimum VM size that will support your number of VHDs, so you might not be able to downsize your VMs.</span></span>
* <span data-ttu-id="c4efc-364">Det här scenariot använder Azure **Start AzureStorageBlobCopy** cmdlet, som är asynkron.</span><span class="sxs-lookup"><span data-stu-id="c4efc-364">This scenario would use the Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="c4efc-365">Det finns inga SLA på Kopiera slutförande.</span><span class="sxs-lookup"><span data-stu-id="c4efc-365">There is no SLA on copy completion.</span></span> <span data-ttu-id="c4efc-366">Tiden för kopiorna varierar, medan det beror på vänta i kön beror också på hur mycket data att överföra.</span><span class="sxs-lookup"><span data-stu-id="c4efc-366">The time of the copies varies, while this depends on wait in queue it will also depend on the amount of data to transfer.</span></span> <span data-ttu-id="c4efc-367">Kopiera tid ökar om överföringen kommer att en annan Azure-datacenter som har stöd för Premium-lagring i en annan region.</span><span class="sxs-lookup"><span data-stu-id="c4efc-367">The copy time increases if the transfer is going to another Azure data center that supports Premium Storage in another region.</span></span> <span data-ttu-id="c4efc-368">Om du har precis 2 noder kan du en möjlig lösning om kopian tar längre tid än vid testning.</span><span class="sxs-lookup"><span data-stu-id="c4efc-368">If you just have 2 nodes, consider a possible mitigation in case the copy takes longer than in testing.</span></span> <span data-ttu-id="c4efc-369">Detta kan inkludera följande idéer.</span><span class="sxs-lookup"><span data-stu-id="c4efc-369">This could include the following ideas.</span></span>
  * <span data-ttu-id="c4efc-370">Lägg till en tillfällig 3 SQL Server-nod för hög tillgänglighet innan migreringen överenskomna avbrott.</span><span class="sxs-lookup"><span data-stu-id="c4efc-370">Add a temporary 3rd SQL Server node for HA before the migration with agreed downtime.</span></span>
  * <span data-ttu-id="c4efc-371">Kör migrering utanför Azure schemalagt underhåll.</span><span class="sxs-lookup"><span data-stu-id="c4efc-371">Run the migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="c4efc-372">Kontrollera att du har konfigurerat klustrets kvorum korrekt.</span><span class="sxs-lookup"><span data-stu-id="c4efc-372">Ensure you have configured your cluster quorum correctly.</span></span>  

##### <a name="high-level-steps"></a><span data-ttu-id="c4efc-373">Anvisningar</span><span class="sxs-lookup"><span data-stu-id="c4efc-373">High-level steps</span></span>
<span data-ttu-id="c4efc-374">Det här dokumentet visar inte ett komplett exempel slutpunkt till slutpunkt, men den [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) innehåller information som kan utnyttjas för att utföra detta.</span><span class="sxs-lookup"><span data-stu-id="c4efc-374">This document does not demonstrate a complete end to end example, however the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) provides details that can be leveraged to perform this.</span></span>

![MinimalDowntime][8]

* <span data-ttu-id="c4efc-376">Samla in diskkonfigurationen och ta bort noden (ta inte bort anslutna virtuella hårddiskar).</span><span class="sxs-lookup"><span data-stu-id="c4efc-376">Gather disk configuration, and remove the node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="c4efc-377">Skapa ett premiumlagringskonto-lagring och kopiera virtuella hårddiskar från standardlagringskontot</span><span class="sxs-lookup"><span data-stu-id="c4efc-377">Create a Premium Storage account and copy VHDs from the Standard Storage account</span></span>
* <span data-ttu-id="c4efc-378">Skapa ny molntjänst och distribuera SQL2 VM i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="c4efc-378">Create new cloud service and redeploy the SQL2 VM in that cloud service.</span></span> <span data-ttu-id="c4efc-379">Skapa den virtuella datorn med den kopierade ursprungliga OS virtuell Hårddisk och bifoga de kopierade virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="c4efc-379">Create the VM using the copied original OS VHD and attaching the copied VHDs.</span></span>
* <span data-ttu-id="c4efc-380">Konfigurera ILB / ELB och lägga till slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="c4efc-380">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="c4efc-381">Uppdatera lyssnare genom att antingen:</span><span class="sxs-lookup"><span data-stu-id="c4efc-381">Update Listener by either:</span></span>
  * <span data-ttu-id="c4efc-382">Koppla alltid på gruppen och uppdaterar alltid på lyssnaren med nya ILB / ELB IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c4efc-382">Taking the Always On Group offline and updating the Always On Listener with new ILB / ELB IP address.</span></span>
  * <span data-ttu-id="c4efc-383">Eller lägga till IP-adressresurs av nya Cloud Service ILB/ELB via PowerShell i Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="c4efc-383">Or adding the IP address resource of new Cloud Service ILB/ELB through PowerShell into Windows clustering.</span></span> <span data-ttu-id="c4efc-384">Sedan ange IP-adressresurs möjliga ägare till noden migrerade SQL2, och ange detta eller beroende i nätverksnamn.</span><span class="sxs-lookup"><span data-stu-id="c4efc-384">Then set the Possible owners of the IP Address resource to the migrated node, SQL2, and set this as OR dependency in the Network Name.</span></span> <span data-ttu-id="c4efc-385">Se avsnittet ”lägga till IP-adressresurs i samma undernät, i den [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="c4efc-385">See the ‘Adding IP Address Resource on Same Subnet’ section of the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
* <span data-ttu-id="c4efc-386">Kontrollera DNS-konfiguration/spridningsuppgift till klienterna.</span><span class="sxs-lookup"><span data-stu-id="c4efc-386">Check DNS configuration/propogation to the clients.</span></span>
* <span data-ttu-id="c4efc-387">Migrera SQL1 VM och gå igenom steg 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="c4efc-387">Migrate SQL1 VM, and go through steps 2 – 4.</span></span>
* <span data-ttu-id="c4efc-388">Om du använder steg 5ii, Lägg sedan till SQL1 som möjlig ägare för den tillagda IP-adressresurs</span><span class="sxs-lookup"><span data-stu-id="c4efc-388">If using steps 5ii, then add SQL1 as a Possible Owner for the added IP Address Resource</span></span>
* <span data-ttu-id="c4efc-389">Redundanstestningen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-389">Test failovers.</span></span>

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a><span data-ttu-id="c4efc-390">2. Använda befintlig sekundär tillgänglighetsreplik(er): flera platser</span><span class="sxs-lookup"><span data-stu-id="c4efc-390">2. Utilize existing secondary replica(s): Multi-Site</span></span>
<span data-ttu-id="c4efc-391">Om du har noder i mer än en Azure-datacenter (DC) eller om du har en hybridmiljö kan du använda en Always On-konfiguration i den här miljön för att minimera driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="c4efc-391">If you have nodes in more than one Azure datacenter (DC) or if you have a hybrid environment, then you can use an Always On configuration in this environment to minimize downtime.</span></span>

<span data-ttu-id="c4efc-392">Tillvägagångssättet är att ändra Always On-synkronisering till synkron för lokal eller sekundära Azure Domänkontrollant och sedan redundans över till den SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c4efc-392">The approach is to change the Always On synchronization to Synchronous for the on-premises or secondary Azure DC, and then failover over to that SQL Server.</span></span> <span data-ttu-id="c4efc-393">Kopiera de virtuella hårddiskarna till en Premium Storage-konto, och distribuera datorn till en ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c4efc-393">Then copy the VHDs to a Premium Storage account, and redeploy the machine into a new cloud service.</span></span> <span data-ttu-id="c4efc-394">Uppdatera lyssnaren och växlar sedan tillbaka.</span><span class="sxs-lookup"><span data-stu-id="c4efc-394">Update the listener, and then fail back.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="c4efc-395">Punkter stillestånd</span><span class="sxs-lookup"><span data-stu-id="c4efc-395">Points of downtime</span></span>
<span data-ttu-id="c4efc-396">Avbrottstiden består av tiden till redundans till alternativa DC och tillbaka.</span><span class="sxs-lookup"><span data-stu-id="c4efc-396">The downtime consists of the time to failover to the alternative DC and back.</span></span> <span data-ttu-id="c4efc-397">Det beror också på klienten eller DNS-konfigurationen och klient-återanslutning kan vara fördröjd.</span><span class="sxs-lookup"><span data-stu-id="c4efc-397">It also depends on your client/DNS configuration and your client reconnection may be delayed.</span></span>
<span data-ttu-id="c4efc-398">Överväg följande exempel på en hybrid Always On-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="c4efc-398">Consider the following example of a hybrid Always On configuration:</span></span>

![MultiSite1][9]

##### <a name="advantages"></a><span data-ttu-id="c4efc-400">Fördelar</span><span class="sxs-lookup"><span data-stu-id="c4efc-400">Advantages</span></span>
* <span data-ttu-id="c4efc-401">Du kan använda befintliga infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="c4efc-401">You can utilize existing infrastructure.</span></span>
* <span data-ttu-id="c4efc-402">Du har möjlighet att före uppgradering Azure storage på domänkontrollanten DR Azure först.</span><span class="sxs-lookup"><span data-stu-id="c4efc-402">You have the option to pre-upgrade the Azure storage on the DR Azure DC first.</span></span>
* <span data-ttu-id="c4efc-403">DR Azure DC-lagring kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="c4efc-403">The DR Azure DC storage can be reconfigured.</span></span>
* <span data-ttu-id="c4efc-404">Det finns minst två växling vid fel under migreringen, exklusive redundanstestning.</span><span class="sxs-lookup"><span data-stu-id="c4efc-404">There is a minimum of two failovers during migration, excluding test failovers.</span></span>
* <span data-ttu-id="c4efc-405">Du behöver inte flytta SQL Server-data med säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="c4efc-405">You do not need to move SQL Server data with backup and restore.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="c4efc-406">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="c4efc-406">Disadvantages</span></span>
* <span data-ttu-id="c4efc-407">Beroende på klientåtkomst till SQL Server, kan det finnas ökad latens när SQL Server körs i en annan Domänkontrollant till programmet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-407">Depending on client access to SQL Server, there might be increased latency when SQL Server is running in an alternative DC to the application.</span></span>
* <span data-ttu-id="c4efc-408">Kopiera tidpunkten för virtuella hårddiskar till Premium-lagring kan vara lång.</span><span class="sxs-lookup"><span data-stu-id="c4efc-408">The copy time of VHDs to Premium storage could be long.</span></span> <span data-ttu-id="c4efc-409">Detta kan påverka ditt beslut om om du vill behålla noden i Tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-409">This might affect your decision on whether to keep the node in the Availability Group.</span></span> <span data-ttu-id="c4efc-410">Tänk på detta för när loggen arbetar belastningar som körs under migreringen krävs, eftersom den primära noden måste ha unreplicated transaktioner i dess transaktionsloggen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-410">Consider this for when log intensive work loads are running during the migration is required, since the Primary node will have to keep the unreplicated transactions in its transaction log.</span></span> <span data-ttu-id="c4efc-411">Därför kan detta växa avsevärt.</span><span class="sxs-lookup"><span data-stu-id="c4efc-411">Therefore this could grow significantly.</span></span>
* <span data-ttu-id="c4efc-412">Det här scenariot använder Azure **Start AzureStorageBlobCopy** cmdlet, som är asynkron.</span><span class="sxs-lookup"><span data-stu-id="c4efc-412">This scenario would use the Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="c4efc-413">Det finns inga SLA på slutförande.</span><span class="sxs-lookup"><span data-stu-id="c4efc-413">There is no SLA on completion.</span></span> <span data-ttu-id="c4efc-414">Tiden kopior kan variera medan detta beror på vänta i kön, det beror på hur mycket data att överföra.</span><span class="sxs-lookup"><span data-stu-id="c4efc-414">The time of the copies varies, while this depends on wait in queue, it will also depend on the amount of data to transfer.</span></span> <span data-ttu-id="c4efc-415">Därför ha bara en nod i datacentret 2, du bör vidta säkerhetsåtgärder om kopian tar längre tid än vid testning.</span><span class="sxs-lookup"><span data-stu-id="c4efc-415">Therefore you just have one node in your 2nd data center, you should take mitigation steps in case the copy takes longer than in testing.</span></span> <span data-ttu-id="c4efc-416">Detta kan inkludera följande idéer.</span><span class="sxs-lookup"><span data-stu-id="c4efc-416">This could include the following ideas.</span></span>
  * <span data-ttu-id="c4efc-417">Lägg till en tillfällig SQL-nod 2 för hög tillgänglighet innan migreringen överenskomna avbrott.</span><span class="sxs-lookup"><span data-stu-id="c4efc-417">Add a temporary 2nd SQL node for HA before the migration with agreed downtime.</span></span>
  * <span data-ttu-id="c4efc-418">Kör migrering utanför Azure schemalagt underhåll.</span><span class="sxs-lookup"><span data-stu-id="c4efc-418">Run the migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="c4efc-419">Kontrollera att du har konfigurerat klustrets kvorum korrekt.</span><span class="sxs-lookup"><span data-stu-id="c4efc-419">Ensure you have configured your cluster quorum correctly.</span></span>

<span data-ttu-id="c4efc-420">Det här scenariot förutsätter att du har dokumenterats din installation och vet hur lagring mappas för att göra ändringar för den diskens cacheinställningarna.</span><span class="sxs-lookup"><span data-stu-id="c4efc-420">This scenario assumes that you have documented your install and know how the storage is mapped in order to make changes for optimal disk cache settings.</span></span>

##### <a name="high-level-steps"></a><span data-ttu-id="c4efc-421">Anvisningar</span><span class="sxs-lookup"><span data-stu-id="c4efc-421">High-level steps</span></span>
![Multisite2][10]

* <span data-ttu-id="c4efc-423">Se lokalt / Azure DC för SQL Server-primära alternativ, och det andra automatisk redundans Partner (AFP).</span><span class="sxs-lookup"><span data-stu-id="c4efc-423">Make the on-premises / alternate Azure DC the SQL Server Primary, and make it the other Auto Failover Partner (AFP).</span></span>
* <span data-ttu-id="c4efc-424">Samla in information om diskkonfiguration från SQL2 och ta bort noden (ta inte bort anslutna virtuella hårddiskar).</span><span class="sxs-lookup"><span data-stu-id="c4efc-424">Gather disk configuration information from SQL2, and remove the node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="c4efc-425">Skapa ett premiumlagringskonto-lagring och kopiera virtuella hårddiskar från standardlagringskontot.</span><span class="sxs-lookup"><span data-stu-id="c4efc-425">Create a Premium Storage account and copy VHDs from the Standard Storage account.</span></span>
* <span data-ttu-id="c4efc-426">Skapa en ny molntjänst och skapa den virtuella datorn SQL2 med dess lagring Premier-diskar som är anslutna.</span><span class="sxs-lookup"><span data-stu-id="c4efc-426">Create a new cloud service and create the SQL2 VM with its Premiums Storage disks attached.</span></span>
* <span data-ttu-id="c4efc-427">Konfigurera ILB / ELB och lägga till slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="c4efc-427">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="c4efc-428">Uppdatera alltid på lyssnaren med nya ILB / ELB IP-adress och testa redundans.</span><span class="sxs-lookup"><span data-stu-id="c4efc-428">Update the Always On Listener with new ILB / ELB IP address and test failover.</span></span>
* <span data-ttu-id="c4efc-429">Kontrollera DNS-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-429">Check the DNS configuration.</span></span>
* <span data-ttu-id="c4efc-430">Ändra AFP till SQL2, och migrera SQL1 och gå igenom steg 2 – 5.</span><span class="sxs-lookup"><span data-stu-id="c4efc-430">Change the AFP to SQL2, and then migrate SQL1 and go through steps 2 – 5.</span></span>
* <span data-ttu-id="c4efc-431">Redundanstestningen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-431">Test failovers.</span></span>
* <span data-ttu-id="c4efc-432">Växla AFP tillbaka till SQL1 och SQL2</span><span class="sxs-lookup"><span data-stu-id="c4efc-432">Switch the AFP back to SQL1 and SQL2</span></span>

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a><span data-ttu-id="c4efc-433">Bilaga: Migrera en Multisite alltid på klustret till Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="c4efc-433">Appendix: Migrating a Multisite Always On Cluster to Premium Storage</span></span>
<span data-ttu-id="c4efc-434">Resten av det här avsnittet innehåller ett detaljerat exempel konvertera en Always On kluster på flera platser på Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c4efc-434">The remainder of this topic provides a detailed example of converting a multi-site Always On cluster to Premium storage.</span></span> <span data-ttu-id="c4efc-435">Den konverterar också lyssnaren från att använda en extern belastningsutjämnare (ELB) till en intern belastningsutjämnare (ILB).</span><span class="sxs-lookup"><span data-stu-id="c4efc-435">It also converts the Listener from using an external load balancer (ELB) to an internal load balancer (ILB).</span></span>

### <a name="environment"></a><span data-ttu-id="c4efc-436">Miljö</span><span class="sxs-lookup"><span data-stu-id="c4efc-436">Environment</span></span>
* <span data-ttu-id="c4efc-437">Windows 2k 12 / SQL 2k 12</span><span class="sxs-lookup"><span data-stu-id="c4efc-437">Windows 2k12 / SQL 2k12</span></span>
* <span data-ttu-id="c4efc-438">1 DB-filer på SP</span><span class="sxs-lookup"><span data-stu-id="c4efc-438">1 DB Files on SP</span></span>
* <span data-ttu-id="c4efc-439">2 x lagringspooler per nod</span><span class="sxs-lookup"><span data-stu-id="c4efc-439">2 x Storage Pools per Node</span></span>

![Appendix1][11]

### <a name="vm"></a><span data-ttu-id="c4efc-441">VM:</span><span class="sxs-lookup"><span data-stu-id="c4efc-441">VM:</span></span>
<span data-ttu-id="c4efc-442">I det här exemplet ska vi visa flyttas från en ELB till ILB.</span><span class="sxs-lookup"><span data-stu-id="c4efc-442">In this example we are going to demonstrate moving from an ELB to ILB.</span></span> <span data-ttu-id="c4efc-443">ELB fanns innan ILB, så att det visar hur du växlar till detta under migreringen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-443">ELB was available before ILB, so this shows how to switch to this during the migration.</span></span>

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a><span data-ttu-id="c4efc-445">Pre steg: Ansluta till prenumeration</span><span class="sxs-lookup"><span data-stu-id="c4efc-445">Pre Steps: Connect to Subscription</span></span>
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a><span data-ttu-id="c4efc-446">Steg 1: Skapa nytt Lagringskonto och Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="c4efc-446">Step 1: Create New Storage Account and Cloud Service</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a><span data-ttu-id="c4efc-447">Steg 2: Öka tillåtna fel för resurser<Optional></span><span class="sxs-lookup"><span data-stu-id="c4efc-447">Step 2: Increase the permitted failures on resources <Optional></span></span>
<span data-ttu-id="c4efc-448">På vissa resurser som hör till din Always On-Tillgänglighetsgruppen finns begränsningar på hur många fel som kan uppstå under en period där Klustertjänsten försöker starta om resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-448">On certain resources that belong to your Always On Availability Group there are limits on how many failures that can occur in a period, where the cluster service will attempt to restart the resource group.</span></span> <span data-ttu-id="c4efc-449">Det rekommenderas att du ökar det här medan du gå igenom den här proceduren sedan om du manuellt inte växling vid fel och utlösare redundans genom att stänga av datorer som du kan hämta nära den här gränsen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-449">It is recommended you increase this whilst you are walking through this procedure, since if you don’t manually failover and trigger failovers by shutting down machines you can get close to this limit.</span></span>

<span data-ttu-id="c4efc-450">Det skulle vara klokt fördubbla fel ersättning för att göra detta i hanteraren för redundanskluster, gå till egenskaperna för resursgruppen Always On:</span><span class="sxs-lookup"><span data-stu-id="c4efc-450">It would be prudent to double the failure allowance, to do this in Failover Cluster Manager, go to the Properties of the Always On resource group:</span></span>

![Appendix3][13]

<span data-ttu-id="c4efc-452">Ändra maximalt antal misslyckanden till 6.</span><span class="sxs-lookup"><span data-stu-id="c4efc-452">Change the Maximum Failures to 6.</span></span>

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a><span data-ttu-id="c4efc-453">Steg 3: Lägga IP-adressresurs för klustergruppen<Optional></span><span class="sxs-lookup"><span data-stu-id="c4efc-453">Step 3: Addition IP Address resource for Cluster Group <Optional></span></span>
<span data-ttu-id="c4efc-454">Om du bara ha en IP-adress för klustergruppen och detta justeras till undernätet för molnet, Tänk, om du av misstag koppla från alla noder i klustret i molnet på nätverket sedan klustrets IP-resurs och Klusternätverksnamnet kommer inte att kunna anslutas.</span><span class="sxs-lookup"><span data-stu-id="c4efc-454">If you have only one IP address for the Cluster Group and this is aligned to the cloud subnet, beware, if you accidentally take offline all cluster nodes in the cloud on that network then the Cluster IP resource and Cluster Network Name will not be able to come online.</span></span> <span data-ttu-id="c4efc-455">Vid detta hindrar det uppdateringar till andra resurser i klustret.</span><span class="sxs-lookup"><span data-stu-id="c4efc-455">In the event of this it will prevent updates to other cluster resources.</span></span>

#### <a name="step-4-dns-configuration"></a><span data-ttu-id="c4efc-456">Steg 4: DNS-konfiguration</span><span class="sxs-lookup"><span data-stu-id="c4efc-456">Step 4: DNS configuration</span></span>
<span data-ttu-id="c4efc-457">För att implementera en mjuk övergång beror på hur DNS används utnyttjade och uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c4efc-457">To implement a smooth transition depends on how DNS is being utilized and updated.</span></span>
<span data-ttu-id="c4efc-458">När alltid är installerat, skapas en klusterresurs för Windows-grupp om du öppnar Klusterhanteraren för växling visas att åtminstone den har tre resurser, de två som dokumentet refererar till är:</span><span class="sxs-lookup"><span data-stu-id="c4efc-458">When Always On is installed, it creates a Windows Cluster Resource group, if you open Failover Cluster Manager, you will see that at a minimum it will have three resources, the two that the document refers to are:</span></span>

* <span data-ttu-id="c4efc-459">Virtuellt nätverksnamn (VNN) – detta är DNS-namnet som klienten ansluta till om du vill ansluta till SQL-servrar via alltid på.</span><span class="sxs-lookup"><span data-stu-id="c4efc-459">Virtual Network Name (VNN) – This is the DNS name that client connect to when wanting to connect to SQL Servers via Always On.</span></span>
* <span data-ttu-id="c4efc-460">IP-adressresurs – detta är den IP-adressen som är associerade med VNN, du kan ha flera och i multisitekonfigurationen har du en IP-adress per plats-undernät.</span><span class="sxs-lookup"><span data-stu-id="c4efc-460">IP Address Resource – This is the IP address that associated with the VNN, you can have more than one, and in a multisite configuration you will have an IP address per site/subnet.</span></span>

<span data-ttu-id="c4efc-461">När du ansluter till SQL Server, SQL Server Client drivrutinen hämtar DNS-poster som är associerade med lyssnaren och försök att ansluta till varje alltid på associerade IP-adress, diskuterar nedan vi några faktorer som kan påverka detta.</span><span class="sxs-lookup"><span data-stu-id="c4efc-461">When connecting to SQL Server, the SQL Server Client driver will retrieve the DNS records associated with the listener and try to connect to each Always On associated IP address, below we discuss some factors that can influence this.</span></span>

<span data-ttu-id="c4efc-462">Antalet samtidiga DNS-poster som är associerade med grupplyssnarens namn beror inte bara på antalet IP-adresser som är associerade, men ' RegisterAllIpProviders'setting i redundanskluster för Always ON VNN resursen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-462">The number of concurrent DNS records that are associated with the listener name depends not only on the number of IP addresses associated, but the ‘RegisterAllIpProviders’setting in Failover Clustering for the Always ON VNN resource.</span></span>

<span data-ttu-id="c4efc-463">När du distribuerar alltid på i Azure finns på olika steg för att skapa lyssnare och IP-adresser, måste du manuellt konfigurerar RegisterAllIpProviders 1, detta skiljer sig till en lokal alltid på distribution där det redan har angetts till 1.</span><span class="sxs-lookup"><span data-stu-id="c4efc-463">When you deploy Always On in Azure there are different steps to create the Listener and IP Addresses, you have to manually configure the ‘RegisterAllIpProviders’ to 1, this is different to an on-premises Always On deployment where it is already set to 1.</span></span>

<span data-ttu-id="c4efc-464">'RegisterAllIpProviders' är 0, och sedan visas bara en DNS-post i DNS som är associerade med lyssnare:</span><span class="sxs-lookup"><span data-stu-id="c4efc-464">If ‘RegisterAllIpProviders’ is 0, then you will only see one DNS record in DNS associated with the Listener:</span></span>

![Appendix4][14]

<span data-ttu-id="c4efc-466">Om 'RegisterAllIpProviders' är 1:</span><span class="sxs-lookup"><span data-stu-id="c4efc-466">If ‘RegisterAllIpProviders’ is 1:</span></span>

![Appendix5][15]

<span data-ttu-id="c4efc-468">Koden nedan kommer dump ut VNN inställningarna och ange du, Observera för att ändringarna ska börja gälla behöver du kopplar VNN och aktivera det igen, det här tar den lyssnare offline orsakar avbrott för anslutning av klienten.</span><span class="sxs-lookup"><span data-stu-id="c4efc-468">The code below will dump out the VNN settings and set it for you, please note, for the change to take effect you will need to take the VNN offline and turn it back online, this taking the Listener offline causing client connectivity disruption.</span></span>

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

<span data-ttu-id="c4efc-469">I ett senare migreringssteg behöver du uppdatera Always On-lyssnaren med en uppdaterad IP-adress som ska referera till en belastningsutjämnare detta kommer att omfatta en IP-adress resurs borttagning och tillägg.</span><span class="sxs-lookup"><span data-stu-id="c4efc-469">In a later migration step you will need to update the Always On listener with an updated IP address that will reference a load balancer, this will involve an IP Address resource removal and addition.</span></span> <span data-ttu-id="c4efc-470">När IP-uppdateringen måste du se till att den nya IP-adressen har uppdaterats i DNS-zonen och att klienterna uppdaterar sin lokala DNS-cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-470">After the IP update, you need to ensure the new IP address has been updated in DNS Zone and that the clients are updating their local DNS cache.</span></span>

<span data-ttu-id="c4efc-471">Om klienterna finns i olika nätverkssegment och referera till en annan DNS-server, måste du vad händer om DNS-zonöverföring under migreringen, eftersom programmet återansluta kommer inte begränsas av zonen överför tiden för alla nya IP-adresser för lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="c4efc-471">If your clients reside in a different network segment and reference a different DNS server, you need to consider what happens about DNS Zone Transfer during the migration, as the application reconnect time will be constrained by at least the Zone Transfer Time of any new IP addresses for the listener.</span></span> <span data-ttu-id="c4efc-472">Om du är under tidsbegränsning här ska du diskutera testa att tvinga en inkrementell zonöverföring med Windows-grupper och också DNS-värdpost till en lägre Time To Live (TTL), så uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c4efc-472">If you are under time constraint here, you should discuss and test forcing an incremental zone transfer with your Windows teams, and also put the DNS host record to a lower Time To Live (TTL), so the clients update.</span></span> <span data-ttu-id="c4efc-473">Mer information finns i [inkrementella zonöverföringar](https://technet.microsoft.com/library/cc958973.aspx) och [Start DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4efc-473">For more information, see [Incremental Zone Transfers](https://technet.microsoft.com/library/cc958973.aspx) and [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span></span>

<span data-ttu-id="c4efc-474">Som standard TTL-värde för DNS-post som är kopplad till lyssnare i alltid på i Azure är 1200 sekunder.</span><span class="sxs-lookup"><span data-stu-id="c4efc-474">By default the TTL for DNS Record that is associated with the Listener in Always On in Azure is 1200 seconds.</span></span> <span data-ttu-id="c4efc-475">Kan du minska detta om du är i gång begränsningen under migreringen så klienterna uppdaterar sina DNS med den uppdaterade IP-adressen för lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="c4efc-475">You may wish to reduce this if you are under time constraint during your migration to ensure the clients update their DNS with the updated IP address for the listener.</span></span> <span data-ttu-id="c4efc-476">Du kan se och ändra konfigurationen av dumpning ut konfigurationen av VNN:</span><span class="sxs-lookup"><span data-stu-id="c4efc-476">You can see and modify the configuration by dumping out the configuration of the VNN:</span></span>

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

<span data-ttu-id="c4efc-477">Observera Ju lägre den 'HostRecordTTL', kommer att ske i en större mängd DNS-trafik.</span><span class="sxs-lookup"><span data-stu-id="c4efc-477">Please note, the lower the ‘HostRecordTTL’, a higher amount of DNS traffic will occur.</span></span>

##### <a name="client-application-settings"></a><span data-ttu-id="c4efc-478">Inställningar för program</span><span class="sxs-lookup"><span data-stu-id="c4efc-478">Client application settings</span></span>
<span data-ttu-id="c4efc-479">Om din SQL-klientprogrammet stöder .net 4.5 SQLClient och du kan använda ' MULTISUBNETFAILOVER = TRUE ”nyckelord, det rekommenderas att tillämpas, eftersom den kan användas för snabbare anslutning till SQL Always On-Tillgänglighetsgruppen under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c4efc-479">If your SQL client application supports the .Net 4.5 SQLClient, then you can use ‘MULTISUBNETFAILOVER=TRUE’ keyword, this is recommended to be applied as it allows for faster connection to SQL Always On Availability Group during failover.</span></span> <span data-ttu-id="c4efc-480">Den räknar igenom alla IP-adresser som är associerade med Always On-lyssnaren parallellt och utför en mer aggressiv TCP försök anslutningshastighet under en växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c4efc-480">It enumerates through all IP addresses associated with the Always On listener in parallel, and performs a more aggressive TCP connection retry speed during a failover.</span></span>

<span data-ttu-id="c4efc-481">Mer information om inställningarna ovan finns [MultiSubnetFailover nyckelord och associerade funktioner](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span><span class="sxs-lookup"><span data-stu-id="c4efc-481">For more information regarding the settings above, please see [MultiSubnetFailover Keyword and Associated Features](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span></span> <span data-ttu-id="c4efc-482">Se även [SqlClient-stöd för hög tillgänglighet och katastrofåterställning](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="c4efc-482">Also see [SqlClient Support for High Availability, Disaster Recovery](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span></span>

#### <a name="step-5-cluster-quorum-settings"></a><span data-ttu-id="c4efc-483">Steg 5: Inställningarna för klusterkvorum</span><span class="sxs-lookup"><span data-stu-id="c4efc-483">Step 5: Cluster quorum settings</span></span>
<span data-ttu-id="c4efc-484">Du kommer att ta reda på minst en SQL Server anges i taget, du bör ändra inställningen klustrets kvorum, om du använder filen filresurs vittne (FSW) med 2-noder, bör du ange kvorum för att tillåta Nodmajoritet och använda dynamiska röstning , och detta är att en nod ska vara stående.</span><span class="sxs-lookup"><span data-stu-id="c4efc-484">As you are going to be taking out at least one SQL Server down at a time, you should modify the cluster quorum setting, if using File Share Witness (FSW) with 2 nodes, you should set the quorum to allow node majority and utilize dynamic voting, and this is to allow for a single node to remain standing.</span></span>

    Set-ClusterQuorum -NodeMajority  

<span data-ttu-id="c4efc-485">Mer information om att hantera och konfigurera klustrets kvorum finns [konfigurera och hantera kvorum i ett redundanskluster för Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4efc-485">For more information on managing and configuring the cluster quorum, please see [Configure and Manage the Quorum in a Windows Server 2012 Failover Cluster](https://technet.microsoft.com/library/jj612870.aspx).</span></span>

#### <a name="step-6-extract-existing-endpoints-and-acls"></a><span data-ttu-id="c4efc-486">Steg 6: Extrahera befintliga slutpunkter och ACL: er</span><span class="sxs-lookup"><span data-stu-id="c4efc-486">Step 6: Extract Existing Endpoints and ACLs</span></span>
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

<span data-ttu-id="c4efc-487">Spara dem till en textfil.</span><span class="sxs-lookup"><span data-stu-id="c4efc-487">Save these to a text file.</span></span>

#### <a name="step-7-change-failover-partners-and-replication-modes"></a><span data-ttu-id="c4efc-488">Steg 7: Ändra Redundanspartners och replikering lägen</span><span class="sxs-lookup"><span data-stu-id="c4efc-488">Step 7: Change Failover Partners and Replication Modes</span></span>
<span data-ttu-id="c4efc-489">Om du har mer än 2 SQL-servrar kan du ändra redundans för en annan sekundär i en annan Domänkontrollant eller lokalt till 'Synkron' och göra det en automatisk redundans Partner (AFP), detta är så att du kan hantera HA medan du gör ändringar.</span><span class="sxs-lookup"><span data-stu-id="c4efc-489">If you have more than 2 SQL Servers, you should change the failover of another secondary in another DC or on-premises to ‘Synchronous’ and make it an Automatic Failover Partner (AFP), this is so you maintain HA whilst you are making changes.</span></span> <span data-ttu-id="c4efc-490">Du kan göra detta via TSQL av dock ändra SSMS:</span><span class="sxs-lookup"><span data-stu-id="c4efc-490">You can do this through TSQL of modify though SSMS:</span></span>

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a><span data-ttu-id="c4efc-492">Steg 8: Ta bort sekundära virtuella datorn från Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="c4efc-492">Step 8: Remove Secondary VM from cloud service</span></span>
<span data-ttu-id="c4efc-493">Du bör planera att migrera en sekundär molnnod först om det är för närvarande primära, bör du initiera en manuell växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c4efc-493">You should be planning to migrate a cloud secondary node first, if this is currently primary, you should initiate a manual failover.</span></span>

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="c4efc-494">Steg 9: Ändra disken cacheinställningar i CSV-filen och spara</span><span class="sxs-lookup"><span data-stu-id="c4efc-494">Step 9: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="c4efc-495">För datavolymer som ska dessa anges till READONLY.</span><span class="sxs-lookup"><span data-stu-id="c4efc-495">For data volumes these should be set to READONLY.</span></span>

<span data-ttu-id="c4efc-496">För volymer som TLOG ska dessa anges till NONE.</span><span class="sxs-lookup"><span data-stu-id="c4efc-496">For TLOG volumes these should be set to NONE.</span></span>

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a><span data-ttu-id="c4efc-498">Steg 10: Kopiera virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="c4efc-498">Step 10: Copy VHDS</span></span>
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



<span data-ttu-id="c4efc-499">Du kan kontrollera status för kopia av de virtuella hårddiskarna till Premium Storage-konto:</span><span class="sxs-lookup"><span data-stu-id="c4efc-499">You can check the copy status of the VHDs to the Premium Storage account:</span></span>

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

<span data-ttu-id="c4efc-501">Vänta tills alla dessa registreras åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="c4efc-501">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="c4efc-502">Information för enskilda BLOB:</span><span class="sxs-lookup"><span data-stu-id="c4efc-502">For information for individual blobs:</span></span>

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a><span data-ttu-id="c4efc-503">Steg 11: Registrera OS-disk</span><span class="sxs-lookup"><span data-stu-id="c4efc-503">Step 11: Register OS disk</span></span>
    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a><span data-ttu-id="c4efc-504">Steg 12: Importera sekundär till ny molntjänst</span><span class="sxs-lookup"><span data-stu-id="c4efc-504">Step 12: Import secondary into new cloud service</span></span>
<span data-ttu-id="c4efc-505">Koden nedan använder också alternativet tillagda här kan du importera datorn och använda retainable VIP.</span><span class="sxs-lookup"><span data-stu-id="c4efc-505">The code below also uses the added option here you can import the machine and use the retainable VIP.</span></span>

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="c4efc-506">Steg 13: Skapa ILB på nya molntjänster Svc lägga till belastningen belastningsutjämnade slutpunkter och ACL: er</span><span class="sxs-lookup"><span data-stu-id="c4efc-506">Step 13: Create ILB on New Cloud Svc, Add Load Balanced Endpoints and ACLs</span></span>
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

#### <a name="step-14-update-always-on"></a><span data-ttu-id="c4efc-507">Steg 14: Uppdatera alltid på</span><span class="sxs-lookup"><span data-stu-id="c4efc-507">Step 14: Update Always On</span></span>
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

<span data-ttu-id="c4efc-509">Nu ska du ta bort den gamla Molntjänsten IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c4efc-509">Now remove the old cloud service IP Address.</span></span>

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a><span data-ttu-id="c4efc-511">Steg 15: DNS-uppdateringskontroll</span><span class="sxs-lookup"><span data-stu-id="c4efc-511">Step 15: DNS update check</span></span>
<span data-ttu-id="c4efc-512">Du bör nu se DNS-servrar på SQL Server-klient-nätverk och kontrollera att kluster har lagt till extra värdpost för IP-adress som lagts till.</span><span class="sxs-lookup"><span data-stu-id="c4efc-512">You should now check DNS Servers on your SQL Server client networks and make sure that clustering has added the extra host record for the added IP address.</span></span> <span data-ttu-id="c4efc-513">Om dessa DNS-servrar som inte har uppdaterat, Överväg att tvinga en DNS-zonöverföring och se till att klienterna i det undernätet är kan matcha till båda alltid på IP-adresser, det är så du inte behöver vänta på att automatisk DNS-replikeringen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-513">If those DNS servers have not updated, consider forcing a DNS Zone transfer and ensure that the clients in there subnet are able to resolve to both Always On IP Addresses, this is so you do not need to wait for automatic DNS replication.</span></span>

#### <a name="step-16-reconfigure-always-on"></a><span data-ttu-id="c4efc-514">Steg 16: Konfigurera om alltid på</span><span class="sxs-lookup"><span data-stu-id="c4efc-514">Step 16: Reconfigure Always On</span></span>
<span data-ttu-id="c4efc-515">Då vänta du på sekundärt noden som migrerades att fullständigt omsynkronisering med den lokala noden och växla till synkron replikeringsnod och göra det i AFP.</span><span class="sxs-lookup"><span data-stu-id="c4efc-515">At this point you wait for the secondary that node that was migrated to fully resynchronize with the on-premises node and switch to synchronous replication node and make it the AFP.</span></span>  

#### <a name="step-17-migrate-second-node"></a><span data-ttu-id="c4efc-516">Steg 17: Migrera andra noden</span><span class="sxs-lookup"><span data-stu-id="c4efc-516">Step 17: Migrate second node</span></span>
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="c4efc-517">Steg 18: Ändra disken cacheinställningar i CSV-filen och spara</span><span class="sxs-lookup"><span data-stu-id="c4efc-517">Step 18: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="c4efc-518">För datavolymer som ska dessa anges till READONLY.</span><span class="sxs-lookup"><span data-stu-id="c4efc-518">For data volumes these should be set to READONLY.</span></span>

<span data-ttu-id="c4efc-519">För volymer som TLOG ska dessa anges till NONE.</span><span class="sxs-lookup"><span data-stu-id="c4efc-519">For TLOG volumes these should be set to NONE.</span></span>

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a><span data-ttu-id="c4efc-521">Steg 19: Skapa nytt oberoende Lagringskonto för sekundär nod</span><span class="sxs-lookup"><span data-stu-id="c4efc-521">Step 19: Create New Independent Storage Account for Secondary Node</span></span>
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a><span data-ttu-id="c4efc-522">Steg 20: Kopiera virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="c4efc-522">Step 20: Copy VHDS</span></span>
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


<span data-ttu-id="c4efc-523">Du kan kontrollera VHD-kopian status för alla virtuella hårddiskar: ForEach ($disk i $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Disketikett $diskName = $disk. DiskName</span><span class="sxs-lookup"><span data-stu-id="c4efc-523">You can check the VHD copy status for all VHDs: ForEach ($disk in $diskobjects) { $lun = $disk.Lun $vhdname = $disk.vhdname $cacheoption = $disk.HostCaching $disklabel = $disk.DiskLabel $diskName = $disk.DiskName</span></span>

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

<span data-ttu-id="c4efc-525">Vänta tills alla dessa registreras åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="c4efc-525">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="c4efc-526">Information för enskilda BLOB:</span><span class="sxs-lookup"><span data-stu-id="c4efc-526">For information for individual blobs:</span></span>

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a><span data-ttu-id="c4efc-527">Steg 21: Registrera OS-disk</span><span class="sxs-lookup"><span data-stu-id="c4efc-527">Step 21: Register OS disk</span></span>
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="c4efc-528">Steg 22: Lägg till belastningen belastningsutjämnade slutpunkter och ACL: er</span><span class="sxs-lookup"><span data-stu-id="c4efc-528">Step 22: Add Load Balanced Endpoints and ACLs</span></span>
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a><span data-ttu-id="c4efc-529">Steg 23: Testa redundans</span><span class="sxs-lookup"><span data-stu-id="c4efc-529">Step 23: Test failover</span></span>
<span data-ttu-id="c4efc-530">Nu bör du låta den migrerade noden synkroniseras med lokala alltid på noden och placera den i synkron replikering läge och vänta tills den är synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="c4efc-530">You should now let the migrated node synchronize with the on-premises Always On node, place it in to synchronous replication mode and wait until it is synchronized.</span></span> <span data-ttu-id="c4efc-531">Sedan migreras växling från lokal till den första noden, vilket är AFP.</span><span class="sxs-lookup"><span data-stu-id="c4efc-531">Then failover from on-prem to the first node migrated, which is the AFP.</span></span> <span data-ttu-id="c4efc-532">När som har arbetat ändra sista migrerade noden till AFP.</span><span class="sxs-lookup"><span data-stu-id="c4efc-532">Once that has worked, change the last migrated node to the AFP.</span></span>

<span data-ttu-id="c4efc-533">Du bör redundanstestningen mellan alla noder och kör om chaos testerna för att se till att redundans fungerar som förväntat och en rimlig manor.</span><span class="sxs-lookup"><span data-stu-id="c4efc-533">You should test failovers between all nodes and run though chaos tests to ensure failovers work as expected and in a timely manor.</span></span>

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a><span data-ttu-id="c4efc-534">Steg 24: Lägga tillbaka inställningarna för klusterkvorum / DNS TTL / Failover Pntrs / synkroniseringsinställningar</span><span class="sxs-lookup"><span data-stu-id="c4efc-534">Step 24: Put back cluster quorum settings / DNS TTL / Failover Pntrs / Sync Settings</span></span>
##### <a name="adding-ip-address-resource-on-same-subnet"></a><span data-ttu-id="c4efc-535">Lägger till IP-adressresurs på samma undernät</span><span class="sxs-lookup"><span data-stu-id="c4efc-535">Adding IP Address Resource on Same Subnet</span></span>
<span data-ttu-id="c4efc-536">Om du har bara 2 SQL-servrar och vill migrera dem till en ny molntjänst, men vill behålla dem i samma undernät, kan du undvika tar lyssnaren offline för att ta bort den ursprungliga alltid på IP-adressen och lägga till den nya IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="c4efc-536">If you have only 2 SQL Servers and want to migrate them to a new cloud service, but want to keep them on the same subnet, you can avoid taking the listener offline to delete the original Always On IP Address and add the New IP Address.</span></span> <span data-ttu-id="c4efc-537">Om du migrerar de virtuella datorerna till ett annat undernät behöver du inte göra detta som en ytterligare klusternätverk som kommer att referera till det undernätet.</span><span class="sxs-lookup"><span data-stu-id="c4efc-537">If you are migrating the VMs to another subnet you will not need to do this as there will be an additional cluster network that will reference that subnet.</span></span>

<span data-ttu-id="c4efc-538">När du har växa upp den migrerade sekundärt och läggas till i den nya resursen IP-adressen för den nya Molntjänsten innan redundans den befintliga primärt, bör du vidta dessa åtgärder inom klustret Failover Manager:</span><span class="sxs-lookup"><span data-stu-id="c4efc-538">Once you have brought up the migrated secondary and added in the new IP Address resource for the new cloud service before failover the existing Primary, you should take these steps within the Cluster Failover Manager:</span></span>

<span data-ttu-id="c4efc-539">Om du vill lägga till i IP-adress, finns det [bilaga](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), steg 14.</span><span class="sxs-lookup"><span data-stu-id="c4efc-539">To add in IP Address, see the [Appendix](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), step 14.</span></span>

1. <span data-ttu-id="c4efc-540">Ändra möjliga ägare till 'befintliga primära SQL Server-i exemplet nedan, 'dansqlams4' för den aktuella resursen i IP-adress:</span><span class="sxs-lookup"><span data-stu-id="c4efc-540">For the current IP Address resource, change the possible owner to ‘Existing Primary SQL Server’, in the example below, ‘dansqlams4’:</span></span>

    ![Appendix13][23]
2. <span data-ttu-id="c4efc-542">Ändra möjliga ägare till 'Migrerats sekundära SQL Server ”i exemplet nedan, 'dansqlams5' för den nya resursen i IP-adress:</span><span class="sxs-lookup"><span data-stu-id="c4efc-542">For the new IP Address resource, change the possible owner to ‘Migrated secondary SQL Server’, in the example below, ‘dansqlams5’:</span></span>

    ![Appendix14][24]
3. <span data-ttu-id="c4efc-544">När det här värdet kan du redundans och när den sista noden migreras möjliga ägare måste redigeras så noden läggs till som möjlig ägare:</span><span class="sxs-lookup"><span data-stu-id="c4efc-544">Once this is set you can failover, and when the last node is migrated the Possible Owners should be edited so that node is added as a Possible Owner:</span></span>

    ![Appendix15][25]

## <a name="additional-resources"></a><span data-ttu-id="c4efc-546">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c4efc-546">Additional resources</span></span>
* [<span data-ttu-id="c4efc-547">Azure Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="c4efc-547">Azure Premium Storage</span></span>](../../../storage/common/storage-premium-storage.md)
* [<span data-ttu-id="c4efc-548">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="c4efc-548">Virtual Machines</span></span>](https://azure.microsoft.com/services/virtual-machines/)
* [<span data-ttu-id="c4efc-549">SQLServer i Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="c4efc-549">SQL Server in Azure Virtual Machines</span></span>](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
