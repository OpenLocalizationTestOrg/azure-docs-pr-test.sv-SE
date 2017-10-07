---
title: "aaaWhat toodo hello ett avbrott i Azure-tjänsten som påverkar Azure Cloud Services för händelsen | Microsoft Docs"
description: "Lär dig vilka toodo i ett avbrott i Azure-tjänsten som påverkar Azure Cloud Services hello-händelse."
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: e52634ab-003d-4f1e-85fa-794f6cd12ce4
ms.service: cloud-services
ms.workload: cloud-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: mmccrory
ms.openlocfilehash: bb1f1835fc91a23772f81801b67d5786c190ba19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-toodo-in-hello-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Vilka toodo i ett avbrott i Azure-tjänsten som påverkar Azure Cloud Services hello-händelse
Vi arbetar hårda toomake till att våra tjänster är alltid tillgänglig tooyou när du behöver dem på Microsoft. Framtvingar utanför vårt kontroll påverka oss på ett sätt som orsakar avbrott i tjänsten oplanerad ibland.

Microsoft tillhandahåller en serviceavtalet (SLA) för dess tjänster som ett åtagande för drifttid och anslutningar. hello SLA för enskilda Azure-tjänster finns på [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure har redan många inbyggda funktioner som stöder hög tillgänglighet program. Mer information om dessa tjänster läsa [katastrofåterställning och hög tillgänglighet för Azure-program](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Den här artikeln beskriver en true för katastrofåterställning när en hela region påträffar ett avbrott på grund av toomajor naturkatastrof eller omfattande driftstopp. Det är sällsynt förekomster men måste du förbereda för hello möjligheten att det finns ett avbrott för en hel region. Om en hel region får ett avbrott i tjänsten, är hello lokalt redundanta kopior av dina data tillfälligt inte tillgänglig. Om du har aktiverat geo-replikering, lagras tre fler kopior av dina Azure Storage-blobbar och tabeller i en annan region. Hej för händelsen en fullständig regionalt strömavbrott eller en katastrofåterställning i vilka hello primär region inte kan återställas, Azure omvandlar när alla hello DNS-poster toohello georeplikerad region.

> [!NOTE]
> Tänk på att du inte har någon kontroll över den här processen och sker endast för avbrott i datacenter hela tjänsten. Därför måste du också lita på andra programspecifika säkerhetskopieringsstrategier tooachieve hello högsta tillgänglighet. Mer information finns i [katastrofåterställning och hög tillgänglighet för program som bygger på Microsoft Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md). Om du vill ha toobe kan tooaffect egna redundans, som du kanske vill tooconsider hello användning av [geo-redundant lagring med läsbehörighet (RA-GRS)](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage), vilket skapar en skrivskyddad kopia av dina data i en annan region.
>
>


## <a name="option-1-use-a-backup-deployment-through-azure-traffic-manager"></a>Alternativ 1: Använd en säkerhetskopiering distribution via Azure Traffic Manager
hello mest robusta haveriberedskapslösning innebär att underhålla flera distributioner av programmet i olika regioner och sedan använda [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) toodirect trafiken mellan dem. Azure Traffic Manager innehåller flera [routningsmetoder](../traffic-manager/traffic-manager-routing-methods.md), så du kan välja om toomanage dina distributioner med hjälp av en primär eller säkerhetskopiering modell eller toosplit trafik mellan dem.

![NLB Azure Cloud Services över regioner med Azure Traffic Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

Hello snabbaste svar toohello förlust av en region, är det viktigt att du konfigurerar Traffic Manager [slutpunktsövervakning](../traffic-manager/traffic-manager-monitoring.md).

## <a name="option-2-deploy-your-application-tooa-new-region"></a>Alternativ 2: Distribuera ditt program tooa nya region
Underhålla flera aktiva distributioner enligt beskrivningen i föregående alternativ för hello ådrar sig ytterligare kostnader. Om din recovery tid mål för Återställningstid är tillräckligt flexibelt och du har hello ursprungliga kod eller kompilerade molntjänster paketet, kan du skapa en ny instans av ditt program i en annan region uppdatera DNS-poster toopoint toohello nya distributionen

Mer information om hur toocreate och distribuera ett tjänstprogram i molnet, se [hur toocreate och distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).

Du kan behöva toocheck hello återställningsproceduren datakällan programmet beroende på dina program datakällor.

* Azure Storage-datakällor finns i [Azure Storage-replikering](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage) toocheck på hello-alternativ som är tillgängliga beror på hello väljer replikering modellen för ditt program.
* SQL-databas källor, läsa [översikt: molnet business continuity och databasen katastrofåterställning med SQL Database](../sql-database/sql-database-business-continuity.md) toocheck på hello-alternativ som är tillgängliga utifrån hello valt replikeringsmodell för ditt program.


## <a name="option-3-wait-for-recovery"></a>Alternativ 3: Vänta tills återställningen
I så fall krävs ingen åtgärd från dig, men tjänsten blir otillgänglig tills hello region har återställts. Du kan se hello aktuell status för tjänsten på hello [Azure Hälsoinstrumentpanelen](https://azure.microsoft.com/status/).

## <a name="next-steps"></a>Nästa steg
Mer om hur tooimplement en katastrofåterställning och strategi för hög tillgänglighet, se toolearn [katastrofåterställning och hög tillgänglighet för Azure-program](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

toodevelop en detaljerad tekniska förståelse av en molnplattform funktioner finns [Azure återhämtning teknisk vägledning](../resiliency/resiliency-technical-guidance.md).
