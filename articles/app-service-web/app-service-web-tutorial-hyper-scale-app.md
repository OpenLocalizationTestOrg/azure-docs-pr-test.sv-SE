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
# <a name="build-a-hyper-scale-web-app-in-azure"></a><span data-ttu-id="d69a4-103">Skapa en storskaliga webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="d69a4-103">Build a hyper-scale web app in Azure</span></span>

<span data-ttu-id="d69a4-104">Den här kursen visar hur du skala ut en ASP.NET-webbapp i Azure för att maximera användarförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="d69a4-104">This tutorial shows you how to scale out an ASP.NET web app in Azure to maximize user requests.</span></span>

<span data-ttu-id="d69a4-105">Innan du börjar den här kursen ska du kontrollera att [Azure CLI är installerad](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) på din dator.</span><span class="sxs-lookup"><span data-stu-id="d69a4-105">Before starting this tutorial, ensure that [the Azure CLI is installed](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span> <span data-ttu-id="d69a4-106">Dessutom måste [Visual Studio](https://www.visualstudio.com/vs/) på den lokala datorn för att köra exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d69a4-106">In addition, you need [Visual Studio](https://www.visualstudio.com/vs/) on your local machine to run the sample application.</span></span>

## <a name="step-1---get-sample-application"></a><span data-ttu-id="d69a4-107">Steg 1 – hämta exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="d69a4-107">Step 1 - Get sample application</span></span>
<span data-ttu-id="d69a4-108">I det här steget kan du ställa in lokalt ASP.NET-projekt.</span><span class="sxs-lookup"><span data-stu-id="d69a4-108">In this step, you set up the local ASP.NET project.</span></span>

### <a name="clone-the-application-repository"></a><span data-ttu-id="d69a4-109">Klona lagringsplatsen för programmet</span><span class="sxs-lookup"><span data-stu-id="d69a4-109">Clone the application repository</span></span>

<span data-ttu-id="d69a4-110">Öppna önskat kommandoradsverktyg önskat och `CD` till en arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="d69a4-110">Open the command-line terminal of your choice and `CD` to a working directory.</span></span> <span data-ttu-id="d69a4-111">Kör sedan följande kommandon för att klona exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d69a4-111">Then, run the following commands to clone the sample application.</span></span> 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-the-sample-application-in-visual-studio"></a><span data-ttu-id="d69a4-112">Kör exempelprogrammet i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d69a4-112">Run the sample application in Visual Studio</span></span>

<span data-ttu-id="d69a4-113">Öppna lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d69a4-113">Open the solution in Visual Studio.</span></span>

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

<span data-ttu-id="d69a4-114">Typen `F5` att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="d69a4-114">Type `F5` to run the application.</span></span>

<span data-ttu-id="d69a4-115">Det här exemplet ASP.NET-webbprogram kommer från standardmallen och kvarstår användaren sessioner och använder utdatacachen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-115">This sample ASP.NET web application comes from the default template, and persists user sessions and uses the output cache.</span></span> <span data-ttu-id="d69a4-116">Ta en titt på `HighScaleApp\Controllers\HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-116">Take a look at `HighScaleApp\Controllers\HomeController.cs`.</span></span> <span data-ttu-id="d69a4-117">Den `Index()` metoden lägger till en typ av data i sessionen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-117">The `Index()` method adds a piece of data to the session.</span></span>

```csharp
Session.Add("visited", "true"); 
```

<span data-ttu-id="d69a4-118">Och `About()` och `Contact()` metoder cachelagra sina utdata.</span><span class="sxs-lookup"><span data-stu-id="d69a4-118">And the `About()` and `Contact()` methods cache their output.</span></span>

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-to-azure"></a><span data-ttu-id="d69a4-119">Steg 2 – distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="d69a4-119">Step 2 - Deploy to Azure</span></span>
<span data-ttu-id="d69a4-120">I det här steget att skapa en Azure-webbapp och distribuera ASP.NET exempelprogrammet till den.</span><span class="sxs-lookup"><span data-stu-id="d69a4-120">In this step, you create an Azure web app and deploy your sample ASP.NET application to it.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="d69a4-121">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d69a4-121">Create a resource group</span></span>   
<span data-ttu-id="d69a4-122">Använd [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) att skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) i Västeuropa region.</span><span class="sxs-lookup"><span data-stu-id="d69a4-122">Use [az group create](https://docs.microsoft.com/cli/azure/group#create) to create a [resource group](../azure-resource-manager/resource-group-overview.md) in the West Europe region.</span></span> <span data-ttu-id="d69a4-123">En resursgrupp är där du placerar alla Azure-resurser som du vill hantera tillsammans, som serverdel för webbapp och en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d69a4-123">A resource group is where you put all the Azure resources that you want to manage together, such as the web app and any SQL Database back end.</span></span>

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

<span data-ttu-id="d69a4-124">Att se vilka möjliga värden du kan använda för `---location`, använda den [az apptjänst lista-platser](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) kommando.</span><span class="sxs-lookup"><span data-stu-id="d69a4-124">To see what possible values you can use for `---location`, use the [az appservice list-locations](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="d69a4-125">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="d69a4-125">Create an App Service plan</span></span>
<span data-ttu-id="d69a4-126">Använd [az programtjänstplan skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) att skapa en ”B1” [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d69a4-126">Use [az appservice plan create](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) to create a "B1" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

<span data-ttu-id="d69a4-127">En apptjänstplan är en skalningsenhet som kan innehålla valfritt antal appar som du vill skala upp eller ut över samma App Service-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="d69a4-127">An App Service plan is a scale unit, which can include any number of apps that you want to scale up or out together over the same App Service infrastructure.</span></span> <span data-ttu-id="d69a4-128">Varje plan är även tilldelad ett [prisnivån](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="d69a4-128">Each plan is also assigned a [pricing tier](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span></span> <span data-ttu-id="d69a4-129">Högre nivåer omfattar bättre maskin- och fler funktioner, till exempel mer skalbart instanser.</span><span class="sxs-lookup"><span data-stu-id="d69a4-129">Higher tiers include better hardware and more features, such as more scale-out instances.</span></span>

<span data-ttu-id="d69a4-130">B1 är den lägsta nivå som gör att skala ut till tre instanser för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-130">For this tutorial, B1 is the minimum tier that enables scale out to three instances.</span></span> <span data-ttu-id="d69a4-131">Du kan alltid flytta appen uppåt eller nedåt prisnivån senare genom att köra [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span><span class="sxs-lookup"><span data-stu-id="d69a4-131">You can always move your app up or down the pricing tier later by running [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span></span> 

### <a name="create-a-web-app"></a><span data-ttu-id="d69a4-132">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="d69a4-132">Create a web app</span></span>
<span data-ttu-id="d69a4-133">Använd [az apptjänst web skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) att skapa en webbapp med ett unikt namn i `$appName`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-133">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) to create a web app with a unique name in `$appName`.</span></span>

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a><span data-ttu-id="d69a4-134">Ange autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="d69a4-134">Set deployment credentials</span></span>
<span data-ttu-id="d69a4-135">Använd [az apptjänst web distributionsanvändare har angetts](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) ange dina autentiseringsuppgifter för kontonivå distribution för App Service.</span><span class="sxs-lookup"><span data-stu-id="d69a4-135">Use [az appservice web deployment user set](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) to set your account-level deployment credentials for App Service.</span></span>

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a><span data-ttu-id="d69a4-136">Konfigurera Git-distribution</span><span class="sxs-lookup"><span data-stu-id="d69a4-136">Configure Git deployment</span></span>
<span data-ttu-id="d69a4-137">Använd [az apptjänst web källkontroll config-lokal-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) att konfigurera lokal Git-distribution.</span><span class="sxs-lookup"><span data-stu-id="d69a4-137">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) to configure local Git deployment.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

<span data-ttu-id="d69a4-138">Det här kommandot ger utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="d69a4-138">This command gives you an output that looks like the following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="d69a4-139">Använd returnerade URL: en för att konfigurera Git-remote.</span><span class="sxs-lookup"><span data-stu-id="d69a4-139">Use the returned URL to configure your Git remote.</span></span> <span data-ttu-id="d69a4-140">Följande kommando använder utdata från föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="d69a4-140">The following command uses the preceding output example.</span></span>

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-the-sample-application"></a><span data-ttu-id="d69a4-141">Distribuera exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="d69a4-141">Deploy the sample application</span></span>
<span data-ttu-id="d69a4-142">Du är nu redo att distribuera exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d69a4-142">You are now ready to deploy your sample application.</span></span> <span data-ttu-id="d69a4-143">Kör `git push`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-143">Run `git push`.</span></span>

```powershell
git push azure master
```

<span data-ttu-id="d69a4-144">När du uppmanas att ange lösenord använder du det lösenord som du angav när du körde `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-144">When prompted for password, use the password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-to-azure-web-app"></a><span data-ttu-id="d69a4-145">Bläddra till Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="d69a4-145">Browse to Azure web app</span></span>
<span data-ttu-id="d69a4-146">Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) kör det här kommandot för att se köra appen live i Azure.</span><span class="sxs-lookup"><span data-stu-id="d69a4-146">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) to see your app running live in Azure, run this command.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-to-redis"></a><span data-ttu-id="d69a4-147">Steg 3 – ansluta till Redis</span><span class="sxs-lookup"><span data-stu-id="d69a4-147">Step 3 - Connect to Redis</span></span>
<span data-ttu-id="d69a4-148">I det här steget kan ställa du in Azure Redis-Cache som en extern, samordnade cache till Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="d69a4-148">In this step, you set up Azure Redis Cache as an external, colocated cache to your Azure web app.</span></span> <span data-ttu-id="d69a4-149">Du kan snabbt använda Redis att cachelagra sidans utdata.</span><span class="sxs-lookup"><span data-stu-id="d69a4-149">You can quickly utilize Redis to cache your page output.</span></span> <span data-ttu-id="d69a4-150">Dessutom när du skalar upp dina webbprogram senare Redis hjälper dig att bevara användarsessioner på flera instanser på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="d69a4-150">In addition, when you scale out your web apps later, Redis helps you persist user sessions across multiple instances reliably.</span></span>

### <a name="create-an-azure-redis-cache"></a><span data-ttu-id="d69a4-151">Skapa en Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="d69a4-151">Create an Azure Redis Cache</span></span>
<span data-ttu-id="d69a4-152">Använd [az redis skapa](https://docs.microsoft.com/en-us/cli/azure/redis#create) att skapa ett Azure Redis-Cache och spara JSON-utdata.</span><span class="sxs-lookup"><span data-stu-id="d69a4-152">Use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) to create an Azure Redis Cache and save the JSON output.</span></span> <span data-ttu-id="d69a4-153">Använd ett unikt namn i `$cacheName`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-153">Use a unique name in `$cacheName`.</span></span>

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-the-application-to-use-redis"></a><span data-ttu-id="d69a4-154">Konfigurera program att använda Redis</span><span class="sxs-lookup"><span data-stu-id="d69a4-154">Configure the application to use Redis</span></span>
<span data-ttu-id="d69a4-155">Formatera anslutningssträngen för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="d69a4-155">Format the connection string for your cache.</span></span>

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

<span data-ttu-id="d69a4-156">Den andra raden bör ge dig en utdata som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="d69a4-156">The second line should give you an output that looks like this:</span></span>

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

<span data-ttu-id="d69a4-157">I Visual Studio skapar du en Webbkonfigurationsfilen i projektroten kallas `redis.config` och klistra in följande kod i den.</span><span class="sxs-lookup"><span data-stu-id="d69a4-157">In Visual Studio, create a web configuration file in your project root called `redis.config` and paste the following code into it.</span></span> <span data-ttu-id="d69a4-158">I `value`, använder anslutningssträngen från PowerShell-utdata.</span><span class="sxs-lookup"><span data-stu-id="d69a4-158">In `value`, use the connection string from the PowerShell output.</span></span>

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

<span data-ttu-id="d69a4-159">Om du tittar på det `.gitignore` filen i Git-lagringsplatsen, ser du att den här filen är undantagen från källkontroll.</span><span class="sxs-lookup"><span data-stu-id="d69a4-159">If you look at the `.gitignore` file in your Git repository, you'll see that this file is excluded from source control.</span></span> <span data-ttu-id="d69a4-160">På så sätt känslig information förblir säkra.</span><span class="sxs-lookup"><span data-stu-id="d69a4-160">That way your sensitive information is kept safe.</span></span> 

<span data-ttu-id="d69a4-161">Öppna `Web.config`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-161">Open `Web.config`.</span></span> <span data-ttu-id="d69a4-162">Observera den `<appSettings file="redis.config">` element som hämtar den inställning som du skapade i `redis.config`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-162">Notice the `<appSettings file="redis.config">` element, which gets the setting you created in `redis.config`.</span></span> 

<span data-ttu-id="d69a4-163">Gå till avsnittet kommenterad som innehåller `<sessionState>` och `<caching>`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-163">Find the commented section that includes `<sessionState>` and `<caching>`.</span></span> <span data-ttu-id="d69a4-164">Ta bort kommentarerna i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d69a4-164">Uncomment this section.</span></span>

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

<span data-ttu-id="d69a4-165">Den här koden söker efter Redis anslutningssträngen som du har definierat i `RedisConnection`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-165">This code looks for the Redis connection string you defined in `RedisConnection`.</span></span> 

<span data-ttu-id="d69a4-166">Redis används för att hantera sessioner och cachelagring i ditt program nu.</span><span class="sxs-lookup"><span data-stu-id="d69a4-166">Your application now uses Redis to manage sessions and caching.</span></span> <span data-ttu-id="d69a4-167">Typen `F5` att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="d69a4-167">Type `F5` to run the application.</span></span> <span data-ttu-id="d69a4-168">Om du vill kan du hämta Redis management-klienten om du vill visualisera data som sparas i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="d69a4-168">If you like, you can download a Redis management client to visualize the data that is now saved to the cache.</span></span>

### <a name="configure-the-connection-string-in-azure"></a><span data-ttu-id="d69a4-169">Konfigurera anslutningssträngen i Azure</span><span class="sxs-lookup"><span data-stu-id="d69a4-169">Configure the connection string in Azure</span></span>

<span data-ttu-id="d69a4-170">För programmet att fungera i Azure, måste du konfigurera samma Redis-anslutningssträng i ditt Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="d69a4-170">For your application to work in Azure, you need to configure the same Redis connection string in your Azure web app.</span></span> <span data-ttu-id="d69a4-171">Eftersom `redis.config` behålls inte källkontroll, den inte har distribuerats till Azure när du kör Git-distribution.</span><span class="sxs-lookup"><span data-stu-id="d69a4-171">Since `redis.config` is not maintained in source control, it is not deployed to Azure when you run Git deployment.</span></span>

<span data-ttu-id="d69a4-172">Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) att lägga till anslutningssträngen med samma namn (`RedisConnection`).</span><span class="sxs-lookup"><span data-stu-id="d69a4-172">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) to add the connection string with the same name (`RedisConnection`).</span></span>

<span data-ttu-id="d69a4-173">AZ apptjänst Webbkonfiguration appsettings uppdatera--inställningar ”RedisConnection = $connstring”--name $appName--myResourceGroup för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-173">az appservice web config appsettings update --settings "RedisConnection=$connstring" --name $appName --resource-group myResourceGroup</span></span>

<span data-ttu-id="d69a4-174">Kom ihåg att `$connstring` innehåller formaterad anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-174">Remember that `$connstring` contains the formatted connection string.</span></span>

### <a name="redeploy-the-application-to-azure"></a><span data-ttu-id="d69a4-175">Distribuera programmet till Azure</span><span class="sxs-lookup"><span data-stu-id="d69a4-175">Redeploy the application to Azure</span></span>
<span data-ttu-id="d69a4-176">Använda Git-kommandon för att skicka ändringarna till Azure</span><span class="sxs-lookup"><span data-stu-id="d69a4-176">Use Git commands to push your changes to Azure</span></span>

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

<span data-ttu-id="d69a4-177">När du uppmanas att ange lösenord använder du det lösenord som du angav när du körde `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-177">When prompted for password, use the password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-to-the-azure-web-app"></a><span data-ttu-id="d69a4-178">Bläddra till Azure-webbappen</span><span class="sxs-lookup"><span data-stu-id="d69a4-178">Browse to the Azure web app</span></span>
<span data-ttu-id="d69a4-179">Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) ska kunna se ändringarna live i Azure.</span><span class="sxs-lookup"><span data-stu-id="d69a4-179">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) to see the changes live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-to-multiple-instances"></a><span data-ttu-id="d69a4-180">Steg 4 – skala till flera instanser</span><span class="sxs-lookup"><span data-stu-id="d69a4-180">Step 4 - Scale to multiple instances</span></span>
<span data-ttu-id="d69a4-181">Programtjänstplanen är skalningsenhet för din Azure-webbappar.</span><span class="sxs-lookup"><span data-stu-id="d69a4-181">The App Service plan is the scale unit for your Azure web apps.</span></span> <span data-ttu-id="d69a4-182">Om du vill skala ut ditt webbprogram, kan du skala App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="d69a4-182">To scale out your web app, you scale the App Service plan.</span></span>

<span data-ttu-id="d69a4-183">Använd [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update) ska skalas ut programtjänstplanen till tre instanser som är det maximala antalet tillåtna av B1 prisnivån.</span><span class="sxs-lookup"><span data-stu-id="d69a4-183">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) to scale out the App Service plan to three instances, which is the maximum number allowed by the B1 pricing tier.</span></span> <span data-ttu-id="d69a4-184">Kom ihåg att B1 prisnivå som du valde när du skapade programtjänstplanen tidigare.</span><span class="sxs-lookup"><span data-stu-id="d69a4-184">Remember that B1 is the pricing tier that you chose when you created the App Service plan earlier.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a><span data-ttu-id="d69a4-185">Steg 5 – skala geografiskt</span><span class="sxs-lookup"><span data-stu-id="d69a4-185">Step 5 - Scale geographically</span></span>
<span data-ttu-id="d69a4-186">Vid skalning geografiskt kör appen i flera regioner på Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="d69a4-186">When scaling geographically, you run your app in multiple regions of the Azure cloud.</span></span> <span data-ttu-id="d69a4-187">Den här installationen belastningsutjämnas appen ytterligare baserat på geografisk plats och minskar svarstiden genom att placera din app närmare till klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d69a4-187">This setup load-balances your app further based on geography and lowers the response time by placing your app closer to client browsers.</span></span>

<span data-ttu-id="d69a4-188">I det här steget kan du skala ditt ASP.NET-webbprogram till en andra region med [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span><span class="sxs-lookup"><span data-stu-id="d69a4-188">In this step, you scale your ASP.NET web app to a second region with [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span></span> <span data-ttu-id="d69a4-189">I slutet av steget har du en webbapp som körs i Västeuropa (redan skapat) och ett webbprogram som körs i Sydostasien (har inte skapats ännu).</span><span class="sxs-lookup"><span data-stu-id="d69a4-189">At the end of the step, you will have a web app running in West Europe (already created) and a web app running in Southeast Asia (not yet created).</span></span> <span data-ttu-id="d69a4-190">Både appar hanteras från samma Traffic Manager-URL.</span><span class="sxs-lookup"><span data-stu-id="d69a4-190">Both apps will be served from the same Traffic Manager URL.</span></span>

### <a name="scale-up-the-europe-app-to-standard-tier"></a><span data-ttu-id="d69a4-191">Skala upp appen Europa Standard-nivån</span><span class="sxs-lookup"><span data-stu-id="d69a4-191">Scale up the Europe app to Standard tier</span></span>
<span data-ttu-id="d69a4-192">Integrering med Azure Traffic Manager kräver prisnivån Standard i App Service.</span><span class="sxs-lookup"><span data-stu-id="d69a4-192">In App Service, integration with Azure Traffic Manager requires the Standard pricing tier.</span></span> <span data-ttu-id="d69a4-193">Använd [az apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update) att skala upp din programtjänstplan till S1.</span><span class="sxs-lookup"><span data-stu-id="d69a4-193">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) to scale up your App Service plan to S1.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="d69a4-194">Skapa en Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="d69a4-194">Create a Traffic Manager profile</span></span> 
<span data-ttu-id="d69a4-195">Använd [az network traffic manager-profilen skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) att skapa en Traffic Manager-profilen och lägga till den i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-195">Use [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) to create a Traffic Manager profile and add it to your resource group.</span></span> <span data-ttu-id="d69a4-196">Använd ett unikt DNS-namn i $dnsName.</span><span class="sxs-lookup"><span data-stu-id="d69a4-196">Use a unique DNS name in $dnsName.</span></span>

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> <span data-ttu-id="d69a4-197">`--routing-method Performance`Anger att den här profilen [dirigerar användartrafik till närmaste slutpunkten](../traffic-manager/traffic-manager-routing-methods.md).</span><span class="sxs-lookup"><span data-stu-id="d69a4-197">`--routing-method Performance` specifies that this profile [routes user traffic to the closest endpoint](../traffic-manager/traffic-manager-routing-methods.md).</span></span>

### <a name="get-the-resource-id-of-the-europe-app"></a><span data-ttu-id="d69a4-198">Hämta resurs-ID för appen Europa</span><span class="sxs-lookup"><span data-stu-id="d69a4-198">Get the resource ID of the Europe app</span></span>
<span data-ttu-id="d69a4-199">Använd [az apptjänst web visa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) få resurs-ID för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d69a4-199">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) to get the resource ID of your web app.</span></span>

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-europe-app"></a><span data-ttu-id="d69a4-200">Lägg till en Traffic Manager-slutpunkt för Europa-app</span><span class="sxs-lookup"><span data-stu-id="d69a4-200">Add a Traffic Manager endpoint for the Europe app</span></span>
<span data-ttu-id="d69a4-201">Använd [az network traffic manager-slutpunkt skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) att lägga till en slutpunkt i Traffic Manager-profilen och använda resurs-ID för ditt webbprogram som mål.</span><span class="sxs-lookup"><span data-stu-id="d69a4-201">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) to add an endpoint to your Traffic Manager profile and use the resource ID of your web app as the target.</span></span>

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-the-traffic-manager-endpoint-url"></a><span data-ttu-id="d69a4-202">Hämta Traffic Manager slutpunkts-URL</span><span class="sxs-lookup"><span data-stu-id="d69a4-202">Get the Traffic Manager endpoint URL</span></span>
<span data-ttu-id="d69a4-203">Traffic Manager-profilen har nu en slutpunkt som pekar på din befintliga webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d69a4-203">Your Traffic Manager profile now has an endpoint that points to your existing web app.</span></span> <span data-ttu-id="d69a4-204">Använd [az network traffic manager-profilen visas](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) att hämta dess Webbadress.</span><span class="sxs-lookup"><span data-stu-id="d69a4-204">Use [az network traffic-manager profile show](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) to get its URL.</span></span> 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

<span data-ttu-id="d69a4-205">Kopiera utdata i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d69a4-205">Copy the output into your browser.</span></span> <span data-ttu-id="d69a4-206">Du bör se ditt webbprogram igen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-206">You should see your web app again.</span></span>

### <a name="create-an-azure-redis-cache-in-asia"></a><span data-ttu-id="d69a4-207">Skapa ett Azure Redis-Cache i Asien</span><span class="sxs-lookup"><span data-stu-id="d69a4-207">Create an Azure Redis Cache in Asia</span></span>
<span data-ttu-id="d69a4-208">Nu kan replikera du dina Azure-webbapp till Sydostasien region.</span><span class="sxs-lookup"><span data-stu-id="d69a4-208">Now, you replicate your Azure web app to the Southeast Asia region.</span></span> <span data-ttu-id="d69a4-209">Starta med [az redis skapa](https://docs.microsoft.com/en-us/cli/azure/redis#create) att skapa en andra Azure Redis-Cache i Sydostasien.</span><span class="sxs-lookup"><span data-stu-id="d69a4-209">To start, use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) to create a second Azure Redis Cache in Southeast Asia.</span></span> <span data-ttu-id="d69a4-210">Det här cacheminnet behöver inte samplaceras med din app i Asien.</span><span class="sxs-lookup"><span data-stu-id="d69a4-210">This cache needs to be colocated with your app in Asia.</span></span>

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

<span data-ttu-id="d69a4-211">`--name $cacheName-asia`ger cachen namn västra Europa-cache med de `-asia` suffix.</span><span class="sxs-lookup"><span data-stu-id="d69a4-211">`--name $cacheName-asia` gives the cache the name of the West Europe cache, with the `-asia` suffix.</span></span>

### <a name="create-an-app-service-plan-in-asia"></a><span data-ttu-id="d69a4-212">Skapa en apptjänstplan i Asien</span><span class="sxs-lookup"><span data-stu-id="d69a4-212">Create an App Service plan in Asia</span></span>
<span data-ttu-id="d69a4-213">Använd [az programtjänstplan skapa](https://docs.microsoft.com/cli/azure/appservice/plan#create) att skapa en andra programtjänstplan i Sydostasien region, med samma S1 nivå som Västeuropa planen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-213">Use [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) to create a second App Service plan in the Southeast Asia region, using the same S1 tier as the West Europe plan.</span></span>

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a><span data-ttu-id="d69a4-214">Skapa en webbapp i Asien</span><span class="sxs-lookup"><span data-stu-id="d69a4-214">Create a web app in Asia</span></span>
<span data-ttu-id="d69a4-215">Använd [az apptjänst web skapa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) att skapa en andra webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d69a4-215">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) to create a second web app.</span></span>

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

<span data-ttu-id="d69a4-216">`--name $appName-asia`ger appen namnet på appen Västeuropa med den `-asia` suffix.</span><span class="sxs-lookup"><span data-stu-id="d69a4-216">`--name $appName-asia` gives the app the name of the West Europe app, with the `-asia` suffix.</span></span>

### <a name="configure-the-connection-string-for-redis"></a><span data-ttu-id="d69a4-217">Konfigurera anslutningssträngen för Redis</span><span class="sxs-lookup"><span data-stu-id="d69a4-217">Configure the connection string for Redis</span></span>
<span data-ttu-id="d69a4-218">Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) att lägga till webbprogrammet anslutningssträngen för Sydostasien cachen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-218">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) to add to the web app the connection string for the Southeast Asia cache.</span></span>

<span data-ttu-id="d69a4-219">AZ apptjänst Webbkonfiguration appsettings uppdatera--inställningar ”RedisConnection = $($redis.hostname): $($redis.sslPort), lösenord = $($redis.accessKeys.primaryKey), ssl = True, abortConnect = False”--name $appName-Asien--myResourceGroup för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-219">az appservice web config appsettings update --settings "RedisConnection=$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False" --name $appName-asia --resource-group myResourceGroup</span></span>

### <a name="configure-git-deployment-for-the-asia-app"></a><span data-ttu-id="d69a4-220">Konfigurera Git-distribution för Asien appen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-220">Configure Git deployment for the Asia app.</span></span>
<span data-ttu-id="d69a4-221">Använd [az apptjänst web källkontroll config-lokal-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) att konfigurera lokal Git-distribution för andra webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d69a4-221">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) to configure local Git deployment for the second web app.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="d69a4-222">Det här kommandot ger utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="d69a4-222">This command gives you an output that looks like the following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="d69a4-223">Använd returnerade URL: en för att konfigurera en andra fjärransluten Git för lokala databasen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-223">Use the returned URL to configure a second Git remote for your local repository.</span></span> <span data-ttu-id="d69a4-224">Följande kommando använder utdata från föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="d69a4-224">The following command uses the preceding output example.</span></span>

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a><span data-ttu-id="d69a4-225">Distribuera exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="d69a4-225">Deploy your sample application</span></span>
<span data-ttu-id="d69a4-226">Kör `git push` att distribuera exempelprogrammet till andra fjärransluten Git.</span><span class="sxs-lookup"><span data-stu-id="d69a4-226">Run `git push` to deploy your sample application to the second Git remote.</span></span> 

```bash
git push azure-asia master
```

<span data-ttu-id="d69a4-227">När du uppmanas att ange lösenord använder du det lösenord som du angav när du körde `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-227">When prompted for password, use the password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-to-the-asia-app"></a><span data-ttu-id="d69a4-228">Bläddra till appen Asien</span><span class="sxs-lookup"><span data-stu-id="d69a4-228">Browse to the Asia app</span></span>
<span data-ttu-id="d69a4-229">Använd [az apptjänst web Bläddra](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) att verifiera att appen körs live i Azure.</span><span class="sxs-lookup"><span data-stu-id="d69a4-229">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) to verify that your app is running live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-the-resource-id-of-the-asia-app"></a><span data-ttu-id="d69a4-230">Hämta resurs-ID för appen Asien</span><span class="sxs-lookup"><span data-stu-id="d69a4-230">Get the resource ID of the Asia app</span></span>
<span data-ttu-id="d69a4-231">Använd [az apptjänst web visa](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) att hämta resurs-ID för ditt webbprogram i Sydostasien.</span><span class="sxs-lookup"><span data-stu-id="d69a4-231">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) to get the resource ID of your web app in Southeast Asia.</span></span>

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-asia-app"></a><span data-ttu-id="d69a4-232">Lägg till en Traffic Manager-slutpunkt för Asien appen</span><span class="sxs-lookup"><span data-stu-id="d69a4-232">Add a Traffic Manager endpoint for the Asia app</span></span>
<span data-ttu-id="d69a4-233">Använd [az network traffic manager-slutpunkt skapa](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) att lägga till en andra slutpunkt i Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-233">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) to add a second endpoint to the Traffic Manager profile.</span></span>

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-to-web-apps"></a><span data-ttu-id="d69a4-234">Lägg till regionsidentifierare i webbprogram</span><span class="sxs-lookup"><span data-stu-id="d69a4-234">Add region identifier to web apps</span></span>
<span data-ttu-id="d69a4-235">Använd [az apptjänst web config appsettings uppdatera](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) att lägga till en regionspecifika miljövariabel.</span><span class="sxs-lookup"><span data-stu-id="d69a4-235">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) to add a region-specific environment variable.</span></span>

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="d69a4-236">Programkoden använder redan den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="d69a4-236">Your application code already uses this application setting.</span></span> <span data-ttu-id="d69a4-237">Ta en titt på `HighScaleApp\Views\Home\Index.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="d69a4-237">Take a look at `HighScaleApp\Views\Home\Index.cshtml`.</span></span>

### <a name="complete"></a><span data-ttu-id="d69a4-238">Slutföra!</span><span class="sxs-lookup"><span data-stu-id="d69a4-238">Complete!</span></span>

<span data-ttu-id="d69a4-239">Nu, försök att komma åt URL-Adressen för din Traffic Manager-profil från webbläsare i olika geografiska regioner.</span><span class="sxs-lookup"><span data-stu-id="d69a4-239">Now, try to access the URL of your Traffic Manager profile from browsers in different geographical regions.</span></span> <span data-ttu-id="d69a4-240">Klientwebbläsare från Europa ska visa ”ASP.NET västra Europa” och klientens webbläsare från Asien ska visa ”ASP.NET Sydostasien”.</span><span class="sxs-lookup"><span data-stu-id="d69a4-240">Client browsers from Europe should show "ASP.NET West Europe", and client browser from Asia should show "ASP.NET Southeast Asia."</span></span>

## <a name="more-resources"></a><span data-ttu-id="d69a4-241">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="d69a4-241">More resources</span></span>
