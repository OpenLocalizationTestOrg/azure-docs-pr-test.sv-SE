---
title: "aaaVerify trafik med Azure Network Watcher IP flöda Kontrollera - Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur toocheck om trafik tooor från en virtuell dator tillåts eller nekas med Azure CLI"
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
ms.openlocfilehash: c6becc5c142837b04d15490b2b3bd11124434570
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="974b5-103">Kontrollera om trafik som tillåts eller nekas tooor från en virtuell dator med IP-flöda Kontrollera en komponent i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="974b5-103">Check if traffic is allowed or denied tooor from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="974b5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="974b5-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="974b5-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="974b5-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="974b5-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="974b5-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="974b5-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="974b5-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="974b5-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="974b5-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="974b5-109">IP-flöda verifiera är en funktion i Nätverksbevakaren som gör att du tooverify om trafik tillåts tooor från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="974b5-109">IP Flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="974b5-110">Det här scenariot är användbart tooget aktuella tillstånd om en virtuell dator kan prata tooan extern resurs eller backend.</span><span class="sxs-lookup"><span data-stu-id="974b5-110">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="974b5-111">IP-flöde verifiera går att använda tooverify om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="974b5-111">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="974b5-112">En annan orsak till att använda IP flödet Kontrollera tooensure trafik som du vill blockerade blockeras korrekt av hello NSG.</span><span class="sxs-lookup"><span data-stu-id="974b5-112">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

<span data-ttu-id="974b5-113">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="974b5-113">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="974b5-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="974b5-114">Before you begin</span></span>

<span data-ttu-id="974b5-115">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="974b5-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="974b5-116">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="974b5-116">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="974b5-117">Scenario</span><span class="sxs-lookup"><span data-stu-id="974b5-117">Scenario</span></span>

<span data-ttu-id="974b5-118">Det här scenariot använder IP-flöda Kontrollera tooverify om en virtuell dator kan prata tooa kända Bing IP-adress.</span><span class="sxs-lookup"><span data-stu-id="974b5-118">This scenario uses IP Flow Verify tooverify if a virtual machine can talk tooa known Bing IP address.</span></span> <span data-ttu-id="974b5-119">Om hello trafik nekas returnerar hello säkerhetsregel som nekar trafiken.</span><span class="sxs-lookup"><span data-stu-id="974b5-119">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="974b5-120">toolearn mer om IP-flöda Kontrollera finns [flöda Kontrollera översikt över IP](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="974b5-120">toolearn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>


## <a name="get-a-vm"></a><span data-ttu-id="974b5-121">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="974b5-121">Get a VM</span></span>

<span data-ttu-id="974b5-122">IP-flöde verifiera tester trafik tooor från en IP-adress på en virtuell dator tooor från en extern enhet.</span><span class="sxs-lookup"><span data-stu-id="974b5-122">IP flow verify tests traffic tooor from an IP address on a virtual machine tooor from a remote destination.</span></span> <span data-ttu-id="974b5-123">Ett Id för en virtuell dator krävs för hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="974b5-123">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="974b5-124">Om du redan vet hello-ID för hello virtuella toouse kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="974b5-124">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="get-hello-nics"></a><span data-ttu-id="974b5-125">Hämta hello nätverkskort</span><span class="sxs-lookup"><span data-stu-id="974b5-125">Get hello NICS</span></span>

<span data-ttu-id="974b5-126">hello IP-adress till ett nätverkskort på den virtuella datorn hello krävs i det här exemplet vi hämta hello nätverkskort på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="974b5-126">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="974b5-127">Om du redan vet hello IP-adress som du vill tootest på hello virtuell dator kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="974b5-127">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```
azure network nic show -g resourceGroupName -n nicName
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="974b5-128">Kontrollera kör IP-flöde</span><span class="sxs-lookup"><span data-stu-id="974b5-128">Run IP flow verify</span></span>

<span data-ttu-id="974b5-129">Nu när vi har hello information behövs toorun hello cmdlet vi kör hello `network watcher ip-flow-verify` cmdlet tootest hello trafik.</span><span class="sxs-lookup"><span data-stu-id="974b5-129">Now that we have hello information needed toorun hello cmdlet, we run hello `network watcher ip-flow-verify` cmdlet tootest hello traffic.</span></span> <span data-ttu-id="974b5-130">I det här exemplet använder vi hello första IP-adressen på hello första nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="974b5-130">In this example, we are using hello first IP address on hello first NIC.</span></span>

```
azure network watcher ip-flow-verify -g resourceGroupName -n networkWatcherName -t targetResourceId -d directionInboundorOutbound -p protocolTCPorUDP -o localPort -m remotePort -l localIpAddr -r remoteIpAddr
```

> [!NOTE]
> <span data-ttu-id="974b5-131">IP-flöda Kontrollera kräver att hello Virtuella datorresursen fördelas toorun.</span><span class="sxs-lookup"><span data-stu-id="974b5-131">IP Flow verify requires that hello VM resource is allocated toorun.</span></span>

## <a name="review-results"></a><span data-ttu-id="974b5-132">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="974b5-132">Review Results</span></span>

<span data-ttu-id="974b5-133">När du har kört `network watcher ip-flow-verify` hello resultaten returneras, hello följande exempel är hello resultaten som returnerades från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="974b5-133">After running `network watcher ip-flow-verify` hello results are returned, hello following example is hello results returned from hello preceding step.</span></span>

```
data:    Access                          : Deny
data:    Rule Name                       : defaultSecurityRules/DefaultInboundDenyAll
info:    network watcher ip-flow-verify command OK
```

## <a name="next-steps"></a><span data-ttu-id="974b5-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="974b5-134">Next steps</span></span>

<span data-ttu-id="974b5-135">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som har definierats.</span><span class="sxs-lookup"><span data-stu-id="974b5-135">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

<span data-ttu-id="974b5-136">Läs tooaudit NSG-inställningarna genom att besöka [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="974b5-136">Learn tooaudit your NSG settings by visiting [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md).</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
