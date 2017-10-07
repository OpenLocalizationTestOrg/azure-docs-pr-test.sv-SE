---
title: aaaException Management - hotet Modeling verktyget - Azure | Microsoft Docs
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
ms.openlocfilehash: 247096c10deeca94ebb9b19df7ba60e442ca1e4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-exception-management--mitigations"></a>Säkerhet ram: Hantering av undantag | Åtgärder 
| Produkter eller tjänster | Artikel |
| --------------- | ------- |
| **WCF** | <ul><li>[WCF - Använd innehåller inte serviceDebug nod i konfigurationsfilen](#servicedebug)</li><li>[WCF - Använd innehåller inte noden serviceMetadata i konfigurationsfilen](#servicemetadata)</li></ul> |
| **Webb-API** | <ul><li>[Se till att rätt undantagshantering är klar i ASP.NET Web API](#exception)</li></ul> |
| **Webbprogram** | <ul><li>[Exponera inte säkerhetsinformation i felmeddelanden](#messages)</li><li>[Implementera standard felhantering sida](#default)</li><li>[Ange distributionsmetoden tooRetail i IIS](#deployment)</li><li>[Undantag ska misslyckas på ett säkert sätt](#fail)</li></ul> |

## <a id="servicedebug"></a>WCF - Använd innehåller inte serviceDebug nod i konfigurationsfilen

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk NET Framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | Windows Communication Framework (WCF) tjänster kan vara konfigurerade tooexpose felsökningsinformation. Felsöka information ska inte användas i produktionsmiljöer. Hej `<serviceDebug>` tagg definierar om hello debug information funktionen är aktiverad för en WCF-tjänst. Om hello attributet includeExceptionDetailInFaults är tootrue, returneras undantagsinformation från programmet hello tooclients. Angripare kan utnyttja hello ytterligare information som de får från utdata toomount attacker mål på hello framework, databaser eller andra resurser som används av programmet hello-felsökning. |

### <a name="example"></a>Exempel
hello följande konfigurationsfil innehåller hello `<serviceDebug>` tagg: 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
Inaktivera felsökningsinformation i hello-tjänsten. Detta kan åstadkommas genom att ta bort hello `<serviceDebug>` taggen från programmets konfigurationsfil. 

## <a id="servicemetadata"></a>WCF - Använd innehåller inte noden serviceMetadata i konfigurationsfilen

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Generisk NET Framework 3 |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | Offentligt visar information om en tjänst kan angriparna få värdefulla insikter i hur de kan utnyttja hello-tjänsten. Hej `<serviceMetadata>` taggen kan hello metadata publiceringsfunktionen. Klustertjänstens metadata kan innehålla känslig information som inte ska vara tillgängligt för allmänheten. Tillåt endast betrodda användare tooaccess hello metadata och se till att onödig information inte exponeras minst. Ännu bättre är helt inaktivera hello möjlighet toopublish metadata. En säker WCF-konfigurationen kommer inte att innehålla hello `<serviceMetadata>` tagg. |

## <a id="exception"></a>Se till att rätt undantagshantering är klar i ASP.NET Web API

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC 5, MVC 6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Undantagshantering i ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [modell verifiering i ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Steg** | Som standard är mest undantagsfel utan felhantering i ASP.NET Web API översättas till ett HTTP-svar med statuskoden`500, Internal Server Error`|

### <a name="example"></a>Exempel
toocontrol hello statuskoden som returnerades av hello API, `HttpResponseException` kan användas som visas nedan: 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        throw new HttpResponseException(HttpStatusCode.NotFound);
    }
    return item;
}
```

### <a name="example"></a>Exempel
För ytterligare kontroll på hello undantag svar hello `HttpResponseMessage` klassen kan användas som visas nedan: 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        var resp = new HttpResponseMessage(HttpStatusCode.NotFound)
        {
            Content = new StringContent(string.Format("No product with ID = {0}", id)),
            ReasonPhrase = "Product ID Not Found"
        }
        throw new HttpResponseException(resp);
    }
    return item;
}
```
toocatch ohanterade undantag som inte är av typen hello `HttpResponseException`, undantagsfilter som kan användas. Undantagsfilter implementera hello `System.Web.Http.Filters.IExceptionFilter` gränssnitt. hello enklaste sättet toowrite ett undantagsfilter är tooderive från hello `System.Web.Http.Filters.ExceptionFilterAttribute` klassen och åsidosätta hello OnException metod. 

### <a name="example"></a>Exempel
Här är ett filter som konverterar `NotImplementedException` undantag i HTTP-statuskod `501, Not Implemented`: 
```C#
namespace ProductStore.Filters
{
    using System;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http.Filters;

    public class NotImplExceptionFilterAttribute : ExceptionFilterAttribute 
    {
        public override void OnException(HttpActionExecutedContext context)
        {
            if (context.Exception is NotImplementedException)
            {
                context.Response = new HttpResponseMessage(HttpStatusCode.NotImplemented);
            }
        }
    }
}
```

Det finns flera sätt tooregister ett undantagsfilter webb-API:
- Åtgärden
- Av domänkontrollant
- Globalt

### <a name="example"></a>Exempel
tooapply hello filtrera tooa specifik åtgärd, lägga till hello filter som ett attribut toohello åtgärd: 
```C#
public class ProductsController : ApiController
{
    [NotImplExceptionFilter]
    public Contact GetContact(int id)
    {
        throw new NotImplementedException("This method is not implemented");
    }
}
```
### <a name="example"></a>Exempel
tooapply hello filter tooall hello åtgärder på en `controller`, lägga till hello filter som ett attribut toohello `controller` klass: 

```C#
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a>Exempel
tooapply hello filtrera globalt tooall Web API-styrenheter, lägga till en instans av hello filter toohello `GlobalConfiguration.Configuration.Filters` samling. Undantagsfilter i den här samlingen används tooany Web API-styrenhet åtgärd. 
```C#
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a>Exempel
För modellverifiering av överföras hello modellen tillstånd tooCreateErrorResponse metoden enligt nedan: 
```C#
public HttpResponseMessage PostProduct(Product item)
{
    if (!ModelState.IsValid)
    {
        return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
    }
    // Implementation not shown...
}
```

Kontrollera hello länkarna under hello referenser för ytterligare information om särskilda hantering och modellverifiering i ASP.Net Web API 

## <a id="messages"></a>Exponera inte säkerhetsinformation i felmeddelanden

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>Allmänt felmeddelanden tillhandahålls direkt toohello användare utan inklusive känsliga programdata. Exempel på känsliga data:</p><ul><li>Servernamn</li><li>Anslutningssträngar</li><li>Användarnamn</li><li>Lösenord</li><li>SQL-procedurer</li><li>Information om dynamisk SQL-fel</li><li>Stackspårning och rader med kod</li><li>Variabler som lagrats i minnet</li><li>Enhet och mapp</li><li>Programmet installera punkter</li><li>Konfigurationsinställningar för värden</li><li>Andra interna programinformation</li></ul><p>Svällning alla fel i ett program och ger allmänna felmeddelanden samt aktivera anpassade fel i IIS kan förhindra att information röjs. SQL Server-databas och .NET undantagshantering, bland annat felhantering arkitekturer, är särskilt utförlig och mycket användbara tooa angripare profilering av ditt program. Gör inte direkt visa hello innehållet i en klass härledd från hello .NET undantagsklass och se till att du har rätt undantagshantering så att ett oväntat undantag inte oavsiktligt aktiveras direkt toohello användare.</p><ul><li>Ange allmänna felmeddelanden direkt toohello användare som abstrakt in specifik information direkt i hello undantag/felmeddelande</li><li>Inte visa hello innehållet i en .NET-undantag klassen direkt toohello användare</li><li>Fånga alla felmeddelanden och om det är lämpligt informera hello användare via ett allmänt fel message skickade toohello programmet klient</li><li>Exponera inte hello innehållet i hello undantagsklass direkt toohello användare, särskilt hello returvärde från `.ToString()`, eller hello värdena för hello-meddelande eller stackspårning egenskaper. Loggar den här informationen och visa en mer harmlöst meddelandet toohello användare på ett säkert sätt</li></ul>|

## <a id="default"></a>Implementera standard felhantering sida

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Redigera ASP.NET inställningar dialogrutan felsidor](https://technet.microsoft.com/library/dd569096(WS.10).aspx) |
| **Steg** | <p>När ett ASP.NET-program misslyckas och gör en HTTP/1.x 500 Internt serverfel eller en funktionskonfiguration (till exempel begäransfiltrering) förhindrar att en sida visas, genereras ett felmeddelande. Administratörer kan välja huruvida hello program ska visa ett meddelande toohello klienten, detaljerade fel message toohello klient eller detaljerade fel meddelande toolocalhost. Hej <customErrors> i hello web.config har tre lägen:</p><ul><li>**På:** anger att anpassade fel är aktiverat. Om inget defaultRedirect attribut anges visas ett allmänt fel användare. hello anpassade fel visas toohello fjärrklienter och toohello lokal värd</li><li>**Av:** anger att anpassade fel är inaktiverat. hello detaljerad ASP.NET-fel visas toohello fjärrklienter och toohello lokal värd</li><li>**RemoteOnly:** anger att anpassade fel visas endast toohello fjärrklienter och ASP.NET-fel visas toohello lokala värden. Detta är standardvärdet för hello</li></ul><p>Öppna hello `web.config` filen hello-program eller-platsen och kontrollera att hello taggen har antingen `<customErrors mode="RemoteOnly" />` eller `<customErrors mode="On" />` definieras.</p>|

## <a id="deployment"></a>Ange distributionsmetoden tooRetail i IIS

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [distribution Element (inställningsschema för ASP.NET)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx) |
| **Steg** | <p>Hej `<deployment retail>` växel är avsedd att användas av IIS produktionsservrar. Den här växeln är används toohelp program körs med hello bästa möjliga prestanda och minsta möjliga säkerhetsinformation läckage genom att inaktivera hello programmets förmåga toogenerate spårningsutdata på en sida, inaktivera hello möjlighet toodisplay feldetaljer meddelanden tooend användare och inaktivera hello debug-växeln.</p><p>Ofta, växlar och alternativ som är fokuserad utvecklare, t.ex. Det gick inte begära spårning och felsökning aktiveras under aktiv utveckling. Du rekommenderas att tooretail anges hello distributionsmetoden på en produktionsserver. Öppna hello machine.config-filen och se till att `<deployment retail="true" />` förblir ange tootrue.</p>|

## <a id="fail"></a>Undantag ska misslyckas på ett säkert sätt

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Misslyckas på ett säkert sätt](https://www.owasp.org/index.php/Fail_securely) |
| **Steg** | Programmet ska misslyckas på ett säkert sätt. Valfri metod som returnerar ett booleskt värde baserat på vilka vissa beslut fattas bör ha undantagsblock noggrant skapas. Det finns många logiska fel på grund av toowhich säkerhet problem krypning i när hello undantagsblock skrivs vårdslöst.|

### <a name="example"></a>Exempel
```C#
        public static bool ValidateDomain(string pathToValidate, Uri currentUrl)
        {
            try
            {
                if (!string.IsNullOrWhiteSpace(pathToValidate))
                {
                    var domain = RetrieveDomain(currentUrl);
                    var replyPath = new Uri(pathToValidate);
                    var replyDomain = RetrieveDomain(replyPath);

                    if (string.Compare(domain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                    {
                        //// Adding additional check tooenable CMS urls if they are not hosted on same domain.
                        if (!string.IsNullOrWhiteSpace(Utilities.CmsBase))
                        {
                            var cmsDomain = RetrieveDomain(new Uri(Utilities.Base.Trim()));
                            if (string.Compare(cmDomain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                            {
                                return false;
                            }
                            else
                            {
                                return true;
                            }
                        }

                        return false;
                    }
                }

                return true;
            }
            catch (UriFormatException ex)
            {
                LogHelper.LogException("Utilities:ValidateDomain", ex);
                return true;
            }
        }
```
hello senare metoden returnerar alltid True, om vissa undantag inträffar. Om slutanvändaren hello innehåller en felaktig URL, som hello webbläsare värnar men hello `Uri()` konstruktorn inte detta genereras ett undantagsfel och hello drabbade vidtas toohello giltiga men felaktigt URL. 
