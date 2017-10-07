---
title: aaaSelect Windows VM bilder i Azure | Microsoft Docs
description: "Lär dig hur toouse Azure PowerSHell toodetermine hello utgivare, erbjudande, SKU och version för Marketplace VM-avbildningar."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 752edcd0935f5141832e49503ae800ea0145e219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-windows-vm-images-in-hello-azure-marketplace-with-azure-powershell"></a><span data-ttu-id="e42d5-103">Hur toofind Windows VM bilder i hello Azure Marketplace med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e42d5-103">How toofind Windows VM images in hello Azure Marketplace with Azure PowerShell</span></span>

<span data-ttu-id="e42d5-104">Det här avsnittet beskrivs hur toouse Azure PowerShell toofind VM bilder i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="e42d5-104">This topic describes how toouse Azure PowerShell toofind VM images in hello Azure Marketplace.</span></span> <span data-ttu-id="e42d5-105">Använd den här informationen toospecify Marketplace-avbildning när du skapar en virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="e42d5-105">Use this information toospecify a Marketplace image when you create a Windows VM.</span></span>

<span data-ttu-id="e42d5-106">Kontrollera att du har installerat och konfigurerat hello senaste [Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="e42d5-106">Make sure that you installed and configured hello latest [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>



## <a name="table-of-commonly-used-windows-images"></a><span data-ttu-id="e42d5-107">Tabell med vanliga Windows-avbildningar</span><span class="sxs-lookup"><span data-stu-id="e42d5-107">Table of commonly used Windows images</span></span>
| <span data-ttu-id="e42d5-108">PublisherName</span><span class="sxs-lookup"><span data-stu-id="e42d5-108">PublisherName</span></span> | <span data-ttu-id="e42d5-109">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="e42d5-109">Offer</span></span> | <span data-ttu-id="e42d5-110">Sku</span><span class="sxs-lookup"><span data-stu-id="e42d5-110">Sku</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e42d5-111">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-111">MicrosoftWindowsServer</span></span> |<span data-ttu-id="e42d5-112">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-112">WindowsServer</span></span> |<span data-ttu-id="e42d5-113">2016 Datacenter</span><span class="sxs-lookup"><span data-stu-id="e42d5-113">2016-Datacenter</span></span> |
| <span data-ttu-id="e42d5-114">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-114">MicrosoftWindowsServer</span></span> |<span data-ttu-id="e42d5-115">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-115">WindowsServer</span></span> |<span data-ttu-id="e42d5-116">2016-Datacenter-Server-Core</span><span class="sxs-lookup"><span data-stu-id="e42d5-116">2016-Datacenter-Server-Core</span></span> |
| <span data-ttu-id="e42d5-117">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-117">MicrosoftWindowsServer</span></span> |<span data-ttu-id="e42d5-118">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-118">WindowsServer</span></span> |<span data-ttu-id="e42d5-119">2016 Datacenter med behållare</span><span class="sxs-lookup"><span data-stu-id="e42d5-119">2016-Datacenter-with-Containers</span></span> |
| <span data-ttu-id="e42d5-120">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-120">MicrosoftWindowsServer</span></span> |<span data-ttu-id="e42d5-121">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-121">WindowsServer</span></span> |<span data-ttu-id="e42d5-122">2016 Nano Server</span><span class="sxs-lookup"><span data-stu-id="e42d5-122">2016-Nano-Server</span></span> |
| <span data-ttu-id="e42d5-123">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-123">MicrosoftWindowsServer</span></span> |<span data-ttu-id="e42d5-124">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-124">WindowsServer</span></span> |<span data-ttu-id="e42d5-125">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="e42d5-125">2012-R2-Datacenter</span></span> |
| <span data-ttu-id="e42d5-126">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-126">MicrosoftWindowsServer</span></span> |<span data-ttu-id="e42d5-127">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-127">WindowsServer</span></span> |<span data-ttu-id="e42d5-128">2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="e42d5-128">2008-R2-SP1</span></span> |
| <span data-ttu-id="e42d5-129">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="e42d5-129">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="e42d5-130">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="e42d5-130">DynamicsNAV</span></span> |<span data-ttu-id="e42d5-131">2017</span><span class="sxs-lookup"><span data-stu-id="e42d5-131">2017</span></span> |
| <span data-ttu-id="e42d5-132">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="e42d5-132">MicrosoftSharePoint</span></span> |<span data-ttu-id="e42d5-133">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-133">MicrosoftSharePointServer</span></span> |<span data-ttu-id="e42d5-134">2016</span><span class="sxs-lookup"><span data-stu-id="e42d5-134">2016</span></span> |
| <span data-ttu-id="e42d5-135">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-135">MicrosoftSQLServer</span></span> |<span data-ttu-id="e42d5-136">SQL2016 WS2016</span><span class="sxs-lookup"><span data-stu-id="e42d5-136">SQL2016-WS2016</span></span> |<span data-ttu-id="e42d5-137">Enterprise</span><span class="sxs-lookup"><span data-stu-id="e42d5-137">Enterprise</span></span> |
| <span data-ttu-id="e42d5-138">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="e42d5-138">MicrosoftSQLServer</span></span> |<span data-ttu-id="e42d5-139">SQL2014SP2 WS2012R2</span><span class="sxs-lookup"><span data-stu-id="e42d5-139">SQL2014SP2-WS2012R2</span></span> |<span data-ttu-id="e42d5-140">Enterprise</span><span class="sxs-lookup"><span data-stu-id="e42d5-140">Enterprise</span></span> |
| <span data-ttu-id="e42d5-141">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="e42d5-141">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="e42d5-142">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="e42d5-142">WindowsServerHPCPack</span></span> |<span data-ttu-id="e42d5-143">2012R2</span><span class="sxs-lookup"><span data-stu-id="e42d5-143">2012R2</span></span> |
| <span data-ttu-id="e42d5-144">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="e42d5-144">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="e42d5-145">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="e42d5-145">WindowsServerEssentials</span></span> |<span data-ttu-id="e42d5-146">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="e42d5-146">WindowsServerEssentials</span></span> |

## <a name="find-specific-images"></a><span data-ttu-id="e42d5-147">Söka efter specifika avbildningar</span><span class="sxs-lookup"><span data-stu-id="e42d5-147">Find specific images</span></span>


<span data-ttu-id="e42d5-148">När du skapar en ny virtuell dator med Azure Resource Manager kan i vissa fall behöver du toospecify en bild med hello kombination av hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="e42d5-148">When creating a new virtual machine with Azure Resource Manager, in some cases you need toospecify an image with hello combination of hello following image properties:</span></span>

* <span data-ttu-id="e42d5-149">Utgivare</span><span class="sxs-lookup"><span data-stu-id="e42d5-149">Publisher</span></span>
* <span data-ttu-id="e42d5-150">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="e42d5-150">Offer</span></span>
* <span data-ttu-id="e42d5-151">SKU</span><span class="sxs-lookup"><span data-stu-id="e42d5-151">SKU</span></span>

<span data-ttu-id="e42d5-152">Till exempel använda dessa värden med hello [Set AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell-cmdlet eller med en mall för gruppen av resurs som du måste ange hello typ av VM-toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="e42d5-152">For example, use these values with hello [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell cmdlet, or with a resource group template in which you must specify hello type of VM toobe created.</span></span>

<span data-ttu-id="e42d5-153">Om du behöver toodetermine dessa värden, kan du köra hello [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), och [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets toonavigate hello bilder.</span><span class="sxs-lookup"><span data-stu-id="e42d5-153">If you need toodetermine these values, you can run hello [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), and [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets toonavigate hello images.</span></span> <span data-ttu-id="e42d5-154">Du kan bestämma dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e42d5-154">You determine these values:</span></span>

1. <span data-ttu-id="e42d5-155">Lista hello avbildningen utgivare.</span><span class="sxs-lookup"><span data-stu-id="e42d5-155">List hello image publishers.</span></span>
2. <span data-ttu-id="e42d5-156">Visa en lista över erbjudanden från en viss utgivare.</span><span class="sxs-lookup"><span data-stu-id="e42d5-156">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="e42d5-157">Visa en lista över SKU:er för ett visst erbjudande.</span><span class="sxs-lookup"><span data-stu-id="e42d5-157">For a given offer, list their SKUs.</span></span>

<span data-ttu-id="e42d5-158">Först lista hello utgivare med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="e42d5-158">First, list hello publishers with hello following commands:</span></span>

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

<span data-ttu-id="e42d5-159">Fyll i din valda utgivarens namn och kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="e42d5-159">Fill in your chosen publisher name and run hello following commands:</span></span>

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="e42d5-160">Fyll i namnet på din valda erbjudandet och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="e42d5-160">Fill in your chosen offer name and run hello following commands:</span></span>

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="e42d5-161">Från hello utdata från hello `Get-AzureRMVMImageSku` kommando du har alla hello information du behöver toospecify hello avbildning för en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e42d5-161">From hello output of hello `Get-AzureRMVMImageSku` command, you have all hello information you need toospecify hello image for a new virtual machine.</span></span>

<span data-ttu-id="e42d5-162">hello följande visar ett fullständigt exempel:</span><span class="sxs-lookup"><span data-stu-id="e42d5-162">hello following shows a full example:</span></span>

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

<span data-ttu-id="e42d5-163">Resultat:</span><span class="sxs-lookup"><span data-stu-id="e42d5-163">Output:</span></span>

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

<span data-ttu-id="e42d5-164">För hello ”Microsoft Windows Server” publisher:</span><span class="sxs-lookup"><span data-stu-id="e42d5-164">For hello "MicrosoftWindowsServer" publisher:</span></span>

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="e42d5-165">Resultat:</span><span class="sxs-lookup"><span data-stu-id="e42d5-165">Output:</span></span>

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

<span data-ttu-id="e42d5-166">För hello ”Windows Server”-erbjudande:</span><span class="sxs-lookup"><span data-stu-id="e42d5-166">For hello "WindowsServer" offer:</span></span>

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="e42d5-167">Resultat:</span><span class="sxs-lookup"><span data-stu-id="e42d5-167">Output:</span></span>

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

<span data-ttu-id="e42d5-168">Kopiera hello valt SKU namn från den här listan, och du har alla hello-information för hello `Set-AzureRMVMSourceImage` PowerShell-cmdlet eller för en resurs Gruppmall.</span><span class="sxs-lookup"><span data-stu-id="e42d5-168">From this list, copy hello chosen SKU name, and you have all hello information for hello `Set-AzureRMVMSourceImage` PowerShell cmdlet or for a resource group template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e42d5-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e42d5-169">Next steps</span></span>
<span data-ttu-id="e42d5-170">Nu kan du välja exakt hello bild du toouse.</span><span class="sxs-lookup"><span data-stu-id="e42d5-170">Now you can choose precisely hello image you want toouse.</span></span> <span data-ttu-id="e42d5-171">toocreate finns i en virtuell dator snabbt med hjälp av hello avbildningen information som du har hittat [skapar en virtuell Windows-dator med PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e42d5-171">toocreate a virtual machine quickly by using hello image information, which you just found, see [Create a Windows virtual machine with PowerShell](quick-create-powershell.md).</span></span>
