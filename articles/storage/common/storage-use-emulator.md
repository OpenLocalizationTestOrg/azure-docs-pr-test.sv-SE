---
title: "aaaUse hello Azure storage-emulatorn för utveckling och testning | Microsoft Docs"
description: "hello Azure storage-emulatorn är en kostnadsfri lokal utvecklingsmiljö för att utveckla och testa dina Azure Storage-program. Lär dig hur begäranden autentiseras hur tooconnect toohello emulator från ditt program och hur toouse hello kommandoradsverktyget."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: f480b059-df8a-4a63-b05a-7f2f5d1f5c2a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: marsma
ms.openlocfilehash: 7cbe6ef5f172bb526c9d8a329b2fe9e6c0ec59d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-storage-emulator-for-development-and-testing"></a>Använd hello Azure storage-emulatorn för utveckling och testning

hello Microsoft Azure storage-emulatorn tillhandahåller en lokal miljö som emulerar hello Azure Blob, köer och tabellen services för utveckling. Med hello storage-emulatorn kan testa du programmet mot hello lagringstjänster lokalt, utan att skapa en Azure-prenumeration eller kostnader. När du är nöjd med hur programmet fungerar i hello-emulatorn kan du växla toousing ett Azure storage-konto i hello molnet.

## <a name="get-hello-storage-emulator"></a>Hämta hello storage-emulatorn
hello storage-emulatorn är tillgänglig som en del av hello [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). Du kan också installera hello storage-emulatorn genom att använda hello [fristående installationsprogram](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) (direct nedladdning). tooinstall hello storage-emulatorn måste du ha administratörsbehörighet på datorn.

hello storage-emulatorn för närvarande kan endast köras på Windows. För de överväger storage-emulatorn för Linux, ett alternativ är hello community underhålls öppen källkod storage-emulatorn [Azurite](https://github.com/arafato/azurite).

> [!NOTE]
> Data som har skapats i en version av hello storage-emulatorn är inte garanterat toobe tillgänglig när du använder en annan version. Om du behöver toopersist data för hello lång sikt, rekommenderar vi att du lagrar data i Azure storage-konto i stället för hello storage-emulatorn.
> <p/>
> hello storage-emulatorn är beroende av specifika versioner av hello OData-bibliotek. Ersätt hello OData-DLL: er används av hello storage-emulatorn med andra versioner stöds inte och kan orsaka oväntade resultat. En version av OData som stöds av hello lagringstjänsten kanske används toosend begäranden toohello emulatorn.
>

## <a name="how-hello-storage-emulator-works"></a>Så här fungerar hello storage-emulatorn
hello storage-emulatorn använder en lokal instans av Microsoft SQL Server och hello lokal fil system tooemulate Azure storage-tjänster. Som standard använder en databas i Microsoft SQL Server 2012 Express LocalDB i hello storage-emulatorn. Du kan välja tooconfigure hello storage-emulatorn tooaccess en lokal instans av SQL Server i stället för hello LocalDB-instans. Mer information finns i hello [Start- och initiera hello storage-emulatorn](#start-and-initialize-the-storage-emulator) senare i den här artikeln.

hello storage-emulatorn ansluter tooSQL Server eller LocalDB med Windows-autentisering.

Det finns vissa skillnader i funktionalitet mellan hello storage-emulatorn och Azure storage-tjänster. Mer information om skillnaderna finns hello [skillnader mellan hello storage-emulatorn och Azure Storage](#differences-between-the-storage-emulator-and-azure-storage) senare i den här artikeln.

## <a name="start-and-initialize-hello-storage-emulator"></a>Starta och initiera hello storage-emulatorn
toostart hello Azure storage-emulatorn:
1. Välj hello **starta** eller tryck på knappen hello **Windows** nyckel.
1. Börja skriva `Azure Storage Emulator`.
1. Välj hello emulatorn hello listan över visade program.

När hello storage-emulatorn startar visas Kommandotolkens fönster. Du kan använda den här konsolen fönstret toostart och stoppa hello storage-emulatorn, ta bort data, hämta status och initiera hello-emulatorn. Mer information finns i hello [referens för Storage-emulatorn kommandoradsverktyget](#storage-emulator-command-line-tool-reference) senare i den här artikeln.

När emulatorn hello körs visas en ikon i hello meddelandefältet i Aktivitetsfältet.

När du stänger hello storage-emulatorn kommandotolksfönster fortsätter hello storage-emulatorn toorun. toobring in hello Storage-emulatorn konsolfönstret igen, följ hello föregående steg som om startar hello storage-emulatorn.

hello har första gången du kör hello storage-emulatorn hello lokal lagringsmiljö initierats för dig. hello initieringsprocessen skapar en databas i LocalDB och reserverar http-portar för varje tjänst för lokal lagring.

hello storage-emulatorn installeras som standard för`C:\Program Files (x86)\Microsoft SDKs\Azure\Storage Emulator`.

> [!TIP]
> Du kan använda hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) toowork med lokal lagring emulatorn resurser. Leta efter ”(utveckling)” under ”Lagringskonton” i trädet för hello Lagringsutforskaren resurser när du har installerat och igång hello storage-emulatorn.
>

### <a name="initialize-hello-storage-emulator-toouse-a-different-sql-database"></a>Initiera hello storage-emulatorn toouse en annan SQL-databas
Du kan använda hello storage-emulatorn kommandoradsverktyget tooinitialize hello lagring emulatorn toopoint tooa SQL-databasinstans än hello standard LocalDB-instans:

1. Öppna hello Storage-emulatorn konsolfönstret enligt beskrivningen i hello [Start- och initiera hello storage-emulatorn](#start-and-initialize-the-storage-emulator) avsnitt.
1. Skriv i konsolfönstret hello hello följande kommando, där `<SQLServerInstance>` är hello namnet på hello SQL Server-instansen. toouse LocalDB, ange `(localdb)\MSSQLLocalDb` som hello SQL Server-instansen.

  `AzureStorageEmulator.exe init /server <SQLServerInstance>`

  Du kan också använda följande kommando som riktar hello emulatorn toouse hello SQL Server-standardinstans hello:

  `AzureStorageEmulator.exe init /server .\\`

  Eller så kan du använda följande kommando som initierar hello toohello standard LocalDB databasinstans hello:

  `AzureStorageEmulator.exe init /forceCreate`

Mer information om dessa kommandon finns [referens för Storage-emulatorn kommandoradsverktyget](#storage-emulator-command-line-tool-reference).

> [!TIP]
> Du kan använda hello [Microsoft SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) toomanage SQL Server-instanser, inklusive hello LocalDB installation. I hello SMSS **ansluta tooServer** dialogrutan Ange `(localdb)\MSSQLLocalDb` i hello **servernamn:** fältet tooconnect toohello LocalDB-instans.

## <a name="authenticating-requests-against-hello-storage-emulator"></a>Autentisering av förfrågningar mot hello storage-emulatorn
När du har installerat och igång hello storage-emulatorn kan testa du din kod mot den. Precis som med Azure Storage i molnet hello måste varje begäran som du gör mot hello storage-emulatorn autentiseras, såvida det inte är en anonym begäran. Du kan autentisera begäranden mot hello storage-emulatorn använder autentisering med delad nyckel eller med en signatur för delad åtkomst (SAS).

### <a name="authenticate-with-shared-key-credentials"></a>Autentisera med delad nyckel autentiseringsuppgifter
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Mer information om anslutningssträngar finns [konfigurera Azure Storage-anslutningssträngar](../storage-configure-connection-string.md).

### <a name="authenticate-with-a-shared-access-signature"></a>Autentisering med signatur för delad åtkomst
Vissa Azure storage, klientbiblioteken, till exempel hello Xamarin biblioteket har endast stöd för autentisering med signatur (SAS) för delade åtkomsttoken. Du kan skapa hello SAS-token med ett verktyg som hello [Lagringsutforskaren](http://storageexplorer.com/) eller ett annat program som stöder autentisering med delad nyckel.

Du kan också generera en SAS-token med hjälp av Azure PowerShell. hello skapar följande exempel en SAS-token med fullständig behörighet tooa blob-behållare:

1. Installera Azure PowerShell om du inte redan gjort (hello senaste versionen av hello Azure PowerShell-cmdlets rekommenderas). Installationsanvisningar finns i [installera och konfigurera Azure PowerShell](/powershell/azure/install-azurerm-ps).
2. Öppna Azure PowerShell och kör Hej efter kommandona, ersätter `ACCOUNT_NAME` och `ACCOUNT_KEY==` med dina autentiseringsuppgifter och `CONTAINER_NAME` med ett namn du väljer:

```powershell
$context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="

New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context

$now = Get-Date

New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri
```

hello resulterande signatur för delad åtkomst URI för hello ny behållare bör likna följande:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss
```

signatur för delad åtkomst hello skapas med det här exemplet är giltig i en dag. hello signatur ger fullständig åtkomst (läsa, skriva, ta bort, lista) tooblobs i hello behållaren.

Mer information om signaturer för delad åtkomst finns [använder signaturer för delad åtkomst (SAS) i Azure Storage](../storage-dotnet-shared-access-signature-part-1.md).

## <a name="addressing-resources-in-hello-storage-emulator"></a>Hantera resurser i hello storage-emulatorn
hello slutpunkter för hello storage-emulatorn skiljer sig från de Azure storage-konto. hello skillnaden är eftersom hello lokala datorn inte utföra namnmatchning för domänen som kräver hello storage-emulatorn slutpunkter toobe lokala adresser.

När du adresserar en resurs i ett Azure storage-konto kan du använda hello följer schemat. hello kontonamnet är en del av hello värdnamnet för URI och hello resursen står inför är en del av hello URI-sökväg:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

Till exempel hello följande URI: N är en giltig adress för en blobb i Azure storage-konto:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Dock Hej storage-emulatorn, eftersom hello lokala datorn inte utför namnmatchning för domänen hello kontonamnet är en del av hello URI-sökväg i stället för hello värdnamn. Använd hello följande URI-format för en resurs i hello storage-emulatorn:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Till exempel kan hello följande adress användas för åtkomst till en blobb i hello storage-emulatorn:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

hello slutpunkter för hello storage-emulatorn är:

* BLOB-tjänsten:`http://127.0.0.1:10000/<account-name>/<resource-path>`
* Kötjänsten:`http://127.0.0.1:10001/<account-name>/<resource-path>`
* Tabelltjänsten:`http://127.0.0.1:10002/<account-name>/<resource-path>`

### <a name="addressing-hello-account-secondary-with-ra-grs"></a>Hello konto sekundär med RA-GRS-adressering
Från och med version 3.1 stöder hello storage-emulatorn geo-redundant replikering för läsbehörighet (RA-GRS). För lagringsresurser både hello molnet och lokala hello-emulator, du kan komma åt hello sekundär plats genom att lägga till - sekundär toohello kontonamn. Hello följande adress kan till exempel användas för att komma åt en blob med hello skrivskyddad sekundär i hello storage-emulatorn:

`http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt`

> [!NOTE]
> Använd hello Storage-klientbibliotek för .NET version 3.2 eller senare för programmeringsåtkomst toohello sekundära med hello storage-emulatorn. Se hello [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) mer information.
>
>

## <a name="storage-emulator-command-line-tool-reference"></a>Referens för Storage-emulatorn kommandoradsverktyget
Från och med version 3.0, visas ett konsolfönster när du startar hello Storage-emulatorn. Använd hello kommandoraden i hello konsolen fönstret toostart och stoppa hello emulatorn samt fråga status och utföra andra åtgärder.

> [!NOTE]
> Om du har hello Microsoft Azure compute emulator installerat visas ett ikonen i systemfältet när du startar hello Storage-emulatorn. Högerklicka på hello ikonen tooreveal en meny som ger ett grafiskt sätt toostart och stoppa hello Storage-emulatorn.
>
>

### <a name="command-line-syntax"></a>Kommandoradssyntax
`AzureStorageEmulator.exe [start] [stop] [status] [clear] [init] [help]`

### <a name="options"></a>Alternativ
tooview hello listan är typen `/help` hello Kommandotolken.

| Alternativ | Beskrivning | Kommando | Argument |
| --- | --- | --- | --- |
| **Börja** |Startar hello storage-emulatorn. |`AzureStorageEmulator.exe start [-inprocess]` |*-inprocess*: starta hello emulatorn i hello aktuella processen i stället för att skapa en ny process. |
| **Stanna** |Stoppar hello storage-emulatorn. |`AzureStorageEmulator.exe stop` | |
| **Status** |Utskrifter hello status för hello storage-emulatorn. |`AzureStorageEmulator.exe status` | |
| **Rensa** |Rensar hello data i alla tjänster som anges på kommandoraden för hello. |`AzureStorageEmulator.exe clear [blob] [table] [queue] [all]                                                    ` |*BLOB*: raderar blob-data. <br/>*kön*: tar bort data för kön. <br/>*tabellen*: rensar tabelldata. <br/>*alla*: tar bort alla data i alla tjänster. |
| **Init** |Utför enstaka initieringen tooset in hello-emulatorn. |<code>AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate&#124;-skipcreate] [-reserveports&#124;-unreserveports] [-inprocess]</code> |*-servern ServerNamn\InstansNamn*: Anger hello-server som är värd hello SQL-instans. <br/>*-sqlinstance instanceName*: Anger hello namn hello SQL-instansen toobe används i hello standard server-instansen. <br/>*-forcecreate*: tvingar skapandet av hello SQL-databas, även om den redan finns. <br/>*-skipcreate*: hoppar över skapandet av hello SQL-databas. Detta åsidosätter - forcecreate.<br/>*-reserveports*: försöker tooreserve hello HTTP-portar som är associerade med hello-tjänster.<br/>*-unreserveports*: försöker tooremove reservationer för hello HTTP-portar som är associerade med hello-tjänster. Detta åsidosätter - reserveports.<br/>*-inprocess*: utför initieringen i hello aktuella processen i stället för att skapa en ny process. hello aktuella processen måste startas med förhöjda behörigheter om ändring av porten reservationer. |

## <a name="differences-between-hello-storage-emulator-and-azure-storage"></a>Skillnader mellan hello storage-emulatorn och Azure Storage
Eftersom hello storage-emulatorn är en emulerade miljö som körs i en lokal SQL-instans, finns det skillnader i funktionalitet mellan hello emulatorn och ett Azure storage-konto i molnet hello:

* hello storage-emulatorn stöder bara ett fast konto och en välkänd autentiseringsnyckel.
* hello storage-emulatorn är inte en skalbar lagring-tjänst och har inte stöd för ett stort antal samtidiga klienter.
* Enligt beskrivningen i [adressering resurser i hello storage-emulatorn](#addressing-resources-in-the-storage-emulator), resurser behandlas på olika sätt i hello storage-emulatorn jämfört med ett Azure storage-konto. Denna skillnad beror domän namnmatchning är tillgängliga i hello moln, men inte på hello lokala dator.
* Från och med version 3.1 stöder hello storage-emulatorn konto geo-redundant replikering för läsbehörighet (RA-GRS). Alla konton har RA-GRS aktiverad i hello-emulatorn och det finns aldrig någon fördröjning mellan hello primära och sekundära replikerna. hello få statistik för Blob-tjänsten, få statistik för kön tjänsten och hämta Service tabellstatistik åtgärder stöds på sekundär hello-konto och returnerar alltid hello värdet för hello `LastSyncTime` svar element som hello aktuell tid bl.a toohello underliggande SQL-databas.
* Hej filtjänst och slutpunkter för SMB-protokollet stöds inte för närvarande i hello storage-emulatorn.
* Om du använder en version av hello lagringstjänster som ännu inte stöds av hello-emulatorn returnerar hello storage-emulatorn VersionNotSupportedByEmulator-fel (HTTP-statuskod 400 - Felaktig begäran).

### <a name="differences-for-blob-storage"></a>Skillnaderna för Blob storage
hello följande skillnader gäller tooBlob lagring i hello-emulatorn:

* hello storage-emulatorn har endast stöd för blob-storlekar upp too2 GB.
* Inkrementell kopia kan ögonblicksbilder från över blobbar toobe kopieras, som returnerar ett fel på hello-tjänsten.
* Get-sidintervall Diff fungerar inte mellan ögonblicksbilder kopieras med hjälp av inkrementell kopiera Blob.
* En Put-Blob-åtgärden kan lyckas mot en blob som finns i hello storage-emulatorn med en aktiv livslängd, även om hello lease-ID inte har angetts i hello begäran.
* Lägg till Blob-åtgärder inte stöds av hello-emulatorn. Försöker utföra en åtgärd på en tilläggsblobb returnerar FeatureNotSupportedByEmulator-fel (HTTP-statuskod 400 - Felaktig begäran).

### <a name="differences-for-table-storage"></a>Skillnaderna för tabellagring
hello följande skillnader gäller tooTable lagring i hello-emulatorn:

* Datum egenskaper i hello tabelltjänsterna i hello storage-emulatorn stöder endast hello intervall som stöds av SQL Server 2005 (de är obligatoriska toobe senare än 1 januari 1753). Alla datum före den 1 januari 1753 ändras toothis värde. hello precision med datum är begränsad toohello precisionen för SQL Server 2005, vilket innebär att datum är exakt too1/300th av en sekund.
* hello storage-emulatorn stöder partition nyckel och raden nyckelegenskapen värden för mindre än 512 byte. Dessutom hello total storlek på hello kontonamn, tabellnamnet och namn på nyckelegenskap tillsammans får inte överskrida 900 byte.
* hello totala storleken på en rad i en tabell i hello storage-emulatorn är begränsad tooless än 1 MB.
* Egenskaper för data Skriv i hello storage-emulatorn `Edm.Guid` eller `Edm.Binary` stöd endast hello `Equal (eq)` och `NotEqual (ne)` jämförelseoperatorer i frågan filtrera strängar.

### <a name="differences-for-queue-storage"></a>Skillnaderna för Queue storage
Det finns ingen specifik tooQueue lagring för skillnader i hello-emulatorn.

## <a name="storage-emulator-release-notes"></a>Storage-emulatorn viktig information
### <a name="version-52"></a>Version 5.2
* hello storage-emulatorn stöder nu version 2017-04-17 av hello lagringstjänster på Blob, köer och tabellen slutpunkter.
* Fast ett programfel där egenskapsvärden har inte som kodats korrekt.

### <a name="version-51"></a>Version 5.1
* Fast ett programfel där hello storage-emulatorn returnerade hello `DataServiceVersion` huvudet i vissa svar där hello-tjänsten inte kunde.

### <a name="version-50"></a>Version 5.0
* hello storage-emulatorn installer kontrollerar inte längre för befintliga MSSQL och installerar .NET Framework.
* hello storage-emulatorn installationsprogrammet skapar inte längre hello-databas som en del av installationen. Databasen kommer fortfarande att skapas vid behov som en del av Start.
* Databasen skapas inte längre kräver utökad behörighet.
* Port reservationer behövs inte längre för start.
* Lägger till följande alternativ för hello`init`: `-reserveports` (kräver höjning) `-unreserveports` (kräver höjning) `-skipcreate`.
* hello lagring Emulator UI alternativet på ikonen i systemfältet hello nu startar hello kommandoradsgränssnittet. hello gamla GUI är inte längre tillgänglig.
* Vissa DLL-filer har tagits bort eller bytt namn.

### <a name="version-46"></a>Version 4.6
* hello storage-emulatorn stöder nu version 2016-05-31 av hello lagringstjänster på Blob, köer och tabellen slutpunkter.

### <a name="version-45"></a>Version 4.5
* Fast ett fel som orsakade initieringen och installation av hello storage-emulatorn toofail när hello säkerhetskopiera databasen har bytt namn.

### <a name="version-44"></a>Version 4.4
* hello storage-emulatorn stöder nu version 2015-12-11 av hello lagringstjänster på Blob, köer och tabellen slutpunkter.
* hello är storage-emulatorn skräpinsamling blob-data nu mer effektiv när du hanterar stort antal blobbar.
* Fast ett fel som orsakade att behållarens ACL XML toobe verifieras lite annorlunda från hur hello lagringstjänsten fungerar.
* Fast ett fel som orsakade ibland max och min DateTime värden toobe rapporteras i hello felaktig tidszon.

### <a name="version-43"></a>Version 4.3
* hello storage-emulatorn stöder nu version 2015-07-08 av hello lagringstjänster på Blob, köer och tabellen slutpunkter.

### <a name="version-42"></a>Version 4.2
* hello storage-emulatorn stöder nu version 2015-04-05 av hello lagringstjänster på Blob, köer och tabellen slutpunkter.

### <a name="version-41"></a>Version 4.1
* hello storage-emulatorn stöder nu version 2015-02-21 av hello lagringstjänster på Blob, köer och tabellen slutpunkter, förutom hello nya funktioner för Lägg till Blob.
* Om du använder en version av hello lagringstjänster som ännu inte stöds av hello-emulatorn returnerar hello emulatorn ett beskrivande felmeddelande. Vi rekommenderar att du använder hello senaste versionen av hello-emulatorn. Om det uppstår ett fel i VersionNotSupportedByEmulator (HTTP-statuskod 400 - Felaktig begäran), hämta hello senaste versionen av hello storage-emulatorn.
* Fast ett programfel inom vilket en RAS tillståndet som orsakade tabell entity data toobe felaktig under samtidiga merge-operationer.

### <a name="version-40"></a>Version 4.0
* hello storage-emulatorn körbara har bytt namn för*AzureStorageEmulator.exe*.

### <a name="version-32"></a>Version 3.2
* hello storage-emulatorn har nu stöd för version 2014-02-14 i hello lagringstjänster på Blob, köer och tabellen slutpunkter. Filen Tjänsteslutpunkter stöds inte för närvarande i hello storage-emulatorn. Se [versionshantering för hello Azure Storage-tjänster](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) mer information om version 2014-02-14.

### <a name="version-31"></a>Version 3.1
* Geo-redundant lagring med läsbehörighet (RA-GRS) stöds nu i hello storage-emulatorn. hello hämta Blob-tjänsten statistik, få statistik för kön tjänsten och hämta tabellen Service Stats API: er stöds för hello konto sekundär och returnerar alltid hello värdet för hello LastSyncTime svar element som hello aktuell tid-bl.a toohello underliggande SQL databas. Använd hello Storage-klientbibliotek för .NET version 3.2 eller senare för programmeringsåtkomst toohello sekundära med hello storage-emulatorn. Information finns i hello Microsoft Azure Storage-klientbibliotek för .NET-referens.

### <a name="version-30"></a>Version 3.0
* hello Azure storage-emulatorn levereras inte längre i hello samma paket som hello beräkningsemulatorn.
* hello storage-emulatorn grafiskt gränssnitt har ersatts av ett skript kommandoradsgränssnitt. Mer information om hello kommandoradsgränssnittet finns i Storage-emulatorn Kommandoradsreferens verktyget. hello grafiskt gränssnitt fortsätter toobe finns i version 3.0, men den kan endast användas när hello Compute Emulator har installerats genom att högerklicka på ikonen i systemfältet hello och välja Visa Användargränssnittet för Storage-emulatorn.
* Version 2013-08-15 hello Azure storage-tjänster har nu fullt stöd. (Den här versionen har tidigare endast stöds av Storage-emulatorn version 2.2.1 förhandsgranskning.)

## <a name="next-steps"></a>Nästa steg

* Utvärdera hello plattformsoberoende, community-underhålls öppen källkod storage-emulatorn [Azurite](https://github.com/arafato/azurite). 
* [Azure Storage-exempel med hjälp av .NET](../storage-samples-dotnet.md) innehåller länkar tooseveral kodexempel som du kan använda när du utvecklar programmet.
* Du kan använda hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) toowork med resurser i ditt moln Storage-konto och i hello storage-emulatorn.
