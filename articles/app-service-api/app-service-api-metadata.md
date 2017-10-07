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
# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>App Service API Apps metadata för API-identifiering och kodgenerering
Stöd för [Swagger 2.0](http://swagger.io/) API-metadata är inbyggt i API Apps i Apptjänst. Du har inte toouse Swagger, men om du använder den, kan du dra nytta av API Apps funktioner som gör identifiering och förbrukning av enklare.   

## <a name="swagger-endpoint"></a>Swagger-slutpunkt
Du kan ange en slutpunkt som innehåller Swagger 2.0 JSON-metadata för en API-app i en egenskap för hello API-app. hello endpoint kan vara relativt toohello bas-URL för hello API-app eller en absolut URL. Absoluta URL: er kan pekar utanför hello API-app. 

Många underordnade klienter (till exempel kodgenerering för Visual Studio och PowerApps ”Lägg till API” flöde), hello URL måste vara offentligt tillgänglig (som inte skyddas av användare eller autentiseringen av tjänsten). Det innebär att om du använder App Service-autentisering och vill tooexpose hello API-definition från i din app sig själv, måste toouse autentisering alternativ som tillåter anonym trafik tooreach din API. Mer information finns i [autentisering och auktorisering för API Apps i Apptjänst](app-service-api-authentication.md).

### <a name="portal-blade"></a>Portalblad
I hello [Azure-portalen](https://portal.azure.com/) hello endpoint URL kan visas och ändras på hello **API-Definition** bladet.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure Resource Manager-egenskap
Du kan också konfigurera URL för hello API-definition för en API-app med hjälp av [Resursläsaren](https://resources.azure.com/) eller [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) i kommandoradsverktyg som [Azure PowerShell](/powershell/azureps-cmdlets-docs)och hello [Azure CLI](../cli-install-nodejs.md). 

I **Resursläsaren**, gå för**prenumerationer > {din prenumeration} > resursgrupper > {din resursgrupp} > providers > Microsoft.Web > Platser > {din plats} > config > web**, ser du hello `apiDefinition` egenskapen:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Ett exempel på en Azure Resource Manager-mall som anger hello `apiDefinition` egenskapen, öppna hello [azuredeploy.JSON i hello uppgiftslista exempelprogrammet](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Hitta hello avsnitt i hello-mall som ser ut som hello JSON-exemplet ovan.

### <a name="default-value"></a>Standardvärde
När du använder Visual Studio toocreate en API-app hello API definition slutpunkten anges toohello bas-URL plus hello API-appen automatiskt `/swagger/docs/v1`. Detta är hello standard-URL som hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-paketet använder tooserve dynamiskt genererade Swagger-metadata för ett ASP.NET Web API-projekt. 

## <a name="code-generation"></a>Kodgenerering
En av hello fördelarna med att integrera Swagger i Azure API apps är automatisk kodgenerering. Genererade klientklasser gör det enklare toowrite kod som anropar en API-app.

Du kan generera klientkod till en API-app med hjälp av Visual Studio eller hello-kommandoraden. Information om hur toogenerate klienten klasser i Visual Studio för ett ASP.NET Web API-projekt finns [Kom igång med API Apps och ASP.NET](app-service-api-dotnet-get-started.md#codegen). Information om hur toodo från hello kommandot rad för alla språk som stöds finns i hello Readme-filen för hello [Azure/autorest](https://github.com/azure/autorest) på GitHub.com.

## <a name="next-steps"></a>Nästa steg
En stegvis självstudiekurs som hjälper dig att skapa, distribuera och använda en API-app finns i [Kom igång med API Apps i Azure App Service](app-service-api-dotnet-get-started.md).

Om du använder Azure API Management med API Apps, kan du använda Swagger-metadata tooimport din API i API-hantering. Mer information finns i [hur tooimport hello definitionen av ett API med åtgärder i Azure API Management](../api-management/api-management-howto-import-api.md). 

