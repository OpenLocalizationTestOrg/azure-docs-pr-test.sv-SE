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
# <a name="customize-swashbuckle-generated-api-definitions"></a>Anpassa Swashbuckle-genererade API-definitioner
## <a name="overview"></a>Översikt
Den här artikeln förklarar hur toocustomize Swashbuckle toohandle vanliga scenarier där du kanske vill tooalter hello standardbeteendet:

* Swashbuckle genererar dubbla åtgärden identifierare för överlagringar av kontrollantmetoder
* Swashbuckle förutsätter att hello endast giltigt svar från en metod är HTTP 200 (OK) 

## <a name="customize-operation-identifier-generation"></a>Anpassa generera identifierare
Swashbuckle genererar Swagger åtgärden identifierare genom att sammanbinda Kontrollnamn och metodnamn. Det här mönstret skapar ett problem när du har flera överlagringar av en metod: Swashbuckle genererar duplicerade åtgärds-ID, vilket är ogiltigt Swagger JSON.

Till exempel gör hello följande kontrollantkoden Swashbuckle toogenerate tre Contact_Get åtgärden ID: n.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Du kan lösa problemet hello manuellt genom att ge hello metoder unika namn, till exempel hello följande för det här exemplet:

* Hämta
* GetById
* GetPage

hello alternativ är tooextend Swashbuckle toomake den automatiskt generera unika åtgärds-ID: n.

hello följande steg visar hur toocustomize Swashbuckle med hjälp av hello *SwaggerConfig.cs* fil som ingår i hello projekt som hello projektmall för Visual Studio API Apps Preview.  Du kan också anpassa Swashbuckle i ett Web API-projekt som du konfigurerar för distribution som en API-app.

1. Skapa en anpassad `IOperationFilter` implementering 
   
    Hej `IOperationFilter` gränssnittet tillhandahåller en utökningsbarhet för Swashbuckle-användare som vill toocustomize olika aspekter av processen för hello Swagger-metadata. hello visar följande kod en metod för att ändra beteende för hello generations-id-åtgärden. hello koden lägger till parametern toohello åtgärden id namn.  
   
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
2. I *App_Start\SwaggerConfig.cs* fil, anrop hello `OperationFilter` metoden toocause Swashbuckle toouse hello nya `IOperationFilter` implementering.
   
        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)
   
    Hej *SwaggerConfig.cs* filen som har tagits bort av hello Swashbuckle NuGet-paketet innehåller många kommenterats ut på punkter. hello ytterligare kommentarer visas inte här. 
   
    När du har gjort den här ändringen din `IOperationFilter` implementering används och orsakar unika åtgärds-ID: n toobe genereras.
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>

## <a name="allow-response-codes-other-than-200"></a>Tillåt svarskoder än 200
Som standard Swashbuckle förutsätter att en HTTP-200 (OK) svar hello *endast* berättigade svar från en webb-API-metod. I vissa fall kan kanske du vill tooreturn andra svarskoder utan att orsaka hello klienten tooraise ett undantag.  Till exempel visar hello följande Web API-kod ett scenario där du vill ha hello klienten tooaccept en 200 eller ett 404 som giltigt svar.

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

I det här scenariot anger hello Swagger Swashbuckle genererar som standard bara en giltig HTTP-statuskod, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Eftersom Visual Studio använder hello Swagger API-definition toogenerate koden för hello klienten, skapar klientkod som genererar ett undantag för eventuella svar än en HTTP-200. hello koden nedan är från en C#-klient som genererats för det här exemplet Web API-metoden.

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

Swashbuckle tillhandahåller två sätt att anpassa hello lista över förväntade HTTP-svarskoder som den skapar med hjälp av XML-kommentarer eller hello `SwaggerResponse` attribut. hello-attributet är enklare, men det är endast tillgänglig i Swashbuckle 5.1.5 eller senare. hello mall för API Apps preview nytt projekt i Visual Studio 2013 innehåller Swashbuckle version 5.0.0, så om du använde hello mall och inte vill tooupdate Swashbuckle är ditt enda alternativ toouse XML-kommentarer. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Anpassa förväntade svarskoder med XML-kommentarer
Använd den här metoden toospecify svarskoder om Swashbuckle-versionen är tidigare än 5.1.5.

1. Lägg först till XML-dokumentationskommentarer över hello-metoder som du vill toospecify HTTP svarskoder för. Åtgärda hello exempel Web API visas ovan och använder hello XML-dokumentation tooit skulle leda till kod som hello följande exempel. 
   
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
2. Lägga till instruktioner i hello *SwaggerConfig.cs* filen toodirect Swashbuckle toomake användning av hello XML-dokumentationsfil.
   
   * Öppna *SwaggerConfig.cs* och skapa en metod på hello *SwaggerConfig* klassen toospecify hello sökvägen toohello XML-dokumentationsfil. 
     
           private static string GetXmlCommentsPath()
           {
               return string.Format(@"{0}\XmlComments.xml", 
                   System.AppDomain.CurrentDomain.BaseDirectory);
           }
   * Rulla nedåt i hello *SwaggerConfig.cs* tills du ser hello kommenterats ut kodrad i hello skärmbilden nedan. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
   * Ta bort kommentarerna hello rad tooenable hello XML-kommentarer bearbetning under generering av Swagger. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
3. Gå till hello projektegenskaperna i ordning toogenerate hello XML-dokumentationsfil, och aktivera hello XML-dokumentationsfil enligt hello skärmbilden nedan. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

När du utför dessa steg visar hello Swagger JSON som genererats av Swashbuckle hello HTTP svarskoder som du angav i hello XML-kommentarer. hello skärmbilden nedan visar den här nya JSON-nyttolast. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

När du använder Visual Studio tooregenerate hello klientkod för REST-API accepterar hello C#-kod både hello HTTP OK och gick inte att hitta statuskoder utan att ett undantag, så att dina konsumerande kod toomake beslut på hur toohandle hello tillbaka en null Kontakta post. 

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

hello-koden för den här demonstrationen finns i [GitHub-lagringsplatsen](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Tillsammans med hello Web API är-projekt har definierats med XML-dokumentationskommentarer ett konsolprogram projekt som innehåller en genererad klient för detta API. 

### <a name="customize-expected-response-codes-using-hello-swaggerresponse-attribute"></a>Anpassa förväntade svarskoder med hello SwaggerResponse attribut
Hej [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribut som är tillgängliga i Swashbuckle 5.1.5 och senare. Om du har en tidigare version i ditt projekt, startar det här avsnittet förklarar hur tooupdate hello Swashbuckle NuGet-paketet så att du kan använda det här attributet.

1. I **Solution Explorer**, högerklicka på ditt webb-API-projekt och klickar på **hantera NuGet-paket**. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)
2. Klicka på hello *uppdatering* knappen Nästa toohello *Swashbuckle* NuGet-paketet. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)
3. Lägg till hello *SwaggerResponse* attribut toohello Web API åtgärdsmetoder som du vill toospecify giltig HTTP-svarskoder. 
   
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
4. Lägg till en `using` -instruktion för hello attributets namnområde:
   
        using Swashbuckle.Swagger.Annotations;
5. Bläddra toohello */swagger/docs/v1* URL för ditt projekt och hello olika HTTP-svarskoder kommer att vara synliga i hello Swagger JSON. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

hello-koden för den här demonstrationen finns i [GitHub-lagringsplatsen](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Tillsammans med hello dekorerad Web API-projekt med hello *SwaggerResponse* attributet är ett konsolprogram projekt som innehåller en genererad klient för detta API. 

## <a name="next-steps"></a>Nästa steg
Den här artikeln visar hur toocustomize hello sätt Swashbuckle genererar åtgärden ID och giltig svarskoder. Mer information finns i [Swashbuckle på GitHub](https://github.com/domaindrivendev/Swashbuckle).

