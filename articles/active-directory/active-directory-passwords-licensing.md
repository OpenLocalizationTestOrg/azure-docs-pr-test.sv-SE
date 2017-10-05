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
ms.openlocfilehash: 936134bddad19964f809a17f200ebbeed5aa853c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Återställ Licensieringskrav för självbetjäning Azure AD-lösenord

För återställning av Azure AD-lösenord ska fungera, du **måste ha minst en licens för i din organisation**. Vi behöver inte använda per användare-licensiering om återställning av lösenord. Du måste tilldela licenser till användare som använder premiumfunktioner för att bibehålla kompatibilitet med ditt Microsoft-licensavtalet.

* **Molnet endast användare** -Office 365 (O365) någon betald SKU eller Azure AD Basic
* **Molnet** och/eller **lokala användare** -Azure AD Premium P1 eller P2, Enterprise Mobility + Security (EMS) eller säker produktiva Enterprise (a)

## <a name="licenses-required-for-password-writeback"></a>Licenser som krävs för tillbakaskrivning av lösenord

Om du vill använda tillbakaskrivning av lösenord, måste du ha en av de följande licenser som tilldelats i din klient.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Secure Productive Enterprise E3
* Secure Productive Enterprise E5

> [!NOTE]
> Fristående Office 365-licensiering planer **har inte stöd för tillbakaskrivning av lösenord** och kräver en av de föregående planerna för den här funktionen ska fungera.

Ytterligare licenser uppgifter om kostnaderna kan hittas på följande sidor

* [Platsen för Azure Active Directory-priser](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Säker produktiva Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a>Aktivera grupp eller användarbaserade licensiering

Azure AD nu stöder gruppbaserade licensiering vilket gör att administratörer för att tilldela licenser gruppvis till en grupp användare, snarare än tilldela dem en i taget. [Tilldela kontrollerar och lösa problem med licenser](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Vissa Microsoft-tjänster är inte tillgängliga på alla platser. Innan en användare kan tilldelas en licens, måste administratören ange egenskapen ”användningsplats” för användaren. Tilldelning av licenser kan göras under användare > Profil > Inställningar i Azure-portalen. **När du använder gruppen licenstilldelning ärver alla användare utan en användningsplats anges platsen för katalogen.**

## <a name="next-steps"></a>Nästa steg

Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Data**](active-directory-passwords-data.md) – Förstå de data som krävs och hur de används för lösenordshantering
* [**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här
* [**Anpassa**](active-directory-passwords-customize.md) – Anpassa utseendet för företagets SSPR-funktion.
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? – Svar på allt du någonsin velat fråga
* [**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR

