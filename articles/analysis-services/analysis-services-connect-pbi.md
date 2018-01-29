---
title: Anslut till Azure Analysis Services med Powerbi | Microsoft Docs
description: "Lär dig hur du ansluter till en Azure Analysis Services-server med hjälp av Power BI."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/10/2018
ms.author: owend
ms.openlocfilehash: ea1094d0ce858cd7df9c49f18fb81b07e31fca53
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/11/2018
---
# <a name="connect-with-power-bi"></a>Ansluta med Powerbi

När du har skapat en server i Azure och distribuerat en tabellmodell till den, är användare i din organisation redo att ansluta och börja utforska data. 

> [!TIP]
> Se till att använda den senaste versionen av [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Ansluta i Power BI Desktop

1. Klicka på Power BI Desktop **hämta Data** > **Azure** > **Azure Analysis Services-databasen**.

2. I **Server**, ange namnet på servern. Se till att ta fullständiga URL-Adressen. till exempel asazure://westcentralus.asazure.windows.net/advworks.

3. I **databasen**, om du känner till namnet på tabellmodelldatabas eller perspektiv som du vill ansluta till, klistra in den här. Annars kan du lämna fältet tomt och välja en databas eller perspektiv senare.

4. Låt standardvärdet **Anslut live** alternativet, och tryck sedan på **Anslut**. Importera anslutningar stöds inte för närvarande.

5. Om du uppmanas ange dina inloggningsuppgifter. 

6. I **Navigator**, expandera servern och välj sedan modellen eller perspektiv som du vill ansluta till och klicka sedan på **Anslut**. Klicka på en modell eller perspektiv för att visa alla objekt i vyn.

    Modellen öppnas i Power BI Desktop med en tom rapport i rapportvyn. Listan över fält visar alla icke-dolda modellobjekt. Anslutningsstatus visas i det nedre högra hörnet.

## <a name="connect-in-power-bi-service"></a>Ansluta i Power BI (service)

1. Skapa en Power BI Desktop-fil som har en aktiv anslutning till din modell på servern.
2. I [Power BI](https://powerbi.microsoft.com), klickar du på **hämta Data** > **filer**, leta upp och markera .PBX-fil.



## <a name="see-also"></a>Se också
[Anslut till Azure Analysis Services](analysis-services-connect.md)   
[Klientbibliotek](analysis-services-data-providers.md)

