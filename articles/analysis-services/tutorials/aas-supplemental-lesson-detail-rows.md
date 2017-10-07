---
Rubrik: aaa ”Azure Analysis Services självstudiekursen kompletterande lektionen: detaljrader | Microsoft Docs ”beskrivning: Beskriver hur toocreate en detalj rader uttryck i hello Azure Analysis Services-kursen.
tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend
---
# <a name="supplemental-lesson---detail-rows"></a>Kompletterande lektion – Detaljrader

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Kompletterande nu bör använda du hello DAX Editor toodefine ett uttryck i detalj rader. Ett uttryck för information om rader är en egenskap på ett mått och ger slutanvändare mer information om hello visar det sammanlagda resultatet för ett mått. 
  
Uppskattad tid toocomplete lektionen: **10 minuter**  
  
## <a name="prerequisites"></a>Krav  
Den här kompletterande lektionen ingår i en självstudiekurs om tabellmodeller. Innan du utför hello uppgifter i den här kompletterande lektionen bör du slutfört alla tidigare erfarenheter eller har en slutförd Adventure Works Internet försäljning modellen exempelprojektet.  
  
## <a name="what-do-we-need-toosolve"></a>Vad gör vi behöver toosolve?
Nu ska vi titta hello information för våra InternetTotalSales mått innan du lägger till ett uttryck för information om rader.

1.  Klicka på hello i SSDT, **modellen** menyn > **analysera i Excel** tooopen Excel och skapa en tom pivottabell.
  
2.  I **PivotTable-Fields**, lägga till hello **InternetTotalSales** för att mäta från hello FactInternetSales tabell**värden**, **CalendarYear**från hello DimDate tabell för**kolumner**, och **EnglishCountryRegionName** för**rader**. Vår pivottabell kan nu oss sammanlagda resultat från hello InternetTotalSales mått av regioner och år. 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. Dubbelklicka på ett insamlat värde för ett år och ett regionnamn i hello pivottabellen. Här dubbelklickar vi hello värde för Australien och hello år 2014. Ett nytt blad öppnas som innehåller data, men inte användbara data.

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
Vad vi vill toosee här är en tabell som innehåller kolumner och rader med data som bidrar toohello visar det sammanlagda resultatet för våra InternetTotalSales mått. toodo att vi kan lägga till ett uttryck för information om rader som en egenskap för hello mått.

## <a name="add-a-detail-rows-expression"></a>Lägga till ett uttryck för rader med detaljerad information

#### <a name="toocreate-a-detail-rows-expression"></a>toocreate ett detaljerat rader uttryck 
  
1. Klicka på hello i hello FactInternetSales tabell mått rutnät i SSDT, **InternetTotalSales** mått. 

2. I **egenskaper** > **detalj rader uttryck**, klicka på hello editor knappen tooopen hello DAX-redigeraren.

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. DAX-redigeraren, ange hello följande uttryck:

    ```
    SELECTCOLUMNS(
    FactInternetSales,
    "Sales Order Number", FactInternetSales[SalesOrderNumber],
    "Customer First Name", RELATED(DimCustomer[FirstName]),
    "Customer Last Name", RELATED(DimCustomer[LastName]),
    "City", RELATED(DimGeography[City]),
    "Order Date", FactInternetSales[OrderDate],
    "Internet Total Sales", [InternetTotalSales]
    )

    ```

    Det här uttrycket anger namn, kolumner och mått från hello FactInternetSales tabell och relaterade tabeller returneras när en användare dubbelklickar på ett aggregerade resultat i en pivottabell eller rapporten.

4. Ta bort hello blad skapade i steg3 i Excel, och dubbelklicka på ett insamlat värde. Den här gången med en detalj rader uttryck-egenskap som definierats för hello mått, ett nytt blad öppnas med mycket mer användbar data.

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. Distribuera om din modell.

  
## <a name="see-also"></a>Se även  
[Funktionen SELECTCOLUMNS (DAX)](https://msdn.microsoft.com/library/mt761759.aspx)   
[Kompletterande lektion – Dynamisk säkerhet](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Kompletterande lektion – Ojämna hierarkier](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  
