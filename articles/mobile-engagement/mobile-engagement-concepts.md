---
title: koncept i aaaMobile Engagement | Microsoft Docs
description: Koncept i Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8d19abd1-0a6c-4772-9fa5-5e99980ac5da
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 5aa7f28c00cd641a36a6e040c6b13d802ea6ae41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-concepts"></a>Koncept i Azure Mobile Engagement
Mobile Engagement har definierat några koncept gemensamt tooall stöds plattformar. I den här artikeln beskrivs koncepten kortfattat.

Den här artikeln är ett bra ställe att börja om du är ny tooMobile Engagement. Kontrollera också att tooread hello dokumentationen specifika toohello plattform som du använder, eftersom den vidareutvecklar resonemangen kring hello begrepp som beskrivs i denna artikel med mer information och exempel samt eventuella begränsningar.

## <a name="devices-and-users"></a>Enheter och användare
Mobile Engagement identifierar användare genom att generera ett unikt ID för varje enhet. Detta ID kallas enhets-ID för hello (eller `deviceid`). Det genereras så att alla program som körs av hello samma enhet resursen hello samma enhets-ID.

Implicit, innebär det att Mobile Engagement kopplar en enhet toobelong tooexactly en användare och användare och enhet är alltså likvärdiga koncept.

## <a name="sessions-and-activities"></a>Sessioner och aktiviteter
En session är användningen av hello-program som utförs av en användare från hello tid hello användaren börjar använda, tills hello upphör.

En aktivitet är användningen av en viss underordnad del av hello-program som utförs av en användare (det är vanligtvis en skärm, men det kan vara något lämpligt toohello program).

En användare kan bara utföra en aktivitet i taget.

En aktivitet identifieras med ett namn (begränsat too64 tecken) och kan även välja att bädda in extra data (hello högst 1 024 byte).

Sessioner beräknas automatiskt utifrån hello sekvensen av aktiviteter som utförs av användare. Sessionen startar när hello användaren startar den första aktiviteten och slutar när användarens sista aktivitet är klar. Det innebär att en session inte behöver toobe startas eller stoppas. Istället är det aktiviteterna som uttryckligen startas eller stoppas. Om ingen aktivitet rapporteras så rapporteras heller ingen session.

## <a name="events"></a>Händelser
Händelser är används tooreport omedelbara åtgärder (till exempel knappen är nedtryckt eller artiklar som läses av användare).

En händelse kan vara relaterade toohello aktuell session tooa köra jobb, eller det kan vara en fristående händelse.

En händelse identifieras med ett namn (begränsat too64 tecken) och kan även välja att bädda in extra data (hello högst 1 024 byte).

## <a name="error"></a>Fel
Fel är att använda tooreport problem som identifieras av programmet hello (till exempel felaktiga användaråtgärder eller misslyckade API-anrop).

Ett fel kan vara relaterade toohello aktuell session tooa köra jobb, eller det kan vara ett fristående fel.

Ett fel identifieras med ett namn (begränsat too64 tecken) och kan även välja att bädda in extra data (hello högst 1 024 byte).

## <a name="job"></a>Jobb
Jobb är används tooreport åtgärder som har en varaktighet (som varaktighet för API-anrop, visa för annonser, varaktighet för bakgrundsaktiviteter eller varaktighet för användaråtgärder).

Ett jobb är inte relaterade tooa session eftersom en uppgift kan köras i bakgrunden hello utan någon användarinteraktion.

Ett jobb identifieras med ett namn (begränsat too64 tecken) och kan även välja att bädda in extra data (hello högst 1 024 byte).

## <a name="crash"></a>Krascher
Krascher utfärdas automatiskt av hello Mobile Engagement SDK tooreport program fel där problem som inte identifieras av programmet hello gör det krascha.

## <a name="application-information"></a>Programinformation
Programinformation (eller appinfo) är används tootag användare, det vill säga tooassociate vissa data toohello använder ett program (detta är liknande tooweb cookies, förutom att appinfo lagras på hello på serversidan på plattformen för hello Azure Mobile Engagement).

Appinfo kan registreras med hjälp av hello Mobile Engagement SDK API eller genom att använda hello Mobile Engagement-plattformen Device API.

Appinfo utgörs av en nyckel/värde-par kopplade tooa enhet. hello nyckel är hello namn hello appinfon (begränsat too64 ASCII-bokstäver [a-öA-Ö], siffror [0-9] och understreck [_]). hello-värdet (begränsat too1024 tecken) kan vara en sträng, heltal, datum (åååå-MM-dd) eller booleskt värde (SANT eller FALSKT).

Valfritt antal appinfo kan vara associerade tooa enhet inom hello gränser som definierats av hello Mobile Engagement prisnivå villkoren. För en nyckel Mobile Engagement håller bara reda på senaste värdeuppsättningen för hello (ingen historik). Ange eller ändra hello-värdet för en appinfo tvingas Mobile Engagement toore-utvärdera målgruppen villkor som anges på den här appen info (eventuella) vilket innebär att appinfo kan vara används tootrigger realtid push-meddelanden.

## <a name="extra-data"></a>Extra data
Extra data (eller extra information) utgörs av diverse uppgifter som kan vara anslutna tooevents, fel, aktiviteter och jobb.

Extra data är strukturerade på samma sätt tooJSON objekt: de består av ett träd med nyckel/värde-par. Nycklarna är begränsade too64 ASCII-bokstäver [a-öA-Ö], siffror [0-9] och understreck [_]) och hello tillägg totala storlek är begränsad too1024 tecken (efter kodning i JSON av hello Mobile Engagement SDK).

hello hela trädet med nyckel-/ värdepar lagras som ett JSON-objekt. Dock endast hello första nivån av nycklar/värden är uppdelade toobe direkt tillgänglig toosome avancerade funktioner såsom segment (till exempel, du kan enkelt definiera ett segment som kallas ”SciFi-fans” som består av alla användare som har skickat minst 10 gånger hello händelse med namnet ”visat_innehåll” med hello extra nyckeln ”innehållstyp” set toohello värdet ”scifi” i hello senaste månaden). Därför rekommenderas toosend endast tillägg av enkla listor med nyckel/värde-par med skalära värden (till exempel strängar, datum, heltal eller booleska).

## <a name="next-steps"></a>Nästa steg
* [Översikt över Windows Universal SDK för Azure Mobile Engagement](mobile-engagement-windows-store-sdk-overview.md)
* [Översikt över Windows Phone Silverlight SDK för Azure Mobile Engagement](mobile-engagement-windows-phone-sdk-overview.md)
* [iOS SDK för Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
* [Android SDK för Azure Mobile Engagement](mobile-engagement-android-sdk-overview.md)

