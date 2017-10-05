---
title: Skapa en storskaliga app i Azure | Microsoft Docs
description: "Lär dig hur du använder olika Azure-tjänster för att maximera prestandan för en ASP.NET-app i Azure."
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: erikre
editor: 
ms.assetid: a4d49ac7-0f97-4997-84c5-cdb9c4465757
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 03/23/2017
ms.author: cephalin
ms.openlocfilehash: eac9c5b0d8d0f7802d88e6f4f27d9d23c406e025
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-hyper-scale-web-app-in-azure"></a>Skapa en storskaliga webbapp i Azure

Den här kursen visar hur du skala ut en ASP.NET-webbapp i Azure för att maximera användarförfrågningar.

Innan du börjar den här kursen ska du kontrollera att [Azure CLI är installerad](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) på din dator. Dessutom måste [Visual Studio](https://www.visualstudio.com/vs/) på den lokala datorn för att köra exempelprogrammet.

## <a name="step-1---get-sample-application"></a>Steg 1 – hämta exempelprogrammet
I det här steget kan du ställa in lokalt ASP.NET-projekt.

### <a name="clone-the-application-repository"></a>Klona lagringsplatsen för programmet

Öppna önskat kommandoradsverktyg önskat och `CD` till en arbetskatalog. Kör sedan följande kommandon för att klona exempelprogrammet. 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-the-sample-application-in-visual-studio"></a>Kör exempelprogrammet i Visual Studio

Öppna lösningen i Visual Studio.

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

Typen `F5` att köra programmet.

Det här exemplet ASP.NET-webbprogram kommer från standardmallen och kvarstår användaren sessioner och använder utdatacachen. Ta en titt på `HighScaleApp\Controllers\HomeController.cs`. Den `Index()` metoden lägger till en typ av data i sessionen.

```csharp
Session.Add("visited", "true"); 
```

Och `About()` och `Contact()` metoder cachelagra sina utdata.

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-to-azure"></a>Steg 2 – distribuera till Azure
I det här steget att skapa en Azure-webbapp och distribuera ASP.NET exempelprogrammet till den.

### <a name="create-a-resource-group"></a>Skapa en resursgrupp   
Använd [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) att skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) i Västeuropa region. En resursgrupp är där du placerar alla Azure-resurser som du vill hantera tillsammans, som serverdel för webbapp och en SQL-databas.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

Att se vilka möjliga värden du kan använda för `---location`, använda den [az apptjänst lista-platser](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) kommando.

### <a name="create-an-app-service-plan"></a>Skapa en App Service-plan
Använd [az programtjänstplan skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) att skapa en ”B1” [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

En apptjänstplan är en skalningsenhet som kan innehålla valfritt antal appar som du vill skala upp eller ut över samma App Service-infrastruktur. Varje plan är även tilldelad ett [prisnivån](https://azure.microsoft.com/en-us/pricing/details/app-service/). Högre nivåer omfattar bättre maskin- och fler funktioner, till exempel mer skalbart instanser.

B1 är den lägsta nivå som gör att skala ut till tre instanser för den här kursen. Du kan alltid flytta appen uppåt eller nedåt prisnivån senare genom att köra [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update). 

### <a name="create-a-web-app"></a>Skapa en webbapp
Använd [az apptjänst web skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) att skapa en webbapp med ett unikt namn i `$appName`.

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a>Ange autentiseringsuppgifter för distribution
Använd [az apptjänst web distributionsanvändare har angetts](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) ange dina autentiseringsuppgifter för kontonivå distribution för App Service.

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a>Konfigurera Git-distribution
Använd [az apptjänst web källkontroll config-lokal-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) att konfigurera lokal Git-distribution.

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

Det här kommandot ger utdata som liknar följande:

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

Använd returnerade URL: en för att konfigurera Git-remote. Följande kommando använder utdata från föregående exempel.

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-the-sample-application"></a>Distribuera exempelprogrammet
Du är nu redo att distribuera exempelprogrammet. Kör `git push`.

```powershell
git push azure master
```

När du uppmanas att ange lösenord använder du det lösenord som du angav när du körde `az appservice web deployment user set`.

### <a name="browse-to-azure-web-app"></a>Bläddra till Azure-webbapp
Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) kör det här kommandot för att se köra appen live i Azure.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-to-redis"></a>Steg 3 – ansluta till Redis
I det här steget kan ställa du in Azure Redis-Cache som en extern, samordnade cache till Azure webbapp. Du kan snabbt använda Redis att cachelagra sidans utdata. Dessutom när du skalar upp dina webbprogram senare Redis hjälper dig att bevara användarsessioner på flera instanser på ett tillförlitligt sätt.

### <a name="create-an-azure-redis-cache"></a>Skapa en Azure Redis Cache
Använd [az redis skapa](https://docs.microsoft.com/en-us/cli/azure/redis#create) att skapa ett Azure Redis-Cache och spara JSON-utdata. Använd ett unikt namn i `$cacheName`.

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-the-application-to-use-redis"></a>Konfigurera program att använda Redis
Formatera anslutningssträngen för ditt cacheminne.

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

Den andra raden bör ge dig en utdata som ser ut så här:

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

I Visual Studio skapar du en Webbkonfigurationsfilen i projektroten kallas `redis.config` och klistra in följande kod i den. I `value`, använder anslutningssträngen från PowerShell-utdata.

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

Om du tittar på det `.gitignore` filen i Git-lagringsplatsen, ser du att den här filen är undantagen från källkontroll. På så sätt känslig information förblir säkra. 

Öppna `Web.config`. Observera den `<appSettings file="redis.config">` element som hämtar den inställning som du skapade i `redis.config`. 

Gå till avsnittet kommenterad som innehåller `<sessionState>` och `<caching>`. Ta bort kommentarerna i det här avsnittet.

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

Den här koden söker efter Redis anslutningssträngen som du har definierat i `RedisConnection`. 

Redis används för att hantera sessioner och cachelagring i ditt program nu. Typen `F5` att köra programmet. Om du vill kan du hämta Redis management-klienten om du vill visualisera data som sparas i cacheminnet.

### <a name="configure-the-connection-string-in-azure"></a>Konfigurera anslutningssträngen i Azure

För programmet att fungera i Azure, måste du konfigurera samma Redis-anslutningssträng i ditt Azure webbapp. Eftersom `redis.config` behålls inte källkontroll, den inte har distribuerats till Azure när du kör Git-distribution.

Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) att lägga till anslutningssträngen med samma namn (`RedisConnection`).

AZ apptjänst Webbkonfiguration appsettings uppdatera--inställningar ”RedisConnection = $connstring”--name $appName--myResourceGroup för resursgruppen.

Kom ihåg att `$connstring` innehåller formaterad anslutningssträngen.

### <a name="redeploy-the-application-to-azure"></a>Distribuera programmet till Azure
Använda Git-kommandon för att skicka ändringarna till Azure

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

När du uppmanas att ange lösenord använder du det lösenord som du angav när du körde `az appservice web deployment user set`.

### <a name="browse-to-the-azure-web-app"></a>Bläddra till Azure-webbappen
Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) ska kunna se ändringarna live i Azure.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-to-multiple-instances"></a>Steg 4 – skala till flera instanser
Programtjänstplanen är skalningsenhet för din Azure-webbappar. Om du vill skala ut ditt webbprogram, kan du skala App Service-plan.

Använd [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update) ska skalas ut programtjänstplanen till tre instanser som är det maximala antalet tillåtna av B1 prisnivån. Kom ihåg att B1 prisnivå som du valde när du skapade programtjänstplanen tidigare. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a>Steg 5 – skala geografiskt
Vid skalning geografiskt kör appen i flera regioner på Azure-molnet. Den här installationen belastningsutjämnas appen ytterligare baserat på geografisk plats och minskar svarstiden genom att placera din app närmare till klientens webbläsare.

I det här steget kan du skala ditt ASP.NET-webbprogram till en andra region med [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/). I slutet av steget har du en webbapp som körs i Västeuropa (redan skapat) och ett webbprogram som körs i Sydostasien (har inte skapats ännu). Både appar hanteras från samma Traffic Manager-URL.

### <a name="scale-up-the-europe-app-to-standard-tier"></a>Skala upp appen Europa Standard-nivån
Integrering med Azure Traffic Manager kräver prisnivån Standard i App Service. Använd [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update) att skala upp din programtjänstplan till S1. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a>Skapa en Traffic Manager-profil 
Använd [az network traffic manager-profilen skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) att skapa en Traffic Manager-profilen och lägga till den i resursgruppen. Använd ett unikt DNS-namn i $dnsName.

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> `--routing-method Performance`Anger att den här profilen [dirigerar användartrafik till närmaste slutpunkten](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="get-the-resource-id-of-the-europe-app"></a>Hämta resurs-ID för appen Europa
Använd [az apptjänst web visa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) få resurs-ID för ditt webbprogram.

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-europe-app"></a>Lägg till en Traffic Manager-slutpunkt för Europa-app
Använd [az network traffic manager-slutpunkt skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) att lägga till en slutpunkt i Traffic Manager-profilen och använda resurs-ID för ditt webbprogram som mål.

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-the-traffic-manager-endpoint-url"></a>Hämta Traffic Manager slutpunkts-URL
Traffic Manager-profilen har nu en slutpunkt som pekar på din befintliga webbprogrammet. Använd [az network traffic manager-profilen visas](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) att hämta dess Webbadress. 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

Kopiera utdata i webbläsaren. Du bör se ditt webbprogram igen.

### <a name="create-an-azure-redis-cache-in-asia"></a>Skapa ett Azure Redis-Cache i Asien
Nu kan replikera du dina Azure-webbapp till Sydostasien region. Starta med [az redis skapa](https://docs.microsoft.com/en-us/cli/azure/redis#create) att skapa en andra Azure Redis-Cache i Sydostasien. Det här cacheminnet behöver inte samplaceras med din app i Asien.

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

`--name $cacheName-asia`ger cachen namn västra Europa-cache med de `-asia` suffix.

### <a name="create-an-app-service-plan-in-asia"></a>Skapa en apptjänstplan i Asien
Använd [az programtjänstplan skapa](https://docs.microsoft.com/cli/azure/appservice/plan#create) att skapa en andra programtjänstplan i Sydostasien region, med samma S1 nivå som Västeuropa planen.

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a>Skapa en webbapp i Asien
Använd [az apptjänst web skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) att skapa en andra webbprogram.

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

`--name $appName-asia`ger appen namnet på appen Västeuropa med den `-asia` suffix.

### <a name="configure-the-connection-string-for-redis"></a>Konfigurera anslutningssträngen för Redis
Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) att lägga till webbprogrammet anslutningssträngen för Sydostasien cachen.

AZ apptjänst Webbkonfiguration appsettings uppdatera--inställningar ”RedisConnection = $($redis.hostname): $($redis.sslPort), lösenord = $($redis.accessKeys.primaryKey), ssl = True, abortConnect = False”--name $appName-Asien--myResourceGroup för resursgruppen.

### <a name="configure-git-deployment-for-the-asia-app"></a>Konfigurera Git-distribution för Asien appen.
Använd [az apptjänst web källkontroll config-lokal-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) att konfigurera lokal Git-distribution för andra webbprogram.

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

Det här kommandot ger utdata som liknar följande:

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

Använd returnerade URL: en för att konfigurera en andra fjärransluten Git för lokala databasen. Följande kommando använder utdata från föregående exempel.

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a>Distribuera exempelprogrammet
Kör `git push` att distribuera exempelprogrammet till andra fjärransluten Git. 

```bash
git push azure-asia master
```

När du uppmanas att ange lösenord använder du det lösenord som du angav när du körde `az appservice web deployment user set`.

### <a name="browse-to-the-asia-app"></a>Bläddra till appen Asien
Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) att verifiera att appen körs live i Azure.

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-the-resource-id-of-the-asia-app"></a>Hämta resurs-ID för appen Asien
Använd [az apptjänst web visa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) att hämta resurs-ID för ditt webbprogram i Sydostasien.

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-asia-app"></a>Lägg till en Traffic Manager-slutpunkt för Asien appen
Använd [az network traffic manager-slutpunkt skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) att lägga till en andra slutpunkt i Traffic Manager-profilen.

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-to-web-apps"></a>Lägg till regionsidentifierare i webbprogram
Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) att lägga till en regionspecifika miljövariabel.

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

Programkoden använder redan den här inställningen. Ta en titt på `HighScaleApp\Views\Home\Index.cshtml`.

### <a name="complete"></a>Slutföra!

Nu, försök att komma åt URL-Adressen för din Traffic Manager-profil från webbläsare i olika geografiska regioner. Klientwebbläsare från Europa ska visa ”ASP.NET västra Europa” och klientens webbläsare från Asien ska visa ”ASP.NET Sydostasien”.

## <a name="more-resources"></a>Fler resurser
