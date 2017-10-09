---
title: aaaDevelop lokalt med hello Azure Cosmos DB emulatorn | Microsoft Docs
description: "Med hello Azure Cosmos DB-emulatorn kan du utveckla och testa programmet lokalt för gratis, utan att skapa en Azure-prenumeration."
services: cosmos-db
documentationcenter: 
keywords: Azure Cosmos DB-emulatorn
author: arramac
manager: jhubbard
editor: 
ms.assetid: 90b379a6-426b-4915-9635-822f1a138656
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: arramac
ms.openlocfilehash: fb5449489e5f71664e72d8e11e583315be371bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cosmos-db-emulator-for-local-development-and-testing"></a>Använd hello Azure Cosmos DB-emulatorn för lokal utveckling och testning

<table>
<tr>
  <td><strong>Binärfiler</strong></td>
  <td>[Ladda ned MSI](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><strong>Docker</strong></td>
  <td>[Docker-hubb](https://hub.docker.com/r/microsoft/azure-documentdb-emulator/)</td>
</tr>
<tr>
  <td><strong>Docker-källa</strong></td>
  <td>[Github](https://github.com/azure/azure-documentdb-emulator-docker)</td>
</tr>
</table>
  
hello Azure Cosmos DB emulatorn tillhandahåller en lokal miljö som emulerar hello Azure DB som Cosmos-tjänsten för utveckling. Använda hello Azure Cosmos DB-emulatorn kan du utveckla och testa programmet lokalt, utan att skapa en Azure-prenumeration eller kostnader. När du är nöjd med hur programmet fungerar i hello Azure Cosmos DB-emulatorn kan växla du toousing ett Azure DB som Cosmos-konto i hello molnet.

Den här artikeln beskriver hello följande uppgifter: 

> [!div class="checklist"]
> * Installera hello emulatorn
> * Med Docker för Windows hello emulatorn
> * Autentisering av förfrågningar
> * Med hjälp av hello Data Explorer i hello emulatorn
> * Exportera SSL-certifikat
> * Anropar hello Emulator från hello kommandorad
> * Samla in spårningsfiler

Vi rekommenderar att komma igång genom att titta på hello följande video, där Kirill Gavrylyuk visar hur tooget igång med hello Azure Cosmos DB-emulatorn. Observera att hello video refererar toohello emulatorn som hello DocumentDB-emulatorn, men själva hello-verktyget har bytt hello Azure Cosmos DB emulatorn sedan in hello video. All information i hello video är korrekt för hello Azure Cosmos DB-emulatorn. 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-hello-emulator-works"></a>Så här fungerar hello emulatorn
hello Azure Cosmos DB emulatorn ger en hög återgivning emulering av hello Azure DB som Cosmos-tjänsten. Den stöder identiska funktioner som Azure Cosmos DB, inklusive stöd för att skapa och hämtning av JSON-dokument, etablering och skalning samlingar och köra lagrade procedurer och utlösare. Du kan utveckla och testa program med hjälp av hello Azure Cosmos DB-emulatorn och distribuera dem tooAzure på global nivå genom att bara göra en enda konfigurationsändring toohello Anslutningens slutpunkt för Azure Cosmos DB.

När vi har skapat en lokal emulering hög återgivning av hello faktiska Azure Cosmos DB service är hello implementering av hello Azure Cosmos DB-emulatorn än hello-tjänsten. Till exempel använder hello Azure Cosmos DB emulatorn standard OS-komponenter, till exempel hello lokalt filsystem för beständighet och HTTPS-protokoll-stacken för anslutning. Detta innebär att vissa funktioner som förlitar sig på Azure-infrastrukturen som globala replikering, en siffra millisekunds fördröjning för läsning/skrivning och justerbara konsekvensnivåer inte är tillgängliga via hello Azure Cosmos DB-emulatorn.

> [!NOTE]
> På den här gången hello Data Explorer i hello stöder emulatorn endast hello skapandet av DocumentDB API samlingar och MongoDB-samlingar. hello Data Explorer i hello-emulatorn stöder för närvarande inte hello skapa tabeller och diagram. 

## <a name="system-requirements"></a>Systemkrav
hello Azure Cosmos DB-emulatorn har hello följande maskin- och programvarukrav:

* Programvarukrav
  * Windows Server 2012 R2, Windows Server 2016 eller Windows 10
*   Minsta maskinvarukrav
  * 2 GB RAM-MINNE
  * 10 GB ledigt hårddiskutrymme

## <a name="installation"></a>Installation
Du kan hämta och installera hello Azure Cosmos DB emulatorn från hello [Microsoft Download Center](https://aka.ms/cosmosdb-emulator). 

> [!NOTE]
> tooinstall, konfigurera och köra hello Azure Cosmos DB-emulatorn måste du ha administratörsbehörighet på hello-dator.

## <a name="running-on-docker-for-windows"></a>Körs på Docker för Windows

hello Azure Cosmos DB-emulatorn kan köras på Docker för Windows. hello emulatorn fungerar inte på Docker för Oracle Linux.

När du har [Docker för Windows](https://www.docker.com/docker-windows) installerat kan du dra hello emulatorn avbildningen från Docker-hubben genom att köra följande kommando från dina Favoriter shell hello (cmd.exe, PowerShell, etc.).

```      
docker pull microsoft/azure-cosmosdb-emulator 
```
toostart hello avbildning, kör följande kommandon hello.

``` 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>nul
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i microsoft/azure-cosmosdb-emulator 
```

hello svar ser ut ungefär toohello följande:

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import hello SSL certificate from an administrator command prompt on hello host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

Stänger hello interaktiva shell när hello emulatorn har startats kommer avstängning hello emulatorn behållare.

Använd hello slutpunkt och huvudnyckeln i från hello svar i din klient och importera hello SSL-certifikatet till värden. tooimport hello SSL-certifikat, hello följande från en kommandotolk för administratörer:

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```


## <a name="start-hello-emulator"></a>Starta hello emulatorn

toostart hello Azure Cosmos DB-emulatorn, Välj hello Start-knappen eller tryck hello Windows-tangenten. Börja skriva **Azure Cosmos DB emulatorn**, och välj hello-emulatorn hello listan med program. 

![Välj hello Start-knappen eller tryck på hello Windows-tangenten, börja skriva ** Azure Cosmos DB emulatorn ** och välj hello-emulatorn hello listan över program](./media/local-emulator/database-local-emulator-start.png)

När emulatorn hello körs visas en ikon i hello meddelandefältet i Aktivitetsfältet. ![Azure DB Cosmos lokala emulatorn Aktivitetsfältsmeddelande](./media/local-emulator/database-local-emulator-taskbar.png)

hello Azure Cosmos DB emulatorn som standard körs på hello lokal dator (”localhost”) lyssnar på port 8081.

hello Azure Cosmos DB emulatorn installeras som standard toohello `C:\Program Files\Azure Cosmos DB Emulator` directory. Du kan också starta och stoppa hello emulator från hello kommandoraden. Se [kommandoradsverktyget referens](#command-line) för mer information.

## <a name="start-data-explorer"></a>Starta Data Explorer

När hello Azure Cosmos DB emulatorn startar öppnas automatiskt hello Azure Cosmos DB Data Explorer i webbläsaren. hello adress visas som [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html). Om du stänger hello explorer och vill toore öppna det senare, du kan öppna hello URL i webbläsaren eller starta från hello Azure Cosmos DB emulatorn i hello Windows-ikon som visas nedan.

![Azure DB Cosmos lokala emulatorn data explorer programstart](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Söker efter uppdateringar
Data Explorer anger om det finns en ny uppdatering tillgänglig för hämtning. 

> [!NOTE]
> Data som har skapats i en version av hello Azure Cosmos DB-emulatorn är inte garanterat toobe tillgänglig när du använder en annan version. Om du behöver toopersist data för hello lång sikt, rekommenderas att du lagrar data i ett Azure DB som Cosmos-konto i stället för hello Azure Cosmos DB-emulatorn. 

## <a name="authenticating-requests"></a>Autentisering av förfrågningar
Precis som med Azure Cosmos-DB i hello molnet måste varje begäran som du gör mot hello Azure Cosmos DB-emulatorn autentiseras. hello Azure Cosmos DB emulatorn stöder ett fast konto och en välkänd autentiseringsnyckel för huvudnyckeln för autentisering. Det här kontot och nyckeln är hello endast autentiseringsuppgifter som tillåts för användning med hello Azure Cosmos DB-emulatorn. De är:

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> hello huvudnyckel som stöds av hello Azure Cosmos DB-emulatorn är avsedd att användas endast med hello-emulatorn. Du kan inte använda din produktion Azure Cosmos DB konto och nyckel med hello Azure Cosmos DB-emulatorn. 

> [!NOTE] 
> Om du har startat hello emulator med hello/Key-alternativet kan sedan använda hello genereras nyckel i stället för ”C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw ==”

Dessutom, precis som hello Azure DB som Cosmos-tjänsten, hello Azure Cosmos DB-emulatorn har endast stöd för säker kommunikation via SSL.

## <a name="running-hello-emulator-on-a-local-network"></a>Kör hello-emulatorns i ett lokalt nätverk

Du kan köra hello emulatorn i ett lokalt nätverk. tooenable nätverksåtkomst, ange hello /AllowNetworkAccess alternativet hello [kommandoraden](#command-line-syntax), vilket kräver att du anger/Key = key_string eller/KeyFile = filnamn. Du kan använda /GenKeyFile = filnamn toogenerate en fil med en slumpmässig nyckel förskott.  Sedan kan du skicka den för/KeyFile = filnamn eller/Key = contents_of_file.

tooenable nätverksåtkomst för hello hello användare bör avstängning hello emulatorn och ta bort hello emulatorn datakatalog (C:\Users\user_name\AppData\Local\CosmosDBEmulator).

## <a name="developing-with-hello-emulator"></a>Utveckla med hello emulatorn
När du har hello Azure Cosmos DB-Emulator som körs på skrivbordet, kan du använda valfri stöds [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) eller hello [Azure Cosmos DB REST API](/rest/api/documentdb/) toointeract med hello emulatorn. hello Azure Cosmos DB emulatorn innehåller också en inbyggd Data Explorer där du kan skapa samlingar för hello DocumentDB och MongoDB APIs och visa och redigera dokument utan att skriva någon kod.   

    // Connect toohello Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

Om du använder [Azure Cosmos DB Protokollstöd för MongoDB](mongodb-introduction.md), Använd hello följande anslutningssträng:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true&3t.sslSelfSignedCerts=true

Du kan använda befintliga verktyg [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) tooconnect toohello Azure Cosmos DB-emulatorn. Du kan också migrera data mellan hello Azure Cosmos DB-emulatorn och hello Azure DB som Cosmos-tjänsten med hjälp av hello [Azure Cosmos DB Datamigreringsverktyg](https://github.com/azure/azure-documentdb-datamigrationtool).

> [!NOTE] 
> Om du har startat hello emulator med hello/Key-alternativet kan sedan använda hello genereras nyckel i stället för ”C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw ==”

Med hello Azure Cosmos DB emulatorn som standard kan skapa du too25 enskilda partitionssamlingar eller 1 partitionerad samling. Mer information om hur du ändrar det här värdet finns [hello PartitionCount värdet](#set-partitioncount).

## <a name="export-hello-ssl-certificate"></a>Exportera hello SSL-certifikat

.NET-språk och runtime Använd hello Windows certifikatarkiv toosecurely ansluter toohello Azure Cosmos DB lokala emulatorn. Andra språk har sin egen metod för att hantera och använda certifikat. Java använder sin egen [certifikatarkiv](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) medan Python använder [socket omslutningar](https://docs.python.org/2/library/ssl.html).

I ordning tooobtain behöver en certifikat-toouse med språk och körningar som inte integreras med hello Windows certifikatarkiv du tooexport den med hjälp av hello Windows Certificate Manager. Du kan starta den genom att köra certlm.msc eller följer hello steg-för-steg-instruktioner i [exportera hello Azure Cosmos DB emulatorn certifikat](./local-emulator-export-ssl-certificates.md). När hello Certifikathanteraren körs kan hello öppna hello personliga certifikat som visas nedan och exportera certifikat med hello eget namn ”DocumentDBEmulatorCertificate” som en Base64-kodad X.509 (.cer)-fil.

![Azure DB Cosmos lokala emulatorn SSL-certifikat](./media/local-emulator/database-local-emulator-ssl_certificate.png)

hello X.509-certifikat kan importeras till certifikatarkivet för hello Java genom att följa instruktionerna hello i [att lägga till ett certifikat toohello Java Certifikatutfärdarens certifikatarkivet](https://docs.microsoft.com/azure/java-add-certificate-ca-store). När du har importerat hello certifikatet till certifikatarkivet för hello Java och MongoDB program kommer att vara kan tooconnect toohello Azure Cosmos DB-emulatorn.

När du ansluter toohello emulator från Python och Node.js SDK, är SSL-kontroll inaktiverad.

## <a id="command-line"></a>Kommandoradsverktyget referens
Hello installationsplatsen kan du använda hello kommandoradsverktyget toostart stoppa hello-emulatorn, konfigurera alternativ och utföra andra åtgärder.

### <a name="command-line-syntax"></a>Kommandoradssyntaxen

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

tooview hello listan är typen `CosmosDB.Emulator.exe /?` hello Kommandotolken.

<table>
<tr>
  <td><strong>Alternativet</strong></td>
  <td><strong>Beskrivning</strong></td>
  <td><strong>Kommandot</strong></td>
  <td><strong>Argument</strong></td>
</tr>
<tr>
  <td>[Inga argument]</td>
  <td>Startar hello Azure Cosmos DB emulatorn med standardinställningar.</td>
  <td>CosmosDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>[Hjälp]</td>
  <td>Visar hello lista över kommandoradsargument som stöds.</td>
  <td>CosmosDB.Emulator.exe /?</td>
  <td></td>
</tr>
<tr>
  <td>avstängning</td>
  <td>Stänger ned hello Azure Cosmos DB-emulatorn.</td>
  <td>CosmosDB.Emulator.exe Shutdown</td>
  <td></td>
</tr>
<tr>
  <td>DataPath</td>
  <td>Anger hello sökväg i vilka toostore datafiler. Standardvärdet är % LocalAppdata%\CosmosDBEmulator.</td>
  <td>CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</td>
  <td>&lt;DataPath&gt;: en tillgänglig sökväg</td>
</tr>
<tr>
  <td>Port</td>
  <td>Anger hello port number toouse för hello-emulatorn.  Standardvärdet är 8081.</td>
  <td>CosmosDB.Emulator.exe/port =&lt;port&gt;</td>
  <td>&lt;port&gt;: enskilda portnummer</td>
</tr>
<tr>
  <td>MongoPort</td>
  <td>Anger hello port number toouse för MongoDB kompatibilitet API. Standardvärdet är 10255.</td>
  <td>CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</td>
  <td>&lt;mongoport&gt;: enskilda portnummer</td>
</tr>
<tr>
  <td>DirectPorts</td>
  <td>Anger hello portar toouse för direkt anslutning. Standardvärdena är 10251,10252,10253,10254.</td>
  <td>CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</td>
  <td>&lt;directports&gt;: kommaavgränsad lista över 4 portar</td>
</tr>
<tr>
  <td>Nyckel</td>
  <td>Auktoriseringsnyckeln för hello-emulatorn. Nyckeln måste vara hello Base64-kodning av en vector 64 byte.</td>
  <td>CosmosDB.Emulator.exe/Key:&lt;nyckel&gt;</td>
  <td>&lt;nyckeln&gt;: nyckeln måste vara hello Base64-kodning av en 64-byte-vektor</td>
</tr>
<tr>
  <td>EnableRateLimiting</td>
  <td>Anger räntesatsen begäran att begränsa beteendet är aktiverad.</td>
  <td>CosmosDB.Emulator.exe /EnableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>DisableRateLimiting</td>
  <td>Anger räntesatsen begäran att begränsa beteendet är inaktiverad.</td>
  <td>CosmosDB.Emulator.exe /DisableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>NoUI</td>
  <td>Visa inte hello emulatorn-användargränssnittet.</td>
  <td>CosmosDB.Emulator.exe/noui</td>
  <td></td>
</tr>
<tr>
  <td>NoExplorer</td>
  <td>Visa inte dokumentutforskaren vid start.</td>
  <td>CosmosDB.Emulator.exe /NoExplorer</td>
  <td></td>
</tr>
<tr>
  <td>PartitionCount</td>
  <td>Anger hello maxantalet partitionerade samlingar. Se [ändra hello antal samlingar](#set-partitioncount) för mer information.</td>
  <td>CosmosDB.Emulator.exe /PartitionCount =&lt;partitioncount&gt;</td>
  <td>&lt;partitioncount&gt;: maximalt antal tillåtna enskilda partitionssamlingar. Standardvärdet är 25. Högsta tillåtna är 250.</td>
</tr>
<tr>
  <td>DefaultPartitionCount</td>
  <td>Anger hello standardantalet partitioner för en partitionerad samling.</td>
  <td>CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</td>
  <td>&lt;defaultpartitioncount&gt; standardvärdet är 25.</td>
</tr>
<tr>
  <td>AllowNetworkAccess</td>
  <td>Aktiverar åtkomst till toohello emulatorn över ett nätverk. Du måste också ange/Key =&lt;key_string&gt; eller/KeyFile =&lt;file_name&gt; tooenable nätverksåtkomst.</td>
  <td>CosmosDB.Emulator.exe AllowNetworkAccess /Key =&lt;key_string&gt;<br><br>eller<br><br>CosmosDB.Emulator.exe /AllowNetworkAccess/KeyFile =&lt;filnamn&gt;</td>
  <td></td>
</tr>
<tr>
  <td>NoFirewall</td>
  <td>Justera inte brandväggsregler när /AllowNetworkAccess används.</td>
  <td>CosmosDB.Emulator.exe /NoFirewall</td>
  <td></td>
</tr>
<tr>
  <td>GenKeyFile</td>
  <td>Skapa en ny auktoriserings-nyckel och spara toohello angivna filen. hello genereras nyckeln kan användas med hello/Key eller/KeyFile alternativ.</td>
  <td>CosmosDB.Emulator.exe /GenKeyFile =&lt;sökväg tookey fil&gt;</td>
  <td></td>
</tr>
<tr>
  <td>Konsekvens</td>
  <td>Ange hello konsekvenskontroll standardnivå för hello-kontot.</td>
  <td>CosmosDB.Emulator.exe /Consistency =&lt;konsekvenskontroll&gt;</td>
  <td>&lt;konsekvenskontroll&gt;: värdet måste vara något av följande hello [konsekvensnivåer](consistency-levels.md): Session starka, Eventual eller BoundedStaleness.  hello standardvärdet är Session.</td>
</tr>
<tr>
  <td>?</td>
  <td>Visa hello-meddelande för hjälp.</td>
  <td></td>
  <td></td>
</tr>
</table>

## <a name="differences-between-hello-azure-cosmos-db-emulator-and-azure-cosmos-db"></a>Skillnader mellan hello Azure Cosmos DB-emulatorn och Azure Cosmos DB 
Eftersom hello Azure Cosmos DB-emulatorn är en emulerade miljö som körs på en lokal developer-arbetsstation, finns det vissa skillnader i funktionalitet mellan hello-emulatorn och ett Azure DB som Cosmos-konto i molnet hello:

* hello Azure Cosmos DB emulatorn stöder bara ett fast konto och en välkänd huvudnyckel.  Det går inte att nyckeln återskapas i hello Azure Cosmos DB-emulatorn.
* hello Azure Cosmos DB-emulatorn är inte en skalbar tjänst och stöder inte ett stort antal samlingar.
* hello Azure Cosmos DB emulatorn simulerar inte olika [Azure Cosmos DB konsekvensnivåer](consistency-levels.md).
* hello Azure Cosmos DB emulatorn simulerar inte [flera regioner replikering](distribute-data-globally.md).
* hello Azure Cosmos DB emulatorn stöder inte hello service kvoten åsidosättningar som är tillgängliga i hello Azure DB som Cosmos-tjänsten (t.ex. dokument storleksgränser, ökad partitionerad samling lagring).
* Som ditt exemplar av hello Azure Cosmos DB emulatorn inte eventuellt upp toodate med hello de senaste ändringarna med hello Azure DB som Cosmos-tjänsten, ta [Azure Cosmos DB kapacitetsplaneringsverktyg](https://www.documentdb.com/capacityplanner) tooaccurately uppskattning produktion genomströmning (RUs) behoven för ditt program.

## <a id="set-partitioncount"></a>Ändra hello antal samlingar

Du kan skapa upp too25 enskilda partitionssamlingar eller 1 partitionerad samling med hello Azure Cosmos DB emulatorn som standard. Genom att ändra hello **PartitionCount** värde, kan du skapa too250 enskilda partitionssamlingar eller 10 partitionerade samlingar eller en kombination av hello två som innehåller högst 250 enskild partitionerar (där 1 partitionerad samlingen = 25 enskild partition samling).

Om du försöker toocreate en samling när hello aktuella partitionsantal har överskridits, utlöser hello emulatorn ett ServiceUnavailable undantag med hello efter meddelande.

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    toobring more and more capacity online, and encourage you tootry again. 
    Please do not hesitate tooemail docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

toochange hello antal samlingar tillgängliga toohello Azure Cosmos DB emulatorn hello följande:

1. Ta bort alla lokala data i Azure Cosmos DB-emulatorn genom att högerklicka på hello **Azure Cosmos DB emulatorn** ikon på hello systemfältet och sedan klicka på **återställa Data...** .
2. Ta bort alla emulatorn data i den här mappen C:\Users\user_name\AppData\Local\CosmosDBEmulator.
3. Avsluta alla öppna instanser genom att högerklicka på hello **Azure Cosmos DB emulatorn** ikon på hello systemfältet och sedan klicka på **avsluta**. Det kan ta ett tag för alla instanser tooexit.
4. Installera hello senaste versionen av hello [Azure Cosmos DB emulatorn](https://aka.ms/cosmosdb-emulator).
5. Starta hello emulator med hello PartitionCount flaggan genom att ange ett värde < = 250. Till exempel: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="troubleshooting"></a>Felsökning

Använd hello följande tips toohelp felsöka problem som kan uppstå med hello Azure DB som Cosmos-emulatorn:

- Om du har installerat en ny version av hello emulatorn och fel har uppstått, se till att du återställer data. Du kan återställa dina data genom att högerklicka på ikonen för hello Azure Cosmos DB emulatorn på hello systemfältet och sedan klicka på Återställ Data... Om detta inte löser hello fel, kan du avinstallera och installera hello app. Se [avinstallera hello lokala emulatorn](#uninstall) anvisningar.

- Om hello Azure Cosmos DB emulatorn krascher, samla in filer med felsökningsdumpar från c:\Users\user_name\AppData\Local\CrashDumps mappen komprimeras och bifoga dem tooan e-post för[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Om det uppstår krascher i CosmosDB.StartupEntryPoint.exe, kör du följande kommando från en kommandotolk för admin hello:`lodctr /R` 

- Om du stöter på ett anslutningsproblem, [samla in spårningsfiler](#trace-files), komprimeras och bifogar dem tooan e-post för[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Om du får en **tjänsten är inte tillgänglig** visas, hello emulatorn vara skadad tooinitialize hello network-stacken. Kontrollera toosee om du har hello Pulse secure klient- eller Juniper nätverk klienten är installerad, som filterdrivrutiner nätverket kan orsaka hello problem. Avinstallera drivrutiner från tredje part nätverket filter vanligtvis åtgärdas hello problem.

### <a id="trace-files"></a>Samla in spårningsfiler

toocollect felsökning spårning, kör följande kommandon från en administrativ kommandotolk hello:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. Titta på hello system fack toomake att hello programmet har avslutats kan det ta ett tag. Du kan även klicka **avsluta** i användargränssnittet för hello Azure DB som Cosmos-emulatorn.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Återskapa hello problemet. Om Data Explorer inte fungerar kan behöver du bara toowait för hello webbläsare tooopen för några sekunder toocatch hello-fel.
5. `CosmosDB.Emulator.exe /stoptraces`
6. Navigera för`%ProgramFiles%\Azure Cosmos DB Emulator` och hitta hello docdbemulator_000001.etl filen.
7. Skicka hello etl-filen tillsammans med reproducera anvisningar för[ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) för felsökning.

### <a id="uninstall"></a>Avinstallera hello lokala emulatorn

1. Avsluta alla öppna instanser av hello lokala emulatorn genom att högerklicka på ikonen för hello Azure Cosmos DB emulatorn på hello systemfältet och klicka på Avsluta. Det kan ta ett tag för alla instanser tooexit.
2. Skriv i sökrutan för hello Windows **appar och funktioner** och klicka på hello **appar och funktioner (systeminställningar)** resultat.
3. Rulla för i hello listan över appar**Azure Cosmos DB emulatorn**, markerar du den, klickar du på **avinstallera**, bekräfta och klicka på **avinstallera** igen.
4. När hello appen avinstalleras, navigera tooC:\Users\<användare > \AppData\Local\CosmosDBEmulator och ta bort hello-mapp. 

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Installerat hello lokala emulatorn
> * SLUMP hello Emulator på Docker för Windows
> * Autentiserade begäranden
> * Används hello Data Explorer i hello emulatorn
> * Exporterade SSL-certifikat
> * Kallas hello Emulator från hello kommandorad
> * Insamlade spårningsfiler

I kursen får du har lärt dig hur toouse hello lokala Emulator för kostnadsfria lokal utveckling. Du kan nu fortsätta toohello nästa kurs och lära dig hur tooexport emulatorn SSL-certifikat. 

> [!div class="nextstepaction"]
> [Exportera hello Azure Cosmos DB emulatorn certifikat](local-emulator-export-ssl-certificates.md)
