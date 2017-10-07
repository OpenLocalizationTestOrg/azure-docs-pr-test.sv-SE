---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 5: skapa beräknade kolumner | Microsoft Docs ”beskrivning: Beskriver hur toocreate beräknade kolumner i självstudiekursen hello Azure Analysis Services-projekt. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---
# <a name="lesson-5-create-calculated-columns"></a>Lektion 5: Skapa beräknade kolumner

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Under den här lektionen skapar du data i modellen genom att lägga till beräknade kolumner. Du kan skapa beräknade kolumner (som anpassade kolumner) när du använder hämta Data med hjälp av hello Query Editor eller senare i hello modellen designer liknande gör du här. Det finns fler toolearn [beräknade kolumner](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).
  
Du kan skapa fem nya beräknade kolumner i tre olika tabeller. hello stegen är något annorlunda för varje aktivitet som visar att det finns flera sätt toocreate kolumner, byta namn på dem och placerar dem på olika platser i en tabell.  

Det är även den här lektionen där du först använder dataanalysuttryck (DAX). DAX är ett särskilt språk för att skapa mycket anpassningsbara formeluttryck för tabellmodeller. I den här kursen använder du DAX toocreate beräknade kolumner, mått och filter för rollen. Det finns fler toolearn [DAX i tabellmodeller](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular). 
  
Uppskattad tid toocomplete lektionen: **15 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 4: skapa relationer](../tutorials/aas-lesson-4-create-relationships.md). 
  
## <a name="create-calculated-columns"></a>Skapa beräknade kolumner  
  
#### <a name="create-a-monthcalendar-calculated-column-in-hello-dimdate-table"></a>Skapa en beräknad kolumn MonthCalendar i hello DimDate tabell  
  
1.  Klicka på hello **modellen** menyn > **Model-View** > **datavy**.  
  
    Beräknade kolumner kan endast skapas med hjälp av hello modellen designer i datavyn.  
  
2.  Klicka på hello i hello modellen designer **DimDate** tabellen (fliken).  
  
3.  Högerklicka på hello **CalendarQuarter** kolumnrubriken och klicka sedan på **Infoga kolumn**.  
  
    En ny kolumn med namnet **beräknad kolumn 1** är infogade toohello till vänster i hello **kvartal** kolumn.  
  
4.  Skriv i hello formelfältet ovanför hello tabell hello följande DAX-formel: AutoComplete hjälper till att du skriver hello fullständiga namn på kolumner och tabeller och visar hello funktioner som är tillgängliga.  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    Värden fylls sedan för alla hello rader i hello beräknad kolumn. Om du rulla hello tabellen, visas rader kan ha olika värden för den här kolumnen baserat på hello data på varje rad.    
  
5.  Byt namn på den här kolumnen för**MonthCalendar**. 

    ![aas-lesson5-newcolumn](../tutorials/media/aas-lesson5-newcolumn.png) 
  
hello MonthCalendar beräknade kolumnen innehåller en sorterbar för månad.  
  
#### <a name="create-a-dayofweek-calculated-column-in-hello-dimdate-table"></a>Skapa en beräknad kolumn DayOfWeek i hello DimDate tabell  
  
1.  Med hello **DimDate** tabell fortfarande aktiv klickar du på hello **kolumnen** -menyn och klicka sedan på **Add Column**.  
  
2.  Skriv i formelfältet hello hello följande formel:  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    Tryck på RETUR när du har skapat hello formel. hello nya kolumn läggs toohello längst till höger i hello tabell.  
  
3.  Byt namn på hello kolumnen för**DayOfWeek**.  
  
4.  Klicka på kolumnrubriken hello och dra sedan hello kolumnen mellan hello **EnglishDayNameOfWeek** kolumn och hello **DayNumberOfMonth** kolumn.  
  
    > [!TIP]  
    > Flytta kolumner i tabellen gör det enklare toonavigate.  
  
hello DayOfWeek beräknade kolumnen innehåller ett sorterbar namn för hello dagen i veckan.  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-hello-dimproduct-table"></a>Skapa en beräknad kolumn ProductSubcategoryName i hello DimProduct tabell  
  
  
1.  I hello **DimProduct** tabell, rulla toohello långt till höger i hello tabell. Meddelande hello längst till höger kolumn heter **Add Column** (kursiv), klicka på hello kolumnrubriken.  
  
2.  Skriv i formelfältet hello hello följande formel:  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  Byt namn på hello kolumnen för**ProductSubcategoryName**.  
  
Hej ProductSubcategoryName beräknad kolumn är används toocreate en hierarki i hello DimProduct tabellen som innehåller data från hello EnglishProductSubcategoryName kolumnen i hello DimProductSubcategory tabell. Hierarkier kan inte finnas i mer än en tabell. Du kan skapa hierarkier senare i lektion 9.  
  
#### <a name="create-a-productcategoryname-calculated-column-in-hello-dimproduct-table"></a>Skapa en beräknad kolumn ProductCategoryName i hello DimProduct tabell  
  
1.  Med hello **DimProduct** tabell fortfarande aktiv klickar du på hello **kolumnen** -menyn och klicka sedan på **Add Column**.  
  
2.  Skriv i formelfältet hello hello följande formel:  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  Byt namn på hello kolumnen för**ProductCategoryName**.  
  
Hej ProductCategoryName beräknad kolumn är används toocreate en hierarki i hello DimProduct tabellen som innehåller data från hello EnglishProductCategoryName kolumnen i hello DimProductCategory tabell. Hierarkier kan inte finnas i mer än en tabell.  
  
#### <a name="create-a-margin-calculated-column-in-hello-factinternetsales-table"></a>Skapa en beräknad kolumn marginal i hello FactInternetSales tabell  
  
1.  Välj hello i hello modellen designer **FactInternetSales** tabell.  
  
2.  Skapa en ny beräknad kolumn mellan hello **SalesAmount** kolumn och hello **TaxAmt** kolumn.  
  
3.  Skriv i formelfältet hello hello följande formel:  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  Byt namn på hello kolumnen för**marginal**.  
 
      ![aas-lesson5-newmargin](../tutorials/media/aas-lesson5-newmargin.png)
      
    hello marginal beräknad kolumn är används tooanalyze vinstmarginaler för varje försäljning.  
  
## <a name="whats-next"></a>Nästa steg
[Lektion 6: Skapa mått](../tutorials/aas-lesson-6-create-measures.md).
  
  
  
