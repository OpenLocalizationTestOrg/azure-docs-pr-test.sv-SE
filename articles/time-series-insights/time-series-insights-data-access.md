---
title: "aaaData åtkomstprinciper i Azure tid serien insikter | Microsoft Docs"
description: "I kursen får du lära dig toomanage principerna dataåtkomst i tid serien insikter"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: f286d26c8c5c851c523e9384760dc4b10711fa3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="grant-data-access-tooa-time-series-insights-environment-using-azure-portal"></a>Bevilja åtkomst tooa tid serien insikter datamiljö med hjälp av Azure portal

Time Series Insights-miljöer har två oberoende typer av principer för åtkomst.

* Hantera åtkomstprinciper
* Dataåtkomstprinciper

Båda principerna beviljar Azure Active Directory-huvudkonton (användare och appar) olika behörigheter i en viss miljö. hello säkerhetsobjekt (användare och appar) måste tillhöra toohello active directory (eller ”Azure-klient”) som associeras med hello som innehåller hello-miljö.

Principer för hantering av åtkomst bevilja som behörigheter relaterade toohello konfigurationen av hello-miljö
*   Skapa och ta bort hello miljö, händelsekällor referera till datamängder och
*   Hantering av hello principerna dataåtkomst.

Principerna dataåtkomst bevilja behörighet tooissue datafrågor manipulera referensdata i hello miljö och dela sparade frågor och perspektiv som är associerade med hello-miljön.

hello två typer av principer kan tydlig uppdelning mellan toohello åtkomsthantering av hello miljö och åtkomst till toohello data i hello-miljö. Det är exempelvis möjligt toosetup en miljö så att hello ägare/skapare av hello miljö tas bort från hello dataåtkomst. Samt användare och tjänster som är tillåtna tooread data från hello miljö beviljas ingen åtkomst toohello konfiguration av hello-miljö.

## <a name="grant-data-access"></a>Bevilja åtkomst till data
hello följande steg visar hur toogrant dataåtkomst för en UPN:

1.  Logga in toohello [Azure-portalen](https://portal.azure.com).
2.  Klicka på ”alla resurser” hello menyn hello vänster på hello Azure-portalen.
3.  Välj Time Series Insights-miljö.

  ![Hantera hello tid serien insikter source - miljö](media/data-access/getstarted-grant-data-access1.png)

4.  Välj ”Dataplansåtkomst” och klicka på ”Lägg till”

  ![Hantera hello tid serien insikter källa – Lägg till](media/data-access/getstarted-grant-data-access2.png)

5.  Klicka på ”Välj användare”.
6.  Söka och välja användare via hello e-post.
7.  Klicka på ”Välj” i bladet ”Välj användare”.

  ![Hantera hello tid serien insikter source - Välj användare](media/data-access/getstarted-grant-data-access3.png)

8.  Klicka på ”Välj roll”.
9.  Välj ”bidragsgivare” om du vill tooallow användardata toochange referens och dela sparade frågor och perspektiv med andra användare av hello-miljö. Annars väljer ”Reader” tooallow frågan användardata i hello miljö och spara personliga (inte delade) frågor i hello-miljö.
10. Klicka på ”Ok” hello ”Välj roll”-bladet.

  ![Hantera hello tid serien insikter source - Välj rolltjänster](media/data-access/getstarted-grant-data-access4.png)

11. Klicka på ”Ok” hello ”Välj användarroll”-bladet.
12. Du bör se:

  ![Hantera hello tid serien insikter source - resultat](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Nästa steg

* [Skapa en händelsekälla](time-series-insights-add-event-source.md)
* [Skicka händelser](time-series-insights-send-events.md) toohello händelsekälla
* Visa din miljö i [Time Series Insights-portalen](https://insights.timeseries.azure.com)
