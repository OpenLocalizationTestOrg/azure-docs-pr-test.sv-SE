---
title: "aaaAvailability anger för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för att distribuera Tillgänglighetsuppsättningar i Azure infrastrukturtjänster."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 24f1d91c-8cc0-4251-bb67-ac4c4c37e8cd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 78e4da5c8fd9eb186cafacb46454444b73a9439c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-availability-sets-guidelines-for-linux-vms"></a>Azure tillgänglighet anger riktlinjer för virtuella Linux-datorer

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på förstå hello krävs planeringssteg för tillgänglighetsuppsättningar tooensure dina program är tillgängligt under planerad eller oplanerad händelser.

## <a name="implementation-guidelines-for-availability-sets"></a>Implementeringsriktlinjer för tillgänglighetsuppsättningar
Beslut:

* Hur många tillgänglighetsuppsättningar behöver du för hello olika roller och nivåer i ditt program i infrastrukturen?

Aktiviteter:

* Definiera hello antal virtuella datorer i varje nivå för program som du behöver.
* Kontrollera om du behöver tooadjust hello antalet fel eller update domäner toobe som används för ditt program.
* Definiera hello krävs tillgänglighetsuppsättningar med ditt namngivningskonvention och vilka virtuella datorer finns i dem. En virtuell dator kan endast finnas i en tillgänglighetsuppsättning. 

## <a name="availability-sets"></a>Tillgänglighetsuppsättningar
I Azure, kan virtuella datorer (VM) placeras i tooa logisk gruppering som kallas en tillgänglighetsuppsättning. När du skapar virtuella datorer i en tillgänglighetsuppsättning distribuerar hello Azure-plattformen hello placeringen av dessa virtuella datorer över hello underliggande infrastruktur. Det bör finnas ett planerat underhåll händelse toohello Azure-plattformen eller en underliggande maskinvara eller infrastruktur fel, hello användning av tillgänglighetsuppsättningar garanterar att minst en virtuell dator är igång.

Som bästa praxis bör program inte finns på en enda virtuell dator. En tillgänglighetsuppsättning som innehåller en enda virtuell dator tillgång inte till alla skydd från planerad eller oplanerad händelser i hello Azure-plattformen. Hej [Azure-serviceavtalet](https://azure.microsoft.com/support/legal/sla/virtual-machines) kräver två eller flera virtuella datorer i en tillgänglighet set tooallow hello fördelning av virtuella datorer över hello underliggande infrastruktur. Om du använder [Azure Premium Storage](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello Azure-serviceavtalet gäller tooa enkel VM.

hello underliggande infrastruktur i Azure är uppdelat i toomultiple maskinvara kluster. Varje kluster maskinvara har stöd för ett intervall för VM-storlekar. En tillgänglighetsuppsättning kan bara finnas på ett enda maskinvara kluster när som helst i tid. Hello intervall för VM-storlekar som kan finnas i en enda tillgänglighetsuppsättning är därför begränsade toohello intervall för VM-storlekar som stöds av hello maskinvara klustret. hello maskinvara kluster för hello tillgänglighetsuppsättning är markerad när hello första virtuella datorn i hello tillgänglighetsuppsättning har distribuerats eller starta hello första virtuella dator i en tillgänglighetsuppsättning där alla virtuella datorer är för närvarande i hello stoppats frigjorts tillstånd. hello följande CLI-kommando kan vara används toodetermine hello intervall för VM-storlekar för en tillgänglighetsuppsättning ”: az lista-storlekar på vm - plats \<sträng\>”

Varje maskinvara kluster är uppdelat i toomultiple uppdatering domäner och feldomäner. Dessa domäner definieras av vilka värdar dela en gemensam uppdateringscykel eller resursen liknande fysisk infrastruktur, till exempel ström och nätverk. Azure distribuerar automatiskt dina virtuella datorer i en tillgänglighetsuppsättning över domäner toomaintain tillgänglighet och feltolerans. Beroende på hello storlek för programmet och hello antal virtuella datorer i en tillgänglighetsuppsättning, kan du justera hello antalet domäner som du vill toouse. Du kan läsa mer om [hantera tillgänglighet och domäner för uppdatering och feltolerans](manage-availability.md).

När du designar infrastrukturen för programmet kan du planera hello programmet nivåer toouse. Grupp virtuella datorer som fungerar hello samma syfte i tooavailability uppsättningar, till exempel en tillgänglighetsuppsättning för dina frontend virtuella datorer som kör nginx eller Apache. Skapa en separat tillgänglighetsuppsättning för din serverdel virtuella datorer som kör MongoDB eller MySQL. hello målet är tooensure att varje komponent i ditt program är skyddat av en tillgänglighetsuppsättning och minst en gång instans alltid ska vara igång.

Belastningsutjämnare kan användas framför varje program nivå toowork tillsammans med en tillgänglighetsuppsättning och kontrollera trafik kan alltid dirigeras tooa kör-instansen. Dina virtuella datorer kan fortsätta att köras under planerade och oplanerat underhållshändelser utan en belastningsutjämnare, men användaren kanske inte kan tooresolve dem om hello primära virtuella datorn är inte tillgänglig.

Designa programmet för hög tillgänglighet i lagringsskiktet. hello bästa praxis är för[använder hanterade diskar för virtuella datorer i en Tillgänglighetsuppsättning](manage-availability.md#use-managed-disks-for-vms-in-an-availability-set). Om du använder ohanterade diskar, rekommenderar vi du för[konvertera virtuella datorer i Tillgänglighetsuppsättningen toouse hanterade diskar](convert-unmanaged-to-managed-disks.md#convert-vms-in-an-availability-set).

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

