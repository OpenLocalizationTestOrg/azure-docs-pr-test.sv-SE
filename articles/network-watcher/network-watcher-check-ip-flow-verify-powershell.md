---
title: Kontrollera aaaverify trafik med Azure Network Watcher IP - PowerShell | Microsoft Docs
description: "Den här artikeln beskriver hur toocheck om trafik tooor från en virtuell dator tillåts eller nekas med hjälp av PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e1dad757-8c5d-467f-812e-7cc751143207
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 924da1de1bd554e15816886f8e51d7f170f0e7ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="102bf-103">Kontrollera om trafik som tillåts eller nekas tooor från en virtuell dator med IP-flöde verifiera en komponent i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="102bf-103">Check if traffic is allowed or denied tooor from a VM with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="102bf-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="102bf-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="102bf-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="102bf-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="102bf-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="102bf-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="102bf-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="102bf-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="102bf-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="102bf-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="102bf-109">IP-flöde verifiera är en funktion i Nätverksbevakaren som gör att du tooverify om trafik tillåts tooor från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="102bf-109">IP flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="102bf-110">Det här scenariot är användbart tooget aktuella tillstånd om en virtuell dator kan prata tooan extern resurs eller backend.</span><span class="sxs-lookup"><span data-stu-id="102bf-110">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="102bf-111">IP-flöde verifiera går att använda tooverify om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="102bf-111">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="102bf-112">En annan orsak till att använda IP flödet Kontrollera tooensure trafik som du vill blockerade blockeras korrekt av hello NSG.</span><span class="sxs-lookup"><span data-stu-id="102bf-112">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="102bf-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="102bf-113">Before you begin</span></span>

<span data-ttu-id="102bf-114">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="102bf-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="102bf-115">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="102bf-115">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="102bf-116">Scenario</span><span class="sxs-lookup"><span data-stu-id="102bf-116">Scenario</span></span>

<span data-ttu-id="102bf-117">Det här scenariot använder IP-flödet verifiera tooverify om en virtuell dator kan prata tooa kända Bing IP-adress.</span><span class="sxs-lookup"><span data-stu-id="102bf-117">This scenario uses IP flow verify tooverify if a virtual machine can talk tooa known Bing IP address.</span></span> <span data-ttu-id="102bf-118">Om hello trafik nekas returnerar hello säkerhetsregel som nekar trafiken.</span><span class="sxs-lookup"><span data-stu-id="102bf-118">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="102bf-119">toolearn mer om IP-flöde Kontrollera finns [IP-flöde Kontrollera översikt](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="102bf-119">toolearn more about IP flow verify, visit [IP flow verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="102bf-120">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="102bf-120">Retrieve Network Watcher</span></span>

<span data-ttu-id="102bf-121">hello första steget är tooretrieve hello Nätverksbevakaren instans.</span><span class="sxs-lookup"><span data-stu-id="102bf-121">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="102bf-122">Hej `$networkWatcher` variabel har skickats toohello IP flödet Kontrollera cmdlet.</span><span class="sxs-lookup"><span data-stu-id="102bf-122">hello `$networkWatcher` variable is passed toohello IP flow verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="102bf-123">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="102bf-123">Get a VM</span></span>

<span data-ttu-id="102bf-124">IP-flöde verifiera tester trafik tooor från en IP-adress på en virtuell dator tooor från en extern enhet.</span><span class="sxs-lookup"><span data-stu-id="102bf-124">IP flow verify tests traffic tooor from an IP address on a virtual machine tooor from a remote destination.</span></span> <span data-ttu-id="102bf-125">Ett Id för en virtuell dator krävs för hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="102bf-125">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="102bf-126">Om du redan vet hello-ID för hello virtuella toouse kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="102bf-126">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-hello-nics"></a><span data-ttu-id="102bf-127">Hämta hello nätverkskort</span><span class="sxs-lookup"><span data-stu-id="102bf-127">Get hello NICS</span></span>

<span data-ttu-id="102bf-128">hello IP-adress till ett nätverkskort på den virtuella datorn hello krävs i det här exemplet vi hämta hello nätverkskort på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="102bf-128">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="102bf-129">Om du redan vet hello IP-adress som du vill tootest på hello virtuell dator kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="102bf-129">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="102bf-130">Kontrollera kör IP-flöde</span><span class="sxs-lookup"><span data-stu-id="102bf-130">Run IP flow verify</span></span>

<span data-ttu-id="102bf-131">Nu när vi har hello information behövs toorun hello cmdlet vi kör hello `Test-AzureRmNetworkWatcherIPFlow` cmdlet tootest hello trafik.</span><span class="sxs-lookup"><span data-stu-id="102bf-131">Now that we have hello information needed toorun hello cmdlet, we run hello `Test-AzureRmNetworkWatcherIPFlow` cmdlet tootest hello traffic.</span></span> <span data-ttu-id="102bf-132">I det här exemplet använder vi hello första IP-adressen på hello första nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="102bf-132">In this example, we are using hello first IP address on hello first NIC.</span></span>

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> <span data-ttu-id="102bf-133">IP-flöde Kontrollera kräver att hello Virtuella datorresursen fördelas toorun.</span><span class="sxs-lookup"><span data-stu-id="102bf-133">IP flow verify requires that hello VM resource is allocated toorun.</span></span>

## <a name="review-results"></a><span data-ttu-id="102bf-134">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="102bf-134">Review Results</span></span>

<span data-ttu-id="102bf-135">När du har kört `Test-AzureRmNetworkWatcherIPFlow` hello resultaten returneras, hello följande exempel är hello resultaten som returnerades från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="102bf-135">After running `Test-AzureRmNetworkWatcherIPFlow` hello results are returned, hello following example is hello results returned from hello preceding step.</span></span>

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a><span data-ttu-id="102bf-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="102bf-136">Next steps</span></span>

<span data-ttu-id="102bf-137">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som har definierats.</span><span class="sxs-lookup"><span data-stu-id="102bf-137">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













