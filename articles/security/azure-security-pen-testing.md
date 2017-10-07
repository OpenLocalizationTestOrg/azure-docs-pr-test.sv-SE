---
title: aaaPen testar | Microsoft Docs
description: "hello artikeln ger en översikt över hello intrång testa process (pentest) och hur utför pentest mot dina appar som körs i Azure-infrastrukturen."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: 202c239f46d8693ab7aa85e237235372e743e108
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pen-testing"></a>Penna testning
En av hello fördelarna med att använda Microsoft Azure för att testa program och distribution är att du inte behöver tooput tillsammans en lokal infrastruktur toodevelop, testa och distribuera ditt program. Alla hello infrastruktur har åtgärdat av hello Microsoft Azure-plattformstjänster. Du har inte tooworry om rekvisition, hämta, ”förflyttning och stapling” lokal maskinvara.

Det är bra – men du måste toomake att du utför vanliga säkerheten vederbörlig möda åt. En av hello saker du behöver toodo är intrång test hello program som du distribuerar i Azure.

Du kanske redan vet att Microsoft utför [intrång testning av våra Azure-miljön](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Detta hjälper oss att förbättra vår plattform och hjälper våra åtgärder vad gäller förbättra säkerhetsåtgärder, Introducera nya säkerhetskontrollerna och förbättra våra säkerhetsprocesser.

Vi inte pennan testa ditt program för dig, men vi förstår att du vill ha och behöver tooperform penna tester på dina program. Det beror användbart när du förbättra hello säkerheten för dina program du gör hello hela Azure ekosystemet säkrare.

När du penna testar dina program, kan det ser ut som en attack toous. Vi [kontinuerligt övervakar](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) för angreppsmönster och starta en process för incidenter om vi behöver. Det inte hjälper kan du och det inte hjälper oss om vi utlöser en incidenter på grund av tooyour egna på grund av fordringar penna testning.

Vilka toodo?

När du är klar toopen testa ditt program för Azure som värd, har du ett alternativ för[berätta för oss](https://portal.msrc.microsoft.com/en-us/engage/pentest). När vi vet att du ska utföra specifika test toobe kan vi inte oavsiktligt stänga av du (till exempel blockerar hello IP-adress som du testar från), så länge dina tester överensstämmer toohello Azure pennan tester villkor beskrivs i [Microsoft Cloud Unified intrång testning regler för Engagement](https://technet.microsoft.com/en-us/mt784683).
Standardtester som du kan utföra inkluderar:

* Tester på dina slutpunkter toouncover hello [öppna Web Application säkerhet projekt (OWASP) topp 10 säkerhetsrisker](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Testa Fuzz](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) av dina slutpunkter
* [Port-scanning](https://en.wikipedia.org/wiki/Port_scanner) av dina slutpunkter

En typ av test som du inte kan utföra är någon typ av [Denial of Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) attack. Detta innefattar initierar en DoS-attack eller utför relaterade testerna som kan avgöra, visa eller simulera någon typ av DoS-attacker.

Är du redo att tooget igång med penna testa dina program finns i Microsoft Azure? Om så är fallet och sedan head på över toohello [intrång Test översikt](https://technet.microsoft.com/library/mt784683.aspx) (och klickar på hello knappen Skapa ett testning begäran på hello hello sidans nederkant. Du hittar också mer information om hello penna testa villkor och användbara länkar på hur du kan rapportera säkerhet fel relaterade tooAzure eller andra Microsoft-tjänster.
