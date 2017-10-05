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
# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>App Service API Apps metadata för API-identifiering och kodgenerering
Stöd för [Swagger 2.0](http://swagger.io/) API-metadata är inbyggt i API Apps i Apptjänst. Du behöver använda Swagger, men om du använder den, kan du dra nytta av API Apps funktioner som gör identifiering och förbrukning av enklare.   

## <a name="swagger-endpoint"></a>Swagger-slutpunkt
Du kan ange en slutpunkt som innehåller Swagger 2.0 JSON-metadata för en API-app i en egenskap för API-app. Slutpunkten kan vara i förhållande till bas-URL för API-appen eller en absolut URL. Absoluta URL: er kan pekar utanför API-app. 

Många underordnade klienter (till exempel kodgenerering för Visual Studio och PowerApps ”Lägg till API” flöde), URL-Adressen måste vara allmänt tillgänglig (som inte skyddas av användare eller autentiseringen av tjänsten). Det innebär att om du använder App Service-autentisering och vill exponera API-definition i din app sig själv, du behöver använda autentiseringsalternativet som tillåter anonym trafik till ditt API. Mer information finns i [autentisering och auktorisering för API Apps i Apptjänst](app-service-api-authentication.md).

### <a name="portal-blade"></a>Portalblad
I den [Azure-portalen](https://portal.azure.com/) slutpunkten URL kan visas och ändras på den **API-Definition** bladet.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure Resource Manager-egenskap
Du kan också konfigurera URL: en för API-definition för en API-app med hjälp av [Resursläsaren](https://resources.azure.com/) eller [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) i kommandoradsverktyg som [Azure PowerShell](/powershell/azureps-cmdlets-docs)och [Azure CLI](../cli-install-nodejs.md). 

I **Resursläsaren**, gå till **prenumerationer > {din prenumeration} > resursgrupper > {din resursgrupp} > providers > Microsoft.Web > Platser > {webbplatsen} > config > web** , ser du den `apiDefinition` egenskapen:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Ett exempel på en Azure Resource Manager-mall som anger den `apiDefinition` egenskap, öppna den [azuredeploy.JSON i att göra-lista exempelprogrammet](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Hitta avsnitt i mallen som ser ut som JSON-exemplet ovan.

### <a name="default-value"></a>Standardvärde
När du använder Visual Studio för att skapa en API-app, API-definition slutpunkten anges automatiskt till bas-URL för API-appen plus `/swagger/docs/v1`. Detta är den standard-URL som den [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-paketet använder för att hantera dynamiskt genererade Swagger-metadata för ett ASP.NET Web API-projekt. 

## <a name="code-generation"></a>Kodgenerering
En av fördelarna med att integrera Swagger i Azure API apps är automatisk kodgenerering. Genererade klientklasser gör det enklare att skriva kod som anropar en API-app.

Du kan generera klientkod till en API-app med hjälp av Visual Studio eller från kommandoraden. Information om hur du genererar klientklasser i Visual Studio för ett ASP.NET Web API-projekt finns [Kom igång med API Apps och ASP.NET](app-service-api-dotnet-get-started.md#codegen). Information om hur du gör det från kommandoraden för alla språk som stöds finns i filen Viktigt för de [Azure/autorest](https://github.com/azure/autorest) på GitHub.com.

## <a name="next-steps"></a>Nästa steg
En stegvis självstudiekurs som hjälper dig att skapa, distribuera och använda en API-app finns i [Kom igång med API Apps i Azure App Service](app-service-api-dotnet-get-started.md).

Om du använder Azure API Management med API Apps, kan du använda Swagger-metadata för att importera din API i API-hantering. Mer information finns i [importera definitionen av ett API med åtgärder i Azure API Management](../api-management/api-management-howto-import-api.md). 

