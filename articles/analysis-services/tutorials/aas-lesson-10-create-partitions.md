---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 10: skapa partitioner | Microsoft Docs ”beskrivning: Beskriver hur toocreate partitioner i självstudiekursen hello Azure Analysis Services-projekt. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend
---
# <a name="lesson-10-create-partitions"></a>Lektion 10: Skapa partitioner

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Nu bör skapa du partitioner toodivide hello FactInternetSales tabellen i mindre logiska delar som kan vara bearbetade (uppdateras) oberoende av andra partitioner. Som standard har alla tabeller som du inkludera i din modell en partition som innehåller alla hello tabellens kolumner och rader. Tabellen FactInternetSales hello vill vi toodivide hello data per år; en partition för varje hello tabell fem år. Varje partition kan sedan bearbetas separat. Det finns fler toolearn [partitioner](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular). 
  
Uppskattad tid toocomplete lektionen: **15 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 9: skapa hierarkier](../tutorials/aas-lesson-9-create-hierarchies.md).  
  
## <a name="create-partitions"></a>Skapa partitioner  
  
#### <a name="toocreate-partitions-in-hello-factinternetsales-table"></a>toocreate partitioner i hello FactInternetSales tabell  
  
1.  Expandera **Tabeller** i tabellmodellutforskaren och högerklicka på **FactInternetSales** > **Partitioner**.  
  
2.  I Partitionshanteraren klickar du på **kopiera**, och sedan ändrar hello namn för**FactInternetSales2010**.
  
    Eftersom du vill hello partition tooinclude bara de raderna i en viss tid för hello år 2010, måste du ändra hello frågeuttryck.
  
4.  Klicka på **Design** tooopen fråga redigeraren och klicka sedan på hello **FactInternetSales2010** frågan.

5.  I Förhandsgranska klickar du på hello NEDPIL i hello **OrderDate** kolumnrubriken och klicka sedan på **tidsvärdet filter** > **mellan**.

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  Hello Filtrera rader i dialogrutan i **visa rader där: OrderDate**, lämna **infaller efter eller är lika med**, och ange sedan i hello datumfält **2010-1-1**. Lämna hello **och** operatör som valts, välj sedan **innan**, ange i hello datumfält **2011-1-1**, och klicka sedan på **OK**.

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    Observera att ytterligare ett steg med namnet filtrerade rader finns i frågeredigeraren, i TILLÄMPADE STEG. Det här filtret är tooselect endast datum från 2010.

8.  Klicka på **Importera**.

    I Partitionshanteraren Observera hello fråga uttryck nu har en ytterligare filtrerade rader-instruktion.

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    Den här instruktionen anger den här partitionen bör inkludera endast hello data i de rader där hello OrderDate är i hello 2010 kalenderåret som angetts i hello filtrerade rader satsen.  
  
  
#### <a name="toocreate-a-partition-for-hello-2011-year"></a>toocreate en partition för hello 2011 år  
  
1.  Klicka på hello i hello partitioner **FactInternetSales2010** partition som du skapade och klicka sedan på **kopiera**.  Ändra hello partitionsnamnet för**FactInternetSales2011**. 

    Du behöver inte toouse frågeredigeraren toocreate en ny filtrerade rader-instruktion. Eftersom du har skapat en kopia av hello frågan för 2010 är allt du behöver toodo göra små ändringar i hello frågan 2011.
  
2.  I **frågeuttryck**, i ordning för den här partitionen tooinclude bara de raderna för hello 2011 år, Ersätt hello år i hello filtrerade rader-sats med **2011** och **2012**, t.ex.:  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="toocreate-partitions-for-2012-2013-and-2014"></a>toocreate partitioner för 2012, 2013 och 2014.  
  
- Följ hello föregående steg är att skapa partitioner för 2012, 2013 och 2014, ändra hello år i hello filtrerade rader satsen tooinclude endast rader för det aktuella året. 
  

## <a name="delete-hello-factinternetsales-partition"></a>Ta bort hello FactInternetSales partition
Nu när du har partitioner för varje år kan du ta bort hello FactInternetSales partition. förhindra överlappning när du väljer alla processen vid bearbetning av partitioner.

#### <a name="toodelete-hello-factinternetsales-partition"></a>toodelete hello FactInternetSales partition
-  Klicka på hello FactInternetSales partition och klicka sedan på **ta bort**.



## <a name="process-partitions"></a>Bearbeta partitioner  
I Partitionshanteraren Observera hello **sista bearbetade** kolumn för varje hello nya partitioner som du har skapat visas dessa partitioner inte har bearbetats. När du skapar partitioner, bör du köra en Process partitioner eller processen igen toorefresh hello tabelldata i dessa partitioner.  
  
#### <a name="tooprocess-hello-factinternetsales-partitions"></a>tooprocess hello FactInternetSales partitioner  
  
1.  Klicka på **OK** tooclose Partitionshanteraren.  
  
2.  Klicka på hello **FactInternetSales** tabell och klicka sedan på hello **modellen** menyn > **processen** > **processen partitioner**.  
  
3.  Hello processen partitioner i dialogrutan Kontrollera **läge** har angetts för**processen standard**.  
  
4.  Markera kryssrutan för hello i hello **processen** kolumn för varje hello fem partitionerar du skapade och klicka sedan på **OK**.  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    Om du uppmanas att ange autentiseringsuppgifter för personifiering ange hello Windows-användarnamn och lösenord som du angav i lektionen 2.  
  
    Hej **databearbetning** dialogrutan visas och visar information om processen för varje partition. Observera att olika antal rader överförs för varje partition. Varje partition innehåller bara de raderna för hello år som anges i hello WHERE-satsen i hello SQL-instruktionen. När bearbetningen är klar kan du gå vidare och stänga dialogrutan för hello databearbetning.  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a>Nästa steg
Gå toohello kursen: [lektionen 11: skapa roller](../tutorials/aas-lesson-11-create-roles.md). 
