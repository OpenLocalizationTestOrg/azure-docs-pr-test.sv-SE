---
title: "Integrera loggar från Azure Key Vault med Händelsehubbar | Microsoft Docs"
description: "Självstudiekurs som innehåller de nödvändiga stegen för att Key Vault loggar tillgängliga för en SIEM med hjälp av Azure Log-integrering"
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
ms.openlocfilehash: 3cd80817bf8b2ef2f66e9942eddc186a3eb5b5e4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a><span data-ttu-id="08a16-103">Azure Log Integration Självstudier: processen Azure Key Vault-händelser med hjälp av Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="08a16-103">Azure Log Integration tutorial: Process Azure Key Vault events by using Event Hubs</span></span>

<span data-ttu-id="08a16-104">Du kan använda Azure Log-integrering för att hämta loggade händelser och göra dem tillgängliga för din information och händelse (SIEM) hanteringssystem för informationssäkerhet.</span><span class="sxs-lookup"><span data-stu-id="08a16-104">You can use Azure Log Integration to retrieve logged events and make them available to your security information and event management (SIEM) system.</span></span> <span data-ttu-id="08a16-105">Den här kursen visar ett exempel på hur Azure Log-integrering kan användas för att bearbeta loggar som skaffas genom Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="08a16-105">This tutorial shows an example of how Azure Log Integration can be used to process logs that are acquired through Azure Event Hubs.</span></span>
 
<span data-ttu-id="08a16-106">Använd den här självstudiekursen för att bekanta dig med hur Azure Log-integrering och Händelsehubbar kan fungera tillsammans med följande exempel och förstå hur varje steg stöder lösningen.</span><span class="sxs-lookup"><span data-stu-id="08a16-106">Use this tutorial to get acquainted with how Azure Log Integration and Event Hubs work together by following the example steps and understanding how each step supports the solution.</span></span> <span data-ttu-id="08a16-107">Du kan sedan ta vad du har lärt dig här för att skapa egna steg för att stödja ditt företags unika krav.</span><span class="sxs-lookup"><span data-stu-id="08a16-107">Then you can take what you’ve learned here to create your own steps to support your company’s unique requirements.</span></span>

>[!WARNING]
<span data-ttu-id="08a16-108">Åtgärder och kommandon i den här kursen är inte avsedda att kopieras och klistras in.</span><span class="sxs-lookup"><span data-stu-id="08a16-108">The steps and commands in this tutorial are not intended to be copied and pasted.</span></span> <span data-ttu-id="08a16-109">Det är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="08a16-109">They're examples only.</span></span> <span data-ttu-id="08a16-110">Använd inte PowerShell-kommandon ”i befintligt skick” i live-miljön.</span><span class="sxs-lookup"><span data-stu-id="08a16-110">Do not use the PowerShell commands “as is” in your live environment.</span></span> <span data-ttu-id="08a16-111">Du måste anpassa dem utifrån din miljö.</span><span class="sxs-lookup"><span data-stu-id="08a16-111">You must customize them based on your unique environment.</span></span>


<span data-ttu-id="08a16-112">Den här självstudiekursen vägleder dig genom processen för att Azure Key Vault-aktivitet som loggas på en händelsehubb och göra den tillgänglig som JSON-filer för SIEM-system.</span><span class="sxs-lookup"><span data-stu-id="08a16-112">This tutorial walks you through the process of taking Azure Key Vault activity logged to an event hub and making it available as JSON files to your SIEM system.</span></span> <span data-ttu-id="08a16-113">Du kan sedan konfigurera SIEM-system för att bearbeta JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="08a16-113">You can then configure your SIEM system to process the JSON files.</span></span>

>[!NOTE]
><span data-ttu-id="08a16-114">De flesta av stegen i den här kursen omfattar konfigurera nyckelvalv, lagringskonton och händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="08a16-114">Most of the steps in this tutorial involve configuring key vaults, storage accounts, and event hubs.</span></span> <span data-ttu-id="08a16-115">Hur Azure Log-integrering finns i slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="08a16-115">The specific Azure Log Integration steps are at the end of this tutorial.</span></span> <span data-ttu-id="08a16-116">Utför inte de här stegen i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="08a16-116">Do not perform these steps in a production environment.</span></span> <span data-ttu-id="08a16-117">De är avsedda för en labbmiljö.</span><span class="sxs-lookup"><span data-stu-id="08a16-117">They are intended for a lab environment only.</span></span> <span data-ttu-id="08a16-118">Du måste anpassa instruktionerna innan du använder dem i produktion.</span><span class="sxs-lookup"><span data-stu-id="08a16-118">You must customize the steps before using them in production.</span></span>

<span data-ttu-id="08a16-119">Informationen på vägen hjälper dig att förstå skälen till varje steg.</span><span class="sxs-lookup"><span data-stu-id="08a16-119">Information provided along the way helps you understand the reasons behind each step.</span></span> <span data-ttu-id="08a16-120">Länkar till andra artiklar får du mer information om vissa ämnen.</span><span class="sxs-lookup"><span data-stu-id="08a16-120">Links to other articles give you more detail on certain topics.</span></span>

<span data-ttu-id="08a16-121">Mer information om de tjänster som nämns i den här kursen finns:</span><span class="sxs-lookup"><span data-stu-id="08a16-121">For more information about the services that this tutorial mentions, see:</span></span> 

- [<span data-ttu-id="08a16-122">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="08a16-122">Azure Key Vault</span></span>](../key-vault/key-vault-whatis.md)
- [<span data-ttu-id="08a16-123">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="08a16-123">Azure Event Hubs</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="08a16-124">Azure logganalys-integrering</span><span class="sxs-lookup"><span data-stu-id="08a16-124">Azure Log Integration</span></span>](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a><span data-ttu-id="08a16-125">Installationen</span><span class="sxs-lookup"><span data-stu-id="08a16-125">Initial setup</span></span>

<span data-ttu-id="08a16-126">Innan du kan slutföra stegen i den här artikeln, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="08a16-126">Before you can complete the steps in this article, you need the following:</span></span>

1. <span data-ttu-id="08a16-127">En Azure-prenumeration och konto i den prenumerationen med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="08a16-127">An Azure subscription and account on that subscription with administrator rights.</span></span> <span data-ttu-id="08a16-128">Om du inte har en prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="08a16-128">If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/).</span></span>
 
2. <span data-ttu-id="08a16-129">Ett system med åtkomst till internet som uppfyller kraven för att installera Azure Log-integrering.</span><span class="sxs-lookup"><span data-stu-id="08a16-129">A system with access to the internet that meets the requirements for installing Azure Log Integration.</span></span> <span data-ttu-id="08a16-130">Systemet kan vara på en molnbaserad tjänst eller lagras lokalt.</span><span class="sxs-lookup"><span data-stu-id="08a16-130">The system can be on a cloud service or hosted on-premises.</span></span>

3. <span data-ttu-id="08a16-131">[Azure Log-integrering](https://www.microsoft.com/download/details.aspx?id=53324) installerad.</span><span class="sxs-lookup"><span data-stu-id="08a16-131">[Azure Log Integration](https://www.microsoft.com/download/details.aspx?id=53324) installed.</span></span> <span data-ttu-id="08a16-132">Så här installerar du det:</span><span class="sxs-lookup"><span data-stu-id="08a16-132">To install it:</span></span>

   <span data-ttu-id="08a16-133">a.</span><span class="sxs-lookup"><span data-stu-id="08a16-133">a.</span></span> <span data-ttu-id="08a16-134">Använda Fjärrskrivbord för att ansluta till system som anges i steg 2.</span><span class="sxs-lookup"><span data-stu-id="08a16-134">Use Remote Desktop to connect to the system mentioned in step 2.</span></span>   
   <span data-ttu-id="08a16-135">b.</span><span class="sxs-lookup"><span data-stu-id="08a16-135">b.</span></span> <span data-ttu-id="08a16-136">Kopiera installationsprogrammet för Azure Log-integrering i systemet.</span><span class="sxs-lookup"><span data-stu-id="08a16-136">Copy the Azure Log Integration installer to the system.</span></span> <span data-ttu-id="08a16-137">Du kan [ladda ned installationsfilerna](https://www.microsoft.com/download/details.aspx?id=53324).</span><span class="sxs-lookup"><span data-stu-id="08a16-137">You can [download the installation files](https://www.microsoft.com/download/details.aspx?id=53324).</span></span>   
   <span data-ttu-id="08a16-138">c.</span><span class="sxs-lookup"><span data-stu-id="08a16-138">c.</span></span> <span data-ttu-id="08a16-139">Starta installationsprogrammet och acceptera Licensvillkor för programvara från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="08a16-139">Start the installer and accept the Microsoft Software License Terms.</span></span>   
   <span data-ttu-id="08a16-140">d.</span><span class="sxs-lookup"><span data-stu-id="08a16-140">d.</span></span> <span data-ttu-id="08a16-141">Om du ger telemetri information, lämnar du kryssrutan är markerad.</span><span class="sxs-lookup"><span data-stu-id="08a16-141">If you will provide telemetry information, leave the check box selected.</span></span> <span data-ttu-id="08a16-142">Om du inte skulle istället skicka användningsinformation till Microsoft, avmarkerar du kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="08a16-142">If you'd rather not send usage information to Microsoft, clear the check box.</span></span>
   
   <span data-ttu-id="08a16-143">Läs mer om Azure Log-integrering och hur du installerar det [Azure Log integrering med Azure diagnostikloggning och vidarebefordran av Windows-händelse](security-azure-log-integration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="08a16-143">For more information about Azure Log Integration and how to install it, see [Azure Log Integration with Azure Diagnostics logging and Windows Event Forwarding](security-azure-log-integration-get-started.md).</span></span>

4. <span data-ttu-id="08a16-144">Den senaste versionen i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08a16-144">The latest PowerShell version.</span></span>
 
   <span data-ttu-id="08a16-145">Om du har Windows Server 2016 installerad, så att du har minst PowerShell 5.0.</span><span class="sxs-lookup"><span data-stu-id="08a16-145">If you have Windows Server 2016 installed, then you have at least PowerShell 5.0.</span></span> <span data-ttu-id="08a16-146">Om du använder någon annan version av Windows Server, kanske en tidigare version av PowerShell som är installerad.</span><span class="sxs-lookup"><span data-stu-id="08a16-146">If you're using any other version of Windows Server, you might have an earlier version of PowerShell installed.</span></span> <span data-ttu-id="08a16-147">Du kan kontrollera versionen genom att ange ```get-host``` i PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="08a16-147">You can check the version by entering ```get-host``` in a PowerShell window.</span></span> <span data-ttu-id="08a16-148">Om du inte har PowerShell 5.0 installerat, kan du [ladda ned den](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="08a16-148">If you don't have PowerShell 5.0 installed, you can [download it](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>

   <span data-ttu-id="08a16-149">När du har minst PowerShell 5.0, du kan fortsätta att installera den senaste versionen:</span><span class="sxs-lookup"><span data-stu-id="08a16-149">After you have at least PowerShell 5.0, you can proceed to install the latest version:</span></span>
   
   <span data-ttu-id="08a16-150">a.</span><span class="sxs-lookup"><span data-stu-id="08a16-150">a.</span></span> <span data-ttu-id="08a16-151">Ange i ett PowerShell-fönster i ```Install-Module Azure``` kommando.</span><span class="sxs-lookup"><span data-stu-id="08a16-151">In a PowerShell window, enter the ```Install-Module Azure``` command.</span></span> <span data-ttu-id="08a16-152">Slutför följande steg.</span><span class="sxs-lookup"><span data-stu-id="08a16-152">Complete the installation steps.</span></span>    
   <span data-ttu-id="08a16-153">b.</span><span class="sxs-lookup"><span data-stu-id="08a16-153">b.</span></span> <span data-ttu-id="08a16-154">Ange den ```Install-Module AzureRM``` kommando.</span><span class="sxs-lookup"><span data-stu-id="08a16-154">Enter the ```Install-Module AzureRM``` command.</span></span> <span data-ttu-id="08a16-155">Slutför följande steg.</span><span class="sxs-lookup"><span data-stu-id="08a16-155">Complete the installation steps.</span></span>

   <span data-ttu-id="08a16-156">Mer information finns i [installera Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span><span class="sxs-lookup"><span data-stu-id="08a16-156">For more information, see [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span></span>


## <a name="create-supporting-infrastructure-elements"></a><span data-ttu-id="08a16-157">Skapa stödjande infrastrukturelement</span><span class="sxs-lookup"><span data-stu-id="08a16-157">Create supporting infrastructure elements</span></span>

1. <span data-ttu-id="08a16-158">Öppna ett upphöjt PowerShell-fönster och gå till **C:\Program Files\Microsoft Azure Log integrering**.</span><span class="sxs-lookup"><span data-stu-id="08a16-158">Open an elevated PowerShell window and go to **C:\Program Files\Microsoft Azure Log Integration**.</span></span>
2. <span data-ttu-id="08a16-159">Importera AzLog-cmdlets genom att köra skriptet LoadAzLogModule.ps1.</span><span class="sxs-lookup"><span data-stu-id="08a16-159">Import the AzLog cmdlets by running the script LoadAzLogModule.ps1.</span></span> <span data-ttu-id="08a16-160">Ange den `.\LoadAzLogModule.ps1` kommando.</span><span class="sxs-lookup"><span data-stu-id="08a16-160">Enter the `.\LoadAzLogModule.ps1` command.</span></span> <span data-ttu-id="08a16-161">(Observera den ”. \” i det aktuella kommandot.) Du bör se något liknande följande:</span><span class="sxs-lookup"><span data-stu-id="08a16-161">(Notice the “.\” in that command.) You should see something like this:</span></span></br>

   ![Lista med inlästa moduler](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. <span data-ttu-id="08a16-163">Ange den `Login-AzureRmAccount` kommando.</span><span class="sxs-lookup"><span data-stu-id="08a16-163">Enter the `Login-AzureRmAccount` command.</span></span> <span data-ttu-id="08a16-164">Ange autentiseringsuppgifter för den prenumeration som du ska använda för den här kursen i inloggningsfönstret.</span><span class="sxs-lookup"><span data-stu-id="08a16-164">In the login window, enter the credential information for the subscription that you will use for this tutorial.</span></span>

   >[!NOTE]
   ><span data-ttu-id="08a16-165">Om det här är första gången du loggar till Azure från den här datorn visas ett meddelande om att tillåta Microsoft att samla in användningsdata för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08a16-165">If this is the first time that you're logging in to Azure from this machine, you will see a message about allowing Microsoft to collect PowerShell usage data.</span></span> <span data-ttu-id="08a16-166">Vi rekommenderar att du aktiverar den här Datasamlingen eftersom den används för att förbättra Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08a16-166">We recommend that you enable this data collection because it will be used to improve Azure PowerShell.</span></span>

4. <span data-ttu-id="08a16-167">Du är inloggad och informationen i följande skärmbild visas efter en lyckad autentisering.</span><span class="sxs-lookup"><span data-stu-id="08a16-167">After successful authentication, you're logged in and you see the information in the following screenshot.</span></span> <span data-ttu-id="08a16-168">Anteckna prenumerations-ID och namn, eftersom du behöver dem för att slutföra senare steg.</span><span class="sxs-lookup"><span data-stu-id="08a16-168">Take note of the subscription ID and subscription name, because you'll need them to complete later steps.</span></span>

   ![PowerShell-fönster](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. <span data-ttu-id="08a16-170">Skapa variabler för att lagra värden som kommer att användas senare.</span><span class="sxs-lookup"><span data-stu-id="08a16-170">Create variables to store values that will be used later.</span></span> <span data-ttu-id="08a16-171">Ange var och en av följande rader i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08a16-171">Enter each of the following PowerShell lines.</span></span> <span data-ttu-id="08a16-172">Du kan behöva justera värden som stämmer överens din miljö.</span><span class="sxs-lookup"><span data-stu-id="08a16-172">You might need to adjust the values to match your environment.</span></span>
    - <span data-ttu-id="08a16-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(Namnet på din prenumeration kan vara olika.</span><span class="sxs-lookup"><span data-stu-id="08a16-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’``` (Your subscription name might be different.</span></span> <span data-ttu-id="08a16-174">Du kan se det som en del av utdata från det föregående kommandot.)</span><span class="sxs-lookup"><span data-stu-id="08a16-174">You can see it as part of the output of the previous command.)</span></span>
    - <span data-ttu-id="08a16-175">```$location = 'West US'```(Den här variabeln används för att skicka den plats där resurser ska skapas.</span><span class="sxs-lookup"><span data-stu-id="08a16-175">```$location = 'West US'``` (This variable will be used to pass the location where resources should be created.</span></span> <span data-ttu-id="08a16-176">Du kan ändra den här variabeln för att vara vilken plats som du väljer.)</span><span class="sxs-lookup"><span data-stu-id="08a16-176">You can change this variable to be any location of your choosing.)</span></span>
    - ```$random = Get-Random```
    - <span data-ttu-id="08a16-177">``` $name = 'azlogtest' + $random```(Namnet kan vara allt, men den bör innehålla endast små bokstäver och siffror.)</span><span class="sxs-lookup"><span data-stu-id="08a16-177">``` $name = 'azlogtest' + $random``` (The name can be anything, but it should include only lowercase letters and numbers.)</span></span>
    - <span data-ttu-id="08a16-178">``` $storageName = $name```(Den här variabeln används för lagringskontonamnet.)</span><span class="sxs-lookup"><span data-stu-id="08a16-178">``` $storageName = $name``` (This variable will be used for the storage account name.)</span></span>
    - <span data-ttu-id="08a16-179">```$rgname = $name ```(Den här variabeln används för resursgruppens namn.)</span><span class="sxs-lookup"><span data-stu-id="08a16-179">```$rgname = $name ``` (This variable will be used for the resource group name.)</span></span>
    - <span data-ttu-id="08a16-180">``` $eventHubNameSpaceName = $name```(Detta är namnet på namnområdet event hub.)</span><span class="sxs-lookup"><span data-stu-id="08a16-180">``` $eventHubNameSpaceName = $name``` (This is the name of the event hub namespace.)</span></span>
6. <span data-ttu-id="08a16-181">Ange den prenumeration som du kommer att arbeta med:</span><span class="sxs-lookup"><span data-stu-id="08a16-181">Specify the subscription that you will be working with:</span></span>
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. <span data-ttu-id="08a16-182">Skapa en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="08a16-182">Create a resource group:</span></span>
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   <span data-ttu-id="08a16-183">Om du anger `$rg` du bör nu se utdata som liknar denna skärmbild:</span><span class="sxs-lookup"><span data-stu-id="08a16-183">If you enter `$rg` at this point, you should see output similar to this screenshot:</span></span>

   ![Utdata när den har skapats av en resursgrupp](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. <span data-ttu-id="08a16-185">Skapa ett lagringskonto som ska användas för att hålla reda på statusinformation:</span><span class="sxs-lookup"><span data-stu-id="08a16-185">Create a storage account that will be used to keep track of state information:</span></span>
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. <span data-ttu-id="08a16-186">Skapa namnområdet event hub.</span><span class="sxs-lookup"><span data-stu-id="08a16-186">Create the event hub namespace.</span></span> <span data-ttu-id="08a16-187">Detta är nödvändigt att skapa en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="08a16-187">This is required to create an event hub.</span></span>
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. <span data-ttu-id="08a16-188">Hämta regel-ID som ska användas med insikter providern:</span><span class="sxs-lookup"><span data-stu-id="08a16-188">Get the rule ID that will be used with the insights provider:</span></span>
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. <span data-ttu-id="08a16-189">Hämta alla möjliga Azure platser och lägga till namn till en variabel som kan användas i ett senare steg:</span><span class="sxs-lookup"><span data-stu-id="08a16-189">Get all possible Azure locations and add the names to a variable that can be used in a later step:</span></span>
    
    <span data-ttu-id="08a16-190">a.</span><span class="sxs-lookup"><span data-stu-id="08a16-190">a.</span></span> ```$locationObjects = Get-AzureRMLocation```    
    <span data-ttu-id="08a16-191">b.</span><span class="sxs-lookup"><span data-stu-id="08a16-191">b.</span></span> ```$locations = @('global') + $locationobjects.location```
    
    <span data-ttu-id="08a16-192">Om du anger `$locations` nu visas platsnamnen utan ytterligare information som returneras av Get-AzureRmLocation.</span><span class="sxs-lookup"><span data-stu-id="08a16-192">If you enter `$locations` at this point, you see the location names without the additional information returned by Get-AzureRmLocation.</span></span>
12. <span data-ttu-id="08a16-193">Skapa en profil för Azure Resource Manager-loggen:</span><span class="sxs-lookup"><span data-stu-id="08a16-193">Create an Azure Resource Manager log profile:</span></span> 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    <span data-ttu-id="08a16-194">Mer information om Azure logganalys-profilen finns [översikt över Azure-aktivitetsloggen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="08a16-194">For more information about the Azure log profile, see [Overview of the Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="08a16-195">Du kan få ett felmeddelande när du försöker skapa en profil för loggen.</span><span class="sxs-lookup"><span data-stu-id="08a16-195">You might get an error message when you try to create a log profile.</span></span> <span data-ttu-id="08a16-196">Du kan granska dokumentationen för Get-AzureRmLogProfile och ta bort AzureRmLogProfile.</span><span class="sxs-lookup"><span data-stu-id="08a16-196">You can then review the documentation for Get-AzureRmLogProfile and Remove-AzureRmLogProfile.</span></span> <span data-ttu-id="08a16-197">Om du kör Get-AzureRmLogProfile finns information om profilen för loggen.</span><span class="sxs-lookup"><span data-stu-id="08a16-197">If you run Get-AzureRmLogProfile, you see information about the log profile.</span></span> <span data-ttu-id="08a16-198">Du kan ta bort den befintliga profilen för loggfilen genom att ange den ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` kommando.</span><span class="sxs-lookup"><span data-stu-id="08a16-198">You can delete the existing log profile by entering the ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` command.</span></span>
>
>![Fel för Resource Manager-profil](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a><span data-ttu-id="08a16-200">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="08a16-200">Create a key vault</span></span>

1. <span data-ttu-id="08a16-201">Skapa nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="08a16-201">Create the key vault:</span></span>

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. <span data-ttu-id="08a16-202">Konfigurera loggning för nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="08a16-202">Configure logging for the key vault:</span></span>

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a><span data-ttu-id="08a16-203">Generera loggaktivitet</span><span class="sxs-lookup"><span data-stu-id="08a16-203">Generate log activity</span></span>

<span data-ttu-id="08a16-204">Förfrågningar måste skickas till Key Vault för att generera loggfilen aktivitet.</span><span class="sxs-lookup"><span data-stu-id="08a16-204">Requests need to be sent to Key Vault to generate log activity.</span></span> <span data-ttu-id="08a16-205">Åtgärder som nyckelgenerering, lagring av hemligheter, eller läsa hemligheter från Nyckelvalvet skapas loggposter.</span><span class="sxs-lookup"><span data-stu-id="08a16-205">Actions like key generation, storing secrets, or reading secrets from Key Vault will create log entries.</span></span>

1. <span data-ttu-id="08a16-206">Visa de aktuella lagringsnycklar:</span><span class="sxs-lookup"><span data-stu-id="08a16-206">Display the current storage keys:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. <span data-ttu-id="08a16-207">Generera en ny **key2**:</span><span class="sxs-lookup"><span data-stu-id="08a16-207">Generate a new **key2**:</span></span>
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. <span data-ttu-id="08a16-208">Visa nycklarna igen och se som **key2** innehåller ett annat värde:</span><span class="sxs-lookup"><span data-stu-id="08a16-208">Display the keys again and see that **key2** holds a different value:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. <span data-ttu-id="08a16-209">Ange och läsa en hemlighet för att generera ytterligare loggposter:</span><span class="sxs-lookup"><span data-stu-id="08a16-209">Set and read a secret to generate additional log entries:</span></span>
    
   <span data-ttu-id="08a16-210">a.</span><span class="sxs-lookup"><span data-stu-id="08a16-210">a.</span></span> <span data-ttu-id="08a16-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span><span class="sxs-lookup"><span data-stu-id="08a16-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span></span> ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Returnerade hemliga](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a><span data-ttu-id="08a16-213">Konfigurera Azure logganalys-integrering</span><span class="sxs-lookup"><span data-stu-id="08a16-213">Configure Azure Log Integration</span></span>

<span data-ttu-id="08a16-214">Nu när du har konfigurerat de element som krävs för Key Vault-loggning till en händelsehubb, måste du konfigurera Azure Log-integrering:</span><span class="sxs-lookup"><span data-stu-id="08a16-214">Now that you have configured all the required elements to have Key Vault logging to an event hub, you need to configure Azure Log Integration:</span></span>

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

<span data-ttu-id="08a16-215">Kör kommandot AzLog för varje händelsehubb:</span><span class="sxs-lookup"><span data-stu-id="08a16-215">Run the AzLog command for each event hub:</span></span>

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

<span data-ttu-id="08a16-216">Du bör se JSON-filer som skapas efter en minut eller så körs de sista två kommandona.</span><span class="sxs-lookup"><span data-stu-id="08a16-216">After a minute or so of running the last two commands, you should see JSON files being generated.</span></span> <span data-ttu-id="08a16-217">Du kan bekräfta att genom att övervaka katalogen **C:\users\AzLog\EventHubJson**.</span><span class="sxs-lookup"><span data-stu-id="08a16-217">You can confirm that by monitoring the directory **C:\users\AzLog\EventHubJson**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="08a16-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="08a16-218">Next steps</span></span>

- [<span data-ttu-id="08a16-219">Azure logganalys Integration vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="08a16-219">Azure Log Integration FAQ</span></span>](security-azure-log-integration-faq.md)
- [<span data-ttu-id="08a16-220">Kom igång med Azure Log-integrering</span><span class="sxs-lookup"><span data-stu-id="08a16-220">Get started with Azure Log Integration</span></span>](security-azure-log-integration-get-started.md)
- [<span data-ttu-id="08a16-221">Integrera loggar från Azure-resurser i din SIEM-system</span><span class="sxs-lookup"><span data-stu-id="08a16-221">Integrate logs from Azure resources into your SIEM systems</span></span>](security-azure-log-integration-overview.md)
