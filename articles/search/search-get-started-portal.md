---
Rubrik: aaa ”Självstudier: skapa ditt första Azure Search-index i hello-portalen | Microsoft Docs ”beskrivning: I hello Azure-portalen, använda fördefinierade exempel data toogenerate ett index. Utforska fulltextsökning, filter, fasetter, fuzzy-sökning, geosearch och mycket annat.
tjänster: Sök dokumentationcenter: '' författare: HeidiSteen manager: jhubbard editor: '' taggar: azure-portalen

MS.AssetID: 21adc351-69bb-4a39-bc59-598c60c8f958 ms.service: Sök ms.devlang: na ms.workload: Sök ms.topic: hjälteartikel ms.tgt_pltfrm: na ms.date: 06/26/2017 ms.author: heidist

---
# <a name="tutorial-create-your-first-azure-search-index-in-hello-portal"></a>Självstudier: Skapa ditt första Azure Search-index i hello-portalen

I hello Azure-portalen, börja med en fördefinierad exempel dataset tooquickly genererar ett index med hello **dataimport** guiden. Utforska fulltextsökning, filter, fasetter, fuzzy-sökning och geosearch med **Sökutforskaren**.  

Det här är en introduktion helt utan kodning, så att du kan komma igång med att skriva intressanta frågor direkt utifrån fördefinierade data. Portalverktygen kan aldrig bli en fullgod ersättning för kodning, men de är mycket användbara till följande uppgifter:

+ Praktisk utbildning med minimal startsträcka
+ Skapa ett prototypindex innan du skriver kod i **Importera data**
+ Testa frågor och parsa syntax i **Sökutforskaren**
+ Visa ett befintligt index publicerade tooyour service och leta upp dess attribut

**Tidsuppskattning:** Ungefär 15 minuter, eventuellt längre om det krävs registrering till kontot eller tjänsten. 

Du kan också snabbt igång med hjälp av en [kodbaserad introduktion tooprogramming Azure Search i .NET](search-howto-dotnet-sdk.md).

## <a name="prerequisites"></a>Krav

Självstudiekursen förutsätter en [Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) och tillgång till [Azure Search-tjänsten](search-create-service-portal.md). 

Om du inte vill tooprovision en tjänst direkt, kan du titta på en 6 minuter demonstration av hello stegen i den här kursen börjar vid cirka tre minuter i denna [Azure Search-översikten](https://channel9.msdn.com/Events/Connect/2016/138).

## <a name="find-your-service"></a>Hitta din tjänst
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Öppna hello service instrumentpanelen i Azure Search-tjänsten. Om du inte fäster hello service panelen tooyour instrumentpanelen, kan du hitta din tjänst det här sättet: 
   
   * I hello Jumpbar klickar du på **fler tjänster** längst hello hello vänstra navigeringsfönstret.
   * Skriv i sökrutan hello *Sök* tooget en lista med search-tjänster för din prenumeration. Tjänsten ska visas i hello-listan. 

## <a name="check-for-space"></a>Kontrollera utrymmet
Många kunder börja med hello kostnadsfria tjänsten. Den här versionen är begränsad toothree index, tre datakällor och tre indexerare. Kontrollera att du har plats för extra objekt innan du börjar. Med den här guiden kan du skapa ett objekt av varje sort. 

> [!TIP] 
> Paneler på hello service instrumentpanelen visar hur många index, indexerare och datakällor du redan har. hello innehåller indexeraren indikatorer för lyckade och misslyckade. Klicka på hello panelen tooview hello indexeraren räkna. 
>
> ![Paneler för indexerare och datakällor][1]
>

## <a name="create-index"></a> Skapa ett index och läsa in data
Sökfrågor iterera över ett *index* med sökbara data, metadata och konstruktioner som används för att optimera vissa sökbeteenden.

tookeep aktiviteten portal-baserade vi använder en inbyggd exempeldatamängd crawlas med hjälp av en indexerare via hello **dataimport** guiden. 

#### <a name="step-1-start-hello-import-data-wizard"></a>Steg 1: Starta guiden för Import av hello
1. Klicka på instrumentpanelen för Azure Search-tjänsten **dataimport** i hello kommandot fältet toostart en guide som skapar och fyller i ett index.
   
    ![Kommandot Importera data][2]

2. Klicka på hello guiden **datakällan** > **exempel** > **realestate-oss-sample**. Den här datakällan är förkonfigurerad med namn, typ och anslutningsinformation. När du har skapat den blir den en ”befintlig datakälla” som kan återanvändas i andra importåtgärder.

    ![Välj exempeldatauppsättning][9]

3. Klicka på **OK** toouse den.

#### <a name="step-2-define-hello-index"></a>Steg 2: Definiera hello index
Skapa ett index är vanligtvis manuella och kodbaserad men hello guiden kan generera ett index för en datakälla som den kan crawlas. Åtminstone ett index kräver ett namn och en samling fält, med ett fält har markerats som hello dokumentet viktiga toouniquely identifierar varje dokument.

Fälten har datatyper och attribut. hello kryssrutorna överst hello är *index attribut* styr hur hello fältet används. 

* **Hämtningsbar** innebär att det visas i listor med sökresultat. Du kan markera enskilda fält som ska utelämnas från sökresultat genom att avmarkera den här kryssrutan, till exempel om fält endast används i filteruttryck. 
* **Filtrerbar**, **Sorterbar** och **Fasettbar** avgör om ett fält kan användas i ett filter, en sortering eller en struktur för aspektbaserad navigering. 
* **Sökbar** innebär att ett fält ingår i fulltextsökning. Strängarna är sökbara. Numeriska fält och fält för booleska värden är ofta markerade som icke sökbara. 

Som standard söker hello guiden hello datakälla för unika identifierare som hello grund för hello nyckelfält. Strängarna får attributen hämtningsbara och sökbara. Heltal får attributen hämtningsbara, filtrerbara, sorterbara och fasettbara.

  ![Genererade realestate-index][3]

Klicka på **OK** toocreate hello index.

#### <a name="step-3-define-hello-indexer"></a>Steg 3: Definiera hello indexeraren
Fortfarande i hello **dataimport** guiden, klickar du på **indexeraren** > **namn**, och Skriv ett namn för hello indexeraren. 

Det här objektet definierar en körbar process. Du kan placera den på återkommande schema, men nu använda hello standard alternativet toorun hello indexer när omedelbart, när du klickar på **OK**.  

  ![realestate indexer][8]

## <a name="check-progress"></a>Kontrollera förloppet
toomonitor data Importera, gå tillbaka toohello service instrumentpanelen, bläddra nedåt och dubbelklicka på hello **indexerare** panelen tooopen hello indexerare lista. Du bör se hello nyskapad indexerare i hello listan med status som visar ”pågående” eller lyckades, tillsammans med hello antal dokument som indexeras.

   ![Meddelande om pågående indexeringsaktiviteter][4]

## <a name="query-index"></a>Frågan hello index
Nu har du en sökindex som är redo tooquery. **Sök explorer** är ett Frågeverktyg som är inbyggda i hello-portalen. Det innehåller en sökruta så att du kan kontrollera att en sökning returnerar det du förväntar dig. 

> [!TIP]
> I hello [Azure Search-översikten](https://channel9.msdn.com/Events/Connect/2016/138), hello följande visas på 6m08s till hello video.
>

1. Klicka på **Sök explorer** i hello kommandofält.

   ![Kommandot Sökutforskaren][5]

2. Klicka på **ändra index** på hello kommandot liggande tooswitch för*realestate-oss-sample*.

   ![Index- och API-kommandon][6]

3. Klicka på **ange API-versionen** på hello kommandot fältet toosee som REST API: er är tillgängliga. Förhandsgranska API: er ger dig tillgång toonew funktioner som ännu inte har normalt släpps. Använda hello allmänt tillgänglig version (2016-09-01) för hello frågor nedan, om detta inte anges. 

    > [!NOTE]
    > [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents) och hello [.NET-bibliotek](search-howto-dotnet-sdk.md#core-scenarios) är fullständigt likvärdiga, men **Sök explorer** är bara utrustade toohandle REST-anrop. Den godkänner syntax för både [enkel frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) och [fullständig Lucene frågeparsern](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), plus alla hello sökparametrarna som är tillgängliga i [Sök efter dokument](https://docs.microsoft.com/rest/api/searchservice/search-documents) åtgärder.
    > 

4. Ange hello sökfältet hello frågesträngar nedan och klicka på **Sök**.

  ![Exempel på sökfråga][7]

**`search=seattle`**

+ Hej `search` parametern är i det här fallet används tooinput nyckelordssökning för textsökning, returnerar listor i bestämmer region, Washington, som innehåller *Seattle* i alla sökbara fält i hello dokumentet. 

+ **Sök explorer** returnerar resultat i JSON, vilket är utförlig och svåra tooread om dokument har en kompakta struktur. Beroende på dina dokument, kanske du behöver toowrite kod som hanterar söka resultat tooextract viktiga element. 

+ Dokument som består av alla fält som har markerats som hämtas i hello index. tooview indexattribut i hello-portalen klickar du på *realestate-oss-sample* i hello **index** panelen.

**`search=seattle&$count=true&$top=100`**

+ Hej `&` symbolen är används tooappend sökparametrarna som kan anges i valfri ordning. 

+  Hej `$count=true` parametern returnerar en uppräkning för hello summan av alla dokument som returneras. Du kan verifiera filterfrågor genom att övervaka ändringar som rapporterats via `$count=true`. 

+ Hej `$top=100` returnerar hello högsta rangordnas 100 dokument utanför hello totalt. Som standard returnerar Azure Search hello första 50 bästa matchar. Du kan öka eller minska hello via `$top`.

**`search=*&facet=city&$top=2`**

+ `search=*` är en tom sökning. Tomma sökningar söker efter allt. En orsak till att skicka en tom fråga för filtrera eller aspekten över hello fullständig uppsättning av dokument. Till exempel vill du faceting navigering strukturen tooconsist av alla städer i hello index.

+  `facet`Returnerar en navigering struktur att du skickar tooa UI-kontroll. Den returnerar kategorier och antal. I det här fallet baseras kategorier på hello antalet städer. Det finns ingen aggregering i Azure Search, men du kan uppskatta aggregering via `facet`, som ger en uppräkning av dokument i varje kategori.

+ `$top=2`tillbaka ger två dokument, illustrerar som du kan använda `top` tooboth minska eller öka resultat.

**`search=seattle&facet=beds`**

+ De här frågan är en aspekt för sängar, i en textsökning för *Seattle*. `"beds"`kan anges som en begränsningsaspekt eftersom hello fält har markerats som strängfält, filtrera och facetable i hello index och hello värden den innehåller (numeriska 1 till 5), är lämpliga för att kategorisera listor i grupper (listor med 3 sovrum, 4 sovrum). 

+ Endast filtrerbara fält kan fasetteras. Endast strängfält fält kan returneras i hello resultat.

**`search=seattle&$filter=beds gt 3`**

+ Hej `filter` returnerar resultat som matchade hello kriterier som du angett för parametern. I det här fallet sovrum som är större än 3. 

+ Syntaxen för filtret är en OData-konstruktion. Mer information finns i [OData-filtersyntax](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

**`search=granite countertops&highlight=description`**

+ Träffar syntaxmarkering refererar tooformatting på text matchar hello nyckelordet angivna matchar finns i ett visst fält. Sökordet är djupt begravd i en beskrivning, kan du lägga till träffar färgmarkera toomake den enklare toospot. I det här fallet hello formaterad frasen `"granite countertops"` är enklare toosee i beskrivningsfält hello.

**`search=mice&highlight=description`**

+ Fulltextsökning hittar ordformer med liknande semantisk struktur. I det här fallet innehåller sökresultat markerade text för ”mus” för hem med musen angrepp i svaret tooa nyckelordssökning på ”möss”. Olika former av Hej samma ord som kan visas i resultaten på grund av språkliga analys. 

+ Azure Search har stöd för 56 analysverktyg från både Lucene och Microsoft. hello-standard som används av Azure Search är hello standard Lucene analyzer. 

**`search=samamish`**

+ Felstavade ord, som till exempel 'samamish' för hello Samammish Platån i hello Seattle område, misslyckas tooreturn matchningar i vanlig sökning. Du kan använda fuzzy sökning, beskrivs i nästa exempel för hello toohandle felstavade.

**`search=samamish~&queryType=full`**

+ Fuzzy sökning är aktiverad när du anger hello `~` symbol och använda hello fullständig frågeparsern, vilket tolkar och korrekt tolkar hello `~` syntax. 

+ Fuzzy sökning är tillgänglig när du välja hello fullständig frågeparsern, vilket inträffar när du ställer in `queryType=full`. Läs mer om frågan scenarier hello fullständig frågeparsern [Lucene frågesyntaxen i Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search).

+ När `queryType` är inget hello standard enkla frågeparsern används. hello enkla frågeparsern är snabbare, men om du behöver fuzzy Sök reguljära uttryck, närhet Sök eller andra typer av avancerad fråga måste hello fullständig syntax. 

**`search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`**

+ Geospatiala sökning stöds via hello [edm. GeographyPoint datatyp](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) på ett fält som innehåller koordinater. Geosearch är en filtertyp som finns med i [OData-filtersyntaxen](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

+ hello exempelfråga filtrerar alla resultat för namngivna data om resultaten är mindre än 5 kilometer, från en viss tidpunkt (anges som latitud och longitud koordinater). Genom att lägga till `$count`, du kan se hur många resultat returneras när du ändrar hello avstånd eller hello koordinater. 

+ Geospatial sökning kan vara användbart om sökprogrammet har en funktion av typen ”hitta en bensinstation i närheten av där jag befinner mig” eller om programmet har en funktion för kartnavigering. Det är dock inte fråga om någon fulltextsökning. Lägg till fält som innehåller namnen på stad eller ett land i tillägg toocoordinates om du har användarkrav för att söka på en stad eller ett land efter namn.

## <a name="next-steps"></a>Nästa steg

+ Ändra hello-objekt som du nyss skapade. När du har kört hello guiden en gång, kan du gå tillbaka och visa eller ändra enskilda komponenter: index, indexeraren eller datakälla. Vissa ändringar, till exempel hello ändrar datatyp för hello fält är inte tillåtna i hello index, men de flesta egenskaper och inställningar är ändringsbar.

  tooview enskilda komponenter, klicka på hello **Index**, **indexeraren**, eller **datakällor** panelerna på din instrumentpanel toodisplay en lista över befintliga objekt. toolearn mer om indexet ändringar som inte kräver en återskapning finns [uppdatera Index (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/update-index).

+ Försök hello verktyg och steg med andra datakällor. Hej exempeluppsättningen `realestate-us-sample`, från en Azure SQL Database som Azure Search kan crawlas. Utöver Azure SQL Database kan Azure Search crawla och härleda ett index från fasta datastrukturer i Azure Table Storage, Blob Storage, SQL Server på en virtuell Azure-dator och Azure Cosmos DB. Alla dessa datakällor stöds i hello guiden. I koden kan du på ett enkelt sätt fylla i ett index med en *indexerare*.

+ Alla andra icke indexeraren datakällor stöds via en push-modell, där koden skickar nya och ändrade raduppsättningar i JSON tooyour index. Mer information finns i [Lägga till, uppdatera eller ta bort dokument i Azure Search](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

Mer information om andra funktioner som omnämns i den här artikeln finns på dessa länkar:

* [Översikt över indexerare](search-indexer-overview.md)
* [Skapa Index (inklusive en detaljerad förklaring av hello indexattribut)](https://docs.microsoft.com/rest/api/searchservice/create-index)
* [Sökutforskaren](search-explorer.md)
* [Söka efter dokument (innehåller exempel på frågesyntax)](https://docs.microsoft.com/rest/api/searchservice/search-documents)


<!--Image references-->
[1]: ./media/search-get-started-portal/tiles-indexers-datasources2.png
[2]: ./media/search-get-started-portal/import-data-cmd2.png
[3]: ./media/search-get-started-portal/realestateindex2.png
[4]: ./media/search-get-started-portal/indexers-inprogress2.png
[5]: ./media/search-get-started-portal/search-explorer-cmd2.png
[6]: ./media/search-get-started-portal/search-explorer-changeindex-se2.png
[7]: ./media/search-get-started-portal/search-explorer-query2.png
[8]: ./media/search-get-started-portal/realestate-indexer2.png
[9]: ./media/search-get-started-portal/import-datasource-sample2.png