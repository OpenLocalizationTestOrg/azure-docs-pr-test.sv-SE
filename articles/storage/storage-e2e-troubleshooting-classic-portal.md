---
title: aaaTroubleshooting Azure Storage med diagnostik & Message Analyzer | Microsoft Docs
description: "En självstudiekurs visar slutpunkt till slutpunkt felsökning med Azure Storage Analytics, AzCopy och Microsoft Message Analyzer"
services: storage
documentationcenter: dotnet
author: robinsh
manager: timlt
ms.assetid: 6b23cba5-0d53-439e-870b-de8e406107d8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: robinsh
ms.openlocfilehash: 74ee126bab30b9a45f4904a065b6fe3006f76101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Slutpunkt till slutpunkt-felsökning med hjälp av Azure Storage-mätvärden och loggning, AzCopy och Message Analyzer
[!INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>Översikt
Diagnostisera och felsöka är en nyckel kunskap för att bygga och stöd för program med Microsoft Azure Storage. På grund av toohello distribuerade uppbyggnad ett Azure-program, kan det vara mer komplexa än i vanliga miljöer att diagnostisera och felsöka fel och problem med prestanda.

I den här kursen visar vi hur tooidentify klienten vissa fel som kan påverka prestanda och felsökning av dessa fel från slutpunkt till slutpunkt med hjälp av verktyg som tillhandahålls av Microsoft och Azure Storage, i ordning toooptimize hello-klientprogrammet.

Den här självstudiekursen innehåller en praktisk utforska för ett scenario för slutpunkt till slutpunkt-felsökning. En detaljerad konceptuella guiden tootroubleshooting Azure storage-program, se [övervaka, diagnostisera och felsöka Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Verktyg för felsökning av Azure Storage-program
tootroubleshoot klientprogram som använder Microsoft Azure Storage, kan du använda en kombination av verktyg toodetermine när ett problem har uppstått och vilka problem hello hello orsaken kan vara. Dessa verktyg innefattar:

* **Azure Storage Analytics**. [Azure Storage Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) innehåller mått och loggning för Azure Storage.

  * **Storage-mätvärden** spårar transaktion mått och kapacitetsmått för ditt lagringskonto. Med hjälp av mätvärden, kan du bestämma hur programmet fungerar bl.a tooa mängd olika åtgärder. Se [Storage Analytics mätvärden tabellschemat](http://msdn.microsoft.com/library/azure/hh343264.aspx) för mer information om hello typer av spåras av Storage Analytics mätvärden.
  * **Lagring loggning** loggar varje begäran toohello Azure Storage services tooa serversidan logg. hello spårar detaljerad loggdata för varje begäran, inklusive hello åtgärden utföras hello status för hello igen och svarstid information. Se [Storage Analytics loggformatet](http://msdn.microsoft.com/library/azure/hh343259.aspx) mer information om hello förfrågan och svar data som skrivs toohello loggar av Storage Analytics.

> [!NOTE]
> Storage-konton med en typ av replikering av Zonredundant lagring (ZRS) har inte hello mått eller loggningsfunktioner just nu.
>
>

* **Klassiska Azure-portalen**. Du kan konfigurera mått och loggning för ditt lagringskonto i hello [klassiska Azure-portalen](https://manage.windowsazure.com). Du kan också visa tabeller och diagram som visar hur programmet fungerar över tid och konfigurera aviseringar toonotify du om dina program fungerar annorlunda än förväntat för ett visst mått.

    Se [övervaka ett lagringskonto i hello Azure Portal](storage-monitor-storage-account.md) information om hur du konfigurerar övervakning i hello klassiska Azure-portalen.
* **AzCopy**. Server-loggar för Azure Storage lagras som blobbar, så du kan använda AzCopy toocopy hello blobbar tooa lokala loggkatalog för analys med hjälp av Microsoft Message Analyzer. Se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md) mer information om AzCopy.
* **Microsoft Message Analyzer**. Message Analyzer är ett verktyg som förbrukar loggfiler och visar loggdata i ett visuellt format som gör det enkelt toofilter, Sök och gruppen logga data till användbar anger att du kan använda tooanalyze fel och problem med prestanda. Se [operativsystem handboken för Microsoft Message Analyzer](http://technet.microsoft.com/library/jj649776.aspx) mer information om Message Analyzer.

## <a name="about-hello-sample-scenario"></a>Om hello Exempelscenario
Vi går igenom ett scenario där Azure Storage-mätvärden anger en låg procent slutförandenivå för ett program som anropar Azure-lagring för den här självstudiekursen. Hej låg procent lyckade hastighet mått (visas som **PercentSuccess** i hello klassiska Azure-portalen och hello mått tabeller) spårar åtgärder som lyckas, men som kan returnera ett HTTP-statuskod som är större än 299. I hello serversidan lagring loggfiler åtgärderna registreras med transaktionsstatus **ClientOtherErrors**. Mer information om hello låg procent lyckade mått finns [mätvärdena visar låg PercentSuccess eller analytics loggposter innehålla åtgärder med transaktionsstatus av ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure Storage-åtgärder kan returnera HTTP-statuskoder större än 299 som en del av deras normala funktioner. Men dessa fel i vissa fall kan indikera att du kan toooptimize klientprogrammet för bättre prestanda.

I det här scenariot vi ska anser att en låg procent lyckade hastighet toobe något under 100%. Du kan välja en annan mått nivå, men enligt tooyour behov. Vi rekommenderar att under testningen av programmet, du upprätta en baslinje tolerans för dina nyckeltal. T.ex, du kan fastställa baserat på testning, att ditt program bör ha en konsekvent procent slutförandenivå 90% eller 85%. Om din mätvärdesdata visar att programmet hello avviker från det numret, kan du undersöka vad orsakar hello ökning.

I vårt Exempelscenario när vi har etablerat att hello procent lyckade hastighet mått är mindre än 100%, vi undersöka hello loggar toofind hello fel som toohello mått och använder dem som toofigure reda på vad orsakar hello lägre procent lyckade resultat. Vi ska titta särskilt på fel i hello 400 intervall. Sedan ska vi närmare undersöka 404 (inget hittas) fel.

### <a name="some-causes-of-400-range-errors"></a>Vissa orsakerna till 400-range-fel
hello exemplen nedan visas ett urval av fel 400-intervall för förfrågningar mot Azure Blob Storage och deras möjliga orsaker. Något av dessa fel, samt fel i hello 300 intervall och 500 hello-intervall kan bidra tooa låg procent lyckade resultat.

Observera att hello listorna nedan är långt från klar. Se [Status och felkoder](http://msdn.microsoft.com/library/azure/dd179382.aspx) för information om allmänna Azure Storage-fel och om fel specifika tooeach i hello lagringstjänster.

**Status 404 (inget hittas)-kodexempel**

Inträffar när en Läsåtgärd mot en behållare eller blobb misslyckas eftersom hello blob eller behållaren inte finns.

* Inträffar om en behållare eller blobb har tagits bort av en annan klient innan denna begäran.
* Inträffar om du använder ett API-anrop som skapar hello behållare eller blobb när du har kontrollerat om den finns. Hej CreateIfNotExists APIs gör en HEAD anropa första toocheck hello befintliga hello behållare eller blobb; Om det inte finns ett 404-fel returneras och sedan en andra PUT-anrop toowrite hello behållare eller blob.

**Status-kodexempel 409 (konflikt)**

* Inträffar om du använder en skapa API toocreate en ny behållare eller blobb, utan att först kontrollera befintliga och en behållare eller blob med det namnet finns redan.
* Inträffar om en behållare tas bort och försöker toocreate en ny behållare med hello samma namn innan hello borttagningen är klar.
* Inträffar om du anger ett lån på en behållare eller blobb och det finns redan ett lån finns.

**Statuskod 412 (villkor misslyckades) exempel**

* Inträffar när hello villkor som anges av ett villkorat huvud inte har uppfyllts.
* Inträffar när hello lease-ID som angetts inte matchar hello lease-ID på hello behållare eller blob.

## <a name="generate-log-files-for-analysis"></a>Skapa loggfiler för analys
I den här självstudiekursen kommer använder vi Message Analyzer toowork med tre olika typer av loggfiler, men du kan välja toowork med någon av följande:

* Hej **serverloggen**, som skapas när du aktiverar loggning för Azure Storage. hello serverloggen innehåller information om varje åtgärd som kallas mot en hello Azure Storage services - blob, kön, tabell och fil. hello serverloggen anger vilken åtgärd anropades och vilka statuskoden var returnerade, samt annan information om hello förfrågan och svar.
* Hej **.NET-klientloggen**, som skapas när du aktiverar loggning på klientsidan från .NET-program. hello klientloggen innehåller detaljerad information om hur hello klienten förbereder hello begäran och tar emot och bearbetar hello svar.
* Hej **HTTP nätverket spårningslogg**, som samlar in data på HTTP/HTTPS-begäran och svar data, inklusive för åtgärder mot Azure Storage. I den här kursen ska vi generera hello spårning i nätverket via Message Analyzer.

### <a name="configure-server-side-logging-and-metrics"></a>Konfigurera loggning på serversidan och mått
Först måste behöver vi tooconfigure Azure Storage-loggning och statistik, så att vi har data från hello klienten programmet tooanalyze. Du kan konfigurera loggning och mått i en mängd olika sätt – via hello [klassiska Azure-portalen](https://manage.windowsazure.com), med hjälp av PowerShell, eller programmässigt. Se [aktivera Storage-mätvärden och visa mått Data](http://msdn.microsoft.com/library/azure/dn782843.aspx) och [aktivera loggning för lagring och åtkomst till loggdata](http://msdn.microsoft.com/library/azure/dn782840.aspx) mer information om hur du konfigurerar loggning och mått.

**Via hello klassiska Azure-portalen**

tooconfigure loggning och mått för ditt lagringskonto med hjälp av hello portal, följ instruktionerna för hello på [övervaka ett lagringskonto i hello Azure Portal](storage-monitor-storage-account.md).

> [!NOTE]
> Det är inte möjligt tooset minut mått med hello klassiska Azure-portalen. Vi rekommenderar dock att du har angett dem för hello syftet med den här kursen och för att undersöka prestandaproblem med ditt program. Du kan ange minut mått med hjälp av PowerShell som visas nedan, eller programmässigt eller via hello klassiska Azure-portalen.
>
> Observera att hello klassiska Azure-portalen kan inte visa minut statistik, och endast timvis mått.
>
>

**Via PowerShell**

tooget igång med PowerShell för Azure, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

1. Använd hello [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0) cmdlet tooadd din Azure-användarkonto toohello PowerShell-fönstret:

    ```powershell
     Add-AzureAccount
    ```

2. I hello **logga in tooMicrosoft Azure** fönster, Skriv hello e-postadress och lösenord som är associerat med ditt konto. Azure autentiserar sparar hello autentiseringsuppgifter och sedan stänger hello-fönstret.
3. Ange hello konto toohello lagring standardkontot för lagring som används för hello kursen genom att köra dessa kommandon hello PowerShell-fönstret:

    ```powershell
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Aktivera loggning för hello Blob-tjänsten lagring:

    ```powershell
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```

5. Aktivera storage-mätvärden för hello Blob-tjänsten, vilket gör att tooset **- MetricsType** för`Minute`:

    ```powershell
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>Konfigurera loggning för klientsidan av .NET
tooconfigure klientsidan loggning för en .NET-program, aktivera .NET diagnostik i hello programmets konfigurationsfil (web.config eller app.config). Se [klientsidan loggning med hello Storage-klientbibliotek för .NET](http://msdn.microsoft.com/library/azure/dn782839.aspx) och [klientsidan loggning med hello Microsoft Azure Storage SDK för Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) mer information.

hello klientsidan loggen innehåller detaljerad information om hur hello klienten förbereder hello begäran och tar emot och bearbetar hello svar.

hello Storage-klientbibliotek lagrar klientsidan loggdata i hello-plats som anges i hello programmets konfigurationsfil (web.config eller app.config).

### <a name="collect-a-network-trace"></a>Samla in en spårning i nätverket
Du kan använda Message Analyzer toocollect en spårning i nätverket för HTTP/HTTPS när klientprogrammet körs. Message Analyzer använder [Fiddler](http://www.telerik.com/fiddler) på hello serverdel. Innan du samlar in hello spårning i nätverket, rekommenderar vi att du konfigurerar Fiddler toorecord okrypterade HTTPS-trafik:

1. Installera [Fiddler](http://www.telerik.com/download/fiddler).
2. Starta Fiddler.
3. Välj **verktyg | Alternativ för fiddler**.
4. I dialogrutan för hello alternativ kontrollerar du att **avbilda HTTPS ansluter** och **dekryptera HTTPS-trafik** båda är valda, enligt nedan.

![Konfigurera alternativ för Fiddler](./media/storage-e2e-troubleshooting-classic-portal/fiddler-options-1.png)

Hello genomgång kan samla in och spara en spårning i nätverket först i Message Analyzer och sedan skapa en analys session tooanalyze hello spårning och hello loggar. toocollect en spårning i nätverket i Message Analyzer:

1. Välj i Message Analyzer **filen | Snabb Trace | Okrypterade HTTPS**.
2. hello trace börjar omedelbart. Välj **stoppa** toostop hello trace så att vi kan konfigurera den tootrace lagringsrelaterad trafik.
3. Välj **redigera** tooedit hello spårningssessionen.
4. Välj hello **konfigurera** länka toohello till höger om hello **Microsoft-Pef-WebProxy** ETW-provider.
5. I hello **avancerade inställningar** dialogrutan klickar du på hello **Provider** fliken.
6. I hello **filtret Hostname** anger dina slutpunkter för lagring, avgränsade med blanksteg. Exempelvis kan du ange dina slutpunkter enligt följande; ändra `storagesample` toohello namnet på ditt lagringskonto:

    ```   
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Avsluta hello dialogrutan och klicka på **starta om** toobegin att samla in hello spårningen med hello hostname filter på plats, så att endast Azure Storage-nätverkstrafik ingår i hello spårning.

> [!NOTE]
> När du är klar att samla in en spårning i nätverket, rekommenderar vi att du återställer hello-inställningar som du har ändrat Fiddler toodecrypt HTTPS-trafik. I dialogrutan för hello Fiddler alternativ, avmarkera hello **avbilda HTTPS ansluter** och **dekryptera HTTPS-trafik** kryssrutorna.
>
>

Se [med hello spårning nätverksfunktioner](http://technet.microsoft.com/library/jj674819.aspx) på Technet för mer information.

## <a name="review-metrics-data-in-hello-azure-classic-portal"></a>Granska mätvärdesdata i hello klassiska Azure-portalen
När programmet har körts för en viss tidsperiod, kan du granska hello mått diagrammen som visas i hello Azure klassiska Portal tooobserve hur tjänsten fungerar. Först måste vi lägger till hello **lyckade procentandel** mått toohello övervakning sidan:

1. Navigera toohello instrumentpanelen för ditt lagringskonto i hello [klassiska Azure-portalen](https://manage.windowsazure.com)och välj **övervakaren** tooview hello övervakning sidan.
2. Klicka på **lägga till mätvärden** toodisplay hello **välja mått** dialogrutan.
3. Bläddra nedåt toofind hello **lyckade procentandel** gruppen, expandera den och välj sedan **sammanställd**, som hello bilden nedan. Det här måttet aggregerar lyckade procentandelsdata från alla Blob-åtgärder.

![Välj mått](./media/storage-e2e-troubleshooting-classic-portal/choose-metrics-portal-1.png)

I hello klassiska Azure-portalen, visas nu **lyckade procentandel** hello övervakning diagram, tillsammans med andra mått du kanske har lagt till i (in toosix kan visas i hello diagram samtidigt). I hello bilden nedan ser du att hello procent lyckade reparationer är något under 100%, vilket är hello-scenariot som vi ska undersöka bredvid genom att analysera hello loggar i Message Analyzer:

![Mått diagram i portalen](./media/storage-e2e-troubleshooting-classic-portal/portal-metrics-chart-1.png)

Mer information om att lägga till mätvärden toohello övervakning sidan finns [så här: Lägg till mått toohello mått tabell](storage-monitor-storage-account.md#add-metrics-charts-to-the-portal-dashboard).

> [!NOTE]
> Det kan ta lite tid för din mått data tooappear i hello klassiska Azure-portalen när du aktiverar lagring mått. Det beror på att varje timme mätvärden för hello föregående timma inte visas i hello klassiska Azure-portalen förrän hello aktuell timme har gått ut. Dessutom visas inte minut mått i hello klassiska Azure-portalen. Så det kan ta upp tootwo timmar toosee mått data beroende på när du aktiverar mått.
>
>

## <a name="use-azcopy-toocopy-server-logs-tooa-local-directory"></a>Använd AzCopy toocopy serverloggen tooa lokal katalog
Azure Storage skriver server logga data tooblobs medan mätvärdena skrivs tootables. Loggen blobbar finns i hello välkända `$logs` behållare för ditt lagringskonto. Loggen blobbar namnet hierarkiskt som år, månad, dag och timme, så att du lätt kan hitta hello tidsintervallet för när du vill tooinvestigate. Till exempel i hello `storagesample` kontot hello behållare för hello loggen BLOB för 02/01/2015, från 8 – 9: 00, är `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. hello enskilda blobbar i den här behållaren har sekventiella namn, från och med `000000.log`.

Du kan använda hello AzCopy kommandoradsverktyget toodownload dessa serversidan loggen filer tooa plats på den lokala datorn. Du kan till exempel använda hello efter kommandot toodownload hello loggfiler för blob-åtgärder som tog placera på 2 januari 2015 toohello mappen `C:\Temp\Logs\Server`; Ersätt `<storageaccountname>` med hello namnet på ditt lagringskonto och `<storageaccountkey>` med din åtkomstnyckel:

```azcopy
AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V
```

AzCopy är tillgänglig för hämtning på hello [Azure hämtar](https://azure.microsoft.com/downloads/) sidan. Mer information om hur du använder AzCopy finns [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md).

Mer information om hur du hämtar serversidan loggar finns [hämtar lagring loggning loggdata](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-tooanalyze-log-data"></a>Använd Microsoft Message Analyzer tooanalyze loggdata
Microsoft Message Analyzer är ett verktyg för att samla in, visa och analysera protokollet messaging trafik, händelser och andra system- eller meddelanden i scenarier med felsökning och diagnostik. Message Analyzer kan du tooload, sammanställd, och analysera data från loggen också och spara spårningsfiler. Mer information om Message Analyzer finns [operativsystem handboken för Microsoft Message Analyzer](http://technet.microsoft.com/library/jj649776.aspx).

Message Analyzer innehåller tillgångar för Azure Storage som hjälper dig tooanalyze servern, klienten och loggar för nätverket. I det här avsnittet diskuterar vi hur toouse dessa verktyg tooaddress hello problemet av låg procent hello lagring loggar.

### <a name="download-and-install-message-analyzer-and-hello-azure-storage-assets"></a>Hämta och installera Message Analyzer och hello Azure Storage-tillgångar
1. Hämta [Message Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) hello från Microsoft Download Center och kör installationsprogrammet för hello.
2. Starta Message Analyzer.
3. Från hello **verktyg** väljer du **Tillgångsansvarig**. I hello **Tillgångsansvarig** markerar **hämtar**, sedan filtrera **Azure Storage**. Hello Azure Storage tillgångar, visas som hello bilden nedan.
4. Klicka på **synkronisera alla visas objekt** tooinstall hello Azure Storage tillgångar. hello tillgängliga resurser inkluderar:
   * **Azure Storage-färgregler:** färgregler för Azure Storage kan du toodefine särskilda filter som använder färg, text och stilar toohighlight meddelanden som innehåller specifik information i en spårning.
   * **Azure Storage-diagram:** diagram för Azure Storage är fördefinierade diagram som diagramdata server-loggen. Observera att toouse Azure Storage diagram just nu, du kan bara läsa in hello serverloggen i hello analys rutnätet.
   * **Azure Storage-Parser:** hello Azure Storage Parser parse hello Azure Storage-klienten, server och HTTP-loggar i ordning toodisplay dem i hello analys rutnätet.
   * **Azure Storage-filter:** Azure Storage filter är fördefinierade kriterier som du kan använda tooquery dina data i hello analys rutnätet.
   * **Azure Storage layouter:** layouter Azure Storage är fördefinierad kolumn och grupperingar i hello analys rutnätet.
5. Starta om Message Analyzer när du har installerat hello tillgångar.

![Message Analyzer Tillgångsansvarig](./media/storage-e2e-troubleshooting-classic-portal/mma-start-page-1.png)

> [!NOTE]
> Installera alla hello Azure Storage tillgångarna för hello syftet med den här kursen.
>
>

### <a name="import-your-log-files-into-message-analyzer"></a>Importera loggfilerna till Message Analyzer
Du kan importera alla dina sparade loggfiler (servern, klienten och nätverk) till en enda session i Microsoft Message Analyzer för analys.

1. På hello **filen** Klicka på menyn i Microsoft Message Analyzer **ny Session**, och klicka sedan på **tom Session**. I hello **ny Session** dialogrutan, ange ett namn för din session för analys. I hello **sessionsinformation** klickar du på på hello **filer** knappen.
2. tooload hello nätverket spåra data som genereras av Message Analyzer klickar du på **Lägg till filer**, bläddra toohello plats där du sparade filen .matp från webben spårningssessionen och välj hello .matp filen och klicka på **öppna**.
3. tooload hello serversidan loggdata, klicka på **Lägg till filer**, bläddra toohello plats där du sparade loggarna serversidan, Välj hello loggfiler för hello tidsintervall du tooanalyze och på **öppna**. Sedan hello **sessionsinformation** panelen, ange hello **Text konfigurationen av loggen** listrutan för varje serversidan loggfil för**AzureStorageLog** tooensure som Microsoft Message Analyzer kan parsa hello loggfilen på rätt sätt.
4. tooload hello klientsidan loggdata, klicka på **Lägg till filer**, bläddra toohello plats där du sparade loggarna på klientsidan, välja hello loggfiler du tooanalyze och på **öppna**. Sedan hello **sessionsinformation** panelen, ange hello **Text konfigurationen av loggen** listrutan för varje loggfil på klientsidan för**AzureStorageClientDotNetV4** tooensure som Microsoft Message Analyzer kan parsa hello loggfilen på rätt sätt.
5. Klicka på **starta** i hello **ny Session** dialogrutan tooload och parsa hello loggdata. hello loggdata visar hello Message Analyzer analys rutnätet.

hello bilden nedan visas en exempel-session som har konfigurerats med servern, klienten och spårningsloggfilerna i nätverket.

![Konfigurera Message Analyzer Session](./media/storage-e2e-troubleshooting-classic-portal/configure-mma-session-1.png)

Observera att Message Analyzer läser in loggfiler i minnet. Om du har en stor mängd loggdata vill toofilter i ordning tooget hello bästa möjliga prestanda från Message Analyzer.

Först fastställa hello tidsram som du är intresserad av att granska och att denna tidsram så liten som möjligt. I många fall vill tooreview en tid i minuter eller timmar högst. Importera hello minsta uppsättning loggar som uppfyller dina behov.

Om du fortfarande har en stor mängd loggade data kanske du vill toospecify en session filter toofilter logga data innan du läser in. I hello **Sessionsfiltret** rutan, Välj hello **biblioteket** knappen toochoose ett fördefinierat filter, till exempel välja **Global tid Filter I** från hello Azure Storage-filter toofilter under ett tidsintervall. Du kan redigera hello filter kriterier toospecify hello börjar och slutar tidsstämpel för hello-intervall du vill toosee. Du kan också filtrera på en viss statuskod; Exempelvis kan du välja tooload endast loggposter där är hello statuskod 404.

Mer information om att importera loggdata till Microsoft Message Analyzer finns [hämtar meddelandedata](http://technet.microsoft.com/library/dn772437.aspx) på TechNet.

### <a name="use-hello-client-request-id-toocorrelate-log-file-data"></a>Använd hello klienten begäran-ID toocorrelate loggfilernas data
hello Azure Storage-klientbibliotek genererar automatiskt ett unikt ID för klientbegäran för varje begäran. Det här värdet skrivs toohello klientloggen och serverloggen hello hello spårning i nätverket, så du kan använda den toocorrelate data över alla tre loggar inom Message Analyzer. Se [ID för klientbegäran](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) be för ytterligare information om hello klient-ID.

hello i nedanstående avsnitt beskrivs hur toouse förkonfigurerade och anpassad layout vyer toocorrelate och gruppera data baserat på klientbegäran hello-ID.

### <a name="select-a-view-layout-toodisplay-in-hello-analysis-grid"></a>Välj en vy layout toodisplay i hello analys rutnätet
hello lagring tillgångar för Message Analyzer inkluderar Azure Storage layouter, som är förkonfigurerad vyer som du kan använda toodisplay dina data med användbar grupperingar och kolumner för olika scenarier. Du kan också skapa anpassade layouter och spara dem för återanvändning.

hello bilden nedan visar hello **visa Layout** menyn genom att välja **visa Layout** från hello verktygsfältet menyfliksområdet. hello layouter för Azure Storage är grupperade under hello **Azure Storage** nod hello-menyn. Du kan söka efter `Azure Storage` Visa endast layouter i hello Sök rutan toofilter på Azure Storage. Du kan också välja hello stjärna nästa tooa vyn layout toomake it som en favorit och visa det hello överst i hello-menyn.

![Visa menyn Layout](./media/storage-e2e-troubleshooting-classic-portal/view-layout-menu.png)

toobegin med utvalda **grupperade efter ClientRequestID och modulen**. Den här vyn layout grupper logga data från alla tre loggar först efter ID för klientbegäran, sedan efter loggfil för källdatorn (eller **modulen** i Message Analyzer). Med den här vyn kan du detaljnivån i en viss klient-ID för förfrågan och se data från alla tre loggfiler för klientbegäran-ID.

hello bilden nedan visar den här layouten visa tillämpas toohello loggen exempeldata, med en delmängd med kolumner som visas. Du kan se att hello analys rutnätet för en viss klient begärande-ID visar data från hello-klientloggen och serverloggen spårning i nätverket.

![Azure Storage visa Layout](./media/storage-e2e-troubleshooting-classic-portal/view-layout-client-request-id-module.png)

> [!NOTE]
> Olika loggfilerna har olika kolumner, så när data från flera loggfiler visas i hello analys rutnätet vissa kolumner inte får innehålla några data för en viss rad. Till exempel i hello bilden ovan klienten loggen rader visas inte alla data för hello **tidsstämpel**, **TimeElapsed**, **källa**, och **mål** kolumner, eftersom dessa kolumner finns inte i hello-klientloggen, men finns i hello spårning i nätverket. På liknande sätt hello **tidsstämpel** visar kolumnen tidsstämpel data från hello serverloggen, men inga data visas för hello **TimeElapsed**, **källa**, och  **Mål** kolumner som inte är del av hello serverloggen.
>
>

Dessutom toousing hello Azure Storage layouter, du kan också definiera och spara egna layouter. Du kan välja andra önskade fält för gruppering av data och spara hello gruppering som en del av din anpassade layout.

### <a name="apply-color-rules-toohello-analysis-grid"></a>Tillämpa färg regler toohello analys rutnätet
hello lagring tillgångar också innefatta färgregler som ger en visuell innebär tooidentify olika typer av fel i hello analys rutnätet. hello gäller fördefinierade färgregler tooHTTP fel, så att de visas endast för hello logg- och serverspårning.

Välj tooapply färgregler **färgregler** från hello verktygsfältet menyfliksområdet. Du ser hello Azure Storage färgregler hello-menyn. Hello självstudier, Välj **klientfel (StatusCode mellan 400 och 499)**, som hello bilden nedan.

![Azure Storage visa Layout](./media/storage-e2e-troubleshooting-classic-portal/color-rules-menu.png)

Dessutom toousing hello Azure Storage färg regler, du kan också definiera och spara dina egna färgregler.

### <a name="group-and-filter-log-data-toofind-400-range-errors"></a>Gruppen och filtrera logga datafel toofind 400-intervall
Därefter vi gruppen och filtrera hello logga data toofind alla fel i hello 400 intervall.

1. Leta upp hello **StatusCode** kolumn i hello analys rutnät, högerklicka på hello kolumnen rubrik och välj **gruppen**.
2. Därefter gruppen på hello **ClientRequestId** kolumn. Du ser att hello data i hello analys rutnätet nu är ordnad efter status code och av klientbegäran-ID.
3. Visa hello visningsfilter verktygsfönster om den inte redan visas. Hello verktygsfältet menyfliksområdet, Välj **verktyget Windows**, sedan **visningsfilter**.
4. toofilter hello logga data toodisplay endast 400-intervallet fel, Lägg till hello följande filter kriterier toohello **visningsfilter** , och klicka på **Verkställ**:

    ```   
    (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)
    ```

hello bilden nedan visar hello resultatet av den här grupperingen och filter. Expanderande hello **ClientRequestID** fältet under hello gruppering för statuskod 409, till exempel visar en åtgärd som resulterade i att statuskod.

![Azure Storage visa Layout](./media/storage-e2e-troubleshooting-classic-portal/400-range-errors1.png)

Efter att det här filtret, ser du att rader från hello klientloggen undantas, som hello klientloggen inte innehåller en **StatusCode** kolumn. toobegin med vi granskar hello server och spåra loggar toolocate 404 nätverksfel och sedan returnerar vi toohello klienten loggen tooexamine hello Klientåtgärder som ledde toothem.

> [!NOTE]
> Du kan filtrera på hello **StatusCode** kolumn och fortfarande visa data från alla tre loggar, inklusive hello klientloggen om du lägger till ett uttryck toohello filter som innehåller poster där hello-statuskoden är null. tooconstruct denna filteruttrycket, Använd:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
>
> Det här filtret returnerar alla rader från hello klienten loggen och endast rader från hello serverloggen och HTTP-loggen där hello-statuskoden är större än 400. Om du använder den toohello Visa layout grupperade efter ID för klientbegäran och modulen, kan du söka eller bläddra igenom hello logga poster toofind sådana där alla tre loggar visas.   
>
>

### <a name="filter-log-data-toofind-404-errors"></a>Filtrera loggen data toofind 404-fel
hello lagring tillgångar inkludera fördefinierade filter som du kan använda toonarrow logga data toofind hello fel eller trender som du letar efter. Nu ska vi ska använda två fördefinierade filter: en som filtrerar hello server och nätverk spårningsloggar för 404-fel och en som filtrerar hello data på ett angivet tidsintervall.

1. Visa hello visningsfilter verktygsfönster om den inte redan visas. Hello verktygsfältet menyfliksområdet, Välj **verktyget Windows**, sedan **visningsfilter**.
2. I fönstret Visa Filter hello väljer **biblioteket**, och Sök på `Azure Storage` toofind hello Azure Storage-filter. Välj hello filter för **404 (inget hittas) meddelanden i alla loggar**.
3. Visa hello **biblioteket** menyn igen, och leta upp och markera hello **globala tidsfiltret**.
4. Redigera hello tidsstämplar visas i hello filter toohello intervallet gärna tooview. Detta hjälper toonarrow hello mängd data tooanalyze.
5. Filtret bör visas liknande toohello exemplet nedan. Klicka på **tillämpa** tooapply hello filter toohello analys rutnätet.

    ```   
    ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
    (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)
    ```

![Azure Storage visa Layout](./media/storage-e2e-troubleshooting-classic-portal/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Analysera dina loggdata
Nu när du har grupperats och filtreras data, kan du undersöka hello information om enskilda förfrågningar som genererade fel 404. I hello aktuella Visa layout grupperas hello data efter ID för klientbegäran, sedan efter källan. Eftersom vi filtrerar på begäranden där hello StatusCode fältet innehåller 404 kan se vi endast hello server och nätverk spårningsdata, inte hello klienten loggdata.

hello bilden nedan visar en specifik begäran där en hämta Blob-åtgärd gav en 404 eftersom hello blob inte finns. Observera att vissa kolumner har tagits bort från hello standardvyn i ordning toodisplay hello relevanta data.

![Filtrerade Server och nätverk spårningsloggar](./media/storage-e2e-troubleshooting-classic-portal/server-filtered-404-error.png)

Vi kommer därefter korrelera detta klient-ID för begäran med hello klienten logga data toosee vilka åtgärder hello-klienten tog när hello-fel inträffade. Du kan visa en ny analys rutnätsvy för den här sessionen tooview hello klienten loggdata som öppnas i andra fliken:

1. Kopiera först hello värdet för hello **ClientRequestId** fältet toohello Urklipp. Du kan göra detta genom att välja antingen rad hitta hello **ClientRequestId** fältet, högerklicka på hello datavärdet och välja **kopiera 'ClientRequestId'**.
2. Hello verktygsfältet menyfliksområdet, Välj **nya visningsprogrammet**och välj **analys rutnätet** tooopen en ny fliken hello ny flik visar alla data i loggfilerna, utan gruppering, filtrering eller färgregler.
3. Hello verktygsfältet menyfliksområdet, Välj **visa Layout**och välj **alla .NET klienten kolumner** under hello **Azure Storage** avsnitt. Layout för den här vyn visar data från hello-klientloggen samt hello server- och spårningsloggar. Som standard den sorteras på hello **MessageNumber** kolumn.
4. Därefter letar hello klientloggen hello klient begäran-ID. Hello verktygsfältet menyfliksområdet, Välj **hitta meddelanden**, ange ett filter på hello ID för klientbegäran i hello **hitta** fältet. Använd följande syntax för hello filter att ange en egen klient-ID för begäran:

    ```  
    *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"
    ```

Message Analyzer hittar och väljer hello första loggposten där hello sökvillkor matchar hello klient begäran-ID. Hello-klientloggen finns det flera poster för varje ID för klientbegäran, så du kan toogroup dem på hello **ClientRequestId** fältet toomake den enklare toosee dem alla tillsammans. hello bilden nedan visar alla hälsningsmeddelande i hello-klient loggar för hello angivna klient-begäran-ID.

![Klienten loggen visar 404-fel](./media/storage-e2e-troubleshooting-classic-portal/client-log-analysis-grid1.png)

Med hello data som visas i hello layouter i dessa två flikar kan analysera du hello begäran data toodetermine vad kan ha orsakat hello fel. Du kan också se begäranden som föregås av den här en toosee om en tidigare händelse har lett toohello 404-fel. Du kan till exempel granska hello klienten loggposter som föregår den här klienten begäran-ID toodetermine om hello blob kan ha tagits bort eller om hello fel på grund av toohello klientprogram anropar en CreateIfNotExists API på en behållare eller blobb. I hello klientloggar du hittar hello blob-adress i hello **beskrivning** fältet; i hello server och nätverk spårningsloggar informationen som visas i hello **sammanfattning** fältet.

När du vet hello-adressen för hello blob som gav hello 404-fel kan undersöka du vidare. Om du söker hello loggposter för andra meddelanden som är associerade med åtgärder på hello samma blob, du kan kontrollera om hello klienten tidigare bort hello entitet.

## <a name="analyze-other-types-of-storage-errors"></a>Analysera andra typer av lagring fel
Nu när du är bekant med Message Analyzer tooanalyze dina loggdata kan du analysera andra typer av fel i vyn layouter, färgregler och söka/filtrering. hello tabellerna nedan visar några problem du kan stöta på och hello filterkriterier som du kan använda toolocate dem. Mer information om hur du skapar filter och hello Message Analyzer filtrering språk, se [filtrering meddelandedata](http://technet.microsoft.com/library/jj819365.aspx).

| tooInvestigate... | Använd filteruttrycket... | Uttryck gäller tooLog (klient, Server, nätverk, alla) |
| --- | --- | --- |
| Oväntade fördröjningar i leverans av meddelanden i en kö |AzureStorageClientDotNetV4.Description innehåller ”försöker på nytt misslyckades igen”. |Client |
| HTTP-ökning av PercentThrottlingError |HTTP. Response.StatusCode == 500 &#124; &#124; HTTP. Response.StatusCode == 503 |Nätverk |
| Öka i PercentTimeoutError |HTTP. Response.StatusCode == 500 |Nätverk |
| Öka i PercentTimeoutError (alla) |* StatusCode == 500 |Alla |
| Öka i PercentNetworkError |AzureStorageClientDotNetV4.EventLogEntry.Level < 2 |Client |
| HTTP 403 (nekas) meddelanden |HTTP. Response.StatusCode == 403 |Nätverk |
| HTTP 404 (inget hittas) meddelanden |HTTP. Response.StatusCode == 404 |Nätverk |
| 404 (alla) |* StatusCode == 404 |Alla |
| Delade signatur åtkomst (SAS) auktorisering problemet |AzureStorageLog.RequestStatus == ”SASAuthorizationError” |Nätverk |
| HTTP 409 (konflikt) meddelanden |HTTP. Response.StatusCode == 409 |Nätverk |
| 409 (alla) |* StatusCode == 409 |Alla |
| Låg PercentSuccess eller analytics loggposter innehålla åtgärder med transaktionsstatus för ClientOtherErrors |AzureStorageLog.RequestStatus == ”ClientOtherError” |Server |
| Nagle varning |((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) och (AzureStorageLog.RequestPacketSize < 1 460) och (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200) |Server |
| Tidsintervall i Server- och loggar |#Timestamp > = 2014-10-20T16:36:38 och #Timestamp < = 2014-10-20T16:36:39 |Servern nätverk |
| Tidsintervall i Server-loggar |AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 och AzureStorageLog.Timestamp < = 2014-10-20T16:36:39 |Server |

## <a name="next-steps"></a>Nästa steg
Mer information om felsökning slutpunkt till slutpunkt-scenarier i Azure Storage finns i följande resurser:

* [Övervaka, diagnostisera och felsöka Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md)
* [Lagringsanalys](http://msdn.microsoft.com/library/azure/hh343270.aspx)
* [Övervaka ett lagringskonto i hello Azure-portalen](storage-monitor-storage-account.md)
* [Överföra data med hello kommandoradsverktyget azcopy](storage-use-azcopy.md)
* [Microsoft Message Analyzer fungerar Guide](http://technet.microsoft.com/library/jj649776.aspx)
