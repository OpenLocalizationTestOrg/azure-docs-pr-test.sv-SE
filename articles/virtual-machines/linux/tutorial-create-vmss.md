---
title: "Skapa en Skalningsuppsättningar i virtuella datorer för Linux i Azure | Microsoft Docs"
description: "Skapa och distribuera ett program som har hög tillgänglighet på virtuella Linux-datorer med hjälp av en skaluppsättning för virtuell dator"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.openlocfilehash: 2b8d519e11f70eda164bd8f6e131a3989f242ab0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a><span data-ttu-id="faad9-103">Skapa en Virtual Machine Scale Set och distribuera en app som har hög tillgänglighet på Linux</span><span class="sxs-lookup"><span data-stu-id="faad9-103">Create a Virtual Machine Scale Set and deploy a highly available app on Linux</span></span>
<span data-ttu-id="faad9-104">En skaluppsättning för virtuell dator kan du distribuera och hantera en uppsättning identiska, automatisk skalning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="faad9-104">A virtual machine scale set allows you to deploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="faad9-105">Du kan skala antalet virtuella datorer i skaluppsättning manuellt eller definiera regler för att kunna Autoskala baserat på CPU-användning, minne begäran eller nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="faad9-105">You can scale the number of VMs in the scale set manually, or define rules to autoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="faad9-106">I kursen får distribuera du en virtuell dator skala i Azure.</span><span class="sxs-lookup"><span data-stu-id="faad9-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="faad9-107">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="faad9-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="faad9-108">Använda molntjänster init för att skapa en app att skala</span><span class="sxs-lookup"><span data-stu-id="faad9-108">Use cloud-init to create an app to scale</span></span>
> * <span data-ttu-id="faad9-109">Skapa en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="faad9-109">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="faad9-110">Öka eller minska antalet instanser i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="faad9-110">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="faad9-111">Visa anslutningsinformation för scale set-instanser</span><span class="sxs-lookup"><span data-stu-id="faad9-111">View connection info for scale set instances</span></span>
> * <span data-ttu-id="faad9-112">Använda datadiskar i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="faad9-112">Use data disks in a scale set</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="faad9-113">Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="faad9-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="faad9-114">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="faad9-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="faad9-115">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="faad9-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="scale-set-overview"></a><span data-ttu-id="faad9-116">Skala Set-översikt</span><span class="sxs-lookup"><span data-stu-id="faad9-116">Scale Set overview</span></span>
<span data-ttu-id="faad9-117">En skaluppsättning för virtuell dator kan du distribuera och hantera en uppsättning identiska, automatisk skalning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="faad9-117">A virtual machine scale set allows you to deploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="faad9-118">Skalningsuppsättningar använda samma komponenter som du lärt dig i föregående guiden för att [skapa högtillgängliga virtuella datorer](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="faad9-118">Scale sets use the same components as you learned about in the previous tutorial to [Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="faad9-119">Virtuella datorer i en skaluppsättning skapas i en tillgänglighet ange och fördelad över logik fel- och update-domäner.</span><span class="sxs-lookup"><span data-stu-id="faad9-119">VMs in a scale set are created in an availability set and distributed across logic fault and update domains.</span></span>

<span data-ttu-id="faad9-120">Virtuella datorer skapas efter behov i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="faad9-120">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="faad9-121">Du kan definiera automatiska regler för att styra hur och när virtuella datorer läggs till eller tas bort från skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="faad9-121">You define autoscale rules to control how and when VMs are added or removed from the scale set.</span></span> <span data-ttu-id="faad9-122">De här reglerna kan utlösa baserat på mått som CPU-belastning, minnesanvändning eller nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="faad9-122">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="faad9-123">Skala anger stöd för upp till 1 000 virtuella datorer när du använder en avbildning i Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="faad9-123">Scale sets support up to 1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="faad9-124">För produktionsarbetsbelastningar, kan du [skapa en anpassad VM-avbildning](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="faad9-124">For production workloads, you may wish to [Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="faad9-125">Du kan skapa upp till 100 virtuella datorer i en skala som anges när du använder en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="faad9-125">You can create up to 100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-to-scale"></a><span data-ttu-id="faad9-126">Skapa en app att skala</span><span class="sxs-lookup"><span data-stu-id="faad9-126">Create an app to scale</span></span>
<span data-ttu-id="faad9-127">För produktion, kan du [skapa en anpassad VM-avbildning](tutorial-custom-images.md) som innehåller programmet installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="faad9-127">For production use, you may wish to [Create a custom VM image](tutorial-custom-images.md) that includes your application installed and configured.</span></span> <span data-ttu-id="faad9-128">Den här kursen kan du anpassa de virtuella datorerna på startas för första gången du snabbt vill se en skala som anges i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="faad9-128">For this tutorial, lets customize the VMs on first boot to quickly see a scale set in action.</span></span>

<span data-ttu-id="faad9-129">I en tidigare kursen du lärt dig [hur du anpassar en Linux-dator vid den första starten](tutorial-automate-vm-deployment.md) med molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="faad9-129">In a previous tutorial, you learned [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md) with cloud-init.</span></span> <span data-ttu-id="faad9-130">Du kan använda samma molnet init-konfigurationsfilen för att installera NGINX och köra en enkel ”Hello World” Node.js-app.</span><span class="sxs-lookup"><span data-stu-id="faad9-130">You can use the same cloud-init configuration file to install NGINX and run a simple 'Hello World' Node.js app.</span></span> 

<span data-ttu-id="faad9-131">Skapa en fil med namnet i din aktuella shell *moln init.txt* och klistra in följande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="faad9-131">In your current shell, create a file named *cloud-init.txt* and paste the following configuration.</span></span> <span data-ttu-id="faad9-132">Till exempel skapa filen i molnet Shell inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="faad9-132">For example, create the file in the Cloud Shell not on your local machine.</span></span> <span data-ttu-id="faad9-133">Ange `sensible-editor cloud-init.txt` att skapa filen och se en lista över tillgängliga redigerare.</span><span class="sxs-lookup"><span data-stu-id="faad9-133">Enter `sensible-editor cloud-init.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="faad9-134">Se till att hela molnet init-filen har kopierats korrekt, särskilt den första raden:</span><span class="sxs-lookup"><span data-stu-id="faad9-134">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```


## <a name="create-a-scale-set"></a><span data-ttu-id="faad9-135">Skapa en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="faad9-135">Create a scale set</span></span>
<span data-ttu-id="faad9-136">Innan du kan skapa en skalningsuppsättning, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="faad9-136">Before you can create a scale set, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="faad9-137">I följande exempel skapas en resursgrupp med namnet *myResourceGroupScaleSet* i den *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="faad9-137">The following example creates a resource group named *myResourceGroupScaleSet* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

<span data-ttu-id="faad9-138">Nu skapa en virtuell dator-skala med [az vmss skapa](/cli/azure/vmss#create).</span><span class="sxs-lookup"><span data-stu-id="faad9-138">Now create a virtual machine scale set with [az vmss create](/cli/azure/vmss#create).</span></span> <span data-ttu-id="faad9-139">I följande exempel skapas en uppsättning med namnet skala *myScaleSet*använder molnet init-filen för att anpassa den virtuella datorn och genererar SSH-nycklar, om de inte finns:</span><span class="sxs-lookup"><span data-stu-id="faad9-139">The following example creates a scale set named *myScaleSet*, uses the cloud-init file to customize the VM, and generates SSH keys if they do not exist:</span></span>

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

<span data-ttu-id="faad9-140">Det tar några minuter att skapa och konfigurera alla skala uppsättning resurser och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="faad9-140">It takes a few minutes to create and configure all the scale set resources and VMs.</span></span> <span data-ttu-id="faad9-141">Det finns bakgrundsaktiviteter för att fortsätta att köras när Azure CLI återgår till Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="faad9-141">There are background tasks that continue to run after the Azure CLI returns you to the prompt.</span></span> <span data-ttu-id="faad9-142">Det kan vara en annan några minuter innan du kan komma åt appen.</span><span class="sxs-lookup"><span data-stu-id="faad9-142">It may be another couple of minutes before you can access the app.</span></span>


## <a name="allow-web-traffic"></a><span data-ttu-id="faad9-143">Tillåt webbtrafik</span><span class="sxs-lookup"><span data-stu-id="faad9-143">Allow web traffic</span></span>
<span data-ttu-id="faad9-144">En belastningsutjämnare har skapats automatiskt som en del av virtuella datorns skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="faad9-144">A load balancer was created automatically as part of the virtual machine scale set.</span></span> <span data-ttu-id="faad9-145">Belastningsutjämnaren distribuerar trafik över en uppsättning definierade virtuella datorer med hjälp av regler för inläsning av belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="faad9-145">The load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="faad9-146">Du kan lära dig mer om belastningen belastningsutjämnaren koncept och konfigurationen i nästa kurs [så att belastningsutjämna virtuella datorer i Azure](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="faad9-146">You can learn more about load balancer concepts and configuration in the next tutorial, [How to load balance virtual machines in Azure](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="faad9-147">Skapa en regel med för att tillåta trafik till webbappen [az nätverket lb regeln skapa](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="faad9-147">To allow traffic to reach the web app, create a rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="faad9-148">I följande exempel skapas en regel med namnet *myLoadBalancerRuleWeb*:</span><span class="sxs-lookup"><span data-stu-id="faad9-148">The following example creates a rule named *myLoadBalancerRuleWeb*:</span></span>

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a><span data-ttu-id="faad9-149">Testa din app</span><span class="sxs-lookup"><span data-stu-id="faad9-149">Test your app</span></span>
<span data-ttu-id="faad9-150">Om du vill se din Node.js-app på webben, hämta offentlig IP-adressen för din belastningsutjämnare med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="faad9-150">To see your Node.js app on the web, obtain the public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="faad9-151">I följande exempel hämtar IP-adressen för *myScaleSetLBPublicIP* skapas som en del av skaluppsättning:</span><span class="sxs-lookup"><span data-stu-id="faad9-151">The following example obtains the IP address for *myScaleSetLBPublicIP* created as part of the scale set:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="faad9-152">Ange den offentliga IP-adressen i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="faad9-152">Enter the public IP address in to a web browser.</span></span> <span data-ttu-id="faad9-153">Appen visas, inklusive värdnamnet för den virtuella datorn som belastningsutjämnaren distribuerade trafik till:</span><span class="sxs-lookup"><span data-stu-id="faad9-153">The app is displayed, including the hostname of the VM that the load balancer distributed traffic to:</span></span>

![Node.js-app som körs](./media/tutorial-create-vmss/running-nodejs-app.png)

<span data-ttu-id="faad9-155">Om du vill se skaluppsättningen i praktiken du kan framtvinga-uppdatera webbläsaren om du vill se belastningsutjämnaren distribuerar trafik över alla de virtuella datorerna kör appen.</span><span class="sxs-lookup"><span data-stu-id="faad9-155">To see the scale set in action, you can force-refresh your web browser to see the load balancer distribute traffic across all the VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="faad9-156">Administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="faad9-156">Management tasks</span></span>
<span data-ttu-id="faad9-157">Du kan behöva köra en eller flera administrativa uppgifter i hela livscykeln för skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="faad9-157">Throughout the lifecycle of the scale set, you may need to run one or more management tasks.</span></span> <span data-ttu-id="faad9-158">Dessutom kanske du vill skapa skript som automatiserar olika livscykel-uppgifter.</span><span class="sxs-lookup"><span data-stu-id="faad9-158">Additionally, you may want to create scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="faad9-159">Azure CLI 2.0 tillhandahåller ett snabbt sätt att utföra dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="faad9-159">The Azure CLI 2.0 provides a quick way to do those tasks.</span></span> <span data-ttu-id="faad9-160">Här följer några vanliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="faad9-160">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="faad9-161">Visa virtuella datorer i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="faad9-161">View VMs in a scale set</span></span>
<span data-ttu-id="faad9-162">Du kan visa en lista över virtuella datorer som körs i en skaluppsättning [az vmss listinstanserna](/cli/azure/vmss#list-instances) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="faad9-162">To view a list of VMs running in your scale set, use [az vmss list-instances](/cli/azure/vmss#list-instances) as follows:</span></span>

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

<span data-ttu-id="faad9-163">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="faad9-163">The output is similar to the following example:</span></span>

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="faad9-164">Öka eller minska VM-instanser</span><span class="sxs-lookup"><span data-stu-id="faad9-164">Increase or decrease VM instances</span></span>
<span data-ttu-id="faad9-165">Om du vill se antalet instanser som du har för närvarande i en skaluppsättning [az vmss visa](/cli/azure/vmss#show) och fråga på *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="faad9-165">To see the number of instances you currently have in a scale set, use [az vmss show](/cli/azure/vmss#show) and query on *sku.capacity*:</span></span>

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

<span data-ttu-id="faad9-166">Du kan manuellt öka eller minska antalet virtuella datorer i skaluppsättningen med [az vmss skala](/cli/azure/vmss#scale).</span><span class="sxs-lookup"><span data-stu-id="faad9-166">You can then manually increase or decrease the number of virtual machines in the scale set with [az vmss scale](/cli/azure/vmss#scale).</span></span> <span data-ttu-id="faad9-167">I följande exempel anger hur många virtuella datorer i din skaluppsättningen *5*:</span><span class="sxs-lookup"><span data-stu-id="faad9-167">The following example sets the number of VMs in your scale set to *5*:</span></span>

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

<span data-ttu-id="faad9-168">Autoskala regler kan du definiera hur du skala upp eller ned antalet virtuella datorer i din skaluppsättningen som svar på begäran, till exempel nätverkstrafik eller CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="faad9-168">Autoscale rules let you define how to scale up or down the number of VMs in your scale set in response to demand such as network traffic or CPU usage.</span></span> <span data-ttu-id="faad9-169">De här reglerna kan för närvarande inte anges i Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="faad9-169">Currently, these rules cannot be set in Azure CLI 2.0.</span></span> <span data-ttu-id="faad9-170">Använd den [Azure-portalen](https://portal.azure.com) så här konfigurerar du Autoskala.</span><span class="sxs-lookup"><span data-stu-id="faad9-170">Use the [Azure portal](https://portal.azure.com) to configure autoscale.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="faad9-171">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="faad9-171">Get connection info</span></span>
<span data-ttu-id="faad9-172">Så här skaffar du anslutningsinformationen om de virtuella datorerna i din skaluppsättningar [az vmss lista-instans--anslutningsinformation](/cli/azure/vmss#list-instance-connection-info).</span><span class="sxs-lookup"><span data-stu-id="faad9-172">To obtain connection information about the VMs in your scale sets, use [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info).</span></span> <span data-ttu-id="faad9-173">Detta kommando offentlig IP-adress och port för varje virtuell dator där du kan ansluta med SSH:</span><span class="sxs-lookup"><span data-stu-id="faad9-173">This command outputs the public IP address and port for each VM that allows you to connect with SSH:</span></span>

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a><span data-ttu-id="faad9-174">Använda datadiskar med skaluppsättningar</span><span class="sxs-lookup"><span data-stu-id="faad9-174">Use data disks with scale sets</span></span>
<span data-ttu-id="faad9-175">Du kan skapa och använda datadiskar med skaluppsättningar.</span><span class="sxs-lookup"><span data-stu-id="faad9-175">You can create and use data disks with scale sets.</span></span> <span data-ttu-id="faad9-176">I en tidigare kursen du lärt dig hur du [hantera Azure-diskar](tutorial-manage-disks.md) som beskrivs bästa praxis och prestandaförbättringar för att skapa appar på datadiskar i stället för OS-disk.</span><span class="sxs-lookup"><span data-stu-id="faad9-176">In a previous tutorial, you learned how to [Manage Azure disks](tutorial-manage-disks.md) that outlines the best practices and performance improvements for building apps on data disks rather than the OS disk.</span></span>

### <a name="create-scale-set-with-data-disks"></a><span data-ttu-id="faad9-177">Skapa skaluppsättning med datadiskar</span><span class="sxs-lookup"><span data-stu-id="faad9-177">Create scale set with data disks</span></span>
<span data-ttu-id="faad9-178">Om du vill skapa en skalningsuppsättning och bifoga datadiskar, lägger du till den `--data-disk-sizes-gb` parametern till den [az vmss skapa](/cli/azure/vmss#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="faad9-178">To create a scale set and attach data disks, add the `--data-disk-sizes-gb` parameter to the [az vmss create](/cli/azure/vmss#create) command.</span></span> <span data-ttu-id="faad9-179">I följande exempel skapas en skala med *50*Gb datadiskar kopplade till varje instans:</span><span class="sxs-lookup"><span data-stu-id="faad9-179">The following example creates a scale set with *50*Gb data disks attached to each instance:</span></span>

```azurecli-interactive 
az vmss create \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetDisks \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --custom-data cloud-init.txt \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50
```

<span data-ttu-id="faad9-180">När instanser tas bort från en skaluppsättning för tas eventuella anslutna hårddiskar också bort.</span><span class="sxs-lookup"><span data-stu-id="faad9-180">When instances are removed from a scale set, any attached data disks are also removed.</span></span>

### <a name="add-data-disks"></a><span data-ttu-id="faad9-181">Lägg till datadiskar</span><span class="sxs-lookup"><span data-stu-id="faad9-181">Add data disks</span></span>
<span data-ttu-id="faad9-182">Lägg till en datadisk till instanser i en skaluppsättning för att använda [az vmss disk bifoga](/cli/azure/vmss/disk#attach).</span><span class="sxs-lookup"><span data-stu-id="faad9-182">To add a data disk to instances in your scale set, use [az vmss disk attach](/cli/azure/vmss/disk#attach).</span></span> <span data-ttu-id="faad9-183">I följande exempel läggs en *50*Gb disk till varje instans:</span><span class="sxs-lookup"><span data-stu-id="faad9-183">The following example adds a *50*Gb disk to each instance:</span></span>

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a><span data-ttu-id="faad9-184">Koppla från datadiskar</span><span class="sxs-lookup"><span data-stu-id="faad9-184">Detach data disks</span></span>
<span data-ttu-id="faad9-185">Ta bort en datadisk till instanser i en skaluppsättning med [az vmss disk frånkoppling](/cli/azure/vmss/disk#detach).</span><span class="sxs-lookup"><span data-stu-id="faad9-185">To remove a data disk to instances in your scale set, use [az vmss disk detach](/cli/azure/vmss/disk#detach).</span></span> <span data-ttu-id="faad9-186">I följande exempel tar bort datadisk på LUN *2* från varje instans:</span><span class="sxs-lookup"><span data-stu-id="faad9-186">The following example removes the data disk at LUN *2* from each instance:</span></span>

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a><span data-ttu-id="faad9-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="faad9-187">Next steps</span></span>
<span data-ttu-id="faad9-188">Du har skapat en skaluppsättning för virtuell dator i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="faad9-188">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="faad9-189">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="faad9-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="faad9-190">Använda molntjänster init för att skapa en app att skala</span><span class="sxs-lookup"><span data-stu-id="faad9-190">Use cloud-init to create an app to scale</span></span>
> * <span data-ttu-id="faad9-191">Skapa en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="faad9-191">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="faad9-192">Öka eller minska antalet instanser i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="faad9-192">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="faad9-193">Visa anslutningsinformation för scale set-instanser</span><span class="sxs-lookup"><span data-stu-id="faad9-193">View connection info for scale set instances</span></span>
> * <span data-ttu-id="faad9-194">Använda datadiskar i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="faad9-194">Use data disks in a scale set</span></span>

<span data-ttu-id="faad9-195">Gå vidare till nästa kurs att lära dig mer om koncept för virtuella datorer för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="faad9-195">Advance to the next tutorial to learn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="faad9-196">Belastningsutjämna virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="faad9-196">Load balance virtual machines</span></span>](tutorial-load-balancer.md)