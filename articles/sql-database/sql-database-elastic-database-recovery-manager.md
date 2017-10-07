---
title: aaaUsing Recovery Manager toofix Fragmentera mappa problem | Microsoft Docs
description: "Använd hello RecoveryManager klassen toosolve problem med Fragmentera maps"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 45520ca3-6903-4b39-88ba-1d41b22da9fe
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: ddove
ms.openlocfilehash: 2218fb15122f1df466e65483480461e366317f2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-recoverymanager-class-toofix-shard-map-problems"></a>Med hjälp av hello RecoveryManager klassen toofix Fragmentera kartan problem
Hej [RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) klassen innehåller hello möjlighet tooeasily identifiera och åtgärda eventuella inkonsekvenser mellan hello globala Fragmentera karta (GSM) och hello lokala Fragmentera kartan (LSM) i en databasmiljö med delat ADO.Net-program. 

hello GSM och LSM spåra hello mappningen för varje databas i ett delat. Ibland kan sker en paus mellan hello GSM och hello LSM. I så fall använder hello RecoveryManager klassen toodetect och reparera hello break.

Hej RecoveryManager klassen är en del av hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md). 

![Fragmentera karta][1]

Termdefinitioner finns [elastisk databas verktyg ordlista](sql-database-elastic-scale-glossary.md). hur hello toounderstand **ShardMapManager** används toomanage data i ett delat lösning finns [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md).

## <a name="why-use-hello-recovery-manager"></a>Varför använda hello recovery manager?
I en databasmiljö med delat finns en klient per databas och många databaser per server. Det kan också finnas flera servrar i hello-miljö. Varje databas mappas i hello Fragmentera mappning så anrop kan vara routade toohello rätt server och databas. Databaser spåras enligt tooa **horisontell partitionering nyckeln**, och varje Fragmentera har tilldelats en **viktiga värdeintervallet**. Till exempel kan en nyckel för horisontell partitionering representera hello kundnamn från ”D” för ”F.” Hej mappningen av alla delar (aka databaser) och intervallen mappningen finns i hello **globala Fragmentera karta (GSM)**. Varje databas innehåller också en karta över hello-intervall som finns på hello Fragmentera som kallas hello **lokala Fragmentera karta (LSM)**. När en app ansluter tooa Fragmentera, cachelagras hello mappning med hello app för snabb hämtning. Hej LSM är används toovalidate cachelagrade data. 

hello kan GSM och LSM bli synkroniserad för hello följande orsaker:

1. hello borttagning av en Fragmentera vars intervallet antas toono längre vara används eller ändra namn på en Fragmentera. Om du tar bort en Fragmentera resulterar i en **frånkopplade Fragmentera mappning**. På liknande sätt kan har bytt namn till databasen orsaka en överbliven Fragmentera mappning. Beroende på hello syftet med hello ändring, hello Fragmentera behöva toobe tas bort eller hello Fragmentera placering måste toobe uppdateras. toorecover en borttagen databas finns [återställa en borttagen databas](sql-database-recovery-using-backups.md).
2. Geo-redundans inträffar. toocontinue, måste en uppdatera hello servernamnet och databasnamnet av Fragmentera kartan manager i hello programmet och sedan hello Fragmentera mappning information för alla delar i en Fragmentera karta. Om det finns en geo-redundans, bör sådana recovery logik automatiseras hello redundans arbetsflödet. Automatisera återställningsåtgärder aktiverar ett friktionsfritt hanterbarhet för geo-aktiverade databaser och undviker mänsklig manuella åtgärder. toolearn om alternativ toorecover en databas om ett avbrott för data center, se [företagskontinuitet](sql-database-business-continuity.md) och [Disaster Recovery](sql-database-disaster-recovery.md).
3. En Fragmentera eller hello ShardMapManager databasen är återställda tooan tidigare punkt i tiden. toolearn om punkten i tidsåterställningen med hjälp av säkerhetskopior, se [återställning med hjälp av säkerhetskopior](sql-database-recovery-using-backups.md).

Mer information om Azure SQL Database elastisk Databasverktyg, geo-replikering och återställning finns i hello följande: 

* [Översikt: Molnet business continuity och databasen katastrofåterställning med SQL-databas](sql-database-business-continuity.md) 
* [Kom igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md)  
* [Hantering av ShardMap](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Hämtar RecoveryManager från en ShardMapManager
hello första steget är toocreate en RecoveryManager-instans. Hej [GetRecoveryManager metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) returnerar hello recovery manager för hello aktuella [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) instans. tooaddress eventuella inkonsekvenser i hello Fragmentera mappa, måste du först hämta hello RecoveryManager för hello viss Fragmentera kartan. 

   ```
    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 
   ```

I det här exemplet hello RecoveryManager har initierats från hello ShardMapManager. Hej ShardMapManager som innehåller en ShardMap har också redan initierats. 

Eftersom den här programkod manipulerar hello Fragmentera kartan själva, hello autentiseringsuppgifter i hello fabriksmetoden (i föregående exempel hello smmConnectionString) ska vara autentiseringsuppgifter som har skrivskyddad behörighet på hello GSM databasen som refereras av hello anslutningssträng. Autentiseringsuppgifterna är vanligtvis skiljer sig från autentiseringsuppgifterna tooopen anslutningar för data-beroende routning. Mer information finns i [med autentiseringsuppgifter i hello elastisk databas klienten](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-hello-shardmap-after-a-shard-is-deleted"></a>Ta bort en Fragmentera från hello ShardMap efter en Fragmentera har tagits bort
Hej [DetachShard metoden](https://msdn.microsoft.com/library/azure/dn842083.aspx) tar bort hello får Fragmentera från hello Fragmentera kartan och tar bort mappningar som är associerade med hello Fragmentera.  

* Hej platsparametern är hello Fragmentera plats, särskilt servernamnet och databasnamnet på hello Fragmentera som oberoende. 
* Hej shardMapName parametern är hello Fragmentera mappningsnamn. Detta är endast krävs när flera Fragmentera maps hanteras av hello samma Fragmentera kartan manager. Valfri. 


> [!IMPORTANT]
> Använd den här tekniken endast om du är säker på att hello-intervall för hello uppdateras mappningen är tom. Kontrollera hello metoderna ovan inte data för hello-intervall som ska flyttas, så det är bäst tooinclude kontrollerar i kod.
>

Det här exemplet tar bort shards från hello Fragmentera kartan. 

   ```
   rm.DetachShard(s.Location, customerMap);
   ``` 

hello Fragmentera karta visar hello Fragmentera plats i hello GSM före hello borttagningen av hello Fragmentera. Eftersom hello Fragmentera har tagits bort, förutsätts det var avsiktlig och hello horisontell partitionering viktiga intervallet är inte längre används. Om inte, kan du köra punkt i tidpunkt för återställning. toorecover hello Fragmentera från en tidigare punkt i tiden. (I så fall granska hello följande avsnitt toodetect Fragmentera inkonsekvenser.) toorecover, se [punkten i tidsåterställningen](sql-database-recovery-using-backups.md).

Eftersom det förutsätts hello databasadministratörer att ta bort var avsiktlig hello slutliga administrativa Rensa åtgärden är toodelete hello post toohello Fragmentera i hello Fragmentera kartan manager. Detta förhindrar att hello program oavsiktligt skriver information tooa område som inte förväntas.

## <a name="toodetect-mapping-differences"></a>toodetect mappning skillnader
Hej [DetectMappingDifferences metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) markerar och returnerar ett av hello Fragmentera maps (lokal eller global) som sanningen hello datakälla och synkroniserar mappningar på båda Fragmentera maps (GSM och LSM).

   ```
   rm.DetectMappingDifferences(location, shardMapName);
   ```

* Hej *plats* anger hello servernamnet och databasnamnet. 
* Hej *shardMapName* parametern är hello Fragmentera mappningsnamn. Detta är endast krävs om flera Fragmentera maps hanteras av hello samma Fragmentera kartan manager. Valfri. 

## <a name="tooresolve-mapping-differences"></a>tooresolve mappning skillnader
Hej [ResolveMappingDifferences metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) väljer ett av hello Fragmentera maps (lokal eller global) som sanningen hello datakälla och synkroniserar mappningar på båda Fragmentera maps (GSM och LSM).

   ```
   ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   ```

* Hej *RecoveryToken* parametern räknar hello skillnader i hello mappningar mellan hello GSM och hello LSM för hello specifika Fragmentera. 
* Hej [MappingDifferenceResolution uppräkningen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) är används tooindicate hello metod för att lösa hello skillnaden mellan hello Fragmentera mappningar. 
* **MappingDifferenceResolution.KeepShardMapping** när hello LSM innehåller hello korrekt mappning och därför hello mappning i hello Fragmentera ska användas. Detta gäller vanligtvis hello om det finns en växling vid fel: hello Fragmentera finns nu på en ny server. Eftersom hello Fragmentera måste först tas bort från hello GSM (metoden hello RecoveryManager.DetachShard), finns inte längre en mappning på hello GSM. Hej LSM måste därför vara används toore-upprätta hello Fragmentera mappning.

## <a name="attach-a-shard-toohello-shardmap-after-a-shard-is-restored"></a>Koppla en Fragmentera toohello ShardMap när en Fragmentera har återställts
Hej [AttachShard metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) bifogar hello angivna Fragmentera toohello Fragmentera kartan. Sedan identifierar inkonsekvenser Fragmentera kartan och uppdaterar hello mappningar toomatch hello Fragmentera hello punkt i hello Fragmentera återställningen. Det förutsätts att hello-databasen är också har bytt namn till tooreflect hello ursprungliga databasnamnet (innan hello Fragmentera återställdes), eftersom hello punkt i tiden återställningen som standard tooa ny databas läggas till med hello tidsstämpel. 

   ```
   rm.AttachShard(location, shardMapName)
   ``` 

* Hej *plats* parametern är hello servernamnet och databasnamnet på hello Fragmentera som kopplas. 
* Hej *shardMapName* parametern är hello Fragmentera mappningsnamn. Detta är endast krävs när flera Fragmentera maps hanteras av hello samma Fragmentera kartan manager. Valfri. 

Det här exemplet lägger till en Fragmentera toohello Fragmentera karta som nyligen har återställts från en tidigare punkt i tiden. Eftersom hello Fragmentera (dvs hello mappning för hello Fragmentera i hello LSM) har återställts, är det eventuellt inkonsekvent med hello Fragmentera post i hello GSM. Utanför den här exempelkoden hello Fragmentera har återställts och byta namn på toohello ursprungliga namnet på hello-databasen. Eftersom den har återställts, förutsätts hello mappning i hello LSM finns hello betrodda mappning. 

   ```
   rm.AttachShard(s.Location, customerMap); 
   var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 
   ```

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-hello-shards"></a>Uppdatera Fragmentera platser efter en geo-redundans (återställa) för hello shards
Om det finns en geo-redundans, hello sekundära databasen är åtkomlig skriva och blir hello nya primära databasen. Hej namnet hello server och potentiellt hello-databasen (beroende på din konfiguration), kan skilja sig från hello ursprungliga primära servern. Därför hello avbildningar för hello Fragmentera i hello GSM och LSM måste åtgärdas. På liknande sätt, om hello databasen är återställda tooa annat namn eller plats eller tooan tidigare tidpunkt, detta kan orsaka inkonsekvenser i hello Fragmentera maps. hello Fragmentera kartan Manager hanterar hello distribution av öppna anslutningar toohello rätt databas. Distribution baseras på hello data i hello Fragmentera kartan och hello värdet för hello horisontell partitionering nyckel som är hello mål för hello program begäran. Efter en geo-redundans, måste den här informationen uppdateras med hello korrekt servernamnet, databasnamnet och Fragmentera mappningen av hello återställda databasen. 

## <a name="best-practices"></a>Bästa praxis
GEO-redundans och återställning är åtgärder som vanligtvis hanteras av en administratör molnet för hello-program som använder en Azure SQL-databaser funktionerna för verksamhetskontinuitet avsiktligt. Kontinuitet planering kräver processer och procedurer åtgärder tooensure verksamheten kan fortsätta utan avbrott. Hej metoder tillgängliga som en del av hello RecoveryManager klass som ska användas i den här arbete flödet tooensure hello GSM och LSM hålls uppdaterade baserat på hello recovery åtgärd. Det finns fem grundläggande steg tooproperly säkerställt hello GSM och LSM återspeglar hello korrekt information efter en redundansväxling. Hej programmet kod tooexecute de här stegen kan integreras i befintliga verktyg och arbetsflöde. 

1. Hämta hello RecoveryManager från hello ShardMapManager. 
2. Koppla bort hello gamla Fragmentera från hello Fragmentera mappningen.
3. Koppla hello nya Fragmentera toohello Fragmentera karta, inklusive hello ny Fragmentera plats.
4. Identifiera inkonsekvenser i hello mappning mellan hello GSM och LSM. 
5. Lösa skillnader mellan hello GSM och hello LSM, betrodda hello LSM. 

Det här exemplet utför hello följande steg:

1. Tar bort shards från hello Fragmentera karta som avspeglar Fragmentera platser innan hello redundansväxlingen.
2. Bifogar shards toohello Fragmentera kartan reflekterande hello nya Fragmentera platser (hello parametern ”Configuration.SecondaryServer” är hello-serverns nya namn men hello samma databasnamn).
3. Hämtar hello recovery token med hjälp av mappning skillnader mellan hello GSM och hello LSM för varje Fragmentera. 
4. Löser hello inkonsekvenser av betrodda hello-mappning från hello LSM för varje Fragmentera. 
   
   ```
   var shards = smm.GetShards(); 
   foreach (shard s in shards) 
   { 
     if (s.Location.Server == Configuration.PrimaryServer) 
   
         { 
          ShardLocation slNew = new ShardLocation(Configuration.SecondaryServer, s.Location.Database); 
   
          rm.DetachShard(s.Location); 
   
          rm.AttachShard(slNew); 
   
          var gs = rm.DetectMappingDifferences(slNew); 
   
          foreach (RecoveryToken g in gs) 
            { 
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
            } 
        } 
    } 
   ```

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png

