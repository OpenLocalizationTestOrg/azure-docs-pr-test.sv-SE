---
title: aaaBrowsing och hantera lagringsresurser med Server Explorer | Microsoft Docs
description: "Bläddra och hantera lagringsresurser med Server Explorer"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 658dc064-4a4e-414b-ae5a-a977a34c930d
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/24/2017
ms.author: kraigb
ms.openlocfilehash: f5b456b812f2ad8103c50538d532a57397bcccbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Bläddra och hantera lagringsresurser med Server Explorer
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Översikt
Om du har installerat hello Azure Tools för Microsoft Visual Studio, kan du visa blob, köer och tabelldata från dina lagringskonton för Azure. hello Azure Storage-noden i Server Explorer visar data i ditt konto för lokal lagring emulatorn och andra Azure storage-konton.

Välj tooview Server Explorer i Visual Studio hello menyraden **visa**, **Server Explorer**. hello lagringsnod visas alla hello storage-konton som finns under varje Azure-prenumeration /-certifikatet du är ansluten till. Om ditt lagringskonto inte visas kan du lägga till den genom att följa anvisningarna hello [senare i det här avsnittet](#add-storage-accounts-by-using-server-explorer).

Från och med Azure SDK 2.7, kan du också använda hello nya Cloud Explorer tooview och hantera Azure-resurser. Se [hantera Azure-resurser med Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md) för mer information.

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Visa och hantera lagringsresurser i Visual Studio
Server Explorer visar en lista över blobbar, köer och tabeller automatiskt i ditt lagringskonto i emulatorn. hello storage-emulatorn konto anges i Server Explorer under hello lagringsnod som hello **Development** nod.

toosee hello emulatorn lagringskontots resurser, expandera hello **Development** nod. Om hello storage-emulatorn inte har startats när du har expanderat Hej **Development** nod, startas den automatiskt. Det kan ta några sekunder. Du kan fortsätta toowork i andra delar av Visual Studio när hello storage-emulatorn startar.

tooview resurser i ett lagringskonto, expandera noden hello lagringskontots i Server Explorer. följande undernoder hello visas:

* Blobar
* Köer
* Tabeller

## <a name="work-with-blob-resources"></a>Arbeta med Blob-resurser
Hej Blobbar nod visar en lista av behållare för hello valda lagringskontot. BLOB-behållare innehåller blob-filer och du kan ordna dessa blobbar i mappar och undermappar. Se [hur toouse Blob Storage från .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md) för mer information.

### <a name="toocreate-a-blob-container"></a>toocreate en blob-behållare
1. Öppna hello snabbmenyn för hello **Blobbar** nod, och välj sedan **skapa Blob-behållaren**.
2. I hello **skapa Blob-behållaren** dialogrutan Ange hello namn av hello ny behållare.  
3. Tryck på **RETUR** på tangentbordet eller du kan klicka eller tryck på utanför hello namnet fältet toosave hello blob-behållare.
   
   > [!NOTE]
   > hello blob behållarens namn måste börja med en siffra (0-9) eller en gemen bokstav (a-ö).
   > 
   > 

### <a name="toodelete-a-blob-container"></a>toodelete en blob-behållare
* Öppna hello snabbmenyn för hello blob-behållaren tooremove och välj sedan **ta bort**.

### <a name="toodisplay-a-list-of-hello-items-contained-in-a-blob-container"></a>toodisplay över hello artiklar som ingår i en blob-behållare
* Öppna hello snabbmenyn för namn på en blob-behållare i hello listan och välj sedan **öppna**.
  
    När du visar hello innehållet i en blob-behållare, visas den i en flik kallas hello blob-behållaren vyn.
  
    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)
  
    Du kan utföra följande åtgärder på blobbar med knapparna hello i hello övre högra hörnet av hello blob-behållaren hello:
  
  * Ange ett filtervärde och tillämpa det
  * Uppdatera hello lista över blobbar i behållaren hello
  * Överför en fil
  * Ta bort en blob
    
    > [!NOTE]
    > Ta bort en fil från en blob-behållare inte ta bort hello underliggande fil. det endast tar bort den från hello blob-behållare.
    > 
    > 
  * Öppna en blob
  * Spara en blob toohello lokal dator

### <a name="toocreate-a-folder-or-subfolder-in-a-blob-container"></a>toocreate en mapp eller undermapp i en blobbbehållare
1. Välj hello blob-behållaren i Cloud Explorer. Välj hello i hello behållaren fönstret **överför Blob** knappen.
   
    ![Överför en fil till en blob-mapp](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)
2. I hello **överför nya fil** dialogrutan Välj hello **Bläddra** knappen toospecify hello fil tooupload, och ange ett mappnamn i hello **mapp (valfritt)** rutan .
   
    Du kan lägga till undermappar i behållaren mappar genom att följa hello samma procedur. Om du inte anger ett namn på mappen hello filen kommer att överföras toohello översta nivå hello blob-behållare. hello-filen visas i hello angivna mappen i hello behållaren.
   
    ![Lägga till mappen tooa blob-behållare](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)
3. Dubbelklicka på hello mapp eller tryck på RETUR toosee hello innehållet i hello mapp. När du är i mappen hello behållare du kan gå tillbaka toohello rot hello behållare genom att välja hello **öppna överordnade katalogen** (UPPIL) knappen.

### <a name="toodelete-a-container-folder"></a>toodelete en behållare mapp
* Ta bort alla hello filer i mappen hello
  
  > [!NOTE]
  > Eftersom mappar i blob-behållare är virtuella mappar, du kan inte skapa en tom mapp eller kan du ta bort en mapp toodelete filen innehållet. Du har toodelete hello hela innehållet i en toodelete hello-mappen.
  > 
  > 

### <a name="toofilter-blobs-in-a-container"></a>toofilter blobbar i en behållare
Du kan filtrera hello blob som visas genom att ange ett gemensamt prefix.

Till exempel om du anger prefixet för hello `hello` i hello filterrutan och välj sedan hello **kör** (**!**) knappen, visas för blobbar som börjar med ”hello”.

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

> [!NOTE]
> hello filterfältet är skiftlägeskänsligt och stöder inte filtrering med jokertecken. Blobbar kan endast filtreras efter prefix. hello prefix kan innehålla avgränsare för en om du använder en avgränsare tooorganize blobbar i en virtuell hierarki. Till exempel filtrering på hello prefixet HelloFabric / returnerar alla BLOB som börjar med strängen.
> 
> 

### <a name="toodownload-blob-data"></a>toodownload blob-data
* I **Cloud Explorer**, öppna hello snabbmenyn för en eller flera blobbar och välj sedan **öppna**, eller välj hello blob namn och väljer sedan hello **öppna** eller dubbelklicka hello blob-namnet.
  
    Hej filhämtningsförloppet blob visas i hello **Azure-aktivitetsloggen** fönster.
  
    hello blob öppnas i hello standard Redigeraren för filtypen. Om hello operativsystemet kan identifiera hello filtyp, öppnas hello filen i ett lokalt installerade program. i annat fall uppmanas du toochoose ett program som är lämplig för hello filtyp hello-blob. hello lokal fil som skapas när du hämtar en blob har markerats som skrivskyddad.
  
    BLOB-data cachelagras lokalt och kontrolleras mot hello blob senast ändrad i hello Blob-tjänsten. Om hello-blob har uppdaterats sedan den senast hämtades, kommer den att hämtas igen. Annars läses hello blob från hello lokal disk. Som standard är en blob hämtade tooa tillfällig katalog. toodownload blobbar tooa särskild katalog, öppna hello snabbmenyn för hello valt blob-namn och välj **Spara som**. När du sparar en blobb i det här sättet hello blob-fil har inte öppnats och hello lokal fil skapas med läsa / skriva-attribut.

### <a name="tooupload-blobs"></a>tooupload blobbar
* Välj hello **överför Blob** knappen när hello behållare är öppen för visning i vyn för hello blob-behållaren.
  
    Du kan välja ett eller flera filer tooupload och du kan ladda upp filer av valfri typ. Hej **Azure-aktivitetsloggen** visar hello förloppet för hello överföringen. Mer information om hur toowork med blob-data finns [hur toouse hello Azure Blob Storage-tjänsten i .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="tooview-logs-transferred-tooblobs"></a>tooview loggar överförs tooblobs
* Om du använder Azure-diagnostik toolog data från ditt Azure-program och du har överfört loggar tooyour storage-konto, visas behållare som har skapats av Azure för de här loggarna. Visa dessa loggar i Server Explorer är ett enkelt sätt tooidentify problem med programmet, särskilt om den har distribuerade tooAzure. Läs mer om Azure-diagnostik [samla in Data för loggning med hjälp av Azure-diagnostik](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="tooget-hello-url-for-a-blob"></a>tooget hello URL för en blob
* Öppna hello blob snabbmenyn och välj sedan **Kopiera URL: en**.

### <a name="tooedit-a-blob"></a>tooedit en blob
* Markera hello blob och välj sedan hello **öppna Blob** knappen.
  
    hello-filen är hämtade tooa tillfällig plats och öppnas på hello lokala datorn. Du måste överföra hello blob igen när du har gjort ändringar.

## <a name="work-with-queue-resources"></a>Arbeta med resurser för kön
Storage services köer finns i ett Azure storage-konto och du kan använda dem tooallow din cloud service roller toocommunicate med varandra och med andra tjänster av en mekanism för meddelandehantering. Du kan komma åt hello kön programmässigt via en molnbaserad tjänst och en webbtjänst för externa klienter. Du kan också komma åt hello kön direkt med hjälp av Server Explorer i Visual Studio.

När du utvecklar en molnbaserad tjänst som använder köer kan du vill toouse Visual Studio toocreate köer och arbeta med dem. interaktivt medan du utveckla och testa din kod.

I Server Explorer kan du visa hello köer i ett lagringskonto, skapa och ta bort köer, öppna en kö tooview meddelanden och lägga till meddelanden tooa kön. När du öppnar en kö för att visa kan du visa enskilda hälsningsmeddelande och du kan utföra följande åtgärder för hello kön med knapparna hello i hello övre vänstra hörnet hello:

* Uppdatera vyn hello hello köns
* Lägg till en meddelandekö toohello
* Status Created översta hello-meddelande
* Rensa hello hela kön

hello följande bild visar en kö som innehåller två meddelanden.

![Visa en kö](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

För mer information om storage services köer, se [så här: använda hello Queue Storage Service](http://go.microsoft.com/fwlink/?LinkID=264702). Information om hello webbtjänst för lagringstjänster köer, finns [kötjänst-koncept](http://go.microsoft.com/fwlink/?LinkId=264788). Information om hur toosend meddelanden tooa lagringstjänster kön med hjälp av Visual Studio finns [skickar meddelanden tooa Storage Services kön](https://msdn.microsoft.com/library/azure/jj649344.aspx).

> [!NOTE]
> Tjänster lagringsköer skiljer sig från service bus-köer. Mer information om service bus-köer finns i Service Bus-köer, ämnen och prenumerationer.
> 
> 

## <a name="work-with-table-resources"></a>Arbeta med resurser för tabellen
hello Azure Table storage-tjänsten lagrar stora mängder strukturerade data. hello-tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför hello Azure-molnet. Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.

### <a name="toocreate-a-table"></a>toocreate en tabell
1. Välj hello i Cloud Explorer **tabeller** nod hello lagring kontot och välj sedan **Create Table**.
2. I hello **Create Table** dialogrutan Ange ett namn för hello tabellen.

### <a name="tooview-table-data"></a>tooview tabelldata
1. I Cloud Explorer öppnar du hello **Azure** noden och sedan öppna hello **lagring** nod.
2. Öppna hello konto lagringsnod att du är intresserad av och sedan öppna hello **tabeller** nod toosee en lista över tabeller för hello storage-konto.
3. Öppna hello snabbmenyn för en tabell och välj sedan **visa tabellen**.
   
    ![En Azure-tabellen i Solution Explorer](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

hello tabell är ordnad efter enheter (visas i rader) och egenskaper (visas i kolumner). Hello följande bild visar exempelvis enheter som anges i hello **tabelldesignern**:

### <a name="tooedit-table-data"></a>tooedit tabelldata
1. I hello **tabelldesignern**, öppna hello snabbmenyn för en entitet (en enda rad) eller en egenskap (en cell) och välj sedan **redigera**.
   
    ![Lägg till eller redigera en Tabellentiteten](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)
   
    Entiteter i en tabell är inte obligatoriska toohave hello samma uppsättning egenskaper (kolumner). Kom ihåg hello följande begränsningar för att visa och redigera tabelldata.
   
   * Du kan inte visa eller Redigera binärdata (typen byte[]), men du kan lagra det i en tabell.
   * Du kan inte redigera hello **PartitionKey** eller **RowKey** värden, eftersom tabellen lagring i Azure inte stöder åtgärden.
   * Du kan inte skapa en egenskap som kallas tidsstämpel använder Azure Storage-tjänster en egenskap med det namnet.
   * Om du anger ett DateTime-värde, måste du följa ett format som är lämpliga toohello nationella inställningar och språkinställningar i datorn (t.ex, åååå-MM-DD: mm: ss [AM | PM] för USA Engelska).

### <a name="tooadd-entities"></a>tooadd entiteter
1. I hello **tabelldesignern**, Välj hello **lägga till entiteten** knappen.
   
    ![Lägga till entitet](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)
2. I hello **lägga till entiteten** dialogrutan Ange hello värdena för hello **PartitionKey** och **RowKey** egenskaper.
   
    ![Entiteten dialogrutan Lägg till](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)
   
    Ange värden för hello noggrant eftersom du inte kan ändra dem när du har stängt dialogrutan hello såvida du inte ta bort hello entitet och lägga till den igen.

### <a name="toofilter-entities"></a>toofilter entiteter
Du kan anpassa hello uppsättning entiteter som visas i en tabell om du använder hello Frågebyggaren.

1. tooopen hello frågebyggaren, öppna en tabell för visning.
2. Välj hello frågebyggaren hello tabellvy i verktygsfältet.
   
    Hej **frågebyggaren** dialogrutan visas. hello följande bild visar en fråga som skapas i hello Frågebyggaren.
   
    ![Frågebyggaren](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
3. När du är klar skapar hello frågan, Stäng hello dialogrutan. hello resulterande textformat av hello fråga visas som ett filter för WCF Data Services i en textruta.
4. toorun hello fråga, Välj hello grön triangel ikon.
   
    Du kan också filtrera entitetsdata som visas i hello **tabelldesignern** om du anger en WCF-datatjänster Filtersträng direkt i hello filterfältet. Den här typen av strängen är liknande tooa SQL WHERE-sats men toohello server skickas som en HTTP-begäran. Information om hur tooconstruct filtrera strängar finns [filtret konstruerar strängar för hello tabelldesignern](https://msdn.microsoft.com/library/azure/ff683669.aspx).
   
    hello visar följande bild ett exempel på ett giltigt filter-sträng:
   
    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Uppdatera storage-data
När Server Explorer ansluter tooor hämtar data från ett lagringskonto, kan det ta upp tooa minut för hello åtgärden toocomplete. Om den inte kan ansluta kan hello åtgärden timeout. Du kan fortsätta toowork i andra delar av Visual Studio medan data hämtas. toocancel hello igen om det tar för lång, Välj hello **Stoppa uppdatering** i verktygsfältet hello Server Explorer.

#### <a name="toorefresh-blob-container-data"></a>toorefresh blob-behållardata
* Välj hello **Blobbar** nod under **lagring** och välj hello **uppdatera** i verktygsfältet hello Server Explorer.
* toorefresh hello lista över blob som visas, väljer hello **Execute** knappen.

#### <a name="toorefresh-table-data"></a>toorefresh tabelldata
* Välj hello **tabeller** nod under **lagring** och välj hello **uppdatera** knappen.
* toorefresh hello lista över enheter som visas i hello **tabelldesignern**, Välj hello **kör** hello-knappen **tabelldesignern**.

#### <a name="toorefresh-queue-data"></a>toorefresh kön data
* Välj hello **köer** nod, och välj sedan hello **uppdatera** knappen.

#### <a name="toorefresh-all-items-in-a-storage-account"></a>toorefresh alla objekt i ett lagringskonto
* Välj hello kontonamn och välj sedan hello **uppdatera** i hello verktygsfältet för Server Explorer.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Lägg till storage-konton med hjälp av Server Explorer
Det finns två sätt tooadd storage-konton med hjälp av Server Explorer. Du kan skapa ett nytt lagringskonto i din Azure-prenumeration eller du kan koppla ett befintligt lagringskonto.

#### <a name="toocreate-a-new-storage-account-by-using-server-explorer"></a>toocreate ett nytt lagringskonto med hjälp av Server Explorer
1. Öppna hello snabbmenyn för hello lagringsnod i Server Explorer och välj sedan skapa Storage-konto.
   
    ![Skapa ett nytt Azure storage-konto](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)
2. Välj eller ange följande information för hello nytt lagringskonto i hello hello **skapa Lagringskonto** dialogrutan.
   
   * hello Azure-prenumeration toowhich som du vill tooadd hello storage-konto.
   * hello namn toouse för hello nytt lagringskonto.
   * hello region eller affinitetsgrupp (till exempel västra USA eller Östasien).
   * Hej typ av replikering du vill använda toouse för hello storage-konto, till exempel Geo-Redundant.
3. Välj **Skapa**.
   
    hello nytt lagringskonto visas i hello **lagring** lista i Solution Explorer.

#### <a name="tooattach-an-existing-storage-account-by-using-server-explorer"></a>tooattach ett befintligt lagringskonto med hjälp av Server Explorer
1. I Server Explorer öppnar hello snabbmenyn för hello Azure storage-nod och välj sedan **Anslut extern lagring**.
   
    ![Lägga till ett befintligt lagringskonto](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
2. Välj eller ange följande information för hello nytt lagringskonto i hello hello **skapa Lagringskonto** dialogrutan.
   
   * hello namnet på hello befintliga storage-konto du vill tooattach. Du kan ange ett namn eller välj hello-listan.
   * hello nyckel för hello valt storage-konto. Det här värdet anges vanligtvis för dig när du väljer ett lagringskonto. Om du vill ha Visual Studio tooremember hello lagringskontonyckel Markera hello Kom ihåg konto nyckel.
   * hello protokollet toouse tooconnect toohello lagringskonto, till exempel HTTP, HTTPS eller en anpassad slutpunkt. Se [hur tooConfigure anslutning strängar](https://msdn.microsoft.com/library/azure/ee758697.aspx) mer information om anpassade slutpunkter.

### <a name="tooview-hello-secondary-endpoints"></a>tooview hello sekundära slutpunkter
* Om du har skapat ett lagringskonto med hjälp av hello **läsbehörighet Geo-Redundant** replikeringsalternativet, kan du visa dess sekundär slutpunkter. Öppna hello snabbmenyn för hello kontonamn och välj sedan **egenskaper**.
  
    ![Sekundär lagring-slutpunkter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="tooremove-a-storage-account-from-server-explorer"></a>tooremove ett lagringskonto från Server Explorer
* I Server Explorer öppnar hello snabbmenyn för hello kontonamn och välj sedan **ta bort**. Om du tar bort ett lagringskonto tas alla sparade viktig information för det kontot också bort.
  
  > [!NOTE]
  > Om du tar bort ett lagringskonto från Server Explorer påverkar det inte ditt lagringskonto eller data som den innehåller; den helt enkelt bort hello referens från Server Explorer. toopermanently ta bort ett lagringskonto använder hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).
  > 
  > 

## <a name="next-steps"></a>Nästa steg
toolearn mer om hur använder Azure-lagringstjänster, se [åtkomst till hello Azure-lagringstjänster](https://msdn.microsoft.com/library/azure/ee405490.aspx).

