---
title: aaaAzure virtuella skala anger anslutna Datadiskar | Microsoft Docs
description: "Lär dig hur toouse anslutna datadiskar med virtuella datorer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: guybo
ms.openlocfilehash: 77b66f80934c0aaf7bb1ad0de00a738052a878ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a>Skalningsuppsättningar för virtuella Azure-datorer och anslutna datadiskar
[Skaluppsättningar för virtuella Azure-datorer](/azure/virtual-machine-scale-sets/) stöder nu virtuella datorer med anslutna datadiskar. Datadiskar kan definieras i hello lagringsprofilen för skalningsuppsättningar som har skapats med Azure hanterade diskar. Tidigare var hello endast direktansluten lagring alternativen med virtuella datorer i skalningsuppsättningar hello OS-enhet och temp enheter.

> [!NOTE]
>  När du skapar en skala med anslutna datadiskar som har definierats, måste du ändå toomount och format hello diskar från inom en VM-toouse dem (precis som för fristående virtuella Azure-datorer). Ett enkelt sätt toodo är toouse ett tillägg för anpassat skript som anropar ett standardskript toopartition och formatera hello data diskarna på en virtuell dator.

## <a name="create-a-scale-set-with-attached-data-disks"></a>Skapa en skalningsuppsättning med anslutna datadiskar
Ett enkelt sätt toocreate skaluppsättning med anslutna diskar är toouse hello [Azure CLI](https://github.com/Azure/azure-cli) _vmss skapa_ kommando. hello följande exempel skapar en Azure-resursgrupp och en skalningsuppsättning 10 Ubuntu virtuella datorer, var och en med 2 bifogade datadiskar, 50 GB och 100 GB.
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
Observera att hello _vmss skapa_ vissa konfigurationsvärden standardplatsen om du inte anger dem. toosee hello tillgängliga alternativ att du kan åsidosätta försök:
```bash
az vmss create --help
```
Ett annat sätt toocreate en skala med anslutna diskar är toodefine en skala som i en Azure Resource Manager-mall innehåller en _dataDisks_ avsnitt i hello _storageProfile_, och distribuera hello mall. hello 50 GB och 100 GB disk exemplet ovan skulle definieras så här i hello mallen:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```
Du kan se en fullständig, redo toodeploy exempel på en mall för skala med en ansluten disk som anges här: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).

## <a name="adding-a-data-disk-tooan-existing-scale-set"></a>Lägga till en befintlig skala för data disk tooan ange
> [!NOTE]
>  Du kan bara bifoga diskar tooa skala datamängd som har skapats med [Azure hanterade diskar](./virtual-machine-scale-sets-managed-disks.md).

Du kan lägga till en data disk tooa VM-skala med Azure CLI _az vmss disk bifoga_ kommando. Se till att du anger en lun som inte redan används. följande exempel CLI hello lägger till en 50 GB enhet toolun 3:
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

hello följande PowerShell-exempel lägger till en 50 GB enhet toolun 3:
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> Olika storlekar på VM har olika begränsningar på hello antal anslutna enheter som de stöder. Kontrollera hello [virtuella storlek egenskaper](../virtual-machines/windows/sizes.md) innan du lägger till en ny disk.

Du kan också lägga till en disk genom att lägga till en ny post toohello _dataDisks_ egenskap i hello _storageProfile_ ange definition och tillämpa hello ändring av storlek. tootest detta, hitta en befintlig scale set-definition i hello [resursutforskaren Azure](https://resources.azure.com/). Välj _redigera_ och lägga till en ny disk toohello lista över datadiskar. T.ex. använda hello-exemplet ovan:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

Välj sedan _PLACERA_ tooapply hello ändras tooyour skaluppsättning. Det här exemplet skulle fungera så länge du använder en VM-storlek som har stöd för fler än två anslutna datadiskar.

> [!NOTE]
> När du gör en ändring tooa skaluppsättning definition, till exempel att lägga till eller ta bort en datadisk, det gäller tooall nyligen skapade virtuella datorer, men endast gäller tooexisting virtuella datorer om hello _upgradePolicy_ -egenskapen anges för ”automatisk”. Om det har angetts för ”manuell” är toomanually måste tillämpa hello nya modellen tooexisting virtuella datorer. Du kan göra detta i hello-portalen med hello _uppdatering AzureRmVmssInstance_ PowerShell, eller använda hello _az vmss update-instanser_ CLI-kommando.

## <a name="adding-pre-populated-data-disks-tooan-existent-scale-set"></a>Att lägga till data i förväg diskar tooan befintliga skaluppsättning 
> När du lägger till diskar tooan befintliga skaluppsättning modellen avsiktligt, hello disken skapas alltid tomt. Det här scenariot omfattar också nya instanser som skapats av hello skaluppsättning. Det här beteendet är eftersom hello scaleset definition har en tom datadisk. I ordning toocreate förifyllda dataenheter för en modell av befintliga scale set, kan du välja något av följande två alternativ:

* Kopiera data från hello instans 0 VM toohello data diskarna i hello andra virtuella datorer genom att köra ett anpassat skript.
* Skapa en hanterad avbildning med hello OS-disk plus datadisk (med hello krävs data) och skapa en ny scaleset med hello avbildningen. Det här sättet varje ny virtuell dator skapas har en disk som som har angetts i hello definition av hello scaleset. Eftersom den här definitionen kommer referera tooan avbildningen med en datadisk som har anpassade data kommer varje virtuell dator på hello scaleset automatiskt med ändringarna.

> Hej sätt toocreate en anpassad avbildning finns här: [skapa en hanterad avbildning av en generaliserad virtuell dator i Azure](/azure/virtual-machines/windows/capture-image-resource/) 

> hello användare behöver toocapture hello instansen 0 VM som har hello nödvändiga data och sedan använda den virtuella hårddisken för hello avbildningen definition.

## <a name="removing-a-data-disk-from-a-scale-set"></a>Ta bort en datadisk från en skalningsuppsättning
Du kan ta bort en datadisk från en skalningsuppsättning för virtuella datorer med Azure CLI-kommandot _az vmss disk detach_. Till exempel hello följande kommando tar bort hello disk på lun 2:
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
På samma sätt du kan också ta bort en disk från en skaluppsättningen genom att ta bort en post från hello _dataDisks_ egenskap i hello _storageProfile_ och tillämpa hello ändringen. 

## <a name="additional-notes"></a>Ytterligare information
Stöd för Azure Managed diskar och skala bifogade datadiskar är tillgängligt i API-versionen [ _2016-04-30-preview_ ](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) eller senare av hello Microsoft.Compute-API.

I hello första implementeringen av stöd för skalningsuppsättningar anslutna diskar, kan du koppla eller koppla från datadiskar till eller från enskilda virtuella datorer i en skaluppsättning.

Azure Portal-stöd för anslutna datadiskar i skalningsuppsättningar är begränsat i början. Beroende på dina krav som du kan använda mallar för Azure CLI, PowerShell, SDK: er och REST API toomanage anslutna diskar.


