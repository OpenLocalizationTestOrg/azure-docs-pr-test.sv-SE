---
title: "aaaAzure villkorlig åtkomst för SaaS-appar | Microsoft Docs"
description: "Villkorlig åtkomst i Azure AD kan du tooconfigure programspecifika multifaktorautentisering åtkomstregler och hello möjlighet tooblock åtkomst för användare inte på ett betrott nätverk. "
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
ms.openlocfilehash: 69748014c0c8e266ba66562980c784aba4c68d80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Komma igång med Azure Active Directory villkorlig åtkomst
Azure Active Directory villkorlig åtkomst för [SaaS](https://azure.microsoft.com/overview/what-is-saas/) appar och Azure AD anslutna appar kan du konfigurera villkorlig åtkomst baserat på gruppen, plats och känslighet för programmet. 

Med villkorad tillgång baserat på känslighet för programmet, kan du ange åtkomstregler för multifaktorautentisering (MFA) per program. MFA per program ger hello möjlighet tooblock åtkomst för användare som inte är i ett betrott nätverk. Du kan använda MFA regler tooall användare som är tilldelade toohello program, eller enbart för användare i säkerhetsgrupper som angetts.  Användare kan uteslutas från hello MFA-kravet om de använder hello program från en IP-adress som finns inuti hello organisationens nätverk.

Dessa funktioner blir tillgängliga toocustomers som har köpt en Azure Active Directory Premium-licens.

## <a name="scenario-prerequisites"></a>Krav för scenario
* Licens för Azure Active Directory Premium
* Federerade eller hanterade Azure Active Directory-klient
* Federerade klienter kräver att multifaktorautentisering har aktiverats.

## <a name="configure-per-application-access-rules"></a>Konfigurera åtkomstregler för per program
Det här avsnittet beskrivs hur tooconfigure per program åt regler.

1. Logga in toohello klassiska Azure-portalen med ett konto som är en global administratör för Azure AD.
2. Hello vänster, Välj **Active Directory**.
3. På fliken för hello katalog väljer du din katalog.
4. Välj hello **program** fliken.
5. Välj hello-program som hello regeln ställs in för.
6. Välj hello **konfigurera** fliken.
7. Rulla ned toohello åtkomst regler avsnitt. Välj hello önskad regel.
8. Ange hello användare hello regeln gäller för.
9. Aktivera hello princip genom att välja **aktiverad toobe på**.

## <a name="understanding-access-rules"></a>Förstå åtkomstregler
Det här avsnittet ger en detaljerad beskrivning av hello åtkomstregler som stöds i hello Azure villkorlig åtkomst till program.

### <a name="specifying-hello-users-hello-access-rules-apply-to"></a>Att ange hello användare hello åtkomst regler gäller för
Som standard tillämpas hello Grupprincip tooall användare som har åtkomst till toohello program. Men du kan också begränsa hello princip toousers som är medlemmar i hello angivna säkerhetsgrupper. Hej **Lägg till grupp** är knappen används tooselect en eller flera grupper från dialogrutan hello grupp som hello Åtkomstregeln gäller för. Den här dialogrutan kan också vara används tooremove valda grupper. När hello regler är valda tooapply tooGroups, hello åtkomstregler tillämpas endast för användare som tillhör tooone av hello angivna säkerhetsgrupper.

Säkerhetsgrupper kan uttryckligen undantas från hello princip genom att välja hello **utom** alternativet och ange en eller flera grupper. Användare som är medlem i en grupp i hello **utom** listan kommer inte att ämne toohello multifaktorautentisering krav, även om de är medlem i en grupp som hello Åtkomstregeln gäller för.
hello Åtkomstregeln nedan kräver alla användare i hello chefer grupp toouse multifaktorautentisering vid åtkomst till programmet hello.

![Ange regler för villkorlig åtkomst med MFA](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Regler för villkorlig åtkomst med MFA
Om en användare har konfigurerats med hello per användare multifaktorautentisering funktionen, kommer den här inställningen på hello användaren kombinera med hello multifaktorautentisering regler för hello app. Det innebär att en användare som har konfigurerats för multifaktorautentisering för varje användare kommer att nödvändiga tooperform multifaktorautentisering även om de har undantagits från hello programmet multifaktorautentisering regler. Läs mer om Multi-Factor authentication och användarspecifika inställningar.

### <a name="access-rule-options"></a>Åtkomst till regelalternativ
följande alternativ för hello stöds:

* **Kräver Multi-Factor authentication**: hello användare toowhom hello åtkomstregler gäller för, kommer att nödvändiga toocomplete multifaktorautentisering innan åtkomst till hello-program som hello principen gäller för.
* **Kräver Multi-Factor authentication när de inte är på arbetet**: en användare som kommer från en betrodd IP-adress är inte obligatoriska tooperform multifaktorautentisering. hello betrodda IP-adressintervall kan konfigureras på inställningssidan för hello multifaktorautentisering.
* **Blockera åtkomst när de inte är på arbetet**: en användare som inte kommer från en betrodd IP-adress blockeras. hello betrodda IP-adressintervall kan konfigureras på inställningssidan för hello multifaktorautentisering.

### <a name="setting-rule-status"></a>Anger Regelstatus
Åtkomststatus för regeln kan aktivera hello regler eller inaktivera. När hello åtkomstregler är inaktiverade, tillämpas inte hello multifaktorautentisering krav.

### <a name="access-rule-evaluation"></a>Utvärderingen av distributionsregeln åtkomst
Åtkomstregler utvärderas när en användare ansluter till en federerad program som använder OAuth 2.0, OpenID Connect, SAML och WS-Federation. Dessutom utvärderas åtkomstregler när hello OAuth 2.0 och OpenID Connect använder du en uppdatera token tooacquire en åtkomst-token. Om principutvärdering misslyckas när du använder en uppdateringstoken hello fel **invalid_grant** returneras, innebär hello användare behöver toore-autentisering toohello klient.

### <a name="configure-federation-services-tooprovide-multi-factor-authentication"></a>Konfigurera federation services tooprovide Multi-Factor authentication
För federerade klienter MFA kan utföras genom Azure Active Directory eller hello lokala AD FS-servern.

Som standard sker MFA på en sida hos Azure Active Directory. tooconfigure MFA lokalt, hello **– SupportsMFA** egenskapen måste anges för**SANT** i Azure Active Directory med hjälp av hello Azure AD-modulen för Windows PowerShell.

hello följande exempel visas hur tooenable lokalt MFA med hjälp av hello [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) hello contoso.com innehavaren:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

I tillägg toosetting denna flagga, hello federerade klient AD FS-instansen måste vara konfigurerad tooperform multifaktorautentisering. Följ anvisningarna för hello för [distribuerar Azure Multi-Factor Authentication lokala](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Relaterade artiklar
* [Skydda åtkomsten tooOffice 365 och andra appar anslutna tooAzure Active Directory](active-directory-conditional-access.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

