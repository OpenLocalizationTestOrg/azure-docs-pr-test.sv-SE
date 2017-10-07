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
# <a name="use-powershell-toomanage-service-bus-resources"></a>Använd PowerShell toomanage Service Bus-resurser

Microsoft Azure PowerShell är en skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av Azure-tjänster. Den här artikeln beskriver hur toouse hello [Service Bus Resource Manager PowerShell-modulen](/powershell/module/azurerm.servicebus) tooprovision och hantera Service Bus-entiteter (namnområden, köer, ämnen och prenumerationer) med hjälp av den lokala Azure PowerShell-konsolen eller skript.

Du kan också hantera Service Bus-entiteter med hjälp av Azure Resource Manager-mallar. Mer information finns i artikeln hello [skapa Service Bus-resurser med hjälp av Azure Resource Manager-mallar](service-bus-resource-manager-overview.md).

## <a name="prerequisites"></a>Krav

Innan du börjar behöver du hello följande:

* En Azure-prenumeration. Mer information om hur du skaffar en prenumeration finns [köpalternativ][purchase options], [medlemserbjudanden][member offers], eller [kostnadsfritt konto][free account].
* En dator med Azure PowerShell. Instruktioner finns i [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps).
* En förståelse av PowerShell-skript, NuGet-paket och hello .NET Framework.

## <a name="get-started"></a>Kom igång

hello första steget är toouse PowerShell toolog i tooyour Azure-konto och Azure-prenumeration. Följ anvisningarna för hello i [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps) toolog i tooyour Azure-konto och hämta och komma åt hello resurser i din Azure-prenumeration.

## <a name="provision-a-service-bus-namespace"></a>Etablera ett Service Bus-namnområde

När du arbetar med Service Bus-namnområden kan du använda hello [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [ny AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), och [Set AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) cmdlets.

Det här exemplet skapar några lokala variabler i hello skript; `$Namespace` och `$Location`.

* `$Namespace`är hello Service Bus-namnrymd som vi vill toowork hello namn.
* `$Location`identifierar hello datacenter där vi etablerar hello namnområde.
* `$CurrentNamespace`lagrar hello referens namnområde som vi hämta (eller skapa).

I en verklig skript `$Namespace` och `$Location` kan skickas som parametrar.

Den här delen av hello skript hello följande:

1. Försök tooretrieve en Service Bus-namnrymd med hello angett namn.
2. Om hello namnområde hittas rapporterar vad hittades.
3. Om hello namnområde inte hittas skapar hello namnområde och hämtar hello nyligen skapade namnrymd.
   
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

### <a name="create-a-namespace-authorization-rule"></a>Skapa en regel för auktorisering av namnområdet

hello följande exempel visas hur toomanage namnområde auktoriseringsregler med hello [ny AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), och [ta bort AzureRmServiceBusNamespaceAuthorizationRule cmdlets](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).

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

## <a name="create-a-queue"></a>Skapa en kö

toocreate en kö eller ett ämne, kontrollera namnområdet med hjälp av hello skript i hello föregående avsnitt. Sedan skapa hello kö:

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

### <a name="modify-queue-properties"></a>Ändra egenskaper för kön

Du kan använda hello efter körning hello skriptet i föregående avsnitt hello [Set AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) cmdlet tooupdate hello egenskaperna för en kö, som i följande exempel hello:

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>Etablerar andra Service Bus-entiteter

Du kan använda hello [Service Bus PowerShell-modulen](/powershell/module/azurerm.servicebus) tooprovision andra entiteter, till exempel ämnen och prenumerationer. Dessa cmdlets finns kön skapas av syntaktiskt liknande toohello-cmdlets som visas i hello föregående avsnitt.

## <a name="next-steps"></a>Nästa steg

- Dokumentationen hello fullständig Service Bus Resource Manager PowerShell-modulen [här](/powershell/module/azurerm.servicebus). Den här sidan visas alla tillgängliga cmdlet: ar.
- Information om hur du använder Azure Resource Manager-mallar finns hello artikel [skapa Service Bus-resurser med hjälp av Azure Resource Manager-mallar](service-bus-resource-manager-overview.md).
- Information om [bibliotek för Service Bus .NET](service-bus-management-libraries.md).

Det finns några alternativa sätt toomanage Service Bus-entiteter, enligt beskrivningen i följande blogginlägg:

* [Hur toocreate Service Bus-köer, ämnen och prenumerationer med hjälp av ett PowerShell-skript](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Hur toocreate en Service Bus Namespace och en Händelsehubb med hjälp av ett PowerShell-skript](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [Service Bus PowerShell-skript](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
