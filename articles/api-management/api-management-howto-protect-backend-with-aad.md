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
# <a name="how-tooprotect-a-web-api-backend-with-azure-active-directory-and-api-management"></a><span data-ttu-id="7d61c-103">Hur tooprotect en webb-API-serverdel med Azure Active Directory och API-hantering</span><span class="sxs-lookup"><span data-stu-id="7d61c-103">How tooprotect a Web API backend with Azure Active Directory and API Management</span></span>
<span data-ttu-id="7d61c-104">Hej följande videoklipp visar hur toobuild en webb-API-serverdel och skydda den med hjälp av OAuth 2.0-protokollet med Azure Active Directory och API-hantering.</span><span class="sxs-lookup"><span data-stu-id="7d61c-104">hello following video shows how toobuild a Web API backend and protect it using OAuth 2.0 protocol with Azure Active Directory and API Management.</span></span>  <span data-ttu-id="7d61c-105">Den här artikeln innehåller en översikt och ytterligare information för hello stegen i hello videon.</span><span class="sxs-lookup"><span data-stu-id="7d61c-105">This article provides an overview and additional information for hello steps in hello video.</span></span> <span data-ttu-id="7d61c-106">Den här 24 minuter långa videon visar hur till:</span><span class="sxs-lookup"><span data-stu-id="7d61c-106">This 24 minute video shows you how to:</span></span>

* <span data-ttu-id="7d61c-107">Skapa en webb-API-serverdel och skydda den med AAD - börjar vid 1:30</span><span class="sxs-lookup"><span data-stu-id="7d61c-107">Build a Web API backend and secure it with AAD - starting at 1:30</span></span>
* <span data-ttu-id="7d61c-108">Importera hello API i API Management - inleder 7:10</span><span class="sxs-lookup"><span data-stu-id="7d61c-108">Import hello API into API Management - starting at 7:10</span></span>
* <span data-ttu-id="7d61c-109">Konfigurera hello Developer portal toocall hello API - början på 9:09</span><span class="sxs-lookup"><span data-stu-id="7d61c-109">Configure hello Developer portal toocall hello API - starting at 9:09</span></span>
* <span data-ttu-id="7d61c-110">Konfigurera ett skrivbordsprogram toocall hello API - börjar vid 18:08</span><span class="sxs-lookup"><span data-stu-id="7d61c-110">Configure a desktop application toocall hello API - starting at 18:08</span></span>
* <span data-ttu-id="7d61c-111">Konfigurera en JWT validering princip toopre-godkänna begäranden - börjar vid 20:47</span><span class="sxs-lookup"><span data-stu-id="7d61c-111">Configure a JWT validation policy toopre-authorize requests - starting at 20:47</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a><span data-ttu-id="7d61c-112">Skapa en Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="7d61c-112">Create an Azure AD directory</span></span>
<span data-ttu-id="7d61c-113">toosecure Web API säkerhetskopieras med Azure Active Directory måste du först ha en AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="7d61c-113">toosecure your Web API backed using Azure Active Directory you must first have a an AAD tenant.</span></span> <span data-ttu-id="7d61c-114">I det här videoklippet en klient med namnet **APIMDemo** används.</span><span class="sxs-lookup"><span data-stu-id="7d61c-114">In this video a tenant named **APIMDemo** is used.</span></span> <span data-ttu-id="7d61c-115">toocreate AAD-klient loggar in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) och på **ny**->**Apptjänster**->**Active Directory**->**Directory**->**skapa anpassade**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-115">toocreate an AAD tenant, sign-in toohello [Azure Classic Portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Active Directory**->**Directory**->**Custom Create**.</span></span> 

![Azure Active Directory][api-management-create-aad-menu]

<span data-ttu-id="7d61c-117">I det här exemplet en katalog med namnet **APIMDemo** skapas med en standarddomän med namnet **DemoAPIM.onmicrosoft.com**. Den här katalogen används i hela hello video.</span><span class="sxs-lookup"><span data-stu-id="7d61c-117">In this example a directory named **APIMDemo** is created with a default domain named **DemoAPIM.onmicrosoft.com**. This directory is used throughout hello video.</span></span>

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a><span data-ttu-id="7d61c-119">Skapa en tjänst för webb-API som skyddas av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d61c-119">Create a Web API service secured by Azure Active Directory</span></span>
<span data-ttu-id="7d61c-120">I det här steget skapas en webb-API-serverdel med hjälp av Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7d61c-120">In this step, a Web API backend is created using Visual Studio 2013.</span></span> <span data-ttu-id="7d61c-121">Det här steget i hello video börjar vid 1:30.</span><span class="sxs-lookup"><span data-stu-id="7d61c-121">This step of hello video starts at 1:30.</span></span> <span data-ttu-id="7d61c-122">toocreate Web API backend-projekt i Visual Studio klickar du på **filen**->**ny**->**projekt**, och välj **ASP.NET-webbprogram Programmet** från hello **Web** lista över mallar.</span><span class="sxs-lookup"><span data-stu-id="7d61c-122">toocreate Web API backend project in Visual Studio click **File**->**New**->**Project**, and choose **ASP.NET Web Application** from hello **Web** templates list.</span></span> <span data-ttu-id="7d61c-123">I den här videon hello projektet med namnet **APIMAADDemo**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-123">In this video hello project is named **APIMAADDemo**.</span></span> <span data-ttu-id="7d61c-124">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="7d61c-124">Click **OK** toocreate hello project.</span></span> 

![Visual Studio][api-management-new-web-app]

<span data-ttu-id="7d61c-126">Klicka på **Web API** från hello **markerar du en lista över** toocreate Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="7d61c-126">Click **Web API** from hello **Select a template list** toocreate a Web API project.</span></span> <span data-ttu-id="7d61c-127">Klicka på tooconfigure Azure Directory Authentication **ändra autentisering**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-127">tooconfigure Azure Directory Authentication click **Change Authentication**.</span></span>

![Nytt projekt][api-management-new-project]

<span data-ttu-id="7d61c-129">Klicka på **Organisationskonton**, och ange hello **domän** för din AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="7d61c-129">Click **Organizational Accounts**, and specify hello **Domain** of your AAD tenant.</span></span> <span data-ttu-id="7d61c-130">I det här exemplet hello domänen är **DemoAPIM.onmicrosoft.com**. hello domän i din katalog kan hämtas från hello **domäner** för din katalog.</span><span class="sxs-lookup"><span data-stu-id="7d61c-130">In this example hello domain is **DemoAPIM.onmicrosoft.com**. hello domain of your directory can be obtained from hello **Domains** tab of your directory.</span></span>

![Domäner][api-management-aad-domains]

<span data-ttu-id="7d61c-132">Konfigurera inställningar för hello önskad i hello **ändra autentisering** dialogrutan och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-132">Configure hello desired settings in hello **Change Authentication** dialog box and click **OK**.</span></span>

![Ändra autentisering][api-management-change-authentication]

<span data-ttu-id="7d61c-134">När du klickar på **OK** Visual Studio försöker tooregister ditt program med Azure AD-katalogen och du kan ange toosign i av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d61c-134">When you click **OK** Visual Studio will attempt tooregister your application with your Azure AD directory and you may be prompted toosign in by Visual Studio.</span></span> <span data-ttu-id="7d61c-135">Logga in med ett administratörskonto för din katalog.</span><span class="sxs-lookup"><span data-stu-id="7d61c-135">Sign in using an administrative account for your directory.</span></span>

![Logga in tooVisual Studio][api-management-sign-in-vidual-studio]

<span data-ttu-id="7d61c-137">tooconfigure projektet som en Azure Web API Kontrollera hello rutan för **värd i molnet hello** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-137">tooconfigure this project as an Azure Web API check hello box for **Host in hello cloud** and then click **OK**.</span></span>

![Nytt projekt][api-management-new-project-cloud]

<span data-ttu-id="7d61c-139">Du tillfrågas toosign i tooAzure och du kan sedan konfigurera hello Web App.</span><span class="sxs-lookup"><span data-stu-id="7d61c-139">You may be prompted toosign in tooAzure, and then you can configure hello Web App.</span></span>

![Konfigurera][api-management-configure-web-app]

<span data-ttu-id="7d61c-141">I det här exemplet en ny **programtjänstplanen** med namnet **APIMAADDemo** har angetts.</span><span class="sxs-lookup"><span data-stu-id="7d61c-141">In this example a new **App Service plan** named **APIMAADDemo** is specified.</span></span>

<span data-ttu-id="7d61c-142">Klicka på **OK** tooconfigure hello Web App och skapa hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="7d61c-142">Click **OK** tooconfigure hello Web App and create hello project.</span></span>

## <a name="add-hello-code-toohello-web-api-project"></a><span data-ttu-id="7d61c-143">Lägg till hello kod toohello Web API-projekt</span><span class="sxs-lookup"><span data-stu-id="7d61c-143">Add hello code toohello Web API project</span></span>
<span data-ttu-id="7d61c-144">hello nästa steg i hello video lägger till hello kod toohello Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="7d61c-144">hello next step in hello video adds hello code toohello Web API project.</span></span> <span data-ttu-id="7d61c-145">Det här steget startar vid 4:35.</span><span class="sxs-lookup"><span data-stu-id="7d61c-145">This step starts at 4:35.</span></span>

<span data-ttu-id="7d61c-146">i det här exemplet Hello Web API implementerar en grundläggande Kalkylatorn tjänst med hjälp av en modell och en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="7d61c-146">hello Web API in this example implements a basic calculator service using a model and a controller.</span></span> <span data-ttu-id="7d61c-147">tooadd hello modellen för hello tjänst, högerklicka på **modeller** i **Solution Explorer** och välj **Lägg till**, **klassen**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-147">tooadd hello model for hello service, right-click **Models** in **Solution Explorer** and choose **Add**, **Class**.</span></span> <span data-ttu-id="7d61c-148">Namnge hello klassen `CalcInput` och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-148">Name hello class `CalcInput` and click **Add**.</span></span>

<span data-ttu-id="7d61c-149">Lägg till följande hello `using` instruktionen toohello överkant hello `CalcInput.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="7d61c-149">Add hello following `using` statement toohello top of hello `CalcInput.cs` file.</span></span>

```c#
using Newtonsoft.Json;
```

<span data-ttu-id="7d61c-150">Ersätt hello genereras klassen med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="7d61c-150">Replace hello generated class with hello following code.</span></span>

```c#
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

<span data-ttu-id="7d61c-151">Högerklicka på **domänkontrollanter** i **Solution Explorer** och välj **Lägg till**->**domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-151">Right-click **Controllers** in **Solution Explorer** and choose **Add**->**Controller**.</span></span> <span data-ttu-id="7d61c-152">Välj **Web API 2 styrenhet – tom** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-152">Choose **Web API 2 Controller - Empty** and click **Add**.</span></span> <span data-ttu-id="7d61c-153">Typen **CalcController** hello styrenhet namnet och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-153">Type **CalcController** for hello Controller name and click **Add**.</span></span>

![Lägg till styrenhet][api-management-add-controller]

<span data-ttu-id="7d61c-155">Lägg till följande hello `using` instruktionen toohello överkant hello `CalcController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="7d61c-155">Add hello following `using` statement toohello top of hello `CalcController.cs` file.</span></span>

```c#
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

<span data-ttu-id="7d61c-156">Ersätt hello genereras controller-klassen med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="7d61c-156">Replace hello generated controller class with hello following code.</span></span> <span data-ttu-id="7d61c-157">Den här koden implementerar hello `Add`, `Subtract`, `Multiply`, och `Divide` drift av hello grundläggande Kalkylatorn API.</span><span class="sxs-lookup"><span data-stu-id="7d61c-157">This code implements hello `Add`, `Subtract`, `Multiply`, and `Divide` operations of hello Basic Calculator API.</span></span>

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

<span data-ttu-id="7d61c-158">Tryck på **F6** toobuild och kontrollera hello lösning.</span><span class="sxs-lookup"><span data-stu-id="7d61c-158">Press **F6** toobuild and verify hello solution.</span></span>

## <a name="publish-hello-project-tooazure"></a><span data-ttu-id="7d61c-159">Publicera hello projekt tooAzure</span><span class="sxs-lookup"><span data-stu-id="7d61c-159">Publish hello project tooAzure</span></span>
<span data-ttu-id="7d61c-160">I det här steget hello Visual Studio är-projekt publicerade tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7d61c-160">In this step hello Visual Studio project is published tooAzure.</span></span> <span data-ttu-id="7d61c-161">Det här steget i hello video startar vid 5:45.</span><span class="sxs-lookup"><span data-stu-id="7d61c-161">This step of hello video starts at 5:45.</span></span>

<span data-ttu-id="7d61c-162">toopublish Hej projektet tooAzure, högerklicka på hello **APIMAADDemo** projektet i Visual Studio och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-162">toopublish hello project tooAzure, right-click hello **APIMAADDemo** project in Visual Studio and choose **Publish**.</span></span> <span data-ttu-id="7d61c-163">Tänk hello hello standardinställningarna **Publicera webbplats** dialogrutan och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-163">Keep hello default settings in hello **Publish Web** dialog box and click **Publish**.</span></span>

![Webbpublicering][api-management-web-publish]

## <a name="grant-permissions-toohello-azure-ad-backend-service-application"></a><span data-ttu-id="7d61c-165">Bevilja behörighet toohello Azure AD backend-tjänstprogram</span><span class="sxs-lookup"><span data-stu-id="7d61c-165">Grant permissions toohello Azure AD backend service application</span></span>
<span data-ttu-id="7d61c-166">Ett nytt program för hello backend-tjänsten har skapats i Azure AD-katalogen som en del av hello konfigurationen och publiceringsprocessen i ditt webb-API-projekt.</span><span class="sxs-lookup"><span data-stu-id="7d61c-166">A new application for hello backend service is created in your Azure AD directory as part of hello configuring and publishing process of your Web API project.</span></span> <span data-ttu-id="7d61c-167">I det här steget av hello video behörigheter början på 6:13 toohello Web API-serverdel.</span><span class="sxs-lookup"><span data-stu-id="7d61c-167">In this step of hello video, starting at 6:13, permissions are granted toohello Web API backend.</span></span>

![Program][api-management-aad-backend-app]

<span data-ttu-id="7d61c-169">Klicka på hello programbehörigheter tooconfigure hello krävs hello namn.</span><span class="sxs-lookup"><span data-stu-id="7d61c-169">Click hello name of hello application tooconfigure hello required permissions.</span></span> <span data-ttu-id="7d61c-170">Navigera toohello **konfigurera** och rulla ned toohello **behörigheter tooother program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7d61c-170">Navigate toohello **Configure** tab and scroll down toohello **permissions tooother applications** section.</span></span> <span data-ttu-id="7d61c-171">Klicka på hello **programbehörigheter** listrutan bredvid **Windows** **Azure Active Directory**, hello kryssrutan för **läsa katalogdata**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-171">Click hello **Application Permissions** drop-down beside **Windows** **Azure Active Directory**, check hello box for **Read directory data**, and click **Save**.</span></span>

![Lägg till behörigheter][api-management-aad-add-permissions]

> [!NOTE]
> <span data-ttu-id="7d61c-173">Om **Windows** **Azure Active Directory** är inte finns i listan under behörigheter tooother program klickar du på **lägga till program** och lägga till den hello-listan.</span><span class="sxs-lookup"><span data-stu-id="7d61c-173">If **Windows** **Azure Active Directory** is not listed under permissions tooother applications, click **Add application** and add it from hello list.</span></span>
> 
> 

<span data-ttu-id="7d61c-174">Anteckna hello **App-Id URI** för användning i ett senare skede när ett Azure AD-program är konfigurerat för hello API Management developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-174">Make a note of hello **App Id URI** for use in a subsequent step when an Azure AD application is configured for hello API Management developer portal.</span></span>

![URI för App-Id][api-management-aad-sso-uri]

## <a name="import-hello-web-api-into-api-management"></a><span data-ttu-id="7d61c-176">Importera hello Web API till API-hantering</span><span class="sxs-lookup"><span data-stu-id="7d61c-176">Import hello Web API into API Management</span></span>
<span data-ttu-id="7d61c-177">API: er konfigureras från hello API publisher-portalen som öppnas via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-177">APIs are configured from hello API publisher portal, which is accessed through hello Azure Portal.</span></span> <span data-ttu-id="7d61c-178">tooreach, klickar du på **Publisher portal** hello verktygsfältet för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7d61c-178">tooreach it, click **Publisher portal** from hello toolbar of your API Management service.</span></span> <span data-ttu-id="7d61c-179">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [hantera din första API] [ Manage your first API] kursen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-179">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Manage your first API][Manage your first API] tutorial.</span></span>

![Utgivarportalen][api-management-management-console]

<span data-ttu-id="7d61c-181">Åtgärder kan vara [till tooAPIs manuellt](api-management-howto-add-operations.md), eller kan importeras.</span><span class="sxs-lookup"><span data-stu-id="7d61c-181">Operations can be [added tooAPIs manually](api-management-howto-add-operations.md), or they can be imported.</span></span> <span data-ttu-id="7d61c-182">I det här videoklippet importeras åtgärder i Swagger-format som börjar på 6:40.</span><span class="sxs-lookup"><span data-stu-id="7d61c-182">In this video, operations are imported in Swagger format starting at 6:40.</span></span>

<span data-ttu-id="7d61c-183">Skapa en fil med namnet `calcapi.json` med följande innehåll och spara den tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="7d61c-183">Create a file named `calcapi.json` with following contents and save it tooyour computer.</span></span> <span data-ttu-id="7d61c-184">Se till att hello `host` -attributet pekar tooyour Web API-serverdel.</span><span class="sxs-lookup"><span data-stu-id="7d61c-184">Ensure that hello `host` attribute points tooyour Web API backend.</span></span> <span data-ttu-id="7d61c-185">I det här exemplet `"host": "apimaaddemo.azurewebsites.net"` används.</span><span class="sxs-lookup"><span data-stu-id="7d61c-185">In this example `"host": "apimaaddemo.azurewebsites.net"` is used.</span></span>

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

<span data-ttu-id="7d61c-186">tooimport hello Kalkylatorn API, klickar du på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **Import API**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-186">tooimport hello calculator API, click **APIs** from hello **API Management** menu on hello left, and then click **Import API**.</span></span>

![Knappen Importera API][api-management-import-api]

<span data-ttu-id="7d61c-188">Utföra hello följande steg tooconfigure hello Kalkylatorn API.</span><span class="sxs-lookup"><span data-stu-id="7d61c-188">Perform hello following steps tooconfigure hello calculator API.</span></span>

1. <span data-ttu-id="7d61c-189">Klicka på **från filen**, bläddra toohello `calculator.json` fil som du sparade och klicka på hello **Swagger** knappen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-189">Click **From file**, browse toohello `calculator.json` file you saved, and click hello **Swagger** radio button.</span></span>
2. <span data-ttu-id="7d61c-190">Typen **beräkna** till hello **URL för Web API-suffix** textruta.</span><span class="sxs-lookup"><span data-stu-id="7d61c-190">Type **calc** into hello **Web API URL suffix** textbox.</span></span>
3. <span data-ttu-id="7d61c-191">Klicka i hello **produkter (valfritt)** och väljer **Starter**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-191">Click in hello **Products (optional)** box and choose **Starter**.</span></span>
4. <span data-ttu-id="7d61c-192">Klicka på **spara** tooimport hello API.</span><span class="sxs-lookup"><span data-stu-id="7d61c-192">Click **Save** tooimport hello API.</span></span>

![Lägga till ett nytt API][api-management-import-new-api]

<span data-ttu-id="7d61c-194">När du har importerat hello API visas hello sammanfattningssidan hello-API: t i hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-194">Once hello API is imported, hello summary page for hello API is displayed in hello publisher portal.</span></span>

## <a name="call-hello-api-unsuccessfully-from-hello-developer-portal"></a><span data-ttu-id="7d61c-195">Anropa API för hello fel från hello developer-portalen</span><span class="sxs-lookup"><span data-stu-id="7d61c-195">Call hello API unsuccessfully from hello developer portal</span></span>
<span data-ttu-id="7d61c-196">Nu hello API har importerats till API-hantering, men kan ännu inte anropas har från hello developer-portalen eftersom hello serverdelstjänst skyddas med Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7d61c-196">At this point, hello API has been imported into API Management, but cannot yet be called successfully from hello developer portal because hello backend service is protected with Azure AD authentication.</span></span> <span data-ttu-id="7d61c-197">Detta visas i hello videon inleder med hello följande steg 7:40.</span><span class="sxs-lookup"><span data-stu-id="7d61c-197">This is demonstrated in hello video starting at 7:40 using hello following steps.</span></span>

<span data-ttu-id="7d61c-198">Klicka på **utvecklarportalen** från hello längst upp till höger sida av hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-198">Click **Developer portal** from hello top-right side of hello publisher portal.</span></span>

![Utvecklarportalen][api-management-developer-portal-menu]

<span data-ttu-id="7d61c-200">Klicka på **API: er** och klicka på hello **Kalkylatorn** API.</span><span class="sxs-lookup"><span data-stu-id="7d61c-200">Click **APIs** and click hello **Calculator** API.</span></span>

![Utvecklarportalen][api-management-dev-portal-apis]

<span data-ttu-id="7d61c-202">Klicka på **prova**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-202">Click **Try it**.</span></span>

![Prova][api-management-dev-portal-try-it]

<span data-ttu-id="7d61c-204">Klicka på **skicka** och notera hello svarsstatusen av **401 obehörig**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-204">Click **Send** and note hello response status of **401 Unauthorized**.</span></span>

![Skicka][api-management-dev-portal-send-401]

<span data-ttu-id="7d61c-206">hello-begäran är inte behörig eftersom hello backend API skyddas av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7d61c-206">hello request is unauthorized because hello backend API is protected by Azure Active Directory.</span></span> <span data-ttu-id="7d61c-207">Innan du anropar API hello måste hello developer-portalen vara konfigurerade tooauthorize utvecklare som använder OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="7d61c-207">Before successfully calling hello API hello developer portal must be configured tooauthorize developers using OAuth 2.0.</span></span> <span data-ttu-id="7d61c-208">Den här processen beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="7d61c-208">This process is described in hello following sections.</span></span>

## <a name="register-hello-developer-portal-as-an-aad-application"></a><span data-ttu-id="7d61c-209">Registrera hello developer-portalen som ett AAD-program</span><span class="sxs-lookup"><span data-stu-id="7d61c-209">Register hello developer portal as an AAD application</span></span>
<span data-ttu-id="7d61c-210">hello första steget när du konfigurerar hello developer portal tooauthorize utvecklare som använder OAuth 2.0 är tooregister hello developer-portalen som ett AAD-program.</span><span class="sxs-lookup"><span data-stu-id="7d61c-210">hello first step in configuring hello developer portal tooauthorize developers using OAuth 2.0 is tooregister hello developer portal as an AAD application.</span></span> <span data-ttu-id="7d61c-211">Detta visas från 8:27 i hello videon.</span><span class="sxs-lookup"><span data-stu-id="7d61c-211">This is demonstrated starting at 8:27 in hello video.</span></span>

<span data-ttu-id="7d61c-212">Navigera toohello Azure AD-klient från hello första steget i den här videon i det här exemplet **APIMDemo** och navigera toohello **program** fliken.</span><span class="sxs-lookup"><span data-stu-id="7d61c-212">Navigate toohello Azure AD tenant from hello first step of this video, in this example **APIMDemo** and navigate toohello **Applications** tab.</span></span>

![nytt program][api-management-aad-new-application-devportal]

<span data-ttu-id="7d61c-214">Klicka på hello **Lägg till** knappen toocreate ett nytt program för Azure Active Directory och välj **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-214">Click hello **Add** button toocreate a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![nytt program][api-management-new-aad-application-menu]

<span data-ttu-id="7d61c-216">Välj **Web application och/eller webb-API**, ange ett namn och klicka på pilen för hello nästa.</span><span class="sxs-lookup"><span data-stu-id="7d61c-216">Choose **Web application and/or Web API**, enter a name, and click hello next arrow.</span></span> <span data-ttu-id="7d61c-217">I det här exemplet **APIMDeveloperPortal** används.</span><span class="sxs-lookup"><span data-stu-id="7d61c-217">In this example **APIMDeveloperPortal** is used.</span></span>

![nytt program][api-management-aad-new-application-devportal-1]

<span data-ttu-id="7d61c-219">För **inloggnings-URL** anger hello URL för API Management-tjänsten och Lägg till `/signin`.</span><span class="sxs-lookup"><span data-stu-id="7d61c-219">For **Sign-on URL** enter hello URL of your API Management service and append `/signin`.</span></span> <span data-ttu-id="7d61c-220">I det här exemplet `https://contoso5.portal.azure-api.net/signin` används.</span><span class="sxs-lookup"><span data-stu-id="7d61c-220">In this example `https://contoso5.portal.azure-api.net/signin` is used.</span></span>

<span data-ttu-id="7d61c-221">För **App-Id-URL** anger hello URL för API Management-tjänsten och lägga till vissa unika tecken.</span><span class="sxs-lookup"><span data-stu-id="7d61c-221">For **App Id URL** enter hello URL of your API Management service and append some unique characters.</span></span> <span data-ttu-id="7d61c-222">Det kan vara önskade tecken och i det här exemplet `https://contoso5.portal.azure-api.net/dp` används.</span><span class="sxs-lookup"><span data-stu-id="7d61c-222">These can be any desired characters and in this example `https://contoso5.portal.azure-api.net/dp` is used.</span></span> <span data-ttu-id="7d61c-223">När hello önskad **appegenskaper** är konfigurerad, klicka på hello markerat toocreate hello program.</span><span class="sxs-lookup"><span data-stu-id="7d61c-223">When hello  desired **App properties** are configured, click hello check mark toocreate hello application.</span></span>

![nytt program][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a><span data-ttu-id="7d61c-225">Konfigurera en server för API Management OAuth 2.0-auktorisering</span><span class="sxs-lookup"><span data-stu-id="7d61c-225">Configure an API Management OAuth 2.0 authorization server</span></span>
<span data-ttu-id="7d61c-226">hello nästa steg är tooconfigure en server med OAuth 2.0 auktorisering i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="7d61c-226">hello next step is tooconfigure an OAuth 2.0 authorization server in API Management.</span></span> <span data-ttu-id="7d61c-227">Det här steget visar hello video början på 9:43.</span><span class="sxs-lookup"><span data-stu-id="7d61c-227">This step is demonstrated in hello video starting at 9:43.</span></span>

<span data-ttu-id="7d61c-228">Klicka på **säkerhet** hello API Management-menyn hello vänster klickar du på **OAuth 2.0**, och klicka sedan på **lägga till tillståndet** server.</span><span class="sxs-lookup"><span data-stu-id="7d61c-228">Click **Security** from hello API Management menu on hello left, click **OAuth 2.0**, and then click **Add authorization** server.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server]

<span data-ttu-id="7d61c-230">Ange ett namn och en valfri beskrivning i hello **namn** och **beskrivning** fält.</span><span class="sxs-lookup"><span data-stu-id="7d61c-230">Enter a name and an optional description in hello **Name** and **Description** fields.</span></span> <span data-ttu-id="7d61c-231">Dessa fält är används tooidentify hello OAuth 2.0 auktorisering server inom hello API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="7d61c-231">These fields are used tooidentify hello OAuth 2.0 authorization server within hello API Management service instance.</span></span> <span data-ttu-id="7d61c-232">I det här exemplet **auktorisering server demo** används.</span><span class="sxs-lookup"><span data-stu-id="7d61c-232">In this example **Authorization server demo** is used.</span></span> <span data-ttu-id="7d61c-233">Senare när du anger en toobe för OAuth 2.0-server som används för autentisering för ett API som du väljer det här namnet.</span><span class="sxs-lookup"><span data-stu-id="7d61c-233">Later when you specify an OAuth 2.0 server toobe used for authentication for an API, you will select this name.</span></span>

<span data-ttu-id="7d61c-234">För hello **klienten URL för registrering** ange ett platshållarvärde som `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="7d61c-234">For hello **Client registration page URL** enter a placeholder value such as `http://localhost`.</span></span>  <span data-ttu-id="7d61c-235">Hej **klienten URL för registrering** toohello sidan som användarna kan använda toocreate och konfigurera sina egna konton för OAuth 2.0-leverantörer som stöder Användarhantering av konton.</span><span class="sxs-lookup"><span data-stu-id="7d61c-235">hello **Client registration page URL** points toohello page that users can use toocreate and configure their own accounts for OAuth 2.0 providers that support user management of accounts.</span></span> <span data-ttu-id="7d61c-236">I det här exemplet användare inte skapa och konfigurera sina egna konton, så att en platshållare används.</span><span class="sxs-lookup"><span data-stu-id="7d61c-236">In this example users do not create and configure their own accounts so a placeholder is used.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server-1]

<span data-ttu-id="7d61c-238">Ange sedan **slutpunkts-URL-auktorisering** och **Token slutpunkts-URL**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-238">Next, specify **Authorization endpoint URL** and **Token endpoint URL**.</span></span>

![auktorisering server][api-management-add-authorization-server-1a]

<span data-ttu-id="7d61c-240">Dessa värden kan hämtas från hello **App slutpunkter** sidan hello AAD-program som du skapade för hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-240">These values can be retrieved from hello **App Endpoints** page of hello AAD application you created for hello developer portal.</span></span> <span data-ttu-id="7d61c-241">tooaccess hello slutpunkter navigera toohello **konfigurera** för hello AAD-program och klicka på **visa slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-241">tooaccess hello endpoints navigate toohello **Configure** tab for hello AAD application and click **View endpoints**.</span></span>

![Program][api-management-aad-devportal-application]

![Visa slutpunkter][api-management-aad-view-endpoints]

<span data-ttu-id="7d61c-244">Kopiera hello **OAuth 2.0 autentiseringsslutpunkt** och klistra in den i hello **slutpunkts-URL-auktorisering** textruta.</span><span class="sxs-lookup"><span data-stu-id="7d61c-244">Copy hello **OAuth 2.0 authorization endpoint** and paste it into hello **Authorization endpoint URL** textbox.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server-2]

<span data-ttu-id="7d61c-246">Kopiera hello **OAuth 2.0-token för slutpunkt** och klistra in den i hello **Token slutpunkts-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="7d61c-246">Copy hello **OAuth 2.0 token endpoint** and paste it into hello **Token endpoint URL** textbox.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server-2a]

<span data-ttu-id="7d61c-248">Dessutom toopasting i hello tokenslutpunkten, lägga till en ytterligare brödtext parameter med namnet **resurs** och använda hello för hello värdet **App-Id URI** från hello AAD-program för hello serverdelstjänst som var skapas när hello Visual Studio-projekt har publicerats.</span><span class="sxs-lookup"><span data-stu-id="7d61c-248">In addition toopasting in hello token endpoint, add an additional body parameter named **resource** and for hello value use hello **App Id URI** from hello AAD application for hello backend service that was created when hello Visual Studio project was published.</span></span>

![URI för App-Id][api-management-aad-sso-uri]

<span data-ttu-id="7d61c-250">Ange sedan hello klientens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7d61c-250">Next, specify hello client credentials.</span></span> <span data-ttu-id="7d61c-251">Dessa är hello autentiseringsuppgifter för hello-resurs som du vill tooaccess, i det här fallet hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-251">These are hello credentials for hello resource you want tooaccess, in this case hello developer portal.</span></span>

![Klientens autentiseringsuppgifter][api-management-client-credentials]

<span data-ttu-id="7d61c-253">tooget hello **klient-Id**, navigera toohello **konfigurera** för hello AAD-program för hello developer-portalen och kopiera hello **klient-Id**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-253">tooget hello **Client Id**, navigate toohello **Configure** tab of hello AAD application for hello developer portal and copy hello **Client Id**.</span></span>

<span data-ttu-id="7d61c-254">tooget hello **Klienthemlighet** klickar du på hello **Markera varaktighet** listrutan i hello **nycklar** avsnittet och ange ett intervall.</span><span class="sxs-lookup"><span data-stu-id="7d61c-254">tooget hello **Client Secret** click hello **Select duration** drop-down in hello **Keys** section and specify an interval.</span></span> <span data-ttu-id="7d61c-255">I det här exemplet används 1 år.</span><span class="sxs-lookup"><span data-stu-id="7d61c-255">In this example 1 year is used.</span></span>

![Klient-ID][api-management-aad-client-id]

<span data-ttu-id="7d61c-257">Klicka på **spara** toosave hello konfiguration och visa hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="7d61c-257">Click **Save** toosave hello configuration and display hello key.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="7d61c-258">Anteckna den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="7d61c-258">Make a note of this key.</span></span> <span data-ttu-id="7d61c-259">När du stänger hello Azure Active Directory configuration kan inte hello nyckel visas igen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-259">Once you close hello Azure Active Directory configuration window, hello key cannot be displayed again.</span></span>
> 
> 

<span data-ttu-id="7d61c-260">Kopiera hello viktiga toohello Urklipp, växla tillbaka toohello publisher portal klistra in hello nyckel i hello **Klienthemlighet** textruta och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-260">Copy hello key toohello clipboard, switch back toohello publisher portal, paste hello key into hello **Client Secret** textbox, and click **Save**.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server-3]

<span data-ttu-id="7d61c-262">Direkt efter hello klientens autentiseringsuppgifter är en kod bevilja för auktorisering.</span><span class="sxs-lookup"><span data-stu-id="7d61c-262">Immediately following hello client credentials is an authorization code grant.</span></span> <span data-ttu-id="7d61c-263">Kopiera den här auktoriseringskod och växla tillbaka tooyour portalprogram för Azure AD-utvecklare Konfigurera sidan och klistra in hello authorization grant i hello **Reply URL** fältet och klickar på **spara** igen.</span><span class="sxs-lookup"><span data-stu-id="7d61c-263">Copy this authorization code and switch back tooyour Azure AD developer portal application configure page, and paste hello authorization grant into hello **Reply URL** field, and click **Save** again.</span></span>

![Reply-URL][api-management-aad-reply-url]

<span data-ttu-id="7d61c-265">hello nästa steg är tooconfigure hello behörigheter för hello developer-portalen AAD-program.</span><span class="sxs-lookup"><span data-stu-id="7d61c-265">hello next step is tooconfigure hello permissions for hello developer portal AAD application.</span></span> <span data-ttu-id="7d61c-266">Klicka på **programbehörigheter** och kryssrutan för hello **läsa katalogdata**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-266">Click **Application Permissions** and check hello box for **Read directory data**.</span></span> <span data-ttu-id="7d61c-267">Klicka på **spara** toosave detta ändra och klicka sedan på **lägga till program**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-267">Click **Save** toosave this change, and then click **Add application**.</span></span>

![Lägg till behörigheter][api-management-add-devportal-permissions]

<span data-ttu-id="7d61c-269">Klicka på sökikonen hello, typen **APIM** till hello från och med rutan, Välj **APIMAADDemo**, och klicka på hello markerat toosave.</span><span class="sxs-lookup"><span data-stu-id="7d61c-269">Click hello search icon, type **APIM** into hello Starting with box, select **APIMAADDemo**, and click hello check mark toosave.</span></span>

![Lägg till behörigheter][api-management-aad-add-app-permissions]

<span data-ttu-id="7d61c-271">Klicka på **delegerade behörigheter** för **APIMAADDemo** och kryssrutan för hello **åtkomst APIMAADDemo**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-271">Click **Delegated Permissions** for **APIMAADDemo** and check hello box for **Access APIMAADDemo**, and click **Save**.</span></span> <span data-ttu-id="7d61c-272">På så sätt kan utvecklare hello portalprogram tooaccess hello backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7d61c-272">This allows hello developer portal application tooaccess hello backend service.</span></span>

![Lägg till behörigheter][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-hello-calculator-api"></a><span data-ttu-id="7d61c-274">Aktivera OAuth 2.0 användarautentiseringen för hello Kalkylatorn API</span><span class="sxs-lookup"><span data-stu-id="7d61c-274">Enable OAuth 2.0 user authorization for hello Calculator API</span></span>
<span data-ttu-id="7d61c-275">Hello OAuth 2.0-server är konfigurerat, kan du ange den i hello säkerhetsinställningar för ditt API.</span><span class="sxs-lookup"><span data-stu-id="7d61c-275">Now that hello OAuth 2.0 server is configured, you can specify it in hello security settings for your API.</span></span> <span data-ttu-id="7d61c-276">Det här steget visar hello video från 14:30.</span><span class="sxs-lookup"><span data-stu-id="7d61c-276">This step is demonstrated in hello video starting at 14:30.</span></span>

<span data-ttu-id="7d61c-277">Klicka på **API: er** i hello vänstra menyn och klickar på **Kalkylatorn** tooview och konfigurera dess inställningar.</span><span class="sxs-lookup"><span data-stu-id="7d61c-277">Click **APIs** in hello left menu, and click  **Calculator** tooview and configure its settings.</span></span>

![Kalkylatorn API][api-management-calc-api]

<span data-ttu-id="7d61c-279">Navigera toohello **säkerhet** kontrollerar hello **OAuth 2.0** kryssrutan, Välj hello önskade auktorisering servern från hello **auktorisering server** listrutan och klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-279">Navigate toohello **Security** tab, check hello **OAuth 2.0** checkbox, select hello desired authorization server from hello **Authorization server** drop-down, and click **Save**.</span></span>

![Kalkylatorn API][api-management-enable-aad-calculator]

## <a name="successfully-call-hello-calculator-api-from-hello-developer-portal"></a><span data-ttu-id="7d61c-281">Anropa hello Kalkylatorn API från hello developer-portalen</span><span class="sxs-lookup"><span data-stu-id="7d61c-281">Successfully call hello Calculator API from hello developer portal</span></span>
<span data-ttu-id="7d61c-282">Hello OAuth 2.0-auktorisering är konfigurerat på hello API anropas åtgärderna har från hello developer center.</span><span class="sxs-lookup"><span data-stu-id="7d61c-282">Now that hello OAuth 2.0 authorization is configured on hello API, its operations can be successfully called from hello developer center.</span></span> <span data-ttu-id="7d61c-283">Det här steget visar hello video början på 15:00.</span><span class="sxs-lookup"><span data-stu-id="7d61c-283">THis step is demonstrated in hello video starting at 15:00.</span></span>

<span data-ttu-id="7d61c-284">Gå tillbaka toohello **lägga till två heltal** driften av hello Kalkylatorn service i hello developer-portalen och klicka på **prova**.</span><span class="sxs-lookup"><span data-stu-id="7d61c-284">Navigate back toohello **Add two integers** operation of hello calculator service in hello developer portal and click **Try it**.</span></span> <span data-ttu-id="7d61c-285">Obs hello nytt objekt i hello **auktorisering** avsnittet motsvarande toohello auktorisering server som du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="7d61c-285">Note hello new item in hello **Authorization** section corresponding toohello authorization server you just added.</span></span>

![Kalkylatorn API][api-management-calc-authorization-server]

<span data-ttu-id="7d61c-287">Välj **Auktoriseringskoden** från hello auktorisering nedrullningsbara listan och ange hello autentiseringsuppgifter för hello konto toouse.</span><span class="sxs-lookup"><span data-stu-id="7d61c-287">Select **Authorization code** from hello authorization drop-down list and enter hello credentials of hello account toouse.</span></span> <span data-ttu-id="7d61c-288">Om du redan har loggat in med hello-konto kan du inte ombedd.</span><span class="sxs-lookup"><span data-stu-id="7d61c-288">If you are already signed in with hello account you may not be prompted.</span></span>

![Kalkylatorn API][api-management-devportal-authorization-code]

<span data-ttu-id="7d61c-290">Klicka på **skicka** och Observera hello **svarsstatusen** av **200 OK** och hello resultat av hello-åtgärden i hello svar innehåll.</span><span class="sxs-lookup"><span data-stu-id="7d61c-290">Click **Send** and note hello **Response status** of **200 OK** and hello results of hello operation in hello response content.</span></span>

![Kalkylatorn API][api-management-devportal-response]

## <a name="configure-a-desktop-application-toocall-hello-api"></a><span data-ttu-id="7d61c-292">Konfigurera ett skrivbordsprogram toocall hello API</span><span class="sxs-lookup"><span data-stu-id="7d61c-292">Configure a desktop application toocall hello API</span></span>
<span data-ttu-id="7d61c-293">hello nästa procedur i hello video börjar vid 16:30 och konfigurerar en enkel skrivbordsprogram toocall hello API.</span><span class="sxs-lookup"><span data-stu-id="7d61c-293">hello next procedure in hello video starts at 16:30 and configures a simple desktop application toocall hello API.</span></span> <span data-ttu-id="7d61c-294">hello första steget är tooregister hello skrivbordsprogram i Azure AD och ge det åtkomst toohello directory och toohello serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="7d61c-294">hello first step is tooregister hello desktop application in Azure AD and give it access toohello directory and toohello backend service.</span></span> <span data-ttu-id="7d61c-295">Det finns en demonstration av hello skrivbordsprogram anropar en åtgärd på hello Kalkylatorn API vid 18:25.</span><span class="sxs-lookup"><span data-stu-id="7d61c-295">At 18:25 there is a demonstration of hello desktop application calling an operation on hello calculator API.</span></span>

## <a name="configure-a-jwt-validation-policy-toopre-authorize-requests"></a><span data-ttu-id="7d61c-296">Konfigurera en JWT validering princip toopre-godkänna begäranden</span><span class="sxs-lookup"><span data-stu-id="7d61c-296">Configure a JWT validation policy toopre-authorize requests</span></span>
<span data-ttu-id="7d61c-297">hello sista proceduren i hello videon börjar vid 20:48 och visar hur toouse hello [Validera JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) princip toopre-godkänna begäranden genom att verifiera hello åtkomsttoken för varje inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="7d61c-297">hello final procedure in hello video starts at 20:48 and shows you how toouse hello [Validate JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) policy toopre-authorize requests by validating hello access tokens of each incoming request.</span></span> <span data-ttu-id="7d61c-298">Om hello begäran inte verifieras av hello Validera JWT princip, hello begäran har blockerats av API-hantering och skickas inte vidare toohello backend.</span><span class="sxs-lookup"><span data-stu-id="7d61c-298">If hello request is not validated by hello Validate JWT policy, hello request is blocked by API Management and is not passed along toohello backend.</span></span>

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

<span data-ttu-id="7d61c-299">En annan demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too13:50.</span><span class="sxs-lookup"><span data-stu-id="7d61c-299">For another demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too13:50.</span></span> <span data-ttu-id="7d61c-300">Vidarebefordra snabb too15:00 toosee hello principer som konfigurerats i hello redigeraren och sedan too18:50 för en demonstration av anropa en funktion från hello developer-portalen både med och utan hello krävs autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="7d61c-300">Fast forward too15:00 toosee hello policies configured in hello policy editor and then too18:50 for a demonstration of calling an operation from hello developer portal both with and without hello required authorization token.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d61c-301">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d61c-301">Next steps</span></span>
* <span data-ttu-id="7d61c-302">Checka ut mer [videor](https://azure.microsoft.com/documentation/videos/index/?services=api-management) om API-hantering.</span><span class="sxs-lookup"><span data-stu-id="7d61c-302">Check out more [videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) about API Management.</span></span>
* <span data-ttu-id="7d61c-303">För andra sätt toosecure serverdelstjänsten, se [ömsesidig autentisering](api-management-howto-mutual-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="7d61c-303">For other ways toosecure your backend service, see [Mutual Certificate authentication](api-management-howto-mutual-certificates.md).</span></span>

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
