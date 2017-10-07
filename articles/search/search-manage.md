---
title: "aaaService administration för Azure Search i hello Azure-portalen"
description: "Hantera Azure Search värdbaserade moln search-tjänsten på Microsoft Azure med hello Azure-portalen."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: c87d1fdd-b3b8-4702-a753-6d7e29dbe0a2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/18/2017
ms.author: heidist
ms.openlocfilehash: 9bb33660d93e068e0f35b856cba0a41c92623644
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-administration-for-azure-search-in-hello-azure-portal"></a>Administration av tjänsten för Azure Search i hello Azure-portalen
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Search är en helt hanterad, molnbaserad search-tjänst som används för att skapa en miljö med omfattande Sök i anpassade appar. Den här artikeln beskriver hello *tjänsten administration* uppgifter som du kan utföra i hello [Azure-portalen](https://portal.azure.com) för en söktjänst som du redan har etablerats. *Tjänsten administration* är förenklad avsiktligt begränsad toohello följande uppgifter:

* Hantera och skydda åtkomst toohello *api-nycklar* används för Läs- eller skrivbehörighet tooyour-tjänsten.
* Justera kapacitet tjänster genom att ändra hello allokering av partitioner och repliker.
* Övervaka Resursanvändning, relativa toomaximum gränserna för din tjänstnivån.

**Inte i omfånget** 

*Innehållshantering* (eller index management) refererar toooperations, till exempel analysera Sök trafikvolym toounderstand fråga kan identifiera vilket villkor personer Sök efter och hur lyckad sökning resultat finns i steg kunder toospecific dokument i ditt index. Hjälp i det här området finns [Sök trafik Analytics för Azure Search](search-traffic-analytics.md).

*Fråga prestanda* är också utanför hello i den här artikeln. Mer information finns i [övervaka användnings- och](search-monitor-usage.md) och [prestanda och optimering](search-performance-optimization.md).

*Uppgradera* är inte en administrativ aktivitet. Eftersom resurser tilldelas när hello service etableras, kräver flytta tooa annat skikt en ny tjänst. Mer information finns i [skapa en Azure Search-tjänst](search-create-service-portal.md).

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Administratörsbehörighet
Etablera eller inaktivering av själva hello-tjänsten kan göras av en administratör för Azure-prenumeration eller en medadministratör.

Inom en tjänst har vem som helst med åtkomst toohello tjänst-URL och en admin api-nyckel läs-/ skrivåtkomst toohello service. Läs-/ skrivåtkomst innehåller hello möjlighet tooadd, ta bort eller ändra serverobjekt, inklusive api-nycklar, index, indexerare, datakällor, scheman och rolltilldelningar som implementeras via [RBAC användardefinierade roller](#rbac).

Alla användarinteraktion med Azure Search hamnar i ett läge: skrivskyddad åtkomst toohello-tjänsten (administratörsbehörighet) eller skrivskyddad åtkomst toohello tjänsten (frågan rättigheter). Mer information finns i [hantera hello api-nycklar](#manage-keys).

<a id="sys-info"></a>

## <a name="set-rbac-roles-for-administrative-access"></a>Ange RBAC-roller för administrativ åtkomst
Azure tillhandahåller en [globala rollbaserad auktoriseringsmodellen](../active-directory/role-based-access-control-configure.md) för alla tjänster som hanteras via hello-portalen eller Resource Manager API: er. Ägare, deltagare och läsare roller fastställa hello nivå för tjänsten administration för Active Directory-användare, grupper och säkerhetsroll säkerhetsobjekt tilldelade tooeach. 

RBAC behörigheter avgör hello följande administrativa uppgifter för Azure Search:

| Roll | Aktivitet |
| --- | --- |
| Ägare |Skapa eller ta bort hello tjänst eller ett objekt på hello-tjänsten, inklusive api-nycklar, index, indexerare, indexeraren datakällor och indexeraren scheman.<p>Visa status för tjänsten, inklusive antalet och lagringsstorlek.<p>Lägg till eller ta bort rollmedlemskap (endast en ägare kan hantera medlemskap).<p>Prenumerationsadministratörer och tjänstens ägare är automatisk medlemmar i hello ägarrollen. |
| Deltagare |Samma åtkomstnivå som ägare, minus RBAC rollhantering. Till exempel en deltagare kan visa och återskapa `api-key`, men det går inte att ändra medlemskap i roller. |
| Läsare |Visa tjänstens status och fråga nycklar. Medlemmar i den här rollen kan inte ändra tjänstkonfigurationen eller visa admin nycklar. |

Roller beviljar inte åtkomst rättigheter toohello tjänstslutpunkten. Sök tjänståtgärder, till exempel indexhantering, ifyllning av indexet och frågor på sökdata styrs via api-nycklar, inte roller. Mer information finns i ”tillstånd för hantering jämfört med dataåtgärder” i [vad är rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md).

<a id="secure-keys"></a>
## <a name="logging-and-system-information"></a>Loggning och Systeminformation
Azure Search visar inte gränssnittshändelsen loggfiler för en enskild tjänst hello-portalen eller programgränssnitt. Hello grundläggande nivån och högre övervakar Microsoft Azure Search-tjänster för 99,9% tillgänglighet per servicenivåavtal (SLA). Om hello-tjänsten är långsam eller dataflödet för begäran understiger SLA tröskelvärden, granska supportteam hello log-filer tillgänglig toothem och adress hello problemet.

Vad gäller allmän information om tjänsten, kan du hämta informationen i hello följande sätt:

* I hello-portalen på hello service instrumentpanelen via meddelanden, egenskaper och statusmeddelanden.
* Med hjälp av [PowerShell](search-manage-powershell.md) eller hello [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/) för[hämta tjänstegenskaper](https://docs.microsoft.com/rest/api/searchmanagement/services), eller status på index Resursanvändning.
* Via [söka trafik analytics](search-traffic-analytics.md), som nämndes tidigare.

<a id="manage-keys"></a>

## <a name="manage-api-keys"></a>Hantera api-nycklar
Alla begäranden tooa search-tjänsten måste api-nyckel som har skapats specifikt för din tjänst. Den här api-nyckel är hello enda mekanism för att autentisera åtkomst tooyour Sök tjänstslutpunkten. 

En api-nyckel är en sträng som består av slumpmässigt genererat siffror och bokstäver. Via [RBAC behörigheter](#rbac), kan du ta bort eller läsa hello nycklar, men du kan inte ersätta en nyckel med ett användardefinierat lösenord. 

Två typer av nycklar är används tooaccess search-tjänsten:

* Admin (gäller för alla läsa / skriva-åtgärd mot hello-tjänsten)
* Fråga (gäller för skrivskyddade åtgärder, till exempel frågor mot ett index)

En admin api-nyckel skapas när hello-tjänsten har etablerats. Det finns två admin-nycklar, utses *primära* och *sekundära* tookeep dem direkt, men i själva verket de är utbytbara. Varje tjänst har två admin-nycklar så att rulla en över utan att förlora tooyour-tjänsten för dataåtkomst. Du kan återskapa antingen administrationsnyckeln, men du kan inte lägga till toohello totala admin antal. Det finns högst två admin nycklar per söktjänst.

Frågan nycklar är utformade för klientprogram som anropar Sök direkt. Du kan skapa upp too50 frågan nycklar. I programkoden anger du hello Sök URL och en fråga api-nyckel tooallow läsbehörighet toohello tjänst. Programkoden anger också hello index som används av ditt program. Tillsammans, hello slutpunkt, en api-nyckel för skrivskyddad åtkomst och ett mål index definiera hello scope och åtkomstnivån för hello anslutningen från klientprogrammet.

tooget eller generera api-nycklar, öppna hello service instrumentpanelen. Klicka på **nycklar** tooslide öppna hello nyckelhantering sida. Kommandon för att återskapa eller skapa nycklar är hello överst på hello sidan. Som standard skapas endast admin nycklar. Frågan api-nycklar måste skapas manuellt.

 ![][9]

<a id="rbac"></a>

## <a name="secure-api-keys"></a>Säkra api-nycklar
Nyckelsäkerhet säkerställs genom att begränsa åtkomst via hello-portalen eller Resource Manager-gränssnitt (kommandoradsgränssnittet eller PowerShell). Som nämnts, kan prenumerationsadministratörer visa och återskapa alla api-nycklar. Granska rollen tilldelningar toounderstand som har snabbtangenter toohello admin som en säkerhetsåtgärd.

1. Klicka på hello åtkomst ikonen tooslide-öppna hello bladet användare i hello service instrumentpanelen.
   ![][7]
2. Granska befintliga rolltilldelningar i användare. Som förväntat, har prenumerationsadministratörer redan fullständig åtkomst toohello via hello ägarrollen.
3. Dessutom toodrill klickar du på **prenumerationsadministratörer** och expandera sedan hello rollen tilldelning listan toosee som har administrationsbehörighet för samtidigt på din söktjänst.

Ett annat sätt tooview behörigheter är tooclick **roller** på bladet för hello-användare. Detta visar tillgängliga roller och hello antal användare eller grupper tilldelade tooeach roll.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Övervaka Resursanvändning
I instrumentpanelen för hello är Resursövervakning begränsad toohello informationen som visas i hello service instrumentpanel och några mått som du kan hämta genom att fråga hello-tjänsten. På hello service instrumentpanelen under hello användning Se du snabbt om partitionen resurs nivåer är lämpliga för ditt program.

Du kan hämta ett antal på dokument och index med hello Söktjänsts-API. Det finns gränser som är associerade med detta antal utifrån hello prisnivån. Mer information finns i [söka tjänstbegränsningarna](search-limits-quotas-capacity.md). 

* [Få indexstatistik](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Antal dokument](https://docs.microsoft.com/rest/api/searchservice/count-documents)

> [!NOTE]
> Cachelagring beteenden kan tillfälligt overstate en gräns. Till exempel när använder hello delad tjänst kan du kan se ett dokument antal över hello hårda gränsen på 10 000 dokument. hello överskattning är temporär och upptäcks på hello nästa tvingande utförs. 
> 
> 

## <a name="disaster-recovery-and-service-outages"></a>Disaster recovery och tjänsten avbrott

Vi kan rädda dina data, tillhandahåller Azure Search ingen instant redundans för hello-tjänsten om det finns ett avbrott på hello klustret eller data center. Om ett kluster misslyckas i hello Datacenter hello driftteamet identifierar och fungerar toorestore service. Du får driftstopp under återställning av tjänsten. Du kan begära service krediter toocompensate för tjänsten per hello [serviceavtalet (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Om servicen krävs i hello händelse katastrofalt fel utanför Microsofts kontroll kunde [etablera en ytterligare tjänst](search-create-service-portal.md) i en annan region och implementera en geo-replikering strategi tooensure index är fullständigt redundanta för alla tjänster.

Kunder som använder [indexerare](search-indexer-overview.md) toopopulate och uppdatera index kan hantera katastrofåterställning via geo-specifika indexerare utnyttja hello samma datakälla. Två tjänster i olika regioner, kör en indexerare kan indexera från hello samma datakälla tooachieve geo-redundans. Om du indexerar från datakällor som också geo-redundant, Tänk på att Azure Search indexerare kan bara utföra inkrementell indexering från primära repliker. I en redundansväxling måste vara att toore punkt hello indexeraren toohello primära repliken. 

Om du inte använder indexerare, använder du din kod toopush objekt och data toodifferent Sök programtjänster parallellt. Mer information finns i [prestanda och optimering i Azure Search](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Säkerhetskopiering och återställning

Eftersom Azure Search inte är en lösning för lagring av primära data, tillhandahåller vi en formell mekanism för självbetjäning säkerhetskopiering och återställning. Din programkod som används för att skapa och fylla i ett index är hello faktisk återställningsalternativet om du av misstag tar bort ett index. 

toorebuild ett index kan du ta bort den (förutsatt att den finns), återskapa hello indexet i hello-tjänsten och Läs in igen genom att hämta data från ditt primära dataarkiv. Alternativt kan du nå för[kundsupport]() toosalvage index om det finns ett regionalt strömavbrott.


<a id="scale"></a>

## <a name="scale-up-or-down"></a>Skala upp eller ned
Varje search-tjänsten startar med minst en replik och en partition. Om du har registrerat dig för en [nivå som ger dedicerade resurser](search-limits-quotas-capacity.md), klicka på hello **skala** panelen i hello service instrumentpanelen tooadjust Resursanvändning.

När du lägger till kapacitet via antingen resurs används hello dem automatiskt. Ingen ytterligare åtgärd krävs från din sida, men det finns en viss fördröjning innan realiseras hello effekten av hello ny resurs. Det kan ta 15 minuter eller mer tooprovision ytterligare resurser.

 ![][10]

### <a name="add-replicas"></a>Lägg till repliker
Öka frågor per sekund (QPS) eller att uppnå hög tillgänglighet görs genom att lägga till repliker. Varje replik har en kopia av ett index så att lägga till en mer replik översätter tooone mer index som är tillgängliga för hantering av tjänstbegäranden för frågan. Det krävs minst 3 repliker för hög tillgänglighet (se [kapacitetsplanering](search-capacity-planning.md) information).

En söktjänst med flera repliker kan läsa in frågebegäranden via ett större antal index. Ges en nivå av frågan volym kommer frågan genomströmning toobe snabbare när det finns fler kopior av hello index tillgängliga tooservice hello begäran. Om du upplever svarstid du förväntar dig en positiv inverkan på prestanda när hello ytterligare repliker är online.

Även om frågan genomströmning går upp när du lägger till repliker, den inte exakt dubbla eller trippel när du lägger till repliker tooyour service. Alla sökprogram är ämne tooexternal faktorer som kan ha inverkan på prestanda för frågor. Avancerade frågor och Nätverksfördröjningen är två faktorer som bidrar toovariations i frågan svarstider.

### <a name="add-partitions"></a>Lägg till partitioner
De flesta tjänstprogram behöver inbyggda ha flera repliker i stället för partitioner. Du kan lägga till partitioner om du har registrerat dig för tjänsten som Standard i de fall där en ökad dokumentantal krävs. Grundnivån tillhandahåller inte för fler partitioner.

Hello standardnivån, läggs partitioner i multiplar av 12 (särskilt 1, 2, 3, 4, 6 och 12). Det här är en artefakt för horisontell partitionering. Ett index skapas i 12 delar som kan alla lagras på 1 partition eller lika uppdelat 2, 3, 4, 6 och 12 partitioner (en Fragmentera per partition).

### <a name="remove-replicas"></a>Ta bort repliker
Du kan minska repliker när sökningen frågan belastningar har normaliserade (till exempel efter helgdag försäljning är över) efter perioder med hög frågan volymer.

toodo, flytta hello replik skjutreglaget tillbaka tooa lägre numret. Det finns inga ytterligare steg som krävs från din sida. Sänker hello replikantalet avsäger virtuella datorer i hello datacenter. Dina frågor och införandet åtgärder körs på virtuella färre datorer än före. hello lägsta tillåtna värdet är en replik.

### <a name="remove-partitions"></a>Ta bort partitioner
Till skillnad från att ta bort repliker, vilket kräver inget extra arbete från din sida, kanske vissa arbete toodo om du använder mer lagringsutrymme än kan minskas. Till exempel om lösningen använder tre partitioner, genererar downsizing tooone eller två partitioner ett fel om hello nytt lagringsutrymme är mindre än vad som krävs. Som förväntat, dina val toodelete index eller dokument i ett associerat index toofree utrymme eller behålla hello aktuella konfigurationen.

Det finns inga identifieringsmetod som talar om vilka index shards lagras på specifika partitioner. Varje partition innehåller cirka 25 GB lagring, så du måste tooreduce tooa lagringsstorlek som kan utföras med hello antalet partitioner som du har. Om du vill toorevert tooone partition, måste alla 12 shards toofit.

toohelp med framtida planering du kanske vill toocheck lagring (med hjälp av [hämta indexstatistik](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)) toosee hur mycket du faktiskt används. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment"></a>Bästa metoder för att skala och distribution
Den här 30 minuters videon granskar Metodtips för av avancerade distributionsscenarier, inklusive fördelade arbetsbelastningar. Du kan också se [prestanda och optimering i Azure Search](search-performance-optimization.md) för hjälp sidor som omfattar hello samma pekar.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Nästa steg
När du förstår hello koncepten bakom tjänsten administration kan du använda [PowerShell](search-manage-powershell.md) tooautomate uppgifter.

Vi rekommenderar också att granska hello [prestanda och optimering artikel](search-performance-optimization.md).

En annan rekommendation är toowatch hello video i hello föregående avsnitt. Det ger djupare täckning hello teknik nämns i det här avsnittet.

<!--Image references-->
[7]: ./media/search-manage/rbac-icon.png
[8]: ./media/search-manage/Azure-Search-Manage-1-URL.png
[9]: ./media/search-manage/Azure-Search-Manage-2-Keys.png
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



