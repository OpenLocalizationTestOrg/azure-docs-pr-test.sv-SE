---
title: aaaUse PowerShell-cmdlets med Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toouse Windows PowerShell-cmdlets i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 7d3d5ded-6f73-4de6-a8ac-c1d622e842a2
ms.service: remoteapp
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a09cae2093e2c3a4a2ed673b5d148a22ceb935f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a><span data-ttu-id="f96dd-103">Använda Windows PowerShell-cmdlets med Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="f96dd-103">Use Windows PowerShell cmdlets with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f96dd-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="f96dd-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="f96dd-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="f96dd-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

 <span data-ttu-id="f96dd-106">Du kan använda hello Azure RemoteApp PowerShell-cmdlets tooadminister och underhålla din samlingar.</span><span class="sxs-lookup"><span data-stu-id="f96dd-106">You can use hello Azure RemoteApp PowerShell cmdlets tooadminister and maintain your collections.</span></span> <span data-ttu-id="f96dd-107">Använd följande information tooget igång hello.</span><span class="sxs-lookup"><span data-stu-id="f96dd-107">Use hello following information tooget started.</span></span>

## <a name="get-hello-cmdlets"></a><span data-ttu-id="f96dd-108">Hämta hello-cmdlets</span><span class="sxs-lookup"><span data-stu-id="f96dd-108">Get hello cmdlets</span></span>
- - -
<span data-ttu-id="f96dd-109">Först hämta hello Azure Powershell-cmdlets [här](http://go.microsoft.com/?linkid=9811175), hello RemoteApp-cmdlets som ingår i den.</span><span class="sxs-lookup"><span data-stu-id="f96dd-109">First download hello Azure Powershell cmdlets [here](http://go.microsoft.com/?linkid=9811175), hello RemoteApp cmdlets are included in it.</span></span> 

<span data-ttu-id="f96dd-110">Kolla in hello [Azure RemoteApp-cmdlet-hjälpen](/powershell/module/azure?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="f96dd-110">Check out hello [Azure RemoteApp cmdlet help](/powershell/module/azure?view=azuresmps-3.7.0).</span></span>

## <a name="configure-azure-cmdlets-toouse-your-subscription"></a><span data-ttu-id="f96dd-111">Konfigurera Azure-cmdlets toouse din prenumeration</span><span class="sxs-lookup"><span data-stu-id="f96dd-111">Configure Azure cmdlets toouse your subscription</span></span>
- - -
<span data-ttu-id="f96dd-112">Följ [handboken](/powershell/azure/overview) så att du kan använda hello cmdlets mot dina Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f96dd-112">Follow [this guide](/powershell/azure/overview) so you can use hello cmdlets against your Azure subscription.</span></span>

<span data-ttu-id="f96dd-113">Du kan använda dessa steg tooget snabbt igång:</span><span class="sxs-lookup"><span data-stu-id="f96dd-113">You can use these steps tooget started quickly:</span></span>

1. <span data-ttu-id="f96dd-114">Hämta och installera hello [Azure PowerShell-cmdlets](http://go.microsoft.com/?linkid=9811175).</span><span class="sxs-lookup"><span data-stu-id="f96dd-114">Download and install hello [Azure PowerShell cmdlets](http://go.microsoft.com/?linkid=9811175).</span></span>
2. <span data-ttu-id="f96dd-115">Starta Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f96dd-115">Launch Microsoft Azure PowerShell.</span></span>
3. <span data-ttu-id="f96dd-116">Kör **Add-AzureAccount** tooauthenticate tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f96dd-116">Run **Add-AzureAccount** tooauthenticate tooyour Azure subscription.</span></span> <span data-ttu-id="f96dd-117">När du uppmanas, anger hello samma användarnamn och lösenord du använder toosign i tooAzure portal.</span><span class="sxs-lookup"><span data-stu-id="f96dd-117">When prompted, enter hello same user name and password that you use toosign in tooAzure portal.</span></span>  
4. <span data-ttu-id="f96dd-118">Kör **Get-AzureSubscription** toolist hello prenumerationer som är kopplade till ditt användarkonto.</span><span class="sxs-lookup"><span data-stu-id="f96dd-118">Run **Get-AzureSubscription** toolist hello subscriptions associated with your user account.</span></span> 
5. <span data-ttu-id="f96dd-119">Kör **Välj AzureSubscription - SubscriptionName &lt;prenumerationsnamn&gt;**  eller **Välj AzureSubscription - SubscriptionId &lt;prenumerations-ID&gt;**  toospecify hello prenumeration toouse.</span><span class="sxs-lookup"><span data-stu-id="f96dd-119">Run **Select-AzureSubscription -SubscriptionName &lt;subscription name&gt;** or **Select-AzureSubscription -SubscriptionId &lt;subscription ID&gt;** toospecify hello subscription toouse.</span></span>

<span data-ttu-id="f96dd-120">Grattis, Azure PowerShell-konsolen är konfigurerad och klar toouse.</span><span class="sxs-lookup"><span data-stu-id="f96dd-120">Congratulations, your Azure PowerShell console is configured and ready toouse.</span></span> <span data-ttu-id="f96dd-121">Tänk på att du behöver toorepeate steg 2 till 5 varje gång du startar hello hello Azure PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="f96dd-121">Be aware that you'll need toorepeate steps 2 through 5 each time you start hello hello Azure PowerShell console.</span></span>  


## <a name="list-all-collections"></a><span data-ttu-id="f96dd-122">Visa en lista med alla samlingar</span><span class="sxs-lookup"><span data-stu-id="f96dd-122">List all collections</span></span>
- - -
     <span data-ttu-id="f96dd-123">Get-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="f96dd-123">Get-AzureRemoteAppCollection</span></span>

## <a name="delete-a-collection"></a><span data-ttu-id="f96dd-124">Ta bort en samling</span><span class="sxs-lookup"><span data-stu-id="f96dd-124">Delete a collection</span></span>
- - -
    <span data-ttu-id="f96dd-125">Ta bort AzureRemoteAppCollection<enter collection name></span><span class="sxs-lookup"><span data-stu-id="f96dd-125">Remove-AzureRemoteAppCollection <enter collection name></span></span>

<span data-ttu-id="f96dd-126">Exempel: `Remove-AzureRemoteAppCollection ContosoProduction`.</span><span class="sxs-lookup"><span data-stu-id="f96dd-126">Example:  `Remove-AzureRemoteAppCollection ContosoProduction`.</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="f96dd-127">Skapa en molnsamling</span><span class="sxs-lookup"><span data-stu-id="f96dd-127">Create a cloud collection</span></span>
- - -
<span data-ttu-id="f96dd-128">Det är enkelt, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f96dd-128">It's simple, run hello following command:</span></span>

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

<span data-ttu-id="f96dd-129">Hej ovanför kommandot publiceras automatiskt Microsoft Office 365-program (Excel, OneNote, Outlook, PowerPoint, Visio och Word).</span><span class="sxs-lookup"><span data-stu-id="f96dd-129">hello above command automatically publishes Microsoft Office 365 applications (Excel, OneNote, Outlook, PowerPoint, Visio and Word).</span></span>

<span data-ttu-id="f96dd-130">Skapa en samling kan ta 30 minuter eller längre toocomplete.</span><span class="sxs-lookup"><span data-stu-id="f96dd-130">Collection creation can take 30 minutes or longer toocomplete.</span></span> <span data-ttu-id="f96dd-131">Det här kommandot returnerar därför ett spårnings-ID som du kan använda på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f96dd-131">Therefore, this command returns a tracking ID that you can use as follows:</span></span>

    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

<span data-ttu-id="f96dd-132">Därefter hello samling du kan lägga till användare toohello samling med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f96dd-132">After hello collection is done, you can add users toohello collection with hello following command:</span></span>

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

<span data-ttu-id="f96dd-133">Och du är klar!</span><span class="sxs-lookup"><span data-stu-id="f96dd-133">And you're done!</span></span> <span data-ttu-id="f96dd-134">Att användaren ska kunna tooconnect toohello program med hjälp av hello Azure RemoteApp-klienten hitta [här](https://www.remoteapp.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="f96dd-134">That user should be able tooconnect toohello application using hello Azure RemoteApp client found [here](https://www.remoteapp.windowsazure.com/).</span></span>

## <a name="available-cmdlets"></a><span data-ttu-id="f96dd-135">Tillgängliga cmdlet:ar</span><span class="sxs-lookup"><span data-stu-id="f96dd-135">Available cmdlets</span></span>
<span data-ttu-id="f96dd-136">Det finns många andra kommandon som vi har, hello dokumentationen för dem kommer snart:</span><span class="sxs-lookup"><span data-stu-id="f96dd-136">There are lots of other commands that we have, hello documentation for them will be coming shortly:</span></span>

<span data-ttu-id="f96dd-137">Grundläggande RemoteApp-samlingen cmdlets:</span><span class="sxs-lookup"><span data-stu-id="f96dd-137">Basic RemoteApp Collection cmdlets:</span></span> 

* <span data-ttu-id="f96dd-138">New-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="f96dd-138">New-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="f96dd-139">Get-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="f96dd-139">Get-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="f96dd-140">Set-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="f96dd-140">Set-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="f96dd-141">Update-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="f96dd-141">Update-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="f96dd-142">Remove-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="f96dd-142">Remove-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="f96dd-143">Add-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="f96dd-143">Add-AzureRemoteAppUser</span></span>
* <span data-ttu-id="f96dd-144">Get-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="f96dd-144">Get-AzureRemoteAppUser</span></span>
* <span data-ttu-id="f96dd-145">Remove-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="f96dd-145">Remove-AzureRemoteAppUser</span></span>
* <span data-ttu-id="f96dd-146">Get-AzureRemoteAppSession</span><span class="sxs-lookup"><span data-stu-id="f96dd-146">Get-AzureRemoteAppSession</span></span>
* <span data-ttu-id="f96dd-147">Disconnect-AzureRemoteAppSession</span><span class="sxs-lookup"><span data-stu-id="f96dd-147">Disconnect-AzureRemoteAppSession</span></span>
* <span data-ttu-id="f96dd-148">Invoke-AzureRemoteAppSessionLogoff</span><span class="sxs-lookup"><span data-stu-id="f96dd-148">Invoke-AzureRemoteAppSessionLogoff</span></span>
* <span data-ttu-id="f96dd-149">Send-AzureRemoteAppSessionMessage</span><span class="sxs-lookup"><span data-stu-id="f96dd-149">Send-AzureRemoteAppSessionMessage</span></span>
* <span data-ttu-id="f96dd-150">Get-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="f96dd-150">Get-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="f96dd-151">Get-AzureRemoteAppStartMenuProgram</span><span class="sxs-lookup"><span data-stu-id="f96dd-151">Get-AzureRemoteAppStartMenuProgram</span></span>
* <span data-ttu-id="f96dd-152">Publish-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="f96dd-152">Publish-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="f96dd-153">Unpublish-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="f96dd-153">Unpublish-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="f96dd-154">Get-AzureRemoteAppCollectionUsageDetails</span><span class="sxs-lookup"><span data-stu-id="f96dd-154">Get-AzureRemoteAppCollectionUsageDetails</span></span>
* <span data-ttu-id="f96dd-155">Get-AzureRemoteAppCollectionUsageSummary</span><span class="sxs-lookup"><span data-stu-id="f96dd-155">Get-AzureRemoteAppCollectionUsageSummary</span></span>
* <span data-ttu-id="f96dd-156">Get-AzureRemoteAppPlan</span><span class="sxs-lookup"><span data-stu-id="f96dd-156">Get-AzureRemoteAppPlan</span></span>

<span data-ttu-id="f96dd-157">RemoteApp-cmdlets för virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="f96dd-157">RemoteApp virtual network cmdlets:</span></span>

* <span data-ttu-id="f96dd-158">New-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="f96dd-158">New-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="f96dd-159">Get-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="f96dd-159">Get-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="f96dd-160">Set-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="f96dd-160">Set-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="f96dd-161">Remove-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="f96dd-161">Remove-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="f96dd-162">Get-AzureRemoteAppVpnDevice</span><span class="sxs-lookup"><span data-stu-id="f96dd-162">Get-AzureRemoteAppVpnDevice</span></span>
* <span data-ttu-id="f96dd-163">Get - AzureRemoteAppVpnDeviceConfigScript</span><span class="sxs-lookup"><span data-stu-id="f96dd-163">Get-- AzureRemoteAppVpnDeviceConfigScript</span></span>
* <span data-ttu-id="f96dd-164">Reset-AzureRemoteAppVpnSharedKey</span><span class="sxs-lookup"><span data-stu-id="f96dd-164">Reset-AzureRemoteAppVpnSharedKey</span></span>

<span data-ttu-id="f96dd-165">RemoteApp-mallen avbildningen cmdlets:</span><span class="sxs-lookup"><span data-stu-id="f96dd-165">RemoteApp template image cmdlets:</span></span>

* <span data-ttu-id="f96dd-166">New-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="f96dd-166">New-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="f96dd-167">Get-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="f96dd-167">Get-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="f96dd-168">Rename-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="f96dd-168">Rename-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="f96dd-169">Remove-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="f96dd-169">Remove-AzureRemoteAppTemplateImage</span></span>

<span data-ttu-id="f96dd-170">Andra RemoteApp-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="f96dd-170">Other RemoteApp cmdlets:</span></span>

* <span data-ttu-id="f96dd-171">Get-AzureRemoteAppLocation</span><span class="sxs-lookup"><span data-stu-id="f96dd-171">Get-AzureRemoteAppLocation</span></span>
* <span data-ttu-id="f96dd-172">Get-AzureRemoteAppWorkspace</span><span class="sxs-lookup"><span data-stu-id="f96dd-172">Get-AzureRemoteAppWorkspace</span></span>
* <span data-ttu-id="f96dd-173">Set-AzureRemoteAppWorkspace</span><span class="sxs-lookup"><span data-stu-id="f96dd-173">Set-AzureRemoteAppWorkspace</span></span>
* <span data-ttu-id="f96dd-174">Get-AzureRemoteAppOperationResult</span><span class="sxs-lookup"><span data-stu-id="f96dd-174">Get-AzureRemoteAppOperationResult</span></span>

