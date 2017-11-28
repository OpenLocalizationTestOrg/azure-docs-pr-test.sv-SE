---
title: "aaaApp API Apps i metadata för API-identifiering och kodgenerering | Microsoft Docs"
description: "Lär dig hur API Apps i Azure App Service använda Swagger-metadata toofacilitate API identifiering och kodgenerering."
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
ms.openlocfilehash: b27e70b7dd6bd97f5b0b490b496320befe7442c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a><span data-ttu-id="e5ad3-103">App Service API Apps metadata för API-identifiering och kodgenerering</span><span class="sxs-lookup"><span data-stu-id="e5ad3-103">App Service API Apps metadata for API discovery and code generation</span></span>
<span data-ttu-id="e5ad3-104">Stöd för [Swagger 2.0](http://swagger.io/) API-metadata är inbyggt i API Apps i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-104">Support for [Swagger 2.0](http://swagger.io/) API metadata is built into App Service API Apps.</span></span> <span data-ttu-id="e5ad3-105">Du har inte toouse Swagger, men om du använder den, kan du dra nytta av API Apps funktioner som gör identifiering och förbrukning av enklare.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-105">You don't have toouse Swagger, but if you do use it, you can take advantage of API Apps features that make discovery and consumption easier.</span></span>   

## <a name="swagger-endpoint"></a><span data-ttu-id="e5ad3-106">Swagger-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="e5ad3-106">Swagger endpoint</span></span>
<span data-ttu-id="e5ad3-107">Du kan ange en slutpunkt som innehåller Swagger 2.0 JSON-metadata för en API-app i en egenskap för hello API-app.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-107">You can specify an endpoint that provides Swagger 2.0 JSON metadata for an API app in a property of hello API app.</span></span> <span data-ttu-id="e5ad3-108">hello endpoint kan vara relativt toohello bas-URL för hello API-app eller en absolut URL.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-108">hello endpoint can be relative toohello base URL of hello API app or an absolute URL.</span></span> <span data-ttu-id="e5ad3-109">Absoluta URL: er kan pekar utanför hello API-app.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-109">Absolute URLs can point outside hello API app.</span></span> 

<span data-ttu-id="e5ad3-110">Många underordnade klienter (till exempel kodgenerering för Visual Studio och PowerApps ”Lägg till API” flöde), hello URL måste vara offentligt tillgänglig (som inte skyddas av användare eller autentiseringen av tjänsten).</span><span class="sxs-lookup"><span data-stu-id="e5ad3-110">Many downstream clients (for example, Visual Studio code generation and PowerApps "Add API" flow), hello URL must be publicly accessible (not protected by user or service authentication).</span></span> <span data-ttu-id="e5ad3-111">Det innebär att om du använder App Service-autentisering och vill tooexpose hello API-definition från i din app sig själv, måste toouse autentisering alternativ som tillåter anonym trafik tooreach din API.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-111">This means that if you're using App Service authentication and want tooexpose hello API definition from within your app itself, you need toouse authentication option that allows anonymous traffic tooreach your API.</span></span> <span data-ttu-id="e5ad3-112">Mer information finns i [autentisering och auktorisering för API Apps i Apptjänst](app-service-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="e5ad3-112">For more information, see [Authentication and authorization for App Service API Apps](app-service-api-authentication.md).</span></span>

### <a name="portal-blade"></a><span data-ttu-id="e5ad3-113">Portalblad</span><span class="sxs-lookup"><span data-stu-id="e5ad3-113">Portal blade</span></span>
<span data-ttu-id="e5ad3-114">I hello [Azure-portalen](https://portal.azure.com/) hello endpoint URL kan visas och ändras på hello **API-Definition** bladet.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-114">In hello [Azure portal](https://portal.azure.com/) hello endpoint URL can be seen and changed on hello **API Definition** blade.</span></span>

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a><span data-ttu-id="e5ad3-115">Azure Resource Manager-egenskap</span><span class="sxs-lookup"><span data-stu-id="e5ad3-115">Azure Resource Manager property</span></span>
<span data-ttu-id="e5ad3-116">Du kan också konfigurera URL för hello API-definition för en API-app med hjälp av [Resursläsaren](https://resources.azure.com/) eller [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) i kommandoradsverktyg som [Azure PowerShell](/powershell/azureps-cmdlets-docs)och hello [Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="e5ad3-116">You can also configure hello API definition URL for an API app by using [Resource Explorer](https://resources.azure.com/) or [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) in command line tools such as [Azure PowerShell](/powershell/azureps-cmdlets-docs) and hello [Azure CLI](../cli-install-nodejs.md).</span></span> 

<span data-ttu-id="e5ad3-117">I **Resursläsaren**, gå för**prenumerationer > {din prenumeration} > resursgrupper > {din resursgrupp} > providers > Microsoft.Web > Platser > {din plats} > config > web**, ser du hello `apiDefinition` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="e5ad3-117">In **Resource Explorer**, go too**subscriptions > {your subscription} > resourceGroups > {your resource group} > providers > Microsoft.Web > sites > {your site} > config > web**, and you'll see hello `apiDefinition` property:</span></span>

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

<span data-ttu-id="e5ad3-118">Ett exempel på en Azure Resource Manager-mall som anger hello `apiDefinition` egenskapen, öppna hello [azuredeploy.JSON i hello uppgiftslista exempelprogrammet](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="e5ad3-118">For an example of an Azure Resource Manager template that sets hello `apiDefinition` property, open hello [azuredeploy.json file in hello To-Do List sample application](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span></span> <span data-ttu-id="e5ad3-119">Hitta hello avsnitt i hello-mall som ser ut som hello JSON-exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-119">Find hello section of hello template that looks like hello JSON sample shown above.</span></span>

### <a name="default-value"></a><span data-ttu-id="e5ad3-120">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="e5ad3-120">Default value</span></span>
<span data-ttu-id="e5ad3-121">När du använder Visual Studio toocreate en API-app hello API definition slutpunkten anges toohello bas-URL plus hello API-appen automatiskt `/swagger/docs/v1`.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-121">When you use Visual Studio toocreate an API app, hello API definition endpoint is automatically set toohello base URL of hello API app plus `/swagger/docs/v1`.</span></span> <span data-ttu-id="e5ad3-122">Detta är hello standard-URL som hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-paketet använder tooserve dynamiskt genererade Swagger-metadata för ett ASP.NET Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-122">This is hello default URL that hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet package uses tooserve dynamically generated Swagger metadata for an ASP.NET Web API project.</span></span> 

## <a name="code-generation"></a><span data-ttu-id="e5ad3-123">Kodgenerering</span><span class="sxs-lookup"><span data-stu-id="e5ad3-123">Code generation</span></span>
<span data-ttu-id="e5ad3-124">En av hello fördelarna med att integrera Swagger i Azure API apps är automatisk kodgenerering.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-124">One of hello benefits of integrating Swagger into Azure API apps is automatic code generation.</span></span> <span data-ttu-id="e5ad3-125">Genererade klientklasser gör det enklare toowrite kod som anropar en API-app.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-125">Generated client classes make it easier toowrite code that calls an API app.</span></span>

<span data-ttu-id="e5ad3-126">Du kan generera klientkod till en API-app med hjälp av Visual Studio eller hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-126">You can generate client code for an API app by using Visual Studio or from hello command line.</span></span> <span data-ttu-id="e5ad3-127">Information om hur toogenerate klienten klasser i Visual Studio för ett ASP.NET Web API-projekt finns [Kom igång med API Apps och ASP.NET](app-service-api-dotnet-get-started.md#codegen).</span><span class="sxs-lookup"><span data-stu-id="e5ad3-127">For information about how toogenerate client classes in Visual Studio for an ASP.NET Web API project, see [Get started with API Apps and ASP.NET](app-service-api-dotnet-get-started.md#codegen).</span></span> <span data-ttu-id="e5ad3-128">Information om hur toodo från hello kommandot rad för alla språk som stöds finns i hello Readme-filen för hello [Azure/autorest](https://github.com/azure/autorest) på GitHub.com.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-128">For information about how toodo it from hello command line for all supported languages, see hello readme file of hello [Azure/autorest](https://github.com/azure/autorest) repository on GitHub.com.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5ad3-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5ad3-129">Next steps</span></span>
<span data-ttu-id="e5ad3-130">En stegvis självstudiekurs som hjälper dig att skapa, distribuera och använda en API-app finns i [Kom igång med API Apps i Azure App Service](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e5ad3-130">For a step-by-step tutorial that guides you through creating, deploying, and consuming an API app, see [Get started with API Apps in Azure App Service](app-service-api-dotnet-get-started.md).</span></span>

<span data-ttu-id="e5ad3-131">Om du använder Azure API Management med API Apps, kan du använda Swagger-metadata tooimport din API i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="e5ad3-131">If you use Azure API Management with API Apps, you can use Swagger metadata tooimport your API into API Management.</span></span> <span data-ttu-id="e5ad3-132">Mer information finns i [hur tooimport hello definitionen av ett API med åtgärder i Azure API Management](../api-management/api-management-howto-import-api.md).</span><span class="sxs-lookup"><span data-stu-id="e5ad3-132">For more information, see [How tooimport hello definition of an API with operations in Azure API Management](../api-management/api-management-howto-import-api.md).</span></span> 

