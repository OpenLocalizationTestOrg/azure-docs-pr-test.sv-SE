---
title: "aaaAzure Resource Manager-baserade plattformar kommandoradsverktyg för Azure Web App | Microsoft Docs"
description: "Lär dig hur hello toouse nya Azure Resource Manager-baserade plattformar kommandoradsverktyg toomanage Azure Web Apps."
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
ms.openlocfilehash: 5f5e03edcb01154aef3bd220cd27358f69656ef4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a>Med hjälp av Azure Resource Manager-baserade XPlat CLI för Azure App Service
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Nya kommandon har lagts till med hello-versionen av Microsoft Azure plattformsoberoende kommandoradsverktyg version 0.10.5. De här kommandona ger hello användaren hello möjlighet toouse Azure Resource Manager-baserade PowerShell-kommandon toomanage Apptjänst.

toolearn om hur du hanterar resursgrupper, se [använda hello Azure CLI toomanage Azure resurser och resursgrupper](../azure-resource-manager/xplat-cli-azure-resource-manager.md). 

> [!NOTE] 
> Prova också [Azure CLI 2.0](https://github.com/Azure/azure-cli), en nästa generationens CLI som skrivits i Python för hello resursdistributionsmodell för hantering.
>
>

## <a name="managing-app-service-plans"></a>Hantera App Service-planer
### <a name="create-an-app-service-plan"></a>Skapa en Apptjänstplan
toocreate en apptjänstplan använda hello **azure appserviceplan skapa** kommando.

Nedan följer beskrivningar av hello olika parametrar:

* **--resursgrupp**: resursgruppen som innehåller hello nyskapad app service-plan.
* **--namnet**: namn på hello app service-plan.
* **--plats**: plats för app service-plan.
* **--sku**: hello önskad prisnivå sku (hello alternativ är: F1 (Free). D1 (delad). B1 (grundläggande liten) B2 (grundläggande medel), och B3 (grundläggande stora). S1 (Standard liten) S2 (Standard medel), och S3 (Standard är stora). P1 (liten Premium), P2 (Premium medel), och P3 (stora Premium).)
* **--instanser**: hello antalet arbetare i hello apptjänstplan (standardvärdet är 1).

Exempel toouse denna cmdlet:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a>Skapa en Linux-Programtjänstplan

Med hjälp av hello samma **azure appserviceplan skapa** kommandot med hello extraparametern **--islinux true**. Observera hello begränsningar och regioner som beskrivs i [introduktion tooApp-tjänsten på Linux](app-service-linux-intro.md)

### <a name="list-existing-app-service-plans"></a>Visa en lista med befintliga App Service-planer
toolist hello befintliga apptjänstplaner, Använd **azure appserviceplan lista** kommando.

toolist alla apptjänstplaner under en viss resursgrupp, Använd:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Använd tooget en viss app service-plan **azure appserviceplan visa** kommando:

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfigurera en befintlig App Service-Plan
toochange hello inställningar för en befintlig app service-plan, använda hello **azure appserviceplan config** kommando. Du kan ändra hello sku och hello antalet arbetare 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Skala en Apptjänstplan
Använd en befintlig App Service-Plan, tooscale:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-hello-sku-of-an-app-service-plan"></a>Ändra hello SKU av en App Service-Plan
toochange hello sku av en befintlig App Service-Plan, Använd:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Ta bort en befintlig App Service-Plan
toodelete en befintlig app service-plan, tilldelas alla appar behöver toobe flyttat eller tagit bort först. Med hello **azure webapp ta bort** kommandot kan du ta bort hello app service-plan.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a>Hantera Apptjänst-appar
### <a name="create-a-web-app"></a>Skapa en webbapp
toocreate en webbapp använder hello **azure webapp skapa** kommando.

Nedan följer beskrivningar av hello olika parametrar:

* **--namnet**: namn för hello webbprogrammet.
* **--plan**: namn för hello serviceplan toohost hello-webbprogram.
* **--resursgrupp**: resursgruppen som är värd för hello App service-plan.
* **--plats**: hello app webbplats.

Exempel toouse denna cmdlet:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a>Ta bort en befintlig app
toodelete en befintlig app som du kan använda hello **azure webapp ta bort** kommando. Du behöver toospecify hello namnet på hello app och hello resursgruppens namn.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a>Visa en lista med befintliga appar
toolist hello befintliga appar använder hello **azure webapp listan** kommando.

toolist Använd för alla program i en viss resursgrupp:

    azure webapp list --resource-group ContosoAzureResourceGroup

tooget en viss app använder hello **azure webapp visa** kommando.

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a>Konfigurera en befintlig app
toochange hello inställningar och konfigurationer för en befintlig app använder hello **azure webapp konfigurationsuppsättning** kommando.

Exempel (1): ändra hello php-versionen av en app 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Exempel (2): lägga till eller ändra app-inställningar

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

tooknow vilka andra konfigurationsobjekt kan ändras, Använd hello **azure webapp ställas -h** kommando.

### <a name="change-hello-state-of-an-existing-app"></a>Ändra hello tillståndet för en befintlig app
#### <a name="restart-an-app"></a>Starta om en app
Du måste ange hello namn och resurs grupp hello app toorestart en app.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a>Stoppa en app
Du måste ange hello namn och resurs grupp hello app toostop en app.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a>Starta en app
Du måste ange hello namn och resurs grupp hello app toostart en app.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a>Hantera publishing profiler av en app
Varje app har en publiceringsprofil som kan använda toopublish din kod.

#### <a name="get-publishing-profile"></a>Hämta Publicera profil
tooget hello Publicera profil för en app, Använd:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Det här kommandot ekon hello Publicera profil användarnamn och lösenord toohello kommandoraden.

### <a name="manage-app-hostnames"></a>Hantera appen värdnamn
toomanage värdnamnsbindningar för din app använder hello **azure webapp config värdnamn** kommando  

#### <a name="list-hostname-bindings"></a>Lista värdnamnsbindningar
tooget hello aktuella värdnamnsbindningar för en app, Använd:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Lägg till värdnamnsbindningar
tooadd hostname bindningar tooan appen, Använd:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Ta bort värdnamnsbindningar
toodelete värdnamnsbindningar, Använd:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a>Nästa steg
* toolearn om Azure Resource Manager CLI-stöd finns [använda hello Azure CLI toomanage Azure resurser och resursgrupper.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)
* toolearn om hur du hanterar App Service med PowerShell, se [Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)
* toolearn om Azure App Service på Linux, se [introduktion tooApp-tjänsten på Linux](app-service-linux-intro.md)
