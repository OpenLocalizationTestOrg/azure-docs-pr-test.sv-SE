---
title: aaaUse Azure Premium-lagring med SQL Server | Microsoft Docs
description: "Den här artikeln använder resurser som har skapats med hello klassiska distributionsmodellen och ger vägledning om hur du använder Azure Premium-lagring med SQL Server som körs på Azure Virtual Machines."
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
ms.openlocfilehash: 393ea2020b39ea686302ae632e1049935c24af00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a><span data-ttu-id="e85c0-103">Använd Azure Premium Storage med SQL Server på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e85c0-103">Use Azure Premium Storage with SQL Server on Virtual Machines</span></span>
## <a name="overview"></a><span data-ttu-id="e85c0-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e85c0-104">Overview</span></span>
<span data-ttu-id="e85c0-105">[Azure Premium-lagring](../../../storage/common/storage-premium-storage.md) är hello nästa generation av lagring som innehåller låg latens och hög genomströmning IO.</span><span class="sxs-lookup"><span data-stu-id="e85c0-105">[Azure Premium Storage](../../../storage/common/storage-premium-storage.md) is hello next generation of storage that provides low latency and high throughput IO.</span></span> <span data-ttu-id="e85c0-106">Det fungerar bäst för nyckel i/o-intensiv arbetsbelastning, till exempel SQL Server på IaaS [virtuella datorer](https://azure.microsoft.com/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="e85c0-106">It works best for key IO intensive workloads, such as SQL Server on IaaS [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e85c0-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e85c0-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="e85c0-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="e85c0-109">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="e85c0-110">Den här artikeln innehåller planering och vägledning för att migrera en virtuell dator som kör SQL Server toouse Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="e85c0-110">This article provides planning and guidance for migrating a Virtual Machine running SQL Server toouse Premium Storage.</span></span> <span data-ttu-id="e85c0-111">Detta inkluderar Azure-infrastrukturen (nätverk, lagring) och Gäst Windows VM steg.</span><span class="sxs-lookup"><span data-stu-id="e85c0-111">This includes Azure infrastructure (networking, storage) and guest Windows VM steps.</span></span> <span data-ttu-id="e85c0-112">hello exemplet i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) visar en fullständig omfattande slutet tooend migrering av hur toomove större virtuella datorer tootake nytta av bättre lokala SSD-lagring med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e85c0-112">hello example in hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) shows a full comprehensive end tooend migration of how toomove larger VMs tootake advantage of improved local SSD storage with PowerShell.</span></span>

<span data-ttu-id="e85c0-113">Det är viktigt toounderstand hello slutpunkt till slutpunkt-processen genom att använda Azure Premium-lagring med SQL Server på IAAS-VM.</span><span class="sxs-lookup"><span data-stu-id="e85c0-113">It is important toounderstand hello end-to-end process of utilizing Azure Premium Storage with SQL Server on IAAS VMs.</span></span> <span data-ttu-id="e85c0-114">Detta omfattar:</span><span class="sxs-lookup"><span data-stu-id="e85c0-114">This includes:</span></span>

* <span data-ttu-id="e85c0-115">Identifiering av hello krav toouse Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="e85c0-115">Identification of hello prerequisites toouse Premium Storage.</span></span>
* <span data-ttu-id="e85c0-116">Exempel på distribution av SQL Server på IaaS tooPremium lagring för nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="e85c0-116">Examples of deploying SQL Server on IaaS tooPremium Storage for new deployments.</span></span>
* <span data-ttu-id="e85c0-117">Exempel på migrera befintliga distributioner, både fristående servrar och distributioner med hjälp av SQL Always On-Tillgänglighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="e85c0-117">Examples of migrating existing deployments, both stand-alone servers and deployments using SQL Always On Availability Groups.</span></span>
* <span data-ttu-id="e85c0-118">Möjliga migrering metoder.</span><span class="sxs-lookup"><span data-stu-id="e85c0-118">Possible migration approaches.</span></span>
* <span data-ttu-id="e85c0-119">Fullständig slutpunkt till slutpunkt i exemplet visar Azure, Windows och SQL Server stegen för hello migrering av en befintlig Always On-implementering.</span><span class="sxs-lookup"><span data-stu-id="e85c0-119">Full end-to-end example showing Azure, Windows, and SQL Server steps for hello migration of an existing Always On implementation.</span></span>

<span data-ttu-id="e85c0-120">Mer information om SQL Server i Azure Virtual Machines finns [SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e85c0-120">For more background information on SQL Server in Azure Virtual Machines, see [SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

<span data-ttu-id="e85c0-121">**Skapad av:** Mikael Sol **Teknisk granskare:** Thomas Carlos Vargas sill, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Pettersson.</span><span class="sxs-lookup"><span data-stu-id="e85c0-121">**Author:** Daniel Sol **Technical Reviewers:** Luis Carlos Vargas Herring, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.</span></span>

## <a name="prerequisites-for-premium-storage"></a><span data-ttu-id="e85c0-122">Krav för Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="e85c0-122">Prerequisites for Premium Storage</span></span>
<span data-ttu-id="e85c0-123">Det finns flera förutsättningar för att använda Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="e85c0-123">There are several prerequisites for using Premium Storage.</span></span>

### <a name="machine-size"></a><span data-ttu-id="e85c0-124">Storleken på datorn</span><span class="sxs-lookup"><span data-stu-id="e85c0-124">Machine size</span></span>
<span data-ttu-id="e85c0-125">För att använda Premium-lagring måste toouse DS-serien virtuella datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="e85c0-125">For using Premium Storage you will need toouse DS series Virtual Machines (VM).</span></span> <span data-ttu-id="e85c0-126">Om du inte har använt DS-serien datorer i din molntjänst innan du måste ta bort befintliga VM hello hålla hello anslutna diskar och sedan skapa en ny molntjänst innan återskapa hello VM som DS * rollstorleken.</span><span class="sxs-lookup"><span data-stu-id="e85c0-126">If you have not used DS Series machines in your cloud service before, you must delete hello existing VM, keep hello attached disks, and then create a new cloud service before recreating hello VM as DS* role size.</span></span> <span data-ttu-id="e85c0-127">Mer information om storlekar för virtuella datorer finns [virtuell dator och Molntjänststorlekar för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e85c0-127">For more information on Virtual Machine sizes, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="cloud-services"></a><span data-ttu-id="e85c0-128">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="e85c0-128">Cloud services</span></span>
<span data-ttu-id="e85c0-129">Du kan bara använda DS * virtuella datorer med Premium-lagring när de skapas i en ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="e85c0-129">You can only use DS* VMs with Premium Storage when they are created in a new cloud service.</span></span> <span data-ttu-id="e85c0-130">Om du använder SQL Server alltid aktiverad i Azure syftar hello alltid lyssnare för toohello Azure interna eller externa belastningen belastningsutjämnaren IP-adress som är associerad med en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-130">If you are using SQL Server Always On in Azure, hello Always On Listener will refer toohello Azure Internal or External Load Balancer IP address that is associated with a cloud service.</span></span> <span data-ttu-id="e85c0-131">Den här artikeln fokuserar på hur toomigrate samtidigt är tillgänglig i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="e85c0-131">This article focuses on how toomigrate while maintaining availability in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="e85c0-132">En serie med DS * hello första virtuella dator som är distribuerade toohello ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="e85c0-132">A DS* Series must be hello first VM that is deployed toohello new Cloud Service.</span></span>
>
>

### <a name="regional-vnets"></a><span data-ttu-id="e85c0-133">Regional VNET</span><span class="sxs-lookup"><span data-stu-id="e85c0-133">Regional VNETS</span></span>
<span data-ttu-id="e85c0-134">Du måste konfigurera hello virtuella nätverk (VNET) värd för dina virtuella datorer toobe regionala för DS * virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-134">For DS* VMs you must configure hello Virtual Network (VNET) hosting your VMs toobe regional.</span></span> <span data-ttu-id="e85c0-135">Den här ”utökar” Hej VNET är tooallow hello större virtuella datorer toobe etablerad på andra kluster och tillåta kommunikation mellan dem.</span><span class="sxs-lookup"><span data-stu-id="e85c0-135">This “widens” hello VNET is tooallow hello larger VMs toobe provisioned in other clusters and allow communication between them.</span></span> <span data-ttu-id="e85c0-136">I följande skärmbild hello, visar hello markerade platsen regional Vnet hello första resultatet visar ett ”smala” VNET.</span><span class="sxs-lookup"><span data-stu-id="e85c0-136">In hello following screenshot, hello highlighted Location shows regional VNETs, whereas hello first result shows a “narrow” VNET.</span></span>

![RegionalVNET][1]

<span data-ttu-id="e85c0-138">Du kan höja ett Microsoft-supporten biljett toomigrate tooa regionalt VNET, Microsoft gör en ändring sedan toocomplete hello migrering tooregional Vnet, ändra hello egenskapen AffinityGroup i hello nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-138">You can raise a Microsoft support ticket toomigrate tooa regional VNET, Microsoft will make a change, then toocomplete hello migration tooregional VNETs, change hello property AffinityGroup in hello network configuration.</span></span> <span data-ttu-id="e85c0-139">Exportera hello nätverkskonfigurationen i PowerShell och Skriv hello **AffinityGroup** egenskap i hello **VirtualNetworkSite** element med en **plats** Egenskapen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-139">First export hello Network Configuration in PowerShell, and then replace hello **AffinityGroup** property in hello **VirtualNetworkSite** element with a **Location** property.</span></span> <span data-ttu-id="e85c0-140">Ange `Location = XXXX` där `XXXX` är en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="e85c0-140">Specify `Location = XXXX` where `XXXX` is an Azure region.</span></span> <span data-ttu-id="e85c0-141">Importera hello ny konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e85c0-141">Then import hello new configuration.</span></span>

<span data-ttu-id="e85c0-142">Till exempel överväger hello följande konfiguration av virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="e85c0-142">For example, considering hello following VNET configuration:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

<span data-ttu-id="e85c0-143">toomove denna tooa regionalt VNET i västra Europa ändra hello configuration toohello följande:</span><span class="sxs-lookup"><span data-stu-id="e85c0-143">toomove this tooa regional VNET in West Europe, change hello configuration toohello following:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a><span data-ttu-id="e85c0-144">Lagringskonton</span><span class="sxs-lookup"><span data-stu-id="e85c0-144">Storage accounts</span></span>
<span data-ttu-id="e85c0-145">Du behöver toocreate ett nytt lagringskonto som är konfigurerad för Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="e85c0-145">You will need toocreate a new storage account that is configured for Premium Storage.</span></span> <span data-ttu-id="e85c0-146">Observera att hello användning av Premium-lagring är inställt på hello storage-konto, inte på enskilda virtuella hårddiskar, men när du använder en DS * serien virtuell dator kan du bifoga VHD från Premium och standardlagring konton.</span><span class="sxs-lookup"><span data-stu-id="e85c0-146">Note that hello use of Premium Storage is set at hello storage account, not on individual VHDs, however when using a DS* Series VM you can attach VHD’s from Premium and Standard Storage accounts.</span></span> <span data-ttu-id="e85c0-147">Du kan överväga att detta om du inte vill att tooplace hello OS VHD på toohello Premium Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e85c0-147">You may consider this if you do not want tooplace hello OS VHD on toohello Premium Storage account.</span></span>

<span data-ttu-id="e85c0-148">hello följande **ny AzureStorageAccountPowerShell** med hello ”Premium_LRS” **typen** skapar ett Premiumlagringskonto:</span><span class="sxs-lookup"><span data-stu-id="e85c0-148">hello following **New-AzureStorageAccountPowerShell** command with hello "Premium_LRS" **Type** creates a Premium Storage Account:</span></span>

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a><span data-ttu-id="e85c0-149">Inställningar för cachelagring av virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="e85c0-149">VHDs Cache Settings</span></span>
<span data-ttu-id="e85c0-150">hello största skillnaden mellan att skapa diskar som är en del av ett premiumlagringskonto är hello diskcache-inställningen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-150">hello main difference between creating disks that are part of a Premium Storage account is hello disk cache setting.</span></span> <span data-ttu-id="e85c0-151">För SQL Server-Data volym diskar det rekommenderas att du använder '**Läs cachelagring**'.</span><span class="sxs-lookup"><span data-stu-id="e85c0-151">For SQL Server Data volume disks it is recommended that you use ‘**Read Caching**’.</span></span> <span data-ttu-id="e85c0-152">För transaktionen loggvolymer hello diskcache-inställningen ska vara inställd för '**ingen**'.</span><span class="sxs-lookup"><span data-stu-id="e85c0-152">For Transaction log volumes, hello disk cache setting should be set too‘**None**’.</span></span> <span data-ttu-id="e85c0-153">Detta skiljer sig från hello rekommendationer för Standard Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="e85c0-153">This is different from hello recommendations for Standard Storage accounts.</span></span>

<span data-ttu-id="e85c0-154">Det kopplade hello virtuella hårddiskar, kan hello cache-inställningen inte ändras.</span><span class="sxs-lookup"><span data-stu-id="e85c0-154">Once hello VHDs have been attached, hello cache setting cannot be altered.</span></span> <span data-ttu-id="e85c0-155">Du behöver toodetach och Återanslut hello VHD med en uppdaterad cache-inställningen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-155">You would need toodetach and reattach hello VHD with an updated cache setting.</span></span>

### <a name="windows-storage-spaces"></a><span data-ttu-id="e85c0-156">Lagringsutrymmen för Windows</span><span class="sxs-lookup"><span data-stu-id="e85c0-156">Windows storage spaces</span></span>
<span data-ttu-id="e85c0-157">Du kan använda [Windows lagringsutrymmen](https://technet.microsoft.com/library/hh831739.aspx) som du gjorde med föregående standardlagring detta gör att du toomigrate en virtuell dator som redan använder lagringsutrymmen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-157">You can use [Windows Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx) as you did with previous Standard Storage, this will allow you toomigrate a VM that is already utilizing Storage Spaces.</span></span> <span data-ttu-id="e85c0-158">hello exemplet i [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (steg 9 och framåt) visar hello Powershell kod tooextract och importera en virtuell dator med flera anslutna virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-158">hello example in [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (step 9 and forward) demonstrates hello Powershell code tooextract and import a VM with multiple attached VHDs.</span></span>

<span data-ttu-id="e85c0-159">Lagringspooler användes med Standard-Azure storage-konto tooenhance genomströmning och minska svarstiden.</span><span class="sxs-lookup"><span data-stu-id="e85c0-159">Storage Pools were used with Standard Azure storage account tooenhance throughput and reduce latency.</span></span> <span data-ttu-id="e85c0-160">Du kan hitta värdet i testning lagringspooler med Premium-lagring för nya distributioner, men de lägger till ytterligare komplexitet med Lagringsinställningar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-160">You might find value in testing Storage Pools with Premium Storage for new deployments, but they do add additional complexity with storage setup.</span></span>

#### <a name="how-toofind-which-azure-virtual-disks-map-toostorage-pools"></a><span data-ttu-id="e85c0-161">Hur toofind vilka virtuella Azure-diskar mappa toostorage pooler</span><span class="sxs-lookup"><span data-stu-id="e85c0-161">How toofind which Azure Virtual Disks map toostorage pools</span></span>
<span data-ttu-id="e85c0-162">Eftersom det finns olika cache inställningen rekommendationer för anslutna virtuella hårddiskar, kan du bestämma toocopy hello virtuella hårddiskar tooa Premium Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e85c0-162">As there are different cache setting recommendations for attached VHDs, you might decide toocopy hello VHDs tooa Premium Storage account.</span></span> <span data-ttu-id="e85c0-163">När du återansluta dem toohello nya DS-serien VM, behöva tooalter hello cacheinställningarna.</span><span class="sxs-lookup"><span data-stu-id="e85c0-163">However, when you reattach them toohello new DS series VM, you might need tooalter hello cache settings.</span></span> <span data-ttu-id="e85c0-164">Det är enklare tooapply hello Premium-lagring rekommenderade inställningar när du har separata virtuella hårddiskar för hello SQL-Data filer och log-filer (istället för en enda virtuell Hårddisk som innehåller både).</span><span class="sxs-lookup"><span data-stu-id="e85c0-164">It is simpler tooapply hello Premium Storage recommended cache settings when you have separate VHDs for hello SQL Data files and log files (rather than a single VHD that contains both).</span></span>

> [!NOTE]
> <span data-ttu-id="e85c0-165">Om du har SQL Server data och loggfilen filer på samma volym, hello cachelagring alternativ du väljer beror på hello-i/o-åtkomstmönster för din databasarbetsbelastningar hello.</span><span class="sxs-lookup"><span data-stu-id="e85c0-165">If you have SQL Server data and log files on hello same volume, hello caching option you choose depends on hello IO access patterns for your database workloads.</span></span> <span data-ttu-id="e85c0-166">Testa bara kan visa vilket alternativ för cachelagring är bäst för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="e85c0-166">Only testing can demonstrate which caching option is best for this scenario.</span></span>
>
>

<span data-ttu-id="e85c0-167">Men om du använder Windows lagringsutrymmen som består av flera virtuella hårddiskar måste toolook på din ursprungliga skript tooidentify som ansluten virtuella hårddiskar finns i vilka specifika poolen, så du kan ange inställningar för cachelagring av hello därefter för varje disk.</span><span class="sxs-lookup"><span data-stu-id="e85c0-167">However, if you are using Windows Storage Spaces which are made up of multiple VHDs you will need toolook at your original scripts tooidentify which attached VHDs are in what specific pool, so you can then set hello cache settings accordingly for each disk.</span></span>

<span data-ttu-id="e85c0-168">Om du inte har ursprungliga skriptet tillgängliga tooshow vilka virtuella hårddiskar mappa toohello lagringspoolen, du kan använda hello följande steg toodetermine hello disklagring/poolen mappning.</span><span class="sxs-lookup"><span data-stu-id="e85c0-168">If you do not have original script available tooshow you which VHDs map toohello storage pool, you can use hello following steps toodetermine hello disk/storage pool mapping.</span></span>

<span data-ttu-id="e85c0-169">Använd hello följande steg för varje disk:</span><span class="sxs-lookup"><span data-stu-id="e85c0-169">For each disk, use hello following steps:</span></span>

1. <span data-ttu-id="e85c0-170">Hämta listan över diskar kopplade tooVM med hello **Get-AzureVM** kommando:</span><span class="sxs-lookup"><span data-stu-id="e85c0-170">Get list of disks attached tooVM with hello **Get-AzureVM** command:</span></span>

    <span data-ttu-id="e85c0-171">Get-AzureVM - ServiceName <servicename> -namnet <vmname> | Get-AzureDataDisk</span><span class="sxs-lookup"><span data-stu-id="e85c0-171">Get-AzureVM -ServiceName <servicename> -Name <vmname> | Get-AzureDataDisk</span></span>
2. <span data-ttu-id="e85c0-172">Observera hello Diskname och LUN.</span><span class="sxs-lookup"><span data-stu-id="e85c0-172">Note hello Diskname and LUN.</span></span>

    ![DisknameAndLUN][2]
3. <span data-ttu-id="e85c0-174">Fjärrskrivbord till hello VM.</span><span class="sxs-lookup"><span data-stu-id="e85c0-174">Remote desktop into hello VM.</span></span> <span data-ttu-id="e85c0-175">Gå sedan för**Datorhantering** | **Enhetshanteraren** | **diskenheter**.</span><span class="sxs-lookup"><span data-stu-id="e85c0-175">Then go too**Computer Management** | **Device Manager** | **Disk Drives**.</span></span> <span data-ttu-id="e85c0-176">Titta på hello egenskaperna för varje hello Microsoft virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="e85c0-176">Look at hello properties of each of hello ‘Microsoft Virtual Disks’</span></span>

    ![VirtualDiskProperties][3]
4. <span data-ttu-id="e85c0-178">hello LUN-nummer här är en referens toohello LUN-nummer som du anger när du ansluter hello VHD toohello VM.</span><span class="sxs-lookup"><span data-stu-id="e85c0-178">hello LUN number here is a reference toohello LUN number you specify when attaching hello VHD toohello VM.</span></span>
5. <span data-ttu-id="e85c0-179">Hello Microsoft Virtual Disk finns toohello **information** på fliken sedan hello **egenskapen** listan, gå för**drivrutinen nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="e85c0-179">For hello ‘Microsoft Virtual Disk’ go toohello **Details** tab, then in hello **Property** list, go too**Driver Key**.</span></span> <span data-ttu-id="e85c0-180">I hello **värdet**, Observera hello **Offset**, vilket är 0002 i hello följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="e85c0-180">In hello **Value**, note hello **Offset**, which is 0002 in hello following screenshot.</span></span> <span data-ttu-id="e85c0-181">hello 0002 anger hello PhysicalDisk2 som hello storage pool referenser.</span><span class="sxs-lookup"><span data-stu-id="e85c0-181">hello 0002 denotes hello PhysicalDisk2 that hello storage pool references.</span></span>

    ![VirtualDiskPropertyDetails][4]
6. <span data-ttu-id="e85c0-183">För varje lagringspool associerade dump ut hello diskar:</span><span class="sxs-lookup"><span data-stu-id="e85c0-183">For each storage pool, dump out hello associated disks:</span></span>

    <span data-ttu-id="e85c0-184">Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk</span><span class="sxs-lookup"><span data-stu-id="e85c0-184">Get-StoragePool -FriendlyName AMS1pooldata | Get-PhysicalDisk</span></span>

    ![GetStoragePool][5]

<span data-ttu-id="e85c0-186">Nu kan du använda anslutna den här informationen tooassociate virtuella hårddiskar tooPhysical diskar i lagringspooler.</span><span class="sxs-lookup"><span data-stu-id="e85c0-186">Now you can use this information tooassociate attached VHDs tooPhysical Disks in Storage Pools.</span></span>

<span data-ttu-id="e85c0-187">När du har mappat virtuella hårddiskar tooPhysical diskar i lagringspooler som du kan koppla bort och kopiera dem över tooa Premium Storage-konto kan sedan koppla dem med hello rätt cache inställningen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-187">Once you have mapped VHDs tooPhysical Disks in Storage Pools you can then detach and copy them over tooa Premium Storage account, then attach them with hello correct cache setting.</span></span> <span data-ttu-id="e85c0-188">Se hello exemplet i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), steg 8 till och med 12.</span><span class="sxs-lookup"><span data-stu-id="e85c0-188">Please see hello example in hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), steps 8 through 12.</span></span> <span data-ttu-id="e85c0-189">Dessa steg visar hur tooextract en VM-ansluten virtuell Hårddisk disk configuration tooa CSV-fil, kopiera hello virtuella hårddiskar, alter hello disk cache konfigurationsinställningar och slutligen omdistribuera hello VM som en serie DS VM med alla hello anslutna diskar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-189">These steps show how tooextract a VM-attached VHD disk configuration tooa CSV file, copy hello VHDs, alter hello disk configuration cache settings, and finally redeploy hello VM as a DS series VM with all hello attached disks.</span></span>

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a><span data-ttu-id="e85c0-190">VM-lagring bandbredd och kapaciteten för lagring av virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="e85c0-190">VM storage bandwidth and VHD storage throughput</span></span>
<span data-ttu-id="e85c0-191">hello beroende lagringsprestanda hello DS * VM-storlek som har angetts och hello VHD-storlek.</span><span class="sxs-lookup"><span data-stu-id="e85c0-191">hello amount of storage performance depends on hello DS* VM size specified and hello VHD sizes.</span></span> <span data-ttu-id="e85c0-192">hello virtuella datorer ha olika tillägg för hello antalet virtuella hårddiskar som kan bifogas och hello Maximal bandbredd som de stöder (MB/s).</span><span class="sxs-lookup"><span data-stu-id="e85c0-192">hello VMs have different allowances for hello number of VHDs that can be attached and hello maximum bandwidth they will support (MB/s).</span></span> <span data-ttu-id="e85c0-193">Hello specifika bandbredd siffror finns [virtuell dator och Molntjänststorlekar för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e85c0-193">For hello specific bandwidth numbers, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="e85c0-194">Ökad IOPS uppnås med större storlekar för diskar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-194">Increased IOPS are achieved with larger disk sizes.</span></span> <span data-ttu-id="e85c0-195">Du bör överväga när du funderar på sökvägen för migreringen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-195">You should consider this when you think about your migration path.</span></span> <span data-ttu-id="e85c0-196">Mer information [finns hello tabellen för IOPS och disktyper](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="e85c0-196">For details, [see hello table for IOPS and Disk Types](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span></span>

<span data-ttu-id="e85c0-197">Slutligen bör du att virtuella datorer har olika maximal disk bandbredder de stöder för alla diskar som är anslutna.</span><span class="sxs-lookup"><span data-stu-id="e85c0-197">Finally, consider that VMs have different maximum disk bandwidths they will support for all disks attached.</span></span> <span data-ttu-id="e85c0-198">Du kan fylla hello maximal disk tillgänglig bandbredd för Virtuella datorns rollstorlek under hög belastning.</span><span class="sxs-lookup"><span data-stu-id="e85c0-198">Under high load, you could saturate hello maximum disk bandwidth available for that VM role size.</span></span> <span data-ttu-id="e85c0-199">Till exempel stöder en Standard_DS14 upp too512MB/s. därför med tre P30 diskar fylla hello diskbandbredden för hello VM.</span><span class="sxs-lookup"><span data-stu-id="e85c0-199">For example a Standard_DS14 will support up too512MB/s; therefore, with three P30 disks you could saturate hello disk bandwidth of hello VM.</span></span> <span data-ttu-id="e85c0-200">Men i det här exemplet hello genomströmning gränsen kan överskridas beroende på hello blandning av läsning och skrivning IOs.</span><span class="sxs-lookup"><span data-stu-id="e85c0-200">But in this example, hello throughput limit could be exceeded depending on hello mix of read and write IOs.</span></span>

## <a name="new-deployments"></a><span data-ttu-id="e85c0-201">Nya distributioner</span><span class="sxs-lookup"><span data-stu-id="e85c0-201">New deployments</span></span>
<span data-ttu-id="e85c0-202">hello följande två avsnitt visar hur du kan distribuera virtuella SQL Server-datorer tooPremium lagring.</span><span class="sxs-lookup"><span data-stu-id="e85c0-202">hello next two sections demonstrate how you can deploy SQL Server VMs tooPremium Storage.</span></span> <span data-ttu-id="e85c0-203">Som nämnts så bör behöver du inte nödvändigtvis tooplace hello OS-disken till Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="e85c0-203">As mentioned before, you do not necessarily need tooplace hello OS disk onto Premium storage.</span></span> <span data-ttu-id="e85c0-204">Du kan välja toodo detta om du avsikten är tooplace alla i/o-arbetsbelastningar på hello OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="e85c0-204">You might choose toodo this if you are intending tooplace any intensive IO workloads on hello OS VHD.</span></span>

<span data-ttu-id="e85c0-205">hello första exemplet visar genom att använda befintliga avbildningar i Azure-galleriet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-205">hello first example demonstrates utilizing existing Azure Gallery Images.</span></span> <span data-ttu-id="e85c0-206">hello det andra exemplet visas hur toouse anpassade VM avbildning som du har i ett befintligt lagringskonto som Standard.</span><span class="sxs-lookup"><span data-stu-id="e85c0-206">hello second example shows how toouse a custom VM image that you have in an existing Standard storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="e85c0-207">Dessa exempel förutsätter att du redan har skapat ett regionalt VNET.</span><span class="sxs-lookup"><span data-stu-id="e85c0-207">These examples assume that you have already created a Regional VNET.</span></span>
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a><span data-ttu-id="e85c0-208">Skapa en ny virtuell dator med Premium-lagring med bild galleriet</span><span class="sxs-lookup"><span data-stu-id="e85c0-208">Create a new VM with Premium Storage with Gallery Image</span></span>
<span data-ttu-id="e85c0-209">hello exemplet nedan visar hur tooplace hello OS VHD till premium-lagring och bifoga Premium Storage virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-209">hello example below shows how tooplace hello OS VHD onto premium storage and attach Premium Storage VHDs.</span></span> <span data-ttu-id="e85c0-210">Du kan också placera hello OS-disken i ett standardlagringskonto och sedan koppla virtuella hårddiskar som finns i ett Premium Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e85c0-210">However, you can also place hello OS disk in a Standard Storage account and then attach VHDs that reside in a Premium Storage account.</span></span> <span data-ttu-id="e85c0-211">Båda scenarierna är visas.</span><span class="sxs-lookup"><span data-stu-id="e85c0-211">Both scenarios are demonstrated.</span></span>

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a><span data-ttu-id="e85c0-212">Steg 1: Skapa ett Premiumlagringskonto</span><span class="sxs-lookup"><span data-stu-id="e85c0-212">Step 1: Create a Premium Storage Account</span></span>
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a><span data-ttu-id="e85c0-213">Steg 2: Skapa en ny molntjänst</span><span class="sxs-lookup"><span data-stu-id="e85c0-213">Step 2: Create a New Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a><span data-ttu-id="e85c0-214">Steg 3: Reservera en Cloud Service VIP (valfritt)</span><span class="sxs-lookup"><span data-stu-id="e85c0-214">Step 3: Reserve a Cloud Service VIP (Optional)</span></span>
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a><span data-ttu-id="e85c0-215">Steg 4: Skapa en VM-behållare</span><span class="sxs-lookup"><span data-stu-id="e85c0-215">Step 4: Create a VM Container</span></span>
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a><span data-ttu-id="e85c0-216">Steg 5: Avyttring OS VHD Standard eller Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="e85c0-216">Step 5: Placing OS VHD on Standard or Premium Storage</span></span>
    #NOTE: Set up subscription and default storage account which will be used tooplace hello OS VHD in

    #If you want tooplace hello OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted tooplace hello OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a><span data-ttu-id="e85c0-217">Steg 6: Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e85c0-217">Step 6: Create VM</span></span>
    #Get list of available SQL Server Images from hello Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember toochange tooDS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks tooVM Config
    #Note hello size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising hello Premium Storage enabled Storage account

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


### <a name="create-a-new-vm-toouse-premium-storage-with-a-custom-image"></a><span data-ttu-id="e85c0-218">Skapa en ny VM toouse Premium-lagring med en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="e85c0-218">Create a new VM toouse Premium Storage with a custom image</span></span>
<span data-ttu-id="e85c0-219">Det här scenariot visar där du har befintliga anpassade avbildningar som finns i ett standardlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e85c0-219">This scenario demonstrates where you have existing customized images that reside in a Standard Storage account.</span></span> <span data-ttu-id="e85c0-220">Som tidigare nämnts om du vill tooplace hello OS VHD i Premium-lagring måste toocopy hello avbildning som finns i hello standardlagringskonto och överför dem tooa Premium-lagring innan den kan användas.</span><span class="sxs-lookup"><span data-stu-id="e85c0-220">As mentioned if you want tooplace hello OS VHD on Premium Storage you will need toocopy hello image that exists in hello Standard Storage account and transfer them tooa Premium Storage before it can be used.</span></span> <span data-ttu-id="e85c0-221">Om du har en bild på lokalt, kan du också använda den här metoden toocopy som direkt toohello Premium Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e85c0-221">If you have an image on-premises, you can also use this method toocopy that directly toohello Premium Storage account.</span></span>

#### <a name="step-1-create-storage-account"></a><span data-ttu-id="e85c0-222">Steg 1: Skapa Storage-konto</span><span class="sxs-lookup"><span data-stu-id="e85c0-222">Step 1: Create Storage Account</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a><span data-ttu-id="e85c0-223">Steg 2 Skapa molntjänst</span><span class="sxs-lookup"><span data-stu-id="e85c0-223">Step 2 Create Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a><span data-ttu-id="e85c0-224">Steg 3: Använd befintlig avbildning</span><span class="sxs-lookup"><span data-stu-id="e85c0-224">Step 3: Use existing image</span></span>
<span data-ttu-id="e85c0-225">Du kan använda en befintlig avbildning.</span><span class="sxs-lookup"><span data-stu-id="e85c0-225">You can use an existing image.</span></span> <span data-ttu-id="e85c0-226">Du kan [ta en bild av en befintlig dator](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e85c0-226">Or, you can [take an image of an existing machine](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="e85c0-227">Obs hello datorn du tar avbildningen har inte toobe DS * datorn.</span><span class="sxs-lookup"><span data-stu-id="e85c0-227">Note hello machine you image does not have toobe DS* machine.</span></span> <span data-ttu-id="e85c0-228">När du har hello avbildningen hello följande steg visar hur toocopy den toohello premiumlagringskonto med hello **Start AzureStorageBlobCopy** PowerShell-kommandot.</span><span class="sxs-lookup"><span data-stu-id="e85c0-228">Once you have hello image, hello following steps show how toocopy it toohello Premium Storage account with hello **Start-AzureStorageBlobCopy** PowerShell commandlet.</span></span>

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for hello storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a><span data-ttu-id="e85c0-229">Steg 4: Kopiera Blob mellan Storage-konton</span><span class="sxs-lookup"><span data-stu-id="e85c0-229">Step 4: Copy Blob between Storage Accounts</span></span>
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a><span data-ttu-id="e85c0-230">Steg 5: Kontrollera regelbundet-kopian status:</span><span class="sxs-lookup"><span data-stu-id="e85c0-230">Step 5: Regularly check copy status:</span></span>
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-tooazure-disk-repository-in-subscription"></a><span data-ttu-id="e85c0-231">Steg 6: Lägg till avbildning disk tooAzure disk databasen i prenumerationen</span><span class="sxs-lookup"><span data-stu-id="e85c0-231">Step 6: Add Image disk tooAzure disk Repository in Subscription</span></span>
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> <span data-ttu-id="e85c0-232">Du kanske att även om hello statusrapporter om åtgärden lyckades, du kan fortfarande få ett lån diskfel.</span><span class="sxs-lookup"><span data-stu-id="e85c0-232">You may find that even though hello status reports as success, you could still get a disk lease error.</span></span> <span data-ttu-id="e85c0-233">I så fall väntar du cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="e85c0-233">In this case, wait about 10 minutes.</span></span>
>
>

#### <a name="step-7--build-hello-vm"></a><span data-ttu-id="e85c0-234">Steg 7: Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="e85c0-234">Step 7:  Build hello VM</span></span>
<span data-ttu-id="e85c0-235">Här bygger du hello VM från avbildningen och ansluta två Premium Storage virtuella hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="e85c0-235">Here you are building hello VM from your image and attaching two Premium Storage VHDs:</span></span>

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need toobe a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use tooDS Series VM
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

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a><span data-ttu-id="e85c0-236">Befintliga distributioner som inte använder alltid på Tillgänglighetsgrupper</span><span class="sxs-lookup"><span data-stu-id="e85c0-236">Existing deployments that do not use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="e85c0-237">Befintliga distributioner först finns hello [krav](#prerequisites-for-premium-storage) i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-237">For existing deployments, first see hello [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="e85c0-238">Det finns olika överväganden för SQL Server-distributioner som inte använder alltid på Tillgänglighetsgrupper och det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="e85c0-238">There are different considerations for SQL Server deployments that do not use Always On Availability Groups and those that do.</span></span> <span data-ttu-id="e85c0-239">Om du inte använder alltid på och har en befintlig fristående SQL Server kan du uppgradera tooPremium lagring med hjälp av ett nytt cloud service och storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e85c0-239">If you are not using Always On and have an existing standalone SQL Server, you can upgrade tooPremium Storage by using a new cloud service and storage account.</span></span> <span data-ttu-id="e85c0-240">Tänk hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="e85c0-240">Consider hello following options:</span></span>

* <span data-ttu-id="e85c0-241">**Skapa en ny SQL Server-VM**.</span><span class="sxs-lookup"><span data-stu-id="e85c0-241">**Create a new SQL Server VM**.</span></span> <span data-ttu-id="e85c0-242">Du kan skapa en ny SQL Server-VM som använder ett premiumlagringskonto, enligt beskrivningen i nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="e85c0-242">You can create a new SQL Server VM that uses a Premium Storage account, as documented in New Deployments.</span></span> <span data-ttu-id="e85c0-243">Sedan säkerhetskopiera och återställa SQL Server-databaserna konfiguration och användare.</span><span class="sxs-lookup"><span data-stu-id="e85c0-243">Then backup and restore your SQL Server configuration and user databases.</span></span> <span data-ttu-id="e85c0-244">hello program behöver uppdateras toobe tooreference hello nya SQL-servern om den används internt eller externt.</span><span class="sxs-lookup"><span data-stu-id="e85c0-244">hello application will need toobe updated tooreference hello new SQL Server if it is being accessed internally or externally.</span></span> <span data-ttu-id="e85c0-245">Behöver du toocopy alla out-of-db-objekt som om du höll på en sida vid sida (SxS) SQL Server-migrering.</span><span class="sxs-lookup"><span data-stu-id="e85c0-245">You would need toocopy all ‘out of db’ objects as if you were doing a Side by Side (SxS) SQL Server migration.</span></span> <span data-ttu-id="e85c0-246">Detta inkluderar objekt som inloggningar, certifikat och länkade servrar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-246">This includes objects such as logins, certificates, and linked servers.</span></span>
* <span data-ttu-id="e85c0-247">**Migrera en befintlig SQL Server-VM**.</span><span class="sxs-lookup"><span data-stu-id="e85c0-247">**Migrate an existing SQL Server VM**.</span></span> <span data-ttu-id="e85c0-248">Detta kräver att hello SQL Server-VM tas offline och sedan överföra den tooa nya Molntjänsten, som innehåller kopierar alla dess anslutna virtuella hårddiskar toohello Premium Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e85c0-248">This will require taking hello SQL Server VM offline, then transferring it tooa new cloud service, which includes copying all of its attached VHDs toohello Premium Storage account.</span></span> <span data-ttu-id="e85c0-249">När hello VM är online kommer programmet hello referera hello värdnamn för server som innan.</span><span class="sxs-lookup"><span data-stu-id="e85c0-249">When hello VM comes online, hello application will reference hello server host name as before.</span></span> <span data-ttu-id="e85c0-250">Tänk på att hello hello befintliga diskens storlek påverkas hello prestandaegenskaper.</span><span class="sxs-lookup"><span data-stu-id="e85c0-250">Be aware that hello size of hello existing disk will affect hello performance characteristics.</span></span> <span data-ttu-id="e85c0-251">En 400 GB disk hämtar avrundat tooa P20.</span><span class="sxs-lookup"><span data-stu-id="e85c0-251">For example, a 400 GB disk gets rounded up tooa P20.</span></span> <span data-ttu-id="e85c0-252">Om du vet att du inte behöver som diskprestanda, kan du återskapa hello VM som virtuell dator DS-serien och bifoga Premium Storage virtuella hårddiskar på hello storlek och prestanda-specifikation som du behöver.</span><span class="sxs-lookup"><span data-stu-id="e85c0-252">If you know that you do not require that disk performance, then you could recreate hello VM as a DS Series VM, and attach Premium Storage VHDs of hello size/performance specification you require.</span></span> <span data-ttu-id="e85c0-253">Sedan kan du koppla från och Återanslut hello SQL DB-filer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-253">Then you could detach and reattach hello SQL DB files.</span></span>

> [!NOTE]
> <span data-ttu-id="e85c0-254">När kopierar hello VHD-diskar som du bör vara medveten om hello storlek, beroende på hello storlek innebär vilka Premium Storage disktyp de hör till, detta avgör disk prestanda-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-254">When copying hello VHD disks you should be aware of hello size, depending on hello size will mean what Premium Storage Disk type they fall into, this determines disk performance specification.</span></span> <span data-ttu-id="e85c0-255">Kommer Azure att avrunda uppåt toohello närmsta disk storlek, så om du har en 400 GB disk, detta kommer att avrundas uppåt tooa P20.</span><span class="sxs-lookup"><span data-stu-id="e85c0-255">Azure will round up toohello nearest disk size, so if you have a 400GB disk, this will be rounded up tooa P20.</span></span> <span data-ttu-id="e85c0-256">Beroende på din befintliga i/o-kraven i hello OS-VHD, kanske du inte behöver toomigrate denna tooa Premium Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e85c0-256">Depending on your existing IO requirements of hello OS VHD, you might not need toomigrate this tooa Premium Storage account.</span></span>
>
>

<span data-ttu-id="e85c0-257">Om din SQL Server används externt ändras hello cloud service VIP.</span><span class="sxs-lookup"><span data-stu-id="e85c0-257">If your SQL Server is accessed externally, then hello cloud service VIP will change.</span></span> <span data-ttu-id="e85c0-258">Du har också tooupdate slutpunkter, ACL: er och DNS-inställningar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-258">You will also have tooupdate end points, ACLs, and DNS settings.</span></span>

## <a name="existing-deployments-that-use-always-on-availability-groups"></a><span data-ttu-id="e85c0-259">Befintliga distributioner som använder alltid på Tillgänglighetsgrupper</span><span class="sxs-lookup"><span data-stu-id="e85c0-259">Existing deployments that use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="e85c0-260">Befintliga distributioner först finns hello [krav](#prerequisites-for-premium-storage) i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-260">For existing deployments, first see hello [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="e85c0-261">Först ska vi titta på hur alltid på samverkar med Azure-nätverk i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-261">Initially in this section we will look at how Always On interacts with Azure Networking.</span></span> <span data-ttu-id="e85c0-262">Vi kommer sedan att dela upp migreringar i tootwo scenarier: där vissa avbrott kan tillåtas migreringar och migreringar där du måste uppnå minimal avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="e85c0-262">We will then break down migrations in tootwo scenarios: migrations where some downtime can be tolerated and migrations where you must achieve minimal downtime.</span></span>

<span data-ttu-id="e85c0-263">Lokal SQL Server alltid på Tillgänglighetsgrupper använder en lyssnare lokalt som registrerar ett virtuella DNS-namn tillsammans med en IP-adress som delas mellan en eller flera SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-263">On-premises SQL Server Always On Availability Groups use a Listener on-premises that registers a virtual DNS name along with an IP address that is shared between one or more SQL Servers.</span></span> <span data-ttu-id="e85c0-264">När klienter ansluter dirigeras de via hello lyssnare IP toohello primära SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="e85c0-264">When clients connect they are routed through hello listener IP toohello Primary SQL Server.</span></span> <span data-ttu-id="e85c0-265">Detta är hello-servern som äger hello alltid på IP-resurs som för närvarande.</span><span class="sxs-lookup"><span data-stu-id="e85c0-265">This is hello server that owns hello Always On IP resource at that time.</span></span>

![DeploymentsUseAlways på][6]

<span data-ttu-id="e85c0-267">I Microsoft Azure kan du ha endast en IP-adress som tilldelats tooa NIC på hello VM, i ordning tooachieve Hej samma lager Abstraktionslager som lokalt, Azure använder hello IP-adress som är tilldelad toohello intern/extern belastningsutjämnare (ILB/ELB).</span><span class="sxs-lookup"><span data-stu-id="e85c0-267">In Microsoft Azure you can have only one IP address assigned tooa NIC on hello VM, so in order tooachieve hello same layer of abstraction as on-premises, Azure utilizes hello IP address that is assigned toohello Internal/External Load Balancers (ILB/ELB).</span></span> <span data-ttu-id="e85c0-268">hello IP-resurs som delas mellan hello servrar anges toohello samma IP-adress som hello ILB/ELB.</span><span class="sxs-lookup"><span data-stu-id="e85c0-268">hello IP resource that is shared between hello servers is set toohello same IP as hello ILB/ELB.</span></span> <span data-ttu-id="e85c0-269">Det har publicerats i hello DNS och klienttrafik överförs via hello ILB/ELB toohello primära SQL Server-repliken.</span><span class="sxs-lookup"><span data-stu-id="e85c0-269">This is published in hello DNS, and client traffic is passed through hello ILB/ELB toohello Primary SQL Server replica.</span></span> <span data-ttu-id="e85c0-270">Hej ILB/ELB vet vilken SQL Server är primär eftersom den använder avsökningar tooprobe hello alltid på IP-resurs.</span><span class="sxs-lookup"><span data-stu-id="e85c0-270">hello ILB/ELB knows which SQL Server is primary since it uses probes tooprobe hello Always On IP resource.</span></span> <span data-ttu-id="e85c0-271">I föregående exempel hello den avsökningar varje nod som har en slutpunkt som refereras av hello ELB/ILB, beroende på vilket som svarar är hello primära SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="e85c0-271">In hello previous example, it probes each node that has an endpoint referenced by hello ELB/ILB, whichever responds is hello Primary SQL Server.</span></span>

> [!NOTE]
> <span data-ttu-id="e85c0-272">hello ILB och ELB båda tilldelas tooa viss Azure cloud service, alla molnmigrering i Azure innebär därför troligen att hello Load Balancer IP ändras.</span><span class="sxs-lookup"><span data-stu-id="e85c0-272">hello ILB and ELB are both assigned tooa particular Azure cloud service, therefore any cloud migration in Azure will most likely mean that hello Load Balancer IP will change.</span></span>
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a><span data-ttu-id="e85c0-273">Migrera alltid på distributioner som kan tillåta att vissa avbrott</span><span class="sxs-lookup"><span data-stu-id="e85c0-273">Migrating Always On deployments that can allow some downtime</span></span>
<span data-ttu-id="e85c0-274">Det finns två strategier toomigrate alltid distributioner så att vissa avbrott:</span><span class="sxs-lookup"><span data-stu-id="e85c0-274">There are two strategies toomigrate Always On deployments that allow for some downtime:</span></span>

1. <span data-ttu-id="e85c0-275">**Lägga till flera sekundära repliker tooan befintliga alltid på klustret**</span><span class="sxs-lookup"><span data-stu-id="e85c0-275">**Add more secondary replicas tooan existing Always On Cluster**</span></span>
2. <span data-ttu-id="e85c0-276">**Migrera tooa nya alltid på klustret**</span><span class="sxs-lookup"><span data-stu-id="e85c0-276">**Migrate tooa new Always On Cluster**</span></span>

#### <a name="1-add-more-secondary-replicas-tooan-existing-always-on-cluster"></a><span data-ttu-id="e85c0-277">1. Lägga till flera sekundära repliker tooan befintliga alltid på klustret</span><span class="sxs-lookup"><span data-stu-id="e85c0-277">1. Add more Secondary Replicas tooan Existing Always On Cluster</span></span>
<span data-ttu-id="e85c0-278">En strategi är tooadd toohello mer sekundärservrar Always On-Tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-278">One strategy is tooadd more secondaries toohello Always On Availability Group.</span></span> <span data-ttu-id="e85c0-279">Du behöver tooadd dem i en ny molntjänst och uppdatera hello lyssnare med hello nya belastningen belastningsutjämnaren IP.</span><span class="sxs-lookup"><span data-stu-id="e85c0-279">You need tooadd these into a new cloud service and update hello listener with hello new load balancer IP.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="e85c0-280">Punkter stillestånd:</span><span class="sxs-lookup"><span data-stu-id="e85c0-280">Points of downtime:</span></span>
* <span data-ttu-id="e85c0-281">Klusterverifieringen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-281">Cluster Validation.</span></span>
* <span data-ttu-id="e85c0-282">Testa alltid på redundans för nya sekundärservrar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-282">Testing Always On failovers for New Secondaries.</span></span>

<span data-ttu-id="e85c0-283">Om du använder Windows lagringspooler inom hello VM för högre i/o-genomflöde kommer sedan dessa att kopplas från under en fullständig verifiering av klustret.</span><span class="sxs-lookup"><span data-stu-id="e85c0-283">If you are using Windows Storage Pools within hello VM for higher IO throughput, then these will be taken offline during a Full Cluster Validation.</span></span> <span data-ttu-id="e85c0-284">hello verifieringstest krävs när du lägger till noder toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="e85c0-284">hello validation test is required when you add nodes toohello cluster.</span></span> <span data-ttu-id="e85c0-285">hello tid det tar toorun hello test kan variera, så bör du testa detta i din miljö representativt test-tooget en ungefärlig tid för hur lång tid detta tar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-285">hello time it takes toorun hello test can vary, so you should test this in your representative test environment tooget an approximate time of how long this will take.</span></span>

<span data-ttu-id="e85c0-286">Du måste etablera tid där du kan utföra manuell växling vid fel och chaos tester på hello nyligen lagt till noder tooensure alltid på hög tillgänglighet funktioner som förväntat.</span><span class="sxs-lookup"><span data-stu-id="e85c0-286">You should provision time where you can perform manual failover and chaos testing on hello newly added nodes tooensure Always On High Availability functions as expected.</span></span>

![DeploymentUseAlways On2][7]

> [!NOTE]
> <span data-ttu-id="e85c0-288">Du bör avsluta alla instanser av SQL Server där hello lagringspooler används innan hello validering körs.</span><span class="sxs-lookup"><span data-stu-id="e85c0-288">You should stop all instances of SQL Server where hello Storage Pools are used before hello Validation runs.</span></span>
>
> ##### <a name="high-level-steps"></a><span data-ttu-id="e85c0-289">Anvisningar</span><span class="sxs-lookup"><span data-stu-id="e85c0-289">High-level steps</span></span>
>

1. <span data-ttu-id="e85c0-290">Skapa två nya SQL-servrar i ny molntjänst med anslutna Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="e85c0-290">Create two new SQL Servers in new cloud service with attached Premium Storage.</span></span>
2. <span data-ttu-id="e85c0-291">Kopiera över fullständig säkerhetskopiering och återställning med **NORECOVERY**.</span><span class="sxs-lookup"><span data-stu-id="e85c0-291">Copy over FULL backups and restore with **NORECOVERY**.</span></span>
3. <span data-ttu-id="e85c0-292">Kopiera över 'utanför user DB' beroende objekt, till exempel inloggningar osv.</span><span class="sxs-lookup"><span data-stu-id="e85c0-292">Copy over ‘out of user DB’ dependent objects, such as logins etc.</span></span>
4. <span data-ttu-id="e85c0-293">Skapa en ny intern belastning belastningsutjämnare (ILB) eller använda en extern belastningen belastningsutjämnare (ELB) och sedan ställa in belastningen belastningsutjämnade slutpunkter på både nya noder.</span><span class="sxs-lookup"><span data-stu-id="e85c0-293">Create new a new Internal Load Balancer (ILB) or use an External Load Balancer (ELB), and then set up Load Balanced Endpoints on both new nodes.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e85c0-294">Kontrollera att alla noder har hello rätt slutpunktskonfiguration innan du fortsätter</span><span class="sxs-lookup"><span data-stu-id="e85c0-294">Check all Nodes have hello correct Endpoint configuration before you continue</span></span>
   >
   >
5. <span data-ttu-id="e85c0-295">Stoppa användare/programåtkomst toohello SQL Server (om du använder lagringspooler).</span><span class="sxs-lookup"><span data-stu-id="e85c0-295">Stop User/Application Access toohello SQL Server (if using Storage Pools).</span></span>
6. <span data-ttu-id="e85c0-296">Stoppa SQL Server Engine-tjänster på alla noder (om du använder lagringspooler).</span><span class="sxs-lookup"><span data-stu-id="e85c0-296">Stop SQL Server Engine Services on All Nodes (if using Storage Pools).</span></span>
7. <span data-ttu-id="e85c0-297">Lägg till nya noder toocluster och kör fullständig verifiering.</span><span class="sxs-lookup"><span data-stu-id="e85c0-297">Add new Nodes toocluster and run full validation.</span></span>
8. <span data-ttu-id="e85c0-298">När verifieringen är klar startar du alla SQL Server-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e85c0-298">Once Validation is successful, start all SQL Server Services.</span></span>
9. <span data-ttu-id="e85c0-299">Säkerhetskopiera transaktionsloggar och återställa databaserna.</span><span class="sxs-lookup"><span data-stu-id="e85c0-299">Backup Transaction logs, and restore user databases.</span></span>
10. <span data-ttu-id="e85c0-300">Lägg till nya noder i hello alltid på Tillgänglighetsgruppen och placera replikering till **synkron**.</span><span class="sxs-lookup"><span data-stu-id="e85c0-300">Add new nodes into hello Always On Availability Group and place replication into **Synchronous**.</span></span>
11. <span data-ttu-id="e85c0-301">Lägg till hello IP-adressresurs av hello nya Cloud Service ILB/ELB via PowerShell för Always On baserat på hello flera platser exemplet i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="e85c0-301">Add hello IP address resource of hello new Cloud Service ILB/ELB through PowerShell for Always On based on hello Multi-site example in hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span> <span data-ttu-id="e85c0-302">Ange hello i Windows-kluster **möjliga ägare** av hello **IP-adress** resurs toohello nya noder gamla.</span><span class="sxs-lookup"><span data-stu-id="e85c0-302">In Windows clustering, set hello **Possible owners** of hello **IP Address** resource toohello new nodes old.</span></span> <span data-ttu-id="e85c0-303">Avsnittet hello ”lägga till IP-adressresurs i samma undernät' i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="e85c0-303">See hello ‘Adding IP Address Resource on Same Subnet’ section of hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
12. <span data-ttu-id="e85c0-304">Redundans tooone hello nya noder.</span><span class="sxs-lookup"><span data-stu-id="e85c0-304">Failover tooone of hello new nodes.</span></span>
13. <span data-ttu-id="e85c0-305">Se hello nya noder automatiskt Redundanspartners och redundanstestningen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-305">Make hello new nodes Auto Failover Partners and test failovers.</span></span>
14. <span data-ttu-id="e85c0-306">Ta bort ursprungliga noder från Tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-306">Remove original nodes from Availability Group.</span></span>

##### <a name="advantages"></a><span data-ttu-id="e85c0-307">Fördelar</span><span class="sxs-lookup"><span data-stu-id="e85c0-307">Advantages</span></span>
* <span data-ttu-id="e85c0-308">Ny SQL-servrar kan vara testas (SQL Server och program) innan de läggs tooAlways på.</span><span class="sxs-lookup"><span data-stu-id="e85c0-308">New SQL Servers can be tested (SQL Server and Application) before they are added tooAlways On.</span></span>
* <span data-ttu-id="e85c0-309">Du kan ändra hello VM-storlek och anpassa hello tooyour exakt lagringsbehov.</span><span class="sxs-lookup"><span data-stu-id="e85c0-309">You can change hello VM size and customize hello storage tooyour exact requirements.</span></span> <span data-ttu-id="e85c0-310">Det kan dock vara bra tookeep alla hello SQL sökvägar hello samma.</span><span class="sxs-lookup"><span data-stu-id="e85c0-310">However, it would be beneficial tookeep all hello SQL file paths hello same.</span></span>
* <span data-ttu-id="e85c0-311">Du kan styra när hello överföringen av hello DB säkerhetskopieringar toohello sekundära repliker har startats.</span><span class="sxs-lookup"><span data-stu-id="e85c0-311">You can control when hello transfer of hello DB backups toohello Secondary Replicas are started.</span></span> <span data-ttu-id="e85c0-312">Detta skiljer sig från att använda Azure **Start AzureStorageBlobCopy** kommandot toocopy virtuella hårddiskar, eftersom det är en asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="e85c0-312">This differs from using Azure **Start-AzureStorageBlobCopy** commandlet toocopy VHDs, because that is an asynchronous copy.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="e85c0-313">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="e85c0-313">Disadvantages</span></span>
* <span data-ttu-id="e85c0-314">När du använder Windows-lagringspooler finns klusterdriftstopp under hello fullständig Klusterverifieringen för hello nya ytterligare noder.</span><span class="sxs-lookup"><span data-stu-id="e85c0-314">When using Windows Storage Pools, there is Cluster downtime during hello Full Cluster Validation for hello new additional nodes.</span></span>
* <span data-ttu-id="e85c0-315">Beroende på hello SQL Server-Version och hello befintliga antalet sekundära repliker, kan inte vara kan tooadd flera sekundära repliker utan att ta bort befintliga sekundärservrar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-315">Depending on hello SQL Server Version and hello existing number of secondary replicas, you might not be able tooadd more secondary replicas without removing existing secondaries.</span></span>
* <span data-ttu-id="e85c0-316">Det kan finnas långa SQL dataöverföringstid när du konfigurerar hello sekundärservrar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-316">There could be long SQL data transfer time while setting up hello secondaries.</span></span>
* <span data-ttu-id="e85c0-317">Det finns ytterligare kostnad under migreringen när du har nya datorer som körs parallellt.</span><span class="sxs-lookup"><span data-stu-id="e85c0-317">There is additional cost during migration while you have new machines running in parallel.</span></span>

#### <a name="2-migrate-tooa-new-always-on-cluster"></a><span data-ttu-id="e85c0-318">2. Migrera tooa nya alltid på klustret</span><span class="sxs-lookup"><span data-stu-id="e85c0-318">2. Migrate tooa new Always On Cluster</span></span>
<span data-ttu-id="e85c0-319">En annan strategi är toocreate en helt ny alltid på klustret med helt nya noder i ny molntjänst och omdirigering hello klienter toouse den.</span><span class="sxs-lookup"><span data-stu-id="e85c0-319">Another strategy is toocreate a brand new Always On Cluster with brand new nodes in new cloud service and then redirect hello clients toouse it.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="e85c0-320">Punkter stillestånd</span><span class="sxs-lookup"><span data-stu-id="e85c0-320">Points of downtime</span></span>
<span data-ttu-id="e85c0-321">Det finns en avbrottstid när du överför program och användare toohello alltid på lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="e85c0-321">There is downtime when you transfer applications and users toohello new Always On listener.</span></span> <span data-ttu-id="e85c0-322">hello driftstopp beror på:</span><span class="sxs-lookup"><span data-stu-id="e85c0-322">hello downtime depends on:</span></span>

* <span data-ttu-id="e85c0-323">hello tidsåtgång toorestore slutliga transaction log säkerhetskopieringar toodatabases på nya servrar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-323">hello time taken toorestore final transaction log backups toodatabases on new servers.</span></span>
* <span data-ttu-id="e85c0-324">hello tidsåtgång tooupdate klienten program toouse alltid på lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="e85c0-324">hello time taken tooupdate client applications toouse new Always On listener.</span></span>

##### <a name="advantages"></a><span data-ttu-id="e85c0-325">Fördelar</span><span class="sxs-lookup"><span data-stu-id="e85c0-325">Advantages</span></span>
* <span data-ttu-id="e85c0-326">Du kan testa hello faktiska produktionsmiljön, SQL Server och Operativsystemets build-ändringar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-326">You can test hello actual production environment, SQL Server, and OS build changes.</span></span>
* <span data-ttu-id="e85c0-327">Du har hello alternativet toocustomize hello lagring och toopotentially minska storleken för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e85c0-327">You have hello option toocustomize hello storage and toopotentially reduce size of VM.</span></span> <span data-ttu-id="e85c0-328">Det kan leda till minskad kostnaden.</span><span class="sxs-lookup"><span data-stu-id="e85c0-328">This could result in cost reduction.</span></span>
* <span data-ttu-id="e85c0-329">Under den här processen kan du uppdatera din version eller SQL Server-version.</span><span class="sxs-lookup"><span data-stu-id="e85c0-329">You can update your SQL Server build or version during this process.</span></span> <span data-ttu-id="e85c0-330">Du kan också uppgradera hello operativsystem.</span><span class="sxs-lookup"><span data-stu-id="e85c0-330">You can also upgrade hello Operating System.</span></span>
* <span data-ttu-id="e85c0-331">hello tidigare alltid på kluster kan fungera som ett fast rollback-mål.</span><span class="sxs-lookup"><span data-stu-id="e85c0-331">hello previous Always On Cluster can act as a solid rollback target.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="e85c0-332">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="e85c0-332">Disadvantages</span></span>
* <span data-ttu-id="e85c0-333">Du måste toochange hello DNS-namnet på hello lyssnare om du vill att båda Always On-kluster körs samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e85c0-333">You need toochange hello DNS name of hello listener if you want both Always On clusters running simultaneously.</span></span> <span data-ttu-id="e85c0-334">Detta lägger till administration försämras under migreringen hello som klienten programmet strängar måste återspeglar hello ny lyssnare namn.</span><span class="sxs-lookup"><span data-stu-id="e85c0-334">This adds administration overhead during hello migration as client application strings must reflect hello new Listener name.</span></span>
* <span data-ttu-id="e85c0-335">Du måste implementera en synkroniseringsmekanism för mellan hello två miljöer tookeep dem som nära som möjligt toominimize hello slutlig synkroniseringskrav innan migreringen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-335">You must implement a synchronization mechanism between hello two environments tookeep them as close as possible toominimize hello final synchronization requirements before migration.</span></span>
* <span data-ttu-id="e85c0-336">Det läggs till kostnad under migreringen när du har hello nya miljö körs.</span><span class="sxs-lookup"><span data-stu-id="e85c0-336">There is added cost during migration while you have hello new environment running.</span></span>

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a><span data-ttu-id="e85c0-337">Migrera alltid på distributioner för minimal driftstörning</span><span class="sxs-lookup"><span data-stu-id="e85c0-337">Migrating Always On Deployments for minimal downtime</span></span>
<span data-ttu-id="e85c0-338">Det finns två metoder för att migrera Always On-distributioner för minimal driftstörning:</span><span class="sxs-lookup"><span data-stu-id="e85c0-338">There are two strategies for migrating Always On deployments for minimal downtime:</span></span>

1. <span data-ttu-id="e85c0-339">**Använda en befintlig sekundär: enskild plats**</span><span class="sxs-lookup"><span data-stu-id="e85c0-339">**Utilize an Existing Secondary: Single-Site**</span></span>
2. <span data-ttu-id="e85c0-340">**Använda befintlig sekundär tillgänglighetsreplik(er): flera platser**</span><span class="sxs-lookup"><span data-stu-id="e85c0-340">**Utilize Existing Secondary Replica(s): Multi-Site**</span></span>

#### <a name="1-utilize-an-existing-secondary-single-site"></a><span data-ttu-id="e85c0-341">1. Använda en befintlig sekundär: enskild plats</span><span class="sxs-lookup"><span data-stu-id="e85c0-341">1. Utilize an existing secondary: Single-Site</span></span>
<span data-ttu-id="e85c0-342">En strategi för minimal driftstörning är tootake ett befintligt moln sekundära och ta bort den från hello aktuella tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-342">One strategy for minimal downtime is tootake an existing cloud secondary and remove it from hello current cloud service.</span></span> <span data-ttu-id="e85c0-343">Kopiera hello virtuella hårddiskar toohello nya Premium Storage-konto och skapa hello VM i hello ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="e85c0-343">Then copy hello VHDs toohello new Premium Storage account, and create hello VM in hello new cloud service.</span></span> <span data-ttu-id="e85c0-344">Uppdatera hello lyssnare i kluster och växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="e85c0-344">Then update hello listener in clustering and failover.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="e85c0-345">Punkter stillestånd</span><span class="sxs-lookup"><span data-stu-id="e85c0-345">Points of downtime</span></span>
* <span data-ttu-id="e85c0-346">Det finns en avbrottstid när du uppdaterar hello sista noden med hello belastningsutjämnade slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e85c0-346">There is downtime when you update hello final node with hello Load Balanced endpoint.</span></span>
* <span data-ttu-id="e85c0-347">Klient-återanslutning kan fördröjas beroende på din klient/DNS-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e85c0-347">Your client reconnection might be delayed depending on your client/DNS configuration.</span></span>
* <span data-ttu-id="e85c0-348">Det finns ytterligare driftstopp om du väljer tootake hello alltid på klustret grupp offline tooswap ut hello IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="e85c0-348">There is additional downtime if you choose tootake hello Always On Cluster group offline tooswap out hello IP addresses.</span></span> <span data-ttu-id="e85c0-349">Du kan undvika detta genom att använda ett beroende eller och möjliga ägare för hello läggs IP-adressresurs.</span><span class="sxs-lookup"><span data-stu-id="e85c0-349">You can avoid this by using an OR dependency and Possible Owners for hello added IP Address resource.</span></span> <span data-ttu-id="e85c0-350">Avsnittet hello ”lägga till IP-adressresurs i samma undernät' i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="e85c0-350">See hello ‘Adding IP Address Resource on Same Subnet’ section of hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>

> [!NOTE]
> <span data-ttu-id="e85c0-351">När du vill hello tillagda noden toopartake i som alltid på Failover-Partner, måste tooadd en Azure-slutpunkt med en referens toohello belastningen belastningsutjämnade anges.</span><span class="sxs-lookup"><span data-stu-id="e85c0-351">When you want hello added node toopartake in as an Always On Failover Partner, you need tooadd an Azure Endpoint with a reference toohello Load Balanced Set.</span></span> <span data-ttu-id="e85c0-352">När du kör hello **Lägg till AzureEndpoint** kommandot toodo detta, öppna tooremain i aktuella anslutningar, men anslutningar toohello lyssnaren inte kan toobe upprätta förrän hello belastningsutjämnaren har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="e85c0-352">When you run hello **Add-AzureEndpoint** command toodo this, current connections tooremain open, but new connections toohello listener will not be able toobe established until hello load balancer has updated.</span></span> <span data-ttu-id="e85c0-353">Testning av detta visas toolast 90-120seconds, detta bör testas.</span><span class="sxs-lookup"><span data-stu-id="e85c0-353">In testing this was seen toolast 90-120seconds, this should be tested.</span></span>
>
>

##### <a name="advantages"></a><span data-ttu-id="e85c0-354">Fördelar</span><span class="sxs-lookup"><span data-stu-id="e85c0-354">Advantages</span></span>
* <span data-ttu-id="e85c0-355">Inga extra kostnaden under migreringen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-355">No extra cost incurred during migration.</span></span>
* <span data-ttu-id="e85c0-356">En-till-en migrering.</span><span class="sxs-lookup"><span data-stu-id="e85c0-356">A one-to-one migration.</span></span>
* <span data-ttu-id="e85c0-357">Minskad komplexitet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-357">Reduced complexity.</span></span>
* <span data-ttu-id="e85c0-358">Ger ökad IOPS från Premium Storage SKU: er.</span><span class="sxs-lookup"><span data-stu-id="e85c0-358">Allows for increased IOPS from Premium Storage SKUs.</span></span> <span data-ttu-id="e85c0-359">När hello diskar har kopplats från hello VM och kopierade toohello ny molntjänst en 3 part att verktyget använda tooincrease hello VHD storlek, vilket ger högre genomflöden.</span><span class="sxs-lookup"><span data-stu-id="e85c0-359">When hello disks are detached from hello VM and copied toohello new cloud service, a 3rd party tool can be used tooincrease hello VHD size, which provides higher throughputs.</span></span> <span data-ttu-id="e85c0-360">Öka storleken på VHD finns i följande [forumdiskussion](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span><span class="sxs-lookup"><span data-stu-id="e85c0-360">For increasing VHD sizes, see this [forum discussion](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="e85c0-361">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="e85c0-361">Disadvantages</span></span>
* <span data-ttu-id="e85c0-362">Det finns en temporär förlust av hög tillgänglighet och Katastrofåterställning under migreringen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-362">There is a temporary loss of HA and DR during migration.</span></span>
* <span data-ttu-id="e85c0-363">Eftersom det är en 1:1-migrering måste toouse en minsta VM-storlek som stöder antal virtuella hårddiskar, så du inte kanske kan toodownsize dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-363">As this is a 1:1 migration, you will have toouse a minimum VM size that will support your number of VHDs, so you might not be able toodownsize your VMs.</span></span>
* <span data-ttu-id="e85c0-364">Det här scenariot använder hello Azure **Start AzureStorageBlobCopy** cmdlet, som är asynkron.</span><span class="sxs-lookup"><span data-stu-id="e85c0-364">This scenario would use hello Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="e85c0-365">Det finns inga SLA på Kopiera slutförande.</span><span class="sxs-lookup"><span data-stu-id="e85c0-365">There is no SLA on copy completion.</span></span> <span data-ttu-id="e85c0-366">hello tid hello kopior varierar, medan det beror på vänta i kön beror också på hello mängd data tootransfer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-366">hello time of hello copies varies, while this depends on wait in queue it will also depend on hello amount of data tootransfer.</span></span> <span data-ttu-id="e85c0-367">hello kopiera tid ökar om hello överföring ska tooanother Azure-datacenter som har stöd för Premium-lagring i en annan region.</span><span class="sxs-lookup"><span data-stu-id="e85c0-367">hello copy time increases if hello transfer is going tooanother Azure data center that supports Premium Storage in another region.</span></span> <span data-ttu-id="e85c0-368">Om du har precis 2 noder kan du en möjlig lösning om hello kopiera tar längre tid än vid testning.</span><span class="sxs-lookup"><span data-stu-id="e85c0-368">If you just have 2 nodes, consider a possible mitigation in case hello copy takes longer than in testing.</span></span> <span data-ttu-id="e85c0-369">Detta kan omfatta hello följande idéer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-369">This could include hello following ideas.</span></span>
  * <span data-ttu-id="e85c0-370">Lägga till en tillfällig 3 SQL Server-nod för hög tillgänglighet innan hello migreringen överenskomna avbrott.</span><span class="sxs-lookup"><span data-stu-id="e85c0-370">Add a temporary 3rd SQL Server node for HA before hello migration with agreed downtime.</span></span>
  * <span data-ttu-id="e85c0-371">Kör hello migrering utanför Azure schemalagt underhåll.</span><span class="sxs-lookup"><span data-stu-id="e85c0-371">Run hello migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="e85c0-372">Kontrollera att du har konfigurerat klustrets kvorum korrekt.</span><span class="sxs-lookup"><span data-stu-id="e85c0-372">Ensure you have configured your cluster quorum correctly.</span></span>  

##### <a name="high-level-steps"></a><span data-ttu-id="e85c0-373">Anvisningar</span><span class="sxs-lookup"><span data-stu-id="e85c0-373">High-level steps</span></span>
<span data-ttu-id="e85c0-374">Det här dokumentet visar inte en fullständig slutet tooend exempelvis men hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) innehåller information som kan vara balanserad tooperform detta.</span><span class="sxs-lookup"><span data-stu-id="e85c0-374">This document does not demonstrate a complete end tooend example, however hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) provides details that can be leveraged tooperform this.</span></span>

![MinimalDowntime][8]

* <span data-ttu-id="e85c0-376">Samla diskkonfigurationen och ta bort hello nod (ta inte bort anslutna virtuella hårddiskar).</span><span class="sxs-lookup"><span data-stu-id="e85c0-376">Gather disk configuration, and remove hello node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="e85c0-377">Skapa ett premiumlagringskonto-lagring och kopiera virtuella hårddiskar från hello standardlagringskonto</span><span class="sxs-lookup"><span data-stu-id="e85c0-377">Create a Premium Storage account and copy VHDs from hello Standard Storage account</span></span>
* <span data-ttu-id="e85c0-378">Skapa ny molntjänst och omdistribuera hello SQL2 VM i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="e85c0-378">Create new cloud service and redeploy hello SQL2 VM in that cloud service.</span></span> <span data-ttu-id="e85c0-379">Skapa hello VM med hjälp av hello kopieras ursprungliga OS VHD och bifoga hello kopieras virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-379">Create hello VM using hello copied original OS VHD and attaching hello copied VHDs.</span></span>
* <span data-ttu-id="e85c0-380">Konfigurera ILB / ELB och lägga till slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="e85c0-380">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="e85c0-381">Uppdatera lyssnare genom att antingen:</span><span class="sxs-lookup"><span data-stu-id="e85c0-381">Update Listener by either:</span></span>
  * <span data-ttu-id="e85c0-382">Tar hello alltid på gruppen offline och uppdaterar hello alltid på lyssnare med nya ILB / ELB IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e85c0-382">Taking hello Always On Group offline and updating hello Always On Listener with new ILB / ELB IP address.</span></span>
  * <span data-ttu-id="e85c0-383">Eller lägga till hello IP-adressresurs av nya Cloud Service ILB/ELB via PowerShell i Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="e85c0-383">Or adding hello IP address resource of new Cloud Service ILB/ELB through PowerShell into Windows clustering.</span></span> <span data-ttu-id="e85c0-384">Sedan set hello möjliga ägare till hello IP-adress resurs toohello migreras nod, SQL2, och ange som hello nätverksnamn eller beroendet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-384">Then set hello Possible owners of hello IP Address resource toohello migrated node, SQL2, and set this as OR dependency in hello Network Name.</span></span> <span data-ttu-id="e85c0-385">Avsnittet hello ”lägga till IP-adressresurs i samma undernät' i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="e85c0-385">See hello ‘Adding IP Address Resource on Same Subnet’ section of hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
* <span data-ttu-id="e85c0-386">Kontrollera DNS-konfiguration/spridningsuppgift toohello klienter.</span><span class="sxs-lookup"><span data-stu-id="e85c0-386">Check DNS configuration/propogation toohello clients.</span></span>
* <span data-ttu-id="e85c0-387">Migrera SQL1 VM och gå igenom steg 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="e85c0-387">Migrate SQL1 VM, and go through steps 2 – 4.</span></span>
* <span data-ttu-id="e85c0-388">Om du använder steg 5ii, Lägg sedan till SQL1 möjlig ägare till hello lägga till IP-adressresurs</span><span class="sxs-lookup"><span data-stu-id="e85c0-388">If using steps 5ii, then add SQL1 as a Possible Owner for hello added IP Address Resource</span></span>
* <span data-ttu-id="e85c0-389">Redundanstestningen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-389">Test failovers.</span></span>

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a><span data-ttu-id="e85c0-390">2. Använda befintlig sekundär tillgänglighetsreplik(er): flera platser</span><span class="sxs-lookup"><span data-stu-id="e85c0-390">2. Utilize existing secondary replica(s): Multi-Site</span></span>
<span data-ttu-id="e85c0-391">Om du har noder i mer än en Azure-datacenter (DC) eller om du har en hybridmiljö kan du använda en Always On-konfiguration i den här miljön toominimize driftstopp.</span><span class="sxs-lookup"><span data-stu-id="e85c0-391">If you have nodes in more than one Azure datacenter (DC) or if you have a hybrid environment, then you can use an Always On configuration in this environment toominimize downtime.</span></span>

<span data-ttu-id="e85c0-392">hello-metoden är toochange hello alltid på synkronisering tooSynchronous för hello lokala eller sekundära Azure Domänkontrollant och sedan redundans över toothat SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e85c0-392">hello approach is toochange hello Always On synchronization tooSynchronous for hello on-premises or secondary Azure DC, and then failover over toothat SQL Server.</span></span> <span data-ttu-id="e85c0-393">Kopiera hello virtuella hårddiskar tooa Premium Storage-konto och distribuera om hello datorn till en ny molntjänst.</span><span class="sxs-lookup"><span data-stu-id="e85c0-393">Then copy hello VHDs tooa Premium Storage account, and redeploy hello machine into a new cloud service.</span></span> <span data-ttu-id="e85c0-394">Uppdatera hello-lyssnare och växlar sedan tillbaka.</span><span class="sxs-lookup"><span data-stu-id="e85c0-394">Update hello listener, and then fail back.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="e85c0-395">Punkter stillestånd</span><span class="sxs-lookup"><span data-stu-id="e85c0-395">Points of downtime</span></span>
<span data-ttu-id="e85c0-396">hello driftstopp består av hello tid toofailover toohello alternativ DC och tillbaka.</span><span class="sxs-lookup"><span data-stu-id="e85c0-396">hello downtime consists of hello time toofailover toohello alternative DC and back.</span></span> <span data-ttu-id="e85c0-397">Det beror också på klienten eller DNS-konfigurationen och klient-återanslutning kan vara fördröjd.</span><span class="sxs-lookup"><span data-stu-id="e85c0-397">It also depends on your client/DNS configuration and your client reconnection may be delayed.</span></span>
<span data-ttu-id="e85c0-398">Tänk hello följande exempel på en hybrid Always On-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="e85c0-398">Consider hello following example of a hybrid Always On configuration:</span></span>

![MultiSite1][9]

##### <a name="advantages"></a><span data-ttu-id="e85c0-400">Fördelar</span><span class="sxs-lookup"><span data-stu-id="e85c0-400">Advantages</span></span>
* <span data-ttu-id="e85c0-401">Du kan använda befintliga infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="e85c0-401">You can utilize existing infrastructure.</span></span>
* <span data-ttu-id="e85c0-402">Du har hello alternativet toopre uppgraderingen hello Azure storage på hello DR Azure DC först.</span><span class="sxs-lookup"><span data-stu-id="e85c0-402">You have hello option toopre-upgrade hello Azure storage on hello DR Azure DC first.</span></span>
* <span data-ttu-id="e85c0-403">hello DR Azure DC-lagring kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="e85c0-403">hello DR Azure DC storage can be reconfigured.</span></span>
* <span data-ttu-id="e85c0-404">Det finns minst två växling vid fel under migreringen, exklusive redundanstestning.</span><span class="sxs-lookup"><span data-stu-id="e85c0-404">There is a minimum of two failovers during migration, excluding test failovers.</span></span>
* <span data-ttu-id="e85c0-405">Du inte behöver toomove SQL Server-data med säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="e85c0-405">You do not need toomove SQL Server data with backup and restore.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="e85c0-406">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="e85c0-406">Disadvantages</span></span>
* <span data-ttu-id="e85c0-407">Beroende på klienten åtkomst tooSQL Server, kan det vara ökad latens när SQL Server körs i ett alternativt DC toohello program.</span><span class="sxs-lookup"><span data-stu-id="e85c0-407">Depending on client access tooSQL Server, there might be increased latency when SQL Server is running in an alternative DC toohello application.</span></span>
* <span data-ttu-id="e85c0-408">hello kopiera tiden för virtuella hårddiskar tooPremium lagring kan vara lång.</span><span class="sxs-lookup"><span data-stu-id="e85c0-408">hello copy time of VHDs tooPremium storage could be long.</span></span> <span data-ttu-id="e85c0-409">Detta kan påverka ditt beslut om huruvida tookeep hello nod i hello Availability Group.</span><span class="sxs-lookup"><span data-stu-id="e85c0-409">This might affect your decision on whether tookeep hello node in hello Availability Group.</span></span> <span data-ttu-id="e85c0-410">Tänk på detta för när loggen arbetar belastningar körs under migreringen hello krävs, eftersom hello primära noden måste tookeep hello unreplicated transaktioner i dess transaktionsloggen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-410">Consider this for when log intensive work loads are running during hello migration is required, since hello Primary node will have tookeep hello unreplicated transactions in its transaction log.</span></span> <span data-ttu-id="e85c0-411">Därför kan detta växa avsevärt.</span><span class="sxs-lookup"><span data-stu-id="e85c0-411">Therefore this could grow significantly.</span></span>
* <span data-ttu-id="e85c0-412">Det här scenariot använder hello Azure **Start AzureStorageBlobCopy** cmdlet, som är asynkron.</span><span class="sxs-lookup"><span data-stu-id="e85c0-412">This scenario would use hello Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="e85c0-413">Det finns inga SLA på slutförande.</span><span class="sxs-lookup"><span data-stu-id="e85c0-413">There is no SLA on completion.</span></span> <span data-ttu-id="e85c0-414">hello tid hello kopior varierar, medan det beror på vänta i kön, beror också på hello mängd data tootransfer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-414">hello time of hello copies varies, while this depends on wait in queue, it will also depend on hello amount of data tootransfer.</span></span> <span data-ttu-id="e85c0-415">Därför ha bara en nod i datacentret 2, du bör vidta säkerhetsåtgärder om hello kopiera tar längre tid än vid testning.</span><span class="sxs-lookup"><span data-stu-id="e85c0-415">Therefore you just have one node in your 2nd data center, you should take mitigation steps in case hello copy takes longer than in testing.</span></span> <span data-ttu-id="e85c0-416">Detta kan omfatta hello följande idéer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-416">This could include hello following ideas.</span></span>
  * <span data-ttu-id="e85c0-417">Lägga till en tillfällig SQL-nod 2 för hög tillgänglighet innan hello migreringen överenskomna avbrott.</span><span class="sxs-lookup"><span data-stu-id="e85c0-417">Add a temporary 2nd SQL node for HA before hello migration with agreed downtime.</span></span>
  * <span data-ttu-id="e85c0-418">Kör hello migrering utanför Azure schemalagt underhåll.</span><span class="sxs-lookup"><span data-stu-id="e85c0-418">Run hello migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="e85c0-419">Kontrollera att du har konfigurerat klustrets kvorum korrekt.</span><span class="sxs-lookup"><span data-stu-id="e85c0-419">Ensure you have configured your cluster quorum correctly.</span></span>

<span data-ttu-id="e85c0-420">Det här scenariot förutsätter att du har dokumenterats din installation och vet hur hello lagring mappas i ordning toomake ändringar för den diskens cacheinställningarna.</span><span class="sxs-lookup"><span data-stu-id="e85c0-420">This scenario assumes that you have documented your install and know how hello storage is mapped in order toomake changes for optimal disk cache settings.</span></span>

##### <a name="high-level-steps"></a><span data-ttu-id="e85c0-421">Anvisningar</span><span class="sxs-lookup"><span data-stu-id="e85c0-421">High-level steps</span></span>
![Multisite2][10]

* <span data-ttu-id="e85c0-423">Se hello lokalt / alternativa Azure DC hello SQL Server primära och gör den hello andra automatisk redundans Partner (AFP).</span><span class="sxs-lookup"><span data-stu-id="e85c0-423">Make hello on-premises / alternate Azure DC hello SQL Server Primary, and make it hello other Auto Failover Partner (AFP).</span></span>
* <span data-ttu-id="e85c0-424">Samla in information om diskkonfiguration från SQL2 och ta bort hello nod (ta inte bort anslutna virtuella hårddiskar).</span><span class="sxs-lookup"><span data-stu-id="e85c0-424">Gather disk configuration information from SQL2, and remove hello node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="e85c0-425">Skapa ett premiumlagringskonto-lagring och kopiera virtuella hårddiskar från hello standardlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e85c0-425">Create a Premium Storage account and copy VHDs from hello Standard Storage account.</span></span>
* <span data-ttu-id="e85c0-426">Skapa en ny molntjänst och skapa hello SQL2 VM med dess lagring Premier-diskar som är anslutna.</span><span class="sxs-lookup"><span data-stu-id="e85c0-426">Create a new cloud service and create hello SQL2 VM with its Premiums Storage disks attached.</span></span>
* <span data-ttu-id="e85c0-427">Konfigurera ILB / ELB och lägga till slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="e85c0-427">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="e85c0-428">Uppdatera hello alltid på lyssnare med nya ILB / ELB IP-adress och testa redundans.</span><span class="sxs-lookup"><span data-stu-id="e85c0-428">Update hello Always On Listener with new ILB / ELB IP address and test failover.</span></span>
* <span data-ttu-id="e85c0-429">Kontrollera hello DNS-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e85c0-429">Check hello DNS configuration.</span></span>
* <span data-ttu-id="e85c0-430">Ändra hello AFP tooSQL2 och migrera SQL1 och gå igenom steg 2 – 5.</span><span class="sxs-lookup"><span data-stu-id="e85c0-430">Change hello AFP tooSQL2, and then migrate SQL1 and go through steps 2 – 5.</span></span>
* <span data-ttu-id="e85c0-431">Redundanstestningen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-431">Test failovers.</span></span>
* <span data-ttu-id="e85c0-432">Växla hello AFP tillbaka tooSQL1 och SQL2</span><span class="sxs-lookup"><span data-stu-id="e85c0-432">Switch hello AFP back tooSQL1 and SQL2</span></span>

## <a name="appendix-migrating-a-multisite-always-on-cluster-toopremium-storage"></a><span data-ttu-id="e85c0-433">Bilaga: Migrera en Multisite alltid på klustret tooPremium lagring</span><span class="sxs-lookup"><span data-stu-id="e85c0-433">Appendix: Migrating a Multisite Always On Cluster tooPremium Storage</span></span>
<span data-ttu-id="e85c0-434">hello resten av det här avsnittet innehåller ett detaljerat exempel konvertera en flera platser alltid på tooPremium klusterlagringen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-434">hello remainder of this topic provides a detailed example of converting a multi-site Always On cluster tooPremium storage.</span></span> <span data-ttu-id="e85c0-435">Den konverterar också hello lyssnare från att använda en extern belastningsutjämnare (ELB) tooan interna belastningsutjämnare (ILB).</span><span class="sxs-lookup"><span data-stu-id="e85c0-435">It also converts hello Listener from using an external load balancer (ELB) tooan internal load balancer (ILB).</span></span>

### <a name="environment"></a><span data-ttu-id="e85c0-436">Miljö</span><span class="sxs-lookup"><span data-stu-id="e85c0-436">Environment</span></span>
* <span data-ttu-id="e85c0-437">Windows 2k 12 / SQL 2k 12</span><span class="sxs-lookup"><span data-stu-id="e85c0-437">Windows 2k12 / SQL 2k12</span></span>
* <span data-ttu-id="e85c0-438">1 DB-filer på SP</span><span class="sxs-lookup"><span data-stu-id="e85c0-438">1 DB Files on SP</span></span>
* <span data-ttu-id="e85c0-439">2 x lagringspooler per nod</span><span class="sxs-lookup"><span data-stu-id="e85c0-439">2 x Storage Pools per Node</span></span>

![Appendix1][11]

### <a name="vm"></a><span data-ttu-id="e85c0-441">VM:</span><span class="sxs-lookup"><span data-stu-id="e85c0-441">VM:</span></span>
<span data-ttu-id="e85c0-442">I det här exemplet ska vi ska toodemonstrate flyttar från en ELB tooILB.</span><span class="sxs-lookup"><span data-stu-id="e85c0-442">In this example we are going toodemonstrate moving from an ELB tooILB.</span></span> <span data-ttu-id="e85c0-443">ELB fanns innan ILB, så att det visar hur hello tooswitch toothis under migreringen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-443">ELB was available before ILB, so this shows how tooswitch toothis during hello migration.</span></span>

![Appendix2][12]

### <a name="pre-steps-connect-toosubscription"></a><span data-ttu-id="e85c0-445">Pre steg: Ansluta tooSubscription</span><span class="sxs-lookup"><span data-stu-id="e85c0-445">Pre Steps: Connect tooSubscription</span></span>
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a><span data-ttu-id="e85c0-446">Steg 1: Skapa nytt Lagringskonto och Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="e85c0-446">Step 1: Create New Storage Account and Cloud Service</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where hello vm toomigrate resides
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

#### <a name="step-2-increase-hello-permitted-failures-on-resources-optional"></a><span data-ttu-id="e85c0-447">Steg 2: Öka hello tillåtna fel för resurser<Optional></span><span class="sxs-lookup"><span data-stu-id="e85c0-447">Step 2: Increase hello permitted failures on resources <Optional></span></span>
<span data-ttu-id="e85c0-448">På vissa resurser som tillhör tooyour Always On-Tillgänglighetsgruppen finns begränsningar på hur många fel som kan uppstå under en period där hello görs försök toorestart hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-448">On certain resources that belong tooyour Always On Availability Group there are limits on how many failures that can occur in a period, where hello cluster service will attempt toorestart hello resource group.</span></span> <span data-ttu-id="e85c0-449">Det rekommenderas att du ökar det samtidigt som du går igenom den här proceduren eftersom om du inte manuellt redundans och utlösare redundans genom att stänga av datorer som du kan hämta Stäng toothis gränsen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-449">It is recommended you increase this whilst you are walking through this procedure, since if you don’t manually failover and trigger failovers by shutting down machines you can get close toothis limit.</span></span>

<span data-ttu-id="e85c0-450">Det skulle vara försiktig toodouble hello fel ersättning toodo detta i hanteraren för redundanskluster, gå toohello hello alltid på resursgruppens egenskaper:</span><span class="sxs-lookup"><span data-stu-id="e85c0-450">It would be prudent toodouble hello failure allowance, toodo this in Failover Cluster Manager, go toohello Properties of hello Always On resource group:</span></span>

![Appendix3][13]

<span data-ttu-id="e85c0-452">Ändra hello maximalt antal misslyckanden too6.</span><span class="sxs-lookup"><span data-stu-id="e85c0-452">Change hello Maximum Failures too6.</span></span>

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a><span data-ttu-id="e85c0-453">Steg 3: Lägga IP-adressresurs för klustergruppen<Optional></span><span class="sxs-lookup"><span data-stu-id="e85c0-453">Step 3: Addition IP Address resource for Cluster Group <Optional></span></span>
<span data-ttu-id="e85c0-454">Om du har en enda IP-adress för hello klustergrupp och detta är justerade toohello moln undernät, varning, om du av misstag koppla från alla noder i klustret i hello molnet att nätverket och sedan hello klustrets IP-resurs och Klusternätverksnamnet inte kommer att kunna toocome online.</span><span class="sxs-lookup"><span data-stu-id="e85c0-454">If you have only one IP address for hello Cluster Group and this is aligned toohello cloud subnet, beware, if you accidentally take offline all cluster nodes in hello cloud on that network then hello Cluster IP resource and Cluster Network Name will not be able toocome online.</span></span> <span data-ttu-id="e85c0-455">I hello uppdaterar händelsen för detta hindrar det tooother klusterresurser.</span><span class="sxs-lookup"><span data-stu-id="e85c0-455">In hello event of this it will prevent updates tooother cluster resources.</span></span>

#### <a name="step-4-dns-configuration"></a><span data-ttu-id="e85c0-456">Steg 4: DNS-konfiguration</span><span class="sxs-lookup"><span data-stu-id="e85c0-456">Step 4: DNS configuration</span></span>
<span data-ttu-id="e85c0-457">tooimplement en smidig övergång beror på hur DNS används utnyttjade och uppdateras.</span><span class="sxs-lookup"><span data-stu-id="e85c0-457">tooimplement a smooth transition depends on how DNS is being utilized and updated.</span></span>
<span data-ttu-id="e85c0-458">När alltid är installerat, skapas en klusterresurs för Windows-grupp om du öppnar Klusterhanteraren för växling visas att åtminstone den har tre resurser, hello två som hello dokumentet refererar tooare:</span><span class="sxs-lookup"><span data-stu-id="e85c0-458">When Always On is installed, it creates a Windows Cluster Resource group, if you open Failover Cluster Manager, you will see that at a minimum it will have three resources, hello two that hello document refers tooare:</span></span>

* <span data-ttu-id="e85c0-459">Virtuellt nätverksnamn (VNN) – detta är hello DNS-namn som klienten ansluta toowhen förskjutning tooconnect tooSQL servrar via alltid på.</span><span class="sxs-lookup"><span data-stu-id="e85c0-459">Virtual Network Name (VNN) – This is hello DNS name that client connect toowhen wanting tooconnect tooSQL Servers via Always On.</span></span>
* <span data-ttu-id="e85c0-460">IP-adressresurs – det här är IP-adressen som som är associerade med hello VNN hello, du kan ha flera och i multisitekonfigurationen har du en IP-adress per plats-undernät.</span><span class="sxs-lookup"><span data-stu-id="e85c0-460">IP Address Resource – This is hello IP address that associated with hello VNN, you can have more than one, and in a multisite configuration you will have an IP address per site/subnet.</span></span>

<span data-ttu-id="e85c0-461">När anslutande tooSQL Server, hello-drivrutin för SQL Server-klienten hämtar hello DNS-poster som är associerade med hello-lyssnare och försök tooconnect tooeach alltid på associerade IP-adress, nedan diskuterar vi några faktorer som kan påverka detta.</span><span class="sxs-lookup"><span data-stu-id="e85c0-461">When connecting tooSQL Server, hello SQL Server Client driver will retrieve hello DNS records associated with hello listener and try tooconnect tooeach Always On associated IP address, below we discuss some factors that can influence this.</span></span>

<span data-ttu-id="e85c0-462">hello antalet samtidiga DNS-poster som är associerade med hello grupplyssnarens namn beror inte bara på hello antalet IP-adresser som är kopplade men hello ' RegisterAllIpProviders'setting i redundanskluster för hello Always ON VNN resurs.</span><span class="sxs-lookup"><span data-stu-id="e85c0-462">hello number of concurrent DNS records that are associated with hello listener name depends not only on hello number of IP addresses associated, but hello ‘RegisterAllIpProviders’setting in Failover Clustering for hello Always ON VNN resource.</span></span>

<span data-ttu-id="e85c0-463">När du distribuerar alltid på i Azure finns olika steg toocreate hello lyssnare och IP-adresser, du toomanually konfigurera hello 'RegisterAllIpProviders' too1, detta är olika tooan lokalt alltid på distribution där det redan har angetts too1.</span><span class="sxs-lookup"><span data-stu-id="e85c0-463">When you deploy Always On in Azure there are different steps toocreate hello Listener and IP Addresses, you have toomanually configure hello ‘RegisterAllIpProviders’ too1, this is different tooan on-premises Always On deployment where it is already set too1.</span></span>

<span data-ttu-id="e85c0-464">'RegisterAllIpProviders' är 0, och sedan visas bara en DNS-post i DNS som är associerade med hello lyssnare:</span><span class="sxs-lookup"><span data-stu-id="e85c0-464">If ‘RegisterAllIpProviders’ is 0, then you will only see one DNS record in DNS associated with hello Listener:</span></span>

![Appendix4][14]

<span data-ttu-id="e85c0-466">Om 'RegisterAllIpProviders' är 1:</span><span class="sxs-lookup"><span data-stu-id="e85c0-466">If ‘RegisterAllIpProviders’ is 1:</span></span>

![Appendix5][15]

<span data-ttu-id="e85c0-468">hello koden nedan kommer dump ut hello VNN inställningar och ange det åt dig, du Anmärkning för hello ändra tootake kraft du behöver tootake hello VNN offline och slå på den online igen, den här ta hello lyssnare offline orsakar klienten anslutning avbrott.</span><span class="sxs-lookup"><span data-stu-id="e85c0-468">hello code below will dump out hello VNN settings and set it for you, please note, for hello change tootake effect you will need tootake hello VNN offline and turn it back online, this taking hello Listener offline causing client connectivity disruption.</span></span>

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

<span data-ttu-id="e85c0-469">I ett senare migreringssteg måste tooupdate hello alltid på lyssnare med en uppdaterad IP-adress som ska referera till en belastningsutjämnare, detta kommer att omfatta en IP-adress resurs borttagning och tillägg.</span><span class="sxs-lookup"><span data-stu-id="e85c0-469">In a later migration step you will need tooupdate hello Always On listener with an updated IP address that will reference a load balancer, this will involve an IP Address resource removal and addition.</span></span> <span data-ttu-id="e85c0-470">När du hello IP-uppdatering måste tooensure hello nya IP-adress har uppdaterats i DNS-zonen och att hello klienter uppdaterar sina lokala DNS-cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-470">After hello IP update, you need tooensure hello new IP address has been updated in DNS Zone and that hello clients are updating their local DNS cache.</span></span>

<span data-ttu-id="e85c0-471">Om klienterna finns i olika nätverkssegment och referera till en annan DNS-server, måste tooconsider vad händer om DNS-zonöverföring under migreringen hello hello programmet återansluta ska vara begränsad tid med minst hello zonen överför tid för alla nya IP-adresser för hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="e85c0-471">If your clients reside in a different network segment and reference a different DNS server, you need tooconsider what happens about DNS Zone Transfer during hello migration, as hello application reconnect time will be constrained by at least hello Zone Transfer Time of any new IP addresses for hello listener.</span></span> <span data-ttu-id="e85c0-472">Om du är under tidsbegränsning här du ska diskutera och testa att tvinga en inkrementell zonöverföring med Windows-grupper, och också hello DNS värden poster tooa minskar Time tooLive (TTL), så att uppdatera hello-klienter.</span><span class="sxs-lookup"><span data-stu-id="e85c0-472">If you are under time constraint here, you should discuss and test forcing an incremental zone transfer with your Windows teams, and also put hello DNS host record tooa lower Time tooLive (TTL), so hello clients update.</span></span> <span data-ttu-id="e85c0-473">Mer information finns i [inkrementella zonöverföringar](https://technet.microsoft.com/library/cc958973.aspx) och [Start DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span><span class="sxs-lookup"><span data-stu-id="e85c0-473">For more information, see [Incremental Zone Transfers](https://technet.microsoft.com/library/cc958973.aspx) and [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span></span>

<span data-ttu-id="e85c0-474">Som standard hello TTL-värde för DNS-post som är associerad med hello lyssnare i alltid på i Azure är 1200 sekunder.</span><span class="sxs-lookup"><span data-stu-id="e85c0-474">By default hello TTL for DNS Record that is associated with hello Listener in Always On in Azure is 1200 seconds.</span></span> <span data-ttu-id="e85c0-475">Vill du kanske tooreduce detta om du är under tidsbegränsning under migreringen tooensure hello klienter uppdateringen deras DNS med hello uppdatera IP-adressen för hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="e85c0-475">You may wish tooreduce this if you are under time constraint during your migration tooensure hello clients update their DNS with hello updated IP address for hello listener.</span></span> <span data-ttu-id="e85c0-476">Du kan se och ändra hello konfigurationen av dumpning ut hello konfigurationen av hello VNN:</span><span class="sxs-lookup"><span data-stu-id="e85c0-476">You can see and modify hello configuration by dumping out hello configuration of hello VNN:</span></span>

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

<span data-ttu-id="e85c0-477">Observera hello lägre hello HostRecordTTL, inträffar ett högre värde för DNS-trafik.</span><span class="sxs-lookup"><span data-stu-id="e85c0-477">Please note, hello lower hello ‘HostRecordTTL’, a higher amount of DNS traffic will occur.</span></span>

##### <a name="client-application-settings"></a><span data-ttu-id="e85c0-478">Inställningar för program</span><span class="sxs-lookup"><span data-stu-id="e85c0-478">Client application settings</span></span>
<span data-ttu-id="e85c0-479">Om din SQL-klienten programmet stöder hello .net 4.5 SQLClient och du kan använda ' MULTISUBNETFAILOVER = TRUE ”nyckelord, detta rekommenderas toobe tillämpas så som den kan användas för snabbare anslutning tooSQL Always On-Tillgänglighetsgruppen under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="e85c0-479">If your SQL client application supports hello .Net 4.5 SQLClient, then you can use ‘MULTISUBNETFAILOVER=TRUE’ keyword, this is recommended toobe applied as it allows for faster connection tooSQL Always On Availability Group during failover.</span></span> <span data-ttu-id="e85c0-480">Den räknar igenom alla IP-adresser som är associerade med hello alltid lyssnare för parallellt och utför en mer aggressiv TCP försök anslutningshastighet under en växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="e85c0-480">It enumerates through all IP addresses associated with hello Always On listener in parallel, and performs a more aggressive TCP connection retry speed during a failover.</span></span>

<span data-ttu-id="e85c0-481">Mer information om hello inställningarna ovan finns [MultiSubnetFailover nyckelord och associerade funktioner](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span><span class="sxs-lookup"><span data-stu-id="e85c0-481">For more information regarding hello settings above, please see [MultiSubnetFailover Keyword and Associated Features](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span></span> <span data-ttu-id="e85c0-482">Se även [SqlClient-stöd för hög tillgänglighet och katastrofåterställning](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="e85c0-482">Also see [SqlClient Support for High Availability, Disaster Recovery](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span></span>

#### <a name="step-5-cluster-quorum-settings"></a><span data-ttu-id="e85c0-483">Steg 5: Inställningarna för klusterkvorum</span><span class="sxs-lookup"><span data-stu-id="e85c0-483">Step 5: Cluster quorum settings</span></span>
<span data-ttu-id="e85c0-484">Som du kommer toobe tar reda på minst en SQL Server ned samtidigt, bör du ändra hello kvoruminställningen för klustret, om filen filresurs vittne (FSW) med 2 noder, ska du ange hello kvorum tooallow Nodmajoritet och använda dynamiska röstning och det är tooallow för en enskild nod tooremain position.</span><span class="sxs-lookup"><span data-stu-id="e85c0-484">As you are going toobe taking out at least one SQL Server down at a time, you should modify hello cluster quorum setting, if using File Share Witness (FSW) with 2 nodes, you should set hello quorum tooallow node majority and utilize dynamic voting, and this is tooallow for a single node tooremain standing.</span></span>

    Set-ClusterQuorum -NodeMajority  

<span data-ttu-id="e85c0-485">Mer information om att hantera och konfigurera hello klustrets kvorum finns [konfigurera och hantera hello kvorum i ett redundanskluster för Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx).</span><span class="sxs-lookup"><span data-stu-id="e85c0-485">For more information on managing and configuring hello cluster quorum, please see [Configure and Manage hello Quorum in a Windows Server 2012 Failover Cluster](https://technet.microsoft.com/library/jj612870.aspx).</span></span>

#### <a name="step-6-extract-existing-endpoints-and-acls"></a><span data-ttu-id="e85c0-486">Steg 6: Extrahera befintliga slutpunkter och ACL: er</span><span class="sxs-lookup"><span data-stu-id="e85c0-486">Step 6: Extract Existing Endpoints and ACLs</span></span>
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for hello Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

<span data-ttu-id="e85c0-487">Spara dessa tooa textfil.</span><span class="sxs-lookup"><span data-stu-id="e85c0-487">Save these tooa text file.</span></span>

#### <a name="step-7-change-failover-partners-and-replication-modes"></a><span data-ttu-id="e85c0-488">Steg 7: Ändra Redundanspartners och replikering lägen</span><span class="sxs-lookup"><span data-stu-id="e85c0-488">Step 7: Change Failover Partners and Replication Modes</span></span>
<span data-ttu-id="e85c0-489">Om du har mer än 2 SQL-servrar du bör ändra hello växling vid fel på en annan sekundär i en annan Domänkontrollant eller lokala too'Synchronous' och göra det en automatisk redundans Partner (AFP), det är så du upprätthålla hög tillgänglighet, samtidigt som du gör ändringar.</span><span class="sxs-lookup"><span data-stu-id="e85c0-489">If you have more than 2 SQL Servers, you should change hello failover of another secondary in another DC or on-premises too‘Synchronous’ and make it an Automatic Failover Partner (AFP), this is so you maintain HA whilst you are making changes.</span></span> <span data-ttu-id="e85c0-490">Du kan göra detta via TSQL av dock ändra SSMS:</span><span class="sxs-lookup"><span data-stu-id="e85c0-490">You can do this through TSQL of modify though SSMS:</span></span>

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a><span data-ttu-id="e85c0-492">Steg 8: Ta bort sekundära virtuella datorn från Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="e85c0-492">Step 8: Remove Secondary VM from cloud service</span></span>
<span data-ttu-id="e85c0-493">Du bör planera toomigrate ett moln sekundära noden först om det är för närvarande primära du ska initiera en manuell växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="e85c0-493">You should be planning toomigrate a cloud secondary node first, if this is currently primary, you should initiate a manual failover.</span></span>

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

    #Check disk config, make sure below returns hello disks associated with hello VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="e85c0-494">Steg 9: Ändra disken cacheinställningar i CSV-filen och spara</span><span class="sxs-lookup"><span data-stu-id="e85c0-494">Step 9: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="e85c0-495">Dessa bör anges tooREADONLY för datavolymer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-495">For data volumes these should be set tooREADONLY.</span></span>

<span data-ttu-id="e85c0-496">Dessa bör anges tooNONE för TLOG volymer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-496">For TLOG volumes these should be set tooNONE.</span></span>

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a><span data-ttu-id="e85c0-498">Steg 10: Kopiera virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="e85c0-498">Step 10: Copy VHDS</span></span>
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
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



<span data-ttu-id="e85c0-499">Du kan kontrollera statusen för hello kopia av hello virtuella hårddiskar toohello Premium Storage-konto:</span><span class="sxs-lookup"><span data-stu-id="e85c0-499">You can check hello copy status of hello VHDs toohello Premium Storage account:</span></span>

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

<span data-ttu-id="e85c0-501">Vänta tills alla dessa registreras åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="e85c0-501">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="e85c0-502">Information för enskilda BLOB:</span><span class="sxs-lookup"><span data-stu-id="e85c0-502">For information for individual blobs:</span></span>

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a><span data-ttu-id="e85c0-503">Steg 11: Registrera OS-disk</span><span class="sxs-lookup"><span data-stu-id="e85c0-503">Step 11: Register OS disk</span></span>
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

#### <a name="step-12-import-secondary-into-new-cloud-service"></a><span data-ttu-id="e85c0-504">Steg 12: Importera sekundär till ny molntjänst</span><span class="sxs-lookup"><span data-stu-id="e85c0-504">Step 12: Import secondary into new cloud service</span></span>
<span data-ttu-id="e85c0-505">hello koden nedan även använder hello läggas till här alternativ du kan importera hello dator och använda hello retainable VIP.</span><span class="sxs-lookup"><span data-stu-id="e85c0-505">hello code below also uses hello added option here you can import hello machine and use hello retainable VIP.</span></span>

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember toochange tooXIO
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

    ###Attaching disks tooa VM during a deploy tooa new cloud service and new storage account is different from just attaching VHDs toojust a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="e85c0-506">Steg 13: Skapa ILB på nya molntjänster Svc lägga till belastningen belastningsutjämnade slutpunkter och ACL: er</span><span class="sxs-lookup"><span data-stu-id="e85c0-506">Step 13: Create ILB on New Cloud Svc, Add Load Balanced Endpoints and ACLs</span></span>
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

#### <a name="step-14-update-always-on"></a><span data-ttu-id="e85c0-507">Steg 14: Uppdatera alltid på</span><span class="sxs-lookup"><span data-stu-id="e85c0-507">Step 14: Update Always On</span></span>
    #Code toobe executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # hello azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency tooListener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

<span data-ttu-id="e85c0-509">Nu ska du ta bort Molntjänsten för hello gamla IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e85c0-509">Now remove hello old cloud service IP Address.</span></span>

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a><span data-ttu-id="e85c0-511">Steg 15: DNS-uppdateringskontroll</span><span class="sxs-lookup"><span data-stu-id="e85c0-511">Step 15: DNS update check</span></span>
<span data-ttu-id="e85c0-512">Du bör nu se DNS-servrar på SQL Server-klient-nätverk och kontrollera att kluster har lagts till hello extra värdpost för hello lägga till IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e85c0-512">You should now check DNS Servers on your SQL Server client networks and make sure that clustering has added hello extra host record for hello added IP address.</span></span> <span data-ttu-id="e85c0-513">Om dessa DNS-servrar inte har uppdaterat, Överväg att tvinga en DNS-zonöverföring och kontrollera att hello klienter i undernätet är kan tooresolve tooboth alltid på IP-adresser, det är så du inte behöver toowait automatisk DNS-replikeringen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-513">If those DNS servers have not updated, consider forcing a DNS Zone transfer and ensure that hello clients in there subnet are able tooresolve tooboth Always On IP Addresses, this is so you do not need toowait for automatic DNS replication.</span></span>

#### <a name="step-16-reconfigure-always-on"></a><span data-ttu-id="e85c0-514">Steg 16: Konfigurera om alltid på</span><span class="sxs-lookup"><span data-stu-id="e85c0-514">Step 16: Reconfigure Always On</span></span>
<span data-ttu-id="e85c0-515">Då vänta du hello sekundära noden som var migrerade toofully omsynkronisering med hello lokala nod och växla toosynchronous replikeringsnod och gör det hello AFP.</span><span class="sxs-lookup"><span data-stu-id="e85c0-515">At this point you wait for hello secondary that node that was migrated toofully resynchronize with hello on-premises node and switch toosynchronous replication node and make it hello AFP.</span></span>  

#### <a name="step-17-migrate-second-node"></a><span data-ttu-id="e85c0-516">Steg 17: Migrera andra noden</span><span class="sxs-lookup"><span data-stu-id="e85c0-516">Step 17: Migrate second node</span></span>
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

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="e85c0-517">Steg 18: Ändra disken cacheinställningar i CSV-filen och spara</span><span class="sxs-lookup"><span data-stu-id="e85c0-517">Step 18: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="e85c0-518">Dessa bör anges tooREADONLY för datavolymer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-518">For data volumes these should be set tooREADONLY.</span></span>

<span data-ttu-id="e85c0-519">Dessa bör anges tooNONE för TLOG volymer.</span><span class="sxs-lookup"><span data-stu-id="e85c0-519">For TLOG volumes these should be set tooNONE.</span></span>

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a><span data-ttu-id="e85c0-521">Steg 19: Skapa nytt oberoende Lagringskonto för sekundär nod</span><span class="sxs-lookup"><span data-stu-id="e85c0-521">Step 19: Create New Independent Storage Account for Secondary Node</span></span>
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset hello storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a><span data-ttu-id="e85c0-522">Steg 20: Kopiera virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="e85c0-522">Step 20: Copy VHDS</span></span>
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
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


<span data-ttu-id="e85c0-523">Du kan kontrollera hello VHD-kopian status för alla virtuella hårddiskar: ForEach ($disk i $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Disketikett $diskName = $disk. DiskName</span><span class="sxs-lookup"><span data-stu-id="e85c0-523">You can check hello VHD copy status for all VHDs: ForEach ($disk in $diskobjects) { $lun = $disk.Lun $vhdname = $disk.vhdname $cacheoption = $disk.HostCaching $disklabel = $disk.DiskLabel $diskName = $disk.DiskName</span></span>

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

<span data-ttu-id="e85c0-525">Vänta tills alla dessa registreras åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="e85c0-525">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="e85c0-526">Information för enskilda BLOB:</span><span class="sxs-lookup"><span data-stu-id="e85c0-526">For information for individual blobs:</span></span>

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a><span data-ttu-id="e85c0-527">Steg 21: Registrera OS-disk</span><span class="sxs-lookup"><span data-stu-id="e85c0-527">Step 21: Register OS disk</span></span>
    #change storage account toohello new XIO storage account
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

    #Join tooexisting Avaiability Set

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

    ###This is different toojust a straight cloud service change
    #note if you do not have a disk label hello command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="e85c0-528">Steg 22: Lägg till belastningen belastningsutjämnade slutpunkter och ACL: er</span><span class="sxs-lookup"><span data-stu-id="e85c0-528">Step 22: Add Load Balanced Endpoints and ACLs</span></span>
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in hello Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a><span data-ttu-id="e85c0-529">Steg 23: Testa redundans</span><span class="sxs-lookup"><span data-stu-id="e85c0-529">Step 23: Test failover</span></span>
<span data-ttu-id="e85c0-530">Nu bör du låta hello migrerade nod synkronisera med hello lokalt alltid på noden, placera det i toosynchronous replikeringsläge och vänta tills den är synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="e85c0-530">You should now let hello migrated node synchronize with hello on-premises Always On node, place it in toosynchronous replication mode and wait until it is synchronized.</span></span> <span data-ttu-id="e85c0-531">Sedan migreras växling från den första noden i lokal toohello, vilket är hello AFP.</span><span class="sxs-lookup"><span data-stu-id="e85c0-531">Then failover from on-prem toohello first node migrated, which is hello AFP.</span></span> <span data-ttu-id="e85c0-532">När du som har arbetat migreras ändra hello senast nod toohello AFP.</span><span class="sxs-lookup"><span data-stu-id="e85c0-532">Once that has worked, change hello last migrated node toohello AFP.</span></span>

<span data-ttu-id="e85c0-533">Du bör redundanstestningen mellan alla noder och kör om chaos testerna tooensure redundans fungerar som förväntat och en rimlig manor.</span><span class="sxs-lookup"><span data-stu-id="e85c0-533">You should test failovers between all nodes and run though chaos tests tooensure failovers work as expected and in a timely manor.</span></span>

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a><span data-ttu-id="e85c0-534">Steg 24: Lägga tillbaka inställningarna för klusterkvorum / DNS TTL / Failover Pntrs / synkroniseringsinställningar</span><span class="sxs-lookup"><span data-stu-id="e85c0-534">Step 24: Put back cluster quorum settings / DNS TTL / Failover Pntrs / Sync Settings</span></span>
##### <a name="adding-ip-address-resource-on-same-subnet"></a><span data-ttu-id="e85c0-535">Lägger till IP-adressresurs på samma undernät</span><span class="sxs-lookup"><span data-stu-id="e85c0-535">Adding IP Address Resource on Same Subnet</span></span>
<span data-ttu-id="e85c0-536">Om du har bara 2 SQL-servrar och vill toomigrate dem tooa nya Molntjänsten, men vill tookeep dem på hello samma undernät, kan du undvika tar du alltid offline toodelete hello ursprungliga hello lyssnare på IP-adress och lägga till hello nya IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="e85c0-536">If you have only 2 SQL Servers and want toomigrate them tooa new cloud service, but want tookeep them on hello same subnet, you can avoid taking hello listener offline toodelete hello original Always On IP Address and add hello New IP Address.</span></span> <span data-ttu-id="e85c0-537">Om du migrerar hello VMs tooanother undernätet inte behöver du toodo detta som en ytterligare klusternätverk som kommer att referera till det undernätet.</span><span class="sxs-lookup"><span data-stu-id="e85c0-537">If you are migrating hello VMs tooanother subnet you will not need toodo this as there will be an additional cluster network that will reference that subnet.</span></span>

<span data-ttu-id="e85c0-538">När du har fört hello migreras sekundär och lagts till i hello nya IP-adressresurs för hello ny molntjänst innan redundans hello befintlig primär, bör du se dessa inom hello Klusterhanterare för växling vid fel:</span><span class="sxs-lookup"><span data-stu-id="e85c0-538">Once you have brought up hello migrated secondary and added in hello new IP Address resource for hello new cloud service before failover hello existing Primary, you should take these steps within hello Cluster Failover Manager:</span></span>

<span data-ttu-id="e85c0-539">tooadd i IP-adress finns hello [bilaga](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), steg 14.</span><span class="sxs-lookup"><span data-stu-id="e85c0-539">tooadd in IP Address, see hello [Appendix](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), step 14.</span></span>

1. <span data-ttu-id="e85c0-540">Ändra hello möjlig ägare too'Existing primära SQL Server för hello aktuella IP-adressresurs ', i hello exemplet nedan 'dansqlams4':</span><span class="sxs-lookup"><span data-stu-id="e85c0-540">For hello current IP Address resource, change hello possible owner too‘Existing Primary SQL Server’, in hello example below, ‘dansqlams4’:</span></span>

    ![Appendix13][23]
2. <span data-ttu-id="e85c0-542">Ändra hello möjlig ägare too'Migrated för hello nya IP-adressresurs, den sekundära SQL Server ”, i hello exemplet nedan 'dansqlams5':</span><span class="sxs-lookup"><span data-stu-id="e85c0-542">For hello new IP Address resource, change hello possible owner too‘Migrated secondary SQL Server’, in hello example below, ‘dansqlams5’:</span></span>

    ![Appendix14][24]
3. <span data-ttu-id="e85c0-544">När det här värdet kan du redundans och när hello sista noden migreras hello möjliga ägare måste redigeras så noden läggs till som möjlig ägare:</span><span class="sxs-lookup"><span data-stu-id="e85c0-544">Once this is set you can failover, and when hello last node is migrated hello Possible Owners should be edited so that node is added as a Possible Owner:</span></span>

    ![Appendix15][25]

## <a name="additional-resources"></a><span data-ttu-id="e85c0-546">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e85c0-546">Additional resources</span></span>
* [<span data-ttu-id="e85c0-547">Azure Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="e85c0-547">Azure Premium Storage</span></span>](../../../storage/common/storage-premium-storage.md)
* [<span data-ttu-id="e85c0-548">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="e85c0-548">Virtual Machines</span></span>](https://azure.microsoft.com/services/virtual-machines/)
* [<span data-ttu-id="e85c0-549">SQLServer i Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="e85c0-549">SQL Server in Azure Virtual Machines</span></span>](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

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
