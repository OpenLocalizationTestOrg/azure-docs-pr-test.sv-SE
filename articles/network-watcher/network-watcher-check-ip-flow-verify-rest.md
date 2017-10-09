---
title: Kontrollera aaaVerify trafik med Azure Network Watcher IP - REST | Microsoft Docs
description: "Den här artikeln beskriver hur toocheck om trafik tooor från en virtuell dator tillåts eller nekas"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3307a79f-03be-46a0-aaaf-b2079cb5f3b2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 956db0d326db597c6c402a9e8d4a5522c47c02d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="b4e39-103">Kontrollera om trafik tillåts eller nekas med IP-flöde verifiera en komponent i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="b4e39-103">Check if traffic is allowed or denied with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="b4e39-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b4e39-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="b4e39-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b4e39-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="b4e39-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b4e39-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="b4e39-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b4e39-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="b4e39-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="b4e39-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="b4e39-109">IP-flöde verifiera är en funktion i Nätverksbevakaren som gör att du tooverify om trafik tillåts tooor från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b4e39-109">IP flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="b4e39-110">hello verifiering kan köras för inkommande eller utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="b4e39-110">hello validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="b4e39-111">Det här scenariot är användbart tooget aktuella tillstånd om en virtuell dator kan prata tooan extern resurs eller backend.</span><span class="sxs-lookup"><span data-stu-id="b4e39-111">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="b4e39-112">IP-flöde verifiera går att använda tooverify om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="b4e39-112">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="b4e39-113">En annan orsak till att använda IP flödet Kontrollera tooensure trafik som du vill blockerade blockeras korrekt av hello NSG.</span><span class="sxs-lookup"><span data-stu-id="b4e39-113">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b4e39-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b4e39-114">Before you begin</span></span>

<span data-ttu-id="b4e39-115">ARMclient är används toocall hello REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b4e39-115">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="b4e39-116">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="b4e39-116">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="b4e39-117">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="b4e39-117">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="b4e39-118">Scenario</span><span class="sxs-lookup"><span data-stu-id="b4e39-118">Scenario</span></span>

<span data-ttu-id="b4e39-119">Det här scenariot använder IP-flöde Kontrollera tooverify om en virtuell dator kan prata tooanother datorn via port 443.</span><span class="sxs-lookup"><span data-stu-id="b4e39-119">This scenario uses IP flow Verify tooverify if a virtual machine can talk tooanother machine over port 443.</span></span> <span data-ttu-id="b4e39-120">Om hello trafik nekas returnerar hello säkerhetsregel som nekar trafiken.</span><span class="sxs-lookup"><span data-stu-id="b4e39-120">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="b4e39-121">toolearn mer om IP-flöde kontrollera, besök [IP-flöde Kontrollera översikt](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b4e39-121">toolearn more about IP flow Verify, visit [IP flow verify overview](network-watcher-ip-flow-verify-overview.md)</span></span>

<span data-ttu-id="b4e39-122">I det här scenariot kan du:</span><span class="sxs-lookup"><span data-stu-id="b4e39-122">In this scenario, you:</span></span>

* <span data-ttu-id="b4e39-123">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b4e39-123">Retrieve a virtual machine</span></span>
* <span data-ttu-id="b4e39-124">Verifiera anropet IP-flöde</span><span class="sxs-lookup"><span data-stu-id="b4e39-124">Call IP flow verify</span></span>
* <span data-ttu-id="b4e39-125">Verifiera resultaten</span><span class="sxs-lookup"><span data-stu-id="b4e39-125">Verify results</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="b4e39-126">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="b4e39-126">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="b4e39-127">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b4e39-127">Retrieve a virtual machine</span></span>

<span data-ttu-id="b4e39-128">Kör följande skript tooreturn hello en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b4e39-128">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="b4e39-129">hello följande kod måste ha värden för hello variabler:</span><span class="sxs-lookup"><span data-stu-id="b4e39-129">hello following code needs values for hello variables:</span></span>

* <span data-ttu-id="b4e39-130">**subscriptionId** -hello prenumerations-Id toouse.</span><span class="sxs-lookup"><span data-stu-id="b4e39-130">**subscriptionId** - hello subscription Id toouse.</span></span>
* <span data-ttu-id="b4e39-131">**resourceGroupName** - hello namn på en resursgrupp som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b4e39-131">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="b4e39-132">hello information som behövs är hello-id under hello typ `Microsoft.Compute/virtualMachines`.</span><span class="sxs-lookup"><span data-stu-id="b4e39-132">hello information that is needed is hello id under hello type `Microsoft.Compute/virtualMachines`.</span></span> <span data-ttu-id="b4e39-133">hello resultaten ska vara liknande toohello följande kodexempel:</span><span class="sxs-lookup"><span data-stu-id="b4e39-133">hello results should be similar toohello following code sample:</span></span>

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft
.Network/networkInterfaces/contosovm842"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Com
pute/virtualMachines/ContosoVM/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="call-ip-flow-verify"></a><span data-ttu-id="b4e39-134">Anropa IP-flöde Kontrollera</span><span class="sxs-lookup"><span data-stu-id="b4e39-134">Call IP flow Verify</span></span>

<span data-ttu-id="b4e39-135">hello skapas följande exempel en begäran tooverify hello-trafik för en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b4e39-135">hello following example creates a request tooverify hello traffic for a specified virtual machine.</span></span> <span data-ttu-id="b4e39-136">hello svar returnerar om hello trafik tillåts eller nekas hello trafik.</span><span class="sxs-lookup"><span data-stu-id="b4e39-136">hello response returns if hello traffic is allowed or if hello traffic is denied.</span></span> <span data-ttu-id="b4e39-137">Om trafik nekas den returnerar även vilka regeln block hello trafik.</span><span class="sxs-lookup"><span data-stu-id="b4e39-137">If traffic is denied it also returns what rule blocks hello traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="b4e39-138">IP-flöde Kontrollera kräver att hello Virtuella datorresursen allokeras.</span><span class="sxs-lookup"><span data-stu-id="b4e39-138">IP flow verify requires that hello VM resource is allocated.</span></span>

<span data-ttu-id="b4e39-139">hello skriptet kräver hello resursens Id för en virtuell dator och ett nätverkskort på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b4e39-139">hello script requires hello resource Id of a virtual machine and of a network interface card on hello virtual machine.</span></span> <span data-ttu-id="b4e39-140">Dessa värden tillhandahålls av hello föregående utdata.</span><span class="sxs-lookup"><span data-stu-id="b4e39-140">These values are provided by hello preceding output.</span></span>

> [!Important]
> <span data-ttu-id="b4e39-141">För alla nätverk Watcher REST-anrop hello resursgruppens namn i hello Begärd URI är hello en som innehåller hello Nätverksbevakaren instansen inte hello resurser som du utför hello diagnostiska åtgärder på.</span><span class="sxs-lookup"><span data-stu-id="b4e39-141">For all Network Watcher REST calls hello resource group name in hello request URI is hello one that contains hello Network Watcher instance, not hello resources you are performing hello diagnostic actions on.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$vmName = "<vm name>"
$vmNICName = "<vm NIC name>"
$direction = "<direction of traffic>" # Examples are: Inbound or Outbound
$localIP = "<source IP>"
$localPort = "<source Port>"
$remoteIP = "<destination IP>"
$remotePort = "<destination Port>" # Examples are: 80, or 80-120
$protocol = "<UDP, TCP or *>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.compute/virtualMachine/${vmName}
$targetNic = "<uri of target nic resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkInterfaces/${vmNICName}

$requestBody = @"
{
    'targetResourceId':  '$targetUri',
    'direction':  '$direction',
    'protocol':  '$protocol',
    'localPort':  '$localPort',
    'remotePort':  '$remotePort',
    'localIPAddress':  '$localIP',
    'remoteIPAddress':  '$remoteIP',
    'targetNICResourceId':  '$targetNic'
}
"@
        
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/ipFlowVerify?api-version=2016-12-01" $requestBody -verbose
```

## <a name="understanding-hello-results"></a><span data-ttu-id="b4e39-142">Förstå hello resultat</span><span class="sxs-lookup"><span data-stu-id="b4e39-142">Understanding hello results</span></span>

<span data-ttu-id="b4e39-143">hello svar åter berättar om hello trafik tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="b4e39-143">hello response you get back tells you whether hello traffic is allowed or denied.</span></span> <span data-ttu-id="b4e39-144">Det ser ut som något av följande exempel hello hello svar:</span><span class="sxs-lookup"><span data-stu-id="b4e39-144">hello response looks like one of hello following examples:</span></span>

<span data-ttu-id="b4e39-145">**Tillåtna**</span><span class="sxs-lookup"><span data-stu-id="b4e39-145">**Allowed**</span></span>

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

<span data-ttu-id="b4e39-146">**Nekad**</span><span class="sxs-lookup"><span data-stu-id="b4e39-146">**Denied**</span></span>

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a><span data-ttu-id="b4e39-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b4e39-147">Next steps</span></span>

<span data-ttu-id="b4e39-148">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) toolearn mer om Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="b4e39-148">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) toolearn more about Network Security Groups.</span></span>












