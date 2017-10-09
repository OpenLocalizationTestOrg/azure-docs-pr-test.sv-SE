---
title: "aaaAzure identitet och säkerhetsmetoder | Microsoft Docs"
description: "Den här artikeln innehåller en uppsättning av bästa praxis för Identitetshantering och kontroll med hjälp av inbyggda funktioner i Azure."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2017
ms.author: yurid
ms.openlocfilehash: af07dfda84758b9124641078ac8f696f725f2bf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure Identitetshantering och åtkomst kontroll säkerhetsmetoder
Många Överväg toobe hello ny gräns Identitetslagret för säkerhet kan ta över rollen ur hello traditionella nätverk till Central. Den här utvecklingen av hello primära pivot för säkerhet kontrolleras och investeringar kommer från hello faktum att nätverket ytgränser har blivit allt porös och den perimeterskydd får inte vara så effektiv som de var en gång tidigare toohello expanderingen av [BYOD](http://aka.ms/byodcg) enheter och molnprogram.

I den här artikeln diskuteras en samling Azure Identitetshantering och säkerhetsmetoder för access control. Följande rekommendationer härleds från våra erfarenhet av [Azure AD](../active-directory/active-directory-whatis.md) och hello erfarenheter från kunder som dig själv.

För varje rekommenderar förklarar vi:

* Vilka hello bra
* Varför du vill tooenable som bästa praxis
* Vad kan vara hello resultat om du inte bästa praxis för tooenable hello
* Möjliga alternativ toohello bästa praxis
* Hur du kan lära dig tooenable hello bästa praxis

Den här Azure Identitetshantering och åtkomst kontroll av säkerhet artikel om bästa praxis är baserad på en konsensus åsikt och Azure plattformsfunktioner och funktioner, eftersom de finns för närvarande hello den här artikeln skrevs. Åsikter och tekniker ändras med tiden och den här artikeln kommer att uppdateras på ett regelbundet tooreflect ändringarna.

Azure identity management och åtkomst kontroll säkerhetsmetoder i den här artikeln omfattar:

* Centralisera din Identitetshantering
* Aktivera enkel inloggning (SSO)
* Distribuera lösenordshantering
* Använda multifaktorautentisering (MFA) för användare
* Använd rollbaserad åtkomstkontroll (RBAC)
* Kontrollen platser där resurser skapas med resource manager
* Guide för utvecklare tooleverage identitetsfunktioner för SaaS-appar
* Övervaka aktivt för misstänkta aktiviteter

## <a name="centralize-your-identity-management"></a>Centralisera din Identitetshantering
Ett viktigt steg mot att skydda din identitet är tooensure att den kan hantera konton från en enda plats om där det här kontot har skapats. Medan hello majoriteten av hello företag IT-organisationer har sina primära konto directory lokalt, hybriddistributioner för molnet finns på hello upphov och det är viktigt att förstå hur toointegrate lokalt och molnet kataloger och ger en smidigt toohello slutanvändaren.

tooaccomplish detta [hybrididentitet](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) scenariot rekommenderar vi två alternativ:

* Synkronisera din lokala katalog med din molnkatalog med Azure AD Connect
* Federera din lokala identitet med en molnbaserad katalog med hjälp av [Active Directory Federation Services](https://msdn.microsoft.com/library/bb897402.aspx) (AD FS)

Organisationer som inte toointegrate sin lokala identitet med sina molnidentitet får ökade administrativa omkostnader hantera konton, vilket ökar sannolikheten för hello av misstag och säkerhetsintrång.

Mer information om Azure AD-synkronisering Läs hello artikeln [integrera dina lokala identiteter med Azure Active Directory](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Aktivera enkel inloggning (SSO)
När du har flera kataloger toomanage detta blir ett administrativa problem inte bara för IT-avdelningen, utan även för användare som har flera lösenord tooremember. Med hjälp av [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) du ger användarna hello möjligheten för Använd hello samma uppsättning autentiseringsuppgifter toosign i och komma åt hello resurser som de behöver, oavsett om resursen är lokalt eller i hello molnet.

Använda enkel inloggning tooenable användare tooaccess sina [SaaS-program](../active-directory/active-directory-appssoaccess-whatis.md) baserat på deras organisationens konto i Azure AD. Detta gäller inte bara för Microsoft SaaS-appar, men även andra appar som [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) och [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Programmet kan vara konfigurerade toouse Azure AD som en [SAML-baserade identitet](../active-directory/fundamentals-identity.md) provider. Azure AD utfärdar inte en token så att de toosign i hello program om de har beviljats åtkomst med hjälp av Azure AD som en säkerhetskontroll. Du kan bevilja åtkomst direkt eller via en grupp att de är medlem i.

> [!NOTE]
> hello beslut toouse SSO påverkar hur du integrerar din lokala katalog med din molnkatalog. Om du vill använda enkel inloggning måste toouse federation, eftersom katalogsynkronisering ger endast [samma inloggning](../active-directory/active-directory-aadconnect.md).
>
>

Organisationer som inte behöver använda enkel inloggning för sina användare och program är mer synliga tooscenarios där användarna har flera lösenord som direkt ökar hello sannolikheten för att användare återanvända lösenord eller med svaga lösenord.

Du kan lära dig mer om Azure AD SSO genom att läsa hello artikel [AD FS-hantering och anpassning med Azure AD Connect](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Distribuera lösenordshantering
I scenarier där du har flera innehavare eller där du vill att tooenable användare för[återställa sina egna lösenord](../active-directory/active-directory-passwords-update-your-own-password.md), är det viktigt att du använder rätt säkerhetsbehörigheter principer tooprevent missbruk. I Azure kan du utnyttja hello självbetjäning lösenordsåterställning och anpassa hello säkerhet alternativ toomeet dina affärsbehov.

Det är särskilt viktigt tooobtain feedback från dessa användare och sig av sina erfarenheter när de försöker tooperform dessa steg. Baserat på dessa upplevelser, utveckla en plan toomitigate potentiella problem som kan uppstå under hello distributionen för en större grupp. Det rekommenderas också att du använder hello [lösenord återställer registrering aktivitetsrapport](../active-directory/active-directory-passwords-get-insights.md) toomonitor hello användare som registrerar.

Organisationer som vill tooavoid lösenord ändra supportsamtal men aktivera användare tooreset sina egna lösenord är mer känslig tooa högre anropet volym toohello helpdesk på grund av problem med toopassword. I organisationer som har flera klienter, är det viktigt att du implementera den här typen av kapacitet och aktivera användare tooperform lösenordsåterställning inom säkerhetsgränser som fastställdes i hello säkerhetsprincipen.

Du kan lära dig mer om lösenordsåterställning genom att läsa hello artikel [distribuera lösenordshantering och utbildning användarna toouse den](../active-directory/active-directory-passwords-best-practices.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Använda multifaktorautentisering (MFA) för användare
För organisationer som behöver toobe följer branschstandarder, t.ex [PCI DSS version 3.2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32), Multi-Factor authentication är ett måste ha funktionen för att autentisera användare. Utöver att vara kompatibel med branschstandarder, framtvinga MFA tooauthenticate användare kan också hjälpa organisationer toomitigate autentiseringstypen stöld av angrepp, t.ex [Pass-the-Hash (PtH)](http://aka.ms/PtHPaper).

Genom att aktivera Azure MFA för dina användare kan du lägger till ett andra säkerhetslager säkerhet toouser inloggningar och transaktioner. I det här fallet en transaktion kan att komma åt ett dokument som finns på en filserver eller i SharePoint Online. Azure MFA hjälper även IT tooreduce hello sannolikheten att avslöjade autentiseringsuppgifter kommer att ha åtkomst tooorganization data.

Exempel: du införa Azure MFA för användarna och konfigurera den toouse ett telefonsamtal eller SMS som verifiering. Om hello användarens autentiseringsuppgifter komprometteras kan hello angripare kommer inte att kunna tooaccess alla resurser eftersom han inte har åtkomst till toouser phone. Organisationer som inte lägga till extra skyddslager identitet är mer känslig för autentiseringsuppgifter attack med lösenordsstöld, vilket kan leda till röjande av toodata.

Ett alternativ för organisationer som vill tookeep hello hela autentiseringen kontroll lokalt är toouse [Azure Multi-Factor Authentication-servern](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), kallas även MFA lokala. Du kommer fortfarande att kunna tooenforce multifaktorautentisering, samtidigt som hello MFA server lokalt med hjälp av den här metoden.

Mer information om Azure MFA finns hello artikel [komma igång med Azure Multi-Factor Authentication i molnet hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Använd rollbaserad åtkomstkontroll (RBAC)
Begränsa åtkomst baserat på hello [måste tooknow](https://en.wikipedia.org/wiki/Need_to_know) och [minsta privilegium](https://en.wikipedia.org/wiki/Principle_of_least_privilege) säkerhetsprinciper är viktigt för organisationer som vill tooenforce säkerhetsprinciper för dataåtkomst. Azure rollbaserad åtkomstkontroll (RBAC) kan vara används tooassign behörigheter toousers, grupper och program för ett visst område. hello omfånget för en rolltilldelning kan vara en prenumeration, resursgrupp eller en enskild resurs.

Du kan utnyttja [inbyggda RBAC](../active-directory/role-based-access-built-in-roles.md) roller i Azure tooassign privilegier toousers. Överväg att använda *Storage-konto deltagare* för molnoperatörer som behöver toomanage storage-konton och *klassiska Storage-konto deltagare* rollen toomanage klassiska lagringskonton. För molnoperatörer som behöver toomanage virtuella datorer och storage-konto, Överväg att lägga till dem för*Virtual Machine-deltagare* roll.

Organisationer som inte behöver använda data åtkomstkontroll genom att utnyttja funktioner, till exempel RBAC kan ger fler behörigheter än nödvändigt tootheir användare. Detta kan leda till röjande av toodata som ger användare åtkomst toocertain typer av datatyper (t.ex. stora marknadsfördelar) som de inte borde ha hello första plats.

Du kan lära dig mer om Azure RBAC genom att läsa hello artikel [rollbaserad åtkomstkontroll i](../active-directory/role-based-access-control-configure.md).

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Kontrollen platser där resurser skapas med resource manager
Det är mycket viktigt att aktivera molnet operatörer tooperform uppgifter när hindrar dem från att bryta konventioner som behövs toomanage organisationens resurser. Organisationer som vill toocontrol hello platser där resurser skapas bör hård code dessa platser.

tooachieve kan organisationer kan skapa säkerhetsprinciper som har definitioner som beskriver hello åtgärder eller resurser som uttryckligen har nekats. Du tilldelar dessa principdefinitioner hello önskad definitionsområdet, till exempel hello prenumeration, resursgrupp eller en enskild resurs.

> [!NOTE]
> Detta är inte Hej samma som RBAC, den faktiskt använder RBAC tooauthenticate hello användare som har behörigheten toocreate resurserna.
>
>

Utnyttjar [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toocreate anpassade principer för scenarier där hello organisationen vill tooallow åtgärder endast när hello lämpliga kostnadsställe är också associerade; annars hello begäran ska nekas.

Organisationer som inte styr hur resurserna skapas är mer känslig toousers som kan missbruka hello-tjänsten genom att skapa fler resurser än de behöver. Härdning av hello resurs-processen är ett viktigt steg toosecure ett scenario med flera innehavare.

Du kan lära dig mer om hur du skapar principer med Azure Resource Manager genom att läsa hello artikel [använda princip toomanage resurser och kontrollera åtkomst](../azure-resource-manager/resource-manager-policy.md).

## <a name="guide-developers-tooleverage-identity-capabilities-for-saas-apps"></a>Guide för utvecklare tooleverage identitetsfunktioner för SaaS-appar
Användaridentitet ska utnyttjas i många situationer när användare har åtkomst till [SaaS-appar](https://azure.microsoft.com/marketplace/active-directory/all/) som kan integreras med lokala eller molnbaserade directory. Först och främst måste vi rekommenderar att utvecklare använder en säker metod toodevelop dessa appar som [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). Azure AD förenklar autentisering för utvecklare genom att tillhandahålla identitet som en tjänst med stöd för standardiserade protokoll som [OAuth 2.0](http://oauth.net/2/) och [OpenID Connect](http://openid.net/connect/), samt öppen källkod bibliotek för olika plattformar.

Se till att tooregister alla program som outsources autentisering tooAzure AD, detta är en obligatorisk procedur. hello beror bakom detta på eftersom Azure AD måste toocoordinate hello kommunikation med hello programmet när du hanterar inloggning (SSO) eller byta ut tokens. hello användarens session upphör att gälla när hello livstid hello-token som utfärdas av Azure AD upphör att gälla. Utvärdera alltid om programmet ska använda den här gången eller om du kan minska den här gången. Livslängd för att minska hello kan fungera som en säkerhetsåtgärd som tvingar användare toosign ut baserat på en period av inaktivitet.

Organisationer som inte behöver använda identitet Kontrollera tooaccess appar och inte leder utvecklarens på hur toosecurely integrera appar med deras identity management-systemet kan vara svårare toocredential stöld av typen av angrepp, t.ex [svaga autentisering och session management som beskrivs i öppna Web Application säkerhet projekt (OWASP) topp 10](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet).

Du kan lära dig mer om autentiseringsscenarier för SaaS-appar genom att läsa [Autentiseringsscenarier för Azure AD](../active-directory/active-directory-authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Övervaka aktivt för misstänkta aktiviteter
Enligt för[Verizon 2016 Data intrång rapporten](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/), avslöjade autentiseringsuppgifter är fortfarande i hello upphov och blir en hello mest lönsamma företag för cyber tjuvar. Därför är det viktigt toohave en aktiv identitetssystem Övervakare som snabbt kan identifiera misstänkt beteendeaktivitet och utlösa en avisering om ytterligare undersökning. Azure AD har två viktiga funktioner som hjälper organisationer att övervaka sina identiteter: Azure AD Premium [avvikelseidentifiering rapporter](../active-directory/active-directory-view-access-usage-reports.md) och Azure AD [identitetsskydd](../active-directory/active-directory-identityprotection.md) kapaciteten.

Se till att toouse hello avvikelseidentifiering rapporter tooidentify försök toosign i [utan som spåras](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md), [brute force](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) attacker mot ett särskilt konto försöker toosign i från flera platser, logga in från [infekterade enheter](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md) och misstänkta IP-adresser. Tänk på att de är rapporter. Du måste alltså ha processer och procedurer i placera för IT-administratörer toorun rapporterna hello dag eller på begäran (vanligtvis i ett scenario för incidenter).

Däremot Azure AD identity protection är en aktiv övervakningssystem och den kommer flaggan hello aktuella risker på sin egen instrumentpanel. Förutom att får du även dagliga sammanfattning meddelanden via e-post. Vi rekommenderar att du justerar hello risknivå enligt tooyour affärsbehov. hello risknivå för en risk är en indikation (hög, medel eller låg) på hello allvarlighetsgraden hello risk händelse. hello risknivå hjälper användarna att identitetsskydd prioritera hello åtgärder de måste vidta tooreduce hello risk tootheir organisation.

Organisationer som inte aktivt övervakar sina identitetssystem finns risk för att användarens autentiseringsuppgifter komprometteras. Placera utan vetskap misstänkta aktiviteter tar med dessa autentiseringsuppgifter och organisationer kommer inte att kunna toomitigate den här typen av hot.
Du kan lära dig mer om Azure Identity protection genom att läsa [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md).
