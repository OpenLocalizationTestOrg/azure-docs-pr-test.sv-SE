---
title: "aaaPower BI självstudiekurs för Azure Cosmos DB connector | Microsoft Docs"
description: "Använd den här självstudiekursen Power BI-tooimport JSON, skapa insiktsfulla rapporter och visualisera data med hjälp av hello Azure Cosmos DB och Power BI-anslutningen."
keywords: Power bi kursen, visualisera data, power bi-anslutningsprogrammet
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: cd1b7f70-ef99-40b7-ab1c-f5f3e97641f7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: mimig
ms.openlocfilehash: ca0bb8b76db8ef2ec936722b682af6a9488a3501
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="power-bi-tutorial-for-azure-cosmos-db-visualize-data-using-hello-power-bi-connector"></a>Power BI-självstudiekurs för Azure Cosmos DB: visualisera data med Power BI-anslutningsprogrammet för hello
[PowerBI.com](https://powerbi.microsoft.com/) är en onlinetjänst där du kan skapa och dela instrumentpaneler och rapporter med data som är viktiga tooyou och din organisation.  Power BI Desktop är ett dedikerat rapportera redigeringsverktyg som gör att du tooretrieve data från olika datakällor, slå samman och transformera hello data, skapa kraftfulla rapporter och visualiseringar och publicera hello rapporter tooPower BI.  Du kan nu ansluta tooyour Cosmos DB kontot via hello Cosmos-DB-anslutningen för Power BI med hello senaste versionen av Power BI Desktop.   

I självstudierna Power BI vi gå igenom hello steg tooconnect tooa Cosmos-DB-konto i Power BI Desktop, navigera tooa samling där vi vill tooextract hello data med hjälp av hello Navigator, transformera JSON-data i tabellformat med Power BI Desktop Query Redigeraren, och skapa och publicera en rapport tooPowerBI.com.

Den här kursen Power BI, att du kunna tooanswer hello följande frågor:  

* Hur kan jag skapa rapporter med data från Cosmos-databasen med hjälp av Power BI Desktop?
* Hur kan jag ansluta tooa Cosmos-DB-konto i Power BI Desktop?
* Hur kan jag hämta data från en samling i Power BI Desktop
* Hur kan jag omvandla kapslade JSON-data i Power BI Desktop?
* Hur kan jag för att publicera och dela Mina rapporter på PowerBI.com?

> [!NOTE]
> hello Power BI-anslutningsprogrammet för Azure Cosmos DB ansluter tooPower BI Desktop för extrahering och transformering av data. Rapporter som skapas i Power BI Desktop kan sedan vara publicerade tooPowerBI.com. Direkt extrahering och omvandling av Azure Cosmos DB data kan inte utföras på PowerBI.com. 

## <a name="prerequisites"></a>Krav
Innan du följer instruktionerna hello i självstudierna Power BI, kontrollera att åtkomst toohello följande resurser:

* [hello senaste versionen av Power BI Desktop](https://powerbi.microsoft.com/desktop).
* Åtkomstkonto tooour demo eller data i ditt Cosmos-DB-konto.
  * hello demo-konto fylls med hello vulkanen data som visas i den här självstudiekursen. Det här demo-kontot inte är bunden av alla serviceavtal och är avsedd endast i demonstrationssyfte.  Vi reservera hello rätt toomake ändringar toothis demo konto inklusive men inte begränsat till, avslutar hello konto, hello nyckel, begränsa åtkomst, ändra, ändra och ta bort hello data, när som helst utan varsel eller orsak.
    * URL: https://analytics.documents.azure.com
    * Skrivskyddad sekundärnyckel: MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR + YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw ==
  * Eller toocreate ditt eget konto finns [skapa ett Azure Cosmos DB konto hello Azure-portalen](https://azure.microsoft.com/documentation/articles/create-account/). Sedan tooget vulkanen exempeldata som är liknande toowhat används i den här kursen (men inte innehåller hello GeoJSON block), se hello [NOAA plats](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5) och sedan importera hello data med hjälp av hello [Azure Cosmos DB datamigrering verktyget](import-data.md).

tooshare dina rapporter på PowerBI.com, måste du ha ett konto på PowerBI.com.  toolearn mer om Power BI för kostnadsfria och Power BI Pro, besök [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Nu börjar vi
I den här kursen ska vi anta att du en geologist studerar snö hello världen.  Hej vulkanen data lagras i ett Cosmos-DB-konto och hello JSON-dokument som ser ut som hello följande exempel-dokument.

    {
        "Volcano Name": "Rainier",
           "Country": "United States",
          "Region": "US-Washington",
          "Location": {
            "type": "Point",
            "coordinates": [
              -121.758,
              46.87
            ]
          },
          "Elevation": 4392,
          "Type": "Stratovolcano",
          "Status": "Dendrochronology",
          "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }

Vill du tooretrieve hello vulkanen data från hello Cosmos-DB-kontot och visualisera data i en interaktiv Power BI-rapport som hello följande rapport.

![Den här kursen Power BI med Power BI-anslutningsprogrammet för hello kommer du att kunna toovisualize data med hello Power BI Desktop vulkanen rapport](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

Redo toogive detta till en försök? Nu sätter vi igång.

1. Kör Power BI Desktop på din arbetsstation.
2. När Power BI Desktop startas en *Välkommen* visas.
   
    ![Power BI Desktop välkomstskärmen - anslutningsprogrammet för Power BI](./media/powerbi-visualize/power_bi_connector_welcome.png)
3. Du kan **hämta Data**, se **de senaste källorna**, eller **öppna andra rapporter** direkt från hello *Välkommen* skärmen.  Klicka på hello X hello övre högra hörnet tooclose hello-skärmen. Hej **rapporten** av Power BI Desktop visas.
   
    ![Power BI Desktop rapportvyn - anslutningsprogrammet för Powerbi](./media/powerbi-visualize/power_bi_connector_pbireportview.png)
4. Välj hello **Start** band, och klicka sedan på **hämta Data**.  Hej **hämta Data** fönster visas.
5. Klicka på **Azure**väljer **Microsoft Azure DocumentDB (Beta)**, och klicka sedan på **Anslut**. 

    ![Power BI Desktop hämta Data - anslutningsprogrammet för Powerbi](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   
6. På hello **Preview Connector** klickar du på **Fortsätt**. Hej **Microsoft Azure DocumentDB Connect** visas.
7. Ange hello Cosmos DB konto slutpunkts-URL du gillar tooretrieve hello data från enligt nedan och klickar sedan på **OK**. toouse ditt eget konto kan du hämta hello URL: en från hello URI rutan i hello  **[nycklar](manage-account.md#keys)**  bladet för hello Azure-portalen. toouse hello demonstrera konto, ange `https://analytics.documents.azure.com` för hello-URL. 
   
    Ingenting hello databasens namn, samlingsnamn och SQL-uttryck som dessa fält är valfria.  I stället använder vi hello Navigator tooselect hello databas och samling tooidentify där hello data kommer från.
   
    ![Power BI självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - Desktop ansluta fönster](./media/powerbi-visualize/power_bi_connector_pbiconnectwindow.png)
8. Om du ansluter toothis slutpunkt för hello första gången uppmanas du hello kontonyckel. Hämta hello nyckeln för ditt eget konto från hello **primärnyckel** rutan i hello  **[skrivskyddade nycklar](manage-account.md#keys)**  bladet för hello Azure-portalen. För hello demo konto hello nyckeln är `MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==`. Ange rätt nyckel för hello och klicka sedan på **Anslut**.
   
    Vi rekommenderar att du använder hello skrivskyddad sekundärnyckel när du skapar rapporter.  Detta förhindrar onödig exponering av hello huvudnyckeln toopotential säkerhetsrisker. hello skrivskyddad nyckeln är tillgänglig från hello [nycklar](manage-account.md#keys) bladet hello Azure-portalen eller du kan använda hello demo kontoinformation som anges ovan.
   
    ![Power BI självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - Kontonyckel](./media/powerbi-visualize/power_bi_connector_pbidocumentdbkey.png)
    
    > [!NOTE] 
    > Om du får ett felmeddelande med texten kunde ”hello angivna databasen inte hittas”. Se hello lösning steg i den här [Power BI problemet](https://community.powerbi.com/t5/Issues/Document-DB-Power-BI/idi-p/208200).
    
9. När hello kontot är korrekt ansluten, hello **Navigator** visas.  Hej **Navigator** visar en lista över databaser under hello-konto.
10. Klicka på och expandera hello databasen där hello data hello rapporten hämtas från, om du använder hello demo konto väljer du **volcanodb**.   
11. Nu Välj en samling som du ska hämta hello data från. Om du använder hello demo-konto markerar **volcano1**.
    
    hello förhandsgranskning visar en lista över **post** objekt.  Ett dokument återges som en **post** typ i Power BI. På samma sätt kan en kapslad JSON-block i ett dokument är också en **post**.
    
    ![Power BI självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - Navigator-fönstret](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Klicka på **redigera** toolaunch hello frågeredigeraren i ett nytt fönster tootransform hello data.

## <a name="flattening-and-transforming-json-documents"></a>Förenkla och omvandla JSON-dokument
1. Växla toohello Power BI Query Editor-fönstret, där hello **dokument** kolumn i hello mellersta rutan.
   ![Power BI Desktop frågeredigeraren](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Klicka på hello expander på hello höger sida av hello **dokument** kolumnrubriken.  hello snabbmenyn med en lista med fält visas.  Välj hello fält som behövs för rapporten, till exempel, vulkanen namn, land, Region, plats, höjning, typ, Status och senaste vet fall och klicka sedan på **OK**.
   
    ![Power BI-självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - Expandera dokument](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. hello mitten av fönstret visas en förhandsgranskning av hello resultatet med hello fält som valts.
   
    ![Power BI-självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - platta ut resultat](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. I vårt exempel är hello egenskapen Location som ett GeoJSON-block i ett dokument.  Som du ser plats representeras som en **post** typ i Power BI Desktop.  
5. Klicka på hello expander på hello höger sida av hello plats kolumnrubriken.  hello snabbmenyn med typ och koordinater fält visas.  Vi väljer hello koordinater fältet och klickar på **OK**.
   
    ![Power BI självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - posten](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. hello mittrutan visas nu en kolumn för koordinater i **lista** typen.  Visas hello början av hello kursen är hello GeoJSON data i den här kursen av typen punkt med latitud och longitud värden i hello koordinater matris.
   
    hello koordinater [0] element representerar longitud medan koordinater [1] representerar latitud.
    ![Power BI självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - koordinater lista](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. tooflatten hello samordnar matris, skapar vi en **anpassad kolumnen** kallas LatLong.  Välj hello **Add Column** band och klicka på **Lägg till anpassad kolumn**.  Hej **Lägg till anpassad kolumn** fönster visas.
8. Ange ett namn för hello ny kolumn, t.ex. LatLong.
9. Ange sedan hello anpassad formel för hello nya kolumnen.  I vårt exempel kommer vi sammanfoga hello latitud och longitud värden åtskilda med kommatecken enligt nedan med hello följande formel: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. Klicka på **OK**.
   
    Mer information om Data Analysis uttryck (DAX) inklusive DAX-funktioner finns [DAX grundläggande i Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).
   
    ![Power BI-självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - Lägg till anpassad kolumn](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)
10. Nu visas hello mittrutan hello nya LatLong kolumnen fylls med hello latitud och longitudvärden åtskilda med kommatecken.
    
    ![Power BI självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - kolumnen för anpassade LatLong](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Om du får ett fel i hello ny kolumn, se till att hello tillämpas steg under frågeinställningar matchar hello följande bild:
    
    ![Tillämpade steg ska vara källan, navigering, expanderas dokumentet, expanderas Document.Location, lägga till anpassad](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    Om du är olika, ta bort hello extra steg och försök lägga till anpassade hello-kolumnen igen. 
11. Vi har nu slutfört förenkling hello data i tabellformat.  Du kan utnyttja alla hello-funktioner som är tillgängliga i hello frågeredigeraren tooshape och transformera data i Cosmos-databasen.  Om du använder hello exempel ändra hello datatyp för höjning för**heltal** genom att ändra hello **datatyp** på hello **Start** menyfliksområdet.
    
    ![Power BI självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - ändra kolumntypen](./media/powerbi-visualize/power_bi_connector_pbichangetype.png)
12. Klicka på **stängs och** toosave hello datamodellen.
    
    ![Power BI självstudiekurs för Azure Cosmos DB Power BI-anslutningsprogrammet - Stäng & tillämpa](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-hello-reports"></a>Skapa hello rapporter
Power BI Desktop rapportvyn är där du kan börja skapa rapporter toovisualize data.  Du kan skapa rapporter genom att dra och släppa fält i hello **rapporten** arbetsyta.

![Power BI Desktop rapportvyn - anslutningsprogrammet för Powerbi](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

I hello rapportvyn, bör du hitta:

1. Hej **fält** rutan detta är där du kan se en lista över datamodeller med fält som du kan använda för dina rapporter.
2. Hej **visualiseringar** fönstret. En rapport kan innehålla en eller flera visualiseringar.  Välj hello visual typer passa dina behov från hello **visualiseringar** fönstret.
3. Hej **rapporten** arbetsyta, det är där du skapar hello visuell information för rapporten.
4. Hej **rapporten** sidan. Du kan lägga till flera rapportsidor i Power BI Desktop.

hello följande visar hello grundläggande steg för att skapa en enkel interaktiv karta Visa rapport.

1. I vårt exempel skapar vi en karta vy som visar hello platsen för varje vulkanen.  I hello **visualiseringar** klickar du på hello visual typ som är markerade i hello skärmbilden ovan.  Du bör se hello visual karttyp återges i hello **rapporten** arbetsyta.  Hej **visualiseringen** rutan bör också visa en uppsättning egenskaper relaterade toohello visual schematyp.
2. Nu kan dra och släppa hello LatLong fält från hello **fält** fönstret toohello **plats** egenskap i **visualiseringar** fönstret.
3. Därefter dra och släpp hello vulkanen namnet fältet toohello **förklaring** egenskapen.  
4. Dra och släpp hello höjning fältet toohello **storlek** egenskapen.  
5. Du bör nu se hello kartan visual visar en uppsättning bubblor som visar hello platsen för varje vulkanen med hello storlek på hello bubblan korrelerar toohello höjning av hello vulkanen.
6. Du har nu skapat en enkel rapport.  Du kan anpassa rapporten hello ytterligare genom att lägga till fler.  I vårt fall vi har lagt till en vulkanen utsnitt toomake hello rapport interaktiva.  
   
    ![Skärmbild av hello slutgiltiga Power BI Desktop rapporten vid slutförandet av hello Power BI-självstudiekurs för Azure Cosmos DB](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

## <a name="publish-and-share-your-report"></a>Publicera och dela din rapport
tooshare rapporten, du måste ha ett konto på PowerBI.com.

1. I hello Power BI Desktop, klickar du på hello **Start** menyfliksområdet.
2. Klicka på **Publicera**.  Du kan ange tooenter hello användarnamnet och lösenordet för ditt konto på PowerBI.com.
3. När hello autentiseringsuppgifter har autentiserats är hello rapporten publicerade tooyour mål som du har valt.
4. Klicka på **öppna 'PowerBITutorial.pbix' i Power BI** toosee och dela din rapport på PowerBI.com.
   
    ![Publishing tooPower BI lyckades! Öppna kursen i Power BI](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>Skapa en instrumentpanel i PowerBI.com
Nu när du har en rapport kan du dela den på PowerBI.com

När du publicerar din rapport från Power BI Desktop tooPowerBI.com genererar en **rapporten** och en **Dataset** i PowerBI.com-klienten. Till exempel när du har publicerat en rapport som kallas **PowerBITutorial** tooPowerBI.com, PowerBITutorial visas i båda hello **rapporter** och **datauppsättningar** avsnitten på PowerBI.com.

   ![Skärmbild av hello ny rapport och datamängd på PowerBI.com](./media/powerbi-visualize/powerbi-reports-datasets.png)

toocreate delbar instrumentpanelen, klicka på hello **PIN-kod Live sidan** knappen på PowerBI.com rapporten.

   ![Skärmbild av hello ny rapport och datamängd på PowerBI.com](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Följ sedan instruktionerna hello i [fästa en panel från en rapport](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) toocreate en ny instrumentpanel. 

Du kan också göra tillfälliga ändringar tooreport innan du skapar en instrumentpanel. Det rekommenderas dock att du använder Power BI Desktop tooperform hello ändringar och publicera hello rapporten tooPowerBI.com.

## <a name="refresh-data-in-powerbicom"></a>Uppdatera data på PowerBI.com
Det finns två sätt toorefresh data, ad hoc- och schemalagda.

För en ad hoc-uppdatering, klicka på hello eclipses (...) av hello **Dataset**, t.ex. PowerBITutorial. Du bör se en lista med åtgärder, inklusive **Uppdatera nu**. Klicka på **Uppdatera nu** toorefresh hello data.

![Skärmbild av Uppdatera nu på PowerBI.com](./media/powerbi-visualize/power-bi-refresh-now.png)

För en schemalagd uppdatering hello följande.

1. Klicka på **Uppdatera schema** i listan över hello. 

    ![Skärmbild av hello Uppdatera schema på PowerBI.com](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. I hello **inställningar** expanderar **datakällans autentiseringsuppgifter**. 
3. Klicka på **redigera autentiseringsuppgifter**. 
   
    hello Konfigurera popup-fönster visas. 
4. Ange hello viktiga tooconnect toohello Cosmos DB konto för uppsättningen data och klicka sedan på **logga in**. 
5. Expandera **Uppdatera schema** och ställa in hello schema du vill toorefresh hello dataset. 
6. Klicka på **tillämpa** och du är klar konfigurerar hello schemalagd uppdatering.

## <a name="next-steps"></a>Nästa steg
* toolearn mer om Power BI finns [Kom igång med Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* toolearn mer om Cosmos DB finns hello [Azure Cosmos DB dokumentations-landningssidan](https://azure.microsoft.com/documentation/services/documentdb/).

