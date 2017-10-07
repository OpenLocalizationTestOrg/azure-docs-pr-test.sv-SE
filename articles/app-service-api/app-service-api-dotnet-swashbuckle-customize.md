---
title: aaaCustomize Swashbuckle-genererade API-definitioner
description: "Lär dig hur toocustomize Swagger API-definitioner som genereras av Swashbuckle för en API-app i Azure App Service."
services: app-service\api
documentationcenter: .net
author: bradygaster
manager: erikre
editor: jimbe
ms.assetid: 6b8cbc38-d282-4a0f-b0c5-762631bae6f3
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: e31c665f8993533c5ec9a935e42cce34f86a5ade
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-swashbuckle-generated-api-definitions"></a><span data-ttu-id="f1d3e-103">Anpassa Swashbuckle-genererade API-definitioner</span><span class="sxs-lookup"><span data-stu-id="f1d3e-103">Customize Swashbuckle-generated API definitions</span></span>
## <a name="overview"></a><span data-ttu-id="f1d3e-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f1d3e-104">Overview</span></span>
<span data-ttu-id="f1d3e-105">Den här artikeln förklarar hur toocustomize Swashbuckle toohandle vanliga scenarier där du kanske vill tooalter hello standardbeteendet:</span><span class="sxs-lookup"><span data-stu-id="f1d3e-105">This article explains how toocustomize Swashbuckle toohandle common scenarios where you may want tooalter hello default behavior:</span></span>

* <span data-ttu-id="f1d3e-106">Swashbuckle genererar dubbla åtgärden identifierare för överlagringar av kontrollantmetoder</span><span class="sxs-lookup"><span data-stu-id="f1d3e-106">Swashbuckle generates duplicate operation identifiers for overloads of controller methods</span></span>
* <span data-ttu-id="f1d3e-107">Swashbuckle förutsätter att hello endast giltigt svar från en metod är HTTP 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="f1d3e-107">Swashbuckle assumes that hello only valid response from a method is HTTP 200 (OK)</span></span> 

## <a name="customize-operation-identifier-generation"></a><span data-ttu-id="f1d3e-108">Anpassa generera identifierare</span><span class="sxs-lookup"><span data-stu-id="f1d3e-108">Customize operation identifier generation</span></span>
<span data-ttu-id="f1d3e-109">Swashbuckle genererar Swagger åtgärden identifierare genom att sammanbinda Kontrollnamn och metodnamn.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-109">Swashbuckle generates Swagger operation identifiers by concatenating controller name and method name.</span></span> <span data-ttu-id="f1d3e-110">Det här mönstret skapar ett problem när du har flera överlagringar av en metod: Swashbuckle genererar duplicerade åtgärds-ID, vilket är ogiltigt Swagger JSON.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-110">This pattern creates a problem when you have multiple overloads of a method: Swashbuckle generates duplicate operation ids, which is invalid Swagger JSON.</span></span>

<span data-ttu-id="f1d3e-111">Till exempel gör hello följande kontrollantkoden Swashbuckle toogenerate tre Contact_Get åtgärden ID: n.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-111">For example, hello following controller code causes Swashbuckle toogenerate three Contact_Get operation ids.</span></span>

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

<span data-ttu-id="f1d3e-112">Du kan lösa problemet hello manuellt genom att ge hello metoder unika namn, till exempel hello följande för det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="f1d3e-112">You can solve hello problem manually by giving hello methods unique names, such as hello following for this example:</span></span>

* <span data-ttu-id="f1d3e-113">Hämta</span><span class="sxs-lookup"><span data-stu-id="f1d3e-113">Get</span></span>
* <span data-ttu-id="f1d3e-114">GetById</span><span class="sxs-lookup"><span data-stu-id="f1d3e-114">GetById</span></span>
* <span data-ttu-id="f1d3e-115">GetPage</span><span class="sxs-lookup"><span data-stu-id="f1d3e-115">GetPage</span></span>

<span data-ttu-id="f1d3e-116">hello alternativ är tooextend Swashbuckle toomake den automatiskt generera unika åtgärds-ID: n.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-116">hello alternative is tooextend Swashbuckle toomake it automatically generate unique operation ids.</span></span>

<span data-ttu-id="f1d3e-117">hello följande steg visar hur toocustomize Swashbuckle med hjälp av hello *SwaggerConfig.cs* fil som ingår i hello projekt som hello projektmall för Visual Studio API Apps Preview.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-117">hello following steps show how toocustomize Swashbuckle by using hello *SwaggerConfig.cs* file that is included in hello project by hello Visual Studio API Apps Preview project template.</span></span>  <span data-ttu-id="f1d3e-118">Du kan också anpassa Swashbuckle i ett Web API-projekt som du konfigurerar för distribution som en API-app.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-118">You can also customize Swashbuckle in a Web API project that you configure for deployment as an API app.</span></span>

1. <span data-ttu-id="f1d3e-119">Skapa en anpassad `IOperationFilter` implementering</span><span class="sxs-lookup"><span data-stu-id="f1d3e-119">Create a custom `IOperationFilter` implementation</span></span> 
   
    <span data-ttu-id="f1d3e-120">Hej `IOperationFilter` gränssnittet tillhandahåller en utökningsbarhet för Swashbuckle-användare som vill toocustomize olika aspekter av processen för hello Swagger-metadata.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-120">hello `IOperationFilter` interface provides an extensibility point for Swashbuckle users who want toocustomize various aspects of hello Swagger metadata process.</span></span> <span data-ttu-id="f1d3e-121">hello visar följande kod en metod för att ändra beteende för hello generations-id-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-121">hello following code demonstrates one method of changing hello operation-id-generation behavior.</span></span> <span data-ttu-id="f1d3e-122">hello koden lägger till parametern toohello åtgärden id namn.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-122">hello code appends parameter names toohello operation id name.</span></span>  
   
        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
   
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }
2. <span data-ttu-id="f1d3e-123">I *App_Start\SwaggerConfig.cs* fil, anrop hello `OperationFilter` metoden toocause Swashbuckle toouse hello nya `IOperationFilter` implementering.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-123">In *App_Start\SwaggerConfig.cs* file, call hello `OperationFilter` method toocause Swashbuckle toouse hello new `IOperationFilter` implementation.</span></span>
   
        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)
   
    <span data-ttu-id="f1d3e-124">Hej *SwaggerConfig.cs* filen som har tagits bort av hello Swashbuckle NuGet-paketet innehåller många kommenterats ut på punkter.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-124">hello *SwaggerConfig.cs* file that is dropped in by hello Swashbuckle NuGet package contains many commented-out examples of extensibility points.</span></span> <span data-ttu-id="f1d3e-125">hello ytterligare kommentarer visas inte här.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-125">hello additional comments are not shown here.</span></span> 
   
    <span data-ttu-id="f1d3e-126">När du har gjort den här ändringen din `IOperationFilter` implementering används och orsakar unika åtgärds-ID: n toobe genereras.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-126">After you make this change, your `IOperationFilter` implementation is used and causes unique operation ids toobe generated.</span></span>
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>

## <a name="allow-response-codes-other-than-200"></a><span data-ttu-id="f1d3e-127">Tillåt svarskoder än 200</span><span class="sxs-lookup"><span data-stu-id="f1d3e-127">Allow response codes other than 200</span></span>
<span data-ttu-id="f1d3e-128">Som standard Swashbuckle förutsätter att en HTTP-200 (OK) svar hello *endast* berättigade svar från en webb-API-metod.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-128">By default, Swashbuckle assumes that an HTTP 200 (OK) response is hello *only* legitimate response from a Web API method.</span></span> <span data-ttu-id="f1d3e-129">I vissa fall kan kanske du vill tooreturn andra svarskoder utan att orsaka hello klienten tooraise ett undantag.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-129">In some cases, you may want tooreturn other response codes without causing hello client tooraise an exception.</span></span>  <span data-ttu-id="f1d3e-130">Till exempel visar hello följande Web API-kod ett scenario där du vill ha hello klienten tooaccept en 200 eller ett 404 som giltigt svar.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-130">For example, hello following Web API code demonstrates a scenario in which you would want hello client tooaccept either a 200 or a 404 as valid responses.</span></span>

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

<span data-ttu-id="f1d3e-131">I det här scenariot anger hello Swagger Swashbuckle genererar som standard bara en giltig HTTP-statuskod, HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-131">In this scenario, hello Swagger that Swashbuckle generates by default specifies only one legitimate HTTP status code, HTTP 200.</span></span>

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

<span data-ttu-id="f1d3e-132">Eftersom Visual Studio använder hello Swagger API-definition toogenerate koden för hello klienten, skapar klientkod som genererar ett undantag för eventuella svar än en HTTP-200.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-132">Since Visual Studio uses hello Swagger API definition toogenerate code for hello client, it creates client code that raises an exception for any response other than an HTTP 200.</span></span> <span data-ttu-id="f1d3e-133">hello koden nedan är från en C#-klient som genererats för det här exemplet Web API-metoden.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-133">hello code below is from a C# client generated for this sample Web API method.</span></span>

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

<span data-ttu-id="f1d3e-134">Swashbuckle tillhandahåller två sätt att anpassa hello lista över förväntade HTTP-svarskoder som den skapar med hjälp av XML-kommentarer eller hello `SwaggerResponse` attribut.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-134">Swashbuckle provides two ways of customizing hello list of expected HTTP response codes that it generates, using XML comments or hello `SwaggerResponse` attribute.</span></span> <span data-ttu-id="f1d3e-135">hello-attributet är enklare, men det är endast tillgänglig i Swashbuckle 5.1.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-135">hello attribute is easier, but it is only available in Swashbuckle 5.1.5 or later.</span></span> <span data-ttu-id="f1d3e-136">hello mall för API Apps preview nytt projekt i Visual Studio 2013 innehåller Swashbuckle version 5.0.0, så om du använde hello mall och inte vill tooupdate Swashbuckle är ditt enda alternativ toouse XML-kommentarer.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-136">hello API Apps preview new-project template in Visual Studio 2013 includes Swashbuckle version 5.0.0, so if you used hello template and don't want tooupdate Swashbuckle, your only option is toouse XML comments.</span></span> 

### <a name="customize-expected-response-codes-using-xml-comments"></a><span data-ttu-id="f1d3e-137">Anpassa förväntade svarskoder med XML-kommentarer</span><span class="sxs-lookup"><span data-stu-id="f1d3e-137">Customize expected response codes using XML comments</span></span>
<span data-ttu-id="f1d3e-138">Använd den här metoden toospecify svarskoder om Swashbuckle-versionen är tidigare än 5.1.5.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-138">Use this method toospecify response codes if your Swashbuckle version is earlier than 5.1.5.</span></span>

1. <span data-ttu-id="f1d3e-139">Lägg först till XML-dokumentationskommentarer över hello-metoder som du vill toospecify HTTP svarskoder för.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-139">First, add XML documentation comments over hello methods you wish toospecify HTTP response codes for.</span></span> <span data-ttu-id="f1d3e-140">Åtgärda hello exempel Web API visas ovan och använder hello XML-dokumentation tooit skulle leda till kod som hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-140">Taking hello sample Web API action shown above and applying hello XML documentation tooit would result in code like hello following example.</span></span> 
   
        /// <summary>
        /// Returns hello specified contact.
        /// </summary>
        /// <param name="id">hello ID of hello contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
   
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
   
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }
2. <span data-ttu-id="f1d3e-141">Lägga till instruktioner i hello *SwaggerConfig.cs* filen toodirect Swashbuckle toomake användning av hello XML-dokumentationsfil.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-141">Add instructions in hello *SwaggerConfig.cs* file toodirect Swashbuckle toomake use of hello XML documentation file.</span></span>
   
   * <span data-ttu-id="f1d3e-142">Öppna *SwaggerConfig.cs* och skapa en metod på hello *SwaggerConfig* klassen toospecify hello sökvägen toohello XML-dokumentationsfil.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-142">Open *SwaggerConfig.cs* and create a method on hello *SwaggerConfig* class toospecify hello path toohello documentation XML file.</span></span> 
     
           private static string GetXmlCommentsPath()
           {
               return string.Format(@"{0}\XmlComments.xml", 
                   System.AppDomain.CurrentDomain.BaseDirectory);
           }
   * <span data-ttu-id="f1d3e-143">Rulla nedåt i hello *SwaggerConfig.cs* tills du ser hello kommenterats ut kodrad i hello skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-143">Scroll down in hello *SwaggerConfig.cs* file until you see hello commented-out line of code resembling hello screen shot below.</span></span> 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
   * <span data-ttu-id="f1d3e-144">Ta bort kommentarerna hello rad tooenable hello XML-kommentarer bearbetning under generering av Swagger.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-144">Uncomment hello line tooenable hello XML comments processing during Swagger generation.</span></span> 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
3. <span data-ttu-id="f1d3e-145">Gå till hello projektegenskaperna i ordning toogenerate hello XML-dokumentationsfil, och aktivera hello XML-dokumentationsfil enligt hello skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-145">In order toogenerate hello XML documentation file, go into hello project's properties and enable hello XML documentation file as shown in hello screenshot below.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

<span data-ttu-id="f1d3e-146">När du utför dessa steg visar hello Swagger JSON som genererats av Swashbuckle hello HTTP svarskoder som du angav i hello XML-kommentarer.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-146">Once you perform these steps, hello Swagger JSON generated by Swashbuckle will reflect hello HTTP response codes that you specified in hello XML comments.</span></span> <span data-ttu-id="f1d3e-147">hello skärmbilden nedan visar den här nya JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-147">hello screenshot below demonstrates this new JSON payload.</span></span> 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

<span data-ttu-id="f1d3e-148">När du använder Visual Studio tooregenerate hello klientkod för REST-API accepterar hello C#-kod både hello HTTP OK och gick inte att hitta statuskoder utan att ett undantag, så att dina konsumerande kod toomake beslut på hur toohandle hello tillbaka en null Kontakta post.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-148">When you use Visual Studio tooregenerate hello client code for your REST API, hello C# code accepts both hello HTTP OK and Not Found status codes without raising an exception, allowing your consuming code toomake decisions on how toohandle hello return of a null Contact record.</span></span> 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

<span data-ttu-id="f1d3e-149">hello-koden för den här demonstrationen finns i [GitHub-lagringsplatsen](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse).</span><span class="sxs-lookup"><span data-stu-id="f1d3e-149">hello code for this demonstration can be found in [this GitHub repository](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse).</span></span> <span data-ttu-id="f1d3e-150">Tillsammans med hello Web API är-projekt har definierats med XML-dokumentationskommentarer ett konsolprogram projekt som innehåller en genererad klient för detta API.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-150">Along with hello Web API project marked up with XML documentation comments is a Console Application project that contains a generated client for this API.</span></span> 

### <a name="customize-expected-response-codes-using-hello-swaggerresponse-attribute"></a><span data-ttu-id="f1d3e-151">Anpassa förväntade svarskoder med hello SwaggerResponse attribut</span><span class="sxs-lookup"><span data-stu-id="f1d3e-151">Customize expected response codes using hello SwaggerResponse attribute</span></span>
<span data-ttu-id="f1d3e-152">Hej [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribut som är tillgängliga i Swashbuckle 5.1.5 och senare.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-152">hello [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribute is available in Swashbuckle 5.1.5 and later.</span></span> <span data-ttu-id="f1d3e-153">Om du har en tidigare version i ditt projekt, startar det här avsnittet förklarar hur tooupdate hello Swashbuckle NuGet-paketet så att du kan använda det här attributet.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-153">In case you have an earlier version in your project, this section starts by explaining how tooupdate hello Swashbuckle NuGet package so that you can use this attribute.</span></span>

1. <span data-ttu-id="f1d3e-154">I **Solution Explorer**, högerklicka på ditt webb-API-projekt och klickar på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-154">In **Solution Explorer**, right-click your Web API project and click **Manage NuGet Packages**.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)
2. <span data-ttu-id="f1d3e-155">Klicka på hello *uppdatering* knappen Nästa toohello *Swashbuckle* NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-155">Click hello *Update* button next toohello *Swashbuckle* NuGet package.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)
3. <span data-ttu-id="f1d3e-156">Lägg till hello *SwaggerResponse* attribut toohello Web API åtgärdsmetoder som du vill toospecify giltig HTTP-svarskoder.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-156">Add hello *SwaggerResponse* attributes toohello Web API action methods for which you want toospecify valid HTTP response codes.</span></span> 
   
        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
   
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }
4. <span data-ttu-id="f1d3e-157">Lägg till en `using` -instruktion för hello attributets namnområde:</span><span class="sxs-lookup"><span data-stu-id="f1d3e-157">Add a `using` statement for hello attribute's namespace:</span></span>
   
        using Swashbuckle.Swagger.Annotations;
5. <span data-ttu-id="f1d3e-158">Bläddra toohello */swagger/docs/v1* URL för ditt projekt och hello olika HTTP-svarskoder kommer att vara synliga i hello Swagger JSON.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-158">Browse toohello */swagger/docs/v1* URL of your project and hello various HTTP response codes will be visible in hello Swagger JSON.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

<span data-ttu-id="f1d3e-159">hello-koden för den här demonstrationen finns i [GitHub-lagringsplatsen](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes).</span><span class="sxs-lookup"><span data-stu-id="f1d3e-159">hello code for this demonstration can be found in [this GitHub repository](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes).</span></span> <span data-ttu-id="f1d3e-160">Tillsammans med hello dekorerad Web API-projekt med hello *SwaggerResponse* attributet är ett konsolprogram projekt som innehåller en genererad klient för detta API.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-160">Along with hello Web API project decorated with hello *SwaggerResponse* attribute is a Console Application project that contains a generated client for this API.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f1d3e-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1d3e-161">Next steps</span></span>
<span data-ttu-id="f1d3e-162">Den här artikeln visar hur toocustomize hello sätt Swashbuckle genererar åtgärden ID och giltig svarskoder.</span><span class="sxs-lookup"><span data-stu-id="f1d3e-162">This article has shown how toocustomize hello way Swashbuckle generates operation ids and valid response codes.</span></span> <span data-ttu-id="f1d3e-163">Mer information finns i [Swashbuckle på GitHub](https://github.com/domaindrivendev/Swashbuckle).</span><span class="sxs-lookup"><span data-stu-id="f1d3e-163">For more information, see [Swashbuckle on GitHub](https://github.com/domaindrivendev/Swashbuckle).</span></span>

