---
title: "aaaBuild en IoT-lösning med hjälp av Stream Analytics | Microsoft Docs"
description: "Komma igång-kursen för hello Stream Analytics IoT-lösningen på ett scenario med vaktkur"
keywords: "IOT-lösningen, fönstrets funktioner"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: e37fc5b56c4ffc4a2d7b820afe0c17631e577ea0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Skapa en IoT-lösning med hjälp av Stream Analytics
## <a name="introduction"></a>Introduktion
I den här kursen får du lära dig hur toouse Azure Stream Analytics tooget realtidsinsikter från dina data. Utvecklare kan enkelt kombinera dataströmmar, till exempel klicka på dataströmmar, loggar och händelser som genereras av enheten med historisk poster eller referens data tooderive affärsinsikter. Som en helt hanterad, realtid dataströmmen beräkning tjänst som finns i Microsoft Azure tillhandahåller Azure Stream Analytics inbyggd återhämtning, låg latens och skalbarhet tooget du upp och körs i minuter.

När du har slutfört den här självstudiekursen kommer du att kunna:

* Bekanta dig med hello Azure Stream Analytics-portalen.
* Konfigurera och distribuera ett direktuppspelningsjobb.
* Tydligt verkliga problem och lösa dem med hjälp av hello Stream Analytics-frågespråket.
* Utveckla strömning lösningar för kunderna genom att använda Stream Analytics med förtroende.
* Använd hello övervakning och loggning upplevelse tootroubleshoot problem.

## <a name="prerequisites"></a>Krav
Du kommer måste hello följande förutsättningar toocomplete den här kursen:

* hello senaste versionen av [Azure PowerShell](/powershell/azure/overview)
* Visual Studio 2017 2015 eller hello ledigt [Visual Studio Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
* En [Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/)
* Administratörsbehörighet på datorn hello
* Hämtning av [TollApp.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) från hello Microsoft Download Center
* Valfritt: Källkod för hello TollApp händelse generator i [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Scenariot introduktion: ”Hello, avgift”!
En avgift station är en gemensam företeelse. Du hittar dem i många utsättas bryggor och tunnlar mellan hello world. Varje station avgift har flera avgift kabiner. Vid manuell kabiner stoppa toopay hello avgift tooan attendant. Vid automatisk kabiner söker en sensor ovanpå varje monter RFID-kort som är anbringad toohello vindrutan av din fordon som du skickar hello avgift monter. Det är enkelt toovisualize hello övergången genom dessa avgift stationer som en händelse dataström som intressanta åtgärder kan utföras.

![Bild av bilar på avgift kabiner](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Inkommande data
Den här kursen fungerar med två dataströmmar. Sensorer installerade hello ingång och avsluta hello avgift stationer producera hello första dataströmmen. hello är andra en statisk sökning datamängd som har vehicle registreringsdata.

### <a name="entry-data-stream"></a>Posten dataström
dataströmmen för hello post innehåller information om bilar då de anträder avgift stationer.

| TollID | EntryTime | LicensePlate | Status | Kontrollera | Modellen | VehicleType | VehicleWeight | Avgift | Tagga |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |JNB 7001 |NY |Honda |CRV |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |YXZ 1001 |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |ABC 1004 |DATAFÄLT |Ford |Taurus |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |DATAFÄLT |Toyota |Corolla |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |BNJ 1007 |NY |Honda |CRV |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |CDE 1007 |NJ |Toyota |4 x 4 |1 |0 |6 |321987654 |

Här är en kort beskrivning av hello kolumner:

| Kolumn | Beskrivning |
| --- | --- |
| TollID |hello avgift monter ID som unikt identifierar en avgift monter |
| EntryTime |hello datum och tid för registrering av hello vehicle toohello avgift monter i UTC |
| LicensePlate |hello licens skylt antalet hello vehicle |
| Status |Ett tillstånd i USA |
| Kontrollera |hello tillverkaren av hello bil |
| Modellen |hello modellnumret för hello bil |
| VehicleType |Antingen 1 för fordon eller 2 för kommersiella fordon |
| WeightType |Vehicle vikt i ton; 0 för fordon |
| Avgift |hello avgift värdet i USD |
| Tagga |hello e-tagg på hello bil som automatiserar betalning. tomt där hello betalning gjordes manuellt |

### <a name="exit-data-stream"></a>Avsluta dataström
hello avsluta dataströmmen innehåller information om bilar lämnar hello avgift station.

| **TollId** | **ExitTime** | **LicensePlate** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.0000000Z |JNB 7001 |
| 1 |2014-09-10T12:03:00.0000000Z |YXZ 1001 |
| 3 |2014-09-10T12:04:00.0000000Z |ABC 1004 |
| 2 |2014-09-10T12:07:00.0000000Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.0000000Z |BNJ 1007 |
| 2 |2014-09-10T12:07:00.0000000Z |CDE 1007 |

Här är en kort beskrivning av hello kolumner:

| Kolumn | Beskrivning |
| --- | --- |
| TollID |hello avgift monter ID som unikt identifierar en avgift monter |
| ExitTime |hello datum och tid för Avsluta hello fordon från avgift monter i UTC |
| LicensePlate |hello licens skylt antalet hello vehicle |

### <a name="commercial-vehicle-registration-data"></a>Kommersiellt fordon registreringsdata
hello kursen använder en statisk ögonblicksbild av en databas för registrering av kommersiellt fordon.

| LicensePlate | RegistrationId | Upphört att gälla |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| SÄKERHETS 1005 |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

Här är en kort beskrivning av hello kolumner:

| Kolumn | Beskrivning |
| --- | --- |
| LicensePlate |hello licens skylt antalet hello vehicle |
| RegistrationId |hello vehicle registrerings-ID |
| Upphört att gälla |Hej registreringsstatus hello fordon: 0 om vehicle registrering är aktiv, 1 om registreringen har upphört att gälla |

## <a name="set-up-hello-environment-for-azure-stream-analytics"></a>Konfigurera hello miljö för Azure Stream Analytics
toocomplete den här självstudiekursen kommer du behöver ett Microsoft Azure-prenumeration. Microsoft erbjuder kostnadsfri utvärderingsversion för Microsoft Azure-tjänster.

Om du inte har ett Azure-konto kan du [begära en kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> toosign dig för en kostnadsfri utvärderingsversion, behöver du en mobil enhet som kan ta emot textmeddelanden och ett giltigt kreditkort.
> 
> 

Glöm toofollow hello stegen i avsnittet ”Rensa ditt Azure-konto” hello för hello slutet av den här artikeln så att du kan göra hello bästa användning av Azure-kredit.

## <a name="provision-azure-resources-required-for-hello-tutorial"></a>Etablera Azure-resurser som krävs för hello självstudiekursen
Den här kursen kräver två event hubs tooreceive *post* och *avsluta* dataströmmar. Azure SQL Database överför hello resultaten av hello Stream Analytics-jobb. Azure Storage lagrar referensdata om vehicle registreringar.

Du kan använda hello Setup.ps1 skript i hello TollApp mapp på GitHub toocreate alla nödvändiga resurser. Hello intresse tid rekommenderar vi att du kör den. Om du vill läsa mer om hur tooconfigure resurserna i hello Azure-portalen finns toolearn toohello ”konfigurera självstudiekurs resurser i Azure-portalen” tillägg.

Hämta och spara hello stöder [TollApp](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) mappar och filer.

Öppna en **Microsoft Azure PowerShell** fönstret *som administratör*. Om du inte ännu har Azure PowerShell, följer du anvisningarna för hello i [installera och konfigurera Azure PowerShell](/powershell/azure/overview) tooinstall den.

Eftersom Windows blockerar automatiskt .ps1, .dll och .exe-filer, måste tooset hello körningsprincipen innan du kör hello skript. Se till att hello Azure PowerShell-fönstret körs *som administratör*. Kör **Set-ExecutionPolicy obegränsad**. När du uppmanas ange **Y**.

![Skärmbild av ”Set-ExecutionPolicy obegränsad” körs i Azure PowerShell-fönster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Kör **Get-ExecutionPolicy** toomake till att hello kommandot fungerade.

![Skärmbild av ”Get-ExecutionPolicy” körs i Azure PowerShell-fönster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Gå toohello directory som har hello skript och generator program.

![Skärmbild av ”cd .\TollApp\TollApp” körs i hello Azure PowerShell-fönster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Typen **.\\ Setup.ps1** tooset in din Azure-konto, skapa och konfigurera alla nödvändiga resurser och starta toogenerate händelser. hello skript hämtar slumpmässigt en region toocreate dina resurser. tooexplicitly anger en region, du kan skicka hello **-plats** parameter som i följande exempel hello:

**. \\Setup.ps1-platsen ”centrala USA”**

![Skärmbild av sidan hello Azure-inloggning](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

hello skriptet öppnas hello **logga In** sidan för Microsoft Azure. Ange autentiseringsuppgifterna för ditt konto.

> [!NOTE]
> Om ditt konto har åtkomst toomultiple prenumerationer, kommer du att och tooenter hello prenumerationsnamn som du vill toouse hello genomgång.
> 
> 

hello skript kan ta flera minuter toorun. När den är klar hello utdata bör se ut som följande skärmbild hello.

![Skärmbild av utdata hello skriptet i hello Azure PowerShell-fönster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Ett annat fönster som är liknande toohello följande skärmbild visas också. Det här programmet skickar händelser tooAzure Händelsehubbar som är nödvändiga toorun hello kursen. Därför inte stoppa hello program eller stänga det här fönstret förrän du har avslutat hello kursen.

![Skärmbild av ”skicka event hub data”](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Du ska kunna toosee dina resurser i Azure-portalen nu. Gå för<https://portal.azure.com>, och logga in med autentiseringsuppgifterna för ditt konto. Observera att för närvarande vissa funktioner använder hello klassiska portalen. De här stegen att tydligt anges.

### <a name="azure-event-hubs"></a>Azure Event Hubs
I hello Azure-portalen klickar du på **fler tjänster** hello längst ned i hello hantering av vänstra fönstret. Typen **händelsehubbar** i hello fältet och klickar på **händelsehubbar**. Detta startar en ny webbläsare fönstret toodisplay hello **SERVICE BUS** område i hello **klassiska portalen**. Här kan du se hello Event Hub som skapats av hello Setup.ps1 skript.

![Service Bus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Klicka på hello som börjar med *tolldata*. Klicka på hello **HÄNDELSEHUBBAR** fliken. Du ser två händelsehubbar med namnet *post* och *avsluta* skapas i det här namnområdet.

![Event Hubs fliken i hello klassiska portalen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

### <a name="azure-storage-container"></a>Azure Storage-behållare
1. Gå tillbaka toohello fliken i din webbläsare öppna tooAzure portal. Klicka på **lagring** på vänster sida av hello Azure portal toosee hello Azure Storage-behållare som används i självstudiekursen hello hello.
   
    ![Menyalternativet för lagring](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)
2. Klicka på hello något som börjar med *tolldata*. Klicka på hello **behållare** fliken toosee hello skapas behållaren.
   
    ![Fliken behållare i hello Azure-portalen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)
3. Klicka på hello **tolldata** behållare toosee hello upp JSON-fil som har vehicle registreringsdata.
   
    ![Skärmbild av hello registration.json filen i hello behållaren](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

### <a name="azure-sql-database"></a>Azure SQL Database
1. Gå tillbaka toohello Azure-portalen på hello första fliken som har öppnats i hello webbläsare. Klicka på **SQL-databaser** på vänster sida av hello Azure portal toosee hello SQL-databas som ska användas i hello självstudier och klickar på hello **tolldatadb**.
   
    ![Skärmbild av hello skapade SQL-databas](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)
2. Kopiera hello servernamn utan hello portnummer (*servername*. database.windows.net, till exempel).
    ![Skärmbild av hello skapade SQL-databas db](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15a.png)

## <a name="connect-toohello-database-from-visual-studio"></a>Ansluta toohello databasen från Visual Studio
Använd Visual Studio tooaccess frågeresultat i hello Utdatadatabasen.

Anslut toohello SQL-databas (hello mål) från Visual Studio:

1. Öppna Visual Studio och klicka sedan på **verktyg** > **ansluta tooDatabase**.
2. Om du blir tillfrågad, klickar du på **Microsoft SQL Server** som en datakälla.
   
    ![Ändra dialogrutan för datakälla](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)
3. I hello **servernamn** fältet, klistra in hello-namn som du kopierade i föregående avsnitt i hello från hello Azure-portalen (det vill säga *servername*. database.windows.net).
4. Klicka på **använda SQL Server-autentisering**.
5. Ange **tolladmin** i hello **användarnamn** fält och **123toll!** i hello **lösenord** fältet.
6. Klicka på **Välj eller ange ett databasnamn**, och välj **TollDataDB** som hello-databas.
   
    ![Lägg till dialogrutan anslutning](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)
7. Klicka på **OK**.
8. Öppna Server Explorer.
   
    ![Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)
9. Se fyra tabeller i hello TollDataDB databas.
   
    ![Tabeller i hello TollDataDB databas](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Händelsen generator: TollApp exempelprojektet
hello PowerShell-skript startar automatiskt toosend händelser med hjälp av hello TollApp programmet exempelprogrammet. Du behöver inte tooperform eventuella ytterligare steg.

Du är intresserad av implementeringsdetaljer kan du söka efter hello källkoden för hello TollApp program i GitHub [exempel/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Skärmbild av exempelkod som visas i Visual Studio](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Skapa ett Stream Analytics-jobb
1. I hello Azure-portalen hello Klicka grönt plustecken i hello övre vänstra hörnet i hello sidan toocreate ett nytt Stream Analytics-jobb. Välj **Intelligence + analys** och klicka sedan på **Stream Analytics-jobbet**.
   
    ![Knappen Nytt](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)
2. Ange ett jobbnamn, verifiera hello prenumerationen är korrekt och sedan skapa en ny resursgrupp i hello samma region som hello Event hub lagring (standard är södra centrala USA för hello skriptet).
3. Klicka på **PIN-kod toodashboard** och sedan **skapa** på hello hello sidans nederkant.
   
    ![Skapa Stream Analytics-jobbet alternativ](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Definiera indatakällor
1. hello jobbet skapar och öppnar hello jobbet sidan. Eller klicka hello skapas analytics-jobbet på hello portalens instrumentpanel.

2. Klicka på hello **indata** fliken toodefine hello källdata.
   
    ![hello indata fliken](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.png)
3. Klicka på **lägga till indata**.
   
    ![hello Lägg till ett indata-alternativ](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)
4. Ange **EntryStream** som **INMATADE ALIASET**.
5. Datakällans typ är **dataström**
6. Källan är **händelsehubb**.
7. **Service bus-namnrymd** ska vara hello TollData något i hello listrutan.
8. **Namnet på händelsehubben** ska anges för**post**.
9. **Namnet på händelsehubben princip*är **RootManageSharedAccessKey** (hello standardvärdet).
10. Välj **JSON** för **händelse SERIALISERINGSFORMAT** och **UTF8** för **KODNING**.
   
    Dina inställningar kommer att se ut:
   
    ![Händelsehubbsinställningar](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

10. Klicka på **skapa** längst hello hello toofinish hello guiden.
    
    Nu när du har skapat hello post stream, ska du följa hello samma steg toocreate hello avsluta dataström. Vara säker på att tooenter värden som på hello följande skärmbild.
    
    ![Inställningar för hello avsluta ström](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)
    
    Du har definierat två inkommande dataströmmar:
    
    ![Definierats inkommande dataströmmar i hello Azure-portalen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.png)
    
    Sedan ska du lägga till referensindata för hello blob-fil som innehåller bil registreringsdata.
11. Klicka på **lägga till**, och följ sedan hello samma process för hello dataströmmen indata men väljer **REFERENSDATA** i stället för **dataströmmen** och hello **indata Alias**  är **registrering**.

12. Storage-konto som börjar med **tolldata**. hello behållarens namn bör vara **tolldata**, och hello **SÖKVÄGAR** ska vara **registration.json**. Det här namnet är skiftlägeskänsligt och måste vara **gemener**.
    
    ![Inställningar för lagring av blogg](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)
13. Klicka på **skapa** toofinish hello guiden.

Nu har alla indata definierats.

## <a name="define-output"></a>Definiera målet
1. Översiktsrutan hello Stream Analytics-jobbet, Välj **utdata**.
   
    ![Hej fliken utmatning och alternativet ”Lägg till utdata”](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.png)
2. Klicka på **Lägg till**.
3. Ange hello **utdataalias** too'output ”och sedan **Sink** för**SQL-databas**.
3. Välj hello servernamnet som användes i hello ”Anslut tooDatabase från Visual Studio” avsnittet av hello artikel. hello databasnamnet är **TollDataDB**.
4. Ange **tolladmin** i hello **användarnamn** fältet **123toll!** i hello **lösenord** fältet och **TollDataRefJoin** i hello **tabell** fältet.
   
    ![Inställningar för SQL-databas](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.png)
5. Klicka på **Skapa**.

## <a name="azure-stream-analytics-query"></a>Azure Stream analytics-fråga
Hej **FRÅGAN** fliken innehåller en SQL-fråga att transformeringar hello inkommande data.

![En fråga läggs fliken för toohello fråga](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.png)

Den här självstudiekursen försöker tooanswer flera företag frågor om tootoll data och konstruktioner Stream Analytics-frågor som kan användas i Azure Stream Analytics tooprovide relevant svar.

Innan du börjar din första Stream Analytics-jobbet utforska vi några scenarier och hello frågesyntaxen.

## <a name="introduction-tooazure-stream-analytics-query-language"></a>Introduktion tooAzure Stream Analytics-frågespråket
- - -
Anta att du behöver toocount hello antalet fordon som anger en avgift monter. Eftersom detta är en ständig ström med händelser har du toodefine en ”period tid”. Nu ska vi ändra hello fråga toobe ”hur många fordon anger en avgift monter alla tre minuter”?. Det här är ofta hänvisas tooas hello rullande count.

Nu ska vi titta på hello Azure Stream Analytics-fråga som svar på den här frågan:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Som du kan se använder Azure Stream Analytics ett frågespråk som som SQL och lägger till några tillägg toospecify tidsrelaterade aspekter av hello frågan.

Mer information finns i avsnittet om [tidshantering](https://msdn.microsoft.com/library/azure/mt582045.aspx) och [fönsterhantering](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstruktioner som används i hello frågan från MSDN.

## <a name="testing-azure-stream-analytics-queries"></a>Testa Azure Stream Analytics-frågor
Nu när du har skrivit ditt första Azure Stream Analytics-fråga är tid tootest den med hjälp av exempeldatafiler finns i mappen TollApp i hello följande sökväg:

**.. \\TollApp\\TollApp\\Data**

Den här mappen innehåller hello följande filer:

* Entry.JSON
* Exit.JSON
* Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Fråga 1: Antal fordon att ange en avgift monter
1. Öppna hello Azure-portalen och gå tooyour skapade Azure Stream Analytics-jobbet. Klicka på hello **FRÅGAN** fliken och klistra in frågan från hello föregående avsnitt.

2. toovalidate den här frågan mot exempeldata, överför hello data till hello EntryStream indata genom att klicka på hello... symbol och välja **ladda upp exempeldata från filen**.

    ![Skärmbild av hello Entry.json fil](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)
3. Hello i fönstret som visas väljer hello-fil (Entry.json) på din lokala dator och klicka på **OK**. Hej **Test** ikon kommer nu belysa och vara klickbara.
   
    ![Skärmbild av hello Entry.json fil](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.png)
3. Verifiera att hello utdata från hello frågan är korrekt:
   
    ![Resultaten av hello test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.png)

## <a name="question-2-report-total-time-for-each-car-toopass-through-hello-toll-booth"></a>Fråga 2: Rapporten total tid för varje bil toopass via hello avgift monter
hello Genomsnittlig tid som krävs för en bil toopass via hello avgift hjälper tooassess hello effektiviteten för hello processen och hello kundupplevelsen.

toofind hello totala tiden du behöver toojoin hello EntryTime ström med hello ExitTime dataströmmen. Du kommer att ansluta till hello dataströmmar för TollId och LicencePlate kolumner. Hej **ansluta** operatören kräver toospecify temporal handtag som beskriver hello acceptabel tid skillnaden mellan hello ansluten händelser. Du kommer att använda **DATEDIFF** fungerar toospecify att händelser ska vara mer än 15 minuter från varandra. Du kan också använda hello **DATEDIFF** funktionen tooexit och transaktionen tider toocompute hello faktiska tiden som en bil tillbringar i hello avgiftsbelagt station. Observera hello skillnaden mellan hello användning av **DATEDIFF** när den används i en **Välj** instruktionen snarare än en **ansluta** villkor.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. tootest den här frågan update hello-fråga på hello **FRÅGAN** för hello jobbet. Lägg till hello testfil för **ExitStream** precis som **EntryStream** angavs ovan.
   
2. Klicka på **Test**.

3. Välj hello kryssrutan tootest hello frågan och visa hello utdata:
   
    ![Utdata från hello test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Fråga 3: Rapportera alla kommersiella fordon med utgångna registrering
Azure Stream Analytics kan använda statiska ögonblicksbilder av data toojoin med temporala dataströmmar. toodemonstrate den här funktionen kan använda hello följande exempel fråga.

Om en kommersiell vehicle registreras med hello avgift företag, släpper den igenom hello avgift monter utan att ha stoppats för inspektion. Du använder kommersiellt fordon registrering sökning tabell tooidentify alla kommersiella fordon som har upphört att gälla registreringar.

```
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

tootest en fråga med hjälp av referensdata, behöver du toodefine ingen källa för hello referensdata som du redan har gjort.

tootest den här frågan, klistra in hello frågan i hello **FRÅGAN** klickar du på **Test**, och ange hello två indatakällor hello registrering exempeldata och klicka på **Test**.  
   
![Utdata från hello test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

## <a name="start-hello-stream-analytics-job"></a>Starta hello Stream Analytics-jobbet
Nu är det toofinish hello konfiguration och starta hello jobbet. Spara hello frågan från fråga 3 som resultat som matchar hello schemat för hello **TollDataRefJoin** utdatatabellen.

Gå toohello jobbet **INSTRUMENTPANELEN**, och klicka på **starta**.

![Skärmbild av hello Start-knappen i hello jobbet instrumentpanel](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.png)

Ändra hello hello dialogrutan som öppnas, **starta utdata** tid för**anpassad tid**. Ändra hello timme tooone timme före hello aktuell tid. Den här ändringen gör att alla händelser från hello händelsehubb bearbetas eftersom du startade toogenerate hello händelser hello början av hello kursen. Klicka på hello **starta** knappen toostart hello jobb.

![Val av anpassade tid](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Startar hello jobb kan ta några minuter. Du kan se status för hello på översta nivån hello-sidan för Stream Analytics.

![Skärmbild av hello hello jobbets status](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.png)

## <a name="check-results-in-visual-studio"></a>Kontrollera resultaten i Visual Studio
1. Öppna Visual Studio Server Explorer och högerklicka på hello **TollDataRefJoin** tabell.
2. Klicka på **visa tabelldata** toosee hello utdata för jobbet.
   
    ![Val av ”Visa Data för tabell” i Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Skala ut Azure Stream Analytics-jobb
Azure Stream Analytics utformats tooelastically skala så att den kan hantera stora mängder data. hello Azure Stream Analytics-fråga kan använda en **PARTITION BY** satsen tootell hello system som det här steget ska skalas ut. **PartitionId** är en särskild kolumn som hello system lägger till toomatch hello partitions-ID för hello-indata (händelsehubb).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Stoppa hello aktuella jobb, uppdatering hello frågan i hello **FRÅGAN** flik och öppna hello **inställningar** redskap hello jobbet instrumentpanelen. Klicka på **skala**.
   
    **STREAMING enheter** definiera hello mycket datorkraft som hello jobb kan ta emot.
2. Ändra hello nedrullningsbara från 1 från 6.
   
    ![Skärmbild av du väljer 6 enheter för strömning](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.png)
3. Gå toohello **utdata** fliken och ändra hello namnet på hello SQL-tabellen för**TollDataTumblingCountPartitioned**.

Om du startar hello jobbet nu kan Azure Stream Analytics distribuera arbete i fler beräkningsresurser och få bättre genomströmning. Observera att hello TollApp programmet även skickar händelser som har partitionerats med TollId.

## <a name="monitor"></a>Övervaka
Hej **ÖVERVAKAREN** innehåller statistik om hello köra jobb. Första gången konfiguration behövs toouse hello storage-konto i hello samma region (namnet avgift som hello resten av det här dokumentet).   

![Skärmbild av Övervakare](media/stream-analytics-build-an-iot-solution-using-stream-analytics/monitoring.png)

Du kan komma åt **aktivitetsloggar** hello jobbet instrumentpanel **inställningar** samt område.


## <a name="conclusion"></a>Slutsats
Den här kursen introduceras toohello Azure Stream Analytics-tjänsten. Det visas hur tooconfigure indata och utdata för hello Stream Analytics-jobbet. Med hello avgift Data scenariot beskrivs hello kursen vanliga problem som uppstår i hello utrymme för data i rörelse och hur de kan lösas med den enkla SQL-liknande frågor i Azure Stream Analytics. hello kursen beskrivs SQL tillägget konstruktioner för att arbeta med temporala data. Den visade hur toojoin dataströmmar, hur tooenrich hello dataströmmen med statiska referensdata och hur tooscale ut en fråga tooachieve högre genomströmning.

Även om den här kursen ger en bra introduktion, är det inte fullständig på något sätt. Du kan hitta mer frågemönster med hello SAQL språk på [fråga exempel för vanliga Stream Analytics användningsmönster](stream-analytics-stream-analytics-query-patterns.md).
Se toohello [onlinedokumentation](https://azure.microsoft.com/documentation/services/stream-analytics/) toolearn mer om Azure Stream Analytics.

## <a name="clean-up-your-azure-account"></a>Rensa ditt Azure-konto
1. Stoppa hello Stream Analytics-jobbet i hello Azure-portalen.
   
    Hej Setup.ps1 skript skapar två händelsehubbar och en SQL-databas. följande instruktioner hjälp Rensa resurser hello slutet av kursen hello hello.
2. Ange i ett PowerShell-fönster **.\\ CleanUp.ps1** toostart hello skript som tar bort resurser som används i hello självstudiekursen.
   
   > [!NOTE]
   > Resurser som identifieras av hello namn. Kontrollera att varje objekt granska noggrant innan du bekräftar borttagningen.
   > 
   > 


