---
title: aaaAzure Multi-Factor Authentication - hur det fungerar
description: "Azure Multi-Factor Authentication hjälper till att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning. Det ger ökad säkerhet genom att kräva andra formen av autentisering och ger stark autentisering via en mängd alternativ för enkel verifiering."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 82f234fb86f145c42e8e56b8bdd2d61720c9ff2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a>Hur Azure Multi-Factor Authentication fungerar
hello säkerheten för tvåstegsverifiering ligger i dess överlappande tillvägagångssättet. Att kompromettera flera autentiseringsfaktorer presenterar en betydande utmaning för angripare. Även om en angripare lyckas toolearn hello användarens lösenord, är det oanvändbar utan också tillgång hello betrodd enhet. 

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)

Azure Multi-Factor Authentication hjälper till att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning.  Det ger ökad säkerhet genom att kräva andra formen av autentisering och ger stark autentisering via en mängd alternativ för enkel verifiering.


## <a name="methods-available-for-two-step-verification"></a>Metoderna för tvåstegsverifiering
När en användare loggar in, skickas en ytterligare verifiering toohello användare.  hello nedan följer en lista över metoder som kan användas för andra verifieringen.

| Verifieringsmetod | Beskrivning |
| --- | --- |
| Telefonsamtal |Ett anrop placeras tooa användarens registrerade telefon. hello användaren anger en PIN-kod om det behövs och sedan trycker hello #-knappen. |
| Textmeddelande |Ett textmeddelande skickas tooa användarens mobiltelefon med en sexsiffrig kod. hello användaren anger den här koden på hello-inloggningssida. |
| Avisering från mobilappen |En begäran om identitetsverifiering skickas tooa användarens Smartphone. hello användaren anger en PIN-kod vid behov och sedan väljer **Kontrollera** på hello mobila appar. |
| Verifieringskod för mobila appar |Hej mobilappen som körs på en användares Smartphone, visar en Verifieringskod som ändrar var 30: e sekund. hello användare söker efter hello senaste kod och registrerar den på hello-inloggningssida. |
| OATH-token från tredje part | Azure Multi-Factor Authentication-servern kan vara konfigurerade tooaccept tredjepartsverifiering metoder. |

Azure Multi-Factor Authentication ger valbar verifieringsmetoderna för moln- och server. Du kan välja vilka metoder som är tillgängliga för användarna: telefonsamtal, text, avisering via app eller app-koder. Mer information finns i [valbar verifieringsmetoderna](multi-factor-authentication-whats-next.md#selectable-verification-methods).

## <a name="next-steps"></a>Nästa steg

- Läs mer om olika hello [versioner och av förbrukningsmetoder för Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

- Välj om toodeploy Azure MFA [i hello molnet eller lokalt](multi-factor-authentication-get-started.md)

- Läs svar på [vanliga frågor och svar](multi-factor-authentication-faq.md)