---
title: "Välj Windows VM-avbildningar i Azure | Microsoft Docs"
description: "Lär dig hur du använder Azure PowerSHell för att fastställa utgivare, erbjudande, SKU och version för Marketplace VM-avbildningar."
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
ms.openlocfilehash: 814ae260123c045d4b6766bf4b312f874cd77068
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a><span data-ttu-id="ba328-103">Hitta Windows VM-avbildningar i Azure Marketplace med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ba328-103">How to find Windows VM images in the Azure Marketplace with Azure PowerShell</span></span>

<span data-ttu-id="ba328-104">Det här avsnittet beskriver hur du använder Azure PowerShell för att hitta VM-avbildningar i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ba328-104">This topic describes how to use Azure PowerShell to find VM images in the Azure Marketplace.</span></span> <span data-ttu-id="ba328-105">Använd informationen för att ange en Marketplace-avbildning när du skapar en virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="ba328-105">Use this information to specify a Marketplace image when you create a Windows VM.</span></span>

<span data-ttu-id="ba328-106">Kontrollera att du har installerat och konfigurerat senast [Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="ba328-106">Make sure that you installed and configured the latest [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>



## <a name="table-of-commonly-used-windows-images"></a><span data-ttu-id="ba328-107">Tabell med vanliga Windows-avbildningar</span><span class="sxs-lookup"><span data-stu-id="ba328-107">Table of commonly used Windows images</span></span>
| <span data-ttu-id="ba328-108">PublisherName</span><span class="sxs-lookup"><span data-stu-id="ba328-108">PublisherName</span></span> | <span data-ttu-id="ba328-109">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="ba328-109">Offer</span></span> | <span data-ttu-id="ba328-110">Sku</span><span class="sxs-lookup"><span data-stu-id="ba328-110">Sku</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ba328-111">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-111">MicrosoftWindowsServer</span></span> |<span data-ttu-id="ba328-112">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-112">WindowsServer</span></span> |<span data-ttu-id="ba328-113">2016 Datacenter</span><span class="sxs-lookup"><span data-stu-id="ba328-113">2016-Datacenter</span></span> |
| <span data-ttu-id="ba328-114">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-114">MicrosoftWindowsServer</span></span> |<span data-ttu-id="ba328-115">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-115">WindowsServer</span></span> |<span data-ttu-id="ba328-116">2016-Datacenter-Server-Core</span><span class="sxs-lookup"><span data-stu-id="ba328-116">2016-Datacenter-Server-Core</span></span> |
| <span data-ttu-id="ba328-117">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-117">MicrosoftWindowsServer</span></span> |<span data-ttu-id="ba328-118">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-118">WindowsServer</span></span> |<span data-ttu-id="ba328-119">2016 Datacenter med behållare</span><span class="sxs-lookup"><span data-stu-id="ba328-119">2016-Datacenter-with-Containers</span></span> |
| <span data-ttu-id="ba328-120">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-120">MicrosoftWindowsServer</span></span> |<span data-ttu-id="ba328-121">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-121">WindowsServer</span></span> |<span data-ttu-id="ba328-122">2016 Nano Server</span><span class="sxs-lookup"><span data-stu-id="ba328-122">2016-Nano-Server</span></span> |
| <span data-ttu-id="ba328-123">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-123">MicrosoftWindowsServer</span></span> |<span data-ttu-id="ba328-124">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-124">WindowsServer</span></span> |<span data-ttu-id="ba328-125">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="ba328-125">2012-R2-Datacenter</span></span> |
| <span data-ttu-id="ba328-126">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-126">MicrosoftWindowsServer</span></span> |<span data-ttu-id="ba328-127">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="ba328-127">WindowsServer</span></span> |<span data-ttu-id="ba328-128">2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="ba328-128">2008-R2-SP1</span></span> |
| <span data-ttu-id="ba328-129">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="ba328-129">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="ba328-130">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="ba328-130">DynamicsNAV</span></span> |<span data-ttu-id="ba328-131">2017</span><span class="sxs-lookup"><span data-stu-id="ba328-131">2017</span></span> |
| <span data-ttu-id="ba328-132">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="ba328-132">MicrosoftSharePoint</span></span> |<span data-ttu-id="ba328-133">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="ba328-133">MicrosoftSharePointServer</span></span> |<span data-ttu-id="ba328-134">2016</span><span class="sxs-lookup"><span data-stu-id="ba328-134">2016</span></span> |
| <span data-ttu-id="ba328-135">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="ba328-135">MicrosoftSQLServer</span></span> |<span data-ttu-id="ba328-136">SQL2016 WS2016</span><span class="sxs-lookup"><span data-stu-id="ba328-136">SQL2016-WS2016</span></span> |<span data-ttu-id="ba328-137">Enterprise</span><span class="sxs-lookup"><span data-stu-id="ba328-137">Enterprise</span></span> |
| <span data-ttu-id="ba328-138">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="ba328-138">MicrosoftSQLServer</span></span> |<span data-ttu-id="ba328-139">SQL2014SP2 WS2012R2</span><span class="sxs-lookup"><span data-stu-id="ba328-139">SQL2014SP2-WS2012R2</span></span> |<span data-ttu-id="ba328-140">Enterprise</span><span class="sxs-lookup"><span data-stu-id="ba328-140">Enterprise</span></span> |
| <span data-ttu-id="ba328-141">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="ba328-141">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="ba328-142">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="ba328-142">WindowsServerHPCPack</span></span> |<span data-ttu-id="ba328-143">2012R2</span><span class="sxs-lookup"><span data-stu-id="ba328-143">2012R2</span></span> |
| <span data-ttu-id="ba328-144">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="ba328-144">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="ba328-145">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="ba328-145">WindowsServerEssentials</span></span> |<span data-ttu-id="ba328-146">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="ba328-146">WindowsServerEssentials</span></span> |

## <a name="find-specific-images"></a><span data-ttu-id="ba328-147">Söka efter specifika avbildningar</span><span class="sxs-lookup"><span data-stu-id="ba328-147">Find specific images</span></span>


<span data-ttu-id="ba328-148">När du skapar en ny virtuell dator med Azure Resource Manager måste du i vissa fall ange en avbildning med följande kombination av egenskaper:</span><span class="sxs-lookup"><span data-stu-id="ba328-148">When creating a new virtual machine with Azure Resource Manager, in some cases you need to specify an image with the combination of the following image properties:</span></span>

* <span data-ttu-id="ba328-149">Utgivare</span><span class="sxs-lookup"><span data-stu-id="ba328-149">Publisher</span></span>
* <span data-ttu-id="ba328-150">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="ba328-150">Offer</span></span>
* <span data-ttu-id="ba328-151">SKU</span><span class="sxs-lookup"><span data-stu-id="ba328-151">SKU</span></span>

<span data-ttu-id="ba328-152">Till exempel använda dessa värden med den [Set AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell-cmdlet eller med en mall för gruppen av resurs som du måste ange vilken typ av virtuell dator som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="ba328-152">For example, use these values with the [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell cmdlet, or with a resource group template in which you must specify the type of VM to be created.</span></span>

<span data-ttu-id="ba328-153">Om du behöver kunna bedöma dessa värden kan du köra den [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), och [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) -cmdletar för att navigera bilder.</span><span class="sxs-lookup"><span data-stu-id="ba328-153">If you need to determine these values, you can run the [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), and [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets to navigate the images.</span></span> <span data-ttu-id="ba328-154">Du kan bestämma dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ba328-154">You determine these values:</span></span>

1. <span data-ttu-id="ba328-155">Visa en lista över avbildningsutgivare.</span><span class="sxs-lookup"><span data-stu-id="ba328-155">List the image publishers.</span></span>
2. <span data-ttu-id="ba328-156">Visa en lista över erbjudanden från en viss utgivare.</span><span class="sxs-lookup"><span data-stu-id="ba328-156">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="ba328-157">Visa en lista över SKU:er för ett visst erbjudande.</span><span class="sxs-lookup"><span data-stu-id="ba328-157">For a given offer, list their SKUs.</span></span>

<span data-ttu-id="ba328-158">Börja med att skapa en lista över utgivare med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ba328-158">First, list the publishers with the following commands:</span></span>

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

<span data-ttu-id="ba328-159">Fyll i namnet på den valda utgivaren och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ba328-159">Fill in your chosen publisher name and run the following commands:</span></span>

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="ba328-160">Fyll i namnet på det valda erbjudandet och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ba328-160">Fill in your chosen offer name and run the following commands:</span></span>

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="ba328-161">Från utdata från den `Get-AzureRMVMImageSku` kommando du har all information du behöver ange bild för en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ba328-161">From the output of the `Get-AzureRMVMImageSku` command, you have all the information you need to specify the image for a new virtual machine.</span></span>

<span data-ttu-id="ba328-162">Följande visar ett fullständigt exempel:</span><span class="sxs-lookup"><span data-stu-id="ba328-162">The following shows a full example:</span></span>

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

<span data-ttu-id="ba328-163">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ba328-163">Output:</span></span>

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

<span data-ttu-id="ba328-164">För utgivaren ”MicrosoftWindowsServer”:</span><span class="sxs-lookup"><span data-stu-id="ba328-164">For the "MicrosoftWindowsServer" publisher:</span></span>

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="ba328-165">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ba328-165">Output:</span></span>

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

<span data-ttu-id="ba328-166">För erbjudandet ”WindowsServer”:</span><span class="sxs-lookup"><span data-stu-id="ba328-166">For the "WindowsServer" offer:</span></span>

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="ba328-167">Resultat:</span><span class="sxs-lookup"><span data-stu-id="ba328-167">Output:</span></span>

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

<span data-ttu-id="ba328-168">Kopiera det valda SKU-namnet från listan så har du all information om `Set-AzureRMVMSourceImage` PowerShell-cmdleten eller om en resursgruppmall.</span><span class="sxs-lookup"><span data-stu-id="ba328-168">From this list, copy the chosen SKU name, and you have all the information for the `Set-AzureRMVMSourceImage` PowerShell cmdlet or for a resource group template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba328-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba328-169">Next steps</span></span>
<span data-ttu-id="ba328-170">Du kan nu välja exakt den avbildning som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="ba328-170">Now you can choose precisely the image you want to use.</span></span> <span data-ttu-id="ba328-171">För att snabbt skapa en virtuell dator med hjälp av avbildningsinformationen som du har hittat finns [skapar en virtuell Windows-dator med PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ba328-171">To create a virtual machine quickly by using the image information, which you just found, see [Create a Windows virtual machine with PowerShell](quick-create-powershell.md).</span></span>
