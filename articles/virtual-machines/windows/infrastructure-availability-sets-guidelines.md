---
title: "Tillgänglighetsuppsättningar för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Lär dig mer om viktiga riktlinjer för designen och implementeringen för distribution av Tillgänglighetsuppsättningar i Azure infrastrukturtjänster."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f9449f58-664b-4d5d-82f6-84c5d083047f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f94004fbd32dcc8a2394fcdfe2ac61519364f176
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-availability-sets-guidelines-for-windows-vms"></a>Azure tillgänglighet anger riktlinjer för virtuella Windows-datorer

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå nödvändiga planeringsstegen för tillgänglighetsuppsättningar för att se till att ditt program är tillgängliga under planerade eller oplanerade händelser.

## <a name="implementation-guidelines-for-availability-sets"></a>Implementeringsriktlinjer för tillgänglighetsuppsättningar
Beslut:

* Hur många tillgänglighetsuppsättningar behöver du för olika roller och nivåer i ditt program i infrastrukturen?

Aktiviteter:

* Ange hur många virtuella datorer i varje nivå för program som du behöver.
* Bestäm om du behöver justera antalet fel eller uppdatera domäner som ska användas för ditt program.
* Definiera de nödvändiga tillgänglighetsuppsättningar med ditt namngivningskonvention och vad virtuella datorer finns i dem. En virtuell dator kan endast finnas i en tillgänglighetsuppsättning.

## <a name="availability-sets"></a>Tillgänglighetsuppsättningar
I Azure, kan virtuella datorer (VM) placeras i till en logisk gruppering som kallas en tillgänglighetsuppsättning. När du skapar virtuella datorer i en tillgänglighetsuppsättning distribuerar Azure-plattformen placeringen av dessa virtuella datorer i den underliggande infrastrukturen. Det bör finnas en planerad underhållshändelse till Azure-plattformen eller en underliggande maskinvara eller infrastruktur fel, användning av tillgänglighetsuppsättningar garanterar att minst en virtuell dator är igång.

Som bästa praxis bör program inte finns på en enda virtuell dator. En tillgänglighetsuppsättning som innehåller en enda virtuell dator tillgång inte till alla skydd från planerad eller oplanerad händelser i Azure-plattformen. Den [Azure-serviceavtalet](https://azure.microsoft.com/support/legal/sla/virtual-machines) kräver två eller flera virtuella datorer i en tillgänglighetsuppsättning för att tillåta distribution av virtuella datorer i den underliggande infrastrukturen. Om du använder [Azure Premium Storage](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), Azure-serviceavtalet gäller för en enda virtuell dator.

Den underliggande infrastrukturen i Azure är uppdelat i till flera maskinvara kluster. Varje kluster maskinvara har stöd för ett intervall för VM-storlekar. En tillgänglighetsuppsättning kan bara finnas på ett enda maskinvara kluster när som helst i tid. Intervall för VM-storlekar som kan finnas i en enda tillgänglighetsuppsättning är därför begränsade till intervall för VM-storlekar som stöds av maskinvaran klustret. Kluster för maskinvara för tillgänglighetsuppsättningen är markerad när den första virtuella datorn i tillgänglighetsuppsättningen distribueras eller när du startar den första virtuella datorn i en tillgänglighetsuppsättning där alla virtuella datorer är för närvarande i tillståndet stoppad frigjorts. Följande PowerShell-kommando kan användas för att fastställa intervall för VM-storlekar för en tillgänglighetsuppsättning ”: Get-AzureRmVMSize - ResourceGroupName \<sträng\> - AvailabilitySetName \<sträng\>”

Varje maskinvara kluster är uppdelat i flera domäner för uppdatering och feldomäner. Dessa domäner definieras av vilka värdar dela en gemensam uppdateringscykel eller resursen liknande fysisk infrastruktur, till exempel ström och nätverk. Azure distribuerar automatiskt dina virtuella datorer i en tillgänglighetsuppsättning över domäner för att underhålla tillgänglighet och feltolerans. Beroende på storleken på ditt program och antal virtuella datorer i en tillgänglighetsuppsättning, kan du justera antalet domäner som du vill använda. Du kan läsa mer om [hantera tillgänglighet och domäner för uppdatering och feltolerans](manage-availability.md).

Planera programnivåerna som du använder när du designar infrastrukturen för programmet. Grupp virtuella datorer som fungerar på samma sätt i till tillgänglighet anger till exempel en tillgänglighetsuppsättning för dina frontend virtuella datorer som kör IIS. Skapa en separat tillgänglighetsuppsättning för din serverdel virtuella datorer som kör SQL Server. Målet är att säkerställa att varje komponent i ditt program är skyddat av en tillgänglighetsuppsättning och minst en gång instans alltid ska vara igång.

Belastningsutjämnare kan användas framför varje nivå för programmet att fungera tillsammans med en tillgänglighetsuppsättning och se till att trafiken kan alltid dirigeras till en instans som körs. Dina virtuella datorer kan fortsätta att köras under planerade och oplanerat underhållshändelser utan en belastningsutjämnare, men användaren kan inte kunna lösas om den primära virtuella datorn inte är tillgänglig.

Designa programmet för hög tillgänglighet i lagringsskiktet. Det bästa sättet är att [använder hanterade diskar för virtuella datorer i en Tillgänglighetsuppsättning](manage-availability.md#use-managed-disks-for-vms-in-an-availability-set). Om du använder ohanterade diskar, rekommenderar vi att [konvertera virtuella datorer i Tillgänglighetsuppsättningen du använda hanterade diskar](convert-unmanaged-to-managed-disks.md#convert-vms-in-an-availability-set).

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]
