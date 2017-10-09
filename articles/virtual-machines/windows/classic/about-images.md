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
# <a name="about-images-for-windows-virtual-machines"></a>Om avbildningar för Windows-datorer
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Information om att hitta och använda bilder i hello Resource Manager-modellen finns [här](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a>Arbeta med bilder

Du kan använda hello Azure PowerShell-modulen och hello Azure portal toomanage hello avbildningar tillgängliga tooyour Azure-prenumeration. hello Azure PowerShell-modulen innehåller fler kommandoalternativ, så du kan hitta vad toosee eller gör. hello Azure-portalen innehåller ett grafiskt användargränssnitt för många hello vanliga administrativa uppgifter.

Här följer några exempel som använder hello Azure PowerShell-modulen.

* **Hämta alla bilder**:`Get-AzureVMImage`returnerar en lista över alla hello avbildningar som är tillgängliga i din aktuella prenumeration: bilderna och de som tillhandahålls av Azure eller partners. hello resultatlistan kan vara stora. Hej nästa exempel visas hur tooget en kortare lista.
* **Hämta bild familjer**:`Get-AzureVMImage | select ImageFamily` hämtar en lista över avbildningen familjer genom att visa strängar **ImageFamily** egenskapen.
* **Hämta alla bilder i en specifik familj**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **Hitta VM-avbildningar**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` denna cmdlet fungerar genom att filtrera hello DataDiskConfiguration-egenskap som endast gäller tooVM bilder. Det här exemplet filtrerar också hello tooonly hello etikett- och utdatanamn.
* **Spara en generaliserad avbildning**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **Spara en specialiserad avbildning**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`

  > [!TIP]
  > Hej OSState parametern är obligatorisk toocreate en VM-avbildning som innehåller hello operativsystemdisken och anslutna datadiskar. Om du inte använder parametern hello skapar hello cmdlet en operativsystemavbildning. hello värdet hello-parametern anger om hello bilden är generaliserad eller specialiserade, baserat på om hello operativsystemdisken har förberetts för återanvändning.

* **Ta bort en bild**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`

## <a name="next-steps"></a>Nästa steg
Du kan också [skapar en Windows-dator med hello Azure-portalen](tutorial.md).
