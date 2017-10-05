---
title: "Om avbildningar för Windows-datorer | Microsoft Docs"
description: "Läs mer om hur bilder används med Windows-datorer i Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cynthn
ms.openlocfilehash: d421cee0becabdf81d865036d0c98b12b077152b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="about-images-for-windows-virtual-machines"></a><span data-ttu-id="a5290-103">Om avbildningar för Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="a5290-103">About images for Windows virtual machines</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a5290-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a5290-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a5290-105">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="a5290-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="a5290-106">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="a5290-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="a5290-107">Information om att hitta och använda bilder i Resource Manager-modellen finns [här](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a5290-107">For information about finding and using images in the Resource Manager model, see [here](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a><span data-ttu-id="a5290-108">Arbeta med bilder</span><span class="sxs-lookup"><span data-stu-id="a5290-108">Working with images</span></span>

<span data-ttu-id="a5290-109">Du kan använda Azure PowerShell-modulen och Azure portal för att hantera bilderna som finns på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="a5290-109">You can use the Azure PowerShell module and the Azure portal to manage the images available to your Azure subscription.</span></span> <span data-ttu-id="a5290-110">Azure PowerShell-modulen innehåller fler kommandoalternativ, så du kan hitta exakt vad du vill se eller göra.</span><span class="sxs-lookup"><span data-stu-id="a5290-110">The Azure PowerShell module provides more command options, so you can pinpoint exactly what you want to see or do.</span></span> <span data-ttu-id="a5290-111">Azure portal tillhandahåller ett grafiskt användargränssnitt för många av de vanliga administrativa uppgifterna.</span><span class="sxs-lookup"><span data-stu-id="a5290-111">The Azure portal provides a GUI for many of the everyday administrative tasks.</span></span>

<span data-ttu-id="a5290-112">Här följer några exempel som använder Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="a5290-112">Here are some examples that use the Azure PowerShell module.</span></span>

* <span data-ttu-id="a5290-113">**Hämta alla bilder**:`Get-AzureVMImage`returnerar en lista över alla avbildningar som är tillgängliga i din aktuella prenumeration: bilderna och de som tillhandahålls av Azure eller partners.</span><span class="sxs-lookup"><span data-stu-id="a5290-113">**Get all images**:`Get-AzureVMImage`returns a list of all the images available in your current subscription: your images and those provided by Azure or partners.</span></span> <span data-ttu-id="a5290-114">Listan kan vara stora.</span><span class="sxs-lookup"><span data-stu-id="a5290-114">The resulting list could be large.</span></span> <span data-ttu-id="a5290-115">I nästa exempel visas hur du hämtar en kortare lista.</span><span class="sxs-lookup"><span data-stu-id="a5290-115">The next examples show how to get a shorter list.</span></span>
* <span data-ttu-id="a5290-116">**Hämta bild familjer**:`Get-AzureVMImage | select ImageFamily` hämtar en lista över avbildningen familjer genom att visa strängar **ImageFamily** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a5290-116">**Get image families**:`Get-AzureVMImage | select ImageFamily` gets a list of image families by showing strings **ImageFamily** property.</span></span>
* <span data-ttu-id="a5290-117">**Hämta alla bilder i en specifik familj**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span><span class="sxs-lookup"><span data-stu-id="a5290-117">**Get all images in a specific family**: `Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span></span>
* <span data-ttu-id="a5290-118">**Hitta VM-avbildningar**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` denna cmdlet fungerar genom att filtrera egenskapen DataDiskConfiguration, som gäller endast för VM-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="a5290-118">**Find VM Images**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` This cmdlet works by filtering the DataDiskConfiguration property, which only applies to VM Images.</span></span> <span data-ttu-id="a5290-119">Det här exemplet filtrerar också utdata till endast etikett- och namn.</span><span class="sxs-lookup"><span data-stu-id="a5290-119">This example also filters the output to only the label and image name.</span></span>
* <span data-ttu-id="a5290-120">**Spara en generaliserad avbildning**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span><span class="sxs-lookup"><span data-stu-id="a5290-120">**Save a generalized image**: `Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span></span>
* <span data-ttu-id="a5290-121">**Spara en specialiserad avbildning**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span><span class="sxs-lookup"><span data-stu-id="a5290-121">**Save a specialized image**: `Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span></span>

  > [!TIP]
  > <span data-ttu-id="a5290-122">Parametern OSState krävs för att skapa en VM-avbildning som innehåller operativsystemets disk och anslutna datadiskar.</span><span class="sxs-lookup"><span data-stu-id="a5290-122">The OSState parameter is required to create a VM image, which includes the operating system disk and attached data disks.</span></span> <span data-ttu-id="a5290-123">Om du inte använder parametern skapar cmdleten en operativsystemavbildning.</span><span class="sxs-lookup"><span data-stu-id="a5290-123">If you don’t use the parameter, the cmdlet creates an OS image.</span></span> <span data-ttu-id="a5290-124">Värdet för parametern anger om bilden är generaliserad eller specialiserade, baserat på om operativsystemdisken har förberetts för återanvändning.</span><span class="sxs-lookup"><span data-stu-id="a5290-124">The value of the parameter indicates whether the image is generalized or specialized, based on whether the operating system disk has been prepared for reuse.</span></span>

* <span data-ttu-id="a5290-125">**Ta bort en bild**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`</span><span class="sxs-lookup"><span data-stu-id="a5290-125">**Delete an image**: `Remove-AzureVMImage –ImageName "MyOldVmImage"`</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5290-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5290-126">Next Steps</span></span>
<span data-ttu-id="a5290-127">Du kan också [skapa en Windows-dator med hjälp av Azure portal](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="a5290-127">You can also [create a Windows machine using the Azure portal](tutorial.md).</span></span>
