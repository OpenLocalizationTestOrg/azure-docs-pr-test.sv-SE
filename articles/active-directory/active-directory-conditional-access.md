---
title: "Villkorlig åtkomst i den klassiska Azure-portalen | Microsoft Docs"
description: "Använd villkorlig åtkomstkontroll i den klassiska Azure-portalen för att söka efter särskilda villkor vid autentisering för åtkomst till program."
services: active-directory
keywords: "villkorlig åtkomst till appar, villkorlig åtkomst med Azure AD, säker åtkomst till företagets resurser, principer för villkorlig åtkomst"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: da3f0a44-1399-4e0b-aefb-03a826ae4ead
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.custom: oldportal
ms.openlocfilehash: d96eeb07c4bf3944be82d9c54aea93d52b54a37a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="conditional-access-in-the-azure-classic-portal"></a>Villkorlig åtkomst i den klassiska Azure-portalen

Det här avsnittet handlar om hur villkorlig åtkomst i den klassiska Azure-portalen. Den senaste informationen om villkorlig åtkomst i Azure Active Directory, se [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md).


Kontrollen funktionerna i Azure Active Directory (AD Azure) villkorlig åtkomst erbjuder enkelt sätt att skydda resurser i molnet och lokalt. Principer för villkorlig åtkomst som multifaktorautentisering kan skydda mot risken för stulna och phished autentiseringsuppgifter. Andra principer för villkorlig åtkomst kan skydda data i din organisation. Till exempel förutom att kräva autentiseringsuppgifter, kanske du har en princip som endast enheter som registreras i ett system för hantering av mobila enheter som Microsoft Intune har åtkomst till känsliga tjänster för din organisation.

## <a name="prerequisites"></a>Krav
Azure AD villkorlig åtkomst är en funktion i [Azure Active Directory Premium](http://www.microsoft.com/identity). Varje användare som har åtkomst till ett program som har principer för villkorlig åtkomst tillämpas måste ha en Azure AD Premium-licens. Du kan lära dig mer om licenskrav i [olicensierad användaren rapporten](https://aka.ms/utc5ix).

## <a name="how-is-conditional-access-control-enforced"></a>Hur tillämpas villkorlig åtkomstkontroll?
Azure AD med villkorlig åtkomstkontroll på plats, kontrollerar om specifikt villkor du anger för en användare åtkomst till ett program. När krav är uppfyllda, användaren autentiseras och kan komma åt programmet.  

![Villkorlig åtkomst: översikt](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Villkor
Dessa är villkor som du kan inkludera i en princip för villkorlig åtkomst:

* **Gruppmedlemskap**. Kontrollera en användares åtkomst baserat på medlemskap i en grupp.
* **Plats**. Använd plats för användaren som ska utlösa multifaktorautentisering och Använd block kontroller när en användare inte är i ett betrott nätverk.
* **Enhetsplattform**. Använd plattform, till exempel iOS, Android, Windows Mobile och Windows, som ett villkor för att tillämpa principen.
* **Enheten aktiverad**. Enhetens tillstånd verifieras om aktiveras eller inaktiveras, under utvärdering av principen för enheten. Om du inaktiverar en förlorad eller stulen enhet i katalogen kan uppfyller den inte längre principkraven.
* **Risken för inloggning och användaren**. Du kan använda [Azure AD Identity Protection](active-directory-identityprotection.md) för principer för villkorlig åtkomst risk. Principer för villkorlig åtkomst risk hjälp i din organisation avancerade skydd baserat på riskhändelser och ovanlig inloggning aktiviteter.

## <a name="controls"></a>Kontroller
Här är kontroller som du kan använda för att tillämpa en princip för villkorlig åtkomst:

* **Multifaktorautentisering**. Du kan kräva stark autentisering via multifaktorautentisering. Du kan använda multifaktorautentisering med Azure Multi-Factor Authentication eller genom att använda en lokal flerfunktionsautentiseringsleverantör, tillsammans med Active Directory Federation Services (AD FS). Använda Multi-Factor authentication skyddar resurser från används av en obehörig användare som kan ha fick tillgång till autentiseringsuppgifterna för en giltig användare.
* **Blockera**. Du kan använda villkor som plats att blockera användaråtkomst. Du kan till exempel blockera åtkomst när en användare inte är i ett betrott nätverk.
* **Kompatibla enheter**. Du kan ange principer för villkorlig åtkomst på enhetsnivå. Du kan ställa in en princip så att endast datorer som är ansluten till domänen eller mobila enheter som är registrerade i en mobil enhet hanteringsprogram kan komma åt företagets resurser. Du kan till exempel använda Intune för att kontrollera enheternas kompatibilitet och rapportera det till Azure AD för tvingande när användaren försöker komma åt ett program. Detaljerad information om hur du använder Intune för att skydda data och appar finns [skydda data och appar med Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Du kan också använda Intune för att genomdriva dataskydd för borttappade eller stulna enheter. Mer information finns i [skydda dina data med fullständig eller selektiv rensning med Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Program
Du kan tillämpa en princip för villkorlig åtkomst på programnivå. Ange åtkomstnivåer för program och tjänster i molnet eller lokalt. Principen tillämpas direkt på den webbplats eller tjänst. Principen aktiveras för åtkomst till webbläsaren och program som har åtkomst till tjänsten.

## <a name="device-based-conditional-access"></a>Enhetsbaserad villkorlig åtkomst
Du kan begränsa åtkomsten till program från enheter som registreras med Azure AD och som uppfyller specifika villkor. Enhetsbaserad villkorlig åtkomst förhindrar användare som försöker komma åt resurser från en organisations resurser:

* Okänt eller ohanterade enheter.
* Enheter som inte uppfyller säkerhetsprinciperna organisationen ställa in.

Du kan ange principer baserat på dessa krav:

* **Domänanslutna enheter**. Ange en princip för att begränsa åtkomsten till enheter som är anslutna till en lokal Active Directory-domän och som också har registrerats med Azure AD. Den här principen gäller för Windows-arbetsstationer, bärbara datorer och enterprise surfplattor.
  Mer information om hur du ställer in automatisk registrering av domänanslutna enheter med Azure AD finns [konfigurera automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
* **Kompatibla enheter**. Ange en princip för att begränsa åtkomsten till enheter som är markerade **kompatibla** i systemkatalogen för hantering. Den här principen ser till att endast enheter som uppfyller säkerhetsprinciper till exempel framtvinga kryptering av filer på en enhet har åtkomst. Du kan använda den här principen för att begränsa åtkomst från följande enheter:
  
  * **Windows-domänanslutna enheter**. Hanteras av System Center Configuration Manager (i aktuell gren) distribuerat i en hybrid-konfiguration.
  * **Windows 10 Mobile arbets- eller personliga enheter**. Hanteras av Intune eller av ett system för hantering av stöd från tredje part för mobila enheter.
  * **iOS och Android-enheter**. Hanteras av Intune.

Användare som har åtkomst till program som skyddas av en enhetsbaserad principen måste åtkomst till programmet från en enhet som uppfyller den här principen. Åtkomst nekas om försökte på en enhet som inte uppfyller principkraven.

Information om hur du konfigurerar en enhetsbaserad, principen i Azure AD finns i [skapa princip enhetsbaserad villkorlig åtkomst för Azure Active Directory-anslutna program](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Resurser
Se följande resurs kategorier och artiklar för att lära dig mer om hur villkorlig åtkomst för din organisation.

### <a name="multi-factor-authentication-and-location-policies"></a>Principer för multi-Factor authentication och plats
* [Komma igång med villkorlig åtkomst till Azure AD-anslutna appar som är baserade på gruppen, plats och multifaktorautentisering principer](active-directory-conditional-access-azuread-connected-apps.md)
* [Program och webbläsare som stöds](active-directory-conditional-access-supported-apps.md)

### <a name="device-based-conditional-access"></a>Enhetsbaserad villkorlig åtkomst
* [Ange princip för åtkomstkontroll för enhetsbaserad villkorlig åtkomst till Azure Active Directory-anslutna program](active-directory-conditional-access-policy-connected-applications.md)
* [Konfigurera automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)
* [Felsökning av problem med Azure Active Directory](active-directory-conditional-access-device-remediation.md)
* [Skydda data på borttappade eller stulna enheter med Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)

### <a name="protect-resources-based-on-sign-in-risk"></a>Skydda resurser baserat på inloggning risk
* [Azure AD identity protection](active-directory-identityprotection.md)

### <a name="next-steps"></a>Nästa steg
* [Villkorlig åtkomst vanliga frågor och svar](active-directory-conditional-faqs.md)
* [Teknisk referens](active-directory-conditional-access-technical-reference.md)

