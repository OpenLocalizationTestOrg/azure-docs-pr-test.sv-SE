---
title: 'aaaAzure API: er Mobile Engagement Web SDK | Microsoft Docs'
description: "Hej senaste uppdateringarna och procedurer för hello Web SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8a87d5ac-d8b7-4a0d-bdee-414dbcc561b2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: ec1261d6ad573b8c3ad6d5f616ab7bbe560d6fe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-mobile-engagement-api-in-a-web-application"></a>Använd hello Azure Mobile Engagement API i ett webbprogram
Det här dokumentet är ett tillägg toohello dokument som anger hur för[Mobile Engagement ska integreras i ett webbprogram](mobile-engagement-web-integrate-engagement.md). Det ger detaljerad information om hur toouse hello Azure Mobile Engagement API tooreport programstatistik.

hello Mobile Engagement API tillhandahålls av hello `engagement.agent` objekt. hello Azure Mobile Engagement Web SDK alias är standard `engagement`. Du kan ändra detta alias från hello SDK-konfigurationen.

## <a name="mobile-engagement-concepts"></a>Koncept i Mobile Engagement
hello följande delar förfina vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md) för hello webbplattform.

### <a name="session-and-activity"></a>`Session` och `Activity`
Om hello användaren är inaktiv under mer än ett par sekunder mellan två aktiviteter, är hello användarens sekvensen av aktiviteter uppdelad i två olika sessioner. Dessa några sekunder kallas hello-sessions Time-out.

Om ditt webbprogram inte deklarera hello slutet av användaraktiviteter ensamt (genom att anropa hello `engagement.agent.endActivity` funktionen), hello Mobile Engagement server upphör automatiskt hello användarsession inom tre minuter efter hello appen på sidan är stängd. Detta kallas hello server sessions Time-out.

### `Crash`
Automatisk rapporter om undantagsfel utan felhantering JavaScript skapas inte som standard. Dock kan du rapportera krascher manuellt med hjälp av hello `sendCrash` fungera (se avsnittet hello reporting krascher).

## <a name="reporting-activities"></a>Rapportering
Rapportering av användaraktivitet inkluderar när en användare startar en ny aktivitet och när hello användaren slutar hello aktuella aktiviteten.

### <a name="user-starts-a-new-activity"></a>Användaren startar en ny aktivitet
    engagement.agent.startActivity("MyUserActivity");

Du behöver toocall `startActivity()` varje gång användaraktivitet ändras. hello första anropet toothis funktionen startar en ny session.

### <a name="user-ends-hello-current-activity"></a>Användaren slutar hello aktuell aktivitet
    engagement.agent.endActivity();

Du behöver toocall `endActivity()` minst en gång när hello användaren är klar deras senaste aktivitet. Det informerar hello Mobile Engagement Web SDK hello användare är för närvarande inaktiv, och att hello användarsession måste toobe stängs när hello sessionstidsgränsen upphör att gälla. Om du anropar `startActivity()` innan hello sessionstidsgränsen upphör att gälla hello sessionen är helt enkelt återupptas.

Eftersom det finns ingen tillförlitlig anrop för när hello navigator fönstret stängs, är det ofta svårt eller omöjligt toocatch hello slutet av användaraktiviteter inuti en webbmiljö. Som är varför hello Mobile Engagement server upphör automatiskt hello användarsession inom tre minuter efter hello appen på sidan är stängd.

## <a name="reporting-events"></a>Rapporteringshändelser
Rapportering av händelser som täcker Sessionshändelser och fristående händelser.

### <a name="session-events"></a>Sessionshändelser
Sessionshändelser är oftast används tooreport hello åtgärder som utförs av en användare under hello användarens session.

**Exempel utan extra data:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Exempel med extra data:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Fristående händelser
Till skillnad från Sessionshändelser, kan det ske fristående händelser utanför hello kontexten för en session.

För att använda ``engagement.agent.sendEvent`` i stället för ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Rapporterat fel
Rapportering om felen omfattar session fel och fristående fel.

### <a name="session-errors"></a>Sessionen fel
Sessionen felen är oftast används tooreport hello fel som kan påverkar hello användaren under hello användarsession.

**Exempel utan extra data:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Exempel med extra data:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Fristående fel
Till skillnad från sessionen fel inträffa fristående fel utanför hello kontexten för en session.

För att använda `engagement.agent.sendError` i stället för `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Rapportering av jobb
Rapportering av jobb omfattar rapportera fel och händelser som inträffar när ett jobb och rapportering av krascher.

**Exempel:**

Om du vill toomonitor en begäran om AJAX använder hello följande:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Rapporterat fel under ett jobb
Fel kan vara relaterade tooa köra jobb i stället för toohello aktuella användarsessionen.

**Exempel:**

Om du vill tooreport ett fel om en begäran om AJAX misslyckas:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Rapporteringshändelser under ett jobb
Händelser kan vara relaterade tooa köra jobb i stället för toohello aktuella användarsessionen, tack vare toohello `engagement.agent.sendJobEvent` funktion.

Den här funktionen fungerar på samma sätt som `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Rapportering krascher
Använd hello `sendCrash` funktionen tooreport kraschar manuellt.

Hej `crashid` argumentet är en sträng som identifierar hello typ av krascher.
Hej `crash` argumentet är vanligtvis hello stackspårning hello havererade som en sträng.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Extra parametrar
Du kan koppla godtyckliga data tooan händelse, fel, aktivitet eller jobb.

hello data kan vara ett JSON-objekt (men inte en matris eller primitiv typ).

**Exempel:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Begränsningar
Gränser som gäller tooextra parametrar har hello områden i reguljära uttryck för nycklar, värdetyper och storlek.

#### <a name="keys"></a>Nycklar
Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:

    ^[a-zA-Z][a-zA-Z_0-9]*

Detta innebär att nycklar måste börja med minst en bokstav följt av bokstäver, siffror eller understreck (\_).

#### <a name="values"></a>Värden
Värden är begränsad toostring, antal och typer av boolesk.

#### <a name="size"></a>Storlek
Tillägg är begränsad too1, 024 tecken per anrop (efter hello Mobile Engagement Web SDK kodar den i JSON).

## <a name="reporting-application-information"></a>Rapportering programinformation
Du kan manuellt rapportera spåra information (eller programspecifik information) med hjälp av hello `sendAppInfo()` funktion.

Observera att denna information kan skickas inkrementellt. Endast hello senaste värdet för en viss nyckel sparas för en specifik enhet.

Du kan använda en JSON-objekt tooabstract programinformation som händelsen tillägg. Observera att matriser eller underordnade objekt behandlas som flat strängar (med JSON-serialisering).

**Exempel:**

Här följer ett exempel på koden för att skicka hello användarens kön och födelsedatum:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Begränsningar
Gränser som gäller tooapplication information finns i hello områden i reguljära uttryck för nycklar och storlek.

#### <a name="keys"></a>Nycklar
Varje nyckel i hello-objekt måste matcha hello följande reguljära uttryck:

    ^[a-zA-Z][a-zA-Z_0-9]*

Detta innebär att nycklar måste börja med minst en bokstav följt av bokstäver, siffror eller understreck (\_).

#### <a name="size"></a>Storlek
Information om programmet är begränsad too1, 024 tecken per anrop (efter hello Mobile Engagement Web SDK kodar den i JSON).

I föregående exempel hello, skickas hello JSON toohello servern är 44 tecken:

    {"birthdate":"1983-12-07","gender":"female"}
