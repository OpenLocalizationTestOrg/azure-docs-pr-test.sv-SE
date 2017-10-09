---
title: "aaaLearn om tvåstegsverifiering i Azure MFA | Microsoft Docs"
description: "Vad är Azure Multi-Factor Authentication, varför ska jag använda MFA, mer information om hello Multi-Factor Authentication-klienten och hello olika metoder och versioner som är tillgängliga. "
keywords: "Introduktion tooMFA mfa översikt vad är mfa"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: kgremban
ms.openlocfilehash: a91b8d6941d2b6ce72a789a97c57e10e594e7ada
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a>Vad är Azure Multi-Factor Authentication?
Tvåstegsverifiering är en autentiseringsmetod som kräver mer än en verifieringsmetod och lägger till ett kritiskt andra säkerhetslager säkerhet toouser inloggningar och transaktioner. Det fungerar genom att två eller flera av följande verifieringsmetoderna hello:

* Något du vet (normalt ett lösenord)
* Något du har (betrodda enheter som inte enkelt dubbleras, t.ex. en telefon)
* Något du är (biometrik)

<center>![Användarnamn och lösenord](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certifikat](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![smartkort](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtuellt smartkort](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![användarnamn och Lösenord](./media/multi-factor-authentication/cert.png)</center>

Azure Multi-Factor Authentication (MFA) är Microsofts verifieringslösning i två steg. Azure MFA hjälper dig att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning. Den ger stark autentisering via en mängd verifieringsmetoder, inklusive telefonsamtal, textmeddelande eller verifiering av mobilappar.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/WA-MFA-Overview/player]
>
>

## <a name="why-use-azure-multi-factor-authentication"></a>Varför använda Azure Multi-Factor Authentication?
I större utsträckning än någonsin, ansluts allt personer. Med smarta telefoner, surfplattor, bärbara datorer och datorer kan ha personer flera olika alternativ för hur de ska tooconnect och upprätthålla anslutningen när som helst. Användare kan komma åt sina konton och program från valfri plats, vilket innebär att de kan få mer arbete gjort och hantera sina kunder bättre.

Azure Multi-Factor Authentication är ett enkelt toouse skalbara och tillförlitliga som ger en annan metod för autentisering så att användarna alltid är skyddad.

| ![Enkelt tooUse](./media/multi-factor-authentication/simple.png) | ![Skalbar](./media/multi-factor-authentication/scalable.png) | ![Alltid skyddad](./media/multi-factor-authentication/protected.png) | ![Tillförlitlig](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Enkelt toouse** |**Skalbar** |**Alltid skyddad** |**Tillförlitlig** |

* **Enkelt tooUse** -Azure Multi-Factor Authentication är enkla tooset upp och användning. hello tillåter extra skydd som medföljer Azure Multi-Factor Authentication att användare toomanage sina egna enheter. Bästa av alla i många fall det kan ställas in med ett par enkla klick.
* **Skalbar** -Azure Multi-Factor Authentication använder hello kraften i Molnets för hello och kan integreras med din lokala AD och anpassade appar. Det här skyddet utökas även tooyour stora volymer, verksamhetskritiska scenarier.
* **Alltid skyddad** -Azure Multi-Factor Authentication ger stark autentisering med hjälp av hello högsta branschstandarder.
* **Tillförlitliga** -vi garanterar 99,9% tillgänglighet för Azure Multi-Factor Authentication. hello tjänsten räknas som otillgänglig när det inte går tooreceive eller process verifiering begäranden för hello tvåstegsverifiering.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [hur Azure Multi-Factor Authentication fungerar](multi-factor-authentication-how-it-works.md)

- Läs mer om olika hello [versioner och av förbrukningsmetoder för Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)
