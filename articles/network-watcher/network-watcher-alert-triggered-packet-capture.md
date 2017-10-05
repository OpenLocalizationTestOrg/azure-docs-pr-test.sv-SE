---
title: "Använd paketinsamling för att göra proaktiv nätverksövervakning med varningar och Azure Functions | Microsoft Docs"
description: "Den här artikeln beskriver hur du skapar en avisering utlösta paketinsamling med Azure Nätverksbevakaren"
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
ms.openlocfilehash: b813172fc1fc1cc683f463f05370c95bfec10f8d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a><span data-ttu-id="ccdc6-103">Använd paketinsamling för proaktiv nätverksövervakning med varningar och Azure Functions</span><span class="sxs-lookup"><span data-stu-id="ccdc6-103">Use packet capture for proactive network monitoring with alerts and Azure Functions</span></span>

<span data-ttu-id="ccdc6-104">Nätverket Watcher paketinsamling skapar avbilda sessioner för att spåra trafik till och från virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-104">Network Watcher packet capture creates capture sessions to track traffic in and out of virtual machines.</span></span> <span data-ttu-id="ccdc6-105">Avbilda filen kan ha ett filter som definieras för att spåra endast trafik som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-105">The capture file can have a filter that is defined to track only the traffic that you want to monitor.</span></span> <span data-ttu-id="ccdc6-106">Dessa data lagras sedan i en lagringsblob-eller lokalt på gästdatorn.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-106">This data is then stored in a storage blob or locally on the guest machine.</span></span>

<span data-ttu-id="ccdc6-107">Den här funktionen kan startas från en fjärrdator från andra automatiseringsscenarier, till exempel Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-107">This capability can be started remotely from other automation scenarios such as Azure Functions.</span></span> <span data-ttu-id="ccdc6-108">Paketinsamling ger dig möjlighet att köra proaktiv insamlingar baserat på definierad nätverket avvikelser.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-108">Packet capture gives you the capability to run proactive captures based on defined network anomalies.</span></span> <span data-ttu-id="ccdc6-109">Andra användningsområden omfattar att samla in nätverksstatistik för att hämta information om nätverket intrång och felsökning klient-/ serverkommunikation.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-109">Other uses include gathering network statistics, getting information about network intrusions, debugging client-server communications, and more.</span></span>

<span data-ttu-id="ccdc6-110">Resurser som distribueras i Azure kör 24/7.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-110">Resources that are deployed in Azure run 24/7.</span></span> <span data-ttu-id="ccdc6-111">Du och din personal kan inte aktivt övervaka status för alla resurser 24/7.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-111">You and your staff cannot actively monitor the status of all resources 24/7.</span></span> <span data-ttu-id="ccdc6-112">Till exempel vad händer om ett problem inträffar kl 2?</span><span class="sxs-lookup"><span data-stu-id="ccdc6-112">For example, what happens if an issue occurs at 2 AM?</span></span>

<span data-ttu-id="ccdc6-113">Genom att använda Nätverksbevakaren, aviseringar och funktioner från i Azure-ekosystemet, kan du proaktivt svara med data och verktyg för att lösa problem i nätverket.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-113">By using Network Watcher, alerting, and functions from within the Azure ecosystem, you can proactively respond with the data and tools to solve problems in your network.</span></span>

![Scenario][scenario]

## <a name="prerequisites"></a><span data-ttu-id="ccdc6-115">Krav</span><span class="sxs-lookup"><span data-stu-id="ccdc6-115">Prerequisites</span></span>

* <span data-ttu-id="ccdc6-116">Den senaste versionen av [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="ccdc6-116">The latest version of [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span></span>
* <span data-ttu-id="ccdc6-117">En befintlig instans av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-117">An existing instance of Network Watcher.</span></span> <span data-ttu-id="ccdc6-118">Om du inte redan har en, [skapa en instans av Nätverksbevakaren](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="ccdc6-118">If you don't already have one, [create an instance of Network Watcher](network-watcher-create.md).</span></span>
* <span data-ttu-id="ccdc6-119">En befintlig virtuell dator i samma region som Nätverksbevakaren med den [Windows tillägget](../virtual-machines/windows/extensions-nwa.md) eller [Linux-tillägg för virtuell dator](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="ccdc6-119">An existing virtual machine in the same region as Network Watcher with the [Windows extension](../virtual-machines/windows/extensions-nwa.md) or [Linux virtual machine extension](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="ccdc6-120">Scenario</span><span class="sxs-lookup"><span data-stu-id="ccdc6-120">Scenario</span></span>

<span data-ttu-id="ccdc6-121">Din virtuella dator skickar flera TCP-segment än vanligt i det här exemplet och du vill bli aviserad om.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-121">In this example, your VM is sending more TCP segments than usual, and you want to be alerted.</span></span> <span data-ttu-id="ccdc6-122">TCP-segment som används som exempel här, men du kan använda alla aviseringstillståndet.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-122">TCP segments are used as an example here, but you can use any alert condition.</span></span>

<span data-ttu-id="ccdc6-123">När du meddelas vill du ta emot paketnivå data för att förstå varför kommunikation har ökat.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-123">When you are alerted, you want to receive packet-level data to understand why communication has increased.</span></span> <span data-ttu-id="ccdc6-124">Du kan sedan vidta åtgärder för att återställa den virtuella datorn till vanlig kommunikation.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-124">Then you can take steps to return the virtual machine to regular communication.</span></span>

<span data-ttu-id="ccdc6-125">Det här scenariot förutsätter att du har en befintlig instans av Nätverksbevakaren och en resursgrupp med en giltig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-125">This scenario assumes that you have an existing instance of Network Watcher and a resource group with a valid virtual machine.</span></span>

<span data-ttu-id="ccdc6-126">I följande lista finns en översikt över arbetsflödet som äger rum:</span><span class="sxs-lookup"><span data-stu-id="ccdc6-126">The following list is an overview of the workflow that takes place:</span></span>

1. <span data-ttu-id="ccdc6-127">En avisering utlöses på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-127">An alert is triggered on your VM.</span></span>
1. <span data-ttu-id="ccdc6-128">Aviseringen anropar din Azure-funktion via en webhook.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-128">The alert calls your Azure function via a webhook.</span></span>
1. <span data-ttu-id="ccdc6-129">Din Azure-funktion bearbetar aviseringen och startar en Nätverksbevakaren paket avbildningssessionen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-129">Your Azure function processes the alert and starts a Network Watcher packet capture session.</span></span>
1. <span data-ttu-id="ccdc6-130">Paketinsamling körs på den virtuella datorn och samlar in trafik.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-130">The packet capture runs on the VM and collects traffic.</span></span>
1. <span data-ttu-id="ccdc6-131">Filen i paketet har överförts till ett lagringskonto för granskning och diagnos.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-131">The packet capture file is uploaded to a storage account for review and diagnosis.</span></span>

<span data-ttu-id="ccdc6-132">Om du vill automatisera processen vi skapa och ansluta en avisering på vår VM att utlösa när händelsen inträffar.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-132">To automate this process, we create and connect an alert on our VM to trigger when the incident occurs.</span></span> <span data-ttu-id="ccdc6-133">Vi kan också skapa en funktion för att anropa Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-133">We also create a function to call into Network Watcher.</span></span>

<span data-ttu-id="ccdc6-134">Det här scenariot gör följande:</span><span class="sxs-lookup"><span data-stu-id="ccdc6-134">This scenario does the following:</span></span>

* <span data-ttu-id="ccdc6-135">Skapar en Azure-funktion som startar en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-135">Creates an Azure function that starts a packet capture.</span></span>
* <span data-ttu-id="ccdc6-136">Skapar en aviseringsregel på en virtuell dator och konfigurerar varningsregel för att anropa funktionen Azure.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-136">Creates an alert rule on a virtual machine and configures the alert rule to call the Azure function.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="ccdc6-137">Skapa en Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="ccdc6-137">Create an Azure function</span></span>

<span data-ttu-id="ccdc6-138">Det första steget är att skapa en Azure-funktion för att bearbeta aviseringen och skapa en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-138">The first step is to create an Azure function to process the alert and create a packet capture.</span></span>

1. <span data-ttu-id="ccdc6-139">I den [Azure-portalen](https://portal.azure.com)väljer **ny** > **Compute** > **Funktionsapp**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-139">In the [Azure portal](https://portal.azure.com), select **New** > **Compute** > **Function App**.</span></span>

    ![Skapa en funktionsapp][1-1]

2. <span data-ttu-id="ccdc6-141">På den **Funktionsapp** bladet, ange följande värden och välj sedan **OK** att skapa appen:</span><span class="sxs-lookup"><span data-stu-id="ccdc6-141">On the **Function App** blade, enter the following values, and then select **OK** to create the app:</span></span>

    |<span data-ttu-id="ccdc6-142">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-142">**Setting**</span></span> | <span data-ttu-id="ccdc6-143">**Värde**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-143">**Value**</span></span> | <span data-ttu-id="ccdc6-144">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-144">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="ccdc6-145">**Appens namn**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-145">**App name**</span></span>|<span data-ttu-id="ccdc6-146">PacketCaptureExample</span><span class="sxs-lookup"><span data-stu-id="ccdc6-146">PacketCaptureExample</span></span>|<span data-ttu-id="ccdc6-147">Namnet på funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-147">The name of the function app.</span></span>|
    |<span data-ttu-id="ccdc6-148">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-148">**Subscription**</span></span>|<span data-ttu-id="ccdc6-149">[Din prenumeration] Prenumerationen för att skapa funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-149">[Your subscription]The subscription for which to create the function app.</span></span>||
    |<span data-ttu-id="ccdc6-150">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-150">**Resource Group**</span></span>|<span data-ttu-id="ccdc6-151">PacketCaptureRG</span><span class="sxs-lookup"><span data-stu-id="ccdc6-151">PacketCaptureRG</span></span>|<span data-ttu-id="ccdc6-152">Resursgruppen som innehåller funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-152">The resource group to contain the function app.</span></span>|
    |<span data-ttu-id="ccdc6-153">**Värd för planen**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-153">**Hosting Plan**</span></span>|<span data-ttu-id="ccdc6-154">Planera för användning</span><span class="sxs-lookup"><span data-stu-id="ccdc6-154">Consumption Plan</span></span>| <span data-ttu-id="ccdc6-155">Typ av planera din app använder för funktionen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-155">The type of plan your function app uses.</span></span> <span data-ttu-id="ccdc6-156">Alternativen är förbrukning eller Azure App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-156">Options are Consumption or Azure App Service plan.</span></span> |
    |<span data-ttu-id="ccdc6-157">**Plats**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-157">**Location**</span></span>|<span data-ttu-id="ccdc6-158">Centrala USA</span><span class="sxs-lookup"><span data-stu-id="ccdc6-158">Central US</span></span>| <span data-ttu-id="ccdc6-159">Den region där du skapar den funktionen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-159">The region in which to create the function app.</span></span>|
    |<span data-ttu-id="ccdc6-160">**Storage-konto**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-160">**Storage Account**</span></span>|<span data-ttu-id="ccdc6-161">{namn} automatiskt</span><span class="sxs-lookup"><span data-stu-id="ccdc6-161">{autogenerated}</span></span>| <span data-ttu-id="ccdc6-162">Lagringskontot som Azure Functions måste för allmänna lagring.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-162">The storage account that Azure Functions needs for general-purpose storage.</span></span>|

3. <span data-ttu-id="ccdc6-163">På den **PacketCaptureExample funktionen appar** bladet väljer **funktioner** > **anpassad funktionen**  >  **+**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-163">On the **PacketCaptureExample Function Apps** blade, select **Functions** > **Custom function** >**+**.</span></span>

4. <span data-ttu-id="ccdc6-164">Välj **HttpTrigger Powershell**, och ange sedan återstående information.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-164">Select **HttpTrigger-Powershell**, and then enter the remaining information.</span></span> <span data-ttu-id="ccdc6-165">Slutligen vill skapa funktionen väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-165">Finally, to create the function, select **Create**.</span></span>

    |<span data-ttu-id="ccdc6-166">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-166">**Setting**</span></span> | <span data-ttu-id="ccdc6-167">**Värde**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-167">**Value**</span></span> | <span data-ttu-id="ccdc6-168">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-168">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="ccdc6-169">**Scenario**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-169">**Scenario**</span></span>|<span data-ttu-id="ccdc6-170">experiment</span><span class="sxs-lookup"><span data-stu-id="ccdc6-170">Experimental</span></span>|<span data-ttu-id="ccdc6-171">Typen av scenario</span><span class="sxs-lookup"><span data-stu-id="ccdc6-171">Type of scenario</span></span>|
    |<span data-ttu-id="ccdc6-172">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-172">**Name your function**</span></span>|<span data-ttu-id="ccdc6-173">AlertPacketCapturePowerShell</span><span class="sxs-lookup"><span data-stu-id="ccdc6-173">AlertPacketCapturePowerShell</span></span>|<span data-ttu-id="ccdc6-174">Namnet på funktionen</span><span class="sxs-lookup"><span data-stu-id="ccdc6-174">Name of the function</span></span>|
    |<span data-ttu-id="ccdc6-175">**Åtkomstnivå**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-175">**Authorization level**</span></span>|<span data-ttu-id="ccdc6-176">Funktionen</span><span class="sxs-lookup"><span data-stu-id="ccdc6-176">Function</span></span>|<span data-ttu-id="ccdc6-177">Åtkomstnivå för funktionen</span><span class="sxs-lookup"><span data-stu-id="ccdc6-177">Authorization level for the function</span></span>|

![Exempel på funktioner][functions1]

> [!NOTE]
> <span data-ttu-id="ccdc6-179">PowerShell-mallen är experiment och har inte fullständigt stöd.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-179">The PowerShell template is experimental and does not have full support.</span></span>

<span data-ttu-id="ccdc6-180">Anpassningar som krävs för det här exemplet och beskrivs i följande steg.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-180">Customizations are required for this example and are explained in the following steps.</span></span>

### <a name="add-modules"></a><span data-ttu-id="ccdc6-181">Lägg till moduler</span><span class="sxs-lookup"><span data-stu-id="ccdc6-181">Add modules</span></span>

<span data-ttu-id="ccdc6-182">Överför den senaste PowerShell-modulen till appen med funktionen för att använda nätverket Watcher PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-182">To use Network Watcher PowerShell cmdlets, upload the latest PowerShell module to the function app.</span></span>

1. <span data-ttu-id="ccdc6-183">Kör följande PowerShell-kommando på den lokala datorn med Azure PowerShell-moduler som är installerad:</span><span class="sxs-lookup"><span data-stu-id="ccdc6-183">On your local machine with the latest Azure PowerShell modules installed, run the following PowerShell command:</span></span>

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    <span data-ttu-id="ccdc6-184">Det här exemplet får du den lokala sökvägen för dina Azure PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-184">This example gives you the local path of your Azure PowerShell modules.</span></span> <span data-ttu-id="ccdc6-185">Dessa mappar som används i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-185">These folders are used in a later step.</span></span> <span data-ttu-id="ccdc6-186">Moduler som används i det här scenariot är:</span><span class="sxs-lookup"><span data-stu-id="ccdc6-186">The modules that are used in this scenario are:</span></span>

    * <span data-ttu-id="ccdc6-187">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="ccdc6-187">AzureRM.Network</span></span>

    * <span data-ttu-id="ccdc6-188">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="ccdc6-188">AzureRM.Profile</span></span>

    * <span data-ttu-id="ccdc6-189">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="ccdc6-189">AzureRM.Resources</span></span>

    ![PowerShell-mappar][functions5]

1. <span data-ttu-id="ccdc6-191">Välj **fungerar appinställningar** > **gå till App Service Editor**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-191">Select **Function app settings** > **Go to App Service Editor**.</span></span>

    ![Funktionen app-inställningar][functions2]

1. <span data-ttu-id="ccdc6-193">Högerklicka på den **AlertPacketCapturePowershell** mapp, och sedan skapa en mapp med namnet **azuremodules**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-193">Right-click the **AlertPacketCapturePowershell** folder, and then create a folder called **azuremodules**.</span></span> 

4. <span data-ttu-id="ccdc6-194">Skapa en undermapp för varje modul som du behöver.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-194">Create a subfolder for each module that you need.</span></span>

    ![Mappen och undermappar][functions3]

    * <span data-ttu-id="ccdc6-196">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="ccdc6-196">AzureRM.Network</span></span>

    * <span data-ttu-id="ccdc6-197">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="ccdc6-197">AzureRM.Profile</span></span>

    * <span data-ttu-id="ccdc6-198">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="ccdc6-198">AzureRM.Resources</span></span>

1. <span data-ttu-id="ccdc6-199">Högerklicka på den **AzureRM.Network** undermapp och välj sedan **Överför filer**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-199">Right-click the **AzureRM.Network** subfolder, and then select **Upload Files**.</span></span> 

6. <span data-ttu-id="ccdc6-200">Gå till din Azure-moduler.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-200">Go to your Azure modules.</span></span> <span data-ttu-id="ccdc6-201">I lokalt **AzureRM.Network** mapp, markera alla filer i mappen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-201">In the local **AzureRM.Network** folder, select all the files in the folder.</span></span> <span data-ttu-id="ccdc6-202">Välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-202">Then select **OK**.</span></span> 

7. <span data-ttu-id="ccdc6-203">Upprepa dessa steg för **AzureRM.Profile** och **AzureRM.Resources**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-203">Repeat these steps for **AzureRM.Profile** and **AzureRM.Resources**.</span></span>

    ![Överföra filer][functions6]

1. <span data-ttu-id="ccdc6-205">När du är klar, var mappen bör ha PowerShell-modulen filer från din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-205">After you've finished, each folder should have the PowerShell module files from your local machine.</span></span>

    ![PowerShell-filer][functions7]

### <a name="authentication"></a><span data-ttu-id="ccdc6-207">Autentisering</span><span class="sxs-lookup"><span data-stu-id="ccdc6-207">Authentication</span></span>

<span data-ttu-id="ccdc6-208">Om du vill använda PowerShell-cmdlets, måste du autentisera.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-208">To use the PowerShell cmdlets, you must authenticate.</span></span> <span data-ttu-id="ccdc6-209">Du kan konfigurera autentisering i appen funktion.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-209">You configure authentication in the function app.</span></span> <span data-ttu-id="ccdc6-210">Om du vill konfigurera autentisering måste du konfigurera miljövariabler och överföra en krypterad nyckelfilen till appen med funktionen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-210">To configure authentication, you must configure environment variables and upload an encrypted key file to the function app.</span></span>

> [!NOTE]
> <span data-ttu-id="ccdc6-211">Det här scenariot ger bara ett exempel på hur du implementerar autentisering med Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-211">This scenario provides just one example of how to implement authentication with Azure Functions.</span></span> <span data-ttu-id="ccdc6-212">Det finns andra sätt att göra detta.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-212">There are other ways to do this.</span></span>

#### <a name="encrypted-credentials"></a><span data-ttu-id="ccdc6-213">Krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="ccdc6-213">Encrypted credentials</span></span>

<span data-ttu-id="ccdc6-214">Följande PowerShell-skript skapar en nyckelfil som kallas **PassEncryptKey.key**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-214">The following PowerShell script creates a key file called **PassEncryptKey.key**.</span></span> <span data-ttu-id="ccdc6-215">Det ger också en krypterad version av det lösenord som har angetts.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-215">It also provides an encrypted version of the password that's supplied.</span></span> <span data-ttu-id="ccdc6-216">Lösenordet är samma lösenord som har definierats för det Azure Active Directory-program som används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-216">This password is the same password that is defined for the Azure Active Directory application that's used for authentication.</span></span>

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

<span data-ttu-id="ccdc6-217">I App Service Redigeraren för funktionsapp, skapa en mapp med namnet **nycklar** under **AlertPacketCapturePowerShell**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-217">In the App Service Editor of the function app, create a folder called **keys** under **AlertPacketCapturePowerShell**.</span></span> <span data-ttu-id="ccdc6-218">Sedan ladda upp den **PassEncryptKey.key** -fil som du skapade i föregående exempel PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-218">Then upload the **PassEncryptKey.key** file that you created in the previous PowerShell sample.</span></span>

![Funktioner nyckel][functions8]

### <a name="retrieve-values-for-environment-variables"></a><span data-ttu-id="ccdc6-220">Hämta värden för miljövariabler</span><span class="sxs-lookup"><span data-stu-id="ccdc6-220">Retrieve values for environment variables</span></span>

<span data-ttu-id="ccdc6-221">Det slutliga kravet är att ställa in miljövariabler som är nödvändiga för att komma åt värden för autentisering.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-221">The final requirement is to set up the environment variables that are necessary to access the values for authentication.</span></span> <span data-ttu-id="ccdc6-222">I följande lista visas de miljövariabler som har skapats:</span><span class="sxs-lookup"><span data-stu-id="ccdc6-222">The following list shows the environment variables that are created:</span></span>

* <span data-ttu-id="ccdc6-223">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="ccdc6-223">AzureClientID</span></span>

* <span data-ttu-id="ccdc6-224">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="ccdc6-224">AzureTenant</span></span>

* <span data-ttu-id="ccdc6-225">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="ccdc6-225">AzureCredPassword</span></span>


#### <a name="azureclientid"></a><span data-ttu-id="ccdc6-226">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="ccdc6-226">AzureClientID</span></span>

<span data-ttu-id="ccdc6-227">Klient-ID är program-ID för ett program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-227">The client ID is the Application ID of an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="ccdc6-228">Om du inte redan har ett program att använda, kör du följande exempel för att skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-228">If you don't already have an application to use, run the following example to create an application.</span></span>

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > <span data-ttu-id="ccdc6-229">Det lösenord som du använder för att skapa programmet ska vara samma lösenord som du skapade tidigare när du sparar nyckelfilen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-229">The password that you use when creating the application should be the same password that you created earlier when saving the key file.</span></span>

1. <span data-ttu-id="ccdc6-230">Välj i Azure-portalen **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-230">In the Azure portal, select **Subscriptions**.</span></span> <span data-ttu-id="ccdc6-231">Välj prenumerationen du använder och välj sedan **åtkomstkontroll (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-231">Select the subscription to use, and then select **Access control (IAM)**.</span></span>

    ![Funktioner IAM][functions9]

1. <span data-ttu-id="ccdc6-233">Välj kontot som ska användas och välj sedan **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-233">Choose the account to use, and then select **Properties**.</span></span> <span data-ttu-id="ccdc6-234">Kopiera program-ID.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-234">Copy the Application ID.</span></span>

    ![Funktioner program-ID][functions10]

#### <a name="azuretenant"></a><span data-ttu-id="ccdc6-236">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="ccdc6-236">AzureTenant</span></span>

<span data-ttu-id="ccdc6-237">Hämta klient-ID genom att köra följande PowerShell-exempel:</span><span class="sxs-lookup"><span data-stu-id="ccdc6-237">Obtain the tenant ID  by running the following PowerShell sample:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a><span data-ttu-id="ccdc6-238">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="ccdc6-238">AzureCredPassword</span></span>

<span data-ttu-id="ccdc6-239">Värdet för miljövariabeln AzureCredPassword är det värde som du får från att köra följande PowerShell-exempel.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-239">The value of the AzureCredPassword environment variable is the value that you get from running the following PowerShell sample.</span></span> <span data-ttu-id="ccdc6-240">Det här exemplet är samma som visas i den föregående **krypterade autentiseringsuppgifter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-240">This example is the same one that's shown in the preceding **Encrypted credentials** section.</span></span> <span data-ttu-id="ccdc6-241">Värdet som behövs är resultatet av den `$Encryptedpassword` variabeln.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-241">The value that's needed is the output of the `$Encryptedpassword` variable.</span></span>  <span data-ttu-id="ccdc6-242">Det här är lösenordet för tjänstens huvudnamn som du har krypterats med hjälp av PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-242">This is the service principal password that you encrypted by using the PowerShell script.</span></span>

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

### <a name="store-the-environment-variables"></a><span data-ttu-id="ccdc6-243">Lagra miljövariablerna</span><span class="sxs-lookup"><span data-stu-id="ccdc6-243">Store the environment variables</span></span>

1. <span data-ttu-id="ccdc6-244">Gå till funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-244">Go to the function app.</span></span> <span data-ttu-id="ccdc6-245">Välj sedan **fungerar appinställningar** > **konfigurera appinställningar**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-245">Then select **Function app settings** > **Configure app settings**.</span></span>

    ![Konfigurera appinställningar][functions11]

1. <span data-ttu-id="ccdc6-247">Lägg till miljövariablerna och deras värden i app-inställningar och välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-247">Add the environment variables and their values to the app settings, and then select **Save**.</span></span>

    ![App-inställningar][functions12]

### <a name="add-powershell-to-the-function"></a><span data-ttu-id="ccdc6-249">Lägg till PowerShell till funktionen</span><span class="sxs-lookup"><span data-stu-id="ccdc6-249">Add PowerShell to the function</span></span>

<span data-ttu-id="ccdc6-250">Det är nu att ringa till Nätverksbevakaren från i Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-250">It's now time to make calls into Network Watcher from within the Azure function.</span></span> <span data-ttu-id="ccdc6-251">Implementeringen av den här funktionen kan variera beroende på krav.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-251">Depending on the requirements, the implementation of this function can vary.</span></span> <span data-ttu-id="ccdc6-252">Dock är det allmänna flödet av koden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ccdc6-252">However, the general flow of the code is as follows:</span></span>

1. <span data-ttu-id="ccdc6-253">Processen indataparametrar.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-253">Process input parameters.</span></span>
2. <span data-ttu-id="ccdc6-254">Frågan befintliga paket samlar in för att kontrollera gränser och lösa namnkonflikter.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-254">Query existing packet captures to verify limits and resolve name conflicts.</span></span>
3. <span data-ttu-id="ccdc6-255">Skapa en paketinsamling med lämpliga parametrar.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-255">Create a packet capture with appropriate parameters.</span></span>
4. <span data-ttu-id="ccdc6-256">Avsökningen paket avbilda regelbundet tills den är klar.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-256">Poll packet capture periodically until it's complete.</span></span>
5. <span data-ttu-id="ccdc6-257">Meddela användaren att hämtningens paket har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-257">Notify the user that the packet capture session is complete.</span></span>

<span data-ttu-id="ccdc6-258">I följande exempel är PowerShell-kod som kan användas i funktionen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-258">The following example is PowerShell code that can be used in the function.</span></span> <span data-ttu-id="ccdc6-259">Det finns värden som behöver ersättas för **subscriptionId**, **resourceGroupName**, och **storageAccountName**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-259">There are values that need to be replaced for **subscriptionId**, **resourceGroupName**, and **storageAccountName**.</span></span>

```powershell
            #Import Azure PowerShell modules required to make calls to Network Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID to save captures in
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


            #Get the VM that fired the alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get the Network Watcher in the VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by the function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on the VM that fired the alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-the-function-url"></a><span data-ttu-id="ccdc6-260">Hämta funktions-URL</span><span class="sxs-lookup"><span data-stu-id="ccdc6-260">Retrieve the function URL</span></span> 
1. <span data-ttu-id="ccdc6-261">När du har skapat din funktion kan du konfigurera aviseringen för att anropa den URL som är associerad med funktionen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-261">After you've created your function, configure your alert to call the URL that's associated with the function.</span></span> <span data-ttu-id="ccdc6-262">Kopiera URL som funktionen från din funktion för att få det här värdet.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-262">To get this value, copy the function URL from your function app.</span></span>

    ![Hitta funktions-URL][functions13]

2. <span data-ttu-id="ccdc6-264">Kopiera URL-Adressen för funktionen för din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-264">Copy the function URL for your function app.</span></span>

    ![Kopiera funktions-URL][2]

<span data-ttu-id="ccdc6-266">Om du behöver anpassade egenskaper i nyttolasten för POST-begäran webhook läsa [konfigurera en webhook på en Azure mått avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="ccdc6-266">If you require custom properties in the payload of the webhook POST request, refer to [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="configure-an-alert-on-a-vm"></a><span data-ttu-id="ccdc6-267">Konfigurera en avisering på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ccdc6-267">Configure an alert on a VM</span></span>

<span data-ttu-id="ccdc6-268">Aviseringar kan konfigureras för att meddela personer när ett specifikt mått överskrider ett tröskelvärde som är tilldelad.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-268">Alerts can be configured to notify individuals when a specific metric crosses a threshold that's assigned to it.</span></span> <span data-ttu-id="ccdc6-269">Aviseringen är TCP-segment som skickas i det här exemplet, men aviseringen kan aktiveras för många andra mått.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-269">In this example, the alert is on the TCP segments that are sent, but the alert can be triggered for many other metrics.</span></span> <span data-ttu-id="ccdc6-270">I det här exemplet konfigureras en avisering för att anropa en webhook för att anropa funktionen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-270">In this example, an alert is configured to call a webhook to call the function.</span></span>

### <a name="create-the-alert-rule"></a><span data-ttu-id="ccdc6-271">Skapa varningsregeln</span><span class="sxs-lookup"><span data-stu-id="ccdc6-271">Create the alert rule</span></span>

<span data-ttu-id="ccdc6-272">Gå till en befintlig virtuell dator och sedan lägga till en varningsregel.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-272">Go to an existing virtual machine, and then add an alert rule.</span></span> <span data-ttu-id="ccdc6-273">Mer detaljerad dokumentation om hur du konfigurerar aviseringar finns på [skapa aviseringar i Azure-Monitor för Azure-tjänster - Azure-portalen](../monitoring-and-diagnostics/insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ccdc6-273">More detailed documentation about configuring alerts can be found at [Create alerts in Azure Monitor for Azure services - Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md).</span></span> <span data-ttu-id="ccdc6-274">Ange följande värden i den **varningsregeln** bladet och väljer sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-274">Enter the following values in the **Alert rule** blade, and then select **OK**.</span></span>

  |<span data-ttu-id="ccdc6-275">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-275">**Setting**</span></span> | <span data-ttu-id="ccdc6-276">**Värde**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-276">**Value**</span></span> | <span data-ttu-id="ccdc6-277">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-277">**Details**</span></span> |
  |---|---|---|
  |<span data-ttu-id="ccdc6-278">**Namn**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-278">**Name**</span></span>|<span data-ttu-id="ccdc6-279">TCP_Segments_Sent_Exceeded</span><span class="sxs-lookup"><span data-stu-id="ccdc6-279">TCP_Segments_Sent_Exceeded</span></span>|<span data-ttu-id="ccdc6-280">Namnet på regeln.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-280">Name of the alert rule.</span></span>|
  |<span data-ttu-id="ccdc6-281">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-281">**Description**</span></span>|<span data-ttu-id="ccdc6-282">TCP-segment skickas överskred tröskeln</span><span class="sxs-lookup"><span data-stu-id="ccdc6-282">TCP segments sent exceeded threshold</span></span>|<span data-ttu-id="ccdc6-283">Beskrivning för regeln.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-283">The description for the alert rule.</span></span>||
  |<span data-ttu-id="ccdc6-284">**Mått**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-284">**Metric**</span></span>|<span data-ttu-id="ccdc6-285">TCP-segment som skickats</span><span class="sxs-lookup"><span data-stu-id="ccdc6-285">TCP segments sent</span></span>| <span data-ttu-id="ccdc6-286">Måttet som du använder för att utlösa aviseringen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-286">The metric to use to trigger the alert.</span></span> |
  |<span data-ttu-id="ccdc6-287">**Villkor**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-287">**Condition**</span></span>|<span data-ttu-id="ccdc6-288">Större än</span><span class="sxs-lookup"><span data-stu-id="ccdc6-288">Greater than</span></span>| <span data-ttu-id="ccdc6-289">Villkoret du vill använda vid utvärdering av måttet.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-289">The condition to use when evaluating the metric.</span></span>|
  |<span data-ttu-id="ccdc6-290">**Tröskelvärde**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-290">**Threshold**</span></span>|<span data-ttu-id="ccdc6-291">100</span><span class="sxs-lookup"><span data-stu-id="ccdc6-291">100</span></span>| <span data-ttu-id="ccdc6-292">Värdet för det mått som utlöser varningen.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-292">The  value of the metric that triggers the alert.</span></span> <span data-ttu-id="ccdc6-293">Det här värdet ska anges till ett giltigt värde för din miljö.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-293">This value should be set to a valid value for your environment.</span></span>|
  |<span data-ttu-id="ccdc6-294">**Period**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-294">**Period**</span></span>|<span data-ttu-id="ccdc6-295">Under de senaste fem minuterna</span><span class="sxs-lookup"><span data-stu-id="ccdc6-295">Over the last five minutes</span></span>| <span data-ttu-id="ccdc6-296">Anger den period som ska sökas efter tröskelvärdet för måttet.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-296">Determines the period in which to look for the threshold on the metric.</span></span>|
  |<span data-ttu-id="ccdc6-297">**Webhook**</span><span class="sxs-lookup"><span data-stu-id="ccdc6-297">**Webhook**</span></span>|<span data-ttu-id="ccdc6-298">[Webhooksadressen från funktionsapp]</span><span class="sxs-lookup"><span data-stu-id="ccdc6-298">[webhook URL from function app]</span></span>| <span data-ttu-id="ccdc6-299">Webhooksadressen från funktionsapp som skapades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-299">The webhook URL from the function app that was created in the previous steps.</span></span>|

> [!NOTE]
> <span data-ttu-id="ccdc6-300">Mått för TCP-segment är inte aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-300">The TCP segments metric is not enabled by default.</span></span> <span data-ttu-id="ccdc6-301">Mer information om hur du aktiverar fler mått genom att besöka [aktivera övervakning och diagnostik](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="ccdc6-301">Learn more about how to enable additional metrics by visiting [Enable monitoring and diagnostics](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span></span>

## <a name="review-the-results"></a><span data-ttu-id="ccdc6-302">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="ccdc6-302">Review the results</span></span>

<span data-ttu-id="ccdc6-303">Efter det att kriterierna för avisering utlösare skapas en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-303">After the criteria for the alert triggers, a packet capture is created.</span></span> <span data-ttu-id="ccdc6-304">Gå till Nätverksbevakaren och välj sedan **paketinsamling**.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-304">Go to Network Watcher, and then select **Packet capture**.</span></span> <span data-ttu-id="ccdc6-305">Du kan välja paket avbilda filen länk för att hämta paketinsamling på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-305">On this page, you can select the packet capture file link to download the packet capture.</span></span>

![Visa paketinsamling][functions14]

<span data-ttu-id="ccdc6-307">Om filen lagras lokalt kan du hämta det genom att logga in till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-307">If the capture file is stored locally, you can retrieve it by signing in to the virtual machine.</span></span>

<span data-ttu-id="ccdc6-308">Anvisningar om att hämta filer från Azure storage-konton finns [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="ccdc6-308">For instructions about downloading files from Azure storage accounts, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="ccdc6-309">Ett annat verktyg som du kan använda är [Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="ccdc6-309">Another tool you can use is [Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="ccdc6-310">När din avbildning har hämtats, du kan visa den med ett verktyg som kan läsa en **CAP** fil.</span><span class="sxs-lookup"><span data-stu-id="ccdc6-310">After your capture has been downloaded, you can view it by using any tool that can read a **.cap** file.</span></span> <span data-ttu-id="ccdc6-311">Följande är länkar till två av dessa verktyg:</span><span class="sxs-lookup"><span data-stu-id="ccdc6-311">Following are links to two of these tools:</span></span>

- [<span data-ttu-id="ccdc6-312">Microsoft Message Analyzer</span><span class="sxs-lookup"><span data-stu-id="ccdc6-312">Microsoft Message Analyzer</span></span>](https://technet.microsoft.com/library/jj649776.aspx)
- [<span data-ttu-id="ccdc6-313">WireShark</span><span class="sxs-lookup"><span data-stu-id="ccdc6-313">WireShark</span></span>](https://www.wireshark.org/)

## <a name="next-steps"></a><span data-ttu-id="ccdc6-314">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ccdc6-314">Next steps</span></span>

<span data-ttu-id="ccdc6-315">Lär dig hur du visar paket-insamlingar genom att besöka [paket avbilda analys med Wireshark](network-watcher-deep-packet-inspection.md).</span><span class="sxs-lookup"><span data-stu-id="ccdc6-315">Learn how to view your packet captures by visiting [Packet capture analysis with Wireshark](network-watcher-deep-packet-inspection.md).</span></span>


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
