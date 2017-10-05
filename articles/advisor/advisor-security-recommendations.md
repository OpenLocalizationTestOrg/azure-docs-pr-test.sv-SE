---
title: "Säkerhetsrekommendationer i Azure Advisor | Microsoft Docs"
description: "Använda Azure Advisor för att förbättra säkerheten för din Azure-distributioner."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 53be05593c3733ccb27979a3a026414013125779
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-security-recommendations"></a>Advisor säkerhetsrekommendationer

Azure Advisor ger en konsekvent konsoliderad vy över rekommendationer för alla dina Azure-resurser. Den kan integreras med Azure Security Center så att du säkerhetsrekommendationer. Du kan hämta säkerhetsrekommendationer från den **säkerhet** på Advisor-instrumentpanelen.

![Knappen Advisor-säkerhet](./media/advisor-security-recommendations/advisor-security-tab.png)

Med hjälp av Security Center kan du förebygga, upptäcka och åtgärda hot med bättre överblick och kontroll över säkerheten för dina resurser i Azure. Funktionen analyserar regelbundet säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer. Rekommendationerna som leder dig genom processen att konfigurera kontroller som du behöver. 

![Fliken Advisor-säkerhet](./media/advisor-security-recommendations/advisor-security-recommendations.png)

Mer information om säkerhetsrekommendationer finns [hantera säkerhetsrekommendationer i Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).

## <a name="how-to-access-security-recommendations-in-azure-advisor"></a>Hur du kommer åt säkerhetsrekommendationer i Azure Advisor

1. Logga in på [Azure Portal](https://portal.azure.com).

2. I den vänstra rutan klickar du på **fler tjänster**.

3. I fönstret service menyn under **övervakning och hantering av**, klickar du på **Azure Advisor**.  
 Advisor-instrumentpanelen visas.

4. Advisor-instrumentpanelen, klicka på den **säkerhet** fliken.

5. Välj den prenumeration som du vill få rekommendationer och klicka sedan på **få rekommendationer**.

> [!NOTE]
> Om du vill komma åt Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor. En prenumeration registreras när en *prenumeration ägare* startar Advisor instrumentpanelen och klickar på den **få rekommendationer** knappen. Det här är en *engångsåtgärd*. När prenumerationen har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.

## <a name="next-steps"></a>Nästa steg

Mer information om Advisor-rekommendationer finns:
* [Introduktion till Advisor](advisor-overview.md)
* [Kom igång med Advisor](advisor-get-started.md)
* [Kostnad Advisor-rekommendationer](advisor-performance-recommendations.md)
* [Advisor-rekommendationer](advisor-performance-recommendations.md)
* [Advisor-rekommendationer för hög tillgänglighet](advisor-high-availability-recommendations.md)


 
