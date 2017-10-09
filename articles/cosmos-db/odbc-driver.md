---
title: "aaaConnect tooAzure Cosmos-databas med hjälp av verktyg för BI analytics | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Cosmos DB ODBC-drivrutinen toocreate tabeller och vyer så att normaliserade data kan visas i BI och data analytics-programvara."
keywords: ODBC, odbc-drivrutinen
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 9967f4e5-4b71-4cd7-8324-221a8c789e6b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.openlocfilehash: e12a70f7805445f09fac01411e4bfbccc9a29c7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooazure-cosmos-db-using-bi-analytics-tools-with-hello-odbc-driver"></a>Ansluta tooAzure Cosmos DB med ODBC-drivrutinen för hello BI-verktyg för analys

hello Azure Cosmos DB ODBC-drivrutinen möjliggör tooconnect tooAzure Cosmos-databasen med BI analytics verktyg som SQL Server Integration Services, Power BI Desktop och Tableau så att du kan analysera och skapa visualiseringar av dina Azure Cosmos DB-data i dem lösningar.

hello Azure Cosmos DB ODBC-drivrutinen är ODBC 3.8 kompatibel och stöder ANSI SQL-92-syntax. Hej drivrutinen ger omfattande funktioner toohelp renormalize av data i Azure Cosmos DB. Du kan med hello drivrutin, för att representera data i Azure Cosmos DB som tabeller och vyer. hello-drivrutinen möjliggör tooperform SQL-åtgärder mot hello tabeller och vyer inklusive Gruppera efter frågor, infogningar, uppdateringar och borttagningar.

## <a name="why-do-i-need-toonormalize-my-data"></a>Varför måste jag göra toonormalize Mina data?
Azure Cosmos-databasen är en schemalös databas, så det möjliggör snabb utveckling av appar genom att aktivera program tooiterate sina data modell hello direkt och begränsa dem inte tooa strikt schema. En enda Azure Cosmos-DB-databas kan innehålla olika strukturer JSON-dokument. Det är bra för snabb utveckling, men när du vill tooanalyze och skapa rapporter för dina data med hjälp av dataanalys och BI-verktyg, hello data ofta måste toobe förenklas och följa tooa visst schema.

Det är där hello ODBC-drivrutinen kommer in. Med hjälp av hello ODBC-drivrutinen, kan du nu renormalized data i Azure Cosmos DB i tabeller och vyer passning tooyour data analys och rapportering måste. Hej renormalized scheman har ingen inverkan på hello underliggande data och inte begränsa utvecklare tooadhere toothem kan de helt enkelt aktiverar du tooleverage ODBC-kompatibel verktyg tooaccess hello data. Nu Azure Cosmos-DB-databas kommer inte bara vara en favorit för Utvecklingsteamet, men data analytikerna kommer gillar det för.

Nu kan komma igång med hello ODBC-drivrutinen.

## <a id="install"></a>Steg 1: Installera hello Azure Cosmos DB ODBC-drivrutinen

1. Hämta hello drivrutiner för din miljö:

    * [Microsoft Azure Cosmos DB ODBC 64-bit.msi](https://aka.ms/documentdb-odbc-64x64) för 64-bitars Windows
    * [Microsoft Azure Cosmos DB ODBC 32 x 64-bit.msi](https://aka.ms/documentdb-odbc-32x64) för 32-bitars på Windows 64-bitars
    * [Microsoft Azure Cosmos DB ODBC 32-bit.msi](https://aka.ms/documentdb-odbc-32x32) för 32-bitars Windows

    Kör hello msi-filen lokalt, vilket startar hello **installationsguiden för Microsoft Azure Cosmos DB ODBC-drivrutinen**. 
2. Slutför installationsguiden för hello hello standard inkommande tooinstall hello ODBC-drivrutin.
3. Öppna hello **ODBC Data source Administrator** appen på din dator och du kan göra detta genom att skriva **ODBC-datakällor** i Windows hello sökrutan. 
    Du kan bekräfta hello-drivrutinen har installerats genom att klicka på hello **drivrutiner** flik och se till att **ODBC-drivrutinen för Microsoft Azure Cosmos DB** visas.

    ![Azure Cosmos DB ODBC Data Source Administrator](./media/odbc-driver/odbc-driver.png)

## <a id="connect"></a>Steg 2: Anslut tooyour Azure Cosmos-DB-databas

1. Efter [installerar hello Azure Cosmos DB ODBC-drivrutinen](#install), i hello **ODBC Data Source Administrator** -fönstret klickar du på **Lägg till**. Du kan skapa en användare eller System-DSN. I det här exemplet skapar vi ett användar-DSN.
2. I hello **Skapa ny datakälla** väljer **ODBC-drivrutinen för Microsoft Azure Cosmos DB**, och klicka sedan på **Slutför**.
3. I hello **SDN installationsprogram för Azure Cosmos DB ODBC-drivrutinen** Fyll i hello följande: 

    ![Azure Cosmos DB ODBC-drivrutinen DSN inställningsfönstret](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Namn på datakälla**: egna eget namn för hello ODBC DSN. Det här namnet är unikt tooyour Azure Cosmos DB konto, så namnet på lämpligt sätt om du har flera konton.
    - **Beskrivning**: en kort beskrivning av hello-datakälla.
    - **Värden**: URI för Azure DB som Cosmos-konto. Du kan hämta det från hello Azure Cosmos DB nycklar bladet i hello Azure-portalen som visas i följande skärmbild hello. 
    - **Snabbtangent**: hello primär eller sekundär, skrivskyddad eller skrivskyddad nyckeln från bladet för hello Azure Cosmos DB nycklar i hello Azure-portalen som visas i följande skärmbild hello. Vi rekommenderar att du använder hello skrivskyddad sekundärnyckel om hello DSN används för skrivskyddade databehandling och rapportering.
    ![Azure DB-nycklar för Cosmos-bladet](./media/odbc-driver/odbc-driver-keys.png)
    - **Kryptera åtkomstnyckeln för**: Välj hello bäst baserat på hello användare för den här datorn. 
4. Klicka på hello **Test** knappen toomake att du kan ansluta tooyour Azure DB som Cosmos-konto. 
5. Klicka på **avancerade alternativ** och ange hello följande värden:
    - **Fråga konsekvenskontroll**: Välj hello [konsekvensnivå](consistency-levels.md) för din verksamhet. hello standardvärdet är Session.
    - **Antal återförsök**: Ange hello många gånger tooretry en åtgärd om hello första begäran inte slutföras på grund av tooservice begränsning.
    - **Schemafilen**: du har ett antal alternativ här.
        - Som standard, lämnar den här posten är (tom) söker hello drivrutinen hello första sidan data för alla samlingar toodetermine hello schemat för varje samling. Detta kallas samlingen mappning. Utan en schemafilen som definierats hello drivrutinen har tooperform hello genomsökning för varje drivrutin-session och kan resultera i en senare tid för ett program med hjälp av hello DSN att starta. Vi rekommenderar att du alltid associerar en schemafil för en Datakälla.
        - Om du redan har en schemafilen (eventuellt en som du skapat med hjälp av hello [schemat Editor](#schema-editor)), kan du klicka på **Bläddra**navigerar tooyour filen, klickar du på **spara**, och klicka sedan på **OK**.
        - Om du vill toocreate ett nytt schema, klickar du på **OK**, och klicka sedan på **schemat Editor** i hello huvudfönstret. Gå sedan vidare toohello [schemat Editor](#schema-editor) information. När du skapar hello nya schemafilen, Kom ihåg toogo tillbaka toohello **avancerade alternativ** fönstret tooinclude hello nyskapad schemafilen.

6. När du slutför och stänger hello **Azure Cosmos DB ODBC-drivrutinen DSN för** fönstret hello nya användar-DSN är läggs toohello användar-DSN-fliken.

    ![Nya Azure Cosmos DB ODBC DSN hello användar-DSN på fliken](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a id="#collection-mapping"></a>Steg 3: Skapa schemadefinition metoden hello samling mappning

Det finns två typer av provtagningsmetoder som du kan använda: **samling mappning** eller **tabell avgränsare**. En provtagning session kan använda båda provtagningsmetoderna, men varje samling kan bara använda en specifik urvalsmetoden. hello stegen nedan för att skapa ett schema för hello data i en eller flera samlingar metoden hello samling mappning. Den här urvalsmetoden hämtar hello data hello-sidan i en samling toodetermine hello struktur hello data. Den transposes en samling tooa tabell på hello ODBC-sida. Den här urvalsmetoden är effektiv och snabb när hello data i en samling är homogen. Om en samling innehåller heterogena typ av data, rekommenderar vi du använder hello [tabell avgränsare mappning metoden](#table-mapping) eftersom det ger en mer robusta urvalsmetoden toodetermine hello datastrukturer i hello samling. 

1. När du har slutfört steg 1 – 4 i [Anslut tooyour Azure Cosmos DB databasen](#connect), klickar du på **schemat Editor** i hello **Azure Cosmos DB ODBC-drivrutinen DSN för** fönster.

    ![Schemat editor-knapp i hello Azure Cosmos DB ODBC-drivrutinen DSN för fönster](./media/odbc-driver/odbc-driver-schema-editor.png)
2. I hello **schemat Editor** -fönstret klickar du på **Skapa nytt**.
    Hej **generera schemat** fönstret visas alla hello samlingar i hello Azure DB som Cosmos-konto. 
3. Välj en eller flera samlingar toosample och klicka sedan på **exempel**. 
4. I hello **designvyn** fliken, hello database, schema och tabellen representeras. I hello tabellvy visar hello genomsökning hello uppsättning egenskaper associerade med hello kolumnnamn (SQL-namn, namnet på datakällan, osv.).
    För varje kolumn, kan du ändra hello kolumnnamnet SQL, hello SQL-typ, SQL-längd (om tillämpligt), skala (om tillämpligt), Precision (om tillämpligt) och kan ha värdet null.
    - Du kan ange **Dölj kolumn** för**SANT** om du vill tooexclude kolumnen från frågeresultaten. Kolumner markerad Dölj kolumn = true returneras inte för val och projektion, även om de fortfarande är en del av hello schemat. Du kan till exempel dölja alla hello Azure Cosmos DB krävs Systemegenskaper börjar med ”_”.
    - Hej **id** kolumnen är hello fält som inte får vara dolda eftersom den används som hello primärnyckel i hello normaliserade schemat. 
5. När du har definierat hello schemat klickar du på **filen** | **spara**, navigera toohello katalogschemat toosave hello och klicka sedan på **spara**.

    Om du vill ha toouse i hello framtida schemat med en Datakälla öppnas hello Azure Cosmos DB ODBC-drivrutinen DSN för (via hello ODBC Data Source Administrator), avancerade alternativ och gå sedan toohello spara schemat hello schemafilen i rutan. Sparar en schemat filen tooan ändrar befintliga DSN hello DSN anslutning tooscope toohello data och struktur som definieras av schemat.

## <a id="table-mapping"></a>Steg 4: Skapa en schemadefinition som använder hello tabell-avgränsare mappning metod

Det finns två typer av provtagningsmetoder som du kan använda: **samling mappning** eller **tabell avgränsare**. En provtagning session kan använda båda provtagningsmetoderna, men varje samling kan bara använda en specifik urvalsmetoden. 

hello följande steg att skapa ett schema för hello data i en eller flera samlingar med hello **tabell avgränsare** mappning av metoden. Vi rekommenderar att du använder den här provtagningsmetoden när samlingar innehåller heterogena typ av data. Du kan använda den här metoden tooscope hello provtagning tooa uppsättning attribut och dess motsvarande värden. Om ett dokument innehåller en egenskap för ”typ”, kan du definiera hello provtagning toohello värden för den här egenskapen. hello slutresultatet hello provtagning är en uppsättning tabeller för varje hello värden för typ som du har angett. Skriv till exempel = bil genererar en bil tabell vid typ = plan skulle skapa en plan tabell.

1. När du har slutfört steg 1 – 4 i [Anslut tooyour Azure Cosmos DB databasen](#connect), klickar du på **schemat Editor** i hello Azure Cosmos DB ODBC-drivrutinen DSN inst.
2. I hello **schemat Editor** -fönstret klickar du på **Skapa nytt**.
    Hej **generera schemat** fönstret visas alla hello samlingar i hello Azure DB som Cosmos-konto. 
3. Markera en samling på hello **Exempelvyn** på fliken hello **definitionen mappning** för hello samlingen, klickar du på **redigera**. I hello **definitionen mappning** väljer **tabell avgränsare** metod. Sedan hello följande:

    a. I hello **attribut** rutan, hello namn på en egenskap som avgränsare. Detta är en egenskap i dokumentet som du vill tooscope hello, till exempel City och tryck RETUR. 

    b. Om du bara vill tooscope hello provtagning toocertain värden för hello-attribut som du precis angav Markera hello-attributet i hello alternativrutan och ange ett värde i hello **värdet** rutan, till exempel Seattle och tryck på RETUR. Du kan fortsätta tooadd flera värden för attributen. Se bara till att rätt attribut är markerad när du anger värden hello.

    Om du inkluderar exempelvis en **attribut** värdet av ort, och du vill toolimit tabell-tooonly innehåller rader med ett värde för ort i New York och Dubai, anger du stad i hello attribut, och New York och sedan Dubai i hello  **Värden** rutan.
4. Klicka på **OK**. 
5. När du har slutfört hello mappning definitioner för hello samlingar du vill toosample i hello **schemat Editor** -fönstret klickar du på **exempel**.
     För varje kolumn, kan du ändra hello kolumnnamnet SQL, hello SQL-typ, SQL-längd (om tillämpligt), skala (om tillämpligt), Precision (om tillämpligt) och kan ha värdet null.
    - Du kan ange **Dölj kolumn** för**SANT** om du vill tooexclude kolumnen från frågeresultaten. Kolumner markerad Dölj kolumn = true returneras inte för val och projektion, även om de fortfarande är en del av hello schemat. Du kan till exempel dölja alla hello Azure Cosmos DB krävs Systemegenskaper börjar med ”_”.
    - Hej **id** kolumnen är hello fält som inte får vara dolda eftersom den används som hello primärnyckel i hello normaliserade schemat. 
6. När du har definierat hello schemat klickar du på **filen** | **spara**, navigera toohello katalogschemat toosave hello och klicka sedan på **spara**.
7. Tillbaka i hello **Azure Cosmos DB ODBC-drivrutinen DSN för** -fönstret klickar du på ** Avancerade alternativ **. Sedan hello **schemafilen** , navigera toohello sparade schemafilen och klicka **OK**. Klicka på **OK** igen toosave hello DSN. Detta sparar hello schema du skapade toohello DSN. 

## <a name="optional-creating-views"></a>(Valfritt) Skapa vyer
Du kan definiera och skapa vyer som en del av hello provtagning. Dessa vyer finns motsvarande tooSQL vyer. De är skrivskyddade och scope hello val och hello som definierats i Azure Cosmos-Databasens SQL-projektioner. 

toocreate en vy för dina data i hello **schemat Editor** i hello fönstret **vydefinitioner** kolumn, klickar du på **Lägg till** på hello raden i hello samlingen toosample. I hello **vydefinitioner** fönstret hello följande:
1. Klicka på **ny**, ange ett namn för hello vy, till exempel EmployeesfromSeattleView och klicka sedan på **OK**.
2. I hello **Redigera vy** och ange en Azure Cosmos DB-fråga. Detta måste vara en Azure Cosmos-Databasens SQL-fråga, till exempel`SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Gender, c.Manager FROM c WHERE c.City = “Seattle”`, och klicka sedan på **OK**.

Du kan skapa flera vyer som du vill. När du är klar definierande hello vyer, kan du sedan prov hello data. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>Steg 5: Visa dina data i BI-verktyg som Power BI Desktop

Du kan använda din nya DSN tooconnect DocumentADB med en ODBC-kompatibel tools - det här steget bara visas hur tooconnect tooPower BI Desktop och skapa en Power BI-visualisering.

1. Öppna Power BI Desktop.
2. Klicka på **hämta Data**.
3. I hello **hämta Data** -fönstret klickar du på **andra** | **ODBC** | **Anslut**.
4. I hello **från ODBC** fönster, Välj hello namn på datakälla du skapat och klicka sedan på **OK**. Du kan lämna hello **avancerade alternativ** poster som är tom.
5. I hello **åtkomst till en datakälla med hjälp av en ODBC-drivrutin** väljer **standard eller anpassad** och klicka sedan på **Anslut**. Du behöver inte tooinclude hello **autentiseringsuppgifter anslutningssträngsegenskaper**.
6. I hello **Navigator** i hello till vänster i fönstret expanderar hello databasen, hello schemat och välj sedan hello tabell. hello resultatfönstret innehåller hello data med hjälp av hello-schema som du skapade.
7. toovisualize hello data i Power BI desktop kryssrutan hello framför hello tabellnamn klicka sedan på **belastningen**.
8. Välj fliken för hello Data i Power BI Desktop på hello längst till vänster, ![Datafliken i Power BI Desktop](./media/odbc-driver/odbc-driver-data-tab.png) tooconfirm dina data har importerats.
9. Du kan nu skapa grafer med Power BI genom att klicka på fliken för hello rapport ![fliken rapport i Power BI Desktop](./media/odbc-driver/odbc-driver-report-tab.png), klicka på **nya Visual**, och anpassa din panel. Mer information om hur du skapar visualiseringar i Power BI Desktop finns [visualiseringstyper i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Felsökning

Om du får följande fel hello Kontrollera hello **värden** och **åtkomstnyckeln** värden som du kopierade hello Azure-portalen i [steg 2](#connect) är korrekta och försök sedan igen. Använd hello kopiera knappar toohello höger i hello **värden** och **åtkomstnyckeln** värden i hello Azure portal toocopy hello värden felfritt.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"hello input authorization token can't serve hello request. Please check that hello expected payload is built as per hello protocol, and check hello key being used. Server used hello following payload toosign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Nästa steg

toolearn mer om Azure Cosmos DB finns [vad är Azure Cosmos DB?](introduction.md).
