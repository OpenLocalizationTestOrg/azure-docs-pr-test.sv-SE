---
title: "Analysera nätverkssäkerhet med Azure Network Watcher säkerhet gruppvyn - PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du använder PowerShell för att analysera en säkerhet för virtuella datorer med Gruppvy för säkerhet."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 04e76b49-6a1b-4d0f-9a9b-51cf2f4df5a2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 363fdd9f1de933bb4050f91e1e111aaf3e419058
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-powershell"></a><span data-ttu-id="c703e-103">Analysera dina virtuella säkerhet med säkerhet gruppvyn med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="c703e-103">Analyze your Virtual Machine security with Security Group View using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="c703e-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c703e-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="c703e-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c703e-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="c703e-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c703e-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="c703e-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="c703e-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="c703e-108">Säkerhet gruppvyn returnerar konfigurerade och effektivt Nätverkssäkerhetsregler som tillämpas på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c703e-108">Security group view returns configured and effective network security rules that are applied to a virtual machine.</span></span> <span data-ttu-id="c703e-109">Den här funktionen är användbar för att granska och diagnostisera Nätverkssäkerhetsgrupper och regler som är konfigurerade på en virtuell dator så trafik som tillåts eller nekas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="c703e-109">This capability is useful to audit and diagnose Network Security Groups and rules that are configured on a VM to ensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="c703e-110">I den här artikeln hur vi du kan hämta de konfigurerade och effektivt säkerhetsreglerna till en virtuell dator med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="c703e-110">In this article, we show you how to retrieve the configured and effective security rules to a virtual machine using PowerShell</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c703e-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c703e-111">Before you begin</span></span>

<span data-ttu-id="c703e-112">I det här scenariot kan du köra den `Get-AzureRmNetworkWatcherSecurityGroupView` för att hämta säkerhetsinformation för regeln.</span><span class="sxs-lookup"><span data-stu-id="c703e-112">In this scenario, you run the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet to retrieve the security rule information.</span></span>

<span data-ttu-id="c703e-113">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="c703e-113">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="c703e-114">Scenario</span><span class="sxs-lookup"><span data-stu-id="c703e-114">Scenario</span></span>

<span data-ttu-id="c703e-115">Det scenario som beskrivs i den här artikeln hämtar konfigurerade och effektivt säkerhetsregler för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c703e-115">The scenario covered in this article retrieves the configured and effective security rules for a given virtual machine.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="c703e-116">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="c703e-116">Retrieve Network Watcher</span></span>

<span data-ttu-id="c703e-117">Det första steget är att hämta Nätverksbevakaren-instans.</span><span class="sxs-lookup"><span data-stu-id="c703e-117">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="c703e-118">Den här variabeln har överförts till den `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="c703e-118">This variable is passed to the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="get-a-vm"></a><span data-ttu-id="c703e-119">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c703e-119">Get a VM</span></span>

<span data-ttu-id="c703e-120">En virtuell dator krävs för att köra den `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet mot.</span><span class="sxs-lookup"><span data-stu-id="c703e-120">A virtual machine is required to run the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="c703e-121">I följande exempel hämtas ett VM-objekt.</span><span class="sxs-lookup"><span data-stu-id="c703e-121">The following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName testrg -Name testvm1
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="c703e-122">Hämta gruppvy för säkerhet</span><span class="sxs-lookup"><span data-stu-id="c703e-122">Retrieve security group view</span></span>

<span data-ttu-id="c703e-123">Nästa steg är att hämta säkerhet grupp visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="c703e-123">The next step is to retrieve the security group view result.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="viewing-the-results"></a><span data-ttu-id="c703e-124">Visa resultaten</span><span class="sxs-lookup"><span data-stu-id="c703e-124">Viewing the results</span></span>

<span data-ttu-id="c703e-125">I följande exempel är ett kortare resultaten-svar.</span><span class="sxs-lookup"><span data-stu-id="c703e-125">The following example is a shortened response of the results returned.</span></span> <span data-ttu-id="c703e-126">Resultaten visar alla säkerhet effektiva och tillämpa regler på den virtuella datorn som är uppdelad i grupper med **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, och **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="c703e-126">The results show all the effective and applied security rules on the virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```
NetworkInterfaces : [
                      {
                        "NetworkInterfaceSecurityRules": [
                          {
                            "Name": "default-allow-rdp",
                            "Etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
                            "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/securityRules/default-allow-rdp",
                            "Protocol": "TCP",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "3389",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "Access": "Allow",
                            "Priority": 1000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "DefaultSecurityRules": [
                          {
                            "Name": "AllowVnetInBound",
                            "Id": "/subscriptions00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/defaultSecurityRules/",
                            "Description": "Allow inbound traffic from all VMs in VNET",
                            "Protocol": "*",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "*",
                            "SourceAddressPrefix": "VirtualNetwork",
                            "DestinationAddressPrefix": "VirtualNetwork",
                            "Access": "Allow",
                            "Priority": 65000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "EffectiveSecurityRules": [
                          {
                            "Name": "DefaultOutboundDenyAll",
                            "Protocol": "All",
                            "SourcePortRange": "0-65535",
                            "DestinationPortRange": "0-65535",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "ExpandedSourceAddressPrefix": [],
                            "ExpandedDestinationAddressPrefix": [],
                            "Access": "Deny",
                            "Priority": 65500,
                            "Direction": "Outbound"
                          },
                          ...
                        ]
                      }
                    ]
```

## <a name="next-steps"></a><span data-ttu-id="c703e-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c703e-127">Next steps</span></span>

<span data-ttu-id="c703e-128">Besök [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md) information om hur du automatiserar validering av Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="c703e-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) to learn how to automate validation of Network Security Groups.</span></span>


