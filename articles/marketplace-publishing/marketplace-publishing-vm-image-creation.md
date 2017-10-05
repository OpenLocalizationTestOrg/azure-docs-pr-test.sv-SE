---
title: "Skapa en avbildning av virtuell dator för Azure Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar om hur du skapar en avbildning av virtuell dator på Azure Marketplace för andra att köpa."
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
ms.openlocfilehash: 046ce7af40301014746c6aef07d08d81ab4adcc2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="guide-to-create-a-virtual-machine-image-for-the-azure-marketplace"></a><span data-ttu-id="21556-103">Guide för att skapa en avbildning av virtuell dator för Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="21556-103">Guide to create a virtual machine image for the Azure Marketplace</span></span>
<span data-ttu-id="21556-104">Den här artikeln **steg 2**, vägleder dig genom förbereder de virtuella hårddiskar (VHD) som du ska distribuera till Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="21556-104">This article, **Step 2**, walks you through preparing the virtual hard disks (VHDs) that you will deploy to the Azure Marketplace.</span></span> <span data-ttu-id="21556-105">De virtuella hårddiskarna är grunden för dina SKU: N.</span><span class="sxs-lookup"><span data-stu-id="21556-105">Your VHDs are the foundation of your SKU.</span></span> <span data-ttu-id="21556-106">Processen skiljer sig åt beroende på om du tillhandahåller en Linux- eller Windows-baserade SKU.</span><span class="sxs-lookup"><span data-stu-id="21556-106">The process differs depending on whether you are providing a Linux-based or Windows-based SKU.</span></span> <span data-ttu-id="21556-107">Den här artikeln täcker båda scenarierna.</span><span class="sxs-lookup"><span data-stu-id="21556-107">This article covers both scenarios.</span></span> <span data-ttu-id="21556-108">Den här processen kan utföras parallellt med [skapande av konton och registrering][link-acct-creation].</span><span class="sxs-lookup"><span data-stu-id="21556-108">This process can be performed in parallel with [Account creation and registration][link-acct-creation].</span></span>

## <a name="1-define-offers-and-skus"></a><span data-ttu-id="21556-109">1. Definiera erbjudanden och SKU: er</span><span class="sxs-lookup"><span data-stu-id="21556-109">1. Define Offers and SKUs</span></span>
<span data-ttu-id="21556-110">I det här avsnittet kan du lära dig att definiera vilka erbjudanden och deras associerade SKU: er.</span><span class="sxs-lookup"><span data-stu-id="21556-110">In this section, you learn to define the offers and their associated SKUs.</span></span>

<span data-ttu-id="21556-111">Ett erbjudande är överordnat alla sina SKU:er.</span><span class="sxs-lookup"><span data-stu-id="21556-111">An offer is a "parent" to all of its SKUs.</span></span> <span data-ttu-id="21556-112">Du kan ha flera erbjudanden.</span><span class="sxs-lookup"><span data-stu-id="21556-112">You can have multiple offers.</span></span> <span data-ttu-id="21556-113">Hur du väljer att strukturera dina erbjudanden är upp till dig.</span><span class="sxs-lookup"><span data-stu-id="21556-113">How you decide to structure your offers is up to you.</span></span> <span data-ttu-id="21556-114">När ett erbjudande skickas till mellanlagringen, skickas det tillsammans med alla SKU:er.</span><span class="sxs-lookup"><span data-stu-id="21556-114">When an offer is pushed to staging, it is pushed along with all of its SKUs.</span></span> <span data-ttu-id="21556-115">Du noga överväga din SKU-identifierare, eftersom de kommer att vara synliga i URL-Adressen:</span><span class="sxs-lookup"><span data-stu-id="21556-115">Carefully consider your SKU identifiers, because they will be visible in the URL:</span></span>

* <span data-ttu-id="21556-116">Azure.com: http://azure.microsoft.com/marketplace/partners/ {PartnerNamespace} / {OfferIdentifier}-{SKUidentifier}</span><span class="sxs-lookup"><span data-stu-id="21556-116">Azure.com: http://azure.microsoft.com/marketplace/partners/{PartnerNamespace}/{OfferIdentifier}-{SKUidentifier}</span></span>
* <span data-ttu-id="21556-117">Azure preview portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {SKUIDdentifier}</span><span class="sxs-lookup"><span data-stu-id="21556-117">Azure preview portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{SKUIDdentifier}</span></span>  

<span data-ttu-id="21556-118">En SKU är kommersiella namnet för en VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="21556-118">A SKU is the commercial name for a VM image.</span></span> <span data-ttu-id="21556-119">En VM-avbildning som innehåller en operativsystemdisk och noll eller flera datadiskar.</span><span class="sxs-lookup"><span data-stu-id="21556-119">A VM image contains one operating system disk and zero or more data disks.</span></span> <span data-ttu-id="21556-120">Det är i grund och botten den fullständiga lagringsprofilen för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="21556-120">It is essentially the complete storage profile for a virtual machine.</span></span> <span data-ttu-id="21556-121">En virtuell Hårddisk krävs per disk.</span><span class="sxs-lookup"><span data-stu-id="21556-121">One VHD is needed per disk.</span></span> <span data-ttu-id="21556-122">Även tomt datadiskar kräver en virtuell Hårddisk som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="21556-122">Even blank data disks require a VHD to be created.</span></span>

<span data-ttu-id="21556-123">Oavsett vilket operativsystem du använder lägger du endast till det minsta antalet datadiskar som SKU:n kräver.</span><span class="sxs-lookup"><span data-stu-id="21556-123">Regardless of which operating system you use, add only the minimum number of data disks needed by the SKU.</span></span> <span data-ttu-id="21556-124">Kunder kan inte ta bort diskar som är en del av en avbildning vid tidpunkten för distribution, men kan alltid lägga till diskar under eller efter distributionen om de behöver dem..</span><span class="sxs-lookup"><span data-stu-id="21556-124">Customers cannot remove disks that are part of an image at the time of deployment but can always add disks during or after deployment if they need them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21556-125">**Ändra inte Diskantalet i en ny Avbildningsversion.**</span><span class="sxs-lookup"><span data-stu-id="21556-125">**Do not change disk count in a new image version.**</span></span> <span data-ttu-id="21556-126">Definiera en ny SKU om du måste konfigurera om datadiskar i bilden.</span><span class="sxs-lookup"><span data-stu-id="21556-126">If you must reconfigure Data disks in the image, define a new SKU.</span></span> <span data-ttu-id="21556-127">En ny version för avbildningen med annan disk antal kommer ha risken för senaste ny distribution baseras på den nya avbildning versionen i fall av automatisk skalning, automatisk distribution av lösningar via ARM-mallar och andra scenarier.</span><span class="sxs-lookup"><span data-stu-id="21556-127">Publishing a new image version with different disk counts will have the potential of breaking new deployment based on the new image version in cases of auto-scaling, automatic deployments of solutions through ARM templates and other scenarios.</span></span>
>
>

### <a name="11-add-an-offer"></a><span data-ttu-id="21556-128">1.1 Lägg till ett erbjudande</span><span class="sxs-lookup"><span data-stu-id="21556-128">1.1 Add an offer</span></span>
1. <span data-ttu-id="21556-129">Logga in på den [Publiceringsportal] [ link-pubportal] med säljaren-konto.</span><span class="sxs-lookup"><span data-stu-id="21556-129">Sign in to the [Publishing Portal][link-pubportal] by using your seller account.</span></span>
2. <span data-ttu-id="21556-130">Välj den **virtuella datorer** fliken Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="21556-130">Select the **Virtual Machines** tab of the Publishing Portal.</span></span> <span data-ttu-id="21556-131">Ange ditt erbjudande i fältet Ange transaktion.</span><span class="sxs-lookup"><span data-stu-id="21556-131">In the prompted entry field, enter your offer name.</span></span> <span data-ttu-id="21556-132">Erbjudandenamn är vanligtvis namnet på den produkt eller tjänst som du planerar att sälja i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="21556-132">The offer name is typically the name of the product or service that you plan to sell in the Azure Marketplace.</span></span>
3. <span data-ttu-id="21556-133">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="21556-133">Select **Create**.</span></span>

### <a name="12-define-a-sku"></a><span data-ttu-id="21556-134">1.2 definiera en SKU</span><span class="sxs-lookup"><span data-stu-id="21556-134">1.2 Define a SKU</span></span>
<span data-ttu-id="21556-135">När du har lagt till ett erbjudande, måste du definiera och identifiera din SKU: er.</span><span class="sxs-lookup"><span data-stu-id="21556-135">After you have added an offer, you need to define and identify your SKUs.</span></span> <span data-ttu-id="21556-136">Du kan ha flera erbjudanden och varje erbjudande kan ha flera SKU: er under den.</span><span class="sxs-lookup"><span data-stu-id="21556-136">You can have multiple offers, and each offer can have multiple SKUs under it.</span></span> <span data-ttu-id="21556-137">När ett erbjudande skickas till mellanlagringen, skickas det tillsammans med alla SKU:er.</span><span class="sxs-lookup"><span data-stu-id="21556-137">When an offer is pushed to staging, it is pushed along with all of its SKUs.</span></span>

1. <span data-ttu-id="21556-138">**Lägg till en SKU.**</span><span class="sxs-lookup"><span data-stu-id="21556-138">**Add a SKU.**</span></span> <span data-ttu-id="21556-139">SKU: N kräver en identifierare som används i URL: en.</span><span class="sxs-lookup"><span data-stu-id="21556-139">The SKU requires an identifier, which is used in the URL.</span></span> <span data-ttu-id="21556-140">Identifieraren måste vara unikt inom din publiceringsprofil, men det finns ingen risk för identifierare kollision med andra utgivare.</span><span class="sxs-lookup"><span data-stu-id="21556-140">The identifier must be unique within your publishing profile, but there is no risk of identifier collision with other publishers.</span></span>

   > [!NOTE]
   > <span data-ttu-id="21556-141">Erbjudandet och SKU-ID: n visas i URL-Adressen för erbjudande på Marketplace.</span><span class="sxs-lookup"><span data-stu-id="21556-141">The offer and SKU identifiers are displayed in the offer URL in the Marketplace.</span></span>
   >
   >
2. <span data-ttu-id="21556-142">**Lägga till en sammanfattande beskrivning för din SKU.**</span><span class="sxs-lookup"><span data-stu-id="21556-142">**Add a summary description for your SKU.**</span></span> <span data-ttu-id="21556-143">Översikt över beskrivningar är synlig för kunderna, så bör du se dem lättläst.</span><span class="sxs-lookup"><span data-stu-id="21556-143">Summary descriptions are visible to customers, so you should make them easily readable.</span></span> <span data-ttu-id="21556-144">Den här informationen behöver inte vara låst tills fasen ”Push till mellanlagring”.</span><span class="sxs-lookup"><span data-stu-id="21556-144">This information does not need to be locked until the "Push to Staging" phase.</span></span> <span data-ttu-id="21556-145">Till dess kan du redigera den som du vill.</span><span class="sxs-lookup"><span data-stu-id="21556-145">Until then, you are free to edit it.</span></span>
3. <span data-ttu-id="21556-146">Om du använder Windows-baserade SKU:er följer du de rekommenderade länkarna för att hämta godkända versioner av Windows Server.</span><span class="sxs-lookup"><span data-stu-id="21556-146">If you are using Windows-based SKUs, follow the suggested links to acquire the approved versions of Windows Server.</span></span>

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a><span data-ttu-id="21556-147">2. Skapa en virtuell Hårddisk på Azure-kompatibel (Linux-baserat)</span><span class="sxs-lookup"><span data-stu-id="21556-147">2. Create an Azure-compatible VHD (Linux-based)</span></span>
<span data-ttu-id="21556-148">Det här avsnittet fokuserar på bästa praxis för att skapa en Linux-baserade VM-avbildning för Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="21556-148">This section focuses on best practices for creating a Linux-based VM image for the Azure Marketplace.</span></span> <span data-ttu-id="21556-149">En stegvis genomgång finns följande dokumentation: [skapa och ladda upp en virtuell hårddisk som innehåller Linux-operativsystem](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="21556-149">For a step-by-step walkthrough, refer to the following documentation: [Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a><span data-ttu-id="21556-150">3. Skapa en virtuell Hårddisk på Azure-kompatibel (Windows-baserade)</span><span class="sxs-lookup"><span data-stu-id="21556-150">3. Create an Azure-compatible VHD (Windows-based)</span></span>
<span data-ttu-id="21556-151">Det här avsnittet fokuserar på hur du skapar en SKU baserat på Windows Server för Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="21556-151">This section focuses on the steps to create a SKU based on Windows Server for the Azure Marketplace.</span></span>

### <a name="31-ensure-that-you-are-using-the-correct-base-vhds"></a><span data-ttu-id="21556-152">3.1 kontrollerar du att du använder rätt base VHD</span><span class="sxs-lookup"><span data-stu-id="21556-152">3.1 Ensure that you are using the correct base VHDs</span></span>
<span data-ttu-id="21556-153">Operativsystemet VHD för VM-avbildning måste baseras på en Azure-godkända basavbildning som innehåller Windows Server eller SQL Server.</span><span class="sxs-lookup"><span data-stu-id="21556-153">The operating system VHD for your VM image must be based on an Azure-approved base image that contains Windows Server or SQL Server.</span></span>

<span data-ttu-id="21556-154">Om du vill börja skapa en virtuell dator från en av följande avbildningar som finns i den [Microsoft Azure-portalen][link-azure-portal]:</span><span class="sxs-lookup"><span data-stu-id="21556-154">To begin, create a VM from one of the following images, located at the [Microsoft Azure portal][link-azure-portal]:</span></span>

* <span data-ttu-id="21556-155">Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1][link-datactr-2008-r2])</span><span class="sxs-lookup"><span data-stu-id="21556-155">Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1][link-datactr-2008-r2])</span></span>
* <span data-ttu-id="21556-156">SQLServer 2014 ([Enterprise][link-sql-2014-ent], [Standard][link-sql-2014-std], [Web][link-sql-2014-web])</span><span class="sxs-lookup"><span data-stu-id="21556-156">SQL Server 2014 ([Enterprise][link-sql-2014-ent], [Standard][link-sql-2014-std], [Web][link-sql-2014-web])</span></span>
* <span data-ttu-id="21556-157">SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [Standard][link-sql-2012-std], [Web][link-sql-2012-web])</span><span class="sxs-lookup"><span data-stu-id="21556-157">SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [Standard][link-sql-2012-std], [Web][link-sql-2012-web])</span></span>

<span data-ttu-id="21556-158">Du hittar även dessa länkar i publiceringsportalen på SKU-sidan.</span><span class="sxs-lookup"><span data-stu-id="21556-158">These links can also be found in the Publishing Portal under the SKU page.</span></span>

> [!TIP]
> <span data-ttu-id="21556-159">Om du använder den aktuella Azure-portalen eller PowerShell, har Windows Server-avbildningar publicerade på 8 September 2014 och senare godkänts.</span><span class="sxs-lookup"><span data-stu-id="21556-159">If you are using the current Azure portal or PowerShell, Windows Server images published on September 8, 2014 and later are approved.</span></span>
>
>

### <a name="32-create-your-windows-based-vm"></a><span data-ttu-id="21556-160">3.2 Skapa ditt Windows-baserad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="21556-160">3.2 Create your Windows-based VM</span></span>
<span data-ttu-id="21556-161">Du kan använda Microsoft Azure-portalen för att skapa den virtuella datorn baserat på en godkänd basavbildning i några enkla steg.</span><span class="sxs-lookup"><span data-stu-id="21556-161">From the Microsoft Azure portal, you can create your VM based on an approved base image in just a few simple steps.</span></span> <span data-ttu-id="21556-162">Följande är en översikt över processen:</span><span class="sxs-lookup"><span data-stu-id="21556-162">The following is an overview of the process:</span></span>

1. <span data-ttu-id="21556-163">Från sidan basavbildning Välj **Skapa virtuell dator** omdirigeras till den nya [Microsoft Azure-portalen][link-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="21556-163">From the base image page, select **Create Virtual Machine** to be directed to the new [Microsoft Azure portal][link-azure-portal].</span></span>

    ![Rita][img-acom-1]
2. <span data-ttu-id="21556-165">Logga in på portalen med Microsoft-konto och lösenord för Azure-prenumerationen du vill använda.</span><span class="sxs-lookup"><span data-stu-id="21556-165">Sign in to the portal with the Microsoft account and password for the Azure subscription you want to use.</span></span>
3. <span data-ttu-id="21556-166">Följ anvisningarna för att skapa en virtuell dator med hjälp av basavbildningen som du har valt.</span><span class="sxs-lookup"><span data-stu-id="21556-166">Follow the prompts to create a VM by using the base image you have selected.</span></span> <span data-ttu-id="21556-167">Du måste ange en värd (namn på datorn), användarnamn (registrerad som administratör) och lösenord för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="21556-167">You need to provide a host name (name of the computer), user name (registered as an administrator), and password for the VM.</span></span>

    ![Rita][img-portal-vm-create]
4. <span data-ttu-id="21556-169">Välj storleken på den virtuella datorn ska distribueras:</span><span class="sxs-lookup"><span data-stu-id="21556-169">Select the size of the VM to deploy:</span></span>

    <span data-ttu-id="21556-170">a.</span><span class="sxs-lookup"><span data-stu-id="21556-170">a.</span></span>    <span data-ttu-id="21556-171">Om du planerar att utveckla VHD lokal spelar storleken ingen roll.</span><span class="sxs-lookup"><span data-stu-id="21556-171">If you plan to develop the VHD on-premises, the size does not matter.</span></span> <span data-ttu-id="21556-172">Överväg att använda en av de mindre virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="21556-172">Consider using one of the smaller VMs.</span></span>

    <span data-ttu-id="21556-173">b.</span><span class="sxs-lookup"><span data-stu-id="21556-173">b.</span></span>    <span data-ttu-id="21556-174">Om du planerar att utveckla avbildningen i Azure bör du överväga att använda en av de storlekar som rekommenderas för den valda avbildningen.</span><span class="sxs-lookup"><span data-stu-id="21556-174">If you plan to develop the image in Azure, consider using one of the recommended VM sizes for the selected image.</span></span>

    <span data-ttu-id="21556-175">c.</span><span class="sxs-lookup"><span data-stu-id="21556-175">c.</span></span>    <span data-ttu-id="21556-176">Mer information om priser finns i den **rekommenderas prisnivåer** selector visas på portalen.</span><span class="sxs-lookup"><span data-stu-id="21556-176">For pricing information, refer to the **Recommended pricing tiers** selector displayed on the portal.</span></span> <span data-ttu-id="21556-177">Den tillhandahåller de tre storlekar som rekommenderas av utgivaren.</span><span class="sxs-lookup"><span data-stu-id="21556-177">It will provide the three recommended sizes provided by the publisher.</span></span> <span data-ttu-id="21556-178">(I det här fallet är utgivaren Microsoft.)</span><span class="sxs-lookup"><span data-stu-id="21556-178">(In this case, the publisher is Microsoft.)</span></span>

    ![Rita][img-portal-vm-size]
5. <span data-ttu-id="21556-180">Ange egenskaper för:</span><span class="sxs-lookup"><span data-stu-id="21556-180">Set properties:</span></span>

    <span data-ttu-id="21556-181">a.</span><span class="sxs-lookup"><span data-stu-id="21556-181">a.</span></span>    <span data-ttu-id="21556-182">Du kan lämna standardvärdena för egenskaper under för snabbdistribution, **valfri konfiguration** och **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="21556-182">For quick deployment, you can leave the default values for the properties under **Optional Configuration** and **Resource Group**.</span></span>

    <span data-ttu-id="21556-183">b.</span><span class="sxs-lookup"><span data-stu-id="21556-183">b.</span></span>    <span data-ttu-id="21556-184">Under **Lagringskonto**, du kan också välja lagringskontot som operativsystemet virtuella Hårddisken ska sparas.</span><span class="sxs-lookup"><span data-stu-id="21556-184">Under **Storage Account**, you can optionally select the storage account in which the operating system VHD will be stored.</span></span>

    <span data-ttu-id="21556-185">c.</span><span class="sxs-lookup"><span data-stu-id="21556-185">c.</span></span>    <span data-ttu-id="21556-186">Under **resursgruppen**, du kan också välja logisk grupp att placera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="21556-186">Under **Resource Group**, you can optionally select the logical group in which to place the VM.</span></span>
6. <span data-ttu-id="21556-187">Välj den **plats** för distribution:</span><span class="sxs-lookup"><span data-stu-id="21556-187">Select the **Location** for deployment:</span></span>

    <span data-ttu-id="21556-188">a.</span><span class="sxs-lookup"><span data-stu-id="21556-188">a.</span></span>    <span data-ttu-id="21556-189">Om du planerar att utveckla VHD lokal roll inte platsen eftersom du kommer att överföra avbildningen till Azure senare.</span><span class="sxs-lookup"><span data-stu-id="21556-189">If you plan to develop the VHD on-premises, the location does not matter because you will upload the image to Azure later.</span></span>

    <span data-ttu-id="21556-190">b.</span><span class="sxs-lookup"><span data-stu-id="21556-190">b.</span></span>    <span data-ttu-id="21556-191">Om du planerar att utveckla avbildningen i Azure bör du överväga att använda en av amerikanska Microsoft Azure-regionerna från start.</span><span class="sxs-lookup"><span data-stu-id="21556-191">If you plan to develop the image in Azure, consider using one of the US-based Microsoft Azure regions from the beginning.</span></span> <span data-ttu-id="21556-192">Detta förbättrar VHD kopieringen som Microsoft utför åt dig när du skickar avbildningen för certifiering.</span><span class="sxs-lookup"><span data-stu-id="21556-192">This speeds up the VHD copying process that Microsoft performs on your behalf when you submit your image for certification.</span></span>

    ![Rita][img-portal-vm-location]
7. <span data-ttu-id="21556-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="21556-194">Click **Create**.</span></span> <span data-ttu-id="21556-195">Den virtuella datorn börjar distribuera.</span><span class="sxs-lookup"><span data-stu-id="21556-195">The VM starts to deploy.</span></span> <span data-ttu-id="21556-196">Inom några minuter har du en färdig distribution och kan börja skapa avbildningen för din SKU.</span><span class="sxs-lookup"><span data-stu-id="21556-196">Within minutes, you will have a successful deployment and can begin to create the image for your SKU.</span></span>

### <a name="33-develop-your-vhd-in-the-cloud"></a><span data-ttu-id="21556-197">3.3 utveckla den virtuella Hårddisken i molnet</span><span class="sxs-lookup"><span data-stu-id="21556-197">3.3 Develop your VHD in the cloud</span></span>
<span data-ttu-id="21556-198">Vi rekommenderar starkt att du utveckla den virtuella Hårddisken i molnet genom att använda Remote Desktop Protocol (RDP).</span><span class="sxs-lookup"><span data-stu-id="21556-198">We strongly recommend that you develop your VHD in the cloud by using Remote Desktop Protocol (RDP).</span></span> <span data-ttu-id="21556-199">Du ansluter till RDP med användarnamn och lösenord har angetts under etableringen.</span><span class="sxs-lookup"><span data-stu-id="21556-199">You connect to RDP with the user name and password specified during provisioning.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21556-200">Om du utvecklar dina VHD lokalt (vilket inte rekommenderas), se [och skapa en avbildning av virtuell dator lokalt](marketplace-publishing-vm-image-creation-on-premise.md).</span><span class="sxs-lookup"><span data-stu-id="21556-200">If you develop your VHD on-premises (which is not recommended), see [Creating a virtual machine image on-premises](marketplace-publishing-vm-image-creation-on-premise.md).</span></span> <span data-ttu-id="21556-201">Hämta den virtuella Hårddisken är inte nödvändigt om du utvecklar i molnet.</span><span class="sxs-lookup"><span data-stu-id="21556-201">Downloading your VHD is not necessary if you are developing in the cloud.</span></span>
>
>

<span data-ttu-id="21556-202">**Ansluta via RDP med hjälp av den [Microsoft Azure-portalen][link-azure-portal]**</span><span class="sxs-lookup"><span data-stu-id="21556-202">**Connect via RDP using the [Microsoft Azure portal][link-azure-portal]**</span></span>

1. <span data-ttu-id="21556-203">Välj **Bläddra** > **VMs**.</span><span class="sxs-lookup"><span data-stu-id="21556-203">Select **Browse** > **VMs**.</span></span>
2. <span data-ttu-id="21556-204">Öppnar bladet för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="21556-204">The Virtual machines blade opens.</span></span> <span data-ttu-id="21556-205">Kontrollera att den virtuella datorn som du vill ansluta med körs och markera den i listan med distribuerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="21556-205">Ensure that the VM that you want to connect with is running, and then select it from the list of deployed VMs.</span></span>
3. <span data-ttu-id="21556-206">Då öppnas ett blad som beskriver den valda virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="21556-206">A blade opens that describes the selected VM.</span></span> <span data-ttu-id="21556-207">Överst, klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="21556-207">At the top, click **Connect**.</span></span>
4. <span data-ttu-id="21556-208">Du uppmanas att ange användarnamn och lösenord som du angav under etableringen.</span><span class="sxs-lookup"><span data-stu-id="21556-208">You are prompted to enter the user name and password that you specified during provisioning.</span></span>

<span data-ttu-id="21556-209">**Anslut via RDP med hjälp av PowerShell**</span><span class="sxs-lookup"><span data-stu-id="21556-209">**Connect via RDP using PowerShell**</span></span>

<span data-ttu-id="21556-210">Du kan hämta en remote desktop-fil till en lokal dator den [cmdlet Get-AzureRemoteDesktopFile][link-technet-2].</span><span class="sxs-lookup"><span data-stu-id="21556-210">To download a remote desktop file to a local machine, use the [Get-AzureRemoteDesktopFile cmdlet][link-technet-2].</span></span> <span data-ttu-id="21556-211">Du måste känna till namnet på tjänsten och namnet på den virtuella datorn för att kunna använda denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="21556-211">In order to use this cmdlet, you need to know the name of the service and name of the VM.</span></span> <span data-ttu-id="21556-212">Om du har skapat den virtuella datorn från den [Microsoft Azure-portalen][link-azure-portal], du kan hitta den här informationen under Egenskaper för Virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="21556-212">If you created the VM from the [Microsoft Azure portal][link-azure-portal], you can find this information under VM properties:</span></span>

1. <span data-ttu-id="21556-213">Välj i Microsoft Azure-portalen **Bläddra** > **VMs**.</span><span class="sxs-lookup"><span data-stu-id="21556-213">In the Microsoft Azure portal, select **Browse** > **VMs**.</span></span>
2. <span data-ttu-id="21556-214">Öppnar bladet för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="21556-214">The Virtual machines blade opens.</span></span> <span data-ttu-id="21556-215">Välj den virtuella datorn som du har distribuerat.</span><span class="sxs-lookup"><span data-stu-id="21556-215">Select the VM that you deployed.</span></span>
3. <span data-ttu-id="21556-216">Då öppnas ett blad som beskriver den valda virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="21556-216">A blade opens that describes the selected VM.</span></span>
4. <span data-ttu-id="21556-217">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="21556-217">Click **Properties**.</span></span>
5. <span data-ttu-id="21556-218">Den första delen av domännamnet är tjänstens namn.</span><span class="sxs-lookup"><span data-stu-id="21556-218">The first portion of the domain name is the service name.</span></span> <span data-ttu-id="21556-219">Värdnamnet är namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="21556-219">The host name is the VM name.</span></span>

    ![Rita][img-portal-vm-rdp]
6. <span data-ttu-id="21556-221">För att ladda ned RDP-filen för den skapade Virtuellt för administratörens lokala skrivbordet är som följer.</span><span class="sxs-lookup"><span data-stu-id="21556-221">The cmdlet to download the RDP file for the created VM to the administrator's local desktop is as follows.</span></span>

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

<span data-ttu-id="21556-222">Mer information om RDP kan hittas på MSDN i artikeln [Anslut till en virtuell Azure-dator med RDP eller SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).</span><span class="sxs-lookup"><span data-stu-id="21556-222">More information about RDP can be found on MSDN in the article [Connect to an Azure VM with RDP or SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).</span></span>

<span data-ttu-id="21556-223">**Konfigurera en virtuell dator och skapa din SKU**</span><span class="sxs-lookup"><span data-stu-id="21556-223">**Configure a VM and create your SKU**</span></span>

<span data-ttu-id="21556-224">Efter att operativsystemet VHD hämtas, Använd Hyper-v och konfigurera en virtuell dator för att skapa din SKU.</span><span class="sxs-lookup"><span data-stu-id="21556-224">After the operating system VHD is downloaded, use Hyper­V and configure a VM to begin creating your SKU.</span></span> <span data-ttu-id="21556-225">Detaljerade anvisningar finns i följande TechNet-länk: [installera Hyper-v och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="21556-225">Detailed steps can be found at the following TechNet link: [Install Hyper­V and Configure a VM](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="34-choose-the-correct-vhd-size"></a><span data-ttu-id="21556-226">3.4 Välj rätt storlek för virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="21556-226">3.4 Choose the correct VHD size</span></span>
<span data-ttu-id="21556-227">Windows-operativsystemet VHD i VM-avbildning ska skapas som en virtuell Hårddisk fast format 128 GB.</span><span class="sxs-lookup"><span data-stu-id="21556-227">The Windows operating system VHD in your VM image should be created as a 128-GB fixed-format VHD.</span></span>  

<span data-ttu-id="21556-228">Om den fysiska storleken är mindre än 128 GB, vara den virtuella Hårddisken sparse.</span><span class="sxs-lookup"><span data-stu-id="21556-228">If the physical size is less than 128 GB, the VHD should be sparse.</span></span> <span data-ttu-id="21556-229">Grundläggande Windows- och SQL Server avbildningar som uppfyller redan kraven, så ändras inte formatet eller storleken på den virtuella Hårddisken som hämtades.</span><span class="sxs-lookup"><span data-stu-id="21556-229">The base Windows and SQL Server images provided already meet these requirements, so do not change the format or the size of the VHD obtained.</span></span>  

<span data-ttu-id="21556-230">Datadiskar kan vara så stor som 1 TB.</span><span class="sxs-lookup"><span data-stu-id="21556-230">Data disks can be as large as 1 TB.</span></span> <span data-ttu-id="21556-231">När du funderar över diskens storlek, Kom ihåg att kunder inte ändra storlek på virtuella hårddiskar i en bild vid tidpunkten för distribution.</span><span class="sxs-lookup"><span data-stu-id="21556-231">When deciding on the disk size, remember that customers cannot resize VHDs within an image at the time of deployment.</span></span> <span data-ttu-id="21556-232">Data disk virtuella hårddiskar ska skapas som en virtuell Hårddisk med fast format.</span><span class="sxs-lookup"><span data-stu-id="21556-232">Data disk VHDs should be created as a fixed-format VHD.</span></span> <span data-ttu-id="21556-233">De bör också vara sparse.</span><span class="sxs-lookup"><span data-stu-id="21556-233">They should also be sparse.</span></span> <span data-ttu-id="21556-234">Datadiskar kan antingen vara tomma eller innehålla data.</span><span class="sxs-lookup"><span data-stu-id="21556-234">Data disks can be empty or contain data.</span></span>

### <a name="35-install-the-latest-windows-patches"></a><span data-ttu-id="21556-235">3.5 Installera de senaste korrigeringarna för Windows</span><span class="sxs-lookup"><span data-stu-id="21556-235">3.5 Install the latest Windows patches</span></span>
<span data-ttu-id="21556-236">Källavbildningen innehåller de senaste uppdateringarna fram till och med det datum då den publicerades.</span><span class="sxs-lookup"><span data-stu-id="21556-236">The base images contain the latest patches up to their published date.</span></span> <span data-ttu-id="21556-237">Se till att Windows Update har körts och att samtliga senaste kritiska och viktiga uppdateringar har installerats innan du publicerar operativsystemet VHD som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="21556-237">Before publishing the operating system VHD you have created, ensure that Windows Update has been run and that all the latest Critical and Important security updates have been installed.</span></span>

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a><span data-ttu-id="21556-238">3,6 utföra ytterligare aktiviteter för konfiguration och schema vid behov</span><span class="sxs-lookup"><span data-stu-id="21556-238">3.6 Perform additional configuration and schedule tasks as necessary</span></span>
<span data-ttu-id="21556-239">Överväg att använda en schemalagd aktivitet som körs vid start att göra ytterligare ändringar för den virtuella datorn när den har distribuerats om ytterligare konfiguration krävs:</span><span class="sxs-lookup"><span data-stu-id="21556-239">If additional configuration is needed, consider using a scheduled task that runs at startup to make any final changes to the VM after it has been deployed:</span></span>

* <span data-ttu-id="21556-240">Det är bäst att låta uppgiften radera sig själv när den här utförts.</span><span class="sxs-lookup"><span data-stu-id="21556-240">It is a best practice to have the task delete itself upon successful execution.</span></span>
* <span data-ttu-id="21556-241">Ingen konfiguration bör lita på enheter än enheter C eller D, eftersom de är bara två enheterna som alltid är garanterat finns.</span><span class="sxs-lookup"><span data-stu-id="21556-241">No configuration should rely on drives other than drives C or D, because these are the only two drives that are always guaranteed to exist.</span></span> <span data-ttu-id="21556-242">Enhet D är tillfällig lokal disk enhet C är operativsystemets disk.</span><span class="sxs-lookup"><span data-stu-id="21556-242">Drive C is the operating system disk, and drive D is the temporary local disk.</span></span>

### <a name="37-generalize-the-image"></a><span data-ttu-id="21556-243">3.7 generalisera avbildningen</span><span class="sxs-lookup"><span data-stu-id="21556-243">3.7 Generalize the image</span></span>
<span data-ttu-id="21556-244">Alla avbildningar i Azure Marketplace måste vara återanvändbara i ett allmänt sätt.</span><span class="sxs-lookup"><span data-stu-id="21556-244">All images in the Azure Marketplace must be reusable in a generic fashion.</span></span> <span data-ttu-id="21556-245">Med andra ord måste vara generaliserad virtuell Hårddisk för operativsystem:</span><span class="sxs-lookup"><span data-stu-id="21556-245">In other words, the operating system VHD must be generalized:</span></span>

* <span data-ttu-id="21556-246">För Windows, bilden ska vara ”Sysprep” och inga konfigurationer bör göras som inte stöder den **sysprep** kommando.</span><span class="sxs-lookup"><span data-stu-id="21556-246">For Windows, the image should be "sysprepped," and no configurations should be done that do not support the **sysprep** command.</span></span>
* <span data-ttu-id="21556-247">Du kan köra följande kommando från katalogen % windir%\System32\Sysprep.</span><span class="sxs-lookup"><span data-stu-id="21556-247">You can run the following command from the directory %windir%\System32\Sysprep.</span></span>

        sysprep.exe /generalize /oobe /shutdown

  <span data-ttu-id="21556-248">Riktlinjer för att köra sysprep tillhandahålls hur operativsystemet i steg i följande MSDN-artikel: [skapa och ladda upp en Windows Server VHD till Azure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="21556-248">Guidance on how to sysprep the operating system is provided in Step of the following MSDN article: [Create and upload a Windows Server VHD to Azure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="4-deploy-a-vm-from-your-vhds"></a><span data-ttu-id="21556-249">4. Distribuera en virtuell dator från din virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="21556-249">4. Deploy a VM from your VHDs</span></span>
<span data-ttu-id="21556-250">När du har laddat upp de virtuella hårddiskarna (operativsystemet generaliserad virtuell Hårddisk och noll eller fler data disk virtuella hårddiskar) till ett Azure storage-konto, kan du registrera dem som en användare VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="21556-250">After you have uploaded your VHDs (the generalized operating system VHD and zero or more data disk VHDs) to an Azure storage account, you can register them as a user VM image.</span></span> <span data-ttu-id="21556-251">Du kan sedan testa avbildningen.</span><span class="sxs-lookup"><span data-stu-id="21556-251">Then you can test that image.</span></span> <span data-ttu-id="21556-252">Observera att eftersom operativsystemet VHD är generaliserad, du inte kan direkt distribuera den virtuella datorn genom att tillhandahålla VHD-URL.</span><span class="sxs-lookup"><span data-stu-id="21556-252">Note that because your operating system VHD is generalized, you cannot directly deploy the VM by providing the VHD URL.</span></span>

<span data-ttu-id="21556-253">Om du vill veta mer om VM-avbildningar kan du granska följande blogginlägg:</span><span class="sxs-lookup"><span data-stu-id="21556-253">To learn more about VM images, review the following blog posts:</span></span>

* [<span data-ttu-id="21556-254">VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="21556-254">VM Image</span></span>](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [<span data-ttu-id="21556-255">VM avbildningen PowerShell så</span><span class="sxs-lookup"><span data-stu-id="21556-255">VM Image PowerShell How To</span></span>](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [<span data-ttu-id="21556-256">Om VM-avbildningar i Azure</span><span class="sxs-lookup"><span data-stu-id="21556-256">About VM images in Azure</span></span>](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-the-necessary-tools-powershell-and-azure-cli"></a><span data-ttu-id="21556-257">Ställa in verktyg som krävs, PowerShell och Azure CLI</span><span class="sxs-lookup"><span data-stu-id="21556-257">Set up the necessary tools, PowerShell and Azure CLI</span></span>
* [<span data-ttu-id="21556-258">Hur du konfigurerar PowerShell</span><span class="sxs-lookup"><span data-stu-id="21556-258">How to setup PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="21556-259">Hur du konfigurerar Azure CLI</span><span class="sxs-lookup"><span data-stu-id="21556-259">How to setup Azure CLI</span></span>](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a><span data-ttu-id="21556-260">4.1 skapa en användare VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="21556-260">4.1 Create a user VM image</span></span>
#### <a name="capture-vm"></a><span data-ttu-id="21556-261">Spela in VM</span><span class="sxs-lookup"><span data-stu-id="21556-261">Capture VM</span></span>
<span data-ttu-id="21556-262">Läs länkar som anges nedan vägledning om att avbilda den virtuella datorn med Azure-API/PowerShell CLI.</span><span class="sxs-lookup"><span data-stu-id="21556-262">Please read the links given below for guidance on capturing the VM using API/PowerShell/Azure CLI.</span></span>

* [<span data-ttu-id="21556-263">API</span><span class="sxs-lookup"><span data-stu-id="21556-263">API</span></span>](https://msdn.microsoft.com/library/mt163560.aspx)
* [<span data-ttu-id="21556-264">PowerShell</span><span class="sxs-lookup"><span data-stu-id="21556-264">PowerShell</span></span>](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="21556-265">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="21556-265">Azure CLI</span></span>](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a><span data-ttu-id="21556-266">Generalisera avbildningen</span><span class="sxs-lookup"><span data-stu-id="21556-266">Generalize Image</span></span>
<span data-ttu-id="21556-267">Läs länkar som anges nedan vägledning om att avbilda den virtuella datorn med Azure-API/PowerShell CLI.</span><span class="sxs-lookup"><span data-stu-id="21556-267">Please read the links given below for guidance on capturing the VM using API/PowerShell/Azure CLI.</span></span>

* [<span data-ttu-id="21556-268">API</span><span class="sxs-lookup"><span data-stu-id="21556-268">API</span></span>](https://msdn.microsoft.com/library/mt269439.aspx)
* [<span data-ttu-id="21556-269">PowerShell</span><span class="sxs-lookup"><span data-stu-id="21556-269">PowerShell</span></span>](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="21556-270">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="21556-270">Azure CLI</span></span>](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a><span data-ttu-id="21556-271">4.2 distribuera en virtuell dator från en användare VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="21556-271">4.2 Deploy a VM from a user VM image</span></span>
<span data-ttu-id="21556-272">Om du vill distribuera en virtuell dator från en användare VM-avbildning, du kan använda aktuellt [Azure-portalen](https://manage.windowsazure.com) eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21556-272">To deploy a VM from a user VM image, you can use the current [Azure portal](https://manage.windowsazure.com) or PowerShell.</span></span>

<span data-ttu-id="21556-273">**Distribuera en virtuell dator från den aktuella Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="21556-273">**Deploy a VM from the current Azure portal**</span></span>

1. <span data-ttu-id="21556-274">Gå till **ny** > **Compute** > **virtuella** > **från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="21556-274">Go to **New** > **Compute** > **Virtual machine** > **From gallery**.</span></span>

    ![Rita][img-manage-vm-new]
2. <span data-ttu-id="21556-276">Gå till **Mina avbildningar**, och välj sedan VM-avbildning som du vill distribuera en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="21556-276">Go to **My images**, and then select the VM image from which to deploy a VM:</span></span>

   1. <span data-ttu-id="21556-277">Var uppmärksam på vilken avbildning som du väljer, eftersom den **Mina avbildningar** visas både avbildningar av operativsystem och VM-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="21556-277">Pay close attention to which image you select, because the **My images** view lists both operating system images and VM images.</span></span>
   2. <span data-ttu-id="21556-278">Titta på antalet diskar kan hjälpa avgöra vilken typ av bild som du distribuerar, eftersom flesta av VM-avbildningar har mer än en disk.</span><span class="sxs-lookup"><span data-stu-id="21556-278">Looking at the number of disks can help determine what type of image you are deploying, because the majority of VM images have more than one disk.</span></span> <span data-ttu-id="21556-279">Det är dock fortfarande möjligt att ha en VM-avbildning med bara en enda disk, som sedan måste **antalet diskar** värdet 1.</span><span class="sxs-lookup"><span data-stu-id="21556-279">However, it is still possible to have a VM image with only a single operating system disk, which would then have **Number of disks** set to 1.</span></span>

      ![Rita][img-manage-vm-select]
3. <span data-ttu-id="21556-281">Följ guiden Skapa virtuell dator och ange den VM namn, VM storlek, plats, användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="21556-281">Follow the VM creation wizard and specify the VM name, VM size, location, user name, and password.</span></span>

<span data-ttu-id="21556-282">**Distribuera en virtuell dator från PowerShell**</span><span class="sxs-lookup"><span data-stu-id="21556-282">**Deploy a VM from PowerShell**</span></span>

<span data-ttu-id="21556-283">Du kan använda följande cmdletar för att distribuera en stor virtuell dator från generaliserad VM-avbildning som precis har skapat.</span><span class="sxs-lookup"><span data-stu-id="21556-283">To deploy a large VM from the generalized VM image just created, you can use the following cmdlets.</span></span>

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> <span data-ttu-id="21556-284">Se [felsöka vanliga problem som påträffas under skapande av VHD] för ytterligare hjälp.</span><span class="sxs-lookup"><span data-stu-id="21556-284">Please refer [Troubleshooting common issues encountered during VHD creation] for additional assistance.</span></span>
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a><span data-ttu-id="21556-285">5. Hämta certifikat för VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="21556-285">5. Obtain certification for your VM image</span></span>
<span data-ttu-id="21556-286">Nästa steg för att förbereda dina VM-avbildning för Azure Marketplace är den certifierade.</span><span class="sxs-lookup"><span data-stu-id="21556-286">The next step in preparing your VM image for the Azure Marketplace is to have it certified.</span></span>

<span data-ttu-id="21556-287">Den här processen omfattar köra ett verktyg för särskilda certifikatutfärdare, överför Verifieringsresultat till Azure-behållaren där de virtuella hårddiskarna finnas, lägga till ett erbjudande, definiera din SKU och skickar VM-avbildning för certifiering.</span><span class="sxs-lookup"><span data-stu-id="21556-287">This process includes running a special certification tool, uploading the verification results to the Azure container where your VHDs reside, adding an offer, defining your SKU, and submitting your VM image for certification.</span></span>

### <a name="51-download-and-run-the-certification-test-tool-for-azure-certified"></a><span data-ttu-id="21556-288">5.1 hämta och kör verktyget certifikatutfärdare Test för Azure-certifierad</span><span class="sxs-lookup"><span data-stu-id="21556-288">5.1 Download and run the Certification Test Tool for Azure Certified</span></span>
<span data-ttu-id="21556-289">Verktyget certifikatutfärdare körs på en aktiv virtuell dator, etablerats från användaren VM-avbildning så att den Virtuella datorn är kompatibel med Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="21556-289">The certification tool runs on a running VM, provisioned from your user VM image, to ensure that the VM image is compatible with Microsoft Azure.</span></span> <span data-ttu-id="21556-290">Verktyget verifierar att vägledningen och kraven för förberedelserna av din VHD har uppfyllts.</span><span class="sxs-lookup"><span data-stu-id="21556-290">It will verify that the guidance and requirements about preparing your VHD have been met.</span></span> <span data-ttu-id="21556-291">Utdata från verktyget är en Kompatibilitetsrapport som ska överföras i Publishing Portal när du begär certifiering.</span><span class="sxs-lookup"><span data-stu-id="21556-291">The output of the tool is a compatibility report, which should be uploaded on the Publishing Portal while requesting certification.</span></span>

<span data-ttu-id="21556-292">Verktyget certifikatutfärdare kan användas med både Windows och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="21556-292">The certification tool can be used with both Windows and Linux VMs.</span></span> <span data-ttu-id="21556-293">Ansluter till Windows-baserade virtuella datorer via PowerShell och ansluter till Linux virtuella datorer via SSH.Net:</span><span class="sxs-lookup"><span data-stu-id="21556-293">It connects to Windows-based VMs via PowerShell and connects to Linux VMs via SSH.Net:</span></span>

1. <span data-ttu-id="21556-294">Först hämta verktyget certifikatutfärdare på den [Microsoft hämtningsplats][link-msft-download].</span><span class="sxs-lookup"><span data-stu-id="21556-294">First, download the certification tool at the [Microsoft download site][link-msft-download].</span></span>
2. <span data-ttu-id="21556-295">Öppna verktyget certifikatutfärdare och klicka sedan på den **Starta nytt Test** knappen.</span><span class="sxs-lookup"><span data-stu-id="21556-295">Open the certification tool, and then click the **Start New Test** button.</span></span>
3. <span data-ttu-id="21556-296">Från den **testa Information** skärmen, anger du ett namn för kör.</span><span class="sxs-lookup"><span data-stu-id="21556-296">From the **Test Information** screen, enter a name for the test run.</span></span>
4. <span data-ttu-id="21556-297">Välj om din virtuella dator körs på Linux eller Windows.</span><span class="sxs-lookup"><span data-stu-id="21556-297">Choose whether your VM is on Linux or Windows.</span></span> <span data-ttu-id="21556-298">Beroende på vad du väljer, markerar du efterföljande alternativ.</span><span class="sxs-lookup"><span data-stu-id="21556-298">Depending on which you choose, select the subsequent options.</span></span>

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a><span data-ttu-id="21556-299">**Ansluta certifikatutfärdare verktyget till en Linux-VM-avbildning**</span><span class="sxs-lookup"><span data-stu-id="21556-299">**Connect the certification tool to a Linux VM image**</span></span>
1. <span data-ttu-id="21556-300">Välj autentiseringsläge för SSH: lösenord eller nyckelfil.</span><span class="sxs-lookup"><span data-stu-id="21556-300">Select the SSH authentication mode: password or key file.</span></span>
2. <span data-ttu-id="21556-301">Ange Domain Name System (DNS) namn, användarnamn och lösenord om du använder lösenordsbaserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="21556-301">If using password-­based authentication, enter the Domain Name System (DNS) name, user name, and password.</span></span>
3. <span data-ttu-id="21556-302">Om nyckeln autentisering med anger du DNS-namn, användarnamn och privat nyckel plats.</span><span class="sxs-lookup"><span data-stu-id="21556-302">If using key file authentication, enter the DNS name, user name, and private key location.</span></span>

   ![För lösenordsautentisering av Linux VM-avbildning][img-cert-vm-pswd-lnx]

   ![Nyckelfilen autentisering av Linux VM-avbildning][img-cert-vm-key-lnx]

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a><span data-ttu-id="21556-305">**Ansluta certifikatutfärdare verktyget till en Windows-baserade VM-avbildning**</span><span class="sxs-lookup"><span data-stu-id="21556-305">**Connect the certification tool to a Windows-based VM image**</span></span>
1. <span data-ttu-id="21556-306">Ange det fullständigt kvalificerade VM DNS-namnet (till exempel MyVMName.Cloudapp.net).</span><span class="sxs-lookup"><span data-stu-id="21556-306">Enter the fully qualified VM DNS name (for example, MyVMName.Cloudapp.net).</span></span>
2. <span data-ttu-id="21556-307">Ange användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="21556-307">Enter the user name and password.</span></span>

   ![Autentisering av lösenord i Windows VM-avbildning][img-cert-vm-pswd-win]

<span data-ttu-id="21556-309">När du har valt rätt alternativ för Linux- eller Windows-baserade Virtuella bilden, Välj **Testanslutningen** så att du har en giltig anslutning för testning av SSH.Net eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21556-309">After you have selected the correct options for your Linux or Windows-based VM image, select **Test Connection** to ensure that SSH.Net or PowerShell has a valid connection for testing purposes.</span></span> <span data-ttu-id="21556-310">När en anslutning upprättas väljer **nästa** att starta testet.</span><span class="sxs-lookup"><span data-stu-id="21556-310">After a connection is established, select **Next** to start the test.</span></span>

<span data-ttu-id="21556-311">När testet är klart visas resultaten (Pass/Fail/Warning) för varje del av testet.</span><span class="sxs-lookup"><span data-stu-id="21556-311">When the test is complete, you will receive the results (Pass/Fail/Warning) for each test element.</span></span>

![Testfall för Linux VM-avbildning][img-cert-vm-test-lnx]

![Testfall för Windows VM-avbildning][img-cert-vm-test-win]

<span data-ttu-id="21556-314">Om någon av testerna misslyckas certifieras inte bilden.</span><span class="sxs-lookup"><span data-stu-id="21556-314">If any of the tests fail, your image will not be certified.</span></span> <span data-ttu-id="21556-315">Om detta inträffar kan du granska kraven och gör nödvändiga ändringar.</span><span class="sxs-lookup"><span data-stu-id="21556-315">If this occurs, review the requirements and make any necessary changes.</span></span>

<span data-ttu-id="21556-316">Efter den automatiserade testen uppmanas du att ange ytterligare kommentarer om VM-avbildning via en frågeformulär skärm.</span><span class="sxs-lookup"><span data-stu-id="21556-316">After the automated test, you are asked to provide additional input on your VM image via a questionnaire screen.</span></span>  <span data-ttu-id="21556-317">Slutför frågorna och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="21556-317">Complete the questions, and then select **Next**.</span></span>

![Certifikatutfärdare verktyget frågeformulär][img-cert-vm-questionnaire]

![Certifikatutfärdare verktyget frågeformulär][img-cert-vm-questionnaire-2]

<span data-ttu-id="21556-320">När du har slutfört frågeformuläret, kan du ange ytterligare information, till exempel SSH åtkomstinformation för Linux VM avbildningen och en förklaring till misslyckade bedömningar.</span><span class="sxs-lookup"><span data-stu-id="21556-320">After you have completed the questionnaire, you can provide additional information such as SSH access information for the Linux VM image and an explanation for any failed assessments.</span></span> <span data-ttu-id="21556-321">Du kan hämta testresultat och loggfiler för utförda testfall förutom dina svar på frågeformulär.</span><span class="sxs-lookup"><span data-stu-id="21556-321">You can download the test results and log files for the executed test cases in addition to your answers to the questionnaire.</span></span> <span data-ttu-id="21556-322">Spara resultatet i samma behållare som de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="21556-322">Save the results in the same container as your VHDs.</span></span>

![Spara certifikatutfärdare testresultat][img-cert-vm-results]

### <a name="52-get-the-shared-access-signature-uri-for-your-vm-images"></a><span data-ttu-id="21556-324">5.2 hämta signatur för delad åtkomst URI för VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="21556-324">5.2 Get the shared access signature URI for your VM images</span></span>
<span data-ttu-id="21556-325">Under publiceringsprocessen, kan du ange de uniform resource Identifier () som kan leda till var och en av de virtuella hårddiskarna som du har skapat för ditt SKU.</span><span class="sxs-lookup"><span data-stu-id="21556-325">During the publishing process, you specify the uniform resource identifiers (URIs) that lead to each of the VHDs you have created for your SKU.</span></span> <span data-ttu-id="21556-326">Microsoft behöver åtkomst till dessa VHD:er under certifieringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="21556-326">Microsoft needs access to these VHDs during the certification process.</span></span> <span data-ttu-id="21556-327">Därför måste du skapa en signatur för delad åtkomst URI för varje virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="21556-327">Therefore, you need to create a shared access signature URI for each VHD.</span></span> <span data-ttu-id="21556-328">Det här är den URI som ska anges i den **bilder** fliken i Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="21556-328">This is the URI that should be entered in the **Images** tab in the Publishing Portal.</span></span>

<span data-ttu-id="21556-329">Signatur för delad åtkomst URI skapas måste uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="21556-329">The shared access signature URI created should adhere to the following requirements:</span></span>

* <span data-ttu-id="21556-330">När du genererar signatur för delad åtkomst URI: er för de virtuella hårddiskarna räcker lista och läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="21556-330">When generating shared access signature URIs for your VHDs, List and Read­ permissions are sufficient.</span></span> <span data-ttu-id="21556-331">Bevilja inte skriv- eller raderingsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="21556-331">Do not provide Write or Delete access.</span></span>
* <span data-ttu-id="21556-332">Varaktighet för åtkomst ska vara minst tre (3) veckor från när signatur för delad åtkomst URI: N har skapats.</span><span class="sxs-lookup"><span data-stu-id="21556-332">The duration for access should be a minimum of three (3) weeks from when the shared access signature URI is created.</span></span>
* <span data-ttu-id="21556-333">Om du vill skydda för UTC-tid, väljer du dag före det aktuella datumet.</span><span class="sxs-lookup"><span data-stu-id="21556-333">To safeguard for UTC time, select the day before the current date.</span></span> <span data-ttu-id="21556-334">Det aktuella datumet infaller 6 oktober 2014 väljer du exempelvis 2014-10-5.</span><span class="sxs-lookup"><span data-stu-id="21556-334">For example, if the current date is October 6, 2014, select 10/5/2014.</span></span>

<span data-ttu-id="21556-335">SAS-URL kan skapas på flera olika sätt att dela den virtuella Hårddisken för Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="21556-335">SAS URL can be generated in multiple ways to share your VHD for Azure Marketplace.</span></span>
<span data-ttu-id="21556-336">Följande är 3 rekommenderade verktyg:</span><span class="sxs-lookup"><span data-stu-id="21556-336">Following are the 3 recommended tools:</span></span>

1.  <span data-ttu-id="21556-337">Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="21556-337">Azure Storage Explorer</span></span>
2.  <span data-ttu-id="21556-338">Microsoft Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="21556-338">Microsoft Storage Explorer</span></span>
3.  <span data-ttu-id="21556-339">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="21556-339">Azure CLI</span></span>

<span data-ttu-id="21556-340">**Azure Lagringsutforskaren (rekommenderas för Windows-användare)**</span><span class="sxs-lookup"><span data-stu-id="21556-340">**Azure Storage Explorer (Recommended for Windows Users)**</span></span>

<span data-ttu-id="21556-341">Nedan följer stegen för att generera SAS-URL med hjälp av Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="21556-341">Following are the steps for generating SAS URL by using Azure Storage Explorer</span></span>

1. <span data-ttu-id="21556-342">Hämta [förhandsvisning 3 av Azure Storage Explorer 6](https://azurestorageexplorer.codeplex.com/) från CodePlex.</span><span class="sxs-lookup"><span data-stu-id="21556-342">Download [Azure Storage Explorer 6 Preview 3](https://azurestorageexplorer.codeplex.com/) from CodePlex.</span></span> <span data-ttu-id="21556-343">Gå till [Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) och på **”hämtar”**.</span><span class="sxs-lookup"><span data-stu-id="21556-343">Go to [Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) and click **"Downloads"**.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. <span data-ttu-id="21556-345">Hämta [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) och installera efter packa upp den.</span><span class="sxs-lookup"><span data-stu-id="21556-345">Download [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) and install after unzipping it.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. <span data-ttu-id="21556-347">Öppna programmet när den har installerats.</span><span class="sxs-lookup"><span data-stu-id="21556-347">After it is installed, open the application.</span></span>
4. <span data-ttu-id="21556-348">Klicka på **Lägg till konto**.</span><span class="sxs-lookup"><span data-stu-id="21556-348">Click **Add Account**.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. <span data-ttu-id="21556-350">Ange lagringskontonamn, lagringskontonyckel och lagring slutpunkter domän.</span><span class="sxs-lookup"><span data-stu-id="21556-350">Specify the storage account name, storage account key, and storage endpoints domain.</span></span> <span data-ttu-id="21556-351">Detta är storage-konto i din Azure-prenumeration där du har sparat den virtuella Hårddisken på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="21556-351">This is the storage account in your Azure subscription where you have kept your VHD on Azure portal.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. <span data-ttu-id="21556-353">När Azure Lagringsutforskaren är ansluten till specifika storage-konto, startas med alla de innehåller inom lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="21556-353">Once Azure Storage Explorer is connected to your specific storage account, it will start showing all of the contains within the storage account.</span></span> <span data-ttu-id="21556-354">Markera den behållare som du kopierade operativsystemet disk VHD-filen (även datadiskar om de är tillämpliga för ditt scenario).</span><span class="sxs-lookup"><span data-stu-id="21556-354">Select the container where you copied the operating system disk VHD file (also data disks if they are applicable for your scenario).</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. <span data-ttu-id="21556-356">När du har valt blob-behållare startar Azure Lagringsutforskaren visar filerna i behållaren.</span><span class="sxs-lookup"><span data-stu-id="21556-356">After selecting the blob container, Azure Storage Explorer starts showing the files within the container.</span></span> <span data-ttu-id="21556-357">Välj bildfil (.vhd) som ska skickas.</span><span class="sxs-lookup"><span data-stu-id="21556-357">Select the image file (.vhd) that needs to be submitted.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  <span data-ttu-id="21556-359">När du har valt VHD-filen i behållaren, klicka på den **säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="21556-359">After selecting the .vhd file in the container, click the **Security** tab.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  <span data-ttu-id="21556-361">I den **Blob-behållaren säkerhet** dialogrutan rutan, lämna standardvärdena på den **åtkomstnivå** fliken och klicka sedan på **signaturer för delad åtkomst** fliken.</span><span class="sxs-lookup"><span data-stu-id="21556-361">In the **Blob Container Security** dialog box, leave the defaults on the **Access Level** tab, and then click **Shared Access Signatures** tab.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. <span data-ttu-id="21556-363">Följ stegen nedan för att generera en signatur för delad åtkomst URI för VHD-avbildningen:</span><span class="sxs-lookup"><span data-stu-id="21556-363">Follow the steps below to generate a shared access signature URI for the .vhd image:</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    <span data-ttu-id="21556-365">a.</span><span class="sxs-lookup"><span data-stu-id="21556-365">a.</span></span> <span data-ttu-id="21556-366">**Åtkomst tillåts från:** för att skydda för UTC-tid, väljer du dag före det aktuella datumet.</span><span class="sxs-lookup"><span data-stu-id="21556-366">**Access permitted from:** To safeguard for UTC time, select the day before the current date.</span></span> <span data-ttu-id="21556-367">Det aktuella datumet infaller 6 oktober 2014 väljer du exempelvis 2014-10-5.</span><span class="sxs-lookup"><span data-stu-id="21556-367">For example, if the current date is October 6, 2014, select 10/5/2014.</span></span>

    <span data-ttu-id="21556-368">b.</span><span class="sxs-lookup"><span data-stu-id="21556-368">b.</span></span> <span data-ttu-id="21556-369">**Åtkomst till tillåten:** Välj ett datum som är minst tre veckor efter den **åtkomst tillåts från** datum.</span><span class="sxs-lookup"><span data-stu-id="21556-369">**Access permitted to:** Select a date that is at least 3 weeks after the **Access permitted from** date.</span></span>

    <span data-ttu-id="21556-370">c.</span><span class="sxs-lookup"><span data-stu-id="21556-370">c.</span></span> <span data-ttu-id="21556-371">**Åtgärder som tillåts:** markerar du den **lista** och **Läs** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="21556-371">**Actions permitted:** Select the **List** and **Read** permissions.</span></span>

    <span data-ttu-id="21556-372">d.</span><span class="sxs-lookup"><span data-stu-id="21556-372">d.</span></span> <span data-ttu-id="21556-373">Om du har valt VHD-filen på rätt sätt och sedan filen visas i **blobbnamnet till** med filnamnstillägget .vhd.</span><span class="sxs-lookup"><span data-stu-id="21556-373">If you have selected your .vhd file correctly, then your file appears in **Blob name to access** with extension .vhd.</span></span>

    <span data-ttu-id="21556-374">e.</span><span class="sxs-lookup"><span data-stu-id="21556-374">e.</span></span> <span data-ttu-id="21556-375">Klicka på **generera signatur**.</span><span class="sxs-lookup"><span data-stu-id="21556-375">Click **Generate Signature**.</span></span>

    <span data-ttu-id="21556-376">f.</span><span class="sxs-lookup"><span data-stu-id="21556-376">f.</span></span> <span data-ttu-id="21556-377">I **genereras delad åtkomst signatur URI: N för den här behållaren**, Sök efter följande markerade ovan:</span><span class="sxs-lookup"><span data-stu-id="21556-377">In **Generated Shared Access Signature URI of this container**, check for the following as highlighted above:</span></span>

       - <span data-ttu-id="21556-378">Se till att bilden filnamn och **”VHD”** i URI: N.</span><span class="sxs-lookup"><span data-stu-id="21556-378">Make sure that your image file name and **".vhd"** are in the URI.</span></span>
       - <span data-ttu-id="21556-379">I slutet av signaturen, se till att **”= rl”** visas.</span><span class="sxs-lookup"><span data-stu-id="21556-379">At the end of the signature, make sure that **"=rl"** appears.</span></span> <span data-ttu-id="21556-380">Detta demonstrerar att läs- och åtkomst har angetts korrekt.</span><span class="sxs-lookup"><span data-stu-id="21556-380">This demonstrates that Read and List access was provided successfully.</span></span>
       - <span data-ttu-id="21556-381">I mitten av signaturen, se till att **”sr = c”** visas.</span><span class="sxs-lookup"><span data-stu-id="21556-381">In middle of the signature, make sure that **"sr=c"** appears.</span></span> <span data-ttu-id="21556-382">Detta demonstrerar att du har åtkomst behållare</span><span class="sxs-lookup"><span data-stu-id="21556-382">This demonstrates that you have container level access</span></span>

11. <span data-ttu-id="21556-383">För att säkerställa att den genererade delad åtkomst signatur URI fungerar, klickar du på **Test i webbläsaren**.</span><span class="sxs-lookup"><span data-stu-id="21556-383">To ensure that the generated shared access signature URI works, click **Test in Browser**.</span></span> <span data-ttu-id="21556-384">Den bör startar hämtningen.</span><span class="sxs-lookup"><span data-stu-id="21556-384">It should start the download process.</span></span>

12. <span data-ttu-id="21556-385">Kopiera signatur för delad åtkomst URI.</span><span class="sxs-lookup"><span data-stu-id="21556-385">Copy the shared access signature URI.</span></span> <span data-ttu-id="21556-386">Detta är URI:n som ska klistras in i publiceringsportalen.</span><span class="sxs-lookup"><span data-stu-id="21556-386">This is the URI to paste into the Publishing Portal.</span></span>

13. <span data-ttu-id="21556-387">Upprepa steg 6 – 10 för varje virtuell Hårddisk i SKU: N.</span><span class="sxs-lookup"><span data-stu-id="21556-387">Repeat steps 6-10 for each VHD in the SKU.</span></span>

<span data-ttu-id="21556-388">**Microsoft Azure Lagringsutforskaren (Windows-/ MAC/Linux)**</span><span class="sxs-lookup"><span data-stu-id="21556-388">**Microsoft Azure Storage Explorer (Windows/MAC/Linux)**</span></span>

<span data-ttu-id="21556-389">Nedan följer stegen för att generera SAS-URL genom att använda Microsoft Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="21556-389">Following are the steps for generating SAS URL by using Microsoft Azure Storage Explorer</span></span>

1.  <span data-ttu-id="21556-390">Ladda ned Microsoft Azure Lagringsutforskaren formuläret [http://storageexplorer.com/](http://storageexplorer.com/) webbplats.</span><span class="sxs-lookup"><span data-stu-id="21556-390">Download Microsoft Azure Storage Explorer form [http://storageexplorer.com/](http://storageexplorer.com/) website.</span></span> <span data-ttu-id="21556-391">Gå till [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/releasenotes.html) och på **”hämta för Windows”**.</span><span class="sxs-lookup"><span data-stu-id="21556-391">Go to [Microsoft Azure Storage Explorer](http://storageexplorer.com/releasenotes.html) and click **“Download for Windows”**.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  <span data-ttu-id="21556-393">Öppna programmet när den har installerats.</span><span class="sxs-lookup"><span data-stu-id="21556-393">After it is installed, open the application.</span></span>

3.  <span data-ttu-id="21556-394">Klicka på **Lägg till konto**.</span><span class="sxs-lookup"><span data-stu-id="21556-394">Click **Add Account**.</span></span>

4.  <span data-ttu-id="21556-395">Konfigurera Microsoft Azure Lagringsutforskaren till din prenumeration genom att logga in på ditt konto</span><span class="sxs-lookup"><span data-stu-id="21556-395">Configure Microsoft Azure Storage Explorer to your subscription by sign in to your account</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  <span data-ttu-id="21556-397">Gå till lagringskontot och välj behållaren</span><span class="sxs-lookup"><span data-stu-id="21556-397">Go to storage account and select the Container</span></span>

6.  <span data-ttu-id="21556-398">Välj **”hämta resursen Åtkomstsignatur...”**</span><span class="sxs-lookup"><span data-stu-id="21556-398">Select **“Get Share Access Signature..”**</span></span> <span data-ttu-id="21556-399">med hjälp av högerklickar du på den **behållare**</span><span class="sxs-lookup"><span data-stu-id="21556-399">by using Right Click of the **container**</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  <span data-ttu-id="21556-401">Starttiden för uppdateringen, förfallotiden och behörigheter enligt följande</span><span class="sxs-lookup"><span data-stu-id="21556-401">Update Start time, Expiry time and Permissions as per following</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    <span data-ttu-id="21556-403">a.</span><span class="sxs-lookup"><span data-stu-id="21556-403">a.</span></span>  <span data-ttu-id="21556-404">**Starttid:** för att skydda för UTC-tid, väljer du dag före det aktuella datumet.</span><span class="sxs-lookup"><span data-stu-id="21556-404">**Start Time:** To safeguard for UTC time, select the day before the current date.</span></span> <span data-ttu-id="21556-405">Det aktuella datumet infaller 6 oktober 2014 väljer du exempelvis 2014-10-5.</span><span class="sxs-lookup"><span data-stu-id="21556-405">For example, if the current date is October 6, 2014, select 10/5/2014.</span></span>

    <span data-ttu-id="21556-406">b.</span><span class="sxs-lookup"><span data-stu-id="21556-406">b.</span></span>  <span data-ttu-id="21556-407">**Förfallotiden:** Välj ett datum som är minst tre veckor efter den **starttid** datum.</span><span class="sxs-lookup"><span data-stu-id="21556-407">**Expiry Time:** Select a date that is at least 3 weeks after the **Start Time** date.</span></span>

    <span data-ttu-id="21556-408">c.</span><span class="sxs-lookup"><span data-stu-id="21556-408">c.</span></span>  <span data-ttu-id="21556-409">**Behörigheter:** markerar du den **lista** och **Läs** behörigheter</span><span class="sxs-lookup"><span data-stu-id="21556-409">**Permissions:** Select the **List** and **Read** permissions</span></span>

8.  <span data-ttu-id="21556-410">Kopiera URI för signatur för delad åtkomst av behållare</span><span class="sxs-lookup"><span data-stu-id="21556-410">Copy Container shared access signature URI</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    <span data-ttu-id="21556-412">Genererade SAS-URL är för behållare nivå och nu behöver lägga till VHD-namn i den.</span><span class="sxs-lookup"><span data-stu-id="21556-412">Generated SAS URL is for container Level and now we need to add VHD name in it.</span></span>

    <span data-ttu-id="21556-413">Format för behållare nivån SAS-URL:`https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span><span class="sxs-lookup"><span data-stu-id="21556-413">Format of Container Level SAS URL: `https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span></span>

    <span data-ttu-id="21556-414">Infoga VHD namn efter behållarens namn i SAS-URL som nedan`https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span><span class="sxs-lookup"><span data-stu-id="21556-414">Insert VHD name after the container name in SAS URL as below `https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span></span>

    <span data-ttu-id="21556-415">Exempel:</span><span class="sxs-lookup"><span data-stu-id="21556-415">Example:</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    <span data-ttu-id="21556-417">TestRGVM201631920152.vhd är den virtuella Hårddiskens namn sedan VHD SAS-URL kommer att`https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span><span class="sxs-lookup"><span data-stu-id="21556-417">TestRGVM201631920152.vhd is the VHD Name then VHD SAS URL will be `https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`</span></span>

    - <span data-ttu-id="21556-418">Se till att bilden filnamn och **”VHD”** i URI: N.</span><span class="sxs-lookup"><span data-stu-id="21556-418">Make sure that your image file name and **".vhd"** are in the URI.</span></span>
    - <span data-ttu-id="21556-419">I mitten av signaturen, se till att **”sp = rl”** visas.</span><span class="sxs-lookup"><span data-stu-id="21556-419">In middle of the signature, make sure that **"sp=rl"** appears.</span></span> <span data-ttu-id="21556-420">Detta demonstrerar att läs- och åtkomst har angetts korrekt.</span><span class="sxs-lookup"><span data-stu-id="21556-420">This demonstrates that Read and List access was provided successfully.</span></span>
    - <span data-ttu-id="21556-421">I mitten av signaturen, se till att **”sr = c”** visas.</span><span class="sxs-lookup"><span data-stu-id="21556-421">In middle of the signature, make sure that **"sr=c"** appears.</span></span> <span data-ttu-id="21556-422">Detta demonstrerar att du har åtkomst behållare</span><span class="sxs-lookup"><span data-stu-id="21556-422">This demonstrates that you have container level access</span></span>

9.  <span data-ttu-id="21556-423">För att säkerställa att den genererade delad åtkomst signatur URI fungerar, testa den i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="21556-423">To ensure that the generated shared access signature URI works, test it in browser.</span></span> <span data-ttu-id="21556-424">Den bör startar hämtningen</span><span class="sxs-lookup"><span data-stu-id="21556-424">It should start the download process</span></span>

10. <span data-ttu-id="21556-425">Kopiera signatur för delad åtkomst URI.</span><span class="sxs-lookup"><span data-stu-id="21556-425">Copy the shared access signature URI.</span></span> <span data-ttu-id="21556-426">Detta är URI:n som ska klistras in i publiceringsportalen.</span><span class="sxs-lookup"><span data-stu-id="21556-426">This is the URI to paste into the Publishing Portal.</span></span>

11. <span data-ttu-id="21556-427">Upprepa stegen för varje VHD i SKU:n.</span><span class="sxs-lookup"><span data-stu-id="21556-427">Repeat these steps for each VHD in the SKU.</span></span>

<span data-ttu-id="21556-428">**Azure CLI (rekommenderas för icke-Windows & kontinuerlig Integration)**</span><span class="sxs-lookup"><span data-stu-id="21556-428">**Azure CLI (Recommended for Non-Windows & Continuous Integration)**</span></span>

<span data-ttu-id="21556-429">Nedan följer stegen för att generera SAS-URL med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="21556-429">Following are the steps for generating SAS URL by using Azure CLI</span></span>

1.  <span data-ttu-id="21556-430">Ladda ned Microsoft Azure CLI från [här](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/).</span><span class="sxs-lookup"><span data-stu-id="21556-430">Download Microsoft Azure CLI from [here](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/).</span></span> <span data-ttu-id="21556-431">Du kan också hitta olika länkar för  **[Windows](http://aka.ms/webpi-azure-cli)**  och  **[MAC OS x](http://aka.ms/mac-azure-cli)**.</span><span class="sxs-lookup"><span data-stu-id="21556-431">You can also find different links for **[Windows](http://aka.ms/webpi-azure-cli)** and **[MAC OS](http://aka.ms/mac-azure-cli)**.</span></span>

2.  <span data-ttu-id="21556-432">När du har laddat ned och installera</span><span class="sxs-lookup"><span data-stu-id="21556-432">Once it is downloaded, please install</span></span>

3.  <span data-ttu-id="21556-433">Skapa en PowerShell-fil med följande kod och spara den i lokalt</span><span class="sxs-lookup"><span data-stu-id="21556-433">Create a PowerShell file with following code and save it in local</span></span>

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    <span data-ttu-id="21556-434">Uppdatera följande parametrar i ovan</span><span class="sxs-lookup"><span data-stu-id="21556-434">Update the following parameters in above</span></span>

    <span data-ttu-id="21556-435">a.</span><span class="sxs-lookup"><span data-stu-id="21556-435">a.</span></span> <span data-ttu-id="21556-436">**`<StorageAccountName>`**: Ange namnet på ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="21556-436">**`<StorageAccountName>`**: Give your storage account name</span></span>

    <span data-ttu-id="21556-437">b.</span><span class="sxs-lookup"><span data-stu-id="21556-437">b.</span></span> <span data-ttu-id="21556-438">**`<Storage Account Key>`**: Ger din lagringskontonyckel</span><span class="sxs-lookup"><span data-stu-id="21556-438">**`<Storage Account Key>`**: Give your storage account key</span></span>

    <span data-ttu-id="21556-439">c.</span><span class="sxs-lookup"><span data-stu-id="21556-439">c.</span></span> <span data-ttu-id="21556-440">**`<Permission Start Date>`**: Om du vill skydda för UTC-tid, väljer du dag före det aktuella datumet.</span><span class="sxs-lookup"><span data-stu-id="21556-440">**`<Permission Start Date>`**: To safeguard for UTC time, select the day before the current date.</span></span> <span data-ttu-id="21556-441">Till exempel om det aktuella datumet infaller 26 oktober 2016 sedan värdet bör vara 10/25/2016</span><span class="sxs-lookup"><span data-stu-id="21556-441">For example, if the current date is October 26, 2016, then value should be 10/25/2016</span></span>

    <span data-ttu-id="21556-442">d.</span><span class="sxs-lookup"><span data-stu-id="21556-442">d.</span></span> <span data-ttu-id="21556-443">**`<Permission End Date>`**: Välj ett datum som är minst tre veckor efter den **startdatum**.</span><span class="sxs-lookup"><span data-stu-id="21556-443">**`<Permission End Date>`**: Select a date that is at least 3 weeks after the **Start Date**.</span></span> <span data-ttu-id="21556-444">Värdet ska vara **2016-02-11**.</span><span class="sxs-lookup"><span data-stu-id="21556-444">Then value should be **11/02/2016**.</span></span>

    <span data-ttu-id="21556-445">Följande är exempelkoden när du har uppdaterat rätt parametrar</span><span class="sxs-lookup"><span data-stu-id="21556-445">Following is the example code after updating proper parameters</span></span>

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=TIQE5QWMKHpT5q2VnF1bb+NUV7NVMY2xmzVx1rdgIVsw7h0pcI5nMM6+DVFO65i4bQevx21dmrflA91r0Vh2Yw=="
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  <span data-ttu-id="21556-446">Öppna Redigeraren för Powershell med ”Kör som administratör”-läge och öppna filen i steg #3.</span><span class="sxs-lookup"><span data-stu-id="21556-446">Open Powershell editor with “Run as Administrator” mode and open file in step #3.</span></span>

5.  <span data-ttu-id="21556-447">Kör skript och det ger dig SAS-URL för behållaren åtkomst</span><span class="sxs-lookup"><span data-stu-id="21556-447">Run the script and it will provide you the SAS URL for container level access</span></span>

    <span data-ttu-id="21556-448">Följande ska vara resultatet av den SAS-signaturen och kopiera den markerade del i en anteckningar</span><span class="sxs-lookup"><span data-stu-id="21556-448">Following will be the output of the SAS Signature and copy the highlighted part in a notepad</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  <span data-ttu-id="21556-450">Nu får du behållaren nivå SAS-URL och du behöver lägga till VHD-namn i den.</span><span class="sxs-lookup"><span data-stu-id="21556-450">Now you will get container level SAS URL and you need to add VHD name in it.</span></span>

    <span data-ttu-id="21556-451">Behållaren nivån SAS-URL #</span><span class="sxs-lookup"><span data-stu-id="21556-451">Container level SAS URL #</span></span>

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  <span data-ttu-id="21556-452">Infoga VHD namn efter behållarens namn i SAS-URL som visas nedan`https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`</span><span class="sxs-lookup"><span data-stu-id="21556-452">Insert VHD name after the container name in SAS URL as shown below `https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`</span></span>

    <span data-ttu-id="21556-453">Exempel:</span><span class="sxs-lookup"><span data-stu-id="21556-453">Example:</span></span>

    <span data-ttu-id="21556-454">TestRGVM201631920152.vhd är den virtuella Hårddiskens namn sedan VHD SAS-URL kommer att</span><span class="sxs-lookup"><span data-stu-id="21556-454">TestRGVM201631920152.vhd is the VHD Name then VHD SAS URL will be</span></span>

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - <span data-ttu-id="21556-455">Kontrollera att dina bildfilens namn och ”VHD” i URI: N.</span><span class="sxs-lookup"><span data-stu-id="21556-455">Make sure that your image file name and ".vhd" are in the URI.</span></span>
    -   <span data-ttu-id="21556-456">I mitten av signaturen, se till att ”sp = rl” visas.</span><span class="sxs-lookup"><span data-stu-id="21556-456">In middle of the signature, make sure that "sp=rl" appears.</span></span> <span data-ttu-id="21556-457">Detta demonstrerar att läs- och åtkomst har angetts korrekt.</span><span class="sxs-lookup"><span data-stu-id="21556-457">This demonstrates that Read and List access was provided successfully.</span></span>
    -   <span data-ttu-id="21556-458">I mitten av signaturen, se till att ”sr = c” visas.</span><span class="sxs-lookup"><span data-stu-id="21556-458">In middle of the signature, make sure that "sr=c" appears.</span></span> <span data-ttu-id="21556-459">Detta demonstrerar att du har åtkomst behållare</span><span class="sxs-lookup"><span data-stu-id="21556-459">This demonstrates that you have container level access</span></span>

8.  <span data-ttu-id="21556-460">För att säkerställa att den genererade delad åtkomst signatur URI fungerar, testa den i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="21556-460">To ensure that the generated shared access signature URI works, test it in browser.</span></span> <span data-ttu-id="21556-461">Den bör startar hämtningen</span><span class="sxs-lookup"><span data-stu-id="21556-461">It should start the download process</span></span>

9.  <span data-ttu-id="21556-462">Kopiera signatur för delad åtkomst URI.</span><span class="sxs-lookup"><span data-stu-id="21556-462">Copy the shared access signature URI.</span></span> <span data-ttu-id="21556-463">Detta är URI:n som ska klistras in i publiceringsportalen.</span><span class="sxs-lookup"><span data-stu-id="21556-463">This is the URI to paste into the Publishing Portal.</span></span>

10. <span data-ttu-id="21556-464">Upprepa stegen för varje VHD i SKU:n.</span><span class="sxs-lookup"><span data-stu-id="21556-464">Repeat these steps for each VHD in the SKU.</span></span>


### <a name="53-provide-information-about-the-vm-image-and-request-certification-in-the-publishing-portal"></a><span data-ttu-id="21556-465">5.3 innehåller information om VM-avbildning och begär certifiering i Publishing Portal</span><span class="sxs-lookup"><span data-stu-id="21556-465">5.3 Provide information about the VM image and request certification in the Publishing Portal</span></span>
<span data-ttu-id="21556-466">När du har skapat ditt erbjudande och SKU, ska du ange avbildningsinformation som är associerade med den SKU:</span><span class="sxs-lookup"><span data-stu-id="21556-466">After you have created your offer and SKU, you should enter the image details associated with that SKU:</span></span>

1. <span data-ttu-id="21556-467">Gå till den [Publiceringsportal][link-pubportal], och sedan logga in med ditt säljare.</span><span class="sxs-lookup"><span data-stu-id="21556-467">Go to the [Publishing Portal][link-pubportal], and then sign in with your seller account.</span></span>
2. <span data-ttu-id="21556-468">Välj den **VM-avbildningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="21556-468">Select the **VM images** tab.</span></span>
3. <span data-ttu-id="21556-469">Identifieraren visas längst upp på sidan är faktiskt erbjuder identifierare och inte SKU-ID.</span><span class="sxs-lookup"><span data-stu-id="21556-469">The identifier listed at the top of the page is actually the offer identifier and not the SKU identifier.</span></span>
4. <span data-ttu-id="21556-470">Ange egenskaper under den **SKU: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="21556-470">Fill out the properties under the **SKUs** section.</span></span>
5. <span data-ttu-id="21556-471">Under **operativsystemsfamilj**, klicka på den typ av operativsystem som är associerade med operativsystemet VHD.</span><span class="sxs-lookup"><span data-stu-id="21556-471">Under **Operating system family**, click the operating system type associated with the operating system VHD.</span></span>
6. <span data-ttu-id="21556-472">I den **operativsystemet** rutan, beskriver operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="21556-472">In the **Operating system** box, describe the operating system.</span></span> <span data-ttu-id="21556-473">Överväg ett format som operativsystemsfamilj, typ, version och uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="21556-473">Consider a format such as operating system family, type, version, and updates.</span></span> <span data-ttu-id="21556-474">Ett exempel är ”Windows Server Datacenter 2014 R2”.</span><span class="sxs-lookup"><span data-stu-id="21556-474">An example is "Windows Server Datacenter 2014 R2."</span></span>
7. <span data-ttu-id="21556-475">Välj upp till sex rekommenderade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="21556-475">Select up to six recommended virtual machine sizes.</span></span> <span data-ttu-id="21556-476">Dessa är rekommendationer som visas till kunden i bladet priser nivå i Azure Portal när de vill köpa och distribuera avbildningen.</span><span class="sxs-lookup"><span data-stu-id="21556-476">These are recommendations that get displayed to the customer in the Pricing tier blade in the Azure Portal when they decide to purchase and deploy your image.</span></span> <span data-ttu-id="21556-477">**Detta är endast rekommendationer. Kunden kan välja VM-storlek som innehåller diskar som anges i bilden.**</span><span class="sxs-lookup"><span data-stu-id="21556-477">**These are only recommendations. The customer is able to select any VM size that accommodates the disks specified in your image.**</span></span>
8. <span data-ttu-id="21556-478">Ange vilken version.</span><span class="sxs-lookup"><span data-stu-id="21556-478">Enter the version.</span></span> <span data-ttu-id="21556-479">Versionsfältet kapslar in en semantiska version för att identifiera produkten med uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="21556-479">The version field encapsulates a semantic version to identify the product and its updates:</span></span>
   * <span data-ttu-id="21556-480">Versioner ska vara i formatet X.Y.Z, där X, Y och Z är heltal.</span><span class="sxs-lookup"><span data-stu-id="21556-480">Versions should be of the form X.Y.Z, where X, Y, and Z are integers.</span></span>
   * <span data-ttu-id="21556-481">Bilder i olika SKU: er kan ha olika versioner av högre och lägre.</span><span class="sxs-lookup"><span data-stu-id="21556-481">Images in different SKUs can have different major and minor versions.</span></span>
   * <span data-ttu-id="21556-482">Versioner i en SKU bara ska inkrementella ändringar som ökar patch-version (Z från X.Y.Z).</span><span class="sxs-lookup"><span data-stu-id="21556-482">Versions within a SKU should only be incremental changes, which increase the patch version (Z from X.Y.Z).</span></span>
9. <span data-ttu-id="21556-483">I den **OS VHD URL** Ange Signatur för delad åtkomst URI som skapats för operativsystemet VHD.</span><span class="sxs-lookup"><span data-stu-id="21556-483">In the **OS VHD URL** box, enter the shared access signature URI created for the operating system VHD.</span></span>
10. <span data-ttu-id="21556-484">Om det finns datadiskar som är associerade med denna SKU, Välj logiskt enhetsnummer (LUN) som du vill att data disken som ska monteras på distribution.</span><span class="sxs-lookup"><span data-stu-id="21556-484">If there are data disks associated with this SKU, select the logical unit number (LUN) to which you would like this data disk to be mounted upon deployment.</span></span>
11. <span data-ttu-id="21556-485">I den **LUN X VHD URL** Ange Signatur för delad åtkomst URI som skapats för den första data VHD.</span><span class="sxs-lookup"><span data-stu-id="21556-485">In the **LUN X VHD URL** box, enter the shared access signature URI created for the first data VHD.</span></span>

    ![Rita](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a><span data-ttu-id="21556-487">Vanliga problem med SAS-URL & korrigeringar</span><span class="sxs-lookup"><span data-stu-id="21556-487">Common SAS URL issues & fixes</span></span>

|<span data-ttu-id="21556-488">Problem</span><span class="sxs-lookup"><span data-stu-id="21556-488">Issue</span></span>|<span data-ttu-id="21556-489">Felmeddelande</span><span class="sxs-lookup"><span data-stu-id="21556-489">Failure Message</span></span>|<span data-ttu-id="21556-490">Åtgärda</span><span class="sxs-lookup"><span data-stu-id="21556-490">Fix</span></span>|<span data-ttu-id="21556-491">Länk till dokumentation</span><span class="sxs-lookup"><span data-stu-id="21556-491">Documentation Link</span></span>|
|---|---|---|---|
|<span data-ttu-id="21556-492">Det gick inte att kopiera bilder - ””? hittades inte i SAS-url</span><span class="sxs-lookup"><span data-stu-id="21556-492">Failure in copying images - "?" is not found in SAS url</span></span>|<span data-ttu-id="21556-493">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="21556-493">Failure: Copying Images.</span></span> <span data-ttu-id="21556-494">Det går inte att hämta blob att använda som SAS-Uri.</span><span class="sxs-lookup"><span data-stu-id="21556-494">Not able to download blob using provided SAS Uri.</span></span>|<span data-ttu-id="21556-495">Uppdatera SAS-Url med rekommenderade verktyg</span><span class="sxs-lookup"><span data-stu-id="21556-495">Update the SAS Url using recommended tools</span></span>|[<span data-ttu-id="21556-496">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="21556-496">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="21556-497">Det gick inte att kopiera bilder - ”a” och ”se” parametrar inte i SAS-url</span><span class="sxs-lookup"><span data-stu-id="21556-497">Failure in copying images - “st” and “se” parameters not in SAS url</span></span>|<span data-ttu-id="21556-498">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="21556-498">Failure: Copying Images.</span></span> <span data-ttu-id="21556-499">Det går inte att hämta blob att använda som SAS-Uri.</span><span class="sxs-lookup"><span data-stu-id="21556-499">Not able to download blob using provided SAS Uri.</span></span>|<span data-ttu-id="21556-500">Uppdatera SAS-Url med Start- och slutdatum på den</span><span class="sxs-lookup"><span data-stu-id="21556-500">Update the SAS Url with Start and End dates on it</span></span>|[<span data-ttu-id="21556-501">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="21556-501">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="21556-502">Det gick inte att kopiera avbildningar - ”sp = rl” inte i SAS-url</span><span class="sxs-lookup"><span data-stu-id="21556-502">Failure in copying images - “sp=rl” not in SAS url</span></span>|<span data-ttu-id="21556-503">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="21556-503">Failure: Copying Images.</span></span> <span data-ttu-id="21556-504">Det går inte att hämta blob att använda som SAS-Uri</span><span class="sxs-lookup"><span data-stu-id="21556-504">Not able to download blob using provided SAS Uri</span></span>|<span data-ttu-id="21556-505">Uppdatera SAS-Url med behörigheter som har angetts som ”läsa” och ”lista</span><span class="sxs-lookup"><span data-stu-id="21556-505">Update the SAS Url with permissions set as “Read” & “List</span></span>|[<span data-ttu-id="21556-506">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="21556-506">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="21556-507">Det gick inte att kopiera avbildningar - SAS-url har blanksteg i vhd-namn</span><span class="sxs-lookup"><span data-stu-id="21556-507">Failure in copying images - SAS url have white spaces in vhd name</span></span>|<span data-ttu-id="21556-508">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="21556-508">Failure: Copying Images.</span></span> <span data-ttu-id="21556-509">Det går inte att hämta blob att använda som SAS-Uri.</span><span class="sxs-lookup"><span data-stu-id="21556-509">Not able to download blob using provided SAS Uri.</span></span>|<span data-ttu-id="21556-510">Uppdatera SAS-Url utan blanksteg</span><span class="sxs-lookup"><span data-stu-id="21556-510">Update the SAS Url without white spaces</span></span>|[<span data-ttu-id="21556-511">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="21556-511">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|<span data-ttu-id="21556-512">Det gick inte att kopiera avbildningar – Url-auktorisering för SAS-fel</span><span class="sxs-lookup"><span data-stu-id="21556-512">Failure in copying images – SAS Url Authorization error</span></span>|<span data-ttu-id="21556-513">Fel: Kopiera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="21556-513">Failure: Copying Images.</span></span> <span data-ttu-id="21556-514">Det går inte att hämta blob på grund av Behörighetsfel</span><span class="sxs-lookup"><span data-stu-id="21556-514">Not able to download blob due to authorization error</span></span>|<span data-ttu-id="21556-515">Återskapa SAS-Url</span><span class="sxs-lookup"><span data-stu-id="21556-515">Regenerate the SAS Url</span></span>|[<span data-ttu-id="21556-516">https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/</span><span class="sxs-lookup"><span data-stu-id="21556-516">https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/</span></span>](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|


## <a name="next-step"></a><span data-ttu-id="21556-517">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="21556-517">Next step</span></span>
<span data-ttu-id="21556-518">När du är klar med SKU-uppgifter du kan gå till den [administratörshandboken för Azure Marketplace marketing content][link-pushstaging].</span><span class="sxs-lookup"><span data-stu-id="21556-518">After you are done with the SKU details, you can move forward to the [Azure Marketplace marketing content guide][link-pushstaging].</span></span> <span data-ttu-id="21556-519">I steget publiceringsprocessen och ger du marknadsföring innehållet priser och annan information som krävs före **steg3: testa den virtuella datorn erbjuder i Förproduktion**, där du kan testa olika användningsfall scenarier innan du distribuerar erbjudanden på Azure Marketplace för offentliga synlighet och inköp.</span><span class="sxs-lookup"><span data-stu-id="21556-519">In that step of the publishing process, you provide the marketing content, pricing, and other information necessary prior to **Step 3: Testing your VM offer in staging**, where you test various use-case scenarios before deploying the offer to the Azure Marketplace for public visibility and purchase.</span></span>  

## <a name="see-also"></a><span data-ttu-id="21556-520">Se även</span><span class="sxs-lookup"><span data-stu-id="21556-520">See also</span></span>
* [<span data-ttu-id="21556-521">Komma igång: hur du publicerar ett erbjudande på Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="21556-521">Getting started: How to publish an offer to the Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

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
