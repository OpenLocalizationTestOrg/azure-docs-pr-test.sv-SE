---
title: "aaaGet igång med Azure Advisor | Microsoft Docs"
description: "Kom igång med Azure Advisor."
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: 30fc8b8f3823f6f047e46cb9000189f3ccb3d514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-advisor"></a>Kom igång med Azure Advisor

Lär dig hur tooaccess Advisor via hello Azure-portalen get rekommendationer implementera rekommendationer, söka efter rekommendationer och uppdatera rekommendationer.

## <a name="get-advisor-recommendations"></a>Få Advisor-rekommendationer

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Hello vänster klickar du på **fler tjänster**.

3. I hello tjänsten menyn rutan under **övervakning och hantering av**, klickar du på **Azure Advisor**.  
 hello Advisor instrumentpanelen visas.

   ![Åtkomst till Azure Advisor med hello Azure-portalen](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. Välj hello prenumerationen för vilken du vill tooreceive rekommendationer på hello Advisor instrumentpanelen.  
hello Advisor instrumentpanelen visar anpassade rekommendationer för den valda prenumerationen. 

5. tooget rekommendationer för en viss kategori, klicka på någon av flikarna hello: **hög tillgänglighet**, **säkerhet**, **prestanda**, eller **kostnaden**.
 
> [!NOTE]
> tooaccess Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor. En prenumeration registreras när en *prenumeration ägare* startar hello Advisor instrumentpanelen och klickar på hello **få rekommendationer** knappen. Det här är en *engångsåtgärd*. När hello prenumeration har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.

  ![Azure Advisor-instrumentpanelen](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a>Hämta information om Advisor rekommendation och implementera en lösning

Hej **rekommendation** bladet i Advisor erbjuder ytterligare information om hello rekommendation. 

1. Logga in toohello [Azure-portalen](https://portal.azure.com), och sedan starta [Azure Advisor](https://aka.ms/azureadvisordashboard).

2. På hello **Advisor-rekommendationer** instrumentpanelen, klickar du på **få rekommendationer**.

3. Klicka på en rekommendation som du vill tooreview i detalj i hello lista över rekommendationerna.  
Hej **rekommendation** bladet visas.

4. På hello **rekommendationer** bladet granska information om åtgärder du kan utföra tooresolve potentiella problem eller dra nytta av en kostnad spara affärsmöjlighet. 
  
  ![hello Advisor rekommendation bladet](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a>Sök efter Advisor-rekommendationer

Du kan söka efter rekommendationer för en viss grupp av prenumerationen eller resursen. Du kan också söka efter rekommendationer efter status.

1. Logga in toohello [Azure-portalen](https://portal.azure.com), och sedan starta [Azure Advisor](https://aka.ms/azureadvisordashboard).

2. Sök efter rekommendationer genom att filtrera för prenumerationer och resursgrupper rekommendation status (**Active** eller **Snoozed**).

3. toodisplay en lista med Advisor-rekommendationer som baseras på dina sökfilter kriterier klickar du på **få rekommendationer**.

  ![Advisor sökfilter villkor](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a>Viloläge eller ignorera Advisor-rekommendationer

1. Logga in toohello [Azure-portalen](https://portal.azure.com), och sedan starta [Azure Advisor](https://aka.ms/azureadvisordashboard).

2. Klicka på **få rekommendationer**, och i hello lista över rekommendationerna, klickar du på en rekommendation.

3. På hello **rekommendation** bladet, klickar du på **viloläge**.  

   ![Advisor rekommendation åtgärd exempel](./media/advisor-get-started/advisor-snooze.png)

4. Ange en viloläge tidsperiod, eller välj **aldrig** toodismiss hello rekommendation.


## <a name="next-steps"></a>Nästa steg

toolearn mer om Advisor, se:
* [Introduktion tooAzure Advisor](advisor-overview.md)
* [Advisor-rekommendationer för hög tillgänglighet](advisor-high-availability-recommendations.md)
* [Advisor säkerhetsrekommendationer](advisor-security-recommendations.md)
-  [Advisor-rekommendationer](advisor-performance-recommendations.md)
* [Kostnad Advisor-rekommendationer](advisor-performance-recommendations.md)
