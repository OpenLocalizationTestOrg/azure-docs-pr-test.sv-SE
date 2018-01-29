---
title: "Skapa och hantera åtgärdsgrupper i Azure portal | Microsoft Docs"
description: "Lär dig hur du skapar och hanterar åtgärdsgrupper i Azure-portalen."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: ancav
ms.openlocfilehash: 05775415e210333cf63565e7b5b554d014f6ba23
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/18/2017
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Skapa och hantera åtgärdsgrupper i Azure-portalen
## <a name="overview"></a>Översikt ##
Den här artikeln visar hur du skapar och hanterar åtgärdsgrupper i Azure-portalen.

Du kan konfigurera en lista med åtgärder med åtgärdsgrupper. Dessa grupper kan sedan användas när du definierar aktiviteten loggen aviseringar. Dessa grupper kan sedan återanvändas av varje aktivitet loggen aviseringen som du definierar, se till att samma åtgärder vidtas för varje gång som aktiviteten loggen avisering utlöses.

En grupp kan ha upp till 10 för varje åtgärdstyp av. Varje åtgärd består av följande egenskaper:

* **Namnet**: en unik identifierare inom åtgärdsgruppen.  
* **Åtgärdstyp**: skicka ett SMS, skicka ett e-postmeddelande, anropa en webhook, skicka data till ett ITSM verktyg, anropa en Azure-program eller kör en Automation-runbook.
* **Information om**: motsvarande phone nummer, e-postadress, webhook URI eller ITSM anslutningsinformation.

Information om hur du använder Azure Resource Manager-mallar för att konfigurera åtgärdsgrupper finns [åtgärd grupp Resource Manager-mallar](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Skapa en grupp med hjälp av Azure portal ##
1. I den [portal](https://portal.azure.com)väljer **övervakaren**. Den **övervakaren** bladet konsoliderar alla dina övervakning inställningar och data i en vy.

    ![Tjänsten ”Övervakaren”](./media/monitoring-action-groups/home-monitor.png)
2. I den **inställningar** väljer **åtgärdsgrupper**.

    ![Fliken ”åtgärdsgrupper”](./media/monitoring-action-groups/action-groups-blade.png)
3. Välj **Lägg till grupp**, och Fyll i fälten.

    ![Kommandot ”Lägg till grupp”](./media/monitoring-action-groups/add-action-group.png)
4. Ange ett namn i den **åtgärd gruppnamn** och ange ett namn i den **kortnamnet** rutan. Det korta namnet används i stället för en fullständig åtgärd gruppnamn när meddelanden som skickas med den här gruppen.

      ![Dialogrutan Lägg till grupp för åtgärden ”](./media/monitoring-action-groups/action-group-define.png)

5. Den **prenumeration** rutan autofills med din aktuella prenumeration. Den här prenumerationen är den som åtgärdsgruppen sparas.

6. Välj den **resursgruppen** i åtgärdsgruppen har sparats.

7. Definiera en lista med åtgärder genom att ge varje åtgärd:

    a. **Namnet**: Ange en unik identifierare för den här åtgärden.

    b. **Åtgärdstyp**: Välj SMS, e-post, webhook, Azure-app, ITSM eller Automation-Runbook.

    c. **Information om**: baserat på typen av Ange telefonnummer, e-postadress, webhook URI, Azure-app, ITSM anslutning eller Automation-runbook. Ange dessutom för ITSM åtgärd **arbetsobjekt** och andra fält ITSM-verktyget kräver. 

   > [!NOTE]
   > ITSM-åtgärden kräver en ITSM-anslutning. Lär dig hur du skapar en [ITSM anslutning](../log-analytics/log-analytics-itsmc-overview.md). ITSM åtgärden fungerar för närvarande endast för aktiviteten loggen aviseringar. Den här åtgärden är för närvarande en no-op för andra aviseringstyper.

8. Välj **OK** att skapa åtgärdsgruppen.

## <a name="manage-your-action-groups"></a>Hantera åtgärdsgrupper ##
När du skapar en grupp, är det visas i den **åtgärdsgrupper** avsnitt i den **övervakaren** bladet. Välj den grupp du vill hantera att:

* Lägga till, redigera eller ta bort åtgärder.
* Ta bort åtgärdsgruppen.

## <a name="next-steps"></a>Nästa steg ##
* Lär dig mer om [SMS Varna beteende](monitoring-sms-alert-behavior.md).  
* Få en [förståelse av aviseringen webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).  
* Lär dig mer om [ITSM koppling](../log-analytics/log-analytics-itsmc-overview.md)
* Lär dig mer om [hastighetsbegränsning](monitoring-alerts-rate-limiting.md) aviseringar för. 
* Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur du tar emot aviseringar.  
* Lär dig hur du [konfigurera aviseringar när ett meddelande om tjänstens hälsa är bokförd](monitoring-activity-log-alerts-on-service-notifications.md).
