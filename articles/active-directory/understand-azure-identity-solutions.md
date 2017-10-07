---
title: aaaUnderstand Azure Identity | Microsoft Docs
description: "Hämta en grundläggande förståelse för Microsoft Azure identitet lösning villkoren, begrepp och rekommendationer för du toomake hello bästa identitet styrning beslut för din organisation."
keywords: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/17/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: 4d9c90bd7a6bcc9637be3107998f9da5bd4cbdae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-identity-solutions"></a>Förstå identitetslösningar i Azure
Microsoft Azure Active Directory (AD Azure) är ett identitets- och molnet hanteringslösning som tillhandahåller katalogtjänster, identitet styrning och hantering av åtkomst. Azure AD snabbt [aktiverar enkel inloggning (SSO)](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) too1 000 förintegrerade kommersiella och anpassade appar i hello [Azure AD application gallery](https://azure.microsoft.com/marketplace/active-directory/all/). Många av de här apparna som du förmodligen redan använder till exempel Office 365, Salesforce.com, rutan, ServiceNow och Workday.

En enda Azure AD-katalog associeras automatiskt med en Azure-prenumeration när den skapas. Som hello identitetstjänst i Azure innehåller Azure AD sedan alla Identitetshantering och access control-funktioner för molnbaserade resurser. Dessa resurser kan innehålla användare, appar och grupper för en enskild klientorganisation (organisation) som visas i följande diagram hello:

![Azure Active Directory](./media/understand-azure-identity-solutions/azure-ad.png)

Microsoft Azure erbjuder flera sätt tooleverage identitet som en tjänst (IDaaS) med olika behörighetsnivåer komplexitet toomeet enskilda organisationens behov. hello resten av den här artikeln hjälper dig att förstå grundläggande Azure identity termer och begrepp som rekommendationer för du toomake hello bäst från hello tillgängliga alternativ.

## <a name="terms-tooknow"></a>Villkoren tooknow

Innan du kan välja i en Azure identity-lösningen för din organisation, måste en grundläggande förståelse för hello termer som används när man talar om Azure identity services.

|Termen tooknow| Beskrivning|
|-----|-----|
|Azure-prenumeration |Prenumerationer är används toopay för Azure-molntjänster och är vanligtvis länkade tooa kreditkort. Du kan ha flera prenumerationer, men det kan vara svårt tooshare resurser mellan prenumerationer.|
|Azure-klient | En Azure AD-klient är av en enda organisation. Det är en dedikerad, betrodda instans av Azure AD som skapas automatiskt när en organisation registrerar sig för en Microsoft cloud service-prenumeration som Azure, Intune och Office 365. Klienter kan få åtkomst till tooservices antingen i en dedikerad miljö (enstaka klient) eller i en miljö med delad med andra organisationer (multitenant).|
|Azure AD-katalog | Varje Azure-klient har en dedikerad, betrodda Azure AD-katalog som innehåller hello klient användare, grupper och program. Det är används tooperform identitets- och hanteringsfunktioner för klientresurser. Eftersom en unik Azure AD-katalog är automatiskt etablerade toorepresent din organisation när du registrerar dig för en Microsoft-molntjänst som Azure, Microsoft Intune eller Office 365, ibland visas hello villkoren *klient*, *Azure AD*, och *Azure AD-katalog* utbytbara. |
|Egen domän | När du först registrerar dig för en prenumeration på Microsoft cloud tjänster, din klient (organisation) använder en *. onmicrosoft.com* domännamn. De flesta organisationer har dock en eller flera domännamn som används toodo företag och att användarna använder tooaccess företagets resurser. Du kan lägga till din anpassade domän namn tooAzure AD så att hello domännamn är bekant tooyour användare som  *alice@contoso.com*  i stället för  *alice@contoso.onmicrosoft.com* . |
|Azure AD-konto | Dessa är identiteter som skapas med hjälp av Azure AD eller en annan Microsoft-molntjänst som Office 365. De lagras i Azure AD och tillgänglig tooany av hello organisation prenumerationer på molntjänster. |
|Administratör för Azure-prenumeration| hello kontoadministratör är hello person som registrerade sig för eller köpt hello Azure-prenumeration. De kan använda hello [Kontocenter](https://account.windowsazure.com/Home/Index) tooperform olika hanteringsuppgifter som skapa prenumerationer, avbryta prenumerationer, ändra hello fakturering för en prenumeration eller ändra hello tjänstadministratör. |
|Global administratör för Azure AD | Azure AD globala administratörer har fullständig åtkomst tooall Azure AD administrativa funktioner. hello person som registrerar sig för en prenumeration på Microsoft cloud tjänster blir automatiskt en global administratör som standard. Du kan ha fler än en global administratör, men endast globala administratörer kan tilldela någon av [hello andra administratörsroller](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) toousers. |
|Microsoft-konto | Microsoft-konton (som skapats av du för personligt bruk) ger åtkomst tooconsumer indatavärdena Microsoft-produkter och molntjänster, t.ex. Outlook (Hotmail), OneDrive, Xbox LIVE eller Office 365. Dessa identiteter skapas och lagras i hello Microsoft-konto konsumentidentitetssystemet körs av Microsoft.|
|Arbets-eller skolkonton | Arbets- eller skolkonto konton (som utfärdats av en administratör för företag/academic) ger åtkomst tooenterprise affärsnivå Microsofts molntjänster, till exempel Azure, Intune och Office 365.|


## <a name="concepts-toounderstand"></a>Begrepp toounderstand

Nu när du vet hello grundläggande Azure identity villkor bör du läsa mer om dessa Azure identity-begrepp som hjälper dig fatta ett välgrundat Azure identity service beslut.

|Konceptet toounderstand |Beskrivning|
|-----|-----|
|[Hur Azure-prenumerationer är associerade med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) |Alla Azure-prenumerationer har en förtroenderelation med en Azure AD directory tooauthenticate användare, tjänster och enheter. *Flera prenumerationer kan lita hello samma Azure AD-katalog, men en prenumeration litar bara på en enda Azure AD-katalog*. Den här förtroenderelationen skiljer sig från hello-relation med en prenumeration med andra Azure-resurser (webbplatser, databaser och så vidare), som är mer som underordnade resurser till en prenumeration. Om en prenumeration upphör att gälla sedan stoppas åtkomst tooresources som är associerade med prenumerationen hello än Azure AD också. Dock hello Azure AD-katalogen finns kvar i Azure, så att du kan associera en annan prenumeration med katalogen och fortsätta toomanage klientresurser.|
|[Hur Azure AD-licensiering fungerar](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-get-started-azure-portal) | När du köper eller aktiverar Enterprise Mobility Suite, Azure AD Premium eller Azure AD Basic, katalogen uppdateras med hello-prenumeration, inklusive giltighetsperioden och förskottsbetald licenser. När hello prenumeration är aktiv, hello service hanteras av Azure AD globala administratörer och används av licensierade användare. Din prenumerationsinformation hello antalet tilldelade eller tillgängliga licenser är tillgängliga i hello Azure-portalen från hello **Azure Active Directory** > **licenser** bladet. Detta är också hello bästa plats toomanage licenstilldelningar.|
|[Rollbaserad åtkomstkontroll i hello Azure-portalen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is)|Azure rollbaserad åtkomstkontroll (RBAC) ger detaljerad åtkomsthantering för Azure-resurser. För många behörigheter kan exponera och kontot tooattackers. För få behörigheter innebär att anställda kan få arbetet gjort effektivt. Med RBAC kan du ge anställda hello exakt behörigheter de behöver baserat på tre grundläggande roller som gäller tooall resursgrupper: ägare, deltagare och läsare. Du kan också skapa in too2 000 egen [anpassade RBAC-roller](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) toomeet dina särskilda behov. |
|[Hybrididentitet](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)|Hybrididentitet uppnås genom att integrera dina lokala Windows Server Active Directory (AD DS) med Azure AD med hjälp av [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Detta gör att du tooprovide en gemensam identitet för dina användare för Office 365, Azure, och lokala appar eller SaaS-program som är integrerade med Azure AD. Med hybrid identity utöka du effektivt din lokala miljö toohello molnet för identitets- och.|

### <a name="hello-difference-between-windows-server-ad-ds-and-azure-ad"></a>hello skillnaden mellan Windows Server AD DS och AD Azure
Både Azure Active Directory (Azure AD) och lokala Active Directory (AD DS eller Active Directory Domain Services) är system som lagrar katalogdata och hanterar kommunikation mellan användare och resurser, inklusive användarprocesser inloggning, autentisering, och katalogsökningar.

Om du redan är bekant med lokala Windows Server Active Directory Domain Services (AD DS) introducerades med Windows 2000 Server och sedan du förmodligen förstår hello grundläggande koncept för en identity-tjänsten. Det är dock också viktigt toounderstand att Azure AD inte är bara en domänkontrollant i hello molnet. Det är ett helt nytt sätt för att tillhandahålla identitet som en tjänst (IDaaS) i Azure som kräver ett helt nytt sätt att tänka toofully embrace molnbaserade funktioner och skydda din organisation mot aktuella hot. 

AD DS är en serverroll på Windows Server, vilket innebär att den kan användas på fysiska eller virtuella datorer. Den har en hierarkisk struktur utifrån X.500. DNS används för att hitta objekt, kan vara har åtgärdat med LDAP och främst används Kerberos för autentisering. Active Directory aktiverar organisationsenheter (OU) och grupprincipobjekt (GPO) dessutom toojoining datorer toohello domän och förtroenden skapas mellan domäner.

IT har skyddat deras säkerhet perimeternätverk för år med hjälp av AD DS, men moderna, perimeter mindre företag stöder identitet behov för anställda, kunder och partner kräver en ny kontrollplan. Azure AD är detta plan för kontroll av identiteten. Säkerhet är bortom hello företagsbrandvägg toohello moln där Azure AD skyddar företagets resurser och åtkomst genom att tillhandahålla en gemensam identitet för användare (antingen lokalt eller i molnet hello). Detta ger din användare hello flexibilitet toosecurely åtkomst hello appar de behöver tooget sitt arbete från nästan alla enheter. Sömlös risk-baserade data protection kontroller, backas upp av machine learning-funktioner och detaljerad rapportering finns också IT behov tookeep företaget säkra data.

Azure AD är en offentlig katalogtjänst för flera kunder vilket innebär att du kan skapa en klient för molnservrar och program, till exempel Office 365 i Azure AD. Användare och grupper skapas i en platt struktur utan organisationsenheter eller grupprincipobjekt. Autentiseringen utförs via protokoll, till exempel SAML WS-Federation och OAuth. Det är möjligt tooquery Azure AD, men i stället för att använda LDAP måste du använda en REST-API som kallas AD Graph API. Dessa alla fungerar över HTTP och HTTPS.

### <a name="extend-office-365-management-and-security-capabilities"></a>Utöka Office 365-funktioner för hantering och säkerhet
Redan använder Office 365? Du kan påskynda din digitala transformation genom att utöka inbyggda funktioner för Office 365 med Azure AD toosecure dina resurser, aktivera säker produktivitet för personalen hela. När du använder Azure AD kan dessutom tooOffice 365 funktioner du kan skydda din portfölj för hela programmet med en identitet som möjliggör enkel inloggning för alla appar. Du kan expandera din funktioner för villkorlig åtkomst utifrån inte bara enheten tillstånd, men användaren, plats och program samt risk. Multifaktorautentisering (MFA) ger ytterligare skydd när du behöver den. Du får ytterligare tillsyn av behörigheter och ge på begäran, just-in-time administrativ åtkomst till. Användarna kommer vara mer produktiva, och skapa färre supportavdelningen biljetter, ger tack toohello självbetjäningsfunktionerna Azure AD som återställer bortglömda lösenord, programförfrågningar, och skapa och hantera grupper.

> [!TIP]
> Vill du toolearn mer om hur du använder Azure AD identity management med Office 365? [Hämta hello e-bok](https://info.microsoft.com/Extend-Office-365-security-with-EMS.html).

## <a name="microsoft-azure-identity-solutions"></a>Identitetslösningar i Microsoft Azure

Microsoft Azure tillhandahåller flera olika sätt toomanage användarnas identiteter om de underhålls fullständigt lokalt, i hello moln eller till och med någonstans mellan. Dessa alternativ inkluderar: själv (DIY) AD DS i Azure, Azure Active Directory (Azure AD), Hybrididentitet och Azure AD Domain Services.

### <a name="do-it-yourself-diy-ad-ds"></a>Själv (själv) AD DS
För företag som behöver en kompakta i hello molnet **själv (DIY) AD DS** i Azure kan användas. Det här alternativet stöder många scenarier för Windows Server AD DS som lämpar sig väl för distribution som virtuella datorer (VM) på Azure. Du kan till exempel skapa en Azure VM som en domänkontrollant som körs i ett avlägset datacenter som är anslutna toohello fjärrnätverk. Därifrån hello VM skulle kunna toosupport autentiseringsbegäranden från externa användare och förbättra prestanda. Det här alternativet är också väl lämpade som en relativt billig substitute toootherwise kostsamma katastrofåterställningsplatser genom att lägga upp ett litet antal domänkontrollanter och ett enda virtuellt nätverk i Azure. Slutligen kan du behöver toodeploy ett program på Azure, t.ex SharePoint, som kräver Windows Server AD DS, men har inga beroende hello lokala nätverk eller hello företagets Windows Server Active Directory. I det här fallet kan du distribuera en isolerad skog på Azure toomeet hello SharePoint server servergruppens krav. Det har också stöd toodeploy nätverksprogram som kräver anslutning toohello lokala nätverk och hello lokala Active Directory.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (AD Azure)
**Azure AD-fristående** är en helt molnbaserade identitets- och åtkomsthantering som en tjänst (IDaaS). Azure AD innehåller kraftfulla och stabila funktioner toomanage användare och grupper. Det hjälper att skydda åtkomst tooon lokala program och molnprogram, inklusive Microsoft web-tjänster som Office 365 och många icke-Microsoft-programvara som en tjänst (SaaS)-program. Azure AD finns i tre versioner: ledigt, grundläggande och Premium. Azure AD förstärker företagets effektivitet och utökar säkerhet utöver hello perimeter brandväggen tooa nya kontrollplan som skyddas av Azure machine learning och andra avancerade säkerhetsfunktioner.

### <a name="hybrid-identity"></a>Hybrid-identitet
I stället för att välja mellan lokala eller molnbaserade identitets-lösningar, utökar många vanlig tänka IT-chefer och företag som har börjat förutseende företagets framtidens, sina lokala kataloger toohello molnet via **hybrididentitet** lösningar. Du får en verkligen global identitet och åtkomst-hanteringslösning som ger säker och effektiv åtkomst toohello program användare måste toodo sitt arbete med hybrid identity.

> [!TIP]
> toolearn mer om hur IT-chefer har gjort Azure Active Directory en central del av sina IT-strategier hämta hello [its Guide tooAzure Active Directory](https://aka.ms/AzureADCIOGuide).

### <a name="azure-ad-domain-services"></a>Azure AD Domain Services
**Azure AD Domain Services** innehåller en molnbaserad alternativet toouse AD DS för lightweight Azure VM configuration kontroll och en sätt toomeet lokalt identitetskrav för nätverksutveckling och testning. Azure AD Domain Services är inte avsedd toolift och flytta dina lokala AD DS-infrastruktur tooAzure virtuella datorer som hanteras av Azure AD Domain Services. Hello Azure virtuella datorer i hanterade domäner ska i stället använda toosupport hello utveckling, testning och flytt av lokala program som kräver AD DS-autentisering metoder toohello moln.

## <a name="common-scenarios-and-recommendations"></a>Vanliga scenarier och rekommendationer

Här följer några vanliga scenarier för identitets- och rekommendationer som toowhich Azure identitetsalternativ är mest lämpligt för varje.

|Scenario med identiteter| Rekommendation|
|-----|-----|
|Min organisation har gjort stora investeringar i lokala Windows Server Active Directory, men vi vill tooextend identitet toohello moln.| hello används mest Azure identitetslösning är [hybrididentitet](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Om du redan gjort investeringar i lokala AD DS, kan du enkelt utöka identitet toohello moln med Azure AD Connect.|
|Mitt företag föddes i hello molnet och vi har inga investeringar i lokala identitetslösningar.| [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) hello bästa valet för endast molnbaserad företag utan lokalt investeringar.|
|Jag behöver lightweight Azure VM konfigurations- och toomeet lokalt identitetskrav för app-utveckling och testning.|[Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) är bra om du behöver toouse AD DS för kontroll av lightweight Azure VM-konfiguration eller letar toodevelop eller migrera äldre, katalog-medvetna lokala program toohello moln.|  
|Jag behöver toosupport några virtuella datorer i Azure, men mitt företag fortfarande kraftigt har investerat i lokala Active Directory (AD DS).|Använd [själv AD DS](https://msdn.microsoft.com/library/azure/jj156090.aspx) toouse virtuella Azure-datorer när du behöver toosupport några virtuella datorer och har stor AD DS investeringar lokalt. |

## <a name="where-can-i-learn-more"></a>Var kan jag lära sig mer?
Vi har massor av bra resurser online toohelp du lär dig allt om Azure AD. Här är en lista över bra artiklar tooget du startade:

* [Aktivera din katalog för hybridhantering av med Azure AD Connect](active-directory-aadconnect.md)
* [Ytterligare säkerhet för ett någonsin anslutna world](../multi-factor-authentication/multi-factor-authentication.md)
* [Automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](active-directory-saas-app-provisioning.md)
* [Komma igång med Azure AD Reporting](active-directory-reporting-getting-started.md)
* [Hantera lösenord från valfri plats](active-directory-passwords.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](active-directory-saas-app-provisioning.md)
* [Hur säkra tooprovide fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md)
* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Vad är Microsoft Azure Active Directory-licensiering?](active-directory-licensing-what-is.md)
* [Hur kan identifiera ej sanktionerade molnappar som används inom organisationen](active-directory-cloudappdiscovery-whatis.md)

## <a name="next-steps"></a>Nästa steg

Nu när du förstår Azure identity begrepp och hello alternativ tillgängliga tooyou, kan du använda följande resurser tooget igång implementerar hello alternativ du har valt hello:

[Lär dig mer om Azure hybrididentitetslösningar](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution)

[Läs mer i en Azure Proof of Concept-miljö](https://aka.ms/aad-poc)

[Distribuera Azure AD i produktion](https://aka.ms/aad-onboard)
