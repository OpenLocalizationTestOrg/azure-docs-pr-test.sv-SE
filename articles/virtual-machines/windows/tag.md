---
title: aaaHow tootag Windows VM resurs i Azure | Microsoft Docs
description: "Lär dig mer om en Windows-dator som skapats i Azure med hjälp av Resource Manager-modellen hello-märkning"
services: virtual-machines-windows
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 56d17f45-e4a7-4d84-8022-b40334ae49d2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 160416ddc35998b3c98c6e579668a6a5eb6de6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-windows-virtual-machine-in-azure"></a>Hur tootag en Windows-dator i Azure
Den här artikeln beskrivs olika sätt tootag en Windows-dator i Azure med hello Resource Manager-modellen. Taggar är användardefinierade nyckel/värde-par som kan placeras direkt på en resurs eller en resursgrupp. Azure stöder för närvarande in too15 taggar per resurs och resursgruppen. Taggar kan placeras på en resurs för närvarande hello skapas eller lägga tooan befintlig resurs. Observera att taggar stöds för resurser som skapats via hello Resource Manager distributionsmodellen. Om du vill tootag en Linux-dator, se [hur tootag en Linux-dator i Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Tagga med PowerShell
toocreate, lägga till och ta bort taggar via PowerShell, måste du först tooset upp din [PowerShell-miljö med Azure Resource Manager][PowerShell environment with Azure Resource Manager]. När du har slutfört installationen hello, kan du placera taggar på beräkning, nätverk och lagring resurser vid skapandet eller när hello resursen har skapats via PowerShell. Den här artikeln kommer att inriktas på Visa/redigera taggar som placeras på virtuella datorer.

Först gå tooa virtuella datorn via hello `Get-AzureRmVM` cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Om den virtuella datorn redan innehåller taggar kan sedan visas alla hello taggar på resursen:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Om du vill tooadd taggar via PowerShell kan du använda hello `Set-AzureRmResource` kommando. Observera vid uppdatering av taggarna via PowerShell taggar uppdateras som helhet. Så om du lägger till en tagg tooa resurs som redan har taggar måste tooinclude alla hello-taggar som du vill toobe placerad hello resurs. Nedan visas ett exempel på hur tooadd ytterligare taggar tooa resursen via PowerShell-Cmdlets.

Den här första cmdlet anger alla hello taggarna placerad *MyTestVM* toohello *$tags* variabel, med hjälp av hello `Get-AzureRmResource` och `Tags` egenskapen.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

hello andra kommandot visar hello taggar för hello angivna variabeln.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment

hello tredje kommandot lägger till en ytterligare taggen toohello *$tags* variabeln. Observera hello användning av hello  **+=**  tooappend hello nytt nyckel/värde-par toohello *$tags* lista.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

hello fjärde kommando anges alla hello-taggar som definierats i hello *$tags* variabeln toohello angivna resursen. I det här fallet är det MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

hello femte kommando visar alla hello taggarna på hello resurs. Som du kan se *plats* nu har definierats som en tagg med *MyLocation* som hello-värde.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment
        Value        MyLocation
        Name        Location

Mer om toolearn taggning via PowerShell, kolla hello [Cmdlets för Azure-resurs][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nästa steg
* toolearn mer information om resurserna i Azure-märkning finns [översikt över Azure Resource Manager] [ Azure Resource Manager Overview] och [med hjälp av taggar tooorganize dina Azure-resurser] [ Using Tags tooorganize your Azure Resources].
* toosee hur taggar kan hjälpa dig att hantera din användning av Azure-resurser, se [förstå fakturan Azure] [ Understanding your Azure Bill] och [få insikter om dina Microsoft Azure-resursförbrukning] [Gain insights into your Microsoft Azure resource consumption].

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
