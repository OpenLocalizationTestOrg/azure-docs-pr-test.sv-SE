---
title: "aaaMoving data mellan databaser som skalats ut molntjänster | Microsoft Docs"
description: "Förklarar hur toomanipulate delar och flytta data via en automatisk värdbaserade tjänst med elastisk databas API: er."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 204fd902-0397-4185-985a-dea3ed7c7d9f
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 629dee06e22609e9b61edf93ba5c38d997132d8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="moving-data-between-scaled-out-cloud-databases"></a>Flytta data mellan utskalade molndatabaser
Om du är en programvara som en tjänst-utvecklare och plötsligt enorm begäran görs i appen, måste tooaccommodate hello tillväxt. Så får du lägga till flera databaser (shards). Hur du distribuera hello data toohello nya databaser utan att störa hello dataintegritet? Använd hello **för delade sökvägssammanslagning** toomove data från begränsad databaser toohello nya databaser.  

hello delade dokument verktyget körs som en Azure-webbtjänst. En administratör eller utvecklare använder hello verktyget toomove shardlets (data från en Fragmentera) mellan olika databaser (shards). hello verktyget använder Fragmentera kartan management toomaintain hello service metadata-databasen och säkerställa konsekvent mappningar.

![Översikt][1]

## <a name="download"></a>Ladda ned
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)

## <a name="documentation"></a>Dokumentation
1. [Elastisk dela Merge tool självstudie om databaser](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
2. [Dela dokument säkerhetskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md)
3. [Dela dokument säkerhetsaspekter](sql-database-elastic-scale-split-merge-security-configuration.md)
4. [Karthantering för shard](sql-database-elastic-scale-shard-map-management.md)
5. [Migrera befintliga databaser tooscale ut](sql-database-elastic-convert-to-use-elastic-tools.md)
6. [Elastisk Databasverktyg](sql-database-elastic-scale-introduction.md)
7. [Ordlista för verktyg för elastisk databas](sql-database-elastic-scale-glossary.md)

## <a name="why-use-hello-split-merge-tool"></a>Varför använda hello dela merge tool?
**Flexibilitet**

Program behöver toostretch flexibelt utöver hello gränsvärden för en enskild Azure SQL DB-databas. Använd hello verktyget toomove data som behövs toonew databaser samtidigt integritet.

**Dela toogrow** 

Du behöver tooincrease övergripande kapacitet toohandle enorm tillväxt. toodo skapa ytterligare kapacitet så genom delning hello data och distribuera den över inkrementellt flera databaser tills kapacitetsbehov är uppfyllda. Detta är ett typiskt exempel hello-split-funktionen. 

**Sammanfoga tooshrink**

Kapacitet måste krympa på grund av hur toohello när en organisation. hello verktyg kan du skala ned toofewer skalenheter när business långsammare. hello '' utskrift i hello elastisk skalbarhet split-kopplingstjänsten beskriver det här kravet. 

**Hantera anslutningar genom att flytta shardlets**

Med flera innehavare per databas leda hello fördelning av shardlets tooshards toocapacity flaskhalsar på vissa delar. Detta kräver omfördelning shardlets eller flytta upptagen shardlets toonew eller mindre utnyttjade delar. 

## <a name="concepts--key-features"></a>Begrepp & viktiga funktioner
**Kund-värdbaserade tjänster**

hello dela sammanslagna levereras som en kund-värdbaserad tjänst. Du måste distribuera och värd hello-tjänsten i din Microsoft Azure-prenumeration. hello-paket som du hämtar från NuGet innehåller en konfiguration mallen toocomplete med hello information för din distribution. Se hello [delade dokument kursen](sql-database-elastic-scale-configure-deploy-split-and-merge.md) mer information. Eftersom hello-tjänsten körs i din Azure-prenumeration kan du styra och konfigureras säkerhet hello service. hello standardmallen innehåller hello alternativ tooconfigure SSL, certifikatbaserad klientautentisering, kryptering för lagrade autentiseringsuppgifter, DoS skyddar och IP-begränsningar. Du hittar mer information om hello säkerhetsaspekter i följande dokument hello [delade dokument säkerhetskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md).

hello standard distribuerade tjänsten körs med en arbetare och en webbroll. Var används hello A1 VM-storlek i Azure-molntjänster. Du inte kan ändra dessa inställningar när du distribuerar hello paketet, kan du ändra dem efter en lyckad distribution i hello kör molnbaserad tjänst (via hello Azure-portalen). Observera att hello worker-rollen inte måste konfigureras för mer än en instans av tekniska skäl. 

**Fragmentera kartan integrering**

hello split-kopplingstjänsten samverkar med hello Fragmentera karta över hello program. När du använder hello dela kopplingstjänsten toosplit eller merge-adressintervall eller toomove shardlets mellan shards, håller hello service automatiskt hello Fragmentera kartan in toodate. toodo så hello-tjänsten ansluter toohello Fragmentera kartan manager-databasen av programmet hello och underhåller intervall och mappningar som delade/merge/move-begäran pågår. Detta säkerställer som hello Fragmentera mappar alltid anger aktuell när ska dela dokument. Dela, implementeras dokument och shardlet förflyttning genom att flytta en batch med shardlets från hello källa Fragmentera toohello mål Fragmentera. Under hello shardlet flytt åtgärden hello shardlets ämne toohello aktuell batch är markerad som offline i hello Fragmentera mappning och är inte tillgängligt för data-beroende anslutningar med hello **OpenConnectionForKey** API. 

**Konsekvent shardlet anslutningar**

Dataflyttning att starta en ny grupp med shardlets, alla angivna Fragmentera mappning data beroende routning anslutningar toohello Fragmentera lagra hello shardlet finns avlivade och efterföljande anslutningar från hello Fragmentera kartan API: er toohello dessa shardlets blockeras medan hello dataflyttning pågår i ordning tooavoid inkonsekvenser. Anslutningar tooother shardlets på hello samma Fragmentera också hämta avslutats, men kommer att lyckas igen omedelbart på försök igen. När hello batch flyttas hello shardlets markeras online igen för hello mål Fragmentera och hello källdata tas bort från hello källa Fragmentera. hello tjänsten går igenom de här stegen för varje batch tills alla shardlets har flyttats. Detta leder tooseveral anslutningsåtgärder kill under hello hello fullständig move-dela/merge-åtgärd.  

**Hantera shardlet tillgänglighet**

Begränsa hello anslutningen om du avbryter toohello aktuell batch för shardlets som beskrivs ovan begränsar hello omfattning otillgänglighet tooone batch med shardlets i taget. Detta är att föredra över en metod där hello fullständig Fragmentera är offline för alla shardlets under hello loppet av en delad eller merge-åtgärd. hello storleken på en grupp som definierats som hello antalet distinkta shardlets toomove samtidigt, är en konfigurationsparameter. Det kan definieras för varje dela och slå samman åtgärd beroende på behov av hello programmets tillgänglighet och prestanda. Observera att hello-intervall som håller på att låsas i hello Fragmentera mappning kan vara större än hello batchstorlek anges. Det beror på att hello service hämtar hello intervallet storlek så att hello faktiska horisontell partitionering nyckelvärdena i hello data cirka matchar hello batchstorlek. Detta är särskilt viktigt tooremember för glesbefolkade horisontell partitionering nycklar. 

**Utrymmet för metadata**

hello split-kopplingstjänsten använder en databas toomaintain status och tookeep loggar under bearbetning av begäran. hello användaren i prenumerationen skapas denna databas och ger hello anslutningssträngen för den i hello konfigurationsfilen för hello service-distributionen. Administratörer från hello användarens organisation kan också ansluta toothis databasen tooreview begäran förlopp och tooinvestigate detaljerad information om potentiella problem.

**Horisontell partitionering medvetenhet**

hello split-kopplingstjänsten skiljer mellan (1) delat tabeller och (2) referenstabellerna (3) normal tabeller. hello semantiken för en move-dela/merge-åtgärd beror på hello hello-tabell som används och definieras enligt följande: 

* **Delat tabeller**: dela dokument och flytta flytta shardlets från källan tootarget Fragmentera. Efter slutförd hello övergripande begära dessa shardlets inte längre finns på hello källan. Observera att hello måltabeller måste tooexist på hello mål Fragmentera och får inte innehålla data i hello mål intervallet tidigare tooprocessing av hello-åtgärden. 
* **Refererar till tabeller**: för register, hello delning, slå samman och flytta operations kopiera hello data från hello källa toohello mål Fragmentera. Observera att inga ändringar inträffar på hello mål Fragmentera för en viss tabell om en rad finns redan i den här tabellen på hello målet. hello-tabellen har toobe tomt för alla referens tabell Kopiera åtgärden tooget bearbetas.
* **Andra tabeller**: andra tabeller kan finnas på hello käll- eller hello målet för en delning och merge-åtgärd. hello split-kopplingstjänsten ignorerar dessa tabeller för dataflyttning eller kopiering. Observera att de stör dessa åtgärder vid begränsningar.

hello-information för referens kontra delat tabeller tillhandahålls av hello **SchemaInfo** API: er på hello Fragmentera karta. hello som följande exempel visar hello användning av dessa API: er på en viss Fragmentera kartan manager objektet smm: 

    // Create hello schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

hello tabeller 'region' och 'landet' har definierats som referenstabellerna och kommer att kopieras med move-dela/merge-åtgärder. ”kund” och 'order' definieras i sin tur som delat tabeller. C_CUSTKEY och O_CUSTKEY fungera som hello horisontell partitionering nyckel. 

**Referensintegritet**

hello split-kopplingstjänsten analyserar alla beroenden mellan tabellerna och använder primära nyckel sekundärnyckelrelationer toostage hello åtgärder för att flytta referenstabellerna och shardlets. I allmänhet kopieras referenstabellerna först i beroendeordningen, och sedan shardlets kopieras i den ordning de beroenden inom varje grupp. Detta är nödvändigt så att gäller FK PK begränsningar på hello mål Fragmentera som anländer hello nya data. 

**Fragmentera kartan konsekvens och eventuell slutförande**

I hello närvaro fel, hello split-kopplingstjänsten återupptar åtgärder efter eventuella avbrott och syftar toocomplete någon i förloppet begäranden. Det kan dock finnas oåterkalleligt situationer, t.ex. när hello mål Fragmentera tappas bort eller komprometteras inte repareras. Under dessa omständigheter kan vissa shardlets som skulle flyttas toobe fortsätta tooreside på hello källa Fragmentera. hello service garanterar att shardlet mappningar uppdateras bara efter hello nödvändiga data har korrekt kopierade toohello mål. Shardlets bara ta bort på hello källan när alla data har kopierats toohello mål och hello motsvarande mappningar har uppdaterats. hello borttagningsåtgärden avbryts sker i bakgrunden hello när hello intervallet är redan online på hello mål Fragmentera. hello split-kopplingstjänsten garanterar alltid hello avbildningar lagras i hello Fragmentera mappningen är korrekt.

## <a name="hello-split-merge-user-interface"></a>användargränssnittet för hello delade dokument
hello dela kopplingstjänsten paketet innehåller en arbetsroll och en webbroll. Hej webbroll begärs används toosubmit delade dokument på ett interaktivt sätt. hello huvudkomponenterna i hello användargränssnittet är följande:

* Åtgärdstyp av: hello åtgärdstypen är en alternativknapp som styr hello typ av åtgärd som utförs av hello-tjänsten för denna begäran. Du kan välja mellan hello dela, slå samman och flytta scenarier. Du kan också avbryta en tidigare skickade. Du kan använda delade, slå samman och flytta begäranden för intervallet Fragmentera maps. Lista Fragmentera mappar endast stöd för move-åtgärder.
* Fragmentera mappa: hello nästa avsnitt av begäran parametrar omfattar information om hello Fragmentera karta och hello databasen värd Fragmentera kartan. I synnerhet måste tooprovide hello namn hello Azure SQL Database-server och databas som värd för hello shardmap, autentiseringsuppgifter tooconnect toohello Fragmentera mappa databasen och slutligen hello namnet på hello Fragmentera kartan. För närvarande accepterar hello åtgärden bara en enda uppsättning autentiseringsuppgifter. Dessa autentiseringsuppgifter måste toohave behörighet tooperform ändras toohello Fragmentera kartan samt toohello användardata på hello delar.
* Käll-adressintervall (dela och sammanfoga): en åtgärd för dela och slå samman bearbetar ett intervall med den lägsta och högsta nyckeln. toospecify en åtgärd med ett unbounded hög nyckelvärde, kontrollera hello ”övre nyckel är max” kryssrutan och lämna hello hög fält tomt. hello intervallet nyckelvärden som du anger göra måste tooprecisely matchar inte en mappning och dess gränser i kartan Fragmentera. Om du inte anger någon adressintervallsgränser alls kommer hello tjänsten härleda hello närmaste intervall du automatiskt. Du kan använda hello GetMappings.ps1 PowerShell tooretrieve hello aktuella skriptmappningar i en given Fragmentera karta.
* Dela källa beteende (delning): för dela-åtgärder, definiera hello punkt toosplit hello käll-adressintervall. Det gör du genom att tillhandahålla hello horisontell partitionering nyckeln där du vill att hello dela toooccur. Använd hello knappen Ange om du vill att hello nedre delen av hello intervallet (exklusive hello dela nyckel) toomove eller om du vill att hello övre delen toomove (inklusive hello delad nyckel).
* Datakällan Shardlet (flytta): flytta operations skiljer sig från delade eller merge-åtgärder eftersom de inte kräver en intervallet toodescribe hello källa. En källa för flytta identifieras bara av hello horisontell partitionering nyckelvärdet du planerar toomove.
* Rikta Fragmentera (delning): när du har angett hello information i hello källa split-åtgärden, måste toodefine där du vill att hello data toobe kopieras tooby tillhandahåller hello Azure SQL Db-server och databasnamnet för hello mål.
* Målområdet (merge): Merge-operationer flytta shardlets tooan befintliga Fragmentera. Du kan identifiera hello befintliga Fragmentera genom att tillhandahålla hello adressintervallsgränser av befintliga hello-intervall som du vill toomerge med.
* Batchstorlek: hello batch storlek kontroller hello antalet shardlets försätts offline i taget under hello dataflyttning. Detta är ett heltalsvärde där du kan använda mindre värden när du är känsliga toolong stilleståndsperioder för shardlets. Högre värden ökar hello tid som en given shardlet är offline men kan förbättra prestanda.
* Åtgärds-Id (Avbryt): Om du har en pågående åtgärd som inte längre behövs, du kan avbryta hello genom att ange dess åtgärds-ID i det här fältet. Du kan hämta hello åtgärds-ID från hello begäran statustabellen (se avsnittet 8.1) eller från hello utdata i hello webbläsare där du skickade begäran om hello.

## <a name="requirements-and-limitations"></a>Krav och begränsningar
hello aktuella implementationen av hello split-kopplingstjänsten är ämne toohello följande krav och begränsningar: 

* hello delar måste tooexist och registreras i hello Fragmentera mappningen innan en delad merge-åtgärd på dessa delar kan utföras. 
* hello-tjänsten kan inte skapa tabeller eller andra databasobjekt automatiskt som en del av dessa åtgärder. Det betyder att hello schemat för alla delat tabeller och refererar till tabeller måste tooexist på hello mål Fragmentera tidigare tooany dela/merge/move-åtgärd. Delat tabeller är särskilt krävs toobe tomt i hello intervallet där nya shardlets är toobe som lagts till av en move-dela/merge-åtgärd. Annars misslyckas åtgärden hello hello inledande konsekvenskontroll på hello mål Fragmentera. Observera referensdata kopieras bara om hello referenstabellen är tom och att det inte finns några konsekvens garanterar även med beaktande tooother samtidiga skrivåtgärder på hello register. Vi rekommenderar detta: när du kör dela/merge-operationer kan inga andra skrivåtgärder ändrar toohello register.
* hello tjänst förlitar sig på rad-ID som upprättas genom ett unikt index eller en nyckel som innehåller hello horisontell partitionering viktiga tooimprove prestanda och tillförlitlighet för stora shardlets. Detta gör att hello service toomove data på ett även ökad detaljnivå än bara hello horisontell partitionering nyckel/värde. Detta hjälper tooreduce hello maximal mängd loggutrymme och lås som krävs under hello-åtgärd. Överväg att skapa ett unikt index eller primärnyckel inklusive hello horisontell partitionering nyckel för en given tabell om du vill toouse tabellen med delade/merge/move-begäranden. Av prestandaskäl ska hello horisontell partitionering nyckeln hello inledande kolumn i hello nyckel eller hello index.
* Under hello behandling av begäranden, kanske vissa shardlet data finns både på hello käll- och hello mål Fragmentera. Detta är nödvändigt tooprotect mot fel under hello shardlet förflyttning. hello integrering av dela dokument med hello Fragmentera karta garanterar att anslutningar via hello data beroende routning API: er med hjälp av hello **OpenConnectionForKey** metod på hello Fragmentera karta inte ser alla mellanliggande inkonsekvent tillstånd. Men när anslutande toohello käll- eller hello mot shards utan att använda hello **OpenConnectionForKey** metoden inkonsekvent mellanliggande tillstånd kanske visas när du ska dela/merge/move-begäranden. Dessa anslutningar kan visa helt eller delvis dubbla resultat beroende på hello tidsinställning eller hello Fragmentera underliggande hello anslutningen. Den här begränsningen innehåller för närvarande hello anslutningar av elastisk skalbarhet flera-Shard-frågor.
* hello metadata-databasen för hello split-kopplingstjänsten måste inte delas mellan olika roller. Till exempel måste en roll hello delade dokument service körs i Förproduktion toopoint tooa olika metadata-databasen än hello produktion roll.

## <a name="billing"></a>Fakturering
hello dela merge-tjänsten körs som en tjänst i molnet i Microsoft Azure-prenumerationen. Därför gäller avgifter för molntjänster tooyour hello service-instans. Om du ofta utför move-dela/merge-åtgärder rekommenderar vi att du tar bort dina delade dokument Molntjänsten. Som kostnaderna för att köra eller distribueras molnet tjänstinstanser. Du kan distribuera och starta lätt körbara konfigurationen när du behöver tooperform split- eller merge-åtgärder. 

## <a name="monitoring"></a>Övervakning
### <a name="status-tables"></a>Status för tabeller
hello dela kopplingstjänsten ger hello **RequestStatus** tabell i databasen hello metadata lagring för övervakning av slutfördes och en pågående begäranden. hello tabell innehåller en rad för varje delad kopplingsbegäran som har skickats toothis instans av hello split-kopplingstjänsten. Den ger hello följande information för varje begäran:

* **Tidsstämpel**: hello tid och datum när hello begäran startades.
* **Åtgärds-ID**: ett GUID som unikt identifierar hello-begäran. Denna begäran kan också vara används toocancel hello åtgärden medan det pågår fortfarande.
* **Status för**: hello hello begäran aktuella tillstånd. För pågående begäranden dessutom visas hello Aktuell fas i vilken hello-begäran är.
* **CancelRequest**: en flagga som anger om hello begäran har avbrutits.
* **Förlopp**: en del uppskattning av hello-åtgärden har slutförts. Ett värde på 50 anger att hello åtgärden är klar ungefär 50%.
* **Information om**: en XML-värde som innehåller en mer detaljerad rapport. Hej förloppsrapport uppdateras regelbundet som anger rader kopieras från källan tootarget. Den här kolumnen innehåller också mer detaljerad information om hello fel vid fel eller undantag.

### <a name="azure-diagnostics"></a>Azure Diagnostics
hello split-kopplingstjänsten använder Azure-diagnostik baserat på Azure SDK 2.5 för övervakning och diagnostik. Du kan styra hello diagnostik konfigurationen som beskrivs här: [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](../cloud-services/cloud-services-dotnet-diagnostics.md). hello hämtningspaketet innehåller två konfigurationer av diagnostik - en för hello-webbroll och en för hello worker-rollen. Konfigurationerna diagnostik för hello tjänsten följer hello vägledning från [Cloud Service grunderna i Microsoft Azure](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Den omfattar hello definitioner toolog prestandaräknare, IIS loggar, Windows-händelseloggar och dela dokument, program-händelseloggar. 

## <a name="deploy-diagnostics"></a>Distribuera diagnostik
tooenable övervakning och diagnostik använder hello diagnostikkonfiguration för hello webb- och arbetsroller roller som tillhandahålls av hello NuGet-paketet, kör följande kommandon med hjälp av Azure PowerShell hello: 

    $storage_name = "<YourAzureStorageAccount>" 

    $key = "<YourAzureStorageAccountKey" 

    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  


    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 


    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Du hittar mer information om hur tooconfigure och distribuera här diagnostikinställningar: [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Hämta diagnostik
Du kan enkelt komma åt din diagnostik från hello Visual Studio Server Explorer i hello Azure en del av hello Server Explorer träd. Öppna en Visual Studio-instans och i hello menyraden klickar du på Visa och Server Explorer. Klicka på hello Azure ikonen tooconnect tooyour Azure-prenumeration. Gå sedan tooAzure -> Storage -> <your storage account> -> tabeller -> WADLogsTable. Mer information finns i [surfning lagringsresurser med Server Explorer](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

Hej WADLogsTable markerade i hello siffran ovan innehåller hello detaljerad händelser från hello delade dokument service programloggen. Observera att hello standardkonfigurationen av hello hämtade paketet riktar sig till en Produktionsdistribution. Därför är Hej då loggar och räknare hämtas från hello tjänstinstanser stor (5 minuter). Lägre hello intervall genom att justera hello diagnostikinställningarna för hello webb- eller hello worker-rollen tooyour måste för testning och utveckling. Högerklicka på hello roll i hello Visual Studio Server Explorer (se ovan) och justera sedan hello överföra Period i hello dialogrutan för hello diagnostikinställningarna för konfiguration: 

![Konfiguration][3]

## <a name="performance"></a>Prestanda
I allmänhet är bättre prestanda toobe förväntades från hello senare, mer performant tjänstnivåer i Azure SQL Database. Högre i/o-, processor- och minnesresurser tilldelningarna för hello högre tjänstnivåer dra hello masskopiering och delete-åtgärder som hello dela kopplingstjänsten använder. Av den anledningen bör öka hello tjänstnivån för dessa databaser för ett definierat begränsad tidsperiod.

hello-tjänsten utför även validering frågor som en del av dess normal drift. Frågorna verifiering för oväntat förekomsten av data i intervallet för hello mål och kontrollerar att alla delade/merge/flyttningsåtgärd startar från ett konsekvent tillstånd. De här frågorna alla arbeta via delning nyckelintervall definieras av hello omfattning hello åtgärden och hello batchstorlek tillhandahålls som en del av definitionen för hello förfrågningen. De här frågorna gör bäst ifrån sig när ett index som har hello horisontell partitionering nyckeln som hello inledande kolumn. 

Dessutom tillåter en unikhet egenskap med hello horisontell partitionering nyckel som hello inledande kolumnen hello service toouse en optimerad metod som begränsar förbrukning av nätverksresurser vad gäller loggutrymme och minne. Egenskapen unikhet är obligatoriska toomove stora datamängder (vanligtvis ovanför 1GB). 

## <a name="how-tooupgrade"></a>Hur tooupgrade
1. Gör så hello i [distribuera en delad kopplingstjänsten](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Ändra konfigurationsfilen cloud service för dina delade dokument distribution tooreflect hello nya konfigurationsparametrar. En ny obligatorisk parameter är hello information om hello-certifikat som används för kryptering. Ett enkelt sätt toodo detta är toocompare hello ny mall konfigurationsfil från hello hämta mot den befintliga konfigurationen. Kontrollera att du lägger till hello inställningar för ”DataEncryptionPrimaryCertificateThumbprint” och ”DataEncryptionPrimary” för både hello webb- och hello worker-rollen.
3. Se till att alla som körs dela merge-åtgärder har slutförts innan du distribuerar hello uppdatering tooAzure. Du kan göra detta genom att fråga hello RequestStatus och PendingWorkflows tabeller i hello dela sammanslagna metadata-databasen för pågående begäranden.
4. Uppdatera den befintliga cloud service-distributionen för att dela dokument i Azure-prenumerationen med hello nya paketet och uppdaterade konfigurationsfilen.

Du behöver inte tooprovision en ny databas för metadata för delade dokument tooupgrade. hello nya versionen uppgraderas automatiskt din befintliga metadata toohello nya databasversionen. 

## <a name="best-practices--troubleshooting"></a>Bästa praxis och felsöka
* Definiera en Testklient och utnyttja dina viktigaste move-dela/merge-åtgärder med hello testklienten över flera delar. Se till att alla metadata har definierats korrekt i Fragmentera map och att hello operations inte strider mot villkor eller sekundärnycklar.
* Behåll hello test klient datastorleken ovan hello maximal storlek för din största klient tooensure datastorleken inte uppstår problem som rör. Detta hjälper dig att utvärdera en övre gräns på hello tid det tar toomove en enskild klient runt. 
* Kontrollera att schemat tillåter borttagning. hello split-kopplingstjänsten kräver hello möjlighet tooremove data från hello källa Fragmentera när hello data har korrekt kopierade toohello mål. Till exempel **ta bort utlösare** kan förhindra hello tjänsten från att ta bort hello data på hello källan och kan orsaka operations toofail.
* hello horisontell partitionering nyckel måste vara hello inledande kolumn i din primär nyckel eller ett unikt Indexdefinition. Som säkerställer hello bästa prestanda för hello delade eller merge frågor för verifiering och för hello faktiska data movement och borttagning åtgärder som använder alltid nyckelintervall för horisontell partitionering.
* Samordna split-kopplingstjänsten i hello region och datacenter där databaserna finns. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png

