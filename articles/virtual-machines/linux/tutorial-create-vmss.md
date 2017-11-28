---
title: "aaaCreate en Skalningsuppsättningar i virtuella datorer för Linux i Azure | Microsoft Docs"
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
ms.openlocfilehash: 00dd81043f9be46ef2dc6dfe97eefdb20944ee13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a><span data-ttu-id="b66bc-103">Skapa en Virtual Machine Scale Set och distribuera en app som har hög tillgänglighet på Linux</span><span class="sxs-lookup"><span data-stu-id="b66bc-103">Create a Virtual Machine Scale Set and deploy a highly available app on Linux</span></span>
<span data-ttu-id="b66bc-104">En skaluppsättning för virtuell dator kan du toodeploy och hantera en uppsättning identiska, automatisk skalning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b66bc-104">A virtual machine scale set allows you toodeploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="b66bc-105">Du kan skala hello antal virtuella datorer i hello skaluppsättning manuellt eller definiera regler tooautoscale baserat på CPU-användning, minne begäran eller nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="b66bc-105">You can scale hello number of VMs in hello scale set manually, or define rules tooautoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="b66bc-106">I kursen får distribuera du en virtuell dator skala i Azure.</span><span class="sxs-lookup"><span data-stu-id="b66bc-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="b66bc-107">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="b66bc-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b66bc-108">Använda molntjänster init toocreate tooscale en app</span><span class="sxs-lookup"><span data-stu-id="b66bc-108">Use cloud-init toocreate an app tooscale</span></span>
> * <span data-ttu-id="b66bc-109">Skapa en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b66bc-109">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="b66bc-110">Öka eller minska hello antalet instanser i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="b66bc-110">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="b66bc-111">Visa anslutningsinformation för scale set-instanser</span><span class="sxs-lookup"><span data-stu-id="b66bc-111">View connection info for scale set instances</span></span>
> * <span data-ttu-id="b66bc-112">Använda datadiskar i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="b66bc-112">Use data disks in a scale set</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b66bc-113">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b66bc-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b66bc-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="b66bc-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="b66bc-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b66bc-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="scale-set-overview"></a><span data-ttu-id="b66bc-116">Skala Set-översikt</span><span class="sxs-lookup"><span data-stu-id="b66bc-116">Scale Set overview</span></span>
<span data-ttu-id="b66bc-117">En skaluppsättning för virtuell dator kan du toodeploy och hantera en uppsättning identiska, automatisk skalning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b66bc-117">A virtual machine scale set allows you toodeploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="b66bc-118">Skalningsuppsättningarna Använd hello samma komponenter som du har läst om i hello tidigare självstudier för[skapa högtillgängliga virtuella datorer](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="b66bc-118">Scale sets use hello same components as you learned about in hello previous tutorial too[Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="b66bc-119">Virtuella datorer i en skaluppsättning skapas i en tillgänglighet ange och fördelad över logik fel- och update-domäner.</span><span class="sxs-lookup"><span data-stu-id="b66bc-119">VMs in a scale set are created in an availability set and distributed across logic fault and update domains.</span></span>

<span data-ttu-id="b66bc-120">Virtuella datorer skapas efter behov i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="b66bc-120">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="b66bc-121">Du kan definiera automatiska regler toocontrol hur och när virtuella datorer läggs till eller tas bort från hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="b66bc-121">You define autoscale rules toocontrol how and when VMs are added or removed from hello scale set.</span></span> <span data-ttu-id="b66bc-122">De här reglerna kan utlösa baserat på mått som CPU-belastning, minnesanvändning eller nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="b66bc-122">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="b66bc-123">Skala anger stöd upp too1 000 virtuella datorer när du använder en avbildning i Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="b66bc-123">Scale sets support up too1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="b66bc-124">För produktionsarbetsbelastningar gärna för[skapa en anpassad VM-avbildning](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="b66bc-124">For production workloads, you may wish too[Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="b66bc-125">Du kan skapa upp too100 virtuella datorer i en skala som anges när du använder en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="b66bc-125">You can create up too100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-tooscale"></a><span data-ttu-id="b66bc-126">Skapa en app tooscale</span><span class="sxs-lookup"><span data-stu-id="b66bc-126">Create an app tooscale</span></span>
<span data-ttu-id="b66bc-127">För produktion, gärna för[skapa en anpassad VM-avbildning](tutorial-custom-images.md) som innehåller programmet installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="b66bc-127">For production use, you may wish too[Create a custom VM image](tutorial-custom-images.md) that includes your application installed and configured.</span></span> <span data-ttu-id="b66bc-128">För den här kursen kan du anpassa hello virtuella datorer på den första start tooquickly finns en skala som anges i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b66bc-128">For this tutorial, lets customize hello VMs on first boot tooquickly see a scale set in action.</span></span>

<span data-ttu-id="b66bc-129">I en tidigare kursen du lärt dig [hur toocustomize en Linux-dator vid den första starten](tutorial-automate-vm-deployment.md) med molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="b66bc-129">In a previous tutorial, you learned [How toocustomize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md) with cloud-init.</span></span> <span data-ttu-id="b66bc-130">Du kan använda hello samma molnet init configuration file tooinstall NGINX och kör en enkel Hello World Node.js-app.</span><span class="sxs-lookup"><span data-stu-id="b66bc-130">You can use hello same cloud-init configuration file tooinstall NGINX and run a simple 'Hello World' Node.js app.</span></span> 

<span data-ttu-id="b66bc-131">Skapa en fil med namnet i din aktuella shell *moln init.txt* och klistra in hello följande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b66bc-131">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="b66bc-132">Till exempel skapa hello-filen i hello molnet Shell inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="b66bc-132">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="b66bc-133">Ange `sensible-editor cloud-init.txt` toocreate hello filen och visas i listan över tillgängliga redigerare.</span><span class="sxs-lookup"><span data-stu-id="b66bc-133">Enter `sensible-editor cloud-init.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="b66bc-134">Kontrollera att filen hello hela molnet initiering kopierats korrekt, särskilt hello första raden:</span><span class="sxs-lookup"><span data-stu-id="b66bc-134">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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


## <a name="create-a-scale-set"></a><span data-ttu-id="b66bc-135">Skapa en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="b66bc-135">Create a scale set</span></span>
<span data-ttu-id="b66bc-136">Innan du kan skapa en skalningsuppsättning, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="b66bc-136">Before you can create a scale set, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="b66bc-137">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupScaleSet* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="b66bc-137">hello following example creates a resource group named *myResourceGroupScaleSet* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

<span data-ttu-id="b66bc-138">Nu skapa en virtuell dator-skala med [az vmss skapa](/cli/azure/vmss#create).</span><span class="sxs-lookup"><span data-stu-id="b66bc-138">Now create a virtual machine scale set with [az vmss create](/cli/azure/vmss#create).</span></span> <span data-ttu-id="b66bc-139">hello följande exempel skapas en uppsättning med namnet skala *myScaleSet*, använder hello molnet init filen toocustomize hello VM och genererar SSH-nycklar, om de inte finns:</span><span class="sxs-lookup"><span data-stu-id="b66bc-139">hello following example creates a scale set named *myScaleSet*, uses hello cloud-init file toocustomize hello VM, and generates SSH keys if they do not exist:</span></span>

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

<span data-ttu-id="b66bc-140">Det tar några minuter toocreate och konfigurera alla hello skala uppsättning resurser och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b66bc-140">It takes a few minutes toocreate and configure all hello scale set resources and VMs.</span></span> <span data-ttu-id="b66bc-141">Det finns bakgrundsaktiviteter för att fortsätta toorun när hello Azure CLI returnerar toohello prompt.</span><span class="sxs-lookup"><span data-stu-id="b66bc-141">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="b66bc-142">Det kan vara en annan några minuter innan du kan komma åt hello app.</span><span class="sxs-lookup"><span data-stu-id="b66bc-142">It may be another couple of minutes before you can access hello app.</span></span>


## <a name="allow-web-traffic"></a><span data-ttu-id="b66bc-143">Tillåt webbtrafik</span><span class="sxs-lookup"><span data-stu-id="b66bc-143">Allow web traffic</span></span>
<span data-ttu-id="b66bc-144">En belastningsutjämnare har skapats automatiskt som en del av hello virtuella datorns skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="b66bc-144">A load balancer was created automatically as part of hello virtual machine scale set.</span></span> <span data-ttu-id="b66bc-145">hello belastningsutjämnare distribuerar trafik över en uppsättning definierade virtuella datorer med hjälp av regler för inläsning av belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="b66bc-145">hello load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="b66bc-146">Du kan lära dig mer om belastningen belastningsutjämnaren koncept och konfigurationen i nästa kurs för hello, [hur tooload balansera virtuella datorer i Azure](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="b66bc-146">You can learn more about load balancer concepts and configuration in hello next tutorial, [How tooload balance virtual machines in Azure](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="b66bc-147">tooallow trafik tooreach hello webbapp skapar en regel med [az nätverket lb regeln skapa](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="b66bc-147">tooallow traffic tooreach hello web app, create a rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="b66bc-148">hello följande exempel skapas en regel med namnet *myLoadBalancerRuleWeb*:</span><span class="sxs-lookup"><span data-stu-id="b66bc-148">hello following example creates a rule named *myLoadBalancerRuleWeb*:</span></span>

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

## <a name="test-your-app"></a><span data-ttu-id="b66bc-149">Testa din app</span><span class="sxs-lookup"><span data-stu-id="b66bc-149">Test your app</span></span>
<span data-ttu-id="b66bc-150">toosee Node.js-appen på hello web hämta hello offentliga IP-adressen för din belastningsutjämnare med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="b66bc-150">toosee your Node.js app on hello web, obtain hello public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="b66bc-151">hello följande exempel hämtar hello IP-adress för *myScaleSetLBPublicIP* skapas som en del av skaluppsättning för hello:</span><span class="sxs-lookup"><span data-stu-id="b66bc-151">hello following example obtains hello IP address for *myScaleSetLBPublicIP* created as part of hello scale set:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="b66bc-152">Ange hello offentliga IP-adressen i tooa webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b66bc-152">Enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="b66bc-153">hello app visas, inklusive hello värdnamnet för hello VM som hello läsa in belastningsutjämning distribuerade trafik till:</span><span class="sxs-lookup"><span data-stu-id="b66bc-153">hello app is displayed, including hello hostname of hello VM that hello load balancer distributed traffic to:</span></span>

![Node.js-app som körs](./media/tutorial-create-vmss/running-nodejs-app.png)

<span data-ttu-id="b66bc-155">toosee hello skaluppsättningen i praktiken, du kan kraft uppdatera din webbplats webbläsare toosee hello belastningen belastningsutjämnare distribuerar trafik över alla hello virtuella datorer som kör din app.</span><span class="sxs-lookup"><span data-stu-id="b66bc-155">toosee hello scale set in action, you can force-refresh your web browser toosee hello load balancer distribute traffic across all hello VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="b66bc-156">Administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="b66bc-156">Management tasks</span></span>
<span data-ttu-id="b66bc-157">Du kan behöva toorun under hello livscykel för skaluppsättning för hello, en eller flera hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b66bc-157">Throughout hello lifecycle of hello scale set, you may need toorun one or more management tasks.</span></span> <span data-ttu-id="b66bc-158">Dessutom kan du toocreate skript som automatiserar olika livscykel-uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b66bc-158">Additionally, you may want toocreate scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="b66bc-159">hello Azure CLI 2.0 innehåller ett snabbt sätt toodo dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b66bc-159">hello Azure CLI 2.0 provides a quick way toodo those tasks.</span></span> <span data-ttu-id="b66bc-160">Här följer några vanliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b66bc-160">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="b66bc-161">Visa virtuella datorer i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="b66bc-161">View VMs in a scale set</span></span>
<span data-ttu-id="b66bc-162">tooview en lista över virtuella datorer som körs i din skala anges använder [az vmss listinstanserna](/cli/azure/vmss#list-instances) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b66bc-162">tooview a list of VMs running in your scale set, use [az vmss list-instances](/cli/azure/vmss#list-instances) as follows:</span></span>

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

<span data-ttu-id="b66bc-163">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b66bc-163">hello output is similar toohello following example:</span></span>

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="b66bc-164">Öka eller minska VM-instanser</span><span class="sxs-lookup"><span data-stu-id="b66bc-164">Increase or decrease VM instances</span></span>
<span data-ttu-id="b66bc-165">toosee hello antal förekomster av du för närvarande i en skala har använder [az vmss visa](/cli/azure/vmss#show) och fråga på *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="b66bc-165">toosee hello number of instances you currently have in a scale set, use [az vmss show](/cli/azure/vmss#show) and query on *sku.capacity*:</span></span>

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

<span data-ttu-id="b66bc-166">Du kan sedan manuellt öka eller minska hello antalet virtuella datorer i hello skala med [az vmss skala](/cli/azure/vmss#scale).</span><span class="sxs-lookup"><span data-stu-id="b66bc-166">You can then manually increase or decrease hello number of virtual machines in hello scale set with [az vmss scale](/cli/azure/vmss#scale).</span></span> <span data-ttu-id="b66bc-167">hello följande exempel anger hello antal virtuella datorer i din skaluppsättningen för*5*:</span><span class="sxs-lookup"><span data-stu-id="b66bc-167">hello following example sets hello number of VMs in your scale set too*5*:</span></span>

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

<span data-ttu-id="b66bc-168">Autoskala regler kan du definiera hur tooscale uppåt eller nedåt hello antal virtuella datorer i din skala in i svaret toodemand, till exempel nätverkstrafik eller CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="b66bc-168">Autoscale rules let you define how tooscale up or down hello number of VMs in your scale set in response toodemand such as network traffic or CPU usage.</span></span> <span data-ttu-id="b66bc-169">De här reglerna kan för närvarande inte anges i Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="b66bc-169">Currently, these rules cannot be set in Azure CLI 2.0.</span></span> <span data-ttu-id="b66bc-170">Använd hello [Azure-portalen](https://portal.azure.com) tooconfigure Autoskala.</span><span class="sxs-lookup"><span data-stu-id="b66bc-170">Use hello [Azure portal](https://portal.azure.com) tooconfigure autoscale.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="b66bc-171">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="b66bc-171">Get connection info</span></span>
<span data-ttu-id="b66bc-172">tooobtain anslutningsinformationen om hello virtuella datorer i din skaluppsättningar använder [az vmss lista-instans--anslutningsinformation](/cli/azure/vmss#list-instance-connection-info).</span><span class="sxs-lookup"><span data-stu-id="b66bc-172">tooobtain connection information about hello VMs in your scale sets, use [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info).</span></span> <span data-ttu-id="b66bc-173">Detta kommando hello offentlig IP-adress och port för varje virtuell dator som du kan använda tooconnect med SSH:</span><span class="sxs-lookup"><span data-stu-id="b66bc-173">This command outputs hello public IP address and port for each VM that allows you tooconnect with SSH:</span></span>

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a><span data-ttu-id="b66bc-174">Använda datadiskar med skaluppsättningar</span><span class="sxs-lookup"><span data-stu-id="b66bc-174">Use data disks with scale sets</span></span>
<span data-ttu-id="b66bc-175">Du kan skapa och använda datadiskar med skaluppsättningar.</span><span class="sxs-lookup"><span data-stu-id="b66bc-175">You can create and use data disks with scale sets.</span></span> <span data-ttu-id="b66bc-176">I en tidigare kursen du lärt dig hur för[hantera Azure-diskar](tutorial-manage-disks.md) som ger en översikt över hello metodtips och prestanda för att skapa appar på datadiskar i stället för hello OS-disk.</span><span class="sxs-lookup"><span data-stu-id="b66bc-176">In a previous tutorial, you learned how too[Manage Azure disks](tutorial-manage-disks.md) that outlines hello best practices and performance improvements for building apps on data disks rather than hello OS disk.</span></span>

### <a name="create-scale-set-with-data-disks"></a><span data-ttu-id="b66bc-177">Skapa skaluppsättning med datadiskar</span><span class="sxs-lookup"><span data-stu-id="b66bc-177">Create scale set with data disks</span></span>
<span data-ttu-id="b66bc-178">toocreate en skala ange och bifoga datadiskar, lägga till hello `--data-disk-sizes-gb` parametern toohello [az vmss skapa](/cli/azure/vmss#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="b66bc-178">toocreate a scale set and attach data disks, add hello `--data-disk-sizes-gb` parameter toohello [az vmss create](/cli/azure/vmss#create) command.</span></span> <span data-ttu-id="b66bc-179">hello följande exempel skapas en skala med *50*Gb datadiskar kopplade tooeach instans:</span><span class="sxs-lookup"><span data-stu-id="b66bc-179">hello following example creates a scale set with *50*Gb data disks attached tooeach instance:</span></span>

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

<span data-ttu-id="b66bc-180">När instanser tas bort från en skaluppsättning för tas eventuella anslutna hårddiskar också bort.</span><span class="sxs-lookup"><span data-stu-id="b66bc-180">When instances are removed from a scale set, any attached data disks are also removed.</span></span>

### <a name="add-data-disks"></a><span data-ttu-id="b66bc-181">Lägg till datadiskar</span><span class="sxs-lookup"><span data-stu-id="b66bc-181">Add data disks</span></span>
<span data-ttu-id="b66bc-182">tooadd tooinstances en data-disk i dina skala ange använder [az vmss disk bifoga](/cli/azure/vmss/disk#attach).</span><span class="sxs-lookup"><span data-stu-id="b66bc-182">tooadd a data disk tooinstances in your scale set, use [az vmss disk attach](/cli/azure/vmss/disk#attach).</span></span> <span data-ttu-id="b66bc-183">hello följande exempel lägger till en *50*Gb disk tooeach instans:</span><span class="sxs-lookup"><span data-stu-id="b66bc-183">hello following example adds a *50*Gb disk tooeach instance:</span></span>

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a><span data-ttu-id="b66bc-184">Koppla från datadiskar</span><span class="sxs-lookup"><span data-stu-id="b66bc-184">Detach data disks</span></span>
<span data-ttu-id="b66bc-185">tooremove tooinstances en data-disk i dina skala ange använder [az vmss disk frånkoppling](/cli/azure/vmss/disk#detach).</span><span class="sxs-lookup"><span data-stu-id="b66bc-185">tooremove a data disk tooinstances in your scale set, use [az vmss disk detach](/cli/azure/vmss/disk#detach).</span></span> <span data-ttu-id="b66bc-186">hello följande exempel tar bort hello datadisk på LUN *2* från varje instans:</span><span class="sxs-lookup"><span data-stu-id="b66bc-186">hello following example removes hello data disk at LUN *2* from each instance:</span></span>

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a><span data-ttu-id="b66bc-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b66bc-187">Next steps</span></span>
<span data-ttu-id="b66bc-188">Du har skapat en skaluppsättning för virtuell dator i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="b66bc-188">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="b66bc-189">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="b66bc-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b66bc-190">Använda molntjänster init toocreate tooscale en app</span><span class="sxs-lookup"><span data-stu-id="b66bc-190">Use cloud-init toocreate an app tooscale</span></span>
> * <span data-ttu-id="b66bc-191">Skapa en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b66bc-191">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="b66bc-192">Öka eller minska hello antalet instanser i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="b66bc-192">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="b66bc-193">Visa anslutningsinformation för scale set-instanser</span><span class="sxs-lookup"><span data-stu-id="b66bc-193">View connection info for scale set instances</span></span>
> * <span data-ttu-id="b66bc-194">Använda datadiskar i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="b66bc-194">Use data disks in a scale set</span></span>

<span data-ttu-id="b66bc-195">Avancera toohello nästa självstudiekurs toolearn mer om koncept för virtuella datorer för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="b66bc-195">Advance toohello next tutorial toolearn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b66bc-196">Belastningsutjämna virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b66bc-196">Load balance virtual machines</span></span>](tutorial-load-balancer.md)