---
title: Kontrollera trafik med Azure Network Watcher IP verifiera - REST | Microsoft Docs
description: "Den här artikeln beskrivs hur du kontrollerar om trafik till eller från en virtuell dator tillåts eller nekas"
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
ms.openlocfilehash: 6d3ce00a7d4f9c0cd57fa8815625a1065b03b5b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="99725-103">Kontrollera om trafik tillåts eller nekas med IP-flöde verifiera en komponent i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="99725-103">Check if traffic is allowed or denied with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="99725-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="99725-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="99725-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="99725-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="99725-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="99725-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="99725-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="99725-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="99725-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="99725-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="99725-109">IP-flöde verifiera är en funktion i Nätverksbevakaren som hjälper dig att kontrollera om tillåts trafik till eller från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="99725-109">IP flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="99725-110">Verifieringen kan köras för inkommande eller utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="99725-110">The validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="99725-111">Det här scenariot är användbart för att hämta aktuella tillstånd om en virtuell dator kan kommunicera med en extern resurs eller backend.</span><span class="sxs-lookup"><span data-stu-id="99725-111">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or backend.</span></span> <span data-ttu-id="99725-112">IP-flöde Kontrollera kan användas för att kontrollera om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="99725-112">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="99725-113">En annan orsak till att använda IP flödet Kontrollera är att kontrollera att trafik som du vill blockerade blockeras korrekt av NSG: N.</span><span class="sxs-lookup"><span data-stu-id="99725-113">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="99725-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="99725-114">Before you begin</span></span>

<span data-ttu-id="99725-115">ARMclient används för att anropa REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="99725-115">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="99725-116">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="99725-116">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="99725-117">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="99725-117">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="99725-118">Scenario</span><span class="sxs-lookup"><span data-stu-id="99725-118">Scenario</span></span>

<span data-ttu-id="99725-119">Det här scenariot använder IP-flöde Kontrollera för att verifiera om en virtuell dator kan du kontakta en annan dator via port 443.</span><span class="sxs-lookup"><span data-stu-id="99725-119">This scenario uses IP flow Verify to verify if a virtual machine can talk to another machine over port 443.</span></span> <span data-ttu-id="99725-120">Om trafiken nekas returnerar säkerhetsregeln som nekar trafiken.</span><span class="sxs-lookup"><span data-stu-id="99725-120">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="99725-121">Läs mer om IP-flöde Kontrollera [IP-flöde Kontrollera översikt](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="99725-121">To learn more about IP flow Verify, visit [IP flow verify overview](network-watcher-ip-flow-verify-overview.md)</span></span>

<span data-ttu-id="99725-122">I det här scenariot kan du:</span><span class="sxs-lookup"><span data-stu-id="99725-122">In this scenario, you:</span></span>

* <span data-ttu-id="99725-123">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="99725-123">Retrieve a virtual machine</span></span>
* <span data-ttu-id="99725-124">Verifiera anropet IP-flöde</span><span class="sxs-lookup"><span data-stu-id="99725-124">Call IP flow verify</span></span>
* <span data-ttu-id="99725-125">Verifiera resultaten</span><span class="sxs-lookup"><span data-stu-id="99725-125">Verify results</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="99725-126">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="99725-126">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="99725-127">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="99725-127">Retrieve a virtual machine</span></span>

<span data-ttu-id="99725-128">Kör följande skript för att returnera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="99725-128">Run the following script to return a virtual machine.</span></span> <span data-ttu-id="99725-129">Följande kod måste värdena för variabler:</span><span class="sxs-lookup"><span data-stu-id="99725-129">The following code needs values for the variables:</span></span>

* <span data-ttu-id="99725-130">**subscriptionId** -prenumerations-Id ska användas.</span><span class="sxs-lookup"><span data-stu-id="99725-130">**subscriptionId** - The subscription Id to use.</span></span>
* <span data-ttu-id="99725-131">**resourceGroupName** -namnet på en resursgrupp som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="99725-131">**resourceGroupName** - The name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="99725-132">Den information som behövs är id under typen `Microsoft.Compute/virtualMachines`.</span><span class="sxs-lookup"><span data-stu-id="99725-132">The information that is needed is the id under the type `Microsoft.Compute/virtualMachines`.</span></span> <span data-ttu-id="99725-133">Resultatet bör likna följande kodexempel:</span><span class="sxs-lookup"><span data-stu-id="99725-133">The results should be similar to the following code sample:</span></span>

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

## <a name="call-ip-flow-verify"></a><span data-ttu-id="99725-134">Anropa IP-flöde Kontrollera</span><span class="sxs-lookup"><span data-stu-id="99725-134">Call IP flow Verify</span></span>

<span data-ttu-id="99725-135">I följande exempel skapas en begäran om att kontrollera trafik för en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="99725-135">The following example creates a request to verify the traffic for a specified virtual machine.</span></span> <span data-ttu-id="99725-136">Svaret returnerar om trafiken tillåts eller nekas trafiken.</span><span class="sxs-lookup"><span data-stu-id="99725-136">The response returns if the traffic is allowed or if the traffic is denied.</span></span> <span data-ttu-id="99725-137">Om trafik nekas den returnerar även vilken regel som blockerar trafiken.</span><span class="sxs-lookup"><span data-stu-id="99725-137">If traffic is denied it also returns what rule blocks the traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="99725-138">IP-flöde Kontrollera kräver att den Virtuella datorresursen allokeras.</span><span class="sxs-lookup"><span data-stu-id="99725-138">IP flow verify requires that the VM resource is allocated.</span></span>

<span data-ttu-id="99725-139">Skriptet kräver resursens Id för en virtuell dator och ett nätverkskort på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99725-139">The script requires the resource Id of a virtual machine and of a network interface card on the virtual machine.</span></span> <span data-ttu-id="99725-140">Dessa värden tillhandahålls av föregående utdata.</span><span class="sxs-lookup"><span data-stu-id="99725-140">These values are provided by the preceding output.</span></span>

> [!Important]
> <span data-ttu-id="99725-141">För alla nätverk Watcher REST-anrop är det som innehåller Nätverksbevakaren-instans, inte de resurser som du utför de diagnostiska åtgärderna på resursgruppens namn i URI-begäran.</span><span class="sxs-lookup"><span data-stu-id="99725-141">For all Network Watcher REST calls the resource group name in the request URI is the one that contains the Network Watcher instance, not the resources you are performing the diagnostic actions on.</span></span>

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

## <a name="understanding-the-results"></a><span data-ttu-id="99725-142">Förstå resultaten</span><span class="sxs-lookup"><span data-stu-id="99725-142">Understanding the results</span></span>

<span data-ttu-id="99725-143">Svaret åter anger om trafik tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="99725-143">The response you get back tells you whether the traffic is allowed or denied.</span></span> <span data-ttu-id="99725-144">Svaret ser ut som följande exempel:</span><span class="sxs-lookup"><span data-stu-id="99725-144">The response looks like one of the following examples:</span></span>

<span data-ttu-id="99725-145">**Tillåtna**</span><span class="sxs-lookup"><span data-stu-id="99725-145">**Allowed**</span></span>

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

<span data-ttu-id="99725-146">**Nekad**</span><span class="sxs-lookup"><span data-stu-id="99725-146">**Denied**</span></span>

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a><span data-ttu-id="99725-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99725-147">Next steps</span></span>

<span data-ttu-id="99725-148">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) lära dig mer om Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="99725-148">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to learn more about Network Security Groups.</span></span>












