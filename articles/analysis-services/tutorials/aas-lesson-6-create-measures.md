---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 6: skapa mått | Microsoft Docs ”beskrivning: Beskriver hur toocreate åtgärder i självstudiekursen hello Azure Analysis Services-projekt. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---
# <a name="lesson-6-create-measures"></a>Lektion 6: Skapa mått

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Nu bör skapa du mått toobe ingår i din modell. Liknande toohello beräknade kolumner som du har skapat, ett mått är en beräkning som skapats med hjälp av en DAX-formel. Men till skillnad från beräknade kolumner utvärderas mått baserat på ett *filter* som en användare har valt. Till exempel lägga en viss kolumn eller utsnitt toohello radetiketter fält i en pivottabell. Ett värde för varje cell i hello filter beräknas sedan genom hello tillämpas mått. Åtgärder är kraftfulla, flexibla beräkningar som du vill tooinclude i nästan alla tabellmodeller tooperform dynamiska beräkningar på numeriska data. Det finns fler toolearn [åtgärder](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).
  
toocreate åtgärder som du använder hello *mått rutnätet*. Varje tabell har som standard ett tomt rutnät för mått, men vanligtvis skapar du inte mått för varje tabell. hello mått rutnätet visas under en tabell i hello modellen designer i datavyn. toohide eller visa hello mått rutnät för en tabell, klickar du på hello **tabell** -menyn och klicka sedan på **visa mått rutnät**.  
  
Du kan skapa ett mått genom att klicka på en tom cell i hello mått rutnätet och sedan skriva en DAX-formel i formelfältet hello. När du klickar du på RETUR toocomplete hello formel, hello mått visas sedan i hello cell. Du kan också skapa mått med hjälp av en standard aggregeringsfunktionen genom att klicka på en kolumn och sedan klicka på hello Autosumma (**∑**) hello i verktygsfältet. Åtgärder som skapats med hjälp av funktionen för hello Autosumma visas i hello mått rutnätscell direkt under hello kolumn, men kan flyttas.  
  
I kursen skapar du mått med både en DAX-formel inom hello formelfältet och med hello Autosumma-funktionen.  
  
Uppskattad tid toocomplete lektionen: **30 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 5: skapa beräknade kolumner](../tutorials/aas-lesson-5-create-calculated-columns.md).  
  
## <a name="create-measures"></a>Skapa mått  
  
#### <a name="toocreate-a-dayscurrentquartertodate-measure-in-hello-dimdate-table"></a>toocreate ett DaysCurrentQuarterToDate mått i hello DimDate tabell  
  
1.  Klicka på hello i hello modellen designer **DimDate** tabell.  
  
2.  Klicka på hello övre vänstra tom cell i hello mått rutnätet.  
  
3.  Skriv i formelfältet hello hello följande formel:  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    Meddelande hello övre vänstra cellen innehåller nu ett Måttnamn **DaysCurrentQuarterToDate**, följt av hello resultatet **92**.
    
      ![aas-lesson6-newmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    Till skillnad från beräknade kolumner med formler för mått skriver du hello Måttnamn följt av ett kolon och hello formeln uttryck.

  
#### <a name="toocreate-a-daysincurrentquarter-measure-in-hello-dimdate-table"></a>toocreate ett DaysInCurrentQuarter mått i hello DimDate tabell  
  
1.  Med hello **DimDate** tabell fortfarande aktiva i hello modellen designer i hello mått rutnät, hello tom cell nedan hello mått som du skapade.  
  
2.  Skriv i formelfältet hello hello följande formel:  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    När du skapar en jämförelse förhållandet mellan en ofullständig period och hello föregående period. hello formeln måste beräkna hello andelen hello period som har gått ut och jämför den toohello samma procentandel hello föregående period. I detta fall [DaysCurrentQuarterToDate] / [DaysInCurrentQuarter] ger hello andelen förfluten tid i hello aktuella perioden.  
  
#### <a name="toocreate-an-internetdistinctcountsalesorder-measure-in-hello-factinternetsales-table"></a>toocreate ett InternetDistinctCountSalesOrder mått i hello FactInternetSales tabell  
  
1.  Klicka på hello **FactInternetSales** tabell.   
  
2.  Klicka på hello **SalesOrderNumber** kolumnrubriken.  
  
3.  Hello nedåtpilen nästa toohello Autosumma på hello verktygsfältet (**∑**) och välj sedan **DistinctCount**.  
  
    hello Autosumma funktionen skapar automatiskt ett mått för hello markerade kolumnen, med formeln för hello DistinctCount standard aggregering.  
    
       ![aas-lesson6-newmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  I hello mått rutnätet klickar du på hello nytt mått, och klicka sedan på hello **egenskaper** fönster i **Måttnamn**, byta namn på hello mått för**InternetDistinctCountSalesOrder**. 
 
  
#### <a name="toocreate-additional-measures-in-hello-factinternetsales-table"></a>toocreate ytterligare åtgärder i hello FactInternetSales tabell  
  
1.  Med funktionen hello Autosumma kan skapa och namnet hello följande åtgärder:  

    |Kolumn|Måttnamn|Autosumma (∑)|Formel|  
    |----------------|----------|-----------------|-----------|  
    |SalesOrderLineNumber|InternetOrderLinesCount|Antal|=COUNTA([SalesOrderLineNumber])|  
    |OrderQuantity|InternetTotalUnits|Summa|=SUM([OrderQuantity])|  
    |DiscountAmount|InternetTotalDiscountAmount|Summa|=SUM([DiscountAmount])|  
    |TotalProductCost|InternetTotalProductCost|Summa|=SUM([TotalProductCost])|  
    |SalesAmount|InternetTotalSales|Summa|=SUM([SalesAmount])|  
    |Marginal|InternetTotalMargin|Summa|=SUM([Margin])|  
    |TaxAmt|InternetTotalTaxAmt|Summa|=SUM([TaxAmt])|  
    |Frakt|InternetTotalFreight|Summa|=SUM([Freight])|  
  
2.  Genom att klicka på en tom cell i hello mått rutnät och genom att använda hello formelfältet, skapa och namnet hello följande åtgärder i ordning:  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
Åtgärder för hello FactInternetSales tabell kan vara används tooanalyze kritiska ekonomiska data, till exempel försäljning, kostnader och vinst för objekt som definieras av valda hello Användarfilter.  
  
## <a name="whats-next"></a>Nästa steg
[Lektion 7: Skapa KPI:er](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  

  
