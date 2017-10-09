---
title: aaaCreate och hantera virtuella datorer i DevTest Labs med Azure CLI | Microsoft Docs
description: "Lär dig hur toouse Azure DevTest Labs toocreate och hantera virtuella datorer med Azure CLI 2.0"
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
ms.openlocfilehash: 98ea3aa7b2489bff61971573aaf584984cd811e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-hello-azure-cli"></a><span data-ttu-id="651ba-103">Skapa och hantera virtuella datorer med DevTest Labs med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="651ba-103">Create and manage virtual machines with DevTest Labs using hello Azure CLI</span></span>
<span data-ttu-id="651ba-104">Den här snabbstartsguide vägleder dig genom att skapa, starta, ansluta, uppdaterar och rensar en utvecklingsdator i labbet.</span><span class="sxs-lookup"><span data-stu-id="651ba-104">This quick start will guide you through creating, starting, connecting, updating and cleaning up a development machine in your lab.</span></span> 

<span data-ttu-id="651ba-105">Innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="651ba-105">Before you begin:</span></span>

* <span data-ttu-id="651ba-106">Om ett labb som inte har skapats anvisningar finns [här](devtest-lab-create-lab.md).</span><span class="sxs-lookup"><span data-stu-id="651ba-106">If a lab has not been created, instructions can be found [here](devtest-lab-create-lab.md).</span></span>

* <span data-ttu-id="651ba-107">[Installera CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="651ba-107">[Install CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="651ba-108">toostart kör az inloggningen toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="651ba-108">toostart, run az login toocreate a connection with Azure.</span></span> 

## <a name="create-and-verify-hello-virtual-machine"></a><span data-ttu-id="651ba-109">Skapa och verifiera hello virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="651ba-109">Create and verify hello virtual machine</span></span> 
<span data-ttu-id="651ba-110">Skapa en virtuell dator från en marketplace-avbildning med ssh autentisering.</span><span class="sxs-lookup"><span data-stu-id="651ba-110">Create a VM from a marketplace image with ssh authentication.</span></span>
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> <span data-ttu-id="651ba-111">Placera hello **resursgrupp för labbets** i hello--resursgrupp parametern.</span><span class="sxs-lookup"><span data-stu-id="651ba-111">Put hello **lab's resource group** name in hello --resource-group parameter.</span></span>
>

<span data-ttu-id="651ba-112">Om du vill toocreate en virtuell dator med en formel, använda hello--formeln parameter i [az lab vm skapa](https://docs.microsoft.com/cli/azure/lab/vm#create).</span><span class="sxs-lookup"><span data-stu-id="651ba-112">If you want toocreate a VM using a formula, use hello --formula parameter in [az lab vm create](https://docs.microsoft.com/cli/azure/lab/vm#create).</span></span>


<span data-ttu-id="651ba-113">Kontrollera att hello VM är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="651ba-113">Verify that hello VM is available.</span></span>
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

## <a name="start-and-connect-toohello-virtual-machine"></a><span data-ttu-id="651ba-114">Starta och Anslut toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="651ba-114">Start and connect toohello virtual machine</span></span>
<span data-ttu-id="651ba-115">Starta en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="651ba-115">Start a VM.</span></span>
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> <span data-ttu-id="651ba-116">Placera hello **resursgrupp för labbets** i hello--resursgrupp parametern.</span><span class="sxs-lookup"><span data-stu-id="651ba-116">Put hello **lab's resource group** name in hello --resource-group parameter.</span></span>
>

<span data-ttu-id="651ba-117">Ansluta tooa VM: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) eller [fjärrskrivbord](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="651ba-117">Connect tooa VM: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) or [Remote Desktop](../virtual-machines/windows/connect-logon.md).</span></span>
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-hello-virtual-machine"></a><span data-ttu-id="651ba-118">Uppdatera hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="651ba-118">Update hello virtual machine</span></span>
<span data-ttu-id="651ba-119">Tillämpa artefakter tooa VM.</span><span class="sxs-lookup"><span data-stu-id="651ba-119">Apply artifacts tooa VM.</span></span>
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

<span data-ttu-id="651ba-120">Lista artefakter i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="651ba-120">List artifacts available in hello lab.</span></span>
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-hello-virtual-machine"></a><span data-ttu-id="651ba-121">Stoppa och ta bort hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="651ba-121">Stop and delete hello virtual machine</span></span>    
<span data-ttu-id="651ba-122">Stoppa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="651ba-122">Stop a VM.</span></span>
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

<span data-ttu-id="651ba-123">Ta bort en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="651ba-123">Delete a VM.</span></span>
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]