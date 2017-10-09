---
title: "aaaGuidance för att utveckla Azure Functions | Microsoft Docs"
description: "Lär dig hello Azure Functions koncept och tekniker som du behöver toodevelop funktioner i Azure, för alla programmeringsspråk och bindningar."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "utvecklare guide, azure-funktion, funktioner, händelsebearbetning, webhooks, dynamiska beräkning, serverlösa arkitektur"
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: chrande
ms.openlocfilehash: 86d37dae5333f615faafc966e9da6e08e0a6354e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-developers-guide"></a><span data-ttu-id="faf1c-104">Azure Functions-guide för utvecklare</span><span class="sxs-lookup"><span data-stu-id="faf1c-104">Azure Functions developers guide</span></span>
<span data-ttu-id="faf1c-105">I Azure Functions dela funktioner några grundläggande tekniska begrepp och komponenter, oavsett hello språk eller bindning som du använder.</span><span class="sxs-lookup"><span data-stu-id="faf1c-105">In Azure Functions, specific functions share a few core technical concepts and components, regardless of hello language or binding you use.</span></span> <span data-ttu-id="faf1c-106">Vara säker på att tooread via den här översikten som gäller tooall av dem innan du går till learning information specifik tooa språk eller bindning.</span><span class="sxs-lookup"><span data-stu-id="faf1c-106">Before you jump into learning details specific tooa given language or binding, be sure tooread through this overview that applies tooall of them.</span></span>

<span data-ttu-id="faf1c-107">Den här artikeln förutsätter att du redan har läst hello [översikt över Azure Functions](functions-overview.md) och är bekant med [WebJobs SDK begrepp som utlösare och bindningar hello JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="faf1c-107">This article assumes that you've already read hello [Azure Functions overview](functions-overview.md) and are familiar with [WebJobs SDK concepts such as triggers, bindings, and hello JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md).</span></span> <span data-ttu-id="faf1c-108">Azure Functions baseras på hello WebJobs-SDK.</span><span class="sxs-lookup"><span data-stu-id="faf1c-108">Azure Functions is based on hello WebJobs SDK.</span></span> 

## <a name="function-code"></a><span data-ttu-id="faf1c-109">Funktionskoden</span><span class="sxs-lookup"><span data-stu-id="faf1c-109">Function code</span></span>
<span data-ttu-id="faf1c-110">En *funktionen* är hello primära begrepp i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="faf1c-110">A *function* is hello primary concept in Azure Functions.</span></span> <span data-ttu-id="faf1c-111">Du skriva kod för en funktion på ett annat språk och spara hello kod och konfigurationsfiler i hello samma mapp.</span><span class="sxs-lookup"><span data-stu-id="faf1c-111">You write code for a function in a language of your choice and save hello code and configuration files in hello same folder.</span></span> <span data-ttu-id="faf1c-112">hello configuration heter `function.json`, som innehåller JSON-konfigurationsdata.</span><span class="sxs-lookup"><span data-stu-id="faf1c-112">hello configuration is named `function.json`, which contains JSON configuration data.</span></span> <span data-ttu-id="faf1c-113">Olika språk som stöds och var och en har ett något annorlunda upplevelse optimerade toowork bäst för det språket.</span><span class="sxs-lookup"><span data-stu-id="faf1c-113">Various languages are supported, and each one has a slightly different experience optimized toowork best for that language.</span></span> 

<span data-ttu-id="faf1c-114">Hej function.json filen definierar hello funktionsbindningar och andra konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="faf1c-114">hello function.json file defines hello function bindings and other configuration settings.</span></span> <span data-ttu-id="faf1c-115">hello runtime använder den här filen toodetermine hello händelser toomonitor och hur toopass data till och returnera data från att fungera körning.</span><span class="sxs-lookup"><span data-stu-id="faf1c-115">hello runtime uses this file toodetermine hello events toomonitor and how toopass data into and return data from function execution.</span></span> <span data-ttu-id="faf1c-116">hello följande är ett exempel function.json-fil.</span><span class="sxs-lookup"><span data-stu-id="faf1c-116">hello following is an example function.json file.</span></span>

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

<span data-ttu-id="faf1c-117">Ange hello `disabled` egenskapen för`true` tooprevent hello funktion från utförs.</span><span class="sxs-lookup"><span data-stu-id="faf1c-117">Set hello `disabled` property too`true` tooprevent hello function from being executed.</span></span>

<span data-ttu-id="faf1c-118">Hej `bindings` egenskapen är där du kan konfigurera både utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="faf1c-118">hello `bindings` property is where you configure both triggers and bindings.</span></span> <span data-ttu-id="faf1c-119">Varje bindning delar några vanliga inställningar och vissa inställningar som är specifika tooa viss typ av bindningen.</span><span class="sxs-lookup"><span data-stu-id="faf1c-119">Each binding shares a few common settings and some settings, which are specific tooa particular type of binding.</span></span> <span data-ttu-id="faf1c-120">Alla bindningar kräver hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="faf1c-120">Every binding requires hello following settings:</span></span>

| <span data-ttu-id="faf1c-121">Egenskap</span><span class="sxs-lookup"><span data-stu-id="faf1c-121">Property</span></span> | <span data-ttu-id="faf1c-122">Värdetyper /</span><span class="sxs-lookup"><span data-stu-id="faf1c-122">Values/Types</span></span> | <span data-ttu-id="faf1c-123">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="faf1c-123">Comments</span></span> |
| --- | --- | --- |
| `type` |<span data-ttu-id="faf1c-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="faf1c-124">string</span></span> |<span data-ttu-id="faf1c-125">Bindningstyp.</span><span class="sxs-lookup"><span data-stu-id="faf1c-125">Binding type.</span></span> <span data-ttu-id="faf1c-126">Till exempel `queueTrigger`.</span><span class="sxs-lookup"><span data-stu-id="faf1c-126">For example, `queueTrigger`.</span></span> |
| `direction` |<span data-ttu-id="faf1c-127">'i', 'out'</span><span class="sxs-lookup"><span data-stu-id="faf1c-127">'in', 'out'</span></span> |<span data-ttu-id="faf1c-128">Anger om hello-bindning för att ta emot data till hello funktionen eller skicka data från hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="faf1c-128">Indicates whether hello binding is for receiving data into hello function or sending data from hello function.</span></span> |
| `name` |<span data-ttu-id="faf1c-129">Sträng</span><span class="sxs-lookup"><span data-stu-id="faf1c-129">string</span></span> |<span data-ttu-id="faf1c-130">hello-namnet som används för hello bunden data i hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="faf1c-130">hello name that is used for hello bound data in hello function.</span></span> <span data-ttu-id="faf1c-131">Detta är ett argumentnamn; för C# för JavaScript, det är hello nyckeln i en lista med nyckel/värde.</span><span class="sxs-lookup"><span data-stu-id="faf1c-131">For C#, this is an argument name; for JavaScript, it's hello key in a key/value list.</span></span> |

## <a name="function-app"></a><span data-ttu-id="faf1c-132">Funktionsapp</span><span class="sxs-lookup"><span data-stu-id="faf1c-132">Function app</span></span>
<span data-ttu-id="faf1c-133">En funktionsapp består av en eller flera enskilda funktioner som hanteras tillsammans i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="faf1c-133">A function app is comprised of one or more individual functions that are managed together by Azure App Service.</span></span> <span data-ttu-id="faf1c-134">Alla hello funktioner på en resurs som funktionen app hello samma priser plan, kontinuerlig distribution och versionen av körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="faf1c-134">All of hello functions in a function app share hello same pricing plan, continuous deployment and runtime version.</span></span> <span data-ttu-id="faf1c-135">Funktioner som skrivits i flera språk kan alla dela hello funktionsapp samma.</span><span class="sxs-lookup"><span data-stu-id="faf1c-135">Functions written in multiple languages can all share hello same function app.</span></span> <span data-ttu-id="faf1c-136">Tänk på en funktionsapp som ett sätt tooorganize och gemensamt hantera dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="faf1c-136">Think of a function app as a way tooorganize and collectively manage your functions.</span></span> 

## <a name="runtime-script-host-and-web-host"></a><span data-ttu-id="faf1c-137">Runtime (skriptvärden och webbvärd)</span><span class="sxs-lookup"><span data-stu-id="faf1c-137">Runtime (script host and web host)</span></span>
<span data-ttu-id="faf1c-138">hello runtime eller skriptvärden, är hello underliggande WebJobs SDK värden som lyssnar efter händelser, samlar in och skickar data och slutligen körs din kod.</span><span class="sxs-lookup"><span data-stu-id="faf1c-138">hello runtime, or script host, is hello underlying WebJobs SDK host that listens for events, gathers and sends data, and ultimately runs your code.</span></span> 

<span data-ttu-id="faf1c-139">toofacilitate HTTP-utlösare, det finns också en värd som är utformad toosit framför hello skriptvärden i produktion scenarier.</span><span class="sxs-lookup"><span data-stu-id="faf1c-139">toofacilitate HTTP triggers, there is also a web host that is designed toosit in front of hello script host in production scenarios.</span></span> <span data-ttu-id="faf1c-140">Med två värdar hjälper tooisolate hello script host från hello klientdelen trafiken hanteras av hello webbvärd.</span><span class="sxs-lookup"><span data-stu-id="faf1c-140">Having two hosts helps tooisolate hello script host from hello front end traffic managed by hello web host.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="faf1c-141">Mappstruktur</span><span class="sxs-lookup"><span data-stu-id="faf1c-141">Folder Structure</span></span>
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

<span data-ttu-id="faf1c-142">När inställningen upp ett projekt för distribution av funktioner tooa funktionsapp i Azure App Service, kan du behandla den här mappstrukturen som din Platskod.</span><span class="sxs-lookup"><span data-stu-id="faf1c-142">When setting-up a project for deploying functions tooa function app in Azure App Service, you can treat this folder structure as your site code.</span></span> <span data-ttu-id="faf1c-143">Du kan använda befintliga verktyg kontinuerlig integrering och distribution eller distribution av anpassade skript för detta distribuera tid installationen eller code transpilation.</span><span class="sxs-lookup"><span data-stu-id="faf1c-143">You can use existing tools like continuous integration and deployment, or custom deployment scripts for doing deploy time package installation or code transpilation.</span></span>

> [!NOTE]
> <span data-ttu-id="faf1c-144">Se till att toodeploy din `host.json` fil- och mappar direkt toohello `wwwroot` mapp.</span><span class="sxs-lookup"><span data-stu-id="faf1c-144">Make sure toodeploy your `host.json` file and function folders directly toohello `wwwroot` folder.</span></span> <span data-ttu-id="faf1c-145">Inkludera inte hello `wwwroot` mapp i dina distributioner.</span><span class="sxs-lookup"><span data-stu-id="faf1c-145">Do not include hello `wwwroot` folder in your deployments.</span></span> <span data-ttu-id="faf1c-146">Annars kan du få `wwwroot\wwwroot` mappar.</span><span class="sxs-lookup"><span data-stu-id="faf1c-146">Otherwise, you end up with `wwwroot\wwwroot` folders.</span></span> 
> 
> 

## <span data-ttu-id="faf1c-147"><a id="fileupdate"></a>Hur fungerar tooupdate programfilerna</span><span class="sxs-lookup"><span data-stu-id="faf1c-147"><a id="fileupdate"></a> How tooupdate function app files</span></span>
<span data-ttu-id="faf1c-148">hello funktionen editor inbyggd hello Azure-portalen kan du uppdatera hello *function.json* fil- och hello kodfilen för en funktion.</span><span class="sxs-lookup"><span data-stu-id="faf1c-148">hello function editor built into hello Azure portal lets you update hello *function.json* file and hello code file for a function.</span></span> <span data-ttu-id="faf1c-149">tooupload eller uppdatera andra filer som *package.json* eller *project.json* eller beroenden, du har toouse andra metoder för distribution.</span><span class="sxs-lookup"><span data-stu-id="faf1c-149">tooupload or update other files such as *package.json* or *project.json* or dependencies, you have toouse other deployment methods.</span></span>

<span data-ttu-id="faf1c-150">Funktionen appar bygger på Apptjänst, så alla hello [distributionsalternativ tillgängliga toostandard webbappar](../app-service-web/web-sites-deploy.md) är också tillgängliga för funktionen appar.</span><span class="sxs-lookup"><span data-stu-id="faf1c-150">Function apps are built on App Service, so all hello [deployment options available toostandard web apps](../app-service-web/web-sites-deploy.md) are also available for function apps.</span></span> <span data-ttu-id="faf1c-151">Här följer några metoder som du kan använda tooupload eller uppdatera funktionen programfilerna.</span><span class="sxs-lookup"><span data-stu-id="faf1c-151">Here are some methods you can use tooupload or update function app files.</span></span> 

#### <a name="toouse-app-service-editor"></a><span data-ttu-id="faf1c-152">toouse App Service Editor</span><span class="sxs-lookup"><span data-stu-id="faf1c-152">toouse App Service Editor</span></span>
1. <span data-ttu-id="faf1c-153">I hello Azure Functions-portalen klickar du på **fungerar appinställningar**.</span><span class="sxs-lookup"><span data-stu-id="faf1c-153">In hello Azure Functions portal, click **Function app settings**.</span></span>
2. <span data-ttu-id="faf1c-154">I hello **avancerade inställningar** klickar du på **gå tooApp inställningar**.</span><span class="sxs-lookup"><span data-stu-id="faf1c-154">In hello **Advanced Settings** section, click **Go tooApp Service Settings**.</span></span>
3. <span data-ttu-id="faf1c-155">Klicka på **App Service Editor** i Appmenyn Nav under **UTVECKLINGSVERKTYG**.</span><span class="sxs-lookup"><span data-stu-id="faf1c-155">Click **App Service Editor** in App Menu Nav under **DEVELOPMENT TOOLS**.</span></span>
4. <span data-ttu-id="faf1c-156">Klicka på **Gå**.</span><span class="sxs-lookup"><span data-stu-id="faf1c-156">click **Go**.</span></span>
   
   <span data-ttu-id="faf1c-157">När App Service Editor har lästs in, ser hello *host.json* fil- och mappar under *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="faf1c-157">After App Service Editor loads, you'll see hello *host.json* file and function folders under *wwwroot*.</span></span> 
5. <span data-ttu-id="faf1c-158">Öppna filer tooedit dem, eller dra och släpp från tooupload för utveckling datorfiler.</span><span class="sxs-lookup"><span data-stu-id="faf1c-158">Open files tooedit them, or drag and drop from your development machine tooupload files.</span></span>

#### <a name="toouse-hello-function-apps-scm-kudu-endpoint"></a><span data-ttu-id="faf1c-159">toouse hello funktionen appens SCM (Kudu) slutpunkt</span><span class="sxs-lookup"><span data-stu-id="faf1c-159">toouse hello function app's SCM (Kudu) endpoint</span></span>
1. <span data-ttu-id="faf1c-160">Gå till: `https://<function_app_name>.scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="faf1c-160">Navigate to: `https://<function_app_name>.scm.azurewebsites.net`.</span></span>
2. <span data-ttu-id="faf1c-161">Klicka på **Debug konsolen > CMD**.</span><span class="sxs-lookup"><span data-stu-id="faf1c-161">Click **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="faf1c-162">Navigera för`D:\home\site\wwwroot\` tooupdate *host.json* eller `D:\home\site\wwwroot\<function_name>` tooupdate filer för en funktion.</span><span class="sxs-lookup"><span data-stu-id="faf1c-162">Navigate too`D:\home\site\wwwroot\` tooupdate *host.json* or `D:\home\site\wwwroot\<function_name>` tooupdate a function's files.</span></span>
4. <span data-ttu-id="faf1c-163">Dra och släpp en fil som du vill tooupload till hello lämplig mapp i hello filen rutnät.</span><span class="sxs-lookup"><span data-stu-id="faf1c-163">Drag-and-drop a file you want tooupload into hello appropriate folder in hello file grid.</span></span> <span data-ttu-id="faf1c-164">Det finns två områden i hello filen rutnät där du kan släppa en fil.</span><span class="sxs-lookup"><span data-stu-id="faf1c-164">There are two areas in hello file grid where you can drop a file.</span></span> <span data-ttu-id="faf1c-165">För *.zip* filer, visas en ruta med hello etiketten ”dra hit tooupload och packa”.</span><span class="sxs-lookup"><span data-stu-id="faf1c-165">For *.zip* files, a box appears with hello label "Drag here tooupload and unzip."</span></span> <span data-ttu-id="faf1c-166">Ta bort i rutnätet för hello-filen men utanför hello ”packa” för andra filtyper.</span><span class="sxs-lookup"><span data-stu-id="faf1c-166">For other file types, drop in hello file grid but outside hello "unzip" box.</span></span>

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on hello consumption plan --DonnaM -->

#### <a name="toouse-continuous-deployment"></a><span data-ttu-id="faf1c-167">toouse kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="faf1c-167">toouse continuous deployment</span></span>
<span data-ttu-id="faf1c-168">Följ hello anvisningarna i avsnittet hello [kontinuerlig distribution för Azure Functions](functions-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="faf1c-168">Follow hello instructions in hello topic [Continuous deployment for Azure Functions](functions-continuous-deployment.md).</span></span>

## <a name="parallel-execution"></a><span data-ttu-id="faf1c-169">Parallell körning</span><span class="sxs-lookup"><span data-stu-id="faf1c-169">Parallel execution</span></span>
<span data-ttu-id="faf1c-170">När flera utlösande händelser inträffar snabbare än en enkeltrådig funktionen runtime kan bearbeta dem, kan hello körning anropa hello-funktionen flera gånger i parallellt.</span><span class="sxs-lookup"><span data-stu-id="faf1c-170">When multiple triggering events occur faster than a single-threaded function runtime can process them, hello runtime may invoke hello function multiple times in parallel.</span></span>  <span data-ttu-id="faf1c-171">Om en funktionsapp använder hello [förbrukning som värd för planen](functions-scale.md#how-the-consumption-plan-works), hello funktionsapp kan byggas ut automatiskt.</span><span class="sxs-lookup"><span data-stu-id="faf1c-171">If a function app is using hello [Consumption hosting plan](functions-scale.md#how-the-consumption-plan-works), hello function app could scale out automatically.</span></span>  <span data-ttu-id="faf1c-172">Varje instans av hello funktionsapp om hello appen körs på hello förbrukning värd plan eller en vanlig [Apptjänst som är värd för planen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), kan bearbeta samtidiga funktionsanrop parallellt med flera trådar.</span><span class="sxs-lookup"><span data-stu-id="faf1c-172">Each instance of hello function app, whether hello app runs on hello Consumption hosting plan or a regular [App Service hosting plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), might process concurrent function invocations in parallel using multiple threads.</span></span>  <span data-ttu-id="faf1c-173">hello maximalt antal samtidiga funktionsanrop i varje funktionen app-instansen varierar beroende på hello typen av utlösare som används samt hello resurser som används av andra funktioner i hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="faf1c-173">hello maximum number of concurrent function invocations in each function app instance varies based on hello type of trigger being used as well as hello resources used by other functions within hello function app.</span></span>

## <a name="functions-runtime-versioning"></a><span data-ttu-id="faf1c-174">Funktioner runtime versionshantering</span><span class="sxs-lookup"><span data-stu-id="faf1c-174">Functions runtime versioning</span></span>

<span data-ttu-id="faf1c-175">Du kan konfigurera hello version av hello Functions-runtime med hello `FUNCTIONS_EXTENSION_VERSION` appinställningen.</span><span class="sxs-lookup"><span data-stu-id="faf1c-175">You can configure hello version of hello Functions runtime using hello `FUNCTIONS_EXTENSION_VERSION` app setting.</span></span> <span data-ttu-id="faf1c-176">Till exempel anger hello värdet ”~ 1” att appen funktionen ska använda 1 enligt dess huvudversion.</span><span class="sxs-lookup"><span data-stu-id="faf1c-176">For example, hello value "~1" indicates that your Function App will use 1 as its major version.</span></span> <span data-ttu-id="faf1c-177">Funktionen appar är uppgraderade tooeach nya delversion när de blir tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="faf1c-177">Function Apps are upgraded tooeach new minor version as they are released.</span></span> <span data-ttu-id="faf1c-178">Du kan visa hello exakt vilken version av appen funktionen i hello **inställningar** fliken i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="faf1c-178">You can view hello exact version of your Function App in hello **Settings** tab in hello Azure Portal.</span></span>

## <a name="repositories"></a><span data-ttu-id="faf1c-179">Centrallager</span><span class="sxs-lookup"><span data-stu-id="faf1c-179">Repositories</span></span>
<span data-ttu-id="faf1c-180">hello-koden för Azure Functions är öppen källkod och lagras i GitHub-databaser:</span><span class="sxs-lookup"><span data-stu-id="faf1c-180">hello code for Azure Functions is open source and stored in GitHub repositories:</span></span>

* [<span data-ttu-id="faf1c-181">Azure Functions-runtime</span><span class="sxs-lookup"><span data-stu-id="faf1c-181">Azure Functions runtime</span></span>](https://github.com/Azure/azure-webjobs-sdk-script/)
* [<span data-ttu-id="faf1c-182">Azure Functions-portalen</span><span class="sxs-lookup"><span data-stu-id="faf1c-182">Azure Functions portal</span></span>](https://github.com/projectkudu/AzureFunctionsPortal)
* [<span data-ttu-id="faf1c-183">Azure Functions-mallar</span><span class="sxs-lookup"><span data-stu-id="faf1c-183">Azure Functions templates</span></span>](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [<span data-ttu-id="faf1c-184">Azure WebJobs-SDK</span><span class="sxs-lookup"><span data-stu-id="faf1c-184">Azure WebJobs SDK</span></span>](https://github.com/Azure/azure-webjobs-sdk/)
* [<span data-ttu-id="faf1c-185">Azure WebJobs-SDK-tillägg</span><span class="sxs-lookup"><span data-stu-id="faf1c-185">Azure WebJobs SDK Extensions</span></span>](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a><span data-ttu-id="faf1c-186">Bindningar</span><span class="sxs-lookup"><span data-stu-id="faf1c-186">Bindings</span></span>
<span data-ttu-id="faf1c-187">Här är en tabell med alla bindningar som stöds.</span><span class="sxs-lookup"><span data-stu-id="faf1c-187">Here is a table of all supported bindings.</span></span>

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a><span data-ttu-id="faf1c-188">Rapportera problem</span><span class="sxs-lookup"><span data-stu-id="faf1c-188">Reporting Issues</span></span>
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a><span data-ttu-id="faf1c-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="faf1c-189">Next steps</span></span>
<span data-ttu-id="faf1c-190">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="faf1c-190">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="faf1c-191">Metodtips för Azure Functions</span><span class="sxs-lookup"><span data-stu-id="faf1c-191">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="faf1c-192">Azure Functions C# för utvecklare</span><span class="sxs-lookup"><span data-stu-id="faf1c-192">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="faf1c-193">Azure Functions F # för utvecklare</span><span class="sxs-lookup"><span data-stu-id="faf1c-193">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="faf1c-194">Azure Functions NodeJS för utvecklare</span><span class="sxs-lookup"><span data-stu-id="faf1c-194">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="faf1c-195">Azure Functions-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="faf1c-195">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)
* <span data-ttu-id="faf1c-196">[Azure Functions: hello resa](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) på hello Azure App Service-teamets blogg.</span><span class="sxs-lookup"><span data-stu-id="faf1c-196">[Azure Functions: hello Journey](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) on hello Azure App Service team blog.</span></span> <span data-ttu-id="faf1c-197">En historik över hur Azure Functions utvecklades.</span><span class="sxs-lookup"><span data-stu-id="faf1c-197">A history of how Azure Functions was developed.</span></span>

