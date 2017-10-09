---
title: aaaAzure Active Directory-autentisering och Resource Manager | Microsoft Docs
description: "En utvecklare guiden tooauthentication med hello Azure Resource Manager API och Azure Active Directory för att integrera en app med andra Azure-prenumerationer."
services: azure-resource-manager,active-directory
documentationcenter: na
author: dushyantgill
manager: timlt
editor: tysonn
ms.assetid: 17b2b40d-bf42-4c7d-9a88-9938409c5088
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/27/2016
ms.author: dugill;tomfitz
ms.openlocfilehash: 757e45fdb28488b45de70647746461888bf35a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-resource-manager-authentication-api-tooaccess-subscriptions"></a>Använd Resource Manager API-autentisering tooaccess prenumerationer
## <a name="introduction"></a>Introduktion
Om du är programutvecklare som behöver toocreate en app som hanterar kundens Azure-resurser kan visar det här avsnittet hur tooauthenticate med hello Azure Resource Manager API: er och få åtkomst till tooresources i andra prenumerationer.

Appen kan komma åt hello Resource Manager API: er i på två sätt:

1. **Användar- + appåtkomst**: för appar som har åtkomst till resurser för en inloggad användare. Den här metoden fungerar för appar, till exempel webbprogram och kommandoradsverktyg som handlar om endast ”interaktiva management” Azure-resurser.
2. **Endast App-åtkomst**: för appar som körs daemon tjänster och schemalagda jobb. hello appens identitet beviljas direktåtkomst toohello resurser. Den här metoden fungerar för appar som behöver den långsiktiga fjärradministrerade (obevakad installation) åtkomst tooAzure.

Det här avsnittet innehåller stegvisa instruktioner toocreate en app som använder båda metoderna auktorisering. Den visar hur tooperform varje steg med REST API- eller C#. hello fullständig ASP.NET MVC-program finns på [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

## <a name="what-hello-web-app-does"></a>Vilka hello webbprogrammet har
hello-webbprogram:

1. Loggar in en Azure-användare.
2. Fråga användaren toogrant hello web app åtkomst tooResource Manager.
3. Hämtar användar- + app åtkomst-token för åtkomst till Resource Manager.
4. Använder token (från steg 3) toocall Resource Manager och tilldela hello app service principal tooa roll i hello prenumeration, som ger hello app långsiktiga åtkomst toohello prenumeration.
5. Hämtar endast app-åtkomst-token.
6. Använder token (från steg 5) toomanage resurser i hello prenumerationen via Resource Manager.

Här är hello slutpunkt till slutpunkt flödet av hello webbprogram.

![Autentiseringsflödet för hanteraren för filserverresurser](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Som användare kan du ange hello prenumerations-id för hello prenumeration som du vill toouse:

![Ange prenumerations-id](./media/resource-manager-api-authentication/sample-ux-1.png)

Välj hello konto toouse för att logga in.

![Välj konto](./media/resource-manager-api-authentication/sample-ux-2.png)

Ange dina autentiseringsuppgifter.

![Ange autentiseringsuppgifter](./media/resource-manager-api-authentication/sample-ux-3.png)

Bevilja hello app åtkomst tooyour Azure-prenumerationer:

![Bevilja åtkomst](./media/resource-manager-api-authentication/sample-ux-4.png)

Hantera dina anslutna prenumerationer:

![Ansluta prenumeration](./media/resource-manager-api-authentication/sample-ux-7.png)

## <a name="register-application"></a>Registrera program
Innan du börjar kodning måste du registrera ditt webbprogram med Azure Active Directory (AD). Hej appregistrering skapar en central identitet för din app i Azure AD. Den innehåller grundläggande information om programmet som OAuth-klient-ID, svars-URL: er och autentiseringsuppgifter för att programmet använder tooauthenticate och åtkomst till Azure Resource Manager API: er. Hej appregistrering registrerar också hello olika delegerade behörigheter som programmet behöver få åtkomst till Microsoft APIs hello användarens räkning.

Eftersom din app har åtkomst till andra prenumerationer, måste du konfigurera den som ett program med flera innehavare. verifiering av toopass, ange en domän som är associerade med Azure Active Directory. toosee hello domäner som är associerade med Azure Active Directory, logga in toohello [klassiska portalen](https://manage.windowsazure.com). Välj Azure Active Directory och välj sedan **domäner**.

hello som följande exempel visar hur tooregister hello appen med hjälp av Azure PowerShell. Du måste ha hello senaste versionen (augusti 2016) av Azure PowerShell för det här kommandot toowork.

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true

toolog i som hello AD-program som du behöver hello program-id och lösenord. toosee hello program-id som returnerades från hello föregående kommando, Använd:

    $app.ApplicationId

hello som följande exempel visar hur tooregister hello appen med hjälp av Azure CLI.

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

hello resultat innehåller hello AppId som du behöver vid autentisering som hello program.

### <a name="optional-configuration---certificate-credential"></a>Valfri konfiguration - autentiseringsuppgifter för certifikat
Azure AD stöder också certifikat autentiseringsuppgifter för program: skapa ett självsignerat certifikat, hålla hello privat nyckel och Lägg till registrering av hello offentliga nyckel tooyour Azure AD-applikation. För autentisering skickar programmet en liten nyttolast tooAzure AD som signerats med din privata nyckel och AD Azure verifierar hello signatur med hjälp av hello offentlig nyckel som du har registrerat.

Information om hur du skapar en AD-app med ett certifikat finns i [Använd Azure PowerShell toocreate ett huvudnamn för tjänsten tooaccess resurser](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority) eller [Använd Azure CLI toocreate ett huvudnamn för tjänsten tooaccess resurser](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Hämta klient-id från prenumerations-id
toorequest en token som kan använda toocall Resource Manager, måste programmet tooknow hello klient-ID för hello Azure AD-klient som är värd för hello Azure-prenumeration. Troligen användarna känner sina prenumerations-ID: n, men vet de inte klienten ID: n för Azure Active Directory. tooget hello användarens klient-id, ber hello användaren för hello prenumerations-id. Ange prenumerations-id när du skickar en begäran om hello prenumerationen:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

hello begäran misslyckas eftersom hello användaren inte har loggat in ännu, men du kan hämta hello klient-id från hello svar. Hämta hello klient-id i det här undantaget från hello svar huvudvärde för **WWW-autentisera**. Du ser den här implementeringen i hello [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) metod.

## <a name="get-user--app-access-token"></a>Hämta användar- + app åtkomst-token
Programmet omdirigerar hello användaren tooAzure AD med en OAuth 2.0 godkänna begäran - tooauthenticate hello användarens autentiseringsuppgifter och få tillbaka en auktoriseringskod. Programmet använder hello auktorisering koden tooget en åtkomst-token för Resource Manager. Hej [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) metoden skapar hello auktoriseringsbegäran.

Det här avsnittet visar hello REST API-begäranden tooauthenticate hello användare. Du kan också använda helper bibliotek tooperform autentisering i koden. Mer information om dessa bibliotek finns [Azure Active Directory-Autentiseringsbibliotek](../active-directory/active-directory-authentication-libraries.md). Anvisningar om att integrera Identitetshantering i ett program finns [Utvecklarhandbok för Azure Active Directory](../active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Auth-begäranden (OAuth 2.0)
Utfärda en öppna ID Connect/OAuth2.0 godkänna begäran toohello Azure AD auktorisera slutpunkt:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

hello frågan string-parametrar som är tillgängliga för denna begäran beskrivs i hello [begära en auktoriseringskod](../active-directory/develop/active-directory-protocols-oauth-code.md#request-an-authorization-code) avsnittet.

följande exempel visar hur hello toorequest OAuth2.0 tillstånd:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD autentiserar användaren hello och, om det behövs, frågar hello användaren toogrant behörighet toohello app. Den returnerar hello auktorisering kod toohello Reply-URL för programmet. Beroende på hello begärt response_mode, Azure AD antingen skickar tillbaka hello data i frågesträngen eller som postdata.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Auth-begäranden (Öppna ID Connect)
Om du inte bara vill tooaccess Azure Resource Manager hello användarens räkning, men även tillåta hello användaren toosign i tooyour program med hjälp av sina Azure AD-kontot, göra en öppna ID Connect auktorisera begäran. Med öppna ID Connect får program också en id_token från Azure AD som din app kan använda toosign i hello användare.

hello frågan string-parametrar som är tillgängliga för denna begäran beskrivs i hello [hello inloggning begäran om att skicka](../active-directory/develop/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) avsnittet.

En exempel öppna ID Connect-begäran är:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD autentiserar användaren hello och, om det behövs, frågar hello användaren toogrant behörighet toohello app. Den returnerar hello auktorisering kod toohello Reply-URL för programmet. Beroende på hello begärt response_mode, Azure AD antingen skickar tillbaka hello data i frågesträngen eller som postdata.

Är ett exempel öppna ID Connect svar:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Tokenbegäran (OAuth2.0 kod bevilja flöda)
Nu när ditt program har fått hello Auktoriseringskoden från Azure AD, är den tid tooget hello åtkomst-token för Azure Resource Manager.  Efter en OAuth2.0 kod bevilja Token begära toohello Azure AD-Token för slutpunkt:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

hello frågan string-parametrar som är tillgängliga för denna begäran beskrivs i hello [använda hello auktoriseringskod](../active-directory/develop/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) avsnittet.

hello som följande exempel visar en begäran om koden bevilja token med lösenord autentiseringsuppgifter:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

När du arbetar med autentiseringsuppgifter för certifikat, skapa en JSON-Webbtoken (JWT) och logga (RSA SHA256) med hjälp av hello privata nyckeln för certifikatet autentiseringsuppgifterna för ditt program. hello anspråkstyper för hello token som visas i [JWT-token anspråk](../active-directory/develop/active-directory-protocols-oauth-code.md#jwt-token-claims). Referenser finns hello [Active Directory Authentication Library (.NET) kod](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) toosign klienten Assertion JWT-token.

Se hello [öppna ID Connect spec](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) information om klientautentisering.

hello som följande exempel visar en begäran om koden bevilja token med autentiseringsuppgifter för certifikat:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Ett exempelsvar för kod bevilja token:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Hantera koden bevilja token svar
Ett lyckat svar för token innehåller hello (användar- + app) åtkomst-token för Azure Resource Manager. Programmet använder den här åtkomst-token tooaccess Resource Manager för hello användares räkning. hello livslängden för åtkomst-token som utfärdas av Azure AD är en timme. Det är inte troligt att ditt webbprogram måste toorenew hello (användar- + app) åtkomst-token. Använda hello uppdateringstoken som programmet tar emot hello token svar om den behöver toorenew hello åtkomst-token. Efter en OAuth2.0 Token begära toohello Azure AD-Token för slutpunkt:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

hello parametrar toouse med hello uppdateringsbegäran beskrivs i [uppdaterar hello åtkomsttoken](../active-directory/develop/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

hello följande exempel visas hur toouse hello uppdatera token:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Även om uppdaterings-tokens kan använda tooget ny åtkomsttoken för Azure Resource Manager kan är de inte lämpliga för offline-åtkomst av ditt program. hello uppdatera token livstid är begränsat och uppdateringstoken är bundna toohello användare. Om hello användare lämnar organisationen hello, förlorat hello program med hjälp av hello uppdateringstoken åtkomst. Den här metoden är inte lämplig för program som används av team toomanage sina Azure-resurser.

## <a name="check-if-user-can-assign-access-toosubscription"></a>Kontrollera om användaren kan tilldela åtkomst toosubscription
Ditt program nu har en token tooaccess Azure Resource Manager för hello användares räkning. hello nästa steg är tooconnect din app toohello prenumeration. När du ansluter kan din app kan hantera dessa prenumerationer även när hello användaren inte finns (långsiktiga offlineåtkomst).

För varje prenumeration tooconnect anropa hello [Resource Manager listbehörigheter](https://docs.microsoft.com/rest/api/authorization/permissions) API toodetermine om hello användaren har behörighet för hantering för hello prenumeration.

Hej [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) metod för hello ASP.NET MVC-exempelapp implementerar det här anropet.

följande exempel visar hur hello toorequest en användares behörigheter för en prenumeration. 83cfe939-2402-4581-b761-4f59b0a041e4 är hello-id för hello prenumeration.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Ett exempel på hello svar tooget användares behörigheter för prenumerationen är:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

hello behörigheter API returnerar flera behörigheter. Varje behörighet består av tillåtna åtgärder (åtgärder) och otillåtna åtgärder (notactions). Om det finns en åtgärd i hello tillåts åtgärdslista över alla behörighet och inte finns i hello notactions lista med behörigheten hello användaren är tillåten tooperform åtgärden. **Microsoft.Authorization/RoleAssignments/Write** hello åtgärd som beviljar åtkomst rights management. Programmet måste parsa hello behörigheter resultatet toolook efter den här åtgärden strängen i hello åtgärder och notactions för varje behörighet regex.

## <a name="get-app-only-access-token"></a>Hämta endast app-åtkomst-token
Nu vet du om hello användare kan tilldela åtkomst toohello Azure-prenumeration. hello nästa steg är:

1. Tilldela hello lämpliga RBAC rollen tooyour programmets identitet på hello prenumeration.
2. Validera hello åtkomst tilldelningen genom att fråga om hello program behörighet på hello prenumeration eller genom att öppna Resource Manager med endast app-token.
3. Registrera hello anslutning i ditt program ”anslutna prenumerationer” datastruktur - beständighet hello-id för hello prenumeration.

Nu ska vi titta närmare på hello första steget. tooassign hello lämpliga RBAC rollen toohello programmets identitet, måste du bestämma:

* hello objekt-id för programmets identitet i hello användarens Azure Active Directory
* hello identifierare för hello RBAC roll som krävs för ditt program för hello prenumeration

När programmet autentiserar en användare från en Azure AD, skapas ett huvudnamn service-objekt för tillämpningsprogrammet i den Azure AD. Azure kan RBAC roller toobe tilldelade tooservice säkerhetsobjekt toogrant direktåtkomst toocorresponding program på Azure-resurser. Den här åtgärden är exakt vad vi vill toodo. Frågan hello Azure AD Graph API toodetermine hello identifierare för hello tjänstens huvudnamn för programmet i hello inloggade användaren har Azure AD.

Du bara ha en åtkomst-token för Azure Resource Manager - du behöver en ny åtkomst-token toocall hello Azure AD Graph API. Alla program i Azure AD har behörigheten tooquery eget huvudnamn serviceobjektet, så att endast app-åtkomst-token är tillräckligt.

<a id="app-azure-ad-graph" />

### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Hämta endast app-åtkomst-token för Azure AD Graph API
tooauthenticate din app och hämta en token tooAzure AD Graph API, utfärda en klient autentiseringsuppgifter bevilja OAuth2.0 flöde tokenbegäran tooAzure AD-token för slutpunkt (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/Token**).

Hej [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) metod för hello ASP.net MVC exempelprogrammet hämtar endast app-åtkomst-token för Graph API med hjälp av hello Active Directory Authentication Library för .NET.

hello frågan string-parametrar som är tillgängliga för denna begäran beskrivs i hello [begära en åtkomst-Token](../active-directory/develop/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) avsnittet.

En exempelbegäran för klientreferensen bevilja token:

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Ett exempelsvar för klientreferensen bevilja token:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Hämta ObjectId för programmet tjänstens huvudnamn i användaren Azure AD
Nu kan använda hello endast app-åtkomst-token tooquery hello [Azure AD Graph tjänstens huvudnamn](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API toodetermine hello objekt-Id för hello programmet tjänstens huvudnamn i hello directory.

Hej [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) metod för hello ASP.net MVC exempelprogrammet implementerar det här anropet.

följande exempel visar hur hello toorequest tjänstens huvudnamn för ett program. a0448380-c346-4f9f-b897-c18733de9394 är hello klient-id för programmet hello.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

hello följande exempel visas en svar toohello begäran om ett program service principal

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow hello application tooaccess CloudSense on behalf of hello signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow hello application tooaccess CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Hämta Azure RBAC roll-ID
tooassign hello lämpliga RBAC rollen tooyour tjänstens huvudnamn, måste du bestämma hello identifierare för hello Azure RBAC roll.

hello rätt RBAC roll för ditt program:

* Om programmet övervakar endast hello prenumeration, utan ändringar, kräver endast läsare behörigheter på hello prenumeration. Tilldela hello **Reader** roll.
* Om programmet hanterar hello Azure-prenumeration, skapa/Ändra/ta bort enheter kräver en hello deltagarbehörighet.
  * toomanage en viss typ av resurs, tilldela hello deltagare resursspecifika roller (Virtual Machine-deltagare, Virtual Network-deltagare, Storage-konto deltagare, etc.)
  * toomanage alla resurstyp, tilldela hello **deltagare** roll.

hello rolltilldelning för ditt program är synliga toousers, så Välj hello minst krävs behörighet.

Anropa hello [rolldefinitionen för Resource Manager API](https://docs.microsoft.com/rest/api/authorization/roledefinitions) toolist Azure RBAC-roller och sök sedan iterera över hello resultatet toofind hello önskad rolldefinitionen efter namn.

Hej [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) metod för hello ASP.net MVC-exempelapp implementerar det här anropet.

hello följande begäran exempel visar hur tooget Azure RBAC Rollidentifierare. 09cbd307-aa71-4aca-b346-5f253e6e3ebb är hello-id för hello prenumeration.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

hello svaret är i hello följande format:

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access toothem.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access toothem.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Du behöver inte toocall detta API kontinuerligt. När du har bestämt Hej välkända GUID för hello rolldefinitionen, du kan skapa hello rolldefinitions-ID: som:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Här följer hello välkända GUID används ofta inbyggda roller:

| Roll | GUID |
| --- | --- |
| Läsare |acdd72a7-3385-48EF-bd42-f606fba81ae7 |
| Deltagare |b24988ac-6180-42A0-ab88-20f7382dd24c |
| Virtual Machine-deltagare |d73bb868-a0df-4d4d-bd69-98a00b01fccb |
| Virtual Network-deltagare |b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
| Storage-konto deltagare |86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
| Webbplatsen deltagare |de139f84-1756-47ae-9be6-808fbbe84772 |
| Web Plan deltagare |2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
| SQL Server-deltagare |6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
| SQL DB-deltagare |9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |

### <a name="assign-rbac-role-tooapplication"></a>Tilldela RBAC roll tooapplication
Du har allt du behöver tooassign hello lämpliga RBAC rollen tooyour tjänstens huvudnamn med hjälp av hello [Resource Manager skapa rolltilldelning](https://docs.microsoft.com/rest/api/authorization/roleassignments) API.

Hej [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) metod för hello ASP.net MVC-exempelapp implementerar det här anropet.

Ett exempel begäran tooassign RBAC rollen tooapplication:

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

Hello-begäran används hello följande värden:

| GUID | Beskrivning |
| --- | --- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb |hello-id för hello prenumeration |
| c3097b31-7309-4c59-b4e3-770f8406bad2 |hello objekt-id för hello tjänstens huvudnamn för programmet hello |
| acdd72a7-3385-48EF-bd42-f606fba81ae7 |hello-id för rollen för hello läsare |
| 4f87261d-2816-465D-8311-70a27558df4c |ett nytt guid som skapats för hello ny rolltilldelning |

hello svaret är i hello följande format:

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Hämta endast app-åtkomsttoken för Azure Resource Manager
toovalidate appen har hello önskad åtkomst på hello prenumeration, utför en test-aktivitet på hello prenumeration med hjälp av en app-only-token.

tooget endast app-åtkomst-token Följ instruktionerna från avsnittet [Hämta endast app-åtkomst-token för Azure AD Graph API](#app-azure-ad-graph), med ett annat värde för parametern för hello-resurs:

    https://management.core.windows.net/

Hej [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) metod för hello ASP.NET MVC exempelprogrammet hämtar endast app-åtkomst-token för att använda Azure Resource Manager hello Active Directory Authentication Library för .net.

#### <a name="get-applications-permissions-on-subscription"></a>Hämta programmets behörigheter för prenumerationen
toocheck som programmet har hello önskad åtkomst på en Azure-prenumeration, du kan också kontakta hello [Resource Manager behörigheter](https://docs.microsoft.com/rest/api/authorization/permissions) API. Den här metoden är liknande toohow du bestämma om hello användaren har behörighet för åtkomsthantering för hello prenumeration. Den här tiden kan anropa dock hello behörigheter API med hello endast app-åtkomst-token som du fick i hello föregående steg.

Hej [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) metod för hello ASP.NET MVC-exempelapp implementerar det här anropet.

## <a name="manage-connected-subscriptions"></a>Hantera anslutna prenumerationer
När hello rätt RBAC roll tilldelas tooyour programmet tjänstens huvudnamn på hello prenumeration, kan ditt program hålla övervakning/hantera den med hjälp av endast app-åtkomst-token för Azure Resource Manager.

Om en prenumeration ägare tar bort rolltilldelning för ditt program med hjälp av hello klassiska portalen eller kommandoradsverktyg, ditt program är inte längre kan tooaccess den prenumerationen. I så fall bör du meddela hello användaren att hello anslutning med hello prenumeration fjärrdisken bröts från utanför hello-program och ger dem ett alternativ för ”reparera” hello anslutning. ”Reparera” skulle återskapa bara hello rolltilldelning som tagits offline.

Precis som du har aktiverat hello tooconnect prenumerationer tooyour användarprogram måste du tillåta hello användaren toodisconnect prenumerationer för. Koppla från innebär att ta bort hello rolltilldelning med hello programmet tjänstens huvudnamn på hello prenumeration från en access management-synvinkel. Du kan också kan några tillstånd i hello-programmet för hello prenumeration tas bort för.
Endast användare med behörighet för hantering på hello prenumeration är kan toodisconnect hello prenumeration.

Hej [RevokeRoleFromServicePrincipalOnSubscription metoden](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) av hello ASP.net MVC exempelapp implementerar det här anropet.

Det - användare kan nu enkelt ansluta och hantera sina Azure-prenumerationer med ditt program.
