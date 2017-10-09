---
title: aaaData replikering i Azure Storage | Microsoft Docs
description: "Data i ditt Microsoft Azure Storage-konto replikeras för hållbarhet och hög tillgänglighet. Replikeringsalternativ är lokalt redundant lagring (LRS), zonredundant lagring (ZRS), geo-redundant lagring (GRS) och geo-redundant lagring med läsbehörighet (RA-GRS)."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 86bdb6d4-da59-4337-8375-2527b6bdf73f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: 539bc54f57fe8cb661665d2788961a0783b5ae7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-replication"></a>Azure Storage-replikering

hello data i din Microsoft Azure storage-konto är alltid replikeras tooensure hållbarhet och hög tillgänglighet. Replikering kopierar dina data, antingen inom hello samma Datacenter eller tooa andra datacenter, beroende på vilket replikeringsalternativ du väljer. Replikering skyddar dina data och bevara dina program drifttid i hello händelse av tillfälliga maskinvarufel. Om dina data är replikerade tooa andra datacenter, är skyddat från ett oåterkalleligt fel i hello primär plats.

Replikeringen garanterar att ditt lagringskonto uppfyller hello [servicenivåer serviceavtalet (SLA) för lagring](https://azure.microsoft.com/support/legal/sla/storage/) även i hello ansikte fel. Se hello SLA information om Azure Storage garanterar för hållbarhet och tillgänglighet.

När du skapar ett lagringskonto kan välja du något av följande replikeringsalternativ hello:

* [Lokalt redundant lagring (LRS)](#locally-redundant-storage)
* [Zonredundant lagring (ZRS)](#zone-redundant-storage)
* [Geo-redundant lagring (GRS)](#geo-redundant-storage)
* [Geo-redundant lagring med läsbehörighet (RA-GRS)](#read-access-geo-redundant-storage)

Geo-redundant lagring med läsbehörighet (RA-GRS) är hello standardalternativet när du skapar ett lagringskonto.

hello följande tabell ger en snabb överblick över hello skillnaderna mellan LRS-, ZRS-, GRS- och RA-GRS medan efterföljande avsnitt åtgärda varje typ av replikering i detalj.

| Replikeringsstrategi | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Data replikeras mellan flera datacenter. |Nej |Ja |Ja |Ja |
| Data kan läsas från en sekundär plats samt hello primär plats. |Nej |Nej |Nej |Ja |
| Antal kopior av data som finns på olika noder. |3 |3 |6 |6 |

Se [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/) för prisuppgifter för hello olika redundansalternativ.

> [!NOTE]
> Premium-lagring stöder endast lokalt redundant lagring (LRS). Information om Premium-lagring finns [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](storage-premium-storage.md).
>

> [!NOTE]
> Azure File storage stöder endast lokalt redundant lagring (LRS) och georedundant lagring (GRS). Information om Azure File Storage finns [översikt över Azure File storage](storage-files-introduction.md).
>

## <a name="locally-redundant-storage"></a>Lokalt redundant lagring
Lokalt redundant lagring (LRS) replikeras dina data tre gånger i en lagringsenhet för skalan som finns i ett datacenter i hello region där du skapade ditt lagringskonto. En Skriv-begäran returnerar har endast när det har skrivits tooall tre repliker. Dessa tre repliker finns i separata feldomäner och uppgradera domäner inom en lagringsenhet för skalan.

En skalningsenhet för lagring är en samling av rack lagringsnoder. En feldomän (FD) är en uppsättning noder som representerar en fysisk enhet till felet och kan anses noder som tillhör toohello samma fysiska rack. En uppgraderingsdomän (UD) är en uppsättning noder som uppgraderas tillsammans under hello processen för en Serviceuppgradering av (distribution). hello tre repliker sprids över UDs och FDs inom en lagring skala enhet tooensure att data är tillgängliga även om maskinvarufel påverkar ett enda rack eller när noder uppgraderas under en distribution.

LRS är hello lägsta kostnad och ger minst hållbarhet jämfört med tooother alternativ. I hello-händelse en nivå katastrofåterställning för datacenter (brand, överbelasta osv) kanske alla tre repliker förlorade eller oåterkalleligt. toomitigate detta risk, rekommenderas Geo-Redundant lagring (GRS) för de flesta program.

Lokalt redundant lagring kan fortfarande vara önskvärt i vissa scenarier:

* Ger högsta Maximal bandbredd för alternativ för Azure Storage-replikering.
* Om ditt program lagrar data som enkelt kan rekonstrueras, kan du välja LRS.
* Vissa program är begränsat tooreplicating data endast inom ett land på grund av toodata styrningskrav. En parad region kan vara i ett annat land. Läs mer på region par [Azure-regioner](https://azure.microsoft.com/regions/).

## <a name="zone-redundant-storage"></a>Zonredundant lagring
Zonredundant lagring (ZRS) replikerar data asynkront i datacenter i en eller två områden i tillägg toostoring tre repliker liknande tooLRS, vilket ger högre hållbarhet än LRS. Data som lagras i ZRS är beständig även om hello primära datacenter är tillgänglig eller så är oåterkalleligt.
Kunder som planerar toouse ZRS bör vara medveten om som:

* ZRS är endast tillgängligt för blockblobbar i allmänna lagringskonton och fungerar bara i storage-tjänstversioner 2014-02-14 och senare.
* Eftersom asynkron replikering innebär en fördröjning i hello händelse för en lokal katastrofåterställning är det kommer möjligt att ändringar som ännu inte har replikerats toohello sekundära att förloras om hello data inte kan återställas från hello primära.
* hello repliken kanske inte tillgänglig förrän Microsoft initierar redundans toohello sekundär.
* ZRS konton kan inte konverteras senare tooLRS eller GRS. På liknande sätt kan ett befintligt LRS eller GRS-konto kan inte vara konverteras tooa ZRS-konto.
* ZRS konton har inte mått eller loggningsfunktioner.

## <a name="geo-redundant-storage"></a>Geografiskt redundant lagring.
GEO-redundant lagring (GRS) replikeras dina data tooa sekundära region som är hundratals mil bort från hello primär region. Om ditt lagringskonto har GRS aktiverat, sedan dina data skyddas även i hello fallet med ett komplett regionalt strömavbrott eller en katastrofåterställning i vilka hello primär region inte kan återställas.

För ett lagringskonto med GRS aktiverad är en uppdatering första allokerat toohello primära regionen, där de replikeras tre gånger. Sedan hello update replikeras asynkront toohello sekundär region där den också replikeras tre gånger.

Med GRS båda hello primära och sekundära regioner hantera repliker i separata feldomäner och uppgradera domäner inom en skala lagringsenhet enligt med LRS.

Att tänka på:

* Eftersom asynkron replikering innebär en fördröjning i hello-händelse en regionala katastrof som det är möjligt att ändringar som ännu inte har replikerats kommer toohello sekundära regionen att förloras om hello data inte kan återställas från hello primär region.
* hello repliken är inte tillgängligt om inte Microsoft initierar redundans toohello sekundär region. Om Microsoft initierat en sekundär region toohello för växling vid fel kan har du Läs- och skrivbehörighet toothat data när hello växling vid fel har slutförts. Mer information finns [Disaster Recovery vägledning](storage-disaster-recovery-guidance.md). 
* Om ett program tooread från hello sekundär region, bör hello användare aktivera RA-GRS.

När du skapar ett lagringskonto kan du välja hello primär region för hello-kontot. hello sekundär region bestäms utifrån hello primär region och kan inte ändras. hello följande tabell visar hello primära och sekundära region pairings.

| Primär | Sekundär |
| --- | --- |
| Norra centrala USA |Södra centrala USA |
| Södra centrala USA |Norra centrala USA |
| Östra USA |Västra USA |
| Västra USA |Östra USA |
| Östra USA 2 |Centrala USA |
| Centrala USA |Östra USA 2 |
| Norra Europa |Västra Europa |
| Västra Europa |Norra Europa |
| Sydostasien |Östasien |
| Östasien |Sydostasien |
| Östra Kina |Norra Kina |
| Norra Kina |Östra Kina |
| Östra Japan |Västra Japan |
| Västra Japan |Östra Japan |
| Södra Brasilien |Södra centrala USA |
| Östra Australien |Sydöstra Australien |
| Sydöstra Australien |Östra Australien |
| Södra Indien |Centrala Indien |
| Centrala Indien |Södra Indien |
| Västra Indien |Södra Indien |
| Iowa (USA-förvaltad region) |Virginia (USA-förvaltad region) |
| Virginia (USA-förvaltad region) |Texas (USA-förvaltad region) |
| Texas (USA-förvaltad region) |Arizona (USA-förvaltad region) |
| Arizona (USA-förvaltad region) |Texas (USA-förvaltad region) |
| Centrala Kanada |Östra Kanada |
| Östra Kanada |Centrala Kanada |
| Storbritannien, västra |Storbritannien, södra |
| Storbritannien, södra |Storbritannien, västra |
| Centrala Tyskland |Nordöstra Tyskland |
| Nordöstra Tyskland |Centrala Tyskland |
| Västra USA 2 |Västra centrala USA |
| Västra centrala USA |Västra USA 2 |

Uppdaterad information om regioner som stöds av Azure finns [Azure-regioner](https://azure.microsoft.com/regions/).

>[!NOTE]  
> USA Gov Virginia sekundär region är oss Gov Texas. Tidigare värdsystemet oss Gov Virginia oss Gov Iowa som en sekundär region. Storage-konton fortfarande utnyttja oss Gov Iowa som en sekundär region som ska migreras tooUS Gov Texas som ett sekundärt område. 
> 
> 

## <a name="read-access-geo-redundant-storage"></a>Geo-redundant lagring med läsbehörighet
Geo-redundant lagring med läsbehörighet (RA-GRS) maximerar tillgänglighet för ditt lagringskonto med läsbehörighet toohello data i hello sekundär plats, dessutom toohello replikering mellan två regioner som tillhandahålls av GRS.

När du aktiverar läsbehörighet tooyour data i hello sekundär region är dina data tillgängliga på en sekundär slutpunkt i tillägg toohello primär slutpunkt för ditt lagringskonto. sekundär slutpunkt för hello är liknande toohello primära slutpunkt, men lägger till hello suffix `–secondary` toohello kontonamn. Till exempel om din primär slutpunkt för hello Blob-tjänsten är `myaccount.blob.core.windows.net`, sekundära slutpunkten är `myaccount-secondary.blob.core.windows.net`. Hej åtkomstnycklarna för ditt lagringskonto är hello samma för både hello primära och sekundära slutpunkter.

Att tänka på:

* Programmet har toomanage vilken slutpunkt den interagerar med när du använder RA-GRS.
* Eftersom asynkron replikering innebär en fördröjning i hello-händelse en regionala katastrof som det är möjligt att ändringar som ännu inte har replikerats kommer toohello sekundära regionen att förloras om hello data inte kan återställas från hello primär region.
* Om Microsoft initierar redundans toohello sekundär region, kommer du har läs- och skrivbehörighet toothat data när hello växling vid fel har slutförts. Mer information finns [Disaster Recovery vägledning](storage-disaster-recovery-guidance.md). 
* RA-GRS är avsedd för hög tillgänglighet. Riktlinjer för skalbarhet, granska hello [prestanda checklista](storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-hello-geo-replication-type-of-my-storage-account"></a>1. Hur kan jag ändra hello geo-replikeringstyp av mitt lagringskonto?

   Du kan ändra typ av hello geo-replikering för ditt lagringskonto mellan LRS, GRS och RA-GRS med hello [Azure-portalen](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md) eller programmässigt med hjälp av en av våra många lagring-klient Bibliotek. Observera att ZRS-konton inte får vara konverterade LRS eller GRS. På liknande sätt kan ett befintligt LRS eller GRS-konto kan inte vara konverteras tooa ZRS-konto.

<a id="changedowntime"></a>
#### <a name="2-will-there-be-any-down-time-if-i-change-hello-replication-type-of-my-storage-account"></a>2. Kommer det att finnas någon stillestånd om jag ändrar hello replikeringstyp för mitt lagringskonto?

   Nej, det finns inte någon stillestånd.

<a id="changecost"></a>
#### <a name="3-will-there-be-any-additional-cost-if-i-change-hello-replication-type-of-my-storage-account"></a>3. Kommer det att finnas någon extra kostnad ändrar hello replikeringstyp för mitt lagringskonto?

   Ja. Om du ändrar från LRS tooGRS (eller RA-GRS) för ditt lagringskonto, skulle det innebära en extra kostnad för utgående trafik som ingår i kopiera befintliga data från primär plats toohello sekundär plats. När hello inledande data kopieras finns det ingen ytterligare ytterligare utgång kostnad för geo-replikering av hello data från hello primära toosecondary plats. hello-information för bandbredd avgifter kan hittas på hello [priser för Azure Storage sidan](https://azure.microsoft.com/pricing/details/storage/blobs/). Om du ändrar från GRS tooLRS finns utan extra kostnad, men data tas bort från hello sekundär plats.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Hur kan RA-GRS hjälpa mig?
   
   GRS storage tillhandahåller replikering av data från en primär tooa sekundär region hundratals mil bort från hello primära region som. Dina data skyddas i sådant fall även i hello fallet med ett komplett regionalt strömavbrott eller en katastrofåterställning i vilka hello primär region inte kan återställas. RA-GRS lagring inkluderar den här plus hello möjlighet tooread hello data läggs från hello sekundär plats. För vissa idéer om hur tooleverage denna möjlighet finns för[skapar hög tillgängliga program använder RA-GRS lagring](storage-designing-ha-apps-with-ragrs.md). 

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-for-me-toofigure-out-how-long-it-takes-tooreplicate-my-data-from-hello-primary-toohello-secondary-region"></a>5. Finns det ett sätt för mig toofigure ut hur lång tid det tar tooreplicate Mina data från hello primära toohello sekundär region?
   
   Om du använder RA-GRS-lagring kan kontrollera du hello tid för senaste synkronisering för ditt lagringskonto. Tid för senaste synkronisering är ett GMT datum/tid-värde. alla primära skrivningar innan hello tid för senaste synkronisering har skrivits toohello sekundär plats, vilket innebär att de är tillgängliga toobe läsa från hello sekundär plats. Primär skriver efter hello tid för senaste synkronisering kanske eller kanske inte tillgänglig för läsningar ännu. Du kan fråga efter det här värdet med hello [Azure-portalen](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), eller genom programmering med hello REST API eller en av hello Storage, Klientbiblioteken. 

<a id="outage"></a>
#### <a name="6-how-can-i-switch-toohello-secondary-region-if-there-is-an-outage-in-hello-primary-region"></a>6. Hur kan jag byta toohello sekundär region om det finns ett avbrott i hello primär region?
   
   Se artikeln toohello [vilka toodo om ett Azure Storage-avbrott inträffar](storage-disaster-recovery-guidance.md) för mer information.

<a id="rpo-rto"></a>
#### <a name="7-what-is-hello-rpo-and-rto-with-grs"></a>7. Vad är hello Återställningspunktmål och Återställningstidsmål med GRS?
   
   Återställningspunktmål för återställningspunkt (RPO): I GRS och RA-GRS hello lagring tjänsten asynkront geo-replikeras hello data från hello primära toohello sekundär plats. Om det finns en större katastrof för regional och en redundansväxling har toobe utförs kan sedan senaste deltaändringar som inte har georeplikerad gå förlorade. hello antal minuter potentiella data förlorade är refererad tooas hello Återställningspunktmål (vilket innebär hello punkt i tiden toowhich data kan återställas). Vi har vanligtvis ett Återställningspunktmål mindre än 15 minuter, även om det finns för närvarande inga SLA på hur länge geo-replikering tar.

   Återställning tid mål för Återställningstid: Detta är ett mått på hur lång tid det tar toodo hello redundans och komma hello storage-konto online om vi har toodo en växling vid fel. hello tid toodo hello redundans omfattar hello följande:
    * hello tiden det tar oss tooinvestigate och avgöra om vi kan återställa hello data på hello primära platsen eller om vi behöver toodo en växling vid fel.
    * Redundans hello konto genom att ändra hello primär DNS-poster toopoint toohello sekundär plats.

   Vi tar hello ansvar bevara data allvar, så om det finns några risken för att återställa hello data, kommer håller på gör hello redundans och fokusera på att återställa hello data i hello primär plats. I framtida hello, planerar vi tooprovide API tooallow du tootrigger en växling vid fel på en kontonivå, vilket skulle sedan kan du toocontrol hello RTO själv, men detta är inte tillgängligt ännu.
   
## <a name="next-steps"></a>Nästa steg
* [Utforma högtillgänglig program som använder RA-GRS-lagring](storage-designing-ha-apps-with-ragrs.md)
* [Prissättning för Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Om Azure storage-konton](storage-create-storage-account.md)
* [Skalbarhets- och prestandamål i Azure Storage](storage-scalability-targets.md)
* [Microsoft Azure Storage redundans alternativ och läsbehörighet geo-redundant lagring](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP Paper - Azure Storage: En högtillgänglig lagring molntjänst med stark konsekvens](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

