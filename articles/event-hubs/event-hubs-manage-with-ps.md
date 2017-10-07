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
# <a name="use-powershell-toomanage-event-hubs-resources"></a>Använd PowerShell toomanage Händelsehubbar resurser

Microsoft Azure PowerShell är en skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av Azure-tjänster. Den här artikeln beskriver hur toouse hello [Event Hubs Resource Manager PowerShell-modulen](/powershell/module/azurerm.eventhub) tooprovision och hantera Händelsehubbar entiteter (namnområden, enskilda händelsehubbar och konsumentgrupper) med hjälp av den lokala Azure PowerShell-konsolen eller skript.

Du kan också hantera Händelsehubbar resurser med hjälp av Azure Resource Manager-mallar. Mer information finns i artikeln hello [skapa ett namnområde för Händelsehubbar med nav- och konsumenten händelsegruppen med en Azure Resource Manager-mall](event-hubs-resource-manager-namespace-event-hub.md).

## <a name="prerequisites"></a>Krav

Innan du börjar behöver du hello följande:

* En Azure-prenumeration. Mer information om hur du skaffar en prenumeration finns [köpalternativ][purchase options], [medlemserbjudanden][member offers], eller [kostnadsfritt konto][free account].
* En dator med Azure PowerShell. Instruktioner finns i [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps).
* En förståelse av PowerShell-skript, NuGet-paket och hello .NET Framework.

## <a name="get-started"></a>Kom igång

hello första steget är toouse PowerShell toolog i tooyour Azure-konto och Azure-prenumeration. Följ anvisningarna för hello i [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps) toolog i tooyour Azure-konto sedan hämta och komma åt hello resurser i din Azure-prenumeration.

## <a name="provision-an-event-hubs-namespace"></a>Etablera ett namnområde för Händelsehubbar

När du arbetar med Händelsehubbar namnområden kan du använda hello [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [ny AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [ta bort AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace) , och [Set AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlets.

Det här exemplet skapar några lokala variabler i hello skript; `$Namespace` och `$Location`.

* `$Namespace`är hello Händelsehubbar namnområde som vi vill toowork hello namn.
* `$Location`identifierar hello datacenter där vi etablerar hello namnområde.
* `$CurrentNamespace`lagrar hello referens namnområde som vi hämta (eller skapa).

I en verklig skript `$Namespace` och `$Location` kan skickas som parametrar.

Den här delen av hello skript hello följande:

1. Försök tooretrieve ett Händelsehubbar namnområde med hello angett namn.
2. Om hello namnområde hittas rapporterar vad hittades.
3. Om hello namnområde inte hittas skapar hello namnområde och hämtar hello nyligen skapade namnrymd.

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

## <a name="create-an-event-hub"></a>Skapa en händelsehubb

toocreate en händelsehubb, kontrollera namnområdet med hjälp av hello skript i hello föregående avsnitt. Använd sedan hello [ny AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) cmdlet toocreate hello händelsehubb:

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

### <a name="create-a-consumer-group"></a>Skapa en konsumentgrupp

toocreate en konsumentgrupp i en händelsehubb, utföra hello namnområde och händelsen hubb kontroller med hjälp av hello skript i hello föregående avsnitt. Använd sedan hello [ny AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) cmdlet toocreate hello konsumentgrupp inom hello händelsehubb. Exempel:

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

#### <a name="set-user-metadata"></a>Ange Användarmetadata

När hello skript körs i hello föregående avsnitt, kan du använda hello [Set AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet tooupdate hello egenskaperna för en konsumentgrupp, som i följande exempel hello:

```powershell
# Set some user metadata on hello CG
Write-Host "Setting hello UserMetadata field too'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a>Ta bort händelsehubb

tooremove hello händelsehubbar du har skapat, kan du använda hello `Remove-*` cmdlets, som i följande exempel hello:

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a>Nästa steg

- Dokumentationen hello fullständig Event Hubs Resource Manager PowerShell-modulen [här](/powershell/module/azurerm.eventhub). Den här sidan visas alla tillgängliga cmdlet: ar.
- Information om hur du använder Azure Resource Manager-mallar finns hello artikel [skapa ett namnområde för Händelsehubbar med nav- och konsumenten händelsegruppen med en Azure Resource Manager-mall](event-hubs-resource-manager-namespace-event-hub.md).
- Information om [bibliotek för Event Hubs .NET](event-hubs-management-libraries.md).

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
