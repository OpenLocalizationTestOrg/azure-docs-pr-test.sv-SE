---
title: aaaMigrate dina data tooSQL Data Warehouse | Microsoft Docs
description: "Tips för att migrera dina data tooAzure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: d78f954a-f54c-4aa4-9040-919bc6414887
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/29/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: fe4c6b7e82094c59c45e06be6da225fee1b707ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data"></a>Migrera dina Data
Data kan flyttas från olika källor till din SQL Data Warehouse med en olika verktyg.  ADF kopiera, SSIS och bcp kan alla vara används tooachieve det här målet. När hello mängden data ökar bör du tänka på bryta ned hello migrering av data i steg. Detta en hello möjlighet toooptimize varje steg både för prestanda och återhämtning tooensure smooth datamigrering.

Den här artikeln beskrivs först hello enkla Migreringsscenarier ADF kopia, SSIS och bcp. Den sedan studera lite djupare hur hello migrering kan optimeras.

## <a name="azure-data-factory-adf-copy"></a>Azure Data Factory (ADM) kopiera
[Kopiera ADF] [ ADF Copy] är en del av [Azure Data Factory][Azure Data Factory]. Du kan använda ADF kopiera tooexport dina tooflat filer som finns på lokal lagring tooremote flat-filer som lagras i Azure blob storage eller direkt till SQL Data Warehouse.

Om dina data startar i flat-filer, så måste du först tootransfer den tooAzure lagringsblob innan du påbörjar en inläsning till SQL Data Warehouse. När hello data överförs till Azure blob storage kan du välja toouse [ADF kopiera] [ ADF Copy] igen toopush hello data till SQL Data Warehouse.

PolyBase ger också ett alternativ för högpresterande för hello data läses in. Dock innebär som använder två verktyg i stället för en. Om du behöver hello bästa prestanda och Använd sedan PolyBase. Om du vill använda en enda verktyg upplevelse (inte och hello data massiv) är ADF svaret.


> 
> 

HEAD över toohello följande artikel för vissa bra [ADF prover][ADF samples].

## <a name="integration-services"></a>Integrationstjänster
Integration Services (SSIS) är ett kraftfullt och flexibelt extrahera Transform och Load (ETL) som stöder komplexa arbetsflöden, DTS och flera alternativ för inläsning av data. Använd SSIS toosimply överför data tooAzure eller som en del av en bredare migrering.

> [!NOTE]
> SSIS kan exportera tooUTF 8 utan hello byte-ordningsmarkering i hello-filen. Detta måste du först använda hello härledda teckendata för kolumnen komponenten tooconvert hello i tooconfigure hello data flödet toouse hello 65001 UTF-8-teckentabellen. När hello kolumner har konverterats skriva hello data toohello flat fil målet nätverkskort att säkerställa att 65001 också har markerats som hello teckentabell för hello-filen.
> 
> 

SSIS ansluter tooSQL datalagret precis som den ska ansluta tooa SQL Server-distributionen. Anslutningarna behöver toobe som använder en ADO.NET Anslutningshanteraren. Också noga tooconfigure hello ”Använd massinfogning när det är tillgängligt” inställningen toomaximize genomflöde. Se toohello [ADO.NET-målnätverkskort] [ ADO.NET destination adapter] artikel toolearn mer om den här egenskapen

> [!NOTE]
> Det går inte att ansluta tooAzure SQL Data Warehouse med hjälp av OLEDB.
> 
> 

Dessutom finns alltid hello möjligheten att ett paket kan misslyckas på grund av problem med toothrottling eller nätverket. Design paket så att de kan återupptas vid hello felpunkt utan göra om fungerar som slutförts innan hello misslyckades.

Mer information finns i hello [SSIS-dokumentationen][SSIS documentation].

## <a name="bcp"></a>bcp
BCP är ett kommandoradsverktyg som har utformats för flat fil dataimport och export. Vissa omvandling kan ske under export av data. tooperform enkel transformationer använder en fråga tooselect och transformera hello data. När exporteras, kan sedan hello flata filer laddas direkt i hello måldatabasen hello SQL Data Warehouse.

> [!NOTE]
> Det är ofta en bra idé tooencapsulate hello transformationer som används under data exportera i en vy på hello källsystemet. Detta säkerställer att hello logik sparas och hello processen repeterbara.
> 
> 

Fördelarna med bcp är:

* Enkelhet. BCP kommandon är enkla toobuild och köra
* Nytt starta inläsningen. En gång exporterade hello inläsningen kan vara körs många gånger

Begränsningar av bcp är:

* BCP fungerar med endast tabellform flata filer. Det fungerar inte med filer som XML- eller JSON
* Data transformation funktioner är begränsade toohello export endast mellanlagring och är enkel till sin natur
* BCP har inte anpassats toobe robust vid läsning av data över hello internet. Instabilitet nätverket kan orsaka en Inläsningsfel.
* BCP är beroende av hello schemat finns i hello mål databasen tidigare toohello belastning

Mer information finns i [använda bcp tooload data till SQL Data Warehouse][Use bcp tooload data into SQL Data Warehouse].

## <a name="optimizing-data-migration"></a>Optimera datamigrering
Migrerar en SQLDW data kan delas effektivt upp till tre separata steg:

1. Export av källdata
2. Överföring av data tooAzure
3. Läs in till hello måldatabasen SQLDW

Varje steg kan vara individuellt optimerade toocreate en robust, starta om och flexibel migreringsprocessen som maximerar prestandan i varje steg.

## <a name="optimizing-data-load"></a>Optimera datainläsning
Titta på dessa i omvänd ordning för ett ögonblick; hello snabbaste sättet tooload data är via PolyBase. Optimera för en PolyBase-inläsningen placeras krav hello föregående steg så att det är bästa toounderstand detta summa. De är:

1. Kodning av datafiler
2. Formatet för datafiler
3. Platsen för datafiler

### <a name="encoding"></a>Encoding
PolyBase kräver data filer toobe UTF-8- eller UTF-16FE. 



### <a name="format-of-data-files"></a>Formatet för datafiler
PolyBase kräver en fast radbegränsaren \n eller ny rad. Datafiler måste följa toothis standard. Det inte finns några begränsningar på avslutarna sträng eller en kolumn.

Du måste toodefine alla kolumner i hello-filen som en del av en extern tabell i PolyBase. Se till att alla exporterade kolumnerna är nödvändiga och att hello typer överensstämmer toohello krävs standarder.

Se tillbaka toohello [migrera schemat] artikeln för information om vilka datatyper.

### <a name="location-of-data-files"></a>Platsen för datafiler
SQL Data Warehouse använder uteslutande PolyBase tooload data från Azure Blob Storage. Följaktligen måste hello data ha först överförts till blob-lagring.

## <a name="optimizing-data-transfer"></a>Optimera dataöverföring
En av hello långsammaste delar av datamigrering är hello överföringen av hello data tooAzure. Inte bara nätverkets bandbredd kan bli ett problem utan också nätverket tillförlitlighet kan försvåra pågår. Migrera data tooAzure är över hello internet så hello risken för överföringen eventuella fel som inträffar rimligen kan som standard. Dessa fel kan dock kräva data toobe skickas på nytt helt eller delvis.

Lyckligtvis har du flera alternativ tooimprove hello hastighet och återhämtning i den här processen:

### <a name="expressrouteexpressroute"></a>[ExpressRoute][ExpressRoute]
Du kanske använder tooconsider [ExpressRoute] [ ExpressRoute] toospeed in hello-överföring. [ExpressRoute] [ ExpressRoute] ger dig med en privat upprättad anslutning tooAzure så hello anslutning inte går hello offentliga internet. Under inga omständigheter är ett obligatoriskt steg. Dock förbättras dataflöde vid push-installation tooAzure data från en lokal eller samplacering.

Hej fördelarna med att använda [ExpressRoute] [ ExpressRoute] är:

1. Ökad tillförlitlighet
2. Snabbare nätverkshastigheten
3. Lägre nätverksfördröjning
4. högre nätverkssäkerhet

[ExpressRoute] [ ExpressRoute] är bra för ett antal scenarier, inte bara hello migrering.

Är du intresserad? Mer information och priser du besöker hello [ExpressRoute dokumentation][ExpressRoute documentation].

### <a name="azure-import-and-export-service"></a>Azure Import och Export Service
hello är Azure importera och exportera Service en process för överföring av data utformad för stora (GB ++) toomassive (TB ++) överföringar av data till Azure. Det innebär att skriva data-toodisks och leverera dem tooan Azure-datacenter. hello diskinnehållet läses sedan in i Azure Storage Blobs å dina vägnar.

En översikt över för export av hello importen är följande:

1. Konfigurera en Azure Blob Storage-behållare tooreceive hello data
2. Exportera toolocal datalagringen
3. Kopiera hello data too3.5 tums SATA II/III hårddiskar med hello [Azure Import/Export verktyg]
4. Skapa ett importjobb med hello Azure importera och exportera tjänst som tillhandahåller hello journalfiler produceras av hello [Azure Import/Export verktyg]
5. Leverera hello diskar utsedda Azure datacentret
6. Dina data är överförda tooyour Azure Blob Storage-behållare
7. Läs in hello data till SQLDW med PolyBase

### <a name="azcopyazcopy-utility"></a>[AZCopy][AZCopy] verktyget
Hej [AZCopy][AZCopy] verktyget är ett bra verktyg för att hämta dina data som överförs till Azure Storage-Blobbar. Den är utformad för mindre (MB ++) toovery stora (GB ++) överföring av data. [AZCopy] har också utformad tooprovide bra flexibel dataflöde vid överföring av data tooAzure och det är ett bra alternativ för steget hello-överföring. En gång överförda kan du läsa in hello-data med PolyBase i SQL Data Warehouse. Du kan även inkludera AZCopy i din SSIS-paket med hjälp av en uppgift ”köra processen”.

toouse AZCopy du först måste toodownload och installera den. Det finns en [produktionsversion] [ production version] och en [förhandsversionen] [ preview version] tillgängliga.

tooupload en fil från din dator behöver du ett kommando som hello en nedan:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

En sammanfattning av övergripande processen kan vara:

1. Konfigurera ett Azure storage blob-behållaren tooreceive hello-data
2. Exportera toolocal datalagringen
3. AZCopy dina data i hello Azure Blob Storage-behållare
4. Läs in hello data till SQL Data Warehouse med PolyBase

Fullständig dokumentation tillgänglig: [AZCopy][AZCopy].

## <a name="optimizing-data-export"></a>Optimera export av data
Dessutom kan tooensuring att hello export överensstämmer toohello kraven av PolyBase du också söka toooptimize hello export av hello tooimprove hello av data ytterligare.



### <a name="data-compression"></a>Datakomprimering
PolyBase kan läsa gzip komprimerade data. Om du kan toocompress kommer toogzip datafiler du minimera hello mängden data som flyttas över hello nätverk.

### <a name="multiple-files"></a>Flera filer
Dela upp stora tabeller i flera filer hjälper inte bara tooimprove exportera hastighet, hjälper också med överföra re-startability och hello övergripande hanterbarhet hello data en gång i hello Azure blob storage. En av hello många bra funktioner i PolyBase är att den ska läsa alla hello-filer i en mapp och behandla det som en tabell. Därför är det en bra idé tooisolate hello filer för varje tabell till en egen mapp.

PolyBase stöder också en funktion som kallas ”rekursiv mappen traversal”. Du kan använda den här funktionen toofurther förbättra hello organisationen av din exporterade data tooimprove din datahantering.

toolearn mer information om hur du läser in data med PolyBase, se [Använd PolyBase tooload data till SQL Data Warehouse][Use PolyBase tooload data into SQL Data Warehouse].

## <a name="next-steps"></a>Nästa steg
Mer information om migrering finns i [migrera din lösning tooSQL datalagret][Migrate your solution tooSQL Data Warehouse].
För fler utvecklingstips, se [utvecklingsöversikt][development overview].

<!--Image references-->

<!--Article references-->
[AZCopy]:../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution tooSQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp tooload data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase tooload data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
