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
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a>Skydda en webbserver med SSL-certifikat på en virtuell Linux-dator i Azure
toosecure webbservrar som kan vara ett certifikat senare SSL (Secure Sockets) används tooencrypt webbtrafik. Dessa SSL-certifikat kan lagras i Azure Key Vault och tillåta säker distribution av certifikat tooLinux virtuella maskiner (VMs) i Azure. I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa ett Azure Key Vault
> * Generera och överför ett certifikat toohello Key Vault
> * Skapa en virtuell dator och installera hello NGINX-webbserver
> * Mata in hello certifikat i hello VM och konfigurera NGINX med en SSL-bindning

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).  


## <a name="overview"></a>Översikt
Azure Key Vault skyddar kryptografiska nycklar och hemligheter, dessa certifikat eller lösenord. Key Vault hjälper till att förenkla hanteringen för hello certifikat och aktiverar toomaintain kontrollen över nycklar som har åtkomst till dessa certifikat. Du kan skapa ett självsignerat certifikat i Nyckelvalvet eller ladda upp en befintlig, betrodda certifikat som du redan äger.

I stället för att använda en anpassad VM-avbildning som inkluderar certifikat inbyggd-modulen Certifikat Injicera i en aktiv virtuell dator. Den här processen säkerställer att hello senaste certifikat installeras på en webbserver under distributionen. Om du förnya eller ersätta ett certifikat kan har du också inte toocreate en ny anpassad VM-avbildning. hello senaste certifikat är automatiskt matas in när du skapar ytterligare virtuella datorer. Under hela processen hello hello certifikat aldrig lämna hello Azure-plattformen eller exponeras i ett skript, kommandoradsverktyget historik eller mall.


## <a name="create-an-azure-key-vault"></a>Skapa ett Azure Key Vault
Innan du kan skapa ett Nyckelvalv och certifikat, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroupSecureWeb* i hello *eastus* plats:

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

Skapa sedan ett Nyckelvalv med [az keyvault skapa](/cli/azure/keyvault#create) och aktivera den för användning när du distribuerar en virtuell dator. Varje Key Vault kräver ett unikt namn och bör vara alla gemen. Ersätt * <mykeyvault> * i följande exempel med ditt eget unikt namn för Key Vault hello:

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Generera ett certifikat och lagra i Key Vault
För produktion, ska du importera ett giltigt certifikat som signerats av en betrodd provider med [az keyvault certifikat import](/cli/azure/certificate#import). Den här självstudien hello följande exempel visar hur du kan skapa ett självsignerat certifikat med [az keyvault certifikat skapa](/cli/azure/certificate#create) som använder hello standardprincipen för certifikat:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a>Förbereda ett certifikat för användning med en virtuell dator
toouse hello certifikatet under hello VM Skapa process, hämta hello-ID för ditt certifikat med [az keyvault lista-versionerna](/cli/azure/keyvault/secret#list-versions). Konvertera hello certifikat med [az vm format-hemlighet](/cli/azure/vm#format-secret). följande exempel hello tilldelar hello resultatet av dessa kommandon toovariables för att underlätta användning i hello nästa steg:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-toosecure-nginx"></a>Skapa en moln-init config toosecure NGINX
[Molnet init](https://cloudinit.readthedocs.io) är en mycket vanlig metod toocustomize en Linux-VM när den startas för hello första gången. Du kan använda molnet init tooinstall paket och skriva filer, eller tooconfigure användare och säkerhet. Eftersom molnet init körs under hello ursprungliga startprocessen, det finns inga ytterligare åtgärder krävs agenter tooapply konfigurationen eller.

När du skapar en virtuell dator, certifikat och nycklar lagras i hello skyddade */var/lib/waagent/* directory. tooautomate lägger till hello certifikat toohello VM och konfigurera hello webbserver använda molnet initiering. I det här exemplet vi installera och konfigurera hello NGINX-webbserver. Du kan använda hello samma bearbeta tooinstall och konfigurera Apache. 

Skapa en fil med namnet *moln-init-webb-server.txt* och klistra in hello följande konfiguration:

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

### <a name="create-a-secure-vm"></a>Skapa en säker virtuell dator
Nu skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create). hello certifikatdata injekteras från Nyckelvalvet med hello `--secrets` parameter. Du skickar i hello molnet init config med hello `--custom-data` parameter:

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

Det tar några minuter för hello VM toobe skapats hello paket tooinstall och hello app toostart. När du har skapat hello VM anteckna hello `publicIpAddress` visas av hello Azure CLI. Den här adressen är används tooaccess webbplatsen i en webbläsare.

tooallow skydda web trafik tooreach den virtuella datorn, öppna port 443 från hello Internet med [az vm öppna port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-hello-secure-web-app"></a>Testa hello säkra webbprogram
Nu kan du öppna en webbläsare och ange *https://<publicIpAddress> * i hello adressfältet. Ange dina egna offentliga IP-adress från hello VM skapa processen. Acceptera hello Säkerhetsvarning om du använder ett självsignerat certifikat:

![Acceptera web Säkerhetsvarning för webbläsare](./media/tutorial-secure-web-server/browser-warning.png)

Din NGINX-webbplatsen visas som i följande exempel hello:

![Visa körs säker NGINX-webbplats](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen skyddas en NGINX-webbserver med ett SSL-certifikat som lagras i Azure Key Vault. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa ett Azure Key Vault
> * Generera och överför ett certifikat toohello Key Vault
> * Skapa en virtuell dator och installera hello NGINX-webbserver
> * Mata in hello certifikat i hello VM och konfigurera NGINX med en SSL-bindning

Följ den här länken toosee inbyggd skriptexempel för virtuell dator.

> [!div class="nextstepaction"]
> [Windows skriptexempel för virtuell dator](./cli-samples.md)