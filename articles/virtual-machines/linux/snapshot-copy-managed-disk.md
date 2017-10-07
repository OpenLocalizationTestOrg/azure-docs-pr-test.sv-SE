---
title: "ett Azure hanterade disken för tillbaka in aaaCopy | Microsoft Docs"
description: "Lär dig hur toocreate en kopia av en Azure-hanterade disken toouse för tillbaka dig eller felsökning disken utfärdar."
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/6/2017
ms.author: rasquill
ms.openlocfilehash: 41b91c2d68eb5be9c493a66be5f7d085a70450d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Skapa en kopia av en virtuell Hårddisk lagras som en hanterad Azure-disken med hjälp av hanterade ögonblicksbilder
Ta en ögonblicksbild av en hanterad disk för säkerhetskopiering eller skapa en Disk som hanteras av hello ögonblicksbild och koppla den tooa test virtuella tootroubleshoot. En ögonblicksbild av en hanterad är en fullständig i tidpunkt kopia av en virtuell dator hanteras Disk. Den skapar en skrivskyddad kopia av den virtuella Hårddisken och som standard lagras som en Standard hanteras Disk. 

Information om priser finns i [priser för Azure Storage](https://azure.microsoft.com/pricing/details/managed-disks/). <!--Add link tootopic or blog post that explains managed disks. -->

Använd antingen hello Azure-portalen eller hello Azure CLI 2.0 tootake en ögonblicksbild av hello hanterade disken.

## <a name="use-azure-cli-20-tootake-a-snapshot"></a>Använda Azure CLI 2.0 tootake en ögonblicksbild

> [!NOTE] 
> hello kräver följande exempel hello Azure CLI 2.0 installerat och loggat in på ditt Azure-konto.

hello följande steg visar hur tooobtain och ta en ögonblicksbild av en hanterad OS-disken med hello `az snapshot create` med hello `--source-disk` parameter. hello följande exempel förutsätter att det finns en virtuell dator kallas `myVM` skapats med en hanterad OS-disk i hello `myResourceGroup` resursgruppen.

```azure-cli
# take hello disk id with which toocreate a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

hello utdata ska se ut ungefär:

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-tootake-a-snapshot"></a>Använda Azure portal tootake en ögonblicksbild 

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Från och med hello längst upp till vänster, klicka på **ny** och Sök efter **ögonblicksbild**.
3. I hello Snapshot-bladet, klickar du på **skapa**.
4. Ange en **namn** för hello ögonblicksbilden.
5. Välj en befintlig [resursgruppen](../../azure-resource-manager/resource-group-overview.md#resource-groups) eller hello-typnamn för en ny. 
6. Välj ett Azure-datacenter plats.  
7. För **källdisken**, Välj hello hanteras Disk toosnapshot.
8. Välj hello **kontotyp** toouse toostore hello ögonblicksbild. Vi rekommenderar **Standard_LRS** om du inte behöver den lagras på en disk med hög prestanda.
9. Klicka på **Skapa**.

Om du planerar toouse hello ögonblicksbild toocreate en Disk som hanteras och koppla den till en virtuell dator som behöver toobe högpresterande, använda hello parametern `--sku Premium_LRS` med hello `az snapshot create` kommando. Detta skapar hello ögonblicksbild så att den lagras som en Premium hanteras Disk. Hanterade Premiumdiskar bättre eftersom de SSD-enheter (SSD), men kostar mer än standarddiskar (HDD).


