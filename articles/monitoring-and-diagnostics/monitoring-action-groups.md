---
title: "aaaCreate och hantera åtgärdsgrupper i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate och hantera åtgärdsgrupper i hello Azure-portalen."
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
ms.openlocfilehash: 97e0b22bea7787fff6856f895a7e6256c177efd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-action-groups-in-hello-azure-portal"></a>Skapa och hantera åtgärdsgrupper i hello Azure-portalen
## <a name="overview"></a>Översikt ##
Den här artikeln beskrivs hur du toocreate och hantera åtgärdsgrupper i hello Azure-portalen.

Du kan konfigurera en lista med åtgärder med åtgärdsgrupper. Dessa grupper kan sedan användas när du definierar aktiviteten loggen aviseringar. Dessa grupper kan sedan återanvändas av varje aktivitet loggen aviseringen som du definierar, se till att hello samma åtgärder vidtas för varje gång hello loggen varning utlöses.

En grupp kan ha upp too10 för varje åtgärdstyp av. Varje åtgärd består av hello följande egenskaper:

* **Namnet**: en unik identifierare inom hello grupp.  
* **Åtgärdstyp**: skicka ett SMS, skicka ett e-postmeddelande eller anropa en webhook.  
* **Information om**: hello motsvarande telefonnummer, e-postadress eller webhook URI.

Mer information om hur toouse Azure Resource Manager-mallar tooconfigure åtgärdsgrupper, se [åtgärd grupp Resource Manager-mallar](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-hello-azure-portal"></a>Skapa en grupp med hjälp av hello Azure-portalen ##
1. I hello [portal](https://portal.azure.com)väljer **övervakaren**. Hej **övervakaren** bladet konsoliderar alla dina övervakning inställningar och data i en vy.

    ![Hej ”övervakningstjänsten”](./media/monitoring-action-groups/home-monitor.png)
2. I hello **aktivitetsloggen** väljer **åtgärdsgrupper**.

    ![Hej ”åtgärdsgrupper” fliken](./media/monitoring-action-groups/action-groups-blade.png)
3. Välj **Lägg till grupp**, och hello fält fylls i.

    ![Hej ”Lägg till grupp” kommando](./media/monitoring-action-groups/add-action-group.png)
4. Ange ett namn i hello **åtgärd gruppnamn** och ange ett namn i hello **kortnamnet** rutan. hello kort namn används i stället för en fullständig åtgärd gruppnamn när meddelanden som skickas med den här gruppen.

      ![dialogrutan hello Lägg till grupp ”](./media/monitoring-action-groups/action-group-define.png)

5. Hej **prenumeration** rutan autofills med din aktuella prenumeration. Den här prenumerationen är hello något i vilken åtgärd hello-grupp har sparats.

6. Välj hello **resursgruppen** där hello åtgärd spara gruppen.

7. Definiera en lista med åtgärder genom att ge varje åtgärd:

    a. **Namnet**: Ange en unik identifierare för den här åtgärden.

    b. **Åtgärdstyp**: Välj SMS, e-post eller webhooken.

    c. **Information om**: baserat på hello åtgärdstyp, ange ett telefonnummer, e-postadress eller webhook URI.

8. Välj **OK** toocreate hello grupp.

## <a name="manage-your-action-groups"></a>Hantera åtgärdsgrupper ##
När du har skapat en grupp, den är synlig i hello **åtgärdsgrupper** avsnitt i hello **övervakaren** bladet. Välj hello åtgärd du vill toomanage till:

* Lägga till, redigera eller ta bort åtgärder.
* Ta bort hello grupp.

## <a name="next-steps"></a>Nästa steg ##
* Lär dig mer om [SMS Varna beteende](monitoring-sms-alert-behavior.md).  
* Få en [förståelse av hello avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).  
* Lär dig mer om [hastighetsbegränsning](monitoring-alerts-rate-limiting.md) aviseringar för. 
* Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur tooreceive aviseringar.  
* Lär dig hur för[konfigurera aviseringar när ett meddelande om tjänstens hälsa är bokförd](monitoring-activity-log-alerts-on-service-notifications.md).
