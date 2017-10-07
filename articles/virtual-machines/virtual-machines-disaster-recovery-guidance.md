---
title: "aaaDisaster återställningsscenarier för virtuella datorer i Azure | Microsoft Docs"
description: "Lär dig vilka toodo i hello händelse att ett avbrott i Azure-tjänsten påverkar virtuella Azure-datorer."
services: virtual-machines
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: kmouss;aglick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 839c6f9c51a7f35b48ef3636bc346eef332bb06d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-toodo-in-hello-event-that-an-azure-service-disruption-impacts-azure-vms"></a>Vilka toodo i hello händelse att ett avbrott i Azure-tjänsten påverkar virtuella Azure-datorer
Vi arbetar hårda toomake till att våra tjänster är alltid tillgänglig tooyou när du behöver dem på Microsoft. Framtvingar utanför vårt kontroll påverka oss på ett sätt som orsakar avbrott i tjänsten oplanerad ibland.

Microsoft tillhandahåller en serviceavtalet (SLA) för dess tjänster som ett åtagande för drifttid och anslutningar. hello SLA för enskilda Azure-tjänster finns på [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure har redan många inbyggda funktioner som stöder hög tillgänglighet program. Mer information om dessa tjänster läsa [katastrofåterställning och hög tillgänglighet för Azure-program](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Den här artikeln beskriver en true för katastrofåterställning när en hela region påträffar ett avbrott på grund av toomajor naturkatastrof eller omfattande driftstopp. Det är sällsynt förekomster men måste du förbereda för hello möjligheten att det finns ett avbrott för en hel region. Om en hel region får ett avbrott i tjänsten, är hello lokalt redundanta kopior av dina data tillfälligt inte tillgänglig. Om du har aktiverat geo-replikering, lagras tre fler kopior av dina Azure Storage-blobbar och tabeller i en annan region. Hej för händelsen en fullständig regionalt strömavbrott eller en katastrofåterställning i vilka hello primär region inte kan återställas, Azure omvandlar när alla hello DNS-poster toohello georeplikerad region.

toohelp som du hanterar dessa sällsynta förekomster vi ger hello följande riktlinjer för virtuella datorer i Azure hello gäller störningar i hello hela region där Azure-dator som programmet distribueras.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>Alternativ 1: Initiera redundans med hjälp av Azure Site Recovery
Du kan konfigurera Azure Site Recovery för din virtuella dator så att du kan återställa ditt program med en enda klickning i frågan minuter. Du kan replikera tooAzure region önskat och inte begränsat toopaired regioner. Du kan komma igång med [replikering av virtuella datorer](https://aka.ms/a2a-getting-started). Du kan [skapa en återställningsplan](../site-recovery/site-recovery-create-recovery-plans.md) så att du kan automatisera hello hela failover-processen för ditt program. Du kan [din redundanstestningen](../site-recovery/site-recovery-test-failover-to-azure.md) förhand utan att påverka program eller hello pågående produktionsreplikering. I en primär region avbrott hello-händelse, som du precis [påbörja en växling](../site-recovery/site-recovery-failover.md) och se till att ditt program i målregionen.


## <a name="option-2-wait-for-recovery"></a>Alternativ 2: Vänta tills återställningen
I så fall krävs ingen åtgärd. Vet att det fungerar ordentligt toorestore tjänsttillgänglighet. Du kan se hello aktuell status för tjänsten på vår [Azure Hälsoinstrumentpanelen](https://azure.microsoft.com/status/).

Detta är hello bästa alternativet om du inte har ställt in Azure Site Recovery, geo-redundant lagring med läsbehörighet eller geo-redundant lagring tidigare toohello avbrott. Om du har ställt in geo-redundant lagring eller läsbehörighet geo-redundant lagring för lagringskontot hello där dina VM virtuella hårddiskar (VHD) lagras, kan du leta toorecover hello basavbildning virtuell Hårddisk och försök tooprovision en ny virtuell dator från den. Detta är inte en förvalda alternativet eftersom det finns inga garantier för synkronisering av data. Det här alternativet är därför inte garanterat toowork.


> [!NOTE]
> Tänk på att du inte har någon kontroll över den här processen och sker endast för avbrott i region hela tjänsten. Därför måste du också lita på andra programspecifika säkerhetskopieringsstrategier tooachieve hello högsta tillgänglighet. Mer information finns i avsnittet hello på [Data strategier för katastrofåterställning](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications#data-strategies-for-disaster-recovery).
>
>

## <a name="next-steps"></a>Nästa steg

- Starta [skyddar ditt program som körs på Azure virtual machines](https://aka.ms/a2a-getting-started) med hjälp av Azure Site Recovery

- Mer om hur tooimplement en katastrofåterställning och strategi för hög tillgänglighet, se toolearn [katastrofåterställning och hög tillgänglighet för Azure-program](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

- toodevelop en detaljerad tekniska förståelse av en molnplattform funktioner finns [Azure återhämtning teknisk vägledning](../resiliency/resiliency-technical-guidance.md).


- Om hello instruktioner inte avmarkera, eller om du vill att Microsoft toodo hello åtgärder för din räkning, kontakta [kundsupport](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
