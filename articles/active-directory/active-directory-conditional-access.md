---
title: "aaaConditional åtkomst i hello klassiska Azure-portalen | Microsoft Docs"
description: "Använda villkorlig åtkomstkontroll i hello Azure klassiska portal toocheck för specifika förhållanden vid autentisering för åtkomst tooapplications."
services: active-directory
keywords: "villkorlig åtkomst tooapps, villkorlig åtkomst med Azure AD, säker åtkomst toocompany resurser, principer för villkorlig åtkomst"
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
ms.openlocfilehash: 049bd3f6ec9e35479319ee7608b007b9a1e16647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-in-hello-azure-classic-portal"></a>Villkorlig åtkomst i hello klassiska Azure-portalen

Det här avsnittet handlar om hur villkorlig åtkomst i hello klassiska Azure-portalen. Hello senaste information om villkorlig åtkomst i hello Azure Active Directory finns i [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md).


hello kontrollen funktioner i Azure Active Directory (AD Azure) villkorlig åtkomst erbjuder enkelt sätt toohelp säkra resurser i hello molnet och lokalt. Principer för villkorlig åtkomst som multifaktorautentisering kan skydda mot hello risken för stulna och phished autentiseringsuppgifter. Andra principer för villkorlig åtkomst kan skydda data i din organisation. Till exempel dessutom toorequiring autentiseringsuppgifter du kanske har en princip som endast enheter som registreras i ett system för hantering av mobila enheter som Microsoft Intune har åtkomst till känsliga tjänster för din organisation.

## <a name="prerequisites"></a>Krav
Azure AD villkorlig åtkomst är en funktion i [Azure Active Directory Premium](http://www.microsoft.com/identity). Varje användare som har åtkomst till ett program som har principer för villkorlig åtkomst tillämpas måste ha en Azure AD Premium-licens. Du kan lära dig mer om licenskrav i [olicensierad användaren rapporten](https://aka.ms/utc5ix).

## <a name="how-is-conditional-access-control-enforced"></a>Hur tillämpas villkorlig åtkomstkontroll?
Med villkorlig åtkomstkontroll på plats kontrollerar Azure AD för hello särskilda villkor du anger för en användare tooaccess ett program. När krav är uppfyllda, hello användaren autentiseras och kan komma åt programmet hello.  

![Villkorlig åtkomst: översikt](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Villkor
Dessa är villkor som du kan inkludera i en princip för villkorlig åtkomst:

* **Gruppmedlemskap**. Kontrollera en användares åtkomst baserat på medlemskap i en grupp.
* **Plats**. Använd hello platsen för hello användaren tootrigger multifaktorautentisering och Använd block kontroller när en användare inte är i ett betrott nätverk.
* **Enhetsplattform**. Använd hello plattform, till exempel iOS, Android, Windows Mobile och Windows, som ett villkor för att tillämpa principen.
* **Enheten aktiverad**. Enhetens tillstånd verifieras om aktiveras eller inaktiveras, under utvärdering av principen för enheten. Om du inaktiverar en förlorad eller stulen enhet i hello directory uppfyller den inte längre principkraven.
* **Risken för inloggning och användaren**. Du kan använda [Azure AD Identity Protection](active-directory-identityprotection.md) för principer för villkorlig åtkomst risk. Principer för villkorlig åtkomst risk hjälp i din organisation avancerade skydd baserat på riskhändelser och ovanlig inloggning aktiviteter.

## <a name="controls"></a>Kontroller
Det här är kontroller som du kan använda tooenforce en princip för villkorlig åtkomst:

* **Multifaktorautentisering**. Du kan kräva stark autentisering via multifaktorautentisering. Du kan använda multifaktorautentisering med Azure Multi-Factor Authentication eller genom att använda en lokal flerfunktionsautentiseringsleverantör, tillsammans med Active Directory Federation Services (AD FS). Använda Multi-Factor authentication skyddar resurser från används av en obehörig användare som kan ha fick tillgång toohello autentiseringsuppgifterna för en giltig användare.
* **Blockera**. Du kan använda villkor som användaren plats tooblock användaråtkomst. Du kan till exempel blockera åtkomst när en användare inte är i ett betrott nätverk.
* **Kompatibla enheter**. Du kan ange principer för villkorlig åtkomst på enhetsnivå hello. Du kan ställa in en princip så att endast datorer som är ansluten till domänen eller mobila enheter som är registrerade i en mobil enhet hanteringsprogram kan komma åt företagets resurser. Du kan till exempel använda Intune toocheck enheternas kompatibilitet och rapportera det tooAzure AD för tvingande när hello användare försöker tooaccess ett program. Detaljerad vägledning om hur toouse Intune tooprotect appar och data, se [skydda data och appar med Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Du kan också använda Intune tooenforce dataskydd för borttappade eller stulna enheter. Mer information finns i [skydda dina data med fullständig eller selektiv rensning med Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Program
Du kan tillämpa en princip för villkorlig åtkomst på programnivå hello. Ange åtkomstnivåer för program och tjänster i hello molnet eller lokalt. hello principen är tillämpade direkt toohello webbplats eller tjänst. hello principen aktiveras för åtkomst toohello webbläsare och tooapplications som har åtkomst till hello-tjänsten.

## <a name="device-based-conditional-access"></a>Enhetsbaserad villkorlig åtkomst
Du kan begränsa åtkomst tooapplications från enheter som registreras med Azure AD och som uppfyller specifika villkor. Enhetsbaserad villkorlig åtkomst förhindrar användare som försöker tooaccess hello resurser från en organisations resurser:

* Okänt eller ohanterade enheter.
* Enheter som inte uppfyller hello säkerhetsprinciper organisationen ställa in.

Du kan ange principer baserat på dessa krav:

* **Domänanslutna enheter**. Ange en princip toorestrict åtkomst toodevices som är kopplade tooan lokala Active Directory-domän och som också har registrerats med Azure AD. Den här principen gäller tooWindows arbetsstationer, bärbara datorer och surfplattor enterprise.
  Mer information om hur tooset in automatisk registrering av domänanslutna enheter med Azure AD finns [konfigurera automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
* **Kompatibla enheter**. Ange en princip toorestrict åtkomst toodevices som är markerade **kompatibla** i systemkatalogen för hello management. Den här principen ser till att endast enheter som uppfyller säkerhetsprinciper till exempel framtvinga kryptering av filer på en enhet har åtkomst. Du kan använda den här principen toorestrict åtkomst från hello följande enheter:
  
  * **Windows-domänanslutna enheter**. Hanteras av System Center Configuration Manager (i hello aktuell gren) distribuerat i en hybrid-konfiguration.
  * **Windows 10 Mobile arbets- eller personliga enheter**. Hanteras av Intune eller av ett system för hantering av stöd från tredje part för mobila enheter.
  * **iOS och Android-enheter**. Hanteras av Intune.

Användare som har åtkomst till program som skyddas av en enhetsbaserad principen måste öppna hello programmet från en enhet som uppfyller den här principen. Åtkomst nekas om försökte på en enhet som inte uppfyller principkraven.

Information om hur en enhetsbaserad, tooconfigure principen i Azure AD, se [skapa princip enhetsbaserad villkorlig åtkomst för Azure Active Directory-anslutna program](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Resurser
Se hello efter resurs kategorier och artiklar toolearn mer om att ställa in villkorlig åtkomst för din organisation.

### <a name="multi-factor-authentication-and-location-policies"></a>Principer för multi-Factor authentication och plats
* [Komma igång med villkorlig åtkomst tooAzure AD-anslutna appar baserat på gruppen, plats och multifaktorautentisering principer](active-directory-conditional-access-azuread-connected-apps.md)
* [Program och webbläsare som stöds](active-directory-conditional-access-supported-apps.md)

### <a name="device-based-conditional-access"></a>Enhetsbaserad villkorlig åtkomst
* [Ange princip för enhetsbaserad villkorlig åtkomst för kontrollen tooAzure Active Directory-anslutna program](active-directory-conditional-access-policy-connected-applications.md)
* [Konfigurera automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)
* [Felsökning av problem med Azure Active Directory](active-directory-conditional-access-device-remediation.md)
* [Skydda data på borttappade eller stulna enheter med Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)

### <a name="protect-resources-based-on-sign-in-risk"></a>Skydda resurser baserat på inloggning risk
* [Azure AD identity protection](active-directory-identityprotection.md)

### <a name="next-steps"></a>Nästa steg
* [Villkorlig åtkomst vanliga frågor och svar](active-directory-conditional-faqs.md)
* [Teknisk referens](active-directory-conditional-access-technical-reference.md)

