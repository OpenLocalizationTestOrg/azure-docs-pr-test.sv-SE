---
title: "Azure villkorlig åtkomst för SaaS-appar | Microsoft Docs"
description: "Villkorlig åtkomst i Azure AD kan du konfigurera regler för åtkomst av programspecifika multifaktorautentisering och möjligheten att blockera åtkomst för användare inte på ett betrott nätverk. "
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: efaa70467346e910a78a63d182041029bb34b1cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Komma igång med Azure Active Directory villkorlig åtkomst
Azure Active Directory villkorlig åtkomst för [SaaS](https://azure.microsoft.com/overview/what-is-saas/) appar och Azure AD anslutna appar kan du konfigurera villkorlig åtkomst baserat på gruppen, plats och känslighet för programmet. 

Med villkorad tillgång baserat på känslighet för programmet, kan du ange åtkomstregler för multifaktorautentisering (MFA) per program. MFA per program ger dig möjlighet att blockera åtkomst för användare som inte är i ett betrott nätverk. Du kan använda MFA regler för alla användare som är kopplade till programmet, eller enbart för användare i de angivna säkerhetsgrupperna.  Användare kan uteslutas från MFA-kravet om de använder programmet från en IP-adress som är inne i organisationens nätverk.

Dessa funktioner blir tillgängliga för kunder som har köpt en Azure Active Directory Premium-licens.

## <a name="scenario-prerequisites"></a>Krav för scenario
* Licens för Azure Active Directory Premium
* Federerade eller hanterade Azure Active Directory-klient
* Federerade klienter kräver att multifaktorautentisering har aktiverats.

## <a name="configure-per-application-access-rules"></a>Konfigurera åtkomstregler för per program
Det här avsnittet beskrivs hur du konfigurerar åtkomstregler per program.

1. Logga in på den klassiska Azure-portalen med ett konto som är en global administratör för Azure AD.
2. Välj **Active Directory** i det vänstra fönstret.
3. Välj din katalog på fliken katalog.
4. Välj den **program** fliken.
5. Välj det program som regeln ska anges för.
6. Välj fliken **Konfigurera**.
7. Rulla ned till avsnittet åtkomst regler. Välj önskad Åtkomstregeln.
8. Ange vilka användare som regeln gäller för.
9. Aktivera principen genom att välja **aktiverad på**.

## <a name="understanding-access-rules"></a>Förstå åtkomstregler
Det här avsnittet ger en detaljerad beskrivning av åtkomstregler som stöds i den Azure villkorlig åtkomst till program.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Ange användare åtkomstregler som gäller för
Som standard gäller principen för alla användare som har åtkomst till programmet. Du kan också begränsa principen till användare som är medlemmar i de angivna säkerhetsgrupperna. Den **Lägg till grupp** knappen används för att välja en eller flera grupper som regeln ska gälla för dialogrutan för val av grupp. Den här dialogrutan kan också användas för att ta bort valda grupper. När reglerna har markerats för att använda för grupper, tillämpas endast regler för åtkomst för användare som tillhör en av de angivna säkerhetsgrupperna.

Säkerhetsgrupper kan uttryckligen undantas från principen genom att välja den **utom** alternativet och ange en eller flera grupper. Användare som är medlem i en grupp i den **utom** listan kommer inte att omfattas multifaktorautentisering krav, även om de är medlem i en grupp som regeln gäller.
Alla användare i gruppen Ansvariga för att kunna använda multifaktorautentisering vid åtkomst till programmet kräver den åtkomstregel som visas nedan.

![Ange regler för villkorlig åtkomst med MFA](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Regler för villkorlig åtkomst med MFA
Om en användare har konfigurerats med hjälp av funktionen per användare multifaktorautentisering, kommer den här inställningen på användaren kombinera med multifaktorautentisering reglerna för appen. Det innebär att en användare som har konfigurerats för multifaktorautentisering för per användare behöver utföra multifaktorautentisering, även om de har undantagits från programmet multifaktorautentisering regler. Läs mer om Multi-Factor authentication och användarspecifika inställningar.

### <a name="access-rule-options"></a>Åtkomst till regelalternativ
Stöd för följande alternativ:

* **Kräver Multi-Factor authentication**: användare som gäller åtkomstregler till, kommer att behöva fullständig multifaktorautentisering innan du använder det program som principen gäller för.
* **Kräver Multi-Factor authentication när de inte är på arbetet**: en användare som kommer från en betrodd IP-adress inte behöver utföra multifaktorautentisering. Betrodda IP-adressintervall kan konfigureras på inställningssidan för multifaktorautentisering.
* **Blockera åtkomst när de inte är på arbetet**: en användare som inte kommer från en betrodd IP-adress blockeras. Betrodda IP-adressintervall kan konfigureras på inställningssidan för multifaktorautentisering.

### <a name="setting-rule-status"></a>Anger Regelstatus
Åtkomststatus för regeln kan aktivera regler eller inaktivera. Krav för multifaktorautentisering tillämpas inte när det finns regler för åtkomst av.

### <a name="access-rule-evaluation"></a>Utvärderingen av distributionsregeln åtkomst
Åtkomstregler utvärderas när en användare ansluter till en federerad program som använder OAuth 2.0, OpenID Connect, SAML och WS-Federation. Dessutom utvärderas åtkomstregler när OAuth 2.0 och OpenID Connect använder en uppdateringstoken för att erhålla en åtkomst-token. Om principutvärdering misslyckas när du använder en uppdateringstoken felet **invalid_grant** returneras, betyder det att användaren behöver återautentisera till klienten.

### <a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Konfigurera federation services för att tillhandahålla multifaktorautentisering
För federerade klienter MFA kan utföras genom Azure Active Directory eller lokal AD FS-servern.

Som standard sker MFA på en sida hos Azure Active Directory. Så här konfigurerar du MFA lokalt, i **– SupportsMFA** egenskapen måste anges till **SANT** i Azure Active Directory med hjälp av Azure AD-modulen för Windows PowerShell.

I följande exempel visas hur du aktiverar Multifaktorautentisering för lokalt med hjälp av den [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) för contoso.com innehavaren:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Utöver att ställa in den här flaggan måste federerade klient AD FS-instans konfigureras för att utföra multifaktorautentisering. Följ instruktionerna för [distribuerar Azure Multi-Factor Authentication lokala](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Relaterade artiklar
* [Skydda åtkomst till Office 365 och andra appar ansluten till Azure Active Directory](active-directory-conditional-access.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

