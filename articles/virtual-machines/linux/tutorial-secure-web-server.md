---
title: aaaSecure en webbserver med SSL-certifikat i Azure | Microsoft Docs
description: "Lär dig hur toosecure hello NGINX-webbserver med SSL-certifikat på en Linux-VM i Azure"
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
ms.date: 07/17/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d3a62d77ac05c9aa2a44356b7c8e44cb485b81aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="2fd85-103">Skydda en webbserver med SSL-certifikat på en virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="2fd85-103">Secure a web server with SSL certificates on a Linux virtual machine in Azure</span></span>
<span data-ttu-id="2fd85-104">toosecure webbservrar som kan vara ett certifikat senare SSL (Secure Sockets) används tooencrypt webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="2fd85-104">toosecure web servers, a Secure Sockets Later (SSL) certificate can be used tooencrypt web traffic.</span></span> <span data-ttu-id="2fd85-105">Dessa SSL-certifikat kan lagras i Azure Key Vault och tillåta säker distribution av certifikat tooLinux virtuella maskiner (VMs) i Azure.</span><span class="sxs-lookup"><span data-stu-id="2fd85-105">These SSL certificates can be stored in Azure Key Vault, and allow secure deployments of certificates tooLinux virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="2fd85-106">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="2fd85-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2fd85-107">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2fd85-107">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="2fd85-108">Generera och överför ett certifikat toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="2fd85-108">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="2fd85-109">Skapa en virtuell dator och installera hello NGINX-webbserver</span><span class="sxs-lookup"><span data-stu-id="2fd85-109">Create a VM and install hello NGINX web server</span></span>
> * <span data-ttu-id="2fd85-110">Mata in hello certifikat i hello VM och konfigurera NGINX med en SSL-bindning</span><span class="sxs-lookup"><span data-stu-id="2fd85-110">Inject hello certificate into hello VM and configure NGINX with an SSL binding</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="2fd85-111">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2fd85-111">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="2fd85-112">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="2fd85-112">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="2fd85-113">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2fd85-113">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>  


## <a name="overview"></a><span data-ttu-id="2fd85-114">Översikt</span><span class="sxs-lookup"><span data-stu-id="2fd85-114">Overview</span></span>
<span data-ttu-id="2fd85-115">Azure Key Vault skyddar kryptografiska nycklar och hemligheter, dessa certifikat eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="2fd85-115">Azure Key Vault safeguards cryptographic keys and secrets, such certificates or passwords.</span></span> <span data-ttu-id="2fd85-116">Key Vault hjälper till att förenkla hanteringen för hello certifikat och aktiverar toomaintain kontrollen över nycklar som har åtkomst till dessa certifikat.</span><span class="sxs-lookup"><span data-stu-id="2fd85-116">Key Vault helps streamline hello certificate management process and enables you toomaintain control of keys that access those certificates.</span></span> <span data-ttu-id="2fd85-117">Du kan skapa ett självsignerat certifikat i Nyckelvalvet eller ladda upp en befintlig, betrodda certifikat som du redan äger.</span><span class="sxs-lookup"><span data-stu-id="2fd85-117">You can create a self-signed certificate inside Key Vault, or upload an existing, trusted certificate that you already own.</span></span>

<span data-ttu-id="2fd85-118">I stället för att använda en anpassad VM-avbildning som inkluderar certifikat inbyggd-modulen Certifikat Injicera i en aktiv virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2fd85-118">Rather than using a custom VM image that includes certificates baked-in, you inject certificates into a running VM.</span></span> <span data-ttu-id="2fd85-119">Den här processen säkerställer att hello senaste certifikat installeras på en webbserver under distributionen.</span><span class="sxs-lookup"><span data-stu-id="2fd85-119">This process ensures that hello most up-to-date certificates are installed on a web server during deployment.</span></span> <span data-ttu-id="2fd85-120">Om du förnya eller ersätta ett certifikat kan har du också inte toocreate en ny anpassad VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="2fd85-120">If you renew or replace a certificate, you don't also have toocreate a new custom VM image.</span></span> <span data-ttu-id="2fd85-121">hello senaste certifikat är automatiskt matas in när du skapar ytterligare virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2fd85-121">hello latest certificates are automatically injected as you create additional VMs.</span></span> <span data-ttu-id="2fd85-122">Under hela processen hello hello certifikat aldrig lämna hello Azure-plattformen eller exponeras i ett skript, kommandoradsverktyget historik eller mall.</span><span class="sxs-lookup"><span data-stu-id="2fd85-122">During hello whole process, hello certificates never leave hello Azure platform or are exposed in a script, command-line history, or template.</span></span>


## <a name="create-an-azure-key-vault"></a><span data-ttu-id="2fd85-123">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2fd85-123">Create an Azure Key Vault</span></span>
<span data-ttu-id="2fd85-124">Innan du kan skapa ett Nyckelvalv och certifikat, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="2fd85-124">Before you can create a Key Vault and certificates, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="2fd85-125">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupSecureWeb* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="2fd85-125">hello following example creates a resource group named *myResourceGroupSecureWeb* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

<span data-ttu-id="2fd85-126">Skapa sedan ett Nyckelvalv med [az keyvault skapa](/cli/azure/keyvault#create) och aktivera den för användning när du distribuerar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2fd85-126">Next, create a Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable it for use when you deploy a VM.</span></span> <span data-ttu-id="2fd85-127">Varje Key Vault kräver ett unikt namn och bör vara alla gemen.</span><span class="sxs-lookup"><span data-stu-id="2fd85-127">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="2fd85-128">Ersätt * <mykeyvault> * i följande exempel med ditt eget unikt namn för Key Vault hello:</span><span class="sxs-lookup"><span data-stu-id="2fd85-128">Replace *<mykeyvault>* in hello following example with your own unique Key Vault name:</span></span>

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a><span data-ttu-id="2fd85-129">Generera ett certifikat och lagra i Key Vault</span><span class="sxs-lookup"><span data-stu-id="2fd85-129">Generate a certificate and store in Key Vault</span></span>
<span data-ttu-id="2fd85-130">För produktion, ska du importera ett giltigt certifikat som signerats av en betrodd provider med [az keyvault certifikat import](/cli/azure/certificate#import).</span><span class="sxs-lookup"><span data-stu-id="2fd85-130">For production use, you should import a valid certificate signed by trusted provider with [az keyvault certificate import](/cli/azure/certificate#import).</span></span> <span data-ttu-id="2fd85-131">Den här självstudien hello följande exempel visar hur du kan skapa ett självsignerat certifikat med [az keyvault certifikat skapa](/cli/azure/certificate#create) som använder hello standardprincipen för certifikat:</span><span class="sxs-lookup"><span data-stu-id="2fd85-131">For this tutorial, hello following example shows how you can generate a self-signed certificate with [az keyvault certificate create](/cli/azure/certificate#create) that uses hello default certificate policy:</span></span>

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a><span data-ttu-id="2fd85-132">Förbereda ett certifikat för användning med en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2fd85-132">Prepare a certificate for use with a VM</span></span>
<span data-ttu-id="2fd85-133">toouse hello certifikatet under hello VM Skapa process, hämta hello-ID för ditt certifikat med [az keyvault lista-versionerna](/cli/azure/keyvault/secret#list-versions).</span><span class="sxs-lookup"><span data-stu-id="2fd85-133">toouse hello certificate during hello VM create process, obtain hello ID of your certificate with [az keyvault secret list-versions](/cli/azure/keyvault/secret#list-versions).</span></span> <span data-ttu-id="2fd85-134">Konvertera hello certifikat med [az vm format-hemlighet](/cli/azure/vm#format-secret).</span><span class="sxs-lookup"><span data-stu-id="2fd85-134">Convert hello certificate with [az vm format-secret](/cli/azure/vm#format-secret).</span></span> <span data-ttu-id="2fd85-135">följande exempel hello tilldelar hello resultatet av dessa kommandon toovariables för att underlätta användning i hello nästa steg:</span><span class="sxs-lookup"><span data-stu-id="2fd85-135">hello following example assigns hello output of these commands toovariables for ease of use in hello next steps:</span></span>

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-toosecure-nginx"></a><span data-ttu-id="2fd85-136">Skapa en moln-init config toosecure NGINX</span><span class="sxs-lookup"><span data-stu-id="2fd85-136">Create a cloud-init config toosecure NGINX</span></span>
<span data-ttu-id="2fd85-137">[Molnet init](https://cloudinit.readthedocs.io) är en mycket vanlig metod toocustomize en Linux-VM när den startas för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="2fd85-137">[Cloud-init](https://cloudinit.readthedocs.io) is a widely used approach toocustomize a Linux VM as it boots for hello first time.</span></span> <span data-ttu-id="2fd85-138">Du kan använda molnet init tooinstall paket och skriva filer, eller tooconfigure användare och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="2fd85-138">You can use cloud-init tooinstall packages and write files, or tooconfigure users and security.</span></span> <span data-ttu-id="2fd85-139">Eftersom molnet init körs under hello ursprungliga startprocessen, det finns inga ytterligare åtgärder krävs agenter tooapply konfigurationen eller.</span><span class="sxs-lookup"><span data-stu-id="2fd85-139">As cloud-init runs during hello initial boot process, there are no additional steps or required agents tooapply your configuration.</span></span>

<span data-ttu-id="2fd85-140">När du skapar en virtuell dator, certifikat och nycklar lagras i hello skyddade */var/lib/waagent/* directory.</span><span class="sxs-lookup"><span data-stu-id="2fd85-140">When you create a VM, certificates and keys are stored in hello protected */var/lib/waagent/* directory.</span></span> <span data-ttu-id="2fd85-141">tooautomate lägger till hello certifikat toohello VM och konfigurera hello webbserver använda molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="2fd85-141">tooautomate adding hello certificate toohello VM and configuring hello web server, use cloud-init.</span></span> <span data-ttu-id="2fd85-142">I det här exemplet vi installera och konfigurera hello NGINX-webbserver.</span><span class="sxs-lookup"><span data-stu-id="2fd85-142">In this example, we install and configure hello NGINX web server.</span></span> <span data-ttu-id="2fd85-143">Du kan använda hello samma bearbeta tooinstall och konfigurera Apache.</span><span class="sxs-lookup"><span data-stu-id="2fd85-143">You can use hello same process tooinstall and configure Apache.</span></span> 

<span data-ttu-id="2fd85-144">Skapa en fil med namnet *moln-init-webb-server.txt* och klistra in hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="2fd85-144">Create a file named *cloud-init-web-server.txt* and paste hello following configuration:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a><span data-ttu-id="2fd85-145">Skapa en säker virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2fd85-145">Create a secure VM</span></span>
<span data-ttu-id="2fd85-146">Nu skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="2fd85-146">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="2fd85-147">hello certifikatdata injekteras från Nyckelvalvet med hello `--secrets` parameter.</span><span class="sxs-lookup"><span data-stu-id="2fd85-147">hello certificate data is injected from Key Vault with hello `--secrets` parameter.</span></span> <span data-ttu-id="2fd85-148">Du skickar i hello molnet init config med hello `--custom-data` parameter:</span><span class="sxs-lookup"><span data-stu-id="2fd85-148">You pass in hello cloud-init config with hello `--custom-data` parameter:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

<span data-ttu-id="2fd85-149">Det tar några minuter för hello VM toobe skapats hello paket tooinstall och hello app toostart.</span><span class="sxs-lookup"><span data-stu-id="2fd85-149">It takes a few minutes for hello VM toobe created, hello packages tooinstall, and hello app toostart.</span></span> <span data-ttu-id="2fd85-150">När du har skapat hello VM anteckna hello `publicIpAddress` visas av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="2fd85-150">When hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="2fd85-151">Den här adressen är används tooaccess webbplatsen i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="2fd85-151">This address is used tooaccess your site in a web browser.</span></span>

<span data-ttu-id="2fd85-152">tooallow skydda web trafik tooreach den virtuella datorn, öppna port 443 från hello Internet med [az vm öppna port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="2fd85-152">tooallow secure web traffic tooreach your VM, open port 443 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-hello-secure-web-app"></a><span data-ttu-id="2fd85-153">Testa hello säkra webbprogram</span><span class="sxs-lookup"><span data-stu-id="2fd85-153">Test hello secure web app</span></span>
<span data-ttu-id="2fd85-154">Nu kan du öppna en webbläsare och ange *https://<publicIpAddress> * i hello adressfältet.</span><span class="sxs-lookup"><span data-stu-id="2fd85-154">Now you can open a web browser and enter *https://<publicIpAddress>* in hello address bar.</span></span> <span data-ttu-id="2fd85-155">Ange dina egna offentliga IP-adress från hello VM skapa processen.</span><span class="sxs-lookup"><span data-stu-id="2fd85-155">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="2fd85-156">Acceptera hello Säkerhetsvarning om du använder ett självsignerat certifikat:</span><span class="sxs-lookup"><span data-stu-id="2fd85-156">Accept hello security warning if you used a self-signed certificate:</span></span>

![Acceptera web Säkerhetsvarning för webbläsare](./media/tutorial-secure-web-server/browser-warning.png)

<span data-ttu-id="2fd85-158">Din NGINX-webbplatsen visas som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="2fd85-158">Your secured NGINX site is then displayed as in hello following example:</span></span>

![Visa körs säker NGINX-webbplats](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a><span data-ttu-id="2fd85-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2fd85-160">Next steps</span></span>

<span data-ttu-id="2fd85-161">I den här självstudiekursen skyddas en NGINX-webbserver med ett SSL-certifikat som lagras i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2fd85-161">In this tutorial, you secured an NGINX web server with an SSL certificate stored in Azure Key Vault.</span></span> <span data-ttu-id="2fd85-162">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="2fd85-162">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2fd85-163">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2fd85-163">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="2fd85-164">Generera och överför ett certifikat toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="2fd85-164">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="2fd85-165">Skapa en virtuell dator och installera hello NGINX-webbserver</span><span class="sxs-lookup"><span data-stu-id="2fd85-165">Create a VM and install hello NGINX web server</span></span>
> * <span data-ttu-id="2fd85-166">Mata in hello certifikat i hello VM och konfigurera NGINX med en SSL-bindning</span><span class="sxs-lookup"><span data-stu-id="2fd85-166">Inject hello certificate into hello VM and configure NGINX with an SSL binding</span></span>

<span data-ttu-id="2fd85-167">Följ den här länken toosee inbyggd skriptexempel för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2fd85-167">Follow this link toosee pre-built virtual machine script samples.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2fd85-168">Windows skriptexempel för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2fd85-168">Windows virtual machine script samples</span></span>](./cli-samples.md)