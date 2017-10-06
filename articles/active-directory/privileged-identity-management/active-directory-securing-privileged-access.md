---
title: "aaaSecuring privilegierad åtkomst i Azure AD | Microsoft Docs"
description: "Ett avsnitt som förklarar hello närmar sig för att skydda privilegierad åtkomst i Azure, Azure Active Directory och Microsoft Online Services."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: 694835dc5c41640673dbd996d44b0d1f217220de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a>Skydda privilegierad åtkomst i Azure AD
Skydda privilegierad åtkomst är ett viktigt första steg toohelp skydda företagets tillgångar i en modern organisation. Privilegierade konton är konton som administrera och hantera IT-system. Cyber angripare rikta dessa konton toogain åtkomst tooan organisationens data och datorer. toosecure privilegierad åtkomst, bör du isolera hello konton och system från hello risken att exponeras tooa angripare.

Fler användare har börjat tooget privilegierad åtkomst via molntjänster. Detta kan inkludera globala administratörer för Office 365, Azure-prenumerationsadministratörer och användare som har administratörsbehörighet i virtuella datorer eller på SaaS-appar.

Microsoft rekommenderar att du följer den här vägledningen [skydda privilegierad åtkomst](https://technet.microsoft.com/library/mt631194.aspx).

För kunder som använder Azure Active Directory, Office 365 eller andra Microsoft-tjänster och program, tillämpa dessa principer om användarkonton hanteras och har autentiserats av Active Directory eller i Azure Active Directory. hello följande avsnitt innehåller mer information om Azure AD-funktioner toosupport skydda privilegierad åtkomst.

## <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication
Du bör kräva tvåstegsverifiering innan du beviljar behörighet tooincrease hello säkerhet för autentisering av administratörer. Tvåstegsverifiering är en metod för att verifiera vem du är som kräver hello använder mer än bara ett användarnamn och lösenord. Det ger ett andra säkerhetslager säkerhet toouser inloggningar och transaktioner.

Azure Multi-Factor Authentication (MFA) är Microsofts tvåstegsverifiering verifiering lösning som hjälper dig att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning. Den ger stark autentisering via en mängd alternativ för enkel verifiering inklusive:

- telefonsamtal
- Textmeddelanden
- mobilapp-meddelanden
- koder för mobilappar
- OATH-token från tredje part

En översikt över hur Azure Multi-Factor Authentication fungerar finns i följande video hello:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

Mer information finns i [MFA för Office 365 och Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Tidsbundna privilegier
Vissa organisationer kan hitta de har för många användare i mycket Privilegierade roller. En användare kan ha lagts toohello roll för en viss aktivitet som toosign dig för en tjänst, men använder inte dessa behörigheter ofta efteråt.

toolower hello tidsperioden privilegier och öka din insyn i deras användning, begränsa användare tooonly tar på sina privilegier just-in-time ”(JIT) när de behöver tooperform en aktivitet. Du kan använda för Azure Active Directory och Microsoft Online Services [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).

![PIM-instrumentpanelen][2]

## <a name="attack-detection"></a>Angreppsidentifiering
[Azure Active Directory-identitetsskydd](../active-directory-identityprotection.md) ger en samlad vy riskhändelser och potentiella säkerhetsproblem som påverkar din organisations identiteter. Baserat på riskhändelser identitetsskydd beräknar en användare risknivå för varje användare, vilket gör att du tooconfigure risk-baserade policys tooautomatically skydda hello identiteter i din organisation. Dessa principer tillsammans med andra kontroller för villkorlig åtkomst som tillhandahålls av Azure Active Directory och EMS, kan automatiskt blockerar hello användare eller erbjuda förslag som inkluderar lösenordsåterställning och multifaktorautentisering tvingande.

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a>Villkorlig åtkomst
Med villkorlig åtkomstkontroll kontrollerar hello särskilda villkor som du väljer när en användare autentiseras innan åtkomst tooan program i Azure Active Directory. När dessa villkor är uppfyllda, hello användare autentiseras och tillåtet åtkomst toohello program.

![Ange regler för villkorlig åtkomst med MFA][4]

## <a name="related-articles"></a>Relaterade artiklar
* Aktivera [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Aktivera [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)
* Aktivera [Azure AD Identity Protection](../active-directory-identityprotection.md)
* Aktivera [villkorlig åtkomstkontroll](../active-directory-conditional-access.md)

Mer information om hur du skapar en plan för fullständig säkerhet avsnittet hello ”kundens ansvarsområden och översikt över” i hello [Microsoft Cloud Security för Enterprise-arkitekter](http://aka.ms/securecustomer) dokumentet. Mer information om att engagera er tooassist för Microsoft-tjänster med något av dessa avsnitt, kontakta din Microsoft-representant eller besök vår [Cybersecurity lösningar sidan](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
