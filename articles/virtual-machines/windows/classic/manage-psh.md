---
title: "aaaManage dina virtuella datorer med hjälp av Azure PowerShell | Microsoft Docs"
description: "Lär dig kommandon som du kan använda tooautomate uppgifter vid hantering av virtuella datorer."
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
ms.openlocfilehash: e4ca6f098519243a321eac98b6692790fe18c22c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Hantera dina virtuella datorer med Azure PowerShell
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Vanliga PowerShell-kommandon med hjälp av hello Resource Manager-modellen, se [här](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Många uppgifter du utför varje dag toomanage dina virtuella datorer kan automatiseras med hjälp av Azure PowerShell-cmdlets. Den här artikeln innehåller exempel på kommandon för enklare aktiviteter och länkar tooarticles som visar hello-kommandon för mer avancerade aktiviteter.

> [!NOTE]
> Om du inte har installerat och konfigurerat Azure PowerShell men du kan hämta instruktioner i hello artikel [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
> 
> 

## <a name="how-toouse-hello-example-commands"></a>Hur toouse hello exempelkommandon
Du behöver tooreplace vissa hello text i hello kommandon med text som är lämpliga för din miljö. Ange text som du behöver tooreplace Hej < och > symboler. När du ersätter hello text, ta bort hello symboler men lämna hello citattecken på plats.

## <a name="get-a-vm"></a>Hämta en virtuell dator
Detta är en grundläggande uppgift som du ofta använder. Använd den tooget information om en virtuell dator, utföra aktiviteter på en virtuell dator eller hämta utdata toostore i en variabel.

tooget information om hello VM, kör detta kommando ersätter allt i hello citattecken, inklusive Hej < och > tecken:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

toostore hello utdata i en variabel i $vm, kör:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-tooa-windows-based-vm"></a>Logga in tooa Windows-baserade Virtuella
Kör följande kommandon:

> [!NOTE]
> Du kan hämta hello virtuell dator och molntjänstnamnet från hello visningen av hello **Get-AzureVM** kommando.
> 
> $svcName = ”<cloud service name>” $vmName = ”<virtual machine name>” $localPath = ”< enhet och mapp plats toostore hello ned RDP-filen, exempel: c:\temp >” $localFile = $localPath + ”\" + $vmname +” .rdp ”Get-AzureRemoteDesktopFile - ServiceName $svcName-Name $vmName - LocalPath $localFile-starta
> 
> 

## <a name="stop-a-vm"></a>Stoppa en virtuell dator
Kör följande kommando:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Använd den här parametern tookeep hello virtuella IP (VIP) för hello moln tjänsten om det är hello senaste VM i Molntjänsten. <br><br> Om du använder hello StayProvisioned parameter kan faktureras du fortfarande för hello VM.
> 
> 

## <a name="start-a-vm"></a>Starta en virtuell dator
Kör följande kommando:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Anslut en datadisk
Den här uppgiften krävs några steg. Använd först hello *** Add-AzureDataDisk *** cmdlet tooadd hello toohello $vm diskobjektet. Sedan kan du använda **uppdatering AzureVM** cmdlet tooupdate hello konfigurationen av hello VM.

Du måste också toodecide om tooattach en ny disk eller en som innehåller data. För en ny disk hello skapar hello VHD-filen och bifogas.

tooattach kör det här kommandot för en ny disk:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

tooattach en befintlig datadisk kör detta kommando:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

tooattach datadiskar från en befintlig VHD-fil i blob storage kör detta kommando:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Skapa en Windows-baserad virtuell dator
toocreate en ny Windows-baserad virtuell dator i Azure, Använd hello instruktionerna i [använda Azure PowerShell toocreate och förkonfigurera en Windows-baserade virtuella datorer](create-powershell.md). Det här avsnittet steg du via hello skapandet av en Azure PowerShell kommandouppsättning som skapar ett Windows-baserade Virtuella kan förkonfigureras:

* Med medlemskap i Active Directory-domän.
* Med ytterligare diskar.
* Anger som en medlem av en befintlig belastningsutjämnad.
* Med en statisk IP-adress.

