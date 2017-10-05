---
title: "Kontrollera trafik med Azure Network Watcher IP flöda Kontrollera - Azure CLI | Microsoft Docs"
description: "Den här artikeln beskrivs hur du kontrollerar om trafik till eller från en virtuell dator tillåts eller nekas med Azure CLI"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: c5fe6c662b3ee2a443904b0f12cbfd495d9bc85e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="0d9da-103">Kontrollera om trafik tillåts eller nekas till eller från en virtuell dator med IP-flöda Kontrollera en komponent i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="0d9da-103">Check if traffic is allowed or denied to or from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="0d9da-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0d9da-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="0d9da-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d9da-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="0d9da-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="0d9da-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="0d9da-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0d9da-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="0d9da-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="0d9da-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="0d9da-109">IP-flöda verifiera är en funktion i Nätverksbevakaren som hjälper dig att kontrollera om tillåts trafik till eller från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0d9da-109">IP Flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="0d9da-110">Det här scenariot är användbart för att hämta aktuella tillstånd om en virtuell dator kan kommunicera med en extern resurs eller backend.</span><span class="sxs-lookup"><span data-stu-id="0d9da-110">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or backend.</span></span> <span data-ttu-id="0d9da-111">IP-flöde Kontrollera kan användas för att kontrollera om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="0d9da-111">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="0d9da-112">En annan orsak till att använda IP flödet Kontrollera är att kontrollera att trafik som du vill blockerade blockeras korrekt av NSG: N.</span><span class="sxs-lookup"><span data-stu-id="0d9da-112">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

<span data-ttu-id="0d9da-113">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="0d9da-113">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0d9da-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="0d9da-114">Before you begin</span></span>

<span data-ttu-id="0d9da-115">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="0d9da-115">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="0d9da-116">Det här scenariot förutsätter att det finns en resursgrupp med en giltig virtuell dator som ska användas.</span><span class="sxs-lookup"><span data-stu-id="0d9da-116">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="0d9da-117">Scenario</span><span class="sxs-lookup"><span data-stu-id="0d9da-117">Scenario</span></span>

<span data-ttu-id="0d9da-118">Det här scenariot använder IP-flöda Kontrollera för att verifiera om en virtuell dator kan du kontakta en känd Bing IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0d9da-118">This scenario uses IP Flow Verify to verify if a virtual machine can talk to a known Bing IP address.</span></span> <span data-ttu-id="0d9da-119">Om trafiken nekas returnerar säkerhetsregeln som nekar trafiken.</span><span class="sxs-lookup"><span data-stu-id="0d9da-119">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="0d9da-120">Läs mer om IP-flöda Kontrollera [flöda Kontrollera översikt över IP](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="0d9da-120">To learn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>


## <a name="get-a-vm"></a><span data-ttu-id="0d9da-121">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0d9da-121">Get a VM</span></span>

<span data-ttu-id="0d9da-122">IP-flöde verifiera tester trafik till eller från en IP-adress på en virtuell dator till eller från en extern enhet.</span><span class="sxs-lookup"><span data-stu-id="0d9da-122">IP flow verify tests traffic to or from an IP address on a virtual machine to or from a remote destination.</span></span> <span data-ttu-id="0d9da-123">Ett Id för en virtuell dator krävs för cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0d9da-123">An Id of a virtual machine is required for the cmdlet.</span></span> <span data-ttu-id="0d9da-124">Om du redan känner till ID: T för den virtuella datorn att använda, kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="0d9da-124">If you already know the ID of the virtual machine to use, you can skip this step.</span></span>

```
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="get-the-nics"></a><span data-ttu-id="0d9da-125">Hämta Nätverkskorten</span><span class="sxs-lookup"><span data-stu-id="0d9da-125">Get the NICS</span></span>

<span data-ttu-id="0d9da-126">IP-adressen för ett nätverkskort på den virtuella datorn krävs i det här exemplet vi hämta nätverkskort på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0d9da-126">The IP address of a NIC on the virtual machine is needed, in this example we retrieve the NICs on a virtual machine.</span></span> <span data-ttu-id="0d9da-127">Om du redan känner till IP-adressen som du vill testa på den virtuella datorn, kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="0d9da-127">If you already know the IP address that you want to test on the virtual machine, you can skip this step.</span></span>

```
azure network nic show -g resourceGroupName -n nicName
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="0d9da-128">Kontrollera kör IP-flöde</span><span class="sxs-lookup"><span data-stu-id="0d9da-128">Run IP flow verify</span></span>

<span data-ttu-id="0d9da-129">Nu när vi har information som behövs för att köra cmdlet vi kör den `network watcher ip-flow-verify` för att testa trafiken.</span><span class="sxs-lookup"><span data-stu-id="0d9da-129">Now that we have the information needed to run the cmdlet, we run the `network watcher ip-flow-verify` cmdlet to test the traffic.</span></span> <span data-ttu-id="0d9da-130">I det här exemplet använder du den första IP-adressen på det första nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="0d9da-130">In this example, we are using the first IP address on the first NIC.</span></span>

```
azure network watcher ip-flow-verify -g resourceGroupName -n networkWatcherName -t targetResourceId -d directionInboundorOutbound -p protocolTCPorUDP -o localPort -m remotePort -l localIpAddr -r remoteIpAddr
```

> [!NOTE]
> <span data-ttu-id="0d9da-131">IP-flöda Kontrollera kräver att den Virtuella datorresursen har allokerats för att köras.</span><span class="sxs-lookup"><span data-stu-id="0d9da-131">IP Flow verify requires that the VM resource is allocated to run.</span></span>

## <a name="review-results"></a><span data-ttu-id="0d9da-132">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="0d9da-132">Review Results</span></span>

<span data-ttu-id="0d9da-133">När du har kört `network watcher ip-flow-verify` resultaten returneras, i följande exempel är resultatet har returnerats från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="0d9da-133">After running `network watcher ip-flow-verify` the results are returned, the following example is the results returned from the preceding step.</span></span>

```
data:    Access                          : Deny
data:    Rule Name                       : defaultSecurityRules/DefaultInboundDenyAll
info:    network watcher ip-flow-verify command OK
```

## <a name="next-steps"></a><span data-ttu-id="0d9da-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d9da-134">Next steps</span></span>

<span data-ttu-id="0d9da-135">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) att spåra de grupp och säkerhet Nätverkssäkerhetsregler som har definierats.</span><span class="sxs-lookup"><span data-stu-id="0d9da-135">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are defined.</span></span>

<span data-ttu-id="0d9da-136">Lär dig hur du granskningsinställningar NSG genom att besöka [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="0d9da-136">Learn to audit your NSG settings by visiting [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md).</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
