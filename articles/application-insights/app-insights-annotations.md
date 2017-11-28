---
title: "aaaRelease anteckningar för Application Insights | Microsoft Docs"
description: "Lägg till distribution eller skapa markörer tooyour metrics explorer-diagram i Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: e802d22701cb69e96fd1a6b469ea67454195f77a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="annotations-on-metric-charts-in-application-insights"></a><span data-ttu-id="508d0-103">Anteckningar i mått diagram i Application Insights</span><span class="sxs-lookup"><span data-stu-id="508d0-103">Annotations on metric charts in Application Insights</span></span>
<span data-ttu-id="508d0-104">Anteckningar på [Metrics Explorer](app-insights-metrics-explorer.md) diagram visas där du har distribuerat en ny version eller betydande händelse.</span><span class="sxs-lookup"><span data-stu-id="508d0-104">Annotations on [Metrics Explorer](app-insights-metrics-explorer.md) charts show where you deployed a new build, or other significant event.</span></span> <span data-ttu-id="508d0-105">De gör det enkelt toosee om ändringarna hade någon inverkan på programmets prestanda.</span><span class="sxs-lookup"><span data-stu-id="508d0-105">They make it easy toosee whether your changes had any effect on your application's performance.</span></span> <span data-ttu-id="508d0-106">De kan skapas automatiskt av hello [Visual Studio Team Services bygg system](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs).</span><span class="sxs-lookup"><span data-stu-id="508d0-106">They can be automatically created by hello [Visual Studio Team Services build system](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs).</span></span> <span data-ttu-id="508d0-107">Du kan också skapa anteckningar tooflag alla händelser som du vill genom att [skapa dem från PowerShell](#create-annotations-from-powershell).</span><span class="sxs-lookup"><span data-stu-id="508d0-107">You can also create annotations tooflag any event you like by [creating them from PowerShell](#create-annotations-from-powershell).</span></span>

![Exempel på anteckningar med synliga korrelation med serversvarstid](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a><span data-ttu-id="508d0-109">Släpp anteckningar med VSTS build</span><span class="sxs-lookup"><span data-stu-id="508d0-109">Release annotations with VSTS build</span></span>

<span data-ttu-id="508d0-110">Versionen anteckningar är en funktion i hello molnbaserade version och utgåva tjänst av Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="508d0-110">Release annotations are a feature of hello cloud-based build and release service of Visual Studio Team Services.</span></span> 

### <a name="install-hello-annotations-extension-one-time"></a><span data-ttu-id="508d0-111">Installera hello anteckningar tillägg (en gång)</span><span class="sxs-lookup"><span data-stu-id="508d0-111">Install hello Annotations extension (one time)</span></span>
<span data-ttu-id="508d0-112">toobe kan toocreate versionen anteckningar, behöver du tooinstall en av hello hello många Team Service tillägg som är tillgängliga i Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="508d0-112">toobe able toocreate release annotations, you'll need tooinstall one of hello many Team Service extensions available in hello Visual Studio Marketplace.</span></span>

1. <span data-ttu-id="508d0-113">Logga in tooyour [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projekt.</span><span class="sxs-lookup"><span data-stu-id="508d0-113">Sign in tooyour [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) project.</span></span>
2. <span data-ttu-id="508d0-114">I Visual Studio Marketplace [ta hello versionen anteckningar tillägget](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), och Lägg till den tooyour Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="508d0-114">In Visual Studio Marketplace, [get hello Release Annotations extension](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), and add it tooyour Team Services account.</span></span>

![AT övre högra av Team Services webbsida, öppna Marketplace.](./media/app-insights-annotations/10.png)

<span data-ttu-id="508d0-117">Du behöver bara toodo detta en gång för Visual Studio Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="508d0-117">You only need toodo this once for your Visual Studio Team Services account.</span></span> <span data-ttu-id="508d0-118">Versionen anteckningar kan nu konfigureras för alla projekt i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="508d0-118">Release annotations can now be configured for any project in your account.</span></span> 

### <a name="configure-release-annotations"></a><span data-ttu-id="508d0-119">Konfigurera versionen anteckningar</span><span class="sxs-lookup"><span data-stu-id="508d0-119">Configure release annotations</span></span>

<span data-ttu-id="508d0-120">Du behöver tooget separat API-nyckel för varje VSTS versionsmall.</span><span class="sxs-lookup"><span data-stu-id="508d0-120">You need tooget a separate API key for each VSTS release template.</span></span>

1. <span data-ttu-id="508d0-121">Logga in toohello [Microsoft Azure Portal](https://portal.azure.com) och öppna hello Application Insights-resurs som övervakar dina program.</span><span class="sxs-lookup"><span data-stu-id="508d0-121">Sign in toohello [Microsoft Azure Portal](https://portal.azure.com) and open hello Application Insights resource that monitors your application.</span></span> <span data-ttu-id="508d0-122">(Eller [skapa en nu](app-insights-overview.md), om du inte gjort det ännu.)</span><span class="sxs-lookup"><span data-stu-id="508d0-122">(Or [create one now](app-insights-overview.md), if you haven't done so yet.)</span></span>
2. <span data-ttu-id="508d0-123">Öppna **API-åtkomst**, **Application Insights Id**.</span><span class="sxs-lookup"><span data-stu-id="508d0-123">Open **API Access**,  **Application Insights Id**.</span></span>
   
    ![Öppna Application Insights-resurs i portal.azure.com, och välja inställningar.](./media/app-insights-annotations/20.png)

4. <span data-ttu-id="508d0-127">Öppna (eller skapa) på hello versionsmall som hanterar din distribution från Visual Studio Team Services i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="508d0-127">In a separate browser window, open (or create) hello release template that manages your deployments from Visual Studio Team Services.</span></span> 
   
    <span data-ttu-id="508d0-128">Lägga till en aktivitet och markera hello Application Insights versionen anteckningen aktiviteten hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="508d0-128">Add a task, and select hello Application Insights Release Annotation task from hello menu.</span></span>
   
    <span data-ttu-id="508d0-129">Klistra in hello **program-Id** som du kopierade från hello bladet för API-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="508d0-129">Paste hello **Application Id** that you copied from hello API Access blade.</span></span>
   
    ![Öppna versionen i Visual Studio Team Services, Välj en definition av versionen och väljer Redigera.](./media/app-insights-annotations/30.png)
4. <span data-ttu-id="508d0-133">Ange hello **APIKey** tooa variabeln `$(ApiKey)`.</span><span class="sxs-lookup"><span data-stu-id="508d0-133">Set hello **APIKey** field tooa variable `$(ApiKey)`.</span></span>

5. <span data-ttu-id="508d0-134">Skapa en ny API-nyckel i hello Azure fönstret, och ta en kopia av den.</span><span class="sxs-lookup"><span data-stu-id="508d0-134">Back in hello Azure window, create a new API Key and take a copy of it.</span></span>
   
    ![Klicka på Skapa API-nyckel i hello bladet API-åtkomst i hello Azure fönster.](./media/app-insights-annotations/40.png)

6. <span data-ttu-id="508d0-138">Öppna fliken för hello konfiguration av hello versionsmall.</span><span class="sxs-lookup"><span data-stu-id="508d0-138">Open hello Configuration tab of hello release template.</span></span>
   
    <span data-ttu-id="508d0-139">Skapa en variabel definition för `ApiKey`.</span><span class="sxs-lookup"><span data-stu-id="508d0-139">Create a variable definition for `ApiKey`.</span></span>
   
    <span data-ttu-id="508d0-140">Klistra in din nyckel API-toohello ApiKey variabeldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="508d0-140">Paste your API key toohello ApiKey variable definition.</span></span>
   
    ![Välj fliken för konfiguration av hello hello Team Services fönstret och klicka på Lägg till variabel.](./media/app-insights-annotations/50.png)
7. <span data-ttu-id="508d0-143">Slutligen **spara** hello versionen definition.</span><span class="sxs-lookup"><span data-stu-id="508d0-143">Finally, **Save** hello release definition.</span></span>


## <a name="view-annotations"></a><span data-ttu-id="508d0-144">Visa anteckningar</span><span class="sxs-lookup"><span data-stu-id="508d0-144">View annotations</span></span>
<span data-ttu-id="508d0-145">Nu när du använder hello versionen mallen toodeploy en ny version skickas anteckningen tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="508d0-145">Now, whenever you use hello release template toodeploy a new release, an annotation will be sent tooApplication Insights.</span></span> <span data-ttu-id="508d0-146">hello anteckningar visas i diagram i Metrics Explorer.</span><span class="sxs-lookup"><span data-stu-id="508d0-146">hello annotations will appear on charts in Metrics Explorer.</span></span>

<span data-ttu-id="508d0-147">Klicka på någon anteckning markör tooopen information om hello versionen, inklusive begärande, källa kontrollen gren, versionen definition, miljö med mera.</span><span class="sxs-lookup"><span data-stu-id="508d0-147">Click on any annotation marker tooopen details about hello release, including requestor, source control branch, release definition, environment, and more.</span></span>

![Klicka på valfri utgåva anteckningen markör.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a><span data-ttu-id="508d0-149">Skapa anpassade anteckningar från PowerShell</span><span class="sxs-lookup"><span data-stu-id="508d0-149">Create custom annotations from PowerShell</span></span>
<span data-ttu-id="508d0-150">Du kan också skapa anteckningar från någon annan process som du vill (utan att använda VS Team System).</span><span class="sxs-lookup"><span data-stu-id="508d0-150">You can also create annotations from any process you like (without using VS Team System).</span></span> 


1. <span data-ttu-id="508d0-151">Skapa en lokal kopia av hello [Powershell-skript från GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).</span><span class="sxs-lookup"><span data-stu-id="508d0-151">Make a local copy of hello [Powershell script from GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).</span></span>

2. <span data-ttu-id="508d0-152">Hämta hello program-ID och skapa en API-nyckel från hello bladet för API-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="508d0-152">Get hello Application ID and create an API key from hello API Access blade.</span></span>

3. <span data-ttu-id="508d0-153">Anropa hello skript så här:</span><span class="sxs-lookup"><span data-stu-id="508d0-153">Call hello script like this:</span></span>

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

<span data-ttu-id="508d0-154">Det är enkelt toomodify hello skript, till exempel toocreate anteckningar för hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="508d0-154">It's easy toomodify hello script, for example toocreate annotations for hello past.</span></span>

## <a name="next-steps"></a><span data-ttu-id="508d0-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="508d0-155">Next steps</span></span>

* [<span data-ttu-id="508d0-156">Skapa arbetsobjekt</span><span class="sxs-lookup"><span data-stu-id="508d0-156">Create work items</span></span>](app-insights-diagnostic-search.md#create-work-item)
* [<span data-ttu-id="508d0-157">Automatisering med PowerShell</span><span class="sxs-lookup"><span data-stu-id="508d0-157">Automation with PowerShell</span></span>](app-insights-powershell.md)
