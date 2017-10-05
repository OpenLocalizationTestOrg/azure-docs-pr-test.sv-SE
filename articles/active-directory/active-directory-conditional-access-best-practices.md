---
title: "Bästa praxis för villkorlig åtkomst i Azure Active Directory | Microsoft Docs"
description: "Lär dig mer om saker du bör känna till och vad det är bör du undvika detta när du konfigurerar principer för villkorlig åtkomst."
services: active-directory
keywords: "villkorlig åtkomst till appar, villkorlig åtkomst med Azure AD, säker åtkomst till företagets resurser, principer för villkorlig åtkomst"
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
ms.openlocfilehash: 3e524c116479c1af6eb6a601c9b57d27a697c5a2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Metodtips för villkorlig åtkomst i Azure Active Directory

Det här avsnittet ger information om saker du bör känna till och vad det är bör du undvika detta när du konfigurerar principer för villkorlig åtkomst. Innan du läser det här avsnittet bör du bekanta dig med begrepp och termer som beskrivs i [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md)

## <a name="what-you-should-know"></a>Vad du bör känna till

### <a name="whats-required-to-make-a-policy-work"></a>Vad har krävs för att skapa en princip för att fungera?

När du skapar en ny princip, finns det inga användare, grupper, appar eller åtkomstkontroller som valts.

![Molnappar](./media/active-directory-conditional-access-best-practices/02.png)


Om du vill att din princip för att fungera, måste du konfigurera följande:


|Vad           | Hur                                  | Varför|
|:--            | :--                                  | :-- |
|**Molnappar** |Du måste välja en eller flera appar.  | Målet med en princip för villkorlig åtkomst är så att du kan finjustera hur behöriga användare kan komma åt dina appar.|
| **Användare och grupper** | Du måste välja minst en användare eller grupp som har behörighet att få åtkomst till molnappar som du har valt. | En villkorlig åtkomstprincip som har ingen användare och grupper som har tilldelats, utlöses aldrig. |
| **Åtkomstkontroll** | Du måste välja minst en åtkomstkontroll. | Princip för processorn behöver veta vad du gör om dina villkor är uppfyllda.|


Förutom de grundläggande kraven bör i många fall kan du också konfigurera ett villkor. När en princip fungerar också utan ett konfigurerat villkor, är villkor drivande faktor för att finjustera åtkomst till dina appar.


![Molnappar](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Hur utvärderas tilldelningar?

Alla tilldelningar som är logiskt **and**. Om du har mer än en tilldelning som konfigurerats för att utlösa en princip, måste alla tilldelningar vara uppfyllda.  

Om du behöver konfigurera en plats-villkor som gäller för alla anslutningar från utanför företagets nätverk åstadkommer detta genom att:

- Inklusive **alla platser**
- Förutom **alla betrodda IP-adresser**

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Vad händer om du har principer i den klassiska Azure-portalen och Azure-portalen konfigurerad?  
Båda principerna tillämpas av Azure Active Directory och användaren får åtkomst till endast när alla krav är uppfyllda.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Vad händer om du har principer i Intune Silverlight-portalen och Azure Portal?
Båda principerna tillämpas av Azure Active Directory och användaren får åtkomst till endast när alla krav är uppfyllda.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Vad händer om jag har flera principer för samma användare konfigurerade?  
För varje inloggning, Azure Active Directory utvärderar alla principer och garanterar att alla krav är uppfyllda innan du beviljar åtkomst till användaren.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Fungerar villkorlig åtkomst med Exchange ActiveSync

Ja, du kan använda Exchange ActiveSync i en princip för villkorlig åtkomst.


## <a name="what-you-should-avoid-doing"></a>Vad du bör undvika att göra

Villkorlig åtkomst framework ger dig en utmärkt konfigurationsflexibilitet. Stor flexibilitet innebär dock också att du noggrant läser varje princip för konfiguration innan du släpper den för att undvika oönskade resultat. I den här kontexten bör du vara särskilt uppmärksam på tilldelningar som påverkar fullständiga **alla användare / grupper / molnappar**.

I din miljö bör du undvika följande konfigurationer:


**För alla användare alla molnappar:**

- **Blockera åtkomst** -den här konfigurationen blockerar hela organisationen, som definitivt inte är en bra idé.

- **Kräver en kompatibel enhet** – för användare som inte har registrerat sina enheter än den här principen blockerar all åtkomst, inklusive åtkomst till Intune-portalen. Om du är administratör saknar en registrerad enhet blockerar du från att få tillbaka till Azure portal för att ändra principen i den här principen.

- **Kräva domänanslutning** – den här principen blockera åtkomst har också möjlighet att blockera åtkomst för alla användare i organisationen om du inte har en domänansluten enhet ännu.


**För alla användare, alla molnappar, alla enhetsplattformar:**

- **Blockera åtkomst** -den här konfigurationen blockerar hela organisationen, som definitivt inte är en bra idé.


## <a name="common-scenarios"></a>Vanliga scenarier

### <a name="requiring-multi-factor-authentication-for-apps"></a>Att kräva multifaktorautentisering för appar

Många miljöer har appar som kräver en högre nivå av skydd än den andra.
Detta är exempelvis fallet för appar som har åtkomst till känslig information.
Om du vill lägga till ett extra skyddslager för dessa appar kan du konfigurera en princip för villkorlig åtkomst som kräver multifaktorautentisering när användarna kommer åt dessa appar.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Kräver multifaktorautentisering för åtkomst från nätverk som inte är betrodda

Det här scenariot liknar föregående scenario eftersom den lägger till ett krav för multifaktorautentisering.
Den största skillnaden är dock villkoret för det här kravet.  
Medan fokus i det föregående scenariot var i appar med åtkomst till sensitve data, fokuserar i det här scenariot på betrodda platser.  
Du kan med andra ord ha ett krav för multifaktorautentisering om en app öppnas av en användare från ett nätverk som du inte litar på.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Endast betrodda enheter kan komma åt Office 365-tjänster

Om du använder Intune i din miljö kan du direkt börja använda gränssnittet för princip för villkorlig åtkomst i Azure-konsolen.

Många Intune-kunder använder villkorlig åtkomst för att se till att endast betrodda enheter kan komma åt Office 365-tjänster. Detta innebär att mobila enheter har registrerats med Intune och uppfyller krav på efterlevnad och att Windows-datorer är anslutna till en lokal domän. En viktiga förbättringar är att du inte behöver ange samma princip för varje Office 365-tjänster.  När du skapar en ny princip, konfigurera molnappar för att inkludera varje O365-appar som du vill skydda med med villkorlig åtkomst.

## <a name="next-steps"></a>Nästa steg

Om du vill veta hur du konfigurerar en princip för villkorlig åtkomst finns [Kom igång med villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
