---
title: aaaMitigations - hotet Modeling verktyget - Azure | Microsoft Docs
description: "Åtgärder på sidan hello hot Modeling verktyget färgmarkera möjliga lösningar toohello mest exponeras genereras hot."
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
ms.openlocfilehash: 000c0980d976b09fc9287e582e3776efaf7e390c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-threat-modeling-tool-mitigations"></a>Microsoft Threat Modeling verktyget åtgärder

hello hot Modeling verktyget utgör kärnan i hello Microsoft Security Development Lifecycle (SDL). Det tillåter programvara utformar tooidentify och minimera potentiella säkerhetsproblem tidigt, när de är relativt enkel och kostnadseffektiv tooresolve. Därför kan antalet det minskar hello totalkostnaden för utveckling. Dessutom utformat vi hello verktyget med ej säkerhet experter ihåg förenklar hotmodellering för alla utvecklare genom att ge klara riktlinjer för att skapa och analysera hot modeller.

Besök hello  **[hot Modeling verktyget](./azure-security-threat-modeling-tool.md)**  tooget redan idag!

## <a name="mitigation-categories"></a>Minskning kategorier

hello hot Modeling verktyget åtgärder kategoriseras enligt toohello Web Application säkerhet ram, som består av följande hello:

| Kategori | Beskrivning |
| -------- | ----------- |
| **[Granskning och loggning](./azure-security-threat-modeling-tool-auditing-and-logging.md)** | Vem som gjorde vad och när? Granskning och loggning referera toohow ditt program innehåller säkerhetshändelser |
| **[Autentisering](./azure-security-threat-modeling-tool-authentication.md)** | Vem är du? Autentisering är hello process där en entitet bevisar hello identiteten för en annan entitet vanligtvis via autentiseringsuppgifter, till exempel användarnamn och lösenord |
| **[Auktorisering](./azure-security-threat-modeling-tool-authorization.md)** | Vad kan du göra? Auktorisering är hur programmet ger åtkomstkontroll för resurser och åtgärder |
| **[KOMMUNIKATIONSSÄKERHET](./azure-security-threat-modeling-tool-communication-security.md)** | Som du talar till? KOMMUNIKATIONSSÄKERHET garanterar all kommunikation klar är så säkert som möjligt |
| **[Konfigurationshantering](./azure-security-threat-modeling-tool-configuration-management.md)** | Som körs programmet som? Vilka databaser ansluter den till? Hur styrs ditt program? Hur skyddas de här inställningarna? Konfigurationshantering refererar toohow programmet hanterar dessa operativa problem |
| **[Kryptografi](./azure-security-threat-modeling-tool-cryptography.md)** | Hur behåller du hemligheter (sekretess)? Hur ska du nyckellagring språkverktyg data eller bibliotek (integritet)? Hur tillhandahåller du frö för slumpmässiga värden som måste vara kryptografiskt starka? Kryptografi refererar toohow programmet tillämpar sekretess och integritet |
| **[Hantering av undantag](./azure-security-threat-modeling-tool-exception-management.md)** | När ett metodanrop i ditt program inte vad är ditt program? Hur mycket du avslöja? Du eget fel returneras information tooend användare? Du skickar värdefulla undantag information tillbaka toohello anroparen? Misslyckas programmet avslutas? |
| **[Verifiering av indata](./azure-security-threat-modeling-tool-input-validation.md)** | Hur vet du att hello indata program får är giltigt och säkert? Verifiering av indata refererar toohow programmet filtrerar buskmarker eller avvisar indata innan ytterligare bearbetning. Överväg att begränsa indata via startpunkter och kodning utdata via avsluta punkter. Litar du data från källor som databaser och filresurser? |
| **[Känsliga Data](./azure-security-threat-modeling-tool-sensitive-data.md)** | Hur hanterar programmet känsliga data? Känsliga data refererar toohow programmet hanterar alla data som måste skyddas i minnet, hello nätverket, eller i beständiga lagrar |
| **[Sessionshantering](./azure-security-threat-modeling-tool-session-management.md)** | Hur ditt program hantera och skydda användarsessioner? En session refererar tooa serie relaterade samverkan mellan användare och ditt webbprogram |

Det hjälper dig att identifiera:

* Var finns hello vanligaste gjorda av misstag
* Var finns hello mest tillämplig förbättringar

Därför kan du använder dessa kategorier toofocus och prioritera ditt arbete för säkerhet, så att om du vet hello vanligaste säkerhetsproblem uppstå i kategorier för hello inkommande verifiering, autentisering och auktorisering kan du starta det. Mer information finns  **[patent länken](https://www.google.com/patents/US7818788)**

## <a name="next-steps"></a>Nästa steg

Besök  **[hot Modeling verktyget hot](./azure-security-threat-modeling-tool-threats.md)**  toolearn mer om hello hot kategorier hello verktyget använder toogenerate möjliga design hot.
