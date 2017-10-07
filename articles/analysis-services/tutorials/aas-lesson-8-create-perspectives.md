---
title: "aaa ”Azure Analysis Services självstudiekursen lektionen 8 skapa perspektiv | Microsoft Docs ”"
description: "Beskriver hur toocreate perspektiv i hello självstudiekursen Azure Analysis Services-projekt."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.openlocfilehash: 25391813e1969ecb22af4d6f9c1ccd8358d812fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="lesson-8-create-perspectives"></a>Lektion 8: Skapa perspektiv

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

I den här lektionen skapar du ett perspektiv för Internetförsäljning. Ett perspektiv definierar en visningsbar delmängd av en modell som ger fokuserade, affärsspecifika eller programspecifika översiktsvyer. När en användare ansluter tooa modellen med hjälp av ett perspektiv, visas endast de modellobjekt (tabeller, kolumner, mått, hierarkier och KPI: er) som fält som definierats i det perspektivet. Det finns fler toolearn [perspektiv](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).
  
hello Internet försäljning perspektiv som du skapar i den här lektionen undantar hello DimCustomer tabellobjekt. När du skapar ett perspektiv som undantar vissa objekt från vyn finns objektet fortfarande i hello modellen. Men det är inte synligt i fältlistan i rapporteringsklienten. Beräknade kolumner och mått, som antingen är inkluderade i ett perspektiv eller inte, kan fortfarande göra beräkningar från objektdata som exkluderas.  
  
hello syftet med den här lektionen är toodescribe hur toocreate perspektiv och bekanta dig med hello tabellmodell redigeringsverktyg. Om du senare expanderar den här modellen tooinclude ytterligare tabeller kan du skapa ytterligare perspektiv toodefine olika synvinkel hello modell, till exempel inventerings- och försäljning.  
  
Uppskattad tid toocomplete lektionen: **fem minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 7: skapa prestationsindikatorer](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  
  
## <a name="create-perspectives"></a>Skapa perspektiv  
  
#### <a name="toocreate-an-internet-sales-perspective"></a>toocreate ett Internet försäljning perspektiv  
  
1.  Klicka på hello **modellen** menyn > **perspektiv** > **skapa och hantera**.  
  
2.  I hello **perspektiv** dialogrutan klickar du på **nytt perspektiv**.  
  
3.  Dubbelklicka på hello **nytt perspektiv** kolumnrubriken och Byt sedan namn **Internet försäljning**.  
  
4.  Välj hello alla hello tabeller *utom* **DimCustomer**.  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    I en senare lektionen använder du hello analysera i Excel funktionen tootest det här perspektivet. hello Excel fältlistan för pivottabell innehåller varje tabell utom hello DimCustomer tabell.  

## <a name="whats-next"></a>Nästa steg
[Lektion 9: Skapa hierarkier](../tutorials/aas-lesson-9-create-hierarchies.md).
  
  
  
  
