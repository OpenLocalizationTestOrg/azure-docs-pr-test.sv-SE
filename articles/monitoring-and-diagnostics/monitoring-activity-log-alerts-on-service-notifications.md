---
title: "aaaReceive aktivitet Logga varningar på tjänstmeddelanden | Microsoft Docs"
description: "Håll dig informerad via SMS, e-post eller webhook när Azure-tjänsten inträffar."
author: johnkemnetz
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
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: dd35e8f39d2a522efdae4dfed20779c992c1dd27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Skapa aktivitet loggen aviseringar på tjänstmeddelanden
## <a name="overview"></a>Översikt
Den här artikeln visar hur tooset in aktivitetsloggen aviseringar för meddelanden om hälsostatus med hjälp av hello Azure-portalen.  

Du kan ta emot en avisering när Azure skickar tjänsten hälsa meddelanden tooyour Azure-prenumeration. Du kan konfigurera hello avisering baserat på:

- hello klass av tjänstens hälsa meddelande (incident, underhåll, information, etc.).
- hello tjänster påverkas.
- Hej region(erna) påverkas.
- hello status för hello-meddelande (aktiv kontra löst).
- hello andelen hello meddelanden (information, varning, fel).

Du kan också konfigurera som hello avisering ska skickas till:

- Välj en befintlig grupp.
- Skapa en ny grupp (som kan användas för framtida aviseringar).

toolearn mer om åtgärdsgrupper finns [skapa och hantera åtgärdsgrupper](monitoring-action-groups.md).

Mer information om hur tooconfigure service hälsa postmeddelanden aviseringar med hjälp av Azure Resource Manager-mallar finns [Resource Manager-mallar](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-hello-azure-portal"></a>Skapa en avisering på ett meddelande som tjänsten hälsa för en ny grupp med hjälp av hello Azure-portalen
1. I hello [portal](https://portal.azure.com)väljer **övervakaren**.

    ![Hej ”övervakningstjänsten”](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. I hello **aktivitetsloggen** väljer **aviseringar**.

    ![Hej ”” aviseringsfliken](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. Välj **Lägg till aktivitet loggen avisering**, och hello fält fylls i.

    ![Hej ”Lägg till loggen varning” kommando](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. Ange ett namn i hello **aktivitet avisering loggnamn** rutan och ange en **beskrivning**.

    ![Hej ”Lägg till loggen varning” dialogrutan](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. Hej **prenumeration** rutan autofills med din aktuella prenumeration. Den här prenumerationen har använt toosave hello loggen varning. hello avisering resursen är distribuerade toothis prenumeration och Övervakare händelser i hello aktivitetsloggen för den.

6. Välj hello **resursgruppen** i vilka hello avisering resursen har skapats. Det är inte hello resursgruppen som övervakas av hello aviseringen. Det är hello resursgruppen där hello avisering resursen finns.

7. I hello **händelsekategori** väljer **Hälsotjänst**. Alternativt, Välj hello **Service**, **Region**, **typen**, **Status**, och **nivå** för tjänsten meddelanden om hälsostatus som du vill tooreceive.

8. Under **avisering via**väljer hello **ny** åtgärdsknappen för gruppen. Ange ett namn i hello **åtgärd gruppnamn** och ange ett namn i hello **kortnamnet** rutan. hello kortnamnet refereras i hello-meddelanden som skickas när den här aviseringen utlöses.

9. Definiera en lista över mottagare genom att tillhandahålla hello mottagare:

    a. **Namnet**: Ange hello mottagare, alias eller identifierare.

    b. **Åtgärdstyp**: Välj SMS, e-post eller webhooken.

    c. **Information om**: baserat på hello åtgärdstyp valt, ange ett telefonnummer, e-postadress eller webhook URI.

10. Välj **OK** toocreate hello avisering.

Inom några minuter hello aviseringen är aktiv och börjar tootrigger baserat på hello villkor som du angav när du skapar.

Mer information om hello webhook-schemat för aktiviteten loggen aviseringar finns [Webhooks för Azure aktiviteten Logga varningar](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>hello åtgärdsgrupp som definierats i de här stegen är återanvändbara som en befintlig grupp för alla framtida definitioner för aviseringen.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-hello-azure-portal"></a>Skapa en avisering på ett meddelande med tjänstens hälsa för en befintlig grupp med hello Azure-portalen

1. Följ steg 1 till 7 i hello föregående avsnitt toocreate health service-meddelande. 

2. Under **avisering via**väljer hello **befintliga** åtgärdsknappen för gruppen. Välj grupp för hello lämplig åtgärd.

3. Välj **OK** toocreate hello avisering.

Inom några minuter hello aviseringen är aktiv och börjar tootrigger baserat på hello villkor som du angav när du skapar.

## <a name="manage-your-alerts"></a>Hantera aviseringar

När du skapar en avisering, är det visas i hello **aviseringar** avsnitt i hello **övervakaren** bladet. Välj hello avisering du vill toomanage till:

* Redigera den.
* Ta bort den.
* Inaktivera eller aktivera den, om du vill stoppa tootemporarily eller återuppta tar emot meddelanden om hello avisering.

## <a name="next-steps"></a>Nästa steg
- Lär dig mer om [tjänsten meddelanden om hälsostatus](monitoring-service-notifications.md).
- Lär dig mer om [meddelande hastighetsbegränsning](monitoring-alerts-rate-limiting.md).
- Granska hello [avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).
- Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur tooreceive aviseringar. 
- Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md).
