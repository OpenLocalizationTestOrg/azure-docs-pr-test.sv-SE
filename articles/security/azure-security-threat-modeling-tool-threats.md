---
title: aaaThreats - hotet Modeling verktyget - Azure | Microsoft Docs
description: "Hot kategorisidan för hello hot Modeling verktyget som innehåller kategorier för alla exponeras genereras hot."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 0bd51f81370b6385ff1ac9769e34fc089e1dfc9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-threat-modeling-tool-threats"></a>Microsoft Threat Modeling verktyget hot

hello hot Modeling verktyget utgör kärnan i hello Microsoft Security Development Lifecycle (SDL). Det tillåter programvara utformar tooidentify och minimera potentiella säkerhetsproblem tidigt, när de är relativt enkel och kostnadseffektiv tooresolve. Därför kan antalet det minskar hello totalkostnaden för utveckling. Dessutom utformat vi hello verktyget med ej säkerhet experter ihåg förenklar hotmodellering för alla utvecklare genom att ge klara riktlinjer för att skapa och analysera hot modeller.

> Besök hello  **[hot Modeling verktyget](./azure-security-threat-modeling-tool.md)**  tooget redan idag!

hello hot Modeling verktyget hjälper dig att besvara vissa frågor, till exempel hello de nedan:

* Hur kan en angripare ändra hello autentiseringsdata?
* Vad är hello effekt om en angripare kan läsa hello användarprofildata?
* Vad händer om åtkomst nekas toohello användardatabas profilen?

## <a name="stride-model"></a>STRIDE modellen

toobetter hjälp du formulerar dessa typer av pekar frågor, Microsoft använder hello STRIDE modell, som kategoriserar olika typer av hot och förenklar hello övergripande säkerhet konversationer.

| Kategori | Beskrivning |
| -------- | ----------- |
| **Förfalskning** | Omfattar sätt att komma åt och sedan använda en annan användares autentiseringsinformation, till exempel användarnamn och lösenord |
| **Manipulation** | Omfattar hello ändringar av data. Exempel obehöriga ändringar toopersistent data, till exempel som lagras i en databas och hello ändring av data när den förs vidare mellan två datorer över öppna nätverk, till exempel hello Internet |
| **Repudiation** | Som är kopplade till användare och neka utför en åtgärd utan någon part som har alla sätt tooprove annars – till exempel en användare utför en otillåten åtgärd i ett system som saknar hello möjlighet tootrace hello förbjudet åtgärder. Icke-förnekande refererar toohello möjligheten för en system toocounter repudiation hot. En användare som köper en artikel kan till exempel ha toosign för hello-objektet har tagits emot. hello leverantör kan sedan Använd hello signerade inleverans som bevis hello användaren fick hello-paket |
| **Avslöjande av information** | Innebär att information tooindividuals som inte är avsedda toohave åtkomst tooit hello exponeras – till exempel hello möjligheten för användare tooread en fil som de inte har åtkomst till, eller hello möjligheten för en inkräktare tooread data under överföring mellan två datorer |
| **Denial of Service** | Denial of service (DoS) attacker neka tjänsten toovalid användare – till exempel genom att göra en webbserver tillfälligt otillgänglig eller inte kan användas. Du måste skydda dig mot vissa typer av DoS hot bara tooimprove systemtillgänglighet och tillförlitlighet |
| **Rättighetsökning** | En icke-privilegierade användare får privilegierad åtkomst och därmed har tillräcklig åtkomst toocompromise eller förstöra hello hela systemet. Höjning av privilegier hot omfattar dessa situationer där en angripare har effektivt genombryts försvar för alla system och blir en del av själva farliga situationer verkligen hello betrodda-systemet |

## <a name="next-steps"></a>Nästa steg

Gå vidare för**[hot Modeling verktyget åtgärder](./azure-security-threat-modeling-tool-mitigations.md)**  toolearn hello olika sätt man kan minimera dessa hot med Azure.
