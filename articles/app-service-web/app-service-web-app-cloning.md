---
title: "aaaWeb App kloning med hjälp av PowerShell"
description: "Lär dig hur tooclone dina Web Apps toonew webbprogram med hjälp av PowerShell."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: b8882370d6db6939f8e4473ccc1414091bdcb8f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure Apptjänst-App kloning med hjälp av PowerShell
Hello-versionen av Microsoft Azure PowerShell version 1.1.0 ett nytt alternativ har lagts till i tooNew AzureRMWebApp som skulle ge hello användaren hello möjlighet tooclone en befintlig Webbapp tooa nyskapad app i en annan region eller hello samma region. Detta gör att kunder toodeploy ett antal appar över olika regioner, snabbt och enkelt.

Kloning av appen är för närvarande stöds endast för apptjänstplaner för premium-nivån. hello nya funktion använder hello samma begränsningar som Web Apps Backup-funktionen finns [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

toolearn om hur du använder Azure Resource Manager baserat Azure PowerShell-cmdlets toomanage Web Apps-Kontrollera [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Kloning av en befintlig App
Scenario: En befintlig webbapp i södra centrala USA region hello användaren vill tooclone hello innehållet tooa ny webbapp i norra centrala USA region. Detta kan åstadkommas med hjälp av hello Azure Resource Manager version av hello PowerShell cmdlet toocreate ett nytt webbprogram hello - SourceWebApp alternativet.

Att veta hello resurs gruppnamn som innehåller hello källa webbapp, vi använder hello följande PowerShell-kommandot tooget hello källa webbprogram information (i det här fallet med namnet källa webapp):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

toocreate som en ny App Service-Plan som vi kan använda kommandot New AzureRmAppServicePlan som i följande exempel hello

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Hello ny AzureRmWebApp kommandot vi kan skapa nytt webbprogram för hello i hello norra centrala USA region och koppla den tooan befintliga premiumnivån App Service-Plan kan dessutom vi använda hello samma resurs gruppen som hello källa webbapp eller definiera en ny resursgrupp , hello följande visar att:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

en befintlig webbapp inklusive alla associerade distributionsplatser hello användaren måste toouse tooclone Hej IncludeSourceWebAppSlots parameter, hello följande PowerShell-kommando visar hello använder parametern med hello ny AzureRmWebApp kommando:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

tooclone en befintlig webbapp inom hello samma region hello användaren måste en ny resursgrupp och en ny apptjänst planerar i hello toocreate samma region och sedan använda hello följande PowerShell-kommandot tooclone hello webbprogram

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a>Kloning av en befintlig App tooan Apptjänstmiljö
Scenario: En befintlig webbapp i södra centrala USA region hello användaren vill tooclone hello innehållet tooa nya web app tooan befintliga App Service miljö (ASE).

Att veta hello resurs gruppnamn som innehåller hello källa webbapp, vi använder hello följande PowerShell-kommandot tooget hello källa webbprogram information (i det här fallet med namnet källa webapp):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Att veta hello ASE namn och hello resursgruppens namn som hello ASE tillhör kan hello användaren använda hello ny AzureRmWebApp kommandot toocreate hello ny webbapp i hello befintliga ASE, hello följande visar att:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Hej platsparametern krävs på grund av toolegacy orsak, men hello gäller att skapa en app i en ASE kommer att ignoreras. 

## <a name="cloning-an-existing-app-slot"></a>Kloning av en befintlig App-plats
Scenario: hello användaren vill tooclone en befintlig Webbprogramplats tooeither ett nytt webbprogram eller en ny plats för webbprogram. hello nytt webbprogram kan vara i hello samma region som hello ursprungliga webbprogrammet plats eller i en annan region.

Att veta hello resurs gruppnamn som innehåller hello källa webbapp, vi använder följande PowerShell-kommando tooget hello källa webbprogramplatss information (i det här fallet med namnet källa webappslot) knutna tooWeb App käll-webapp hello:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

hello följande visar hur du skapar en klon av hello källa web app tooa nytt webbprogram:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Konfigurera Traffic Manager under kloning av en App
Skapa flera regioner webbprogram och konfigurera Azure Traffic Manager tooroute trafik tooall dessa webbprogram, är en n viktiga scenariot tooinsure som kunders appar är hög tillgänglighet när du klonar en befintlig webbapp som du har hello alternativet tooconnect både webbserver appar tooeither en ny traffic manager-profil eller en befintlig - medveten om att endast Azure Resource Manager-version för Traffic Manager stöds.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Skapa en ny Traffic Manager-profil under kloning av en App
Scenario: hello användaren vill tooclone en web app tooanother region, när du konfigurerar en Azure Resource Manager traffic manager-profil som innehåller både webbprogram. hello följande visar hur du skapar en klon av hello källa web app tooa nytt webbprogram när du konfigurerar en ny Traffic Manager-profil:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-tooan-existing-traffic-manager-profile"></a>Lägga till nya klonade webbprogrammet tooan befintlig trafikhanterarprofil
Scenario: hello användaren redan har en Azure Resource Manager trafikhanterarprofilen att han vill tooadd både webbappar som slutpunkter. toodo så vi måste först tooassemble hello befintlig traffic manager profil-id, måste vi hello prenumerations-id, resursgruppens namn och hello befintliga traffic manager profilnamnet.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Efter att ha hello traffic Manager-id Visar hello följande att skapa en klon av hello källa web app tooa nytt webbprogram när du lägger till dem tooan befintlig Traffic Manager-profilen:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Aktuella begränsningar
Den här funktionen är för närvarande under förhandsgranskning, vi arbetar tooadd nya funktioner över tid, hello följande lista är hello kända begränsningar för hello nuvarande version av appen kloning:

* Inställningar för automatisk skalning klonas inte
* Schemat för säkerhetskopiering inställningar klonas inte
* VNET-inställningarna klonas inte
* App Insights konfigureras inte automatiskt på hello mål för webbprogram
* Enkelt autentiseringsinställningarna klonas inte
* Kudu-tillägget klonas inte
* Tips regler klonas inte
* Databasen innehåll klonas inte

### <a name="references"></a>Referenser
* [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
* [Web App kloning med hjälp av Azure Portal](app-service-web-app-cloning-portal.md)
* [Säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md)
* [Azure Resource Manager-stöd för Azure Traffic Manager-Preview](../traffic-manager/traffic-manager-powershell-arm.md)
* [Introduktion tooApp-miljö](app-service-app-service-environment-intro.md)
* [Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md)

