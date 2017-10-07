---
title: "aaaCreate en kopia av en Azure-hanterade disken för tillbaka in | Microsoft Docs"
description: "Lär dig hur toocreate en kopia av en Azure-hanterade disken toouse för tillbaka dig eller felsökning disken utfärdar."
documentationcenter: 
author: cwatson-cat
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 2/9/2017
ms.author: cwatson
ms.openlocfilehash: 2f33dbbee624bcd813f3c7c3e3401072d0933714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Skapa en kopia av en virtuell Hårddisk lagras som en hanterad Azure-disken med hjälp av hanterade ögonblicksbilder
Ta en ögonblicksbild av en hanterad Disk för säkerhetskopiering eller skapa en Disk som hanteras av hello ögonblicksbild och koppla den tooa test virtuella tootroubleshoot. En ögonblicksbild av en hanterad är en fullständig i tidpunkt kopia av en virtuell dator hanteras Disk. Den skapar en skrivskyddad kopia av den virtuella Hårddisken och som standard lagras som en Standard hanteras Disk. Mer information om hanterade diskar finns [översikt över Azure hanterade diskar](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

Information om priser finns i [priser för Azure Storage](https://azure.microsoft.com/pricing/details/managed-disks/). 

## <a name="before-you-begin"></a>Innan du börjar
Om du använder PowerShell kan du se till att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen. Kör följande kommando tooinstall hello den.

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).

## <a name="copy-hello-vhd-with-a-snapshot"></a>Kopiera hello VHD med en ögonblicksbild
Använd hello Azure-portalen eller PowerShell tootake en ögonblicksbild av hello hanterade disken.

### <a name="use-azure-portal-tootake-a-snapshot"></a>Använda Azure portal tootake en ögonblicksbild 

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Från och med hello övre vänstra hörnet, klickar du på **ny** och Sök efter **ögonblicksbild**.
3. I hello Snapshot-bladet, klickar du på **skapa**.
4. Ange en **namn** för hello ögonblicksbilden.
5. Välj en befintlig [resursgruppen](../../azure-resource-manager/resource-group-overview.md#resource-groups) eller hello-typnamn för en ny. 
6. Välj ett Azure-datacenter plats.  
7. För **källdisken**, Välj hello hanteras Disk toosnapshot.
8. Välj hello **kontotyp** toouse toostore hello ögonblicksbild. Vi rekommenderar **Standard_LRS** om du inte behöver den lagras på en disk med hög prestanda.
9. Klicka på **Skapa**.

### <a name="use-powershell-tootake-a-snapshot"></a>Använd PowerShell tootake en ögonblicksbild
hello följande steg visar hur tooget hello VHD disk toobe kopieras, skapa hello konfigurationer av ögonblicksbilder och ta en ögonblicksbild av hello disk med hjälp av cmdlet hello ny AzureRmSnapshot<!--Add link toocmdlet when available-->. 

1. Ange vissa parametrar. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  Ersätt hello parametervärden:
  -  ”myResourceGroup” hello VM resursgruppen.
  -  ”southeastasia” med hello geografiska plats där du vill att din hanteras ögonblicksbild lagras. <!---How do you look these up? -->
  -  ”ContosoMD_datadisk1” med namnet hello av hello VHD-disk som du vill toocopy.
  -  ”ContosoMD_datadisk1_snapshot1” med hello namn du vill använda toouse för hello ny ögonblicksbild.

2. Hämta hello VHD disk toobe kopieras.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. Skapa hello konfigurationer av ögonblicksbilder. 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. Ta hello ögonblicksbild.

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
Om du planerar toouse hello ögonblicksbild toocreate en Disk som hanteras och koppla den till en virtuell dator som behöver toobe högpresterande, använda hello parametern `-AccountType Premium_LRS` med hello ny AzureRmSnapshot kommando. hello-parametern skapar hello ögonblicksbild så att den lagras som en Premium hanteras Disk. Premium-hanterade diskar är dyrare än Standard. Vara säker på att du verkligen behöver Premium innan du använder parametern.


