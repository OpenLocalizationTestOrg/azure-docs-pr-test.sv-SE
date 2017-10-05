---
title: Anpassa Swashbuckle-genererade API-definitioner
description: "Lär dig hur du anpassar Swagger API-definitioner som genereras av Swashbuckle för en API-app i Azure App Service."
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
ms.openlocfilehash: c83905a97fb2ee988fe06fc1f9a7379c1741fd02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="customize-swashbuckle-generated-api-definitions"></a>Anpassa Swashbuckle-genererade API-definitioner
## <a name="overview"></a>Översikt
Den här artikeln förklarar hur du anpassar Swashbuckle för att hantera vanliga scenarier där du kanske vill ändra standardbeteendet:

* Swashbuckle genererar dubbla åtgärden identifierare för överlagringar av kontrollantmetoder
* Swashbuckle förutsätter att endast giltigt svar från en metod HTTP 200 (OK) 

## <a name="customize-operation-identifier-generation"></a>Anpassa generera identifierare
Swashbuckle genererar Swagger åtgärden identifierare genom att sammanbinda Kontrollnamn och metodnamn. Det här mönstret skapar ett problem när du har flera överlagringar av en metod: Swashbuckle genererar duplicerade åtgärds-ID, vilket är ogiltigt Swagger JSON.

Till exempel gör följande kod i styrenheten Swashbuckle generera tre Contact_Get åtgärden ID: n.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Du kan lösa problemet manuellt genom att ge metoderna unika namn, till exempel följande för det här exemplet:

* Hämta
* GetById
* GetPage

Alternativet är att utöka Swashbuckle så att den automatiskt generera unika åtgärds-ID: n.

Följande steg visar hur du anpassar Swashbuckle med hjälp av den *SwaggerConfig.cs* -fil som ingår i projektet genom projektmall för Visual Studio API Apps Preview.  Du kan också anpassa Swashbuckle i ett Web API-projekt som du konfigurerar för distribution som en API-app.

1. Skapa en anpassad `IOperationFilter` implementering 
   
    Den `IOperationFilter` gränssnitt tillhandahåller en utökningspunkt för Swashbuckle-användare som vill anpassa olika aspekter av processen för Swagger-metadata. Följande kod visar en metod för att ändra beteende för generations-id-åtgärden. Koden lägger till parameternamn id åtgärdsnamn.  
   
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
2. I *App_Start\SwaggerConfig.cs* fil, anropa den `OperationFilter` metod för att orsaka Swashbuckle att använda den nya `IOperationFilter` implementering.
   
        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)
   
    Den *SwaggerConfig.cs* filen som har tagits bort av Swashbuckle NuGet-paketet innehåller många kommenterats ut på punkter. Ytterligare kommentarer visas inte här. 
   
    När du har gjort den här ändringen din `IOperationFilter` implementering används och orsakar unika åtgärds-ID: n som ska genereras.
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>

## <a name="allow-response-codes-other-than-200"></a>Tillåt svarskoder än 200
Som standard Swashbuckle förutsätter att ett svar HTTP 200 (OK) är den *endast* berättigade svar från en webb-API-metod. I vissa fall kanske du vill returnera andra svarskoder utan att orsaka att klienten kan generera ett undantag.  Följande Web API-kod visar till exempel ett scenario där du vill att klienten kan acceptera en 200 eller ett 404 som giltigt svar.

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

I det här scenariot anger Swagger som Swashbuckle genererar som standard bara en giltig HTTP-statuskod, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Eftersom Visual Studio använder Swagger API-definitionen för att generera kod för klienten, skapar klientkod som genererar ett undantag för eventuella svar än en HTTP-200. Koden nedan är från en C#-klient som genererats för det här exemplet Web API-metoden.

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

Swashbuckle tillhandahåller två sätt att anpassa listan med förväntade HTTP-svarskoder som den skapar med hjälp av XML-kommentarer eller `SwaggerResponse` attribut. Attributet är enklare, men det är endast tillgänglig i Swashbuckle 5.1.5 eller senare. Mallen API Apps preview nytt projekt i Visual Studio 2013 innehåller Swashbuckle version 5.0.0, så om du använde mallen och inte vill uppdatera Swashbuckle är ditt enda alternativ att använda XML-kommentarer. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Anpassa förväntade svarskoder med XML-kommentarer
Använd den här metoden för att ange svarskoder om Swashbuckle-versionen är tidigare än 5.1.5.

1. Lägg först till XML-dokumentationskommentarer över de metoder som du vill ange HTTP-svarskoder för. Ta prov Web API skulle åtgärden ovan och tillämpa XML-dokumentationen på den leda till kod som i följande exempel. 
   
        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
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
2. Lägg till anvisningarna i den *SwaggerConfig.cs* fil att dirigera Swashbuckle för att använda XML-dokumentationsfil.
   
   * Öppna *SwaggerConfig.cs* och skapa en metod i den *SwaggerConfig* klassen för att ange sökvägen till XML-dokumentationsfil. 
     
           private static string GetXmlCommentsPath()
           {
               return string.Format(@"{0}\XmlComments.xml", 
                   System.AppDomain.CurrentDomain.BaseDirectory);
           }
   * Rulla nedåt i den *SwaggerConfig.cs* tills du ser den kommenterats ut kodraden i skärmbilden nedan. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
   * Ta bort kommentarerna linje för att aktivera XML-kommentarer bearbetning under generering av Swagger. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
3. Gå till projektets egenskaper och aktivera XML-dokumentationsfil som visas i skärmbilden nedan för att generera XML-dokumentationsfil. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

När du utför dessa steg visar Swagger JSON som genererats av Swashbuckle http-svarskoder som har angetts i XML-kommentarer. Skärmbilden nedan visar den här nya JSON-nyttolast. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

När du använder Visual Studio för att återskapa klientkoden för REST-API, accepterar både HTTP OK och gick inte att hitta statuskoder utan att ett undantag, så att konsumerande koden för att fatta beslut om hur du hanterar returnera null kontakta poster C#-kod. 

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

Koden för den här demonstrationen finns i [GitHub-lagringsplatsen](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Tillsammans med webb-API är-projekt har definierats med XML-dokumentationskommentarer ett konsolprogram projekt som innehåller en genererad klient för detta API. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Anpassa förväntade svarskoder med attributet SwaggerResponse
Den [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribut som är tillgängliga i Swashbuckle 5.1.5 och senare. Om du har en tidigare version i ditt projekt, startar det här avsnittet förklarar hur du uppdaterar Swashbuckle NuGet-paketet så att du kan använda det här attributet.

1. I **Solution Explorer**, högerklicka på ditt webb-API-projekt och klickar på **hantera NuGet-paket**. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)
2. Klicka på den *uppdatering* knappen bredvid den *Swashbuckle* NuGet-paketet. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)
3. Lägg till den *SwaggerResponse* attribut till webb-API åtgärdsmetoder som du vill ange en giltig HTTP-svarskoder. 
   
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
4. Lägg till en `using` -instruktion för attributets namnområde:
   
        using Swashbuckle.Swagger.Annotations;
5. Bläddra till den */swagger/docs/v1* URL för ditt projekt och olika HTTP-svarskoder kommer att vara synliga i Swagger JSON. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Koden för den här demonstrationen finns i [GitHub-lagringsplatsen](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Tillsammans med Web API-projekt dekorerad med den *SwaggerResponse* attributet är ett konsolprogram projekt som innehåller en genererad klient för detta API. 

## <a name="next-steps"></a>Nästa steg
Den här artikeln visar hur du anpassar hur Swashbuckle genererar åtgärden-ID: n och giltig svarskoder. Mer information finns i [Swashbuckle på GitHub](https://github.com/domaindrivendev/Swashbuckle).

