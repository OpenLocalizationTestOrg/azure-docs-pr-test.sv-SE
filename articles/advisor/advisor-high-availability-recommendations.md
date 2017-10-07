---
title: "aaaAzure hög tillgänglighet för Advisor-rekommendationer | Microsoft Docs"
description: "Använd Azure Advisor tooimprove hög tillgänglighet för din Azure-distributioner."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 3ac75ce401271f0212d198d7a7dc75ab702b6eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-high-availability-recommendations"></a>Advisor-rekommendationer för hög tillgänglighet

Azure Advisor hjälper dig att kontrollera och förbättra hello kontinuiteten i dina verksamhetskritiska program. Du kan få rekommendationer för hög tillgänglighet av Advisor från hello **hög tillgänglighet** fliken hello Advisor instrumentpanelen.

![Hög tillgänglighet knappen hello Advisor instrumentpanelen](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a>Se till att virtuella feltolerans

Advisor identifierar virtuella datorer som inte är en del av en tillgänglighetsuppsättning och rekommenderar flytta dem till en tillgänglighetsuppsättning. För att ge redundans tooyour program, rekommenderar vi att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning. Den här konfigurationen garanterar att under antingen en planerad eller oplanerad underhållshändelse, minst en virtuell dator är tillgänglig och uppfyller hello Azure virtuella SLA. Du kan välja antingen toocreate en tillgänglighetsuppsättning för hello virtuell dator eller tooadd hello virtuella tooan befintlig tillgänglighetsuppsättning.

> [!NOTE]
> Om du väljer toocreate tillgänglighet, du måste lägga till minst en mer virtuella datorn till den. Vi rekommenderar att du grupperar två eller flera virtuella datorer i en tillgänglighetsgrupp anger tooensure som minst en dator som är tillgänglig under ett avbrott.

![Advisor-rekommendationer: Använd tillgänglighetsuppsättningar för redundans för virtuell dator,](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a>Kontrollera tillgänglighet feltolerans 

Advisor identifierar tillgänglighetsuppsättningar som innehåller en enda virtuell dator och att du lägger till en eller flera virtuella datorer tooit. För att ge redundans tooyour program, rekommenderar vi att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning. Den här konfigurationen garanterar att under antingen en planerad eller oplanerad underhållshändelse, minst en virtuell dator är tillgänglig och uppfyller hello Azure virtuella SLA. Du kan välja antingen toocreate en virtuell dator eller toouse en befintlig virtuell dator och tooadd den toohello tillgänglighetsuppsättning.  

![Advisor-rekommendationer: Lägg till en eller flera virtuella datorer toothis tillgänglighetsuppsättning](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a>Se till att programmet gateway feltolerans
tooensure hello affärskontinuitet av verksamhetskritiska program som tillhandahålls av programgatewayer, Advisor identifierar programmet gateway-instanser som inte är konfigurerade för feltolerans och förslag åtgärder som du kan vidta . Advisor identifierar medelstora eller stora enkelinstansprogram gateways och rekommenderar att lägga till minst en mer instansen. Den identifierar en eller flera instance små programgatewayer och rekommenderar migrera toomedium eller stora SKU: er. Advisor rekommenderar dessa åtgärder tooensure att ditt program gateway-instanserna är konfigurerade toosatisfy hello SLA krav som ställs för dessa resurser.

![Advisor-rekommendationer: distribuera minst två medelstora eller stora storlek gateway programinstanser](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-hello-performance-and-reliability-of-virtual-machine-disks"></a>Förbättra hello prestanda och tillförlitlighet för virtuella diskar

Advisor identifierar virtuella datorer med standarddiskar och rekommenderar att du uppgraderar toopremium diskar.
 
Azure Premium Storage ger stöd för virtuella datorer som körs I/O-intensiva arbetsbelastningar diskar med hög prestanda, låg latens. Virtuella diskar som använder premiumlagringskonton data som lagras på SSD-enheter (SSD). För hello bästa prestanda för ditt program rekommenderar vi att du migrerar alla virtuella diskar som kräver hög IOPS toopremium lagring. 

Om diskarna inte behöver höga IOPS, kan du begränsa kostnader genom att hålla dem i standardlagring. Standardlagring lagrar data för virtuell disk på hårddiskar (HDD) i stället för SSD-enheter. Du kan välja toomigrate virtuella diskar toopremium diskar. Premiumdiskar stöds i de flesta virtuella SKU: er. I vissa fall, om du vill toouse premiumdiskar kan du dock behöva tooupgrade din virtuella dator SKU: er samt.

![Advisor-rekommendationer: förbättra hello tillförlitlighet för virtuella diskar genom att uppgradera toopremium diskar](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Skydda dina data för virtuell dator tas bort av misstag
Advisor identifierar virtuella datorer där säkerhetskopiering inte har aktiverats och rekommenderar att aktivera säkerhetskopiering. Konfigurera säkerhetskopiering av virtuella datorer garanterar hello tillgängligheten för affärskritiska data och ger skydd mot oavsiktlig borttagning eller skadade data.

![Advisor-rekommendationer: Konfigurera virtuella säkerhetskopiering tooprotect dina kritiska data](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a>Rekommendationer för åtkomst med hög tillgänglighet i Advisor

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Hello vänster klickar du på **fler tjänster**.

3. I hello tjänsten menyn rutan under **övervakning och hantering av**, klickar du på **Azure Advisor**.  
 hello Advisor instrumentpanelen visas.

4. Klicka på hello Advisor instrumentpanelen hello **hög tillgänglighet** och välj hello prenumeration som du vill tooreceive rekommendationer.

> [!NOTE]
> tooaccess Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor. En prenumeration registreras när en *prenumeration ägare* startar hello Advisor instrumentpanelen och klickar på hello **få rekommendationer** knappen. Det här är en *engångsåtgärd*. När hello prenumeration har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.

## <a name="next-steps"></a>Nästa steg

Mer information om Advisor-rekommendationer finns:
* [Introduktion tooAzure Advisor](advisor-overview.md)
* [Kom igång med Advisor](advisor-get-started.md)
* [Kostnad Advisor-rekommendationer](advisor-performance-recommendations.md)
* [Advisor-rekommendationer](advisor-performance-recommendations.md)
* [Advisor säkerhetsrekommendationer](advisor-security-recommendations.md)

