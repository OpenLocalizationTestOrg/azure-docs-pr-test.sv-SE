---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 7: skapa prestationsindikatorer | Microsoft Docs ”beskrivning: Beskriver hur toocreate prestationsindikatorer i hello självstudiekursen Azure Analysis Services-projekt. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend
---
# <a name="lesson-7-create-key-performance-indicators"></a>Lektion 7: Skapa KPI:er

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

I den här lektionen skapar du KPI:er. KPI: er är används toogauge prestanda för ett värde som definieras av en *Base* måttet, mot en *mål* värdet som definierats av ett mått eller som ett absolut värde. I rapporter klientprogram, kan KPI: er ger företag tekniker ett snabbt och enkelt sätt toounderstand en sammanfattning av företag har lyckats eller tooidentify trender. Det finns fler toolearn [KPI: er](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)
  
Uppskattad tid toocomplete lektionen: **15 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 6: skapa mått](../tutorials/aas-lesson-6-create-measures.md).   
  
## <a name="create-key-performance-indicators"></a>Skapa KPI:er  
  
#### <a name="toocreate-an-internetcurrentquartersalesperformance-kpi"></a>toocreate en InternetCurrentQuarterSalesPerformance KPI  
  
1.  Klicka på hello i hello modellen designer **FactInternetSales** tabell.  
  
2.  Klicka på en tom cell i hello mått rutnätet.  
  
3.  Skriv följande formel hello i hello formelfältet ovanför hello tabell: 
 
    ```  
    InternetCurrentQuarterSalesPerformance :=DIVIDE([InternetCurrentQuarterSales]/[InternetPreviousQuarterSalesProportionToQTD],BLANK())  
    ```

    Det här måttet fungerar som hello basmåttet för hello KPI.  
  
4.  Högerklicka på **InternetCurrentQuarterSalesPerformance** > **Skapa KPI**.   
  
5.  Hello Key Performance Indicator (KPI) i dialogrutan i **mål** Välj **absoluta värdet**, och skriv sedan **1.1**.  
  
7.  Skriv i hello vänstra (låg) skjutreglaget **1**, och klicka sedan på hello höger (hög) skjutreglaget anger **1,07**.  
  
8.  I **Välj ikonen Style**väljer hello rombformade (röd), triangel (gul), cirkel (grön) ikonen typen.
  
    ![aas-lesson7-kpi](../tutorials/media/aas-lesson7-kpi.png)
    
    > [!TIP]  
    > Meddelande hello utbyggbara **beskrivningar** etiketten under hello tillgängliga ikonformat. Använda beskrivningar för hello olika KPI element toomake dem mer identifierbart i klientprogram.  
  
9. Klicka på **OK** toocomplete hello KPI.  
  
    Observera i hello mått rutnätet hello ikonen nästa toohello **InternetCurrentQuarterSalesPerformance** mått. Den här ikonen anger att det här måttet är basvärdet för en KPI.  
  
#### <a name="toocreate-an-internetcurrentquartermarginperformance-kpi"></a>toocreate en InternetCurrentQuarterMarginPerformance KPI  
  
1.  I hello mått rutnät för hello **FactInternetSales** tabell, en tom cell.  
  
2.  Skriv följande formel hello i hello formelfältet ovanför hello tabell:  

    ```
    InternetCurrentQuarterMarginPerformance :=IF([InternetPreviousQuarterMarginProportionToQTD]<>0,([InternetCurrentQuarterMargin]-[InternetPreviousQuarterMarginProportionToQTD])/[InternetPreviousQuarterMarginProportionToQTD],BLANK())  
    ```
 
3.  Högerklicka på **InternetCurrentQuarterMarginPerformance** > **Skapa KPI**.  
  
4.  Hello Key Performance Indicator (KPI) i dialogrutan i **mål** Välj **absoluta värdet**, och skriv sedan **1,25**.   
  
5.  Dra tills hello fältet visar hello vänstra (låg) skjutreglaget fältet **0,8**, och sedan bilden hello höger (hög) skjutreglaget fältet tills hello fältet visar **1,03 enligt följande**.  
  
6.  I **Välj ikonen Style**, Välj hello rombformade (röd), triangel (gul), cirkel (grön) Ikontyp och klicka sedan på **OK**.  
  
## <a name="whats-next"></a>Nästa steg
[Lektion 8: Skapa perspektiv](../tutorials/aas-lesson-8-create-perspectives.md).
  
  
