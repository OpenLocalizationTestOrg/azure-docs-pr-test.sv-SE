---
title: "Azure Resource Manager-baserade plattformar kommandoradsverktyg för Azure Web App | Microsoft Docs"
description: "Lär dig hur du använder den nya Azure Resource Manager-baserade plattformar kommandoradsverktyg för att hantera dina Azure-webbprogram."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: d415b195-4262-416f-b59f-7e1aef200054
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: 7a03e1417617453c43edcc3787da10d171359757
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a>Med hjälp av Azure Resource Manager-baserade XPlat CLI för Azure App Service
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Nya kommandon har lagts till med lanseringen av Microsoft Azure plattformsoberoende kommandoradsverktyg version 0.10.5. Dessa kommandon ge användaren möjlighet att använda Azure Resource Manager-baserade PowerShell-kommandon för att hantera App Service.

Läs om hur du hanterar resursgrupper i [använda Azure CLI för att hantera Azure-resurser och resursgrupper](../azure-resource-manager/xplat-cli-azure-resource-manager.md). 

> [!NOTE] 
> Prova också [Azure CLI 2.0](https://github.com/Azure/azure-cli), en nästa generationens CLI som skrivits i Python för resursdistributionsmodell för hantering.
>
>

## <a name="managing-app-service-plans"></a>Hantera App Service-planer
### <a name="create-an-app-service-plan"></a>Skapa en Apptjänstplan
Använd för att skapa en apptjänstplan i **azure appserviceplan skapa** kommando.

Nedan följer beskrivningar av olika parametrar:

* **--resursgrupp**: resursgruppen som innehåller den nyligen skapade apptjänstplanen.
* **--namnet**: namn på app service-plan.
* **--plats**: plats för app service-plan.
* **--sku**: önskade prisnivå SKU: n (alternativen är: F1 (Free). D1 (delad). B1 (grundläggande liten) B2 (grundläggande medel), och B3 (grundläggande stora). S1 (Standard liten) S2 (Standard medel), och S3 (Standard är stora). P1 (liten Premium), P2 (Premium medel), och P3 (stora Premium).)
* **--instanser**: antal arbetare i appen tjänsten plan (standardvärdet är 1).

Exempel för att använda denna cmdlet:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a>Skapa en Linux-Programtjänstplan

Använder samma **azure appserviceplan skapa** kommandot med extraparametern **--islinux true**. Observera de begränsningar och regioner som beskrivs i [introduktion till App Service på Linux](app-service-linux-intro.md)

### <a name="list-existing-app-service-plans"></a>Visa en lista med befintliga App Service-planer
Om du vill visa befintliga apptjänstplaner använder **azure appserviceplan lista** kommando.

Om du vill visa en lista med alla apptjänstplaner under en viss resursgrupp, använder du:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

För att få en viss app service-plan kan använda **azure appserviceplan visa** kommando:

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfigurera en befintlig App Service-Plan
Om du vill ändra inställningarna för en befintlig app service-plan i **azure appserviceplan config** kommando. Du kan ändra SKU: n och antalet arbetare 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Skala en Apptjänstplan
Om du vill skala en befintlig App Service-Plan, använder du:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Ändra en Apptjänstplan SKU
Om du vill ändra sku av en befintlig App Service-Plan, använder du:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Ta bort en befintlig App Service-Plan
Om du vill ta bort en befintlig app service-plan måste alla tilldelade appar som ska flyttas eller tas bort först. Med hjälp av den **azure webapp ta bort** kommandot kan du ta bort app service-plan.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a>Hantera Apptjänst-appar
### <a name="create-a-web-app"></a>Skapa en webbapp
Använd för att skapa en webbapp i **azure webapp skapa** kommando.

Nedan följer beskrivningar av olika parametrar:

* **--namnet**: namn för webbappen.
* **--plan**: namn för service-plan som används som värd för webbprogrammet.
* **--resursgrupp**: resursgruppen som är värd för App service-plan.
* **--plats**: app webbplats.

Exempel för att använda denna cmdlet:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a>Ta bort en befintlig app
Om du vill ta bort en befintlig app som du kan använda den **azure webapp ta bort** kommando. Du måste ange namnet på appen och resursgruppens namn.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a>Visa en lista med befintliga appar
Om du vill visa befintliga appar använder den **azure webapp listan** kommando.

Om du vill visa en lista över alla program i en viss resursgrupp, använder du:

    azure webapp list --resource-group ContosoAzureResourceGroup

För att få en viss app måste du använda den **azure webapp visa** kommando.

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a>Konfigurera en befintlig app
Om du vill ändra inställningar och konfigurationer för en befintlig app som använder den **azure webapp konfigurationsuppsättning** kommando.

Exempel (1): ändra php-versionen av en app 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Exempel (2): lägga till eller ändra app-inställningar

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Du behöver veta vad annan konfiguration kan vara ändras, använder den **azure webapp ställas -h** kommando.

### <a name="change-the-state-of-an-existing-app"></a>Ändra tillstånd för en befintlig app
#### <a name="restart-an-app"></a>Starta om en app
Om du vill starta om en app måste du ange gruppen namn och en resurs i appen.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a>Stoppa en app
Om du vill stoppa en app måste du ange gruppen namn och en resurs i appen.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a>Starta en app
Om du vill starta en app måste du ange gruppen namn och en resurs i appen.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a>Hantera publishing profiler av en app
Varje app har en publiceringsprofil som kan användas för att publicera din kod.

#### <a name="get-publishing-profile"></a>Hämta Publicera profil
Använd följande för att hämta publiceringsprofilen för en app:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Det här kommandot ekon publishing profil användarnamn och lösenord till kommandoraden.

### <a name="manage-app-hostnames"></a>Hantera appen värdnamn
Om du vill hantera värdnamnsbindningar för din app använder det **azure webapp config värdnamn** kommando  

#### <a name="list-hostname-bindings"></a>Lista värdnamnsbindningar
Om du vill hämta de aktuella värdnamnsbindningar för en app, använder du:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Lägg till värdnamnsbindningar
Lägg till värdnamnsbindningar i en app, använder du:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Ta bort värdnamnsbindningar
Om du vill ta bort värdnamnsbindningar, använder du:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a>Nästa steg
* Läs om Azure Resource Manager CLI-stöd i [använda Azure CLI för att hantera Azure-resurser och resursgrupper.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)
* Läs om hur du hanterar App Service med PowerShell i [Using Azure Resource Manager-Based PowerShell för att hantera Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)
* Mer information om Azure App Service på Linux, se [introduktion till App Service på Linux](app-service-linux-intro.md)
