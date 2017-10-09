---
title: "aaaMigrate tooManaged diskar för virtuella datorer i Azure | Microsoft Docs"
description: "Migrera Azure virtuella datorer som skapats med hjälp av ohanterade diskar i lagringen konton toouse hanterade diskar."
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
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 29420f13c4ffd5b25726e0ef1aafe89347286a89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-vms-toomanaged-disks-in-azure"></a>Migrera virtuella datorer i Azure tooManaged diskar i Azure

Azure-hanterade diskar förenklas hanteringen lagring genom att ta bort behovet av hello tooseparately hantera storage-konton.  Du kan också migrera dina befintliga virtuella datorer i Azure tooManaged diskar toobenefit från bättre tillförlitlighet för virtuella datorer i en Tillgänglighetsuppsättning. Det garanterar att hello diskar för olika virtuella datorer i en Tillgänglighetsuppsättning kommer att tillräckligt isoleras från alla andra tooavoid enda fel. Diskar för olika virtuella datorer placeras i en Tillgänglighetsuppsättning i olika skalningsenheter (stämplar) som begränsar hello effekten av enskild skala enhet lagringsfel grund förfallodatum toohardware och programvarufel automatiskt.
Utifrån dina behov kan välja du mellan två typer av lagringsalternativ:

- [Hanterade Premiumdiskar](../../storage/common/storage-premium-storage.md) är fylld tillstånd enhet (SSD) baserade lagringsmedia som levererar highperformance, låg latens diskstöd för virtuella datorer som körs I/O-intensiva arbetsbelastningar. Du kan dra nytta av hello hastighet och prestanda för dessa diskar genom att migrera tooPremium hanterade diskar.

- [Hanterade standarddiskar](../../storage/common/storage-standard-storage.md) använda hårddisken (HDD) baserade lagringsmedia och passar bäst för utveckling och testning och andra regelbunden åtkomst arbetsbelastningar som är mindre känsliga tooperformance variationen.

Du kan migrera tooManaged diskar i följande scenarier:

| Migrera...                                            | Länk till dokumentation                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Konvertera fristående virtuella datorer och virtuella datorer i en tillgänglighet set toomanaged diskar   | [Konvertera virtuella datorer toouse hanterade diskar](convert-unmanaged-to-managed-disks.md) |
| En enda virtuell dator från klassiska tooResource Manager på hanterade diskar     | [Migrera en enda virtuell dator](migrate-single-classic-to-resource-manager.md)  | 
| Alla hello virtuella datorer i ett virtuellt nätverk från klassiska tooResource Manager på hanterade diskar     | [Migrera IaaS-resurser från klassiska tooResource Manager](migration-classic-resource-manager-ps.md) och sedan [konvertera en virtuell dator från ohanterade diskar toomanaged diskar](convert-unmanaged-to-managed-disks.md) | 






## <a name="plan-for-hello-conversion-toomanaged-disks"></a>Planera för hello konvertering tooManaged diskar

Det här avsnittet hjälper dig att toomake hello bästa beslut på VM- och diskresurser typer.


## <a name="location"></a>Plats

Välj en plats där Azure hanterade diskar är tillgängliga. Om du flyttar tooPremium hanterade diskar måste också se till att Premium-lagring finns i hello region där du planerar toomove till. Se [Azure-tjänster efter Region](https://azure.microsoft.com/regions/#services) uppdaterad information om tillgängliga platser.

## <a name="vm-sizes"></a>VM-storlekar

Om du migrerar tooPremium hanterade diskar har tooupdate hello storleken på hello VM tooPremium kompatibla lagringsstorlek tillgängliga i hello-regionen där VM finns. Granska hello VM-storlekar som är Premium-lagring som är kompatibla. hello Azure VM storlek specifikationer listas i [storlekar för virtuella datorer](sizes.md).
Granska hello prestandaegenskaper för virtuella datorer som fungerar med Premium-lagring och välj hello lämpligaste VM-storlek som bäst passar din arbetsbelastning. Kontrollera att det finns tillräckligt med bandbredd på VM toodrive hello disk trafiken.

## <a name="disk-sizes"></a>Diskstorlekar

**Premium hanterade diskar**

Det finns sju typer av hanterade premiumdiskar som kan användas med den virtuella datorn var och en har särskilda IOPs och genomströmning gränser. Ta hänsyn till dessa begränsningar när du väljer hello Premium disktyp för den virtuella datorn baserat på hello behoven för ditt program vad gäller kapacitet, prestanda, skalbarhet och belastning läses in.

| Premium diskar typ  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Diskstorlek           | 128 GB| 512 GB| 128 GB| 512 GB            | 1 024 GB (1 TB)    | 2 048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS per disk       | 120   | 240   | 500   | 2 300              | 5000              | 7500              | 7500              | 
| Dataflöde per disk | 25 MB per sekund  | 50 MB per sekund  | 100 MB per sekund | 150 MB per sekund | 200 MB per sekund | 250 MB per sekund | 250 MB per sekund |

**Hanterade standarddiskar**

Det finns sju typer av hanterade standarddiskar som kan användas med den virtuella datorn. Var och en av dem har olika kapacitet men samma IOPS och genomströmning gränser. Välj hello typ av Standard hanterade diskar baserat på hello kapacitetsbehov för programmet.

| Disk av standardtyp  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Diskstorlek           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1 024 GB (1 TB)   | 2 048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOPS per disk       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Dataflöde per disk | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 60 MB per sekund | 

## <a name="disk-caching-policy"></a>Princip för cachelagring av disk

**Premium hanterade diskar**

Princip för cachelagring av disk är som standard *skrivskyddad* för alla hello Premium datadiskar och *Read-Write* för hello Premium operativsystemdisken kopplade toohello VM. Den här konfigurationen rekommenderas tooachieve hello optimala prestanda för ditt program IOs. Inaktivera cachelagring på disk så att du kan få bättre prestanda för programmet för skrivintensiv eller lässkyddad datadiskar (till exempel loggfiler för SQL Server).

## <a name="pricing"></a>Prissättning

Granska hello [priser för hanterade diskar](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Priser för hanterade Premiumdiskar är samma som hello ohanterade Premiumdiskar. Men priser för hanterade standarddiskar skiljer sig från ohanterade standarddiskar.



## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [hanterade diskar](managed-disks-overview.md)
