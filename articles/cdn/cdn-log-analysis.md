---
title: "aaaLog analys för Azure CDN | Microsoft Docs"
description: "Kunden kan aktivera logganalys för Azure CDN."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: 56e5a4fec46fd156cf38252732afb4522741d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a><span data-ttu-id="f0aba-103">Diagnostik-loggarna för Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f0aba-103">Diagnostics Logs for Azure CDN</span></span>

<span data-ttu-id="f0aba-104">När du aktiverar CDN för ditt program, kommer du förmodligen vill toomonitor hello CDN användning, kontrollera din leverans hello hälsa och felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="f0aba-104">After enabling CDN for your application, you will likely want toomonitor hello CDN usage, check hello health of your delivery, and troubleshoot potential issues.</span></span> <span data-ttu-id="f0aba-105">Azure CDN ger dessa funktioner med [CDN Core Analytics](cdn-analyze-usage-patterns.md) och [diagnostikloggar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span><span class="sxs-lookup"><span data-stu-id="f0aba-105">Azure CDN provides these capabilities with [CDN Core Analytics](cdn-analyze-usage-patterns.md) and [Diagnostic Logs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span></span>

## <a name="cdn-core-analytics"></a><span data-ttu-id="f0aba-106">CDN Core Analytics</span><span class="sxs-lookup"><span data-stu-id="f0aba-106">CDN Core Analytics</span></span>
<span data-ttu-id="f0aba-107">Som en aktuell Azure CDN-användare med Verizon standard eller premium-profil har du redan kan tooview core analytics hello kompletterande portalen tillgänglig via hello ”hantera” alternativet från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f0aba-107">As a current Azure CDN user with Verizon standard or premium profile, you are already able tooview core analytics in hello supplemental portal accessible via hello "Manage" option from hello Azure portal.</span></span> 

## <a name="azure-diagnostic-logs"></a><span data-ttu-id="f0aba-108">Azure diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="f0aba-108">Azure Diagnostic Logs</span></span>

<span data-ttu-id="f0aba-109">Azure med denna nya funktion, du kan nu visa core analytics och spara dem i en eller flera mål, inklusive:</span><span class="sxs-lookup"><span data-stu-id="f0aba-109">Azure With this new feature, you can now view core analytics and save them into one or more destinations including:</span></span>

 - <span data-ttu-id="f0aba-110">Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="f0aba-110">Azure Storage account</span></span>
 - <span data-ttu-id="f0aba-111">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f0aba-111">Azure Event Hubs</span></span>
 - [<span data-ttu-id="f0aba-112">OMS Log Analytics-databasen</span><span class="sxs-lookup"><span data-stu-id="f0aba-112">OMS Log Analytics repository</span></span>](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 <span data-ttu-id="f0aba-113">Den här funktionen är tillgänglig för alla CDN-slutpunkter som hör tooVerizon (Standard och Premium) och Akamai (Standard) CDN profiler.</span><span class="sxs-lookup"><span data-stu-id="f0aba-113">This feature is available for all CDN endpoints belonging tooVerizon (Standard & Premium) and Akamai (Standard) CDN Profiles.</span></span>

<span data-ttu-id="f0aba-114">Diagnostik loggar Tillåt tooexport grundläggande användningsstatistik från din CDN-slutpunkten tooa olika källor så att du kan använda dem i ett anpassat sätt.</span><span class="sxs-lookup"><span data-stu-id="f0aba-114">Diagnostics logs allow you tooexport basic usage metrics from your CDN endpoint tooa variety of sources so that you can consume them in a customized way.</span></span> <span data-ttu-id="f0aba-115">Du kan till exempel göra hello följande typer av export av data:</span><span class="sxs-lookup"><span data-stu-id="f0aba-115">For example, you can do hello following types of data export:</span></span>

- <span data-ttu-id="f0aba-116">Exportera tooblob datalagring, exportera tooCSV och skapa diagram i excel.</span><span class="sxs-lookup"><span data-stu-id="f0aba-116">Export data tooblob storage, export tooCSV, and generate graphs in excel.</span></span>
- <span data-ttu-id="f0aba-117">Exportera data tooevent NAV och korrelera med data från andra azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f0aba-117">Export data tooevent hubs and correlate with data from other azure services.</span></span>
- <span data-ttu-id="f0aba-118">Exportera data toolog analytics och visa data i din egen OMS-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="f0aba-118">Export data toolog analytics and view data in your own OMS work space</span></span>

<span data-ttu-id="f0aba-119">hello visar följande bild en typisk CDN Core Analytics överblick över data.</span><span class="sxs-lookup"><span data-stu-id="f0aba-119">hello following figure shows a typical CDN Core Analytics view into data.</span></span>

![Portal - diagnostik loggar](./media/cdn-diagnostics-log/01_OMS-workspace.png)

<span data-ttu-id="f0aba-121">*Bild 1 - CDN Core Analytics visa*</span><span class="sxs-lookup"><span data-stu-id="f0aba-121">*Figure 1 - CDN Core Analytics view*</span></span>

<span data-ttu-id="f0aba-122">hello efter genomgången genomgår hello schemat för hello core analysdata, steg som ingår i hello funktionen aktiveras och leverera dem toovarious mål och förbrukar från dessa mål.</span><span class="sxs-lookup"><span data-stu-id="f0aba-122">hello following walkthrough goes through hello schema of hello core analytics data, steps involved in enabling hello feature and delivering them toovarious destinations, and consuming from these destinations.</span></span>

## <a name="enable-logging-with-azure-portal"></a><span data-ttu-id="f0aba-123">Aktivera loggning med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f0aba-123">Enable logging with Azure portal</span></span>

> [!NOTE]
> <span data-ttu-id="f0aba-124">hello diagnostik loggar aktiveras **av** som standard.</span><span class="sxs-lookup"><span data-stu-id="f0aba-124">hello diagnostics logs are turned **off** by default.</span></span> 

<span data-ttu-id="f0aba-125">Så hello nedan tooenable loggning med CDN Core Analytics:</span><span class="sxs-lookup"><span data-stu-id="f0aba-125">Follow hello steps below tooenable logging with CDN Core Analytics:</span></span>

<span data-ttu-id="f0aba-126">Logga in toohello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f0aba-126">Sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="f0aba-127">Om du inte redan har CDN som aktiverats för ditt arbetsflöde [aktivera Azure CDN](cdn-create-new-endpoint.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="f0aba-127">If you don't already have CDN enabled for your workflow, [Enable Azure CDN](cdn-create-new-endpoint.md) before you continue.</span></span>

1. <span data-ttu-id="f0aba-128">I hello portal, navigerar för**CDN-profilen**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-128">In hello portal, navigate too**CDN profile**.</span></span>
2. <span data-ttu-id="f0aba-129">Välj en CDN-profil och sedan hello CDN-slutpunkt som du vill tooenable **diagnostik loggar**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-129">Select a CDN profile, then select hello CDN endpoint that you want tooenable **Diagnostics Logs**.</span></span>

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. <span data-ttu-id="f0aba-131">Gå för**diagnostik loggar** bladet Under **övervakning** och sedan ändra hello status för**på**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-131">Go too**Diagnostics Logs** blade Under **Monitoring** section, then change hello status too**On**.</span></span>

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a><span data-ttu-id="f0aba-133">Aktivera loggning med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f0aba-133">Enable logging with Azure Storage</span></span>
    
<span data-ttu-id="f0aba-134">toouse Azure Storage toostore hello loggar, Välj **Arkivera tooa lagringskonto**, Välj kvarhållning dagar och på **CoreAnalytics** under **loggen**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-134">toouse Azure Storage toostore hello logs, select **Archive tooa storage account**, select retention days, and click **CoreAnalytics** under **Log**.</span></span>

![Portal - diagnostik loggar](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

<span data-ttu-id="f0aba-136">*Bild 2 - loggning med Azure Storage*</span><span class="sxs-lookup"><span data-stu-id="f0aba-136">*Figure 2 - Logging with Azure Storage*</span></span>

### <a name="logging-with-oms-log-analytics"></a><span data-ttu-id="f0aba-137">Loggning med OMS logganalys</span><span class="sxs-lookup"><span data-stu-id="f0aba-137">Logging with OMS Log Analytics</span></span>

<span data-ttu-id="f0aba-138">toouse OMS logganalys toostore hello loggar så här:</span><span class="sxs-lookup"><span data-stu-id="f0aba-138">toouse OMS Log Analytics toostore hello logs, follow these steps:</span></span>

1. <span data-ttu-id="f0aba-139">Från hello **diagnostik loggar** bladet Under **övervakning**väljer **skicka tooLog Analytics** från</span><span class="sxs-lookup"><span data-stu-id="f0aba-139">From hello **Diagnostics Logs** blade Under **Monitoring**, select **Send tooLog Analytics** from</span></span> 

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. <span data-ttu-id="f0aba-141">Konfigurera hello logganalys loggning genom att klicka på Konfigurera.</span><span class="sxs-lookup"><span data-stu-id="f0aba-141">Configure hello Log Analytics logging by clicking on Configure.</span></span> <span data-ttu-id="f0aba-142">Då kommer du tooa dialogrutan där du kan välja en tidigare arbetsyta eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="f0aba-142">This takes you tooa dialog where you can select a previous workspace or create a new one.</span></span>

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. <span data-ttu-id="f0aba-144">Klicka på **Skapa ny arbetsyta**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-144">Click **Create New Workspace**.</span></span>

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/07_Create-new.png)

4. <span data-ttu-id="f0aba-146">Därefter måste du välja ett nytt Arbetsytenamn, befintliga prenumeration, resursgrupp (ny eller befintlig), plats och prisnivå.</span><span class="sxs-lookup"><span data-stu-id="f0aba-146">Next you must select a new workspace name, existing subscription, resource group (new or existing), location, and pricing tier.</span></span> <span data-ttu-id="f0aba-147">Du har hello möjlighet att fästa den här konfigurationen tooyour instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f0aba-147">You have hello option of pinning this configuration tooyour dashboard.</span></span> <span data-ttu-id="f0aba-148">Klicka på OK toocomplete hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f0aba-148">Click OK toocomplete hello configuration.</span></span>

    <span data-ttu-id="f0aba-149">Därefter bör du se din arbetsyta med din OMS-arbetsytan och resurs gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="f0aba-149">Next you should see your workspace with your OMS Workspace and Resource group names.</span></span> <span data-ttu-id="f0aba-150">Namn måste vara unikt och kan bara använda bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="f0aba-150">Names must be unique and can only use letters, numbers, and hyphens.</span></span> <span data-ttu-id="f0aba-151">Blanksteg och understreck tillåts inte.</span><span class="sxs-lookup"><span data-stu-id="f0aba-151">Spaces and underscores are not allowed.</span></span> 

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. <span data-ttu-id="f0aba-153">Du får sedan ett kort meddelande om att ditt arbetsområde har skapats och du kommer tillbaka tooyour loggning skärm för konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f0aba-153">You next get a short message saying that your workspace has been created and you are returned tooyour logging configuration screen.</span></span> <span data-ttu-id="f0aba-154">Du kan bekräfta hello namnet på logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="f0aba-154">You can confirm hello name of your Log Analytics workspace.</span></span>

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    <span data-ttu-id="f0aba-156">När du har konfigurerat hello konfiguration för logganalys se till att du även kryssrutan hello CoreAnalytics för CDN-loggning.</span><span class="sxs-lookup"><span data-stu-id="f0aba-156">Once you have set up hello Log Analytics configuration, make sure you also check hello CoreAnalytics box for CDN logging.</span></span>

6. <span data-ttu-id="f0aba-157">Om allt tooyour önskemål klickar du på hello **spara** knappen hello överst i hello konfigurationsdialogruta.</span><span class="sxs-lookup"><span data-stu-id="f0aba-157">If everything is tooyour liking, click hello **Save** button at hello top of hello configuration dialog.</span></span>

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/10_Save-me.png)

    <span data-ttu-id="f0aba-159">Hej **spara** knappen inte längre är aktiv och att hello på/av-knappen är nu vidare men blå lila.</span><span class="sxs-lookup"><span data-stu-id="f0aba-159">hello **Save** button is no longer active and that hello ON/OFF button is now ON, but blue instead of purple.</span></span>

7. <span data-ttu-id="f0aba-160">Om du vill toosee din nya OMS-arbetsyta, gå tooyour Azure-portalen instrumentpanelen, klickar du på hello för logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="f0aba-160">If you want toosee your new OMS workspace, go tooyour Azure portal Dashboard, click hello name of your Log Analytics workspace.</span></span> <span data-ttu-id="f0aba-161">Nu visas ditt arbetsområde (se till att OMS-arbetsyta är markerad hello vänster).</span><span class="sxs-lookup"><span data-stu-id="f0aba-161">Next you will see your workspace (make sure that OMS Workspace is highlighted on hello left).</span></span> <span data-ttu-id="f0aba-162">Klicka på hello OMS-portalen panelen toosee arbetsytan i hello OMS-databas.</span><span class="sxs-lookup"><span data-stu-id="f0aba-162">Click on hello OMS Portal tile toosee your workspace in hello OMS repository.</span></span> 

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    <span data-ttu-id="f0aba-164">OMS-databasen är nu redo toolog data.</span><span class="sxs-lookup"><span data-stu-id="f0aba-164">Your OMS repository is now ready toolog data.</span></span> <span data-ttu-id="f0aba-165">Ordna tooconsume data måste du använda en [OMS lösningen](#consuming-oms-log-analytics-data), tas upp senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f0aba-165">In order tooconsume that data, you must use an [OMS Solution](#consuming-oms-log-analytics-data), covered later in this article.</span></span>

<span data-ttu-id="f0aba-166">Mer information om logga data fördröjningar [här](#log-data-delays).</span><span class="sxs-lookup"><span data-stu-id="f0aba-166">For more information about log data delays, go [here](#log-data-delays).</span></span>

## <a name="enable-logging-with-powershell"></a><span data-ttu-id="f0aba-167">Aktivera loggning med PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0aba-167">Enable logging with PowerShell</span></span>

<span data-ttu-id="f0aba-168">Nedan finns ett exempel på hur tooenable och få diagnostikloggar via hello Azure PowerShell-Cmdlets.</span><span class="sxs-lookup"><span data-stu-id="f0aba-168">Below is an example on how tooenable and get Diagnostic Logs via hello Azure PowerShell Cmdlets.</span></span>

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a><span data-ttu-id="f0aba-169">Aktivera diagnostik loggar i ett Lagringskonto</span><span class="sxs-lookup"><span data-stu-id="f0aba-169">Enabling Diagnostic Logs in a Storage Account</span></span>

<span data-ttu-id="f0aba-170">Först logga in och välj en prenumeration:</span><span class="sxs-lookup"><span data-stu-id="f0aba-170">First log in and select a subscription:</span></span>

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


<span data-ttu-id="f0aba-171">tooEnable diagnostikloggar i ett Lagringskonto, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f0aba-171">tooEnable Diagnostic Logs in a Storage Account, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
<span data-ttu-id="f0aba-172">tooEnable diagnostik loggar i OMS-arbetsyta, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f0aba-172">tooEnable Diagnostics Logs in an OMS workspace, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a><span data-ttu-id="f0aba-173">Förbrukar diagnostik loggar från Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f0aba-173">Consuming diagnostics logs from Azure Storage</span></span>
<span data-ttu-id="f0aba-174">Det här avsnittet beskriver hello schemat för hello CDN core analytics, hur de är uppdelade i ett Azure Storage-konto och ger exempel kod toodownload hello loggar i tooa CSV-filen.</span><span class="sxs-lookup"><span data-stu-id="f0aba-174">This section describes hello schema of hello CDN core analytics, how these are organized inside of an Azure Storage Account and provides sample code toodownload hello logs in tooa CSV file.</span></span>

### <a name="using-microsoft-azure-storage-explorer"></a><span data-ttu-id="f0aba-175">Med Microsoft Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="f0aba-175">Using Microsoft Azure Storage Explorer</span></span>
<span data-ttu-id="f0aba-176">Innan du kan komma åt hello core analysdata från hello Azure Storage-konto, måste du först verktyget tooaccess hello innehållet i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f0aba-176">Before you can access hello core analytics data from hello Azure Storage Account, you first need a tool tooaccess hello contents in a storage account.</span></span> <span data-ttu-id="f0aba-177">Det finns flera verktyg i hello marknaden, är hello något som vi rekommenderar hello Microsoft Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="f0aba-177">While there are several tools available in hello market, hello one that we recommend is hello Microsoft Azure Storage Explorer.</span></span> <span data-ttu-id="f0aba-178">Du kan hämta verktyget hello från [här](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="f0aba-178">You can download hello tool from [here](http://storageexplorer.com/).</span></span> <span data-ttu-id="f0aba-179">När du hämtar och installerar programmet hello, konfigurerar den hello toouse samma Azure Storage-konto som har konfigurerats som en destination toohello CDN diagnostik loggar.</span><span class="sxs-lookup"><span data-stu-id="f0aba-179">After downloading and installing hello software, configure it toouse hello same Azure Storage Account that was configured as a destination toohello CDN Diagnostics Logs.</span></span>

1.  <span data-ttu-id="f0aba-180">Öppna **Lagringsutforskaren för Microsoft Azure**</span><span class="sxs-lookup"><span data-stu-id="f0aba-180">Open **Microsoft Azure Storage Explorer**</span></span>
2.  <span data-ttu-id="f0aba-181">Leta upp hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="f0aba-181">Locate hello storage account</span></span>
3.  <span data-ttu-id="f0aba-182">Gå toohello **”Blob-behållare”** nod under lagringen kontot och expandera hello nod</span><span class="sxs-lookup"><span data-stu-id="f0aba-182">Go toohello **“Blob Containers”** node under this storage account and expand hello node</span></span>
4.  <span data-ttu-id="f0aba-183">Välj hello behållare med namnet **”insikter-loggar-coreanalytics”** och dubbelklicka på det.</span><span class="sxs-lookup"><span data-stu-id="f0aba-183">Select hello container named **“insights-logs-coreanalytics”** and double-click it</span></span>
5.  <span data-ttu-id="f0aba-184">Resultatet visar upp i hello högra fönstret från och med hello först nivå, som ser ut så **”resourceId =”**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-184">Results show up on hello right-hand pane starting with hello first level, which looks like **“resourceId=”**.</span></span> <span data-ttu-id="f0aba-185">Klickar du på alla hello sätt tills du ser hello filen **PT1H.json**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-185">Continue clicking all hello way until you see hello file **PT1H.json**.</span></span> <span data-ttu-id="f0aba-186">Se hello följande Obs förklaring av hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="f0aba-186">See hello following note for explanation of hello path.</span></span>
6.  <span data-ttu-id="f0aba-187">Varje blobb **PT1H.json** representerar hello analytics loggar under en timme för en specifik CDN-slutpunkten eller den anpassa domänen.</span><span class="sxs-lookup"><span data-stu-id="f0aba-187">Each blob **PT1H.json** represents hello analytics logs for one hour for a specific CDN endpoint or its custom domain.</span></span>
7.  <span data-ttu-id="f0aba-188">hello schemat för hello innehållet i JSON-fil beskrivs i hello schemat i hello Core Analytics loggar</span><span class="sxs-lookup"><span data-stu-id="f0aba-188">hello schema of hello contents of this JSON file is described in hello section Schema of hello Core Analytics Logs</span></span>


> [!NOTE]
> <span data-ttu-id="f0aba-189">**Formatet för BLOB-sökväg**</span><span class="sxs-lookup"><span data-stu-id="f0aba-189">**Blob path format**</span></span>
> 
> <span data-ttu-id="f0aba-190">Core Analytics loggar skapas varje timme.</span><span class="sxs-lookup"><span data-stu-id="f0aba-190">Core Analytics logs are generated every hour.</span></span> <span data-ttu-id="f0aba-191">Alla data för en timme som samlas in och lagras i en enda Azure-Blob som en JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="f0aba-191">All data for an hour are collected and stored inside a single Azure Blob as a JSON payload.</span></span> <span data-ttu-id="f0aba-192">hello sökvägen toothis Azure Blob visas som om det är en hierarkisk struktur.</span><span class="sxs-lookup"><span data-stu-id="f0aba-192">hello path toothis Azure Blob appears as if there is a hierarchical structure.</span></span> <span data-ttu-id="f0aba-193">Detta är eftersom hello lagring explorer verktyget tolkar '/' som en katalogavgränsare som visar hello hierarki i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="f0aba-193">This is because hello Storage explorer tool interprets '/' as a directory separator and shows hello hierarchy for convenience.</span></span> <span data-ttu-id="f0aba-194">Hello hela sökvägen representerar egentligen, just hello blob-namnet.</span><span class="sxs-lookup"><span data-stu-id="f0aba-194">Actually, hello whole path just represents hello blob name.</span></span> <span data-ttu-id="f0aba-195">Det här namnet på hello blob följer hello följande namngivningskonvention</span><span class="sxs-lookup"><span data-stu-id="f0aba-195">This name of hello blob follows hello following naming convention</span></span> 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

<span data-ttu-id="f0aba-196">**Beskrivning av fält:**</span><span class="sxs-lookup"><span data-stu-id="f0aba-196">**Description of fields:**</span></span>

|<span data-ttu-id="f0aba-197">värde</span><span class="sxs-lookup"><span data-stu-id="f0aba-197">value</span></span>|<span data-ttu-id="f0aba-198">description</span><span class="sxs-lookup"><span data-stu-id="f0aba-198">description</span></span>|
|-------|---------|
|<span data-ttu-id="f0aba-199">Prenumerations-ID:t</span><span class="sxs-lookup"><span data-stu-id="f0aba-199">Subscription ID</span></span>    |<span data-ttu-id="f0aba-200">ID för hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f0aba-200">ID of hello Azure Subscription.</span></span> <span data-ttu-id="f0aba-201">Detta är i hello Guid-format.</span><span class="sxs-lookup"><span data-stu-id="f0aba-201">This is in hello Guid format.</span></span>|
|<span data-ttu-id="f0aba-202">Resurs</span><span class="sxs-lookup"><span data-stu-id="f0aba-202">Resource</span></span> |<span data-ttu-id="f0aba-203">Gruppnamn namn hello resurs grupp toowhich hello CDN resurser tillhör.</span><span class="sxs-lookup"><span data-stu-id="f0aba-203">Group Name   Name of hello resource group toowhich hello CDN resources belong.</span></span>|
|<span data-ttu-id="f0aba-204">Profilnamn</span><span class="sxs-lookup"><span data-stu-id="f0aba-204">Profile Name</span></span> |<span data-ttu-id="f0aba-205">Namnet på hello CDN-profilen</span><span class="sxs-lookup"><span data-stu-id="f0aba-205">Name of hello CDN Profile</span></span>|
|<span data-ttu-id="f0aba-206">Namnet på slutpunkten</span><span class="sxs-lookup"><span data-stu-id="f0aba-206">Endpoint Name</span></span> |<span data-ttu-id="f0aba-207">Namnet på hello CDN-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="f0aba-207">Name of hello CDN Endpoint</span></span>|
|<span data-ttu-id="f0aba-208">År</span><span class="sxs-lookup"><span data-stu-id="f0aba-208">Year</span></span>|  <span data-ttu-id="f0aba-209">4-siffrig representation av hello år exempelvis 2017</span><span class="sxs-lookup"><span data-stu-id="f0aba-209">4-digit representation of hello year for example, 2017</span></span>|
|<span data-ttu-id="f0aba-210">Månad</span><span class="sxs-lookup"><span data-stu-id="f0aba-210">Month</span></span>| <span data-ttu-id="f0aba-211">2-siffrig representation av hello månadsnummer.</span><span class="sxs-lookup"><span data-stu-id="f0aba-211">2-digit representation of hello month number.</span></span> <span data-ttu-id="f0aba-212">01 = januari... 12 = December</span><span class="sxs-lookup"><span data-stu-id="f0aba-212">01=January ... 12=December</span></span>|
|<span data-ttu-id="f0aba-213">Dag</span><span class="sxs-lookup"><span data-stu-id="f0aba-213">Day</span></span>|   <span data-ttu-id="f0aba-214">2 siffra representation av hello dagen i månaden för hello</span><span class="sxs-lookup"><span data-stu-id="f0aba-214">2 digit representation of hello day of hello month</span></span>|
|<span data-ttu-id="f0aba-215">PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="f0aba-215">PT1H.json</span></span>| <span data-ttu-id="f0aba-216">Faktiska JSON-fil som innehåller hello analytics-data</span><span class="sxs-lookup"><span data-stu-id="f0aba-216">Actual JSON file where hello analytics data is stored</span></span>|

### <a name="exporting-hello-core-analytics-data-tooa-csv-file"></a><span data-ttu-id="f0aba-217">Exportera hello Core analysdata tooa CSV-fil</span><span class="sxs-lookup"><span data-stu-id="f0aba-217">Exporting hello Core Analytics Data tooa CSV File</span></span>

<span data-ttu-id="f0aba-218">toomake it lätt tooaccess hello Core Analytics vi erbjuder en exempelkod för ett verktyg som gör att ladda ned hello JSON-filer till en platt CSV-format, vilket kan vara används tooeasily skapa diagram eller andra aggregeringar.</span><span class="sxs-lookup"><span data-stu-id="f0aba-218">toomake it easy tooaccess hello Core Analytics, we provide a sample code for a tool, which allows downloading hello JSON files into a flat comma-separated file format, which can be used tooeasily create charts or other aggregations.</span></span>

<span data-ttu-id="f0aba-219">Här är hur du kan använda verktyget hello:</span><span class="sxs-lookup"><span data-stu-id="f0aba-219">Here is how you can use hello tool:</span></span>

1.  <span data-ttu-id="f0aba-220">Gå hello github länk: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span><span class="sxs-lookup"><span data-stu-id="f0aba-220">Visit hello github link: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span></span>
2.  <span data-ttu-id="f0aba-221">Hämta hello koden</span><span class="sxs-lookup"><span data-stu-id="f0aba-221">Download hello code</span></span>
3.  <span data-ttu-id="f0aba-222">Följ anvisningarna toocompile och konfigurera</span><span class="sxs-lookup"><span data-stu-id="f0aba-222">Follow instructions toocompile and configure</span></span>
4.  <span data-ttu-id="f0aba-223">Kör verktyget hello</span><span class="sxs-lookup"><span data-stu-id="f0aba-223">Run hello tool</span></span>
5.  <span data-ttu-id="f0aba-224">Resulterande CSV-fil innehåller hello analytics-data i en enkel platt hierarki.</span><span class="sxs-lookup"><span data-stu-id="f0aba-224">Resulting CSV file shows hello analytics data in a simple flat hierarchy.</span></span>

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a><span data-ttu-id="f0aba-225">Förbrukar diagnostik loggar från en OMS Log Analytics-databas</span><span class="sxs-lookup"><span data-stu-id="f0aba-225">Consuming diagnostics logs from an OMS Log Analytics repository</span></span>
<span data-ttu-id="f0aba-226">Log Analytics är en tjänst i Operations Management Suite (OMS) och som övervakar dina molntjänster och lokala miljöer toomaintain deras tillgänglighet och prestanda.</span><span class="sxs-lookup"><span data-stu-id="f0aba-226">Log Analytics is a service in Operations Management Suite (OMS) that monitors your cloud and on-premises environments toomaintain their availability and performance.</span></span> <span data-ttu-id="f0aba-227">Den samlar in data som genereras av resurser i dina miljöer i molnet och lokalt och från andra övervakning verktyg tooprovide analys över flera källor.</span><span class="sxs-lookup"><span data-stu-id="f0aba-227">It collects data generated by resources in your cloud and on-premises environments and from other monitoring tools tooprovide analysis across multiple sources.</span></span> 

<span data-ttu-id="f0aba-228">toouse Log Analytics, måste du [aktivera loggning](#enable-logging-with-azure-storage) toohello Azure logganalys för OMS-lagringsplatsen, vilket beskrivs tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f0aba-228">toouse Log Analytics, you must [enable logging](#enable-logging-with-azure-storage) toohello Azure OMS Log Analytics repository, which is discussed earlier in this article.</span></span>

### <a name="using-hello-oms-repository"></a><span data-ttu-id="f0aba-229">Med hjälp av hello OMS-databasen</span><span class="sxs-lookup"><span data-stu-id="f0aba-229">Using hello OMS Repository</span></span>

 <span data-ttu-id="f0aba-230">följande diagram visar hello arkitekturen för hello indata och utdata för hello databasen hello:</span><span class="sxs-lookup"><span data-stu-id="f0aba-230">hello following diagram shows hello architecture of hello inputs and outputs of hello repository:</span></span>

![OMS Log Analytics-databasen](./media/cdn-diagnostics-log/12_Repo-overview.png)

<span data-ttu-id="f0aba-232">*Bild 3 - Log Analytics-databasen*</span><span class="sxs-lookup"><span data-stu-id="f0aba-232">*Figure 3 - Log Analytics Repository*</span></span>

<span data-ttu-id="f0aba-233">Du kan visa hello data i en mängd olika sätt med hjälp av lösningar för hantering.</span><span class="sxs-lookup"><span data-stu-id="f0aba-233">You can display hello data in a variety of ways by using Management Solutions.</span></span> <span data-ttu-id="f0aba-234">Du kan få hanteringslösningar från hello [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span><span class="sxs-lookup"><span data-stu-id="f0aba-234">You can obtain Management Solutions from hello [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span></span>

<span data-ttu-id="f0aba-235">Du kan installera hanteringslösningar från Azure marketplace genom att klicka på hello **blir det nu** länken längst ned hello i varje lösning.</span><span class="sxs-lookup"><span data-stu-id="f0aba-235">You can install management solutions from Azure marketplace by clicking hello **Get it now** link at hello bottom of each solution.</span></span>

### <a name="adding-an-oms-cdn-management-solution"></a><span data-ttu-id="f0aba-236">Lägga till en OMS CDN hanteringslösning</span><span class="sxs-lookup"><span data-stu-id="f0aba-236">Adding an OMS CDN Management Solution</span></span>

<span data-ttu-id="f0aba-237">Följ dessa steg tooadd en lösning:</span><span class="sxs-lookup"><span data-stu-id="f0aba-237">Follow these steps tooadd a Management Solution:</span></span>

1.   <span data-ttu-id="f0aba-238">Om du inte redan har gjort logga in toohello Azure-portalen med din Azure-prenumeration och gå tooyour instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f0aba-238">If you haven't already done so, sign in toohello Azure portal using your Azure subscription and go tooyour Dashboard.</span></span>
    <span data-ttu-id="f0aba-239">![Instrumentpanelen för Azure](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span><span class="sxs-lookup"><span data-stu-id="f0aba-239">![Azure Dashboard](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span></span>

2. <span data-ttu-id="f0aba-240">I hello **ny** bladet under **Marketplace**väljer **övervakning + management**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-240">In hello **New** blade under **Marketplace**, select **Monitoring + management**.</span></span>

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. <span data-ttu-id="f0aba-242">I hello **övervakning + management** bladet, klickar du på **se alla**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-242">In hello **Monitoring + management** blade, click **See all**.</span></span>

    ![Se alla](./media/cdn-diagnostics-log/15_See-all.png)

4.  <span data-ttu-id="f0aba-244">Sök efter CDN i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="f0aba-244">Search for CDN in hello search box.</span></span>

    ![Se alla](./media/cdn-diagnostics-log/16_Search-for.png)

5.  <span data-ttu-id="f0aba-246">Välj **Azure CDN Core Analytics**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-246">Select **Azure CDN Core Analytics**.</span></span> 

    ![Se alla](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  <span data-ttu-id="f0aba-248">När du klickar på **skapa**, kommer du att uppmanas ange toocreate en ny OMS-arbetsyta eller använda en befintlig.</span><span class="sxs-lookup"><span data-stu-id="f0aba-248">After clicking **Create**, you will be asked toocreate a new OMS workspace or use an existing one.</span></span> 

    ![Se alla](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  <span data-ttu-id="f0aba-250">Välj hello arbetsytans innan.</span><span class="sxs-lookup"><span data-stu-id="f0aba-250">Select hello workspace you created before.</span></span> <span data-ttu-id="f0aba-251">Du måste sedan tooadd ett automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f0aba-251">You then need tooadd an automation account.</span></span>

    ![Se alla](./media/cdn-diagnostics-log/19_Add-automation.png)

8. <span data-ttu-id="f0aba-253">hello visar följande skärmbild hello automation kontoformuläret måste du fylla ut.</span><span class="sxs-lookup"><span data-stu-id="f0aba-253">hello following screen shows hello automation account form you must fill out.</span></span> 

    ![Se alla](./media/cdn-diagnostics-log/20_Automation.png)

9. <span data-ttu-id="f0aba-255">När du har skapat hello automation-konto, är du redo tooadd din lösning.</span><span class="sxs-lookup"><span data-stu-id="f0aba-255">Once you have created hello automation account, you are ready tooadd your solution.</span></span> <span data-ttu-id="f0aba-256">Klicka på hello **skapa** knappen.</span><span class="sxs-lookup"><span data-stu-id="f0aba-256">Click hello **Create** button.</span></span>

    ![Se alla](./media/cdn-diagnostics-log/21_Ready.png)

10. <span data-ttu-id="f0aba-258">Din lösning har nu lagts tooyour arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="f0aba-258">Your solution has now been added tooyour workspace.</span></span> <span data-ttu-id="f0aba-259">Gå tillbaka tooyour Azure-portalen instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f0aba-259">Go back tooyour Azure portal Dashboard.</span></span>

    ![Se alla](./media/cdn-diagnostics-log/22_Dashboard.png)

    <span data-ttu-id="f0aba-261">Klicka på hello logganalys-arbetsytan som du skapade toogo tooyour arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="f0aba-261">Click hello Log Analytics workspace you created toogo tooyour workspace.</span></span> 

11. <span data-ttu-id="f0aba-262">Klicka på hello **OMS-portalen** panelen toosee den nya lösningen i hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="f0aba-262">Click hello **OMS Portal** tile toosee your new solution in hello OMS portal.</span></span>

    ![Se alla](./media/cdn-diagnostics-log/23_workspace.png)

12. <span data-ttu-id="f0aba-264">OMS-portalen bör nu se ut som följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="f0aba-264">Your OMS portal should now look like hello following screen:</span></span>

    ![Se alla](./media/cdn-diagnostics-log/24_OMS-solution.png)

    <span data-ttu-id="f0aba-266">Klicka på en hello paneler toosee flera vyer i dina data.</span><span class="sxs-lookup"><span data-stu-id="f0aba-266">Click one of hello tiles toosee several views into your data.</span></span>

    ![Se alla](./media/cdn-diagnostics-log/25_Interior-view.png)

    <span data-ttu-id="f0aba-268">Du kan rulla åt vänster eller höger toosee ytterligare rutor som representerar enskilda vyer i hello data.</span><span class="sxs-lookup"><span data-stu-id="f0aba-268">You can scroll left or right toosee further tiles representing individual views into hello data.</span></span> 

    <span data-ttu-id="f0aba-269">Klicka på någon av hello paneler får du mer information om dina data.</span><span class="sxs-lookup"><span data-stu-id="f0aba-269">Clicking one of hello tiles gives you more details about your data.</span></span>

     ![Se alla](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a><span data-ttu-id="f0aba-271">Erbjudanden och prisnivåer</span><span class="sxs-lookup"><span data-stu-id="f0aba-271">Offers and pricing tiers</span></span>

<span data-ttu-id="f0aba-272">Du kan se erbjudanden och prisnivåer för OMS hanteringslösningar [här](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span><span class="sxs-lookup"><span data-stu-id="f0aba-272">You can see offers and pricing tiers for OMS management solutions [here](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span></span>

### <a name="customizing-views"></a><span data-ttu-id="f0aba-273">Anpassa vyer</span><span class="sxs-lookup"><span data-stu-id="f0aba-273">Customizing views</span></span>

<span data-ttu-id="f0aba-274">Du kan anpassa hello vyn i dina data med hjälp av hello **Vydesigner**.</span><span class="sxs-lookup"><span data-stu-id="f0aba-274">You can customize hello view into your data by using hello **View Designer**.</span></span> <span data-ttu-id="f0aba-275">Gå tooyour OMS-arbetsytan och börjar designa genom att klicka på hello **Vydesigner** panelen.</span><span class="sxs-lookup"><span data-stu-id="f0aba-275">Go tooyour OMS workspace and begin designing by clicking hello **View Designer** tile.</span></span>

![Vydesigner](./media/cdn-diagnostics-log/27_Designer.png)

<span data-ttu-id="f0aba-277">Du kan dra diagramtyper från hello vänster och Fyll i hello information om du vill tooanalyze på hello kvar.</span><span class="sxs-lookup"><span data-stu-id="f0aba-277">You can drag and drop types of charts from hello left and fill in hello data details you want tooanalyze on hello left.</span></span>

![Vydesigner](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a><span data-ttu-id="f0aba-279">Logga data fördröjningar</span><span class="sxs-lookup"><span data-stu-id="f0aba-279">Log data delays</span></span>

<span data-ttu-id="f0aba-280">Verizon logga data fördröjningar</span><span class="sxs-lookup"><span data-stu-id="f0aba-280">Verizon log data delays</span></span> | <span data-ttu-id="f0aba-281">Akamai logga data fördröjningar</span><span class="sxs-lookup"><span data-stu-id="f0aba-281">Akamai log data delays</span></span>
--- | ---
<span data-ttu-id="f0aba-282">Loggdata för Verizon försenas 1 timme och tar upp too2 timmar toostart visas efter att endpoint-spridningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f0aba-282">Verizon log data is 1 hour delayed, and take up too2 hours toostart appearing after endpoint propagation completion.</span></span> | <span data-ttu-id="f0aba-283">Loggdata för Akamai är 24 timmar fördröjd och tar upp too2 timmar toostart visas om det skapades mer än 24 timmar sedan.</span><span class="sxs-lookup"><span data-stu-id="f0aba-283">Akamai log data is 24 hours delayed, and takes up too2 hours toostart appearing if it was created more than 24 hours ago.</span></span> <span data-ttu-id="f0aba-284">Om den nyligen har skapats kan det ta upp too25 timmar för hello loggar toostart visas.</span><span class="sxs-lookup"><span data-stu-id="f0aba-284">If it was recently created, it can take up too25 hours for hello logs toostart appearing.</span></span>

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a><span data-ttu-id="f0aba-285">Typer av diagnostiska loggen för CDN Core Analytics</span><span class="sxs-lookup"><span data-stu-id="f0aba-285">Diagnostic log types for CDN Core Analytics</span></span>

<span data-ttu-id="f0aba-286">Vi har för närvarande endast Core Analytics loggarna, vilket innehåller mått som visar statistik för HTTP-svar och utgång sett från hello CDN POP/kanter.</span><span class="sxs-lookup"><span data-stu-id="f0aba-286">We currently offer only Core Analytics logs, which contain metrics showing HTTP response statistics and egress statistics as seen from hello CDN POPs/edges.</span></span>

### <a name="core-analytics-metrics-details"></a><span data-ttu-id="f0aba-287">Core Analytics mätvärden information</span><span class="sxs-lookup"><span data-stu-id="f0aba-287">Core Analytics Metrics Details</span></span>
<span data-ttu-id="f0aba-288">hello följande tabell visar en lista över tillgängliga i hello Core Analytics loggar mått.</span><span class="sxs-lookup"><span data-stu-id="f0aba-288">hello following table shows a list of metrics available in hello Core Analytics logs.</span></span> <span data-ttu-id="f0aba-289">Inte alla mått är tillgängliga från alla leverantörer, även om dessa skillnader är minimal.</span><span class="sxs-lookup"><span data-stu-id="f0aba-289">Not all metrics are available from all providers, although such differences are minimal.</span></span> <span data-ttu-id="f0aba-290">hello också följande tabell visar om ett visst mått är tillgänglig från en leverantör.</span><span class="sxs-lookup"><span data-stu-id="f0aba-290">hello following table also shows if a given metric is available from a provider.</span></span> <span data-ttu-id="f0aba-291">Observera att hello mått är tillgängliga för bara de CDN-slutpunkter som har trafik på dem..</span><span class="sxs-lookup"><span data-stu-id="f0aba-291">Please note that hello metrics are available for only those CDN endpoints that have traffic on them.</span></span>


|<span data-ttu-id="f0aba-292">Mått</span><span class="sxs-lookup"><span data-stu-id="f0aba-292">Metric</span></span>                     | <span data-ttu-id="f0aba-293">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0aba-293">Description</span></span>   | <span data-ttu-id="f0aba-294">Verizon</span><span class="sxs-lookup"><span data-stu-id="f0aba-294">Verizon</span></span>  | <span data-ttu-id="f0aba-295">Akamai</span><span class="sxs-lookup"><span data-stu-id="f0aba-295">Akamai</span></span> 
|---------------------------|---------------|---|---|
| <span data-ttu-id="f0aba-296">RequestCountTotal</span><span class="sxs-lookup"><span data-stu-id="f0aba-296">RequestCountTotal</span></span>         |<span data-ttu-id="f0aba-297">Totalt antal träffar för begäran under denna period</span><span class="sxs-lookup"><span data-stu-id="f0aba-297">Total number of request hits during this period</span></span>| <span data-ttu-id="f0aba-298">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-298">Yes</span></span>  |<span data-ttu-id="f0aba-299">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-299">Yes</span></span>   |
| <span data-ttu-id="f0aba-300">RequestCountHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="f0aba-300">RequestCountHttpStatus2xx</span></span> |<span data-ttu-id="f0aba-301">Antal alla begäranden som resulterade i en 2xx http-kod (t.ex. 200, 202)</span><span class="sxs-lookup"><span data-stu-id="f0aba-301">Count of all requests that resulted in a 2xx HTTP code (e.g. 200, 202)</span></span>              | <span data-ttu-id="f0aba-302">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-302">Yes</span></span>  |<span data-ttu-id="f0aba-303">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-303">Yes</span></span>   |
| <span data-ttu-id="f0aba-304">RequestCountHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="f0aba-304">RequestCountHttpStatus3xx</span></span> | <span data-ttu-id="f0aba-305">Antal alla begäranden som resulterade i en 3xx http-kod (t.ex. 300, 302)</span><span class="sxs-lookup"><span data-stu-id="f0aba-305">Count of all requests that resulted in a 3xx HTTP code (e.g. 300, 302)</span></span>              | <span data-ttu-id="f0aba-306">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-306">Yes</span></span>  |<span data-ttu-id="f0aba-307">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-307">Yes</span></span>   |
| <span data-ttu-id="f0aba-308">RequestCountHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="f0aba-308">RequestCountHttpStatus4xx</span></span> |<span data-ttu-id="f0aba-309">Antal alla begäranden som resulterade i en 4xx http-kod (t.ex. 400, 404)</span><span class="sxs-lookup"><span data-stu-id="f0aba-309">Count of all requests that resulted in a 4xx HTTP code (e.g. 400, 404)</span></span>               | <span data-ttu-id="f0aba-310">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-310">Yes</span></span>   |<span data-ttu-id="f0aba-311">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-311">Yes</span></span>   |
| <span data-ttu-id="f0aba-312">RequestCountHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="f0aba-312">RequestCountHttpStatus5xx</span></span> | <span data-ttu-id="f0aba-313">Antal alla begäranden som resulterade i en 5xx http-kod (t.ex. 500, 504)</span><span class="sxs-lookup"><span data-stu-id="f0aba-313">Count of all requests that resulted in a 5xx HTTP code (e.g. 500, 504)</span></span>              | <span data-ttu-id="f0aba-314">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-314">Yes</span></span>  |<span data-ttu-id="f0aba-315">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-315">Yes</span></span>   |
| <span data-ttu-id="f0aba-316">RequestCountHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="f0aba-316">RequestCountHttpStatusOthers</span></span> |  <span data-ttu-id="f0aba-317">Uppräkning av alla andra HTTP-koder (utanför 2xx 5xx)</span><span class="sxs-lookup"><span data-stu-id="f0aba-317">Count of all other HTTP codes (outside of 2xx-5xx)</span></span> | <span data-ttu-id="f0aba-318">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-318">Yes</span></span>  |<span data-ttu-id="f0aba-319">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-319">Yes</span></span>   |
| <span data-ttu-id="f0aba-320">RequestCountHttpStatus200</span><span class="sxs-lookup"><span data-stu-id="f0aba-320">RequestCountHttpStatus200</span></span> | <span data-ttu-id="f0aba-321">Antal alla begäranden som resulterade i en 200 HTTP-svar för kod</span><span class="sxs-lookup"><span data-stu-id="f0aba-321">Count of all requests that resulted in a 200 HTTP code response</span></span>              |<span data-ttu-id="f0aba-322">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-322">No</span></span>   |<span data-ttu-id="f0aba-323">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-323">Yes</span></span>   |
| <span data-ttu-id="f0aba-324">RequestCountHttpStatus206</span><span class="sxs-lookup"><span data-stu-id="f0aba-324">RequestCountHttpStatus206</span></span> | <span data-ttu-id="f0aba-325">Antal alla begäranden som resulterade i ett 206 HTTP-svar för kod</span><span class="sxs-lookup"><span data-stu-id="f0aba-325">Count of all requests that resulted in a 206 HTTP code response</span></span>              |<span data-ttu-id="f0aba-326">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-326">No</span></span>   |<span data-ttu-id="f0aba-327">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-327">Yes</span></span>   |
| <span data-ttu-id="f0aba-328">RequestCountHttpStatus302</span><span class="sxs-lookup"><span data-stu-id="f0aba-328">RequestCountHttpStatus302</span></span> | <span data-ttu-id="f0aba-329">Antal alla begäranden som resulterade i en 302 HTTP-svar för kod</span><span class="sxs-lookup"><span data-stu-id="f0aba-329">Count of all requests that resulted in a 302 HTTP code response</span></span>              |<span data-ttu-id="f0aba-330">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-330">No</span></span>   |<span data-ttu-id="f0aba-331">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-331">Yes</span></span>   |
| <span data-ttu-id="f0aba-332">RequestCountHttpStatus304</span><span class="sxs-lookup"><span data-stu-id="f0aba-332">RequestCountHttpStatus304</span></span> |  <span data-ttu-id="f0aba-333">Antal alla begäranden som resulterade i ett 304 HTTP-svar för kod</span><span class="sxs-lookup"><span data-stu-id="f0aba-333">Count of all requests that resulted in a 304 HTTP code response</span></span>             |<span data-ttu-id="f0aba-334">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-334">No</span></span>   |<span data-ttu-id="f0aba-335">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-335">Yes</span></span>   |
| <span data-ttu-id="f0aba-336">RequestCountHttpStatus404</span><span class="sxs-lookup"><span data-stu-id="f0aba-336">RequestCountHttpStatus404</span></span> | <span data-ttu-id="f0aba-337">Antal alla begäranden som resulterade i ett 404 HTTP-svar för kod</span><span class="sxs-lookup"><span data-stu-id="f0aba-337">Count of all requests that resulted in a 404 HTTP code response</span></span>              |<span data-ttu-id="f0aba-338">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-338">No</span></span>   |<span data-ttu-id="f0aba-339">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-339">Yes</span></span>   |
| <span data-ttu-id="f0aba-340">RequestCountCacheHit</span><span class="sxs-lookup"><span data-stu-id="f0aba-340">RequestCountCacheHit</span></span> |<span data-ttu-id="f0aba-341">Antal alla begäranden som resulterade i ett antal träffar.</span><span class="sxs-lookup"><span data-stu-id="f0aba-341">Count of all requests that resulted in a Cache Hit.</span></span> <span data-ttu-id="f0aba-342">Det innebär att hello tillgången behandlades direkt från hello POP toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="f0aba-342">This means hello asset was served directly from hello POP toohello Client.</span></span>               | <span data-ttu-id="f0aba-343">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-343">Yes</span></span>  |<span data-ttu-id="f0aba-344">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-344">No</span></span>   |
| <span data-ttu-id="f0aba-345">RequestCountCacheMiss</span><span class="sxs-lookup"><span data-stu-id="f0aba-345">RequestCountCacheMiss</span></span> | <span data-ttu-id="f0aba-346">Antal alla begäranden som resulterade i en Cache-Miss.</span><span class="sxs-lookup"><span data-stu-id="f0aba-346">Count of all requests that resulted in a Cache Miss.</span></span> <span data-ttu-id="f0aba-347">Detta innebär hello tillgången hittades inte på hello POP närmaste toohello klient och därför har hämtats från hello ursprung.</span><span class="sxs-lookup"><span data-stu-id="f0aba-347">This means hello asset was not found on hello POP closest toohello client, and therefore was retrieved from hello Origin.</span></span>              |<span data-ttu-id="f0aba-348">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-348">Yes</span></span>   | <span data-ttu-id="f0aba-349">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-349">No</span></span>  |
| <span data-ttu-id="f0aba-350">RequestCountCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="f0aba-350">RequestCountCacheNoCache</span></span> | <span data-ttu-id="f0aba-351">Antalet begäranden tooan tillgång som hindras från att cachelagras på grund av tooa Användarkonfiguration hello kant.</span><span class="sxs-lookup"><span data-stu-id="f0aba-351">Count of all requests tooan asset that are prevented from being cached due tooa user configuration on hello edge.</span></span>              |<span data-ttu-id="f0aba-352">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-352">Yes</span></span>   | <span data-ttu-id="f0aba-353">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-353">No</span></span>  |
| <span data-ttu-id="f0aba-354">RequestCountCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="f0aba-354">RequestCountCacheUncacheable</span></span> | <span data-ttu-id="f0aba-355">Antalet begäranden tooassets hindras från att cachelagras av hello tillgången Cache-Control och upphör att gälla rubriker, vilket indikerar att det inte ska cachelagras på en POP eller av hello HTTP-klienten</span><span class="sxs-lookup"><span data-stu-id="f0aba-355">Count of all requests tooassets that are prevented from being cached by hello asset's Cache-Control and Expires headers, which indicate that it should not be cached on a POP or by hello HTTP client</span></span>                |<span data-ttu-id="f0aba-356">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-356">Yes</span></span>   |<span data-ttu-id="f0aba-357">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-357">No</span></span>   |
| <span data-ttu-id="f0aba-358">RequestCountCacheOthers</span><span class="sxs-lookup"><span data-stu-id="f0aba-358">RequestCountCacheOthers</span></span> | <span data-ttu-id="f0aba-359">Antal begäranden med cachen inte omfattas av ovan.</span><span class="sxs-lookup"><span data-stu-id="f0aba-359">Count of all requests with cache status not covered by above.</span></span>              |<span data-ttu-id="f0aba-360">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-360">Yes</span></span>   | <span data-ttu-id="f0aba-361">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-361">No</span></span>  |
| <span data-ttu-id="f0aba-362">EgressTotal</span><span class="sxs-lookup"><span data-stu-id="f0aba-362">EgressTotal</span></span> | <span data-ttu-id="f0aba-363">Utgående dataöverföring i GB</span><span class="sxs-lookup"><span data-stu-id="f0aba-363">Outbound data transfer in GB</span></span>              |<span data-ttu-id="f0aba-364">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-364">Yes</span></span>   |<span data-ttu-id="f0aba-365">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-365">Yes</span></span>   |
| <span data-ttu-id="f0aba-366">EgressHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="f0aba-366">EgressHttpStatus2xx</span></span> | <span data-ttu-id="f0aba-367">Utgående data transfer * för svar med 2xx HTTP-statuskoder i GB</span><span class="sxs-lookup"><span data-stu-id="f0aba-367">Outbound data transfer* for responses with 2xx HTTP status codes in GB</span></span>            |<span data-ttu-id="f0aba-368">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-368">Yes</span></span>   |<span data-ttu-id="f0aba-369">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-369">No</span></span>   |
| <span data-ttu-id="f0aba-370">EgressHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="f0aba-370">EgressHttpStatus3xx</span></span> | <span data-ttu-id="f0aba-371">Utgående dataöverföring för svar med 3xx HTTP-statuskoder i GB</span><span class="sxs-lookup"><span data-stu-id="f0aba-371">Outbound data transfer for responses with 3xx HTTP status codes in GB</span></span>              |<span data-ttu-id="f0aba-372">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-372">Yes</span></span>   |<span data-ttu-id="f0aba-373">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-373">No</span></span>   |
| <span data-ttu-id="f0aba-374">EgressHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="f0aba-374">EgressHttpStatus4xx</span></span> | <span data-ttu-id="f0aba-375">Utgående dataöverföring för svar med 4xx HTTP-statuskoder i GB</span><span class="sxs-lookup"><span data-stu-id="f0aba-375">Outbound data transfer for responses with 4xx HTTP status codes in GB</span></span>               |<span data-ttu-id="f0aba-376">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-376">Yes</span></span>   | <span data-ttu-id="f0aba-377">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-377">No</span></span>  |
| <span data-ttu-id="f0aba-378">EgressHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="f0aba-378">EgressHttpStatus5xx</span></span> | <span data-ttu-id="f0aba-379">Utgående dataöverföring för svar med 5xx HTTP-statuskoder i GB</span><span class="sxs-lookup"><span data-stu-id="f0aba-379">Outbound data transfer for responses with 5xx HTTP status codes in GB</span></span>               |<span data-ttu-id="f0aba-380">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-380">Yes</span></span>   |  <span data-ttu-id="f0aba-381">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-381">No</span></span> |
| <span data-ttu-id="f0aba-382">EgressHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="f0aba-382">EgressHttpStatusOthers</span></span> | <span data-ttu-id="f0aba-383">Utgående dataöverföring för svar med andra HTTP-statuskoder i GB</span><span class="sxs-lookup"><span data-stu-id="f0aba-383">Outbound data transfer for responses with other HTTP status codes in GB</span></span>                |<span data-ttu-id="f0aba-384">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-384">Yes</span></span>   |<span data-ttu-id="f0aba-385">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-385">No</span></span>   |
| <span data-ttu-id="f0aba-386">EgressCacheHit</span><span class="sxs-lookup"><span data-stu-id="f0aba-386">EgressCacheHit</span></span> |  <span data-ttu-id="f0aba-387">Utgående dataöverföring för svar som har levererats direkt från hello CDN cache på hello CDN POP/kanter</span><span class="sxs-lookup"><span data-stu-id="f0aba-387">Outbound data transfer for responses that were delivered directly from hello CDN cache on hello CDN POPs/Edges</span></span>  |<span data-ttu-id="f0aba-388">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-388">Yes</span></span>   |  <span data-ttu-id="f0aba-389">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-389">No</span></span> |
| <span data-ttu-id="f0aba-390">EgressCacheMiss</span><span class="sxs-lookup"><span data-stu-id="f0aba-390">EgressCacheMiss</span></span> | <span data-ttu-id="f0aba-391">Utgående dataöverföring för svar som inte hittades på hello närmaste POP-server, och hämtas från hello ursprungsservern</span><span class="sxs-lookup"><span data-stu-id="f0aba-391">Outbound data transfer for responses that were not found on hello nearest POP server, and retrieved from hello origin server</span></span>              |<span data-ttu-id="f0aba-392">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-392">Yes</span></span>   |  <span data-ttu-id="f0aba-393">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-393">No</span></span> |
| <span data-ttu-id="f0aba-394">EgressCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="f0aba-394">EgressCacheNoCache</span></span> | <span data-ttu-id="f0aba-395">Utgående dataöverföring för tillgångar som hindras från att cachelagras på grund av tooa Användarkonfiguration hello kant.</span><span class="sxs-lookup"><span data-stu-id="f0aba-395">Outbound data transfer for assets that are prevented from being cached due tooa user configuration on hello edge.</span></span>                |<span data-ttu-id="f0aba-396">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-396">Yes</span></span>   |<span data-ttu-id="f0aba-397">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-397">No</span></span>   |
| <span data-ttu-id="f0aba-398">EgressCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="f0aba-398">EgressCacheUncacheable</span></span> | <span data-ttu-id="f0aba-399">Utgående dataöverföring för tillgångar som hindras från att cachelagras av hello tillgången Cache-Control eller Expires-huvuden som indikerar att det inte ska cachelagras på en POP eller av hello HTTP-klienten</span><span class="sxs-lookup"><span data-stu-id="f0aba-399">Outbound data transfer for assets that are prevented from being cached by hello asset's Cache-Control and/or Expires headers, which indicate that it should not be cached on a POP or by hello HTTP client</span></span>                    |<span data-ttu-id="f0aba-400">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-400">Yes</span></span>   | <span data-ttu-id="f0aba-401">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-401">No</span></span>  |
| <span data-ttu-id="f0aba-402">EgressCacheOthers</span><span class="sxs-lookup"><span data-stu-id="f0aba-402">EgressCacheOthers</span></span> |  <span data-ttu-id="f0aba-403">Utgående dataöverföringar för andra cache-scenarier.</span><span class="sxs-lookup"><span data-stu-id="f0aba-403">Outbound data transfers for other cache scenarios.</span></span>             |<span data-ttu-id="f0aba-404">Ja</span><span class="sxs-lookup"><span data-stu-id="f0aba-404">Yes</span></span>   | <span data-ttu-id="f0aba-405">Nej</span><span class="sxs-lookup"><span data-stu-id="f0aba-405">No</span></span>  |

<span data-ttu-id="f0aba-406">* Utgående dataöverföring refererar tootraffic levereras från CDN POP servrar toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="f0aba-406">*Outbound data transfer refers tootraffic delivered from CDN POP servers toohello client.</span></span>


### <a name="schema-of-hello-core-analytics-logs"></a><span data-ttu-id="f0aba-407">Schemat för hello Core Analytics loggar</span><span class="sxs-lookup"><span data-stu-id="f0aba-407">Schema of hello Core Analytics Logs</span></span> 

<span data-ttu-id="f0aba-408">Alla loggar lagras i JSON-format och varje post innehåller strängfält följande hello nedan schemat:</span><span class="sxs-lookup"><span data-stu-id="f0aba-408">All logs are stored in JSON format and each entry has string fields following hello below schema:</span></span>

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of hello CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of hello domain for which hello statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

<span data-ttu-id="f0aba-409">Där representerar hello ”time” hello starttiden för hello timme gräns som hello statistik rapporteras.</span><span class="sxs-lookup"><span data-stu-id="f0aba-409">Where hello ‘time’ represents hello start time of hello hour boundary for which hello statistics is reported.</span></span> <span data-ttu-id="f0aba-410">När ett mått inte stöds av en CDN-providern, i stället för ett värde för double eller heltal, ska det finnas ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="f0aba-410">When a metric is not supported by a CDN provider, instead of a double or integer value, there will be a null value.</span></span> <span data-ttu-id="f0aba-411">Null-värde som anger hello avsaknaden av ett mått och detta skiljer sig från värdet 0.</span><span class="sxs-lookup"><span data-stu-id="f0aba-411">This null value indicates hello absence of a metric, and this is different from a 0 value.</span></span> <span data-ttu-id="f0aba-412">Observera också att en uppsättning av de här måtten per domän som konfigurerats på hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f0aba-412">Also note that there will be one set of these metrics per domain configured on hello endpoint.</span></span>

<span data-ttu-id="f0aba-413">Exempel egenskaper nedan:</span><span class="sxs-lookup"><span data-stu-id="f0aba-413">Example properties below:</span></span>

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a><span data-ttu-id="f0aba-414">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f0aba-414">Additional resources</span></span>

* [<span data-ttu-id="f0aba-415">Azure diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="f0aba-415">Azure Diagnostic logs</span></span>](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [<span data-ttu-id="f0aba-416">Core analytics via Azure CDN kompletterande portalen</span><span class="sxs-lookup"><span data-stu-id="f0aba-416">Core analytics via Azure CDN supplemental portal</span></span>](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [<span data-ttu-id="f0aba-417">Azure OMS logganalys</span><span class="sxs-lookup"><span data-stu-id="f0aba-417">Azure OMS Log Analytics</span></span>](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [<span data-ttu-id="f0aba-418">Azure logganalys REST-API</span><span class="sxs-lookup"><span data-stu-id="f0aba-418">Azure Log Analytics REST API</span></span>](https://docs.microsoft.com/en-us/rest/api/loganalytics)







