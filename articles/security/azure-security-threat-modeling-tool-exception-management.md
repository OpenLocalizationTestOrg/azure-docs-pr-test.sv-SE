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
# <a name="security-frame-exception-management--mitigations"></a><span data-ttu-id="21144-103">Säkerhet ram: Hantering av undantag | Åtgärder</span><span class="sxs-lookup"><span data-stu-id="21144-103">Security Frame: Exception Management | Mitigations</span></span> 
| <span data-ttu-id="21144-104">Produkter eller tjänster</span><span class="sxs-lookup"><span data-stu-id="21144-104">Product/Service</span></span> | <span data-ttu-id="21144-105">Artikel</span><span class="sxs-lookup"><span data-stu-id="21144-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="21144-106">**WCF**</span><span class="sxs-lookup"><span data-stu-id="21144-106">**WCF**</span></span> | <ul><li>[<span data-ttu-id="21144-107">WCF - Använd innehåller inte serviceDebug nod i konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="21144-107">WCF- Do not include serviceDebug node in configuration file</span></span>](#servicedebug)</li><li>[<span data-ttu-id="21144-108">WCF - Använd innehåller inte noden serviceMetadata i konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="21144-108">WCF- Do not include serviceMetadata node in configuration file</span></span>](#servicemetadata)</li></ul> |
| <span data-ttu-id="21144-109">**Webb-API**</span><span class="sxs-lookup"><span data-stu-id="21144-109">**Web API**</span></span> | <ul><li>[<span data-ttu-id="21144-110">Se till att rätt undantagshantering är klar i ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="21144-110">Ensure that proper exception handling is done in ASP.NET Web API </span></span>](#exception)</li></ul> |
| <span data-ttu-id="21144-111">**Webbprogram**</span><span class="sxs-lookup"><span data-stu-id="21144-111">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="21144-112">Exponera inte säkerhetsinformation i felmeddelanden</span><span class="sxs-lookup"><span data-stu-id="21144-112">Do not expose security details in error messages </span></span>](#messages)</li><li>[<span data-ttu-id="21144-113">Implementera standard felhantering sida</span><span class="sxs-lookup"><span data-stu-id="21144-113">Implement Default error handling page </span></span>](#default)</li><li>[<span data-ttu-id="21144-114">Ange distributionsmetoden tooRetail i IIS</span><span class="sxs-lookup"><span data-stu-id="21144-114">Set Deployment Method tooRetail in IIS</span></span>](#deployment)</li><li>[<span data-ttu-id="21144-115">Undantag ska misslyckas på ett säkert sätt</span><span class="sxs-lookup"><span data-stu-id="21144-115">Exceptions should fail safely</span></span>](#fail)</li></ul> |

## <span data-ttu-id="21144-116"><a id="servicedebug"></a>WCF - Använd innehåller inte serviceDebug nod i konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="21144-116"><a id="servicedebug"></a>WCF- Do not include serviceDebug node in configuration file</span></span>

| <span data-ttu-id="21144-117">Rubrik</span><span class="sxs-lookup"><span data-stu-id="21144-117">Title</span></span>                   | <span data-ttu-id="21144-118">Information</span><span class="sxs-lookup"><span data-stu-id="21144-118">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="21144-119">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="21144-119">**Component**</span></span>               | <span data-ttu-id="21144-120">WCF</span><span class="sxs-lookup"><span data-stu-id="21144-120">WCF</span></span> | 
| <span data-ttu-id="21144-121">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="21144-121">**SDL Phase**</span></span>               | <span data-ttu-id="21144-122">Utveckla</span><span class="sxs-lookup"><span data-stu-id="21144-122">Build</span></span> |  
| <span data-ttu-id="21144-123">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="21144-123">**Applicable Technologies**</span></span> | <span data-ttu-id="21144-124">Generisk NET Framework 3</span><span class="sxs-lookup"><span data-stu-id="21144-124">Generic, NET Framework 3</span></span> |
| <span data-ttu-id="21144-125">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="21144-125">**Attributes**</span></span>              | <span data-ttu-id="21144-126">Saknas</span><span class="sxs-lookup"><span data-stu-id="21144-126">N/A</span></span>  |
| <span data-ttu-id="21144-127">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="21144-127">**References**</span></span>              | <span data-ttu-id="21144-128">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html)</span><span class="sxs-lookup"><span data-stu-id="21144-128">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html)</span></span> |
| <span data-ttu-id="21144-129">**Steg**</span><span class="sxs-lookup"><span data-stu-id="21144-129">**Steps**</span></span> | <span data-ttu-id="21144-130">Windows Communication Framework (WCF) tjänster kan vara konfigurerade tooexpose felsökningsinformation.</span><span class="sxs-lookup"><span data-stu-id="21144-130">Windows Communication Framework (WCF) services can be configured tooexpose debugging information.</span></span> <span data-ttu-id="21144-131">Felsöka information ska inte användas i produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="21144-131">Debug information should not be used in production environments.</span></span> <span data-ttu-id="21144-132">Hej `<serviceDebug>` tagg definierar om hello debug information funktionen är aktiverad för en WCF-tjänst.</span><span class="sxs-lookup"><span data-stu-id="21144-132">hello `<serviceDebug>` tag defines whether hello debug information feature is enabled for a WCF service.</span></span> <span data-ttu-id="21144-133">Om hello attributet includeExceptionDetailInFaults är tootrue, returneras undantagsinformation från programmet hello tooclients.</span><span class="sxs-lookup"><span data-stu-id="21144-133">If hello attribute includeExceptionDetailInFaults is set tootrue, exception information from hello application will be returned tooclients.</span></span> <span data-ttu-id="21144-134">Angripare kan utnyttja hello ytterligare information som de får från utdata toomount attacker mål på hello framework, databaser eller andra resurser som används av programmet hello-felsökning.</span><span class="sxs-lookup"><span data-stu-id="21144-134">Attackers can leverage hello additional information they gain from debugging output toomount attacks targeted on hello framework, database, or other resources used by hello application.</span></span> |

### <a name="example"></a><span data-ttu-id="21144-135">Exempel</span><span class="sxs-lookup"><span data-stu-id="21144-135">Example</span></span>
<span data-ttu-id="21144-136">hello följande konfigurationsfil innehåller hello `<serviceDebug>` tagg:</span><span class="sxs-lookup"><span data-stu-id="21144-136">hello following configuration file includes hello `<serviceDebug>` tag:</span></span> 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
<span data-ttu-id="21144-137">Inaktivera felsökningsinformation i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="21144-137">Disable debugging information in hello service.</span></span> <span data-ttu-id="21144-138">Detta kan åstadkommas genom att ta bort hello `<serviceDebug>` taggen från programmets konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="21144-138">This can be accomplished by removing hello `<serviceDebug>` tag from your application's configuration file.</span></span> 

## <span data-ttu-id="21144-139"><a id="servicemetadata"></a>WCF - Använd innehåller inte noden serviceMetadata i konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="21144-139"><a id="servicemetadata"></a>WCF- Do not include serviceMetadata node in configuration file</span></span>

| <span data-ttu-id="21144-140">Rubrik</span><span class="sxs-lookup"><span data-stu-id="21144-140">Title</span></span>                   | <span data-ttu-id="21144-141">Information</span><span class="sxs-lookup"><span data-stu-id="21144-141">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="21144-142">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="21144-142">**Component**</span></span>               | <span data-ttu-id="21144-143">WCF</span><span class="sxs-lookup"><span data-stu-id="21144-143">WCF</span></span> | 
| <span data-ttu-id="21144-144">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="21144-144">**SDL Phase**</span></span>               | <span data-ttu-id="21144-145">Utveckla</span><span class="sxs-lookup"><span data-stu-id="21144-145">Build</span></span> |  
| <span data-ttu-id="21144-146">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="21144-146">**Applicable Technologies**</span></span> | <span data-ttu-id="21144-147">Generisk</span><span class="sxs-lookup"><span data-stu-id="21144-147">Generic</span></span> |
| <span data-ttu-id="21144-148">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="21144-148">**Attributes**</span></span>              | <span data-ttu-id="21144-149">Generisk NET Framework 3</span><span class="sxs-lookup"><span data-stu-id="21144-149">Generic, NET Framework 3</span></span> |
| <span data-ttu-id="21144-150">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="21144-150">**References**</span></span>              | <span data-ttu-id="21144-151">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html)</span><span class="sxs-lookup"><span data-stu-id="21144-151">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html)</span></span> |
| <span data-ttu-id="21144-152">**Steg**</span><span class="sxs-lookup"><span data-stu-id="21144-152">**Steps**</span></span> | <span data-ttu-id="21144-153">Offentligt visar information om en tjänst kan angriparna få värdefulla insikter i hur de kan utnyttja hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="21144-153">Publicly exposing information about a service can provide attackers with valuable insight into how they might exploit hello service.</span></span> <span data-ttu-id="21144-154">Hej `<serviceMetadata>` taggen kan hello metadata publiceringsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="21144-154">hello `<serviceMetadata>` tag enables hello metadata publishing feature.</span></span> <span data-ttu-id="21144-155">Klustertjänstens metadata kan innehålla känslig information som inte ska vara tillgängligt för allmänheten.</span><span class="sxs-lookup"><span data-stu-id="21144-155">Service metadata could contain sensitive information that should not be publicly accessible.</span></span> <span data-ttu-id="21144-156">Tillåt endast betrodda användare tooaccess hello metadata och se till att onödig information inte exponeras minst.</span><span class="sxs-lookup"><span data-stu-id="21144-156">At a minimum, only allow trusted users tooaccess hello metadata and ensure that unnecessary information is not exposed.</span></span> <span data-ttu-id="21144-157">Ännu bättre är helt inaktivera hello möjlighet toopublish metadata.</span><span class="sxs-lookup"><span data-stu-id="21144-157">Better yet, entirely disable hello ability toopublish metadata.</span></span> <span data-ttu-id="21144-158">En säker WCF-konfigurationen kommer inte att innehålla hello `<serviceMetadata>` tagg.</span><span class="sxs-lookup"><span data-stu-id="21144-158">A safe WCF configuration will not contain hello `<serviceMetadata>` tag.</span></span> |

## <span data-ttu-id="21144-159"><a id="exception"></a>Se till att rätt undantagshantering är klar i ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="21144-159"><a id="exception"></a>Ensure that proper exception handling is done in ASP.NET Web API</span></span>

| <span data-ttu-id="21144-160">Rubrik</span><span class="sxs-lookup"><span data-stu-id="21144-160">Title</span></span>                   | <span data-ttu-id="21144-161">Information</span><span class="sxs-lookup"><span data-stu-id="21144-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="21144-162">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="21144-162">**Component**</span></span>               | <span data-ttu-id="21144-163">Webb-API</span><span class="sxs-lookup"><span data-stu-id="21144-163">Web API</span></span> | 
| <span data-ttu-id="21144-164">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="21144-164">**SDL Phase**</span></span>               | <span data-ttu-id="21144-165">Utveckla</span><span class="sxs-lookup"><span data-stu-id="21144-165">Build</span></span> |  
| <span data-ttu-id="21144-166">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="21144-166">**Applicable Technologies**</span></span> | <span data-ttu-id="21144-167">MVC 5, MVC 6</span><span class="sxs-lookup"><span data-stu-id="21144-167">MVC 5, MVC 6</span></span> |
| <span data-ttu-id="21144-168">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="21144-168">**Attributes**</span></span>              | <span data-ttu-id="21144-169">Saknas</span><span class="sxs-lookup"><span data-stu-id="21144-169">N/A</span></span>  |
| <span data-ttu-id="21144-170">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="21144-170">**References**</span></span>              | <span data-ttu-id="21144-171">[Undantagshantering i ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [modell verifiering i ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api)</span><span class="sxs-lookup"><span data-stu-id="21144-171">[Exception Handling in ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [Model Validation in ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api)</span></span> |
| <span data-ttu-id="21144-172">**Steg**</span><span class="sxs-lookup"><span data-stu-id="21144-172">**Steps**</span></span> | <span data-ttu-id="21144-173">Som standard är mest undantagsfel utan felhantering i ASP.NET Web API översättas till ett HTTP-svar med statuskoden`500, Internal Server Error`</span><span class="sxs-lookup"><span data-stu-id="21144-173">By default, most uncaught exceptions in ASP.NET Web API are translated into an HTTP response with status code `500, Internal Server Error`</span></span>|

### <a name="example"></a><span data-ttu-id="21144-174">Exempel</span><span class="sxs-lookup"><span data-stu-id="21144-174">Example</span></span>
<span data-ttu-id="21144-175">toocontrol hello statuskoden som returnerades av hello API, `HttpResponseException` kan användas som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="21144-175">toocontrol hello status code returned by hello API, `HttpResponseException` can be used as shown below:</span></span> 
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

### <a name="example"></a><span data-ttu-id="21144-176">Exempel</span><span class="sxs-lookup"><span data-stu-id="21144-176">Example</span></span>
<span data-ttu-id="21144-177">För ytterligare kontroll på hello undantag svar hello `HttpResponseMessage` klassen kan användas som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="21144-177">For further control on hello exception response, hello `HttpResponseMessage` class can be used as shown below:</span></span> 
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
<span data-ttu-id="21144-178">toocatch ohanterade undantag som inte är av typen hello `HttpResponseException`, undantagsfilter som kan användas.</span><span class="sxs-lookup"><span data-stu-id="21144-178">toocatch unhandled exceptions that are not of hello type `HttpResponseException`, Exception Filters can be used.</span></span> <span data-ttu-id="21144-179">Undantagsfilter implementera hello `System.Web.Http.Filters.IExceptionFilter` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="21144-179">Exception filters implement hello `System.Web.Http.Filters.IExceptionFilter` interface.</span></span> <span data-ttu-id="21144-180">hello enklaste sättet toowrite ett undantagsfilter är tooderive från hello `System.Web.Http.Filters.ExceptionFilterAttribute` klassen och åsidosätta hello OnException metod.</span><span class="sxs-lookup"><span data-stu-id="21144-180">hello simplest way toowrite an exception filter is tooderive from hello `System.Web.Http.Filters.ExceptionFilterAttribute` class and override hello OnException method.</span></span> 

### <a name="example"></a><span data-ttu-id="21144-181">Exempel</span><span class="sxs-lookup"><span data-stu-id="21144-181">Example</span></span>
<span data-ttu-id="21144-182">Här är ett filter som konverterar `NotImplementedException` undantag i HTTP-statuskod `501, Not Implemented`:</span><span class="sxs-lookup"><span data-stu-id="21144-182">Here is a filter that converts `NotImplementedException` exceptions into HTTP status code `501, Not Implemented`:</span></span> 
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

<span data-ttu-id="21144-183">Det finns flera sätt tooregister ett undantagsfilter webb-API:</span><span class="sxs-lookup"><span data-stu-id="21144-183">There are several ways tooregister a Web API exception filter:</span></span>
- <span data-ttu-id="21144-184">Åtgärden</span><span class="sxs-lookup"><span data-stu-id="21144-184">By action</span></span>
- <span data-ttu-id="21144-185">Av domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="21144-185">By controller</span></span>
- <span data-ttu-id="21144-186">Globalt</span><span class="sxs-lookup"><span data-stu-id="21144-186">Globally</span></span>

### <a name="example"></a><span data-ttu-id="21144-187">Exempel</span><span class="sxs-lookup"><span data-stu-id="21144-187">Example</span></span>
<span data-ttu-id="21144-188">tooapply hello filtrera tooa specifik åtgärd, lägga till hello filter som ett attribut toohello åtgärd:</span><span class="sxs-lookup"><span data-stu-id="21144-188">tooapply hello filter tooa specific action, add hello filter as an attribute toohello action:</span></span> 
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
### <a name="example"></a><span data-ttu-id="21144-189">Exempel</span><span class="sxs-lookup"><span data-stu-id="21144-189">Example</span></span>
<span data-ttu-id="21144-190">tooapply hello filter tooall hello åtgärder på en `controller`, lägga till hello filter som ett attribut toohello `controller` klass:</span><span class="sxs-lookup"><span data-stu-id="21144-190">tooapply hello filter tooall of hello actions on a `controller`, add hello filter as an attribute toohello `controller` class:</span></span> 

```C#
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a><span data-ttu-id="21144-191">Exempel</span><span class="sxs-lookup"><span data-stu-id="21144-191">Example</span></span>
<span data-ttu-id="21144-192">tooapply hello filtrera globalt tooall Web API-styrenheter, lägga till en instans av hello filter toohello `GlobalConfiguration.Configuration.Filters` samling.</span><span class="sxs-lookup"><span data-stu-id="21144-192">tooapply hello filter globally tooall Web API controllers, add an instance of hello filter toohello `GlobalConfiguration.Configuration.Filters` collection.</span></span> <span data-ttu-id="21144-193">Undantagsfilter i den här samlingen används tooany Web API-styrenhet åtgärd.</span><span class="sxs-lookup"><span data-stu-id="21144-193">Exception filters in this collection apply tooany Web API controller action.</span></span> 
```C#
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a><span data-ttu-id="21144-194">Exempel</span><span class="sxs-lookup"><span data-stu-id="21144-194">Example</span></span>
<span data-ttu-id="21144-195">För modellverifiering av överföras hello modellen tillstånd tooCreateErrorResponse metoden enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="21144-195">For model validation, hello model state can be passed tooCreateErrorResponse method as shown below:</span></span> 
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

<span data-ttu-id="21144-196">Kontrollera hello länkarna under hello referenser för ytterligare information om särskilda hantering och modellverifiering i ASP.Net Web API</span><span class="sxs-lookup"><span data-stu-id="21144-196">Check hello links in hello references section for additional details about exceptional handling and model validation in ASP.Net Web API</span></span> 

## <span data-ttu-id="21144-197"><a id="messages"></a>Exponera inte säkerhetsinformation i felmeddelanden</span><span class="sxs-lookup"><span data-stu-id="21144-197"><a id="messages"></a>Do not expose security details in error messages</span></span>

| <span data-ttu-id="21144-198">Rubrik</span><span class="sxs-lookup"><span data-stu-id="21144-198">Title</span></span>                   | <span data-ttu-id="21144-199">Information</span><span class="sxs-lookup"><span data-stu-id="21144-199">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="21144-200">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="21144-200">**Component**</span></span>               | <span data-ttu-id="21144-201">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="21144-201">Web Application</span></span> | 
| <span data-ttu-id="21144-202">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="21144-202">**SDL Phase**</span></span>               | <span data-ttu-id="21144-203">Utveckla</span><span class="sxs-lookup"><span data-stu-id="21144-203">Build</span></span> |  
| <span data-ttu-id="21144-204">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="21144-204">**Applicable Technologies**</span></span> | <span data-ttu-id="21144-205">Generisk</span><span class="sxs-lookup"><span data-stu-id="21144-205">Generic</span></span> |
| <span data-ttu-id="21144-206">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="21144-206">**Attributes**</span></span>              | <span data-ttu-id="21144-207">Saknas</span><span class="sxs-lookup"><span data-stu-id="21144-207">N/A</span></span>  |
| <span data-ttu-id="21144-208">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="21144-208">**References**</span></span>              | <span data-ttu-id="21144-209">Saknas</span><span class="sxs-lookup"><span data-stu-id="21144-209">N/A</span></span>  |
| <span data-ttu-id="21144-210">**Steg**</span><span class="sxs-lookup"><span data-stu-id="21144-210">**Steps**</span></span> | <p><span data-ttu-id="21144-211">Allmänt felmeddelanden tillhandahålls direkt toohello användare utan inklusive känsliga programdata.</span><span class="sxs-lookup"><span data-stu-id="21144-211">Generic error messages are provided directly toohello user without including sensitive application data.</span></span> <span data-ttu-id="21144-212">Exempel på känsliga data:</span><span class="sxs-lookup"><span data-stu-id="21144-212">Examples of sensitive data include:</span></span></p><ul><li><span data-ttu-id="21144-213">Servernamn</span><span class="sxs-lookup"><span data-stu-id="21144-213">Server names</span></span></li><li><span data-ttu-id="21144-214">Anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="21144-214">Connection strings</span></span></li><li><span data-ttu-id="21144-215">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="21144-215">Usernames</span></span></li><li><span data-ttu-id="21144-216">Lösenord</span><span class="sxs-lookup"><span data-stu-id="21144-216">Passwords</span></span></li><li><span data-ttu-id="21144-217">SQL-procedurer</span><span class="sxs-lookup"><span data-stu-id="21144-217">SQL procedures</span></span></li><li><span data-ttu-id="21144-218">Information om dynamisk SQL-fel</span><span class="sxs-lookup"><span data-stu-id="21144-218">Details of dynamic SQL failures</span></span></li><li><span data-ttu-id="21144-219">Stackspårning och rader med kod</span><span class="sxs-lookup"><span data-stu-id="21144-219">Stack trace and lines of code</span></span></li><li><span data-ttu-id="21144-220">Variabler som lagrats i minnet</span><span class="sxs-lookup"><span data-stu-id="21144-220">Variables stored in memory</span></span></li><li><span data-ttu-id="21144-221">Enhet och mapp</span><span class="sxs-lookup"><span data-stu-id="21144-221">Drive and folder locations</span></span></li><li><span data-ttu-id="21144-222">Programmet installera punkter</span><span class="sxs-lookup"><span data-stu-id="21144-222">Application install points</span></span></li><li><span data-ttu-id="21144-223">Konfigurationsinställningar för värden</span><span class="sxs-lookup"><span data-stu-id="21144-223">Host configuration settings</span></span></li><li><span data-ttu-id="21144-224">Andra interna programinformation</span><span class="sxs-lookup"><span data-stu-id="21144-224">Other internal application details</span></span></li></ul><p><span data-ttu-id="21144-225">Svällning alla fel i ett program och ger allmänna felmeddelanden samt aktivera anpassade fel i IIS kan förhindra att information röjs.</span><span class="sxs-lookup"><span data-stu-id="21144-225">Trapping all errors within an application and providing generic error messages, as well as enabling custom errors within IIS will help prevent information disclosure.</span></span> <span data-ttu-id="21144-226">SQL Server-databas och .NET undantagshantering, bland annat felhantering arkitekturer, är särskilt utförlig och mycket användbara tooa angripare profilering av ditt program.</span><span class="sxs-lookup"><span data-stu-id="21144-226">SQL Server database and .NET Exception handling, among other error handling architectures, are especially verbose and extremely useful tooa malicious user profiling your application.</span></span> <span data-ttu-id="21144-227">Gör inte direkt visa hello innehållet i en klass härledd från hello .NET undantagsklass och se till att du har rätt undantagshantering så att ett oväntat undantag inte oavsiktligt aktiveras direkt toohello användare.</span><span class="sxs-lookup"><span data-stu-id="21144-227">Do not directly display hello contents of a class derived from hello .NET Exception class, and ensure that you have proper exception handling so that an unexpected exception isn't inadvertently raised directly toohello user.</span></span></p><ul><li><span data-ttu-id="21144-228">Ange allmänna felmeddelanden direkt toohello användare som abstrakt in specifik information direkt i hello undantag/felmeddelande</span><span class="sxs-lookup"><span data-stu-id="21144-228">Provide generic error messages directly toohello user that abstract away specific details found directly in hello exception/error message</span></span></li><li><span data-ttu-id="21144-229">Inte visa hello innehållet i en .NET-undantag klassen direkt toohello användare</span><span class="sxs-lookup"><span data-stu-id="21144-229">Do not display hello contents of a .NET exception class directly toohello user</span></span></li><li><span data-ttu-id="21144-230">Fånga alla felmeddelanden och om det är lämpligt informera hello användare via ett allmänt fel message skickade toohello programmet klient</span><span class="sxs-lookup"><span data-stu-id="21144-230">Trap all error messages and if appropriate inform hello user via a generic error message sent toohello application client</span></span></li><li><span data-ttu-id="21144-231">Exponera inte hello innehållet i hello undantagsklass direkt toohello användare, särskilt hello returvärde från `.ToString()`, eller hello värdena för hello-meddelande eller stackspårning egenskaper.</span><span class="sxs-lookup"><span data-stu-id="21144-231">Do not expose hello contents of hello Exception class directly toohello user, especially hello return value from `.ToString()`, or hello values of hello Message or StackTrace properties.</span></span> <span data-ttu-id="21144-232">Loggar den här informationen och visa en mer harmlöst meddelandet toohello användare på ett säkert sätt</span><span class="sxs-lookup"><span data-stu-id="21144-232">Securely log this information and display a more innocuous message toohello user</span></span></li></ul>|

## <span data-ttu-id="21144-233"><a id="default"></a>Implementera standard felhantering sida</span><span class="sxs-lookup"><span data-stu-id="21144-233"><a id="default"></a>Implement Default error handling page</span></span>

| <span data-ttu-id="21144-234">Rubrik</span><span class="sxs-lookup"><span data-stu-id="21144-234">Title</span></span>                   | <span data-ttu-id="21144-235">Information</span><span class="sxs-lookup"><span data-stu-id="21144-235">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="21144-236">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="21144-236">**Component**</span></span>               | <span data-ttu-id="21144-237">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="21144-237">Web Application</span></span> | 
| <span data-ttu-id="21144-238">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="21144-238">**SDL Phase**</span></span>               | <span data-ttu-id="21144-239">Utveckla</span><span class="sxs-lookup"><span data-stu-id="21144-239">Build</span></span> |  
| <span data-ttu-id="21144-240">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="21144-240">**Applicable Technologies**</span></span> | <span data-ttu-id="21144-241">Generisk</span><span class="sxs-lookup"><span data-stu-id="21144-241">Generic</span></span> |
| <span data-ttu-id="21144-242">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="21144-242">**Attributes**</span></span>              | <span data-ttu-id="21144-243">Saknas</span><span class="sxs-lookup"><span data-stu-id="21144-243">N/A</span></span>  |
| <span data-ttu-id="21144-244">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="21144-244">**References**</span></span>              | <span data-ttu-id="21144-245">[Redigera ASP.NET inställningar dialogrutan felsidor](https://technet.microsoft.com/library/dd569096(WS.10).aspx)</span><span class="sxs-lookup"><span data-stu-id="21144-245">[Edit ASP.NET Error Pages Settings Dialog Box](https://technet.microsoft.com/library/dd569096(WS.10).aspx)</span></span> |
| <span data-ttu-id="21144-246">**Steg**</span><span class="sxs-lookup"><span data-stu-id="21144-246">**Steps**</span></span> | <p><span data-ttu-id="21144-247">När ett ASP.NET-program misslyckas och gör en HTTP/1.x 500 Internt serverfel eller en funktionskonfiguration (till exempel begäransfiltrering) förhindrar att en sida visas, genereras ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="21144-247">When an ASP.NET application fails and causes an HTTP/1.x 500 Internal Server Error, or a feature configuration (such as Request Filtering) prevents a page from being displayed, an error message will be generated.</span></span> <span data-ttu-id="21144-248">Administratörer kan välja huruvida hello program ska visa ett meddelande toohello klienten, detaljerade fel message toohello klient eller detaljerade fel meddelande toolocalhost.</span><span class="sxs-lookup"><span data-stu-id="21144-248">Administrators can choose whether or not hello application should display a friendly message toohello client, detailed error message toohello client, or detailed error message toolocalhost only.</span></span> <span data-ttu-id="21144-249">Hej <customErrors> i hello web.config har tre lägen:</span><span class="sxs-lookup"><span data-stu-id="21144-249">hello <customErrors> tag in hello web.config has three modes:</span></span></p><ul><li><span data-ttu-id="21144-250">**På:** anger att anpassade fel är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="21144-250">**On:** Specifies that custom errors are enabled.</span></span> <span data-ttu-id="21144-251">Om inget defaultRedirect attribut anges visas ett allmänt fel användare.</span><span class="sxs-lookup"><span data-stu-id="21144-251">If no defaultRedirect attribute is specified, users see a generic error.</span></span> <span data-ttu-id="21144-252">hello anpassade fel visas toohello fjärrklienter och toohello lokal värd</span><span class="sxs-lookup"><span data-stu-id="21144-252">hello custom errors are shown toohello remote clients and toohello local host</span></span></li><li><span data-ttu-id="21144-253">**Av:** anger att anpassade fel är inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="21144-253">**Off:** Specifies that custom errors are disabled.</span></span> <span data-ttu-id="21144-254">hello detaljerad ASP.NET-fel visas toohello fjärrklienter och toohello lokal värd</span><span class="sxs-lookup"><span data-stu-id="21144-254">hello detailed ASP.NET errors are shown toohello remote clients and toohello local host</span></span></li><li><span data-ttu-id="21144-255">**RemoteOnly:** anger att anpassade fel visas endast toohello fjärrklienter och ASP.NET-fel visas toohello lokala värden.</span><span class="sxs-lookup"><span data-stu-id="21144-255">**RemoteOnly:** Specifies that custom errors are shown only toohello remote clients, and that ASP.NET errors are shown toohello local host.</span></span> <span data-ttu-id="21144-256">Detta är standardvärdet för hello</span><span class="sxs-lookup"><span data-stu-id="21144-256">This is hello default value</span></span></li></ul><p><span data-ttu-id="21144-257">Öppna hello `web.config` filen hello-program eller-platsen och kontrollera att hello taggen har antingen `<customErrors mode="RemoteOnly" />` eller `<customErrors mode="On" />` definieras.</span><span class="sxs-lookup"><span data-stu-id="21144-257">Open hello `web.config` file for hello application/site and ensure that hello tag has either `<customErrors mode="RemoteOnly" />` or `<customErrors mode="On" />` defined.</span></span></p>|

## <span data-ttu-id="21144-258"><a id="deployment"></a>Ange distributionsmetoden tooRetail i IIS</span><span class="sxs-lookup"><span data-stu-id="21144-258"><a id="deployment"></a>Set Deployment Method tooRetail in IIS</span></span>

| <span data-ttu-id="21144-259">Rubrik</span><span class="sxs-lookup"><span data-stu-id="21144-259">Title</span></span>                   | <span data-ttu-id="21144-260">Information</span><span class="sxs-lookup"><span data-stu-id="21144-260">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="21144-261">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="21144-261">**Component**</span></span>               | <span data-ttu-id="21144-262">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="21144-262">Web Application</span></span> | 
| <span data-ttu-id="21144-263">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="21144-263">**SDL Phase**</span></span>               | <span data-ttu-id="21144-264">Distribution</span><span class="sxs-lookup"><span data-stu-id="21144-264">Deployment</span></span> |  
| <span data-ttu-id="21144-265">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="21144-265">**Applicable Technologies**</span></span> | <span data-ttu-id="21144-266">Generisk</span><span class="sxs-lookup"><span data-stu-id="21144-266">Generic</span></span> |
| <span data-ttu-id="21144-267">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="21144-267">**Attributes**</span></span>              | <span data-ttu-id="21144-268">Saknas</span><span class="sxs-lookup"><span data-stu-id="21144-268">N/A</span></span>  |
| <span data-ttu-id="21144-269">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="21144-269">**References**</span></span>              | <span data-ttu-id="21144-270">[distribution Element (inställningsschema för ASP.NET)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx)</span><span class="sxs-lookup"><span data-stu-id="21144-270">[deployment Element (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx)</span></span> |
| <span data-ttu-id="21144-271">**Steg**</span><span class="sxs-lookup"><span data-stu-id="21144-271">**Steps**</span></span> | <p><span data-ttu-id="21144-272">Hej `<deployment retail>` växel är avsedd att användas av IIS produktionsservrar.</span><span class="sxs-lookup"><span data-stu-id="21144-272">hello `<deployment retail>` switch is intended for use by production IIS servers.</span></span> <span data-ttu-id="21144-273">Den här växeln är används toohelp program körs med hello bästa möjliga prestanda och minsta möjliga säkerhetsinformation läckage genom att inaktivera hello programmets förmåga toogenerate spårningsutdata på en sida, inaktivera hello möjlighet toodisplay feldetaljer meddelanden tooend användare och inaktivera hello debug-växeln.</span><span class="sxs-lookup"><span data-stu-id="21144-273">This switch is used toohelp applications run with hello best possible performance and least possible security information leakages by disabling hello application's ability toogenerate trace output on a page, disabling hello ability toodisplay detailed error messages tooend users, and disabling hello debug switch.</span></span></p><p><span data-ttu-id="21144-274">Ofta, växlar och alternativ som är fokuserad utvecklare, t.ex. Det gick inte begära spårning och felsökning aktiveras under aktiv utveckling.</span><span class="sxs-lookup"><span data-stu-id="21144-274">Often times, switches and options that are developer-focused, such as failed request tracing and debugging, are enabled during active development.</span></span> <span data-ttu-id="21144-275">Du rekommenderas att tooretail anges hello distributionsmetoden på en produktionsserver.</span><span class="sxs-lookup"><span data-stu-id="21144-275">It is recommended that hello deployment method on any production server be set tooretail.</span></span> <span data-ttu-id="21144-276">Öppna hello machine.config-filen och se till att `<deployment retail="true" />` förblir ange tootrue.</span><span class="sxs-lookup"><span data-stu-id="21144-276">open hello machine.config file and ensure that `<deployment retail="true" />` remains set tootrue.</span></span></p>|

## <span data-ttu-id="21144-277"><a id="fail"></a>Undantag ska misslyckas på ett säkert sätt</span><span class="sxs-lookup"><span data-stu-id="21144-277"><a id="fail"></a>Exceptions should fail safely</span></span>

| <span data-ttu-id="21144-278">Rubrik</span><span class="sxs-lookup"><span data-stu-id="21144-278">Title</span></span>                   | <span data-ttu-id="21144-279">Information</span><span class="sxs-lookup"><span data-stu-id="21144-279">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="21144-280">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="21144-280">**Component**</span></span>               | <span data-ttu-id="21144-281">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="21144-281">Web Application</span></span> | 
| <span data-ttu-id="21144-282">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="21144-282">**SDL Phase**</span></span>               | <span data-ttu-id="21144-283">Utveckla</span><span class="sxs-lookup"><span data-stu-id="21144-283">Build</span></span> |  
| <span data-ttu-id="21144-284">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="21144-284">**Applicable Technologies**</span></span> | <span data-ttu-id="21144-285">Generisk</span><span class="sxs-lookup"><span data-stu-id="21144-285">Generic</span></span> |
| <span data-ttu-id="21144-286">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="21144-286">**Attributes**</span></span>              | <span data-ttu-id="21144-287">Saknas</span><span class="sxs-lookup"><span data-stu-id="21144-287">N/A</span></span>  |
| <span data-ttu-id="21144-288">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="21144-288">**References**</span></span>              | [<span data-ttu-id="21144-289">Misslyckas på ett säkert sätt</span><span class="sxs-lookup"><span data-stu-id="21144-289">Fail securely</span></span>](https://www.owasp.org/index.php/Fail_securely) |
| <span data-ttu-id="21144-290">**Steg**</span><span class="sxs-lookup"><span data-stu-id="21144-290">**Steps**</span></span> | <span data-ttu-id="21144-291">Programmet ska misslyckas på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="21144-291">Application should fail safely.</span></span> <span data-ttu-id="21144-292">Valfri metod som returnerar ett booleskt värde baserat på vilka vissa beslut fattas bör ha undantagsblock noggrant skapas.</span><span class="sxs-lookup"><span data-stu-id="21144-292">Any method that returns a Boolean value, based on which certain decision is made, should have exception block carefully created.</span></span> <span data-ttu-id="21144-293">Det finns många logiska fel på grund av toowhich säkerhet problem krypning i när hello undantagsblock skrivs vårdslöst.</span><span class="sxs-lookup"><span data-stu-id="21144-293">There are lot of logical errors due toowhich security issues creep in, when hello exception block is written carelessly.</span></span>|

### <a name="example"></a><span data-ttu-id="21144-294">Exempel</span><span class="sxs-lookup"><span data-stu-id="21144-294">Example</span></span>
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
<span data-ttu-id="21144-295">hello senare metoden returnerar alltid True, om vissa undantag inträffar.</span><span class="sxs-lookup"><span data-stu-id="21144-295">hello above method will always return True, if some exception happens.</span></span> <span data-ttu-id="21144-296">Om slutanvändaren hello innehåller en felaktig URL, som hello webbläsare värnar men hello `Uri()` konstruktorn inte detta genereras ett undantagsfel och hello drabbade vidtas toohello giltiga men felaktigt URL.</span><span class="sxs-lookup"><span data-stu-id="21144-296">If hello end user provides a malformed URL, that hello browser respects, but hello `Uri()` constructor doesn't, this will throw an exception, and hello victim will be taken toohello valid but malformed URL.</span></span> 
