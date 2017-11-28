---
title: "aaaAnalyze nätverkssäkerhet med Azure Network Watcher säkerhet gruppvyn - Azure CLI 1.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse Azure CLI 1.0 tooanalyze en virtuella datorer säkerhet med Gruppvy för säkerhet."
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
ms.openlocfilehash: 96383a734b94d215d5b0f3d47339e46940d700b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a><span data-ttu-id="2f714-103">Analysera dina virtuella säkerhet med säkerhet gruppvyn använder Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2f714-103">Analyze your Virtual Machine security with Security Group View using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="2f714-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f714-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="2f714-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2f714-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="2f714-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2f714-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="2f714-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="2f714-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="2f714-108">Säkerhet gruppvyn returnerar konfigurerade och effektivt Nätverkssäkerhetsregler som är kopplade tooa virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2f714-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="2f714-109">Den här funktionen är användbart tooaudit och diagnostisera Nätverkssäkerhetsgrupper och regler som är konfigurerade på en VM tooensure trafik som tillåts eller nekas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="2f714-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="2f714-110">I den här artikeln visar vi hur tooretrieve hello konfigurerade och effektiv säkerhet regler tooa virtuell dator med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2f714-110">In this article, we show you how tooretrieve hello configured and effective security rules tooa virtual machine using Azure CLI</span></span>

<span data-ttu-id="2f714-111">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="2f714-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="2f714-112">Nätverksbevakaren använder för närvarande Azure CLI 1.0 för CLI-stöd.</span><span class="sxs-lookup"><span data-stu-id="2f714-112">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2f714-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2f714-113">Before you begin</span></span>

<span data-ttu-id="2f714-114">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="2f714-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="2f714-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="2f714-115">Scenario</span></span>

<span data-ttu-id="2f714-116">hello-scenario som beskrivs i den här artikeln hämtar hello konfigurerad och effektiva säkerhetsregler för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2f714-116">hello scenario covered in this article retrieves hello configured and effective security rules for a given virtual machine.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="2f714-117">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2f714-117">Get a VM</span></span>

<span data-ttu-id="2f714-118">En virtuell dator är obligatoriska toorun hello `vm list` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="2f714-118">A virtual machine is required toorun hello `vm list` cmdlet.</span></span> <span data-ttu-id="2f714-119">hello visar följande kommando hello virtuella machinese i en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="2f714-119">hello following command lists hello virtual machinese in a resource group:</span></span>

```azurecli
azure vm list -g resourceGroupName
```

<span data-ttu-id="2f714-120">När du vet hello virtuell dator kan du använda hello `vm show` cmdlet tooget dess resursens Id:</span><span class="sxs-lookup"><span data-stu-id="2f714-120">Once you know hello virtual machine, you can use hello `vm show` cmdlet tooget its resource Id:</span></span>

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="2f714-121">Hämta gruppvy för säkerhet</span><span class="sxs-lookup"><span data-stu-id="2f714-121">Retrieve security group view</span></span>

<span data-ttu-id="2f714-122">hello nästa steg är tooretrieve hello säkerhet grupp visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="2f714-122">hello next step is tooretrieve hello security group view result.</span></span> <span data-ttu-id="2f714-123">Att lägga till hello ”--json” flaggan formaterar hello resulterar i json.</span><span class="sxs-lookup"><span data-stu-id="2f714-123">Adding hello "--json" flag will format hello results in json.</span></span>

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-hello-results"></a><span data-ttu-id="2f714-124">Visa hello resultat</span><span class="sxs-lookup"><span data-stu-id="2f714-124">Viewing hello results</span></span>

<span data-ttu-id="2f714-125">hello är följande exempel ett kortare svar hello resultat returneras.</span><span class="sxs-lookup"><span data-stu-id="2f714-125">hello following example is a shortened response of hello results returned.</span></span> <span data-ttu-id="2f714-126">hello resultatet visar alla hello effektiva och tillämpade säkerhetsregler på hello virtuella datorn är uppdelade i grupper med **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, och ** EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="2f714-126">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="2f714-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2f714-127">Next steps</span></span>

<span data-ttu-id="2f714-128">Besök [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md) toolearn hur tooautomate validering av Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="2f714-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>

<span data-ttu-id="2f714-129">Mer information om hello säkerhetsregler som tillämpade tooyour nätverksresurser genom att besöka [Säkerhetsöversikt grupp vy](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2f714-129">Learn more about hello security rules that are applied tooyour network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
