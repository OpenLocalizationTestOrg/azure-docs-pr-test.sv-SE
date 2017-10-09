---
title: "aaaHandling säkerhetsaviseringar i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet hjälper dig att toouse Azure Security Center funktioner toohandle säkerhetsincidenter."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: e8feb669-8f30-49eb-ba38-046edf3f9656
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: edb911c298a2ce93cd0ea5b22ce002005040090f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="handling-security-incidents-in-azure-security-center"></a>Hantera säkerhetsincidenter i Azure Security Center
Triaging och undersöker säkerhetsaviseringar kan ta lång tid för även hello mest skicklig säkerhetsanalytiker och många är det svårt tooeven vet var toobegin. Med hjälp av [analytics](security-center-detection-capabilities.md) tooconnect hello information mellan olika [säkerhetsaviseringar](security-center-managing-and-responding-alerts.md), Security Center kan förse dig med en enda vy av en attack kampanj och alla hello relaterade aviseringar – du kan snabbt förstå vilka åtgärder hello angripare tog och vilka resurser har påverkas.

Det här dokumentet beskrivs hur toouse säkerhet varnar kapaciteten i Security Center tooassist hantering av säkerhetsincidenter.

## <a name="what-is-a-security-incident"></a>Vad är en säkerhetsincident?
I Security Center är en säkerhetsincident en sammanställning av alla aviseringar för en resurs som överensstämmer med särskilda [händelsekedjemönster](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/). Incidenter visas i hello [säkerhetsaviseringar](security-center-managing-and-responding-alerts.md) panelen och bladet. En Incident avslöja hello lista över relaterade aviseringar, vilket gör att du tooobtain mer information om varje förekomst.

## <a name="managing-security-incidents"></a>Hantera säkerhetsincidenter
Du kan granska din aktuella säkerhetsincidenter genom att titta på hello säkerhetsrutan aviseringar. Komma åt hello Azure-portalen och gör hello nedan toosee mer information om varje säkerhetsincident:

1. På instrumentpanelen för hello Security Center ser du hello **säkerhetsaviseringar** panelen.

    ![Panelen Säkerhetsaviseringar i Security Center](./media/security-center-incident/security-center-incident-fig1.png)

2. Klicka på den här panelen tooexpand och om en säkerhetsincident har identifierats, visas den under hello säkerhet aviseringar diagram som visas nedan:

    ![Säkerhetsincident](./media/security-center-incident/security-center-incident-fig2.png)

3. Observera att hello säkerhet incidentbeskrivning har olika ikoner jämfört med tooother aviseringar. Klicka på den tooview mer information om den här incidenten.

    ![Säkerhetsincident](./media/security-center-incident/security-center-incident-fig3.png)

4. På hello **incident** bladet visas mer information om den här säkerhetsincident som innehåller en fullständig beskrivning, dess allvarlighetsgrad (som i det här fallet är hög), det aktuella tillståndet (i det här fallet är det fortfarande *active*, vilket innebär att hello användaren har inte tagit en åtgärd tooit – kan du göra det genom att högerklicka på hello incident i hello **säkerhetsaviseringar** bladet), hello angripna resursen (i det här fallet *VM1*) hello steg för hello incidenten och i hello längst ned i fönstret har du hello aviseringar som ingår i den här incidenten. Om du vill tooobtain mer information om varje avisering, öppnas Klicka på den och ett nytt blad som visas nedan:

    ![Säkerhetsincident](./media/security-center-incident/security-center-incident-fig4.png)

hello information om det här bladet varierar bl.a toohello avisering. Läs [hantering och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) för mer information om hur toomanage aviseringarna. Viktigt att tänka på för den här funktionen:

* Ett nytt filter kan du toocustomize din tooIncident Visa endast aviseringar endast eller båda.
* hello samma avisering kan finnas som en del av en Incident (om tillämpligt), samt toobe visas som en fristående avisering.

## <a name="see-also"></a>Se även
I det här dokumentet du har lärt dig hur toouse hello säkerhet incident kapaciteten i Security Center. toolearn mer om Security Center finns hello följande:

* [Hantera och åtgärda toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Identifieringsfunktioner i Azure Security Center](security-center-detection-capabilities.md)
* [Planerings- och bruksanvisning för Azure Security Center](security-center-planning-and-operations-guide.md)
* [Hantera och åtgärda toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Vanliga frågor om Azure Security Center](security-center-faq.md)--finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/): Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.
