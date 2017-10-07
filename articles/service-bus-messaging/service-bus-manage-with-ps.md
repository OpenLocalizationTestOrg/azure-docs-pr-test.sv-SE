---
title: aaaUse PowerShell toomanage Azure Service Bus-resurser | Microsoft Docs
description: "Använda PowerShell-modulen toocreate och hantera Service Bus-resurser"
services: service-bus-messaging
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/06/2017
ms.author: sethm
ms.openlocfilehash: 737044def913c5798e7e05fc4f1aeece76c8f4dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-service-bus-resources"></a><span data-ttu-id="c5776-103">Använd PowerShell toomanage Service Bus-resurser</span><span class="sxs-lookup"><span data-stu-id="c5776-103">Use PowerShell toomanage Service Bus resources</span></span>

<span data-ttu-id="c5776-104">Microsoft Azure PowerShell är en skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="c5776-104">Microsoft Azure PowerShell is a scripting environment that you can use toocontrol and automate hello deployment and management of Azure services.</span></span> <span data-ttu-id="c5776-105">Den här artikeln beskriver hur toouse hello [Service Bus Resource Manager PowerShell-modulen](/powershell/module/azurerm.servicebus) tooprovision och hantera Service Bus-entiteter (namnområden, köer, ämnen och prenumerationer) med hjälp av den lokala Azure PowerShell-konsolen eller skript.</span><span class="sxs-lookup"><span data-stu-id="c5776-105">This article describes how toouse hello [Service Bus Resource Manager PowerShell module](/powershell/module/azurerm.servicebus) tooprovision and manage Service Bus entities (namespaces, queues, topics, and subscriptions) using a local Azure PowerShell console or script.</span></span>

<span data-ttu-id="c5776-106">Du kan också hantera Service Bus-entiteter med hjälp av Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="c5776-106">You can also manage Service Bus entities using Azure Resource Manager templates.</span></span> <span data-ttu-id="c5776-107">Mer information finns i artikeln hello [skapa Service Bus-resurser med hjälp av Azure Resource Manager-mallar](service-bus-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c5776-107">For more information, see hello article [Create Service Bus resources using Azure Resource Manager templates](service-bus-resource-manager-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5776-108">Krav</span><span class="sxs-lookup"><span data-stu-id="c5776-108">Prerequisites</span></span>

<span data-ttu-id="c5776-109">Innan du börjar behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="c5776-109">Before you begin, you'll need hello following:</span></span>

* <span data-ttu-id="c5776-110">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5776-110">An Azure subscription.</span></span> <span data-ttu-id="c5776-111">Mer information om hur du skaffar en prenumeration finns [köpalternativ][purchase options], [medlemserbjudanden][member offers], eller [kostnadsfritt konto][free account].</span><span class="sxs-lookup"><span data-stu-id="c5776-111">For more information about obtaining a subscription, see [purchase options][purchase options], [member offers][member offers], or [free account][free account].</span></span>
* <span data-ttu-id="c5776-112">En dator med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5776-112">A computer with Azure PowerShell.</span></span> <span data-ttu-id="c5776-113">Instruktioner finns i [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="c5776-113">For instructions, see [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps).</span></span>
* <span data-ttu-id="c5776-114">En förståelse av PowerShell-skript, NuGet-paket och hello .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c5776-114">A general understanding of PowerShell scripts, NuGet packages, and hello .NET Framework.</span></span>

## <a name="get-started"></a><span data-ttu-id="c5776-115">Kom igång</span><span class="sxs-lookup"><span data-stu-id="c5776-115">Get started</span></span>

<span data-ttu-id="c5776-116">hello första steget är toouse PowerShell toolog i tooyour Azure-konto och Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5776-116">hello first step is toouse PowerShell toolog in tooyour Azure account and Azure subscription.</span></span> <span data-ttu-id="c5776-117">Följ anvisningarna för hello i [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps) toolog i tooyour Azure-konto och hämta och komma åt hello resurser i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5776-117">Follow hello instructions in [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps) toolog in tooyour Azure account, and retrieve and access hello resources in your Azure subscription.</span></span>

## <a name="provision-a-service-bus-namespace"></a><span data-ttu-id="c5776-118">Etablera ett Service Bus-namnområde</span><span class="sxs-lookup"><span data-stu-id="c5776-118">Provision a Service Bus namespace</span></span>

<span data-ttu-id="c5776-119">När du arbetar med Service Bus-namnområden kan du använda hello [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [ny AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), och [Set AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) cmdlets.</span><span class="sxs-lookup"><span data-stu-id="c5776-119">When working with Service Bus namespaces, you can use hello [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), and [Set-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) cmdlets.</span></span>

<span data-ttu-id="c5776-120">Det här exemplet skapar några lokala variabler i hello skript; `$Namespace` och `$Location`.</span><span class="sxs-lookup"><span data-stu-id="c5776-120">This example creates a few local variables in hello script; `$Namespace` and `$Location`.</span></span>

* <span data-ttu-id="c5776-121">`$Namespace`är hello Service Bus-namnrymd som vi vill toowork hello namn.</span><span class="sxs-lookup"><span data-stu-id="c5776-121">`$Namespace` is hello name of hello Service Bus namespace with which we want toowork.</span></span>
* <span data-ttu-id="c5776-122">`$Location`identifierar hello datacenter där vi etablerar hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="c5776-122">`$Location` identifies hello data center in which will we provision hello namespace.</span></span>
* <span data-ttu-id="c5776-123">`$CurrentNamespace`lagrar hello referens namnområde som vi hämta (eller skapa).</span><span class="sxs-lookup"><span data-stu-id="c5776-123">`$CurrentNamespace` stores hello reference namespace that we retrieve (or create).</span></span>

<span data-ttu-id="c5776-124">I en verklig skript `$Namespace` och `$Location` kan skickas som parametrar.</span><span class="sxs-lookup"><span data-stu-id="c5776-124">In an actual script, `$Namespace` and `$Location` can be passed as parameters.</span></span>

<span data-ttu-id="c5776-125">Den här delen av hello skript hello följande:</span><span class="sxs-lookup"><span data-stu-id="c5776-125">This part of hello script does hello following:</span></span>

1. <span data-ttu-id="c5776-126">Försök tooretrieve en Service Bus-namnrymd med hello angett namn.</span><span class="sxs-lookup"><span data-stu-id="c5776-126">Attempts tooretrieve a Service Bus namespace with hello specified name.</span></span>
2. <span data-ttu-id="c5776-127">Om hello namnområde hittas rapporterar vad hittades.</span><span class="sxs-lookup"><span data-stu-id="c5776-127">If hello namespace is found, it reports what was found.</span></span>
3. <span data-ttu-id="c5776-128">Om hello namnområde inte hittas skapar hello namnområde och hämtar hello nyligen skapade namnrymd.</span><span class="sxs-lookup"><span data-stu-id="c5776-128">If hello namespace is not found, it creates hello namespace and then retrieves hello newly created namespace.</span></span>
   
    ``` powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a><span data-ttu-id="c5776-129">Skapa en regel för auktorisering av namnområdet</span><span class="sxs-lookup"><span data-stu-id="c5776-129">Create a namespace authorization rule</span></span>

<span data-ttu-id="c5776-130">hello följande exempel visas hur toomanage namnområde auktoriseringsregler med hello [ny AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), och [ta bort AzureRmServiceBusNamespaceAuthorizationRule cmdlets](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).</span><span class="sxs-lookup"><span data-stu-id="c5776-130">hello following example shows how toomanage namespace authorization rules using hello [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), and [Remove-AzureRmServiceBusNamespaceAuthorizationRule cmdlets](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).</span></span>

```powershell
# Query toosee if rule exists
$CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if hello rule already exists or needs toobe created
if ($CurrentRule)
{
    Write-Host "hello $AuthRule rule already exists for hello namespace $Namespace."
}
else
{
    Write-Host "hello $AuthRule rule does not exist."
    Write-Host "Creating hello $AuthRule rule for hello $Namespace namespace..."
    New-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "hello $AuthRule rule for hello $Namespace namespace has been successfully created."

    Write-Host "Setting rights on hello namespace"
    $authRuleObj = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights toohello namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzureRmServiceBusNamespaceKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
}
```

## <a name="create-a-queue"></a><span data-ttu-id="c5776-131">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="c5776-131">Create a queue</span></span>

<span data-ttu-id="c5776-132">toocreate en kö eller ett ämne, kontrollera namnområdet med hjälp av hello skript i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c5776-132">toocreate a queue or topic, perform a namespace check using hello script in hello previous section.</span></span> <span data-ttu-id="c5776-133">Sedan skapa hello kö:</span><span class="sxs-lookup"><span data-stu-id="c5776-133">Then, create hello queue:</span></span>

```powershell
# Check if queue already exists
$CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "hello queue $QueueName already exists in hello $Location region:"
}
else
{
    Write-Host "hello $QueueName queue does not exist."
    Write-Host "Creating hello $QueueName queue in hello $Location region..."
    New-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "hello $QueueName queue in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a><span data-ttu-id="c5776-134">Ändra egenskaper för kön</span><span class="sxs-lookup"><span data-stu-id="c5776-134">Modify queue properties</span></span>

<span data-ttu-id="c5776-135">Du kan använda hello efter körning hello skriptet i föregående avsnitt hello [Set AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) cmdlet tooupdate hello egenskaperna för en kö, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="c5776-135">After executing hello script in hello preceding section, you can use hello [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) cmdlet tooupdate hello properties of a queue, as in hello following example:</span></span>

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a><span data-ttu-id="c5776-136">Etablerar andra Service Bus-entiteter</span><span class="sxs-lookup"><span data-stu-id="c5776-136">Provisioning other Service Bus entities</span></span>

<span data-ttu-id="c5776-137">Du kan använda hello [Service Bus PowerShell-modulen](/powershell/module/azurerm.servicebus) tooprovision andra entiteter, till exempel ämnen och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c5776-137">You can use hello [Service Bus PowerShell module](/powershell/module/azurerm.servicebus) tooprovision other entities, such as topics and subscriptions.</span></span> <span data-ttu-id="c5776-138">Dessa cmdlets finns kön skapas av syntaktiskt liknande toohello-cmdlets som visas i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c5776-138">These cmdlets are syntactically similar toohello queue creation cmdlets demonstrated in hello previous section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5776-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5776-139">Next steps</span></span>

- <span data-ttu-id="c5776-140">Dokumentationen hello fullständig Service Bus Resource Manager PowerShell-modulen [här](/powershell/module/azurerm.servicebus).</span><span class="sxs-lookup"><span data-stu-id="c5776-140">See hello complete Service Bus Resource Manager PowerShell module documentation [here](/powershell/module/azurerm.servicebus).</span></span> <span data-ttu-id="c5776-141">Den här sidan visas alla tillgängliga cmdlet: ar.</span><span class="sxs-lookup"><span data-stu-id="c5776-141">This page lists all available cmdlets.</span></span>
- <span data-ttu-id="c5776-142">Information om hur du använder Azure Resource Manager-mallar finns hello artikel [skapa Service Bus-resurser med hjälp av Azure Resource Manager-mallar](service-bus-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c5776-142">For information about using Azure Resource Manager templates, see hello article [Create Service Bus resources using Azure Resource Manager templates](service-bus-resource-manager-overview.md).</span></span>
- <span data-ttu-id="c5776-143">Information om [bibliotek för Service Bus .NET](service-bus-management-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="c5776-143">Information about [Service Bus .NET management libraries](service-bus-management-libraries.md).</span></span>

<span data-ttu-id="c5776-144">Det finns några alternativa sätt toomanage Service Bus-entiteter, enligt beskrivningen i följande blogginlägg:</span><span class="sxs-lookup"><span data-stu-id="c5776-144">There are some alternate ways toomanage Service Bus entities, as described in these blog posts:</span></span>

* [<span data-ttu-id="c5776-145">Hur toocreate Service Bus-köer, ämnen och prenumerationer med hjälp av ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="c5776-145">How toocreate Service Bus queues, topics and subscriptions using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [<span data-ttu-id="c5776-146">Hur toocreate en Service Bus Namespace och en Händelsehubb med hjälp av ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="c5776-146">How toocreate a Service Bus Namespace and an Event Hub using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [<span data-ttu-id="c5776-147">Service Bus PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="c5776-147">Service Bus PowerShell Scripts</span></span>](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
