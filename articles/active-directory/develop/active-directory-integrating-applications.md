---
title: aaaIntegrating program med Azure Active Directory | Microsoft Docs
description: Information om hur tooadd, uppdatera eller ta bort ett program i Azure Active Directory (AD Azure).
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
ms.openlocfilehash: c6bf1123bb3a4d78b55c1c55558e684fb844e687
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-applications-with-azure-active-directory"></a>Integrera program med Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Utvecklare av företagsprogram och programvara som en-tjänst (SaaS)-leverantörer kan utveckla kommersiella molntjänster eller affärsprogram som kan integreras med Azure Active Directory (AD Azure) tooprovide säker inloggning och auktorisering för sina tjänster. toointegrate ett program eller tjänst med Azure AD, en utvecklare måste först registrera hello information om sina program med Azure AD via hello klassiska Azure-portalen.

Den här artikeln visar hur tooadd, uppdatera eller ta bort ett program i Azure AD. Får du lära dig om hello olika typer av program som kan integreras med Azure AD, hur tooconfigure program-tooaccess andra resurser, till exempel web API: er och mycket mer.

toolearn mer om hello två Azure AD-objekt som representerar en registrerade programmet och hello förhållandet mellan dem, se [program och tjänstens huvudnamn objekt](active-directory-application-objects.md); toolearn mer om hello företagsanpassning riktlinjer du ska använda när du utvecklar program med Azure Active Directory, se [företagsanpassning riktlinjer för integrerade appar](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Lägga till ett program
Alla program som vill toouse hello funktionerna i Azure AD måste först registreras i Azure AD-klient. Registrering under processen ger Azure AD-information om ditt program, till exempel hello URL där det är placerad, Hej URL toosend svar när en användare autentiseras, hello URI som identifierar hello app och så vidare.

Du kan bara följa hello anvisningarna nedan om du utvecklar ett program som bara behöver toosupport-inloggning för användare i Azure AD. Om programmet behöver autentiseringsuppgifter eller behörigheter tooaccess tooa web API eller måste tooallow användare från andra Azure AD innehavare tooaccess, se [uppdatering av ett program](#updating-an-application) avsnittet toocontinue konfigurera ditt program.

### <a name="tooregister-a-new-application-in-hello-azure-portal"></a>tooregister ett nytt program i hello Azure-portalen
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Hello vänstra navigeringsfönstret och välj **fler tjänster**, klickar du på **App registreringar**, och klicka på **Lägg till**.
4. Följ anvisningarna för hello och skapa ett nytt program. Om du vill att specifika exempel för webbprogram och interna program, Kolla in våra [Snabbstart](active-directory-developers-guide.md).
  * Ange hello för webbprogram, **inloggnings-URL**, vilket är hello bas-URL för din app, där användarna kan logga in t.ex `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: hello **App ID URI** is a unique identifier for your application. hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Interna program, ange en **omdirigerings-URI**, vilka Azure AD använder tooreturn token svar. Ange ett värde specifika tooyour program. t.ex.`http://MyFirstAADApp`
5. När du har slutfört registreringen, tilldelar Azure AD ditt program ett unikt klient-ID, hello program-ID. Programmet har lagts till och du kommer att tas toohello snabbstartsidan för ditt program. Beroende på om ditt program är en webbplats eller det ursprungliga programmet, visas olika alternativ för hur tooadd ytterligare funktioner tooyour program. När programmet har lagts kan du börja uppdatera programmet tooenable användare toosign i åtkomst till webb-API: er i andra program eller konfigurera program för flera innehavare (som gör andra organisationer tooaccess tillämpningsprogrammet).

> [!NOTE]
> Som standard är hello nyskapad programregistrering konfigurerade tooallow användare från din katalog toosign i tooyour program.
> 
> 

## <a name="updating-an-application"></a>Uppdatering av ett program
När programmet har registrerats med Azure AD kan behöva uppdateras toobe tooprovide tooweb API: er kan göras tillgänglig i andra organisationer och mycket mer. Det här avsnittet beskrivs olika sätt som du kanske måste tooconfigure ytterligare ditt program. Först börjar vi med en översikt över hello medgivande Framework, som är viktiga toounderstand om du skapar resursen/API-program som används av klientprogram som skapats av utvecklarna i din organisation eller en annan organisation.

Mer information om hello sätt autentisering fungerar i Azure AD, se [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md).

### <a name="overview-of-hello-consent-framework"></a>Översikt över hello medgivande framework
Azure AD medgivande framework gör det enkelt toodevelop flera innehavare web och native client-program som behöver tooaccess webb-API: er som skyddas av en Azure AD-klient, som skiljer sig från hello en där hello klientprogrammet har registrerats. Dessa webb-API: er omfattar hello Microsoft Graph API (tooaccess Azure Active Directory, Intune och tjänster i Office 365) och andra Microsoft-tjänster API: er, utöver tooyour äger web API: er. hello framework baseras på en användare eller administratör ge medgivande tooan program som begär toobe registrerats i katalogen, vilket kan innebära att få åtkomst till katalogdata.

Om en klient webbprogram måste tooread kalenderinformationen om hello användaren från Office 365, kommer användaren att nödvändiga tooconsent toohello klientprogrammet. När tillstånd ges kommer hello klientprogrammet att kunna toocall hello Microsoft Graph API för hello användares räkning och använda hello kalenderinformationen efter behov. Hej [Microsoft Graph API](https://graph.microsoft.io) ger åtkomst toodata i Office 365 (till exempel kalendrar och meddelanden från Exchange, platser och listor från SharePoint, dokument från OneDrive, uppgifter från Planner arbetsböcker från OneNote-anteckningsböcker Excel, osv), samt användare och grupper från Azure AD och andra dataobjekt från flera Microsoft-molntjänster. 

hello medgivande framework bygger på OAuth 2.0 och dess olika flöden, till exempel auktoriseringskod bevilja och klienten autentiseringsuppgifter ger, med hjälp av offentliga eller konfidentiell klienter. Med hjälp av OAuth 2.0, Azure AD gör det möjligt toobuild många olika typer av program, så som på en telefon tablet, server eller ett webbprogram och komma åt resurser i toohello krävs.

Mer detaljerad information om hello medgivande framework finns [OAuth 2.0 i Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx), [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md), och för information om hur du får en auktoriserad åtkomst tooOffice 365 via Microsoft Graph finns [App autentisering med Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

#### <a name="example-of-hello-consent-experience"></a>Exempel på hello medgivande upplevelse
hello visar följande steg hur hello medgivande experience fungerar för både hello programutvecklaren och användare.

1. Ange hello behörigheter som krävs för ditt program med hjälp av hello menyer i hello krävs behörigheter på din klient webbprogrammets konfigurationssidan i hello Azure-portalen.
   
    ![Behörigheter tooother program](./media/active-directory-integrating-applications/requiredpermissions.png)
2. Överväg att ditt program behörighet har uppdaterats, hello programmet körs och en användare är om toouse det för hello första gången. Om programmet hello redan inte har ingått ett åtkomst eller uppdatera token, hello program måste toogo tooAzure Annonsens auktorisering endpoint tooobtain en Auktoriseringskoden som kan använda tooacquire en ny tillgång och uppdateringstoken.
3. Om hello användaren inte redan har autentiserats, ombeds de toosign i tooAzure AD.
   
    ![Användaren eller administratören logga in tooAzure AD](./media/active-directory-integrating-applications/usersignin.png)
4. När hello användaren har loggat in, avgör Azure AD om hello användare behöver toobe visas en sida medgivande. Detta baseras på om hello användare (eller organisationens administratör) har redan beviljats hello programmet medgivande. Om medgivande inte redan har fått, Azure AD uppmanas hello användaren efter medgivande och visas hello krävs behörigheter som behövs för toofunction. hello uppsättning behörigheter som visas i dialogrutan för hello medgivande är hello samma som vad du har valt i hello delegerade behörigheter i hello Azure-portalen.
   
    ![Medgivande användarupplevelse](./media/active-directory-integrating-applications/consent.png)
5. När hello användare ger ditt medgivande, returneras ett auktoriseringskod tooyour program som kan återköpta tooacquire en åtkomst-token och uppdatera token. Mer information om det här flödet finns hello [web tooweb API programavsnittet](active-directory-authentication-scenarios.md#web-application-to-web-api) i avsnittet [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md).

6. Som administratör kan medgivande du också tooan programmet delegerade behörigheter för alla hello de användare i din klient. Detta förhindrar att hello medgivande dialogrutan som visas för varje användare i hello-klient. Du kan göra detta från hello [Azure-portalen](https://portal.azure.com) från sidan program. Från hello **inställningar** bladet för programmet, klickar du på **nödvändiga behörigheter** och klicka på hello **bevilja med** knappen. 

    ![Bevilja behörigheter för explicit admin medgivande](./media/active-directory-integrating-applications/grantpermissions.png)
    
> [!NOTE]
> Ge medgivande med hello **bevilja med** knappen krävs för närvarande för enstaka sida program (SPA) som använder ADAL.js, eftersom hello åtkomsttoken har begärts utan en medgivandetext misslyckas om medgivande inte är redan beviljats.   

### <a name="configuring-a-client-application-tooaccess-web-apis"></a>Konfigurera en klient programmet tooaccess-webb-API: er
För en webbserver konfidentiell klienten programmet toobe kan tooparticipate i ett tillstånd bevilja flöde som kräver autentisering (och få en åtkomsttoken), måste den upprätta säkra referenser. hello standardmetoden för autentisering stöds av hello Azure-portalen är klient-ID + symmetrisk nyckel. Det här avsnittet beskriver hello configuration steg krävs tooprovide hello hemlig nyckel din klientens autentiseringsuppgifter.

Dessutom innan en klient kan åtkomst till webb-API exponeras av en resursprogram (ie: Microsoft Graph API), hello medgivande framework säkerställer hello-klienten erhåller hello behörighet bevilja nödvändiga, baserat på hello behörigheter som begärdes. Som standard kan alla program välja behörigheter från Azure Active Directory (Graph API) och Azure Service Management API med hello Azure AD ”aktivera inloggning och Läs användarprofil” behörighet som redan är markerad som standard. Om klientprogrammet registreras i en Azure AD-klient för Office 365, vara web API: er och behörigheter för SharePoint och Exchange Online också väljas. Du kan välja mellan [två typer av behörigheter](active-directory-dev-glossary.md#permissions) i hello nedrullningsbara menyerna nästa toohello önskad webb-API:

* Behörigheter för program: Klientprogrammet måste tooaccess hello webb-API som själva (ingen användarkontext). Den här typen av behörighet kräver godkännande av administratören och är inte heller tillgänglig för interna program.
* Delegerade behörigheter: Klientprogrammet måste tooaccess hello webb-API som hello inloggade användaren, men med åtkomst begränsas av hello valt behörighet. Den här typen av behörighet kan beviljas av en användare om hello behörighet har konfigurerats som kräver godkännande av administratören. 

> [!NOTE]
> Lägga till en delegerad behörighet tooan program ger automatiskt inte medgivande toohello användare inom hello-klient som i hello klassiska Azure-portalen. hello användare måste manuellt godkänna för hello läggs delegerade behörigheter vid körning, såvida inte Hej administratör klickar hello **bevilja med** knappen från hello **nödvändiga behörigheter** avsnitt på sidan med hello program i hello Azure-portalen. 

#### <a name="tooadd-credentials-or-permissions-tooaccess-web-apis"></a>tooadd autentiseringsuppgifter eller behörigheter tooaccess webb-API: er
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Välj hello översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på hello-program som du vill tooconfigure. Detta kommer ta toohello programmet QuickStart sida, samt öppna hello inställningsbladet för hello program.
4. tooadd en hemlig nyckel för autentiseringsuppgifterna för ditt webbprogram, klicka på hello ”nycklar” avsnitt från hello inställningsbladet.  
   
   * Lägg till en beskrivning för din nyckel och välj antingen 1 eller 2 år varaktighet. 
   * hello längst till höger kolumnen att innehålla hello nyckelvärde, när du har sparat hello konfigurationsändringar. Säker toocome användas tillbaka toothis avsnitt och kopiera den när du klickar på Spara, så att du har för i ditt klientprogram under autentiseringen vid körning.
5. tooadd behörigheterna tooaccess resurs API: er från din klient klickar du på hello ”krävs” Säkerhetsavsnittet från hello inställningsbladet. 
   
   * Först klicka hello ”Lägg till”.
   * Klicka på ”Välj ett API” tooselect hello resurser du vill toopick från.
   * Bläddra igenom hello lista över tillgängliga API: er eller Använd hello Sök rutan tooselect från hello tillgänglig resurs för program i din katalog som exponerar ett webb-API. Klicka på hello resurs du är intresserad av och klickar sedan **Välj**.
   * När du valt kan du flytta toohello **Välj behörigheter** -menyn där du kan välja hello ”programbehörigheter” och ”delegerade behörigheter” för programmet.
   
6. När du är klar klickar du på hello **klar** knappen.

> [!NOTE]
> Klicka på hello **klar** knapp anger också automatiskt hello behörigheter för ditt program i din katalog baserat på hello behörigheter tooother program som du konfigurerade.  Du kan visa dessa behörigheter för program genom att titta på programmet hello **inställningar** bladet.
> 
> 

### <a name="configuring-a-resource-application-tooexpose-web-apis"></a>Konfigurera en resurs programmet tooexpose-webb-API: er
Du kan utveckla ett webb-API och göra den tillgänglig tooclient program genom att exponera åtkomst [scope](active-directory-dev-glossary.md#scopes) och [roller](active-directory-dev-glossary.md#roles). En korrekt konfigurerad web API görs tillgänglig precis som andra Microsoft webb-API: er, inklusive hello Graph API hello och hello Office 365-API: er. Åtkomstscope och roller är tillgängliga via din [programmets manifest](active-directory-dev-glossary.md#application-manifest), vilket är en JSON-fil som representerar identitet programkonfigurationen.  

hello efter avsnittet visar hur tooexpose access scope genom att ändra hello resurs programmets manifest.

#### <a name="adding-access-scopes-tooyour-resource-application"></a>Lägga till access scope tooyour resursprogram
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Välj hello översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på hello-program som du vill tooconfigure. Detta kommer ta toohello programmet QuickStart sida, samt öppna hello inställningsbladet för hello program.
4. Klicka på **Manifest** från hello programmet tooopen hello infogade manifestet sidredigeraren. 
5. Ersätt ”oauth2Permissions” nod med hello följande JSON-fragment. Den här fragment är ett exempel på hur tooexpose ett omfång kallas ”personifiering”, vilket gör att en resurs ägare toogive ett klientprogram en typ av delegerad åtkomst tooa resurs. Kontrollera att du ändrar hello text och värden för ditt eget program:
   
        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow hello application full access toohello Todo List service on behalf of hello signed-in     user",
            "adminConsentDisplayName": "Have full access toohello Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow hello application full access toohello todo service on your behalf",
            "userConsentDisplayName": "Have full access toohello todo service",
            "value": "user_impersonation"
            }
        ],
   
    hello-id-värdet måste vara en ny genererade GUID som du skapar med hjälp av en [GUID generation verktyget](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) eller programmässigt. Representerar en unik identifierare för hello behörighet som exponeras av hello webb-API. När klienten är korrekt konfigurerad toorequest åtkomst tooyour webb-API och anrop Hej webb-API, den visar en OAuth 2.0 JWT-token som har hello omfång (scp) Ange toohello anspråksvärdet ovan, som i det här fallet är user_impersonation.
   
   > [!NOTE]
   > Du kan visa ytterligare scope senare, vid behov. Överväg att ditt webb-API kan det hända att flera scope som är associerade med en mängd olika funktioner. Nu kan du styra åtkomst toohello webb-API med hjälp av hello omfång (scp)-anspråk i hello emot OAuth 2.0 JWT-token.
   > 
   > 
6. Klicka på **spara** toosave hello manifest. Webb-API: t har nu konfigurerats för toobe som används av andra program i din katalog.

#### <a name="tooverify-hello-web-api-is-exposed-tooother-applications-in-your-directory"></a>tooverify hello webb-API är utsatta tooother program i din katalog
1. På hello översta menyn **App registreringar**väljer hello önskad klientprogrammet tooconfigure åtkomst toohello webb-API och om du vill navigera toohello inställningsbladet.
2. Från hello **nödvändiga behörigheter** väljer hello webb-API som du precis har behörigheter för. Välj hello ny behörighet hello delegerade behörigheter nedrullningsbara menyn.

![Göra-lista behörigheter visas](./media/active-directory-integrating-applications/todolistpermissions.png)

#### <a name="more-on-hello-application-manifest"></a>Mer information om hello programmanifestet
hello programmanifestet faktiskt fungerar som en mekanism för att uppdatera hello programmet entitet som definierar alla attribut för ett Azure AD-program identitet konfigurationen, inklusive hello API åtkomstscope som vi diskuterade. Mer information om hello programmet entiteten finns hello [Graph API entitet dokumentationen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). I den hittar du utförlig referensinformation om hello entitet medlemmar används toospecify programbehörigheter för din API:  

* Hej appRoles medlem, som är en samling av [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) entiteter som kan använda toodefine hello **programbehörigheter** för ett webb-API  
* Hej oauth2Permissions medlem, som är en samling av [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) entiteter som kan använda toodefine hello **delegerade behörigheter** för ett webb-API

Mer information om application manifest begrepp i allmänhet finns för[förstå hello Azure Active Directory-programmanifestet](active-directory-application-manifest.md).

### <a name="accessing-hello-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Åtkomst till hello Azure AD-diagram och Office 365 via Microsoft Graph API: er  
Du kan uppdatera din klient programmet tooaccess API: er som exponeras av resurser i Microsofts som nämndes tidigare, dessutom tooexposing/åtkomst till API: er på din egen resursprogram.  Hej Microsoft Graph API, som kallas ”Microsoft Graph” hello-listan om du av behörigheter tooother program, är tillgänglig eller alla program som har registrerats med Azure AD. Om du registrerar ditt klientprogram i Azure AD-klient som har etablerats av Office 365 kan du också öppna alla hello behörigheter som exponeras av hello Microsoft Graph API toovarious Office 365-resurser.

En fullständig beskrivning på åtkomstscope som exponeras av Microsoft Graph API finns hello [behörighetsomfattningen | Microsoft Graph API begrepp](https://graph.microsoft.io/docs/authorization/permission_scopes) artikel.

> [!NOTE]
> På grund av tooa aktuell begränsning, kan interna klientprogram endast anropar hello Azure AD Graph API om de använder hello ”åtkomst till din organisations katalog” behörighet.  Den här begränsningen gäller inte för webbprogram.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Konfigurera program för flera innehavare
När du lägger till ett program tooAzure AD du ditt program toobe endast nås av användare i din organisation. Alternativt kan du ditt program toobe nås av användare i externa organisationer. Dessa två programtyper kallas en organisation och program med flera klienter. Du kan ändra hello konfigurationen av en enskild klient programmet toomake den ett program för flera innehavare, vilket det här avsnittet beskrivs nedan.

Det är viktigt toonote hello skillnader mellan en enda klient och flera innehavare program:  

* En enskild klient program är avsedd att användas i en organisation. De är vanligtvis ett line-of-business (LoB)-program som skrivits av en enterprise-utvecklare. En enskild klient räcker toobe nås av användare i en katalog och därför kan det bara behöver toobe etableras i en katalog.
* Ett program i flera innehavare som är avsedda att användas i många organisationer. De är en programvara som en tjänst (SaaS)-webbprogram som vanligtvis skrivs av en oberoende programvaruleverantör (ISV). Program med flera klienter behöver toobe etablerad på varje katalog där de kommer att användas, vilket kräver att användaren eller administratören medgivande tooregister dem via hello Azure AD medgivande framework som stöds. Observera att alla native client-program med flera innehavare som standard som de är installerade på hello resursägare enheten. Hello översikt över hello medgivande Framework ovan för mer information om hello medgivande framework finns.

#### <a name="enabling-external-users-toogrant-your-application-access-tootheir-resources"></a>Att aktivera externa användare toogrant dina program åtkomst tootheir resurser
Om du skriver ett program som du vill toomake tillgängliga tooyour kunder eller partners utanför din organisation behöver tooupdate hello programmets definition i hello Azure-portalen.

> [!NOTE]
> När du aktiverar flera innehavare, måste du kontrollera att ditt program App-ID URI tillhör i en verifierad domän. Dessutom hello Retur-URL måste börja med https://. Mer information finns i [program och tjänstens huvudnamn objekt](active-directory-application-objects.md).
> 
> 

tooenable access tooyour-appen för externa användare: 

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Välj hello översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på hello-program som du vill tooconfigure. Detta kommer ta toohello programmet QuickStart sida, samt öppna hello inställningsbladet för hello program.
4. Hello inställningar-bladet klickar du på **egenskaper** och vänd hello **flera innehavare** växla för**Ja**.

När du har gjort hello ändra ovan, användare och administratörer i andra organisationer kommer att kunna toogrant tootheir programkatalogen åtkomst och andra data.

#### <a name="triggering-hello-azure-ad-consent-framework-at-runtime"></a>Utlöser hello Azure AD medgivande framework vid körning
toouse hello medgivande framework klientprogram för flera innehavare måste begära auktorisering med OAuth 2.0. [Kodexempel](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) är tillgängliga tooshow du hur ett webbprogram, programspecifika eller server-daemon programmet begäranden auktorisering koder och komma åt tokens toocall webb-API: er.

Webbprogrammet kan erbjuda en anmälan upplevelse för användare. Om du erbjuder en anmälan upplevelse förväntas hello användaren klicka sedan på en registrering för att omdirigera hello webbläsare toohello Azure AD OAuth2.0 godkänner endpoint eller en OpenID Connect användarinformationen slutpunkt. Dessa slutpunkter kan hello programmet tooget information om hello nya användare genom att inspektera hello id_token. Följande hello anmälan fasen hello användare visas med en medgivande fråga liknande toohello visas ovan i hello översikt över hello medgivande Framework-avsnittet.

Webbprogrammet kan du erbjuda en upplevelse som gör att administratörer för ”logga in mitt företag”. Det här upplevelsen skulle också omdirigera hello användaren toohello Azure AD OAuth 2.0 auktorisera slutpunkt. I det här fallet kan du skickar en fråga = admin_consent parametern toohello godkänna endpoint tooforce Hej administratör medgivande upplevelse, där Hej administratör kommer att ge medgivande för organisationen. En användare som autentiseras med ett konto som tillhör toohello rollen som Global administratör kan ge samtycke. andra får ett felmeddelande. På lyckad medgivande hello svaret innehåller admin_consent = true. När du löser ett åtkomsttoken, får du även ett id_token som ger information om hello organisationen och hello-administratör som har registrerat dig för ditt program.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Aktivera OAuth bevilja 2.0 implicit för den enda sidan program
Enskild sida programmets (SPA) är vanligtvis strukturerade med JavaScript-frekventa klientdelen som körs i hello webbläsare, som anropar hello programmet webb-API serverdel tooperform dess affärslogik. SPA finns i Azure AD, du använder OAuth 2.0 Implicit Grant tooauthenticate hello användare med Azure AD och få en token som du kan använda toosecure-anrop från hello programmet JavaScript klienten tooits tillbaka slutet webb-API. När hello användare har beviljats medgivande, kan detta samma autentiseringsprotokoll att använda tooobtain token toosecure anrop mellan hello-klienten och andra webb-API-resurser som har konfigurerats för programmet hello. toolearn mer om hello implicit authorization grant och hjälper dig att du bestämmer dig för om det är bäst för ditt program scenario finns [förstå hello OAuth2 implicit bevilja flödet i Azure Active Directory ](active-directory-dev-understanding-oauth2-implicit-grant.md).

Som standard inaktiveras OAuth 2.0 implicit bevilja för program. Du kan aktivera OAuth 2.0 Implicit Grant för ditt program genom att ange hello `oauth2AllowImplicitFlow` värde i dess [programmanifestet](active-directory-application-manifest.md), vilket är en JSON-fil som representerar identitet programkonfigurationen.

#### <a name="tooenable-oauth-20-implicit-grant"></a>tooenable OAuth 2.0 implicit bevilja
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Välj hello översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på hello-program som du vill tooconfigure. Detta kommer ta toohello programmet QuickStart sida, samt öppna hello inställningsbladet för hello program.
4. Hello programmet sidan klickar du på **Manifest** tooopen hello infogade manifestet editor.
   Leta upp och ange hello ”oauth2AllowImplicitFlow”-värdet för ”true”. Som standard är ”false”.
   
    `"oauth2AllowImplicitFlow": true,`
5. Spara hello uppdateras manifest. När du sparat konfigurerad webbplatsen API är nu toouse OAuth 2.0 Implicit Grant tooauthenticate användare.


## <a name="removing-an-application"></a>Tar bort ett program
Det här avsnittet beskrivs hur tooremove ett program från din Azure AD-klient.

### <a name="removing-an-application-authored-by-your-organization"></a>Tar bort ett program som skapats av din organisation
Dessa är hello-program som visas under hello ”program mitt företag äger” filter ”program” hello huvudsidan för din Azure AD-klient. Tekniskt sett dessa program som du har registrerat antingen manuellt via hello klassiska Azure-portalen eller programmässigt via PowerShell eller hello Graph API. Mer specifikt representeras de av både ett program och tjänstens huvudnamn objekt i din klient. Se [program och tjänstens huvudnamn objekt](active-directory-application-objects.md) för mer information.

#### <a name="tooremove-a-single-tenant-application-from-your-directory"></a>tooremove en enskild klient programmet från katalogen
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Välj hello översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på hello-program som du vill tooconfigure. Detta kommer ta toohello programmet QuickStart sida, samt öppna hello inställningsbladet för hello program.
4. Hello programmet sidan klickar du på **ta bort**.
5. Klicka på **Ja** i hello bekräftelsemeddelande.

#### <a name="tooremove-a-multi-tenant-application-from-your-directory"></a>tooremove ett program för flera innehavare från katalogen
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Välj hello översta menyn **Azure Active Directory**, klickar du på **App registreringar**, och klicka sedan på hello-program som du vill tooconfigure. Detta kommer ta toohello programmet QuickStart sida, samt öppna hello inställningsbladet för hello program.
4. Hello inställningar-bladet välj **egenskaper** och vänd hello **flera innehavare** växla för**nr**. Detta konverterar din programmet toobe enstaka klient, men programmet hello finns kvar i en organisation som redan har godkänt tooit.
5. Klicka på hello **ta bort** knappen från hello appen på sidan.
6. Klicka på **Ja** i hello bekräftelsemeddelande.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Tar bort ett program för flera innehavare godkända av en annan organisation
Det här är en delmängd av hello program visas under hello ”program företaget använder” filtret på sidan för hello huvudsakliga ”program” för din Azure AD-klient, särskilt de som inte är listade under hello ”program mitt företag äger” listan hello. Tekniskt sett är dessa program med flera klienter registrerad under hello medgivande. Mer specifikt representeras de av endast ett huvudnamn för tjänsten objekt i din klient. Se [program och tjänstens huvudnamn objekt](active-directory-application-objects.md) för mer information.

I order tooremove ett program med flera innehavare access tooyour-katalogen (när du har beviljat medgivande), hello företagsadministratör måste ha en Azure-prenumeration tooremove åtkomst via hello Azure-portalen. Hello företagets administratör kan också använda hello [Azure AD PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151) tooremove åtkomst.

## <a name="next-steps"></a>Nästa steg
* Se hello [företagsanpassning riktlinjer för integrerade appar](active-directory-branding-guidelines.md) tips om visual vägledning för din app.
* Mer information om hello förhållandet mellan program och tjänstens huvudnamn objekt i ett program, se [program och tjänstens huvudnamn objekt](active-directory-application-objects.md).
* toolearn mer om hello rollen hello app manifestet spelar, se [förstå hello Azure Active Directory-programmanifestet](active-directory-application-manifest.md)
* Se hello [Azure AD-utvecklare ordlista](active-directory-dev-glossary.md) definitioner av vissa begrepp för utvecklare hello core Azure Active Directory (AD).
* Besök hello [Active Directory-guide för utvecklare](active-directory-developers-guide.md) en översikt över alla utvecklare relaterat innehåll.

