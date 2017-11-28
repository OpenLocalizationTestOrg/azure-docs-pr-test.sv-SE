---
title: aaaUse PowerShell toomanage Azure Event Hubs resurser | Microsoft Docs
description: "Använda PowerShell-modulen toocreate och hantera Händelsehubbar"
services: event-hubs
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: d79cb307c2b4a031d059ce6ca67117ffc0b4600b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-event-hubs-resources"></a><span data-ttu-id="bc48d-103">Använd PowerShell toomanage Händelsehubbar resurser</span><span class="sxs-lookup"><span data-stu-id="bc48d-103">Use PowerShell toomanage Event Hubs resources</span></span>

<span data-ttu-id="bc48d-104">Microsoft Azure PowerShell är en skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="bc48d-104">Microsoft Azure PowerShell is a scripting environment that you can use toocontrol and automate hello deployment and management of Azure services.</span></span> <span data-ttu-id="bc48d-105">Den här artikeln beskriver hur toouse hello [Event Hubs Resource Manager PowerShell-modulen](/powershell/module/azurerm.eventhub) tooprovision och hantera Händelsehubbar entiteter (namnområden, enskilda händelsehubbar och konsumentgrupper) med hjälp av den lokala Azure PowerShell-konsolen eller skript.</span><span class="sxs-lookup"><span data-stu-id="bc48d-105">This article describes how toouse hello [Event Hubs Resource Manager PowerShell module](/powershell/module/azurerm.eventhub) tooprovision and manage Event Hubs entities (namespaces, individual event hubs, and consumer groups) using a local Azure PowerShell console or script.</span></span>

<span data-ttu-id="bc48d-106">Du kan också hantera Händelsehubbar resurser med hjälp av Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="bc48d-106">You can also manage Event Hubs resources using Azure Resource Manager templates.</span></span> <span data-ttu-id="bc48d-107">Mer information finns i artikeln hello [skapa ett namnområde för Händelsehubbar med nav- och konsumenten händelsegruppen med en Azure Resource Manager-mall](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="bc48d-107">For more information, see hello article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc48d-108">Krav</span><span class="sxs-lookup"><span data-stu-id="bc48d-108">Prerequisites</span></span>

<span data-ttu-id="bc48d-109">Innan du börjar behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="bc48d-109">Before you begin, you'll need hello following:</span></span>

* <span data-ttu-id="bc48d-110">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bc48d-110">An Azure subscription.</span></span> <span data-ttu-id="bc48d-111">Mer information om hur du skaffar en prenumeration finns [köpalternativ][purchase options], [medlemserbjudanden][member offers], eller [kostnadsfritt konto][free account].</span><span class="sxs-lookup"><span data-stu-id="bc48d-111">For more information about obtaining a subscription, see [purchase options][purchase options], [member offers][member offers], or [free account][free account].</span></span>
* <span data-ttu-id="bc48d-112">En dator med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bc48d-112">A computer with Azure PowerShell.</span></span> <span data-ttu-id="bc48d-113">Instruktioner finns i [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="bc48d-113">For instructions, see [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps).</span></span>
* <span data-ttu-id="bc48d-114">En förståelse av PowerShell-skript, NuGet-paket och hello .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bc48d-114">A general understanding of PowerShell scripts, NuGet packages, and hello .NET Framework.</span></span>

## <a name="get-started"></a><span data-ttu-id="bc48d-115">Kom igång</span><span class="sxs-lookup"><span data-stu-id="bc48d-115">Get started</span></span>

<span data-ttu-id="bc48d-116">hello första steget är toouse PowerShell toolog i tooyour Azure-konto och Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bc48d-116">hello first step is toouse PowerShell toolog in tooyour Azure account and Azure subscription.</span></span> <span data-ttu-id="bc48d-117">Följ anvisningarna för hello i [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps) toolog i tooyour Azure-konto sedan hämta och komma åt hello resurser i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bc48d-117">Follow hello instructions in [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps) toolog in tooyour Azure account, then retrieve and access hello resources in your Azure subscription.</span></span>

## <a name="provision-an-event-hubs-namespace"></a><span data-ttu-id="bc48d-118">Etablera ett namnområde för Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="bc48d-118">Provision an Event Hubs namespace</span></span>

<span data-ttu-id="bc48d-119">När du arbetar med Händelsehubbar namnområden kan du använda hello [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [ny AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [ta bort AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace) , och [Set AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlets.</span><span class="sxs-lookup"><span data-stu-id="bc48d-119">When working with Event Hubs namespaces, you can use hello [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Remove-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace), and [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlets.</span></span>

<span data-ttu-id="bc48d-120">Det här exemplet skapar några lokala variabler i hello skript; `$Namespace` och `$Location`.</span><span class="sxs-lookup"><span data-stu-id="bc48d-120">This example creates a few local variables in hello script; `$Namespace` and `$Location`.</span></span>

* <span data-ttu-id="bc48d-121">`$Namespace`är hello Händelsehubbar namnområde som vi vill toowork hello namn.</span><span class="sxs-lookup"><span data-stu-id="bc48d-121">`$Namespace` is hello name of hello Event Hubs namespace with which we want toowork.</span></span>
* <span data-ttu-id="bc48d-122">`$Location`identifierar hello datacenter där vi etablerar hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="bc48d-122">`$Location` identifies hello data center in which will we provision hello namespace.</span></span>
* <span data-ttu-id="bc48d-123">`$CurrentNamespace`lagrar hello referens namnområde som vi hämta (eller skapa).</span><span class="sxs-lookup"><span data-stu-id="bc48d-123">`$CurrentNamespace` stores hello reference namespace that we retrieve (or create).</span></span>

<span data-ttu-id="bc48d-124">I en verklig skript `$Namespace` och `$Location` kan skickas som parametrar.</span><span class="sxs-lookup"><span data-stu-id="bc48d-124">In an actual script, `$Namespace` and `$Location` can be passed as parameters.</span></span>

<span data-ttu-id="bc48d-125">Den här delen av hello skript hello följande:</span><span class="sxs-lookup"><span data-stu-id="bc48d-125">This part of hello script does hello following:</span></span>

1. <span data-ttu-id="bc48d-126">Försök tooretrieve ett Händelsehubbar namnområde med hello angett namn.</span><span class="sxs-lookup"><span data-stu-id="bc48d-126">Attempts tooretrieve an Event Hubs namespace with hello specified name.</span></span>
2. <span data-ttu-id="bc48d-127">Om hello namnområde hittas rapporterar vad hittades.</span><span class="sxs-lookup"><span data-stu-id="bc48d-127">If hello namespace is found, it reports what was found.</span></span>
3. <span data-ttu-id="bc48d-128">Om hello namnområde inte hittas skapar hello namnområde och hämtar hello nyligen skapade namnrymd.</span><span class="sxs-lookup"><span data-stu-id="bc48d-128">If hello namespace is not found, it creates hello namespace and then retrieves hello newly created namespace.</span></span>

    ```powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a><span data-ttu-id="bc48d-129">Skapa en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="bc48d-129">Create an event hub</span></span>

<span data-ttu-id="bc48d-130">toocreate en händelsehubb, kontrollera namnområdet med hjälp av hello skript i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bc48d-130">toocreate an event hub, perform a namespace check using hello script in hello previous section.</span></span> <span data-ttu-id="bc48d-131">Använd sedan hello [ny AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) cmdlet toocreate hello händelsehubb:</span><span class="sxs-lookup"><span data-stu-id="bc48d-131">Then, use hello [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) cmdlet toocreate hello event hub:</span></span>

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "hello event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $EventHubName event hub does not exist."
    Write-Host "Creating hello $EventHubName event hub in hello $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $EventHubName event hub in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a><span data-ttu-id="bc48d-132">Skapa en konsumentgrupp</span><span class="sxs-lookup"><span data-stu-id="bc48d-132">Create a consumer group</span></span>

<span data-ttu-id="bc48d-133">toocreate en konsumentgrupp i en händelsehubb, utföra hello namnområde och händelsen hubb kontroller med hjälp av hello skript i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bc48d-133">toocreate a consumer group within an event hub, perform hello namespace and event hub checks using hello scripts in hello previous section.</span></span> <span data-ttu-id="bc48d-134">Använd sedan hello [ny AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) cmdlet toocreate hello konsumentgrupp inom hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="bc48d-134">Then, use hello [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) cmdlet toocreate hello consumer group within hello event hub.</span></span> <span data-ttu-id="bc48d-135">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bc48d-135">For example:</span></span>

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "hello consumer group $ConsumerGroupName in event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating hello $ConsumerGroupName consumer group in hello $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a><span data-ttu-id="bc48d-136">Ange Användarmetadata</span><span class="sxs-lookup"><span data-stu-id="bc48d-136">Set user metadata</span></span>

<span data-ttu-id="bc48d-137">När hello skript körs i hello föregående avsnitt, kan du använda hello [Set AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet tooupdate hello egenskaperna för en konsumentgrupp, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="bc48d-137">After executing hello scripts in hello preceding sections, you can use hello [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet tooupdate hello properties of a consumer group, as in hello following example:</span></span>

```powershell
# Set some user metadata on hello CG
Write-Host "Setting hello UserMetadata field too'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a><span data-ttu-id="bc48d-138">Ta bort händelsehubb</span><span class="sxs-lookup"><span data-stu-id="bc48d-138">Remove event hub</span></span>

<span data-ttu-id="bc48d-139">tooremove hello händelsehubbar du har skapat, kan du använda hello `Remove-*` cmdlets, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="bc48d-139">tooremove hello event hubs you created, you can use hello `Remove-*` cmdlets, as in hello following example:</span></span>

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a><span data-ttu-id="bc48d-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc48d-140">Next steps</span></span>

- <span data-ttu-id="bc48d-141">Dokumentationen hello fullständig Event Hubs Resource Manager PowerShell-modulen [här](/powershell/module/azurerm.eventhub).</span><span class="sxs-lookup"><span data-stu-id="bc48d-141">See hello complete Event Hubs Resource Manager PowerShell module documentation [here](/powershell/module/azurerm.eventhub).</span></span> <span data-ttu-id="bc48d-142">Den här sidan visas alla tillgängliga cmdlet: ar.</span><span class="sxs-lookup"><span data-stu-id="bc48d-142">This page lists all available cmdlets.</span></span>
- <span data-ttu-id="bc48d-143">Information om hur du använder Azure Resource Manager-mallar finns hello artikel [skapa ett namnområde för Händelsehubbar med nav- och konsumenten händelsegruppen med en Azure Resource Manager-mall](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="bc48d-143">For information about using Azure Resource Manager templates, see hello article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>
- <span data-ttu-id="bc48d-144">Information om [bibliotek för Event Hubs .NET](event-hubs-management-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="bc48d-144">Information about [Event Hubs .NET management libraries](event-hubs-management-libraries.md).</span></span>

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
