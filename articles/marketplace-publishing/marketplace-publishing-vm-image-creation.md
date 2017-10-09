---
title: "aaaCreating en avbildning av virtuell dator för hello Azure Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar om hur toocreate en virtuell dator bild för hello Azure Marketplace för andra toopurchase."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio; v-divte
ms.openlocfilehash: 65e1c0530bb050fb379a52544e36c55faacd84df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toocreate-a-virtual-machine-image-for-hello-azure-marketplace"></a><span data-ttu-id="6bc09-103">Guiden toocreate en avbildning av virtuell dator för hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="6bc09-103">Guide toocreate a virtual machine image for hello Azure Marketplace</span></span>
<span data-ttu-id="6bc09-104">Den här artikeln **steg 2**, vägleder dig genom förbereder hello virtuella hårddiskar (VHD) som du ska distribuera toohello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6bc09-104">This article, **Step 2**, walks you through preparing hello virtual hard disks (VHDs) that you will deploy toohello Azure Marketplace.</span></span> <span data-ttu-id="6bc09-105">De virtuella hårddiskarna är hello foundation för din SKU: N.</span><span class="sxs-lookup"><span data-stu-id="6bc09-105">Your VHDs are hello foundation of your SKU.</span></span> <span data-ttu-id="6bc09-106">hello processen skiljer sig åt beroende på om du tillhandahåller en Linux- eller Windows-baserade SKU.</span><span class="sxs-lookup"><span data-stu-id="6bc09-106">hello process differs depending on whether you are providing a Linux-based or Windows-based SKU.</span></span> <span data-ttu-id="6bc09-107">Den här artikeln täcker båda scenarierna.</span><span class="sxs-lookup"><span data-stu-id="6bc09-107">This article covers both scenarios.</span></span> <span data-ttu-id="6bc09-108">Den här processen kan utföras parallellt med [skapande av konton och registrering][link-acct-creation].</span><span class="sxs-lookup"><span data-stu-id="6bc09-108">This process can be performed in parallel with [Account creation and registration][link-acct-creation].</span></span>

## <a name="1-define-offers-and-skus"></a><span data-ttu-id="6bc09-109">1. Definiera erbjudanden och SKU: er</span><span class="sxs-lookup"><span data-stu-id="6bc09-109">1. Define Offers and SKUs</span></span>
<span data-ttu-id="6bc09-110">I det här avsnittet lär du dig toodefine hello erbjudanden och deras associerade SKU: er.</span><span class="sxs-lookup"><span data-stu-id="6bc09-110">In this section, you learn toodefine hello offers and their associated SKUs.</span></span>

<span data-ttu-id="6bc09-111">Ett erbjudande är en ”överordnad” tooall av dess SKU: er.</span><span class="sxs-lookup"><span data-stu-id="6bc09-111">An offer is a "parent" tooall of its SKUs.</span></span> <span data-ttu-id="6bc09-112">Du kan ha flera erbjudanden.</span><span class="sxs-lookup"><span data-stu-id="6bc09-112">You can have multiple offers.</span></span> <span data-ttu-id="6bc09-113">Hur du bestämmer dig för toostructure är erbjudandena upp tooyou.</span><span class="sxs-lookup"><span data-stu-id="6bc09-113">How you decide toostructure your offers is up tooyou.</span></span> <span data-ttu-id="6bc09-114">När ett erbjudande pushas toostaging pushas tillsammans med alla dess SKU: er.</span><span class="sxs-lookup"><span data-stu-id="6bc09-114">When an offer is pushed toostaging, it is pushed along with all of its SKUs.</span></span> <span data-ttu-id="6bc09-115">Du noga överväga din SKU-identifierare, eftersom de kommer att vara synliga i hello-URL:</span><span class="sxs-lookup"><span data-stu-id="6bc09-115">Carefully consider your SKU identifiers, because they will be visible in hello URL:</span></span>

* <span data-ttu-id="6bc09-116">Azure.com: http://azure.microsoft.com/marketplace/partners/ {PartnerNamespace} / {OfferIdentifier}-{SKUidentifier}</span><span class="sxs-lookup"><span data-stu-id="6bc09-116">Azure.com: http://azure.microsoft.com/marketplace/partners/{PartnerNamespace}/{OfferIdentifier}-{SKUidentifier}</span></span>
* <span data-ttu-id="6bc09-117">Azure preview portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {SKUIDdentifier}</span><span class="sxs-lookup"><span data-stu-id="6bc09-117">Azure preview portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{SKUIDdentifier}</span></span>  

<span data-ttu-id="6bc09-118">En SKU är hello kommersiella namn för en VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="6bc09-118">A SKU is hello commercial name for a VM image.</span></span> <span data-ttu-id="6bc09-119">En VM-avbildning som innehåller en operativsystemdisk och noll eller flera datadiskar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-119">A VM image contains one operating system disk and zero or more data disks.</span></span> <span data-ttu-id="6bc09-120">Det är i stort sett hello fullständig lagringsprofil för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6bc09-120">It is essentially hello complete storage profile for a virtual machine.</span></span> <span data-ttu-id="6bc09-121">En virtuell Hårddisk krävs per disk.</span><span class="sxs-lookup"><span data-stu-id="6bc09-121">One VHD is needed per disk.</span></span> <span data-ttu-id="6bc09-122">Även tomt datadiskar kräver en VHD-toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-122">Even blank data disks require a VHD toobe created.</span></span>

<span data-ttu-id="6bc09-123">Oavsett vilket operativsystem som du använder, lägga till endast hello minsta antal datadiskar som krävs av hello SKU.</span><span class="sxs-lookup"><span data-stu-id="6bc09-123">Regardless of which operating system you use, add only hello minimum number of data disks needed by hello SKU.</span></span> <span data-ttu-id="6bc09-124">Kunder kan inte ta bort diskar som är en del av en avbildning vid hello tiden för distributionen, men kan alltid lägga till diskar under eller efter distributionen om de behöver dem..</span><span class="sxs-lookup"><span data-stu-id="6bc09-124">Customers cannot remove disks that are part of an image at hello time of deployment but can always add disks during or after deployment if they need them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6bc09-125">**Ändra inte Diskantalet i en ny Avbildningsversion.**</span><span class="sxs-lookup"><span data-stu-id="6bc09-125">**Do not change disk count in a new image version.**</span></span> <span data-ttu-id="6bc09-126">Om du måste konfigurera om datadiskar hello bild, definiera en ny SKU.</span><span class="sxs-lookup"><span data-stu-id="6bc09-126">If you must reconfigure Data disks in hello image, define a new SKU.</span></span> <span data-ttu-id="6bc09-127">En ny version för avbildningen med annan disk antal har hello risken för att dela nya distribution baserad på hello ny Avbildningsversion i fall av automatisk skalning, automatisk distribution av lösningar via ARM-mallar och andra scenarier.</span><span class="sxs-lookup"><span data-stu-id="6bc09-127">Publishing a new image version with different disk counts will have hello potential of breaking new deployment based on hello new image version in cases of auto-scaling, automatic deployments of solutions through ARM templates and other scenarios.</span></span>
>
>

### <a name="11-add-an-offer"></a><span data-ttu-id="6bc09-128">1.1 Lägg till ett erbjudande</span><span class="sxs-lookup"><span data-stu-id="6bc09-128">1.1 Add an offer</span></span>
1. <span data-ttu-id="6bc09-129">Logga in toohello [Publiceringsportal] [ link-pubportal] med säljaren-konto.</span><span class="sxs-lookup"><span data-stu-id="6bc09-129">Sign in toohello [Publishing Portal][link-pubportal] by using your seller account.</span></span>
2. <span data-ttu-id="6bc09-130">Välj hello **virtuella datorer** fliken hello Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="6bc09-130">Select hello **Virtual Machines** tab of hello Publishing Portal.</span></span> <span data-ttu-id="6bc09-131">Ange ditt erbjudande i hello ange fältet.</span><span class="sxs-lookup"><span data-stu-id="6bc09-131">In hello prompted entry field, enter your offer name.</span></span> <span data-ttu-id="6bc09-132">hello är erbjudandet vanligtvis hello namn för hello produkt eller tjänst som du planerar toosell i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6bc09-132">hello offer name is typically hello name of hello product or service that you plan toosell in hello Azure Marketplace.</span></span>
3. <span data-ttu-id="6bc09-133">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-133">Select **Create**.</span></span>

### <a name="12-define-a-sku"></a><span data-ttu-id="6bc09-134">1.2 definiera en SKU</span><span class="sxs-lookup"><span data-stu-id="6bc09-134">1.2 Define a SKU</span></span>
<span data-ttu-id="6bc09-135">När du har lagt till ett erbjudande, du behöver toodefine och identifiera din SKU: er.</span><span class="sxs-lookup"><span data-stu-id="6bc09-135">After you have added an offer, you need toodefine and identify your SKUs.</span></span> <span data-ttu-id="6bc09-136">Du kan ha flera erbjudanden och varje erbjudande kan ha flera SKU: er under den.</span><span class="sxs-lookup"><span data-stu-id="6bc09-136">You can have multiple offers, and each offer can have multiple SKUs under it.</span></span> <span data-ttu-id="6bc09-137">När ett erbjudande pushas toostaging pushas tillsammans med alla dess SKU: er.</span><span class="sxs-lookup"><span data-stu-id="6bc09-137">When an offer is pushed toostaging, it is pushed along with all of its SKUs.</span></span>

1. <span data-ttu-id="6bc09-138">**Lägg till en SKU.**</span><span class="sxs-lookup"><span data-stu-id="6bc09-138">**Add a SKU.**</span></span> <span data-ttu-id="6bc09-139">hello SKU kräver en identifierare som används i hello-URL.</span><span class="sxs-lookup"><span data-stu-id="6bc09-139">hello SKU requires an identifier, which is used in hello URL.</span></span> <span data-ttu-id="6bc09-140">hello-ID måste vara unikt inom din publiceringsprofil, men det finns ingen risk för identifierare kollision med andra utgivare.</span><span class="sxs-lookup"><span data-stu-id="6bc09-140">hello identifier must be unique within your publishing profile, but there is no risk of identifier collision with other publishers.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6bc09-141">hello erbjudandet och SKU-ID visas i URL: en i hello erbjudandet i hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6bc09-141">hello offer and SKU identifiers are displayed in hello offer URL in hello Marketplace.</span></span>
   >
   >
2. <span data-ttu-id="6bc09-142">**Lägga till en sammanfattande beskrivning för din SKU.**</span><span class="sxs-lookup"><span data-stu-id="6bc09-142">**Add a summary description for your SKU.**</span></span> <span data-ttu-id="6bc09-143">Översikt över beskrivningar är synliga toocustomers så bör du se dem lättläst.</span><span class="sxs-lookup"><span data-stu-id="6bc09-143">Summary descriptions are visible toocustomers, so you should make them easily readable.</span></span> <span data-ttu-id="6bc09-144">Den här informationen behövs inte toobe låst tills hello ”Push tooStaging” fas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-144">This information does not need toobe locked until hello "Push tooStaging" phase.</span></span> <span data-ttu-id="6bc09-145">Fram till dess kan du är ledigt tooedit den.</span><span class="sxs-lookup"><span data-stu-id="6bc09-145">Until then, you are free tooedit it.</span></span>
3. <span data-ttu-id="6bc09-146">Om du använder Windows-baserade SKU: er kan du följa hello föreslagna länkar tooacquire hello godkända versioner av Windows Server.</span><span class="sxs-lookup"><span data-stu-id="6bc09-146">If you are using Windows-based SKUs, follow hello suggested links tooacquire hello approved versions of Windows Server.</span></span>

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a><span data-ttu-id="6bc09-147">2. Skapa en virtuell Hårddisk på Azure-kompatibel (Linux-baserat)</span><span class="sxs-lookup"><span data-stu-id="6bc09-147">2. Create an Azure-compatible VHD (Linux-based)</span></span>
<span data-ttu-id="6bc09-148">Det här avsnittet fokuserar på bästa praxis för att skapa en Linux-baserade VM-avbildning för hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6bc09-148">This section focuses on best practices for creating a Linux-based VM image for hello Azure Marketplace.</span></span> <span data-ttu-id="6bc09-149">En stegvis genomgång finns toohello följande dokumentation: [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="6bc09-149">For a step-by-step walkthrough, refer toohello following documentation: [Creating and Uploading a Virtual Hard Disk that Contains hello Linux Operating System](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a><span data-ttu-id="6bc09-150">3. Skapa en virtuell Hårddisk på Azure-kompatibel (Windows-baserade)</span><span class="sxs-lookup"><span data-stu-id="6bc09-150">3. Create an Azure-compatible VHD (Windows-based)</span></span>
<span data-ttu-id="6bc09-151">Det här avsnittet fokuserar på hello steg toocreate en SKU baserat på Windows Server för hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6bc09-151">This section focuses on hello steps toocreate a SKU based on Windows Server for hello Azure Marketplace.</span></span>

### <a name="31-ensure-that-you-are-using-hello-correct-base-vhds"></a><span data-ttu-id="6bc09-152">3.1 se till att du använder hello korrigera grundläggande virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="6bc09-152">3.1 Ensure that you are using hello correct base VHDs</span></span>
<span data-ttu-id="6bc09-153">hello operativsystemet VHD för VM-avbildning måste baseras på en Azure-godkända basavbildning som innehåller Windows Server eller SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6bc09-153">hello operating system VHD for your VM image must be based on an Azure-approved base image that contains Windows Server or SQL Server.</span></span>

<span data-ttu-id="6bc09-154">toobegin, skapa en virtuell dator från en av följande avbildningar som finns på hello hello [Microsoft Azure-portalen][link-azure-portal]:</span><span class="sxs-lookup"><span data-stu-id="6bc09-154">toobegin, create a VM from one of hello following images, located at hello [Microsoft Azure portal][link-azure-portal]:</span></span>

* <span data-ttu-id="6bc09-155">Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1][link-datactr-2008-r2])</span><span class="sxs-lookup"><span data-stu-id="6bc09-155">Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1][link-datactr-2008-r2])</span></span>
* <span data-ttu-id="6bc09-156">SQLServer 2014 ([Enterprise][link-sql-2014-ent], [Standard][link-sql-2014-std], [Web][link-sql-2014-web])</span><span class="sxs-lookup"><span data-stu-id="6bc09-156">SQL Server 2014 ([Enterprise][link-sql-2014-ent], [Standard][link-sql-2014-std], [Web][link-sql-2014-web])</span></span>
* <span data-ttu-id="6bc09-157">SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [Standard][link-sql-2012-std], [Web][link-sql-2012-web])</span><span class="sxs-lookup"><span data-stu-id="6bc09-157">SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [Standard][link-sql-2012-std], [Web][link-sql-2012-web])</span></span>

<span data-ttu-id="6bc09-158">Dessa länkar kan också finnas i hello Publishing Portal under hello SKU sida.</span><span class="sxs-lookup"><span data-stu-id="6bc09-158">These links can also be found in hello Publishing Portal under hello SKU page.</span></span>

> [!TIP]
> <span data-ttu-id="6bc09-159">Om du använder hello aktuella Azure-portalen eller PowerShell, har Windows Server-avbildningar publicerade på 8 September 2014 och senare godkänts.</span><span class="sxs-lookup"><span data-stu-id="6bc09-159">If you are using hello current Azure portal or PowerShell, Windows Server images published on September 8, 2014 and later are approved.</span></span>
>
>

### <a name="32-create-your-windows-based-vm"></a><span data-ttu-id="6bc09-160">3.2 Skapa ditt Windows-baserad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="6bc09-160">3.2 Create your Windows-based VM</span></span>
<span data-ttu-id="6bc09-161">Du kan skapa den virtuella datorn baserat på en godkänd basavbildning i några enkla steg från hello Microsoft Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-161">From hello Microsoft Azure portal, you can create your VM based on an approved base image in just a few simple steps.</span></span> <span data-ttu-id="6bc09-162">hello följande är en översikt över hello processen:</span><span class="sxs-lookup"><span data-stu-id="6bc09-162">hello following is an overview of hello process:</span></span>

1. <span data-ttu-id="6bc09-163">Välj hello basavbildning sidan **Skapa virtuell dator** toobe dirigeras toohello nya [Microsoft Azure-portalen][link-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="6bc09-163">From hello base image page, select **Create Virtual Machine** toobe directed toohello new [Microsoft Azure portal][link-azure-portal].</span></span>

    ![Rita][img-acom-1]
2. <span data-ttu-id="6bc09-165">Logga in toohello portal med hello Microsoft-konto och lösenord för hello Azure-prenumeration du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="6bc09-165">Sign in toohello portal with hello Microsoft account and password for hello Azure subscription you want toouse.</span></span>
3. <span data-ttu-id="6bc09-166">Följ hello prompter toocreate en virtuell dator med hjälp av hello basavbildning som du har valt.</span><span class="sxs-lookup"><span data-stu-id="6bc09-166">Follow hello prompts toocreate a VM by using hello base image you have selected.</span></span> <span data-ttu-id="6bc09-167">Behöver du tooprovide ett värdnamn (namnet på hello dator), användarnamn (registrerad som administratör) och lösenord för hello VM.</span><span class="sxs-lookup"><span data-stu-id="6bc09-167">You need tooprovide a host name (name of hello computer), user name (registered as an administrator), and password for hello VM.</span></span>

    ![Rita][img-portal-vm-create]
4. <span data-ttu-id="6bc09-169">Välj hello storleken på hello VM toodeploy:</span><span class="sxs-lookup"><span data-stu-id="6bc09-169">Select hello size of hello VM toodeploy:</span></span>

    <span data-ttu-id="6bc09-170">a.</span><span class="sxs-lookup"><span data-stu-id="6bc09-170">a.</span></span>    <span data-ttu-id="6bc09-171">Om du planerar toodevelop hello VHD lokala spelar hello storlek ingen roll.</span><span class="sxs-lookup"><span data-stu-id="6bc09-171">If you plan toodevelop hello VHD on-premises, hello size does not matter.</span></span> <span data-ttu-id="6bc09-172">Överväg att använda en av hello mindre virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6bc09-172">Consider using one of hello smaller VMs.</span></span>

    <span data-ttu-id="6bc09-173">b.</span><span class="sxs-lookup"><span data-stu-id="6bc09-173">b.</span></span>    <span data-ttu-id="6bc09-174">Om du planerar toodevelop hello avbildning i Azure bör du bör använda en av hello VM-storlekar för hello valda avbildningen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-174">If you plan toodevelop hello image in Azure, consider using one of hello recommended VM sizes for hello selected image.</span></span>

    <span data-ttu-id="6bc09-175">c.</span><span class="sxs-lookup"><span data-stu-id="6bc09-175">c.</span></span>    <span data-ttu-id="6bc09-176">Information om priser finns toohello **rekommenderas prisnivåer** selector visas på hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-176">For pricing information, refer toohello **Recommended pricing tiers** selector displayed on hello portal.</span></span> <span data-ttu-id="6bc09-177">Det ger hello tre rekommenderade storlekar som tillhandahålls av hello utgivare.</span><span class="sxs-lookup"><span data-stu-id="6bc09-177">It will provide hello three recommended sizes provided by hello publisher.</span></span> <span data-ttu-id="6bc09-178">(I det här fallet hello utgivaren är Microsoft)</span><span class="sxs-lookup"><span data-stu-id="6bc09-178">(In this case, hello publisher is Microsoft.)</span></span>

    ![Rita][img-portal-vm-size]
5. <span data-ttu-id="6bc09-180">Ange egenskaper för:</span><span class="sxs-lookup"><span data-stu-id="6bc09-180">Set properties:</span></span>

    <span data-ttu-id="6bc09-181">a.</span><span class="sxs-lookup"><span data-stu-id="6bc09-181">a.</span></span>    <span data-ttu-id="6bc09-182">Snabb distribution kan du lämna hello standardvärden för hello under **valfri konfiguration** och **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-182">For quick deployment, you can leave hello default values for hello properties under **Optional Configuration** and **Resource Group**.</span></span>

    <span data-ttu-id="6bc09-183">b.</span><span class="sxs-lookup"><span data-stu-id="6bc09-183">b.</span></span>    <span data-ttu-id="6bc09-184">Under **Lagringskonto**, du kan också välja hello storage-konto som hello operativsystemet virtuella Hårddisken ska sparas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-184">Under **Storage Account**, you can optionally select hello storage account in which hello operating system VHD will be stored.</span></span>

    <span data-ttu-id="6bc09-185">c.</span><span class="sxs-lookup"><span data-stu-id="6bc09-185">c.</span></span>    <span data-ttu-id="6bc09-186">Under **resursgruppen**, du kan också välja hello logisk grupp i vilka tooplace hello VM.</span><span class="sxs-lookup"><span data-stu-id="6bc09-186">Under **Resource Group**, you can optionally select hello logical group in which tooplace hello VM.</span></span>
6. <span data-ttu-id="6bc09-187">Välj hello **plats** för distribution:</span><span class="sxs-lookup"><span data-stu-id="6bc09-187">Select hello **Location** for deployment:</span></span>

    <span data-ttu-id="6bc09-188">a.</span><span class="sxs-lookup"><span data-stu-id="6bc09-188">a.</span></span>    <span data-ttu-id="6bc09-189">Om du planerar toodevelop hello VHD lokala roll inte hello plats eftersom du kommer att överföra hello avbildningen tooAzure senare.</span><span class="sxs-lookup"><span data-stu-id="6bc09-189">If you plan toodevelop hello VHD on-premises, hello location does not matter because you will upload hello image tooAzure later.</span></span>

    <span data-ttu-id="6bc09-190">b.</span><span class="sxs-lookup"><span data-stu-id="6bc09-190">b.</span></span>    <span data-ttu-id="6bc09-191">Om du planerar toodevelop hello avbildning i Azure, Överväg att använda någon av hello USA-baserade Microsoft Azure-regioner från hello början.</span><span class="sxs-lookup"><span data-stu-id="6bc09-191">If you plan toodevelop hello image in Azure, consider using one of hello US-based Microsoft Azure regions from hello beginning.</span></span> <span data-ttu-id="6bc09-192">Detta förbättrar hello VHD kopieringen som Microsoft utför åt dig när du skickar avbildningen för certifiering.</span><span class="sxs-lookup"><span data-stu-id="6bc09-192">This speeds up hello VHD copying process that Microsoft performs on your behalf when you submit your image for certification.</span></span>

    ![Rita][img-portal-vm-location]
7. <span data-ttu-id="6bc09-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-194">Click **Create**.</span></span> <span data-ttu-id="6bc09-195">hello VM startar toodeploy.</span><span class="sxs-lookup"><span data-stu-id="6bc09-195">hello VM starts toodeploy.</span></span> <span data-ttu-id="6bc09-196">I minuter, får en lyckad distribution och kan börja toocreate hello avbildning för din SKU.</span><span class="sxs-lookup"><span data-stu-id="6bc09-196">Within minutes, you will have a successful deployment and can begin toocreate hello image for your SKU.</span></span>

### <a name="33-develop-your-vhd-in-hello-cloud"></a><span data-ttu-id="6bc09-197">3.3 utveckla den virtuella Hårddisken i hello moln</span><span class="sxs-lookup"><span data-stu-id="6bc09-197">3.3 Develop your VHD in hello cloud</span></span>
<span data-ttu-id="6bc09-198">Vi rekommenderar starkt att du utveckla den virtuella Hårddisken i hello molnet med hjälp av Remote Desktop Protocol (RDP).</span><span class="sxs-lookup"><span data-stu-id="6bc09-198">We strongly recommend that you develop your VHD in hello cloud by using Remote Desktop Protocol (RDP).</span></span> <span data-ttu-id="6bc09-199">Du kan ansluta tooRDP med hello användarnamn och lösenord som angetts under etableringen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-199">You connect tooRDP with hello user name and password specified during provisioning.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6bc09-200">Om du utvecklar dina VHD lokalt (vilket inte rekommenderas), se [och skapa en avbildning av virtuell dator lokalt](marketplace-publishing-vm-image-creation-on-premise.md).</span><span class="sxs-lookup"><span data-stu-id="6bc09-200">If you develop your VHD on-premises (which is not recommended), see [Creating a virtual machine image on-premises](marketplace-publishing-vm-image-creation-on-premise.md).</span></span> <span data-ttu-id="6bc09-201">Hämta den virtuella Hårddisken är inte nödvändigt om du utvecklar i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="6bc09-201">Downloading your VHD is not necessary if you are developing in hello cloud.</span></span>
>
>

<span data-ttu-id="6bc09-202">**Anslut via RDP med hello [Microsoft Azure-portalen][link-azure-portal]**</span><span class="sxs-lookup"><span data-stu-id="6bc09-202">**Connect via RDP using hello [Microsoft Azure portal][link-azure-portal]**</span></span>

1. <span data-ttu-id="6bc09-203">Välj **Bläddra** > **VMs**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-203">Select **Browse** > **VMs**.</span></span>
2. <span data-ttu-id="6bc09-204">hello virtuella datorer blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-204">hello Virtual machines blade opens.</span></span> <span data-ttu-id="6bc09-205">Kontrollera att hello VM som du vill tooconnect med körs och markera den hello listan med distribuerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6bc09-205">Ensure that hello VM that you want tooconnect with is running, and then select it from hello list of deployed VMs.</span></span>
3. <span data-ttu-id="6bc09-206">Ett blad öppnas som beskriver hello valt VM.</span><span class="sxs-lookup"><span data-stu-id="6bc09-206">A blade opens that describes hello selected VM.</span></span> <span data-ttu-id="6bc09-207">Hello överkant, klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-207">At hello top, click **Connect**.</span></span>
4. <span data-ttu-id="6bc09-208">Du kan ange tooenter hello användarnamn och lösenord som du angav under etableringen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-208">You are prompted tooenter hello user name and password that you specified during provisioning.</span></span>

<span data-ttu-id="6bc09-209">**Anslut via RDP med hjälp av PowerShell**</span><span class="sxs-lookup"><span data-stu-id="6bc09-209">**Connect via RDP using PowerShell**</span></span>

<span data-ttu-id="6bc09-210">toodownload en fjärrfil skrivbord tooa lokala dator använder hello [cmdlet Get-AzureRemoteDesktopFile][link-technet-2].</span><span class="sxs-lookup"><span data-stu-id="6bc09-210">toodownload a remote desktop file tooa local machine, use hello [Get-AzureRemoteDesktopFile cmdlet][link-technet-2].</span></span> <span data-ttu-id="6bc09-211">I ordning toouse denna cmdlet måste du tooknow hello namnet på hello-tjänst och namnet på hello VM.</span><span class="sxs-lookup"><span data-stu-id="6bc09-211">In order toouse this cmdlet, you need tooknow hello name of hello service and name of hello VM.</span></span> <span data-ttu-id="6bc09-212">Om du har skapat hello VM från hello [Microsoft Azure-portalen][link-azure-portal], du kan hitta den här informationen under Egenskaper för Virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="6bc09-212">If you created hello VM from hello [Microsoft Azure portal][link-azure-portal], you can find this information under VM properties:</span></span>

1. <span data-ttu-id="6bc09-213">I hello Microsoft Azure portal, väljer **Bläddra** > **VMs**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-213">In hello Microsoft Azure portal, select **Browse** > **VMs**.</span></span>
2. <span data-ttu-id="6bc09-214">hello virtuella datorer blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-214">hello Virtual machines blade opens.</span></span> <span data-ttu-id="6bc09-215">Välj hello VM som du har distribuerat.</span><span class="sxs-lookup"><span data-stu-id="6bc09-215">Select hello VM that you deployed.</span></span>
3. <span data-ttu-id="6bc09-216">Ett blad öppnas som beskriver hello valt VM.</span><span class="sxs-lookup"><span data-stu-id="6bc09-216">A blade opens that describes hello selected VM.</span></span>
4. <span data-ttu-id="6bc09-217">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-217">Click **Properties**.</span></span>
5. <span data-ttu-id="6bc09-218">hello första delen av hello domännamn är hello tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="6bc09-218">hello first portion of hello domain name is hello service name.</span></span> <span data-ttu-id="6bc09-219">hello är värden hello VM-namn.</span><span class="sxs-lookup"><span data-stu-id="6bc09-219">hello host name is hello VM name.</span></span>

    ![Rita][img-portal-vm-rdp]
6. <span data-ttu-id="6bc09-221">hello cmdlet toodownload hello RDP-filen för hello skapade VM toohello administratörens lokala dator är som följer.</span><span class="sxs-lookup"><span data-stu-id="6bc09-221">hello cmdlet toodownload hello RDP file for hello created VM toohello administrator's local desktop is as follows.</span></span>

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

<span data-ttu-id="6bc09-222">Mer information om RDP kan hittas på MSDN i hello artikeln [ansluta tooan virtuella Azure-datorn med RDP eller SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).</span><span class="sxs-lookup"><span data-stu-id="6bc09-222">More information about RDP can be found on MSDN in hello article [Connect tooan Azure VM with RDP or SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).</span></span>

<span data-ttu-id="6bc09-223">**Konfigurera en virtuell dator och skapa din SKU**</span><span class="sxs-lookup"><span data-stu-id="6bc09-223">**Configure a VM and create your SKU**</span></span>

<span data-ttu-id="6bc09-224">Använd Hyper-v efter hello operativsystemet VHD hämtas och konfigurera en VM-toobegin skapar din SKU.</span><span class="sxs-lookup"><span data-stu-id="6bc09-224">After hello operating system VHD is downloaded, use Hyper­V and configure a VM toobegin creating your SKU.</span></span> <span data-ttu-id="6bc09-225">Detaljerade anvisningar finns på följande länk för TechNet hello: [installera Hyper-v och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="6bc09-225">Detailed steps can be found at hello following TechNet link: [Install Hyper­V and Configure a VM](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="34-choose-hello-correct-vhd-size"></a><span data-ttu-id="6bc09-226">3.4 Välj hello rätt storlek för virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="6bc09-226">3.4 Choose hello correct VHD size</span></span>
<span data-ttu-id="6bc09-227">hello Windows-operativsystem VHD i VM-avbildning ska skapas som en virtuell Hårddisk fast format 128 GB.</span><span class="sxs-lookup"><span data-stu-id="6bc09-227">hello Windows operating system VHD in your VM image should be created as a 128-GB fixed-format VHD.</span></span>  

<span data-ttu-id="6bc09-228">Om hello fysiska storlek är mindre än 128 GB, vara hello VHD sparse.</span><span class="sxs-lookup"><span data-stu-id="6bc09-228">If hello physical size is less than 128 GB, hello VHD should be sparse.</span></span> <span data-ttu-id="6bc09-229">hello grundläggande Windows och SQL Server-avbildningar som uppfyller redan kraven, så ändra inte hello-format eller hello storleken på hello VHD hämtades.</span><span class="sxs-lookup"><span data-stu-id="6bc09-229">hello base Windows and SQL Server images provided already meet these requirements, so do not change hello format or hello size of hello VHD obtained.</span></span>  

<span data-ttu-id="6bc09-230">Datadiskar kan vara så stor som 1 TB.</span><span class="sxs-lookup"><span data-stu-id="6bc09-230">Data disks can be as large as 1 TB.</span></span> <span data-ttu-id="6bc09-231">Kom ihåg att kunder inte ändra storlek på virtuella hårddiskar i en bild när hello distribution när du funderar över hello diskstorleken.</span><span class="sxs-lookup"><span data-stu-id="6bc09-231">When deciding on hello disk size, remember that customers cannot resize VHDs within an image at hello time of deployment.</span></span> <span data-ttu-id="6bc09-232">Data disk virtuella hårddiskar ska skapas som en virtuell Hårddisk med fast format.</span><span class="sxs-lookup"><span data-stu-id="6bc09-232">Data disk VHDs should be created as a fixed-format VHD.</span></span> <span data-ttu-id="6bc09-233">De bör också vara sparse.</span><span class="sxs-lookup"><span data-stu-id="6bc09-233">They should also be sparse.</span></span> <span data-ttu-id="6bc09-234">Datadiskar kan antingen vara tomma eller innehålla data.</span><span class="sxs-lookup"><span data-stu-id="6bc09-234">Data disks can be empty or contain data.</span></span>

### <a name="35-install-hello-latest-windows-patches"></a><span data-ttu-id="6bc09-235">3.5 Installera hello senaste korrigeringarna i Windows</span><span class="sxs-lookup"><span data-stu-id="6bc09-235">3.5 Install hello latest Windows patches</span></span>
<span data-ttu-id="6bc09-236">hello grundläggande avbildningar innehåller hello senaste korrigeringarna in tootheir publiceringsdatum.</span><span class="sxs-lookup"><span data-stu-id="6bc09-236">hello base images contain hello latest patches up tootheir published date.</span></span> <span data-ttu-id="6bc09-237">Se till att Windows Update har körts och att alla hello senaste kritiska och viktiga uppdateringar har installerats innan du publicerar hello operativsystemet VHD som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="6bc09-237">Before publishing hello operating system VHD you have created, ensure that Windows Update has been run and that all hello latest Critical and Important security updates have been installed.</span></span>

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a><span data-ttu-id="6bc09-238">3,6 utföra ytterligare aktiviteter för konfiguration och schema vid behov</span><span class="sxs-lookup"><span data-stu-id="6bc09-238">3.6 Perform additional configuration and schedule tasks as necessary</span></span>
<span data-ttu-id="6bc09-239">Överväg att använda en schemalagd aktivitet som körs vid start toomake alla slutgiltiga ändringar toohello VM när den har distribuerats om ytterligare konfiguration krävs:</span><span class="sxs-lookup"><span data-stu-id="6bc09-239">If additional configuration is needed, consider using a scheduled task that runs at startup toomake any final changes toohello VM after it has been deployed:</span></span>

* <span data-ttu-id="6bc09-240">Det är en bästa praxis toohave hello aktiviteten Ta bort själva vid har körts.</span><span class="sxs-lookup"><span data-stu-id="6bc09-240">It is a best practice toohave hello task delete itself upon successful execution.</span></span>
* <span data-ttu-id="6bc09-241">Ingen konfiguration bör lita på enheter än enheter C eller D, eftersom de är bara två hello-enheter som alltid är garanterat tooexist.</span><span class="sxs-lookup"><span data-stu-id="6bc09-241">No configuration should rely on drives other than drives C or D, because these are hello only two drives that are always guaranteed tooexist.</span></span> <span data-ttu-id="6bc09-242">Enhet C är hello operativsystemdisken och enhet D är hello tillfälliga lokal disk.</span><span class="sxs-lookup"><span data-stu-id="6bc09-242">Drive C is hello operating system disk, and drive D is hello temporary local disk.</span></span>

### <a name="37-generalize-hello-image"></a><span data-ttu-id="6bc09-243">3.7 generalisera hello-bild</span><span class="sxs-lookup"><span data-stu-id="6bc09-243">3.7 Generalize hello image</span></span>
<span data-ttu-id="6bc09-244">Alla avbildningar i hello Azure Marketplace måste vara återanvändbara i ett allmänt sätt.</span><span class="sxs-lookup"><span data-stu-id="6bc09-244">All images in hello Azure Marketplace must be reusable in a generic fashion.</span></span> <span data-ttu-id="6bc09-245">Med andra ord måste hello operativsystemet VHD vara generaliserad:</span><span class="sxs-lookup"><span data-stu-id="6bc09-245">In other words, hello operating system VHD must be generalized:</span></span>

* <span data-ttu-id="6bc09-246">För Windows hello bilden ska vara ”Sysprep” och inga konfigurationer bör göras som inte stöder hello **sysprep** kommando.</span><span class="sxs-lookup"><span data-stu-id="6bc09-246">For Windows, hello image should be "sysprepped," and no configurations should be done that do not support hello **sysprep** command.</span></span>
* <span data-ttu-id="6bc09-247">Du kan köra följande kommando från hello katalogen % windir%\System32\Sysprep hello.</span><span class="sxs-lookup"><span data-stu-id="6bc09-247">You can run hello following command from hello directory %windir%\System32\Sysprep.</span></span>

        sysprep.exe /generalize /oobe /shutdown

  <span data-ttu-id="6bc09-248">Information om hur toosysprep hello operativsystem finns i steg i följande MSDN-artikel hello: [skapa och ladda upp en Windows Server VHD-tooAzure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6bc09-248">Guidance on how toosysprep hello operating system is provided in Step of hello following MSDN article: [Create and upload a Windows Server VHD tooAzure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="4-deploy-a-vm-from-your-vhds"></a><span data-ttu-id="6bc09-249">4. Distribuera en virtuell dator från din virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="6bc09-249">4. Deploy a VM from your VHDs</span></span>
<span data-ttu-id="6bc09-250">När du har laddat upp de virtuella hårddiskarna (operativsystem hello generaliserad virtuell Hårddisk och noll eller fler data disk virtuella hårddiskar) tooan Azure storage-konto kan du registrera dem som en användare VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="6bc09-250">After you have uploaded your VHDs (hello generalized operating system VHD and zero or more data disk VHDs) tooan Azure storage account, you can register them as a user VM image.</span></span> <span data-ttu-id="6bc09-251">Du kan sedan testa avbildningen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-251">Then you can test that image.</span></span> <span data-ttu-id="6bc09-252">Observera att eftersom operativsystemet VHD är generaliserad, du inte kan direkt distribuera hello VM genom att tillhandahålla hello VHD URL.</span><span class="sxs-lookup"><span data-stu-id="6bc09-252">Note that because your operating system VHD is generalized, you cannot directly deploy hello VM by providing hello VHD URL.</span></span>

<span data-ttu-id="6bc09-253">toolearn mer om VM-avbildningar, granska hello följande blogginlägg:</span><span class="sxs-lookup"><span data-stu-id="6bc09-253">toolearn more about VM images, review hello following blog posts:</span></span>

* [<span data-ttu-id="6bc09-254">VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="6bc09-254">VM Image</span></span>](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [<span data-ttu-id="6bc09-255">VM avbildningen PowerShell så</span><span class="sxs-lookup"><span data-stu-id="6bc09-255">VM Image PowerShell How To</span></span>](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [<span data-ttu-id="6bc09-256">Om VM-avbildningar i Azure</span><span class="sxs-lookup"><span data-stu-id="6bc09-256">About VM images in Azure</span></span>](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-hello-necessary-tools-powershell-and-azure-cli"></a><span data-ttu-id="6bc09-257">Ställ in hello verktyg som krävs, PowerShell och Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6bc09-257">Set up hello necessary tools, PowerShell and Azure CLI</span></span>
* [<span data-ttu-id="6bc09-258">Hur toosetup PowerShell</span><span class="sxs-lookup"><span data-stu-id="6bc09-258">How toosetup PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="6bc09-259">Hur toosetup Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6bc09-259">How toosetup Azure CLI</span></span>](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a><span data-ttu-id="6bc09-260">4.1 skapa en användare VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="6bc09-260">4.1 Create a user VM image</span></span>
#### <a name="capture-vm"></a><span data-ttu-id="6bc09-261">Spela in VM</span><span class="sxs-lookup"><span data-stu-id="6bc09-261">Capture VM</span></span>
<span data-ttu-id="6bc09-262">Läs hello länkar som anges nedan anvisningar om hur du fångar hello VM med hjälp av Azure-API/PowerShell CLI.</span><span class="sxs-lookup"><span data-stu-id="6bc09-262">Please read hello links given below for guidance on capturing hello VM using API/PowerShell/Azure CLI.</span></span>

* [<span data-ttu-id="6bc09-263">API</span><span class="sxs-lookup"><span data-stu-id="6bc09-263">API</span></span>](https://msdn.microsoft.com/library/mt163560.aspx)
* [<span data-ttu-id="6bc09-264">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6bc09-264">PowerShell</span></span>](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="6bc09-265">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6bc09-265">Azure CLI</span></span>](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a><span data-ttu-id="6bc09-266">Generalisera avbildningen</span><span class="sxs-lookup"><span data-stu-id="6bc09-266">Generalize Image</span></span>
<span data-ttu-id="6bc09-267">Läs hello länkar som anges nedan anvisningar om hur du fångar hello VM med hjälp av Azure-API/PowerShell CLI.</span><span class="sxs-lookup"><span data-stu-id="6bc09-267">Please read hello links given below for guidance on capturing hello VM using API/PowerShell/Azure CLI.</span></span>

* [<span data-ttu-id="6bc09-268">API</span><span class="sxs-lookup"><span data-stu-id="6bc09-268">API</span></span>](https://msdn.microsoft.com/library/mt269439.aspx)
* [<span data-ttu-id="6bc09-269">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6bc09-269">PowerShell</span></span>](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="6bc09-270">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6bc09-270">Azure CLI</span></span>](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a><span data-ttu-id="6bc09-271">4.2 distribuera en virtuell dator från en användare VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="6bc09-271">4.2 Deploy a VM from a user VM image</span></span>
<span data-ttu-id="6bc09-272">toodeploy en virtuell dator från en användare VM-avbildning som du kan använda hello aktuella [Azure-portalen](https://manage.windowsazure.com) eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6bc09-272">toodeploy a VM from a user VM image, you can use hello current [Azure portal](https://manage.windowsazure.com) or PowerShell.</span></span>

<span data-ttu-id="6bc09-273">**Distribuera en virtuell dator från hello aktuella Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="6bc09-273">**Deploy a VM from hello current Azure portal**</span></span>

1. <span data-ttu-id="6bc09-274">Gå för**ny** > **Compute** > **virtuella** > **från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-274">Go too**New** > **Compute** > **Virtual machine** > **From gallery**.</span></span>

    ![Rita][img-manage-vm-new]
2. <span data-ttu-id="6bc09-276">Gå för**Mina avbildningar**, och sedan hello Välj VM-avbildning från vilken toodeploy en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="6bc09-276">Go too**My images**, and then select hello VM image from which toodeploy a VM:</span></span>

   1. <span data-ttu-id="6bc09-277">Betala per uppmärksam toowhich bilden, eftersom hello **Mina avbildningar** visas både avbildningar av operativsystem och VM-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-277">Pay close attention toowhich image you select, because hello **My images** view lists both operating system images and VM images.</span></span>
   2. <span data-ttu-id="6bc09-278">Titta på hello antalet diskar kan hjälpa avgöra vilken typ av bild som du distribuerar, eftersom hello majoriteten av VM-avbildningar har mer än en disk.</span><span class="sxs-lookup"><span data-stu-id="6bc09-278">Looking at hello number of disks can help determine what type of image you are deploying, because hello majority of VM images have more than one disk.</span></span> <span data-ttu-id="6bc09-279">Det är dock fortfarande möjligt toohave en VM-avbildning med bara en enda disk, som sedan måste **antalet diskar** ange too1.</span><span class="sxs-lookup"><span data-stu-id="6bc09-279">However, it is still possible toohave a VM image with only a single operating system disk, which would then have **Number of disks** set too1.</span></span>

      ![Rita][img-manage-vm-select]
3. <span data-ttu-id="6bc09-281">Följ guiden för hello VM skapa och ange hello VM-namn, VM-storlek, plats, användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="6bc09-281">Follow hello VM creation wizard and specify hello VM name, VM size, location, user name, and password.</span></span>

<span data-ttu-id="6bc09-282">**Distribuera en virtuell dator från PowerShell**</span><span class="sxs-lookup"><span data-stu-id="6bc09-282">**Deploy a VM from PowerShell**</span></span>

<span data-ttu-id="6bc09-283">Du kan använda hello följande cmdlets toodeploy som en stor virtuell dator från hello generaliserad VM-avbildning som precis har skapat.</span><span class="sxs-lookup"><span data-stu-id="6bc09-283">toodeploy a large VM from hello generalized VM image just created, you can use hello following cmdlets.</span></span>

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> <span data-ttu-id="6bc09-284">Se [felsöka vanliga problem som påträffas under skapande av VHD] för ytterligare hjälp.</span><span class="sxs-lookup"><span data-stu-id="6bc09-284">Please refer [Troubleshooting common issues encountered during VHD creation] for additional assistance.</span></span>
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a><span data-ttu-id="6bc09-285">5. Hämta certifikat för VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="6bc09-285">5. Obtain certification for your VM image</span></span>
<span data-ttu-id="6bc09-286">hello nästa steg förbereder VM-avbildning för hello Azure Marketplace är den certifierade toohave.</span><span class="sxs-lookup"><span data-stu-id="6bc09-286">hello next step in preparing your VM image for hello Azure Marketplace is toohave it certified.</span></span>

<span data-ttu-id="6bc09-287">Den här processen omfattar att köra ett verktyg för särskilda certifiering, överför hello verifiering resultat toohello Azure-behållaren där de virtuella hårddiskarna finnas, lägga till ett erbjudande, definiera din SKU, och skickar den virtuella datorn image för certifiering.</span><span class="sxs-lookup"><span data-stu-id="6bc09-287">This process includes running a special certification tool, uploading hello verification results toohello Azure container where your VHDs reside, adding an offer, defining your SKU, and submitting your VM image for certification.</span></span>

### <a name="51-download-and-run-hello-certification-test-tool-for-azure-certified"></a><span data-ttu-id="6bc09-288">5.1 hämta och köra hello certifikatutfärdare-verktyget för Azure-certifierad</span><span class="sxs-lookup"><span data-stu-id="6bc09-288">5.1 Download and run hello Certification Test Tool for Azure Certified</span></span>
<span data-ttu-id="6bc09-289">hello certifikatutfärdare verktyget körs på en körande VM som etablerats från dina användare VM-avbildning, tooensure som hello VM-avbildning är kompatibel med Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6bc09-289">hello certification tool runs on a running VM, provisioned from your user VM image, tooensure that hello VM image is compatible with Microsoft Azure.</span></span> <span data-ttu-id="6bc09-290">Den verifierar att hello vägledning och krav om hur du förbereder den virtuella Hårddisken har uppfyllts.</span><span class="sxs-lookup"><span data-stu-id="6bc09-290">It will verify that hello guidance and requirements about preparing your VHD have been met.</span></span> <span data-ttu-id="6bc09-291">hello utdata hello-verktyget är en Kompatibilitetsrapport som ska överföras på hello Publishing Portal när du begär certifiering.</span><span class="sxs-lookup"><span data-stu-id="6bc09-291">hello output of hello tool is a compatibility report, which should be uploaded on hello Publishing Portal while requesting certification.</span></span>

<span data-ttu-id="6bc09-292">hello certifikatutfärdare verktyget kan användas med både Windows och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6bc09-292">hello certification tool can be used with both Windows and Linux VMs.</span></span> <span data-ttu-id="6bc09-293">Ansluter tooWindows-baserade virtuella datorer via PowerShell och ansluter tooLinux virtuella datorer via SSH.Net:</span><span class="sxs-lookup"><span data-stu-id="6bc09-293">It connects tooWindows-based VMs via PowerShell and connects tooLinux VMs via SSH.Net:</span></span>

1. <span data-ttu-id="6bc09-294">Hämta först hello certifikatutfärdare verktyget hello [Microsoft hämtningsplats][link-msft-download].</span><span class="sxs-lookup"><span data-stu-id="6bc09-294">First, download hello certification tool at hello [Microsoft download site][link-msft-download].</span></span>
2. <span data-ttu-id="6bc09-295">Öppna hello certifikatutfärdare för och klicka sedan på hello **Starta nytt Test** knappen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-295">Open hello certification tool, and then click hello **Start New Test** button.</span></span>
3. <span data-ttu-id="6bc09-296">Från hello **testa Information** skärmen, ange ett namn för hello-testkörningen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-296">From hello **Test Information** screen, enter a name for hello test run.</span></span>
4. <span data-ttu-id="6bc09-297">Välj om din virtuella dator körs på Linux eller Windows.</span><span class="sxs-lookup"><span data-stu-id="6bc09-297">Choose whether your VM is on Linux or Windows.</span></span> <span data-ttu-id="6bc09-298">Välj hello efterföljande alternativ beroende på vilket du väljer.</span><span class="sxs-lookup"><span data-stu-id="6bc09-298">Depending on which you choose, select hello subsequent options.</span></span>

### <a name="connect-hello-certification-tool-tooa-linux-vm-image"></a><span data-ttu-id="6bc09-299">**Ansluta hello certifikatutfärdare verktyget tooa Linux VM-avbildning**</span><span class="sxs-lookup"><span data-stu-id="6bc09-299">**Connect hello certification tool tooa Linux VM image**</span></span>
1. <span data-ttu-id="6bc09-300">Välj hello SSH-autentiseringsläge: lösenord eller nyckel filen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-300">Select hello SSH authentication mode: password or key file.</span></span>
2. <span data-ttu-id="6bc09-301">Ange hello Domain Name System (DNS) namn, användarnamn och lösenord om du använder lösenordsbaserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="6bc09-301">If using password-­based authentication, enter hello Domain Name System (DNS) name, user name, and password.</span></span>
3. <span data-ttu-id="6bc09-302">Om nyckeln autentisering med ange hello DNS-namn, användarnamn och privat nyckel plats.</span><span class="sxs-lookup"><span data-stu-id="6bc09-302">If using key file authentication, enter hello DNS name, user name, and private key location.</span></span>

   ![För lösenordsautentisering av Linux VM-avbildning][img-cert-vm-pswd-lnx]

   ![Nyckelfilen autentisering av Linux VM-avbildning][img-cert-vm-key-lnx]

### <a name="connect-hello-certification-tool-tooa-windows-based-vm-image"></a><span data-ttu-id="6bc09-305">**Ansluta hello certifikatutfärdare verktyget tooa Windows-baserade VM-avbildning**</span><span class="sxs-lookup"><span data-stu-id="6bc09-305">**Connect hello certification tool tooa Windows-based VM image**</span></span>
1. <span data-ttu-id="6bc09-306">Ange hello fullständigt kvalificerade VM DNS-namn (till exempel MyVMName.Cloudapp.net).</span><span class="sxs-lookup"><span data-stu-id="6bc09-306">Enter hello fully qualified VM DNS name (for example, MyVMName.Cloudapp.net).</span></span>
2. <span data-ttu-id="6bc09-307">Ange hello användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="6bc09-307">Enter hello user name and password.</span></span>

   ![Autentisering av lösenord i Windows VM-avbildning][img-cert-vm-pswd-win]

<span data-ttu-id="6bc09-309">När du har valt hello rätt alternativ för Linux- eller Windows-baserade Virtuella bilden, Välj **Testanslutningen** tooensure SSH.Net eller PowerShell har en giltig anslutning för testning.</span><span class="sxs-lookup"><span data-stu-id="6bc09-309">After you have selected hello correct options for your Linux or Windows-based VM image, select **Test Connection** tooensure that SSH.Net or PowerShell has a valid connection for testing purposes.</span></span> <span data-ttu-id="6bc09-310">När en anslutning upprättas väljer **nästa** toostart hello test.</span><span class="sxs-lookup"><span data-stu-id="6bc09-310">After a connection is established, select **Next** toostart hello test.</span></span>

<span data-ttu-id="6bc09-311">När hello test har slutförts får du hello resultat (varning-Pass/misslyckas) för varje test-element.</span><span class="sxs-lookup"><span data-stu-id="6bc09-311">When hello test is complete, you will receive hello results (Pass/Fail/Warning) for each test element.</span></span>

![Testfall för Linux VM-avbildning][img-cert-vm-test-lnx]

![Testfall för Windows VM-avbildning][img-cert-vm-test-win]

<span data-ttu-id="6bc09-314">Om någon av hello testerna misslyckas certifieras inte bilden.</span><span class="sxs-lookup"><span data-stu-id="6bc09-314">If any of hello tests fail, your image will not be certified.</span></span> <span data-ttu-id="6bc09-315">Om detta inträffar kan du granska hello krav och gör nödvändiga ändringar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-315">If this occurs, review hello requirements and make any necessary changes.</span></span>

<span data-ttu-id="6bc09-316">När hello automatiserad test, tillfrågas du tooprovide ytterligare indata på VM-avbildning via en frågeformulär skärm.</span><span class="sxs-lookup"><span data-stu-id="6bc09-316">After hello automated test, you are asked tooprovide additional input on your VM image via a questionnaire screen.</span></span>  <span data-ttu-id="6bc09-317">Slutför hello frågor och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-317">Complete hello questions, and then select **Next**.</span></span>

![Certifikatutfärdare verktyget frågeformulär][img-cert-vm-questionnaire]

![Certifikatutfärdare verktyget frågeformulär][img-cert-vm-questionnaire-2]

<span data-ttu-id="6bc09-320">När du har slutfört hello frågeformulär, kan du ange ytterligare information, till exempel SSH åtkomstinformation för hello Linux VM-avbildning och en förklaring till misslyckade bedömningar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-320">After you have completed hello questionnaire, you can provide additional information such as SSH access information for hello Linux VM image and an explanation for any failed assessments.</span></span> <span data-ttu-id="6bc09-321">Du kan hämta hello testresultaten och loggfiler för hello utförs testfall tillägg tooyour svar toohello frågeformulär.</span><span class="sxs-lookup"><span data-stu-id="6bc09-321">You can download hello test results and log files for hello executed test cases in addition tooyour answers toohello questionnaire.</span></span> <span data-ttu-id="6bc09-322">Spara hello resultat i hello samma behållare som de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="6bc09-322">Save hello results in hello same container as your VHDs.</span></span>

![Spara certifikatutfärdare testresultat][img-cert-vm-results]

### <a name="52-get-hello-shared-access-signature-uri-for-your-vm-images"></a><span data-ttu-id="6bc09-324">5.2 hämta signatur för delad åtkomst hello URI för VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="6bc09-324">5.2 Get hello shared access signature URI for your VM images</span></span>
<span data-ttu-id="6bc09-325">Under publiceringsprocessen hello, ange OEM-tillverkare (hello uniform resource Identifier) som leda tooeach av hello virtuella hårddiskar som du har skapat för ditt SKU.</span><span class="sxs-lookup"><span data-stu-id="6bc09-325">During hello publishing process, you specify hello uniform resource identifiers (URIs) that lead tooeach of hello VHDs you have created for your SKU.</span></span> <span data-ttu-id="6bc09-326">Microsoft behöver åtkomst toothese virtuella hårddiskar under hello certifieringsprocess.</span><span class="sxs-lookup"><span data-stu-id="6bc09-326">Microsoft needs access toothese VHDs during hello certification process.</span></span> <span data-ttu-id="6bc09-327">Du måste därför toocreate signatur för delad åtkomst URI för varje virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="6bc09-327">Therefore, you need toocreate a shared access signature URI for each VHD.</span></span> <span data-ttu-id="6bc09-328">Detta är hello URI som ska anges i den **bilder** fliken i hello Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="6bc09-328">This is hello URI that should be entered in the **Images** tab in hello Publishing Portal.</span></span>

<span data-ttu-id="6bc09-329">signatur för delad åtkomst hello URI skapas bör följa toohello följande krav:</span><span class="sxs-lookup"><span data-stu-id="6bc09-329">hello shared access signature URI created should adhere toohello following requirements:</span></span>

* <span data-ttu-id="6bc09-330">När du genererar signatur för delad åtkomst URI: er för de virtuella hårddiskarna räcker lista och läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="6bc09-330">When generating shared access signature URIs for your VHDs, List and Read­ permissions are sufficient.</span></span> <span data-ttu-id="6bc09-331">Bevilja inte skriv- eller raderingsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="6bc09-331">Do not provide Write or Delete access.</span></span>
* <span data-ttu-id="6bc09-332">hello varaktighet för åtkomst ska vara minst tre (3) veckor från när signatur för delad åtkomst hello URI: N har skapats.</span><span class="sxs-lookup"><span data-stu-id="6bc09-332">hello duration for access should be a minimum of three (3) weeks from when hello shared access signature URI is created.</span></span>
* <span data-ttu-id="6bc09-333">toosafeguard för UTC-tid, väljer hello dagen före hello aktuellt datum.</span><span class="sxs-lookup"><span data-stu-id="6bc09-333">toosafeguard for UTC time, select hello day before hello current date.</span></span> <span data-ttu-id="6bc09-334">Hello aktuella datumet infaller 6 oktober 2014 väljer du exempelvis 2014-10-5.</span><span class="sxs-lookup"><span data-stu-id="6bc09-334">For example, if hello current date is October 6, 2014, select 10/5/2014.</span></span>

<span data-ttu-id="6bc09-335">SAS-URL kan vara genererats i flera olika sätt tooshare den virtuella Hårddisken för Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6bc09-335">SAS URL can be generated in multiple ways tooshare your VHD for Azure Marketplace.</span></span>
<span data-ttu-id="6bc09-336">Följande är hello 3 rekommenderade verktyg:</span><span class="sxs-lookup"><span data-stu-id="6bc09-336">Following are hello 3 recommended tools:</span></span>

1.  <span data-ttu-id="6bc09-337">Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="6bc09-337">Azure Storage Explorer</span></span>
2.  <span data-ttu-id="6bc09-338">Microsoft Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="6bc09-338">Microsoft Storage Explorer</span></span>
3.  <span data-ttu-id="6bc09-339">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6bc09-339">Azure CLI</span></span>

<span data-ttu-id="6bc09-340">**Azure Lagringsutforskaren (rekommenderas för Windows-användare)**</span><span class="sxs-lookup"><span data-stu-id="6bc09-340">**Azure Storage Explorer (Recommended for Windows Users)**</span></span>

<span data-ttu-id="6bc09-341">Följande är hello steg för att generera SAS-URL med hjälp av Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="6bc09-341">Following are hello steps for generating SAS URL by using Azure Storage Explorer</span></span>

1. <span data-ttu-id="6bc09-342">Hämta [förhandsvisning 3 av Azure Storage Explorer 6](https://azurestorageexplorer.codeplex.com/) från CodePlex.</span><span class="sxs-lookup"><span data-stu-id="6bc09-342">Download [Azure Storage Explorer 6 Preview 3](https://azurestorageexplorer.codeplex.com/) from CodePlex.</span></span> <span data-ttu-id="6bc09-343">Gå för[Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) och på **”hämtar”**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-343">Go too[Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) and click **"Downloads"**.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. <span data-ttu-id="6bc09-345">Hämta [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) och installera efter packa upp den.</span><span class="sxs-lookup"><span data-stu-id="6bc09-345">Download [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) and install after unzipping it.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. <span data-ttu-id="6bc09-347">Öppna programmet hello när den har installerats.</span><span class="sxs-lookup"><span data-stu-id="6bc09-347">After it is installed, open hello application.</span></span>
4. <span data-ttu-id="6bc09-348">Klicka på **Lägg till konto**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-348">Click **Add Account**.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. <span data-ttu-id="6bc09-350">Ange hello lagringskontonamn, lagringskontonyckel och lagring slutpunkter domän.</span><span class="sxs-lookup"><span data-stu-id="6bc09-350">Specify hello storage account name, storage account key, and storage endpoints domain.</span></span> <span data-ttu-id="6bc09-351">Detta är hello storage-konto i din Azure-prenumeration där du har sparat den virtuella Hårddisken på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-351">This is hello storage account in your Azure subscription where you have kept your VHD on Azure portal.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. <span data-ttu-id="6bc09-353">När Azure Lagringsutforskaren ansluts tooyour lagringskonto, startas visar alla hello innehåller inom hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6bc09-353">Once Azure Storage Explorer is connected tooyour specific storage account, it will start showing all of hello contains within hello storage account.</span></span> <span data-ttu-id="6bc09-354">Välj hello behållare dit du kopierade hello operativsystemet disk VHD-filen (även datadiskar om de är tillämpliga för ditt scenario).</span><span class="sxs-lookup"><span data-stu-id="6bc09-354">Select hello container where you copied hello operating system disk VHD file (also data disks if they are applicable for your scenario).</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. <span data-ttu-id="6bc09-356">När du har valt hello blob-behållare startar Azure Lagringsutforskaren visar hello filer i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="6bc09-356">After selecting hello blob container, Azure Storage Explorer starts showing hello files within hello container.</span></span> <span data-ttu-id="6bc09-357">Välj hello avbildningsfil (.vhd) som behöver toobe har skickats.</span><span class="sxs-lookup"><span data-stu-id="6bc09-357">Select hello image file (.vhd) that needs toobe submitted.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  <span data-ttu-id="6bc09-359">När du har valt hello VHD-filen i hello behållaren, klicka på hello **säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="6bc09-359">After selecting hello .vhd file in hello container, click hello **Security** tab.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  <span data-ttu-id="6bc09-361">I hello **Blob-behållaren säkerhet** dialogrutan lämna hello standardinställningar på hello **åtkomstnivå** fliken och klicka sedan på **signaturer för delad åtkomst** fliken.</span><span class="sxs-lookup"><span data-stu-id="6bc09-361">In hello **Blob Container Security** dialog box, leave hello defaults on hello **Access Level** tab, and then click **Shared Access Signatures** tab.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. <span data-ttu-id="6bc09-363">Så hello nedan toogenerate en signatur för delad åtkomst URI för hello VHD-avbildningen:</span><span class="sxs-lookup"><span data-stu-id="6bc09-363">Follow hello steps below toogenerate a shared access signature URI for hello .vhd image:</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    <span data-ttu-id="6bc09-365">a.</span><span class="sxs-lookup"><span data-stu-id="6bc09-365">a.</span></span> <span data-ttu-id="6bc09-366">**Åtkomst tillåts från:** toosafeguard för UTC-tid, väljer hello dagen före hello aktuellt datum.</span><span class="sxs-lookup"><span data-stu-id="6bc09-366">**Access permitted from:** toosafeguard for UTC time, select hello day before hello current date.</span></span> <span data-ttu-id="6bc09-367">Hello aktuella datumet infaller 6 oktober 2014 väljer du exempelvis 2014-10-5.</span><span class="sxs-lookup"><span data-stu-id="6bc09-367">For example, if hello current date is October 6, 2014, select 10/5/2014.</span></span>

    <span data-ttu-id="6bc09-368">b.</span><span class="sxs-lookup"><span data-stu-id="6bc09-368">b.</span></span> <span data-ttu-id="6bc09-369">**Åtkomst till tillåten:** Välj ett datum som är minst tre veckor efter hello **åtkomst tillåts från** datum.</span><span class="sxs-lookup"><span data-stu-id="6bc09-369">**Access permitted to:** Select a date that is at least 3 weeks after hello **Access permitted from** date.</span></span>

    <span data-ttu-id="6bc09-370">c.</span><span class="sxs-lookup"><span data-stu-id="6bc09-370">c.</span></span> <span data-ttu-id="6bc09-371">**Åtgärder som tillåts:** väljer hello **lista** och **Läs** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="6bc09-371">**Actions permitted:** Select hello **List** and **Read** permissions.</span></span>

    <span data-ttu-id="6bc09-372">d.</span><span class="sxs-lookup"><span data-stu-id="6bc09-372">d.</span></span> <span data-ttu-id="6bc09-373">Om du har valt VHD-filen på rätt sätt och sedan filen visas i **Blob-namnet tooaccess** med filnamnstillägget .vhd.</span><span class="sxs-lookup"><span data-stu-id="6bc09-373">If you have selected your .vhd file correctly, then your file appears in **Blob name tooaccess** with extension .vhd.</span></span>

    <span data-ttu-id="6bc09-374">e.</span><span class="sxs-lookup"><span data-stu-id="6bc09-374">e.</span></span> <span data-ttu-id="6bc09-375">Klicka på **generera signatur**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-375">Click **Generate Signature**.</span></span>

    <span data-ttu-id="6bc09-376">f.</span><span class="sxs-lookup"><span data-stu-id="6bc09-376">f.</span></span> <span data-ttu-id="6bc09-377">I **genereras delad åtkomst signatur URI: N för den här behållaren**, Sök efter hello efter som markerade ovan:</span><span class="sxs-lookup"><span data-stu-id="6bc09-377">In **Generated Shared Access Signature URI of this container**, check for hello following as highlighted above:</span></span>

       - <span data-ttu-id="6bc09-378">Se till att bilden filnamn och **”VHD”** i hello URI.</span><span class="sxs-lookup"><span data-stu-id="6bc09-378">Make sure that your image file name and **".vhd"** are in hello URI.</span></span>
       - <span data-ttu-id="6bc09-379">Hello slutet av hello signatur, se till att **”= rl”** visas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-379">At hello end of hello signature, make sure that **"=rl"** appears.</span></span> <span data-ttu-id="6bc09-380">Detta demonstrerar att läs- och åtkomst har angetts korrekt.</span><span class="sxs-lookup"><span data-stu-id="6bc09-380">This demonstrates that Read and List access was provided successfully.</span></span>
       - <span data-ttu-id="6bc09-381">I mitten av hello signatur, se till att **”sr = c”** visas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-381">In middle of hello signature, make sure that **"sr=c"** appears.</span></span> <span data-ttu-id="6bc09-382">Detta demonstrerar att du har åtkomst behållare</span><span class="sxs-lookup"><span data-stu-id="6bc09-382">This demonstrates that you have container level access</span></span>

11. <span data-ttu-id="6bc09-383">tooensure som hello genereras delad åtkomst signatur URI fungerar klickar du på **Test i webbläsaren**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-383">tooensure that hello generated shared access signature URI works, click **Test in Browser**.</span></span> <span data-ttu-id="6bc09-384">Det bör börja hello hämtningen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-384">It should start hello download process.</span></span>

12. <span data-ttu-id="6bc09-385">Kopiera signatur för delad åtkomst hello URI.</span><span class="sxs-lookup"><span data-stu-id="6bc09-385">Copy hello shared access signature URI.</span></span> <span data-ttu-id="6bc09-386">Detta är hello URI toopaste till hello Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="6bc09-386">This is hello URI toopaste into hello Publishing Portal.</span></span>

13. <span data-ttu-id="6bc09-387">Upprepa steg 6 – 10 för varje virtuell Hårddisk i hello SKU.</span><span class="sxs-lookup"><span data-stu-id="6bc09-387">Repeat steps 6-10 for each VHD in hello SKU.</span></span>

<span data-ttu-id="6bc09-388">**Microsoft Azure Lagringsutforskaren (Windows-/ MAC/Linux)**</span><span class="sxs-lookup"><span data-stu-id="6bc09-388">**Microsoft Azure Storage Explorer (Windows/MAC/Linux)**</span></span>

<span data-ttu-id="6bc09-389">Följande är hello steg för att generera SAS-URL genom att använda Microsoft Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="6bc09-389">Following are hello steps for generating SAS URL by using Microsoft Azure Storage Explorer</span></span>

1.  <span data-ttu-id="6bc09-390">Ladda ned Microsoft Azure Lagringsutforskaren formuläret [http://storageexplorer.com/](http://storageexplorer.com/) webbplats.</span><span class="sxs-lookup"><span data-stu-id="6bc09-390">Download Microsoft Azure Storage Explorer form [http://storageexplorer.com/](http://storageexplorer.com/) website.</span></span> <span data-ttu-id="6bc09-391">Gå för[Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/releasenotes.html) och på **”hämta för Windows”**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-391">Go too[Microsoft Azure Storage Explorer](http://storageexplorer.com/releasenotes.html) and click **“Download for Windows”**.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  <span data-ttu-id="6bc09-393">Öppna programmet hello när den har installerats.</span><span class="sxs-lookup"><span data-stu-id="6bc09-393">After it is installed, open hello application.</span></span>

3.  <span data-ttu-id="6bc09-394">Klicka på **Lägg till konto**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-394">Click **Add Account**.</span></span>

4.  <span data-ttu-id="6bc09-395">Konfigurera Microsoft Azure Lagringsutforskaren tooyour prenumerationen genom att logga in tooyour konto</span><span class="sxs-lookup"><span data-stu-id="6bc09-395">Configure Microsoft Azure Storage Explorer tooyour subscription by sign in tooyour account</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  <span data-ttu-id="6bc09-397">Gå toostorage konto och välj hello behållare</span><span class="sxs-lookup"><span data-stu-id="6bc09-397">Go toostorage account and select hello Container</span></span>

6.  <span data-ttu-id="6bc09-398">Välj **”hämta resursen Åtkomstsignatur...”**</span><span class="sxs-lookup"><span data-stu-id="6bc09-398">Select **“Get Share Access Signature..”**</span></span> <span data-ttu-id="6bc09-399">med hjälp av högerklickar du på hello **behållare**</span><span class="sxs-lookup"><span data-stu-id="6bc09-399">by using Right Click of hello **container**</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  <span data-ttu-id="6bc09-401">Starttiden för uppdateringen, förfallotiden och behörigheter enligt följande</span><span class="sxs-lookup"><span data-stu-id="6bc09-401">Update Start time, Expiry time and Permissions as per following</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    <span data-ttu-id="6bc09-403">a.</span><span class="sxs-lookup"><span data-stu-id="6bc09-403">a.</span></span>  <span data-ttu-id="6bc09-404">**Starttid:** toosafeguard för UTC-tid, väljer hello dagen före hello aktuellt datum.</span><span class="sxs-lookup"><span data-stu-id="6bc09-404">**Start Time:** toosafeguard for UTC time, select hello day before hello current date.</span></span> <span data-ttu-id="6bc09-405">Hello aktuella datumet infaller 6 oktober 2014 väljer du exempelvis 2014-10-5.</span><span class="sxs-lookup"><span data-stu-id="6bc09-405">For example, if hello current date is October 6, 2014, select 10/5/2014.</span></span>

    <span data-ttu-id="6bc09-406">b.</span><span class="sxs-lookup"><span data-stu-id="6bc09-406">b.</span></span>  <span data-ttu-id="6bc09-407">**Förfallotiden:** Välj ett datum som är minst tre veckor efter hello **starttid** datum.</span><span class="sxs-lookup"><span data-stu-id="6bc09-407">**Expiry Time:** Select a date that is at least 3 weeks after hello **Start Time** date.</span></span>

    <span data-ttu-id="6bc09-408">c.</span><span class="sxs-lookup"><span data-stu-id="6bc09-408">c.</span></span>  <span data-ttu-id="6bc09-409">**Behörigheter:** väljer hello **lista** och **Läs** behörigheter</span><span class="sxs-lookup"><span data-stu-id="6bc09-409">**Permissions:** Select hello **List** and **Read** permissions</span></span>

8.  <span data-ttu-id="6bc09-410">Kopiera URI för signatur för delad åtkomst av behållare</span><span class="sxs-lookup"><span data-stu-id="6bc09-410">Copy Container shared access signature URI</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    <span data-ttu-id="6bc09-412">Genererade SAS-URL är för behållare nivå och nu måste vi tooadd VHD namn i den.</span><span class="sxs-lookup"><span data-stu-id="6bc09-412">Generated SAS URL is for container Level and now we need tooadd VHD name in it.</span></span>

    <span data-ttu-id="6bc09-413">Format för behållare nivån SAS-URL:`https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span><span class="sxs-lookup"><span data-stu-id="6bc09-413">Format of Container Level SAS URL: `https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span></span>

    <span data-ttu-id="6bc09-414">Infoga VHD namn efter hello behållarens namn i SAS-URL som nedan`https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span><span class="sxs-lookup"><span data-stu-id="6bc09-414">Insert VHD name after hello container name in SAS URL as below `https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span></span>

    <span data-ttu-id="6bc09-415">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6bc09-415">Example:</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    <span data-ttu-id="6bc09-417">TestRGVM201631920152.vhd är hello VHD-namn ska vara VHD SAS-URL`https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span><span class="sxs-lookup"><span data-stu-id="6bc09-417">TestRGVM201631920152.vhd is hello VHD Name then VHD SAS URL will be `https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span></span>

    - <span data-ttu-id="6bc09-418">Se till att bilden filnamn och **”VHD”** i hello URI.</span><span class="sxs-lookup"><span data-stu-id="6bc09-418">Make sure that your image file name and **".vhd"** are in hello URI.</span></span>
    - <span data-ttu-id="6bc09-419">I mitten av hello signatur, se till att **”sp = rl”** visas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-419">In middle of hello signature, make sure that **"sp=rl"** appears.</span></span> <span data-ttu-id="6bc09-420">Detta demonstrerar att läs- och åtkomst har angetts korrekt.</span><span class="sxs-lookup"><span data-stu-id="6bc09-420">This demonstrates that Read and List access was provided successfully.</span></span>
    - <span data-ttu-id="6bc09-421">I mitten av hello signatur, se till att **”sr = c”** visas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-421">In middle of hello signature, make sure that **"sr=c"** appears.</span></span> <span data-ttu-id="6bc09-422">Detta demonstrerar att du har åtkomst behållare</span><span class="sxs-lookup"><span data-stu-id="6bc09-422">This demonstrates that you have container level access</span></span>

9.  <span data-ttu-id="6bc09-423">tooensure som hello genererade delad åtkomst signatur URI fungerar, testa den i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="6bc09-423">tooensure that hello generated shared access signature URI works, test it in browser.</span></span> <span data-ttu-id="6bc09-424">Det bör starta hämtningsprocessen för hello</span><span class="sxs-lookup"><span data-stu-id="6bc09-424">It should start hello download process</span></span>

10. <span data-ttu-id="6bc09-425">Kopiera signatur för delad åtkomst hello URI.</span><span class="sxs-lookup"><span data-stu-id="6bc09-425">Copy hello shared access signature URI.</span></span> <span data-ttu-id="6bc09-426">Detta är hello URI toopaste till hello Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="6bc09-426">This is hello URI toopaste into hello Publishing Portal.</span></span>

11. <span data-ttu-id="6bc09-427">Upprepa dessa steg för varje virtuell Hårddisk i hello SKU.</span><span class="sxs-lookup"><span data-stu-id="6bc09-427">Repeat these steps for each VHD in hello SKU.</span></span>

<span data-ttu-id="6bc09-428">**Azure CLI (rekommenderas för icke-Windows & kontinuerlig Integration)**</span><span class="sxs-lookup"><span data-stu-id="6bc09-428">**Azure CLI (Recommended for Non-Windows & Continuous Integration)**</span></span>

<span data-ttu-id="6bc09-429">Följande är hello steg för att generera SAS-URL med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6bc09-429">Following are hello steps for generating SAS URL by using Azure CLI</span></span>

1.  <span data-ttu-id="6bc09-430">Ladda ned Microsoft Azure CLI från [här](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/).</span><span class="sxs-lookup"><span data-stu-id="6bc09-430">Download Microsoft Azure CLI from [here](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/).</span></span> <span data-ttu-id="6bc09-431">Du kan också hitta olika länkar för  **[Windows](http://aka.ms/webpi-azure-cli)**  och  **[MAC OS x](http://aka.ms/mac-azure-cli)**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-431">You can also find different links for **[Windows](http://aka.ms/webpi-azure-cli)** and **[MAC OS](http://aka.ms/mac-azure-cli)**.</span></span>

2.  <span data-ttu-id="6bc09-432">När du har laddat ned och installera</span><span class="sxs-lookup"><span data-stu-id="6bc09-432">Once it is downloaded, please install</span></span>

3.  <span data-ttu-id="6bc09-433">Skapa en PowerShell-fil med följande kod och spara den i lokalt</span><span class="sxs-lookup"><span data-stu-id="6bc09-433">Create a PowerShell file with following code and save it in local</span></span>

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    <span data-ttu-id="6bc09-434">Uppdatera hello följande parametrar i ovan</span><span class="sxs-lookup"><span data-stu-id="6bc09-434">Update hello following parameters in above</span></span>

    <span data-ttu-id="6bc09-435">a.</span><span class="sxs-lookup"><span data-stu-id="6bc09-435">a.</span></span> <span data-ttu-id="6bc09-436">**`<StorageAccountName>`**: Ange namnet på ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="6bc09-436">**`<StorageAccountName>`**: Give your storage account name</span></span>

    <span data-ttu-id="6bc09-437">b.</span><span class="sxs-lookup"><span data-stu-id="6bc09-437">b.</span></span> <span data-ttu-id="6bc09-438">**`<Storage Account Key>`**: Ger din lagringskontonyckel</span><span class="sxs-lookup"><span data-stu-id="6bc09-438">**`<Storage Account Key>`**: Give your storage account key</span></span>

    <span data-ttu-id="6bc09-439">c.</span><span class="sxs-lookup"><span data-stu-id="6bc09-439">c.</span></span> <span data-ttu-id="6bc09-440">**`<Permission Start Date>`**: toosafeguard för UTC-tid, väljer hello dagen före hello aktuellt datum.</span><span class="sxs-lookup"><span data-stu-id="6bc09-440">**`<Permission Start Date>`**: toosafeguard for UTC time, select hello day before hello current date.</span></span> <span data-ttu-id="6bc09-441">Till exempel om hello aktuellt datum är 26 oktober 2016 och värdet ska vara 10/25/2016</span><span class="sxs-lookup"><span data-stu-id="6bc09-441">For example, if hello current date is October 26, 2016, then value should be 10/25/2016</span></span>

    <span data-ttu-id="6bc09-442">d.</span><span class="sxs-lookup"><span data-stu-id="6bc09-442">d.</span></span> <span data-ttu-id="6bc09-443">**`<Permission End Date>`**: Välj ett datum som är minst tre veckor efter hello **startdatum**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-443">**`<Permission End Date>`**: Select a date that is at least 3 weeks after hello **Start Date**.</span></span> <span data-ttu-id="6bc09-444">Värdet ska vara **2016-02-11**.</span><span class="sxs-lookup"><span data-stu-id="6bc09-444">Then value should be **11/02/2016**.</span></span>

    <span data-ttu-id="6bc09-445">Följande är kodexempel hello när du har uppdaterat rätt parametrar</span><span class="sxs-lookup"><span data-stu-id="6bc09-445">Following is hello example code after updating proper parameters</span></span>

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=TIQE5QWMKHpT5q2VnF1bb+NUV7NVMY2xmzVx1rdgIVsw7h0pcI5nMM6+DVFO65i4bQevx21dmrflA91r0Vh2Yw=="
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  <span data-ttu-id="6bc09-446">Öppna Redigeraren för Powershell med ”Kör som administratör”-läge och öppna filen i steg #3.</span><span class="sxs-lookup"><span data-stu-id="6bc09-446">Open Powershell editor with “Run as Administrator” mode and open file in step #3.</span></span>

5.  <span data-ttu-id="6bc09-447">Kör hello skript och det ger du hello SAS-URL för behållaren åtkomst</span><span class="sxs-lookup"><span data-stu-id="6bc09-447">Run hello script and it will provide you hello SAS URL for container level access</span></span>

    <span data-ttu-id="6bc09-448">Följande kommer att hello utdata från hello SAS signatur och kopiera hello markerat en del i en anteckningar</span><span class="sxs-lookup"><span data-stu-id="6bc09-448">Following will be hello output of hello SAS Signature and copy hello highlighted part in a notepad</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  <span data-ttu-id="6bc09-450">Nu får du behållaren nivå SAS-URL och du behöver tooadd VHD namn i den.</span><span class="sxs-lookup"><span data-stu-id="6bc09-450">Now you will get container level SAS URL and you need tooadd VHD name in it.</span></span>

    <span data-ttu-id="6bc09-451">Behållaren nivån SAS-URL #</span><span class="sxs-lookup"><span data-stu-id="6bc09-451">Container level SAS URL #</span></span>

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  <span data-ttu-id="6bc09-452">Infoga VHD namn efter hello behållarnamn i SAS-URL som visas nedan`https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`</span><span class="sxs-lookup"><span data-stu-id="6bc09-452">Insert VHD name after hello container name in SAS URL as shown below `https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`</span></span>

    <span data-ttu-id="6bc09-453">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6bc09-453">Example:</span></span>

    <span data-ttu-id="6bc09-454">TestRGVM201631920152.vhd är hello VHD-namn ska vara VHD SAS-URL</span><span class="sxs-lookup"><span data-stu-id="6bc09-454">TestRGVM201631920152.vhd is hello VHD Name then VHD SAS URL will be</span></span>

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - <span data-ttu-id="6bc09-455">Kontrollera att dina bildfilens namn och ”VHD” i hello URI.</span><span class="sxs-lookup"><span data-stu-id="6bc09-455">Make sure that your image file name and ".vhd" are in hello URI.</span></span>
    -   <span data-ttu-id="6bc09-456">I mitten av hello signatur, se till att ”sp = rl” visas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-456">In middle of hello signature, make sure that "sp=rl" appears.</span></span> <span data-ttu-id="6bc09-457">Detta demonstrerar att läs- och åtkomst har angetts korrekt.</span><span class="sxs-lookup"><span data-stu-id="6bc09-457">This demonstrates that Read and List access was provided successfully.</span></span>
    -   <span data-ttu-id="6bc09-458">I mitten av hello signatur, se till att ”sr = c” visas.</span><span class="sxs-lookup"><span data-stu-id="6bc09-458">In middle of hello signature, make sure that "sr=c" appears.</span></span> <span data-ttu-id="6bc09-459">Detta demonstrerar att du har åtkomst behållare</span><span class="sxs-lookup"><span data-stu-id="6bc09-459">This demonstrates that you have container level access</span></span>

8.  <span data-ttu-id="6bc09-460">tooensure som hello genererade delad åtkomst signatur URI fungerar, testa den i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="6bc09-460">tooensure that hello generated shared access signature URI works, test it in browser.</span></span> <span data-ttu-id="6bc09-461">Det bör starta hämtningsprocessen för hello</span><span class="sxs-lookup"><span data-stu-id="6bc09-461">It should start hello download process</span></span>

9.  <span data-ttu-id="6bc09-462">Kopiera signatur för delad åtkomst hello URI.</span><span class="sxs-lookup"><span data-stu-id="6bc09-462">Copy hello shared access signature URI.</span></span> <span data-ttu-id="6bc09-463">Detta är hello URI toopaste till hello Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="6bc09-463">This is hello URI toopaste into hello Publishing Portal.</span></span>

10. <span data-ttu-id="6bc09-464">Upprepa dessa steg för varje virtuell Hårddisk i hello SKU.</span><span class="sxs-lookup"><span data-stu-id="6bc09-464">Repeat these steps for each VHD in hello SKU.</span></span>


### <a name="53-provide-information-about-hello-vm-image-and-request-certification-in-hello-publishing-portal"></a><span data-ttu-id="6bc09-465">5.3 innehåller information om hello VM-avbildning och begär certifiering i hello Publishing Portal</span><span class="sxs-lookup"><span data-stu-id="6bc09-465">5.3 Provide information about hello VM image and request certification in hello Publishing Portal</span></span>
<span data-ttu-id="6bc09-466">När du har skapat ditt erbjudande och SKU, ska du ange hello avbildningsinformation som är associerade med den SKU:</span><span class="sxs-lookup"><span data-stu-id="6bc09-466">After you have created your offer and SKU, you should enter hello image details associated with that SKU:</span></span>

1. <span data-ttu-id="6bc09-467">Gå toohello [Publiceringsportal][link-pubportal], och sedan logga in med ditt säljare.</span><span class="sxs-lookup"><span data-stu-id="6bc09-467">Go toohello [Publishing Portal][link-pubportal], and then sign in with your seller account.</span></span>
2. <span data-ttu-id="6bc09-468">Välj hello **VM-avbildningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="6bc09-468">Select hello **VM images** tab.</span></span>
3. <span data-ttu-id="6bc09-469">hello identifierare som nämns i hello överst på sidan för hello är faktiskt hello erbjuder identifierare och inte hello SKU-identifierare.</span><span class="sxs-lookup"><span data-stu-id="6bc09-469">hello identifier listed at hello top of hello page is actually hello offer identifier and not hello SKU identifier.</span></span>
4. <span data-ttu-id="6bc09-470">Ange hello egenskaper under hello **SKU: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6bc09-470">Fill out hello properties under hello **SKUs** section.</span></span>
5. <span data-ttu-id="6bc09-471">Under **operativsystemsfamilj**, klicka på hello typ av operativsystem som är associerade med hello operativsystemet VHD.</span><span class="sxs-lookup"><span data-stu-id="6bc09-471">Under **Operating system family**, click hello operating system type associated with hello operating system VHD.</span></span>
6. <span data-ttu-id="6bc09-472">I hello **operativsystemet** rutan, beskriver hello-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="6bc09-472">In hello **Operating system** box, describe hello operating system.</span></span> <span data-ttu-id="6bc09-473">Överväg ett format som operativsystemsfamilj, typ, version och uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-473">Consider a format such as operating system family, type, version, and updates.</span></span> <span data-ttu-id="6bc09-474">Ett exempel är ”Windows Server Datacenter 2014 R2”.</span><span class="sxs-lookup"><span data-stu-id="6bc09-474">An example is "Windows Server Datacenter 2014 R2."</span></span>
7. <span data-ttu-id="6bc09-475">Välj toosix rekommenderade storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6bc09-475">Select up toosix recommended virtual machine sizes.</span></span> <span data-ttu-id="6bc09-476">Dessa är rekommendationer som får visas toohello kunden i hello priser nivå bladet i hello Azure-portalen när de bestämma toopurchase och distribuera avbildningen.</span><span class="sxs-lookup"><span data-stu-id="6bc09-476">These are recommendations that get displayed toohello customer in hello Pricing tier blade in hello Azure Portal when they decide toopurchase and deploy your image.</span></span> <span data-ttu-id="6bc09-477">**Detta är endast rekommendationer. hello kunden är kan tooselect VM-storlek som hanterar hello diskar som anges i bilden.**</span><span class="sxs-lookup"><span data-stu-id="6bc09-477">**These are only recommendations. hello customer is able tooselect any VM size that accommodates hello disks specified in your image.**</span></span>
8. <span data-ttu-id="6bc09-478">Ange hello version.</span><span class="sxs-lookup"><span data-stu-id="6bc09-478">Enter hello version.</span></span> <span data-ttu-id="6bc09-479">hello versionsfältet kapslar in en semantiska version tooidentify hello produkt och dess uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="6bc09-479">hello version field encapsulates a semantic version tooidentify hello product and its updates:</span></span>
   * <span data-ttu-id="6bc09-480">Versioner ska hello formatet X.Y.Z, där X, Y och Z är heltal.</span><span class="sxs-lookup"><span data-stu-id="6bc09-480">Versions should be of hello form X.Y.Z, where X, Y, and Z are integers.</span></span>
   * <span data-ttu-id="6bc09-481">Bilder i olika SKU: er kan ha olika versioner av högre och lägre.</span><span class="sxs-lookup"><span data-stu-id="6bc09-481">Images in different SKUs can have different major and minor versions.</span></span>
   * <span data-ttu-id="6bc09-482">Versioner i en SKU bara ska inkrementella ändringar som ökar hello korrigering version (Z från X.Y.Z).</span><span class="sxs-lookup"><span data-stu-id="6bc09-482">Versions within a SKU should only be incremental changes, which increase hello patch version (Z from X.Y.Z).</span></span>
9. <span data-ttu-id="6bc09-483">I hello **OS VHD URL** ange hello signatur för delad åtkomst URI som skapats för hello operativsystemet VHD.</span><span class="sxs-lookup"><span data-stu-id="6bc09-483">In hello **OS VHD URL** box, enter hello shared access signature URI created for hello operating system VHD.</span></span>
10. <span data-ttu-id="6bc09-484">Om det finns datadiskar som är associerade med denna SKU, Välj hello logisk enhet nummer (LUN) toowhich som den här data disk toobe monteras på distribution.</span><span class="sxs-lookup"><span data-stu-id="6bc09-484">If there are data disks associated with this SKU, select hello logical unit number (LUN) toowhich you would like this data disk toobe mounted upon deployment.</span></span>
11. <span data-ttu-id="6bc09-485">I hello **LUN X VHD URL** ange hello signatur för delad åtkomst URI som skapats för hello först data VHD.</span><span class="sxs-lookup"><span data-stu-id="6bc09-485">In hello **LUN X VHD URL** box, enter hello shared access signature URI created for hello first data VHD.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a><span data-ttu-id="6bc09-487">Vanliga problem med SAS-URL & korrigeringar</span><span class="sxs-lookup"><span data-stu-id="6bc09-487">Common SAS URL issues & fixes</span></span>

|<span data-ttu-id="6bc09-488">Problem</span><span class="sxs-lookup"><span data-stu-id="6bc09-488">Issue</span></span>|<span data-ttu-id="6bc09-489">Felmeddelande</span><span class="sxs-lookup"><span data-stu-id="6bc09-489">Failure Message</span></span>|<span data-ttu-id="6bc09-490">Åtgärda</span><span class="sxs-lookup"><span data-stu-id="6bc09-490">Fix</span></span>|<span data-ttu-id="6bc09-491">Länk till dokumentation</span><span class="sxs-lookup"><span data-stu-id="6bc09-491">Documentation Link</span></span>|
|---|---|---|---|
|<span data-ttu-id="6bc09-492">Det gick inte att kopiera bilder - ””? hittades inte i SAS-url</span><span class="sxs-lookup"><span data-stu-id="6bc09-492">Failure in copying images - "?" is not found in SAS url</span></span>|<span data-ttu-id="6bc09-493">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-493">Failure: Copying Images.</span></span> <span data-ttu-id="6bc09-494">Det går inte toodownload blob med som SAS-Uri.</span><span class="sxs-lookup"><span data-stu-id="6bc09-494">Not able toodownload blob using provided SAS Uri.</span></span>|<span data-ttu-id="6bc09-495">Uppdatera hello SAS-Url med rekommenderade verktyg</span><span class="sxs-lookup"><span data-stu-id="6bc09-495">Update hello SAS Url using recommended tools</span></span>|[<span data-ttu-id="6bc09-496">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="6bc09-496">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="6bc09-497">Det gick inte att kopiera bilder - ”a” och ”se” parametrar inte i SAS-url</span><span class="sxs-lookup"><span data-stu-id="6bc09-497">Failure in copying images - “st” and “se” parameters not in SAS url</span></span>|<span data-ttu-id="6bc09-498">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-498">Failure: Copying Images.</span></span> <span data-ttu-id="6bc09-499">Det går inte toodownload blob med som SAS-Uri.</span><span class="sxs-lookup"><span data-stu-id="6bc09-499">Not able toodownload blob using provided SAS Uri.</span></span>|<span data-ttu-id="6bc09-500">Uppdatera hello SAS-Url med Start- och slutdatum på den</span><span class="sxs-lookup"><span data-stu-id="6bc09-500">Update hello SAS Url with Start and End dates on it</span></span>|[<span data-ttu-id="6bc09-501">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="6bc09-501">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="6bc09-502">Det gick inte att kopiera avbildningar - ”sp = rl” inte i SAS-url</span><span class="sxs-lookup"><span data-stu-id="6bc09-502">Failure in copying images - “sp=rl” not in SAS url</span></span>|<span data-ttu-id="6bc09-503">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-503">Failure: Copying Images.</span></span> <span data-ttu-id="6bc09-504">Det går inte toodownload blob med angivna SAS-Uri</span><span class="sxs-lookup"><span data-stu-id="6bc09-504">Not able toodownload blob using provided SAS Uri</span></span>|<span data-ttu-id="6bc09-505">Uppdatera hello SAS-Url med behörigheter som har angetts som ”läsa” och ”lista</span><span class="sxs-lookup"><span data-stu-id="6bc09-505">Update hello SAS Url with permissions set as “Read” & “List</span></span>|[<span data-ttu-id="6bc09-506">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="6bc09-506">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="6bc09-507">Det gick inte att kopiera avbildningar - SAS-url har blanksteg i vhd-namn</span><span class="sxs-lookup"><span data-stu-id="6bc09-507">Failure in copying images - SAS url have white spaces in vhd name</span></span>|<span data-ttu-id="6bc09-508">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-508">Failure: Copying Images.</span></span> <span data-ttu-id="6bc09-509">Det går inte toodownload blob med som SAS-Uri.</span><span class="sxs-lookup"><span data-stu-id="6bc09-509">Not able toodownload blob using provided SAS Uri.</span></span>|<span data-ttu-id="6bc09-510">Uppdatera hello SAS-Url utan blanksteg</span><span class="sxs-lookup"><span data-stu-id="6bc09-510">Update hello SAS Url without white spaces</span></span>|[<span data-ttu-id="6bc09-511">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="6bc09-511">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="6bc09-512">Det gick inte att kopiera avbildningar – Url-auktorisering för SAS-fel</span><span class="sxs-lookup"><span data-stu-id="6bc09-512">Failure in copying images – SAS Url Authorization error</span></span>|<span data-ttu-id="6bc09-513">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="6bc09-513">Failure: Copying Images.</span></span> <span data-ttu-id="6bc09-514">Det går inte toodownload blob på grund av tooauthorization fel</span><span class="sxs-lookup"><span data-stu-id="6bc09-514">Not able toodownload blob due tooauthorization error</span></span>|<span data-ttu-id="6bc09-515">Återskapa hello SAS-Url</span><span class="sxs-lookup"><span data-stu-id="6bc09-515">Regenerate hello SAS Url</span></span>|[<span data-ttu-id="6bc09-516">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="6bc09-516">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|


## <a name="next-step"></a><span data-ttu-id="6bc09-517">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6bc09-517">Next step</span></span>
<span data-ttu-id="6bc09-518">När du är klar med hello SKU uppgifter du kan flytta framåt toohello [administratörshandboken för Azure Marketplace marketing content][link-pushstaging].</span><span class="sxs-lookup"><span data-stu-id="6bc09-518">After you are done with hello SKU details, you can move forward toohello [Azure Marketplace marketing content guide][link-pushstaging].</span></span> <span data-ttu-id="6bc09-519">I steget av hello publiceringsprocessen och ger du hello marknadsföring innehåll, priser och andra nödvändiga innan information för**steg3: testa den virtuella datorn erbjuder i Förproduktion**, där du kan testa olika användningsfall scenarier innan du distribuerar hello erbjudande toohello Azure Marketplace för offentliga synlighet och inköp.</span><span class="sxs-lookup"><span data-stu-id="6bc09-519">In that step of hello publishing process, you provide hello marketing content, pricing, and other information necessary prior too**Step 3: Testing your VM offer in staging**, where you test various use-case scenarios before deploying hello offer toohello Azure Marketplace for public visibility and purchase.</span></span>  

## <a name="see-also"></a><span data-ttu-id="6bc09-520">Se även</span><span class="sxs-lookup"><span data-stu-id="6bc09-520">See also</span></span>
* [<span data-ttu-id="6bc09-521">Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="6bc09-521">Getting started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

[img-acom-1]:media/marketplace-publishing-vm-image-creation/vm-image-acom-datacenter.png
[img-portal-vm-size]:media/marketplace-publishing-vm-image-creation/vm-image-portal-size.png
[img-portal-vm-create]:media/marketplace-publishing-vm-image-creation/vm-image-portal-create-vm.png
[img-portal-vm-location]:media/marketplace-publishing-vm-image-creation/vm-image-portal-location.png
[img-portal-vm-rdp]:media/marketplace-publishing-vm-image-creation/vm-image-portal-rdp.png
[img-azstg-add]:media/marketplace-publishing-vm-image-creation/vm-image-storage-add.png
[img-manage-vm-new]:media/marketplace-publishing-vm-image-creation/vm-image-manage-new.png
[img-manage-vm-select]:media/marketplace-publishing-vm-image-creation/vm-image-manage-select.png
[img-cert-vm-key-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-keyfile-linux.png
[img-cert-vm-pswd-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-linux.png
[img-cert-vm-pswd-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-win.png
[img-cert-vm-test-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-linux.png
[img-cert-vm-test-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-win.png
[img-cert-vm-results]:media/marketplace-publishing-vm-image-creation/vm-image-certification-results.png
[img-cert-vm-questionnaire]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire.png
[img-cert-vm-questionnaire-2]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire-2.png
[img-pubportal-vm-skus]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus.png
[img-pubportal-vm-skus-2]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-2.png

[link-pushstaging]:marketplace-publishing-push-to-staging.md
[link-github-waagent]:https://github.com/Azure/WALinuxAgent
[link-azure-codeplex]:https://azurestorageexplorer.codeplex.com/
[link-azure-2]:../storage/blobs/storage-dotnet-shared-access-signature-part-2.md
[link-azure-1]:../storage/common/storage-dotnet-shared-access-signature-part-1.md
[link-msft-download]:http://www.microsoft.com/download/details.aspx?id=44299
[link-technet-3]:https://technet.microsoft.com/library/hh846766.aspx
[link-technet-2]:https://msdn.microsoft.com/library/dn495261.aspx
[link-azure-portal]:https://portal.azure.com
[link-pubportal]:https://publish.windowsazure.com
[link-sql-2014-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/
[link-sql-2014-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/
[link-sql-2014-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/
[link-sql-2012-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/
[link-sql-2012-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/
[link-sql-2012-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/
[link-datactr-2012-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/
[link-datactr-2012]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/
[link-datactr-2008-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/
