---
title: "aaaAzure händelse rutnätet begrepp"
description: "Beskriver Azure händelse rutnätet och dess begrepp. Definierar flera viktiga komponenter av händelse rutnät."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/10/2017
ms.author: babanisa
ms.openlocfilehash: bb86381330fd2d6d2769167ec1953f0d5c28af85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="concepts-in-azure-event-grid"></a>Koncept i Azure händelse rutnätet

hello grundbegreppen i rutnätet för Azure-händelse är:

## <a name="events"></a>Händelser

En händelse är hello minsta mängden information som beskriver fullständigt något som har inträffat i hello system.  Alla händelser har gemensamma information, till exempel: källa för hello händelse, tid hello händelsen tog plats och unik identifierare.  Alla händelser har också specifik information som är bara relevant toohello specifika händelser. Till exempel innehåller en händelse om en ny fil skapas i Azure Storage information om hello-fil, till exempel hello lastTimeModified värde. Eller en händelse om en virtuell dator startas om innehåller hello namn hello virtuell dator och hello orsaken till omstart.

## <a name="event-sourcespublishers"></a>Källor/händelseutfärdare

En händelsekälla är där hello händelsen inträffar. Azure Storage är till exempel hello händelsekällan för blob som skapats av händelser. Azure VM-strukturen är hello händelsekällan för virtuell dator händelser. Händelsekällan ansvarar för att publicera händelser tooEvent rutnätet.

## <a name="topics"></a>Ämnen

Utgivare kategorisera händelser i avsnitt. hello avsnittet innehåller en slutpunkt där hello publisher skickar händelser. toorespond toocertain typer av händelser, prenumeranter Bestäm vilka avsnitt toosubscribe till. Avsnitt innehåller också en Händelseschema så att prenumeranter kan identifiera hur tooconsume hello händelser på lämpligt sätt.

System avsnitten är inbyggt avsnitt som tillhandahålls av Azure-tjänster. Anpassade avsnitt är program- och tredjeparts-avsnitt.

## <a name="event-subscriptions"></a>Prenumerationer på händelser

En prenumeration instruerar händelse rutnätet på vilka händelser på ett ämne en prenumerant är intresserad av att ta emot.  En prenumeration innehåller också information om hur händelser som ska levereras toohello prenumeranten.

## <a name="event-handlers"></a>Händelsehanterare

En ur händelse rutnätet är en händelsehanterare hello plats där hello händelse skickas. hello-hanteraren tar någon ytterligare åtgärd tooprocess hello-händelse.  Händelsen rutnätet har stöd för flera typer av prenumeranten. Beroende på hello typ av prenumeranten följer händelse rutnätet olika metoder tooguarantee hello leverans av hello-händelse.  För händelsehanterare för HTTP-webhook hello händelse försöks tills hello hanteraren returnerar statuskoden `200 – OK`. För Azure Storage-kö hello händelser är ett nytt försök görs tills hello kötjänsten är kan toosuccessfully processen hello meddelandet push i hello kö.

## <a name="filters"></a>Filter

När du prenumererar tooa avsnittet kan du filtrera hello händelser som skickas toohello slutpunkt. Du kan filtrera efter händelsetyp eller ämne mönster. Mer information finns i [händelse rutnätet prenumeration schemat](subscription-creation-schema.md).

## <a name="security"></a>Säkerhet

Händelsen ger säkerhet för prenumerera tootopics och publicering av information. När du prenumererar, måste du ha tillräckliga behörigheter på hello resurs eller avsnittet. När du publicerar, måste du ha en SAS-token eller nyckelautentisering för hello-avsnittet. Mer information finns i [händelse rutnätet säkerhets- och autentiseringstjänster](security-authentication.md).

## <a name="failed-delivery"></a>Misslyckade leverans

När händelsen rutnätet inte kan bekräfta att en händelse har tagits emot av hello prenumeranten slutpunkten, redelivers hello-händelse. Mer information finns i [händelse rutnätet meddelandeleverans och försök igen](delivery-and-retry.md).

## <a name="next-steps"></a>Nästa steg

* En introduktion tooEvent rutnätet finns [om händelsen rutnätet](overview.md).
* tooquickly komma igång med händelsen rutnätet, se [skapa och flöde anpassade händelser med Azure händelse rutnätet](custom-event-quickstart.md).
