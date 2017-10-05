---
title: "App Service API Apps metadata för API-identifiering och kodgenerering | Microsoft Docs"
description: "Lär dig hur API Apps i Azure App Service använda Swagger-metadata för att underlätta API-identifiering och kodgenerering."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: c7f8e33a-61cc-486f-89df-4a97dc3c71d4
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: alkarche
ms.openlocfilehash: 800bb9df9b957bec2c80abb3edefbaf354b549ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a><span data-ttu-id="b9a91-103">App Service API Apps metadata för API-identifiering och kodgenerering</span><span class="sxs-lookup"><span data-stu-id="b9a91-103">App Service API Apps metadata for API discovery and code generation</span></span>
<span data-ttu-id="b9a91-104">Stöd för [Swagger 2.0](http://swagger.io/) API-metadata är inbyggt i API Apps i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="b9a91-104">Support for [Swagger 2.0](http://swagger.io/) API metadata is built into App Service API Apps.</span></span> <span data-ttu-id="b9a91-105">Du behöver använda Swagger, men om du använder den, kan du dra nytta av API Apps funktioner som gör identifiering och förbrukning av enklare.</span><span class="sxs-lookup"><span data-stu-id="b9a91-105">You don't have to use Swagger, but if you do use it, you can take advantage of API Apps features that make discovery and consumption easier.</span></span>   

## <a name="swagger-endpoint"></a><span data-ttu-id="b9a91-106">Swagger-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="b9a91-106">Swagger endpoint</span></span>
<span data-ttu-id="b9a91-107">Du kan ange en slutpunkt som innehåller Swagger 2.0 JSON-metadata för en API-app i en egenskap för API-app.</span><span class="sxs-lookup"><span data-stu-id="b9a91-107">You can specify an endpoint that provides Swagger 2.0 JSON metadata for an API app in a property of the API app.</span></span> <span data-ttu-id="b9a91-108">Slutpunkten kan vara i förhållande till bas-URL för API-appen eller en absolut URL.</span><span class="sxs-lookup"><span data-stu-id="b9a91-108">The endpoint can be relative to the base URL of the API app or an absolute URL.</span></span> <span data-ttu-id="b9a91-109">Absoluta URL: er kan pekar utanför API-app.</span><span class="sxs-lookup"><span data-stu-id="b9a91-109">Absolute URLs can point outside the API app.</span></span> 

<span data-ttu-id="b9a91-110">Många underordnade klienter (till exempel kodgenerering för Visual Studio och PowerApps ”Lägg till API” flöde), URL-Adressen måste vara allmänt tillgänglig (som inte skyddas av användare eller autentiseringen av tjänsten).</span><span class="sxs-lookup"><span data-stu-id="b9a91-110">Many downstream clients (for example, Visual Studio code generation and PowerApps "Add API" flow), the URL must be publicly accessible (not protected by user or service authentication).</span></span> <span data-ttu-id="b9a91-111">Det innebär att om du använder App Service-autentisering och vill exponera API-definition i din app sig själv, du behöver använda autentiseringsalternativet som tillåter anonym trafik till ditt API.</span><span class="sxs-lookup"><span data-stu-id="b9a91-111">This means that if you're using App Service authentication and want to expose the API definition from within your app itself, you need to use authentication option that allows anonymous traffic to reach your API.</span></span> <span data-ttu-id="b9a91-112">Mer information finns i [autentisering och auktorisering för API Apps i Apptjänst](app-service-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="b9a91-112">For more information, see [Authentication and authorization for App Service API Apps](app-service-api-authentication.md).</span></span>

### <a name="portal-blade"></a><span data-ttu-id="b9a91-113">Portalblad</span><span class="sxs-lookup"><span data-stu-id="b9a91-113">Portal blade</span></span>
<span data-ttu-id="b9a91-114">I den [Azure-portalen](https://portal.azure.com/) slutpunkten URL kan visas och ändras på den **API-Definition** bladet.</span><span class="sxs-lookup"><span data-stu-id="b9a91-114">In the [Azure portal](https://portal.azure.com/) the endpoint URL can be seen and changed on the **API Definition** blade.</span></span>

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a><span data-ttu-id="b9a91-115">Azure Resource Manager-egenskap</span><span class="sxs-lookup"><span data-stu-id="b9a91-115">Azure Resource Manager property</span></span>
<span data-ttu-id="b9a91-116">Du kan också konfigurera URL: en för API-definition för en API-app med hjälp av [Resursläsaren](https://resources.azure.com/) eller [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) i kommandoradsverktyg som [Azure PowerShell](/powershell/azureps-cmdlets-docs)och [Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="b9a91-116">You can also configure the API definition URL for an API app by using [Resource Explorer](https://resources.azure.com/) or [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) in command line tools such as [Azure PowerShell](/powershell/azureps-cmdlets-docs) and the [Azure CLI](../cli-install-nodejs.md).</span></span> 

<span data-ttu-id="b9a91-117">I **Resursläsaren**, gå till **prenumerationer > {din prenumeration} > resursgrupper > {din resursgrupp} > providers > Microsoft.Web > Platser > {webbplatsen} > config > web** , ser du den `apiDefinition` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="b9a91-117">In **Resource Explorer**, go to **subscriptions > {your subscription} > resourceGroups > {your resource group} > providers > Microsoft.Web > sites > {your site} > config > web**, and you'll see the `apiDefinition` property:</span></span>

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

<span data-ttu-id="b9a91-118">Ett exempel på en Azure Resource Manager-mall som anger den `apiDefinition` egenskap, öppna den [azuredeploy.JSON i att göra-lista exempelprogrammet](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="b9a91-118">For an example of an Azure Resource Manager template that sets the `apiDefinition` property, open the [azuredeploy.json file in the To-Do List sample application](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span></span> <span data-ttu-id="b9a91-119">Hitta avsnitt i mallen som ser ut som JSON-exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="b9a91-119">Find the section of the template that looks like the JSON sample shown above.</span></span>

### <a name="default-value"></a><span data-ttu-id="b9a91-120">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="b9a91-120">Default value</span></span>
<span data-ttu-id="b9a91-121">När du använder Visual Studio för att skapa en API-app, API-definition slutpunkten anges automatiskt till bas-URL för API-appen plus `/swagger/docs/v1`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-121">When you use Visual Studio to create an API app, the API definition endpoint is automatically set to the base URL of the API app plus `/swagger/docs/v1`.</span></span> <span data-ttu-id="b9a91-122">Detta är den standard-URL som den [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-paketet använder för att hantera dynamiskt genererade Swagger-metadata för ett ASP.NET Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="b9a91-122">This is the default URL that the [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet package uses to serve dynamically generated Swagger metadata for an ASP.NET Web API project.</span></span> 

## <a name="code-generation"></a><span data-ttu-id="b9a91-123">Kodgenerering</span><span class="sxs-lookup"><span data-stu-id="b9a91-123">Code generation</span></span>
<span data-ttu-id="b9a91-124">En av fördelarna med att integrera Swagger i Azure API apps är automatisk kodgenerering.</span><span class="sxs-lookup"><span data-stu-id="b9a91-124">One of the benefits of integrating Swagger into Azure API apps is automatic code generation.</span></span> <span data-ttu-id="b9a91-125">Genererade klientklasser gör det enklare att skriva kod som anropar en API-app.</span><span class="sxs-lookup"><span data-stu-id="b9a91-125">Generated client classes make it easier to write code that calls an API app.</span></span>

<span data-ttu-id="b9a91-126">Du kan generera klientkod till en API-app med hjälp av Visual Studio eller från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b9a91-126">You can generate client code for an API app by using Visual Studio or from the command line.</span></span> <span data-ttu-id="b9a91-127">Information om hur du genererar klientklasser i Visual Studio för ett ASP.NET Web API-projekt finns [Kom igång med API Apps och ASP.NET](app-service-api-dotnet-get-started.md#codegen).</span><span class="sxs-lookup"><span data-stu-id="b9a91-127">For information about how to generate client classes in Visual Studio for an ASP.NET Web API project, see [Get started with API Apps and ASP.NET](app-service-api-dotnet-get-started.md#codegen).</span></span> <span data-ttu-id="b9a91-128">Information om hur du gör det från kommandoraden för alla språk som stöds finns i filen Viktigt för de [Azure/autorest](https://github.com/azure/autorest) på GitHub.com.</span><span class="sxs-lookup"><span data-stu-id="b9a91-128">For information about how to do it from the command line for all supported languages, see the readme file of the [Azure/autorest](https://github.com/azure/autorest) repository on GitHub.com.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9a91-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9a91-129">Next steps</span></span>
<span data-ttu-id="b9a91-130">En stegvis självstudiekurs som hjälper dig att skapa, distribuera och använda en API-app finns i [Kom igång med API Apps i Azure App Service](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b9a91-130">For a step-by-step tutorial that guides you through creating, deploying, and consuming an API app, see [Get started with API Apps in Azure App Service](app-service-api-dotnet-get-started.md).</span></span>

<span data-ttu-id="b9a91-131">Om du använder Azure API Management med API Apps, kan du använda Swagger-metadata för att importera din API i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="b9a91-131">If you use Azure API Management with API Apps, you can use Swagger metadata to import your API into API Management.</span></span> <span data-ttu-id="b9a91-132">Mer information finns i [importera definitionen av ett API med åtgärder i Azure API Management](../api-management/api-management-howto-import-api.md).</span><span class="sxs-lookup"><span data-stu-id="b9a91-132">For more information, see [How to import the definition of an API with operations in Azure API Management](../api-management/api-management-howto-import-api.md).</span></span> 

