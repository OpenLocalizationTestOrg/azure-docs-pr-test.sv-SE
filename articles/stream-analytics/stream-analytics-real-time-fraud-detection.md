# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Komma igång med Azure Stream Analytics: att upptäcka bedrägerier i realtid

Den här självstudiekursen innehåller en slutpunkt till slutpunkt illustration av hur toouse Azure Stream Analytics. Lär dig att: 

* Ta strömning händelser till en instans av Händelsehubbar i Azure. I den här kursen ska du använda en app som vi tillhandahåller som simulerar en dataström med metadata för mobiltelefon poster.

* Skriva SQL-liknande Stream Analytics-frågor tootransform data, samla information eller söker efter mönster. Du ser hur toouse en fråga tooexamine hello inkommande dataström och leta efter anrop som kan vara falsk.

* Skicka hello resultat tooan utdatamottagaren (lagring) som du kan analysera för ytterligare information. I det här fallet ska du skicka hello misstänkta anropet data tooAzure Blob storage.

Vi använder hello exempel på realtid bedrägeri identifiering baserat på telefonsamtal data i de här självstudierna. Men hello-tekniken som beskriver vi är passar också för andra typer av att upptäcka bedrägerier, till exempel kreditkort bedrägeri eller identitetsstöld. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenario: Telekommunikation och SIM att upptäcka bedrägerier i realtid

Ett telekommunikation företag har stora mängder data för inkommande samtal. hello företaget vill ha toodetect bedrägliga anropar i realtid så att de kan meddela kunder eller stänga av tjänsten för ett visst tal. En typ av SIM bedrägeri omfattar flera anrop från hello samma identitet runt hello samma tid men med geografiskt olika platser. toodetect den här typen av bedrägerier, hello företagets behov tooexamine inkommande phone poster och leta efter specifika mönster – i det här fallet för gjorts runt hello-anrop samtidigt i olika länder. Alla telefonen poster som tillhör den här kategorin skrivs toostorage för efterföljande analys.

## <a name="prerequisites"></a>Krav

I den här självstudiekursen kommer du simulera telefonsamtal data med hjälp av ett klientprogram som genererar exempel telefonsamtal metadata. Vissa av hello-poster som hello app producerar ser ut som bedrägliga anrop. 

Innan du börjar bör du kontrollera att du har hello följande:

* Ett Azure-konto.
* hello anropet händelse generator app. Du kan få detta genom att hämta hello [TelcoGenerator.zip filen](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) från hello Microsoft Download Center. Packa upp paketet till en mapp på datorn. Om du vill toosee hello källkoden och kör hello app i en felsökare, kan du få hello app källkod från [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

    >[!NOTE]
    >Windows kan blockera hello hämtade ZIP-filen. Om du går inte att packa upp Högerklicka hello-filen och välj **egenskaper**. Om du ser hello meddelandet ”den här filen kommer från en annan dator och kan vara blockerade toohelp skydda den här datorn”, Välj hello **avblockera** alternativ och klickar sedan på **tillämpa**.

Om du vill tooexamine hello resultaten av hello Streaming Analytics-jobbet, måste du också ett verktyg för att visa hello innehållet i en Azure Blob Storage-behållare. Om du använder Visual Studio, kan du använda [Azure Tools för Visual Studio](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) eller [Visual Studio Cloud Explorer](https://docs.microsoft.com/en-us/azure/vs-azure-tools-resources-managing-with-cloud-explorer). Du kan också installera fristående verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/) eller [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

## <a name="create-an-azure-event-hubs-tooingest-events"></a>Skapa en Azure händelse hubbar tooingest händelser

tooanalyze en dataström du *infognings-* till Azure. Ett vanligt tooingest data är toouse [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), där du kan mata in miljontals händelser per sekund och sedan bearbeta och lagra hello händelseinformation. För den här självstudiekursen skapar en händelsehubb och har hello anropet händelse generator app skicka anrop data toothat händelsehubb. Mer information om händelsehubbar finns i hello [Azure Service Bus-dokumentationen](https://docs.microsoft.com/en-us/azure/service-bus/).

>[!NOTE]
>En mer detaljerad version av den här proceduren finns [skapa ett namnområde för Händelsehubbar och en händelse hubb med hello Azure-portalen](../event-hubs/event-hubs-create.md). 

### <a name="create-a-namespace-and-event-hub"></a>Skapa ett namnområde och händelse-hubb
I den här proceduren du först skapa en event hub-namnområde och sedan lägger du till en event hub toothat namepsace. Event hub namnområden används toologically gruppera relaterade händelser-bussen instanser. 

1. Logga in på hello Azure-portalen och på **ny** > **Sakernas Internet** > **Händelsehubb**. 

2. I hello **skapa namnområdet** bladet, ange ett namn för namnområdet som `<yourname>-eh-ns-demo`. Du kan använda valfritt namn för hello namnområde, men hello-namnet måste vara giltigt för en URL och det måste vara unikt i Azure. 
    
3. Välj en prenumeration och skapa eller välja en resursgrupp och sedan klicka på **skapa**. 

    ![Skapa ett namnområde för event hub](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png)
 
4. Hitta hello event hub namnområde i din lista över Azure-resurser när distribution hello namnområde har slutförts. 

5. Klicka på hello Nytt namnområde och hello namnområde bladet på  **+ &nbsp;Händelsehubb**. 

    ![hello lägga till Event Hub-knappen för att skapa en ny händelsehubb ](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
6. Namnet hello ny händelsehubb `sa-eh-frauddetection-demo`. Du kan använda ett annat namn. Om du vill anteckna, eftersom du måste ha namnet hello senare. Du behöver inte tooset andra alternativ för hello händelsehubb just nu.

    ![Bladet för att skapa en ny händelsehubb](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png)
    
 
7. Klicka på **Skapa**.
### <a name="grant-access-toohello-event-hub-and-get-a-connection-string"></a>Bevilja åtkomst toohello händelsehubb och få en anslutningssträng

Innan en process kan skicka data tooan händelsehubb, måste hello händelsehubb ha en princip som tillåter åtkomstbehörighet. hello åtkomstprincip producerar en anslutningssträng som innehåller auktoriseringsinformation om.

1.  I hello händelse namnområde bladet, klickar du på **Händelsehubbar** och klicka sedan på hello namnet på din nya event hub.

2.  I hello event hub-blad klickar du på **principer för delad åtkomst** och klicka sedan på  **+ &nbsp;Lägg till**.

    >[!NOTE]
    >Kontrollera att du arbetar med hello händelsehubb, inte hello event hub namnområde.

3.  Lägg till en princip med namnet `sa-policy-manage-demo` och **anspråk**väljer **hantera**.

    ![Bladet för att skapa en ny händelse hubb princip](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png)
 
4.  Klicka på **Skapa**.

5.  Efter hello principen har distribuerats, klickar du på hello lista över principer för delad åtkomst.

6.  Hitta hello rutan **sträng-primära ANSLUTNINGSNYCKEL** och klicka på hello Kopiera knappen Nästa toohello-anslutningssträng. 
    
    ![Kopiera hello primära sträng anslutningsnyckel från hello åtkomstprincipen](./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png)
 
7.  Klistra in hello anslutningssträngen i en textredigerare. Du behöver den här anslutningssträngen för hello nästa avsnitt, när du har gjort vissa små redigeringar tooit.

    hello anslutningssträngen ser ut så här:

        Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=sa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=sa-eh-frauddetection-demo

    Observera att hello-anslutningssträngen innehåller flera nyckel-värdepar, avgränsade med semikolon: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, och `EntityPath`.  

## <a name="configure-and-start-hello-event-generator-application"></a>Konfigurera och starta hello händelse generator program

Innan du börjar hello TelcoGenerator app måste konfigurera du den så att den skickar anropet poster toohello händelsehubb du just har skapat.

### <a name="configure-hello-telcogeneratorapp"></a>Konfigurera hello TelcoGeneratorapp

1.  I Redigeraren för hello dit du kopierade hello anslutningssträngen anteckna hello `EntityPath` värdet och ta sedan bort hello `EntityPath` par (inte Glöm tooremove hello semikolon som föregår den). 

2.  Öppna i hello mappen där du uppackade hello TelcoGenerator.zip filen hello telcodatagen.exe.config fil i en textredigerare. (Det finns fler än en .config-fil, så Tänk på att du öppnar hello höger en).

3.  I hello `<appSettings>` element, göra detta:

    * Värdet för hello hello `EventHubName` viktiga toohello händelsehubbens namn (det vill säga toohello värdet hello entitet sökväg).
    * Värdet för hello hello `Microsoft.ServiceBus.ConnectionString` nyckeln toohello anslutningssträngen. 

    Hej `<appSettings>` avsnitt ser ut hello följande exempel. (För tydlighetens skull vi omsluten hello rader och ta bort vissa tecken från hello autentiseringstoken.)

    ![Konfigurationsfilen för TelcoGenerator app visar hello händelse hubb och anslutningssträngen](./media/stream-analytics-real-time-fraud-detection/stream-analytics-telcogenerator-config-file-app-settings.png)
 
4.  Spara hello-filen. 

### <a name="start-hello-app"></a>Starta hello app
1.  Öppna ett kommandofönster och ändra toohello mappen där hello TelcoGenerator app uppackade.
2.  Ange hello följande kommando:

        telcodatagen.exe 1000 .2 2

    hello-parametrar är: 

    * Antal skivor per timme. 
    * SIM-kort bedrägeri sannolikhet: Hur ofta som en procentandel av alla anrop hello appen bör Simulera ett bedrägliga anrop. hello värdet.2 innebär att cirka 20% av hello anropet poster ser falska.
    * Varaktighet i timmar. hello antal timmar som hello app ska köras. Du kan även stoppa hello app helst genom att trycka på Ctrl + C hello-kommandoraden.

    Efter några sekunder hello appen startar visa telefonsamtal poster på hello-skärmen när den skickar toohello händelsehubb.

Är några av hello nyckelfälten som du ska använda i realtid bedrägeri identifiering av programmet hello följande:

|**Post**|**Definition**|
|----------|--------------|
|`CallrecTime`|hello tidsstämpel för hello anrop starttid. |
|`SwitchNum`|hello telefonväxeln används tooconnect hello anrop. I det här exemplet är hello växlar strängar som representerar hello land ursprungsplats (USA Kina, Storbritannien, Tyskland eller Australien). |
|`CallingNum`|hello telefonnummer hello anroparen. |
|`CallingIMSI`|hello International Mobile Subscriber Identity IMSI (). Detta är hello Unik identifierare för hello anroparen. |
|`CalledNum`|hello telefonnummer hello anropet mottagaren. |
|`CalledIMSI`|Internationella Mobile Subscriber Identity (IMSI). Detta är hello Unik identifierare för hello anropet mottagaren. |


## <a name="create-a-stream-analytics-job-toomanage-streaming-data"></a>Skapa ett Stream Analytics-jobbet toomanage strömmande data

Nu när du har en dataström med anropet händelser kan ställa du in ett Stream Analytics-jobb. hello-jobbet kommer att läsa data från hello händelsehubb som du ställer in. 

### <a name="create-hello-job"></a>Skapa hello jobb 

1. I hello Azure-portalen klickar du på **ny** > **Sakernas Internet** > **Stream Analytics-jobbet**.

2. Hello jobb `sa_frauddetection_job_demo`, ange en prenumeration, resursgrupp och plats.

    Det är en bra idé tooplace hello jobbet och hello händelsehubb i hello samma region för bästa prestanda och så att du inte betalar tootransfer data mellan regioner.

    ![Skapa nytt Stream Analytics-jobb](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png)

3. Klicka på **Skapa**.

    hello jobb skapas och hello portalen visar jobbinformation. Inget kör ännu, men – du har tooconfigure hello jobb innan den kan startas.

### <a name="configure-job-input"></a>Konfigurera jobbet indata

1. I hello instrumentpanel eller hello **alla resurser** bladet, söka efter och välj hello `sa_frauddetection_job_demo` Stream Analytics-jobbet. 
2. I hello **jobbet topologi** avsnitt i hello Stream Analytics-jobbet bladet, klickar du på hello **indata** rutan.

    ![Textrutan under topologi i hello Streaming Analytics-jobbet bladet](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. Klicka på  **+ &nbsp;Lägg till** och fyller sedan hello bladet med dessa värden:

    * **Ett inmatat alias**: Använd hello namn `CallStream`. Om du använder ett annat namn, notera den eftersom du behöver senare.
    * **Typ av datakälla**: Välj **dataströmmen**. (**Referensdata** refererar toostatic sökning data, som du inte använder i den här självstudiekursen.)
    * **Källan**: Välj **händelsehubb**.
    * **Importera alternativet**: Välj **Använd händelsehubb från aktuell prenumeration**. 
    * **Service bus-namnrymd**: Markera hello event hub namnområde som du skapade tidigare (`<yourname>-eh-ns-demo`).
    * **Händelsehubb**: Välj hello händelsehubb som du skapade tidigare (`sa-eh-frauddetection-demo`).
    * **Namnet på händelsehubben princip**: Välj hello åtkomstprincip som du skapade tidigare (`sa-policy-manage-demo`).

    ![Skapa nya indata för Streaming Analytics-jobbet](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png)

4. Klicka på **Skapa**.

## <a name="create-queries-tootransform-real-time-data"></a>Skapa frågor tootransform realtidsdata

Du har nu en Stream Analytics-jobbet konfigurera tooread en inkommande dataström. hello nästa steg är toocreate en omvandling som analyserar hello data i realtid. Det gör du genom att skapa en fråga. Stream Analytics stöder en enkel, deklarativ frågemodell som beskriver omvandlingarna för realtidsbearbetning. hello frågor via en SQL-liknande språk som har vissa tillägg specifika toostream analytics. 

En mycket enkel fråga kan bara läsa alla hello inkommande data. Men skapa du ofta frågor som ser ut för specifika data eller relationer i hello data. I det här avsnittet av kursen hello ska du skapa och testa flera frågor toolearn några sätt som du kan omvandla en indataström för analys. 

hello-frågor som du skapar här visas bara hello transformerade data toohello skärmen. I ett senare avsnitt får du konfigurera utdatamottagare och en fråga som skriver hello transformerade data toothat mottagare.

toolearn mer om hello språk finns hello [referens för Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/dn834998.aspx).

### <a name="get-sample-data-for-testing-queries"></a>Hämta exempeldata för att testa frågor

Hej TelcoGenerator appen skickar anropet poster toohello händelsehubb och Stream Analytics-jobbet är konfigurerade tooread från hello händelsehubb. Du kan använda en fråga tootest hello jobbet toomake att läsa på rätt sätt. Testa för en fråga i hello Azure-konsolen måste du exempeldata. I den här genomgången ska du extrahera exempeldata från hello-dataström som kommer till hello händelsehubb.

1. Kontrollera att hello TelcoGenerator appen körs och producerar anropet poster.
2. Returnera toohello Streaming Analytics-jobbet bladet i hello-portalen. (Om du har stängt hello-bladet, söka efter `sa_frauddetection_job_demo` i hello **alla resurser** bladet.)
3. Klicka på hello **frågan** rutan. Azure visar hello indata och utdata som är konfigurerade för hello jobbet och kan du skapa en fråga som kan du omvandla hello Indataströmmen som toohello utdata skickas.
4. I hello **frågan** bladet, klickar du på hello punkter nästa toohello `CallStream` indata och välj sedan **exempeldata från indata**.

    ![Menyn Alternativ toouse exempeldata för hello Streaming Analytics-jobbet post med ”exempeldata från indata” markerad](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)

    Då öppnas ett blad som du kan ange hur mycket exempel data tooget som definierats i hur länge tooread hello inkommande dataström.

5. Ange **minuter** too3 och klicka sedan på **OK**. 
    
    ![Alternativ för provtagning hello Indataströmmen, med ”3 minuter” markerad.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure exempel 3 minuter kan du se från hello Indataströmmen och meddelar dig när hello exempeldata är klar. (Detta tar en stund.) 

hello exempeldata lagras tillfälligt och är tillgänglig när du har hello frågefönster öppnas. Om du stänger hello frågefönstret hello exempeldata ignoreras och du har toocreate en ny uppsättning exempeldata. 

Alternativt kan du hämta en JSON-fil som innehåller exempeldata [från GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json), och sedan ladda upp toouse som JSON-fil som exempeldata för hello `CallStream` indata. 

### <a name="test-using-a-pass-through-query"></a>Testa med en direktfråga

Om du vill tooarchive varje händelse, kan du använda en direktfråga tooread alla hello fält i hello nyttolast hello-händelse.

1. Ange den här frågan i frågefönstret hello:

        SELECT 
            *
        FROM 
            CallStream

    >[!NOTE]
    >Precis som med SQL, nyckelord är inte skiftlägeskänsliga och blanksteg är inte viktig.

    I den här frågan `CallStream` är hello alias som du angav när du skapade hello indata. Om du använde ett annat alias kan du använda det namnet i stället.

2. Klicka på **Test**.

    hello Stream Analytics-jobbet körs hello frågan mot hello exempeldata och visar hello utdata längst hello hello-fönstret. Du ser att hello händelsehubb och hello Streaming Analytics-jobbet är korrekt konfigurerade. (Som anges senare skapar du utdatamottagare som hello fråga kan skriva data till.)

    ![Stream Analytics-jobbet utdata visar 73 poster som genereras](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    hello exakta antalet poster som visas beror på hur många poster som har hämtats i ditt exempel 3 minuter.
 
### <a name="reduce-hello-number-of-fields-using-a-column-projection"></a>Minska hello antalet fält med en kolumn projektion

I många fall måste inte dina analyser alla hello kolumner från hello Indataströmmen. Du kan använda en fråga tooproject en mindre uppsättning returneras fält än i hello direktfråga.

1. Ändra hello frågan i hello kod editor toohello följande:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
        FROM 
            CallStream

2. Klicka på **Test** igen. 

    ![Stream Analytics-jobbet utdata för projektion som visar 25 poster som genereras](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Antal inkommande anrop per region: rullande fönster med aggregering

Anta att du vill toocount hello antal inkommande anrop per region. I strömmande data när du vill tooperform mängdfunktioner som inventering, behöver du toosegment hello dataström i temporal enheter (eftersom hello dataströmmen själva är effektivt oändliga). Du gör detta med hjälp av en Streaming Analytics [fönstret funktionen](stream-analytics-window-functions.md). Du kan arbeta med hello informationen i fönstret som en enhet.

Den här omvandlingen du vill ha en sekvens av temporala windows som inte överlappar varandra – varje fönster har en separat uppsättning data som du kan gruppera och aggregering. Den här typen av fönstret är refererad tooas en *rullande fönster* . Inom hello rullande fönster kan du räkna hello inkommande samtal grupperade efter `SwitchNum`, som representerar hello land där hello anropet har sitt ursprung. 

1. Ändra hello frågan i hello kod editor toohello följande:

        SELECT 
            System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
        FROM
            CallStream TIMESTAMP BY CallRecTime 
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Den här frågan använder hello `Timestamp By` nyckelord i hello `FROM` satsen toospecify vilket tidsstämpelfält i hello inkommande dataströmmen toouse toodefine hello rullande fönster. I det här fallet hello fönstret dividerar hello data i segment med hello `CallRecTime` i varje post. (Om inget fält har angetts hello fönsterhantering åtgärden använder hello länge varje händelse når hello händelsehubb. Se ”tid Vs programmet ankomsttid” i [strömma Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx). 

    hello projektion innehåller `System.Timestamp`, som returnerar en tidsstämpel för hello slutet av varje fönster. 

    toospecify som du vill toouse en rullande fönster, använda hello [TUMBLINGWINDOW](https://msdn.microsoft.com/library/dn835055.aspx) funktion i hello `GROUP BY `satsen. I hello-funktionen anger du en tidsenhet (mellan en mikrosekundnivå tooa dag) och en fönsterstorlek (hur många enheter). I det här exemplet består hello rullande fönster av 5 sekunder intervall, så du får ett antal per land för samtal som var femte sekund.

2. Klicka på **Test** igen. Observera att hello tidsstämplar under i hello resultat **WindowEnd** finns i steg 5 sekunder.

    ![Stream Analytics-jobbet utdata för aggregering, visar 13 poster som genereras](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Identifiera SIM bedrägeri med hjälp av en självkoppling

I det här exemplet vi kan överväga användning av falska toobe anrop som kommer från hello samma användare men på olika platser inom 5 sekunder från varandra. Exempelvis hello samma användare kan inte legitimt gör ett anrop från hello USA och Australien vid hello samtidigt. 

toocheck för dessa fall, du kan använda en självkoppling för hello strömmande data toojoin hello dataströmmen tooitself baserat på hello `CallRecTime` värde. Du kan sedan söka för anrop posterna för hello `CallingIMSI` värde (hello ursprung nummer) är hello detsamma, men hello `SwitchNum` värde (land för ursprung) är inte hello samma.

När du använder en koppling med strömmande data måste hello koppling ange vissa begränsningar på hur långt hello matchande rader kan delas upp i tid. (Som tidigare nämnts hello strömmande data är effektivt oändlig.) hello tid gränser för hello relationen har angetts i hello `ON` -satsen i hello koppling, med hello `DATEDIFF` funktion. I det här fallet baseras hello koppling på ett samtal data 5 sekunder tidsintervall.

1. Ändra hello frågan i hello kod editor toohello följande: 

        SELECT  System.Timestamp as Time, 
            CS1.CallingIMSI, 
            CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, 
            CS1.SwitchNum as Switch1, 
            CS2.SwitchNum as Switch2 
        FROM CallStream CS1 TIMESTAMP BY CallRecTime 
            JOIN CallStream CS2 TIMESTAMP BY CallRecTime 
            ON CS1.CallingIMSI = CS2.CallingIMSI 
            AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5 
        WHERE CS1.SwitchNum != CS2.SwitchNum

    Den här frågan är precis som alla SQL-koppling förutom hello `DATEDIFF` funktion i hello koppling. Detta är en version av `DATEDIFF` som är specifika tooStreaming Analytics och det måste visas i hello `ON...BETWEEN` satsen. hello parametrar är en tidsenhet (sekunder i det här exemplet) och hello-alias hello två källor för hello koppling. (Detta skiljer sig från hello standard SQL `DATEDIFF` funktionen.) 

    Hej `WHERE` -instruktionen innehåller hello villkor som flaggar hello bedrägliga anropet: hello ursprungliga växlar är inte hello samma. 

2. Klicka på **Test** igen. 

    ![Stream Analytics-jobbet utdata för självkoppling, visar 6 poster som genereras](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. Klicka på **Spara**. Detta sparar hello självkoppling frågan som en del av hello Streaming Analytics-jobbet. (Den inte spara hello exempeldata.)

    ![Spara Stream Analytics-jobbet](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png)

## <a name="create-an-output-sink-toostore-transformed-data"></a>Skapa en mottagare toostore omvandlas av utdata

Du har definierat en händelseström, event hub inkommande tooingest händelser och en fråga tooperform en omvandling över hello dataströmmen. hello sista steget är toodefine utdatamottagare för hello jobb – det vill säga en plats toowrite hello omvandlas dataström som. 

Du kan använda många resurser som utdata sänkor – SQL Server-databasen, tabellagring, Data Lake lagring, Power BI och en annan händelsehubb. Den här självstudiekursen skriver du hello dataströmmen tooAzure Blob Storage, vilket är ett vanliga alternativ för att samla in händelseinformation för senare analys, eftersom den innehåller Ostrukturerade data.

Om du har en befintlig blob storage-konto kan använda du som. Den här självstudiekursen lär du hur toocreate en ny lagringsplats kontot för den här kursen.

### <a name="create-an-azure-blob-storage-account"></a>Skapa ett Azure Blob Storage-konto

1. Returnera toohello Streaming Analytics-jobbet bladet i hello Azure-portalen. (Om du har stängt hello-bladet, söka efter `sa_frauddetection_job_demo` i hello **alla resurser** bladet.)
2. I hello **jobbet topologi** klickar du på hello **utdata** rutan. 
3. I hello **utdata** bladet, klickar du på  **+ &nbsp;Lägg till** och fyller sedan hello bladet med dessa värden:

    * **Ett utdataalias**: Använd hello namn `CallStream-FraudulentCalls`. 
    * **Sink**: Välj **Blob storage**.
    * **Importalternativ**: Välj **använda blob storage från aktuell prenumeration**.
    * **Lagringskontot**. Välj **Skapa nytt lagringskonto.**
    * **Lagringskontot** (andra rutan). Ange `YOURNAMEsademo`, där `YOURNAME` är ditt namn eller en annan unik sträng. hello namn kan använda bara gemena bokstäver och siffror, och den måste vara unikt i Azure. 
    * **Behållaren**. Ange `sa-fraudulentcalls-demo`.
    Hej lagringskontonamn och behållarnamn är används tillsammans tooprovide en URI för hello blob storage, så här: 

    `http://yournamesademo.blob.core.windows.net/sa-fraudulentcalls-demo/...`
    
    ![”Nya utdata” bladet för Stream Analytics-jobbet](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png)
    
4. Klicka på **Skapa**. 

    Azure skapar hello storage-konto och skapar automatiskt en nyckel. 

5. Stäng hello **utdata** bladet. 

## <a name="start-hello-streaming-analytics-job"></a>Starta hello Streaming Analytics-jobbet

hello-jobbet har nu konfigurerats. Du har angett indata (hello händelsehubb), en omvandling (hello frågan toolook för bedrägliga anrop) och utdata (blobblagring). Du kan nu starta hello jobb. 

1. Kontrollera att hello TelcoGenerator appen körs.

2. I hello jobb-bladet, klickar du på **starta**.

    ![Starta hello Stream Analytics-jobbet](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-output.png)

3. I hello **startjobb** bladet för utdata starttid, Välj **nu**. 

4. Klicka på **starta**. 

    ![”Starta jobbet” bladet för hello Stream Analytics-jobbet](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-job-blade.png)

    Azure meddelar dig när hello jobb har startat, och i hello jobb-bladet hello visas statusen **kör**.

    ![Stream Analytics-jobbstatusen, visar ”körs”](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-running-status.png)
    

## <a name="examine-hello-transformed-data"></a>Granska hello omvandlas data

Nu har du en fullständig Streaming Analytics-jobbet. hello jobbet undersöka en dataström med metadata för telefonsamtal, söker efter bedrägliga telefonsamtal i realtid och skriva information om dessa falska anrop toostorage. 

toocomplete den här självstudiekursen kommer du kanske vill toolook hello data som avbildas av hello Streaming Analytics-jobbet. hello data skrivs tooAzure blogg lagring i segment (filer). Du kan använda ett verktyg som läser Azure Blob Storage. Enligt beskrivningen i hello avsnittet förutsättningar, du kan använda Azure tillägg i Visual Studio och du kan använda ett verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/) eller [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

När du undersöker hello innehållet i en fil i blob storage finns ungefär hello följande:

![Azure blob storage med Streaming Analytics-utdata](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Rensa resurser

Vi har ytterligare artiklar som fortsätta med hello upptäckt av bedrägerier scenariot och som använder hello-resurser som du har skapat i den här kursen. Om du vill toocontinue finns hello förslag under **nästa steg** senare.

Men om du är klar och du behöver inte hello-resurser som du har skapat, du kan ta bort dem så att du inte debiteras onödiga Azure. I så fall föreslår vi att du hello följande:

1. Stoppa hello Streaming Analytics-jobbet. I hello **jobb** bladet, klickar du på **stoppa** hello överst.
2. Stoppa hello anger Generator app. Tryck på Ctrl + C i hello-kommandofönster som du startade hello app.
3. Ta bort om du har skapat en ny blob storage-konto för den här kursen. 
4. Ta bort hello Streaming Analytics-jobbet.
5. Ta bort hello händelsehubb.
6. Ta bort hello event hub namnområdet.

## <a name="get-support"></a>Få support

För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg

Den här kursen kan du fortsätta med hello följande artiklar:

* [Strömma analyser och Power BI: en analys i realtid instrumentpanel för strömmande data](stream-analytics-power-bi-dashboard.md). Den här artikeln visar hur toosend hello anger utdata från hello Stream Analytics-jobbet tooPower BI för visualisering i realtid och analys.
* [Hur toostore data från Azure Stream Analytics i en Azure Redis-Cache med hjälp av Azure Functions](stream-analytics-functions-redis.md). Den här artikeln visar hur toouse Azure Functions toowrite bedrägliga anropar tooan Azure Redis-cache via en Service Bus-kö.

Mer information om Stream Analytics i allmänhet försök dessa artiklar:

* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
