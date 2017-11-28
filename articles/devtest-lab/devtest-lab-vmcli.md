---
title: Skapa och hantera virtuella datorer i DevTest Labs med Azure CLI | Microsoft Docs
description: "Lär dig hur du använder Azure DevTest Labs för att skapa och hantera virtuella datorer med Azure CLI 2.0"
services: devtest-lab,virtual-machines
documentationcenter: na
author: lisawong19
manager: douge
editor: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: liwong
ms.openlocfilehash: 42b0448c1bcdfa909715abd5075353d63cab8389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a><span data-ttu-id="3d1b8-103">Skapa och hantera virtuella datorer med DevTest Labs med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3d1b8-103">Create and manage virtual machines with DevTest Labs using the Azure CLI</span></span>
<span data-ttu-id="3d1b8-104">Den här snabbstartsguide vägleder dig genom att skapa, starta, ansluta, uppdaterar och rensar en utvecklingsdator i labbet.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-104">This quick start will guide you through creating, starting, connecting, updating and cleaning up a development machine in your lab.</span></span> 

<span data-ttu-id="3d1b8-105">Innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="3d1b8-105">Before you begin:</span></span>

* <span data-ttu-id="3d1b8-106">Om ett labb som inte har skapats anvisningar finns [här](devtest-lab-create-lab.md).</span><span class="sxs-lookup"><span data-stu-id="3d1b8-106">If a lab has not been created, instructions can be found [here](devtest-lab-create-lab.md).</span></span>

* <span data-ttu-id="3d1b8-107">[Installera CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3d1b8-107">[Install CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="3d1b8-108">Starta, köra az in för att skapa en anslutning med Azure.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-108">To start, run az login to create a connection with Azure.</span></span> 

## <a name="create-and-verify-the-virtual-machine"></a><span data-ttu-id="3d1b8-109">Skapa och kontrollera den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="3d1b8-109">Create and verify the virtual machine</span></span> 
<span data-ttu-id="3d1b8-110">Skapa en virtuell dator från en marketplace-avbildning med ssh autentisering.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-110">Create a VM from a marketplace image with ssh authentication.</span></span>
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> <span data-ttu-id="3d1b8-111">Placera den **resursgrupp för labbets** i--resursgrupp parametern.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-111">Put the **lab's resource group** name in the --resource-group parameter.</span></span>
>

<span data-ttu-id="3d1b8-112">Om du vill skapa en virtuell dator med en formel, Använd--formeln parametern i [az lab vm skapa](https://docs.microsoft.com/cli/azure/lab/vm#create).</span><span class="sxs-lookup"><span data-stu-id="3d1b8-112">If you want to create a VM using a formula, use the --formula parameter in [az lab vm create](https://docs.microsoft.com/cli/azure/lab/vm#create).</span></span>


<span data-ttu-id="3d1b8-113">Kontrollera att den virtuella datorn är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-113">Verify that the VM is available.</span></span>
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand 'properties($expand=ComputeVm,NetworkInterface)' --query '{status: computeVm.statuses[0].displayStatus, fqdn: fqdn, ipAddress: networkInterface.publicIpAddress}'
```
```json
{
  "fqdn": "lisalabvm.southcentralus.cloudapp.azure.com",
  "ipAddress": "13.85.228.112",
  "status": "Provisioning succeeded"
}
```

## <a name="start-and-connect-to-the-virtual-machine"></a><span data-ttu-id="3d1b8-114">Starta och Anslut till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="3d1b8-114">Start and connect to the virtual machine</span></span>
<span data-ttu-id="3d1b8-115">Starta en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-115">Start a VM.</span></span>
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> <span data-ttu-id="3d1b8-116">Placera den **resursgrupp för labbets** i--resursgrupp parametern.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-116">Put the **lab's resource group** name in the --resource-group parameter.</span></span>
>

<span data-ttu-id="3d1b8-117">Ansluta till en virtuell dator: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) eller [fjärrskrivbord](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="3d1b8-117">Connect to a VM: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) or [Remote Desktop](../virtual-machines/windows/connect-logon.md).</span></span>
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a><span data-ttu-id="3d1b8-118">Uppdatera den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="3d1b8-118">Update the virtual machine</span></span>
<span data-ttu-id="3d1b8-119">Tillämpa artefakter för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-119">Apply artifacts to a VM.</span></span>
```azurecli
az lab vm apply-artifacts --lab-name  sampleLabName --name sampleVMName  --resource-group sampleResourceGroup  --artifacts @/artifacts.json
```

```json
[
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-java",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-install-nodejs",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-apt-package",
    "parameters": [
      {
        "name": "packages",
        "value": "abcd"
      },
      {
        "name": "update",
        "value": "true"
      },
      {
        "name": "options",
        "value": ""
      }
    ]
  } 
]
```

<span data-ttu-id="3d1b8-120">Lista artefakter i labbet.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-120">List artifacts available in the lab.</span></span>
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a><span data-ttu-id="3d1b8-121">Stoppa och ta bort den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="3d1b8-121">Stop and delete the virtual machine</span></span>    
<span data-ttu-id="3d1b8-122">Stoppa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-122">Stop a VM.</span></span>
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

<span data-ttu-id="3d1b8-123">Ta bort en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3d1b8-123">Delete a VM.</span></span>
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]