---
title: "Automatisera NSG granskning med Watcher nätverkssäkerhet för Azure gruppvyn | Microsoft Docs"
description: "Den här sidan innehåller instruktioner om hur du konfigurerar granskning av en Nätverkssäkerhetsgrupp"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: a91da330e677c85f16f6f4e506613576b6507d7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a><span data-ttu-id="5995e-103">Automatisera NSG granskning med vyn för Watcher nätverkssäkerhet för Azure-grupp</span><span class="sxs-lookup"><span data-stu-id="5995e-103">Automate NSG auditing with Azure Network Watcher Security group view</span></span>

<span data-ttu-id="5995e-104">Kunder är ofta inför utmaningen i att verifiera säkerhetstillståndet av infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="5995e-104">Customers are often faced with the challenge of verifying the security posture of their infrastructure.</span></span> <span data-ttu-id="5995e-105">Denna utmaning skiljer sig inte för sina virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="5995e-105">This challenge is no different for their VMs in Azure.</span></span> <span data-ttu-id="5995e-106">Det är viktigt att ha en liknande säkerhetsprofil baserat på Nätverkssäkerhetsgrupp (NSG)-regler tillämpas.</span><span class="sxs-lookup"><span data-stu-id="5995e-106">It is important to have a similar security profile based on the Network Security Group (NSG) rules applied.</span></span> <span data-ttu-id="5995e-107">I vyn säkerhet grupp kan nu du få listan över regler som tillämpas på en virtuell dator inom en NSG.</span><span class="sxs-lookup"><span data-stu-id="5995e-107">Using the Security Group View, you can now get the list of rules applied to a VM within an NSG.</span></span> <span data-ttu-id="5995e-108">Du kan definiera en gyllene NSG säkerhetsprofil och initiera Gruppvy för säkerhet i varje vecka takt och jämför utdata till gyllene profilen och skapa en rapport.</span><span class="sxs-lookup"><span data-stu-id="5995e-108">You can define a golden NSG security profile and initiate Security Group View on a weekly cadence and compare the output to the golden profile and create a report.</span></span> <span data-ttu-id="5995e-109">Det här sättet kan du identifiera med alla de virtuella datorer som inte överensstämmer med den föreskrivna säkerhetsprofilen.</span><span class="sxs-lookup"><span data-stu-id="5995e-109">This way you can identify with ease all the VMs that do not conform to the prescribed security profile.</span></span>

<span data-ttu-id="5995e-110">Om du är bekant med Nätverkssäkerhetsgrupper [översikt över säkerheten i nätverket](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="5995e-110">If you are unfamiliar with Network Security Groups, visit [Network Security Overview](../virtual-network/virtual-networks-nsg.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5995e-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="5995e-111">Before you begin</span></span>

<span data-ttu-id="5995e-112">I det här scenariot kan du jämföra kända fungerande baslinje till säkerhet grupp visa resultaten för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5995e-112">In this scenario, you compare a known good baseline to the security group view results returned for a virtual machine.</span></span>

<span data-ttu-id="5995e-113">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="5995e-113">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="5995e-114">Det här scenariot förutsätter att det finns en resursgrupp med en giltig virtuell dator som ska användas.</span><span class="sxs-lookup"><span data-stu-id="5995e-114">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="5995e-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="5995e-115">Scenario</span></span>

<span data-ttu-id="5995e-116">Det scenario som beskrivs i den här artikeln hämtar gruppvyn säkerhet för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5995e-116">The scenario covered in this article gets the security group view for a virtual machine.</span></span>

<span data-ttu-id="5995e-117">I det här scenariot kommer du att:</span><span class="sxs-lookup"><span data-stu-id="5995e-117">In this scenario, you will:</span></span>

- <span data-ttu-id="5995e-118">Hämta en känd bra regeluppsättning</span><span class="sxs-lookup"><span data-stu-id="5995e-118">Retrieve a known good rule set</span></span>
- <span data-ttu-id="5995e-119">Hämta en virtuell dator med Rest API</span><span class="sxs-lookup"><span data-stu-id="5995e-119">Retrieve a virtual machine with Rest API</span></span>
- <span data-ttu-id="5995e-120">Hämta gruppvy för säkerhet för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="5995e-120">Get security group view for virtual machine</span></span>
- <span data-ttu-id="5995e-121">Utvärdera svar</span><span class="sxs-lookup"><span data-stu-id="5995e-121">Evaluate Response</span></span>

## <a name="retrieve-rule-set"></a><span data-ttu-id="5995e-122">Hämta regeluppsättning</span><span class="sxs-lookup"><span data-stu-id="5995e-122">Retrieve rule set</span></span>

<span data-ttu-id="5995e-123">Det första steget i det här exemplet är att arbeta med en befintlig baslinjen.</span><span class="sxs-lookup"><span data-stu-id="5995e-123">The first step in this example is to work with an existing baseline.</span></span> <span data-ttu-id="5995e-124">Följande exempel är vissa json som extraherats från en befintlig säkerhetsgrupp för nätverk med den `Get-AzureRmNetworkSecurityGroup` cmdlet som används som baslinje för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="5995e-124">The following example is some json extracted from an existing Network Security Group using the `Get-AzureRmNetworkSecurityGroup` cmdlet that is used as the baseline for this example.</span></span>

```json
[
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "3389",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1000,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "default-allow-rdp",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/default-allow-rdp"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "111",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1010,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "MyRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/MyRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "112",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1020,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "My2ndRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/My2ndRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "5672",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Deny",
        "Priority":  1030,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "ThisRuleNeedsToStay",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/ThisRuleNeedsToStay"
    }
]
```

## <a name="convert-rule-set-to-powershell-objects"></a><span data-ttu-id="5995e-125">Konvertera regeluppsättning till PowerShell-objekt</span><span class="sxs-lookup"><span data-stu-id="5995e-125">Convert rule set to PowerShell objects</span></span>

<span data-ttu-id="5995e-126">I det här steget ska läser vi en json-fil som du skapade tidigare med regler som förväntas finnas på Nätverkssäkerhetsgruppen för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="5995e-126">In this step, we are reading a json file that was created earlier with the rules that are expected to be on the Network Security Group for this example.</span></span>

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a><span data-ttu-id="5995e-127">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="5995e-127">Retrieve Network Watcher</span></span>

<span data-ttu-id="5995e-128">Nästa steg är att hämta Nätverksbevakaren-instans.</span><span class="sxs-lookup"><span data-stu-id="5995e-128">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="5995e-129">Den `$networkWatcher` variabel har skickats till den `AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5995e-129">The `$networkWatcher` variable is passed to the `AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="5995e-130">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5995e-130">Get a VM</span></span>

<span data-ttu-id="5995e-131">En virtuell dator krävs för att köra den `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet mot.</span><span class="sxs-lookup"><span data-stu-id="5995e-131">A virtual machine is required to run the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="5995e-132">I följande exempel hämtas ett VM-objekt.</span><span class="sxs-lookup"><span data-stu-id="5995e-132">The following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="5995e-133">Hämta gruppvy för säkerhet</span><span class="sxs-lookup"><span data-stu-id="5995e-133">Retrieve security group view</span></span>

<span data-ttu-id="5995e-134">Nästa steg är att hämta säkerhet grupp visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="5995e-134">The next step is to retrieve the security group view result.</span></span> <span data-ttu-id="5995e-135">Det här resultatet jämförs med ”baseline” json som visades tidigare.</span><span class="sxs-lookup"><span data-stu-id="5995e-135">This result is compared to the "baseline" json that was shown earlier.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-the-results"></a><span data-ttu-id="5995e-136">Analysera resultaten</span><span class="sxs-lookup"><span data-stu-id="5995e-136">Analyzing the results</span></span>

<span data-ttu-id="5995e-137">Svaret är grupperad efter nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="5995e-137">The response is grouped by Network interfaces.</span></span> <span data-ttu-id="5995e-138">Olika typer av regler som returnerades är giltiga och standard säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="5995e-138">The different types of rules returned are effective and default security rules.</span></span> <span data-ttu-id="5995e-139">Resultatet är ytterligare fördelade på hur den används, antingen på ett undernät eller ett virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="5995e-139">The result is further broken down by how it is applied, either on a subnet or a virtual NIC.</span></span>

<span data-ttu-id="5995e-140">Följande PowerShell-skript jämför resultaten för en befintlig utdata från en NSG säkerhet gruppvyn.</span><span class="sxs-lookup"><span data-stu-id="5995e-140">The following PowerShell script compares the results of the Security Group View to an existing output of an NSG.</span></span> <span data-ttu-id="5995e-141">Följande exempel är ett enkelt exempel på hur resultaten kan jämföras med `Compare-Object` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5995e-141">The following example is a simple example of how the results can be compared with `Compare-Object` cmdlet.</span></span>

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

<span data-ttu-id="5995e-142">I följande exempel är resultatet.</span><span class="sxs-lookup"><span data-stu-id="5995e-142">The following example is the result.</span></span> <span data-ttu-id="5995e-143">Du kan se två regler i den första regeln inställda inte fanns i jämförelse.</span><span class="sxs-lookup"><span data-stu-id="5995e-143">You can see two of the rules that were in the first rule set were not present in the comparison.</span></span>

```
Name                     : My2ndRuleDoNotDelete
Description              : 
Protocol                 : *
SourcePortRange          : *
DestinationPortRange     : 112
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Allow
Priority                 : 1020
Direction                : Inbound
SideIndicator            : <=

Name                     : ThisRuleNeedsToStay
Description              : 
Protocol                 : TCP
SourcePortRange          : *
DestinationPortRange     : 5672
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Deny
Priority                 : 1030
Direction                : Inbound
SideIndicator            : <=
```

## <a name="next-steps"></a><span data-ttu-id="5995e-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5995e-144">Next steps</span></span>

<span data-ttu-id="5995e-145">Om inställningarna har ändrats, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) att spåra de grupp och säkerhet Nätverkssäkerhetsregler som är i fråga.</span><span class="sxs-lookup"><span data-stu-id="5995e-145">If settings have been changed, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are in question.</span></span>













