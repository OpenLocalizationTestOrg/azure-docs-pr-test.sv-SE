---
title: "aaaMigrate från AWS och andra plattformar tooManaged diskar i Azure | Microsoft Docs"
description: "Skapa virtuella datorer i Azure med hjälp av virtuella hårddiskar som överförts från andra moln som AWS eller andra virtualiseringsplattformar och dra nytta av Azure hanterade diskar."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 66c3912397ab905aafb3910e13ac711befb8f502
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-amazon-web-services-aws-and-other-platforms-toomanaged-disks-in-azure"></a>Migrera från Amazon Web Services (AWS) och andra plattformar tooManaged diskar i Azure

Du kan överföra VHD-filer från AWS eller lokalt virtualisering lösningar tooAzure toocreate virtuella datorer som utnyttjar hanterade diskar. Azure-hanterade diskar tar bort hello behovet av toomanaging lagringskonton Azure IaaS-VM. Du har tooonly ange hello typ (Premium eller Standard) och storleken på disken som du behöver och Azure ska skapa och hantera hello disk du. 

Du kan överföra generaliserad och särskilda virtuella hårddiskar. 
- **Generaliserad virtuell Hårddisk** -har haft all personlig information bort med hjälp av Sysprep. 
- **Specialiserad VHD** -underhåller hello användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn. 

> [!IMPORTANT]
> Du bör följa innan du laddar upp en VHD-tooAzure [förbereda en Windows-VHD eller VHDX tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
>
>


| Scenario                                                                                                                         | Dokumentation                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Du har en befintlig AWS EC2 instanser kunde som du toomigrate tooAzure hanterade diskar                                     | [Flytta en virtuell dator från Amazon Web Services (AWS) tooAzure](aws-to-azure.md)                           |
| Du har en virtuell dator och andra virtualiseringsplattform som du vill att toouse toouse som en bild toocreate flera virtuella Azure-datorer. | [Överför en generaliserad virtuell Hårddisk och använda toocreate en nya virtuella datorer i Azure](upload-generalized-managed.md) |
| Du har ett unikt anpassade virtuell dator som du vill att toorecreate i Azure.                                                      | [Överför en särskild VHD-tooAzure och skapa en ny virtuell dator](create-vm-specialized.md)         |


## <a name="overview-of-managed-disks"></a>Översikt över hanterade diskar

Azure-hanterade diskar förenklar VM management genom att ta bort hello måste toomanage storage-konton. Hanterade diskar också förmånen från bättre tillförlitlighet för virtuella datorer i en Tillgänglighetsuppsättning. Det garanterar att hello diskar för olika virtuella datorer i en Tillgänglighetsuppsättning kommer att tillräckligt isoleras från alla andra tooavoid enda fel. Diskar för olika virtuella datorer placeras i en Tillgänglighetsuppsättning i olika skalningsenheter (stämplar) som begränsar hello effekten av enskild skala enhet lagringsfel grund förfallodatum toohardware och programvarufel automatiskt. Utifrån dina behov kan välja du mellan två typer av lagringsalternativ: 
 
- [Hanterade Premiumdiskar](../../storage/common/storage-premium-storage.md) är fylld tillstånd enhet (SSD) baserade lagring lagringsmedia som levererar highperformance, låg latens diskstöd för virtuella datorer som körs I/O-intensiva arbetsbelastningar. Du kan dra nytta av hello hastighet och prestanda för dessa diskar genom att migrera tooPremium hanterade diskar.  

- [Hanterade standarddiskar](../../storage/common/storage-standard-storage.md) använda hårddisken (HDD) baserade lagringsmedia och passar bäst för utveckling och testning och andra regelbunden åtkomst arbetsbelastningar som är mindre känsliga tooperformance variationen.  

## <a name="plan-for-hello-migration-toomanaged-disks"></a>Planera för migrering av hello tooManaged diskar

Det här avsnittet hjälper dig att toomake hello bästa beslut på VM- och diskresurser typer.


### <a name="location"></a>Plats

Välj en plats där Azure hanterade diskar är tillgängliga. Om du migrerar tooPremium hanterade diskar kan också se till att Premium-lagring finns i hello region där du planerar toomigrate till. Se [Azure-tjänster efter Region](https://azure.microsoft.com/regions/#services) uppdaterad information om tillgängliga platser.

### <a name="vm-sizes"></a>VM-storlekar

Om du migrerar tooPremium hanterade diskar har tooupdate hello storleken på hello VM tooPremium kompatibla lagringsstorlek tillgängliga i hello-regionen där VM finns. Granska hello VM-storlekar som är Premium-lagring som är kompatibla. hello Azure VM storlek specifikationer listas i [storlekar för virtuella datorer](sizes.md).
Granska hello prestandaegenskaper för virtuella datorer som fungerar med Premium-lagring och välj hello lämpligaste VM-storlek som bäst passar din arbetsbelastning. Kontrollera att det finns tillräckligt med bandbredd på VM toodrive hello disk trafiken.

### <a name="disk-sizes"></a>Diskstorlekar

**Premium hanterade diskar**

Det finns tre typer av Premium hanterade diskar som kan användas med den virtuella datorn och var och en har särskilda IOPs och genomströmning gränser. Överväg att dessa begränsningar när du väljer hello Premium disktyp för den virtuella datorn baserat på hello behoven för ditt program vad gäller kapacitet, prestanda, skalbarhet och belastning läses in.

| Premium diskar typ  | P10               | P20               | P30               |
|---------------------|-------------------|-------------------|-------------------|
| Diskstorlek           | 128 GB            | 512 GB            | 1 024 GB (1 TB)    |
| IOPS per disk       | 500               | 2 300              | 5000              |
| Dataflöde per disk | 100 MB per sekund | 150 MB per sekund | 200 MB per sekund |

**Hanterade standarddiskar**

Det finns fem typer av Standard hanterade diskar som kan användas med den virtuella datorn. Var och en av dem har olika kapacitet men samma IOPS och genomströmning gränser. Välj hello typ av Standard hanterade diskar baserat på hello kapacitetsbehov för programmet.

| Disk av standardtyp  | S4               | S6               | S10              | S20              | S30              |
|---------------------|------------------|------------------|------------------|------------------|------------------|
| Diskstorlek           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1 024 GB (1 TB)   |
| IOPS per disk       | 500              | 500              | 500              | 500              | 500              |
| Dataflöde per disk | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund |

### <a name="disk-caching-policy"></a>Princip för cachelagring av disk 

**Premium hanterade diskar**

Princip för cachelagring av disk är som standard *skrivskyddad* för alla hello Premium datadiskar och *Read-Write* för hello Premium operativsystemdisken kopplade toohello VM. Den här konfigurationen rekommenderas tooachieve hello optimala prestanda för ditt program IOs. Inaktivera cachelagring på disk så att du kan få bättre prestanda för programmet för skrivintensiv eller lässkyddad datadiskar (till exempel loggfiler för SQL Server).

### <a name="pricing"></a>Prissättning

Granska hello [priser för hanterade diskar](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Priser för hanterade Premiumdiskar är samma som hello ohanterade Premiumdiskar. Men priser för hanterade standarddiskar skiljer sig från ohanterade standarddiskar.


## <a name="next-steps"></a>Nästa steg

- Du bör följa innan du laddar upp en VHD-tooAzure [förbereda en Windows-VHD eller VHDX tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
