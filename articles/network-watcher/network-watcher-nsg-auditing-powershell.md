---
title: "aaaAutomate NSG granskning med Watcher nätverkssäkerhet för Azure gruppvyn | Microsoft Docs"
description: "Den här sidan innehåller instruktioner om hur tooconfigure granskning av en Nätverkssäkerhetsgrupp"
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
ms.openlocfilehash: 24fc418c433fceaf55a74b7c3b0e354dc46c8729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a><span data-ttu-id="df8bb-103">Automatisera NSG granskning med vyn för Watcher nätverkssäkerhet för Azure-grupp</span><span class="sxs-lookup"><span data-stu-id="df8bb-103">Automate NSG auditing with Azure Network Watcher Security group view</span></span>

<span data-ttu-id="df8bb-104">Kunder inför ofta hello utmaningen i att verifiera hello säkerhetsläget i sin infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="df8bb-104">Customers are often faced with hello challenge of verifying hello security posture of their infrastructure.</span></span> <span data-ttu-id="df8bb-105">Denna utmaning skiljer sig inte för sina virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="df8bb-105">This challenge is no different for their VMs in Azure.</span></span> <span data-ttu-id="df8bb-106">Det är viktigt toohave en liknande säkerhetsprofil baserat på hello Nätverkssäkerhetsgrupp (NSG) regler tillämpas.</span><span class="sxs-lookup"><span data-stu-id="df8bb-106">It is important toohave a similar security profile based on hello Network Security Group (NSG) rules applied.</span></span> <span data-ttu-id="df8bb-107">Med hello säkerhet gruppvyn kan nu du få hello lista över regler tillämpas tooa VM inom en NSG.</span><span class="sxs-lookup"><span data-stu-id="df8bb-107">Using hello Security Group View, you can now get hello list of rules applied tooa VM within an NSG.</span></span> <span data-ttu-id="df8bb-108">Du kan definiera en gyllene NSG säkerhetsprofil och initiera Gruppvy för säkerhet i varje vecka takt och jämföra hello utdata toohello gyllene profil och skapa en rapport.</span><span class="sxs-lookup"><span data-stu-id="df8bb-108">You can define a golden NSG security profile and initiate Security Group View on a weekly cadence and compare hello output toohello golden profile and create a report.</span></span> <span data-ttu-id="df8bb-109">Det här sättet kan du identifiera enkelt alla hello VMs som inte följer toohello föreskrivs säkerhetsprofil.</span><span class="sxs-lookup"><span data-stu-id="df8bb-109">This way you can identify with ease all hello VMs that do not conform toohello prescribed security profile.</span></span>

<span data-ttu-id="df8bb-110">Om du är bekant med Nätverkssäkerhetsgrupper [översikt över säkerheten i nätverket](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="df8bb-110">If you are unfamiliar with Network Security Groups, visit [Network Security Overview](../virtual-network/virtual-networks-nsg.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="df8bb-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="df8bb-111">Before you begin</span></span>

<span data-ttu-id="df8bb-112">I det här scenariot kan du jämföra en säkerhetsgrupp för kända fungerande baslinjen toohello visa resultaten som returnerades för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="df8bb-112">In this scenario, you compare a known good baseline toohello security group view results returned for a virtual machine.</span></span>

<span data-ttu-id="df8bb-113">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="df8bb-113">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="df8bb-114">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="df8bb-114">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="df8bb-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="df8bb-115">Scenario</span></span>

<span data-ttu-id="df8bb-116">hello-scenario som beskrivs i den här artikeln hämtar hello säkerhet gruppvyn för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="df8bb-116">hello scenario covered in this article gets hello security group view for a virtual machine.</span></span>

<span data-ttu-id="df8bb-117">I det här scenariot kommer du att:</span><span class="sxs-lookup"><span data-stu-id="df8bb-117">In this scenario, you will:</span></span>

- <span data-ttu-id="df8bb-118">Hämta en känd bra regeluppsättning</span><span class="sxs-lookup"><span data-stu-id="df8bb-118">Retrieve a known good rule set</span></span>
- <span data-ttu-id="df8bb-119">Hämta en virtuell dator med Rest API</span><span class="sxs-lookup"><span data-stu-id="df8bb-119">Retrieve a virtual machine with Rest API</span></span>
- <span data-ttu-id="df8bb-120">Hämta gruppvy för säkerhet för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="df8bb-120">Get security group view for virtual machine</span></span>
- <span data-ttu-id="df8bb-121">Utvärdera svar</span><span class="sxs-lookup"><span data-stu-id="df8bb-121">Evaluate Response</span></span>

## <a name="retrieve-rule-set"></a><span data-ttu-id="df8bb-122">Hämta regeluppsättning</span><span class="sxs-lookup"><span data-stu-id="df8bb-122">Retrieve rule set</span></span>

<span data-ttu-id="df8bb-123">hello första steget i det här exemplet är toowork med en befintlig baslinje.</span><span class="sxs-lookup"><span data-stu-id="df8bb-123">hello first step in this example is toowork with an existing baseline.</span></span> <span data-ttu-id="df8bb-124">hello följande exempel är vissa json som extraherats från en befintlig säkerhetsgrupp i nätverket med hjälp av hello `Get-AzureRmNetworkSecurityGroup` cmdlet som används som hello baslinje för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-124">hello following example is some json extracted from an existing Network Security Group using hello `Get-AzureRmNetworkSecurityGroup` cmdlet that is used as hello baseline for this example.</span></span>

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

## <a name="convert-rule-set-toopowershell-objects"></a><span data-ttu-id="df8bb-125">Omvandla regeln set tooPowerShell objekt</span><span class="sxs-lookup"><span data-stu-id="df8bb-125">Convert rule set tooPowerShell objects</span></span>

<span data-ttu-id="df8bb-126">I det här steget ska läser vi en json-fil som har skapats tidigare med hello regler som är förväntade toobe på hello Nätverkssäkerhetsgruppen för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-126">In this step, we are reading a json file that was created earlier with hello rules that are expected toobe on hello Network Security Group for this example.</span></span>

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a><span data-ttu-id="df8bb-127">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="df8bb-127">Retrieve Network Watcher</span></span>

<span data-ttu-id="df8bb-128">hello nästa steg är tooretrieve hello Nätverksbevakaren instans.</span><span class="sxs-lookup"><span data-stu-id="df8bb-128">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="df8bb-129">Hej `$networkWatcher` variabel har skickats toohello `AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-129">hello `$networkWatcher` variable is passed toohello `AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="df8bb-130">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="df8bb-130">Get a VM</span></span>

<span data-ttu-id="df8bb-131">En virtuell dator är obligatoriska toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet mot.</span><span class="sxs-lookup"><span data-stu-id="df8bb-131">A virtual machine is required toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="df8bb-132">hello följande exempel hämtar ett VM-objekt.</span><span class="sxs-lookup"><span data-stu-id="df8bb-132">hello following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="df8bb-133">Hämta gruppvy för säkerhet</span><span class="sxs-lookup"><span data-stu-id="df8bb-133">Retrieve security group view</span></span>

<span data-ttu-id="df8bb-134">hello nästa steg är tooretrieve hello säkerhet grupp visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-134">hello next step is tooretrieve hello security group view result.</span></span> <span data-ttu-id="df8bb-135">Resultatet är jämfört med toohello ”baseline” json som visades tidigare.</span><span class="sxs-lookup"><span data-stu-id="df8bb-135">This result is compared toohello "baseline" json that was shown earlier.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-hello-results"></a><span data-ttu-id="df8bb-136">Analyserar hello resultat</span><span class="sxs-lookup"><span data-stu-id="df8bb-136">Analyzing hello results</span></span>

<span data-ttu-id="df8bb-137">hello svar är grupperad efter nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="df8bb-137">hello response is grouped by Network interfaces.</span></span> <span data-ttu-id="df8bb-138">hello olika typer av regler som returnerades är giltiga och standard säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="df8bb-138">hello different types of rules returned are effective and default security rules.</span></span> <span data-ttu-id="df8bb-139">hello resultatet är ytterligare fördelade på hur den används, antingen på ett undernät eller ett virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="df8bb-139">hello result is further broken down by how it is applied, either on a subnet or a virtual NIC.</span></span>

<span data-ttu-id="df8bb-140">hello följande PowerShell-skript för att jämföra hello resultaten av hello säkerhet gruppvyn tooan befintliga utdata från en NSG.</span><span class="sxs-lookup"><span data-stu-id="df8bb-140">hello following PowerShell script compares hello results of hello Security Group View tooan existing output of an NSG.</span></span> <span data-ttu-id="df8bb-141">hello följande exempel är ett enkelt exempel på hur hello resultaten kan jämföras med `Compare-Object` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-141">hello following example is a simple example of how hello results can be compared with `Compare-Object` cmdlet.</span></span>

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

<span data-ttu-id="df8bb-142">följande exempel hello är hello resultat.</span><span class="sxs-lookup"><span data-stu-id="df8bb-142">hello following example is hello result.</span></span> <span data-ttu-id="df8bb-143">Du kan se två hello regler som fanns i hello första regeluppsättning inte finns några hello jämförelse.</span><span class="sxs-lookup"><span data-stu-id="df8bb-143">You can see two of hello rules that were in hello first rule set were not present in hello comparison.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="df8bb-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df8bb-144">Next steps</span></span>

<span data-ttu-id="df8bb-145">Om inställningarna har ändrats, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som är i fråga.</span><span class="sxs-lookup"><span data-stu-id="df8bb-145">If settings have been changed, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are in question.</span></span>













