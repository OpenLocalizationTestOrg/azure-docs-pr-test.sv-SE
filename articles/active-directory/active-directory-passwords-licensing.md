---
title: 'Licensiering: Azure AD SSPR | Microsoft Docs'
description: "Azure AD Självbetjäning för lösenordsåterställning licenskrav"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 9cecaaac429165346f7082f1965dc8a21063fe7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Återställ Licensieringskrav för självbetjäning Azure AD-lösenord

För Azure AD för lösenordsåterställning toofunction du **måste ha minst en licens för i din organisation**. Vi behöver inte använda per användare-licensiering på hello lösenord Återställ upplevelse. toomaintain följer licensavtalet Microsoft, behöver du tooassign licenser tooany användare som använder premiumfunktioner.

* **Molnet endast användare** -Office 365 (O365) någon betald SKU eller Azure AD Basic
* **Molnet** och/eller **lokala användare** -Azure AD Premium P1 eller P2, Enterprise Mobility + Security (EMS) eller säker produktiva Enterprise (a)

## <a name="licenses-required-for-password-writeback"></a>Licenser som krävs för tillbakaskrivning av lösenord

toouse tillbakaskrivning av lösenord, måste du ha en hello följande licenser som tilldelats i din klient.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Secure Productive Enterprise E3
* Secure Productive Enterprise E5

> [!NOTE]
> Fristående Office 365-licensiering planer **har inte stöd för tillbakaskrivning av lösenord** och kräver en hello föregående planer för den här funktionen toowork.

Ytterligare licenser uppgifter om kostnaderna kan hittas på följande sidor hello

* [Platsen för Azure Active Directory-priser](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Säker produktiva Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a>Aktivera grupp eller användarbaserade licensiering

Azure AD nu stöder gruppbaserade licensiering vilket gör att administratörer tooassign licenser i bulk tooa grupp av användare, snarare än tilldela dem en i taget. [Tilldela kontrollerar och lösa problem med licenser](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Vissa Microsoft-tjänster är inte tillgängliga på alla platser. Innan en licens kan tilldelas tooa användare, anger Hej administratör hello ”användning” Platsegenskapen hello användaren. Tilldelning av licenser kan göras under användare > Profil > Inställningar i hello Azure-portalen. **När du använder gruppen licenstilldelning ärver alla användare utan en användningsplats anges hello platsen för hello katalog.**

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR

