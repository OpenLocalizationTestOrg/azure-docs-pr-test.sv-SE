---
title: "aaaBest praxis för villkorlig åtkomst i Azure Active Directory | Microsoft Docs"
description: "Lär dig mer om saker du bör känna till och vad det är bör du undvika detta när du konfigurerar principer för villkorlig åtkomst."
services: active-directory
keywords: "villkorlig åtkomst tooapps, villkorlig åtkomst med Azure AD, säker åtkomst toocompany resurser, principer för villkorlig åtkomst"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4952f8746a2e583380b3bb99cfe2fbdae1c07b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Metodtips för villkorlig åtkomst i Azure Active Directory

Det här avsnittet ger information om saker du bör känna till och vad det är bör du undvika detta när du konfigurerar principer för villkorlig åtkomst. Innan du läser det här avsnittet bör du bekanta dig med hello begrepp och hello terminologi som beskrivs i [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md)

## <a name="what-you-should-know"></a>Vad du bör känna till

### <a name="whats-required-toomake-a-policy-work"></a>Vad har krävt toomake en princip arbete?

När du skapar en ny princip, finns det inga användare, grupper, appar eller åtkomstkontroller som valts.

![Molnappar](./media/active-directory-conditional-access-best-practices/02.png)


toomake principen fungerar måste du konfigurera hello följande:


|Vad           | Hur                                  | Varför|
|:--            | :--                                  | :-- |
|**Molnappar** |Du måste tooselect en eller flera appar.  | hello målet för en princip för villkorlig åtkomst är tooenable du toofine-finjustera hur behöriga användare kan komma åt dina appar.|
| **Användare och grupper** | Du behöver tooselect minst en användare eller grupp som är auktoriserade tooaccess hello molnappar som du har valt. | En villkorlig åtkomstprincip som har ingen användare och grupper som har tilldelats, utlöses aldrig. |
| **Åtkomstkontroll** | Du behöver tooselect minst en åtkomstkontroll. | Princip för processorn måste tooknow vilka toodo om villkoren är uppfyllda.|


I tillägg toothese grundläggande krav, i de flesta fall bör du också konfigurera ett villkor. När en princip fungerar också utan ett konfigurerat villkor, är villkor hello intresseväckande faktor för att finjustera åtkomst tooyour appar.


![Molnappar](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Hur utvärderas tilldelningar?

Alla tilldelningar som är logiskt **and**. Om du har konfigurerat, mer än en tilldelning tootrigger en princip, kommer alla tilldelningar måste vara uppfyllda.  

Om du behöver tooconfigure ett plats-villkor som gäller tooall anslutningar från utanför företagets nätverk, kan du göra detta genom att:

- Inklusive **alla platser**
- Förutom **alla betrodda IP-adresser**

### <a name="what-happens-if-you-have-policies-in-hello-azure-classic-portal-and-azure-portal-configured"></a>Vad händer om du har principer i hello klassiska Azure-portalen och Azure-portalen konfigurerad?  
Båda principerna tillämpas av Azure Active Directory och hello användaren får åtkomst till endast när alla krav är uppfyllda.

### <a name="what-happens-if-you-have-policies-in-hello-intune-silverlight-portal-and-hello-azure-portal"></a>Vad händer om du har principer i hello Intune Silverlight-portalen och hello Azure Portal?
Båda principerna tillämpas av Azure Active Directory och hello användaren får åtkomst till endast när alla krav är uppfyllda.

### <a name="what-happens-if-i-have-multiple-policies-for-hello-same-user-configured"></a>Vad händer om jag har flera principer för hello som konfigurerats för samma användare?  
För varje inloggning, Azure Active Directory utvärderar alla principer och garanterar att alla krav är uppfyllda innan du beviljar åtkomst toohello användare.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Fungerar villkorlig åtkomst med Exchange ActiveSync

Ja, du kan använda Exchange ActiveSync i en princip för villkorlig åtkomst.


## <a name="what-you-should-avoid-doing"></a>Vad du bör undvika att göra

hello villkorlig åtkomst framework ger dig en utmärkt konfigurationsflexibilitet. Stor flexibilitet också innebär dock att du noggrant läser varje tidigare tooreleasing princip för configuration den tooavoid oönskade resultat. I den här kontexten bör du bara särskild uppmärksamhet tooassignments som påverkar fullständiga **alla användare / grupper / molnappar**.

I din miljö bör du undvika hello följande konfigurationer:


**För alla användare alla molnappar:**

- **Blockera åtkomst** -den här konfigurationen blockerar hela organisationen, som definitivt inte är en bra idé.

- **Kräver en kompatibel enhet** – för användare som inte har registrerat sina enheter än den här principen blockerar all åtkomst, inklusive åtkomst toohello Intune-portalen. Om du är administratör saknar en registrerad enhet blockeras den här principen från att få tillbaka till hello Azure portal toochange hello principen.

- **Kräva domänanslutning** – den här principen blockera åtkomst har också hello potentiella tooblock åtkomst för alla användare i organisationen om du inte har en domänansluten enhet ännu.


**För alla användare, alla molnappar, alla enhetsplattformar:**

- **Blockera åtkomst** -den här konfigurationen blockerar hela organisationen, som definitivt inte är en bra idé.


## <a name="common-scenarios"></a>Vanliga scenarier

### <a name="requiring-multi-factor-authentication-for-apps"></a>Att kräva multifaktorautentisering för appar

Många miljöer har appar som kräver en högre nivå av skydd än hello andra.
Detta är till exempel hello fallet för appar som har åtkomst till toosensitive data.
Om du vill tooadd ett extra lager av skydd toothese appar kan konfigurera du en princip för villkorlig åtkomst som kräver multifaktorautentisering när användarna kommer åt dessa appar.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Kräver multifaktorautentisering för åtkomst från nätverk som inte är betrodda

Det här scenariot är liknande toohello föregående scenario eftersom den lägger till ett krav för multifaktorautentisering.
Hello största skillnaden är dock hello villkor för det här kravet.  
Medan hello fokus på hello föregående scenario var i appar med åtkomst till toosensitve data, fokuserar hello i det här scenariot på betrodda platser.  
Du kan med andra ord ha ett krav för multifaktorautentisering om en app öppnas av en användare från ett nätverk som du inte litar på.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Endast betrodda enheter kan komma åt Office 365-tjänster

Om du använder Intune i din miljö kan du direkt börja använda hello villkorlig åtkomst för gränssnittet i hello Azure-konsolen.

Många Intune-kunder använder villkorlig åtkomst tooensure att endast betrodda enheter kan komma åt Office 365-tjänster. Detta innebär att mobila enheter har registrerats med Intune och uppfyller krav på efterlevnad och att Windows-datorer är anslutna tooan lokal domän. En viktiga förbättringar är att du inte har tooset hello samma princip för varje hello Office 365-tjänster.  När du skapar en ny princip, konfigurera hello molnet appar tooinclude varje hello O365-appar som du vill tooprotect med med villkorlig åtkomst.

## <a name="next-steps"></a>Nästa steg

Om du vill tooknow hur tooconfigure en princip för villkorlig åtkomst, se [Kom igång med villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
