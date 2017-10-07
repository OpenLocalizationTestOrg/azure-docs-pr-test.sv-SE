---
title: aaaAzure Mobile Engagement Troubleshooting Guide - Analytics
description: "Felsökning av problem med Analytics, övervakning, segmentering och instrumentpanelen i Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04a7020a-ad74-4491-be69-0bd574890029
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 69c6ff8f5c8540f8ba8b85b9ffec55acc59329fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Felsökningsguide för Analytics, övervakning, segmentering och instrumentpanelen problem
hello följande är möjliga problem som kan uppstå med hur Azure Mobile Engagement samlar in information om dina program, enheter och användare.

## <a name="missingdelayed-information"></a>Information saknas/fördröjd
### <a name="issue"></a>Problem
* Visas i som visas i Analytics, segmentering eller instrumentpanelen.
* Information saknas från övervakning.
* Information saknas från Analytics, segmentering eller instrumentpanelen.
* Träffa segmentering begränsar.

### <a name="causes"></a>Orsaker
* Du kan använda hello Analytics API övervakaren API och segment API toosee om några data du saknas från hello Användargränssnittet syns via hello API: er.
* Om hello Azure Mobile Engagement SDK inte är korrekt integrerad i din app kommer inte att kunna toosee informationen i hello Analytics, segmentering, övervakning och instrumentpaneler.
* Segment får inte vara ändras när de skapas, segment kan endast vara ”klonade” (kopieras) eller ”förstörs” (bort). Segment kan bara innehålla 10 kriterier.
* hello bästa sätt tootest saknas information från övervakning är toosetup en testenhet, avinstallera och installera om hello app på hello testenhet.
* Informationen uppdateras var 24: e timme för Analytics, segmentering eller instrumentpaneler.
* Informationen i nya segment visas inte förrän 24 timmar efter att de skapas även om hello segment baserat på information som tidigare.
* Filtrera data i hello UI analytics visas alla exempel på den här typen oavsett hello version av appen (t.ex.) ”Kraschar” filtrerade efter namn visas från version 1 och version 2 av appen).
* hello baseras tidsperiod för Analytics på hello datum från hello användarnas Enhetsinställningar, så att en användare vars telefonen har hello datum felaktigt som kan visas i hello fel tidsperiod.
* Inga data loggas när du använder hello-knappen på serversidan för ”test” push-meddelanden, data loggas endast för verkliga push-kampanjer.

## <a name="cant-locate-items-in-ui"></a>Det går inte att hitta objekt i Användargränssnittet
### <a name="issue"></a>Problem
* Kan inte skapa segment baserat på vissa inbyggda eller anpassade appinfo tagga kriterier.
* Kan inte hitta vissa inbyggda eller anpassade appinfo tagga kriterier i Analytics, övervakning och instrumentpaneler.
* Det går inte att tolka hello data i Analytics, övervakning, segmentering och instrumentpaneler.

### <a name="causes"></a>Orsaker
* Vissa inbyggda objekt och appinfo taggar är bara tillgängliga som push-villkor, men kanske inte läggas till tooa segment eller synliga från Analytics, övervakning eller instrumentpanelen. 
* För inbyggda objekt och appinfo taggar som inte går att lägga till tooa segment, behöver du toosetup lista över villkor i varje kampanj tooperform hello samma fungera som mål baserat på ett segment som mål.
* Se hello snabbmenyer i hello Analytics, övervakning, segmentering och instrumentpaneler avsnitt i hello Azure Mobile Engagement Användargränssnittet för mer hjälp och hur tooinformation.

## <a name="crash-troubleshooting"></a>Krasch felsökning
### <a name="issue"></a>Problem
* Programmet kraschar som visas i Analytics, övervakning eller instrumentpanelen.

### <a name="causes"></a>Orsaker
* tootroubleshoot programmet kraschar ses i Analytics, övervakning eller instrumentpanel gör att toocheck hello viktig information om kända problem med tidigare versioner av hello SDK.
* toofurther felsöka program kraschar utför en händelse från en testenhet med ditt program installerat och leta upp din enhets-ID under hello ”övervaka – händelser” hello Azure Mobile Engagement-Användargränssnittet. Sedan utföra hello händelse som gör att din ansökan toocrash och leta upp ytterligare information i hello ”övervakaren – krascher” avsnittet av hello Azure Mobile Engagement-Användargränssnittet. 

