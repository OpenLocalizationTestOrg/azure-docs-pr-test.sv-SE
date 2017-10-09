---
title: "aaaDesign överväganden för Azure Virtual Machine-Skalningsuppsättningar | Microsoft Docs"
description: "Lär dig mer om designöverväganden för din Azure Virtual Machine-Skalningsuppsättningar"
keywords: Anger om Linux-dator, virtuella datorn
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: c27c6a59-a0ab-4117-a01b-42b049464ca1
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: f8644d36fe5903bd4b74df26dca5dc3211ee3516
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-considerations-for-scale-sets"></a>Designöverväganden för Skalningsuppsättningar
Det här avsnittet beskrivs överväganden vid utformning för Skalningsuppsättningar i virtuella datorer. Information om vilka virtuella datorer är finns för[översikt över virtuella datorer skala anger](virtual-machine-scale-sets-overview.md).

## <a name="when-toouse-scale-sets-instead-of-virtual-machines"></a>När toouse skalningsuppsättningarna i stället för virtuella datorer?
I allmänhet är skaluppsättningar användbara för att distribuera hög tillgänglighet infrastruktur där en uppsättning datorer har liknande konfiguration. Vissa funktioner är dock endast tillgängliga i skalningsuppsättningar medan andra funktioner är bara tillgängliga i virtuella datorer. Ordna toomake ett välgrundat beslut om när toouse varje teknik vi måste först ta en titt på några av hello vanligaste funktionerna som är tillgängliga i skalningsuppsättningar men inte virtuella datorer:

### <a name="scale-set-specific-features"></a>Scale set-specifika funktioner

- När du anger hello skala ange konfiguration, kan du uppdatera hello ”kapacitet” egenskapen toodeploy flera virtuella datorer parallellt. Detta är mycket enklare än att skriva ett skript tooorchestrate distribuera många enskilda virtuella datorer parallellt.
- Du kan [Använd Azure Autoskala tooautomatically skala en skalningsuppsättning](./virtual-machine-scale-sets-autoscale-overview.md) men inte enskilda virtuella datorer.
- Du kan [avbildningsåterställning skaluppsättning för virtuella datorer](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-a-vm) men [inte enskilda virtuella datorer](https://docs.microsoft.com/rest/api/compute/virtualmachines).
- Du kan [overprovision](./virtual-machine-scale-sets-design-overview.md) skaluppsättning för virtuella datorer för ökad tillförlitlighet och snabbare distributionstider. Du kan göra detta med enskilda virtuella datorer om du skriver anpassade kod toodo.
- Du kan ange en [uppgradera princip](./virtual-machine-scale-sets-upgrade-scale-set.md) toomake den enkelt tooroll ut uppgraderingar över virtuella datorer i en skaluppsättning. Med enskilda virtuella datorer, måste du samordnar uppdateringar själv.

### <a name="vm-specific-features"></a>VM-specifika funktioner

Hej på andra sidan, vissa funktioner är endast tillgängliga i virtuella datorer (minst för närvarande för hello):

- Du kan bifoga data diskar toospecific enskilda virtuella datorer, men bifogade datadiskar har konfigurerats för alla virtuella datorer i en skaluppsättning.
- Du kan koppla icke-tom data diskar tooindividual virtuella datorer men inte virtuella datorer i en skaluppsättning.
- Du kan ögonblicksbilder för en enskild VM men inte en virtuell dator i en skaluppsättning.
- Du kan göra en avbildning från en enskild VM men inte från en virtuell dator i en skaluppsättning.
- Du kan migrera en enskild VM från interna diskar toomanaged diskar, men du kan inte göra detta för virtuella datorer i en skaluppsättning.
- Du kan tilldela IPv6 offentliga IP-adresser tooindividual Virtuella nätverkskort, men inte för virtuella datorer i en skaluppsättning. Observera att du kan tilldela IPv6 offentliga IP-adresser tooload belastningsutjämnare framför antingen enskilda virtuella datorer eller skaluppsättning för virtuella datorer.

## <a name="storage"></a>Lagring

### <a name="scale-sets-with-azure-managed-disks"></a>Med Azure hanterade diskar
Skaluppsättningar kan skapas med [Azure hanterade diskar](../virtual-machines/windows/managed-disks-overview.md) i stället för traditionella Azure storage-konton. Hanterade diskar har hello följande fördelar:
- Du har inte toopre-skapa en uppsättning Azure storage-konton för hello skaluppsättning för virtuella datorer.
- Du kan definiera [anslutna datadiskar](virtual-machine-scale-sets-attached-disks.md) hello virtuella datorer i din skala anges.
- Skaluppsättningar kan konfigureras för[stöder in too1 000 virtuella datorer i en mängd](virtual-machine-scale-sets-placement-groups.md). 

Om du har en befintlig mall kan du också [uppdatera hello mallen toouse hanterade diskar](virtual-machine-scale-sets-convert-template-to-md.md).

### <a name="user-managed-storage"></a>Användarhanterat lagring
En skalningsuppsättning som inte är definierad med Azure hanterade diskar är beroende av användarskapade konton toostore hello OS diskar med lagringsutrymme för virtuella datorer hello i hello set. Förhållandet 20 virtuella datorer per lagringskonto eller mindre rekommenderas tooachieve maximala i/o och dra nytta av _överetablering_ (se nedan). Vi rekommenderar också att du sprids hello början tecknen i hello lagringskontonamn hello alfabetet. Om du gör det hjälper till att sprida belastningen över olika interna system. 


## <a name="overprovisioning"></a>Överetablering
Skalningsuppsättningarna för närvarande standard för ”överetablering” virtuella datorer. Med överetablering aktiverat hello skala ange faktiskt varv upp flera virtuella datorer än du uppge och tar bort hello extra VM: ar när hello begärt antal virtuella datorer har etablerats. Överetablering förbättrar etablering slutförandefrekvenser och minskar distributionstiden för. Du debiteras inte för hello extra VM: ar, och de inte räknas in i din kvotgränser.

Överetablering förbättra etablering slutförandefrekvenser, kan orsaka förvirrande beteendet för ett program som är inte utformad toohandle extra VM: ar som visas och sedan försvinner. tooturn överetablering, kontrollera att du har hello efter strängen i mallen: `"overprovision": "false"`. Mer information finns i hello [skala ange REST API-dokumentation](/rest/api/virtualmachinescalesets/create-or-update-a-set).

Om du inaktiverar överetablering din skaluppsättning använder användarhanterat lagring, du kan ha fler än 20 virtuella datorer per lagringskonto, men toogo ovan 40 IO av prestandaskäl rekommenderas inte. 

## <a name="limits"></a>Begränsningar
En skalningsuppsättning bygger på Marketplace-avbildning (även kallat en plattformsavbildning) och konfigurerat toouse Azure hanterade diskar stöder en kapacitet på upp too1 000 virtuella datorer. Om du konfigurerar din scale set toosupport fler än 100 virtuella datorer, hello samma i alla scenarier arbete (till exempel belastningsutjämning). Mer information finns i [arbeta med stora virtuella datorer](virtual-machine-scale-sets-placement-groups.md). 

En skaluppsättningen som är konfigurerad med användarhanterat storage-konton är för närvarande begränsad too100 virtuella datorer (och rekommenderas för skalans 5 storage-konton).

En skalningsuppsättning som bygger på en anpassad avbildning (en som skapats av du) kan ha en kapacitet på upp too100 virtuella datorer när konfigurerats med Azure hanterade diskar. Om hello skaluppsättning är konfigurerad med användarhanterat storage-konton, måste det skapa alla OS-disk VHD i ett lagringskonto. Därför hello maximalt bör antalet virtuella datorer i en skaluppsättning som bygger på en anpassad avbildning och Användarhanterad lagring är 20. Om du inaktiverar överetablering går du in too40.

För flera virtuella datorer än dessa gränser tillåter du behöver toodeploy flera skalningsuppsättningarna enligt [mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).

