---
title: aaaDevelop Azure Functions med Visual Studio | Microsoft Docs
description: "Lär dig hur toodevelop och testa Azure Functions med hjälp av Azure Functions verktyg för Visual Studio-2017."
services: functions
documentationcenter: .net
author: ggailey777
manager: erikre
editor: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: glenga
ms.openlocfilehash: c9baf882bf58068cb9a8930bea337fe51b2a77ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-tools-for-visual-studio"></a><span data-ttu-id="335e6-103">Azure Functions Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="335e6-103">Azure Functions Tools for Visual Studio</span></span>  

<span data-ttu-id="335e6-104">Azure Functions-verktyg för Visual Studio-2017 är ett tillägg för Visual Studio som låter dig utveckla, testa och distribuera C# funktioner tooAzure.</span><span class="sxs-lookup"><span data-stu-id="335e6-104">Azure Functions Tools for Visual Studio 2017 is an extension for Visual Studio that lets you develop, test, and deploy C# functions tooAzure.</span></span> <span data-ttu-id="335e6-105">Om det här är din första upplevelse med Azure Functions kan du läsa mer i [en introduktion tooAzure fungerar](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="335e6-105">If this is your first experience with Azure Functions, you can learn more at [An introduction tooAzure Functions](functions-overview.md).</span></span>

<span data-ttu-id="335e6-106">hello Azure Functions verktyg ger hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="335e6-106">hello Azure Functions Tools provides hello following benefits:</span></span> 

* <span data-ttu-id="335e6-107">Redigera, skapa och köra funktioner på lokala utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="335e6-107">Edit, build, and run functions on your local development computer.</span></span> 
* <span data-ttu-id="335e6-108">Publicera Azure Functions projektet direkt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="335e6-108">Publish your Azure Functions project directly tooAzure.</span></span> 
* <span data-ttu-id="335e6-109">Använda WebJobs attribut toodeclare funktionsbindningar direkt i hello C#-kod i stället för att upprätthålla en separat function.json för bindning definitioner.</span><span class="sxs-lookup"><span data-stu-id="335e6-109">Use WebJobs attributes toodeclare function bindings directly in hello C# code instead of maintaining a separate function.json for binding definitions.</span></span>
* <span data-ttu-id="335e6-110">Utveckla och distribuera före kompilerade C#-funktioner.</span><span class="sxs-lookup"><span data-stu-id="335e6-110">Develop and deploy pre-compiled C# functions.</span></span> <span data-ttu-id="335e6-111">Före följt funktioner ge en bättre kall start prestanda än C# skript-baserad funktion.</span><span class="sxs-lookup"><span data-stu-id="335e6-111">Pre-complied functions provide a better cold-start performance than C# script-based functions.</span></span> 
* <span data-ttu-id="335e6-112">Felkod dina funktioner i C# med alla hello fördelarna med Visual Studio-utveckling.</span><span class="sxs-lookup"><span data-stu-id="335e6-112">Code your functions in C# while having all of hello benefits of Visual Studio development.</span></span> 

<span data-ttu-id="335e6-113">Det här avsnittet visar hur toouse hello Azure Functions verktyg för Visual Studio 2017 toodevelop dina funktioner i C#.</span><span class="sxs-lookup"><span data-stu-id="335e6-113">This topic shows you how toouse hello Azure Functions Tools for Visual Studio 2017 toodevelop your functions in C#.</span></span> <span data-ttu-id="335e6-114">Du också lära dig hur toopublish ditt projekt tooAzure som en .NET-sammansättning.</span><span class="sxs-lookup"><span data-stu-id="335e6-114">You also learn how toopublish your project tooAzure as a .NET assembly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="335e6-115">Krav</span><span class="sxs-lookup"><span data-stu-id="335e6-115">Prerequisites</span></span>

<span data-ttu-id="335e6-116">Azure Functions-verktyg ingår i hello Azure-utveckling arbetsbelastning [Visual Studio 2017 version 15,3](https://www.visualstudio.com/vs/), eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="335e6-116">Azure Functions Tools is included in hello Azure development workload of [Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/), or a later version.</span></span> <span data-ttu-id="335e6-117">Kontrollera att du inkluderar hello **Azure-utveckling** arbetsbelastning i Visual Studio 2017 version 15,3 installationen:</span><span class="sxs-lookup"><span data-stu-id="335e6-117">Make sure you include hello **Azure development** workload in your Visual Studio 2017 version 15.3 installation:</span></span>

![Installera Visual Studio 2017 med hello Azure-utveckling arbetsbelastning](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

<span data-ttu-id="335e6-119">toocreate och distribuera funktioner måste du också:</span><span class="sxs-lookup"><span data-stu-id="335e6-119">toocreate and deploy functions, you also need:</span></span>

* <span data-ttu-id="335e6-120">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="335e6-120">An active Azure subscription.</span></span> <span data-ttu-id="335e6-121">Om du inte har en Azure-prenumeration [kostnadsfria konton](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="335e6-121">If you don't have an Azure subscription, [free accounts](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) are available.</span></span>

* <span data-ttu-id="335e6-122">Ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="335e6-122">An Azure Storage account.</span></span> <span data-ttu-id="335e6-123">toocreate ett lagringskonto finns [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="335e6-123">toocreate a storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
## <a name="create-an-azure-functions-project"></a><span data-ttu-id="335e6-124">Skapa ett Azure Functions-projekt</span><span class="sxs-lookup"><span data-stu-id="335e6-124">Create an Azure Functions project</span></span> 

[!INCLUDE [Create a project using hello Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-hello-project-for-local-development"></a><span data-ttu-id="335e6-125">Konfigurera hello projekt för lokal utveckling</span><span class="sxs-lookup"><span data-stu-id="335e6-125">Configure hello project for local development</span></span>

<span data-ttu-id="335e6-126">När du skapar ett nytt projekt med hjälp av hello Azure Functions mall kan få du ett tomt C#-projekt som innehåller hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="335e6-126">When you create a new project using hello Azure Functions template, you get an empty C# project that contains hello following files:</span></span>

* <span data-ttu-id="335e6-127">**Host.JSON**: kan du konfigurera hello funktioner värden.</span><span class="sxs-lookup"><span data-stu-id="335e6-127">**host.json**: Lets you configure hello Functions host.</span></span> <span data-ttu-id="335e6-128">Dessa inställningar gäller både när du kör lokalt och i Azure.</span><span class="sxs-lookup"><span data-stu-id="335e6-128">These settings apply both when running locally and in Azure.</span></span> <span data-ttu-id="335e6-129">Mer information finns i [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) referensartikeln.</span><span class="sxs-lookup"><span data-stu-id="335e6-129">For more information, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) reference article.</span></span>
    
* <span data-ttu-id="335e6-130">**Local.Settings.JSON**: underhåller inställningar som används när du kör funktioner lokalt.</span><span class="sxs-lookup"><span data-stu-id="335e6-130">**local.settings.json**: Maintains settings used when running functions locally.</span></span> <span data-ttu-id="335e6-131">De här inställningarna används inte av Azure, de används av hello [Azure Functions grundläggande verktyg](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="335e6-131">These settings are not used by Azure, they are used by hello [Azure Functions Core Tools](functions-run-local.md).</span></span> <span data-ttu-id="335e6-132">Använd den här filen toospecify inställningar, till exempel anslutning strängar tooother Azure services.</span><span class="sxs-lookup"><span data-stu-id="335e6-132">Use this file toospecify settings, such as connection strings tooother Azure services.</span></span> <span data-ttu-id="335e6-133">Lägg till en ny nyckel toohello **värden** matris för varje anslutning som krävs av funktionerna i projektet.</span><span class="sxs-lookup"><span data-stu-id="335e6-133">Add a new key toohello **Values** array for each connection required by functions in your project.</span></span> <span data-ttu-id="335e6-134">Mer information finns i [lokala inställningsfilen](functions-run-local.md#local-settings-file) i hello Azure Functions grundläggande verktyg avsnittet.</span><span class="sxs-lookup"><span data-stu-id="335e6-134">For more information, see [Local settings file](functions-run-local.md#local-settings-file) in hello Azure Functions Core Tools topic.</span></span>

<span data-ttu-id="335e6-135">hello Functions-runtime används internt av Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="335e6-135">hello Functions runtime uses an Azure Storage account internally.</span></span> <span data-ttu-id="335e6-136">För alla utlösa typer än HTTP och webhooks, måste du ange hello **Values.AzureWebJobsStorage** nyckeln tooa giltig Azure Storage-konto anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="335e6-136">For all trigger types other than HTTP and webhooks, you must set hello **Values.AzureWebJobsStorage** key tooa valid Azure Storage account connection string.</span></span>

[!INCLUDE [Note toonot use local storage](../../includes/functions-local-settings-note.md)]

 <span data-ttu-id="335e6-137">anslutningssträngen för tooset hello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="335e6-137">tooset hello storage account connection string:</span></span>

1. <span data-ttu-id="335e6-138">Öppna i Visual Studio **Cloud Explorer**, expandera **Lagringskonto** > **Your Lagringskonto**och välj **egenskaper**och kopiera hello **primära anslutningssträngen** värde.</span><span class="sxs-lookup"><span data-stu-id="335e6-138">In Visual Studio, open **Cloud Explorer**, expand **Storage Account** > **Your Storage Account**, then select **Properties** and copy hello **Primary Connection String** value.</span></span>   

2. <span data-ttu-id="335e6-139">Öppna hello local.settings.json projektfilen i ditt projekt och värdet för hello hello **AzureWebJobsStorage** nyckeln toohello anslutningssträngen som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="335e6-139">In your project, open hello local.settings.json project file and set hello value of hello **AzureWebJobsStorage** key toohello connection string you copied.</span></span>

3. <span data-ttu-id="335e6-140">Upprepa hello föregående steg tooadd unika nycklar toohello **värden** matris för alla anslutningar som krävs av dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="335e6-140">Repeat hello previous step tooadd unique keys toohello **Values** array for any other connections required by your functions.</span></span>  

## <a name="create-a-function"></a><span data-ttu-id="335e6-141">Skapa en funktion</span><span class="sxs-lookup"><span data-stu-id="335e6-141">Create a function</span></span>

<span data-ttu-id="335e6-142">Hello-bindningar som används av hello funktionen definieras i förväg kompilerade funktioner genom att använda i hello koden.</span><span class="sxs-lookup"><span data-stu-id="335e6-142">In pre-compiled functions, hello bindings used by hello function are defined by applying attributes in hello code.</span></span> <span data-ttu-id="335e6-143">När du använder hello Azure Functions verktyg toocreate dina funktioner från hello tillhandahålls mallar tillämpas dessa attribut för dig.</span><span class="sxs-lookup"><span data-stu-id="335e6-143">When you use hello Azure Functions Tools toocreate your functions from hello provided templates, these attributes are applied for you.</span></span> 

1. <span data-ttu-id="335e6-144">I **Solution Explorer** högerklickar du på projektnoden och väljer **Lägg till** > **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="335e6-144">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="335e6-145">Välj **Azure-funktion**, ange ett **namn** hello klass och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="335e6-145">Select **Azure Function**, type a **Name** for hello class, and click **Add**.</span></span>

2. <span data-ttu-id="335e6-146">Välj utlösaren, ange hello bindande egenskaperna och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="335e6-146">Choose your trigger, set hello binding properties, and click **Create**.</span></span> <span data-ttu-id="335e6-147">hello följande exempel visar hello inställningar när du skapar en Queue storage aktiveras funktionen.</span><span class="sxs-lookup"><span data-stu-id="335e6-147">hello following example shows hello settings when creating a Queue storage triggered function.</span></span> 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    <span data-ttu-id="335e6-148">En anslutning strängnyckel som heter **QueueStorage** har angetts som har definierats i hello local.settings.json-filen.</span><span class="sxs-lookup"><span data-stu-id="335e6-148">A connection string key named **QueueStorage** is supplied, which is defined in hello local.settings.json file.</span></span> 
 
3. <span data-ttu-id="335e6-149">Granska hello nyligen tillagda klass.</span><span class="sxs-lookup"><span data-stu-id="335e6-149">Examine hello newly added class.</span></span> <span data-ttu-id="335e6-150">Du ser en statisk **kör** metod som har attributet hello **FunctionName** attribut.</span><span class="sxs-lookup"><span data-stu-id="335e6-150">You see a static **Run** method, that is attributed with hello **FunctionName** attribute.</span></span> <span data-ttu-id="335e6-151">Det här attributet anger att hello-metoden är hello startpunkt för hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="335e6-151">This attribute indicates that hello method is hello entry point for hello function.</span></span> 

    <span data-ttu-id="335e6-152">Till exempel representerar hello följande C#-klass grundläggande kön utlöses lagringsfunktionen:</span><span class="sxs-lookup"><span data-stu-id="335e6-152">For example, hello following C# class represents a basic Queue storage triggered function:</span></span>

    ````csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    
    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]        
            public static void Run([QueueTrigger("myqueue-items", Connection = "QueueStorage")]string myQueueItem, TraceWriter log)
            {
                log.Info($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    } 
    ````
 
    <span data-ttu-id="335e6-153">En bindning-specifika attributet är tillämpade tooeach bindning parametern toohello metoden.</span><span class="sxs-lookup"><span data-stu-id="335e6-153">A binding-specific attribute is applied tooeach binding parameter supplied toohello entry point method.</span></span> <span data-ttu-id="335e6-154">hello attributet tar hello bindningsinformation som parametrar.</span><span class="sxs-lookup"><span data-stu-id="335e6-154">hello attribute takes hello binding information as parameters.</span></span> <span data-ttu-id="335e6-155">I föregående exempel hello hello första parametern har en **QueueTrigger** -attribut som används, som anger kön aktiveras funktionen.</span><span class="sxs-lookup"><span data-stu-id="335e6-155">In hello previous example, hello first parameter has a **QueueTrigger** attribute applied, indicating queue triggered function.</span></span> <span data-ttu-id="335e6-156">hello könamnet och namn för anslutningssträngen inställningen överförs som parametrar.</span><span class="sxs-lookup"><span data-stu-id="335e6-156">hello queue name and connection string setting name are passed as parameters.</span></span>  

## <a name="testing-functions"></a><span data-ttu-id="335e6-157">Testa funktioner</span><span class="sxs-lookup"><span data-stu-id="335e6-157">Testing functions</span></span>

<span data-ttu-id="335e6-158">Med Azure Functions Core Tools kan du köra Azure Functions-projekt på din lokala utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="335e6-158">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="335e6-159">Du kan ange tooinstall verktygen hello första gången du startar en funktion från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="335e6-159">You are prompted tooinstall these tools hello first time you start a function from Visual Studio.</span></span>  

<span data-ttu-id="335e6-160">tootest din funktion, tryck på F5.</span><span class="sxs-lookup"><span data-stu-id="335e6-160">tootest your function, press F5.</span></span> <span data-ttu-id="335e6-161">Om du uppmanas acceptera hello begäran från Visual Studio toodownload och installera verktyg för Azure Functions kärnor (CLI).</span><span class="sxs-lookup"><span data-stu-id="335e6-161">If prompted, accept hello request from Visual Studio toodownload and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="335e6-162">Du kanske också måste tooenable ett undantag i brandväggen så att hello verktyg kan hantera HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="335e6-162">You may also need tooenable a firewall exception so that hello tools can handle HTTP requests.</span></span>

<span data-ttu-id="335e6-163">Du kan testa din kod som du vill testa distribuerade funktionen med hello-projekt som körs.</span><span class="sxs-lookup"><span data-stu-id="335e6-163">With hello project running, you can test your code as you would test deployed function.</span></span> <span data-ttu-id="335e6-164">Mer information finns i [strategier för att testa din kod i Azure Functions](functions-test-a-function.md).</span><span class="sxs-lookup"><span data-stu-id="335e6-164">For more information, see [Strategies for testing your code in Azure Functions](functions-test-a-function.md).</span></span> <span data-ttu-id="335e6-165">När du kör i felsökningsläge nådde brytpunkter i Visual Studio som förväntat.</span><span class="sxs-lookup"><span data-stu-id="335e6-165">When running in debug mode, breakpoints are hit in Visual Studio as expected.</span></span> 

<span data-ttu-id="335e6-166">Ett exempel på hur tootest en kö aktiveras funktionen, se hello [kön aktiveras funktionen Snabbstartsguide](functions-create-storage-queue-triggered-function.md#test-the-function).</span><span class="sxs-lookup"><span data-stu-id="335e6-166">For an example of how tootest a queue triggered function, see hello [queue triggered function quickstart tutorial](functions-create-storage-queue-triggered-function.md#test-the-function).</span></span>  

<span data-ttu-id="335e6-167">toolearn mer information om hur du använder hello Azure Functions grundläggande verktygen finns [koden och testa Azure functions lokalt](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="335e6-167">toolearn more about using hello Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>

## <a name="publish-tooazure"></a><span data-ttu-id="335e6-168">Publicera tooAzure</span><span class="sxs-lookup"><span data-stu-id="335e6-168">Publish tooAzure</span></span>

[!INCLUDE [Publish hello project tooAzure](../../includes/functions-vstools-publish.md)]

>[!NOTE]  
><span data-ttu-id="335e6-169">Alla inställningar som du lade till i hello local.settings.json måste också läggas toohello funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="335e6-169">Any settings you added in hello local.settings.json must be also added toohello function app in Azure.</span></span> <span data-ttu-id="335e6-170">De här inställningarna läggs inte automatiskt.</span><span class="sxs-lookup"><span data-stu-id="335e6-170">These settings are not added automatically.</span></span> <span data-ttu-id="335e6-171">Du kan lägga till nödvändiga inställningar tooyour funktionsapp i något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="335e6-171">You can add required settings tooyour function app in one of these ways:</span></span>
>
>* <span data-ttu-id="335e6-172">[Med hjälp av hello Azure-portalen](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="335e6-172">[Using hello Azure portal](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>
>* <span data-ttu-id="335e6-173">[Med hjälp av hello `--publish-local-settings` publicera alternativet i hello Azure Functions grundläggande verktyg](functions-run-local.md#publish).</span><span class="sxs-lookup"><span data-stu-id="335e6-173">[Using hello `--publish-local-settings` publish option in hello Azure Functions Core Tools](functions-run-local.md#publish).</span></span>
>* <span data-ttu-id="335e6-174">[Med hjälp av hello Azure CLI](/cli/azure/functionapp/config/appsettings#set).</span><span class="sxs-lookup"><span data-stu-id="335e6-174">[Using hello Azure CLI](/cli/azure/functionapp/config/appsettings#set).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="335e6-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="335e6-175">Next steps</span></span>

<span data-ttu-id="335e6-176">Mer information om Azure Functions verktyg finns hello vanliga frågor avsnitt i hello [Visual Studio 2017 verktyg för Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="335e6-176">For more information about Azure Functions Tools, see hello Common Questions section of hello [Visual Studio 2017 Tools for Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blog post.</span></span>

<span data-ttu-id="335e6-177">toolearn mer om hello Azure Functions grundläggande verktygen finns [koden och testa Azure functions lokalt](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="335e6-177">toolearn more about hello Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>  
<span data-ttu-id="335e6-178">toolearn mer information om hur du utvecklar fungerar som .NET-klassbibliotek finns [med hjälp av .NET-klassbibliotek med Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="335e6-178">toolearn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> <span data-ttu-id="335e6-179">Det här avsnittet innehåller också exempel på hur toouse attribut toodeclare hello olika typer av bindningar som stöds av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="335e6-179">This topic also provides examples of how toouse attributes toodeclare hello various types of bindings supported by Azure Functions.</span></span>    
