---
title: "aaaAbout avbildningar för Windows-datorer | Microsoft Docs"
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
ms.openlocfilehash: c7cfa1d018a5e99d5b68f559ec9ae1f14e4dec8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-images-for-windows-virtual-machines"></a><span data-ttu-id="65f1f-103">Om avbildningar för Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="65f1f-103">About images for Windows virtual machines</span></span>
> [!IMPORTANT]
> <span data-ttu-id="65f1f-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="65f1f-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="65f1f-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="65f1f-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="65f1f-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="65f1f-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="65f1f-107">Information om att hitta och använda bilder i hello Resource Manager-modellen finns [här](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="65f1f-107">For information about finding and using images in hello Resource Manager model, see [here](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a><span data-ttu-id="65f1f-108">Arbeta med bilder</span><span class="sxs-lookup"><span data-stu-id="65f1f-108">Working with images</span></span>

<span data-ttu-id="65f1f-109">Du kan använda hello Azure PowerShell-modulen och hello Azure portal toomanage hello avbildningar tillgängliga tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="65f1f-109">You can use hello Azure PowerShell module and hello Azure portal toomanage hello images available tooyour Azure subscription.</span></span> <span data-ttu-id="65f1f-110">hello Azure PowerShell-modulen innehåller fler kommandoalternativ, så du kan hitta vad toosee eller gör.</span><span class="sxs-lookup"><span data-stu-id="65f1f-110">hello Azure PowerShell module provides more command options, so you can pinpoint exactly what you want toosee or do.</span></span> <span data-ttu-id="65f1f-111">hello Azure-portalen innehåller ett grafiskt användargränssnitt för många hello vanliga administrativa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="65f1f-111">hello Azure portal provides a GUI for many of hello everyday administrative tasks.</span></span>

<span data-ttu-id="65f1f-112">Här följer några exempel som använder hello Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="65f1f-112">Here are some examples that use hello Azure PowerShell module.</span></span>

* <span data-ttu-id="65f1f-113">**Hämta alla bilder**:`Get-AzureVMImage`returnerar en lista över alla hello avbildningar som är tillgängliga i din aktuella prenumeration: bilderna och de som tillhandahålls av Azure eller partners.</span><span class="sxs-lookup"><span data-stu-id="65f1f-113">**Get all images**:`Get-AzureVMImage`returns a list of all hello images available in your current subscription: your images and those provided by Azure or partners.</span></span> <span data-ttu-id="65f1f-114">hello resultatlistan kan vara stora.</span><span class="sxs-lookup"><span data-stu-id="65f1f-114">hello resulting list could be large.</span></span> <span data-ttu-id="65f1f-115">Hej nästa exempel visas hur tooget en kortare lista.</span><span class="sxs-lookup"><span data-stu-id="65f1f-115">hello next examples show how tooget a shorter list.</span></span>
* <span data-ttu-id="65f1f-116">**Hämta bild familjer**:`Get-AzureVMImage | select ImageFamily` hämtar en lista över avbildningen familjer genom att visa strängar **ImageFamily** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="65f1f-116">**Get image families**:`Get-AzureVMImage | select ImageFamily` gets a list of image families by showing strings **ImageFamily** property.</span></span>
* <span data-ttu-id="65f1f-117">**Hämta alla bilder i en specifik familj**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span><span class="sxs-lookup"><span data-stu-id="65f1f-117">**Get all images in a specific family**: `Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span></span>
* <span data-ttu-id="65f1f-118">**Hitta VM-avbildningar**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` denna cmdlet fungerar genom att filtrera hello DataDiskConfiguration-egenskap som endast gäller tooVM bilder.</span><span class="sxs-lookup"><span data-stu-id="65f1f-118">**Find VM Images**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` This cmdlet works by filtering hello DataDiskConfiguration property, which only applies tooVM Images.</span></span> <span data-ttu-id="65f1f-119">Det här exemplet filtrerar också hello tooonly hello etikett- och utdatanamn.</span><span class="sxs-lookup"><span data-stu-id="65f1f-119">This example also filters hello output tooonly hello label and image name.</span></span>
* <span data-ttu-id="65f1f-120">**Spara en generaliserad avbildning**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span><span class="sxs-lookup"><span data-stu-id="65f1f-120">**Save a generalized image**: `Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span></span>
* <span data-ttu-id="65f1f-121">**Spara en specialiserad avbildning**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span><span class="sxs-lookup"><span data-stu-id="65f1f-121">**Save a specialized image**: `Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span></span>

  > [!TIP]
  > <span data-ttu-id="65f1f-122">Hej OSState parametern är obligatorisk toocreate en VM-avbildning som innehåller hello operativsystemdisken och anslutna datadiskar.</span><span class="sxs-lookup"><span data-stu-id="65f1f-122">hello OSState parameter is required toocreate a VM image, which includes hello operating system disk and attached data disks.</span></span> <span data-ttu-id="65f1f-123">Om du inte använder parametern hello skapar hello cmdlet en operativsystemavbildning.</span><span class="sxs-lookup"><span data-stu-id="65f1f-123">If you don’t use hello parameter, hello cmdlet creates an OS image.</span></span> <span data-ttu-id="65f1f-124">hello värdet hello-parametern anger om hello bilden är generaliserad eller specialiserade, baserat på om hello operativsystemdisken har förberetts för återanvändning.</span><span class="sxs-lookup"><span data-stu-id="65f1f-124">hello value of hello parameter indicates whether hello image is generalized or specialized, based on whether hello operating system disk has been prepared for reuse.</span></span>

* <span data-ttu-id="65f1f-125">**Ta bort en bild**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`</span><span class="sxs-lookup"><span data-stu-id="65f1f-125">**Delete an image**: `Remove-AzureVMImage –ImageName "MyOldVmImage"`</span></span>

## <a name="next-steps"></a><span data-ttu-id="65f1f-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="65f1f-126">Next Steps</span></span>
<span data-ttu-id="65f1f-127">Du kan också [skapar en Windows-dator med hello Azure-portalen](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="65f1f-127">You can also [create a Windows machine using hello Azure portal](tutorial.md).</span></span>
