---
title: "aaaCustomize en Linux VM på första start i Azure | Microsoft Docs"
description: "Lär dig hur toouse moln initiering och Key Vault toocustomze virtuella Linux-datorer hello första gången de startar i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 280189723ac0205226f9c0068bd605da13249ace
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-a-linux-virtual-machine-on-first-boot"></a><span data-ttu-id="399a4-103">Hur toocustomize en Linux-dator vid den första starten</span><span class="sxs-lookup"><span data-stu-id="399a4-103">How toocustomize a Linux virtual machine on first boot</span></span>
<span data-ttu-id="399a4-104">I en tidigare kursen du lärt dig hur tooSSH tooa virtuell dator (VM) och installera NGINX manuellt.</span><span class="sxs-lookup"><span data-stu-id="399a4-104">In a previous tutorial, you learned how tooSSH tooa virtual machine (VM) and manually install NGINX.</span></span> <span data-ttu-id="399a4-105">är vanligtvis det önskade toocreate virtuella datorer på ett snabbt och konsekvent sätt någon form av automatisering.</span><span class="sxs-lookup"><span data-stu-id="399a4-105">toocreate VMs in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="399a4-106">En gemensam metod toocustomize en VM på första start är toouse [moln init](https://cloudinit.readthedocs.io).</span><span class="sxs-lookup"><span data-stu-id="399a4-106">A common approach toocustomize a VM on first boot is toouse [cloud-init](https://cloudinit.readthedocs.io).</span></span> <span data-ttu-id="399a4-107">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="399a4-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="399a4-108">Skapa en konfigurationsfil för molnet initiering</span><span class="sxs-lookup"><span data-stu-id="399a4-108">Create a cloud-init config file</span></span>
> * <span data-ttu-id="399a4-109">Skapa en virtuell dator som använder en moln-init-fil</span><span class="sxs-lookup"><span data-stu-id="399a4-109">Create a VM that uses a cloud-init file</span></span>
> * <span data-ttu-id="399a4-110">Visa en Node.js-app som körs efter hello virtuell dator skapas</span><span class="sxs-lookup"><span data-stu-id="399a4-110">View a running Node.js app after hello VM is created</span></span>
> * <span data-ttu-id="399a4-111">Använda Key Vault toosecurely store certifikat</span><span class="sxs-lookup"><span data-stu-id="399a4-111">Use Key Vault toosecurely store certificates</span></span>
> * <span data-ttu-id="399a4-112">Automatisera säker distribution av NGINX med molnet initiering</span><span class="sxs-lookup"><span data-stu-id="399a4-112">Automate secure deployments of NGINX with cloud-init</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="399a4-113">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="399a4-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="399a4-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="399a4-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="399a4-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="399a4-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>  



## <a name="cloud-init-overview"></a><span data-ttu-id="399a4-116">Översikt över molntjänster initiering</span><span class="sxs-lookup"><span data-stu-id="399a4-116">Cloud-init overview</span></span>
<span data-ttu-id="399a4-117">[Molnet init](https://cloudinit.readthedocs.io) är en mycket vanlig metod toocustomize en Linux-VM när den startas för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="399a4-117">[Cloud-init](https://cloudinit.readthedocs.io) is a widely used approach toocustomize a Linux VM as it boots for hello first time.</span></span> <span data-ttu-id="399a4-118">Du kan använda molnet init tooinstall paket och skriva filer, eller tooconfigure användare och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="399a4-118">You can use cloud-init tooinstall packages and write files, or tooconfigure users and security.</span></span> <span data-ttu-id="399a4-119">Eftersom molnet init körs under hello ursprungliga startprocessen, det finns inga ytterligare åtgärder krävs agenter tooapply konfigurationen eller.</span><span class="sxs-lookup"><span data-stu-id="399a4-119">As cloud-init runs during hello initial boot process, there are no additional steps or required agents tooapply your configuration.</span></span>

<span data-ttu-id="399a4-120">Molnet init fungerar även över distributioner.</span><span class="sxs-lookup"><span data-stu-id="399a4-120">Cloud-init also works across distributions.</span></span> <span data-ttu-id="399a4-121">Exempelvis kan du inte använda **lgh get installera** eller **yum installera** tooinstall ett paket.</span><span class="sxs-lookup"><span data-stu-id="399a4-121">For example, you don't use **apt-get install** or **yum install** tooinstall a package.</span></span> <span data-ttu-id="399a4-122">I stället kan du definiera en lista över paket tooinstall.</span><span class="sxs-lookup"><span data-stu-id="399a4-122">Instead you can define a list of packages tooinstall.</span></span> <span data-ttu-id="399a4-123">Molnet init använder automatiskt hello interna paketet hanteringsverktyg för hello distro du väljer.</span><span class="sxs-lookup"><span data-stu-id="399a4-123">Cloud-init automatically uses hello native package management tool for hello distro you select.</span></span>

<span data-ttu-id="399a4-124">Vi kan arbeta med våra partners tooget moln initiering ingår och arbeta i hello bilder de ger tooAzure.</span><span class="sxs-lookup"><span data-stu-id="399a4-124">We are working with our partners tooget cloud-init included and working in hello images that they provide tooAzure.</span></span> <span data-ttu-id="399a4-125">hello i den följande tabellen beskrivs hello aktuella molnet init tillgänglighet på Azure-plattformen bilder:</span><span class="sxs-lookup"><span data-stu-id="399a4-125">hello following table outlines hello current cloud-init availability on Azure platform images:</span></span>

| <span data-ttu-id="399a4-126">Alias</span><span class="sxs-lookup"><span data-stu-id="399a4-126">Alias</span></span> | <span data-ttu-id="399a4-127">Utgivare</span><span class="sxs-lookup"><span data-stu-id="399a4-127">Publisher</span></span> | <span data-ttu-id="399a4-128">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="399a4-128">Offer</span></span> | <span data-ttu-id="399a4-129">SKU</span><span class="sxs-lookup"><span data-stu-id="399a4-129">SKU</span></span> | <span data-ttu-id="399a4-130">Version</span><span class="sxs-lookup"><span data-stu-id="399a4-130">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="399a4-131">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="399a4-131">UbuntuLTS</span></span> |<span data-ttu-id="399a4-132">Canonical</span><span class="sxs-lookup"><span data-stu-id="399a4-132">Canonical</span></span> |<span data-ttu-id="399a4-133">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="399a4-133">UbuntuServer</span></span> |<span data-ttu-id="399a4-134">16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="399a4-134">16.04-LTS</span></span> |<span data-ttu-id="399a4-135">senaste</span><span class="sxs-lookup"><span data-stu-id="399a4-135">latest</span></span> |
| <span data-ttu-id="399a4-136">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="399a4-136">UbuntuLTS</span></span> |<span data-ttu-id="399a4-137">Canonical</span><span class="sxs-lookup"><span data-stu-id="399a4-137">Canonical</span></span> |<span data-ttu-id="399a4-138">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="399a4-138">UbuntuServer</span></span> |<span data-ttu-id="399a4-139">14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="399a4-139">14.04.5-LTS</span></span> |<span data-ttu-id="399a4-140">senaste</span><span class="sxs-lookup"><span data-stu-id="399a4-140">latest</span></span> |
| <span data-ttu-id="399a4-141">CoreOS</span><span class="sxs-lookup"><span data-stu-id="399a4-141">CoreOS</span></span> |<span data-ttu-id="399a4-142">CoreOS</span><span class="sxs-lookup"><span data-stu-id="399a4-142">CoreOS</span></span> |<span data-ttu-id="399a4-143">CoreOS</span><span class="sxs-lookup"><span data-stu-id="399a4-143">CoreOS</span></span> |<span data-ttu-id="399a4-144">Stable</span><span class="sxs-lookup"><span data-stu-id="399a4-144">Stable</span></span> |<span data-ttu-id="399a4-145">senaste</span><span class="sxs-lookup"><span data-stu-id="399a4-145">latest</span></span> |


## <a name="create-cloud-init-config-file"></a><span data-ttu-id="399a4-146">Skapa moln init-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="399a4-146">Create cloud-init config file</span></span>
<span data-ttu-id="399a4-147">toosee moln initiering i åtgärden, skapa en virtuell dator som installerar NGINX och kör en enkel ”Hello World” Node.js-app.</span><span class="sxs-lookup"><span data-stu-id="399a4-147">toosee cloud-init in action, create a VM that installs NGINX and runs a simple 'Hello World' Node.js app.</span></span> <span data-ttu-id="399a4-148">hello följande molntjänster init configuration installerar hello krävs paket, skapar en Node.js-appen och sedan initiera och startar hello app.</span><span class="sxs-lookup"><span data-stu-id="399a4-148">hello following cloud-init configuration installs hello required packages, creates a Node.js app, then initialize and starts hello app.</span></span>

<span data-ttu-id="399a4-149">Skapa en fil med namnet i din aktuella shell *moln init.txt* och klistra in hello följande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="399a4-149">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="399a4-150">Till exempel skapa hello-filen i hello molnet Shell inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="399a4-150">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="399a4-151">Du kan använda valfri redigerare som du vill.</span><span class="sxs-lookup"><span data-stu-id="399a4-151">You can use any editor you wish.</span></span> <span data-ttu-id="399a4-152">Ange `sensible-editor cloud-init.txt` toocreate hello filen och visas i listan över tillgängliga redigerare.</span><span class="sxs-lookup"><span data-stu-id="399a4-152">Enter `sensible-editor cloud-init.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="399a4-153">Kontrollera att filen hello hela molnet initiering kopierats korrekt, särskilt hello första raden:</span><span class="sxs-lookup"><span data-stu-id="399a4-153">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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

<span data-ttu-id="399a4-154">Mer information om molnet init konfigurationsalternativ finns [moln init config exempel](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).</span><span class="sxs-lookup"><span data-stu-id="399a4-154">For more information about cloud-init configuration options, see [cloud-init config examples](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="399a4-155">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="399a4-155">Create virtual machine</span></span>
<span data-ttu-id="399a4-156">Innan du kan skapa en virtuell dator, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="399a4-156">Before you can create a VM, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="399a4-157">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupAutomate* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="399a4-157">hello following example creates a resource group named *myResourceGroupAutomate* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

<span data-ttu-id="399a4-158">Nu skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="399a4-158">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="399a4-159">Använd hello `--custom-data` parametern toopass i konfigurationsfilen molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="399a4-159">Use hello `--custom-data` parameter toopass in your cloud-init config file.</span></span> <span data-ttu-id="399a4-160">Ange hello fullständig sökväg toohello *moln init.txt* config om du har sparat filen hello utanför arbetskatalogen finns.</span><span class="sxs-lookup"><span data-stu-id="399a4-160">Provide hello full path toohello *cloud-init.txt* config if you saved hello file outside of your present working directory.</span></span> <span data-ttu-id="399a4-161">hello följande exempel skapas en virtuell dator med namnet *myAutomatedVM*:</span><span class="sxs-lookup"><span data-stu-id="399a4-161">hello following example creates a VM named *myAutomatedVM*:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

<span data-ttu-id="399a4-162">Det tar några minuter för hello VM toobe skapats hello paket tooinstall och hello app toostart.</span><span class="sxs-lookup"><span data-stu-id="399a4-162">It takes a few minutes for hello VM toobe created, hello packages tooinstall, and hello app toostart.</span></span> <span data-ttu-id="399a4-163">Det finns bakgrundsaktiviteter för att fortsätta toorun när hello Azure CLI returnerar toohello prompt.</span><span class="sxs-lookup"><span data-stu-id="399a4-163">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="399a4-164">Det kan vara en annan några minuter innan du kan komma åt hello app.</span><span class="sxs-lookup"><span data-stu-id="399a4-164">It may be another couple of minutes before you can access hello app.</span></span> <span data-ttu-id="399a4-165">När du har skapat hello VM anteckna hello `publicIpAddress` visas av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="399a4-165">When hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="399a4-166">Den här adressen är används tooaccess hello Node.js-appen via en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="399a4-166">This address is used tooaccess hello Node.js app via a web browser.</span></span>

<span data-ttu-id="399a4-167">tooallow web tooreach trafik den virtuella datorn, öppna port 80 från hello Internet med [az vm öppna port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="399a4-167">tooallow web traffic tooreach your VM, open port 80 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a><span data-ttu-id="399a4-168">Testa webbprogram</span><span class="sxs-lookup"><span data-stu-id="399a4-168">Test web app</span></span>
<span data-ttu-id="399a4-169">Nu kan du öppna en webbläsare och ange *http://<publicIpAddress>*  i hello adressfältet.</span><span class="sxs-lookup"><span data-stu-id="399a4-169">Now you can open a web browser and enter *http://<publicIpAddress>* in hello address bar.</span></span> <span data-ttu-id="399a4-170">Ange dina egna offentliga IP-adress från hello VM skapa processen.</span><span class="sxs-lookup"><span data-stu-id="399a4-170">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="399a4-171">Node.js-appen visas som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="399a4-171">Your Node.js app is displayed as in hello following example:</span></span>

![Visa körs NGINX-webbplats](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a><span data-ttu-id="399a4-173">Mata in certifikat från Nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="399a4-173">Inject certificates from Key Vault</span></span>
<span data-ttu-id="399a4-174">Det här valfria avsnittet visar hur du säkert lagra certifikat i Azure Key Vault och mata in dem under hello distribution av Virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="399a4-174">This optional section shows how you can securely store certificates in Azure Key Vault and inject them during hello VM deployment.</span></span> <span data-ttu-id="399a4-175">I stället för att använda en anpassad avbildning som inkluderar hello certifikat inbyggd-modulen för den här processen säkerställer att den senaste hello-certifikat är matas in tooa VM vid den första starten.</span><span class="sxs-lookup"><span data-stu-id="399a4-175">Rather than using a custom image that includes hello certificates baked-in, this process ensures that hello most up-to-date certificates are injected tooa VM on first boot.</span></span> <span data-ttu-id="399a4-176">Under processen hello hello certifikatet aldrig lämnar hello Azure-plattformen eller exponeras i ett skript, kommandoradsverktyget historik eller mall.</span><span class="sxs-lookup"><span data-stu-id="399a4-176">During hello process, hello certificate never leaves hello Azure platform or is exposed in a script, command-line history, or template.</span></span>

<span data-ttu-id="399a4-177">Azure Key Vault skyddar kryptografiska nycklar och hemligheter, till exempel certifikat eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="399a4-177">Azure Key Vault safeguards cryptographic keys and secrets, such as certificates or passwords.</span></span> <span data-ttu-id="399a4-178">Key Vault hjälper till att effektivisera hello nyckelhanteringen och låter dig toomaintain kontrollen över nycklar som kommer åt och krypterar dina data.</span><span class="sxs-lookup"><span data-stu-id="399a4-178">Key Vault helps streamline hello key management process and enables you toomaintain control of keys that access and encrypt your data.</span></span> <span data-ttu-id="399a4-179">Det här scenariot beskriver vissa Key Vault begrepp toocreate och Använd ett certifikat, men är inte en fullständig översikt över hur toouse Key Vault.</span><span class="sxs-lookup"><span data-stu-id="399a4-179">This scenario introduces some Key Vault concepts toocreate and use a certificate, though is not an exhaustive overview on how toouse Key Vault.</span></span>

<span data-ttu-id="399a4-180">hello följande steg visar hur du kan:</span><span class="sxs-lookup"><span data-stu-id="399a4-180">hello following steps show how you can:</span></span>

- <span data-ttu-id="399a4-181">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="399a4-181">Create an Azure Key Vault</span></span>
- <span data-ttu-id="399a4-182">Generera och överför ett certifikat toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="399a4-182">Generate or upload a certificate toohello Key Vault</span></span>
- <span data-ttu-id="399a4-183">Skapa en hemlighet från hello certifikat tooinject i tooa VM</span><span class="sxs-lookup"><span data-stu-id="399a4-183">Create a secret from hello certificate tooinject in tooa VM</span></span>
- <span data-ttu-id="399a4-184">Skapa en virtuell dator och mata in hello certifikat</span><span class="sxs-lookup"><span data-stu-id="399a4-184">Create a VM and inject hello certificate</span></span>

### <a name="create-an-azure-key-vault"></a><span data-ttu-id="399a4-185">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="399a4-185">Create an Azure Key Vault</span></span>
<span data-ttu-id="399a4-186">Skapa först ett Nyckelvalv med [az keyvault skapa](/cli/azure/keyvault#create) och aktivera den för användning när du distribuerar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="399a4-186">First, create a Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable it for use when you deploy a VM.</span></span> <span data-ttu-id="399a4-187">Varje Key Vault kräver ett unikt namn och bör vara alla gemen.</span><span class="sxs-lookup"><span data-stu-id="399a4-187">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="399a4-188">Ersätt *mykeyvault* i följande exempel med ditt eget unikt namn för Key Vault hello:</span><span class="sxs-lookup"><span data-stu-id="399a4-188">Replace *mykeyvault* in hello following example with your own unique Key Vault name:</span></span>

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a><span data-ttu-id="399a4-189">Generera certifikat och lagra i Key Vault</span><span class="sxs-lookup"><span data-stu-id="399a4-189">Generate certificate and store in Key Vault</span></span>
<span data-ttu-id="399a4-190">För produktion, ska du importera ett giltigt certifikat som signerats av en betrodd provider med [az keyvault certifikat import](/cli/azure/keyvault/certificate#import).</span><span class="sxs-lookup"><span data-stu-id="399a4-190">For production use, you should import a valid certificate signed by trusted provider with [az keyvault certificate import](/cli/azure/keyvault/certificate#import).</span></span> <span data-ttu-id="399a4-191">Den här självstudien hello följande exempel visar hur du kan skapa ett självsignerat certifikat med [az keyvault certifikat skapa](/cli/azure/keyvault/certificate#create) som använder hello standardprincipen för certifikat:</span><span class="sxs-lookup"><span data-stu-id="399a4-191">For this tutorial, hello following example shows how you can generate a self-signed certificate with [az keyvault certificate create](/cli/azure/keyvault/certificate#create) that uses hello default certificate policy:</span></span>

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a><span data-ttu-id="399a4-192">Förbereda certifikat för användning med VM</span><span class="sxs-lookup"><span data-stu-id="399a4-192">Prepare certificate for use with VM</span></span>
<span data-ttu-id="399a4-193">toouse hello certifikatet under hello VM Skapa process, hämta hello-ID för ditt certifikat med [az keyvault lista-versionerna](/cli/azure/keyvault/secret#list-versions).</span><span class="sxs-lookup"><span data-stu-id="399a4-193">toouse hello certificate during hello VM create process, obtain hello ID of your certificate with [az keyvault secret list-versions](/cli/azure/keyvault/secret#list-versions).</span></span> <span data-ttu-id="399a4-194">hello VM måste hello certifikat i en vissa format tooinject konvertera programmet på Start, så hello certifikat med [az vm format-hemlighet](/cli/azure/vm#format-secret).</span><span class="sxs-lookup"><span data-stu-id="399a4-194">hello VM needs hello certificate in a certain format tooinject it on boot, so convert hello certificate with [az vm format-secret](/cli/azure/vm#format-secret).</span></span> <span data-ttu-id="399a4-195">följande exempel hello tilldelar hello resultatet av dessa kommandon toovariables för att underlätta användning i hello nästa steg:</span><span class="sxs-lookup"><span data-stu-id="399a4-195">hello following example assigns hello output of these commands toovariables for ease of use in hello next steps:</span></span>

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-toosecure-nginx"></a><span data-ttu-id="399a4-196">Skapa moln init config toosecure NGINX</span><span class="sxs-lookup"><span data-stu-id="399a4-196">Create cloud-init config toosecure NGINX</span></span>
<span data-ttu-id="399a4-197">När du skapar en virtuell dator, certifikat och nycklar lagras i hello skyddade */var/lib/waagent/* directory.</span><span class="sxs-lookup"><span data-stu-id="399a4-197">When you create a VM, certificates and keys are stored in hello protected */var/lib/waagent/* directory.</span></span> <span data-ttu-id="399a4-198">tooautomate att lägga till hello certifikat toohello VM och konfigurera NGINX, kan du använda en uppdaterad molnet initiering config från hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="399a4-198">tooautomate adding hello certificate toohello VM and configuring NGINX, you can use an updated cloud-init config from hello previous example.</span></span>

<span data-ttu-id="399a4-199">Skapa en fil med namnet *moln-init-secured.txt* och klistra in hello efter konfiguration.</span><span class="sxs-lookup"><span data-stu-id="399a4-199">Create a file named *cloud-init-secured.txt* and paste hello following configuration.</span></span> <span data-ttu-id="399a4-200">Om du använder hello molnet Shell skapa igen, hello molnet init-konfigurationsfilen finns det och inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="399a4-200">Again, if you use hello Cloud Shell, create hello cloud-init config file there and not on your local machine.</span></span> <span data-ttu-id="399a4-201">Använd `sensible-editor cloud-init-secured.txt` toocreate hello filen och visas i listan över tillgängliga redigerare.</span><span class="sxs-lookup"><span data-stu-id="399a4-201">Use `sensible-editor cloud-init-secured.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="399a4-202">Kontrollera att filen hello hela molnet initiering kopierats korrekt, särskilt hello första raden:</span><span class="sxs-lookup"><span data-stu-id="399a4-202">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
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
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-secure-vm"></a><span data-ttu-id="399a4-203">Skapa säker virtuell dator</span><span class="sxs-lookup"><span data-stu-id="399a4-203">Create secure VM</span></span>
<span data-ttu-id="399a4-204">Nu skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="399a4-204">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="399a4-205">hello certifikatdata injekteras från Nyckelvalvet med hello `--secrets` parameter.</span><span class="sxs-lookup"><span data-stu-id="399a4-205">hello certificate data is injected from Key Vault with hello `--secrets` parameter.</span></span> <span data-ttu-id="399a4-206">Som i föregående exempel hello du också ange hello molnet init config med hello `--custom-data` parameter:</span><span class="sxs-lookup"><span data-stu-id="399a4-206">As in hello previous example, you also pass in hello cloud-init config with hello `--custom-data` parameter:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-secured.txt \
    --secrets "$vm_secret"
```

<span data-ttu-id="399a4-207">Det tar några minuter för hello VM toobe skapats hello paket tooinstall och hello app toostart.</span><span class="sxs-lookup"><span data-stu-id="399a4-207">It takes a few minutes for hello VM toobe created, hello packages tooinstall, and hello app toostart.</span></span> <span data-ttu-id="399a4-208">Det finns bakgrundsaktiviteter för att fortsätta toorun när hello Azure CLI returnerar toohello prompt.</span><span class="sxs-lookup"><span data-stu-id="399a4-208">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="399a4-209">Det kan vara en annan några minuter innan du kan komma åt hello app.</span><span class="sxs-lookup"><span data-stu-id="399a4-209">It may be another couple of minutes before you can access hello app.</span></span> <span data-ttu-id="399a4-210">När du har skapat hello VM anteckna hello `publicIpAddress` visas av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="399a4-210">When hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="399a4-211">Den här adressen är används tooaccess hello Node.js-appen via en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="399a4-211">This address is used tooaccess hello Node.js app via a web browser.</span></span>

<span data-ttu-id="399a4-212">tooallow skydda web trafik tooreach den virtuella datorn, öppna port 443 från hello Internet med [az vm öppna port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="399a4-212">tooallow secure web traffic tooreach your VM, open port 443 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a><span data-ttu-id="399a4-213">Testa säkra webbprogram</span><span class="sxs-lookup"><span data-stu-id="399a4-213">Test secure web app</span></span>
<span data-ttu-id="399a4-214">Nu kan du öppna en webbläsare och ange *https://<publicIpAddress>*  i hello adressfältet.</span><span class="sxs-lookup"><span data-stu-id="399a4-214">Now you can open a web browser and enter *https://<publicIpAddress>* in hello address bar.</span></span> <span data-ttu-id="399a4-215">Ange dina egna offentliga IP-adress från hello VM skapa processen.</span><span class="sxs-lookup"><span data-stu-id="399a4-215">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="399a4-216">Acceptera hello Säkerhetsvarning om du använder ett självsignerat certifikat:</span><span class="sxs-lookup"><span data-stu-id="399a4-216">Accept hello security warning if you used a self-signed certificate:</span></span>

![Acceptera web Säkerhetsvarning för webbläsare](./media/tutorial-automate-vm-deployment/browser-warning.png)

<span data-ttu-id="399a4-218">Skyddad NGINX-platsen och Node.js app visas som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="399a4-218">Your secured NGINX site and Node.js app is then displayed as in hello following example:</span></span>

![Visa körs säker NGINX-webbplats](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a><span data-ttu-id="399a4-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="399a4-220">Next steps</span></span>
<span data-ttu-id="399a4-221">I kursen får konfigurerat du virtuella datorer på första start med molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="399a4-221">In this tutorial, you configured VMs on first boot with cloud-init.</span></span> <span data-ttu-id="399a4-222">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="399a4-222">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="399a4-223">Skapa en konfigurationsfil för molnet initiering</span><span class="sxs-lookup"><span data-stu-id="399a4-223">Create a cloud-init config file</span></span>
> * <span data-ttu-id="399a4-224">Skapa en virtuell dator som använder en moln-init-fil</span><span class="sxs-lookup"><span data-stu-id="399a4-224">Create a VM that uses a cloud-init file</span></span>
> * <span data-ttu-id="399a4-225">Visa en Node.js-app som körs efter hello virtuell dator skapas</span><span class="sxs-lookup"><span data-stu-id="399a4-225">View a running Node.js app after hello VM is created</span></span>
> * <span data-ttu-id="399a4-226">Använda Key Vault toosecurely store certifikat</span><span class="sxs-lookup"><span data-stu-id="399a4-226">Use Key Vault toosecurely store certificates</span></span>
> * <span data-ttu-id="399a4-227">Automatisera säker distribution av NGINX med molnet initiering</span><span class="sxs-lookup"><span data-stu-id="399a4-227">Automate secure deployments of NGINX with cloud-init</span></span>

<span data-ttu-id="399a4-228">I förväg toohello nästa självstudiekurs toolearn hur toocreate anpassade VM-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="399a4-228">Advance toohello next tutorial toolearn how toocreate custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="399a4-229">Skapa anpassade avbildningar av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="399a4-229">Create custom VM images</span></span>](./tutorial-custom-images.md)
