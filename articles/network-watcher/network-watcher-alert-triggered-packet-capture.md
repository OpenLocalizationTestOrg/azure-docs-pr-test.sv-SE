---
title: "aaaUse paket avbilda toodo proaktiv nätverk övervakning med aviseringar och Azure Functions | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate en avisering utlöses paketinsamling med Azure Nätverksbevakaren"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 4722a831f3a9d5537c0e6f53daba4dfc35d0cf24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a><span data-ttu-id="025b5-103">Använd paketinsamling för proaktiv nätverksövervakning med varningar och Azure Functions</span><span class="sxs-lookup"><span data-stu-id="025b5-103">Use packet capture for proactive network monitoring with alerts and Azure Functions</span></span>

<span data-ttu-id="025b5-104">Nätverket Watcher paketinsamling skapar avbilda sessioner tootrack trafik till och från virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="025b5-104">Network Watcher packet capture creates capture sessions tootrack traffic in and out of virtual machines.</span></span> <span data-ttu-id="025b5-105">hello filen kan ha ett filter som definieras tootrack hello endast trafik som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="025b5-105">hello capture file can have a filter that is defined tootrack only hello traffic that you want toomonitor.</span></span> <span data-ttu-id="025b5-106">Dessa data lagras sedan i en lagringsblob-eller lokalt på hello gästdatorn.</span><span class="sxs-lookup"><span data-stu-id="025b5-106">This data is then stored in a storage blob or locally on hello guest machine.</span></span>

<span data-ttu-id="025b5-107">Den här funktionen kan startas från en fjärrdator från andra automatiseringsscenarier, till exempel Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="025b5-107">This capability can be started remotely from other automation scenarios such as Azure Functions.</span></span> <span data-ttu-id="025b5-108">Paketet avbilda ger du hello kapaciteten toorun proaktiv insamlingar baserat på definierad avvikelser i nätverket.</span><span class="sxs-lookup"><span data-stu-id="025b5-108">Packet capture gives you hello capability toorun proactive captures based on defined network anomalies.</span></span> <span data-ttu-id="025b5-109">Andra användningsområden omfattar att samla in nätverksstatistik för att hämta information om nätverket intrång och felsökning klient-/ serverkommunikation.</span><span class="sxs-lookup"><span data-stu-id="025b5-109">Other uses include gathering network statistics, getting information about network intrusions, debugging client-server communications, and more.</span></span>

<span data-ttu-id="025b5-110">Resurser som distribueras i Azure kör 24/7.</span><span class="sxs-lookup"><span data-stu-id="025b5-110">Resources that are deployed in Azure run 24/7.</span></span> <span data-ttu-id="025b5-111">Du och din personal kan inte aktivt övervaka hello status för alla resurser 24/7.</span><span class="sxs-lookup"><span data-stu-id="025b5-111">You and your staff cannot actively monitor hello status of all resources 24/7.</span></span> <span data-ttu-id="025b5-112">Till exempel vad händer om ett problem inträffar kl 2?</span><span class="sxs-lookup"><span data-stu-id="025b5-112">For example, what happens if an issue occurs at 2 AM?</span></span>

<span data-ttu-id="025b5-113">Genom att använda Nätverksbevakaren, aviseringar och funktioner från inom hello Azure-ekosystemet, kan du proaktivt svara med hello data och verktyg toosolve problem i nätverket.</span><span class="sxs-lookup"><span data-stu-id="025b5-113">By using Network Watcher, alerting, and functions from within hello Azure ecosystem, you can proactively respond with hello data and tools toosolve problems in your network.</span></span>

![Scenario][scenario]

## <a name="prerequisites"></a><span data-ttu-id="025b5-115">Krav</span><span class="sxs-lookup"><span data-stu-id="025b5-115">Prerequisites</span></span>

* <span data-ttu-id="025b5-116">hello senaste versionen av [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="025b5-116">hello latest version of [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span></span>
* <span data-ttu-id="025b5-117">En befintlig instans av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="025b5-117">An existing instance of Network Watcher.</span></span> <span data-ttu-id="025b5-118">Om du inte redan har en, [skapa en instans av Nätverksbevakaren](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="025b5-118">If you don't already have one, [create an instance of Network Watcher](network-watcher-create.md).</span></span>
* <span data-ttu-id="025b5-119">En befintlig virtuell dator i hello samma region som Nätverksbevakaren med hello [Windows tillägget](../virtual-machines/windows/extensions-nwa.md) eller [Linux-tillägg för virtuell dator](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="025b5-119">An existing virtual machine in hello same region as Network Watcher with hello [Windows extension](../virtual-machines/windows/extensions-nwa.md) or [Linux virtual machine extension](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="025b5-120">Scenario</span><span class="sxs-lookup"><span data-stu-id="025b5-120">Scenario</span></span>

<span data-ttu-id="025b5-121">Din virtuella dator skickar flera TCP-segment än vanligt i det här exemplet och du vill toobe aviserad om.</span><span class="sxs-lookup"><span data-stu-id="025b5-121">In this example, your VM is sending more TCP segments than usual, and you want toobe alerted.</span></span> <span data-ttu-id="025b5-122">TCP-segment som används som exempel här, men du kan använda alla aviseringstillståndet.</span><span class="sxs-lookup"><span data-stu-id="025b5-122">TCP segments are used as an example here, but you can use any alert condition.</span></span>

<span data-ttu-id="025b5-123">När du meddelas du tooreceive på paketnivå data toounderstand varför kommunikation har ökat.</span><span class="sxs-lookup"><span data-stu-id="025b5-123">When you are alerted, you want tooreceive packet-level data toounderstand why communication has increased.</span></span> <span data-ttu-id="025b5-124">Du kan sedan vidta åtgärder tooreturn hello virtuella tooregular kommunikation.</span><span class="sxs-lookup"><span data-stu-id="025b5-124">Then you can take steps tooreturn hello virtual machine tooregular communication.</span></span>

<span data-ttu-id="025b5-125">Det här scenariot förutsätter att du har en befintlig instans av Nätverksbevakaren och en resursgrupp med en giltig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="025b5-125">This scenario assumes that you have an existing instance of Network Watcher and a resource group with a valid virtual machine.</span></span>

<span data-ttu-id="025b5-126">hello följande lista är en översikt över hello arbetsflöde som äger rum:</span><span class="sxs-lookup"><span data-stu-id="025b5-126">hello following list is an overview of hello workflow that takes place:</span></span>

1. <span data-ttu-id="025b5-127">En avisering utlöses på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="025b5-127">An alert is triggered on your VM.</span></span>
1. <span data-ttu-id="025b5-128">hello avisering anropar din Azure-funktion via en webhook.</span><span class="sxs-lookup"><span data-stu-id="025b5-128">hello alert calls your Azure function via a webhook.</span></span>
1. <span data-ttu-id="025b5-129">Din Azure-funktion bearbetar hello aviseringen och startar en Nätverksbevakaren paket avbildningssessionen.</span><span class="sxs-lookup"><span data-stu-id="025b5-129">Your Azure function processes hello alert and starts a Network Watcher packet capture session.</span></span>
1. <span data-ttu-id="025b5-130">hello paketinsamling körs på hello VM och samlar in trafik.</span><span class="sxs-lookup"><span data-stu-id="025b5-130">hello packet capture runs on hello VM and collects traffic.</span></span>
1. <span data-ttu-id="025b5-131">hello paket filen har överförts tooa storage-konto för granskning och diagnos.</span><span class="sxs-lookup"><span data-stu-id="025b5-131">hello packet capture file is uploaded tooa storage account for review and diagnosis.</span></span>

<span data-ttu-id="025b5-132">tooautomate den här processen kan vi skapa och ansluta en avisering på vår VM tootrigger när hello incident inträffar.</span><span class="sxs-lookup"><span data-stu-id="025b5-132">tooautomate this process, we create and connect an alert on our VM tootrigger when hello incident occurs.</span></span> <span data-ttu-id="025b5-133">Vi kan också skapa en funktion toocall i Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="025b5-133">We also create a function toocall into Network Watcher.</span></span>

<span data-ttu-id="025b5-134">Det här scenariot hello följande:</span><span class="sxs-lookup"><span data-stu-id="025b5-134">This scenario does hello following:</span></span>

* <span data-ttu-id="025b5-135">Skapar en Azure-funktion som startar en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="025b5-135">Creates an Azure function that starts a packet capture.</span></span>
* <span data-ttu-id="025b5-136">Skapar en aviseringsregel på en virtuell dator och konfigurerar hello varningsregeln toocall hello Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="025b5-136">Creates an alert rule on a virtual machine and configures hello alert rule toocall hello Azure function.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="025b5-137">Skapa en Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="025b5-137">Create an Azure function</span></span>

<span data-ttu-id="025b5-138">hello första steget är toocreate en Azure-funktionen tooprocess hello avisering och skapa en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="025b5-138">hello first step is toocreate an Azure function tooprocess hello alert and create a packet capture.</span></span>

1. <span data-ttu-id="025b5-139">I hello [Azure-portalen](https://portal.azure.com)väljer **ny** > **Compute** > **Funktionsapp**.</span><span class="sxs-lookup"><span data-stu-id="025b5-139">In hello [Azure portal](https://portal.azure.com), select **New** > **Compute** > **Function App**.</span></span>

    ![Skapa en funktionsapp][1-1]

2. <span data-ttu-id="025b5-141">På hello **Funktionsapp** bladet ange hello följande värden och markera sedan **OK** toocreate hello app:</span><span class="sxs-lookup"><span data-stu-id="025b5-141">On hello **Function App** blade, enter hello following values, and then select **OK** toocreate hello app:</span></span>

    |<span data-ttu-id="025b5-142">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="025b5-142">**Setting**</span></span> | <span data-ttu-id="025b5-143">**Värde**</span><span class="sxs-lookup"><span data-stu-id="025b5-143">**Value**</span></span> | <span data-ttu-id="025b5-144">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="025b5-144">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="025b5-145">**Appens namn**</span><span class="sxs-lookup"><span data-stu-id="025b5-145">**App name**</span></span>|<span data-ttu-id="025b5-146">PacketCaptureExample</span><span class="sxs-lookup"><span data-stu-id="025b5-146">PacketCaptureExample</span></span>|<span data-ttu-id="025b5-147">hello namnet på hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="025b5-147">hello name of hello function app.</span></span>|
    |<span data-ttu-id="025b5-148">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="025b5-148">**Subscription**</span></span>|<span data-ttu-id="025b5-149">[Din prenumeration] hello prenumerationen för vilken toocreate hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="025b5-149">[Your subscription]hello subscription for which toocreate hello function app.</span></span>||
    |<span data-ttu-id="025b5-150">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="025b5-150">**Resource Group**</span></span>|<span data-ttu-id="025b5-151">PacketCaptureRG</span><span class="sxs-lookup"><span data-stu-id="025b5-151">PacketCaptureRG</span></span>|<span data-ttu-id="025b5-152">hello resurs grupp toocontain hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="025b5-152">hello resource group toocontain hello function app.</span></span>|
    |<span data-ttu-id="025b5-153">**Värd för planen**</span><span class="sxs-lookup"><span data-stu-id="025b5-153">**Hosting Plan**</span></span>|<span data-ttu-id="025b5-154">Planera för användning</span><span class="sxs-lookup"><span data-stu-id="025b5-154">Consumption Plan</span></span>| <span data-ttu-id="025b5-155">hello typ av planera din app använder för funktionen.</span><span class="sxs-lookup"><span data-stu-id="025b5-155">hello type of plan your function app uses.</span></span> <span data-ttu-id="025b5-156">Alternativen är förbrukning eller Azure App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="025b5-156">Options are Consumption or Azure App Service plan.</span></span> |
    |<span data-ttu-id="025b5-157">**Plats**</span><span class="sxs-lookup"><span data-stu-id="025b5-157">**Location**</span></span>|<span data-ttu-id="025b5-158">Centrala USA</span><span class="sxs-lookup"><span data-stu-id="025b5-158">Central US</span></span>| <span data-ttu-id="025b5-159">hello region i vilken toocreate hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="025b5-159">hello region in which toocreate hello function app.</span></span>|
    |<span data-ttu-id="025b5-160">**Storage-konto**</span><span class="sxs-lookup"><span data-stu-id="025b5-160">**Storage Account**</span></span>|<span data-ttu-id="025b5-161">{namn} automatiskt</span><span class="sxs-lookup"><span data-stu-id="025b5-161">{autogenerated}</span></span>| <span data-ttu-id="025b5-162">Hej lagringskonto som Azure Functions måste för allmänna lagring.</span><span class="sxs-lookup"><span data-stu-id="025b5-162">hello storage account that Azure Functions needs for general-purpose storage.</span></span>|

3. <span data-ttu-id="025b5-163">På hello **PacketCaptureExample funktionen appar** bladet väljer **funktioner** > **anpassad funktionen**  >  **+**.</span><span class="sxs-lookup"><span data-stu-id="025b5-163">On hello **PacketCaptureExample Function Apps** blade, select **Functions** > **Custom function** >**+**.</span></span>

4. <span data-ttu-id="025b5-164">Välj **HttpTrigger Powershell**, och ange sedan hello återstående information.</span><span class="sxs-lookup"><span data-stu-id="025b5-164">Select **HttpTrigger-Powershell**, and then enter hello remaining information.</span></span> <span data-ttu-id="025b5-165">Slutligen toocreate hello funktion, Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="025b5-165">Finally, toocreate hello function, select **Create**.</span></span>

    |<span data-ttu-id="025b5-166">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="025b5-166">**Setting**</span></span> | <span data-ttu-id="025b5-167">**Värde**</span><span class="sxs-lookup"><span data-stu-id="025b5-167">**Value**</span></span> | <span data-ttu-id="025b5-168">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="025b5-168">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="025b5-169">**Scenario**</span><span class="sxs-lookup"><span data-stu-id="025b5-169">**Scenario**</span></span>|<span data-ttu-id="025b5-170">experiment</span><span class="sxs-lookup"><span data-stu-id="025b5-170">Experimental</span></span>|<span data-ttu-id="025b5-171">Typen av scenario</span><span class="sxs-lookup"><span data-stu-id="025b5-171">Type of scenario</span></span>|
    |<span data-ttu-id="025b5-172">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="025b5-172">**Name your function**</span></span>|<span data-ttu-id="025b5-173">AlertPacketCapturePowerShell</span><span class="sxs-lookup"><span data-stu-id="025b5-173">AlertPacketCapturePowerShell</span></span>|<span data-ttu-id="025b5-174">Namnet på hello-funktion</span><span class="sxs-lookup"><span data-stu-id="025b5-174">Name of hello function</span></span>|
    |<span data-ttu-id="025b5-175">**Åtkomstnivå**</span><span class="sxs-lookup"><span data-stu-id="025b5-175">**Authorization level**</span></span>|<span data-ttu-id="025b5-176">Funktionen</span><span class="sxs-lookup"><span data-stu-id="025b5-176">Function</span></span>|<span data-ttu-id="025b5-177">Åtkomstnivå för hello-funktion</span><span class="sxs-lookup"><span data-stu-id="025b5-177">Authorization level for hello function</span></span>|

![Exempel på funktioner][functions1]

> [!NOTE]
> <span data-ttu-id="025b5-179">hello PowerShell mallen är experiment och har inte fullständigt stöd.</span><span class="sxs-lookup"><span data-stu-id="025b5-179">hello PowerShell template is experimental and does not have full support.</span></span>

<span data-ttu-id="025b5-180">Anpassningar som krävs för det här exemplet och beskrivs i följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="025b5-180">Customizations are required for this example and are explained in hello following steps.</span></span>

### <a name="add-modules"></a><span data-ttu-id="025b5-181">Lägg till moduler</span><span class="sxs-lookup"><span data-stu-id="025b5-181">Add modules</span></span>

<span data-ttu-id="025b5-182">toouse nätverk Watcher PowerShell-cmdletarna överför hello senaste PowerShell-modulen toohello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="025b5-182">toouse Network Watcher PowerShell cmdlets, upload hello latest PowerShell module toohello function app.</span></span>

1. <span data-ttu-id="025b5-183">Kör följande PowerShell-kommando hello på den lokala datorn med hello senaste Azure PowerShell-moduler installeras:</span><span class="sxs-lookup"><span data-stu-id="025b5-183">On your local machine with hello latest Azure PowerShell modules installed, run hello following PowerShell command:</span></span>

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    <span data-ttu-id="025b5-184">Det här exemplet ger du hello lokala sökvägen för dina Azure PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="025b5-184">This example gives you hello local path of your Azure PowerShell modules.</span></span> <span data-ttu-id="025b5-185">Dessa mappar som används i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="025b5-185">These folders are used in a later step.</span></span> <span data-ttu-id="025b5-186">hello-moduler som används i det här scenariot är:</span><span class="sxs-lookup"><span data-stu-id="025b5-186">hello modules that are used in this scenario are:</span></span>

    * <span data-ttu-id="025b5-187">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="025b5-187">AzureRM.Network</span></span>

    * <span data-ttu-id="025b5-188">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="025b5-188">AzureRM.Profile</span></span>

    * <span data-ttu-id="025b5-189">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="025b5-189">AzureRM.Resources</span></span>

    ![PowerShell-mappar][functions5]

1. <span data-ttu-id="025b5-191">Välj **fungerar appinställningar** > **gå tooApp Service Editor**.</span><span class="sxs-lookup"><span data-stu-id="025b5-191">Select **Function app settings** > **Go tooApp Service Editor**.</span></span>

    ![Funktionen app-inställningar][functions2]

1. <span data-ttu-id="025b5-193">Högerklicka på hello **AlertPacketCapturePowershell** mapp, och sedan skapa en mapp med namnet **azuremodules**.</span><span class="sxs-lookup"><span data-stu-id="025b5-193">Right-click hello **AlertPacketCapturePowershell** folder, and then create a folder called **azuremodules**.</span></span> 

4. <span data-ttu-id="025b5-194">Skapa en undermapp för varje modul som du behöver.</span><span class="sxs-lookup"><span data-stu-id="025b5-194">Create a subfolder for each module that you need.</span></span>

    ![Mappen och undermappar][functions3]

    * <span data-ttu-id="025b5-196">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="025b5-196">AzureRM.Network</span></span>

    * <span data-ttu-id="025b5-197">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="025b5-197">AzureRM.Profile</span></span>

    * <span data-ttu-id="025b5-198">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="025b5-198">AzureRM.Resources</span></span>

1. <span data-ttu-id="025b5-199">Högerklicka på hello **AzureRM.Network** undermapp och välj sedan **Överför filer**.</span><span class="sxs-lookup"><span data-stu-id="025b5-199">Right-click hello **AzureRM.Network** subfolder, and then select **Upload Files**.</span></span> 

6. <span data-ttu-id="025b5-200">Gå tooyour Azure moduler.</span><span class="sxs-lookup"><span data-stu-id="025b5-200">Go tooyour Azure modules.</span></span> <span data-ttu-id="025b5-201">I hello lokala **AzureRM.Network** mapp, markera alla hello-filer i mappen hello.</span><span class="sxs-lookup"><span data-stu-id="025b5-201">In hello local **AzureRM.Network** folder, select all hello files in hello folder.</span></span> <span data-ttu-id="025b5-202">Välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="025b5-202">Then select **OK**.</span></span> 

7. <span data-ttu-id="025b5-203">Upprepa dessa steg för **AzureRM.Profile** och **AzureRM.Resources**.</span><span class="sxs-lookup"><span data-stu-id="025b5-203">Repeat these steps for **AzureRM.Profile** and **AzureRM.Resources**.</span></span>

    ![Överföra filer][functions6]

1. <span data-ttu-id="025b5-205">När du är klar, var mappen bör ha hello PowerShell-modulen filer från din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="025b5-205">After you've finished, each folder should have hello PowerShell module files from your local machine.</span></span>

    ![PowerShell-filer][functions7]

### <a name="authentication"></a><span data-ttu-id="025b5-207">Autentisering</span><span class="sxs-lookup"><span data-stu-id="025b5-207">Authentication</span></span>

<span data-ttu-id="025b5-208">toouse hello PowerShell-cmdletar som du måste autentisera.</span><span class="sxs-lookup"><span data-stu-id="025b5-208">toouse hello PowerShell cmdlets, you must authenticate.</span></span> <span data-ttu-id="025b5-209">Du kan konfigurera autentisering i hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="025b5-209">You configure authentication in hello function app.</span></span> <span data-ttu-id="025b5-210">tooconfigure autentisering måste du konfigurera miljövariabler och överföra en krypterad nyckelfilen toohello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="025b5-210">tooconfigure authentication, you must configure environment variables and upload an encrypted key file toohello function app.</span></span>

> [!NOTE]
> <span data-ttu-id="025b5-211">Det här scenariot innehåller bara ett exempel på hur tooimplement autentisering med Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="025b5-211">This scenario provides just one example of how tooimplement authentication with Azure Functions.</span></span> <span data-ttu-id="025b5-212">Det finns andra sätt toodo detta.</span><span class="sxs-lookup"><span data-stu-id="025b5-212">There are other ways toodo this.</span></span>

#### <a name="encrypted-credentials"></a><span data-ttu-id="025b5-213">Krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="025b5-213">Encrypted credentials</span></span>

<span data-ttu-id="025b5-214">hello följande PowerShell-skript skapar en nyckelfil som kallas **PassEncryptKey.key**.</span><span class="sxs-lookup"><span data-stu-id="025b5-214">hello following PowerShell script creates a key file called **PassEncryptKey.key**.</span></span> <span data-ttu-id="025b5-215">Det ger också en krypterad version av hello lösenord som har angetts.</span><span class="sxs-lookup"><span data-stu-id="025b5-215">It also provides an encrypted version of hello password that's supplied.</span></span> <span data-ttu-id="025b5-216">Lösenordet är hello samma lösenord som har definierats för hello Azure Active Directory-program som används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="025b5-216">This password is hello same password that is defined for hello Azure Active Directory application that's used for authentication.</span></span>

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

<span data-ttu-id="025b5-217">Skapa en mapp med namnet i hello App Service redigeringsprogram hello funktionsapp **nycklar** under **AlertPacketCapturePowerShell**.</span><span class="sxs-lookup"><span data-stu-id="025b5-217">In hello App Service Editor of hello function app, create a folder called **keys** under **AlertPacketCapturePowerShell**.</span></span> <span data-ttu-id="025b5-218">Överför hello **PassEncryptKey.key** -fil som du skapade i hello tidigare PowerShell-exempel.</span><span class="sxs-lookup"><span data-stu-id="025b5-218">Then upload hello **PassEncryptKey.key** file that you created in hello previous PowerShell sample.</span></span>

![Funktioner nyckel][functions8]

### <a name="retrieve-values-for-environment-variables"></a><span data-ttu-id="025b5-220">Hämta värden för miljövariabler</span><span class="sxs-lookup"><span data-stu-id="025b5-220">Retrieve values for environment variables</span></span>

<span data-ttu-id="025b5-221">hello slutliga kravet är tooset in hello miljövariabler som är nödvändiga tooaccess hello värden för autentisering.</span><span class="sxs-lookup"><span data-stu-id="025b5-221">hello final requirement is tooset up hello environment variables that are necessary tooaccess hello values for authentication.</span></span> <span data-ttu-id="025b5-222">hello visar följande lista hello miljövariabler som har skapats:</span><span class="sxs-lookup"><span data-stu-id="025b5-222">hello following list shows hello environment variables that are created:</span></span>

* <span data-ttu-id="025b5-223">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="025b5-223">AzureClientID</span></span>

* <span data-ttu-id="025b5-224">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="025b5-224">AzureTenant</span></span>

* <span data-ttu-id="025b5-225">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="025b5-225">AzureCredPassword</span></span>


#### <a name="azureclientid"></a><span data-ttu-id="025b5-226">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="025b5-226">AzureClientID</span></span>

<span data-ttu-id="025b5-227">hello klient-ID är hello program-ID för ett program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="025b5-227">hello client ID is hello Application ID of an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="025b5-228">Om du inte redan har ett program toouse, kör du följande exempel toocreate hello ett program.</span><span class="sxs-lookup"><span data-stu-id="025b5-228">If you don't already have an application toouse, run hello following example toocreate an application.</span></span>

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > <span data-ttu-id="025b5-229">hello-lösenord som du använder när du skapar hello program ska vara hello samma lösenord som du skapade tidigare när du sparar hello nyckelfilen.</span><span class="sxs-lookup"><span data-stu-id="025b5-229">hello password that you use when creating hello application should be hello same password that you created earlier when saving hello key file.</span></span>

1. <span data-ttu-id="025b5-230">Välj i hello Azure-portalen, **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="025b5-230">In hello Azure portal, select **Subscriptions**.</span></span> <span data-ttu-id="025b5-231">Välj hello prenumeration toouse och välj sedan **åtkomstkontroll (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="025b5-231">Select hello subscription toouse, and then select **Access control (IAM)**.</span></span>

    ![Funktioner IAM][functions9]

1. <span data-ttu-id="025b5-233">Välj hello konto toouse och välj sedan **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="025b5-233">Choose hello account toouse, and then select **Properties**.</span></span> <span data-ttu-id="025b5-234">Kopiera hello program-ID.</span><span class="sxs-lookup"><span data-stu-id="025b5-234">Copy hello Application ID.</span></span>

    ![Funktioner program-ID][functions10]

#### <a name="azuretenant"></a><span data-ttu-id="025b5-236">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="025b5-236">AzureTenant</span></span>

<span data-ttu-id="025b5-237">Hämta hello klient-ID genom att köra följande PowerShell-exempel hello:</span><span class="sxs-lookup"><span data-stu-id="025b5-237">Obtain hello tenant ID  by running hello following PowerShell sample:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a><span data-ttu-id="025b5-238">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="025b5-238">AzureCredPassword</span></span>

<span data-ttu-id="025b5-239">hello-värdet för hello AzureCredPassword miljövariabeln är hello-värde som du får från att köra följande PowerShell-exempel hello.</span><span class="sxs-lookup"><span data-stu-id="025b5-239">hello value of hello AzureCredPassword environment variable is hello value that you get from running hello following PowerShell sample.</span></span> <span data-ttu-id="025b5-240">Det här exemplet är hello samma som visas i föregående hello **krypterade autentiseringsuppgifter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="025b5-240">This example is hello same one that's shown in hello preceding **Encrypted credentials** section.</span></span> <span data-ttu-id="025b5-241">hello-värde som behövs är hello utdata från hello `$Encryptedpassword` variabeln.</span><span class="sxs-lookup"><span data-stu-id="025b5-241">hello value that's needed is hello output of hello `$Encryptedpassword` variable.</span></span>  <span data-ttu-id="025b5-242">Detta är hello service principal lösenord som du har krypterats med hjälp av hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="025b5-242">This is hello service principal password that you encrypted by using hello PowerShell script.</span></span>

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-hello-environment-variables"></a><span data-ttu-id="025b5-243">Lagra hello miljövariabler</span><span class="sxs-lookup"><span data-stu-id="025b5-243">Store hello environment variables</span></span>

1. <span data-ttu-id="025b5-244">Gå toohello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="025b5-244">Go toohello function app.</span></span> <span data-ttu-id="025b5-245">Välj sedan **fungerar appinställningar** > **konfigurera appinställningar**.</span><span class="sxs-lookup"><span data-stu-id="025b5-245">Then select **Function app settings** > **Configure app settings**.</span></span>

    ![Konfigurera appinställningar][functions11]

1. <span data-ttu-id="025b5-247">Lägg till hello miljövariabler och deras värden toohello app-inställningar och välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="025b5-247">Add hello environment variables and their values toohello app settings, and then select **Save**.</span></span>

    ![App-inställningar][functions12]

### <a name="add-powershell-toohello-function"></a><span data-ttu-id="025b5-249">Lägg till PowerShell toohello funktion</span><span class="sxs-lookup"><span data-stu-id="025b5-249">Add PowerShell toohello function</span></span>

<span data-ttu-id="025b5-250">Det är nu tid toomake anrop till Nätverksbevakaren från inom hello Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="025b5-250">It's now time toomake calls into Network Watcher from within hello Azure function.</span></span> <span data-ttu-id="025b5-251">Hello implementering av den här funktionen kan variera beroende på hello krav.</span><span class="sxs-lookup"><span data-stu-id="025b5-251">Depending on hello requirements, hello implementation of this function can vary.</span></span> <span data-ttu-id="025b5-252">Dock är hello allmänna flödet av hello koden följande:</span><span class="sxs-lookup"><span data-stu-id="025b5-252">However, hello general flow of hello code is as follows:</span></span>

1. <span data-ttu-id="025b5-253">Processen indataparametrar.</span><span class="sxs-lookup"><span data-stu-id="025b5-253">Process input parameters.</span></span>
2. <span data-ttu-id="025b5-254">Frågan befintliga paket avbildas tooverify gränser och lösa namnkonflikter.</span><span class="sxs-lookup"><span data-stu-id="025b5-254">Query existing packet captures tooverify limits and resolve name conflicts.</span></span>
3. <span data-ttu-id="025b5-255">Skapa en paketinsamling med lämpliga parametrar.</span><span class="sxs-lookup"><span data-stu-id="025b5-255">Create a packet capture with appropriate parameters.</span></span>
4. <span data-ttu-id="025b5-256">Avsökningen paket avbilda regelbundet tills den är klar.</span><span class="sxs-lookup"><span data-stu-id="025b5-256">Poll packet capture periodically until it's complete.</span></span>
5. <span data-ttu-id="025b5-257">Meddela användaren hello att hello paket avbildningssessionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="025b5-257">Notify hello user that hello packet capture session is complete.</span></span>

<span data-ttu-id="025b5-258">hello är följande exempel PowerShell-kod som kan användas i hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="025b5-258">hello following example is PowerShell code that can be used in hello function.</span></span> <span data-ttu-id="025b5-259">Det finns värden som behöver ersättas för toobe **subscriptionId**, **resourceGroupName**, och **storageAccountName**.</span><span class="sxs-lookup"><span data-stu-id="025b5-259">There are values that need toobe replaced for **subscriptionId**, **resourceGroupName**, and **storageAccountName**.</span></span>

```powershell
            #Import Azure PowerShell modules required toomake calls tooNetwork Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID toosave captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Add-AzureRMAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get hello VM that fired hello alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get hello Network Watcher in hello VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by hello function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on hello VM that fired hello alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-hello-function-url"></a><span data-ttu-id="025b5-260">Hämta hello funktions-URL</span><span class="sxs-lookup"><span data-stu-id="025b5-260">Retrieve hello function URL</span></span> 
1. <span data-ttu-id="025b5-261">När du har skapat din funktion, konfigurera aviseringen toocall hello URL: en som är kopplad till hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="025b5-261">After you've created your function, configure your alert toocall hello URL that's associated with hello function.</span></span> <span data-ttu-id="025b5-262">tooget detta värde, kopiera hello funktions-URL från din funktion.</span><span class="sxs-lookup"><span data-stu-id="025b5-262">tooget this value, copy hello function URL from your function app.</span></span>

    ![Hitta hello funktions-URL][functions13]

2. <span data-ttu-id="025b5-264">Kopiera hello funktions-URL för din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="025b5-264">Copy hello function URL for your function app.</span></span>

    ![Kopiera hello funktions-URL][2]

<span data-ttu-id="025b5-266">Om du vill använda anpassade egenskaper i hello nyttolast hello webhook POST-begäran, se för[konfigurera en webhook på en Azure mått avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="025b5-266">If you require custom properties in hello payload of hello webhook POST request, refer too[Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="configure-an-alert-on-a-vm"></a><span data-ttu-id="025b5-267">Konfigurera en avisering på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="025b5-267">Configure an alert on a VM</span></span>

<span data-ttu-id="025b5-268">Aviseringar kan vara konfigurerade toonotify enskilda användare när ett specifikt mått överskrider ett tröskelvärde som är tilldelad tooit.</span><span class="sxs-lookup"><span data-stu-id="025b5-268">Alerts can be configured toonotify individuals when a specific metric crosses a threshold that's assigned tooit.</span></span> <span data-ttu-id="025b5-269">I det här exemplet hello aviseringen är på hello TCP-segment som skickas, men hello avisering kan aktiveras för många andra mått.</span><span class="sxs-lookup"><span data-stu-id="025b5-269">In this example, hello alert is on hello TCP segments that are sent, but hello alert can be triggered for many other metrics.</span></span> <span data-ttu-id="025b5-270">I det här exemplet är en avisering konfigurerade toocall en webhook toocall hello-funktion.</span><span class="sxs-lookup"><span data-stu-id="025b5-270">In this example, an alert is configured toocall a webhook toocall hello function.</span></span>

### <a name="create-hello-alert-rule"></a><span data-ttu-id="025b5-271">Skapa hello varningsregel</span><span class="sxs-lookup"><span data-stu-id="025b5-271">Create hello alert rule</span></span>

<span data-ttu-id="025b5-272">Gå tooan befintlig virtuell dator och sedan lägga till en varningsregel.</span><span class="sxs-lookup"><span data-stu-id="025b5-272">Go tooan existing virtual machine, and then add an alert rule.</span></span> <span data-ttu-id="025b5-273">Mer detaljerad dokumentation om hur du konfigurerar aviseringar finns på [skapa aviseringar i Azure-Monitor för Azure-tjänster - Azure-portalen](../monitoring-and-diagnostics/insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="025b5-273">More detailed documentation about configuring alerts can be found at [Create alerts in Azure Monitor for Azure services - Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md).</span></span> <span data-ttu-id="025b5-274">Ange följande värden i hello hello **varningsregeln** bladet och väljer sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="025b5-274">Enter hello following values in hello **Alert rule** blade, and then select **OK**.</span></span>

  |<span data-ttu-id="025b5-275">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="025b5-275">**Setting**</span></span> | <span data-ttu-id="025b5-276">**Värde**</span><span class="sxs-lookup"><span data-stu-id="025b5-276">**Value**</span></span> | <span data-ttu-id="025b5-277">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="025b5-277">**Details**</span></span> |
  |---|---|---|
  |<span data-ttu-id="025b5-278">**Namn**</span><span class="sxs-lookup"><span data-stu-id="025b5-278">**Name**</span></span>|<span data-ttu-id="025b5-279">TCP_Segments_Sent_Exceeded</span><span class="sxs-lookup"><span data-stu-id="025b5-279">TCP_Segments_Sent_Exceeded</span></span>|<span data-ttu-id="025b5-280">Namnet på hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="025b5-280">Name of hello alert rule.</span></span>|
  |<span data-ttu-id="025b5-281">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="025b5-281">**Description**</span></span>|<span data-ttu-id="025b5-282">TCP-segment skickas överskred tröskeln</span><span class="sxs-lookup"><span data-stu-id="025b5-282">TCP segments sent exceeded threshold</span></span>|<span data-ttu-id="025b5-283">hello beskrivning hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="025b5-283">hello description for hello alert rule.</span></span>||
  |<span data-ttu-id="025b5-284">**Mått**</span><span class="sxs-lookup"><span data-stu-id="025b5-284">**Metric**</span></span>|<span data-ttu-id="025b5-285">TCP-segment som skickats</span><span class="sxs-lookup"><span data-stu-id="025b5-285">TCP segments sent</span></span>| <span data-ttu-id="025b5-286">hello mått toouse tootrigger hello avisering.</span><span class="sxs-lookup"><span data-stu-id="025b5-286">hello metric toouse tootrigger hello alert.</span></span> |
  |<span data-ttu-id="025b5-287">**Villkor**</span><span class="sxs-lookup"><span data-stu-id="025b5-287">**Condition**</span></span>|<span data-ttu-id="025b5-288">Större än</span><span class="sxs-lookup"><span data-stu-id="025b5-288">Greater than</span></span>| <span data-ttu-id="025b5-289">hello villkoret toouse vid utvärdering av hello mått.</span><span class="sxs-lookup"><span data-stu-id="025b5-289">hello condition toouse when evaluating hello metric.</span></span>|
  |<span data-ttu-id="025b5-290">**Tröskelvärde**</span><span class="sxs-lookup"><span data-stu-id="025b5-290">**Threshold**</span></span>|<span data-ttu-id="025b5-291">100</span><span class="sxs-lookup"><span data-stu-id="025b5-291">100</span></span>| <span data-ttu-id="025b5-292">hello-värdet för hello mått som utlöser hello varning.</span><span class="sxs-lookup"><span data-stu-id="025b5-292">hello  value of hello metric that triggers hello alert.</span></span> <span data-ttu-id="025b5-293">Det här värdet ska anges tooa giltigt värde för din miljö.</span><span class="sxs-lookup"><span data-stu-id="025b5-293">This value should be set tooa valid value for your environment.</span></span>|
  |<span data-ttu-id="025b5-294">**Period**</span><span class="sxs-lookup"><span data-stu-id="025b5-294">**Period**</span></span>|<span data-ttu-id="025b5-295">Över hello senaste fem minuterna</span><span class="sxs-lookup"><span data-stu-id="025b5-295">Over hello last five minutes</span></span>| <span data-ttu-id="025b5-296">Anger hello period i vilka toolook för hello tröskelvärdet för hello mått.</span><span class="sxs-lookup"><span data-stu-id="025b5-296">Determines hello period in which toolook for hello threshold on hello metric.</span></span>|
  |<span data-ttu-id="025b5-297">**Webhook**</span><span class="sxs-lookup"><span data-stu-id="025b5-297">**Webhook**</span></span>|<span data-ttu-id="025b5-298">[Webhooksadressen från funktionsapp]</span><span class="sxs-lookup"><span data-stu-id="025b5-298">[webhook URL from function app]</span></span>| <span data-ttu-id="025b5-299">Hej Webhooksadressen från hello funktionsapp som skapades i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="025b5-299">hello webhook URL from hello function app that was created in hello previous steps.</span></span>|

> [!NOTE]
> <span data-ttu-id="025b5-300">hello TCP-segment måttet är inte aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="025b5-300">hello TCP segments metric is not enabled by default.</span></span> <span data-ttu-id="025b5-301">Läs mer om hur tooenable ytterligare mått genom att besöka [aktivera övervakning och diagnostik](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="025b5-301">Learn more about how tooenable additional metrics by visiting [Enable monitoring and diagnostics](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span></span>

## <a name="review-hello-results"></a><span data-ttu-id="025b5-302">Granska resultatet från hello</span><span class="sxs-lookup"><span data-stu-id="025b5-302">Review hello results</span></span>

<span data-ttu-id="025b5-303">Efter hello kriterier för hello avisering utlösare skapas en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="025b5-303">After hello criteria for hello alert triggers, a packet capture is created.</span></span> <span data-ttu-id="025b5-304">Gå tooNetwork Watcher och välj sedan **paketinsamling**.</span><span class="sxs-lookup"><span data-stu-id="025b5-304">Go tooNetwork Watcher, and then select **Packet capture**.</span></span> <span data-ttu-id="025b5-305">Du kan välja hello paket avbilda filen länken toodownload hello paketinsamling på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="025b5-305">On this page, you can select hello packet capture file link toodownload hello packet capture.</span></span>

![Visa paketinsamling][functions14]

<span data-ttu-id="025b5-307">Om filen hello lagras lokalt, kan du hämta det genom att logga in toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="025b5-307">If hello capture file is stored locally, you can retrieve it by signing in toohello virtual machine.</span></span>

<span data-ttu-id="025b5-308">Anvisningar om att hämta filer från Azure storage-konton finns [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="025b5-308">For instructions about downloading files from Azure storage accounts, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="025b5-309">Ett annat verktyg som du kan använda är [Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="025b5-309">Another tool you can use is [Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="025b5-310">När din avbildning har hämtats, du kan visa den med ett verktyg som kan läsa en **CAP** fil.</span><span class="sxs-lookup"><span data-stu-id="025b5-310">After your capture has been downloaded, you can view it by using any tool that can read a **.cap** file.</span></span> <span data-ttu-id="025b5-311">Följande är länkar tootwo av dessa verktyg:</span><span class="sxs-lookup"><span data-stu-id="025b5-311">Following are links tootwo of these tools:</span></span>

- [<span data-ttu-id="025b5-312">Microsoft Message Analyzer</span><span class="sxs-lookup"><span data-stu-id="025b5-312">Microsoft Message Analyzer</span></span>](https://technet.microsoft.com/library/jj649776.aspx)
- [<span data-ttu-id="025b5-313">WireShark</span><span class="sxs-lookup"><span data-stu-id="025b5-313">WireShark</span></span>](https://www.wireshark.org/)

## <a name="next-steps"></a><span data-ttu-id="025b5-314">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="025b5-314">Next steps</span></span>

<span data-ttu-id="025b5-315">Lär dig hur tooview dina paket samlar in genom att besöka [paket avbilda analys med Wireshark](network-watcher-deep-packet-inspection.md).</span><span class="sxs-lookup"><span data-stu-id="025b5-315">Learn how tooview your packet captures by visiting [Packet capture analysis with Wireshark](network-watcher-deep-packet-inspection.md).</span></span>


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png
