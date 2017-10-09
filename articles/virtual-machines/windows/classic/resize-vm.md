---
title: aaaResize en virtuell Windows-dator i hello klassiska distributionsmodellen - Azure | Microsoft Docs
description: "Ändra storlek på en Windows-dator som skapats i hello klassiska distributionsmodellen med hjälp av Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e3038215-001c-406e-904d-e0f4e326a4c7
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 39fab14431e2351c9515b0611e850eccfec7a798
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-windows-vm-created-in-hello-classic-deployment-model"></a>Ändra storlek på en Windows-VM som skapats i hello klassiska distributionsmodellen
Den här artikeln visar hur tooresize en Windows-VM skapas i hello klassiska distributionsmodellen med hjälp av Azure Powershell.

När du överväger hello möjlighet tooresize en virtuell dator, finns det två principer som styr hello mängd storlekar tillgängliga tooresize hello virtuell dator. hello första konceptet är hello region där den virtuella datorn distribueras. hello listan över VM-storlekar tillgängligt i region finns under fliken för hello-tjänster på webbsidan för hello Azure-regioner. hello andra begrepp är hello fysisk maskinvara som för närvarande är värd för den virtuella datorn. hello fysiska servrar som är värd för virtuella datorer grupperas i kluster vanliga fysisk maskinvara. hello-metoden för att ändra en VM-storlek skiljer sig åt beroende på om hello önskad nya VM-storlek stöds av hello maskinvara klustret för närvarande värd hello VM.

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Du kan också [ändra storlek på en virtuell dator som skapats i hello Resource Manager-distributionsmodellen](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="add-your-account"></a>Lägg till ditt konto
Du måste konfigurera Azure PowerShell toowork med klassiska Azure-resurser. Följ hello steg nedan tooconfigure Azure PowerShell toomanage klassiska resurser.

1. I hello PowerShell-Kommandotolken, Skriv `Add-AzureAccount` och på **RETUR**. 
2. Ange hello e-postadress som är associerad med din Azure-prenumeration och klicka på **Fortsätt**. 
3. Ange hello lösenord för ditt konto. 
4. Klicka på **logga in**. 

## <a name="resize-in-hello-same-hardware-cluster"></a>Ändra storlek på i hello samma maskinvara kluster
tooresize en VM tooa storlek i hello maskinvara kluster som värd för hello VM, utföra hello följande steg.

1. Kör följande PowerShell-kommando toolist hello VM storlekar som finns tillgängliga i hello maskinvara kluster värdtjänsten hello molnet som innehåller hello VM hello.
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. Kör följande kommandon tooresize hello VM hello.
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Ändra storlek på ett nytt kluster för maskinvara
tooresize en VM tooa storleken är inte tillgänglig i hello maskinvara klustret värd Hej VM, hello Molntjänsten och alla virtuella datorer i hello Molntjänsten måste återskapas. Varje tjänst i molnet finns på ett kluster med samma maskinvara så att alla virtuella datorer i hello Molntjänsten måste ha en storlek som stöds på ett kluster för maskinvara. hello följande steg beskriver hur tooresize en virtuell dator genom att ta bort och sedan återskapa hello cloud service.

1. Kör följande PowerShell-kommandot toolist hello VM storlekar som finns tillgängliga i hello region hello. 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. Anteckna alla konfigurationsinställningar för varje virtuell dator i hello molntjänst som innehåller hello VM toobe storlek. 
3. Ta bort alla virtuella datorer i hello Molntjänsten välja hello alternativet tooretain hello diskar för varje virtuell dator.
4. Återskapa hello VM toobe storlek med hello önskade VM-storlek.
5. Återskapa alla andra virtuella datorer som fanns i hello Molntjänsten med hjälp av en VM-storlek som är tillgängliga i hello maskinvara kluster är nu värd hello-Molntjänsten.

Ett exempelskript för att ta bort och återskapa en tjänst i molnet med hjälp av en ny VM-storlek kan hittas [här](https://github.com/Azure/azure-vm-scripts). 

## <a name="next-steps"></a>Nästa steg
* [Ändra storlek på en virtuell dator som skapats i hello Resource Manager-distributionsmodellen](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

