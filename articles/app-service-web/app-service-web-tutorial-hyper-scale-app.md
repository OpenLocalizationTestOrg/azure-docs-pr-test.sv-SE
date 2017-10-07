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
# <a name="build-a-hyper-scale-web-app-in-azure"></a><span data-ttu-id="e1b57-103">Skapa en storskaliga webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="e1b57-103">Build a hyper-scale web app in Azure</span></span>

<span data-ttu-id="e1b57-104">Den här kursen visar hur tooscale ut en ASP.NET-webbapp i Azure toomaximize användaren begär.</span><span class="sxs-lookup"><span data-stu-id="e1b57-104">This tutorial shows you how tooscale out an ASP.NET web app in Azure toomaximize user requests.</span></span>

<span data-ttu-id="e1b57-105">Innan du börjar den här kursen ska du kontrollera att [hello Azure CLI är installerad](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) på din dator.</span><span class="sxs-lookup"><span data-stu-id="e1b57-105">Before starting this tutorial, ensure that [hello Azure CLI is installed](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span> <span data-ttu-id="e1b57-106">Dessutom måste [Visual Studio](https://www.visualstudio.com/vs/) på din lokala dator toorun hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e1b57-106">In addition, you need [Visual Studio](https://www.visualstudio.com/vs/) on your local machine toorun hello sample application.</span></span>

## <a name="step-1---get-sample-application"></a><span data-ttu-id="e1b57-107">Steg 1 – hämta exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="e1b57-107">Step 1 - Get sample application</span></span>
<span data-ttu-id="e1b57-108">I det här steget kan du ställa in hello lokal ASP.NET-projekt.</span><span class="sxs-lookup"><span data-stu-id="e1b57-108">In this step, you set up hello local ASP.NET project.</span></span>

### <a name="clone-hello-application-repository"></a><span data-ttu-id="e1b57-109">Klona hello programmet databasen</span><span class="sxs-lookup"><span data-stu-id="e1b57-109">Clone hello application repository</span></span>

<span data-ttu-id="e1b57-110">Öppna hello kommandoradsverktyget terminal önskat och `CD` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="e1b57-110">Open hello command-line terminal of your choice and `CD` tooa working directory.</span></span> <span data-ttu-id="e1b57-111">Sedan kommandon kör hello följande tooclone hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e1b57-111">Then, run hello following commands tooclone hello sample application.</span></span> 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-hello-sample-application-in-visual-studio"></a><span data-ttu-id="e1b57-112">Köra hello exempelprogrammet i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1b57-112">Run hello sample application in Visual Studio</span></span>

<span data-ttu-id="e1b57-113">Öppna hello lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e1b57-113">Open hello solution in Visual Studio.</span></span>

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

<span data-ttu-id="e1b57-114">Typen `F5` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="e1b57-114">Type `F5` toorun hello application.</span></span>

<span data-ttu-id="e1b57-115">Det här exemplet ASP.NET-webbprogram hämtas från hello standardmall och kvarstår användaren sessioner och använder hello utdatacachen.</span><span class="sxs-lookup"><span data-stu-id="e1b57-115">This sample ASP.NET web application comes from hello default template, and persists user sessions and uses hello output cache.</span></span> <span data-ttu-id="e1b57-116">Ta en titt på `HighScaleApp\Controllers\HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-116">Take a look at `HighScaleApp\Controllers\HomeController.cs`.</span></span> <span data-ttu-id="e1b57-117">Hej `Index()` metoden lägger till en typ av data toohello session.</span><span class="sxs-lookup"><span data-stu-id="e1b57-117">hello `Index()` method adds a piece of data toohello session.</span></span>

```csharp
Session.Add("visited", "true"); 
```

<span data-ttu-id="e1b57-118">Och hello `About()` och `Contact()` metoder cachelagra sina utdata.</span><span class="sxs-lookup"><span data-stu-id="e1b57-118">And hello `About()` and `Contact()` methods cache their output.</span></span>

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-tooazure"></a><span data-ttu-id="e1b57-119">Steg 2 – distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="e1b57-119">Step 2 - Deploy tooAzure</span></span>
<span data-ttu-id="e1b57-120">I det här steget att skapa en Azure-webbapp och distribuera tooit ditt exempel ASP.NET-program.</span><span class="sxs-lookup"><span data-stu-id="e1b57-120">In this step, you create an Azure web app and deploy your sample ASP.NET application tooit.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="e1b57-121">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e1b57-121">Create a resource group</span></span>   
<span data-ttu-id="e1b57-122">Använd [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) toocreate en [resursgruppen](../azure-resource-manager/resource-group-overview.md) i hello Västeuropa region.</span><span class="sxs-lookup"><span data-stu-id="e1b57-122">Use [az group create](https://docs.microsoft.com/cli/azure/group#create) toocreate a [resource group](../azure-resource-manager/resource-group-overview.md) in hello West Europe region.</span></span> <span data-ttu-id="e1b57-123">En resursgrupp är där du placerar alla hello Azure-resurser som du vill toomanage tillsammans, som serverdel hello webbapp och en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e1b57-123">A resource group is where you put all hello Azure resources that you want toomanage together, such as hello web app and any SQL Database back end.</span></span>

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

<span data-ttu-id="e1b57-124">toosee vilka möjliga värden du kan använda för `---location`, använda hello [az apptjänst lista-platser](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) kommando.</span><span class="sxs-lookup"><span data-stu-id="e1b57-124">toosee what possible values you can use for `---location`, use hello [az appservice list-locations](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="e1b57-125">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="e1b57-125">Create an App Service plan</span></span>
<span data-ttu-id="e1b57-126">Använd [az programtjänstplan skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) toocreate ”B1” [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e1b57-126">Use [az appservice plan create](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) toocreate a "B1" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

<span data-ttu-id="e1b57-127">En apptjänstplan är en skalningsenhet som kan innehålla valfritt antal appar som du vill tooscale in eller ut tillsammans över hello samma App-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="e1b57-127">An App Service plan is a scale unit, which can include any number of apps that you want tooscale up or out together over hello same App Service infrastructure.</span></span> <span data-ttu-id="e1b57-128">Varje plan är även tilldelad ett [prisnivån](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="e1b57-128">Each plan is also assigned a [pricing tier](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span></span> <span data-ttu-id="e1b57-129">Högre nivåer omfattar bättre maskin- och fler funktioner, till exempel mer skalbart instanser.</span><span class="sxs-lookup"><span data-stu-id="e1b57-129">Higher tiers include better hardware and more features, such as more scale-out instances.</span></span>

<span data-ttu-id="e1b57-130">Den här självstudien är B1 hello lägsta nivå som gör att skala ut toothree instanser.</span><span class="sxs-lookup"><span data-stu-id="e1b57-130">For this tutorial, B1 is hello minimum tier that enables scale out toothree instances.</span></span> <span data-ttu-id="e1b57-131">Du kan alltid flytta appen uppåt eller nedåt hello prisnivån senare genom att köra [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span><span class="sxs-lookup"><span data-stu-id="e1b57-131">You can always move your app up or down hello pricing tier later by running [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span></span> 

### <a name="create-a-web-app"></a><span data-ttu-id="e1b57-132">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="e1b57-132">Create a web app</span></span>
<span data-ttu-id="e1b57-133">Använd [az apptjänst web skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate ett webbprogram med ett unikt namn i `$appName`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-133">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate a web app with a unique name in `$appName`.</span></span>

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a><span data-ttu-id="e1b57-134">Ange autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="e1b57-134">Set deployment credentials</span></span>
<span data-ttu-id="e1b57-135">Använd [az apptjänst web distributionsanvändare har angetts](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) tooset kontonivå distributionen av autentiseringsuppgifter för Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="e1b57-135">Use [az appservice web deployment user set](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) tooset your account-level deployment credentials for App Service.</span></span>

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a><span data-ttu-id="e1b57-136">Konfigurera Git-distribution</span><span class="sxs-lookup"><span data-stu-id="e1b57-136">Configure Git deployment</span></span>
<span data-ttu-id="e1b57-137">Använd [az apptjänst web källkontroll config-lokal-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure lokal Git-distribution.</span><span class="sxs-lookup"><span data-stu-id="e1b57-137">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure local Git deployment.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

<span data-ttu-id="e1b57-138">Det här kommandot ger utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="e1b57-138">This command gives you an output that looks like hello following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="e1b57-139">Använd hello returnerade URL tooconfigure din Git fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="e1b57-139">Use hello returned URL tooconfigure your Git remote.</span></span> <span data-ttu-id="e1b57-140">hello använder följande kommando hello föregående exempel på utdata.</span><span class="sxs-lookup"><span data-stu-id="e1b57-140">hello following command uses hello preceding output example.</span></span>

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-hello-sample-application"></a><span data-ttu-id="e1b57-141">Distribuera hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="e1b57-141">Deploy hello sample application</span></span>
<span data-ttu-id="e1b57-142">Du är nu redo toodeploy exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e1b57-142">You are now ready toodeploy your sample application.</span></span> <span data-ttu-id="e1b57-143">Kör `git push`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-143">Run `git push`.</span></span>

```powershell
git push azure master
```

<span data-ttu-id="e1b57-144">När du uppmanas att ange lösenord använder hello-lösenord som du angav när du körde `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-144">When prompted for password, use hello password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-tooazure-web-app"></a><span data-ttu-id="e1b57-145">Bläddra tooAzure webbprogram</span><span class="sxs-lookup"><span data-stu-id="e1b57-145">Browse tooAzure web app</span></span>
<span data-ttu-id="e1b57-146">Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee köra appen live i Azure, kör det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="e1b57-146">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee your app running live in Azure, run this command.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-tooredis"></a><span data-ttu-id="e1b57-147">Steg 3 – ansluta tooRedis</span><span class="sxs-lookup"><span data-stu-id="e1b57-147">Step 3 - Connect tooRedis</span></span>
<span data-ttu-id="e1b57-148">I det här steget kan ställa du in Azure Redis-Cache som en extern, samordnade cache tooyour Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="e1b57-148">In this step, you set up Azure Redis Cache as an external, colocated cache tooyour Azure web app.</span></span> <span data-ttu-id="e1b57-149">Du kan snabbt använda Redis toocache sidans utdata.</span><span class="sxs-lookup"><span data-stu-id="e1b57-149">You can quickly utilize Redis toocache your page output.</span></span> <span data-ttu-id="e1b57-150">Dessutom när du skalar upp dina webbprogram senare Redis hjälper dig att bevara användarsessioner på flera instanser på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="e1b57-150">In addition, when you scale out your web apps later, Redis helps you persist user sessions across multiple instances reliably.</span></span>

### <a name="create-an-azure-redis-cache"></a><span data-ttu-id="e1b57-151">Skapa en Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="e1b57-151">Create an Azure Redis Cache</span></span>
<span data-ttu-id="e1b57-152">Använd [az redis skapa](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate för en Azure Redis-Cache och spara hello JSON-utdata.</span><span class="sxs-lookup"><span data-stu-id="e1b57-152">Use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate an Azure Redis Cache and save hello JSON output.</span></span> <span data-ttu-id="e1b57-153">Använd ett unikt namn i `$cacheName`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-153">Use a unique name in `$cacheName`.</span></span>

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-hello-application-toouse-redis"></a><span data-ttu-id="e1b57-154">Konfigurera hello programmet toouse Redis</span><span class="sxs-lookup"><span data-stu-id="e1b57-154">Configure hello application toouse Redis</span></span>
<span data-ttu-id="e1b57-155">Formatera hello anslutningssträngen för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="e1b57-155">Format hello connection string for your cache.</span></span>

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

<span data-ttu-id="e1b57-156">hello andra raden bör ge dig en utdata som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="e1b57-156">hello second line should give you an output that looks like this:</span></span>

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

<span data-ttu-id="e1b57-157">I Visual Studio skapar du en Webbkonfigurationsfilen i projektroten kallas `redis.config` och hello klistra in följande kod till den.</span><span class="sxs-lookup"><span data-stu-id="e1b57-157">In Visual Studio, create a web configuration file in your project root called `redis.config` and paste hello following code into it.</span></span> <span data-ttu-id="e1b57-158">I `value`, använda hello anslutningssträng från hello PowerShell-utdata.</span><span class="sxs-lookup"><span data-stu-id="e1b57-158">In `value`, use hello connection string from hello PowerShell output.</span></span>

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

<span data-ttu-id="e1b57-159">Om du tittar på hello `.gitignore` filen i Git-lagringsplatsen, ser du att den här filen är undantagen från källkontroll.</span><span class="sxs-lookup"><span data-stu-id="e1b57-159">If you look at hello `.gitignore` file in your Git repository, you'll see that this file is excluded from source control.</span></span> <span data-ttu-id="e1b57-160">På så sätt känslig information förblir säkra.</span><span class="sxs-lookup"><span data-stu-id="e1b57-160">That way your sensitive information is kept safe.</span></span> 

<span data-ttu-id="e1b57-161">Öppna `Web.config`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-161">Open `Web.config`.</span></span> <span data-ttu-id="e1b57-162">Meddelande hello `<appSettings file="redis.config">` element som hämtar hello-inställningen som du skapade i `redis.config`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-162">Notice hello `<appSettings file="redis.config">` element, which gets hello setting you created in `redis.config`.</span></span> 

<span data-ttu-id="e1b57-163">Hitta hello kommenterade avsnitt som innehåller `<sessionState>` och `<caching>`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-163">Find hello commented section that includes `<sessionState>` and `<caching>`.</span></span> <span data-ttu-id="e1b57-164">Ta bort kommentarerna i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e1b57-164">Uncomment this section.</span></span>

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

<span data-ttu-id="e1b57-165">Den här koden söker efter hello Redis-anslutningssträng som du har definierat i `RedisConnection`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-165">This code looks for hello Redis connection string you defined in `RedisConnection`.</span></span> 

<span data-ttu-id="e1b57-166">Redis toomanage sessioner och cachelagring används i ditt program nu.</span><span class="sxs-lookup"><span data-stu-id="e1b57-166">Your application now uses Redis toomanage sessions and caching.</span></span> <span data-ttu-id="e1b57-167">Typen `F5` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="e1b57-167">Type `F5` toorun hello application.</span></span> <span data-ttu-id="e1b57-168">Om du vill kan du hämta ett Redis-klient toovisualize hello hanteringsdata som sparas toohello cache.</span><span class="sxs-lookup"><span data-stu-id="e1b57-168">If you like, you can download a Redis management client toovisualize hello data that is now saved toohello cache.</span></span>

### <a name="configure-hello-connection-string-in-azure"></a><span data-ttu-id="e1b57-169">Konfigurera hello anslutningssträngen i Azure</span><span class="sxs-lookup"><span data-stu-id="e1b57-169">Configure hello connection string in Azure</span></span>

<span data-ttu-id="e1b57-170">För dina program toowork i Azure måste tooconfigure hello samma Redis-anslutningssträng i ditt Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="e1b57-170">For your application toowork in Azure, you need tooconfigure hello same Redis connection string in your Azure web app.</span></span> <span data-ttu-id="e1b57-171">Eftersom `redis.config` behålls inte källkontroll, det inte är distribuerad tooAzure när du kör Git-distribution.</span><span class="sxs-lookup"><span data-stu-id="e1b57-171">Since `redis.config` is not maintained in source control, it is not deployed tooAzure when you run Git deployment.</span></span>

<span data-ttu-id="e1b57-172">Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd hello anslutningssträngen med hello samma namn (`RedisConnection`).</span><span class="sxs-lookup"><span data-stu-id="e1b57-172">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd hello connection string with hello same name (`RedisConnection`).</span></span>

<span data-ttu-id="e1b57-173">AZ apptjänst Webbkonfiguration appsettings uppdatera--inställningar ”RedisConnection = $connstring”--name $appName--myResourceGroup för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e1b57-173">az appservice web config appsettings update --settings "RedisConnection=$connstring" --name $appName --resource-group myResourceGroup</span></span>

<span data-ttu-id="e1b57-174">Kom ihåg att `$connstring` innehåller hello formaterad anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="e1b57-174">Remember that `$connstring` contains hello formatted connection string.</span></span>

### <a name="redeploy-hello-application-tooazure"></a><span data-ttu-id="e1b57-175">Distribuera om hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="e1b57-175">Redeploy hello application tooAzure</span></span>
<span data-ttu-id="e1b57-176">Använda Git-kommandon toopush ändringar-tooAzure</span><span class="sxs-lookup"><span data-stu-id="e1b57-176">Use Git commands toopush your changes tooAzure</span></span>

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

<span data-ttu-id="e1b57-177">När du uppmanas att ange lösenord använder hello-lösenord som du angav när du körde `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-177">When prompted for password, use hello password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="e1b57-178">Bläddra toohello Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="e1b57-178">Browse toohello Azure web app</span></span>
<span data-ttu-id="e1b57-179">Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee hello ändringar live i Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b57-179">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee hello changes live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-toomultiple-instances"></a><span data-ttu-id="e1b57-180">Steg 4 – skala toomultiple instanser</span><span class="sxs-lookup"><span data-stu-id="e1b57-180">Step 4 - Scale toomultiple instances</span></span>
<span data-ttu-id="e1b57-181">Hej programtjänstplanen är hello skalningsenhet för din Azure-webbappar.</span><span class="sxs-lookup"><span data-stu-id="e1b57-181">hello App Service plan is hello scale unit for your Azure web apps.</span></span> <span data-ttu-id="e1b57-182">tooscale ut ditt webbprogram skala hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="e1b57-182">tooscale out your web app, you scale hello App Service plan.</span></span>

<span data-ttu-id="e1b57-183">Använd [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale ut hello App Service-plan toothree instanser som är hello högsta tillåtna antalet för hello B1 prisnivån.</span><span class="sxs-lookup"><span data-stu-id="e1b57-183">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale out hello App Service plan toothree instances, which is hello maximum number allowed by hello B1 pricing tier.</span></span> <span data-ttu-id="e1b57-184">Kom ihåg att B1 hello prisnivån som du valde när du skapade hello tidigare App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="e1b57-184">Remember that B1 is hello pricing tier that you chose when you created hello App Service plan earlier.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a><span data-ttu-id="e1b57-185">Steg 5 – skala geografiskt</span><span class="sxs-lookup"><span data-stu-id="e1b57-185">Step 5 - Scale geographically</span></span>
<span data-ttu-id="e1b57-186">Vid skalning geografiskt kör din app i flera områden av hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="e1b57-186">When scaling geographically, you run your app in multiple regions of hello Azure cloud.</span></span> <span data-ttu-id="e1b57-187">Den här installationen belastningsutjämnas appen ytterligare baserat på geografisk plats och minskar svarstiden för hello genom att placera din app närmare tooclient webbläsare.</span><span class="sxs-lookup"><span data-stu-id="e1b57-187">This setup load-balances your app further based on geography and lowers hello response time by placing your app closer tooclient browsers.</span></span>

<span data-ttu-id="e1b57-188">I det här steget kan du skala din ASP.NET web app tooa andra region med [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span><span class="sxs-lookup"><span data-stu-id="e1b57-188">In this step, you scale your ASP.NET web app tooa second region with [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span></span> <span data-ttu-id="e1b57-189">Hello slutet av hello steg har du en webbapp som körs i Västeuropa (redan skapat) och ett webbprogram som körs i Sydostasien (har inte skapats ännu).</span><span class="sxs-lookup"><span data-stu-id="e1b57-189">At hello end of hello step, you will have a web app running in West Europe (already created) and a web app running in Southeast Asia (not yet created).</span></span> <span data-ttu-id="e1b57-190">Både appar hanteras från hello samma Traffic Manager-URL.</span><span class="sxs-lookup"><span data-stu-id="e1b57-190">Both apps will be served from hello same Traffic Manager URL.</span></span>

### <a name="scale-up-hello-europe-app-toostandard-tier"></a><span data-ttu-id="e1b57-191">Skala upp hello tooStandard Europa app-nivå</span><span class="sxs-lookup"><span data-stu-id="e1b57-191">Scale up hello Europe app tooStandard tier</span></span>
<span data-ttu-id="e1b57-192">I App Service kräver integrering med Azure Traffic Manager hello Standard prisnivå.</span><span class="sxs-lookup"><span data-stu-id="e1b57-192">In App Service, integration with Azure Traffic Manager requires hello Standard pricing tier.</span></span> <span data-ttu-id="e1b57-193">Använd [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale in tooS1 din App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="e1b57-193">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale up your App Service plan tooS1.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="e1b57-194">Skapa en Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="e1b57-194">Create a Traffic Manager profile</span></span> 
<span data-ttu-id="e1b57-195">Använd [az network traffic manager-profilen skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) toocreate en Traffic Manager-profilen och lägga till den tooyour resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e1b57-195">Use [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) toocreate a Traffic Manager profile and add it tooyour resource group.</span></span> <span data-ttu-id="e1b57-196">Använd ett unikt DNS-namn i $dnsName.</span><span class="sxs-lookup"><span data-stu-id="e1b57-196">Use a unique DNS name in $dnsName.</span></span>

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> <span data-ttu-id="e1b57-197">`--routing-method Performance`Anger att den här profilen [dirigerar användaren trafik toohello närmaste endpoint](../traffic-manager/traffic-manager-routing-methods.md).</span><span class="sxs-lookup"><span data-stu-id="e1b57-197">`--routing-method Performance` specifies that this profile [routes user traffic toohello closest endpoint](../traffic-manager/traffic-manager-routing-methods.md).</span></span>

### <a name="get-hello-resource-id-of-hello-europe-app"></a><span data-ttu-id="e1b57-198">Hämta hello resurs-ID för hello Europa app</span><span class="sxs-lookup"><span data-stu-id="e1b57-198">Get hello resource ID of hello Europe app</span></span>
<span data-ttu-id="e1b57-199">Använd [az apptjänst web visa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resurs-ID för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e1b57-199">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resource ID of your web app.</span></span>

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-europe-app"></a><span data-ttu-id="e1b57-200">Lägg till en Traffic Manager-slutpunkt för hello Europa app</span><span class="sxs-lookup"><span data-stu-id="e1b57-200">Add a Traffic Manager endpoint for hello Europe app</span></span>
<span data-ttu-id="e1b57-201">Använd [az network traffic manager-slutpunkt skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd en slutpunkt tooyour Traffic Manager-profil och Använd hello resurs-ID för ditt webbprogram som hello mål.</span><span class="sxs-lookup"><span data-stu-id="e1b57-201">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd an endpoint tooyour Traffic Manager profile and use hello resource ID of your web app as hello target.</span></span>

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-hello-traffic-manager-endpoint-url"></a><span data-ttu-id="e1b57-202">Hämta hello Traffic Manager slutpunkts-URL</span><span class="sxs-lookup"><span data-stu-id="e1b57-202">Get hello Traffic Manager endpoint URL</span></span>
<span data-ttu-id="e1b57-203">Traffic Manager-profilen har nu en slutpunkt som pekar tooyour befintliga webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e1b57-203">Your Traffic Manager profile now has an endpoint that points tooyour existing web app.</span></span> <span data-ttu-id="e1b57-204">Använd [az network traffic manager-profilen visas](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) tooget URL: en.</span><span class="sxs-lookup"><span data-stu-id="e1b57-204">Use [az network traffic-manager profile show](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) tooget its URL.</span></span> 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

<span data-ttu-id="e1b57-205">Kopiera hello utdata i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="e1b57-205">Copy hello output into your browser.</span></span> <span data-ttu-id="e1b57-206">Du bör se ditt webbprogram igen.</span><span class="sxs-lookup"><span data-stu-id="e1b57-206">You should see your web app again.</span></span>

### <a name="create-an-azure-redis-cache-in-asia"></a><span data-ttu-id="e1b57-207">Skapa ett Azure Redis-Cache i Asien</span><span class="sxs-lookup"><span data-stu-id="e1b57-207">Create an Azure Redis Cache in Asia</span></span>
<span data-ttu-id="e1b57-208">Nu kan replikera du Azure web app toohello Sydostasien region.</span><span class="sxs-lookup"><span data-stu-id="e1b57-208">Now, you replicate your Azure web app toohello Southeast Asia region.</span></span> <span data-ttu-id="e1b57-209">toostart, Använd [az redis skapa](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate andra Azure Redis-Cache i Sydostasien.</span><span class="sxs-lookup"><span data-stu-id="e1b57-209">toostart, use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate a second Azure Redis Cache in Southeast Asia.</span></span> <span data-ttu-id="e1b57-210">Det här cacheminnet måste toobe samordnade med din app i Asien.</span><span class="sxs-lookup"><span data-stu-id="e1b57-210">This cache needs toobe colocated with your app in Asia.</span></span>

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

<span data-ttu-id="e1b57-211">`--name $cacheName-asia`ger hello cache hello namnet på hello Västeuropa cache med hello `-asia` suffix.</span><span class="sxs-lookup"><span data-stu-id="e1b57-211">`--name $cacheName-asia` gives hello cache hello name of hello West Europe cache, with hello `-asia` suffix.</span></span>

### <a name="create-an-app-service-plan-in-asia"></a><span data-ttu-id="e1b57-212">Skapa en apptjänstplan i Asien</span><span class="sxs-lookup"><span data-stu-id="e1b57-212">Create an App Service plan in Asia</span></span>
<span data-ttu-id="e1b57-213">Använd [az programtjänstplan skapa](https://docs.microsoft.com/cli/azure/appservice/plan#create) toocreate en andra App Service-plan i hello Sydostasien region, med hjälp av hello samma S1 tjänstnivån som hello Västeuropa plan.</span><span class="sxs-lookup"><span data-stu-id="e1b57-213">Use [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) toocreate a second App Service plan in hello Southeast Asia region, using hello same S1 tier as hello West Europe plan.</span></span>

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a><span data-ttu-id="e1b57-214">Skapa en webbapp i Asien</span><span class="sxs-lookup"><span data-stu-id="e1b57-214">Create a web app in Asia</span></span>
<span data-ttu-id="e1b57-215">Använd [az apptjänst web skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate andra webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e1b57-215">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate a second web app.</span></span>

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

<span data-ttu-id="e1b57-216">`--name $appName-asia`ger hello hello programnamn hello Västeuropa appen med hello `-asia` suffix.</span><span class="sxs-lookup"><span data-stu-id="e1b57-216">`--name $appName-asia` gives hello app hello name of hello West Europe app, with hello `-asia` suffix.</span></span>

### <a name="configure-hello-connection-string-for-redis"></a><span data-ttu-id="e1b57-217">Konfigurera hello anslutningssträngen för Redis</span><span class="sxs-lookup"><span data-stu-id="e1b57-217">Configure hello connection string for Redis</span></span>
<span data-ttu-id="e1b57-218">Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd toohello web app hello anslutningssträngen för hello Sydostasien cache.</span><span class="sxs-lookup"><span data-stu-id="e1b57-218">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd toohello web app hello connection string for hello Southeast Asia cache.</span></span>

<span data-ttu-id="e1b57-219">AZ apptjänst Webbkonfiguration appsettings uppdatera--inställningar ”RedisConnection = $($redis.hostname): $($redis.sslPort), lösenord = $($redis.accessKeys.primaryKey), ssl = True, abortConnect = False”--name $appName-Asien--myResourceGroup för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e1b57-219">az appservice web config appsettings update --settings "RedisConnection=$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False" --name $appName-asia --resource-group myResourceGroup</span></span>

### <a name="configure-git-deployment-for-hello-asia-app"></a><span data-ttu-id="e1b57-220">Konfigurera Git-distribution för hello Asien app.</span><span class="sxs-lookup"><span data-stu-id="e1b57-220">Configure Git deployment for hello Asia app.</span></span>
<span data-ttu-id="e1b57-221">Använd [az apptjänst web källkontroll config-lokal-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure lokal Git-distribution för hello andra webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e1b57-221">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure local Git deployment for hello second web app.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="e1b57-222">Det här kommandot ger utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="e1b57-222">This command gives you an output that looks like hello following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="e1b57-223">Använd hello returnerade URL tooconfigure andra Git fjärråtkomst för din lokala databas.</span><span class="sxs-lookup"><span data-stu-id="e1b57-223">Use hello returned URL tooconfigure a second Git remote for your local repository.</span></span> <span data-ttu-id="e1b57-224">hello använder följande kommando hello föregående exempel på utdata.</span><span class="sxs-lookup"><span data-stu-id="e1b57-224">hello following command uses hello preceding output example.</span></span>

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a><span data-ttu-id="e1b57-225">Distribuera exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="e1b57-225">Deploy your sample application</span></span>
<span data-ttu-id="e1b57-226">Kör `git push` toodeploy din exempel programmet toohello andra Git-fjärrkontroll.</span><span class="sxs-lookup"><span data-stu-id="e1b57-226">Run `git push` toodeploy your sample application toohello second Git remote.</span></span> 

```bash
git push azure-asia master
```

<span data-ttu-id="e1b57-227">När du uppmanas att ange lösenord använder hello-lösenord som du angav när du körde `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-227">When prompted for password, use hello password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-toohello-asia-app"></a><span data-ttu-id="e1b57-228">Bläddra toohello Asien app</span><span class="sxs-lookup"><span data-stu-id="e1b57-228">Browse toohello Asia app</span></span>
<span data-ttu-id="e1b57-229">Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) tooverify som appen körs live i Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b57-229">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) tooverify that your app is running live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-hello-resource-id-of-hello-asia-app"></a><span data-ttu-id="e1b57-230">Hämta hello resurs-ID för hello Asien app</span><span class="sxs-lookup"><span data-stu-id="e1b57-230">Get hello resource ID of hello Asia app</span></span>
<span data-ttu-id="e1b57-231">Använd [az apptjänst web visa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resurs-ID för ditt webbprogram i Sydostasien.</span><span class="sxs-lookup"><span data-stu-id="e1b57-231">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resource ID of your web app in Southeast Asia.</span></span>

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-asia-app"></a><span data-ttu-id="e1b57-232">Lägg till en Traffic Manager-slutpunkt för hello Asien app</span><span class="sxs-lookup"><span data-stu-id="e1b57-232">Add a Traffic Manager endpoint for hello Asia app</span></span>
<span data-ttu-id="e1b57-233">Använd [az network traffic manager-slutpunkt skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd en andra slutpunkt toohello Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="e1b57-233">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd a second endpoint toohello Traffic Manager profile.</span></span>

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-tooweb-apps"></a><span data-ttu-id="e1b57-234">Lägg till region identifierare tooweb appar</span><span class="sxs-lookup"><span data-stu-id="e1b57-234">Add region identifier tooweb apps</span></span>
<span data-ttu-id="e1b57-235">Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd en regionspecifika miljövariabel.</span><span class="sxs-lookup"><span data-stu-id="e1b57-235">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd a region-specific environment variable.</span></span>

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="e1b57-236">Programkoden använder redan den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="e1b57-236">Your application code already uses this application setting.</span></span> <span data-ttu-id="e1b57-237">Ta en titt på `HighScaleApp\Views\Home\Index.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="e1b57-237">Take a look at `HighScaleApp\Views\Home\Index.cshtml`.</span></span>

### <a name="complete"></a><span data-ttu-id="e1b57-238">Slutföra!</span><span class="sxs-lookup"><span data-stu-id="e1b57-238">Complete!</span></span>

<span data-ttu-id="e1b57-239">Prova tooaccess hello URL för Traffic Manager-profilen från webbläsare i olika geografiska regioner.</span><span class="sxs-lookup"><span data-stu-id="e1b57-239">Now, try tooaccess hello URL of your Traffic Manager profile from browsers in different geographical regions.</span></span> <span data-ttu-id="e1b57-240">Klientwebbläsare från Europa ska visa ”ASP.NET västra Europa” och klientens webbläsare från Asien ska visa ”ASP.NET Sydostasien”.</span><span class="sxs-lookup"><span data-stu-id="e1b57-240">Client browsers from Europe should show "ASP.NET West Europe", and client browser from Asia should show "ASP.NET Southeast Asia."</span></span>

## <a name="more-resources"></a><span data-ttu-id="e1b57-241">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="e1b57-241">More resources</span></span>
