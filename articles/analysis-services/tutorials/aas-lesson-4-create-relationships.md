---
title: "Azure Analysis Services självstudiekurs 4: Skapa relationer | Microsoft Docs"
description: "Beskriver hur du skapar relationer i Azure Analysis Services-självstudieprojektet."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/08/2018
ms.author: owend
ms.openlocfilehash: 57b3a172445047291f0aea5b1616b9dcbf6bf745
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/09/2018
---
# <a name="create-relationships"></a>Skapa relationer

I den här lektionen verifierar du relationerna som skapades automatiskt när du importerade data och lägger till nya relationer mellan olika tabeller. En relation är en koppling mellan två tabeller som anger hur data i tabellerna ska vara korrelerade. Tabellen DimProduct och tabellen DimProductSubcategory har till exempel en relation baserat på det faktum att varje produkt tillhör en underkategori. Mer information finns i [Relationer](https://docs.microsoft.com/sql/analysis-services/tabular-models/relationships-ssas-tabular).
  
Uppskattad tidsåtgång för den här lektionen: **10 minuter**  
  
## <a name="prerequisites"></a>Förutsättningar  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför uppgifterna i den här lektionen måste du ha slutfört föregående lektion: [Lektion 3: Markera som datumtabell](../tutorials/aas-lesson-3-mark-as-date-table.md). 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Granska befintliga relationer och lägga till nya relationer  
När du importerade data med Hämta data fick du sju tabeller från databasen AdventureWorksDW2014. Vanligtvis när du importerar data från en relationell källa importeras befintliga relationer automatiskt tillsammans med data. För att Hämta data automatiskt ska kunna skapa relationer i datamodellen måste det finnas relationer mellan tabeller och i datakällan.

Innan du börjar redigera din modell bör du kontrollera att relationerna mellan tabellerna har skapats på korrekt sätt. I den här självstudiekursen lägger du även till tre nya relationer.  

  
#### <a name="to-review-existing-relationships"></a>Granska befintliga relationer  
  
1.  Klicka på menyn **Modell** > **Modellvy** > **Diagramvy**.  

    Modelldesignern visas nu i diagramvyn, ett grafiskt format som visar alla tabeller du har importerat med linjer mellan dem. Linjerna mellan tabellerna visar de relationer som skapades automatiskt när du importerade data.
    
    ![aas-lesson4-diagram](../tutorials/media/aas-lesson4-diagram.png)
  
    > [!NOTE]
    > Om du inte ser några relationer mellan tabeller betyder det troligen att det inte finns några relationer mellan de tabellerna i datakällan.

    Inkludera så många av tabellerna som möjligt med hjälp av miniatyröversiktskontrollerna längst ner till höger i modelldesignern. Du kan också klicka på och dra tabeller till olika platser, dra dem närmare varandra eller placera dem i en viss ordning. Relationerna som finns mellan tabellerna påverkas inte av att du flyttar tabellerna. Om du vill visa alla kolumner i en viss tabell klickar du på och drar i tabellens kant för att göra den större eller mindre.  
  
2.  Klicka på den heldragna linjen mellan tabellen **DimCustomer** och tabellen **DimGeography**. En heldragen linje mellan de två tabellerna visar att relationen är aktiv, det vill säga att den används som standard vid beräkning av DAX-formler.  
  
    Observera att nu visas både kolumnen **GeographyKey** i tabellen **DimCustomer** och kolumnen **GeographyKey** i tabellen **DimGeography** i en ruta. Dessa kolumner används i relationen. Relationens egenskaper visas nu också i fönstret **Egenskaper**.  
  
    > [!TIP]  
    > Förutom att använda modelldesignern i diagramvyn kan du också använda dialogrutan Hantera relationer för att visa relationerna mellan alla tabeller i ett tabellformat. Högerklicka på **Relationer** > **Hantera relationer** i tabellmodellutforskaren.
  
3.  Kontrollera att följande relationer skapades när varje tabell importerades från databasen AdventureWorksDW:  
  
    |Aktiv|Tabell|Relaterad uppslagstabell|  
    |----------|---------|------------------------|  
    |Ja|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Ja|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Ja|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Ja|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Ja|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    Om någon av relationerna saknas kontrollerar du att modellen innehåller följande tabeller: DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory och FactInternetSales. Om tabeller från samma datakällsanslutning importeras vid olika tidpunkter skapas inte relationer mellan dessa tabeller. I så fall måste du skapa dem manuellt. Om inga relationer visas betyder det att det inte finns några relationer i datakällan. Du kan skapa dem manuellt i datakällan.

### <a name="take-a-closer-look"></a>Ta en närmare titt
Observera att det finns en pil, en asterisk och ett nummer på de linjer som visar relationen mellan tabeller i diagramvyn.

![aas-lesson4-line](../tutorials/media/aas-lesson4-line.png)

Pilen visar filterriktningen. Asterisken visar att den här tabellen är ”många”-sidan i relationens kardinalitet och ettan visar att tabellen är ”en”-sidan i relationen. Om du behöver redigera en relation, till exempel ändra relationens filterriktning eller kardinalitet, dubbelklickar du på relationslinjen för att öppna dialogrutan Redigera relation.

![aas-lesson4-edit](../tutorials/media/aas-lesson4-edit.png)

Dessa funktioner är avsedda för avancerad datamodellering och ligger utanför ramen för den här kursen. Mer information finns i [Bi-directional cross filters for tabular models in Analysis Services](https://docs.microsoft.com/sql/analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services) (Dubbelriktade korsfilter för tabellmodeller i Analysis Services).

I vissa fall kan du behöva skapa ytterligare relationer mellan tabeller i modellen som stöd för en viss affärslogik. I den här självstudiekursen måste du skapa tre ytterligare relationer mellan tabellen FactInternetSales och tabellen DimDate.  
  
#### <a name="to-add-new-relationships-between-tables"></a>Så här lägger du till nya relationer mellan tabeller  
  
1.  Klicka på och håll ned kolumnen **OrderDate** i tabellen **FactInternetSales** i modelldesignern och dra sedan markören till kolumnen **Date** i tabellen **DimDate** och släpp.  

    En heldragen linje visas som anger att du har skapat en aktiv relation mellan kolumnen **OrderDate** i tabellen **Internet Sales** och kolumnen **Date** i tabellen **Date**. 
  
      ![aas-lesson4-new](../tutorials/media/aas-lesson4-new.png) 
  
    > [!NOTE]  
    > När du skapar relationer väljs kardinalitet och filterriktning mellan den primära tabellen och den relaterade uppslagstabellen automatiskt.  
  
2.  Klicka på och håll ned kolumnen **DueDate** i tabellen **FactInternetSales** och dra sedan markören till kolumnen **Date** i tabellen **DimDate** och släpp.  
  
    En prickad linje visas som anger att du har skapat en inaktiv relation mellan kolumnen **DueDate** i tabellen **FactInternetSales** och kolumnen **Date** i tabellen **FactInternetSales**. Du kan ha flera relationer mellan tabeller, men endast en relation kan vara aktiv i taget. Inaktiva relationer kan göras aktiva för att utföra särskilda aggregeringar i anpassade DAX-uttryck.  
  
3.  Skapa slutligen en till relation. Klicka på och håll ned kolumnen **ShipDate** i tabellen **FactInternetSales** och dra sedan markören till kolumnen **Date** i tabellen **DimDate** och släpp.  
    
     ![aas-lesson4-newinactive](../tutorials/media/aas-lesson4-newinactive.png)
  
## <a name="whats-next"></a>Nästa steg
[Lektion 5: Skapa beräknade kolumner](../tutorials/aas-lesson-5-create-calculated-columns.md).
  
  
  
