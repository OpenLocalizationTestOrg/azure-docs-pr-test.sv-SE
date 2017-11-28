---
title: "aaaAnalyze nätverkssäkerhet med Azure Network Watcher säkerhet gruppvyn - REST API | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse PowerShell tooanalyze en virtuella datorer säkerhet med Gruppvy för säkerhet."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a2f418fe-f5d2-43ed-8dc3-df0ed2a4d4ac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 0858a64a9454816e05f06dadb9536ad0c755e90e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-rest-api"></a><span data-ttu-id="c5d4a-103">Analysera dina virtuella säkerhet med säkerhet gruppvyn med hjälp av REST API</span><span class="sxs-lookup"><span data-stu-id="c5d4a-103">Analyze your Virtual Machine security with Security Group View using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="c5d4a-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5d4a-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="c5d4a-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c5d4a-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="c5d4a-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c5d4a-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="c5d4a-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="c5d4a-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="c5d4a-108">Säkerhet gruppvyn returnerar konfigurerade och effektivt Nätverkssäkerhetsregler som är kopplade tooa virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="c5d4a-109">Den här funktionen är användbart tooaudit och diagnostisera Nätverkssäkerhetsgrupper och regler som är konfigurerade på en VM tooensure trafik som tillåts eller nekas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="c5d4a-110">I den här artikeln visa vi hur tooretrieve hello effektiva och tillämpade säkerhetsregler tooa virtuell dator med hjälp av REST API</span><span class="sxs-lookup"><span data-stu-id="c5d4a-110">In this article, we show you how tooretrieve hello effective and applied security rules tooa virtual machine using REST API</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c5d4a-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c5d4a-111">Before you begin</span></span>

<span data-ttu-id="c5d4a-112">I detta scenario anropa du hello nätverket Watcher Rest API tooget hello säkerhet gruppvyn för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-112">In this scenario, you call hello Network Watcher Rest API tooget hello security group view for a virtual machine.</span></span> <span data-ttu-id="c5d4a-113">ARMclient är används toocall hello REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-113">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="c5d4a-114">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="c5d4a-114">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="c5d4a-115">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="c5d4a-116">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-116">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="c5d4a-117">Scenario</span><span class="sxs-lookup"><span data-stu-id="c5d4a-117">Scenario</span></span>

<span data-ttu-id="c5d4a-118">hello-scenario som beskrivs i den här artikeln hämtar hello effektiva och tillämpade säkerhetsregler för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-118">hello scenario covered in this article retrieves hello effective and applied security rules for a given virtual machine.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="c5d4a-119">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="c5d4a-119">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="c5d4a-120">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c5d4a-120">Retrieve a virtual machine</span></span>

<span data-ttu-id="c5d4a-121">Kör följande skript tooreturn virtuella machineThe hello måste följande kod variabler:</span><span class="sxs-lookup"><span data-stu-id="c5d4a-121">Run hello following script tooreturn a virtual machineThe following code needs variables:</span></span>

- <span data-ttu-id="c5d4a-122">**subscriptionId** -hello prenumerations-id kan också hämtas med hello **Get-AzureRMSubscription** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-122">**subscriptionId** - hello subscription id can also be retrieved with hello **Get-AzureRMSubscription** cmdlet.</span></span>
- <span data-ttu-id="c5d4a-123">**resourceGroupName** - hello namn på en resursgrupp som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-123">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="c5d4a-124">hello information som behövs är hello **id** under hello typ `Microsoft.Compute/virtualMachines` svar som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="c5d4a-124">hello information that is needed is hello **id** under hello type `Microsoft.Compute/virtualMachines` in response, as seen in hello following example:</span></span>

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft
.Network/networkInterfaces/{nicName}"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Com
pute/virtualMachines/{vmName}/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute
/virtualMachines/{vmName}",
      "name": "{vmName}"
    }
  ]
}
```

## <a name="get-security-group-view-for-virtual-machine"></a><span data-ttu-id="c5d4a-125">Hämta gruppvy för säkerhet för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c5d4a-125">Get security group view for virtual machine</span></span>

<span data-ttu-id="c5d4a-126">följande exempel hello begär hello säkerhet gruppvyn för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-126">hello following example requests hello security group view of a targeted virtual machine.</span></span> <span data-ttu-id="c5d4a-127">hello resultat från det här exemplet kan vara används toocompare toohello regler och säkerhet som definierats av hello ursprung toolook konfigurationsavvikelser.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-127">hello results from this example can be used toocompare toohello rules and security defined by hello origination toolook for configuration drift.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName

$requestBody = @"
{
    'targetResourceId': '${targetUri}'

}
"@
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/securityGroupView?api-version=2016-12-01" $requestBody -verbose
```

## <a name="view-hello-response"></a><span data-ttu-id="c5d4a-128">Visa hello svar</span><span class="sxs-lookup"><span data-stu-id="c5d4a-128">View hello response</span></span>

<span data-ttu-id="c5d4a-129">följande exempel hello är hello svaret som returnerades från hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-129">hello following sample is hello response returned from hello preceding command.</span></span> <span data-ttu-id="c5d4a-130">hello resultatet visar alla hello effektiva och tillämpade säkerhetsregler på hello virtuella datorn är uppdelade i grupper med **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, och  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-130">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json

{
  "networkInterfaces": [
    {
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "securityRules": [
            {
              "name": "default-allow-rdp",
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
              "properties": {
                "provisioningState": "Succeeded",
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "3389",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 1000,
                "direction": "Inbound"
              }
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "name": "AllowVnetInBound",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/",
            "properties": {
              "provisioningState": "Succeeded",
              "description": "Allow inbound traffic from all VMs in VNET",
              "protocol": "*",
              "sourcePortRange": "*",
              "destinationPortRange": "*",
              "sourceAddressPrefix": "VirtualNetwork",
              "destinationAddressPrefix": "VirtualNetwork",
              "access": "Allow",
              "priority": 65000,
              "direction": "Inbound"
            }
          },
          ...
        ],
        "effectiveSecurityRules": [
          {
            "name": "DefaultOutboundDenyAll",
            "protocol": "All",
            "sourcePortRange": "0-65535",
            "destinationPortRange": "0-65535",
            "sourceAddressPrefix": "*",
            "destinationAddressPrefix": "*",
            "access": "Deny",
            "priority": 65500,
            "direction": "Outbound"
          },
          ...
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="c5d4a-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5d4a-131">Next steps</span></span>

<span data-ttu-id="c5d4a-132">Besök [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-security-group-view-powershell.md) toolearn hur tooautomate validering av Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="c5d4a-132">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-security-group-view-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>


