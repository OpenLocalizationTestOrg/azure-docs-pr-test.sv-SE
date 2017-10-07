---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 9: skapa hierarkier | Microsoft Docs ”beskrivning: services: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend
---
# <a name="lesson-9-create-hierarchies"></a>Lektion 9: Skapa hierarkier

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

I den här lektionen skapar du hierarkier. Hierarkier är grupper med kolumner som är ordnade i nivåer. En geografisk hierarki kan till exempel ha undernivåer för land, delstat, län och ort. Hierarkier kan visas separat från andra kolumner i en reporting klienten programmet Fältlista, gör dem lättare för klienten användare toonavigate och ingå i en rapport. Det finns fler toolearn [hierarkier](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)
  
toocreate hierarkier använder hello modellen designer i *diagramvyn*. Skapa och hantera hierarkier stöds inte i datavyn.  
  
Uppskattad tid toocomplete lektionen: **20 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 8: skapa perspektiv](../tutorials/aas-lesson-8-create-perspectives.md).  
  
## <a name="create-hierarchies"></a>Skapa hierarkier  
  
#### <a name="toocreate-a-category-hierarchy-in-hello-dimproduct-table"></a>toocreate en kategorihierarki i hello DimProduct tabell  
  
1.  Högerklicka på hello i hello modellen designer (diagramvyn) **DimProduct** tabell > **skapa hierarkin**. En ny hierarki visas längst ned hello hello tabell fönster. Byt namn på hello hierarkin **kategori**.  
  
2.  Klicka och dra hello **ProductCategoryName** kolumnen toohello nya **kategori** hierarki.  
  
3.  I hello **kategori** hierarki, högerklicka på hello **ProductCategoryName** > **Byt namn på**, och skriv sedan **kategori**.  
  
    > [!NOTE]  
    > Byta namn på en kolumn i en hierarki inte byta namn på kolumnen i tabellen hello. En kolumn i en hierarki är bara en representation av hello kolumn i hello tabell.  
  
4.  Klicka och dra hello **ProductSubcategoryName** kolumnen toohello **kategori** hierarki. Byt namn på den till **Underkategori**. 
  
5.  Högerklicka på hello **%{ModelName/** kolumnen > **lägga till toohierarchy**, och välj sedan **kategori**. Byt namn på den till **Modell**.

6.  Slutligen lägger du till **EnglishProductName** toohello kategorihierarki. Byt namn på den till **Produkt**.  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="toocreate-hierarchies-in-hello-dimdate-table"></a>toocreate hierarkier i hello DimDate tabell  
  
1.  I hello **DimDate** tabell, skapa en hierarki med namnet **kalender**.  
  
3.  Lägg till hello följande kolumner i ordning:

    *  CalendarYear
    *  CalendarSemester
    *  CalendarQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
    
4.  I hello **DimDate** tabell, skapa en **fiskala** hierarki. Inkludera hello följande kolumner i ordning:  
  
    *  FiscalYear
    *  FiscalSemester
    *  FiscalQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
  
5.  Slutligen i hello **DimDate** tabell, skapa en **ProductionCalendar** hierarki. Inkludera hello följande kolumner i ordning:  
    *  CalendarYear
    *  WeekNumberOfYear
    *  DayNumberOfWeek
  
 ## <a name="whats-next"></a>Nästa steg
[Lektion 10: Skapa partitioner](../tutorials/aas-lesson-10-create-partitions.md). 
  
  
