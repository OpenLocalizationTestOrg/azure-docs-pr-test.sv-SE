---
title: "Analysera nätverkssäkerhet med Azure Network Watcher säkerhet gruppvyn - Azure CLI 2.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur du använder Azure CLI 2.0 för att analysera en säkerhet för virtuella datorer med Gruppvy för säkerhet."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 1756e14819e3b7c79361c193413a1fcd7f24a4e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-20"></a><span data-ttu-id="24ac2-103">Analysera dina virtuella säkerhet med säkerhet gruppvyn använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="24ac2-103">Analyze your Virtual Machine security with Security Group View using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="24ac2-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="24ac2-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="24ac2-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="24ac2-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="24ac2-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="24ac2-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="24ac2-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="24ac2-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="24ac2-108">Säkerhet gruppvyn returnerar konfigurerade och effektivt Nätverkssäkerhetsregler som tillämpas på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="24ac2-108">Security group view returns configured and effective network security rules that are applied to a virtual machine.</span></span> <span data-ttu-id="24ac2-109">Den här funktionen är användbar för att granska och diagnostisera Nätverkssäkerhetsgrupper och regler som är konfigurerade på en virtuell dator så trafik som tillåts eller nekas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="24ac2-109">This capability is useful to audit and diagnose Network Security Groups and rules that are configured on a VM to ensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="24ac2-110">I den här artikeln hur vi du kan hämta de konfigurerade och effektivt säkerhetsreglerna till en virtuell dator med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="24ac2-110">In this article, we show you how to retrieve the configured and effective security rules to a virtual machine using Azure CLI</span></span>


<span data-ttu-id="24ac2-111">Den här artikeln använder våra nästa generations CLI för hantering av resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="24ac2-111">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="24ac2-112">Om du vill utföra stegen i den här artikeln, måste du [installera Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="24ac2-112">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="24ac2-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="24ac2-113">Before you begin</span></span>

<span data-ttu-id="24ac2-114">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="24ac2-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="24ac2-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="24ac2-115">Scenario</span></span>

<span data-ttu-id="24ac2-116">Det scenario som beskrivs i den här artikeln hämtar konfigurerade och effektivt säkerhetsregler för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="24ac2-116">The scenario covered in this article retrieves the configured and effective security rules for a given virtual machine.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="24ac2-117">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24ac2-117">Get a VM</span></span>

<span data-ttu-id="24ac2-118">En virtuell dator krävs för att köra den `vm list` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="24ac2-118">A virtual machine is required to run the `vm list` cmdlet.</span></span> <span data-ttu-id="24ac2-119">Följande kommando visar de virtuella datorerna i en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="24ac2-119">The following command lists the virtual machines in a resource group:</span></span>

```azurecli
az vm list -resource-group resourceGroupName
```

<span data-ttu-id="24ac2-120">När du vet att den virtuella datorn kan du använda den `vm show` för att hämta dess resursens Id:</span><span class="sxs-lookup"><span data-stu-id="24ac2-120">Once you know the virtual machine, you can use the `vm show` cmdlet to get its resource Id:</span></span>

```azurecli
az vm show -resource-group resourceGroupName -name virtualMachineName
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="24ac2-121">Hämta gruppvy för säkerhet</span><span class="sxs-lookup"><span data-stu-id="24ac2-121">Retrieve security group view</span></span>

<span data-ttu-id="24ac2-122">Nästa steg är att hämta säkerhet grupp visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="24ac2-122">The next step is to retrieve the security group view result.</span></span>

```azurecli
az network watcher show-security-group-view --resource-group resourceGroupName --vm vmName
```

## <a name="viewing-the-results"></a><span data-ttu-id="24ac2-123">Visa resultaten</span><span class="sxs-lookup"><span data-stu-id="24ac2-123">Viewing the results</span></span>

<span data-ttu-id="24ac2-124">I följande exempel är ett kortare resultaten-svar.</span><span class="sxs-lookup"><span data-stu-id="24ac2-124">The following example is a shortened response of the results returned.</span></span> <span data-ttu-id="24ac2-125">Resultaten visar alla säkerhet effektiva och tillämpa regler på den virtuella datorn som är uppdelad i grupper med **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, och **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="24ac2-125">The results show all the effective and applied security rules on the virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
      "resourceGroup": "{resourceGroupName}",
      "securityRuleAssociations": {
        "defaultSecurityRules": [
          {
            "access": "Allow",
            "description": "Allow inbound traffic from all VMs in VNET",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "*",
            "direction": "Inbound",
            "etag": null,
            "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups//providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/AllowVnetInBound",
            "name": "AllowVnetInBound",
            "priority": 65000,
            "protocol": "*",
            "provisioningState": "Succeeded",
            "resourceGroup": "",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "*"
          }...
        ],
        "effectiveSecurityRules": [
          {
            "access": "Deny",
            "destinationAddressPrefix": "*",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": null,
            "expandedSourceAddressPrefix": null,
            "name": "DefaultOutboundDenyAll",
            "priority": 65500,
            "protocol": "All",
            "sourceAddressPrefix": "*",
            "sourcePortRange": "0-65535"
          },
          {
            "access": "Allow",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "expandedSourceAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "name": "DefaultRule_AllowVnetOutBound",
            "priority": 65000,
            "protocol": "All",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "0-65535"
          },...
        ],
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "resourceGroup": "{resourceGroupName}",
          "securityRules": [
            {
              "access": "Allow",
              "description": null,
              "destinationAddressPrefix": "*",
              "destinationPortRange": "3389",
              "direction": "Inbound",
              "etag": "W/\"efb606c1-2d54-475a-ab20-da3f80393577\"",
              "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "name": "default-allow-rdp",
              "priority": 1000,
              "protocol": "TCP",
              "provisioningState": "Succeeded",
              "resourceGroup": "{resourceGroupName}",
              "sourceAddressPrefix": "*",
              "sourcePortRange": "*"
            }
          ]
        },
        "subnetAssociation": null
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="24ac2-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24ac2-126">Next steps</span></span>

<span data-ttu-id="24ac2-127">Besök [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md) information om hur du automatiserar validering av Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="24ac2-127">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) to learn how to automate validation of Network Security Groups.</span></span>

<span data-ttu-id="24ac2-128">Mer information om säkerhetsregler som tillämpas på nätverksresurserna genom att besöka [Säkerhetsöversikt grupp vy](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="24ac2-128">Learn more about the security rules that are applied to your network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
