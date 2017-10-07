---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 4: skapa relationer | Microsoft Docs ”beskrivning: Beskriver hur toocreate relationer i hello självstudiekursen Azure Analysis Services-projekt. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend
---
# <a name="lesson-4-create-relationships"></a>Lektion 4: Skapa relationer

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Nu bör du verifiera hello relationer som har skapats automatiskt när du har importerat data och lägga till nya relationer mellan olika tabeller. En relation är en anslutning mellan två tabeller som fastställer hur hello data i dessa tabeller bör korreleras. Till exempel ha hello DimProduct hello DimProductSubcategory tabellerna och en relation utifrån hello faktum att varje produkt tillhör tooa underkategori. Det finns fler toolearn [relationer](https://docs.microsoft.com/sql/analysis-services/tabular-models/relationships-ssas-tabular).
  
Uppskattad tid toocomplete lektionen: **10 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 3: Markera som Datumtabell](../tutorials/aas-lesson-3-mark-as-date-table.md). 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Granska befintliga relationer och lägga till nya relationer  
När du har importerat data med hämta Data du har fått sju tabeller från hello AdventureWorksDW2014 databas. I allmänhet när du importerar data från en relationsdatabas källa importeras befintliga relationer automatiskt tillsammans med hello data. Men innan du börjar redigera din modell bör du kontrollera att relationerna mellan tabellerna har skapats på korrekt sätt. I den här självstudiekursen lägger du till tre nya relationer.  
  
#### <a name="tooreview-existing-relationships"></a>tooreview befintliga relationer  
  
1.  Klicka på hello **modellen** menyn > **Model-View** > **diagramvyn**.  

    hello visas modellen designer nu i diagramvyn, ett grafiskt format som visar alla hello-tabeller som du har importerat med linjer mellan dem. hello linjer mellan tabellerna visar hello relationer som skapades automatiskt när du har importerat hello data.
    
    ![aas-lesson4-diagram](../tutorials/media/aas-lesson4-diagram.png)
  
    Innehåller så många hello tabeller som möjligt med hjälp av miniatyröversikt kontroller i hello nedre högra hörnet av hello modellen designer. Du kan också klicka och dra tabeller toodifferent platser samman tabeller närmare eller placera dem i en viss ordning. Hello relationer redan mellan hello tabeller påverkas inte om du flyttar tabeller. tooview alla hello kolumner i en viss tabell klickar du på och dra i en tabell edge tooexpand eller mindre.  
  
2.  Klicka på hello heldragen linje mellan hello **DimCustomer** tabell och hello **DimGeography** tabell. hello heldragen linje mellan de två tabellerna visar den här relationen är aktiv, det vill säga den används som standard vid beräkning av DAX-formler.  
  
    Meddelande hello **GeographyKey** kolumn i hello **DimCustomer** tabell och hello **GeographyKey** kolumn i hello **DimGeography** tabell både visas varje nu i en ruta. Dessa kolumner används i hello relationen. hello relationens egenskaper nu visas också i hello **egenskaper** fönster.  
  
    > [!TIP]  
    > Dessutom toousing hello modellen designer i diagramvyn kan du också använda hello hantera relationer dialogrutan rutan tooshow hello relationer mellan alla tabeller i tabellformat. Högerklicka på **Relationer** > **Hantera relationer** i tabellmodellutforskaren.
  
3.  Kontrollera hello efter relationer skapades när hello tabeller har importerats från hello AdventureWorksDW-databasen:  
  
    |Active|Tabell|Relaterad uppslagstabell|  
    |----------|---------|------------------------|  
    |Ja|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Ja|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Ja|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Ja|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Ja|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    Om någon av relationerna hello saknas kontrollerar du att modellen innehåller hello följande tabeller: DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory och FactInternetSales. Om tabeller från hello samma datakällanslutning importeras på separata gånger, skapas inte några relationer mellan tabellerna och måste skapas manuellt.  

### <a name="take-a-closer-look"></a>Ta en närmare titt
Observera en pil, en asterisk och ett nummer på hello linjer som visar hello relation mellan tabeller i diagramvyn.

![aas-lesson4-line](../tutorials/media/aas-lesson4-line.png)

hello pil visar hello filterriktningen. hello asterisk visar den här tabellen är hello många sida i hello Relationens kardinalitet och hello en visar den här tabellen är hello ena sidan av hello relation. Om du behöver tooedit relationen. till exempel ändra hello relation filterriktningen eller kardinalitet genom att dubbelklicka på hello rad tooopen hello Redigera relation dialogrutan.

![aas-lesson4-edit](../tutorials/media/aas-lesson4-edit.png)

Dessa funktioner är avsedda för avancerade datamodeller och är utanför hello omfånget för den här kursen. Det finns fler toolearn [dubbelriktad kommunikation mellan filter för tabellmodeller i Analysis Services](https://docs.microsoft.com/sql/analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services).

I vissa fall kan behöva du toocreate ytterligare relationer mellan tabeller i din modell toosupport vissa affärslogik. För den här kursen behöver du toocreate tre ytterligare relationer mellan hello FactInternetSales tabellerna och hello DimDate.  
  
#### <a name="tooadd-new-relationships-between-tables"></a>tooadd nya relationer mellan tabeller  
  
1.  I hello modellen designer i hello **FactInternetSales** tabell, klicka och håll på hello **OrderDate** kolumn, och sedan dra hello markören toohello **datum** kolumn i hello  **DimDate** tabell och släpp.  

    En heldragen linje visas med information om du har skapat en aktiv relation mellan hello **OrderDate** kolumn i hello **Internet försäljning** tabell och hello **datum** kolumn i hello **Datum** tabell. 
  
      ![aas-lesson4-new](../tutorials/media/aas-lesson4-new.png) 
  
    > [!NOTE]  
    > När du skapar relationer, väljs automatiskt hello kardinalitet och filtrera riktning mellan hello primära tabell och hello relaterad uppslagstabell.  
  
2.  I hello **FactInternetSales** tabell, klicka och håll på hello **DueDate** kolumn, och sedan dra hello markören toohello **datum** kolumn i hello **DimDate** tabell och släpp.  
  
    En prickad linje visas med information om du har skapat en inaktiv relation mellan hello **DueDate** kolumn i hello **FactInternetSales** tabell och hello **datum** kolumn i Hej **DimDate** tabell. Du kan ha flera relationer mellan tabeller, men endast en relation kan vara aktiv i taget. Inaktiva relationer kan göras active tooperform särskilda aggregeringar i anpassade DAX-uttryck.  
  
3.  Skapa slutligen en till relation. I hello **FactInternetSales** tabell, klicka och håll på hello **leveransdag** kolumn, och sedan dra hello markören toohello **datum** kolumn i hello **DimDate** tabell och släpp.  
    
     ![aas-lesson4-newinactive](../tutorials/media/aas-lesson4-newinactive.png)
  
## <a name="whats-next"></a>Nästa steg
[Lektion 5: Skapa beräknade kolumner](../tutorials/aas-lesson-5-create-calculated-columns.md).
  
  
  
