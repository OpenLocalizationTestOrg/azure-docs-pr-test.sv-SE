---
title: "aaaIntegrate loggar från Azure Key Vault med Händelsehubbar | Microsoft Docs"
description: "Kursen som tillhandahåller hello nödvändiga åtgärder toomake Key Vault loggar tillgängliga tooa SIEM med hjälp av Azure Log-integrering"
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.topic: article
ms.date: 08/07/2017
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: ada2fc846cc6bf09e12cc2c016815b27afef0d50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a><span data-ttu-id="b732b-103">Azure Log Integration Självstudier: processen Azure Key Vault-händelser med hjälp av Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="b732b-103">Azure Log Integration tutorial: Process Azure Key Vault events by using Event Hubs</span></span>

<span data-ttu-id="b732b-104">Du kan använda Azure Log-integrering tooretrieve loggas händelser och göra dem tillgängliga tooyour information och händelse (SIEM) hanteringssystem för informationssäkerhet.</span><span class="sxs-lookup"><span data-stu-id="b732b-104">You can use Azure Log Integration tooretrieve logged events and make them available tooyour security information and event management (SIEM) system.</span></span> <span data-ttu-id="b732b-105">Den här kursen visar ett exempel på hur Azure Log-integrering kan vara används tooprocess loggar som skaffas genom Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="b732b-105">This tutorial shows an example of how Azure Log Integration can be used tooprocess logs that are acquired through Azure Event Hubs.</span></span>
 
<span data-ttu-id="b732b-106">Använd den här självstudiekursen tooget bekanta dig med hur hello Azure Log-integrering och Händelsehubbar fungerar tillsammans med följande exempel steg och förstå hur varje steg stöder hello lösning.</span><span class="sxs-lookup"><span data-stu-id="b732b-106">Use this tutorial tooget acquainted with how Azure Log Integration and Event Hubs work together by following hello example steps and understanding how each step supports hello solution.</span></span> <span data-ttu-id="b732b-107">Sedan kan du dra vad du har lärt dig här toocreate egna steg toosupport ditt företags unika krav.</span><span class="sxs-lookup"><span data-stu-id="b732b-107">Then you can take what you’ve learned here toocreate your own steps toosupport your company’s unique requirements.</span></span>

>[!WARNING]
<span data-ttu-id="b732b-108">hello steg och kommandon i den här självstudiekursen är inte avsedd toobe kopieras och klistras in.</span><span class="sxs-lookup"><span data-stu-id="b732b-108">hello steps and commands in this tutorial are not intended toobe copied and pasted.</span></span> <span data-ttu-id="b732b-109">Det är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="b732b-109">They're examples only.</span></span> <span data-ttu-id="b732b-110">Använd inte hello PowerShell-kommandon ”i befintligt skick” i live-miljön.</span><span class="sxs-lookup"><span data-stu-id="b732b-110">Do not use hello PowerShell commands “as is” in your live environment.</span></span> <span data-ttu-id="b732b-111">Du måste anpassa dem utifrån din miljö.</span><span class="sxs-lookup"><span data-stu-id="b732b-111">You must customize them based on your unique environment.</span></span>


<span data-ttu-id="b732b-112">Den här självstudiekursen vägleder dig genom hello av tar Azure Key Vault aktivitet loggade tooan händelsehubb och göra den tillgänglig som JSON-filer tooyour SIEM-system.</span><span class="sxs-lookup"><span data-stu-id="b732b-112">This tutorial walks you through hello process of taking Azure Key Vault activity logged tooan event hub and making it available as JSON files tooyour SIEM system.</span></span> <span data-ttu-id="b732b-113">Du kan sedan konfigurera SIEM tooprocess hello JSON systemfiler.</span><span class="sxs-lookup"><span data-stu-id="b732b-113">You can then configure your SIEM system tooprocess hello JSON files.</span></span>

>[!NOTE]
><span data-ttu-id="b732b-114">De flesta av hello stegen i den här kursen omfattar konfigurera nyckelvalv, lagringskonton och händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="b732b-114">Most of hello steps in this tutorial involve configuring key vaults, storage accounts, and event hubs.</span></span> <span data-ttu-id="b732b-115">hello specifika Azure Log integreringen är hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="b732b-115">hello specific Azure Log Integration steps are at hello end of this tutorial.</span></span> <span data-ttu-id="b732b-116">Utför inte de här stegen i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b732b-116">Do not perform these steps in a production environment.</span></span> <span data-ttu-id="b732b-117">De är avsedda för en labbmiljö.</span><span class="sxs-lookup"><span data-stu-id="b732b-117">They are intended for a lab environment only.</span></span> <span data-ttu-id="b732b-118">Du måste anpassa hello steg innan du använder dem i produktion.</span><span class="sxs-lookup"><span data-stu-id="b732b-118">You must customize hello steps before using them in production.</span></span>

<span data-ttu-id="b732b-119">Informationen längs hello sätt hjälper till att du förstår hello orsaker bakom varje steg.</span><span class="sxs-lookup"><span data-stu-id="b732b-119">Information provided along hello way helps you understand hello reasons behind each step.</span></span> <span data-ttu-id="b732b-120">Länkar tooother artiklar får du mer information på vissa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b732b-120">Links tooother articles give you more detail on certain topics.</span></span>

<span data-ttu-id="b732b-121">Mer information om hello-tjänster som nämns i den här kursen finns:</span><span class="sxs-lookup"><span data-stu-id="b732b-121">For more information about hello services that this tutorial mentions, see:</span></span> 

- [<span data-ttu-id="b732b-122">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b732b-122">Azure Key Vault</span></span>](../key-vault/key-vault-whatis.md)
- [<span data-ttu-id="b732b-123">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b732b-123">Azure Event Hubs</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="b732b-124">Azure logganalys-integrering</span><span class="sxs-lookup"><span data-stu-id="b732b-124">Azure Log Integration</span></span>](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a><span data-ttu-id="b732b-125">Installationen</span><span class="sxs-lookup"><span data-stu-id="b732b-125">Initial setup</span></span>

<span data-ttu-id="b732b-126">Innan du kan slutföra hello stegen i den här artikeln, måste du hello följande:</span><span class="sxs-lookup"><span data-stu-id="b732b-126">Before you can complete hello steps in this article, you need hello following:</span></span>

1. <span data-ttu-id="b732b-127">En Azure-prenumeration och konto i den prenumerationen med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="b732b-127">An Azure subscription and account on that subscription with administrator rights.</span></span> <span data-ttu-id="b732b-128">Om du inte har en prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="b732b-128">If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/).</span></span>
 
2. <span data-ttu-id="b732b-129">Ett system med åtkomst toohello internet som uppfyller hello för att installera Azure Log-integrering.</span><span class="sxs-lookup"><span data-stu-id="b732b-129">A system with access toohello internet that meets hello requirements for installing Azure Log Integration.</span></span> <span data-ttu-id="b732b-130">hello system kan vara på en molnbaserad tjänst eller lagras lokalt.</span><span class="sxs-lookup"><span data-stu-id="b732b-130">hello system can be on a cloud service or hosted on-premises.</span></span>

3. <span data-ttu-id="b732b-131">[Azure Log-integrering](https://www.microsoft.com/download/details.aspx?id=53324) installerad.</span><span class="sxs-lookup"><span data-stu-id="b732b-131">[Azure Log Integration](https://www.microsoft.com/download/details.aspx?id=53324) installed.</span></span> <span data-ttu-id="b732b-132">tooinstall den:</span><span class="sxs-lookup"><span data-stu-id="b732b-132">tooinstall it:</span></span>

   <span data-ttu-id="b732b-133">a.</span><span class="sxs-lookup"><span data-stu-id="b732b-133">a.</span></span> <span data-ttu-id="b732b-134">Använd Fjärrskrivbord tooconnect toohello system som anges i steg 2.</span><span class="sxs-lookup"><span data-stu-id="b732b-134">Use Remote Desktop tooconnect toohello system mentioned in step 2.</span></span>   
   <span data-ttu-id="b732b-135">b.</span><span class="sxs-lookup"><span data-stu-id="b732b-135">b.</span></span> <span data-ttu-id="b732b-136">Kopiera hello Azure Log-integrering installer toohello system.</span><span class="sxs-lookup"><span data-stu-id="b732b-136">Copy hello Azure Log Integration installer toohello system.</span></span> <span data-ttu-id="b732b-137">Du kan [ladda ned installationsfilerna för hello](https://www.microsoft.com/download/details.aspx?id=53324).</span><span class="sxs-lookup"><span data-stu-id="b732b-137">You can [download hello installation files](https://www.microsoft.com/download/details.aspx?id=53324).</span></span>   
   <span data-ttu-id="b732b-138">c.</span><span class="sxs-lookup"><span data-stu-id="b732b-138">c.</span></span> <span data-ttu-id="b732b-139">Starta hello installer och acceptera licensvillkoren för hello.</span><span class="sxs-lookup"><span data-stu-id="b732b-139">Start hello installer and accept hello Microsoft Software License Terms.</span></span>   
   <span data-ttu-id="b732b-140">d.</span><span class="sxs-lookup"><span data-stu-id="b732b-140">d.</span></span> <span data-ttu-id="b732b-141">Om du ger telemetri information, lämna hello kryssrutan är markerad.</span><span class="sxs-lookup"><span data-stu-id="b732b-141">If you will provide telemetry information, leave hello check box selected.</span></span> <span data-ttu-id="b732b-142">Om du inte skickar hellre användning information tooMicrosoft, avmarkerar du kryssrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="b732b-142">If you'd rather not send usage information tooMicrosoft, clear hello check box.</span></span>
   
   <span data-ttu-id="b732b-143">Mer information om Azure Log-integrering och hur tooinstall, se [Azure Log integrering med Azure diagnostikloggning och vidarebefordran av Windows-händelse](security-azure-log-integration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b732b-143">For more information about Azure Log Integration and how tooinstall it, see [Azure Log Integration with Azure Diagnostics logging and Windows Event Forwarding](security-azure-log-integration-get-started.md).</span></span>

4. <span data-ttu-id="b732b-144">hello senaste PowerShell-version.</span><span class="sxs-lookup"><span data-stu-id="b732b-144">hello latest PowerShell version.</span></span>
 
   <span data-ttu-id="b732b-145">Om du har Windows Server 2016 installerad, så att du har minst PowerShell 5.0.</span><span class="sxs-lookup"><span data-stu-id="b732b-145">If you have Windows Server 2016 installed, then you have at least PowerShell 5.0.</span></span> <span data-ttu-id="b732b-146">Om du använder någon annan version av Windows Server, kanske en tidigare version av PowerShell som är installerad.</span><span class="sxs-lookup"><span data-stu-id="b732b-146">If you're using any other version of Windows Server, you might have an earlier version of PowerShell installed.</span></span> <span data-ttu-id="b732b-147">Du kan kontrollera hello version genom att ange ```get-host``` i PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="b732b-147">You can check hello version by entering ```get-host``` in a PowerShell window.</span></span> <span data-ttu-id="b732b-148">Om du inte har PowerShell 5.0 installerat, kan du [ladda ned den](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="b732b-148">If you don't have PowerShell 5.0 installed, you can [download it](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>

   <span data-ttu-id="b732b-149">När du har minst PowerShell 5.0, kan du fortsätta tooinstall hello senaste versionen:</span><span class="sxs-lookup"><span data-stu-id="b732b-149">After you have at least PowerShell 5.0, you can proceed tooinstall hello latest version:</span></span>
   
   <span data-ttu-id="b732b-150">a.</span><span class="sxs-lookup"><span data-stu-id="b732b-150">a.</span></span> <span data-ttu-id="b732b-151">Ange i ett PowerShell-fönster hello ```Install-Module Azure``` kommando.</span><span class="sxs-lookup"><span data-stu-id="b732b-151">In a PowerShell window, enter hello ```Install-Module Azure``` command.</span></span> <span data-ttu-id="b732b-152">Slutför hello installationssteg.</span><span class="sxs-lookup"><span data-stu-id="b732b-152">Complete hello installation steps.</span></span>    
   <span data-ttu-id="b732b-153">b.</span><span class="sxs-lookup"><span data-stu-id="b732b-153">b.</span></span> <span data-ttu-id="b732b-154">Ange hello ```Install-Module AzureRM``` kommando.</span><span class="sxs-lookup"><span data-stu-id="b732b-154">Enter hello ```Install-Module AzureRM``` command.</span></span> <span data-ttu-id="b732b-155">Slutför hello installationssteg.</span><span class="sxs-lookup"><span data-stu-id="b732b-155">Complete hello installation steps.</span></span>

   <span data-ttu-id="b732b-156">Mer information finns i [installera Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span><span class="sxs-lookup"><span data-stu-id="b732b-156">For more information, see [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span></span>


## <a name="create-supporting-infrastructure-elements"></a><span data-ttu-id="b732b-157">Skapa stödjande infrastrukturelement</span><span class="sxs-lookup"><span data-stu-id="b732b-157">Create supporting infrastructure elements</span></span>

1. <span data-ttu-id="b732b-158">Öppna ett upphöjt PowerShell-fönster och gå för**C:\Program Files\Microsoft Azure Log integrering**.</span><span class="sxs-lookup"><span data-stu-id="b732b-158">Open an elevated PowerShell window and go too**C:\Program Files\Microsoft Azure Log Integration**.</span></span>
2. <span data-ttu-id="b732b-159">Importera hello AzLog cmdlets genom att köra skript hello LoadAzLogModule.ps1.</span><span class="sxs-lookup"><span data-stu-id="b732b-159">Import hello AzLog cmdlets by running hello script LoadAzLogModule.ps1.</span></span> <span data-ttu-id="b732b-160">Ange hello `.\LoadAzLogModule.ps1` kommando.</span><span class="sxs-lookup"><span data-stu-id="b732b-160">Enter hello `.\LoadAzLogModule.ps1` command.</span></span> <span data-ttu-id="b732b-161">(Observera hello ”. \” i det aktuella kommandot.) Du bör se något liknande följande:</span><span class="sxs-lookup"><span data-stu-id="b732b-161">(Notice hello “.\” in that command.) You should see something like this:</span></span></br>

   ![Lista med inlästa moduler](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. <span data-ttu-id="b732b-163">Ange hello `Login-AzureRmAccount` kommando.</span><span class="sxs-lookup"><span data-stu-id="b732b-163">Enter hello `Login-AzureRmAccount` command.</span></span> <span data-ttu-id="b732b-164">Ange hello autentiseringsuppgifter för hello-prenumeration som du ska använda för den här kursen i hello inloggningen fönster.</span><span class="sxs-lookup"><span data-stu-id="b732b-164">In hello login window, enter hello credential information for hello subscription that you will use for this tutorial.</span></span>

   >[!NOTE]
   ><span data-ttu-id="b732b-165">Om detta är hello första gången du loggar i tooAzure från den här datorn, visas ett meddelande om att tillåta att Microsoft toocollect PowerShell användningsdata.</span><span class="sxs-lookup"><span data-stu-id="b732b-165">If this is hello first time that you're logging in tooAzure from this machine, you will see a message about allowing Microsoft toocollect PowerShell usage data.</span></span> <span data-ttu-id="b732b-166">Vi rekommenderar att du aktiverar den här Datasamlingen eftersom den kommer att använda tooimprove Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b732b-166">We recommend that you enable this data collection because it will be used tooimprove Azure PowerShell.</span></span>

4. <span data-ttu-id="b732b-167">Du är inloggad och du ser hello informationen i följande skärmbild hello efter en lyckad autentisering.</span><span class="sxs-lookup"><span data-stu-id="b732b-167">After successful authentication, you're logged in and you see hello information in hello following screenshot.</span></span> <span data-ttu-id="b732b-168">Anteckna hello-ID och din prenumeration prenumerationsnamn, eftersom du behöver dem toocomplete steg senare.</span><span class="sxs-lookup"><span data-stu-id="b732b-168">Take note of hello subscription ID and subscription name, because you'll need them toocomplete later steps.</span></span>

   ![PowerShell-fönster](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. <span data-ttu-id="b732b-170">Skapa variabler toostore värden som kommer att användas senare.</span><span class="sxs-lookup"><span data-stu-id="b732b-170">Create variables toostore values that will be used later.</span></span> <span data-ttu-id="b732b-171">Ange var och en av följande PowerShell raderna hello.</span><span class="sxs-lookup"><span data-stu-id="b732b-171">Enter each of hello following PowerShell lines.</span></span> <span data-ttu-id="b732b-172">Du kan behöva tooadjust hello värden toomatch din miljö.</span><span class="sxs-lookup"><span data-stu-id="b732b-172">You might need tooadjust hello values toomatch your environment.</span></span>
    - <span data-ttu-id="b732b-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(Namnet på din prenumeration kan vara olika.</span><span class="sxs-lookup"><span data-stu-id="b732b-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’``` (Your subscription name might be different.</span></span> <span data-ttu-id="b732b-174">Du kan se det som en del av hello utdata från hello föregående kommandot.)</span><span class="sxs-lookup"><span data-stu-id="b732b-174">You can see it as part of hello output of hello previous command.)</span></span>
    - <span data-ttu-id="b732b-175">```$location = 'West US'```(Den här variabeln kommer att använda toopass hello plats där resurser ska skapas.</span><span class="sxs-lookup"><span data-stu-id="b732b-175">```$location = 'West US'``` (This variable will be used toopass hello location where resources should be created.</span></span> <span data-ttu-id="b732b-176">Du kan ändra den här variabeln toobe valfri plats som du väljer.)</span><span class="sxs-lookup"><span data-stu-id="b732b-176">You can change this variable toobe any location of your choosing.)</span></span>
    - ```$random = Get-Random```
    - <span data-ttu-id="b732b-177">``` $name = 'azlogtest' + $random```(hello namn kan vara allt, men den bör innehålla endast små bokstäver och siffror.)</span><span class="sxs-lookup"><span data-stu-id="b732b-177">``` $name = 'azlogtest' + $random``` (hello name can be anything, but it should include only lowercase letters and numbers.)</span></span>
    - <span data-ttu-id="b732b-178">``` $storageName = $name```(Den här variabeln används för hello lagringskontonamnet.)</span><span class="sxs-lookup"><span data-stu-id="b732b-178">``` $storageName = $name``` (This variable will be used for hello storage account name.)</span></span>
    - <span data-ttu-id="b732b-179">```$rgname = $name ```(Den här variabeln används för hello resursgruppens namn.)</span><span class="sxs-lookup"><span data-stu-id="b732b-179">```$rgname = $name ``` (This variable will be used for hello resource group name.)</span></span>
    - <span data-ttu-id="b732b-180">``` $eventHubNameSpaceName = $name```(Detta är hello namnet på hello event hub namnområde.)</span><span class="sxs-lookup"><span data-stu-id="b732b-180">``` $eventHubNameSpaceName = $name``` (This is hello name of hello event hub namespace.)</span></span>
6. <span data-ttu-id="b732b-181">Ange hello-prenumeration som du kommer att arbeta med:</span><span class="sxs-lookup"><span data-stu-id="b732b-181">Specify hello subscription that you will be working with:</span></span>
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. <span data-ttu-id="b732b-182">Skapa en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="b732b-182">Create a resource group:</span></span>
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   <span data-ttu-id="b732b-183">Om du anger `$rg` du bör nu se utdata liknande toothis skärmbild:</span><span class="sxs-lookup"><span data-stu-id="b732b-183">If you enter `$rg` at this point, you should see output similar toothis screenshot:</span></span>

   ![Utdata när den har skapats av en resursgrupp](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. <span data-ttu-id="b732b-185">Skapa ett lagringskonto som kommer att använda tookeep reda på statusinformation:</span><span class="sxs-lookup"><span data-stu-id="b732b-185">Create a storage account that will be used tookeep track of state information:</span></span>
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. <span data-ttu-id="b732b-186">Skapa hello event hub namnområde.</span><span class="sxs-lookup"><span data-stu-id="b732b-186">Create hello event hub namespace.</span></span> <span data-ttu-id="b732b-187">Detta är obligatorisk toocreate en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="b732b-187">This is required toocreate an event hub.</span></span>
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. <span data-ttu-id="b732b-188">Hämta hello regel-ID som ska användas med hello insikter providern:</span><span class="sxs-lookup"><span data-stu-id="b732b-188">Get hello rule ID that will be used with hello insights provider:</span></span>
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. <span data-ttu-id="b732b-189">Hämta alla möjliga Azure platser och lägga till hello namn tooa variabel som kan användas i ett senare steg:</span><span class="sxs-lookup"><span data-stu-id="b732b-189">Get all possible Azure locations and add hello names tooa variable that can be used in a later step:</span></span>
    
    <span data-ttu-id="b732b-190">a.</span><span class="sxs-lookup"><span data-stu-id="b732b-190">a.</span></span> ```$locationObjects = Get-AzureRMLocation```    
    <span data-ttu-id="b732b-191">b.</span><span class="sxs-lookup"><span data-stu-id="b732b-191">b.</span></span> ```$locations = @('global') + $locationobjects.location```
    
    <span data-ttu-id="b732b-192">Om du anger `$locations` nu du se hello platsnamn utan hello ytterligare information som returneras av Get-AzureRmLocation.</span><span class="sxs-lookup"><span data-stu-id="b732b-192">If you enter `$locations` at this point, you see hello location names without hello additional information returned by Get-AzureRmLocation.</span></span>
12. <span data-ttu-id="b732b-193">Skapa en profil för Azure Resource Manager-loggen:</span><span class="sxs-lookup"><span data-stu-id="b732b-193">Create an Azure Resource Manager log profile:</span></span> 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    <span data-ttu-id="b732b-194">Mer information om hello Azure logganalys-profilen finns [översikt över hello Azure-aktivitetsloggen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="b732b-194">For more information about hello Azure log profile, see [Overview of hello Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b732b-195">Du kan få ett felmeddelande när du försöker toocreate en logg-profil.</span><span class="sxs-lookup"><span data-stu-id="b732b-195">You might get an error message when you try toocreate a log profile.</span></span> <span data-ttu-id="b732b-196">Sedan kan du granska hello-dokumentation för Get-AzureRmLogProfile och ta bort AzureRmLogProfile.</span><span class="sxs-lookup"><span data-stu-id="b732b-196">You can then review hello documentation for Get-AzureRmLogProfile and Remove-AzureRmLogProfile.</span></span> <span data-ttu-id="b732b-197">Om du kör Get-AzureRmLogProfile finns information om hello log-profilen.</span><span class="sxs-lookup"><span data-stu-id="b732b-197">If you run Get-AzureRmLogProfile, you see information about hello log profile.</span></span> <span data-ttu-id="b732b-198">Du kan ta bort hello befintlig logg profil genom att ange hello ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` kommando.</span><span class="sxs-lookup"><span data-stu-id="b732b-198">You can delete hello existing log profile by entering hello ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` command.</span></span>
>
>![Fel för Resource Manager-profil](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a><span data-ttu-id="b732b-200">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="b732b-200">Create a key vault</span></span>

1. <span data-ttu-id="b732b-201">Skapa hello nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="b732b-201">Create hello key vault:</span></span>

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. <span data-ttu-id="b732b-202">Konfigurera loggning för hello nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="b732b-202">Configure logging for hello key vault:</span></span>

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a><span data-ttu-id="b732b-203">Generera loggaktivitet</span><span class="sxs-lookup"><span data-stu-id="b732b-203">Generate log activity</span></span>

<span data-ttu-id="b732b-204">Förfrågningar måste toobe skickas tooKey valvet toogenerate loggaktivitet.</span><span class="sxs-lookup"><span data-stu-id="b732b-204">Requests need toobe sent tooKey Vault toogenerate log activity.</span></span> <span data-ttu-id="b732b-205">Åtgärder som nyckelgenerering, lagring av hemligheter, eller läsa hemligheter från Nyckelvalvet skapas loggposter.</span><span class="sxs-lookup"><span data-stu-id="b732b-205">Actions like key generation, storing secrets, or reading secrets from Key Vault will create log entries.</span></span>

1. <span data-ttu-id="b732b-206">Visar hello aktuella lagringsnycklar:</span><span class="sxs-lookup"><span data-stu-id="b732b-206">Display hello current storage keys:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. <span data-ttu-id="b732b-207">Generera en ny **key2**:</span><span class="sxs-lookup"><span data-stu-id="b732b-207">Generate a new **key2**:</span></span>
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. <span data-ttu-id="b732b-208">Visa hello nycklar igen och se som **key2** innehåller ett annat värde:</span><span class="sxs-lookup"><span data-stu-id="b732b-208">Display hello keys again and see that **key2** holds a different value:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. <span data-ttu-id="b732b-209">Ange och läsa hemliga toogenerate ytterligare loggposter:</span><span class="sxs-lookup"><span data-stu-id="b732b-209">Set and read a secret toogenerate additional log entries:</span></span>
    
   <span data-ttu-id="b732b-210">a.</span><span class="sxs-lookup"><span data-stu-id="b732b-210">a.</span></span> <span data-ttu-id="b732b-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span><span class="sxs-lookup"><span data-stu-id="b732b-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span></span> ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Returnerade hemliga](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a><span data-ttu-id="b732b-213">Konfigurera Azure logganalys-integrering</span><span class="sxs-lookup"><span data-stu-id="b732b-213">Configure Azure Log Integration</span></span>

<span data-ttu-id="b732b-214">Nu när du har konfigurerat alla händelsehubb för hello obligatoriska element toohave Key Vault-loggning tooan, behöver du tooconfigure Azure Log-integrering:</span><span class="sxs-lookup"><span data-stu-id="b732b-214">Now that you have configured all hello required elements toohave Key Vault logging tooan event hub, you need tooconfigure Azure Log Integration:</span></span>

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

<span data-ttu-id="b732b-215">Kör hello AzLog kommando för varje händelsehubb:</span><span class="sxs-lookup"><span data-stu-id="b732b-215">Run hello AzLog command for each event hub:</span></span>

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

<span data-ttu-id="b732b-216">Du bör se JSON-filer som skapas när en minut eller så körs hello senast två kommandon.</span><span class="sxs-lookup"><span data-stu-id="b732b-216">After a minute or so of running hello last two commands, you should see JSON files being generated.</span></span> <span data-ttu-id="b732b-217">Du kan bekräfta att genom att övervaka hello directory **C:\users\AzLog\EventHubJson**.</span><span class="sxs-lookup"><span data-stu-id="b732b-217">You can confirm that by monitoring hello directory **C:\users\AzLog\EventHubJson**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b732b-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b732b-218">Next steps</span></span>

- [<span data-ttu-id="b732b-219">Azure logganalys Integration vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="b732b-219">Azure Log Integration FAQ</span></span>](security-azure-log-integration-faq.md)
- [<span data-ttu-id="b732b-220">Kom igång med Azure Log-integrering</span><span class="sxs-lookup"><span data-stu-id="b732b-220">Get started with Azure Log Integration</span></span>](security-azure-log-integration-get-started.md)
- [<span data-ttu-id="b732b-221">Integrera loggar från Azure-resurser i din SIEM-system</span><span class="sxs-lookup"><span data-stu-id="b732b-221">Integrate logs from Azure resources into your SIEM systems</span></span>](security-azure-log-integration-overview.md)
