---
title: Skydda ett webb-API-serverdelen med Azure Active Directory och API-hantering | Microsoft Docs
description: "Lär dig hur du skyddar en webb-API-serverdel med Azure Active Directory och API-hantering."
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
ms.openlocfilehash: 0dfb4102904c2e972e6617fd3851fb1c50147357
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a><span data-ttu-id="c329a-103">Hur du skyddar en webb-API-serverdel med Azure Active Directory och API-hantering</span><span class="sxs-lookup"><span data-stu-id="c329a-103">How to protect a Web API backend with Azure Active Directory and API Management</span></span>
<span data-ttu-id="c329a-104">Följande videoklipp visar hur du skapar ett Web API-serverdel och skydda den med hjälp av OAuth 2.0-protokollet med Azure Active Directory och API-hantering.</span><span class="sxs-lookup"><span data-stu-id="c329a-104">The following video shows how to build a Web API backend and protect it using OAuth 2.0 protocol with Azure Active Directory and API Management.</span></span>  <span data-ttu-id="c329a-105">Den här artikeln innehåller en översikt och ytterligare information för stegen i videon.</span><span class="sxs-lookup"><span data-stu-id="c329a-105">This article provides an overview and additional information for the steps in the video.</span></span> <span data-ttu-id="c329a-106">Den här 24 minuter långa videon visar hur till:</span><span class="sxs-lookup"><span data-stu-id="c329a-106">This 24 minute video shows you how to:</span></span>

* <span data-ttu-id="c329a-107">Skapa en webb-API-serverdel och skydda den med AAD - börjar vid 1:30</span><span class="sxs-lookup"><span data-stu-id="c329a-107">Build a Web API backend and secure it with AAD - starting at 1:30</span></span>
* <span data-ttu-id="c329a-108">Importera API: et i API Management - inleder 7:10</span><span class="sxs-lookup"><span data-stu-id="c329a-108">Import the API into API Management - starting at 7:10</span></span>
* <span data-ttu-id="c329a-109">Konfigurera Developer-portalen för att anropa API - början på 9:09</span><span class="sxs-lookup"><span data-stu-id="c329a-109">Configure the Developer portal to call the API - starting at 9:09</span></span>
* <span data-ttu-id="c329a-110">Konfigurera ett skrivbordsprogram att anropa API - börjar vid 18:08</span><span class="sxs-lookup"><span data-stu-id="c329a-110">Configure a desktop application to call the API - starting at 18:08</span></span>
* <span data-ttu-id="c329a-111">Konfigurera en princip för verifiering av JWT för att godkänna begäranden - börjar vid 20:47 före</span><span class="sxs-lookup"><span data-stu-id="c329a-111">Configure a JWT validation policy to pre-authorize requests - starting at 20:47</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a><span data-ttu-id="c329a-112">Skapa en Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="c329a-112">Create an Azure AD directory</span></span>
<span data-ttu-id="c329a-113">Säkerhetskopieras med Azure Active Directory måste du skydda webb-API en AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="c329a-113">To secure your Web API backed using Azure Active Directory you must first have a an AAD tenant.</span></span> <span data-ttu-id="c329a-114">I det här videoklippet en klient med namnet **APIMDemo** används.</span><span class="sxs-lookup"><span data-stu-id="c329a-114">In this video a tenant named **APIMDemo** is used.</span></span> <span data-ttu-id="c329a-115">Om du vill skapa en AAD-klient, logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com) och på **ny**->**Apptjänster**->**Active Directory**  -> **Directory**->**skapa anpassade**.</span><span class="sxs-lookup"><span data-stu-id="c329a-115">To create an AAD tenant, sign-in to the [Azure Classic Portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Active Directory**->**Directory**->**Custom Create**.</span></span> 

![Azure Active Directory][api-management-create-aad-menu]

<span data-ttu-id="c329a-117">I det här exemplet en katalog med namnet **APIMDemo** skapas med en standarddomän med namnet **DemoAPIM.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="c329a-117">In this example a directory named **APIMDemo** is created with a default domain named **DemoAPIM.onmicrosoft.com**.</span></span> <span data-ttu-id="c329a-118">Den här katalogen används i hela videon.</span><span class="sxs-lookup"><span data-stu-id="c329a-118">This directory is used throughout the video.</span></span>

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a><span data-ttu-id="c329a-120">Skapa en tjänst för webb-API som skyddas av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c329a-120">Create a Web API service secured by Azure Active Directory</span></span>
<span data-ttu-id="c329a-121">I det här steget skapas en webb-API-serverdel med hjälp av Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c329a-121">In this step, a Web API backend is created using Visual Studio 2013.</span></span> <span data-ttu-id="c329a-122">Det här steget av videon börjar vid 1:30.</span><span class="sxs-lookup"><span data-stu-id="c329a-122">This step of the video starts at 1:30.</span></span> <span data-ttu-id="c329a-123">Skapa Web API backend-projekt i Visual Studio klickar du på **filen**->**ny**->**projekt**, och välj **ASP.NET-webbprogram Programmet** från den **Web** lista över mallar.</span><span class="sxs-lookup"><span data-stu-id="c329a-123">To create Web API backend project in Visual Studio click **File**->**New**->**Project**, and choose **ASP.NET Web Application** from the **Web** templates list.</span></span> <span data-ttu-id="c329a-124">I det här videoklippet projektet med namnet **APIMAADDemo**.</span><span class="sxs-lookup"><span data-stu-id="c329a-124">In this video the project is named **APIMAADDemo**.</span></span> <span data-ttu-id="c329a-125">Klicka på **OK** för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="c329a-125">Click **OK** to create the project.</span></span> 

![Visual Studio][api-management-new-web-app]

<span data-ttu-id="c329a-127">Klicka på **Web API** från den **markerar du en lista över** att skapa ett Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="c329a-127">Click **Web API** from the **Select a template list** to create a Web API project.</span></span> <span data-ttu-id="c329a-128">Konfigurera Azure Directory Authentication Klicka **ändra autentisering**.</span><span class="sxs-lookup"><span data-stu-id="c329a-128">To configure Azure Directory Authentication click **Change Authentication**.</span></span>

![Nytt projekt][api-management-new-project]

<span data-ttu-id="c329a-130">Klicka på **Organisationskonton**, och ange den **domän** för din AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="c329a-130">Click **Organizational Accounts**, and specify the **Domain** of your AAD tenant.</span></span> <span data-ttu-id="c329a-131">I det här exemplet domänen är **DemoAPIM.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="c329a-131">In this example the domain is **DemoAPIM.onmicrosoft.com**.</span></span> <span data-ttu-id="c329a-132">Domänen för din katalog kan hämtas från den **domäner** för din katalog.</span><span class="sxs-lookup"><span data-stu-id="c329a-132">The domain of your directory can be obtained from the **Domains** tab of your directory.</span></span>

![Domäner][api-management-aad-domains]

<span data-ttu-id="c329a-134">Konfigurera önskade inställningar i den **ändra autentisering** dialogrutan och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c329a-134">Configure the desired settings in the **Change Authentication** dialog box and click **OK**.</span></span>

![Ändra autentisering][api-management-change-authentication]

<span data-ttu-id="c329a-136">När du klickar på **OK** Visual Studio kommer att försöka registrera ditt program med Azure AD-katalogen och du uppmanas att logga in med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c329a-136">When you click **OK** Visual Studio will attempt to register your application with your Azure AD directory and you may be prompted to sign in by Visual Studio.</span></span> <span data-ttu-id="c329a-137">Logga in med ett administratörskonto för din katalog.</span><span class="sxs-lookup"><span data-stu-id="c329a-137">Sign in using an administrative account for your directory.</span></span>

![Logga in till Visual Studio][api-management-sign-in-vidual-studio]

<span data-ttu-id="c329a-139">Så här konfigurerar du det här projektet som en Azure Web API du kryssrutan för **värd i molnet** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c329a-139">To configure this project as an Azure Web API check the box for **Host in the cloud** and then click **OK**.</span></span>

![Nytt projekt][api-management-new-project-cloud]

<span data-ttu-id="c329a-141">Du kan uppmanas att logga in på Azure och du kan sedan konfigurera webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c329a-141">You may be prompted to sign in to Azure, and then you can configure the Web App.</span></span>

![Konfigurera][api-management-configure-web-app]

<span data-ttu-id="c329a-143">I det här exemplet en ny **programtjänstplanen** med namnet **APIMAADDemo** har angetts.</span><span class="sxs-lookup"><span data-stu-id="c329a-143">In this example a new **App Service plan** named **APIMAADDemo** is specified.</span></span>

<span data-ttu-id="c329a-144">Klicka på **OK** att konfigurera webbprogrammet och skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="c329a-144">Click **OK** to configure the Web App and create the project.</span></span>

## <a name="add-the-code-to-the-web-api-project"></a><span data-ttu-id="c329a-145">Lägg till kod Web API-projekt</span><span class="sxs-lookup"><span data-stu-id="c329a-145">Add the code to the Web API project</span></span>
<span data-ttu-id="c329a-146">Nästa steg i videon lägger till koden Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="c329a-146">The next step in the video adds the code to the Web API project.</span></span> <span data-ttu-id="c329a-147">Det här steget startar vid 4:35.</span><span class="sxs-lookup"><span data-stu-id="c329a-147">This step starts at 4:35.</span></span>

<span data-ttu-id="c329a-148">Webb-API i det här exemplet implementerar en grundläggande Kalkylatorn tjänst med hjälp av en modell och en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="c329a-148">The Web API in this example implements a basic calculator service using a model and a controller.</span></span> <span data-ttu-id="c329a-149">Om du vill lägga till modellen för tjänsten, högerklicka på **modeller** i **Solution Explorer** och välj **Lägg till**, **klassen**.</span><span class="sxs-lookup"><span data-stu-id="c329a-149">To add the model for the service, right-click **Models** in **Solution Explorer** and choose **Add**, **Class**.</span></span> <span data-ttu-id="c329a-150">Klassen namnet `CalcInput` och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c329a-150">Name the class `CalcInput` and click **Add**.</span></span>

<span data-ttu-id="c329a-151">Lägg till följande `using` uttryck överst i den `CalcInput.cs` filen.</span><span class="sxs-lookup"><span data-stu-id="c329a-151">Add the following `using` statement to the top of the `CalcInput.cs` file.</span></span>

```c#
using Newtonsoft.Json;
```

<span data-ttu-id="c329a-152">Ersätt klassen skapas med följande kod.</span><span class="sxs-lookup"><span data-stu-id="c329a-152">Replace the generated class with the following code.</span></span>

```c#
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

<span data-ttu-id="c329a-153">Högerklicka på **domänkontrollanter** i **Solution Explorer** och välj **Lägg till**->**domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="c329a-153">Right-click **Controllers** in **Solution Explorer** and choose **Add**->**Controller**.</span></span> <span data-ttu-id="c329a-154">Välj **Web API 2 styrenhet – tom** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c329a-154">Choose **Web API 2 Controller - Empty** and click **Add**.</span></span> <span data-ttu-id="c329a-155">Typen **CalcController** styrenheten namn och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c329a-155">Type **CalcController** for the Controller name and click **Add**.</span></span>

![Lägg till styrenhet][api-management-add-controller]

<span data-ttu-id="c329a-157">Lägg till följande `using` uttryck överst i den `CalcController.cs` filen.</span><span class="sxs-lookup"><span data-stu-id="c329a-157">Add the following `using` statement to the top of the `CalcController.cs` file.</span></span>

```c#
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

<span data-ttu-id="c329a-158">Ersätt genererade controller-klassen med följande kod.</span><span class="sxs-lookup"><span data-stu-id="c329a-158">Replace the generated controller class with the following code.</span></span> <span data-ttu-id="c329a-159">Den här koden implementerar den `Add`, `Subtract`, `Multiply`, och `Divide` operations grundläggande Kalkylatorn API.</span><span class="sxs-lookup"><span data-stu-id="c329a-159">This code implements the `Add`, `Subtract`, `Multiply`, and `Divide` operations of the Basic Calculator API.</span></span>

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

<span data-ttu-id="c329a-160">Tryck på **F6** att skapa och verifiera lösningen.</span><span class="sxs-lookup"><span data-stu-id="c329a-160">Press **F6** to build and verify the solution.</span></span>

## <a name="publish-the-project-to-azure"></a><span data-ttu-id="c329a-161">Publicera projektet på Azure</span><span class="sxs-lookup"><span data-stu-id="c329a-161">Publish the project to Azure</span></span>
<span data-ttu-id="c329a-162">I det här steget Visual Studio har project publicerats till Azure.</span><span class="sxs-lookup"><span data-stu-id="c329a-162">In this step the Visual Studio project is published to Azure.</span></span> <span data-ttu-id="c329a-163">Det här steget av videon börjar på 5:45.</span><span class="sxs-lookup"><span data-stu-id="c329a-163">This step of the video starts at 5:45.</span></span>

<span data-ttu-id="c329a-164">Om du vill publicera projektet till Azure, högerklicka på den **APIMAADDemo** projektet i Visual Studio och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c329a-164">To publish the project to Azure, right-click the **APIMAADDemo** project in Visual Studio and choose **Publish**.</span></span> <span data-ttu-id="c329a-165">Behåll standardinställningarna den **Publicera webbplats** dialogrutan och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c329a-165">Keep the default settings in the **Publish Web** dialog box and click **Publish**.</span></span>

![Webbpublicering][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a><span data-ttu-id="c329a-167">Bevilja behörighet till Azure AD backend-tjänstprogram</span><span class="sxs-lookup"><span data-stu-id="c329a-167">Grant permissions to the Azure AD backend service application</span></span>
<span data-ttu-id="c329a-168">Ett nytt program för backend-tjänsten har skapats i Azure AD-katalogen som en del av processen för att konfigurera och publicera för Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="c329a-168">A new application for the backend service is created in your Azure AD directory as part of the configuring and publishing process of your Web API project.</span></span> <span data-ttu-id="c329a-169">I det här steget video, början på 6:13 behörigheter till webb-API-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="c329a-169">In this step of the video, starting at 6:13, permissions are granted to the Web API backend.</span></span>

![Program][api-management-aad-backend-app]

<span data-ttu-id="c329a-171">Klicka på namnet på tillämpningsprogrammet kan konfigurera behörigheterna som krävs.</span><span class="sxs-lookup"><span data-stu-id="c329a-171">Click the name of the application to configure the required permissions.</span></span> <span data-ttu-id="c329a-172">Navigera till den **konfigurera** fliken och rulla ned till den **behörigheter för andra program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c329a-172">Navigate to the **Configure** tab and scroll down to the **permissions to other applications** section.</span></span> <span data-ttu-id="c329a-173">Klicka på den **programbehörigheter** listrutan bredvid **Windows** **Azure Active Directory**, markera kryssrutan för **läsa katalogdata** , och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c329a-173">Click the **Application Permissions** drop-down beside **Windows** **Azure Active Directory**, check the box for **Read directory data**, and click **Save**.</span></span>

![Lägg till behörigheter][api-management-aad-add-permissions]

> [!NOTE]
> <span data-ttu-id="c329a-175">Om **Windows** **Azure Active Directory** är inte i listan under behörigheter för andra program, klickar du på **lägga till program** och lägga till den i listan.</span><span class="sxs-lookup"><span data-stu-id="c329a-175">If **Windows** **Azure Active Directory** is not listed under permissions to other applications, click **Add application** and add it from the list.</span></span>
> 
> 

<span data-ttu-id="c329a-176">Anteckna den **App-Id URI** för användning i ett senare skede när en Azure AD-programmet har konfigurerats för API Management developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="c329a-176">Make a note of the **App Id URI** for use in a subsequent step when an Azure AD application is configured for the API Management developer portal.</span></span>

![URI för App-Id][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a><span data-ttu-id="c329a-178">Importera webb-API till API-hantering</span><span class="sxs-lookup"><span data-stu-id="c329a-178">Import the Web API into API Management</span></span>
<span data-ttu-id="c329a-179">API: er konfigureras från publisher-portalen API som öppnas via Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c329a-179">APIs are configured from the API publisher portal, which is accessed through the Azure Portal.</span></span> <span data-ttu-id="c329a-180">För att nå det klickar du på **Publisher portal** från verktygsfältet för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c329a-180">To reach it, click **Publisher portal** from the toolbar of your API Management service.</span></span> <span data-ttu-id="c329a-181">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i den [hantera din första API] [ Manage your first API] kursen.</span><span class="sxs-lookup"><span data-stu-id="c329a-181">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Manage your first API][Manage your first API] tutorial.</span></span>

![Utgivarportalen][api-management-management-console]

<span data-ttu-id="c329a-183">Åtgärder kan vara [lagts till API: er manuellt](api-management-howto-add-operations.md), eller kan importeras.</span><span class="sxs-lookup"><span data-stu-id="c329a-183">Operations can be [added to APIs manually](api-management-howto-add-operations.md), or they can be imported.</span></span> <span data-ttu-id="c329a-184">I det här videoklippet importeras åtgärder i Swagger-format som börjar på 6:40.</span><span class="sxs-lookup"><span data-stu-id="c329a-184">In this video, operations are imported in Swagger format starting at 6:40.</span></span>

<span data-ttu-id="c329a-185">Skapa en fil med namnet `calcapi.json` med följande innehåll och spara den på datorn.</span><span class="sxs-lookup"><span data-stu-id="c329a-185">Create a file named `calcapi.json` with following contents and save it to your computer.</span></span> <span data-ttu-id="c329a-186">Se till att den `host` attributet pekar till din Web API-serverdel.</span><span class="sxs-lookup"><span data-stu-id="c329a-186">Ensure that the `host` attribute points to your Web API backend.</span></span> <span data-ttu-id="c329a-187">I det här exemplet `"host": "apimaaddemo.azurewebsites.net"` används.</span><span class="sxs-lookup"><span data-stu-id="c329a-187">In this example `"host": "apimaaddemo.azurewebsites.net"` is used.</span></span>

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

<span data-ttu-id="c329a-188">Du importerar kalkylator-API:et genom att klicka på **API:er** på **API Management**-menyn till vänster och sedan på **Importera API**.</span><span class="sxs-lookup"><span data-stu-id="c329a-188">To import the calculator API, click **APIs** from the **API Management** menu on the left, and then click **Import API**.</span></span>

![Knappen Importera API][api-management-import-api]

<span data-ttu-id="c329a-190">Utför följande steg om du vill konfigurera Kalkylatorn API.</span><span class="sxs-lookup"><span data-stu-id="c329a-190">Perform the following steps to configure the calculator API.</span></span>

1. <span data-ttu-id="c329a-191">Klicka på **från filen**, bläddra till den `calculator.json` fil som du sparade och klicka på den **Swagger** knappen.</span><span class="sxs-lookup"><span data-stu-id="c329a-191">Click **From file**, browse to the `calculator.json` file you saved, and click the **Swagger** radio button.</span></span>
2. <span data-ttu-id="c329a-192">Typen **beräkna** till den **URL för Web API-suffix** textruta.</span><span class="sxs-lookup"><span data-stu-id="c329a-192">Type **calc** into the **Web API URL suffix** textbox.</span></span>
3. <span data-ttu-id="c329a-193">Klicka i rutan **Produkter (valfritt)** och välj **Starter**.</span><span class="sxs-lookup"><span data-stu-id="c329a-193">Click in the **Products (optional)** box and choose **Starter**.</span></span>
4. <span data-ttu-id="c329a-194">Klicka på **Spara** för att importera API:et.</span><span class="sxs-lookup"><span data-stu-id="c329a-194">Click **Save** to import the API.</span></span>

![Lägga till ett nytt API][api-management-import-new-api]

<span data-ttu-id="c329a-196">När du har importerat API:et visas sammanfattningssidan för API:et på utgivarportalen.</span><span class="sxs-lookup"><span data-stu-id="c329a-196">Once the API is imported, the summary page for the API is displayed in the publisher portal.</span></span>

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a><span data-ttu-id="c329a-197">Anropa API: et fel från developer-portalen</span><span class="sxs-lookup"><span data-stu-id="c329a-197">Call the API unsuccessfully from the developer portal</span></span>
<span data-ttu-id="c329a-198">Nu API: et har importerats till API-hantering, men kan ännu inte anropas har från developer-portalen eftersom serverdelstjänsten skyddas med Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="c329a-198">At this point, the API has been imported into API Management, but cannot yet be called successfully from the developer portal because the backend service is protected with Azure AD authentication.</span></span> <span data-ttu-id="c329a-199">Detta visas i videon inleder med följande steg 7:40.</span><span class="sxs-lookup"><span data-stu-id="c329a-199">This is demonstrated in the video starting at 7:40 using the following steps.</span></span>

<span data-ttu-id="c329a-200">Klicka på **utvecklarportalen** från upp till höger i portalen utgivare.</span><span class="sxs-lookup"><span data-stu-id="c329a-200">Click **Developer portal** from the top-right side of the publisher portal.</span></span>

![Utvecklarportalen][api-management-developer-portal-menu]

<span data-ttu-id="c329a-202">Klicka på **API: er** och klicka på den **Kalkylatorn** API.</span><span class="sxs-lookup"><span data-stu-id="c329a-202">Click **APIs** and click the **Calculator** API.</span></span>

![Utvecklarportalen][api-management-dev-portal-apis]

<span data-ttu-id="c329a-204">Klicka på **prova**.</span><span class="sxs-lookup"><span data-stu-id="c329a-204">Click **Try it**.</span></span>

![Prova][api-management-dev-portal-try-it]

<span data-ttu-id="c329a-206">Klicka på **skicka** och notera svarsstatusen av **401 obehörig**.</span><span class="sxs-lookup"><span data-stu-id="c329a-206">Click **Send** and note the response status of **401 Unauthorized**.</span></span>

![Skicka][api-management-dev-portal-send-401]

<span data-ttu-id="c329a-208">Begäran har inte behörighet eftersom serverdelen API skyddas av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c329a-208">The request is unauthorized because the backend API is protected by Azure Active Directory.</span></span> <span data-ttu-id="c329a-209">Innan du anropar API: N i developer måste portal konfigureras för att ge utvecklare som använder OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="c329a-209">Before successfully calling the API the developer portal must be configured to authorize developers using OAuth 2.0.</span></span> <span data-ttu-id="c329a-210">Den här processen beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c329a-210">This process is described in the following sections.</span></span>

## <a name="register-the-developer-portal-as-an-aad-application"></a><span data-ttu-id="c329a-211">Registrera developer-portalen som ett AAD-program</span><span class="sxs-lookup"><span data-stu-id="c329a-211">Register the developer portal as an AAD application</span></span>
<span data-ttu-id="c329a-212">Det första steget i att konfigurera developer-portalen för att ge utvecklare som använder OAuth 2.0 är att registrera developer-portalen som ett AAD-program.</span><span class="sxs-lookup"><span data-stu-id="c329a-212">The first step in configuring the developer portal to authorize developers using OAuth 2.0 is to register the developer portal as an AAD application.</span></span> <span data-ttu-id="c329a-213">Detta visas från 8:27 i videon.</span><span class="sxs-lookup"><span data-stu-id="c329a-213">This is demonstrated starting at 8:27 in the video.</span></span>

<span data-ttu-id="c329a-214">Gå till Azure AD-klient från det första steget i den här videon i det här exemplet **APIMDemo** och navigera till den **program** fliken.</span><span class="sxs-lookup"><span data-stu-id="c329a-214">Navigate to the Azure AD tenant from the first step of this video, in this example **APIMDemo** and navigate to the **Applications** tab.</span></span>

![nytt program][api-management-aad-new-application-devportal]

<span data-ttu-id="c329a-216">Klicka på den **Lägg till** knappen om du vill skapa ett nytt program för Azure Active Directory och välj **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="c329a-216">Click the **Add** button to create a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![nytt program][api-management-new-aad-application-menu]

<span data-ttu-id="c329a-218">Välj **Web application och/eller webb-API**, ange ett namn och klicka på pilen Nästa.</span><span class="sxs-lookup"><span data-stu-id="c329a-218">Choose **Web application and/or Web API**, enter a name, and click the next arrow.</span></span> <span data-ttu-id="c329a-219">I det här exemplet **APIMDeveloperPortal** används.</span><span class="sxs-lookup"><span data-stu-id="c329a-219">In this example **APIMDeveloperPortal** is used.</span></span>

![nytt program][api-management-aad-new-application-devportal-1]

<span data-ttu-id="c329a-221">För **inloggnings-URL** anger du URL för API Management-tjänsten och Lägg till `/signin`.</span><span class="sxs-lookup"><span data-stu-id="c329a-221">For **Sign-on URL** enter the URL of your API Management service and append `/signin`.</span></span> <span data-ttu-id="c329a-222">I det här exemplet `https://contoso5.portal.azure-api.net/signin` används.</span><span class="sxs-lookup"><span data-stu-id="c329a-222">In this example `https://contoso5.portal.azure-api.net/signin` is used.</span></span>

<span data-ttu-id="c329a-223">För **App-Id-URL** anger du URL för API Management-tjänsten och lägga till vissa unika tecken.</span><span class="sxs-lookup"><span data-stu-id="c329a-223">For **App Id URL** enter the URL of your API Management service and append some unique characters.</span></span> <span data-ttu-id="c329a-224">Det kan vara önskade tecken och i det här exemplet `https://contoso5.portal.azure-api.net/dp` används.</span><span class="sxs-lookup"><span data-stu-id="c329a-224">These can be any desired characters and in this example `https://contoso5.portal.azure-api.net/dp` is used.</span></span> <span data-ttu-id="c329a-225">När den önskade **appegenskaper** är konfigurerad, klicka på kryssmarkeringen för att skapa programmet.</span><span class="sxs-lookup"><span data-stu-id="c329a-225">When the  desired **App properties** are configured, click the check mark to create the application.</span></span>

![nytt program][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a><span data-ttu-id="c329a-227">Konfigurera en server för API Management OAuth 2.0-auktorisering</span><span class="sxs-lookup"><span data-stu-id="c329a-227">Configure an API Management OAuth 2.0 authorization server</span></span>
<span data-ttu-id="c329a-228">Nästa steg är att konfigurera en server för OAuth 2.0-auktorisering i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="c329a-228">The next step is to configure an OAuth 2.0 authorization server in API Management.</span></span> <span data-ttu-id="c329a-229">Det här steget visas i början på 9:43 videon.</span><span class="sxs-lookup"><span data-stu-id="c329a-229">This step is demonstrated in the video starting at 9:43.</span></span>

<span data-ttu-id="c329a-230">Klicka på **säkerhet** API Management-menyn till vänster klickar du på **OAuth 2.0**, och klicka sedan på **lägga till tillståndet** server.</span><span class="sxs-lookup"><span data-stu-id="c329a-230">Click **Security** from the API Management menu on the left, click **OAuth 2.0**, and then click **Add authorization** server.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server]

<span data-ttu-id="c329a-232">Ange ett namn och en valfri beskrivning i den **namn** och **beskrivning** fält.</span><span class="sxs-lookup"><span data-stu-id="c329a-232">Enter a name and an optional description in the **Name** and **Description** fields.</span></span> <span data-ttu-id="c329a-233">De här fälten används för att identifiera servern OAuth 2.0 auktorisering i API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="c329a-233">These fields are used to identify the OAuth 2.0 authorization server within the API Management service instance.</span></span> <span data-ttu-id="c329a-234">I det här exemplet **auktorisering server demo** används.</span><span class="sxs-lookup"><span data-stu-id="c329a-234">In this example **Authorization server demo** is used.</span></span> <span data-ttu-id="c329a-235">Senare när du anger en OAuth 2.0-server som ska användas för autentisering för ett API som du väljer det här namnet.</span><span class="sxs-lookup"><span data-stu-id="c329a-235">Later when you specify an OAuth 2.0 server to be used for authentication for an API, you will select this name.</span></span>

<span data-ttu-id="c329a-236">För den **klienten URL för registrering** ange ett platshållarvärde som `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="c329a-236">For the **Client registration page URL** enter a placeholder value such as `http://localhost`.</span></span>  <span data-ttu-id="c329a-237">Den **klienten URL för registrering** pekar på den sida som användare kan använda för att skapa och konfigurera sina egna konton för OAuth 2.0-leverantörer som stöder Användarhantering av konton.</span><span class="sxs-lookup"><span data-stu-id="c329a-237">The **Client registration page URL** points to the page that users can use to create and configure their own accounts for OAuth 2.0 providers that support user management of accounts.</span></span> <span data-ttu-id="c329a-238">I det här exemplet användare inte skapa och konfigurera sina egna konton, så att en platshållare används.</span><span class="sxs-lookup"><span data-stu-id="c329a-238">In this example users do not create and configure their own accounts so a placeholder is used.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server-1]

<span data-ttu-id="c329a-240">Ange sedan **slutpunkts-URL-auktorisering** och **Token slutpunkts-URL**.</span><span class="sxs-lookup"><span data-stu-id="c329a-240">Next, specify **Authorization endpoint URL** and **Token endpoint URL**.</span></span>

![auktorisering server][api-management-add-authorization-server-1a]

<span data-ttu-id="c329a-242">Dessa värden kan hämtas från den **App slutpunkter** sidan i AAD-program som du skapade för developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="c329a-242">These values can be retrieved from the **App Endpoints** page of the AAD application you created for the developer portal.</span></span> <span data-ttu-id="c329a-243">Åtkomst till slutpunkterna navigerar du till den **konfigurera** för AAD-program och klicka på **visa slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="c329a-243">To access the endpoints navigate to the **Configure** tab for the AAD application and click **View endpoints**.</span></span>

![Program][api-management-aad-devportal-application]

![Visa slutpunkter][api-management-aad-view-endpoints]

<span data-ttu-id="c329a-246">Kopiera den **OAuth 2.0 autentiseringsslutpunkt** och klistrar in det i den **slutpunkts-URL-auktorisering** textruta.</span><span class="sxs-lookup"><span data-stu-id="c329a-246">Copy the **OAuth 2.0 authorization endpoint** and paste it into the **Authorization endpoint URL** textbox.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server-2]

<span data-ttu-id="c329a-248">Kopiera den **OAuth 2.0-token för slutpunkt** och klistrar in det i den **Token slutpunkts-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="c329a-248">Copy the **OAuth 2.0 token endpoint** and paste it into the **Token endpoint URL** textbox.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server-2a]

<span data-ttu-id="c329a-250">Förutom klistra in tokenslutpunkten, lägga till en ytterligare brödtext parameter med namnet **resurs** och använda värdet i **App-Id URI** från AAD-program för backend-tjänst som skapades när den Visual Studio-projekt har publicerats.</span><span class="sxs-lookup"><span data-stu-id="c329a-250">In addition to pasting in the token endpoint, add an additional body parameter named **resource** and for the value use the **App Id URI** from the AAD application for the backend service that was created when the Visual Studio project was published.</span></span>

![URI för App-Id][api-management-aad-sso-uri]

<span data-ttu-id="c329a-252">Därefter anger du klientautentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="c329a-252">Next, specify the client credentials.</span></span> <span data-ttu-id="c329a-253">Det är dessa autentiseringsuppgifter för den resurs som du vill komma åt i det här fallet developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="c329a-253">These are the credentials for the resource you want to access, in this case the developer portal.</span></span>

![Klientens autentiseringsuppgifter][api-management-client-credentials]

<span data-ttu-id="c329a-255">Att hämta den **klient-Id**, navigera till den **konfigurera** för AAD-programmet för developer-portalen och kopiera den **klient-Id**.</span><span class="sxs-lookup"><span data-stu-id="c329a-255">To get the **Client Id**, navigate to the **Configure** tab of the AAD application for the developer portal and copy the **Client Id**.</span></span>

<span data-ttu-id="c329a-256">Att hämta den **Klienthemlighet** klickar du på den **Markera varaktighet** listrutan i den **nycklar** avsnittet och ange ett intervall.</span><span class="sxs-lookup"><span data-stu-id="c329a-256">To get the **Client Secret** click the **Select duration** drop-down in the **Keys** section and specify an interval.</span></span> <span data-ttu-id="c329a-257">I det här exemplet används 1 år.</span><span class="sxs-lookup"><span data-stu-id="c329a-257">In this example 1 year is used.</span></span>

![Klient-ID][api-management-aad-client-id]

<span data-ttu-id="c329a-259">Klicka på **spara** att spara konfigurationen och visa nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c329a-259">Click **Save** to save the configuration and display the key.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="c329a-260">Anteckna den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c329a-260">Make a note of this key.</span></span> <span data-ttu-id="c329a-261">När du stänger fönstret Azure Active Directory-konfiguration kan nyckeln inte visas igen.</span><span class="sxs-lookup"><span data-stu-id="c329a-261">Once you close the Azure Active Directory configuration window, the key cannot be displayed again.</span></span>
> 
> 

<span data-ttu-id="c329a-262">Kopiera nyckeln till Urklipp, växla tillbaka till publisher-portalen, klistra in nyckeln till den **Klienthemlighet** textruta och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c329a-262">Copy the key to the clipboard, switch back to the publisher portal, paste the key into the **Client Secret** textbox, and click **Save**.</span></span>

![Lägg till auktorisering server][api-management-add-authorization-server-3]

<span data-ttu-id="c329a-264">Direkt efter klientens autentiseringsuppgifter är en kod bevilja för auktorisering.</span><span class="sxs-lookup"><span data-stu-id="c329a-264">Immediately following the client credentials is an authorization code grant.</span></span> <span data-ttu-id="c329a-265">Kopiera den här auktoriseringskod och går tillbaka till Azure AD developer portal programmet Konfigurera och klistra in authorization grant i den **Reply URL** fältet och klickar på **spara** igen.</span><span class="sxs-lookup"><span data-stu-id="c329a-265">Copy this authorization code and switch back to your Azure AD developer portal application configure page, and paste the authorization grant into the **Reply URL** field, and click **Save** again.</span></span>

![Reply-URL][api-management-aad-reply-url]

<span data-ttu-id="c329a-267">Nästa steg är att konfigurera behörigheter för developer-portalen AAD-program.</span><span class="sxs-lookup"><span data-stu-id="c329a-267">The next step is to configure the permissions for the developer portal AAD application.</span></span> <span data-ttu-id="c329a-268">Klicka på **programbehörigheter** och markera kryssrutan för **läsa katalogdata**.</span><span class="sxs-lookup"><span data-stu-id="c329a-268">Click **Application Permissions** and check the box for **Read directory data**.</span></span> <span data-ttu-id="c329a-269">Klicka på **spara** att spara den här ändringen och klicka sedan på **lägga till program**.</span><span class="sxs-lookup"><span data-stu-id="c329a-269">Click **Save** to save this change, and then click **Add application**.</span></span>

![Lägg till behörigheter][api-management-add-devportal-permissions]

<span data-ttu-id="c329a-271">Klicka på sökikonen, typen **APIM** i Starta med rutan, Välj **APIMAADDemo**, och klicka på bockmarkeringen för att spara.</span><span class="sxs-lookup"><span data-stu-id="c329a-271">Click the search icon, type **APIM** into the Starting with box, select **APIMAADDemo**, and click the check mark to save.</span></span>

![Lägg till behörigheter][api-management-aad-add-app-permissions]

<span data-ttu-id="c329a-273">Klicka på **delegerade behörigheter** för **APIMAADDemo** och markera kryssrutan för **åtkomst APIMAADDemo**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c329a-273">Click **Delegated Permissions** for **APIMAADDemo** and check the box for **Access APIMAADDemo**, and click **Save**.</span></span> <span data-ttu-id="c329a-274">Detta kan utvecklare portalprogram för åtkomst till backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c329a-274">This allows the developer portal application to access the backend service.</span></span>

![Lägg till behörigheter][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a><span data-ttu-id="c329a-276">Aktivera OAuth 2.0 användarautentiseringen för Kalkylatorn API</span><span class="sxs-lookup"><span data-stu-id="c329a-276">Enable OAuth 2.0 user authorization for the Calculator API</span></span>
<span data-ttu-id="c329a-277">OAuth 2.0-server är konfigurerat kan du ange den i säkerhetsinställningarna för ditt API.</span><span class="sxs-lookup"><span data-stu-id="c329a-277">Now that the OAuth 2.0 server is configured, you can specify it in the security settings for your API.</span></span> <span data-ttu-id="c329a-278">Det här steget visas i videon börjar på 14:30.</span><span class="sxs-lookup"><span data-stu-id="c329a-278">This step is demonstrated in the video starting at 14:30.</span></span>

<span data-ttu-id="c329a-279">Klicka på **API: er** i den vänstra menyn och klicka på **Kalkylatorn** att visa och konfigurera dess inställningar.</span><span class="sxs-lookup"><span data-stu-id="c329a-279">Click **APIs** in the left menu, and click  **Calculator** to view and configure its settings.</span></span>

![Kalkylatorn API][api-management-calc-api]

<span data-ttu-id="c329a-281">Navigera till den **säkerhet** markerar den **OAuth 2.0** kryssrutan Markera önskad auktorisering servern från den **auktorisering server** listrutan, och klicka på  **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c329a-281">Navigate to the **Security** tab, check the **OAuth 2.0** checkbox, select the desired authorization server from the **Authorization server** drop-down, and click **Save**.</span></span>

![Kalkylatorn API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a><span data-ttu-id="c329a-283">Anropa API: et Kalkylatorn från developer-portalen</span><span class="sxs-lookup"><span data-stu-id="c329a-283">Successfully call the Calculator API from the developer portal</span></span>
<span data-ttu-id="c329a-284">OAuth 2.0-tillståndet är konfigurerat på API anropas åtgärderna har från developer center.</span><span class="sxs-lookup"><span data-stu-id="c329a-284">Now that the OAuth 2.0 authorization is configured on the API, its operations can be successfully called from the developer center.</span></span> <span data-ttu-id="c329a-285">Det här steget visas i videon börjar på 15:00.</span><span class="sxs-lookup"><span data-stu-id="c329a-285">THis step is demonstrated in the video starting at 15:00.</span></span>

<span data-ttu-id="c329a-286">Gå tillbaka till den **lägga till två heltal** Kalkylatorn tjänsten i developer-portalen och klicka på **prova**.</span><span class="sxs-lookup"><span data-stu-id="c329a-286">Navigate back to the **Add two integers** operation of the calculator service in the developer portal and click **Try it**.</span></span> <span data-ttu-id="c329a-287">Observera att det nya objektet i den **auktorisering** avsnittet motsvarar auktoriserings-server som du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="c329a-287">Note the new item in the **Authorization** section corresponding to the authorization server you just added.</span></span>

![Kalkylatorn API][api-management-calc-authorization-server]

<span data-ttu-id="c329a-289">Välj **Auktoriseringskoden** från auktorisering nedrullningsbara listan och ange autentiseringsuppgifterna för kontot som ska användas.</span><span class="sxs-lookup"><span data-stu-id="c329a-289">Select **Authorization code** from the authorization drop-down list and enter the credentials of the account to use.</span></span> <span data-ttu-id="c329a-290">Om du redan har loggat in med kontot kan du inte ombedd.</span><span class="sxs-lookup"><span data-stu-id="c329a-290">If you are already signed in with the account you may not be prompted.</span></span>

![Kalkylatorn API][api-management-devportal-authorization-code]

<span data-ttu-id="c329a-292">Klicka på **skicka** och notera den **svarsstatusen** av **200 OK** och resultatet av åtgärden i svaret innehållet.</span><span class="sxs-lookup"><span data-stu-id="c329a-292">Click **Send** and note the **Response status** of **200 OK** and the results of the operation in the response content.</span></span>

![Kalkylatorn API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a><span data-ttu-id="c329a-294">Konfigurera ett skrivbordsprogram att anropa API: et</span><span class="sxs-lookup"><span data-stu-id="c329a-294">Configure a desktop application to call the API</span></span>
<span data-ttu-id="c329a-295">I nästa procedur i videon börjar vid 16:30 och konfigurerar en enkel skrivbordsprogram att anropa API: et.</span><span class="sxs-lookup"><span data-stu-id="c329a-295">The next procedure in the video starts at 16:30 and configures a simple desktop application to call the API.</span></span> <span data-ttu-id="c329a-296">Det första steget är att registrera programmet i Azure AD och ge det åtkomst till katalogen och serverdelstjänsten.</span><span class="sxs-lookup"><span data-stu-id="c329a-296">The first step is to register the desktop application in Azure AD and give it access to the directory and to the backend service.</span></span> <span data-ttu-id="c329a-297">Det finns en demonstration av skrivbordsprogram anropar en åtgärd på Kalkylatorn API vid 18:25.</span><span class="sxs-lookup"><span data-stu-id="c329a-297">At 18:25 there is a demonstration of the desktop application calling an operation on the calculator API.</span></span>

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a><span data-ttu-id="c329a-298">Konfigurera en princip för verifiering av JWT för att godkänna begäranden före</span><span class="sxs-lookup"><span data-stu-id="c329a-298">Configure a JWT validation policy to pre-authorize requests</span></span>
<span data-ttu-id="c329a-299">Den sista proceduren i videon börjar vid 20:48 och visar hur du använder den [Validera JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) princip för att auktorisera före begäranden genom att verifiera åtkomsttoken för varje inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="c329a-299">The final procedure in the video starts at 20:48 and shows you how to use the [Validate JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) policy to pre-authorize requests by validating the access tokens of each incoming request.</span></span> <span data-ttu-id="c329a-300">Om begäran inte har verifierats av Validera JWT-principen, begäran har blockerats av API-hantering och skickas inte vidare till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="c329a-300">If the request is not validated by the Validate JWT policy, the request is blocked by API Management and is not passed along to the backend.</span></span>

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

<span data-ttu-id="c329a-301">En annan demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och spola framåt till 13:50.</span><span class="sxs-lookup"><span data-stu-id="c329a-301">For another demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 13:50.</span></span> <span data-ttu-id="c329a-302">Spola fram till 15:00 för att se de principer som konfigurerats i Redigeraren för grupprinciper och sedan till 18:50 för en demonstration av anropa en funktion från utvecklarportal både med och utan autentiseringstoken som krävs.</span><span class="sxs-lookup"><span data-stu-id="c329a-302">Fast forward to 15:00 to see the policies configured in the policy editor and then to 18:50 for a demonstration of calling an operation from the developer portal both with and without the required authorization token.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c329a-303">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c329a-303">Next steps</span></span>
* <span data-ttu-id="c329a-304">Checka ut mer [videor](https://azure.microsoft.com/documentation/videos/index/?services=api-management) om API-hantering.</span><span class="sxs-lookup"><span data-stu-id="c329a-304">Check out more [videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) about API Management.</span></span>
* <span data-ttu-id="c329a-305">Andra sätt att skydda din backend-tjänst finns [ömsesidig autentisering](api-management-howto-mutual-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="c329a-305">For other ways to secure your backend service, see [Mutual Certificate authentication](api-management-howto-mutual-certificates.md).</span></span>

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
