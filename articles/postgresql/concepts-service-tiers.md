---
title: "aaa ”prisnivå i Azure-databas för PostgreSQL”"
description: "Prisnivåer i Azure PostgreSQL-databas"
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: article
ms.date: 05/31/2017
ms.openlocfilehash: f10389dd2ad1ff04e83b02786a407c10140a007b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-options-and-performance-understand-whats-available-in-each-pricing-tier"></a>Azure-databas för PostgreSQL-alternativ och prestanda: förstå vad som är tillgängliga i varje prisnivå
När du skapar en Azure-databas för PostgreSQL-server kan besluta du tre huvudsakliga alternativ tooconfigure hello resurser som allokerats för servern. Dessa alternativ påverka hello prestanda och skalning av hello-server.
- Prisnivå
- Compute-enheter
- Storage (GB)

Varje prisnivå har en uppsättning prestanda nivåer (Compute-enheter) toochoose från, beroende på dina arbetsbelastningar krav. Högre prestandanivåer ange ytterligare resurser för din server utformad toodeliver högre genomströmning. Du kan ändra hello-serverns prestanda nivån i en prisnivå praktiskt taget inga driftavbrott.

> [!IMPORTANT]
> Hello-tjänsten är tillgänglig som förhandsversion, men det finns inte en garanterad serviceavtalet (SLA).

Du kan ha en eller flera databaser i en Azure-databas för PostgreSQL-servern. Du kan välja toocreate en enskild databas per server tooutilize alla hello resurser eller skapa flera databaser tooshare hello resurser. 

## <a name="choose-a-pricing-tier"></a>Välj en prisnivå
Under förhandsgranskning, Azure-databas för PostgreSQL erbjuder två prisnivåer: Basic och Standard. Premium-nivån är inte tillgänglig ännu, men blir snart tillgänglig. 

hello innehåller följande tabell exempel på hello prisnivåer bäst lämpade för olika programbelasningar.

| Prisnivå | Målbelastningar |
| :----------- | :----------------|
| Basic | Bäst för små arbetsbelastningar som kräver skalbara beräkning och lagring utan IOPS garantera. Exempel är servrar som används för utveckling och testning eller småskaliga program som inte används. |
| Standard | Hej gå toooption för molnet program som behöver IOPS garantera med hög genomströmning. Exempel webb- eller analytiska applikationer. |
| Premium | Bäst för arbetsbelastningar som behöver låg latens för transaktioner och IO. Ger bästa hello-stöd för många samtidiga användare. Tillämpliga toodatabases som stöder verksamhetskritiska program.<br />hello Premium-prisnivån är inte tillgängliga i förhandsversionen. |

toodecide på en prisnivå-nivån, första start genom att fastställa om din arbetsbelastning behöver en IOPS-garanti. I så fall använder du Standard som prisnivå.

| **Priser för nivån funktioner** | **Basic** | **Standard** |
| :------------------------ | :-------- | :----------- |
| Maximal beräknings-enheter | 100 | 800 | 
| Maximalt totalt lagringsutrymme | 1 TB | 1 TB | 
| Lagring IOPS garanti | Saknas | Ja | 
| Maximalt lagringsutrymme IOPS | Saknas | 3 000 | 
| Databasen period för lagring av säkerhetskopior. | 7 dagar | 35 dagar | 

Du kan inte ändra prisnivån när hello server har skapats under hello preview tidsintervall. I framtida hello, kommer det vara möjligt tooupgrade eller nedgradera en server från en nivå tooanother prisnivå.

## <a name="understand-hello-price"></a>Förstå hello pris
När du skapar en ny Azure-databas för PostgreSQL inuti hello [Azure Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer), klicka på hello **prisnivå** bladet och hello kostnad varje månad visas baserat på hello-alternativ som du har valt. Om du inte har en Azure-prenumeration kan du använda hello Azure prisnivå Kalkylatorn tooget ett beräknat pris. Besök hello [Azure prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/) webbplats, klicka på **lägga till objekt**, expandera hello **databaser** kategori, och välj **Azure-databas för PostgreSQL** toocustomize hello alternativ.

## <a name="choose-a-performance-level-compute-units"></a>Välj prestandanivå (Compute-enheter)
När du har bestämt hello prisnivån för din Azure-databas för PostgreSQL-server är klar toodetermine hello prestandanivå genom att välja hello antal Compute behövs. En bra utgångspunkt är 200 eller 400 Compute-enheter för program som kräver högre samtidig användning för webb- eller analytiska arbetsbelastningar och justera inkrementellt efter behov. 

Compute-enheter som är ett mått på CPU bearbetning dataflödet som garanterat toobe tillgängliga tooa enkel Azure-databas för PostgreSQL-servern. En Compute-enhet är en blandning av processor-och minnesresurser.  Mer information finns i [förklarar Compute-enheter](concepts-compute-unit-and-storage.md)

### <a name="basic-pricing-tier-performance-levels"></a>Grundläggande prisnivå nivå prestandanivåer:

| **Prestandanivå** | **50** | **100** |
| :-------------------- | :----- | :------ |
| Max beräknings-enheter | 50 | 100 |
| Inkluderade lagringsstorlek | 50 GB | 50 GB |
| Maxstorlek för storage server\* | 1 TB | 1 TB |

### <a name="standard-pricing-tier-performance-levels"></a>Standard prisnivå nivå prestandanivåer:

| **Prestandanivå** | **100** | **200** | **400** | **800** |
| :-------------------- | :------ | :------ | :------ | :------ |
| Max beräknings-enheter | 100 | 200 | 400 | 800 |
| Inkluderade lagringsstorlek och etablerade IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS |
| Maxstorlek för storage server\* | 1 TB | 1 TB | 1 TB | 1 TB |
| Konfigurera max servern IOPS | 3 000 IOPS | 3 000 IOPS | 3 000 IOPS | 3 000 IOPS |
| Konfigurera max servern IOPS per GB | Fast 3 IOPS per GB | Fast 3 IOPS per GB | Fast 3 IOPS per GB | Fast 3 IOPS per GB |

\*Maxstorlek server lagring refererar toohello etablerade lagringsstorleken för servern.

## <a name="storage"></a>Lagring 
hello lagringskonfiguration definierar hello mängden lagringsutrymme kapacitet tillgängliga tooan Azure-databas för PostgreSQL-servern. hello lagringsutrymme som används av hello-tjänsten innehåller hello databasfiler och transaktionsloggar hello PostgreSQL i serverloggen. Överväg att hello storleken på lagringsutrymmet toohost databaserna och hello prestandakrav (IOPS) när du väljer hello lagringskonfiguration.

Vissa lagringskapacitet ingår minst med varje prisnivå som anges i föregående tabell som ”ingår lagringsstorlek”. Hej Ytterligare lagringskapacitet kan läggas till när hello server har skapats i steg om 125 GB upp toohello maximalt tillåtna lagringsutrymme. hello ytterligare lagringskapacitet kan konfigureras oberoende av hello Compute enheter konfiguration. hello priset ändras baserat på hello mängden lagringsutrymme som valts.

hello IOPS konfigurationen i varje prestandanivå relaterar toohello prisnivå och hello lagringsstorlek valt. Grundläggande nivån innehåller inte en IOPS-garanti. Inom hello Standard prisnivån hello IOPS skalas proportionellt toomaximum lagringsutrymme i ett fast 3:1-förhållande. hello ingår lagring av 125 GB garantier för 375 etablerats IOPS, var och en med en i/o-storlek på upp too256 KB. Du kan välja ytterligare lagringsutrymme in too1 TB maximalt, tooguarantee 3 000 etablerats IOPS.

Hello mått Övervakningsdiagrammet i hello Azure-portalen eller skriva Azure CLI-kommandon toomeasure hello förbrukningen av lagring och IOPS. Relevanta mått toomonitor är lagringsgräns, lagringsprocent, lagringsutrymme som används och IO-procent.

>[!IMPORTANT]
> Välj hello mängden lagringsutrymme som när hello när hello server skapas i förhandsgranskningen. Ändra hello lagringsutrymme på en befintlig server stöds inte ännu. 

## <a name="scaling-a-server-up-or-down"></a>Skala en server uppåt eller nedåt
Du välja först hello priser tjänstnivå och prestandanivå när du skapar din Azure-databas för PostgreSQL. Senare, du kan skala hello Compute enheter uppåt eller nedåt dynamiskt, inom hello hello samma prisnivån. I hello Azure-portalen, dra hello Compute enheter på hello server priser nivå bladet eller skript genom att följa det här exemplet: [Övervakare och skala en enskild PostgreSQL-server med hjälp av Azure CLI](scripts/sample-scale-server-up-or-down.md)

Skalning hello Compute-enheter sker oberoende av hello lagringsstorleken som du har valt.

Hello bakgrunden, ändra hello prestandanivå för en databas skapar en replik av hello originaldatabasen på hello nya prestandanivå och sedan växlar anslutningar toohello repliken. Inga data går förlorade under den här processen. Under hello kort stund när vi växlar över toohello repliken, inaktiveras anslutningar toohello databas, så vissa transaktioner som rör sig kan återställas. Det här tidsfönstret varierar, men är i genomsnitt är det under 4 sekunder, och i mer än 99% av fallen rör det sig om mindre än 30 sekunder. Om det finns stora antal transaktioner som rör sig vid hello anslutningar för tillfället är inaktiverade, det här fönstret kanske inte längre.

hello hela skalan processen hello varaktighet beror på både hello storlek och prisnivå hello server före och efter hello ändra. Till exempel bör en server som ändrar Compute enheter inom hello Standard prisnivån slutföras inom några minuter. hello nya egenskaper för hello server verkställs inte förrän hello ändringarna är klara.

## <a name="next-steps"></a>Nästa steg
- Mer information om Compute enheter finns i [förklarar Compute-enheter](concepts-compute-unit-and-storage.md)
- Lär dig hur för[Övervakare och skala en enskild PostgreSQL-server med hjälp av Azure CLI](scripts/sample-scale-server-up-or-down.md)
