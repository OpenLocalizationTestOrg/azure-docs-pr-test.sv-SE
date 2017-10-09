---
title: aaaAzure Snabbstart - skapa Windows VM CLI | Microsoft Docs
description: "Lär dig snabbt toocreate en Windows-datorer med hello Azure CLI."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 029bdecec219b12b80b958ceeedda214f1b13149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="40303-103">Skapa en virtuell Windows-dator med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="40303-103">Create a Windows virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="40303-104">hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="40303-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="40303-105">Den här guiden information med hjälp av hello Azure CLI toodeploy en virtuell dator som kör Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="40303-105">This guide details using hello Azure CLI toodeploy a virtual machine running Windows Server 2016.</span></span> <span data-ttu-id="40303-106">När installationen är klar använder vi ansluter toohello server och installera IIS.</span><span class="sxs-lookup"><span data-stu-id="40303-106">Once deployment is complete, we connect toohello server and install IIS.</span></span>

<span data-ttu-id="40303-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="40303-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="40303-108">Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="40303-108">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="40303-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="40303-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="40303-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="40303-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="create-a-resource-group"></a><span data-ttu-id="40303-111">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="40303-111">Create a resource group</span></span>

<span data-ttu-id="40303-112">Skapa en resursgrupp med [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="40303-112">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="40303-113">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="40303-113">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="40303-114">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="40303-114">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="40303-115">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="40303-115">Create virtual machine</span></span>

<span data-ttu-id="40303-116">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="40303-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> 

<span data-ttu-id="40303-117">hello följande exempel skapas en virtuell dator med namnet *myVM*.</span><span class="sxs-lookup"><span data-stu-id="40303-117">hello following example creates a VM named *myVM*.</span></span> <span data-ttu-id="40303-118">Det här exemplet används *azureuser* för en administrativ användarnamn och *myPassword12* som hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="40303-118">This example uses *azureuser* for an administrative user name and *myPassword12* as hello password.</span></span> <span data-ttu-id="40303-119">Uppdatera dessa värden toosomething lämpliga tooyour miljö.</span><span class="sxs-lookup"><span data-stu-id="40303-119">Update these values toosomething appropriate tooyour environment.</span></span> <span data-ttu-id="40303-120">Dessa värden krävs när du skapar en anslutning med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="40303-120">These values are needed when creating a connection with hello virtual machine.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

<span data-ttu-id="40303-121">När du har skapat hello VM visar hello Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="40303-121">When hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="40303-122">Anteckna hello `publicIpAaddress`.</span><span class="sxs-lookup"><span data-stu-id="40303-122">Take note of hello `publicIpAaddress`.</span></span> <span data-ttu-id="40303-123">Den här adressen är används tooaccess hello VM.</span><span class="sxs-lookup"><span data-stu-id="40303-123">This address is used tooaccess hello VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="40303-124">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="40303-124">Open port 80 for web traffic</span></span> 

<span data-ttu-id="40303-125">Som standard tillåts bara en RDP-anslutningar i tooWindows virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="40303-125">By default only RDP connections are allowed in tooWindows virtual machines deployed in Azure.</span></span> <span data-ttu-id="40303-126">Om den här virtuella datorn ska toobe en webbserver, måste tooopen port 80 från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="40303-126">If this VM is going toobe a webserver, you need tooopen port 80 from hello Internet.</span></span> <span data-ttu-id="40303-127">Använd hello [az vm öppna port](/cli/azure/vm#open-port) kommandot tooopen hello önskad port.</span><span class="sxs-lookup"><span data-stu-id="40303-127">Use hello [az vm open-port](/cli/azure/vm#open-port) command tooopen hello desired port.</span></span>  
 
 ```azurecli-interactive  
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```


## <a name="connect-toovirtual-machine"></a><span data-ttu-id="40303-128">Ansluta toovirtual datorn</span><span class="sxs-lookup"><span data-stu-id="40303-128">Connect toovirtual machine</span></span>

<span data-ttu-id="40303-129">Använd hello följande kommando toocreate en fjärrskrivbordssession med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="40303-129">Use hello following command toocreate a remote desktop session with hello virtual machine.</span></span> <span data-ttu-id="40303-130">Ersätt hello IP-adress med hello offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="40303-130">Replace hello IP address with hello public IP address of your virtual machine.</span></span> <span data-ttu-id="40303-131">När du uppmanas ange hello-autentiseringsuppgifter som används när du skapar hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="40303-131">When prompted, enter hello credentials used when creating hello virtual machine.</span></span>

```bash 
mstsc /v:<Public IP Address>
```

## <a name="install-iis-using-powershell"></a><span data-ttu-id="40303-132">Installera IIS med PowerShell</span><span class="sxs-lookup"><span data-stu-id="40303-132">Install IIS using PowerShell</span></span>

<span data-ttu-id="40303-133">Nu när du har loggat in toohello Azure VM, kan du använda en rad med PowerShell tooinstall IIS och aktivera hello lokala brandväggen regeln tooallow webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="40303-133">Now that you have logged in toohello Azure VM, you can use a single line of PowerShell tooinstall IIS and enable hello local firewall rule tooallow web traffic.</span></span> <span data-ttu-id="40303-134">Öppna en PowerShell-kommandotolk och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="40303-134">Open a PowerShell prompt and run hello following command:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="40303-135">Visa hello välkomstsidan för IIS</span><span class="sxs-lookup"><span data-stu-id="40303-135">View hello IIS welcome page</span></span>

<span data-ttu-id="40303-136">Du kan använda en webbläsare för din choice tooview hello IIS välkomstsidan med IIS installerat och port 80 som nu är öppet på den virtuella datorn från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="40303-136">With IIS installed and port 80 now open on your VM from hello Internet, you can use a web browser of your choice tooview hello default IIS welcome page.</span></span> <span data-ttu-id="40303-137">Vara säker på att toouse hello offentliga IP-adressen du dokumenterade över toovisit hello standardsida.</span><span class="sxs-lookup"><span data-stu-id="40303-137">Be sure toouse hello public IP address you documented above toovisit hello default page.</span></span> 

![Standardwebbplatsen i IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="40303-139">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="40303-139">Clean up resources</span></span>

<span data-ttu-id="40303-140">När du inte längre behövs kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="40303-140">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="40303-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="40303-141">Next steps</span></span>

<span data-ttu-id="40303-142">I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver.</span><span class="sxs-lookup"><span data-stu-id="40303-142">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="40303-143">toolearn mer om Azure-datorer, fortsätta toohello självstudier för virtuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="40303-143">toolearn more about Azure virtual machines, continue toohello tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="40303-144">Självstudier om virtuella Azure Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="40303-144">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
