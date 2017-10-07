---
title: aaaScale ut en Azure SQL database | Microsoft Docs
description: "Hur toouse hello ShardMapManager klientbibliotek för elastisk databas"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 0e9d647a-9ba9-4875-aa22-662d01283439
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 4cf670d42ad7ded98fb8d6f0830154587dd2c6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-out-databases-with-hello-shard-map-manager"></a>Skala ut databaser med hello Fragmentera kartan manager
tooeasily skala ut databaser i SQL Azure bör använda en Fragmentera kartan manager. hello Fragmentera kartan manager är en särskild databas som underhåller globala mappningsinformation om alla shards (databaser) i en Fragmentera. hello metadata kan en tooconnect toohello rätt databas baserat på hello värdet för hello **horisontell partitionering nyckeln**. Varje Fragmentera i hello uppsättningen innehåller dessutom maps som spårar hello lokala Fragmentera data (kallas även **shardlets**). 

![Hantering av Fragmentera karta](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

Förstå hur dessa mappningar skapas är viktigt tooshard kartan. Detta görs med hjälp av hello [ShardMapManager klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)hittades i hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md) toomanage Fragmentera mappar.  

## <a name="shard-maps-and-shard-mappings"></a>Fragmentera maps och Fragmentera mappningar
Du måste välja hello typ av Fragmentera kartan toocreate för varje Fragmentera. hello dig beror på hello database-arkitektur: 

1. En organisation per databas  
2. Flera klienter per databas (två typer):
   1. Lista över-mappning
   2. Mappning av intervallet

En enskild klient-modell, skapa en **lista mappning** Fragmentera kartan. modell för hello single-klienter tilldelas en databas per klient. Detta är en effektiv modell för SaaS-utvecklare som det förenklar hanteringen.

![Lista över-mappning][1]

hello modell för flera klienter tilldelas flera innehavare tooa enskild databas (och du kan distribuera grupper av klienter över flera databaser). Använd den här modellen när du förväntar dig varje klient toohave små databehov. I den här modellen vi tilldela ett intervall med klienter tooa en databas med hjälp av **intervallet mappning**. 

![Mappning av intervallet][2]

Eller du kan implementera en databas för flera innehavare modellen med hjälp av en *lista mappning* tooassign flera innehavare tooa enskild databas. Till exempel DB1 är används toostore information om klient-id 1 och 5 och DB2 lagrar data för klienten 7 och 10-klient. 

![Muliple klienter i samma DB][3] 

### <a name="supported-net-types-for-sharding-keys"></a>.Net-typer som stöds för horisontell partitionering nycklar
Elastisk skala stöd hello efter .net Framework typer som horisontell partitionering nycklar:

* heltal
* lång
* GUID
* byte]  
* Datum och tid
* TimeSpan
* DateTimeOffset

### <a name="list-and-range-shard-maps"></a>Lista och intervallet Fragmentera maps
Fragmentera maps kan konstrueras med **listor över enskilda horisontell partitionering nyckeln värden**, eller så kan de vara konstruerade med **intervallen för horisontell partitionering nyckeln värden**. 

### <a name="list-shard-maps"></a>Lista Fragmentera maps
**Shards** innehåller **shardlets** och hello mappningen av shardlets tooshards underhålls av en Fragmentera karta. En **lista Fragmentera kartan** är en association mellan hello enskilda nyckelvärden som identifierar hello shardlets och hello-databaser som fungerar som delar.  **Visa en lista över mappningar** är explicit och annan nyckelvärden kan vara mappade toohello samma databas. Till exempel nyckel 1 mappar tooDatabase A och nyckelvärden 3 och 6 referera databasen B.

| Nyckel | Fragmentera plats |
| --- | --- |
| 1 |Database_A |
| 3 |Database_B |
| 4 |Database_C |
| 6 |Database_B |
| ... |... |

### <a name="range-shard-maps"></a>Intervallet Fragmentera maps
I en **intervallet Fragmentera kartan**, hello viktiga intervallet anges med ett par **[lågt värde, högt värde)** där hello *låg värdet* är hello minsta nyckel i hello intervall och hello *Värdefulla* är hello första värde som är högre än hello intervall. 

Till exempel **[0, 100)** innefattar alla heltal större än eller lika med 0 och mindre än 100. Observera att flera adressintervall kan punkt toohello samma databasen och osammanhängande adressintervall stöds (t.ex. [100,200) och [400,600) både punkt tooDatabase C i hello exemplet nedan.)

| Nyckel | Fragmentera plats |
| --- | --- |
| [1,50) |Database_A |
| [50,100) |Database_B |
| [100,200) |Database_C |
| [400,600) |Database_C |
| ... |... |

Hello-tabeller som visas ovan är ett grundläggande exempel på en **ShardMap** objekt. Varje rad är ett förenklat exempel på en enskild **PointMapping** (för hello listan Fragmentera kartan) eller **RangeMapping** (för hello intervallet Fragmentera kartan) objekt.

## <a name="shard-map-manager"></a>Fragmentera kartan manager
I hello klientbiblioteket är hello Fragmentera kartan manager en samling Fragmentera maps. Hej data som hanteras av en **ShardMapManager** instans sparas på tre platser: 

1. **Globala Fragmentera karta (GSM)**: du anger en databas tooserve som hello lagringsplatsen för alla Fragmentera maps och mappningar. Särskilda tabeller och lagrade procedurer skapas automatiskt toomanage hello information. Detta är vanligtvis en liten databas och lätt öppnas och ska inte användas för andra behov av programmet hello. hello tabeller är i ett särskilt schema med namnet **__ShardManagement**. 
2. **Lokala Fragmentera karta (LSM)**: alla databaser som du anger en Fragmentera är toobe ändrade toocontain flera små tabeller och särskilda lagrade procedurer som innehåller och hantera Fragmentera kartan information specifik toothat Fragmentera. Den här informationen är redundant hello informationen i hello GSM och låter hello toovalidate cachelagras Fragmentera kartan programinformation utan att placera belastning på hello GSM; hello program använder hello LSM toodetermine om en cachelagrad mappning fortfarande är giltigt. hello tabeller motsvarande toohello LSM på varje Fragmentera finns också i hello schemat **__ShardManagement**.
3. **Cacheminne**: varje instans åtkomst programmet till en **ShardMapManager** objekt upprätthåller en lokal minnescache av dess mappningar. Lagrar den routningsinformation som nyligen har hämtats. 

## <a name="constructing-a-shardmapmanager"></a>Hur du skapar en ShardMapManager
En **ShardMapManager** objektet har skapats med hjälp av en [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) mönster. Hej  **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**  metoden tar autentiseringsuppgifter (inklusive hello servernamnet och databasnamnet hålla hello GSM) i hello form av en  **ConnectionString** och returnerar en instans av en **ShardMapManager**.  

**Obs:** hello **ShardMapManager** instansieras bara en gång per app-domän, inom hello initieringen kod för ett program. Skapandet av ytterligare instanser av ShardMapManager i hello samma appdomain resulterar i ökad minne och CPU-användning av programmet hello. En **ShardMapManager** kan innehålla valfritt antal Fragmentera maps. Även om en enda Fragmentera karta är tillräcklig för många program, finns det tillfällen när olika uppsättningar av databaser som används för olika schemat eller för unika ändamål. i sådana fall kan det vara bättre att ange flera Fragmentera maps. 

I den här koden, ett program försöker tooopen en befintlig **ShardMapManager** med hello [TryGetSqlShardMapManager metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).  Om objekt som representerar en Global **ShardMapManager** (GSM) inte ännu finnas inuti hello-databasen, hello klientbiblioteket skapar dem det med hjälp av hello [CreateSqlShardMapManager metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).

    // Try tooget a reference toohello Shard Map Manager 
     // via hello Shard Map Manager database.  
    // If it doesn't already exist, then create it. 
    ShardMapManager shardMapManager; 
    bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

    if (shardMapManagerExists) 
     { 
        Console.WriteLine("Shard Map Manager already exists");
    } 
    else
    {
        // Create hello Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // hello connectionString contains server name, database name, and admin credentials 
        // for privileges on both hello GSM and hello shards themselves.
    } 

Alternativt kan använda du Powershell toocreate en ny Fragmentera kartan chef. Ett exempel är tillgänglig [här](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="get-a-rangeshardmap-or-listshardmap"></a>Hämta en RangeShardMap eller ListShardMap
När du har skapat en Fragmentera kartan manager, du kan hämta hello [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) eller [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) med hello [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [ TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), eller hello [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metod.

    /// <summary>
    /// Creates a new Range Shard Map with hello specified name, or gets hello Range Shard Map if it already exists.
    /// </summary>
    public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
    {
        // Try tooget a reference toohello Shard Map.
        RangeShardMap<T> shardMap;
        bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

        if (shardMapExists)
        {
            ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
        }
        else
        {
            // hello Shard Map does not exist, so create it
            shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
            ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
        }

        return shardMap;
    } 

### <a name="shard-map-administration-credentials"></a>Fragmentera kartan administration autentiseringsuppgifter
Program som administrera och ändra Fragmentera maps skiljer sig från de som använder hello Fragmentera maps tooroute anslutningar. 

tooadminister Fragmentera mappar (lägga till eller ändra shards, Fragmentera kartor, Fragmentera mappningar osv) måste du initiera hello **ShardMapManager** med **autentiseringsuppgifter som har behörighet för båda hello GSM databasen och på varje för läsning och skrivning databasen som fungerar som en Fragmentera**. hello autentiseringsuppgifter måste tillåta skrivningar mot hello tabeller i båda hello GSM och LSM som Fragmentera kartan information anges eller ändras, samt som skapar tabeller för LSM på nya delar.  

Se [autentiseringsuppgifter används tooaccess hello-klientbibliotek för elastisk databas](sql-database-elastic-scale-manage-credentials.md).

### <a name="only-metadata-affected"></a>Endast de metadata som påverkas
Metoder som används för att fylla eller ändra hello **ShardMapManager** data inte ändrar hello data som lagras i hello shards sig själva. Till exempel metoder som **CreateShard**, **DeleteShard**, **UpdateMapping**osv påverkar hello Fragmentera kartan metadata. Ta inte bort, lägga till eller ändra användardata i hello delar. Dessa metoder är i stället utformad toobe används tillsammans med olika åtgärder du utföra toocreate eller ta bort faktiska databaser, eller att flytta rader från en Fragmentera tooanother toorebalance delat miljö.  (hello **delade dokument** verktyget som medföljer elastisk Databasverktyg gör användning av API: erna tillsammans med samordna faktiska dataflytten mellan shards.) Se [skalning hello elastisk databas dela sammanslagna verktyget](sql-database-elastic-scale-overview-split-and-merge.md).

## <a name="populating-a-shard-map-example"></a>Fyller på en karta Fragmentera-exempel
En Exempelsekvens med åtgärder toopopulate en specifik Fragmentera karta visas nedan. hello koden utför de här stegen: 

1. En ny Fragmentera karta skapas inom en Fragmentera kartan manager. 
2. hello metadata för två olika delar läggs toohello Fragmentera kartan. 
3. En mängd viktiga intervallet mappningar läggs och hello övergripande innehållet i hello Fragmentera karta visas. 

hello koden är skriven så att hello-metoden kan köras igen om ett fel inträffar. Varje begäran testar om en Fragmentera eller mappning finns redan, innan du försöker toocreate den. hello koden förutsätter att databaser med namnet **sample_shard_0**, **sample_shard_1** och **sample_shard_2** har redan skapats i hello-server som refereras av strängen **shardServer**. 

    public void CreatePopulatedRangeMap(ShardMapManager smm, string mapName) 
        {            
            RangeShardMap<long> sm = null; 

            // check if shardmap exists and if not, create it 
            if (!smm.TryGetRangeShardMap(mapName, out sm)) 
            { 
                sm = smm.CreateRangeShardMap<long>(mapName); 
            } 

            Shard shard0 = null, shard1=null; 
            // Check if shard exists and if not, 
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetShard(new ShardLocation(
                                     shardServer, 
                                     "sample_shard_0"), 
                                     out shard0)) 
            { 
                Shard0 = sm.CreateShard(new ShardLocation(
                                            shardServer, 
                                            "sample_shard_0")); 
            } 

            if (!sm.TryGetShard(new ShardLocation(
                                    shardServer, 
                                    "sample_shard_1"), 
                                    out shard1)) 
            { 
                Shard1 = sm.CreateShard(new ShardLocation(
                                             shardServer, 
                                            "sample_shard_1"));  
            } 

            RangeMapping<long> rmpg=null; 

            // Check if mapping exists and if not,
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetMappingForKey(0, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                          new RangeMappingCreationInfo<long>
                          (new Range<long>(0, 50), 
                          shard0, 
                          MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(50, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(50, 100), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(100, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long>
                         (new Range<long>(100, 150), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(150, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(150, 200), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(200, out rmpg)) 
            { 
               sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(200, 300), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            // List hello shards and mappings 
            foreach (Shard s in sm.GetShards()
                         .OrderBy(s => s.Location.DataSource)
                         .ThenBy(s => s.Location.Database))
            { 
               Console.WriteLine("shard: "+ s.Location); 
            } 

            foreach (RangeMapping<long> rm in sm.GetMappings()) 
            { 
                Console.WriteLine("range: [" + rm.Value.Low.ToString() + ":" 
                        + rm.Value.High.ToString()+ ")  ==>" +rm.Shard.Location); 
            } 
        } 

Som ett alternativ som du kan använda PowerShell-skript tooachieve hello resultat samma. Vissa hello exempel PowerShell-exemplen finns [här](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).     

När Fragmentera maps är ifyllda, data access-program kan skapas eller anpassade toowork med hello maps. Fylla eller ändra hello maps behöver inte uppstår igen förrän **kartan layout** måste toochange.  

## <a name="data-dependent-routing"></a>Databeroende routning
hello Fragmentera kartan manager används mest i program som kräver anslutningar tooperform hello app-specifik data databasåtgärder. Dessa anslutningar måste vara kopplad till hello rätt databas. Detta kallas **Data beroende routning**. Initiera en Fragmentera kartan manager objekt från hello factory med hjälp av autentiseringsuppgifter som har skrivskyddad åtkomst på hello GSM databas för dessa program. Enskilda förfrågningar för senare anslutningar ange autentiseringsuppgifter som krävs för att ansluta toohello lämplig Fragmentera databasen.

Observera att dessa program (med hjälp av **ShardMapManager** öppnas med skrivskyddad autentiseringsuppgifter) kan inte göra ändringar toohello maps eller mappningar. Skapa administrativa specifika program eller PowerShell-skript som anger högre Privilegierade autentiseringsuppgifter som tidigare diskuterats för dessa behov. Se [autentiseringsuppgifter används tooaccess hello-klientbibliotek för elastisk databas](sql-database-elastic-scale-manage-credentials.md).

Mer information finns i [Data beroende routning](sql-database-elastic-scale-data-dependent-routing.md). 

## <a name="modifying-a-shard-map"></a>Ändra en Fragmentera karta
En Fragmentera karta kan ändras på olika sätt. Alla följande metoder hello ändra hello metadata som beskriver hello delar och deras mappningar, men de kan inte ändra data i hello shards fysiskt eller gör de skapa eller ta bort hello faktiska databaser.  Vissa av hello åtgärder på hello Fragmentera karta som beskrivs nedan behöva toobe samordnas med administrativa åtgärder som fysiskt flytta data eller att lägga till och ta bort databaser som fungerar som delar.

Metoderna tillsammans fungerar som hello byggblock görs tillgängliga för att ändra hello övergripande fördelning av data i din databasmiljö för delat.  

* tooadd eller ta bort delar: använda  **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)**  och  **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)**  av hello [Shardmap klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx). 
  
    hello måste server och databas som representerar hello mål Fragmentera finnas för dessa åtgärder tooexecute. Dessa metoder har inte någon effekt på hello själva databaserna, endast för metadata i hello Fragmentera kartan.
* toocreate eller ta bort punkter eller intervall som är mappad toohello shards: Använd  **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**,  **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)**  av hello [RangeShardMapping klassen](https://msdn.microsoft.com/library/azure/dn807318.aspx), och  **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)**  av hello [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)
  
    Många olika punkter eller intervall kan vara mappade toohello samma Fragmentera. Dessa metoder påverkar endast metadata - de påverkar inte data som kanske redan finns i shards. Om data måste toobe tas bort från databasen hello i ordning toobe konsekvent med **DeleteMapping** åtgärder, behöver du tooperform dessa åtgärder separat men tillsammans med hjälp av dessa metoder.  
* toosplit befintliga områden i två eller merge intilliggande adressintervall till ett: använda  **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)**  och  **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.  
  
    Observera att dela och slå samman operations **inte ändra hello Fragmentera toowhich nyckel värden mappas**. En delning delar ett befintligt intervall i två delar, men lämnar både som mappade toohello samma Fragmentera. En koppling fungerar på två angränsande områden som är redan mappad toohello samma fragment, mottagarsidan dem till ett område.  hello flödet av punkter eller intervall själva mellan shards måste toobe samordnas med hjälp av **UpdateMapping** tillsammans med faktiska dataflytten.  Du kan använda hello **dela/Merge** tjänst som är en del av elastisk databas verktygen toocoordinate Fragmentera kartan ändringar med dataflyttning flytt krävs. 
* toore karta (eller flytta) enskilda punkter eller intervall toodifferent shards: Använd  **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.  
  
    Eftersom data kanske måste flyttas från en Fragmentera tooanother i ordning toobe konsekvent med toobe **UpdateMapping** åtgärder, behöver du tooperform som flytt separat men tillsammans med hjälp av dessa metoder.
* tootake mappningar online och offline: använda  **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)**  och  **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)**  toocontrol hello online tillståndet för en mappning. 
  
    Vissa åtgärder på Fragmentera mappningar tillåts endast när en mappning är i tillståndet ”offline” inklusive **UpdateMapping** och **DeleteMapping**. När en mappning är offline, returneras ett fel en data-beroende begäran baserat på en nyckel som ingår i mappningen. När ett intervall först kopplas från avslutats dessutom alla anslutningar toohello påverkas Fragmentera automatiskt i ordning tooprevent inkonsekventa eller ofullständiga resultat för frågor som riktar sig mot områden som ändras. 

Mappningar är oföränderliga objekt i .net.  Alla hello metoder ovan ändrar mappningar ogiltig även eventuella referenser toothem i koden. toomake it enklare tooperform sekvenser av åtgärder som ändrar tillstånd för en mappning, alla hello-metoder som ändrar en mappning returnera en ny mappning referens, så kan vara härledda åtgärder. Till exempel toodelete en befintlig mappning i shardmap sm som innehåller hello nyckeln 25, kan du köra hello följande: 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## <a name="adding-a-shard"></a>Lägga till en Fragmentera
Program ofta behöver toosimply lägga till nya shards toohandle data som förväntas av nya nycklar eller viktiga områden för en Fragmentera karta som redan finns. Till exempel ett delat program genom att klient-ID kan behöva tooprovision nya Fragmentera för en ny klient eller varje månad delat data måste en ny Fragmentera etableras innan hello början av varje ny månad. 

Om hello nytt intervall nyckel inte är en del av en befintlig mappning och inga data flyttas krävs, är det mycket enkelt tooadd hello nya Fragmentera och associera hello ny nyckel eller intervallet toothat Fragmentera. Mer information om att lägga till nya shards finns [att lägga till en ny Fragmentera](sql-database-elastic-scale-add-a-shard.md).

För scenarier som kräver dataflyttning är hello dela merge tool dock nödvändiga tooorchestrate hello dataförflyttning mellan shards i kombination med hello nödvändiga Fragmentera kartan uppdateringar. Information om hur du använder hello yool dela dokument, finns [översikt över delade dokument](sql-database-elastic-scale-overview-split-and-merge.md) 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
