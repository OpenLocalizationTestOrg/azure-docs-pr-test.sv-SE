---
title: "aaaDevelop och kör Azure functions lokalt | Microsoft Docs"
description: "Lär dig hur toocode och testa Azure functions lokalt på datorn innan du kör dem på Azure Functions."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 07/12/2017
ms.author: glenga
ms.openlocfilehash: 342ed4d6df41a2d2df9067948e19e347bb52c844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="code-and-test-azure-functions-locally"></a><span data-ttu-id="838c1-103">Platskod och testa Azure functions lokalt</span><span class="sxs-lookup"><span data-stu-id="838c1-103">Code and test Azure functions locally</span></span>

<span data-ttu-id="838c1-104">När hello [Azure-portalen] ger en fullständig uppsättning av verktyg för att utveckla och testa Azure Functions, många utvecklare föredrar en lokal utveckling upplevelse.</span><span class="sxs-lookup"><span data-stu-id="838c1-104">While hello [Azure portal] provides a full set of tools for developing and testing Azure Functions, many developers prefer a local development experience.</span></span> <span data-ttu-id="838c1-105">Azure Functions gör det enkelt toouse din favorit code redigerare och lokal utveckling verktyg toodevelop och testa dina funktioner på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="838c1-105">Azure Functions makes it easy toouse your favorite code editor and local development tools toodevelop and test your functions on your local computer.</span></span> <span data-ttu-id="838c1-106">Dina funktioner kan utlösa händelser i Azure och du kan felsöka ditt C# och JavaScript-funktioner på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="838c1-106">Your functions can trigger on events in Azure, and you can debug your C# and JavaScript functions on your local computer.</span></span> 

<span data-ttu-id="838c1-107">Om du är en Visual Studio C# utvecklare Azure Functions även [kan integreras med Visual Studio 2017](functions-develop-vs.md).</span><span class="sxs-lookup"><span data-stu-id="838c1-107">If you are a Visual Studio C# developer, Azure Functions also [integrates with Visual Studio 2017](functions-develop-vs.md).</span></span>

## <a name="install-hello-azure-functions-core-tools"></a><span data-ttu-id="838c1-108">Installera verktyg för hello Azure Functions kärnor</span><span class="sxs-lookup"><span data-stu-id="838c1-108">Install hello Azure Functions Core Tools</span></span>

<span data-ttu-id="838c1-109">Azure Functions grundläggande verktyg är en lokal version av hello Azure Functions-runtime som du kan köra på den lokala Windows-datorn.</span><span class="sxs-lookup"><span data-stu-id="838c1-109">Azure Functions Core Tools is a local version of hello Azure Functions runtime that you can run on your local Windows computer.</span></span> <span data-ttu-id="838c1-110">Det är inte en emulator eller simulatorn.</span><span class="sxs-lookup"><span data-stu-id="838c1-110">It's not an emulator or simulator.</span></span> <span data-ttu-id="838c1-111">Det har hello samma runtime som stänger funktioner i Azure.</span><span class="sxs-lookup"><span data-stu-id="838c1-111">It's hello same runtime that powers Functions in Azure.</span></span>

<span data-ttu-id="838c1-112">Hej [Azure Functions grundläggande verktyg] tillhandahålls som ett npm.</span><span class="sxs-lookup"><span data-stu-id="838c1-112">hello [Azure Functions Core Tools] is provided as an npm package.</span></span> <span data-ttu-id="838c1-113">Du måste först [installera NodeJS](https://docs.npmjs.com/getting-started/installing-node), som innehåller npm.</span><span class="sxs-lookup"><span data-stu-id="838c1-113">You must first [install NodeJS](https://docs.npmjs.com/getting-started/installing-node), which includes npm.</span></span>  

>[!NOTE]
><span data-ttu-id="838c1-114">För tillfället installeras hello Azure Functions Core-verktygspaketet bara på Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="838c1-114">At this time, hello Azure Functions Core Tools package can only be installed on Windows computers.</span></span> <span data-ttu-id="838c1-115">Den här begränsningen är på grund av tooa tillfälliga begränsningen i hello funktioner värden.</span><span class="sxs-lookup"><span data-stu-id="838c1-115">This restriction is due tooa temporary limitation in hello Functions host.</span></span>

<span data-ttu-id="838c1-116">[Azure Functions grundläggande verktyg] lägger du till följande kommando alias hello:</span><span class="sxs-lookup"><span data-stu-id="838c1-116">[Azure Functions Core Tools] adds hello following command aliases:</span></span>
* <span data-ttu-id="838c1-117">**funktionen**</span><span class="sxs-lookup"><span data-stu-id="838c1-117">**func**</span></span>
* <span data-ttu-id="838c1-118">**azfun**</span><span class="sxs-lookup"><span data-stu-id="838c1-118">**azfun**</span></span>
* <span data-ttu-id="838c1-119">**azurefunctions**</span><span class="sxs-lookup"><span data-stu-id="838c1-119">**azurefunctions**</span></span>

<span data-ttu-id="838c1-120">Alla dessa alias kan användas i stället för `func` visas i hello exemplen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="838c1-120">All of these alias can be used instead of `func` shown in hello examples in this topic.</span></span>

## <a name="create-a-local-functions-project"></a><span data-ttu-id="838c1-121">Skapa ett projekt med lokala funktioner</span><span class="sxs-lookup"><span data-stu-id="838c1-121">Create a local Functions project</span></span>

<span data-ttu-id="838c1-122">När du kör lokalt är ett funktioner projekt en katalog som har hello filer host.json och local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="838c1-122">When running locally, a Functions project is a directory that has hello files host.json and local.settings.json.</span></span> <span data-ttu-id="838c1-123">Den här katalogen är hello motsvarar en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="838c1-123">This directory is hello equivalent of a function app in Azure.</span></span> <span data-ttu-id="838c1-124">toolearn mer om hello Azure Functions mappstrukturen finns hello [Azure Functions utvecklarguide för](functions-reference.md#folder-structure).</span><span class="sxs-lookup"><span data-stu-id="838c1-124">toolearn more about hello Azure Functions folder structure, see hello [Azure Functions developers guide](functions-reference.md#folder-structure).</span></span>

<span data-ttu-id="838c1-125">Kör hello följande kommando vid en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="838c1-125">At a command prompt, run hello following command:</span></span>

```
func init MyFunctionProj
```

<span data-ttu-id="838c1-126">hello utdata ser ut som följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="838c1-126">hello output looks like hello following example:</span></span>

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

<span data-ttu-id="838c1-127">tooopt out-of-skapa en Git-lagringsplatsen, Använd hello alternativet `--no-source-control [-n]`.</span><span class="sxs-lookup"><span data-stu-id="838c1-127">tooopt out of creating a Git repository, use hello option `--no-source-control [-n]`.</span></span>

<a name="local-settings"></a>

## <a name="local-settings-file"></a><span data-ttu-id="838c1-128">Lokala inställningsfilen</span><span class="sxs-lookup"><span data-stu-id="838c1-128">Local settings file</span></span>

<span data-ttu-id="838c1-129">hello filen local.settings.json lagrar app-inställningar, anslutningssträngar och inställningar för Azure Functions Core verktyg.</span><span class="sxs-lookup"><span data-stu-id="838c1-129">hello file local.settings.json stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="838c1-130">Det har hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="838c1-130">It has hello following structure:</span></span>

```json
{
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>", 
  },
  "Host": {
    "LocalHttpPort": 7071, 
    "CORS": "*" 
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```
| <span data-ttu-id="838c1-131">Inställning</span><span class="sxs-lookup"><span data-stu-id="838c1-131">Setting</span></span>      | <span data-ttu-id="838c1-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="838c1-132">Description</span></span>                            |
| ------------ | -------------------------------------- |
| <span data-ttu-id="838c1-133">**IsEncrypted**</span><span class="sxs-lookup"><span data-stu-id="838c1-133">**IsEncrypted**</span></span> | <span data-ttu-id="838c1-134">När värdet för**SANT**, alla värden som är krypterade med en lokal dator-nyckel.</span><span class="sxs-lookup"><span data-stu-id="838c1-134">When set too**true**, all values are encrypted using a local machine key.</span></span> <span data-ttu-id="838c1-135">Används med `func settings` kommandon.</span><span class="sxs-lookup"><span data-stu-id="838c1-135">Used with `func settings` commands.</span></span> <span data-ttu-id="838c1-136">Standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="838c1-136">Default value is **false**.</span></span> |
| <span data-ttu-id="838c1-137">**Värden**</span><span class="sxs-lookup"><span data-stu-id="838c1-137">**Values**</span></span> | <span data-ttu-id="838c1-138">Samling av programinställningar som används när du kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="838c1-138">Collection of application settings used when running locally.</span></span> <span data-ttu-id="838c1-139">Lägg till toothis för programmet inställningsobjektet.</span><span class="sxs-lookup"><span data-stu-id="838c1-139">Add your application settings toothis object.</span></span>  |
| <span data-ttu-id="838c1-140">**AzureWebJobsStorage**</span><span class="sxs-lookup"><span data-stu-id="838c1-140">**AzureWebJobsStorage**</span></span> | <span data-ttu-id="838c1-141">Anger hello anslutning sträng toohello Azure Storage-konto som används internt av hello Azure Functions-runtime.</span><span class="sxs-lookup"><span data-stu-id="838c1-141">Sets hello connection string toohello Azure Storage account that is used internally by hello Azure Functions runtime.</span></span> <span data-ttu-id="838c1-142">hello storage-konto har stöd för din funktion utlösare.</span><span class="sxs-lookup"><span data-stu-id="838c1-142">hello storage account supports your function's triggers.</span></span> <span data-ttu-id="838c1-143">Inställningen storage-konto anslutning krävs för alla funktioner utom HTTP utlöses funktioner.</span><span class="sxs-lookup"><span data-stu-id="838c1-143">This storage account connection setting is required for all functions except for HTTP triggered functions.</span></span>  |
| <span data-ttu-id="838c1-144">**AzureWebJobsDashboard**</span><span class="sxs-lookup"><span data-stu-id="838c1-144">**AzureWebJobsDashboard**</span></span> | <span data-ttu-id="838c1-145">Anger hello anslutning sträng toohello Azure Storage-konto som används toostore hello funktionsloggar.</span><span class="sxs-lookup"><span data-stu-id="838c1-145">Sets hello connection string toohello Azure Storage account that is used toostore hello function logs.</span></span> <span data-ttu-id="838c1-146">Det här valfria värdet gör hello loggar tillgängliga i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="838c1-146">This optional value makes hello logs accessible in hello portal.</span></span>|
| <span data-ttu-id="838c1-147">**Värden**</span><span class="sxs-lookup"><span data-stu-id="838c1-147">**Host**</span></span> | <span data-ttu-id="838c1-148">Inställningarna i det här avsnittet Anpassa hello värdprocess för funktioner när du kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="838c1-148">Settings in this section customize hello Functions host process when running locally.</span></span> | 
| <span data-ttu-id="838c1-149">**LocalHttpPort**</span><span class="sxs-lookup"><span data-stu-id="838c1-149">**LocalHttpPort**</span></span> | <span data-ttu-id="838c1-150">Anger hello standardport som används när du kör hello lokala funktioner värden (`func host start` och `func run`).</span><span class="sxs-lookup"><span data-stu-id="838c1-150">Sets hello default port used when running hello local Functions host (`func host start` and `func run`).</span></span> <span data-ttu-id="838c1-151">Hej `--port` kommandoradsalternativet har högre prioritet än detta värde.</span><span class="sxs-lookup"><span data-stu-id="838c1-151">hello `--port` command-line option takes precedence over this value.</span></span> |
| <span data-ttu-id="838c1-152">**CORS**</span><span class="sxs-lookup"><span data-stu-id="838c1-152">**CORS**</span></span> | <span data-ttu-id="838c1-153">Definierar hello ursprung tillåts för [resursdelning för korsande ursprung (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="838c1-153">Defines hello origins allowed for [cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span> <span data-ttu-id="838c1-154">Ursprung tillhandahålls som en kommaavgränsad lista med inga blanksteg.</span><span class="sxs-lookup"><span data-stu-id="838c1-154">Origins are supplied as a comma-separated list with no spaces.</span></span> <span data-ttu-id="838c1-155">Hej jokertecknet (**\***) stöds, vilket gör att begäranden från alla ursprung.</span><span class="sxs-lookup"><span data-stu-id="838c1-155">hello wildcard value (**\***) is supported, which allows requests from any origin.</span></span> |
| <span data-ttu-id="838c1-156">**ConnectionStrings**</span><span class="sxs-lookup"><span data-stu-id="838c1-156">**ConnectionStrings**</span></span> | <span data-ttu-id="838c1-157">Innehåller hello databasanslutningssträngar för dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="838c1-157">Contains hello database connection strings for your functions.</span></span> <span data-ttu-id="838c1-158">Anslutningssträngar i det här objektet läggs toohello miljö med hello providertyp av **System.Data.SqlClient**.</span><span class="sxs-lookup"><span data-stu-id="838c1-158">Connection strings in this object are added toohello environment with hello provider type of **System.Data.SqlClient**.</span></span>  | 

<span data-ttu-id="838c1-159">De flesta utlösare och bindningar har en **anslutning** egenskap som mappar toohello namnet på en miljöinställning för variabeln eller app.</span><span class="sxs-lookup"><span data-stu-id="838c1-159">Most triggers and bindings have a **Connection** property that maps toohello name of an environment variable or app setting.</span></span> <span data-ttu-id="838c1-160">För varje anslutning-egenskap måste det finnas appinställningen som definierats i local.settings.json-filen.</span><span class="sxs-lookup"><span data-stu-id="838c1-160">For each connection property, there must be app setting defined in local.settings.json file.</span></span> 

<span data-ttu-id="838c1-161">De här inställningarna kan också läsa i koden som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="838c1-161">These settings can also be read in your code as environment variables.</span></span> <span data-ttu-id="838c1-162">I C#, använda [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) eller [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="838c1-162">In C#, use [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) or [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span></span> <span data-ttu-id="838c1-163">I JavaScript, använda `process.env`.</span><span class="sxs-lookup"><span data-stu-id="838c1-163">In JavaScript, use `process.env`.</span></span> <span data-ttu-id="838c1-164">Inställningarna som en systemmiljövariabler åsidosätter värden i hello local.settings.json filen.</span><span class="sxs-lookup"><span data-stu-id="838c1-164">Settings specified as a system environment variable take precedence over values in hello local.settings.json file.</span></span> 

<span data-ttu-id="838c1-165">Inställningarna i hello local.settings.json filen används endast av funktioner verktyg när du kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="838c1-165">Settings in hello local.settings.json file are only used by Functions tools when running locally.</span></span> <span data-ttu-id="838c1-166">Som standard dessa inställningar migreras inte automatiskt när hello projektet är publicerade tooAzure.</span><span class="sxs-lookup"><span data-stu-id="838c1-166">By default, these settings are not migrated automatically when hello project is published tooAzure.</span></span> <span data-ttu-id="838c1-167">Använd hello `--publish-local-settings` växla [när du publicerar](#publish) toomake att dessa inställningar har lagts till toohello funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="838c1-167">Use hello `--publish-local-settings` switch [when you publish](#publish) toomake sure these settings are added toohello function app in Azure.</span></span>

<span data-ttu-id="838c1-168">Om ingen giltig lagringsanslutningssträng anges för **AzureWebJobsStorage**, hello följande felmeddelande visas:</span><span class="sxs-lookup"><span data-stu-id="838c1-168">When no valid storage connection string is set for **AzureWebJobsStorage**, hello following error message is shown:</span></span>  

><span data-ttu-id="838c1-169">Värde saknas för AzureWebJobsStorage i local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="838c1-169">Missing value for AzureWebJobsStorage in local.settings.json.</span></span> <span data-ttu-id="838c1-170">Detta krävs för alla utlösare än HTTP.</span><span class="sxs-lookup"><span data-stu-id="838c1-170">This is required for all triggers other than HTTP.</span></span> <span data-ttu-id="838c1-171">Du kan köra ' func azure functionary fetch-app-inställningar <functionAppName>' eller ange en anslutningssträng i local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="838c1-171">You can run 'func azure functionary fetch-app-settings <functionAppName>' or specify a connection string in local.settings.json.</span></span>
  
[!INCLUDE [Note toonot use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a><span data-ttu-id="838c1-172">Konfigurera appinställningar</span><span class="sxs-lookup"><span data-stu-id="838c1-172">Configure app settings</span></span>

<span data-ttu-id="838c1-173">tooset ett värde för anslutningssträngar, du kan göra något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="838c1-173">tooset a value for connection strings, you can do one of hello following:</span></span>
* <span data-ttu-id="838c1-174">Ange hello anslutningssträng från [Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="838c1-174">Enter hello connection string from [Azure Storage Explorer](http://storageexplorer.com/).</span></span>
* <span data-ttu-id="838c1-175">Använd någon av följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="838c1-175">Use one of hello following commands:</span></span>

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    <span data-ttu-id="838c1-176">Båda kommandon måste du toofirst inloggning tooAzure.</span><span class="sxs-lookup"><span data-stu-id="838c1-176">Both commands require you toofirst sign-in tooAzure.</span></span>

## <a name="create-a-function"></a><span data-ttu-id="838c1-177">Skapa en funktion</span><span class="sxs-lookup"><span data-stu-id="838c1-177">Create a function</span></span>

<span data-ttu-id="838c1-178">toocreate en funktion som kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="838c1-178">toocreate a function, run hello following command:</span></span>

```
func new
``` 
<span data-ttu-id="838c1-179">`func new`stöder hello följande valfria argument:</span><span class="sxs-lookup"><span data-stu-id="838c1-179">`func new` supports hello following optional arguments:</span></span>

| <span data-ttu-id="838c1-180">Argumentet</span><span class="sxs-lookup"><span data-stu-id="838c1-180">Argument</span></span>     | <span data-ttu-id="838c1-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="838c1-181">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | <span data-ttu-id="838c1-182">hello mall programmeringsspråket, till exempel C#, F # eller JavaScript.</span><span class="sxs-lookup"><span data-stu-id="838c1-182">hello template programming language, such as C#, F#, or JavaScript.</span></span> |
| **`--template -t`** | <span data-ttu-id="838c1-183">hello mallnamn.</span><span class="sxs-lookup"><span data-stu-id="838c1-183">hello template name.</span></span> |
| **`--name -n`** | <span data-ttu-id="838c1-184">hello funktionsnamn.</span><span class="sxs-lookup"><span data-stu-id="838c1-184">hello function name.</span></span> |

<span data-ttu-id="838c1-185">Till exempel toocreate en JavaScript-HTTP-utlösare, kör:</span><span class="sxs-lookup"><span data-stu-id="838c1-185">For example, toocreate a JavaScript HTTP trigger, run:</span></span>

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

<span data-ttu-id="838c1-186">toocreate en funktion som utlöses av kön, kör:</span><span class="sxs-lookup"><span data-stu-id="838c1-186">toocreate a queue-triggered function, run:</span></span>

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a><span data-ttu-id="838c1-187">Kör funktioner lokalt</span><span class="sxs-lookup"><span data-stu-id="838c1-187">Run functions locally</span></span>

<span data-ttu-id="838c1-188">toorun köra hello funktioner värden för en funktion i projektet.</span><span class="sxs-lookup"><span data-stu-id="838c1-188">toorun a Functions project, run hello Functions host.</span></span> <span data-ttu-id="838c1-189">hello-värden kan utlösare för alla funktioner i hello projekt:</span><span class="sxs-lookup"><span data-stu-id="838c1-189">hello host enables triggers for all functions in hello project:</span></span>

```
func host start
```

<span data-ttu-id="838c1-190">`func host start`stöder hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="838c1-190">`func host start` supports hello following options:</span></span>

| <span data-ttu-id="838c1-191">Alternativ</span><span class="sxs-lookup"><span data-stu-id="838c1-191">Option</span></span>     | <span data-ttu-id="838c1-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="838c1-192">Description</span></span>                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | <span data-ttu-id="838c1-193">hello lokal port toolisten på.</span><span class="sxs-lookup"><span data-stu-id="838c1-193">hello local port toolisten on.</span></span> <span data-ttu-id="838c1-194">Standardvärde: 7071.</span><span class="sxs-lookup"><span data-stu-id="838c1-194">Default value: 7071.</span></span> |
| **`--debug <type>`** | <span data-ttu-id="838c1-195">hello alternativ är `VSCode` och `VS`.</span><span class="sxs-lookup"><span data-stu-id="838c1-195">hello options are `VSCode` and `VS`.</span></span> |
| **`--cors`** | <span data-ttu-id="838c1-196">En kommaavgränsad lista över CORS ursprung, utan blanksteg.</span><span class="sxs-lookup"><span data-stu-id="838c1-196">A comma-separated list of CORS origins, with no spaces.</span></span> |
| **`--nodeDebugPort -n`** | <span data-ttu-id="838c1-197">hello-port för hello nod felsökare toouse.</span><span class="sxs-lookup"><span data-stu-id="838c1-197">hello port for hello node debugger toouse.</span></span> <span data-ttu-id="838c1-198">Standard: Ett värde från launch.json eller 5858.</span><span class="sxs-lookup"><span data-stu-id="838c1-198">Default: A value from launch.json or 5858.</span></span> |
| **`--debugLevel -d`** | <span data-ttu-id="838c1-199">hello konsolen spårningsnivån (av, verbose, info, warning eller error).</span><span class="sxs-lookup"><span data-stu-id="838c1-199">hello console trace level (off, verbose, info, warning, or error).</span></span> <span data-ttu-id="838c1-200">Standard: Info.</span><span class="sxs-lookup"><span data-stu-id="838c1-200">Default: Info.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="838c1-201">hello tidsgräns för hello funktioner värden t o start, i sekunder.</span><span class="sxs-lookup"><span data-stu-id="838c1-201">hello time out for hello Functions host t     o start, in seconds.</span></span> <span data-ttu-id="838c1-202">Standard: 20 sekunder.</span><span class="sxs-lookup"><span data-stu-id="838c1-202">Default: 20 seconds.</span></span>|
| **`--useHttps`** | <span data-ttu-id="838c1-203">Binda toohttps://localhost: {porten} i stället för toohttp://localhost: {port}.</span><span class="sxs-lookup"><span data-stu-id="838c1-203">Bind toohttps://localhost:{port} rather than toohttp://localhost:{port}.</span></span> <span data-ttu-id="838c1-204">Som standard skapas ett betrott certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="838c1-204">By default, this option creates a trusted certificate on your computer.</span></span>|
| **`--pause-on-error`** | <span data-ttu-id="838c1-205">Pausa för ytterligare information innan du avslutar hello-processen.</span><span class="sxs-lookup"><span data-stu-id="838c1-205">Pause for additional input before exiting hello process.</span></span> <span data-ttu-id="838c1-206">Användbar när den startas Azure Functions grundläggande verktyg från en integrerad utvecklingsmiljö (IDE).</span><span class="sxs-lookup"><span data-stu-id="838c1-206">Useful when launching Azure Functions Core Tools from an integrated development environment (IDE).</span></span>|

<span data-ttu-id="838c1-207">När hello funktioner värden startar utdata hello URL: en för HTTP-utlösta funktioner:</span><span class="sxs-lookup"><span data-stu-id="838c1-207">When hello Functions host starts, it outputs hello URL of HTTP-triggered functions:</span></span>

```
Found hello following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a><span data-ttu-id="838c1-208">Felsöka i VS-kod eller Visual Studio</span><span class="sxs-lookup"><span data-stu-id="838c1-208">Debug in VS Code or Visual Studio</span></span>

<span data-ttu-id="838c1-209">tooattach en felsökare skicka hello `--debug` argumentet.</span><span class="sxs-lookup"><span data-stu-id="838c1-209">tooattach a debugger, pass hello `--debug` argument.</span></span> <span data-ttu-id="838c1-210">toodebug JavaScript-funktioner, använda Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="838c1-210">toodebug JavaScript functions, use Visual Studio Code.</span></span> <span data-ttu-id="838c1-211">Använda Visual Studio för C#.</span><span class="sxs-lookup"><span data-stu-id="838c1-211">For C# functions, use Visual Studio.</span></span>

<span data-ttu-id="838c1-212">toodebug C#-funktion, Använd `--debug vs`.</span><span class="sxs-lookup"><span data-stu-id="838c1-212">toodebug C# functions, use `--debug vs`.</span></span> <span data-ttu-id="838c1-213">Du kan också använda [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="838c1-213">You can also use [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span> 

<span data-ttu-id="838c1-214">toolaunch hello värden och Ställ in JavaScript-felsökning, kör:</span><span class="sxs-lookup"><span data-stu-id="838c1-214">toolaunch hello host and set up JavaScript debugging, run:</span></span>

```
func host start --debug vscode
```

<span data-ttu-id="838c1-215">Klicka i Visual Studio Code hello **felsöka** väljer **bifoga tooAzure funktioner**.</span><span class="sxs-lookup"><span data-stu-id="838c1-215">Then, in Visual Studio Code, in hello **Debug** view, select **Attach tooAzure Functions**.</span></span> <span data-ttu-id="838c1-216">Du kan koppla brytpunkter inspektera variabler och stega igenom koden.</span><span class="sxs-lookup"><span data-stu-id="838c1-216">You can attach breakpoints, inspect variables, and step through code.</span></span>

![JavaScript-felsökning med Visual Studio Code](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-tooa-function"></a><span data-ttu-id="838c1-218">Skicka test data tooa funktion</span><span class="sxs-lookup"><span data-stu-id="838c1-218">Passing test data tooa function</span></span>

<span data-ttu-id="838c1-219">Du kan även anropa en funktion direkt med hjälp av `func run <FunctionName>` och ange indata för hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="838c1-219">You can also invoke a function directly by using `func run <FunctionName>` and provide input data for hello function.</span></span> <span data-ttu-id="838c1-220">Det här kommandot är liknande toorunning en funktion med hjälp av hello **Test** fliken i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="838c1-220">This command is similar toorunning a function using hello **Test** tab in hello Azure portal.</span></span> <span data-ttu-id="838c1-221">Detta kommando startar hello hela funktioner värden.</span><span class="sxs-lookup"><span data-stu-id="838c1-221">This command launches hello entire Functions host.</span></span>

<span data-ttu-id="838c1-222">`func run`stöder hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="838c1-222">`func run` supports hello following options:</span></span>

| <span data-ttu-id="838c1-223">Alternativ</span><span class="sxs-lookup"><span data-stu-id="838c1-223">Option</span></span>     | <span data-ttu-id="838c1-224">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="838c1-224">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | <span data-ttu-id="838c1-225">Infogade innehållet.</span><span class="sxs-lookup"><span data-stu-id="838c1-225">Inline content.</span></span> |
| **`--debug -d`** | <span data-ttu-id="838c1-226">Koppla en felsökare toohello värdprocess innan du kör hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="838c1-226">Attach a debugger toohello host process before running hello function.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="838c1-227">Tid toowait (i sekunder) tills hello lokala funktioner värden är klar.</span><span class="sxs-lookup"><span data-stu-id="838c1-227">Time toowait (in seconds) until hello local Functions host is ready.</span></span>|
| **`--file -f`** | <span data-ttu-id="838c1-228">hello-filen namnet toouse som innehåll.</span><span class="sxs-lookup"><span data-stu-id="838c1-228">hello file name toouse as content.</span></span>|
| **`--no-interactive`** | <span data-ttu-id="838c1-229">Begär inte indata.</span><span class="sxs-lookup"><span data-stu-id="838c1-229">Does not prompt for input.</span></span> <span data-ttu-id="838c1-230">Bra automation.</span><span class="sxs-lookup"><span data-stu-id="838c1-230">Useful for automation scenarios.</span></span>|

<span data-ttu-id="838c1-231">Toocall en HTTP-utlösta funktion och skicka innehållsavsnitt, kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="838c1-231">For example, toocall an HTTP-triggered function and pass content body, run hello following command:</span></span>

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <span data-ttu-id="838c1-232"><a name="publish"></a>Publicera tooAzure</span><span class="sxs-lookup"><span data-stu-id="838c1-232"><a name="publish"></a>Publish tooAzure</span></span>

<span data-ttu-id="838c1-233">toopublish en funktion projektet tooa funktionsapp i Azure, Använd hello `publish` kommando:</span><span class="sxs-lookup"><span data-stu-id="838c1-233">toopublish a Functions project tooa function app in Azure, use hello `publish` command:</span></span>

```
func azure functionapp publish <FunctionAppName>
```

<span data-ttu-id="838c1-234">Du kan använda hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="838c1-234">You can use hello following options:</span></span>

| <span data-ttu-id="838c1-235">Alternativ</span><span class="sxs-lookup"><span data-stu-id="838c1-235">Option</span></span>     | <span data-ttu-id="838c1-236">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="838c1-236">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  <span data-ttu-id="838c1-237">Publiceringsinställning i local.settings.json tooAzure, fråga toooverwrite om hello inställningen redan finns.</span><span class="sxs-lookup"><span data-stu-id="838c1-237">Publish settings in local.settings.json tooAzure, prompting toooverwrite if hello setting already exists.</span></span>|
| **`--overwrite-settings -y`** | <span data-ttu-id="838c1-238">Måste användas med `-i`.</span><span class="sxs-lookup"><span data-stu-id="838c1-238">Must be used with `-i`.</span></span> <span data-ttu-id="838c1-239">Skriver över AppSettings i Azure med lokalt värde om olika.</span><span class="sxs-lookup"><span data-stu-id="838c1-239">Overwrites AppSettings in Azure with local value if different.</span></span> <span data-ttu-id="838c1-240">Standardvärdet är fråga.</span><span class="sxs-lookup"><span data-stu-id="838c1-240">Default is prompt.</span></span>|

<span data-ttu-id="838c1-241">Hej `publish` kommandot överför hello innehållet i hello funktioner projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="838c1-241">hello `publish` command uploads hello contents of hello Functions project directory.</span></span> <span data-ttu-id="838c1-242">Om du tar bort filer lokalt hello `publish` kommandot tar inte bort dem från Azure.</span><span class="sxs-lookup"><span data-stu-id="838c1-242">If you delete files locally, hello `publish` command does not delete them from Azure.</span></span> <span data-ttu-id="838c1-243">Du kan ta bort filer i Azure med hjälp av hello [Kudu verktyget](functions-how-to-use-azure-function-app-settings.md#kudu) i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="838c1-243">You can delete files in Azure by using hello [Kudu tool](functions-how-to-use-azure-function-app-settings.md#kudu) in hello [Azure portal].</span></span>

## <a name="next-steps"></a><span data-ttu-id="838c1-244">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="838c1-244">Next steps</span></span>

<span data-ttu-id="838c1-245">Azure Functions grundläggande verktyg är [öppna datakällan och finns på GitHub](https://github.com/azure/azure-functions-cli).</span><span class="sxs-lookup"><span data-stu-id="838c1-245">Azure Functions Core Tools is [open source and hosted on GitHub](https://github.com/azure/azure-functions-cli).</span></span>  
<span data-ttu-id="838c1-246">toofile en bugg eller funktion begäran [öppna ett problem med GitHub](https://github.com/azure/azure-functions-cli/issues).</span><span class="sxs-lookup"><span data-stu-id="838c1-246">toofile a bug or feature request, [open a GitHub issue](https://github.com/azure/azure-functions-cli/issues).</span></span> 

<!-- LINKS -->

[Azure Functions grundläggande verktyg]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-portalen]: https://portal.azure.com 
