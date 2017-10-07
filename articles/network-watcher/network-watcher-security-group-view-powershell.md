---
title: "aaaAnalyze nätverkssäkerhet med Azure Network Watcher säkerhet gruppvyn - PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse PowerShell tooanalyze en virtuella datorer säkerhet med Gruppvy för säkerhet."
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
ms.openlocfilehash: 5e1990d97899bd8585025ec13dd556ab2e034c3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-powershell"></a><span data-ttu-id="a3678-103">Analysera dina virtuella säkerhet med säkerhet gruppvyn med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3678-103">Analyze your Virtual Machine security with Security Group View using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="a3678-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3678-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="a3678-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a3678-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="a3678-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a3678-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="a3678-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="a3678-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="a3678-108">Säkerhet gruppvyn returnerar konfigurerade och effektivt Nätverkssäkerhetsregler som är kopplade tooa virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a3678-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="a3678-109">Den här funktionen är användbart tooaudit och diagnostisera Nätverkssäkerhetsgrupper och regler som är konfigurerade på en VM tooensure trafik som tillåts eller nekas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="a3678-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="a3678-110">I den här artikeln visar vi hur tooretrieve hello konfigurerade och effektiv säkerhet regler tooa virtuell dator med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3678-110">In this article, we show you how tooretrieve hello configured and effective security rules tooa virtual machine using PowerShell</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a3678-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a3678-111">Before you begin</span></span>

<span data-ttu-id="a3678-112">I det här scenariot kan du köra hello `Get-AzureRmNetworkWatcherSecurityGroupView` regeln för cmdlet tooretrieve hello säkerhetsinformation.</span><span class="sxs-lookup"><span data-stu-id="a3678-112">In this scenario, you run hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet tooretrieve hello security rule information.</span></span>

<span data-ttu-id="a3678-113">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="a3678-113">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="a3678-114">Scenario</span><span class="sxs-lookup"><span data-stu-id="a3678-114">Scenario</span></span>

<span data-ttu-id="a3678-115">hello-scenario som beskrivs i den här artikeln hämtar hello konfigurerad och effektiva säkerhetsregler för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a3678-115">hello scenario covered in this article retrieves hello configured and effective security rules for a given virtual machine.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="a3678-116">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="a3678-116">Retrieve Network Watcher</span></span>

<span data-ttu-id="a3678-117">hello första steget är tooretrieve hello Nätverksbevakaren instans.</span><span class="sxs-lookup"><span data-stu-id="a3678-117">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="a3678-118">Den här variabeln skickas toohello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a3678-118">This variable is passed toohello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="get-a-vm"></a><span data-ttu-id="a3678-119">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a3678-119">Get a VM</span></span>

<span data-ttu-id="a3678-120">En virtuell dator är obligatoriska toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet mot.</span><span class="sxs-lookup"><span data-stu-id="a3678-120">A virtual machine is required toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="a3678-121">hello följande exempel hämtar ett VM-objekt.</span><span class="sxs-lookup"><span data-stu-id="a3678-121">hello following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName testrg -Name testvm1
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="a3678-122">Hämta gruppvy för säkerhet</span><span class="sxs-lookup"><span data-stu-id="a3678-122">Retrieve security group view</span></span>

<span data-ttu-id="a3678-123">hello nästa steg är tooretrieve hello säkerhet grupp visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="a3678-123">hello next step is tooretrieve hello security group view result.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="viewing-hello-results"></a><span data-ttu-id="a3678-124">Visa hello resultat</span><span class="sxs-lookup"><span data-stu-id="a3678-124">Viewing hello results</span></span>

<span data-ttu-id="a3678-125">hello är följande exempel ett kortare svar hello resultat returneras.</span><span class="sxs-lookup"><span data-stu-id="a3678-125">hello following example is a shortened response of hello results returned.</span></span> <span data-ttu-id="a3678-126">hello resultatet visar alla hello effektiva och tillämpade säkerhetsregler på hello virtuella datorn är uppdelade i grupper med **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, och  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="a3678-126">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a3678-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a3678-127">Next steps</span></span>

<span data-ttu-id="a3678-128">Besök [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md) toolearn hur tooautomate validering av Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="a3678-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>


