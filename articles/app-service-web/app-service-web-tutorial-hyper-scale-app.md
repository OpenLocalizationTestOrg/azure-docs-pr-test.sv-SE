---
title: aaaBuild en storskaliga app i Azure | Microsoft Docs
description: "Lär dig hur toouse olika Azure-tjänster toomaximize hello prestanda för en ASP.NET-app i Azure."
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
ms.openlocfilehash: 7952647b49a82c286c6a737eb41a7f23a13fd75e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-hyper-scale-web-app-in-azure"></a>Skapa en storskaliga webbapp i Azure

Den här kursen visar hur tooscale ut en ASP.NET-webbapp i Azure toomaximize användaren begär.

Innan du börjar den här kursen ska du kontrollera att [hello Azure CLI är installerad](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) på din dator. Dessutom måste [Visual Studio](https://www.visualstudio.com/vs/) på din lokala dator toorun hello exempelprogrammet.

## <a name="step-1---get-sample-application"></a>Steg 1 – hämta exempelprogrammet
I det här steget kan du ställa in hello lokal ASP.NET-projekt.

### <a name="clone-hello-application-repository"></a>Klona hello programmet databasen

Öppna hello kommandoradsverktyget terminal önskat och `CD` tooa arbetskatalogen. Sedan kommandon kör hello följande tooclone hello exempelprogrammet. 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-hello-sample-application-in-visual-studio"></a>Köra hello exempelprogrammet i Visual Studio

Öppna hello lösningen i Visual Studio.

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

Typen `F5` toorun hello program.

Det här exemplet ASP.NET-webbprogram hämtas från hello standardmall och kvarstår användaren sessioner och använder hello utdatacachen. Ta en titt på `HighScaleApp\Controllers\HomeController.cs`. Hej `Index()` metoden lägger till en typ av data toohello session.

```csharp
Session.Add("visited", "true"); 
```

Och hello `About()` och `Contact()` metoder cachelagra sina utdata.

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-tooazure"></a>Steg 2 – distribuera tooAzure
I det här steget att skapa en Azure-webbapp och distribuera tooit ditt exempel ASP.NET-program.

### <a name="create-a-resource-group"></a>Skapa en resursgrupp   
Använd [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) toocreate en [resursgruppen](../azure-resource-manager/resource-group-overview.md) i hello Västeuropa region. En resursgrupp är där du placerar alla hello Azure-resurser som du vill toomanage tillsammans, som serverdel hello webbapp och en SQL-databas.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

toosee vilka möjliga värden du kan använda för `---location`, använda hello [az apptjänst lista-platser](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) kommando.

### <a name="create-an-app-service-plan"></a>Skapa en App Service-plan
Använd [az programtjänstplan skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) toocreate ”B1” [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

En apptjänstplan är en skalningsenhet som kan innehålla valfritt antal appar som du vill tooscale in eller ut tillsammans över hello samma App-infrastruktur. Varje plan är även tilldelad ett [prisnivån](https://azure.microsoft.com/en-us/pricing/details/app-service/). Högre nivåer omfattar bättre maskin- och fler funktioner, till exempel mer skalbart instanser.

Den här självstudien är B1 hello lägsta nivå som gör att skala ut toothree instanser. Du kan alltid flytta appen uppåt eller nedåt hello prisnivån senare genom att köra [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update). 

### <a name="create-a-web-app"></a>Skapa en webbapp
Använd [az apptjänst web skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate ett webbprogram med ett unikt namn i `$appName`.

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a>Ange autentiseringsuppgifter för distribution
Använd [az apptjänst web distributionsanvändare har angetts](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) tooset kontonivå distributionen av autentiseringsuppgifter för Apptjänst.

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a>Konfigurera Git-distribution
Använd [az apptjänst web källkontroll config-lokal-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure lokal Git-distribution.

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

Det här kommandot ger utdata som liknar hello följande:

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

Använd hello returnerade URL tooconfigure din Git fjärråtkomst. hello använder följande kommando hello föregående exempel på utdata.

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-hello-sample-application"></a>Distribuera hello exempelprogrammet
Du är nu redo toodeploy exempelprogrammet. Kör `git push`.

```powershell
git push azure master
```

När du uppmanas att ange lösenord använder hello-lösenord som du angav när du körde `az appservice web deployment user set`.

### <a name="browse-tooazure-web-app"></a>Bläddra tooAzure webbprogram
Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee köra appen live i Azure, kör det här kommandot.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-tooredis"></a>Steg 3 – ansluta tooRedis
I det här steget kan ställa du in Azure Redis-Cache som en extern, samordnade cache tooyour Azure webbapp. Du kan snabbt använda Redis toocache sidans utdata. Dessutom när du skalar upp dina webbprogram senare Redis hjälper dig att bevara användarsessioner på flera instanser på ett tillförlitligt sätt.

### <a name="create-an-azure-redis-cache"></a>Skapa en Azure Redis Cache
Använd [az redis skapa](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate för en Azure Redis-Cache och spara hello JSON-utdata. Använd ett unikt namn i `$cacheName`.

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-hello-application-toouse-redis"></a>Konfigurera hello programmet toouse Redis
Formatera hello anslutningssträngen för ditt cacheminne.

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

hello andra raden bör ge dig en utdata som ser ut så här:

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

I Visual Studio skapar du en Webbkonfigurationsfilen i projektroten kallas `redis.config` och hello klistra in följande kod till den. I `value`, använda hello anslutningssträng från hello PowerShell-utdata.

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

Om du tittar på hello `.gitignore` filen i Git-lagringsplatsen, ser du att den här filen är undantagen från källkontroll. På så sätt känslig information förblir säkra. 

Öppna `Web.config`. Meddelande hello `<appSettings file="redis.config">` element som hämtar hello-inställningen som du skapade i `redis.config`. 

Hitta hello kommenterade avsnitt som innehåller `<sessionState>` och `<caching>`. Ta bort kommentarerna i det här avsnittet.

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

Den här koden söker efter hello Redis-anslutningssträng som du har definierat i `RedisConnection`. 

Redis toomanage sessioner och cachelagring används i ditt program nu. Typen `F5` toorun hello program. Om du vill kan du hämta ett Redis-klient toovisualize hello hanteringsdata som sparas toohello cache.

### <a name="configure-hello-connection-string-in-azure"></a>Konfigurera hello anslutningssträngen i Azure

För dina program toowork i Azure måste tooconfigure hello samma Redis-anslutningssträng i ditt Azure webbapp. Eftersom `redis.config` behålls inte källkontroll, det inte är distribuerad tooAzure när du kör Git-distribution.

Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd hello anslutningssträngen med hello samma namn (`RedisConnection`).

AZ apptjänst Webbkonfiguration appsettings uppdatera--inställningar ”RedisConnection = $connstring”--name $appName--myResourceGroup för resursgruppen.

Kom ihåg att `$connstring` innehåller hello formaterad anslutningssträng.

### <a name="redeploy-hello-application-tooazure"></a>Distribuera om hello programmet tooAzure
Använda Git-kommandon toopush ändringar-tooAzure

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

När du uppmanas att ange lösenord använder hello-lösenord som du angav när du körde `az appservice web deployment user set`.

### <a name="browse-toohello-azure-web-app"></a>Bläddra toohello Azure-webbapp
Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee hello ändringar live i Azure.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-toomultiple-instances"></a>Steg 4 – skala toomultiple instanser
Hej programtjänstplanen är hello skalningsenhet för din Azure-webbappar. tooscale ut ditt webbprogram skala hello App Service-plan.

Använd [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale ut hello App Service-plan toothree instanser som är hello högsta tillåtna antalet för hello B1 prisnivån. Kom ihåg att B1 hello prisnivån som du valde när du skapade hello tidigare App Service-plan. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a>Steg 5 – skala geografiskt
Vid skalning geografiskt kör din app i flera områden av hello Azure-molnet. Den här installationen belastningsutjämnas appen ytterligare baserat på geografisk plats och minskar svarstiden för hello genom att placera din app närmare tooclient webbläsare.

I det här steget kan du skala din ASP.NET web app tooa andra region med [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/). Hello slutet av hello steg har du en webbapp som körs i Västeuropa (redan skapat) och ett webbprogram som körs i Sydostasien (har inte skapats ännu). Både appar hanteras från hello samma Traffic Manager-URL.

### <a name="scale-up-hello-europe-app-toostandard-tier"></a>Skala upp hello tooStandard Europa app-nivå
I App Service kräver integrering med Azure Traffic Manager hello Standard prisnivå. Använd [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale in tooS1 din App Service-plan. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a>Skapa en Traffic Manager-profil 
Använd [az network traffic manager-profilen skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) toocreate en Traffic Manager-profilen och lägga till den tooyour resursgruppen. Använd ett unikt DNS-namn i $dnsName.

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> `--routing-method Performance`Anger att den här profilen [dirigerar användaren trafik toohello närmaste endpoint](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="get-hello-resource-id-of-hello-europe-app"></a>Hämta hello resurs-ID för hello Europa app
Använd [az apptjänst web visa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resurs-ID för ditt webbprogram.

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-europe-app"></a>Lägg till en Traffic Manager-slutpunkt för hello Europa app
Använd [az network traffic manager-slutpunkt skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd en slutpunkt tooyour Traffic Manager-profil och Använd hello resurs-ID för ditt webbprogram som hello mål.

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-hello-traffic-manager-endpoint-url"></a>Hämta hello Traffic Manager slutpunkts-URL
Traffic Manager-profilen har nu en slutpunkt som pekar tooyour befintliga webbprogrammet. Använd [az network traffic manager-profilen visas](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) tooget URL: en. 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

Kopiera hello utdata i webbläsaren. Du bör se ditt webbprogram igen.

### <a name="create-an-azure-redis-cache-in-asia"></a>Skapa ett Azure Redis-Cache i Asien
Nu kan replikera du Azure web app toohello Sydostasien region. toostart, Använd [az redis skapa](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate andra Azure Redis-Cache i Sydostasien. Det här cacheminnet måste toobe samordnade med din app i Asien.

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

`--name $cacheName-asia`ger hello cache hello namnet på hello Västeuropa cache med hello `-asia` suffix.

### <a name="create-an-app-service-plan-in-asia"></a>Skapa en apptjänstplan i Asien
Använd [az programtjänstplan skapa](https://docs.microsoft.com/cli/azure/appservice/plan#create) toocreate en andra App Service-plan i hello Sydostasien region, med hjälp av hello samma S1 tjänstnivån som hello Västeuropa plan.

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a>Skapa en webbapp i Asien
Använd [az apptjänst web skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate andra webbprogram.

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

`--name $appName-asia`ger hello hello programnamn hello Västeuropa appen med hello `-asia` suffix.

### <a name="configure-hello-connection-string-for-redis"></a>Konfigurera hello anslutningssträngen för Redis
Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd toohello web app hello anslutningssträngen för hello Sydostasien cache.

AZ apptjänst Webbkonfiguration appsettings uppdatera--inställningar ”RedisConnection = $($redis.hostname): $($redis.sslPort), lösenord = $($redis.accessKeys.primaryKey), ssl = True, abortConnect = False”--name $appName-Asien--myResourceGroup för resursgruppen.

### <a name="configure-git-deployment-for-hello-asia-app"></a>Konfigurera Git-distribution för hello Asien app.
Använd [az apptjänst web källkontroll config-lokal-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure lokal Git-distribution för hello andra webbprogram.

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

Det här kommandot ger utdata som liknar hello följande:

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

Använd hello returnerade URL tooconfigure andra Git fjärråtkomst för din lokala databas. hello använder följande kommando hello föregående exempel på utdata.

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a>Distribuera exempelprogrammet
Kör `git push` toodeploy din exempel programmet toohello andra Git-fjärrkontroll. 

```bash
git push azure-asia master
```

När du uppmanas att ange lösenord använder hello-lösenord som du angav när du körde `az appservice web deployment user set`.

### <a name="browse-toohello-asia-app"></a>Bläddra toohello Asien app
Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) tooverify som appen körs live i Azure.

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-hello-resource-id-of-hello-asia-app"></a>Hämta hello resurs-ID för hello Asien app
Använd [az apptjänst web visa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resurs-ID för ditt webbprogram i Sydostasien.

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-asia-app"></a>Lägg till en Traffic Manager-slutpunkt för hello Asien app
Använd [az network traffic manager-slutpunkt skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd en andra slutpunkt toohello Traffic Manager-profilen.

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-tooweb-apps"></a>Lägg till region identifierare tooweb appar
Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd en regionspecifika miljövariabel.

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

Programkoden använder redan den här inställningen. Ta en titt på `HighScaleApp\Views\Home\Index.cshtml`.

### <a name="complete"></a>Slutföra!

Prova tooaccess hello URL för Traffic Manager-profilen från webbläsare i olika geografiska regioner. Klientwebbläsare från Europa ska visa ”ASP.NET västra Europa” och klientens webbläsare från Asien ska visa ”ASP.NET Sydostasien”.

## <a name="more-resources"></a>Fler resurser
