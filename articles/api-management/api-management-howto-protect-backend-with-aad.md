---
title: aaaProtect en webb-API-serverdel med Azure Active Directory och API-hantering | Microsoft Docs
description: "Lär dig hur tooprotect en webb-API-serverdel med Azure Active Directory och API-hantering."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: f856ff03-64a1-4548-9ec4-c0ec4cc1600f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: f4b323034354aa09579c643bade47257fbf1e5c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprotect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Hur tooprotect en webb-API-serverdel med Azure Active Directory och API-hantering
Hej följande videoklipp visar hur toobuild en webb-API-serverdel och skydda den med hjälp av OAuth 2.0-protokollet med Azure Active Directory och API-hantering.  Den här artikeln innehåller en översikt och ytterligare information för hello stegen i hello videon. Den här 24 minuter långa videon visar hur till:

* Skapa en webb-API-serverdel och skydda den med AAD - börjar vid 1:30
* Importera hello API i API Management - inleder 7:10
* Konfigurera hello Developer portal toocall hello API - början på 9:09
* Konfigurera ett skrivbordsprogram toocall hello API - börjar vid 18:08
* Konfigurera en JWT validering princip toopre-godkänna begäranden - börjar vid 20:47

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a>Skapa en Azure AD-katalog
toosecure Web API säkerhetskopieras med Azure Active Directory måste du först ha en AAD-klient. I det här videoklippet en klient med namnet **APIMDemo** används. toocreate AAD-klient loggar in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) och på **ny**->**Apptjänster**->**Active Directory**->**Directory**->**skapa anpassade**. 

![Azure Active Directory][api-management-create-aad-menu]

I det här exemplet en katalog med namnet **APIMDemo** skapas med en standarddomän med namnet **DemoAPIM.onmicrosoft.com**. Den här katalogen används i hela hello video.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Skapa en tjänst för webb-API som skyddas av Azure Active Directory
I det här steget skapas en webb-API-serverdel med hjälp av Visual Studio 2013. Det här steget i hello video börjar vid 1:30. toocreate Web API backend-projekt i Visual Studio klickar du på **filen**->**ny**->**projekt**, och välj **ASP.NET-webbprogram Programmet** från hello **Web** lista över mallar. I den här videon hello projektet med namnet **APIMAADDemo**. Klicka på **OK** toocreate hello projektet. 

![Visual Studio][api-management-new-web-app]

Klicka på **Web API** från hello **markerar du en lista över** toocreate Web API-projekt. Klicka på tooconfigure Azure Directory Authentication **ändra autentisering**.

![Nytt projekt][api-management-new-project]

Klicka på **Organisationskonton**, och ange hello **domän** för din AAD-klient. I det här exemplet hello domänen är **DemoAPIM.onmicrosoft.com**. hello domän i din katalog kan hämtas från hello **domäner** för din katalog.

![Domäner][api-management-aad-domains]

Konfigurera inställningar för hello önskad i hello **ändra autentisering** dialogrutan och klicka på **OK**.

![Ändra autentisering][api-management-change-authentication]

När du klickar på **OK** Visual Studio försöker tooregister ditt program med Azure AD-katalogen och du kan ange toosign i av Visual Studio. Logga in med ett administratörskonto för din katalog.

![Logga in tooVisual Studio][api-management-sign-in-vidual-studio]

tooconfigure projektet som en Azure Web API Kontrollera hello rutan för **värd i molnet hello** och klicka sedan på **OK**.

![Nytt projekt][api-management-new-project-cloud]

Du tillfrågas toosign i tooAzure och du kan sedan konfigurera hello Web App.

![Konfigurera][api-management-configure-web-app]

I det här exemplet en ny **programtjänstplanen** med namnet **APIMAADDemo** har angetts.

Klicka på **OK** tooconfigure hello Web App och skapa hello-projekt.

## <a name="add-hello-code-toohello-web-api-project"></a>Lägg till hello kod toohello Web API-projekt
hello nästa steg i hello video lägger till hello kod toohello Web API-projekt. Det här steget startar vid 4:35.

i det här exemplet Hello Web API implementerar en grundläggande Kalkylatorn tjänst med hjälp av en modell och en domänkontrollant. tooadd hello modellen för hello tjänst, högerklicka på **modeller** i **Solution Explorer** och välj **Lägg till**, **klassen**. Namnge hello klassen `CalcInput` och på **Lägg till**.

Lägg till följande hello `using` instruktionen toohello överkant hello `CalcInput.cs` fil.

```c#
using Newtonsoft.Json;
```

Ersätt hello genereras klassen med följande kod hello.

```c#
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

Högerklicka på **domänkontrollanter** i **Solution Explorer** och välj **Lägg till**->**domänkontrollant**. Välj **Web API 2 styrenhet – tom** och på **Lägg till**. Typen **CalcController** hello styrenhet namnet och klickar på **Lägg till**.

![Lägg till styrenhet][api-management-add-controller]

Lägg till följande hello `using` instruktionen toohello överkant hello `CalcController.cs` fil.

```c#
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

Ersätt hello genereras controller-klassen med följande kod hello. Den här koden implementerar hello `Add`, `Subtract`, `Multiply`, och `Divide` drift av hello grundläggande Kalkylatorn API.

```c#
[Authorize]
public class CalcController : ApiController
{
    [Route("api/add")]
    [HttpGet]
    public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/sub")]
    [HttpGet]
    public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/mul")]
    [HttpGet]
    public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/div")]
    [HttpGet]
    public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }
}
```

Tryck på **F6** toobuild och kontrollera hello lösning.

## <a name="publish-hello-project-tooazure"></a>Publicera hello projekt tooAzure
I det här steget hello Visual Studio är-projekt publicerade tooAzure. Det här steget i hello video startar vid 5:45.

toopublish Hej projektet tooAzure, högerklicka på hello **APIMAADDemo** projektet i Visual Studio och välj **publicera**. Tänk hello hello standardinställningarna **Publicera webbplats** dialogrutan och klicka på **publicera**.

![Webbpublicering][api-management-web-publish]

## <a name="grant-permissions-toohello-azure-ad-backend-service-application"></a>Bevilja behörighet toohello Azure AD backend-tjänstprogram
Ett nytt program för hello backend-tjänsten har skapats i Azure AD-katalogen som en del av hello konfigurationen och publiceringsprocessen i ditt webb-API-projekt. I det här steget av hello video behörigheter början på 6:13 toohello Web API-serverdel.

![Program][api-management-aad-backend-app]

Klicka på hello programbehörigheter tooconfigure hello krävs hello namn. Navigera toohello **konfigurera** och rulla ned toohello **behörigheter tooother program** avsnitt. Klicka på hello **programbehörigheter** listrutan bredvid **Windows** **Azure Active Directory**, hello kryssrutan för **läsa katalogdata**, och klicka på **spara**.

![Lägg till behörigheter][api-management-aad-add-permissions]

> [!NOTE]
> Om **Windows** **Azure Active Directory** är inte finns i listan under behörigheter tooother program klickar du på **lägga till program** och lägga till den hello-listan.
> 
> 

Anteckna hello **App-Id URI** för användning i ett senare skede när ett Azure AD-program är konfigurerat för hello API Management developer-portalen.

![URI för App-Id][api-management-aad-sso-uri]

## <a name="import-hello-web-api-into-api-management"></a>Importera hello Web API till API-hantering
API: er konfigureras från hello API publisher-portalen som öppnas via hello Azure-portalen. tooreach, klickar du på **Publisher portal** hello verktygsfältet för API Management-tjänsten. Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [hantera din första API] [ Manage your first API] kursen.

![Utgivarportalen][api-management-management-console]

Åtgärder kan vara [till tooAPIs manuellt](api-management-howto-add-operations.md), eller kan importeras. I det här videoklippet importeras åtgärder i Swagger-format som börjar på 6:40.

Skapa en fil med namnet `calcapi.json` med följande innehåll och spara den tooyour dator. Se till att hello `host` -attributet pekar tooyour Web API-serverdel. I det här exemplet `"host": "apimaaddemo.azurewebsites.net"` används.

```json
{
  "swagger": "2.0",
  "info": {
    "title": "Calculator",
    "description": "Arithmetics over HTTP!",
    "version": "1.0"
  },
  "host": "apimaaddemo.azurewebsites.net",
  "basePath": "/api",
  "schemes": [
    "http"
  ],
  "paths": {
    "/add?a={a}&b={b}": {
      "get": {
        "description": "Responds with a sum of two numbers.",
        "operationId": "Add two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>51</code>.",
            "required": true,
            "type": "string",
            "default": "51",
            "enum": [
              "51"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>49</code>.",
            "required": true,
            "type": "string",
            "default": "49",
            "enum": [
              "49"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/sub?a={a}&b={b}": {
      "get": {
        "description": "Responds with a difference between two numbers.",
        "operationId": "Subtract two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>50</code>.",
            "required": true,
            "type": "string",
            "default": "50",
            "enum": [
              "50"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/div?a={a}&b={b}": {
      "get": {
        "description": "Responds with a quotient of two numbers.",
        "operationId": "Divide two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/mul?a={a}&b={b}": {
      "get": {
        "description": "Responds with a product of two numbers.",
        "operationId": "Multiply two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>5</code>.",
            "required": true,
            "type": "string",
            "default": "5",
            "enum": [
              "5"
            ]
          }
        ],
        "responses": { }
      }
    }
  }
}
```

tooimport hello Kalkylatorn API, klickar du på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **Import API**.

![Knappen Importera API][api-management-import-api]

Utföra hello följande steg tooconfigure hello Kalkylatorn API.

1. Klicka på **från filen**, bläddra toohello `calculator.json` fil som du sparade och klicka på hello **Swagger** knappen.
2. Typen **beräkna** till hello **URL för Web API-suffix** textruta.
3. Klicka i hello **produkter (valfritt)** och väljer **Starter**.
4. Klicka på **spara** tooimport hello API.

![Lägga till ett nytt API][api-management-import-new-api]

När du har importerat hello API visas hello sammanfattningssidan hello-API: t i hello publisher-portalen.

## <a name="call-hello-api-unsuccessfully-from-hello-developer-portal"></a>Anropa API för hello fel från hello developer-portalen
Nu hello API har importerats till API-hantering, men kan ännu inte anropas har från hello developer-portalen eftersom hello serverdelstjänst skyddas med Azure AD-autentisering. Detta visas i hello videon inleder med hello följande steg 7:40.

Klicka på **utvecklarportalen** från hello längst upp till höger sida av hello publisher-portalen.

![Utvecklarportalen][api-management-developer-portal-menu]

Klicka på **API: er** och klicka på hello **Kalkylatorn** API.

![Utvecklarportalen][api-management-dev-portal-apis]

Klicka på **prova**.

![Prova][api-management-dev-portal-try-it]

Klicka på **skicka** och notera hello svarsstatusen av **401 obehörig**.

![Skicka][api-management-dev-portal-send-401]

hello-begäran är inte behörig eftersom hello backend API skyddas av Azure Active Directory. Innan du anropar API hello måste hello developer-portalen vara konfigurerade tooauthorize utvecklare som använder OAuth 2.0. Den här processen beskrivs i följande avsnitt hello.

## <a name="register-hello-developer-portal-as-an-aad-application"></a>Registrera hello developer-portalen som ett AAD-program
hello första steget när du konfigurerar hello developer portal tooauthorize utvecklare som använder OAuth 2.0 är tooregister hello developer-portalen som ett AAD-program. Detta visas från 8:27 i hello videon.

Navigera toohello Azure AD-klient från hello första steget i den här videon i det här exemplet **APIMDemo** och navigera toohello **program** fliken.

![nytt program][api-management-aad-new-application-devportal]

Klicka på hello **Lägg till** knappen toocreate ett nytt program för Azure Active Directory och välj **Lägg till ett program som min organisation utvecklar**.

![nytt program][api-management-new-aad-application-menu]

Välj **Web application och/eller webb-API**, ange ett namn och klicka på pilen för hello nästa. I det här exemplet **APIMDeveloperPortal** används.

![nytt program][api-management-aad-new-application-devportal-1]

För **inloggnings-URL** anger hello URL för API Management-tjänsten och Lägg till `/signin`. I det här exemplet `https://contoso5.portal.azure-api.net/signin` används.

För **App-Id-URL** anger hello URL för API Management-tjänsten och lägga till vissa unika tecken. Det kan vara önskade tecken och i det här exemplet `https://contoso5.portal.azure-api.net/dp` används. När hello önskad **appegenskaper** är konfigurerad, klicka på hello markerat toocreate hello program.

![nytt program][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Konfigurera en server för API Management OAuth 2.0-auktorisering
hello nästa steg är tooconfigure en server med OAuth 2.0 auktorisering i API-hantering. Det här steget visar hello video början på 9:43.

Klicka på **säkerhet** hello API Management-menyn hello vänster klickar du på **OAuth 2.0**, och klicka sedan på **lägga till tillståndet** server.

![Lägg till auktorisering server][api-management-add-authorization-server]

Ange ett namn och en valfri beskrivning i hello **namn** och **beskrivning** fält. Dessa fält är används tooidentify hello OAuth 2.0 auktorisering server inom hello API Management service-instans. I det här exemplet **auktorisering server demo** används. Senare när du anger en toobe för OAuth 2.0-server som används för autentisering för ett API som du väljer det här namnet.

För hello **klienten URL för registrering** ange ett platshållarvärde som `http://localhost`.  Hej **klienten URL för registrering** toohello sidan som användarna kan använda toocreate och konfigurera sina egna konton för OAuth 2.0-leverantörer som stöder Användarhantering av konton. I det här exemplet användare inte skapa och konfigurera sina egna konton, så att en platshållare används.

![Lägg till auktorisering server][api-management-add-authorization-server-1]

Ange sedan **slutpunkts-URL-auktorisering** och **Token slutpunkts-URL**.

![auktorisering server][api-management-add-authorization-server-1a]

Dessa värden kan hämtas från hello **App slutpunkter** sidan hello AAD-program som du skapade för hello developer-portalen. tooaccess hello slutpunkter navigera toohello **konfigurera** för hello AAD-program och klicka på **visa slutpunkter**.

![Program][api-management-aad-devportal-application]

![Visa slutpunkter][api-management-aad-view-endpoints]

Kopiera hello **OAuth 2.0 autentiseringsslutpunkt** och klistra in den i hello **slutpunkts-URL-auktorisering** textruta.

![Lägg till auktorisering server][api-management-add-authorization-server-2]

Kopiera hello **OAuth 2.0-token för slutpunkt** och klistra in den i hello **Token slutpunkts-URL** textruta.

![Lägg till auktorisering server][api-management-add-authorization-server-2a]

Dessutom toopasting i hello tokenslutpunkten, lägga till en ytterligare brödtext parameter med namnet **resurs** och använda hello för hello värdet **App-Id URI** från hello AAD-program för hello serverdelstjänst som var skapas när hello Visual Studio-projekt har publicerats.

![URI för App-Id][api-management-aad-sso-uri]

Ange sedan hello klientens autentiseringsuppgifter. Dessa är hello autentiseringsuppgifter för hello-resurs som du vill tooaccess, i det här fallet hello developer-portalen.

![Klientens autentiseringsuppgifter][api-management-client-credentials]

tooget hello **klient-Id**, navigera toohello **konfigurera** för hello AAD-program för hello developer-portalen och kopiera hello **klient-Id**.

tooget hello **Klienthemlighet** klickar du på hello **Markera varaktighet** listrutan i hello **nycklar** avsnittet och ange ett intervall. I det här exemplet används 1 år.

![Klient-ID][api-management-aad-client-id]

Klicka på **spara** toosave hello konfiguration och visa hello nyckel. 

> [!IMPORTANT]
> Anteckna den här nyckeln. När du stänger hello Azure Active Directory configuration kan inte hello nyckel visas igen.
> 
> 

Kopiera hello viktiga toohello Urklipp, växla tillbaka toohello publisher portal klistra in hello nyckel i hello **Klienthemlighet** textruta och klicka på **spara**.

![Lägg till auktorisering server][api-management-add-authorization-server-3]

Direkt efter hello klientens autentiseringsuppgifter är en kod bevilja för auktorisering. Kopiera den här auktoriseringskod och växla tillbaka tooyour portalprogram för Azure AD-utvecklare Konfigurera sidan och klistra in hello authorization grant i hello **Reply URL** fältet och klickar på **spara** igen.

![Reply-URL][api-management-aad-reply-url]

hello nästa steg är tooconfigure hello behörigheter för hello developer-portalen AAD-program. Klicka på **programbehörigheter** och kryssrutan för hello **läsa katalogdata**. Klicka på **spara** toosave detta ändra och klicka sedan på **lägga till program**.

![Lägg till behörigheter][api-management-add-devportal-permissions]

Klicka på sökikonen hello, typen **APIM** till hello från och med rutan, Välj **APIMAADDemo**, och klicka på hello markerat toosave.

![Lägg till behörigheter][api-management-aad-add-app-permissions]

Klicka på **delegerade behörigheter** för **APIMAADDemo** och kryssrutan för hello **åtkomst APIMAADDemo**, och klicka på **spara**. På så sätt kan utvecklare hello portalprogram tooaccess hello backend-tjänsten.

![Lägg till behörigheter][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-hello-calculator-api"></a>Aktivera OAuth 2.0 användarautentiseringen för hello Kalkylatorn API
Hello OAuth 2.0-server är konfigurerat, kan du ange den i hello säkerhetsinställningar för ditt API. Det här steget visar hello video från 14:30.

Klicka på **API: er** i hello vänstra menyn och klickar på **Kalkylatorn** tooview och konfigurera dess inställningar.

![Kalkylatorn API][api-management-calc-api]

Navigera toohello **säkerhet** kontrollerar hello **OAuth 2.0** kryssrutan, Välj hello önskade auktorisering servern från hello **auktorisering server** listrutan och klicka på **Spara**.

![Kalkylatorn API][api-management-enable-aad-calculator]

## <a name="successfully-call-hello-calculator-api-from-hello-developer-portal"></a>Anropa hello Kalkylatorn API från hello developer-portalen
Hello OAuth 2.0-auktorisering är konfigurerat på hello API anropas åtgärderna har från hello developer center. Det här steget visar hello video början på 15:00.

Gå tillbaka toohello **lägga till två heltal** driften av hello Kalkylatorn service i hello developer-portalen och klicka på **prova**. Obs hello nytt objekt i hello **auktorisering** avsnittet motsvarande toohello auktorisering server som du just lagt till.

![Kalkylatorn API][api-management-calc-authorization-server]

Välj **Auktoriseringskoden** från hello auktorisering nedrullningsbara listan och ange hello autentiseringsuppgifter för hello konto toouse. Om du redan har loggat in med hello-konto kan du inte ombedd.

![Kalkylatorn API][api-management-devportal-authorization-code]

Klicka på **skicka** och Observera hello **svarsstatusen** av **200 OK** och hello resultat av hello-åtgärden i hello svar innehåll.

![Kalkylatorn API][api-management-devportal-response]

## <a name="configure-a-desktop-application-toocall-hello-api"></a>Konfigurera ett skrivbordsprogram toocall hello API
hello nästa procedur i hello video börjar vid 16:30 och konfigurerar en enkel skrivbordsprogram toocall hello API. hello första steget är tooregister hello skrivbordsprogram i Azure AD och ge det åtkomst toohello directory och toohello serverdelstjänst. Det finns en demonstration av hello skrivbordsprogram anropar en åtgärd på hello Kalkylatorn API vid 18:25.

## <a name="configure-a-jwt-validation-policy-toopre-authorize-requests"></a>Konfigurera en JWT validering princip toopre-godkänna begäranden
hello sista proceduren i hello videon börjar vid 20:48 och visar hur toouse hello [Validera JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) princip toopre-godkänna begäranden genom att verifiera hello åtkomsttoken för varje inkommande begäran. Om hello begäran inte verifieras av hello Validera JWT princip, hello begäran har blockerats av API-hantering och skickas inte vidare toohello backend.

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
        </claim>
    </required-claims>
</validate-jwt>
```

En annan demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too13:50. Vidarebefordra snabb too15:00 toosee hello principer som konfigurerats i hello redigeraren och sedan too18:50 för en demonstration av anropa en funktion från hello developer-portalen både med och utan hello krävs autentiseringstoken.

## <a name="next-steps"></a>Nästa steg
* Checka ut mer [videor](https://azure.microsoft.com/documentation/videos/index/?services=api-management) om API-hantering.
* För andra sätt toosecure serverdelstjänsten, se [ömsesidig autentisering](api-management-howto-mutual-certificates.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Manage your first API]: api-management-get-started.md
