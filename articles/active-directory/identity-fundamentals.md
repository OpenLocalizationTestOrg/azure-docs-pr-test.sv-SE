---
title: aaaFundamentals av Azure Identitetshantering | Microsoft Docs
description: 
keywords: 
author: jeffgilb
manager: femila
ms.reviewr: jsnow
ms.author: jeffgilb
ms.date: 7/5/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: a9710b8e543cdbb2f78ea9e3f83b183e1983b31d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fundamentals-of-azure-identity-management"></a>Grunderna i Azure Identitetshantering
Eftersom fler och fler digitala företagsresurser live utanför hello företagsnätverket, i hello moln och på enheter, blir nödvändigt med en bra molnbaserade identitets- och lösning. Molnbaserad identiteter är nu hello bästa sätt toomaintain kontroll över och insyn i hur och när användare åtkomst till företagets program och data.

Microsoft har skydda molnbaserade identiteter för över tio år och nu med [Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), skydd datorerna är tillgängliga tooyou. Med Azure AD Kontrollera Företagsadministratörer enkelt användar- och accountability med bättre säkerhet och styrning än någonsin tidigare.

Azure AD Premium är en molnbaserad identitets- och lösning med avancerade funktionerna som gör att en säker identitet för alla appar, identitetsskydd (med hello [Microsoft intelligence säkerhet graph](https://www.microsoft.com/en-us/security/intelligence)), och Privileged Identity Management. Inte en övervakning eller reporting verktyget Azure AD Premium kan skydda användaridentiteter i realtid och aktiverar du toocreate risk-baserade, anpassningsbar åtkomst principer tooprotect din organisations data.

Titta på den här korta videon för en snabb översikt över Azure AD identity hanteringen och skyddet:
<iframe width="560" height="315" src="https://www.youtube.com/embed/9LGIJ2-FKIM" frameborder="0" allowfullscreen></iframe>

Microsoft ger inte bara en identitet som tar dig överallt, men också en uppsättning verktyg tooautomate att skydda och hantera IT inom din organisation. Även efter hello ankomsten av molntjänster finns fortfarande kräver toomanage och styra IT-uppgifter som att supportavdelningen anropar tooreset användarlösenord, gruppera användare hanterings- och programförfrågningar. Vilket komplicerar saker ytterligare anställda tar nu med sig sina personliga enheter toowork och använder tillgängliga SaaS-program. Det gör att underhålla kontroll över sina program över företagets datacenter och offentliga molnplattformar svårigheter.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Ansluta lokala Active Directory med Azure AD och Office 365
Organisationer som har gjort stora investeringar i lokala Active Directory kan utöka de investeringar toohello molnet genom att integrera sina lokala kataloger med Azure AD till [hybrididentitetshantering](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Det gör användarna mer produktiva genom att tillhandahålla en gemensam identitet för åtkomst till resurser oavsett plats. Användare och organisationer kan sedan använda enkel inloggning (SSO) tooaccess både lokala resurser och molntjänster som Office 365.

[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) är hello enda verktyg du behöver tooget hello integration är klar. Azure AD Connect innehåller funktioner toosupport din identitetssynkronisering måste och ersätter äldre versioner av identitetsintegrationsverktyg som DirSync och Azure AD Sync. Med Azure AD Connect har aktiverats för Identitetshantering och synkronisering mellan lokala och Azure AD via:

- Synkronisering – Den här komponenten är ansvarig för att skapa användare, grupper och andra objekt. Det är också ansvarig för att kontrollera identitetsinformationen för dina lokala användare och grupper matchar hello molnet. Tillbakaskrivning av lösenord kan också vara aktiverat tookeep lokala kataloger synkroniseras när användaren uppdaterar sina lösenord i Azure AD.
- AD FS - Federation är en valfri funktion som tillhandahålls av Azure AD Connect som kan använda tooconfigure en hybridmiljö med hjälp av en lokal AD FS-infrastruktur. Federation kan användas av organisationer tooaddress komplexa distributioner, till exempel enkel inloggning vid tillämpning av AD-inloggning princip och smartkort eller tredje parts MFA.
- Hälsoövervakning – [Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health) kan tillhandahålla robust övervakning och ger en central plats i hello Azure portal tooview den här aktiviteten.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Öka produktiviteten och minska supportkostnaderna med självbetjäning och enkel inloggning

Anställda är mer produktiva när de har en enda tooremember användarnamn och lösenord och en konsekvent upplevelse från varje enhet. Sparar också tid när de kan utföra självbetjäning aktiviteter som [återställer glömt lösenordet](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) eller begär åtkomst tooan programmet utan att vänta på att få hjälp från hello supportavdelningen.

Azure AD [utökar lokala Active Directory-](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) i hello moln att aktivera användare toouse primära organisationskontot för både sina domänanslutna enheter företagets resurser och alla hello webb- och SaaS-program de måste toouse tooget jobbet. Dessutom baserat toonot med tooremember flera uppsättningar av användarnamn och lösenord, användarnas programåtkomst kan också automatiskt etablera (eller tas bort) på deras organisation gruppmedlemskap och deras status som en medarbetare. Och du kan kontrollera att åtkomsten för galleriet appar eller egna lokala appar som du har utvecklats och publicerats via hello [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="manage-and-control-access-toocorporate-resources"></a>Hantera och kontrollera åtkomst toocorporate resurser
Microsoft identitets- och lösningar hjälp IT skydda åtkomst tooapplications och resurser över hello företagets datacenter och i hello moln aktivera nivåer av verifiering som [multifaktorautentisering](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) och [principer för villkorlig åtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Misstänkt aktivitet med avancerad säkerhet rapportering, granskning och aviseringar hjälper dig att minimera potentiella säkerhetsproblem.

Principer för villkorlig åtkomst i Azure AD Premium ger dig, hello företagsadministratör, hello möjlighet toocreate principbaserad åtkomstregler för alla Azure AD-anslutna program (SaaS-appar, anpassade appar som körs i hello-molnet eller lokala webbprogram). Azure AD utvärderar dessa principer i realtid och tillämpar dem när en användare försöker tooaccess ett program. Azure identity protection-principer kan du vidta åtgärder för tooautomatically om misstänkt aktivitet har identifierats. De här åtgärderna kan inkludera blockerar åtkomst toousers hög risk, framtvinga multifaktorautentisering, och återställa lösenord om det ser ut autentiseringsuppgifter har komprometterats.


## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory Privileged Identity Management

[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started), medföljer hello Azure Active Directory Premium P2 erbjudande, kan du toodiscover, begränsa och övervaka administrativa konton och deras åtkomst tooresources i din Azure Active Directory och andra Microsoft online services. Du kan dessutom administrera på begäran administrativ åtkomst för hello exakt tid du behöver.

Privileged Identity Management kan tillämpa administratörsbehörighet på begäran så att administratörer kan begära Multi-Factor autentiserade, tillfällig rättighetsökning sina för förkonfigurerade tidsperioder innan sina konton returnerar tooa normal användarens tillstånd.

## <a name="benefits-of-azure-identity"></a>Fördelarna med Azure identitet

Med Azure identity management kan du:

-   Skapa och hantera en enstaka identitet för varje användare i hela företaget, synkronisera användare, grupper och enheter med [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

-   Ge åtkomst för enkel inloggning tooyour program, bland annat tusentals förintegrerade SaaS-appar eller ge säker fjärråtkomst tooon lokala SaaS-program med hello [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

-   Aktivera åtkomst programsäkerhet genom att tillämpa regelbaserad [Multifaktorautentisering](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) både lokala och molnbaserade program.

-   Förbättra produktiviteten för användaren med [Självbetjäning för lösenordsåterställning](https://docs.microsoft.com/azure/active-directory/active-directory-passwords), och grupp och programmet begäranden med hello [MyApps portal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-user-help).

-   Dra nytta av hello [hög tillgänglighet och tillförlitlighet](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications) av över hela världen, företagsklass, molnbaserade identitets- och hanteringslösning.

## <a name="next-steps"></a>Nästa steg
[Mer information om identitetslösningar i Azure](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions)