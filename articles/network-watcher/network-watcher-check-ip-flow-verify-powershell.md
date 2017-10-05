---
title: "Kontrollera trafik med Azure Network Watcher IP flödet verifiera - PowerShell | Microsoft Docs"
description: "Den här artikeln beskrivs hur du kontrollerar om trafik till eller från en virtuell dator tillåts eller nekas med hjälp av PowerShell"
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
ms.openlocfilehash: bf0c01a9af0e28647d11ad89a9d164716d5c8312
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="bf3ea-103">Kontrollera om trafik tillåts eller nekas till eller från en virtuell dator med IP-flöde verifiera en komponent i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="bf3ea-103">Check if traffic is allowed or denied to or from a VM with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="bf3ea-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bf3ea-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="bf3ea-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bf3ea-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="bf3ea-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="bf3ea-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="bf3ea-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bf3ea-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="bf3ea-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="bf3ea-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="bf3ea-109">IP-flöde verifiera är en funktion i Nätverksbevakaren som hjälper dig att kontrollera om tillåts trafik till eller från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-109">IP flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="bf3ea-110">Det här scenariot är användbart för att hämta aktuella tillstånd om en virtuell dator kan kommunicera med en extern resurs eller backend.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-110">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or backend.</span></span> <span data-ttu-id="bf3ea-111">IP-flöde Kontrollera kan användas för att kontrollera om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-111">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="bf3ea-112">En annan orsak till att använda IP flödet Kontrollera är att kontrollera att trafik som du vill blockerade blockeras korrekt av NSG: N.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-112">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="bf3ea-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="bf3ea-113">Before you begin</span></span>

<span data-ttu-id="bf3ea-114">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="bf3ea-115">Det här scenariot förutsätter att det finns en resursgrupp med en giltig virtuell dator som ska användas.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-115">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="bf3ea-116">Scenario</span><span class="sxs-lookup"><span data-stu-id="bf3ea-116">Scenario</span></span>

<span data-ttu-id="bf3ea-117">Det här scenariot använder IP-flödet Kontrollera för att kontrollera om en virtuell dator kan du kontakta en känd Bing IP-adress.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-117">This scenario uses IP flow verify to verify if a virtual machine can talk to a known Bing IP address.</span></span> <span data-ttu-id="bf3ea-118">Om trafiken nekas returnerar säkerhetsregeln som nekar trafiken.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-118">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="bf3ea-119">Läs mer om IP-flöde Kontrollera [IP-flöde Kontrollera översikt](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="bf3ea-119">To learn more about IP flow verify, visit [IP flow verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="bf3ea-120">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="bf3ea-120">Retrieve Network Watcher</span></span>

<span data-ttu-id="bf3ea-121">Det första steget är att hämta Nätverksbevakaren-instans.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-121">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="bf3ea-122">Den `$networkWatcher` variabel har skickats till den IP-flöde Kontrollera cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-122">The `$networkWatcher` variable is passed to the IP flow verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="bf3ea-123">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="bf3ea-123">Get a VM</span></span>

<span data-ttu-id="bf3ea-124">IP-flöde verifiera tester trafik till eller från en IP-adress på en virtuell dator till eller från en extern enhet.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-124">IP flow verify tests traffic to or from an IP address on a virtual machine to or from a remote destination.</span></span> <span data-ttu-id="bf3ea-125">Ett Id för en virtuell dator krävs för cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-125">An Id of a virtual machine is required for the cmdlet.</span></span> <span data-ttu-id="bf3ea-126">Om du redan känner till ID: T för den virtuella datorn att använda, kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-126">If you already know the ID of the virtual machine to use, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-the-nics"></a><span data-ttu-id="bf3ea-127">Hämta Nätverkskorten</span><span class="sxs-lookup"><span data-stu-id="bf3ea-127">Get the NICS</span></span>

<span data-ttu-id="bf3ea-128">IP-adressen för ett nätverkskort på den virtuella datorn krävs i det här exemplet vi hämta nätverkskort på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-128">The IP address of a NIC on the virtual machine is needed, in this example we retrieve the NICs on a virtual machine.</span></span> <span data-ttu-id="bf3ea-129">Om du redan känner till IP-adressen som du vill testa på den virtuella datorn, kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-129">If you already know the IP address that you want to test on the virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="bf3ea-130">Kontrollera kör IP-flöde</span><span class="sxs-lookup"><span data-stu-id="bf3ea-130">Run IP flow verify</span></span>

<span data-ttu-id="bf3ea-131">Nu när vi har information som behövs för att köra cmdlet vi kör den `Test-AzureRmNetworkWatcherIPFlow` för att testa trafiken.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-131">Now that we have the information needed to run the cmdlet, we run the `Test-AzureRmNetworkWatcherIPFlow` cmdlet to test the traffic.</span></span> <span data-ttu-id="bf3ea-132">I det här exemplet använder du den första IP-adressen på det första nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-132">In this example, we are using the first IP address on the first NIC.</span></span>

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> <span data-ttu-id="bf3ea-133">IP-flöde Kontrollera kräver att den Virtuella datorresursen har allokerats för att köras.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-133">IP flow verify requires that the VM resource is allocated to run.</span></span>

## <a name="review-results"></a><span data-ttu-id="bf3ea-134">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="bf3ea-134">Review Results</span></span>

<span data-ttu-id="bf3ea-135">När du har kört `Test-AzureRmNetworkWatcherIPFlow` resultaten returneras, i följande exempel är resultatet har returnerats från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-135">After running `Test-AzureRmNetworkWatcherIPFlow` the results are returned, the following example is the results returned from the preceding step.</span></span>

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a><span data-ttu-id="bf3ea-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf3ea-136">Next steps</span></span>

<span data-ttu-id="bf3ea-137">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) att spåra de grupp och säkerhet Nätverkssäkerhetsregler som har definierats.</span><span class="sxs-lookup"><span data-stu-id="bf3ea-137">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













