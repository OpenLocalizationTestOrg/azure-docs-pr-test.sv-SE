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
ms.openlocfilehash: 128a00b4296994551e7e17838a51e6d9de180e21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="7321f-103">Kontrollera om trafik som tillåts eller nekas tooor från en virtuell dator med IP-flöda Kontrollera en komponent i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="7321f-103">Check if traffic is allowed or denied tooor from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="7321f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7321f-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="7321f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7321f-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="7321f-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7321f-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="7321f-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7321f-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="7321f-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="7321f-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="7321f-109">IP-flöda verifiera är en funktion i Nätverksbevakaren som gör att du tooverify om trafik tillåts tooor från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7321f-109">IP Flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="7321f-110">Det här scenariot är användbart tooget aktuella tillstånd om en virtuell dator kan prata tooan extern resurs eller backend.</span><span class="sxs-lookup"><span data-stu-id="7321f-110">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="7321f-111">IP-flöde verifiera går att använda tooverify om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="7321f-111">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="7321f-112">En annan orsak till att använda IP flödet Kontrollera tooensure trafik som du vill blockerade blockeras korrekt av hello NSG.</span><span class="sxs-lookup"><span data-stu-id="7321f-112">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

<span data-ttu-id="7321f-113">Den här artikeln använder våra nästa generations CLI för hello management resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="7321f-113">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="7321f-114">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="7321f-114">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7321f-115">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7321f-115">Before you begin</span></span>

<span data-ttu-id="7321f-116">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="7321f-116">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="7321f-117">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="7321f-117">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="7321f-118">Scenario</span><span class="sxs-lookup"><span data-stu-id="7321f-118">Scenario</span></span>

<span data-ttu-id="7321f-119">Det här scenariot använder IP-flöda Kontrollera tooverify om en virtuell dator kan prata tooa kända Bing IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7321f-119">This scenario uses IP Flow Verify tooverify if a virtual machine can talk tooa known Bing IP address.</span></span> <span data-ttu-id="7321f-120">Om hello trafik nekas returnerar hello säkerhetsregel som nekar trafiken.</span><span class="sxs-lookup"><span data-stu-id="7321f-120">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="7321f-121">toolearn mer om IP-flöda Kontrollera finns [flöda Kontrollera översikt över IP](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="7321f-121">toolearn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="7321f-122">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="7321f-122">Get a VM</span></span>

<span data-ttu-id="7321f-123">IP-flöde verifiera tester trafik tooor från en IP-adress på en virtuell dator tooor från en extern enhet.</span><span class="sxs-lookup"><span data-stu-id="7321f-123">IP flow verify tests traffic tooor from an IP address on a virtual machine tooor from a remote destination.</span></span> <span data-ttu-id="7321f-124">Ett Id för en virtuell dator krävs för hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7321f-124">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="7321f-125">Om du redan vet hello-ID för hello virtuella toouse kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="7321f-125">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```azurecli
az vm show --resource-group MyResourceGroup5431 --name MyVM-Web
```

## <a name="get-hello-nics"></a><span data-ttu-id="7321f-126">Hämta hello nätverkskort</span><span class="sxs-lookup"><span data-stu-id="7321f-126">Get hello NICS</span></span>

<span data-ttu-id="7321f-127">hello IP-adress till ett nätverkskort på den virtuella datorn hello krävs i det här exemplet vi hämta hello nätverkskort på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7321f-127">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="7321f-128">Om du redan vet hello IP-adress som du vill tootest på hello virtuell dator kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="7321f-128">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```azurecli
az network nic show --resource-group MyResourceGroup5431 --name MyNic-Web
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="7321f-129">Kontrollera kör IP-flöde</span><span class="sxs-lookup"><span data-stu-id="7321f-129">Run IP flow verify</span></span>

<span data-ttu-id="7321f-130">Nu när vi har hello information behövs toorun hello cmdlet vi kör hello `az network watcher test-ip-flow` cmdlet tootest hello trafik.</span><span class="sxs-lookup"><span data-stu-id="7321f-130">Now that we have hello information needed toorun hello cmdlet, we run hello `az network watcher test-ip-flow` cmdlet tootest hello traffic.</span></span> <span data-ttu-id="7321f-131">I det här exemplet använder vi hello första IP-adressen på hello första nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="7321f-131">In this example, we are using hello first IP address on hello first NIC.</span></span>

```azurecli
az network watcher test-ip-flow --resource-group resourceGroupName --direction directionInboundorOutbound --protocol protocolTCPorUDP --local ipAddressandPort --remote ipAddressandPort --vm vmNameorID --nic nicNameorID
```

> [!NOTE]
> <span data-ttu-id="7321f-132">IP-flöda Kontrollera kräver att hello Virtuella datorresursen fördelas toorun.</span><span class="sxs-lookup"><span data-stu-id="7321f-132">IP Flow verify requires that hello VM resource is allocated toorun.</span></span>

## <a name="review-results"></a><span data-ttu-id="7321f-133">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="7321f-133">Review Results</span></span>

<span data-ttu-id="7321f-134">När du har kört `az network watcher test-ip-flow` hello resultaten returneras, hello följande exempel är hello resultaten som returnerades från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="7321f-134">After running `az network watcher test-ip-flow` hello results are returned, hello following example is hello results returned from hello preceding step.</span></span>

```azurecli
{
    "access": "Allow",
    "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

## <a name="next-steps"></a><span data-ttu-id="7321f-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7321f-135">Next steps</span></span>

<span data-ttu-id="7321f-136">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som har definierats.</span><span class="sxs-lookup"><span data-stu-id="7321f-136">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

<span data-ttu-id="7321f-137">Läs tooaudit NSG-inställningarna genom att besöka [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7321f-137">Learn tooaudit your NSG settings by visiting [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md).</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
