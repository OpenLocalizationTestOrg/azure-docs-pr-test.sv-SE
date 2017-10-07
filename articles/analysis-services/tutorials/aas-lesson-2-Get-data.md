---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 2: hämta data | Microsoft Docs ”beskrivning: Beskriver hur tooget och importera data i hello självstudiekursen Azure Analysis Services-projekt. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---

# <a name="lesson-2-get-data"></a>Lektion 2: Hämta data

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Nu bör du använda hämta Data i SSDT tooconnect toohello AdventureWorksDW2014 exempeldatabasen, Välj data, förhandsgranskning och filter och sedan importera på din modell-arbetsyta.  
  
Med Hämta data kan du importera data från en mängd olika datakällor: Azure SQL Database, Oracle, Sybase, OData-Feed, Teradata, filer med mera. Det går även att fråga data med ett Power Query M-formeluttryck.
  
Uppskattad tid toocomplete lektionen: **10 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 1: skapa ett nytt projekt tabellmodell](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
## <a name="create-a-connection"></a>Skapa en anslutning  
  
#### <a name="toocreate-a-connection-toohello-adventureworksdw2014-database"></a>toocreate en toohello AdventureWorksDW2014 databas  
  
1.  Högerklicka på **Datakällor** > **Importera från datakälla** i tabellmodellutforskaren.  
  
    Detta startar hämta Data som hjälper dig att anslutande tooa datakälla. Om du inte ser Tabular modellen Explorer i **Solution Explorer**, dubbelklicka på **Model.bim** tooopen hello modellen i hello designer. 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  I Hämta Data klickar du på **Databas** > **SQL Server-databas** > **Anslut**.  
  
3.  I hello **SQL Server-databas** dialogrutan i **Server**hello typnamn för hello server där du installerade hello AdventureWorksDW2014 databasen och klicka sedan på **Anslut**.  

4.  När du uppmanas tooenter autentiseringsuppgifter, behöver du toospecify hello autentiseringsuppgifter för Analysis Services använder tooconnect toohello datakällan vid import och bearbetning av data. I **personifieringsläget** väljer du **Personifiera konto**, anger autentiseringsuppgifter och klickar på **Anslut**. Det rekommenderas att du använder ett konto där hello lösenordet upphör inte att gälla.

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > Med ett Windows-användarkonto och lösenord här hello säkraste metoden för anslutande tooa datakälla.
  
5.  Välj hello i Navigator **AdventureWorksDW2014** databasen och klicka sedan på **OK**. Detta skapar hello toohello databas. 
  
6.  I Navigator hello väljer du kryssrutan för hello följande tabeller: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**,  **DimProductCategory**, **DimProductSubcategory**, och **FactInternetSales**.  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
När du klickar på OK öppnas frågeredigeraren. I nästa avsnitt av hello väljer du endast hello data som du vill tooimport.

  
## <a name="filter-hello-table-data"></a>Filtrera hello tabelldata  
Tabeller i databasen hello AdventureWorksDW2014 har data som inte är nödvändiga tooinclude i modellen. När det är möjligt, vill du toofilter ut onödiga data toosave i-minne som används av hello modell. Du kan filtrera bort vissa hello kolumner från tabeller så att de inte har importerats till hello arbetsytan databas eller hello modelldatabasen när den har distribuerats. 
  
#### <a name="toofilter-hello-table-data-before-importing"></a>toofilter hello tabelldata innan du importerar  
  
1.  I frågeredigeraren Välj hello **DimCustomer** tabell. En vy av hello DimCustomer tabellen hello datasource (AdventureWorksDWQ2014 exempeldatabasen) visas. 
  
2.  Flermarkera (Ctrl + klicka) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation** och högerklicka och klicka sedan på **Ta bort kolumner**. 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    Eftersom hello värdena för dessa kolumner inte är relevanta tooInternet försäljning analys, finns inga behov tooimport dessa kolumner. Genom att ta bort onödiga kolumner blir din modell mindre och effektivare.  
  
4.  Filtrera hello återstående tabeller genom att ta bort hello följande kolumner i varje tabell:  
    
    **DimDate**
    
      |Kolumn|  
      |--------|  
      |DateKey|  
      |**SpanishDayNameOfWeek**|  
      |**FrenchDayNameOfWeek**|  
      |**SpanishMonthName**|  
      |**FrenchMonthName**|  
  
    **DimGeography**
  
      |Kolumn|  
      |-------------|  
      |**SpanishCountryRegionName**|  
      |**FrenchCountryRegionName**|  
      |**IpAddressLocator**|  
  
    **DimProduct**
  
      |Kolumn|  
      |-----------|  
      |**SpanishProductName**|  
      |**FrenchProductName**|  
      |**FrenchDescription**|  
      |**ChineseDescription**|  
      |**ArabicDescription**|  
      |**HebrewDescription**|  
      |**ThaiDescription**|  
      |**GermanDescription**|  
      |**JapaneseDescription**|  
      |**TurkishDescription**|  
  
    **DimProductCategory**
  
      |Kolumn|  
      |--------------------|  
      |**SpanishProductCategoryName**|  
      |**FrenchProductCategoryName**|  
  
    **DimProductSubcategory**
  
      |Kolumn|  
      |-----------------------|  
      |**SpanishProductSubcategoryName**|  
      |**FrenchProductSubcategoryName**|  
  
    **FactInternetSales**
  
      |Kolumn|  
      |------------------|  
      |**OrderDateKey**|  
      |**DueDateKey**|  
      |**ShipDateKey**|   
  
## <a name="Import"></a>Importera hello valt kolumndata  
Nu när du förhandsgranskas och filtreras bort onödiga data kan importera du hello resten av hello data som du vill. hello guiden importerar hello tabelldata tillsammans med eventuella relationer mellan tabeller. Nya tabeller och kolumner som skapas i hello modellen och importeras inte data som du har filtrerats ut.  
  
#### <a name="tooimport-hello-selected-tables-and-column-data"></a>tooimport hello valt kolumndata  
  
1.  Granska dina val. Om allt ser bra ut klickar du på **Importera**. hello databearbetning dialogrutan visar hello status för data som importeras från din datakälla till din arbetsyta-databas.
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  Klicka på **Stäng**.  

  
## <a name="save-your-model-project"></a>Spara modellprojektet  
Det är viktigt toofrequently spara projektet modellen.  
  
#### <a name="toosave-hello-model-project"></a>toosave hello modellprojekt  
  
-   Klicka på **Arkiv** > **Spara alla**.  
  
## <a name="whats-next"></a>Nästa steg
[Lektion 3: Markera som datumtabell](../tutorials/aas-lesson-3-mark-as-date-table.md).

  
  
