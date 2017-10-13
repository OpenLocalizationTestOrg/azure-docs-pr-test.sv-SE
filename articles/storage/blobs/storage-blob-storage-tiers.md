---
title: "Frekvent, lågfrekvent och arkivlagring i Azure för blobbar | Microsoft Docs"
description: "Frekvent, lågfrekvent och arkivlagring för Azure Blob Storage-konton."
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/05/2017
ms.author: mihauss
ms.openlocfilehash: 544b11d74a926fe62b8ceca51570ce9d2ee7e6e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-blob-storage-hot-cool-and-archive-preview-storage-tiers"></a>Azure Blob Storage: Nivåer för frekvent, lågfrekvent och arkivlagring (förhandsversion)

## <a name="overview"></a>Översikt

Azure Storage erbjuder tre lagringsnivåer för lagring av Blob-objekt, så att du kan lagra data så kostnadseffektivt som möjligt, beroende på din användning. Azures **frekventa lagringsnivå** är optimerad för att lagra data som används ofta. Azures **lågfrekventa lagringsnivå** är optimerad för att lagra data som inte används ofta och som lagras i minst en månad. [Arkivlagringsnivån (förhandsversion)](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) är optimerad för att lagra data som används sällan och som lagras i minst sex månader med flexibla svarstidskrav (i storleksordningen timmar). *Arkiv*lagringsnivån kan bara användas på blob-nivå och inte på hela lagringskontot. Data i den lågfrekventa lagringsnivån klarar lite lägre tillgänglighet, men kräver fortfarande hög hållbarhet och liknande åtkomsttid och dataflödesegenskaper som data i frekvent lagringsnivå. För data i den lågfrekventa och arkivlagringsnivån är serviceavtal med lägre tillgänglighet och högre åtkomstkostnader godtagbara med tanke på de mycket lägre lagringskostnaderna.

Idag växer mängden data som lagras i molnet i exponentiell takt. För att hålla kontroll på kostnaderna för dina växande lagringsbehov är det en god idé att ordna data baserat på attribut som åtkomstfrekvens och planerad kvarhållningsperiod. Data som lagras i molnet kan vara olika beroende på hur de genereras, bearbetas och används under livslängden. Vissa data används aktivt och ändras under livslängden. Vissa data används ofta i början av livslängden och sedan minskar användning drastiskt när dessa data blir äldre. Vissa data förblir inaktiva i molnet och används sällan, eller kanske aldrig, när de har lagrats.

För varje scenario finns en lagringsnivå som är optimerad för motsvarande åtkomstmönster. I Azure Blob Storage finns en frekvent, lågfrekvent och arkivlagringsnivå som uppfyller behovet av olika lagringsnivåer med olika prissättningsmodeller.

## <a name="blob-storage-accounts"></a>Blob Storage-konton

**Blob Storage-konton** är särskilda lagringskonton för att lagra ostrukturerade data som blobar (objekt) i Azure Storage. Med Blob Storage-konton kan du nu välja mellan frekvent och lågfrekvent lagringsnivå på kontonivå eller frekvent, lågfrekvent och arkivnivå på blob-nivå, baserat på åtkomstmönster. Lagra lågfrekventa data som används sällan till den lägsta lagringskostnaden, lågfrekventa data som används mindre ofta till en lägre lagringskostnad än frekventa, och lagra frekventa data som används oftare till den lägsta åtkomstkostnaden. Blob Storage-konton liknar dina befintliga allmänna lagringskonton och har samma höga hållbarhet, tillgänglighet, skalbarhet och prestanda som du använder i dag, inklusive 100 procent API-konsekvens för blockblobbar och tilläggsblobbar.

> [!NOTE]
> Blob Storage-konton stöder endast block- och tilläggsblobar, inte sidblobar.

Blob Storage-konton exponerar attributet **Åtkomstnivå** som gör att du kan välja **Frekvent** eller **Lågfrekvent** lagringsnivå beroende på vilken typ av data som lagras på kontot. Du kan när som helst byta mellan de olika lagringsnivåerna om användningsmönstret förändras. Arkivnivån (förhandsversion) kan endast användas på blob-nivå.

> [!NOTE]
> Ändringar av lagringsnivån kan medför ytterligare avgifter. Mer information finns i avsnittet [Priser och fakturering](#pricing-and-billing).

### <a name="hot-access-tier"></a>Frekvent åtkomstnivå

Exempelscenarier för frekvent lagringsnivå:

* Data används aktivt eller förväntas användas ofta (både för att läsa och skriva).
* Data som mellanlagras för bearbetning och eventuell migrering till lågfrekvent åtkomstnivå.

### <a name="cool-access-tier"></a>Lågfrekvent åtkomstnivå

Exempelscenarier för lågfrekvent lagringsnivå:

* Datauppsättningar för kortsiktig säkerhetskopiering och haveriberedskap.
* Äldre medieinnehåll som inte visas så ofta längre, men som förväntas vara tillgängligt direkt vid behov.
* Stora datamängder som behöver en kostnadseffektiv lagring under tiden mer data samlas in för framtida bearbetning. (*Till exempel* långsiktig lagring av vetenskapliga data eller telemetridata (rådata) från en tillverkningsanläggning.)

### <a name="archive-access-tier-preview"></a>Arkivåtkomstnivå (förhandsversion)

[Arkivlagring](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) har lägst lagringskostnad och högre kostnader för datahämtning jämfört med frekvent och lågfrekvent lagring.

När en blob finns i arkivlagring kan den inte läsas, kopieras, skrivas över eller ändras. Du kan inte heller ta ögonblicksbilder av en blob i arkivlagring. Du kan emellertid använda befintliga åtgärder för att ta bort, lista, hämta blob-egenskaper/-metadata eller ändra nivå för din blob. Om du vill läsa data i arkivlagring måste du först ändra nivå för din blob till frekvent eller lågfrekvent. Processen kallas återuppväckning och kan ta upp till 15 timmar för blobbar som är mindre än 50 GB. Hur mycket mer tid som krävs för större blobbar varierar beroende på blobbens dataflödesgräns.

Under återuppväckning kan du kontrollera blob-egenskapen ”arkivstatus” för att bekräfta om nivån har ändrats. Status är ”rehydrate-pending-to-hot” eller ”rehydrate-pending-to-cool” beroende på målnivån. När åtgärden har slutförts tas blob-egenskapen ”arkivstatus” bort och blob-egenskapen ”åtkomstnivå” anger frekvent eller lågfrekvent nivå.  

Exempelscenarier för arkivlagringsnivå:

* Datauppsättningar för långsiktig säkerhetskopiering, arkivering och haveriberedskap
* Ursprungliga rådata som måste bevaras, även efter att de har bearbetats till ett slutligt användbart format. (*Till exempel* mediefiler i RAW-format som har omkodats till andra format.)
* Efterlevnads- och arkiveringsdata som behöver lagras under en längre tid och som nästan aldrig används. (*Till exempel* film från säkerhetskameror, gamla röntgenbilder/magnetröntgenbilder för vårdorganisationer, ljudinspelningar och transkript av kundsamtal för ekonomiska tjänster.)

### <a name="recommendations"></a>Rekommendationer

Mer information om lagringskonton finns i [Om Azure Storage-konton](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

För program som bara behöver block- eller tilläggsbloblagring rekommenderar vi att du använder Blob Storage-konton så att du kan utnyttja de olika prissättningsmodellerna för olika lagringsnivåer. Men i vissa fall kan det vara bättre att använda allmänna lagringskonton, t.ex. om:

* Du behöver använda tabeller, köer eller filer och vill att dina blobar ska lagras i samma lagringskonto. Observera att det inte finns några tekniska fördelar med att lagra dessa på samma konto förutom att du får samma delade nycklar.

* Du ändå måste använda den klassiska distributionsmodellen. Blob Storage-konton är bara tillgängliga via Azure Resource Manager-distributionsmodellen.

* Du behöver använda sidblobar. Blob Storage-konton stöder inte sidblobar. Generellt rekommenderar vi att du använder blockblobar om du inte har ett särskilt behov av sidblobar.

* Du använder en version av [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) som är äldre än 2014-02-14 eller ett klientbiblioteket med en tidigare version än 4.x och det inte går att uppgradera ditt program.

> [!NOTE]
> Blob Storage-konton stöds för närvarande i alla Azure-regionerna.
 

## <a name="blob-level-tiering-feature-preview"></a>Blob-nivåfunktion (förhandsversion)

Med blob-nivå kan du nu ändra nivå för dina data på objektnivå med hjälp av en enda åtgärd som kallas [ange blob-nivå](/rest/api/storageservices/set-blob-tier). Du kan enkelt ändra åtkomstnivå för en blob mellan frekvent, lågfrekvent eller arkivnivå när användningsmönster ändras, utan att behöva flytta data mellan konton. Alla nivåändringar sker omedelbart förutom när en blob återuppväcks från arkivet. Blobar i alla tre lagringsnivåer kan finnas tillsammans i samma konto. En blob som inte har en uttryckligen tilldelad nivå ärver nivån från kontots åtkomstnivåinställning.

Om du vill använda dessa funktioner i förhandsversionen följer du anvisningarna i [blogginlägget om arkiv- och blob-nivå i Azure](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering).

Här är några begränsningar som gäller under förhandsversionen för blob-nivå:

* Endast nya Blob Storage-konton som skapats i USA, östra 2 efter registrering för förhandsversionen har stöd för arkivlagring.

* Endast nya Blob Storage-konton som skapats i offentliga regioner efter registrering för förhandsversionen har stöd för blob-nivå.

* Blob-nivå och arkivlagring stöds endast i [LRS]-lagring (../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#locally-redundant-storage). [GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#geo-redundant-storage) och [RA-GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#read-access-geo-redundant-storage) kommer att stödjas framöver.

* Du kan inte ändra nivå för en blob med ögonblicksbilder.

* Du kan inte kopiera eller skapa en ögonblicksbild av en blob i arkivlagring.

## <a name="comparison-of-the-storage-tiers"></a>Jämförelse av lagringsnivåerna

Följande tabell innehåller en jämförelse av frekvent och lågfrekvent lagringsnivå. Arkiv-blob-nivån är i förhandsversion, så det finns inga serviceavtal för den.

| | **Frekvent lagringsnivå** | **Lågfrekvent lagringsnivå** |
| ---- | ----- | ----- |
| **Tillgänglighet** | 99,9 % | 99 % |
| **Tillgänglighet** <br> **(RA-GRS-läsningar)**| 99,99 % | 99,9 % |
| **Avgifter för användning** | Högre kostnader för lagring, lägre kostnader för åtkomst och transaktioner | Lägre kostnader för lagring, högre kostnader för åtkomst och transaktioner |
| **Minsta objektstorlek** | Saknas | Saknas |
| **Minsta lagringstid** | Saknas | Saknas |
| **Svarstid** <br> **(Tid till första byte)** | millisekunder | millisekunder |
| **Mål för skalbarhet och prestanda** | Samma som allmänna lagringskonton | Samma som allmänna lagringskonton |

> [!NOTE]
> Blob Storage-konton har samma mål för prestanda och skalbarhet som allmänna lagringskonton. Mer information finns i [Skalbarhets- och prestandamål i Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).


## <a name="pricing-and-billing"></a>Priser och fakturering
För Blob Storage-konton används en prissättningsmodell för blob-lagring baserat på lagringsnivå. När du använder ett Blob Storage-konto gäller följande för debitering:

* **Lagringskostnader**: Utöver mängden data som lagras varierar lagringskostnaden beroende på lagringsnivå. Kostnaden per gigabyte är lägre för den lågfrekventa lagringsnivån än för den frekventa lagringsnivån.

* **Kostnader för dataåtkomst**: För data på den lågfrekventa lagringsnivån debiteras du en åtkomstavgift per gigabyte för läsningar och skrivningar.

* **Transaktionskostnader**: Det finns en kostnad per transaktion för båda nivåerna. Kostnaden per transaktion är dock högre för lågfrekvent lagringsnivå än för frekvent lagringsnivå.

* **Dataöverföringskostnader för geo-replikering**: Detta gäller endast konton med konfigurerad geo-replikering, inklusive GRS och RA-GRS. Dataöverföring för geo-replikering debiteras per gigabyte.

* **Kostnader för utgående dataöverföring**: Utgående dataöverföringar (data som överförs utanför en Azure-region) debiteras för bandbreddsanvändning per gigabyte, på samma sätt som för allmänna lagringskonton.

* **Ändring av lagringsnivå**: Om du byter lagringsnivå från lågfrekvent till frekvent utgår en avgift motsvarande läsningen av alla data på lagringskontot för varje övergång. Å andra sidan kostar det inget att byta från frekvent till lågfrekvent lagringsnivå.

> [!NOTE]
> Mer information om priserna för Blob Storage-konton finns på sidan [Pris för Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Mer information om kostnaderna för utgående dataöverföring finns på sidan [Prisinformation om Dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="quickstart"></a>Snabbstart

I det här avsnittet visar vi hur du utför följande åtgärder på Azure Portal:

* Skapar ett Blob Storage-konto.
* Hanterar ett Blob Storage-konto.

Du kan inte ange arkiv som åtkomstnivå i följande exempel eftersom den här inställningen gäller för hela lagringskontot. Arkiv kan bara anges för en specifik blob.

### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Skapa ett Blob Storage-konto på Azure Portal

1. Logga in på [Azure Portal](https://portal.azure.com).

2. På navmenyn väljer du **Nytt** > **Data + Storage** > **Storage-konto**.

3. Ange ett namn för lagringskontot.
   
    Det här namnet måste vara globalt unikt. Det används som en del av URL:en som används för att få åtkomst till objekt i lagringskontot.  

4. Välj **Resource Manager** som distributionsmodell.
   
    Nivåindelad lagring kan bara användas med Resource Manager-lagringskonton. Det här är den rekommenderade distributionsmodellen för nya resurser. Mer information finns i [Översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).  

5. Välj **Blob Storage** i listrutan Typ av konto.
   
    Det är här du väljer typen av lagringskonto. Nivåindelad lagring är inte tillgängligt för allmän lagring. Det är endast tillgängligt med Blob Storage-konton.     
   
    Observera att prestandanivån anges till Standard när du väljer detta. Nivåindelad lagring är inte tillgängligt med Premium-prestandanivån.

6. Välj replikeringsalternativ för lagringskontot: **LRS**, **GRS** eller **RA-GRS**. Standardinställningen är **RA-GRS**.
   
    LRS = lokalt redundant lagring. GRS = geo-redundant lagring (2 regioner). RA-GRS är skrivskyddad geo-redundant lagring (2 regioner med läsbehörighet till den andra).
   
    Mer information om replikeringsalternativen för Azure Storage finns i [Azure Storage-replikering](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

7. Välj rätt lagringsnivå för dina behov: Ange **Åtkomstnivå** till antingen **Cool** (lågfrekvent) eller **Hot** (frekvent). Standardinställningen är **Frekvent**. 

8. Välj den prenumeration som du vill skapa det nya lagringskontot i.

9. Ange en ny resursgrupp eller välj en befintlig resursgrupp. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

10. Välj regionen för ditt lagringskonto.

11. Skapa lagringskontot genom att klicka på **Skapa**.

### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Ändra lagringsnivån för ett Blob Storage-konto med hjälp av Azure Portal

1. Logga in på [Azure Portal](https://portal.azure.com).

2. Gå till ditt lagringskonto genom att välja Alla resurser och välj sedan ditt lagringskonto.

3. Klicka på **Konfiguration** på bladet Inställningar för att visa och/eller ändra kontokonfigurationen.

4. Välj rätt lagringsnivå för dina behov: Ange **Åtkomstnivå** till antingen **Cool** (lågfrekvent) eller **Hot** (frekvent).

5. Klicka på Spara överst på bladet.

> [!NOTE]
> Ändringar av lagringsnivån kan medför ytterligare avgifter. Mer information finns i avsnittet [Priser och fakturering](#pricing-and-billing).


## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Utvärdera och migrera till Blob Storage-konton
Avsikten med det här avsnittet är att hjälpa användarna så att övergången till Blob Storage-konton blir så smidig som möjligt. Det finns två scenarier:

* Du har ett befintligt allmänt lagringskonto och vill utvärdera en övergång till ett Blob Storage-konto med rätt lagringsnivå.
* Du har bestämt dig för att använda ett Blob Storage-konto eller har redan ett och vill utvärdera om du ska använda lågfrekvent eller frekvent lagringsnivå.

I båda fallen är det första du gör att beräkna kostnaden för att lagra och komma åt dina data i Blob Storage-kontot och jämföra det med dina nuvarande kostnader.

## <a name="evaluating-blob-storage-account-tiers"></a>Utvärdera Blob Storage-kontonivåer

För att beräkna kostnaden för att lagra och komma åt data som lagras i ett Blob Storage-konto måste du utvärdera ditt nuvarande användningsmönster eller göra en uppskattning av ditt förväntade användningsmönster. Vanligtvis vill du veta:

* Din lagringsanvändning – hur mycket data lagras och hur ändras lagringen månadsvis?

* Ditt åtkomstmönster – hur mycket data läses in och skrivs till kontot (inklusive nya data)? Hur många transaktioner används för dataåtkomst och vilka typer av transaktioner är de?

## <a name="monitoring-existing-storage-accounts"></a>Övervaka befintliga lagringskonton

Om du vill övervaka dina befintliga lagringskonton och samla in dessa data kan du använda Azure Storage Analytics som utför loggning och tillhandahåller mätvärden för ett lagringskonto. Storage Analytics kan lagra mätvärden som innehåller aggregerad transaktionsstatistik och kapacitetsdata om förfrågningar till Blob Storage både för allmänna lagringskonton och Blob Storage-konton. Dessa data lagras i välkända tabeller i samma lagringskonto.

Mer information finns i [Om mätvärden i Storage Analytics](https://msdn.microsoft.com/library/azure/hh343258.aspx) och [Schema över måttabeller i Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> Blob Storage-konton exponerar endast tabelltjänstens slutpunkt för att lagra och komma åt mätvärden för kontot i fråga.

Om du vill övervaka lagringsanvändningen för Blob Storage-tjänsten måste du aktivera kapacitetsmåtten.
När du har gjort det registreras kapacitetsdata varje dag för ett lagringskontos Blob Service, och registreras som en tabellpost som skrivs till tabellen *$MetricsCapacityBlob* i samma lagringskonto.

Om du vill övervaka dataåtkomstmönstret för Blob Storage-tjänsten måste du aktivera transaktionsmått för varje timme på API-nivå. När du har gjort det aggregeras transaktioner för varje API varje timme, och registreras som en tabellpost som skrivs till tabellen *$MetricsHourPrimaryTransactionsBlob* i samma lagringskonto. Tabellen *$MetricsHourSecondaryTransactionsBlob* registrerar transaktionerna till den sekundära slutpunkten när du använder RA-GRS-lagringskonton.

> [!NOTE]
> Om du har ett allmänt lagringskonto där du har lagrat sidblobbar och virtuella datordiskar jämte block och tillsammans med blockdata och data för tilläggsblobbar så gäller inte den här uppskattningsberäkningen. Det beror på att du inte kan särskilja kapacitets- och transaktionsmått baserat på typen av blobb endast för block och tilläggsblobbar som kan migreras till ett Blob Storage-konto.

För att få en bra uppskattning av din dataförbrukning och ditt åtkomstmönster rekommenderar vi att du väljer en kvarhållningsperiod för mätvärden som är representativ för din normala användning, och att du utgår därifrån. Ett alternativ är att spara mätvärdena i sju dagar och samla in data varje vecka, för att sedan analysera dessa data i slutet av månaden. Ett annat alternativ är att spara mätvärden i 30 dagar och samla in och analysera dessa data i slutet av 30-dagarsperioden.

Mer information om hur du aktiverar, samlar in och visar mätvärden finns i [Aktivera Azure Storage-mätvärden och visa måttdata](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Lagring, åtkomst och hämtning av analysdata debiteras på samma sätt som vanliga användardata.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Beräkna kostnader med hjälp av användningsmått

### <a name="storage-costs"></a>Lagringskostnader

Den senaste posten i kapacitetsmåttstabellen *$MetricsCapacityBlob* med radnyckeln *'data'* visar lagringskapacitet som förbrukats av användardata. Den senaste posten i kapacitetsmåttstabellen *$MetricsCapacityBlob* med radnyckeln *'analytics'* visar lagringskapaciteten som förbrukats av analysloggarna.

Den här totala kapaciteten som förbrukats av både användardata och analysloggar (om detta aktiverats) kan sedan användas för att beräkna kostnaden för datalagring i lagringskontot. Samma metod kan också användas för att uppskatta lagringskostnaderna för block och tilläggsblobbar i allmänna lagringskonton.

### <a name="transaction-costs"></a>Transaktionskostnader

Summan av *'TotalBillableRequests'*, för alla poster för ett API i tabellen över transaktionsmått, visar det totala antalet transaktioner för API:et i fråga. *Till exempel* kan det totala antalet *'GetBlob'*-transaktioner inom en viss period beräknas baserat på summan av totalt antal debiterbara begäranden för alla poster med radnyckeln *'user;GetBlob'*.

För att kunna beräkna transaktionskostnader för Blob Storage-konton måste du dela in transaktionerna i tre grupper eftersom de har olika pris.

* Skrivtransaktioner som *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* och *'CopyBlob'*.
* Borttagningstransaktioner som *'DeleteBlob'* och *'DeleteContainer'*.
* Alla andra transaktioner.

För att kunna beräkna transaktionskostnaderna för allmänna lagringskonton måste du aggregera alla transaktioner oavsett åtgärd/API.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Dataöverföringskostnader för dataåtkomst och geo-replikering

Lagringsanalyserna visar inte mängden data som har lästs eller skrivits till ett lagringskonto, men du kan göra en ungefärlig uppskattning genom att titta i tabellen över transaktionsmått. Summan av *'TotalIngress'* för alla poster för ett API i tabellen över transaktionsmått anger den totala mängden inkommande data i antal byte för API:et i fråga. På samma sätt anger summan av *'TotalEgress'* den totala mängden utgående data i antal byte.

För att kunna beräkna kostnaderna för dataåtkomst för Blob Storage-konton måste du dela in transaktionerna i två grupper. 

* Du kan beräkna mängden data som hämtas från lagringskontot genom att titta på summan av *'TotalEgress'* och särskilt för åtgärderna *'GetBlob'* och *'CopyBlob'*.

* Du kan beräkna mängden data som skrivs till lagringskontot genom att titta på summan av *'TotalIngress'* och särskilt för åtgärderna *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* och *'AppendBlock'*.

När du använder ett GRS- eller RA-GRS-lagringskonto kan kostnaden för dataöverföring med geo-replikering för Blob Storage-konton också beräknas baserat på uppskattningen av mängden data som skrivits.

> [!NOTE]
> Ett mer detaljerat exempel på hur du beräknar kostnaderna för den frekventa eller lågfrekventa lagringsnivån finns i frågeavsnittet *”Vad är lågfrekvent och frekvent lagringsnivå och hur vet jag vilken jag ska använda?”* på [Azure Storage-prissidan](https://azure.microsoft.com/pricing/details/storage/).
 
## <a name="migrating-existing-data"></a>Migrera befintliga data

Ett Blob Storage-konto är specialanpassat för lagring av endast block- och tilläggsblobar. Allmänna lagringskonton, där du kan lagra tabeller, köer, filer och diskar, och även blobbar, kan inte omvandlas till Blob Storage-konton. Om du vill använda lagringsnivåer måste du skapa Blob Storage-konton och migrera dina befintliga data till de nya kontona.

Med följande metoder kan du migrera befintliga data till Blob Storage-konton från lokala lagringsenheter, från externa molnlagringsleverantörer eller från befintliga allmänna lagringskonton i Azure:

### <a name="azcopy"></a>AzCopy

AzCopy är ett Windows-kommandoradsverktyg för högpresterande kopiering av data till och från Azure Storage. Med AzCopy kan du kopiera data till ditt Blob Storage-konto från dina befintliga allmänna lagringskonton eller överföra data från dina lokala lagringsenheter till ditt Blob Storage-konto.

Mer information finns i [Transfer data with the AzCopy Command-Line Utility](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Bibliotek för flytt av data

Azure Storage-biblioteket för flytt av data för .NET är baserat på det ramverk för flytt av data som används för AzCopy. Biblioteket är utformat för högpresterande, tillförlitliga och enkla åtgärder för dataöverföring och liknar de som används i AzCopy. Du kan utnyttja fördelarna med funktionerna som tillhandahålls av AzCopy direkt i din app utan att du behöver köra och övervaka externa instanser av AzCopy.

Mer information finns i [Azure Storage Data Movement Library for .Net](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST-API eller klientbibliotek

Du kan skapa en anpassad app för att migrera dina data till ett Blob Storage-konto med Azures klientbibliotek eller REST-API:t för Azure Storage-tjänster. Azure Storage innehåller omfattande klientbibliotek för flera språk och plattformar som .NET, Java, C++, Node.JS, PHP, Ruby och Python. Klientbiblioteken har avancerade funktioner, t.ex. logik för omprövning, loggning och parallell överföring. Du kan också utveckla direkt mot REST-API:t, som kan anropas med valfritt språk som kan skicka HTTP/HTTPS-begäranden.

Mer information finns i [Get Started with Azure Blob storage](storage-dotnet-how-to-use-blobs.md) (Kom igång med Azure Blob Storage).

> [!NOTE]
> Blobar som krypteras med kryptering på klientsidan lagrar krypteringsrelaterade metadata tillsammans med bloben. Det är absolut nödvändigt att kopieringsmekanismen ser till att blobmetadata, och särskilt krypteringsrelaterade metadata, bevaras. Om du kopierar blobar utan metadata kan blobinnehållet inte hämtas igen. Mer information om krypteringsrelaterade metadata finns i [Azure Storage Client Side Encryption](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
 
## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

1. **Är de befintliga lagringskontona fortfarande tillgängliga?**
   
    Ja, de befintliga lagringskontona är fortfarande tillgängliga. Varken priserna eller funktionerna har ändrats.  Du kan inte välja lagringsnivå med dessa konton och du kommer inte heller att kunna göra det i framtiden.

2. **När och varför ska jag börja använda Blob Storage-konton?**
   
    Blob Storage-konton är särskilt utformade för att lagra blobar och ger oss möjlighet att lägga till nya blobanpassade funktioner. Vi rekommenderar att du använder Blob Storage-konton i fortsättningen för att lagra blobar eftersom framtida funktioner, t.ex. hierarkisk lagring och lagringsnivåer, kommer att läggas till för den här kontotypen. Det är naturligtvis upp till dig när du vill migrera beroende på dina affärsbehov.

3. **Kan jag omvandla mitt befintliga lagringskonto till ett Blob Storage-konto?**
   
    Nej. Blob Storage-kontot är en annan typ av lagringskonto. Du måste skapa ett nytt konto och sedan migrera dina data enligt beskrivningen ovan.

4. **Kan jag lagra objekt på båda lagringsnivåerna i samma konto?**
   
    Attributet *'Access Tier'* anger värdet för lagringsnivån som anges på kontonivå och gäller för alla objekt i det kontot. Men med funktionen för blob-nivå (förhandsversion) kan du ange åtkomstnivå för specifika blobbar. Det åsidosätter åtkomstnivåinställningen för kontot. 

5. **Kan jag ändra lagringsnivå för mitt Blob Storage-konto?**
   
    Ja. Du kan ändra lagringsnivå med attributet *'Access Tier'* för lagringskontot. Ändringar av lagringsnivån gäller för alla objekt som lagras i kontot. Om du ändrar lagringsnivå från frekvent till lågfrekvent lagringsnivå behöver du inte betala något, men om du byter från lågfrekvent till frekvent debiteras du per GB för läsning av alla data på kontot.

6. **Hur ofta kan jag ändra lagringsnivå för mitt Blob Storage-konto?**
   
    Vi har ingen begränsning för hur ofta du kan ändra lagringsnivån, men observera att det kan medföra kostnader att ändra lagringsnivå från lågfrekvent till frekvent. Vi rekommenderar att du inte ändrar lagringsnivån ofta.

7. **Beter sig blobarna på lågfrekvent lagringsnivå annorlunda än på frekvent lagringsnivå?**
   
    Blobbar på frekvent lagringsnivå har samma svarstid som blobbar i allmänna lagringskonton. Blobbar på lågfrekvent lagringsnivå har liknande svarstid (i millisekunder) som blobbar i allmänna lagringskonton.
   
    Blobbar på lågfrekvent lagringsnivå har något lägre tillgänglighetsnivå (enligt SLA) än blobbar som lagras på frekvent lagringsnivå. Mer information finns i [SLA för Storage](https://azure.microsoft.com/support/legal/sla/storage).

8. **Kan jag lagra sidblobar och virtuella datordiskar i Blob Storage-konton?**
   
    Blob Storage-konton stöder endast block- och tilläggsblobar, inte sidblobar. Virtuella datordiskar i Azure backas upp av sidblobar, vilket gör att Blob Storage-konton inte kan användas för att lagra virtuella datordiskar. Däremot kan du lagra säkerhetskopior av virtuella datordiskar som blockblobar i ett Blob Storage-konto.

9. **Måste jag ändra mina befintliga appar för att använda Blob Storage-konton?**
   
    Blob Storage-konton är API-konsekventa till 100 % med allmänna lagringskonton för block- och tilläggsblobar. Så länge ditt program använder blockblobbar eller tilläggsblobbar, och du använder 2014-02-14-versionen av [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) eller senare ska ditt program fungera korrekt. Om du använder en äldre version av protokollet måste du uppdatera appen till den nya versionen för att den ska fungera smidigt med båda typerna av lagringskonton. Normalt rekommenderar vi att du använder den senaste versionen oavsett vilken lagringskontotyp du använder.

10. **Kommer användarupplevelsen att ändras?**
    
    Blob Storage-konton är mycket lika allmänna lagringskonton när det gäller att lagra block- och tilläggsblobar, och de stöder alla viktiga funktioner i Azure Storage, med bland annat hög hållbarhet och tillgänglighet, skalbarhet, prestanda och säkerhet. Förutom funktionerna och begränsningarna som är specifika för Blob Storage-konton och lagringsnivåerna som nämnts ovan är allt annat detsamma.

## <a name="next-steps"></a>Nästa steg

### <a name="evaluate-blob-storage-accounts"></a>Utvärdera Blob Storage-konton

[Kontrollera tillgängligheten för Blob Storage-konton per region](https://azure.microsoft.com/regions/#services)

[Utvärdera användningen av dina aktuella lagringskonton genom att aktivera mätvärden i Azure Storage.](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Se priser för Blob Storage per region](https://azure.microsoft.com/pricing/details/storage/)

[Se priser för dataöverföring](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Börja använda Blob Storage-konton

[Komma igång med Azure Blob Storage](storage-dotnet-how-to-use-blobs.md)

[Flytta data till och från Azure Storage](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Överföra data med kommandoradsverktyget AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Bläddra igenom och utforska dina lagringskonton](http://storageexplorer.com/)