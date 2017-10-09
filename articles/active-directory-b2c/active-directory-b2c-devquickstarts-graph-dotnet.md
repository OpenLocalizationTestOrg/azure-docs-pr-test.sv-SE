---
title: "Azure Active Directory B2C: Använd hello Graph API | Microsoft Docs"
description: "Hur hello toocall Graph API för en B2C-klient med hjälp av en identitet tooautomate hello programprocessen."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f9904516-d9f7-43b1-ae4f-e4d9eb1c67a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: parakhj
ms.openlocfilehash: a17cdc4adf57dbf22592d99ef8ecde41652763fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-use-hello-graph-api"></a>Azure AD B2C: Använda hello Graph API
Azure Active Directory (AD Azure) B2C hyresgäster tenderar toobe mycket stora. Detta innebär att många vanliga klient hanteringsuppgifter måste toobe utföras via programmering. En primär exempel är användarhantering. Du kan behöva toomigrate en befintlig användare store tooa B2C-klient. Du kanske vill toohost användarregistrering på sidan och skapa användarkonton i Azure AD B2C-katalogen hello bakgrunden. Dessa typer av uppgifter kräver hello möjlighet toocreate, läsa, uppdatera och ta bort användarkonton. Du kan utföra dessa uppgifter med hjälp av hello Azure AD Graph API.

För B2C-innehavare finns det två primära lägen kommunicera med hello Graph API.

* För interaktiv, körs en gång aktiviteter, bör du fungerar som ett administratörskontot i hello B2C-klient när du utför hello uppgifter. Det här läget kräver en administratör toosign in med autentiseringsuppgifter innan som administratören kan utföra alla anrop toohello Graph API.
* Du bör använda någon typ av konto som du anger för automatisk, kontinuerlig uppgifter med hello behörighet som krävs för tooperform hanteringsuppgifter. Du kan göra detta genom att registrera ett program och autentisera tooAzure AD i Azure AD. Detta görs med hjälp av en **program-ID** som använder hello [OAuth 2.0 klientens autentiseringsuppgifter bevilja](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). I det här fallet fungerar hello programmet som själva inte som en användare toocall hello Graph API.

I den här artikeln handlar om hur tooperform hello automated användningsfall. toodemonstrate, ska vi skapa en .NET 4.5 `B2CGraphClient` som utför användare skapa, läsa, uppdatera och ta bort CRUD-åtgärder. hello-klienten har en Windows-kommandoradsgränssnittet (CLI) som gör att du tooinvoke olika sätt. Dock skrivs hello kod toobehave i ett icke-interaktiv, automatiserat sätt.

## <a name="get-an-azure-ad-b2c-tenant"></a>Skaffa en Azure AD B2C-klient
Innan du kan skapa program eller användare eller interagera med Azure AD alls, behöver du en Azure AD B2C-klient och ett globalt administratörskonto i hello-klient. Om du inte redan har en klient [Kom igång med Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-your-application-in-your-tenant"></a>Registrera ditt program i din klientorganisation
När du har en B2C-klient måste tooregister ditt program via hello [Azure Portal](https://portal.azure.com).

> [!IMPORTANT]
> toouse hello Graph API med din B2C-klient, måste tooregister ett dedikerat program med hjälp av hello generisk *App registreringar* bladet i hello Azure-portalen **inte** Azure AD B2C  *Program* bladet. Du kan inte återanvända hello redan befintliga B2C program som du har registrerat i hello Azure AD B2C *program* bladet.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj din Azure AD B2C-klient genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Hello vänstra navigeringsfönstret och välj **fler tjänster**, klickar du på **App registreringar**, och klicka på **Lägg till**.
4. Följ anvisningarna för hello och skapa ett nytt program. 
    1. Välj **Webbapp / API** som hello programtyp.    
    2. Ange **någon omdirigerings-URI** (t.ex. https://B2CGraphAPI) eftersom den inte är relevanta för det här exemplet.  
5. hello programmet kommer nu visas i hello lista över program klickar du på den tooobtain hello **program-ID** (även kallat klient-ID). Kopiera den som du behöver i ett senare avsnitt.
6. I hello inställningar-bladet klickar du på **nycklar** och lägga till en ny nyckel (även kallat klienthemlighet). Också kopiera den för användning i ett senare avsnitt.

## <a name="configure-create-read-and-update-permissions-for-your-application"></a>Konfigurera skapa, läsa och uppdatera behörigheterna för ditt program
Nu måste tooconfigure ditt program tooget alla hello krävs behörigheter toocreate, läsa, uppdatera och ta bort användare.

1. Om du fortsätter i hello Azure portal App registreringar bladet, Välj ditt program.
2. I hello inställningar-bladet klickar du på **nödvändiga behörigheter**.
3. I hello behörighet bladet, klickar du på **Windows Azure Active Directory**.
4. Hello Aktivera åtkomst bladet välj hello **läsning och skrivning katalogdata** tillstånd från **programbehörigheter** och på **spara**.
5. Tillbaka i bladet för hello behörigheter som krävs, klicka på hello **bevilja med** knappen.

Nu har du ett program som har behörigheten toocreate, läsa och uppdatera användare från din B2C-klient.

## <a name="configure-delete-permissions-for-your-application"></a>Konfigurera behörighet att ta bort programmet
För närvarande hello *läsning och skrivning katalogdata* behörighet har **inte** inkluderar hello möjlighet toodo eventuella borttagningar, till exempel ta bort användare. Om du vill toogive programmet hello möjlighet toodelete användarna behöver du toodo dessa extra steg som rör PowerShell, annars kan du hoppa över toohello nästa avsnitt.

Först ladda ned och installera hello [Microsoft Online Services-inloggningsassistent](http://go.microsoft.com/fwlink/?LinkID=286152). Hämta och installera hello [64-bitars Azure Active Directory-modulen för Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

När du har installerat hello PowerShell-modulen öppna PowerShell och Anslut tooyour B2C-klient. När du har kört `Get-Credential`, du uppmanas att ett användarnamn och lösenord, ange hello användarnamnet och lösenordet för administratörskontot din B2C-klient.

> [!IMPORTANT]
> Du behöver ett administratörskontot för toouse en B2C-klient som är **lokala** toohello B2C-klient. Dessa konton se ut så här: myusername@myb2ctenant.onmicrosoft.com.

```powershell
Connect-MsolService
```

Nu ska vi använda hello **program-ID** i hello-skriptet nedan tooassign hello programmet hello användaren konto administratörsroll som gör att den toodelete användare. Dessa roller har välkända identifierare, så behöver du toodo matas in din **program-ID** i hello skriptet nedan.

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

Ditt program nu har även behörighet toodelete användare från din B2C-klient.

## <a name="download-configure-and-build-hello-sample-code"></a>Hämta, konfigurera och skapa hello exempelkod
Först hämta hello exempelkod och få det körs. Sedan tar vi en närmare titt på den.  Du kan [hämta hello exempelkod som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Du kan också klona den i en katalog som du väljer:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Öppna hello `B2CGraphClient\B2CGraphClient.sln` Visual Studio-lösning i Visual Studio. I hello `B2CGraphClient` projekt, öppna hello fil `App.config`. Ersätt hello tre appinställningar med egna värden:

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{hello ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{hello Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Högerklicka sedan på hello `B2CGraphClient` lösning och återskapa hello-exempel. Om du lyckas kan du bör nu ha en `B2C.exe` körbar fil som finns i `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-hello-graph-api"></a>Skapa användaren CRUD-åtgärder med hjälp av hello Graph API
toouse hello B2CGraphClient, öppna en `cmd` Windows kommandot fråga och ändra din katalog toohello `Debug` directory. Kör sedan hello `B2C Help` kommando.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Då visas en kort beskrivning av varje kommando. Varje gång du anropar någon av dessa kommandon `B2CGraphClient` gör en begäran toohello Azure AD Graph API.

### <a name="get-an-access-token"></a>Hämta en åtkomsttoken
Alla begäran toohello Graph API kräver en åtkomst-token för autentisering. `B2CGraphClient`använder hello öppen källkod Active Directory Authentication Library (ADAL) toohelp hämta åtkomsttoken. ADAL underlättar token förvärv genom att tillhandahålla ett enkelt API och tar hand om vissa viktig information, till exempel cachelagring åtkomsttoken. Du har inte toouse ADAL tooget token om. Du kan också hämta token genom att utforma HTTP-begäranden.

> [!NOTE]
> Det här kodexemplet använder ADAL v2 i ordning toocommunicate med hello Graph API.  Du måste använda ADAL v2 och v3 i ordning tooget åtkomst-token som kan användas med hello Azure AD Graph API.
> 
> 

När `B2CGraphClient` körs, skapas en instans av hello `B2CGraphClient` klass. hello konstruktören för den här klassen ställer in en scaffold-teknik för ADAL-autentisering:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // hello client_id, client_secret, and tenant are provided in Program.cs, which pulls hello values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // hello AuthenticationContext is ADAL's primary class, in which you indicate hello tenant toouse.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // hello ClientCredential is where you pass in your client_id and client_secret, which are
    // provided tooAzure AD in order tooreceive an access_token by using hello app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Vi använder hello `B2C Get-User` kommandot som exempel. När `B2C Get-User` anropas utan några ytterligare indata hello CLI anrop hello `B2CGraphClient.GetAllUsers(...)` metod. Den här metoden anropar `B2CGraphClient.SendGraphGetRequest(...)`, som skickar en HTTP GET-begäran toohello Graph API. Innan `B2CGraphClient.SendGraphGetRequest(...)` skickar Hej GET-begäran, först hämtar en åtkomst-token med hjälp av ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL tooacquire a token by using hello app's identity (hello credential)
    // hello first parameter is hello resource we want an access_token for; in this case, hello Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Du kan hämta ett åtkomsttoken för hello Graph API genom att anropa hello ADAL `AuthenticationContext.AcquireToken(...)` metod. ADAL returnerar en `access_token` som representerar hello Programidentitet.

### <a name="read-users"></a>Läs användare
När du vill tooget en lista med användare eller hämta en viss användare från hello Graph API, kan du skicka en HTTP `GET` begära toohello `/users` slutpunkt. En begäran om alla hello användare i en klient som ser ut så här:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

toosee denna begäran, kör:

 ```
 > B2C Get-User
 ```

Det finns två viktiga saker toonote:

* hello åtkomsttoken har köpt via ADAL läggs toohello `Authorization` sidhuvud med hjälp av hello `Bearer` schema.
* För B2C-innehavare, måste du använda hello frågeparameter `api-version=1.6`.

Båda dessa uppgifter hanteras i hello `B2CGraphClient.SendGraphGetRequest(...)` metoden:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure toouse hello 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append hello access token for hello Graph API toohello Authorization header of hello request by using hello Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Skapa användarkonton för konsumenten
När du skapar användarkonton i din B2C-klient kan du skicka en HTTP `POST` begära toohello `/users` slutpunkt:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required toocreate consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier hello user uses toosign in toohello account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set too'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying toohello end user
    "mailNickname": "joec",                        // an email alias for hello user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set toofalse
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

De flesta av de här egenskaperna i den här förfrågan är nödvändiga toocreate konsumentanvändare. toolearn mer, klickar du på [här](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Observera att hello `//` kommentarer har inkluderats för bilden. Ta inte med dem i en faktiska begäran.

toosee hello begäran att köra något av följande kommandon hello:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

Hej `Create-User` kommandot tar emot en JSON-fil som indataparameter. Innehåller en JSON-representation av ett användarobjekt. Det finns två exempel JSON-filer i hello exempelkod: `usertemplate-email.json` och `usertemplate-username.json`. Du kan ändra dessa filer toosuit dina behov. Dessutom toohello obligatoriska fält ovan, flera valfria fälten som du kan använda ingår i dessa filer. Information om hello valfria fält finns i hello [Azure AD Graph API Entitetsreferens](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Du kan se hur hello POST-begäran har skapats i `B2CGraphClient.SendGraphPostRequest(...)`.

* En åtkomst-token toohello bifogas `Authorization` huvudet i begäran hello.
* Anger `api-version=1.6`.
* Hello brödtext hello begäran innehåller hello JSON-användarobjektet.

> [!NOTE]
> Om hello konton som du vill toomigrate från en befintlig användararkivet har lägre lösenordssäkerhet än hello [starka lösenord tillämpas av Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), kan du inaktivera hello starkt lösenordskrav med hello `DisableStrongPassword`värde i hello `passwordPolicies` egenskapen. Du kan till exempel ändra hello skapa användarbegäran som anges ovan på följande sätt: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.
> 
> 

### <a name="update-consumer-user-accounts"></a>Uppdatera konsumenten användarkonton
När du uppdaterar användarobjekt är hello processen liknande toohello som du använder toocreate användarobjekt. Men den här processen använder hello HTTP `PATCH` metoden:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only hello user's displayName
}
```

Försök tooupdate en användare genom att uppdatera JSON-filer med nya data. Du kan sedan använda `B2CGraphClient` toorun något av följande kommandon:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Inspektera hello `B2CGraphClient.SendGraphPatchRequest(...)` metod för information om hur toosend denna begäran.

### <a name="search-users"></a>Söka efter användare
Du kan söka efter användare i din B2C-klient på ett par olika sätt. En med hjälp av hello användarens objekt-ID eller två, med hjälp av hello användarens inloggning Identiferare (d.v.s. hello `signInNames` egenskap).

Kör något av följande kommandon toosearch för en viss användare hello:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Här följer några exempel:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Ta bort användare
hello-processen för att ta bort en användare är enkelt. Använd hello HTTP `DELETE` metoden och konstruktion hello URL: en med hello korrigera objekt-ID:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

toosee exempelvis ange det här kommandot och visa hello delete begäran som är tryckt toohello konsolen:

```
> B2C Delete-User <object-id-of-user>
```

Inspektera hello `B2CGraphClient.SendGraphDeleteRequest(...)` metod för information om hur toosend denna begäran.

Du kan utföra många andra åtgärder med hello Azure AD Graph API i tillägg toouser management. Den [Azure AD Graph API-referens](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) innehåller information om varje åtgärd, tillsammans med exempel begäranden.

## <a name="use-custom-attributes"></a>Använd anpassade attribut
De flesta konsumenten program måste toostore någon typ av anpassad profil användarinformation. Ett sätt som du kan göra detta är toodefine ett anpassat attribut i din B2C-klient. Du kan sedan behandla den attributet hello samma sätt som du hanterar andra egenskapen på ett användarobjekt. Du kan uppdatera hello attribut, ta bort hello attribut, fråga efter hello attribut skickar hello attribut som anspråk i inloggning-token och mycket mer.

toodefine ett anpassat attribut i din B2C-klient finns hello [referens för B2C-attributet](active-directory-b2c-reference-custom-attr.md).

Du kan visa hello anpassade attribut som definierats i din B2C-klient med hjälp av `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

hello utdata av dessa funktioner visar hello information om varje anpassade attribut som:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Du kan använda hello fullständigt namn som `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, som en egenskap på din användarobjekt.  Uppdatera JSON-fil med nya hello-egenskapen och ett värde för egenskapen hello och kör sedan:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Med hjälp av `B2CGraphClient`, du har ett tjänstprogram som kan hantera B2C-klient användarna programmässigt. `B2CGraphClient`använder en egen programmet identitet tooauthenticate toohello Azure AD Graph API. Token får också med hjälp av en klienthemlighet. Kom ihåg några viktiga punkter för B2C-appar som du använda den här funktionaliteten i ditt program:

* Du behöver toogrant hello programmet hello rätt behörigheter i hello-klient.
* Du måste toouse ADAL (inte MSAL) tooget åtkomsttoken för tillfället. (Du kan också skicka protokollmeddelanden direkt, utan att använda ett bibliotek.)
* När du anropar hello Graph API, Använd `api-version=1.6`.
* När du skapar och uppdaterar konsumentanvändare krävs några egenskaper, enligt beskrivningen ovan.

Om du har några frågor eller begäran om åtgärder som du vill att tooperform med hello Graph API på din B2C-klient, lämna en kommentar på den här artikeln eller filen ett problem i hello GitHub exempel databasen.

