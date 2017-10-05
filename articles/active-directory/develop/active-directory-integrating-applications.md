---
title: Integrera program med Azure Active Directory | Microsoft Docs
description: "Information om hur du lägger till, uppdatera eller ta bort ett program i Azure Active Directory (AD Azure)."
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: mbaldwin
ms.assetid: ae637be5-0b71-4b1e-b1fe-b83df3eb4845
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: lenalepa
ms.custom: aaddev
ms.reviewer: luleon
ms.openlocfilehash: 3be341bcb897a1481f145825429a1a94dfaae3b0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="integrating-applications-with-azure-active-directory"></a>Integrera program med Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Företagsutvecklare och SaaS-leverantörer (software-as-a-service) kan utveckla kommersiella molntjänster och affärsprogram som kan integreras med Azure Active Directory (Azure AD) och då ger säker inloggning och behörighet till tjänsterna. Om du vill integrera en applikation eller tjänst med Azure AD, måste en utvecklare först registrera information om sina program med Azure AD via den klassiska Azure-portalen.

Den här artikeln visar hur du lägger till, uppdatera eller ta bort ett program i Azure AD. Får du lära dig om olika typer av program som kan integreras med Azure AD, hur du konfigurerar ditt program för att komma åt andra resurser, till exempel web API: er och mycket mer.

Läs mer om de två Azure AD-objekt som representerar ett registrerat program och relationen mellan dem i [program och tjänstens huvudnamn objekt](active-directory-application-objects.md); Läs mer om riktlinjer för varumärkesanpassning bör du använda när du utvecklar program med Azure Active Directory, se [företagsanpassning riktlinjer för integrerade appar](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Lägga till ett program
Alla program som du vill använda funktionerna i Azure AD måste först registreras i Azure AD-klient. Registrering under processen ger Azure AD-information om ditt program, till exempel URL där den finns, URL: en att skicka svar när en användare autentiseras den URI som identifierar app och så vidare.

Du kan bara följa anvisningarna nedan om du utvecklar ett program som behöver stöd för inloggning för användare i Azure AD. Om ditt program måste autentiseringsuppgifter eller behörighet att komma åt ett webb-API eller behöver Tillåt att användare från andra Azure AD-klienter för att komma åt den finns [uppdatering av ett program](#updating-an-application) avsnittet för att fortsätta konfigurera ditt program.

### <a name="to-register-a-new-application-in-the-azure-portal"></a>Att registrera ett nytt program i Azure-portalen
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i det övre högra hörnet på sidan.
3. I det vänstra navigeringsfönstret väljer **fler tjänster**, klickar du på **App registreringar**, och klicka på **Lägg till**.
4. Följ anvisningarna och skapa ett nytt program. Om du vill att specifika exempel för webbprogram och interna program, Kolla in våra [Snabbstart](active-directory-developers-guide.md).
  * Webbprogram, ange den **inloggnings-URL**, vilket är den grundläggande Webbadressen för din app, där användarna kan logga in t.ex `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Interna program, ange en **omdirigerings-URI**, som används i Azure AD för att returnera token svar. Ange ett värde som är specifikt för ditt program, till exempel `http://MyFirstAADApp`
5. När du har slutfört registreringen, tilldelar Azure AD ditt program ett unikt klient-ID, program-ID. Programmet har lagts till och du kommer till sidan Snabbstart för ditt program. Beroende på om ditt program är en webbplats eller det ursprungliga programmet, ser du olika alternativ för hur du lägger till ytterligare funktioner i ditt program. När programmet har lagts till kan börja du uppdatera programmet så att användarna kan logga in, åtkomst till webb-API: er i andra program eller konfigurera program för flera innehavare (vilket gör att andra organisationer kan komma åt programmet).

> [!NOTE]
> Som standard konfigureras den nyligen skapade appregistrering så att användare från din katalog att logga in på ditt program.
> 
> 

## <a name="updating-an-application"></a>Uppdatering av ett program
När programmet har registrerats med Azure AD, kan den behöva uppdateras för att ge åtkomst till webb-API: er, göras tillgänglig i andra organisationer och mycket mer. Det här avsnittet beskrivs olika sätt som du kan behöva konfigurera ytterligare ditt program. Först börjar vi med en översikt över medgivande-ramverket som är viktigt att förstå om du skapar resursen/API-program som används av klientprogram som skapats av utvecklarna i din organisation eller en annan organisation.

Mer information om hur autentisering fungerar i Azure AD, se [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md).

### <a name="overview-of-the-consent-framework"></a>Översikt över medgivande framework
Azure AD medgivande framework gör det enkelt att utveckla flera innehavare webb- och interna program som behöver åtkomst till webb-API: er som skyddas av en Azure AD-klient, som skiljer sig från den där klientprogrammet har registrerats. Dessa webb-API: er är Microsoft Graph API (för att få åtkomst till Azure Active Directory, Intune och tjänster i Office 365) och andra Microsoft-tjänster API: er, utöver egna web API: er. Ramen är baserad på en användare eller administratör ge medgivande till ett program som begär registreras i katalogen, vilket kan innebära att få åtkomst till katalogdata.

Till exempel om ett webbprogram för klienten behöver läsa kalenderinformationen om användaren från Office 365, behöver att användaren samtycker till att klientprogrammet. Efter tillstånd ges kommer klientprogrammet att kunna anropa Microsoft Graph API för användarens räkning och använda kalenderinformationen efter behov. Den [Microsoft Graph API](https://graph.microsoft.io) ger åtkomst till data i Office 365 (till exempel kalendrar och meddelanden från Exchange, platser och listor från SharePoint, dokument från OneDrive, uppgifter från Planner arbetsböcker från Excel, OneNote-anteckningsböcker osv), samt användare och grupper från Azure AD och andra dataobjekt från flera Microsoft-molntjänster. 

Medgivande framework bygger på OAuth 2.0 och dess olika flöden som koden bevilja och klienten autentiseringsuppgifter bevilja med hjälp av offentliga eller konfidentiell klienter. Med hjälp av OAuth 2.0 går Azure AD det att skapa många olika typer av program, t.ex på en telefon, surfplatta, server eller ett webbprogram och få åtkomst till resurserna som krävs.

Mer detaljerad information om ditt medgivande framework finns [OAuth 2.0 i Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx), [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md), och för information om hur du får en auktoriserad åtkomst till Office 365 via Microsoft Graph finns [App autentisering med Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

#### <a name="example-of-the-consent-experience"></a>Exempel på medgivande-upplevelse
Följande steg visar hur samtycke uppleva fungerar för både programutvecklaren och användare.

1. Ange de behörigheter som krävs för ditt program med hjälp av menyerna i avsnittet nödvändiga behörigheter på din klient webbprogrammets konfigurationssidan i Azure-portalen.
   
    ![Behörigheter för andra program](./media/active-directory-integrating-applications/requiredpermissions.png)
2. Överväg att ditt program behörighet har uppdaterats programmet körs och en användare kommer att använda den för första gången. Om programmet redan inte har ingått ett åtkomst eller uppdatera token, som programmet behöver att gå till Azure AD autentiseringsslutpunkt att hämta en Auktoriseringskoden som kan användas för att få en ny tillgång och uppdatera token.
3. Om användaren redan inte är autentiserad uppmanade de att ange att logga in på Azure AD.
   
    ![Användaren eller administratören logga in på Azure AD](./media/active-directory-integrating-applications/usersignin.png)
4. När användaren har loggat in avgör Azure AD om användaren behöver visas en sida medgivande. Detta baseras på om användaren (eller organisationens administratör) har redan beviljats programmet samtycke. Om medgivande inte redan har fått, uppmanas användaren efter medgivande Azure AD och visar de behörigheter som krävs som behövs för att fungera. En uppsättning behörigheter som visas i dialogrutan medgivande är samma som vad du har valt i den delegerade behörigheter i Azure-portalen.
   
    ![Medgivande användarupplevelse](./media/active-directory-integrating-applications/consent.png)
5. När användaren ger ditt medgivande, returneras ett auktoriseringskod till ditt program som kan lösas för att få en åtkomst-token och uppdatera token. Mer information om det här flödet finns i [webbprogrammet för API-avsnittet](active-directory-authentication-scenarios.md#web-application-to-web-api) i avsnittet [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md).

6. Som administratör kan samtycker du också till att ett program delegerade behörigheter för alla användare i din klient. Detta förhindrar att medgivande dialogrutan som visas för varje användare i klienten. Du kan göra detta från den [Azure-portalen](https://portal.azure.com) från sidan program. Från den **inställningar** bladet för programmet, klickar du på **nödvändiga behörigheter** och klicka på den **bevilja med** knappen. 

    ![Bevilja behörigheter för explicit admin medgivande](./media/active-directory-integrating-applications/grantpermissions.png)
    
> [!NOTE]
> Bevilja uttryckligt medgivande med hjälp av den **bevilja med** knappen krävs för närvarande för enstaka sida program (SPA) med hjälp av ADAL.js, som den åtkomst-token har begärts utan en medgivandetext misslyckas om medgivande inte är redan beviljats.   

### <a name="configuring-a-client-application-to-access-web-apis"></a>Konfigurera ett klientprogram att komma åt web API: er
För ett webbprogram konfidentiell klientprogram för att kunna delta i ett tillstånd bevilja flöde som kräver autentisering (och få en åtkomsttoken), måste den upprätta säkra referenser. Standardmetoden för autentisering som stöds av Azure portal är klient-ID + symmetrisk nyckel. Det här avsnittet beskriver de konfigurationssteg som krävs för att ange den hemliga nyckeln för din klient autentiseringsuppgifter.

Dessutom innan en klient kan åtkomst till webb-API exponeras av en resursprogram (ie: Microsoft Graph API), medgivande framework säkerställer klienten hämtar behörighet beviljande krävs, baserat på de behörigheter som begärdes. Som standard kan alla program välja behörigheter från Azure Active Directory (Graph API) och Azure Service Management API med Azure AD-”aktivera inloggning och Läs användarprofil” behörigheten har redan valts som standard. Om klientprogrammet registreras i en Azure AD-klient för Office 365, vara web API: er och behörigheter för SharePoint och Exchange Online också väljas. Du kan välja mellan [två typer av behörigheter](active-directory-dev-glossary.md#permissions) i de nedrullningsbara menyerna bredvid den önskade webb-API:

* Behörigheter för program: Ditt klientprogram behöver åtkomst till webb-API som själva (ingen användarkontext). Den här typen av behörighet kräver godkännande av administratören och är inte heller tillgänglig för interna program.
* Delegerade behörigheter: Ditt klientprogram behöver åtkomst till webb-API som den inloggade användaren, men med åtkomst begränsas av den valda behörigheten. Den här typen av behörighet kan beviljas av en användare om behörigheten har konfigurerats som kräver godkännande av administratören. 

> [!NOTE]
> Lägga till en delegerad behörighet till ett program ger automatiskt inte medgivande till användare i klienten, som i den klassiska Azure-portalen. Användarna måste fortfarande manuellt medgivande för tillagda delegerade behörigheter vid körning, om inte administratören klickar på den **bevilja med** knappen från den **nödvändiga behörigheter** avsnitt i den sidan program i Azure-portalen. 

#### <a name="to-add-credentials-or-permissions-to-access-web-apis"></a>Om du vill lägga till webb autentiseringsuppgifter eller behörighet att komma åt-API: er
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i det övre högra hörnet på sidan.
3. Välj på den översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på det program du vill konfigurera. Detta kommer till programmets QuickStart sida, samt öppna bladet inställningar för programmet.
4. Klicka på avsnittet ”nycklar” i bladet inställningar för att lägga till en hemlig nyckel för autentiseringsuppgifterna för ditt webbprogram.  
   
   * Lägg till en beskrivning för din nyckel och välj antingen 1 eller 2 år varaktighet. 
   * Kolumnen längst till höger innehåller värdet för nyckeln, när du sparar ändringar i konfigurationen. Tänk på att gå tillbaka till det här avsnittet och kopiera den när du klickar på Spara, så att du har för användning i ditt klientprogram under autentiseringen vid körning.
5. Klicka på ”krävs” Säkerhetsavsnittet i bladet inställningar för att lägga till behörigheter för åtkomst till resursen API: er från klienten. 
   
   * Klicka på knappen ”Lägg till” först.
   * Klicka på ”Välj ett API” för att välja vilken typ av resurser som du vill välja från.
   * Bläddra igenom listan över tillgängliga API: er eller Använd sökrutan Om du vill välja från tillgänglig resurs för program i din katalog som exponerar ett webb-API. Klicka på den resurs som du är intresserad av och klickar sedan **Välj**.
   * När du valt kan du gå till den **Välj behörigheter** -menyn där du kan välja ”programbehörigheter” och ”delegerade behörigheter” för programmet.
   
6. När du är klar klickar du på den **klar** knappen.

> [!NOTE]
> Klicka på den **klar** knapp anger också automatiskt behörigheter för ditt program i katalogen baserat på behörigheterna till andra program som du har konfigurerat.  Du kan visa dessa behörigheter för program genom att titta på programmet **inställningar** bladet.
> 
> 

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Konfigurera en resursprogram exponera web API: er
Du kan utveckla ett webb-API och göra den tillgänglig för program genom att exponera åtkomst [scope](active-directory-dev-glossary.md#scopes) och [roller](active-directory-dev-glossary.md#roles). En korrekt konfigurerad web API görs tillgänglig precis som i andra Microsoft webb-API: er, inklusive Graph-API och Office 365-API: er. Åtkomstscope och roller är tillgängliga via din [programmets manifest](active-directory-dev-glossary.md#application-manifest), vilket är en JSON-fil som representerar identitet programkonfigurationen.  

I följande avsnitt visas hur du exponera åtkomstscope, genom att ändra resursen programmanifestet.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Lägga till scope till resursprogram
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i det övre högra hörnet på sidan.
3. Välj på den översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på det program du vill konfigurera. Detta kommer till programmets QuickStart sida, samt öppna bladet inställningar för programmet.
4. Klicka på **Manifest** från sidan programmet att öppna redigeraren infogade manifestet. 
5. Ersätt ”oauth2Permissions” nod med följande JSON-fragment. Den här fragment är ett exempel på hur du exponera en omfattning som kallas ”personifiering”, vilket gör att en resursägare får ett klientprogram en typ av delegerad åtkomst till en resurs. Kontrollera att du ändrar texten och värdena för ditt eget program:
   
        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow the application full access to the Todo List service on behalf of the signed-in     user",
            "adminConsentDisplayName": "Have full access to the Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow the application full access to the todo service on your behalf",
            "userConsentDisplayName": "Have full access to the todo service",
            "value": "user_impersonation"
            }
        ],
   
    Id-värdet måste vara en ny genererade GUID som du skapar med hjälp av en [GUID generation verktyget](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) eller programmässigt. Representerar en unik identifierare för den behörighet som exponeras av webb-API. När klienten är korrekt konfigurerad för att begära åtkomst till dina webb-API och webb-API-anrop, presenterar en OAuth 2.0 JWT-token som har omfång (scp) anspråket värde ovan, som i det här fallet är user_impersonation.
   
   > [!NOTE]
   > Du kan visa ytterligare scope senare, vid behov. Överväg att ditt webb-API kan det hända att flera scope som är associerade med en mängd olika funktioner. Nu kan du styra åtkomst till webb-API med hjälp av omfång (scp)-anspråk i mottaget OAuth 2.0 JWT-token.
   > 
   > 
6. Klicka på **spara** spara manifestet. Webb-API är nu konfigurerad för att användas av andra program i din katalog.

#### <a name="to-verify-the-web-api-is-exposed-to-other-applications-in-your-directory"></a>Om du vill verifiera webben exponeras API för andra program i din katalog
1. Klicka på den översta menyn **App registreringar**, Välj önskade klientprogrammet som du vill konfigurera åtkomst till webb-API och gå till inställningar-bladet.
2. Från den **nödvändiga behörigheter** väljer webb-API som du precis har behörigheter för. Välj den nya behörigheten i den nedrullningsbara menyn delegerade behörigheter.

![Göra-lista behörigheter visas](./media/active-directory-integrating-applications/todolistpermissions.png)

#### <a name="more-on-the-application-manifest"></a>Mer information om programmanifestet
Applikationsmanifestet faktiskt fungerar som en mekanism för att uppdatera entiteten program som definierar alla attribut för ett Azure AD-program identitet konfigurationen, inklusive API åtkomstscope som vi diskuterade. Mer information om entiteten program finns i [Graph API entitet dokumentationen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). I den hittar du utförlig referensinformation om programmet entitetsmedlemmar som används för att ange behörighet för din API:  

* appRoles medlemmen, som är en samling av [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) entiteter som kan användas för att definiera den **programbehörigheter** för ett webb-API  
* oauth2Permissions medlemmen, som är en samling av [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) entiteter som kan användas för att definiera den **delegerade behörigheter** för ett webb-API

Mer information om application manifest begrepp i allmänhet Se [förstå Azure Active Directory-programmanifestet](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Åtkomst till Azure AD-diagram och Office 365 via Microsoft Graph API: er  
Du kan också uppdatera ditt klientprogram att komma åt API: er som exponeras av resurser i Microsofts som nämnts tidigare, förutom exponera/åtkomst till API: er på din egen resursprogram.  Microsoft Graph API, som kallas ”Microsoft Graph” i listan över behörigheter för andra program, är tillgänglig eller alla program som har registrerats med Azure AD. Om du registrerar ditt klientprogram i Azure AD-klient som har etablerats av Office 365 kan du också öppna alla behörigheter som exponeras av Microsoft Graph API till olika resurser för Office 365.

En fullständig beskrivning på åtkomstscope som exponeras av Microsoft Graph API finns i [behörighetsomfattningen | Microsoft Graph API begrepp](https://graph.microsoft.io/docs/authorization/permission_scopes) artikel.

> [!NOTE]
> På grund av en aktuell begränsning native client program kan endast anropa i Azure AD Graph API om de använder behörigheten ”åtkomst till din organisations katalog”.  Den här begränsningen gäller inte för webbprogram.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Konfigurera program för flera innehavare
När du lägger till ett program till Azure AD kan du vill att programmet ska endast användas av användare i din organisation. Alternativt kan du vill att programmet ska kunna nås av användare i externa organisationer. Dessa två programtyper kallas en organisation och program med flera klienter. Du kan ändra konfigurationen av en enskild klient program så att det blir ett program för flera innehavare, vilket det här avsnittet beskrivs nedan.

Det är viktigt att Observera skillnaderna mellan en enda klient och flera innehavare program:  

* En enskild klient program är avsedd att användas i en organisation. De är vanligtvis ett line-of-business (LoB)-program som skrivits av en enterprise-utvecklare. En enskild klient behöver bara användas av användare i en katalog och därför bara behöver etableras i en katalog.
* Ett program i flera innehavare som är avsedda att användas i många organisationer. De är en programvara som en tjänst (SaaS)-webbprogram som vanligtvis skrivs av en oberoende programvaruleverantör (ISV). Program med flera klienter måste tillhandahållas i varje katalog där de kommer att användas, vilket kräver att användaren eller administratören tillstånd att registrera dem via Azure AD medgivande framework som stöds. Observera att alla native client-program med flera innehavare som standard när de installeras på enheten för resursägare. Se en översikt över medgivande Framework ovan för mer information om medgivande framework.

#### <a name="enabling-external-users-to-grant-your-application-access-to-their-resources"></a>Att externa användare kan bevilja den applikationen åtkomsten till sina resurser
Om du skriver ett program som du vill göra tillgängliga för dina kunder eller partners utanför organisationen, behöver du uppdatera programdefinitionen av i Azure-portalen.

> [!NOTE]
> När du aktiverar flera innehavare, måste du kontrollera att ditt program App-ID URI tillhör i en verifierad domän. Dessutom måste Retur-URL börja med https://. Mer information finns i [program och tjänstens huvudnamn objekt](active-directory-application-objects.md).
> 
> 

Att aktivera åtkomst till din app för externa användare: 

1. Logga in på [Azure Portal](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i det övre högra hörnet på sidan.
3. Välj på den översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på det program du vill konfigurera. Detta kommer till programmets QuickStart sida, samt öppna bladet inställningar för programmet.
4. I bladet inställningar klickar du på **egenskaper** och Vänd den **flera innehavare** växla till **Ja**.

När du har gjort ändringen ovan, ska användare och administratörer i andra organisationer kunna bevilja den applikationen åtkomsten till sina kataloger och andra data.

#### <a name="triggering-the-azure-ad-consent-framework-at-runtime"></a>Utlöser Azure AD medgivande framework vid körning
Om du vill använda medgivande framework begäran klientprogram för flera innehavare auktorisering med OAuth 2.0. [Kodexempel](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) är tillgängliga för att visa hur ett webbprogram, programspecifika eller server-daemon programmet begär auktoriseringskoder och åtkomsttoken att anropa webb-API: er.

Webbprogrammet kan erbjuda en anmälan upplevelse för användare. Om du erbjuder en anmälan upplevelse förväntas att användaren kommer att klicka på en registrering knappen att omdirigera webbläsaren att Azure AD OAuth2.0 godkänner endpoint eller en OpenID Connect användarinformationen slutpunkt. Dessa slutpunkter gör det möjligt för programmet för att hämta information om den nya användaren genom att kontrollera id_token. Följande fasen anmälan kommer användaren att visas med en fråga om medgivande liknar det som visas ovan i översikten över avsnittet medgivande Framework.

Webbprogrammet kan du erbjuda en upplevelse som tillåter administratörer att ”logga in mitt företag”. Det här upplevelsen skulle också dirigera om användaren till Azure AD OAuth 2.0 auktorisera slutpunkt. I det här fallet kan du skickar en fråga = admin_consent parameter till slutpunkten för auktorisering för att tvinga administratör medgivande upplevelse, där administratören kommer att ge medgivande för organisationen. En användare som autentiseras med ett konto som tillhör rollen Global administratör kan ge samtycke. andra får ett felmeddelande. På lyckad medgivande svaret innehåller admin_consent = true. När du löser ett åtkomsttoken, får du även ett id_token som ger information om organisationen och den administratör som har registrerat dig för ditt program.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Aktivera OAuth bevilja 2.0 implicit för den enda sidan program
Enskild sida programmets (SPA) är vanligtvis strukturerade med JavaScript-frekventa klientdelen som körs i webbläsaren, som anropar programmets webb-API tillbaka slutet för att utföra sitt affärslogik. För SPA finns i Azure AD, använder du OAuth 2.0 Implicit bevilja för att autentisera användaren med Azure AD och få en token som du kan använda för att säkra anrop från programmets JavaScript-klient till dess backend-webb-API. När användaren har beviljat medgivande, användas detta samma autentiseringsprotokoll för att hämta token för att skydda anrop mellan klienten och andra webb-API-resurser som konfigurerats för programmet. Mer information om implicit authorization grant och hjälpa dig att avgöra om det är bäst för ditt program-scenario genom att se [förstå OAuth2 implicit bevilja flödet i Azure Active Directory ](active-directory-dev-understanding-oauth2-implicit-grant.md).

Som standard inaktiveras OAuth 2.0 implicit bevilja för program. Du kan aktivera OAuth 2.0 Implicit Grant för ditt program genom att ange den `oauth2AllowImplicitFlow` värde i dess [programmanifestet](active-directory-application-manifest.md), vilket är en JSON-fil som representerar identitet programkonfigurationen.

#### <a name="to-enable-oauth-20-implicit-grant"></a>Så här aktiverar du OAuth 2.0 implicit bevilja
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i det övre högra hörnet på sidan.
3. Välj på den översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på det program du vill konfigurera. Detta kommer till programmets QuickStart sida, samt öppna bladet inställningar för programmet.
4. Program-sidan klickar du på **Manifest** att öppna redigeraren infogade manifestet.
   Leta upp och ange värdet ”oauth2AllowImplicitFlow” till ”true”. Som standard är ”false”.
   
    `"oauth2AllowImplicitFlow": true,`
5. Spara det uppdaterade manifestet. När du sparat har ditt webb-API konfigurerats för att använda OAuth 2.0 Implicit bevilja för att autentisera användare.


## <a name="removing-an-application"></a>Tar bort ett program
Det här avsnittet beskrivs hur du tar bort ett program från Azure AD-klienten.

### <a name="removing-an-application-authored-by-your-organization"></a>Tar bort ett program som skapats av din organisation
Dessa är de program som visas under ”program mitt företag äger” filtret på huvudsidan ”program” för din Azure AD-klient. Tekniskt sett är dessa program som du har registrerat antingen manuellt via den klassiska Azure-portalen eller programmässigt via PowerShell eller Graph API. Mer specifikt representeras de av både ett program och tjänstens huvudnamn objekt i din klient. Se [program och tjänstens huvudnamn objekt](active-directory-application-objects.md) för mer information.

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Ta bort en enskild klient programmet från katalogen
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i det övre högra hörnet på sidan.
3. Välj på den översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på det program du vill konfigurera. Detta kommer till programmets QuickStart sida, samt öppna bladet inställningar för programmet.
4. Program-sidan klickar du på **ta bort**.
5. Klicka på **Ja** i bekräftelsemeddelandet.

#### <a name="to-remove-a-multi-tenant-application-from-your-directory"></a>Ta bort ett program med flera innehavare från katalogen
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i det övre högra hörnet på sidan.
3. Välj på den översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på det program du vill konfigurera. Detta kommer till programmets QuickStart sida, samt öppna bladet inställningar för programmet.
4. I bladet inställningar väljer **egenskaper** och Vänd den **flera innehavare** växla till **nr**. Detta omvandlar att programmet ska vara en organisation, men programmet finns kvar i en organisation som redan har samtyckt till den.
5. Klicka på den **ta bort** knappen från appen på sidan.
6. Klicka på **Ja** i bekräftelsemeddelandet.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Tar bort ett program för flera innehavare godkända av en annan organisation
Det här är en delmängd av de program som visas under ”program företaget använder” filtret på sidan huvudsakliga ”program” för din Azure AD-klient, särskilt de som inte är listade i listan ”program mitt företag äger”. Tekniskt sett är dessa program med flera klienter registrerad under medgivande. Mer specifikt representeras de av endast ett huvudnamn för tjänsten objekt i din klient. Se [program och tjänstens huvudnamn objekt](active-directory-application-objects.md) för mer information.

Företagets administratör måste ha en Azure-prenumeration ta bort åtkomst via Azure-portalen för att ta bort ett program med flera innehavare åtkomst till din katalog (när du har beviljat medgivande). Företagets administratör kan också använda den [Azure AD PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151) ta bort åtkomst.

## <a name="next-steps"></a>Nästa steg
* Finns det [företagsanpassning riktlinjer för integrerade appar](active-directory-branding-guidelines.md) tips om visual vägledning för din app.
* Mer information om förhållandet mellan program och tjänstens huvudnamn objekt i ett program finns [program och tjänstens huvudnamn objekt](active-directory-application-objects.md).
* Läs mer om rollen app manifestet spelar i [förstå Azure Active Directory-programmanifestet](active-directory-application-manifest.md)
* Finns det [Azure AD-utvecklare ordlista](active-directory-dev-glossary.md) definitioner av vissa grundbegrepp för utvecklare i Azure Active Directory (AD).
* Besök den [Active Directory-guide för utvecklare](active-directory-developers-guide.md) en översikt över alla utvecklare relaterat innehåll.

