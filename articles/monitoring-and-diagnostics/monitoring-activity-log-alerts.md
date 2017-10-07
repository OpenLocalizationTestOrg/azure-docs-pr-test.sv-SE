---
title: aaaCreate aktivitet loggen aviseringar | Microsoft Docs
description: "Ett meddelande via SMS, webhook och e-post när vissa händelser inträffar i hello aktivitetsloggen."
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
ms.date: 08/03/2017
ms.author: johnkem
ms.openlocfilehash: ba0716cc12a0b3a0024ee5562a025f3f153f8982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts"></a>Skapa aktivitet Logga varningar

## <a name="overview"></a>Översikt
Aktiviteten loggen aviseringar är aviseringar som aktiverar när en ny aktivitet logga en händelse inträffar som matchar hello villkor som anges i hello avisering. De är Azure-resurser, så att de kan skapas med hjälp av en Azure Resource Manager-mall. De kan också skapa, uppdatera, eller tas bort i hello Azure-portalen. Den här artikeln introducerar hello koncepten bakom aktivitet loggen aviseringar. Den sedan visar hur toouse hello Azure portal tooset upp en avisering på aktiviteten logghändelser.

Normalt skapar du aktiviteten loggen aviseringar tooreceive meddelanden när:

* Vissa ändringar sker på resurser i Azure-prenumeration, ofta begränsade tooparticular resursgrupper eller resurser. Exempelvis kanske du vill toobe meddelas när en virtuell dator i myProductionResourceGroup tas bort. Eller så kanske du vill toobe meddelas om några nya roller har tilldelats tooa användare i din prenumeration.
* En service hälsa händelse inträffar. Tjänstens hälsa händelser innehåller meddelanden om incidenter och underhållshändelser som gäller tooresources i din prenumeration.

I båda fallen övervakar en aktivitet loggen avisering bara för händelser i hello prenumeration i vilka hello avisering skapas.

Du kan konfigurera en aktivitet loggen avisering baserat på någon översta egenskap i hello JSON-objekt för en aktivitet händelselogg. Dock visar hello portal hello de vanligaste alternativen:

- **Kategori**: administrativa, tjänsten hälsa och Autoskala rekommendation. Mer information finns i [översikt över hello Azure-aktivitetsloggen](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). toolearn mer om tjänstens hälsa händelser, se [varningar aktivitet loggen på tjänstmeddelanden](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Resursgrupp**
- **Resurs**
- **Resurstyp**
- **Åtgärdsnamnet**: hello åtgärdsnamn för rollbaserad åtkomstkontroll i Resource Manager.
- **Nivå**: hello allvarlighetsgraden hello-händelse (utförlig information, varning, fel eller kritiskt).
- **Status för**: hello status för hello händelse, startas normalt, misslyckades eller lyckades.
- **Händelse som initieras av**: kallas även hello ”anroparen”. hello e-postadress eller Azure Active Directory identifierare för hello-användaren som utförde åtgärden hello.

>[!NOTE]
>Du måste ange minst två av hello före kriterier i aviseringen, med en som hello kategori. Du kan inte skapa en avisering som aktiveras varje gång en händelse skapas i hello aktivitetsloggar.
>
>

När en aktivitet loggen avisering aktiveras den använder en åtgärd gruppera toogenerate åtgärder eller meddelanden. En grupp är en återanvändbar uppsättning notification mottagare, till exempel e-postadresser, webhook URL: er eller SMS telefonnummer. hello mottagare kan refereras från flera aviseringar toocentralize och gruppera dina aviseringskanaler. När du definierar aviseringen aktivitet loggen har du två alternativ. Du kan:

* Använd en befintlig grupp i din logg varning. 
* Skapa en ny grupp. 

toolearn mer om åtgärdsgrupper finns [skapa och hantera åtgärdsgrupper i hello Azure-portalen](monitoring-action-groups.md).

toolearn mer om hälsa tjänstmeddelanden, se [varningar aktivitet loggen på meddelanden om hälsostatus](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-hello-azure-portal"></a>Skapa en avisering på en aktivitet logga en händelse med en ny grupp med hello Azure-portalen
1. I hello [portal](https://portal.azure.com)väljer **övervakaren**.

    ![Hej ”övervakningstjänsten”](./media/monitoring-activity-log-alerts/home-monitor.png)
2. I hello **aktivitetsloggen** väljer **aviseringar**.

    ![Hej ”” aviseringsfliken](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. Välj **Lägg till aktivitet loggen avisering**, och hello fält fylls i.

4. Ange ett namn i hello **aktivitet avisering loggnamn** och välj en **beskrivning**.

    ![Hej ”Lägg till loggen varning” kommando](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. Hej **prenumeration** rutan autofills med din aktuella prenumeration. Den här prenumerationen är hello något i vilken åtgärd hello-grupp har sparats. hello avisering resursen är distribuerade toothis prenumeration och Övervakare aktivitet logghändelser från den.

    ![Hej ”Lägg till loggen varning” dialogrutan](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. Välj hello **resursgruppen** i vilka hello avisering resursen har skapats. Detta är inte hello resursgruppen som övervakas av hello aviseringen. Det är hello resursgruppen där hello avisering resursen finns.

7. Du kan också välja en **händelsekategori** toomodify hello ytterligare filter som visas. För administrativa händelser hello filtren är **resursgruppen**, **resurs**, **resurstypen**, **åtgärdsnamn**, **Nivå**, **Status**, och **händelse som initieras av**. Dessa värden identifiera vilka händelser som bör du övervaka den här aviseringen.

    >[!NOTE]
    >Du måste ange minst en av hello före kriterier i aviseringen. Du kan inte skapa en avisering som aktiveras varje gång en händelse skapas i hello aktivitetsloggar.
    >
    >

8. Ange ett namn i hello **åtgärd gruppnamn** och ange ett namn i hello **kortnamnet** rutan. hello kort namn används i stället för en fullständig åtgärd gruppnamn när meddelanden som skickas med den här gruppen.

9.  Definiera en lista med åtgärder genom att tillhandahålla hello åtgärd:

    a. **Namnet**: Ange hello åtgärdens namn, alias eller identifierare.

    b. **Åtgärdstyp**: Välj SMS, e-post eller webhooken.

    c. **Information om**: baserat på hello åtgärdstyp, ange ett telefonnummer, e-postadress eller webhook URI.

10. Välj **OK** toocreate hello avisering.

hello avisering tar några minuter toofully sprida och sedan aktiveras. Den startar när nya händelser hello avisering matchningsvillkor.

Mer information finns i [förstå hello webhook schema som används i aktiviteten Logga varningar](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>hello åtgärdsgrupp som definierats i de här stegen är återanvändbara som en befintlig grupp för alla framtida definitioner för aviseringen.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-hello-azure-portal"></a>Skapa en avisering på en aktivitet logga en händelse för en befintlig grupp med hello Azure-portalen
1. Följ steg 1 till 7 i hello föregående avsnitt toocreate aktivitet loggen aviseringen.

2. Under **meddela**väljer hello **befintliga** åtgärdsknappen för gruppen. Välj en befintlig grupp hello-listan.

3. Välj **OK** toocreate hello avisering.

hello avisering tar några minuter toofully sprida och sedan aktiveras. Den startar när nya händelser hello avisering matchningsvillkor.

## <a name="manage-your-alerts"></a>Hantera aviseringar

När du skapar en avisering är det synliga under hello aviseringar i hello övervakaren bladet. Välj hello avisering du vill toomanage till:

* Redigera den.
* Ta bort den.
* Inaktivera eller aktivera den, om du vill stoppa tootemporarily eller återuppta tar emot meddelanden om hello avisering.

## <a name="next-steps"></a>Nästa steg
- Hämta en [översikt över aviseringar](monitoring-overview-alerts.md).
- Lär dig mer om [meddelande hastighetsbegränsning](monitoring-alerts-rate-limiting.md).
- Granska hello [avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).
- Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md).  
- Lär dig mer om [tjänsten meddelanden om hälsostatus](monitoring-service-notifications.md).
- Skapa en [aktivitet logga avisering toomonitor alla Autoskala motorn åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Skapa en [aktivitet logga avisering toomonitor alla misslyckade Autoskala skala-/ skalbar åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
