---
title: "aaaAnalyze nätverkssäkerhet med Azure Network Watcher säkerhet gruppvyn - Azure CLI 2.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse Azure CLI 2.0 tooanalyze en virtuella datorer säkerhet med Gruppvy för säkerhet."
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
ms.openlocfilehash: 31a4cd628f54d7548f495251fd275f099e79a060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-20"></a><span data-ttu-id="71bdd-103">Analysera dina virtuella säkerhet med säkerhet gruppvyn använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="71bdd-103">Analyze your Virtual Machine security with Security Group View using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="71bdd-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="71bdd-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="71bdd-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="71bdd-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="71bdd-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="71bdd-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="71bdd-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="71bdd-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="71bdd-108">Säkerhet gruppvyn returnerar konfigurerade och effektivt Nätverkssäkerhetsregler som är kopplade tooa virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="71bdd-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="71bdd-109">Den här funktionen är användbart tooaudit och diagnostisera Nätverkssäkerhetsgrupper och regler som är konfigurerade på en VM tooensure trafik som tillåts eller nekas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="71bdd-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="71bdd-110">I den här artikeln visar vi hur tooretrieve hello konfigurerade och effektiv säkerhet regler tooa virtuell dator med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="71bdd-110">In this article, we show you how tooretrieve hello configured and effective security rules tooa virtual machine using Azure CLI</span></span>


<span data-ttu-id="71bdd-111">Den här artikeln använder våra nästa generations CLI för hello management resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="71bdd-111">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="71bdd-112">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="71bdd-112">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="71bdd-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="71bdd-113">Before you begin</span></span>

<span data-ttu-id="71bdd-114">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="71bdd-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="71bdd-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="71bdd-115">Scenario</span></span>

<span data-ttu-id="71bdd-116">hello-scenario som beskrivs i den här artikeln hämtar hello konfigurerad och effektiva säkerhetsregler för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="71bdd-116">hello scenario covered in this article retrieves hello configured and effective security rules for a given virtual machine.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="71bdd-117">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="71bdd-117">Get a VM</span></span>

<span data-ttu-id="71bdd-118">En virtuell dator är obligatoriska toorun hello `vm list` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="71bdd-118">A virtual machine is required toorun hello `vm list` cmdlet.</span></span> <span data-ttu-id="71bdd-119">hello visar följande kommando hello virtuella datorer i en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="71bdd-119">hello following command lists hello virtual machines in a resource group:</span></span>

```azurecli
az vm list -resource-group resourceGroupName
```

<span data-ttu-id="71bdd-120">När du vet hello virtuell dator kan du använda hello `vm show` cmdlet tooget dess resursens Id:</span><span class="sxs-lookup"><span data-stu-id="71bdd-120">Once you know hello virtual machine, you can use hello `vm show` cmdlet tooget its resource Id:</span></span>

```azurecli
az vm show -resource-group resourceGroupName -name virtualMachineName
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="71bdd-121">Hämta gruppvy för säkerhet</span><span class="sxs-lookup"><span data-stu-id="71bdd-121">Retrieve security group view</span></span>

<span data-ttu-id="71bdd-122">hello nästa steg är tooretrieve hello säkerhet grupp visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="71bdd-122">hello next step is tooretrieve hello security group view result.</span></span>

```azurecli
az network watcher show-security-group-view --resource-group resourceGroupName --vm vmName
```

## <a name="viewing-hello-results"></a><span data-ttu-id="71bdd-123">Visa hello resultat</span><span class="sxs-lookup"><span data-stu-id="71bdd-123">Viewing hello results</span></span>

<span data-ttu-id="71bdd-124">hello är följande exempel ett kortare svar hello resultat returneras.</span><span class="sxs-lookup"><span data-stu-id="71bdd-124">hello following example is a shortened response of hello results returned.</span></span> <span data-ttu-id="71bdd-125">hello resultatet visar alla hello effektiva och tillämpade säkerhetsregler på hello virtuella datorn är uppdelade i grupper med **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, och  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="71bdd-125">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="71bdd-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71bdd-126">Next steps</span></span>

<span data-ttu-id="71bdd-127">Besök [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md) toolearn hur tooautomate validering av Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="71bdd-127">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>

<span data-ttu-id="71bdd-128">Mer information om hello säkerhetsregler som tillämpade tooyour nätverksresurser genom att besöka [Säkerhetsöversikt grupp vy](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="71bdd-128">Learn more about hello security rules that are applied tooyour network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
