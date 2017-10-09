---
title: aaaSession Management - hotet Modeling verktyget - Azure | Microsoft Docs
description: "ändringar för hot som exponeras i hello hot Modeling verktyget"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 915ffae3f775ca6902fcfb93e7e1952ce85612f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-session-management--articles"></a>Säkerhet ram: Sessionshantering | Artiklar 
| Produkter eller tjänster | Artikel |
| --------------- | ------- |
| **Azure AD**    | <ul><li>[Implementera rätt logga ut med hjälp av ADAL metoder när du använder Azure AD](#logout-adal)</li></ul> |
| IoT-enhet | <ul><li>[Använd begränsad livslängd för genererade SaS-token](#finite-tokens)</li></ul> |
| **Azure dokumentet DB** | <ul><li>[Använda minsta livstid för token för den genererade resurs token](#resource-tokens)</li></ul> |
| **ADFS** | <ul><li>[Implementera rätt logga ut med WsFederation metoder när du använder AD FS](#wsfederation-logout)</li></ul> |
| **Identity Server** | <ul><li>[Implementera rätt logga ut när du använder Identity Server](#proper-logout)</li></ul> |
| **Webbprogram** | <ul><li>[Program som är tillgängliga via HTTPS måste använda säkra cookies](#https-secure-cookies)</li><li>[Alla HTTP-baserade program bör ange http endast för cookie-definition](#cookie-definition)</li><li>[Skyddar mot attacker webbplatser begära förfalskning (CSRF) för ASP.NET-webbsidor](#csrf-asp)</li><li>[Ställ in sessionen för inaktivitet livslängd](#inactivity-lifetime)</li><li>[Implementera rätt logga ut från hello program](#proper-app-logout)</li></ul> |
| **Webb-API** | <ul><li>[Skyddar mot attacker webbplatser begära förfalskning (CSRF) på ASP.NET Web API: er](#csrf-api)</li></ul> |

## <a id="logout-adal"></a>Implementera rätt logga ut med hjälp av ADAL metoder när du använder Azure AD

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure AD | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Om programmet hello beroende åtkomst-token som utfärdas av Azure AD, anropa händelsehanteraren för hello logga ut |

### <a name="example"></a>Exempel
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a>Exempel
Det bör också förstöra användarsession genom att anropa metoden Session.Abandon(). Följande metod visar säker implementering av användaren logga ut:
```C#
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <a id="finite-tokens"></a>Använd begränsad livslängd för genererade SaS-token

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | IoT-enhet | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | SaS-token genereras för att autentisera tooAzure IoT-hubben ska ha en begränsad upphör att gälla. Behåll hello SaS token livslängd tooa minsta toolimit hello tidsperiod de spelas om hello token komprometteras.|

## <a id="resource-tokens"></a>Använda minsta livstid för token för den genererade resurs token

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure dokumentet DB | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Minska hello timespan av resursen token tooa lägsta värde som krävs. Resurs-token har ett giltigt timespan standardvärdet 1 timme.|

## <a id="wsfederation-logout"></a>Implementera rätt logga ut med WsFederation metoder när du använder AD FS

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | ADFS | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Om hello programmet är beroende av STS-token som utfärdas av AD FS, anropa hello logga ut händelsehanteraren WSFederationAuthenticationModule.FederatedSignOut() metoden toolog ut hello användare. Även hello aktuell session förstöras och hello session token-värde ska återställa och undanröjda.|

### <a name="example"></a>Exempel
```C#
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes hello user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at hello specified security token service (STS) by using hello WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of hello current session and raises hello appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at hello specified security token service (STS) by using hello WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <a id="proper-logout"></a>Implementera rätt logga ut när du använder Identity Server

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Identity Server | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Federerad IdentityServer3 logga ut](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| **Steg** | IdentityServer stöder hello möjlighet toofederate med externa identitetsleverantörer. När en användare loggar ut från en överordnad identitetsleverantör, kan beroende på hello-protokoll som används, det vara möjligt tooreceive ett meddelande när hello användaren loggar ut. Det gör att IdentityServer toonotify klienter så att de kan också logga hello användaren ut. Hello dokumentationen i hello referensavsnittet för hello implementeringsdetaljer.|

## <a id="https-secure-cookies"></a>Program som är tillgängliga via HTTPS måste använda säkra cookies

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | EnvironmentType - OnPrem |
| **Referenser**              | [httpCookies Element (inställningsschema för ASP.NET)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure egenskapen](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx) |
| **Steg** | Cookies är normalt bara tillgänglig toohello domän som de har begränsats. Tyvärr inkluderas hello definitionen för ”domain” inte hello protokollet så att cookies som skapas via HTTPS är tillgänglig via HTTP. Hej ”säker” attributet anger toohello webbläsare som hello cookie ska endast göras tillgängliga via HTTPS. Kontrollera att alla cookies som via HTTPS använder hello **säker** attribut. hello krav kan tillämpas i hello web.config-filen genom att ange hello requireSSL attributet tootrue. Det är hello önskade metoden eftersom den kommer att verkställa hello **säker** attribut för alla aktuella och framtida cookies utan hello måste toomake ändringar ytterligare kod.|

### <a name="example"></a>Exempel
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
hello inställningen tillämpas även om HTTP används tooaccess hello program. Om du använder HTTP tooaccess Hej program, hello inställningen radbrytningar hello programmet eftersom hello cookies konfigureras med hello säker attribut och hello webbläsare inte kommer att skicka dem tillbaka toohello program.

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Webbformulär, MVC5 |
| **Attribut**              | EnvironmentType - OnPrem |
| **Referenser**              | Saknas  |
| **Steg** | När hello webbprogrammet är hello förlitande part och hello IdP är AD FS-servern, secure hello FedAuth token-attributet kan konfigureras genom att ange requireSSL tooTrue i `system.identityModel.services` avsnitt i web.config:|

### <a name="example"></a>Exempel
```C#
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <a id="cookie-definition"></a>Alla HTTP-baserade program bör ange http endast för cookie-definition

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Säker Cookie-attribut](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| **Steg** | toomitigate hello risken för avslöjande av information med en attack med globala webbplatsskript (XSS), ett nytt attribut - httpOnly - var introducerades toocookies och stöds av alla större webbläsare. hello attribut anger en cookie inte är tillgänglig via skript. Med hjälp av HttpOnly cookies, minskar ett webbprogram hello risk för att känslig information i hello cookie kan stulen via skript och skickas tooan angripare webbplats. |

### <a name="example"></a>Exempel
Alla HTTP-baserade program som använder cookies ska ange HttpOnly i hello cookie definition genom att implementera följande konfiguration i web.config:
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Web Forms |
| **Attribut**              | Saknas  |
| **Referenser**              | [Egenskapen FormsAuthentication.RequireSSL](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| **Steg** | Hej RequireSSL egenskapsvärde anges i hello konfigurationsfilen för ett ASP.NET-program med hjälp av hello requireSSL attribut för hello konfigurationselementet. Du kan ange i hello Web.config-filen för ASP.NET-program om SSL (Secure Sockets Layer) är nödvändiga tooreturn hello formulärautentisering cookie toohello servern genom att inställningen hello requireSSL attribut.|

### <a name="example"></a>Exempel 
hello anger följande kodexempel hello requireSSL attribut i hello Web.config-filen.
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC5 |
| **Attribut**              | EnvironmentType - OnPrem |
| **Referenser**              | [Windows Identity Foundation (WIF) konfiguration – del II](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| **Steg** | tooset httpOnly attributet för FedAuth cookies, hideFromCsript attributvärdet ska ställas in tooTrue. |

### <a name="example"></a>Exempel
Följande konfiguration visar hello korrekt konfiguration:
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <a id="csrf-asp"></a>Skyddar mot attacker webbplatser begära förfalskning (CSRF) för ASP.NET-webbsidor

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Begäran förfalskning (CSRF eller XSRF) är en typ av angrepp där angriparen kan utföra åtgärder i hello säkerhetskontext för en annan användare upprättad session på en webbplats. hello målet är toomodify eller ta bort innehåll, om hello riktade webbplats använder uteslutande session cookies tooauthenticate tog emot begäran. En angripare kan utnyttja detta problem genom att hämta en annan användare webbläsare tooload en URL med ett kommando från en sårbar plats där hello användaren är redan inloggad. Det finns många sätt för en angripare toodo att, exempelvis av en annan webbplats som läses in en resurs från hello sårbara server eller hämtning hello användaren tooclick en länk. hello attack kan förhindras om hello servern skickar klienten en ytterligare token toohello, kräver hello klienten tooinclude den token i alla kommande begäranden och verifierar att alla kommande begäranden inkluderar en token som gäller toohello aktuella sessionen, exempelvis genom att använder hello ASP.NET AntiForgeryToken eller ViewState. |

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC5 MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [XSRF/CSRF förebyggande i ASP.NET MVC och webbsidor](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| **Steg** | Formulär för skydd mot CSRF och ASP.NET MVC - Använd hello `AntiForgeryToken` hjälpmetod på vyer; placera en `Html.AntiForgeryToken()` till hello format, t.ex.|

### <a name="example"></a>Exempel
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a>Exempel
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Exempel
Vid hello samma tid, Html.AntiForgeryToken() ger hello besökare en cookie kallas __RequestVerificationToken med hello samma värde som hello slumpmässiga dolda som visas ovan. Sedan lägger till toovalidate en inkommande formuläret post hello [ValidateAntiForgeryToken] filter toohello mål åtgärdsmetod. Exempel:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Auktoriseringsfilter som kontrollerar att:
* hello inkommande begäran har en cookie som kallas __RequestVerificationToken
* hello inkommande begäran har en `Request.Form` post med namnet __RequestVerificationToken
* Dessa cookies och `Request.Form` värden matchar förutsatt att alla är korrekt, hello begäran går igenom som vanligt. Men om det inte finns sedan ett auktoriseringsfel med meddelandet ”en obligatorisk antiförfalskningstoken har inte angetts eller var ogiltig”. 

### <a name="example"></a>Exempel
Skydd mot CSRF och AJAX: hello formuläret token kan vara ett problem för AJAX-begäranden, eftersom en AJAX-begäran kan skicka JSON-data, inte data i HTML-formulär. En lösning är toosend hello token i ett anpassat HTTP-huvud. hello följande kod använder Razor syntax toogenerate hello token och lägger sedan till hello token tooan AJAX-begäran. 
```C#
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Exempel
När du bearbetar hello begäran extrahera hello token från hello huvudet i begäran. Anropa sedan hello AntiForgery.Validate metoden toovalidate hello token. hello Validate-metoden genereras ett undantag om hello token inte är giltiga.
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Web Forms |
| **Attribut**              | Saknas  |
| **Referenser**              | [Dra nytta av ASP.NET inbyggda funktioner tooFend av Web-attacker](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| **Steg** | CSRF attacker i WebForm baserat program kan begränsas genom att ange ViewStateUserKey tooa slumpmässig sträng som varierar för varje användare - användar-ID eller bättre ännu, sessions-ID. För ett antal tekniska och är sessions-ID en mycket bättre anpassning eftersom en session-ID är oförutsägbart, timeout och varierar per användare.|

### <a name="example"></a>Exempel
Här är hello-kod som du behöver toohave i alla sidor:
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <a id="inactivity-lifetime"></a>Ställ in sessionen för inaktivitet livslängd

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Egenskapen HttpSessionState.Timeout](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx) |
| **Steg** | Tidsgräns för session representerar hello händelse inträffar när en användare inte utför någon åtgärd på en webbplats under ett intervall (definieras av webbserver). Hej händelse på serversidan, ändra hello status för hello användarens session too'invalid' (till exempel ”används inte längre”) och instruera hello web server toodestroy den (ta bort alla data som finns i den). hello anger följande kodexempel hello tidsgränsen för sessionen attributet too15 minuter i hello Web.config-filen.|

### <a name="example"></a>Exempel
'''XML-koden <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Web Forms |
| **Attribut**              | Saknas  |
| **Referenser**              | [Element för formulär för autentisering (inställningsschema för ASP.NET)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx) |
| **Steg** | Ange hello biljetten för formulär för autentisering cookie-timeout too15 minuter|

### <a name="example"></a>Exempel
''' XML-kod<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When hello web application is Relying Party and ADFS is hello STS, hello lifetime of hello authentication cookies - FedAuth tokens - can be set by hello following configuration in web.config:|

### Example
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use hello code below tooenable encryption-decryption of claims received from ADFS. Thumbprint value varies based on hello certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a>Exempel
Även hello ADFS utfärdade token SAML-anspråk livstid ska anges too15 minuter, genom att köra följande powershell-kommando på ADFS-server för hello hello:
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <a id="proper-app-logout"></a>Implementera rätt logga ut från hello program

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Utföra rätt logga ut från programmet hello, när användaren trycker på Logga ut knappen. På Logga ut, bör program förstör användarens session, och även återställa och upphäver session cookie-värde, tillsammans med att återställa och också ångras autentisering cookie-värde. Dessutom när flera sessioner är bundet tooa enanvändarläge identitet, måste de gemensamt avslutas på serversidan för hello på timeout eller logga ut. Kontrollera slutligen att logga ut funktionalitet är tillgänglig på varje sida. |

## <a id="csrf-api"></a>Skyddar mot attacker webbplatser begära förfalskning (CSRF) på ASP.NET Web API: er

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Begäran förfalskning (CSRF eller XSRF) är en typ av angrepp där angriparen kan utföra åtgärder i hello säkerhetskontext för en annan användare upprättad session på en webbplats. hello målet är toomodify eller ta bort innehåll, om hello riktade webbplats använder uteslutande session cookies tooauthenticate tog emot begäran. En angripare kan utnyttja detta problem genom att hämta en annan användare webbläsare tooload en URL med ett kommando från en sårbar plats där hello användaren är redan inloggad. Det finns många sätt för en angripare toodo att, exempelvis av en annan webbplats som läses in en resurs från hello sårbara server eller hämtning hello användaren tooclick en länk. hello attack kan förhindras om hello servern skickar klienten en ytterligare token toohello, kräver hello klienten tooinclude den token i alla kommande begäranden och verifierar att alla kommande begäranden inkluderar en token som gäller toohello aktuella sessionen, exempelvis genom att använder hello ASP.NET AntiForgeryToken eller ViewState. |

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC5 MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Förhindra attacker med förfalskning (CSRF) av begäran i ASP.NET webb-API](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| **Steg** | Skydd mot CSRF och AJAX: hello formuläret token kan vara ett problem för AJAX-begäranden, eftersom en AJAX-begäran kan skicka JSON-data, inte data i HTML-formulär. En lösning är toosend hello token i ett anpassat HTTP-huvud. hello följande kod använder Razor syntax toogenerate hello token och lägger sedan till hello token tooan AJAX-begäran. |

### <a name="example"></a>Exempel
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Exempel
När du bearbetar hello begäran extrahera hello token från hello huvudet i begäran. Anropa sedan hello AntiForgery.Validate metoden toovalidate hello token. hello Validate-metoden genereras ett undantag om hello token inte är giltiga.
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a>Exempel
Anti-CSRF och formulär för ASP.NET MVC - Använd hello AntiForgeryToken hjälpmetod på vyer. Placera en Html.AntiForgeryToken() i hello formulär, t.ex.
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a>Exempel
hello-exemplet ovan kommer att skrivas ut ungefär så hello följande:
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Exempel
Vid hello samma tid, Html.AntiForgeryToken() ger hello besökare en cookie kallas __RequestVerificationToken med hello samma värde som hello slumpmässiga dolda som visas ovan. Sedan lägger till toovalidate en inkommande formuläret post hello [ValidateAntiForgeryToken] filter toohello mål åtgärdsmetod. Exempel:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Auktoriseringsfilter som kontrollerar att:
* hello inkommande begäran har en cookie som kallas __RequestVerificationToken
* hello inkommande begäran har en `Request.Form` post med namnet __RequestVerificationToken
* Dessa cookies och `Request.Form` värden matchar förutsatt att alla är korrekt, hello begäran går igenom som vanligt. Men om det inte finns sedan ett auktoriseringsfel med meddelandet ”en obligatorisk antiförfalskningstoken har inte angetts eller var ogiltig”.

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC5 MVC6 |
| **Attribut**              | Identitet Provider - ADFS, identitetsleverantör - Azure AD |
| **Referenser**              | [Skydda ett webb-API med enskilda konton och lokala inloggning i ASP.NET Web API 2.2](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| **Steg** | Om hello Web API skyddas med hjälp av OAuth 2.0, då förväntar sig ett ägartoken i tillståndet begäran sidhuvud och beviljar åtkomst toohello begäran endast om hello token är giltig. Till skillnad från cookie-baserad autentisering Anslut inte webbläsare hello ägar-token toorequests. hello begär klienten måste tooexplicitly bifoga hello ägar-token i hello huvudet i begäran. ASP.NET Web API: er skyddas med hjälp av OAuth 2.0, betraktas därför ägar-token som skydd mot attacker CSRF. Observera att om hello MVC-delen av programmet hello använder formulärautentisering (d.v.s. använder cookies), skydd mot förfalskning token har toobe som används av hello MVC-webbapp. |

### <a name="example"></a>Exempel
hello Web API har toobe informeras toorely bara på ägar-token och inte på cookies. Det kan göras med följande konfiguration i hello `WebApiConfig.Register` metod: '''C skarpa kod config. SuppressDefaultHostAuthentication(); Config. Filters.Add (nya HostAuthenticationFilter(OAuthDefaults.AuthenticationType));
```
hello SuppressDefaultHostAuthentication method tells Web API tooignore any authentication that happens before hello request reaches hello Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API tooauthenticate only using bearer tokens.
