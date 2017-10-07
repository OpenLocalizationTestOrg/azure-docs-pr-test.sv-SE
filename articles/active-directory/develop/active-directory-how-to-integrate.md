---
title: aaaHow tooIntegrate med Azure Active Directory | Microsoft Docs
description: "En guide toobenefits av och resurser för integrering med Azure Active Directory."
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: d13bba54-96bd-4b81-bee9-c8025ffa1648
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 4542965ae4a7756eda9f57e9e895f8044892f20e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-with-azure-active-directory"></a>Integrera med Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory kan organisationer företagsklass Identitetshantering för molnprogram.  Azure AD-integrering och ger användarna en effektiviserad upplevelse inloggning och hjälper ditt program som överensstämmer tooIT princip.

## <a name="how-toointegrate"></a>Hur tooIntegrate
Det finns flera sätt för dina program toointegrate med Azure AD.  Dra nytta av så många eller som fåtal av dessa scenarier som är lämpligt för ditt program.

### <a name="support-azure-ad-as-a-way-toosign-in-tooyour-application"></a>Stöd för Azure AD som ett sätt tooSign i tooYour program
**Minska inloggning friktion och minska kostnaderna för support.** Med hjälp av Azure AD toosign i tooyour program kan användarna inte en mer tooremember för namn och lösenord.  Som utvecklare bör du ha en mindre lösenord toostore och skydda.  Inte har toohandle glömt lösenordsåterställning kanske enbart betydande besparingar.  Azure AD används logga in för några av hello world mest populära molnprogram, inklusive Office 365 och Microsoft Azure.  Med flera hundra miljoner användare från organisationer miljontals risken kan dina användare är redan inloggad tooAzure AD.  Lär dig mer om [lägger till stöd för Azure AD inloggning](active-directory-authentication-scenarios.md).

**Förenkla logga in för ditt program.**  När du registrerar dig för programmet kan Azure AD skicka viktig information om en användare så att du före fylla din registrering formulär eller ta bort den helt.  Användare kan registrera dig för ditt program med hjälp av sina Azure AD-kontot via en bekant medgivande upplevelse liknande toothose hittades för sociala medier och mobila program.  Alla användare kan logga och logga in tooan program som är integrerade med Azure AD utan inblandning av IT.  Lär dig mer om [registrerat dig ditt program för Azure AD-kontot inloggningen](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-tooyour-application"></a>Sök efter användare, hantera Användaretablering och kontrollera åtkomst tooYour program
**Sök efter användare i katalogen hello.**  Använda hello Graph API toohelp användare Sök- och bläddringsfunktioner för andra personer i organisationen när bjuda in andra eller bevilja åtkomst, i stället för att de tootype-e-postadresser.  Användare kan bläddra i en bekant adress book style-gränssnittet, inklusive visar hello information i hello organisationens hierarki.  Mer information om hello [Graph API](active-directory-graph-api.md).

**Använd Active Directory-grupper och distributionslistor kunden redan hanterar igen.**  Azure AD innehåller hello grupper som kunden redan använder för e-distribution och hantering av åtkomst.  Använda hello Graph API kan återanvända dessa grupper i stället för att din kund toocreate och hantera en separat uppsättning grupper i ditt program.  Information om grupp kan även skickas tooyour program i inloggning token.  Mer information om hello [Graph API](active-directory-graph-api.md).

**Använda Azure AD-toocontrol som har åtkomst till tooyour program.**  Administratörer och ägare i Azure AD kan tilldela åtkomst tooapplications toospecific användare och grupper.  Med hello Graph API kan du läsa den här listan och använda den toocontrol etablering och avetablering av resurser och komma åt i ditt program.

**Använda Azure AD för roller-baserad åtkomstkontroll.**  Administratörer och ägare kan tilldela användare och grupper tooroles som du definierar när du registrerar ditt program i Azure AD.  Rollinformation skickas tooyour program i inloggning token och kan också läsa med hello Graph API.  Lär dig mer om [med hjälp av Azure AD för auktorisering](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-toousers-profile-calendar-email-contacts-files-and-more"></a>Få åtkomst tooUser profil, kalender, e-post, kontakter, filer och mer
**Azure AD är hello auktorisering server för Office 365 och andra Microsoft-företagstjänster.**  Om du stöder Azure AD för inloggning tooyour program eller länka dina aktuella användare konton tooAzure AD användarkonton med hjälp av OAuth 2.0-stöd, kan du begära Läs och skrivbehörighet tooa användarens profil, kalender, e-post, kontakter, filer och annan information.  Du kan sömlöst skriva händelser toouser kalender och läsa eller skriva filer tootheir OneDrive.  Lär dig mer om [åtkomst till hello Office 365-API: er](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-hello-azure-and-office-365-marketplaces"></a>Uppgradera ditt program i hello Azure och Office 365 marknadsplatser
**Uppgradera ditt program toohello miljoner för organisationer som redan använder Azure AD.**  Användare söka och bläddra dessa marknadsplatser använder redan en eller flera molntjänster, gör dem kvalificerade kunder för tjänst i molnet.  Mer information om att uppgradera ditt program i [hello Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**När användare loggar för ditt program, visas den i Azure AD-åtkomstpanelen och Office 365 app starta.**  Användare kommer att kunna tooquickly och enkelt returnerade tooyour programmet senare, förbättra användaren engagement.  Mer information om hello [Azure AD-åtkomstpanelen](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Säker kommunikation för enhet-till-tjänst och tjänster
**Använda Azure AD för Identitetshantering av enheter och tjänster minskar hello kod du behöver toowrite och möjliggör IT toomanage åtkomst.**  Tjänster och enheter kan hämta token från Azure AD med hjälp av OAuth och använda dessa token tooaccess web API: er.  Använda Azure AD kan du undvika skriva Autentiseringskod för sammansatt.  Eftersom hello identiteter hello tjänster och enheter lagras i Azure AD kan IT kan hantera nycklar och återkallade certifikat på en plats i stället för att toodo detta separat i ditt program.

## <a name="benefits-of-integration"></a>Fördelar med integrering
Integrering med Azure AD ingår fördelar som du inte behöver toowrite ytterligare kod.

### <a name="integration-with-enterprise-identity-management"></a>Integrering med Enterprise Identity Management
**Hjälpa ditt program som överensstämmer med IT-principer.**  Organisationer integrera sina företagssystem identity management med Azure AD, så när en person lämnar organisationen, de kommer automatiskt att förlora åtkomst tooyour program utan IT behöver tootake extra steg.  IT kan hantera vem som kan komma åt programmet och ta reda på vilka principer för åtkomst krävs – för exempel Multi-Factor authentication - minskar behovet av toowrite kod toocomply med komplexa företagets principer.  Azure AD ger administratörer en detaljerad granskningsloggen för som inloggad tooyour program så IT kan spåra användning.

**Azure AD utökar Active Directory toohello molnet så att programmet kan integreras med AD.**  Många organisationer runt hello world använder Active Directory UPN-inloggning och identity management-systemet och kräva sina program toowork med AD.  Appen integrerar med Azure AD kan integreras med Active Directory.

### <a name="advanced-security-features"></a>Funktioner för avancerad säkerhet
**Multifaktorautentisering.**  Azure AD innehåller interna multifaktorautentisering.  IT-administratörer kan kräva multifaktorautentisering tooaccess ditt program så att du inte har toocode detta stöd själv.  Lär dig mer om [Multifaktorautentisering](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Avvikande inloggning identifiering.**  Azure AD bearbetar fler än en miljard inloggningar per dag när du använder datorn learning algoritmer toodetect misstänkt aktivitet och meddela IT-administratörer för möjliga problem.  Programmet hämtar hello fördelen med det här skyddet genom att stödja Azure AD-inloggning. Lär dig mer om [Visa rapport för Azure Active Directory-åtkomst](../active-directory-view-access-usage-reports.md).

**Villkorlig åtkomst.**  Dessutom toomulti-factor-autentisering, Administratörer kan kräva att vissa villkor vara uppfyllda innan användarna kan logga in tooyour program.  Villkoren som du kan ange omfattar hello IP-adressintervall för klientenheter, medlemskap i angivna grupper och hello tillstånd för hello-enhet som används för åtkomst.  Lär dig mer om [villkorlig åtkomst i Azure Active Directory](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Enkel utveckling
**Standardprotokollen.**  Microsoft är allokerat toosupporting branschstandarder.  Azure AD stöder hello SAML 2.0, OpenID Connect 1.0, OAuth 2.0 och WS-Federation 1.2 autentiseringsprotokoll.  hello Graph API är OData 4.0-kompatibel.  Om programmet redan stöder hello SAML 2.0 eller OpenID Connect 1.0-protokoll för federerad inloggning, kan det vara enkelt att lägga till stöd för Azure AD.  Lär dig mer om [autentiseringsprotokoll som stöds av Azure AD](active-directory-authentication-protocols.md).

**Bibliotek med öppen källkod.**  Microsoft tillhandahåller bibliotek med öppen källkod för fullt stöd för populära plattformar och språk toospeed utveckling.  hello källkoden är licensierad under Apache 2.0 och du ledigt toofork och bidra tillbaka toohello projekt.  Lär dig mer om [Azure AD-autentiseringsbibliotek](active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Världen och hög tillgänglighet
**Azure AD har distribuerats i datacenter runt hello world och hanteras och övervakas runt hello klockan.**  Azure AD är hello identity management-systemet för Microsoft Azure och Office 365 och har distribuerats i 28 Datacenter hello världen.  Katalogdata garanteras toobe replikeras tooat minst tre datacenter.  Globala belastningsutjämnare Kontrollera användare komma åt hello närmaste kopia av Azure AD som innehåller data och automatiskt omdirigera begäranden tooother Datacenter om ett problem har upptäckts.

## <a name="next-steps"></a>Nästa steg
[Börja skriva kod](active-directory-developers-guide.md#get-started).

[Logga In med hjälp av Azure AD användare](active-directory-authentication-scenarios.md)

