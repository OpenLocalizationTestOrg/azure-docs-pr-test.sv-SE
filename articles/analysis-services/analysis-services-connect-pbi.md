---
title: aaaConnect tooAzure Analysis Services med Power BI | Microsoft Docs
description: "Lär dig hur tooconnect tooan Azure Analysis Services-servern med hjälp av Power BI."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: f6c4cdec6edb92900ad2e552e23a4d9172ba9b84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-power-bi"></a>Ansluta med Powerbi

När du har skapat en server i Azure och distribuerat en tabellmodell tooit, användare i din organisation är klar tooconnect och utforska data. 

> [!TIP]
> Vara säker på att toouse hello senaste versionen av [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Ansluta i Power BI Desktop

1. Klicka på Power BI Desktop **hämta Data** > **Azure** > **Azure Analysis Services-databasen**.

2. I **Server**, ange hello servernamn. 
    
    Vara säker på att tooinclude hello fullständiga URL: en. Till exempel asazure://westcentralus.asazure.windows.net/advworks.

3. I **databasen**, om du känner till hello tabellmodelldatabas eller perspektiv som du vill tooconnect och klistra in den här hello namn. Annars kan du lämna fältet tomt och välja en databas eller perspektiv senare.

4. Lämna hello standard **Anslut live** alternativet, och tryck sedan på **Anslut**. 

5. Om du uppmanas ange dina inloggningsuppgifter. 

6. I **Navigator**, expandera hello server och markerar sedan hello modell eller perspektiv som du vill tooconnect till, klicka på **Anslut**. Klicka på en modell- och perspektivparametrarna tooshow alla hello-objekt för att visa.

    hello modellen öppnas i Power BI Desktop med en tom rapport i rapportvyn. hello fältlistan visas alla icke-dolda modellobjekt. Anslutningsstatus visas i hello nedre högra hörnet.

## <a name="connect-in-power-bi-service"></a>Ansluta i Power BI (service)

1. Skapa en Power BI Desktop-fil som har en aktiv anslutning tooyour modell på servern.
2. I [Power BI](https://powerbi.microsoft.com), klickar du på **hämta Data** > **filer**. Leta upp och markera filen.



## <a name="see-also"></a>Se även
[Ansluta tooAzure Analysis Services](analysis-services-connect.md)   
[Klientbibliotek](analysis-services-data-providers.md)

