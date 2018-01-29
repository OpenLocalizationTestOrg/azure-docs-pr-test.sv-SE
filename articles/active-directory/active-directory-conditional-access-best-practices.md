---
title: "Bästa praxis för villkorlig åtkomst i Azure Active Directory | Microsoft Docs"
description: "Lär dig mer om saker du bör känna till och vad det är bör du undvika detta när du konfigurerar principer för villkorlig åtkomst."
services: active-directory
keywords: "villkorlig åtkomst till appar, villkorlig åtkomst med Azure AD, säker åtkomst till företagets resurser, principer för villkorlig åtkomst"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 8c6707505a6331b53e06b1de60575dd3637ea477
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/12/2017
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



## <a name="policy-migration"></a>Migrering av princip

Du bör migrera principer som du inte har skapat i Azure-portalen eftersom:

- Du kan nu hantera scenarier som du inte kan hantera innan.

- Du kan minska antalet principer som du måste hantera genom att konsolidera dem.   

- Du kan hantera alla dina principer för villkorlig åtkomst på en central plats.

- Den klassiska Azure-portalen ska tas bort.   


Mer information finns i [migrera klassiska principer i Azure portal](active-directory-conditional-access-migration.md).


## <a name="next-steps"></a>Nästa steg

Om du vill veta hur du konfigurerar en princip för villkorlig åtkomst finns [Kom igång med villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
