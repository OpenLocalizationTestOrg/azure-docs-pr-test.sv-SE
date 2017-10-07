---
title: "aaaWhat är meddelanden om hälsostatus | Microsoft Docs"
description: "Meddelanden om hälsostatus kan du tooview meddelanden om hälsa publicera av Microsoft Azure."
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
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 6f2fe72154c3e80d85062655c49dd1799b718e3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-health-notifications"></a>Meddelanden om hälsostatus
## <a name="overview"></a>Översikt

Den här artikeln visar hur tooview hälsa tjänstmeddelanden med hello Azure-portalen.

Meddelanden om hälsostatus kan du tooview service hälsa meddelanden som publiceras av hello Azure-teamet som kan påverka hello resurser i din prenumeration. Dessa aviseringar är en underklass till aktiviteten logghändelser och finns även på hello aktivitet loggen bladet. Meddelanden om hälsostatus kan vara information eller tillämplig beroende på hello-klassen.

Det finns fem typer av meddelanden om hälsostatus:  

- **Åtgärd krävs:** från tid tootime kan vi upptäcker något annorlunda hända om ditt konto. Vi måste eventuellt toowork med du tooremedy detta. Vi skickar dig ett meddelande antingen med hello åtgärder måste tootake eller med information om hur toocontact Azure teknisk eller stöd för.  
- **Assisterad återställning:** en händelse har inträffat och tekniker har bekräftat att du fortfarande har effekt. Tekniker behöver toowork med dig direkt toobring services-toorestoration.  
- **Incident:** en tjänst som påverkar händelse för närvarande påverkar en eller flera av hello resurser i din prenumeration.  
- **Underhåll:** detta är ett meddelande informerar dig om en aktivitet för planerat underhåll som kan påverka en eller flera av hello resurser i din prenumeration.  
- **Information:** från tid tootime vi kan skicka meddelanden att en kommunicera tooyou om potentiella optimeringar som kan hjälpa att förbättra din resursutnyttjande.  
- **Säkerhet:** brådskande säkerhetsrelaterade information om din solution(s) som körs på Azure.

Varje meddelande för tjänsten hälsotillstånd hanterar information på hello räckvidd och konsekvenser tooyour resurser. Innehåller information:

Egenskapsnamn | Beskrivning
-------- | -----------
kanaler | Är en av hello följande värden: ”Admin”, ”åtgärden”
correlationId | Är vanligtvis ett GUID i hello-strängformat. Händelser med som tillhör toohello vanligtvis delar samma uber åtgärd hello samma correlationId.
eventDataId | Är hello Unik identifierare för en händelse
EventName | Är hello titeln på hello-händelse
nivå | Nivå av hello-händelse. En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”
resourceProviderName | Namnet på hello resource provider för hello påverkas resurs
resourceType| hello typ av resurs av hello påverkas resurs
subStatus | Vanligtvis hello HTTP-statuskoden hello motsvarande REST-anrop, men kan även innehålla andra strängar som beskriver en sådan, till exempel värdena vanliga: OK (HTTP-statuskod: 200), skapade (HTTP-statuskod: 201), godkända (HTTP-statuskod: 202), Nej innehåll (HTTP Statuskod: 204), felaktig begäran (HTTP-statuskod: 400) inte hittades (HTTP-statuskod: 404), konflikt (HTTP-statuskod: 409), internt serverfel (HTTP-statuskod: 500), tjänsten inte tillgänglig (HTTP-statuskod: 503), Gateway-Timeout (HTTP-statuskod: 504).
eventTimestamp | Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse.
submissionTimestamp |   Tidsstämpel när hello händelse blev tillgängliga för frågor.
subscriptionId | hello Azure-prenumeration som den här händelsen loggades
status | Sträng som beskriver hello status för hello-åtgärd. Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst.
operationName | Namnet på hello igen.
category | ”ServiceHealth”
resourceId | Resurs-id för hello påverkas resurs.
Properties.title | hello lokaliserade rubrik för den här kommunikationen. Engelska är standardspråk för hello.
Properties.Communication | hello lokaliserad information hello-kommunikation med HTML-kod. Engelska är hello som standard.
Properties.incidentType | Möjliga värden: AssistedRecovery, ActionRequired, Information, incidenter, underhåll, säkerhet
Properties.trackingId | Identifierar hello incident som den här händelsen är associerad med. Använd den här toocorrelate hello händelser relaterade tooan incidenten.
Properties.impactedServices | En ESC JSON-blob som beskriver hello tjänster och områden som påverkas av hello incident. En lista över tjänster, som har en ServiceName och en lista över ImpactedRegions, som har en RegionName.
Properties.defaultLanguageTitle | hello-kommunikation på engelska
Properties.defaultLanguageContent | hello-kommunikation på engelska som HTML-kod eller oformaterad text
Properties.Stage | Möjliga värden för AssistedRecovery, ActionRequired, Information, incidenter, säkerhet: är aktiv, löst. De är för underhåll: aktiv, planerad, InProgress, Avbruten, Rescheduled, löst, Slutför
Properties.communicationId | hello kommunikation den här händelsen är kopplat.


## <a name="viewing-your-service-health-notifications-in-hello-azure-portal"></a>Visa din tjänstmeddelanden hälsa i hello Azure-portalen
1.  I hello [portal](https://portal.azure.com), navigera toohello **övervakaren** service

    ![Övervaka](./media/monitoring-service-notifications/home-monitor.png)
2.  Klicka på hello **övervakaren** alternativet tooopen in hello övervakaren bladet. På det här bladet sammanförs alla dina övervakningsinställningar och -data till en sammanslagen vy. Öppnas toohello **aktivitetsloggen** avsnitt.

3.  Klicka på **tjänstmeddelanden** avsnitt

    ![Övervaka](./media/monitoring-service-notifications/service-health-summary.png)
4.  Klicka på någon av hello radobjekt tooview mer information

5. Klicka på hello **+ Lägg till aktivitet loggen avisering** åtgärden tooreceive meddelanden tooensure får du ett meddelande för framtida tjänstmeddelanden av den här typen. Mer om hur du konfigurerar aviseringar på tjänstmeddelanden toolearn [Klicka här](monitoring-activity-log-alerts-on-service-notifications.md)

## <a name="next-steps"></a>Nästa steg:
Ta emot [aviseringar när ett meddelande om tjänstens hälsa](monitoring-activity-log-alerts-on-service-notifications.md) skickas  
Lär dig mer om [aktivitet loggen aviseringar](monitoring-activity-log-alerts.md)
