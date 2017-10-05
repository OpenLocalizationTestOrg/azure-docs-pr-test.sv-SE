---
title: "Utveckla Azure-funktioner med hjälp av Visual Studio | Microsoft Docs"
description: "Lär dig mer om att utveckla och testa Azure Functions med hjälp av Azure Functions verktyg för Visual Studio-2017."
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
ms.openlocfilehash: 1e0568bc58e8879cabe409cf8e9b5866f922e7c9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-tools-for-visual-studio"></a><span data-ttu-id="9488b-103">Azure Functions Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9488b-103">Azure Functions Tools for Visual Studio</span></span>  

<span data-ttu-id="9488b-104">Azure Functions-verktyg för Visual Studio-2017 är ett tillägg för Visual Studio som låter dig utveckla, testa och distribuera C#-funktioner till Azure.</span><span class="sxs-lookup"><span data-stu-id="9488b-104">Azure Functions Tools for Visual Studio 2017 is an extension for Visual Studio that lets you develop, test, and deploy C# functions to Azure.</span></span> <span data-ttu-id="9488b-105">Om det här är din första upplevelse med Azure Functions kan du läsa mer i [en introduktion till Azure Functions](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9488b-105">If this is your first experience with Azure Functions, you can learn more at [An introduction to Azure Functions](functions-overview.md).</span></span>

<span data-ttu-id="9488b-106">Azure Functions verktyg ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9488b-106">The Azure Functions Tools provides the following benefits:</span></span> 

* <span data-ttu-id="9488b-107">Redigera, skapa och köra funktioner på lokala utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="9488b-107">Edit, build, and run functions on your local development computer.</span></span> 
* <span data-ttu-id="9488b-108">Publicera projektet Azure Functions direkt till Azure.</span><span class="sxs-lookup"><span data-stu-id="9488b-108">Publish your Azure Functions project directly to Azure.</span></span> 
* <span data-ttu-id="9488b-109">Använda WebJobs-attribut för att deklarera funktionsbindningar direkt i C#-kod i stället för att upprätthålla en separat function.json för bindning definitioner.</span><span class="sxs-lookup"><span data-stu-id="9488b-109">Use WebJobs attributes to declare function bindings directly in the C# code instead of maintaining a separate function.json for binding definitions.</span></span>
* <span data-ttu-id="9488b-110">Utveckla och distribuera före kompilerade C#-funktioner.</span><span class="sxs-lookup"><span data-stu-id="9488b-110">Develop and deploy pre-compiled C# functions.</span></span> <span data-ttu-id="9488b-111">Före följt funktioner ge en bättre kall start prestanda än C# skript-baserad funktion.</span><span class="sxs-lookup"><span data-stu-id="9488b-111">Pre-complied functions provide a better cold-start performance than C# script-based functions.</span></span> 
* <span data-ttu-id="9488b-112">Felkod dina funktioner i C# med alla fördelar med Visual Studio-utveckling.</span><span class="sxs-lookup"><span data-stu-id="9488b-112">Code your functions in C# while having all of the benefits of Visual Studio development.</span></span> 

<span data-ttu-id="9488b-113">Det här avsnittet visar hur du använder Azure Functions-verktyg för Visual Studio 2017 för att utveckla dina funktioner i C#.</span><span class="sxs-lookup"><span data-stu-id="9488b-113">This topic shows you how to use the Azure Functions Tools for Visual Studio 2017 to develop your functions in C#.</span></span> <span data-ttu-id="9488b-114">Du också lära dig hur du publicerar projektet till Azure som en .NET-sammansättning.</span><span class="sxs-lookup"><span data-stu-id="9488b-114">You also learn how to publish your project to Azure as a .NET assembly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9488b-115">Krav</span><span class="sxs-lookup"><span data-stu-id="9488b-115">Prerequisites</span></span>

<span data-ttu-id="9488b-116">Azure Functions-verktyg ingår i Azure-utveckling arbetsbelastning [Visual Studio 2017 version 15,3](https://www.visualstudio.com/vs/), eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="9488b-116">Azure Functions Tools is included in the Azure development workload of [Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/), or a later version.</span></span> <span data-ttu-id="9488b-117">Kontrollera att du inkluderar den **Azure-utveckling** arbetsbelastning i Visual Studio 2017 version 15,3 installationen:</span><span class="sxs-lookup"><span data-stu-id="9488b-117">Make sure you include the **Azure development** workload in your Visual Studio 2017 version 15.3 installation:</span></span>

![Installera Visual Studio 2017 med arbetsbelastningen Azure Development](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

<span data-ttu-id="9488b-119">Om du vill skapa och distribuera funktioner, måste du också:</span><span class="sxs-lookup"><span data-stu-id="9488b-119">To create and deploy functions, you also need:</span></span>

* <span data-ttu-id="9488b-120">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9488b-120">An active Azure subscription.</span></span> <span data-ttu-id="9488b-121">Om du inte har en Azure-prenumeration [kostnadsfria konton](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="9488b-121">If you don't have an Azure subscription, [free accounts](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) are available.</span></span>

* <span data-ttu-id="9488b-122">Ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9488b-122">An Azure Storage account.</span></span> <span data-ttu-id="9488b-123">Om du vill skapa ett lagringskonto finns [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="9488b-123">To create a storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
## <a name="create-an-azure-functions-project"></a><span data-ttu-id="9488b-124">Skapa ett Azure Functions-projekt</span><span class="sxs-lookup"><span data-stu-id="9488b-124">Create an Azure Functions project</span></span> 

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-the-project-for-local-development"></a><span data-ttu-id="9488b-125">Konfigurera projektet för lokal utveckling</span><span class="sxs-lookup"><span data-stu-id="9488b-125">Configure the project for local development</span></span>

<span data-ttu-id="9488b-126">När du skapar ett nytt projekt med hjälp av Azure Functions-mall kan få du ett tomt C#-projekt som innehåller följande filer:</span><span class="sxs-lookup"><span data-stu-id="9488b-126">When you create a new project using the Azure Functions template, you get an empty C# project that contains the following files:</span></span>

* <span data-ttu-id="9488b-127">**Host.JSON**: kan du konfigurera funktioner värden.</span><span class="sxs-lookup"><span data-stu-id="9488b-127">**host.json**: Lets you configure the Functions host.</span></span> <span data-ttu-id="9488b-128">Dessa inställningar gäller både när du kör lokalt och i Azure.</span><span class="sxs-lookup"><span data-stu-id="9488b-128">These settings apply both when running locally and in Azure.</span></span> <span data-ttu-id="9488b-129">Mer information finns i [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) referensartikeln.</span><span class="sxs-lookup"><span data-stu-id="9488b-129">For more information, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) reference article.</span></span>
    
* <span data-ttu-id="9488b-130">**Local.Settings.JSON**: underhåller inställningar som används när du kör funktioner lokalt.</span><span class="sxs-lookup"><span data-stu-id="9488b-130">**local.settings.json**: Maintains settings used when running functions locally.</span></span> <span data-ttu-id="9488b-131">De här inställningarna används inte av Azure, de används av den [Azure Functions grundläggande verktyg](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="9488b-131">These settings are not used by Azure, they are used by the [Azure Functions Core Tools](functions-run-local.md).</span></span> <span data-ttu-id="9488b-132">Använd den här filen för att ange inställningar, till exempel anslutningssträngar till andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9488b-132">Use this file to specify settings, such as connection strings to other Azure services.</span></span> <span data-ttu-id="9488b-133">Lägg till en ny nyckel till den **värden** matris för varje anslutning som krävs av funktionerna i projektet.</span><span class="sxs-lookup"><span data-stu-id="9488b-133">Add a new key to the **Values** array for each connection required by functions in your project.</span></span> <span data-ttu-id="9488b-134">Mer information finns i [lokala inställningsfilen](functions-run-local.md#local-settings-file) i avsnittet om Azure Functions grundläggande verktyg.</span><span class="sxs-lookup"><span data-stu-id="9488b-134">For more information, see [Local settings file](functions-run-local.md#local-settings-file) in the Azure Functions Core Tools topic.</span></span>

<span data-ttu-id="9488b-135">Functions-runtime används internt i ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9488b-135">The Functions runtime uses an Azure Storage account internally.</span></span> <span data-ttu-id="9488b-136">För alla utlösa typer än HTTP och webhooks, måste du ange den **Values.AzureWebJobsStorage** nyckel till en giltig anslutningssträng för Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9488b-136">For all trigger types other than HTTP and webhooks, you must set the **Values.AzureWebJobsStorage** key to a valid Azure Storage account connection string.</span></span>

[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

 <span data-ttu-id="9488b-137">Ange anslutningssträngen för lagring konto:</span><span class="sxs-lookup"><span data-stu-id="9488b-137">To set the storage account connection string:</span></span>

1. <span data-ttu-id="9488b-138">Öppna i Visual Studio **Cloud Explorer**, expandera **Lagringskonto** > **Your Lagringskonto**och välj **egenskaper**och kopiera den **primära anslutningssträngen** värde.</span><span class="sxs-lookup"><span data-stu-id="9488b-138">In Visual Studio, open **Cloud Explorer**, expand **Storage Account** > **Your Storage Account**, then select **Properties** and copy the **Primary Connection String** value.</span></span>   

2. <span data-ttu-id="9488b-139">Öppna projektfilen local.settings.json i ditt projekt och värdet för den **AzureWebJobsStorage** nyckel i anslutningssträngen som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="9488b-139">In your project, open the local.settings.json project file and set the value of the **AzureWebJobsStorage** key to the connection string you copied.</span></span>

3. <span data-ttu-id="9488b-140">Upprepa det föregående steget för att lägga till unika nycklar till den **värden** matris för alla anslutningar som krävs av dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="9488b-140">Repeat the previous step to add unique keys to the **Values** array for any other connections required by your functions.</span></span>  

## <a name="create-a-function"></a><span data-ttu-id="9488b-141">Skapa en funktion</span><span class="sxs-lookup"><span data-stu-id="9488b-141">Create a function</span></span>

<span data-ttu-id="9488b-142">I förväg kompilerade funktioner definieras de bindningar som används av funktionen genom att använda i koden.</span><span class="sxs-lookup"><span data-stu-id="9488b-142">In pre-compiled functions, the bindings used by the function are defined by applying attributes in the code.</span></span> <span data-ttu-id="9488b-143">När du använder Azure Functions-verktyg för att skapa dina funktioner från de angivna mallarna, tillämpas dessa attribut för dig.</span><span class="sxs-lookup"><span data-stu-id="9488b-143">When you use the Azure Functions Tools to create your functions from the provided templates, these attributes are applied for you.</span></span> 

1. <span data-ttu-id="9488b-144">I **Solution Explorer** högerklickar du på projektnoden och väljer **Lägg till** > **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="9488b-144">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="9488b-145">Välj **Azure-funktion**, ange ett **namn** för klass och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="9488b-145">Select **Azure Function**, type a **Name** for the class, and click **Add**.</span></span>

2. <span data-ttu-id="9488b-146">Välj utlösaren, ange de bindande egenskaperna och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9488b-146">Choose your trigger, set the binding properties, and click **Create**.</span></span> <span data-ttu-id="9488b-147">I följande exempel visar inställningar när du skapar en Queue storage aktiveras funktionen.</span><span class="sxs-lookup"><span data-stu-id="9488b-147">The following example shows the settings when creating a Queue storage triggered function.</span></span> 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    <span data-ttu-id="9488b-148">En anslutning strängnyckel som heter **QueueStorage** har angetts som definieras i filen local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="9488b-148">A connection string key named **QueueStorage** is supplied, which is defined in the local.settings.json file.</span></span> 
 
3. <span data-ttu-id="9488b-149">Granska nyligen tillagda klassen.</span><span class="sxs-lookup"><span data-stu-id="9488b-149">Examine the newly added class.</span></span> <span data-ttu-id="9488b-150">Du ser en statisk **kör** metod som har den **FunctionName** attribut.</span><span class="sxs-lookup"><span data-stu-id="9488b-150">You see a static **Run** method, that is attributed with the **FunctionName** attribute.</span></span> <span data-ttu-id="9488b-151">Det här attributet anger att metoden är startpunkten för funktionen.</span><span class="sxs-lookup"><span data-stu-id="9488b-151">This attribute indicates that the method is the entry point for the function.</span></span> 

    <span data-ttu-id="9488b-152">Till exempel följande C# klassen representerar en grundläggande kön utlöses lagringsfunktionen:</span><span class="sxs-lookup"><span data-stu-id="9488b-152">For example, the following C# class represents a basic Queue storage triggered function:</span></span>

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
 
    <span data-ttu-id="9488b-153">En bindning-specifika attributet används för varje bindning-parameter som skickades till metoden.</span><span class="sxs-lookup"><span data-stu-id="9488b-153">A binding-specific attribute is applied to each binding parameter supplied to the entry point method.</span></span> <span data-ttu-id="9488b-154">Attributet tar bindningsinformationen som parametrar.</span><span class="sxs-lookup"><span data-stu-id="9488b-154">The attribute takes the binding information as parameters.</span></span> <span data-ttu-id="9488b-155">I föregående exempel är den första parametern har en **QueueTrigger** -attribut som används, som anger kön aktiveras funktionen.</span><span class="sxs-lookup"><span data-stu-id="9488b-155">In the previous example, The first parameter has a **QueueTrigger** attribute applied, indicating queue triggered function.</span></span> <span data-ttu-id="9488b-156">Könamnet och namn för anslutningssträngen inställningen överförs som parametrar.</span><span class="sxs-lookup"><span data-stu-id="9488b-156">The queue name and connection string setting name are passed as parameters.</span></span>  

## <a name="testing-functions"></a><span data-ttu-id="9488b-157">Testa funktioner</span><span class="sxs-lookup"><span data-stu-id="9488b-157">Testing functions</span></span>

<span data-ttu-id="9488b-158">Med Azure Functions Core Tools kan du köra Azure Functions-projekt på din lokala utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="9488b-158">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="9488b-159">Du uppmanas att installera de här verktygen första gången du startar en funktion från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9488b-159">You are prompted to install these tools the first time you start a function from Visual Studio.</span></span>  

<span data-ttu-id="9488b-160">Tryck på F5 för att testa funktionen.</span><span class="sxs-lookup"><span data-stu-id="9488b-160">To test your function, press F5.</span></span> <span data-ttu-id="9488b-161">Acceptera begäran från Visual Studio för att ladda ned och installera Azure Functions Core (CLI)-verktyg.</span><span class="sxs-lookup"><span data-stu-id="9488b-161">If prompted, accept the request from Visual Studio to download and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="9488b-162">Du kan även behöva skapa ett brandväggsundantag så att verktygen kan hantera HTTP-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="9488b-162">You may also need to enable a firewall exception so that the tools can handle HTTP requests.</span></span>

<span data-ttu-id="9488b-163">Du kan testa din kod som du vill testa distribuerade funktionen med projektet körs.</span><span class="sxs-lookup"><span data-stu-id="9488b-163">With the project running, you can test your code as you would test deployed function.</span></span> <span data-ttu-id="9488b-164">Mer information finns i [strategier för att testa din kod i Azure Functions](functions-test-a-function.md).</span><span class="sxs-lookup"><span data-stu-id="9488b-164">For more information, see [Strategies for testing your code in Azure Functions](functions-test-a-function.md).</span></span> <span data-ttu-id="9488b-165">När du kör i felsökningsläge nådde brytpunkter i Visual Studio som förväntat.</span><span class="sxs-lookup"><span data-stu-id="9488b-165">When running in debug mode, breakpoints are hit in Visual Studio as expected.</span></span> 

<span data-ttu-id="9488b-166">Ett exempel på hur du testar en kö som utlöste funktion finns i [kön aktiveras funktionen Snabbstartsguide](functions-create-storage-queue-triggered-function.md#test-the-function).</span><span class="sxs-lookup"><span data-stu-id="9488b-166">For an example of how to test a queue triggered function, see the [queue triggered function quickstart tutorial](functions-create-storage-queue-triggered-function.md#test-the-function).</span></span>  

<span data-ttu-id="9488b-167">Mer information om hur du använder Azure Functions grundläggande verktygen finns [koden och testa Azure functions lokalt](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="9488b-167">To learn more about using the Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>

## <a name="publish-to-azure"></a><span data-ttu-id="9488b-168">Publicera till Azure</span><span class="sxs-lookup"><span data-stu-id="9488b-168">Publish to Azure</span></span>

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

>[!NOTE]  
><span data-ttu-id="9488b-169">Alla inställningar som du lade till i local.settings.json måste också läggas till funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="9488b-169">Any settings you added in the local.settings.json must be also added to the function app in Azure.</span></span> <span data-ttu-id="9488b-170">De här inställningarna läggs inte automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9488b-170">These settings are not added automatically.</span></span> <span data-ttu-id="9488b-171">Du kan lägga till nödvändiga inställningar i appen funktion i något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9488b-171">You can add required settings to your function app in one of these ways:</span></span>
>
>* <span data-ttu-id="9488b-172">[Med hjälp av Azure portal](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="9488b-172">[Using the Azure portal](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>
>* <span data-ttu-id="9488b-173">[Med hjälp av den `--publish-local-settings` alternativet Publicera i Azure Functions Core verktyg](functions-run-local.md#publish).</span><span class="sxs-lookup"><span data-stu-id="9488b-173">[Using the `--publish-local-settings` publish option in the Azure Functions Core Tools](functions-run-local.md#publish).</span></span>
>* <span data-ttu-id="9488b-174">[Med hjälp av Azure CLI](/cli/azure/functionapp/config/appsettings#set).</span><span class="sxs-lookup"><span data-stu-id="9488b-174">[Using the Azure CLI](/cli/azure/functionapp/config/appsettings#set).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9488b-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9488b-175">Next steps</span></span>

<span data-ttu-id="9488b-176">Mer information om Azure Functions verktyg finns i avsnittet vanliga frågor för den [Visual Studio 2017 verktyg för Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="9488b-176">For more information about Azure Functions Tools, see the Common Questions section of the [Visual Studio 2017 Tools for Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blog post.</span></span>

<span data-ttu-id="9488b-177">Läs mer om Azure Functions grundläggande verktyg i [koden och testa Azure functions lokalt](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="9488b-177">To learn more about the Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>  
<span data-ttu-id="9488b-178">Mer information om hur du utvecklar fungerar som .NET-klassbibliotek finns i [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md) (Använda .NET-klassbibliotek med Azure Functions).</span><span class="sxs-lookup"><span data-stu-id="9488b-178">To learn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> <span data-ttu-id="9488b-179">Det här avsnittet innehåller också exempel på hur du använder attribut för att deklarera de olika typerna av bindningar som stöds av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9488b-179">This topic also provides examples of how to use attributes to declare the various types of bindings supported by Azure Functions.</span></span>    
