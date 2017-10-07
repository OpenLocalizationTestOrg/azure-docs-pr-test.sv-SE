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
# <a name="how-toocustomize-a-linux-virtual-machine-on-first-boot"></a>Hur toocustomize en Linux-dator vid den första starten
I en tidigare kursen du lärt dig hur tooSSH tooa virtuell dator (VM) och installera NGINX manuellt. är vanligtvis det önskade toocreate virtuella datorer på ett snabbt och konsekvent sätt någon form av automatisering. En gemensam metod toocustomize en VM på första start är toouse [moln init](https://cloudinit.readthedocs.io). I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa en konfigurationsfil för molnet initiering
> * Skapa en virtuell dator som använder en moln-init-fil
> * Visa en Node.js-app som körs efter hello virtuell dator skapas
> * Använda Key Vault toosecurely store certifikat
> * Automatisera säker distribution av NGINX med molnet initiering


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).  



## <a name="cloud-init-overview"></a>Översikt över molntjänster initiering
[Molnet init](https://cloudinit.readthedocs.io) är en mycket vanlig metod toocustomize en Linux-VM när den startas för hello första gången. Du kan använda molnet init tooinstall paket och skriva filer, eller tooconfigure användare och säkerhet. Eftersom molnet init körs under hello ursprungliga startprocessen, det finns inga ytterligare åtgärder krävs agenter tooapply konfigurationen eller.

Molnet init fungerar även över distributioner. Exempelvis kan du inte använda **lgh get installera** eller **yum installera** tooinstall ett paket. I stället kan du definiera en lista över paket tooinstall. Molnet init använder automatiskt hello interna paketet hanteringsverktyg för hello distro du väljer.

Vi kan arbeta med våra partners tooget moln initiering ingår och arbeta i hello bilder de ger tooAzure. hello i den följande tabellen beskrivs hello aktuella molnet init tillgänglighet på Azure-plattformen bilder:

| Alias | Utgivare | Erbjudande | SKU | Version |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 LTS |senaste |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |senaste |
| CoreOS |CoreOS |CoreOS |Stable |senaste |


## <a name="create-cloud-init-config-file"></a>Skapa moln init-konfigurationsfil
toosee moln initiering i åtgärden, skapa en virtuell dator som installerar NGINX och kör en enkel ”Hello World” Node.js-app. hello följande molntjänster init configuration installerar hello krävs paket, skapar en Node.js-appen och sedan initiera och startar hello app.

Skapa en fil med namnet i din aktuella shell *moln init.txt* och klistra in hello följande konfiguration. Till exempel skapa hello-filen i hello molnet Shell inte på den lokala datorn. Du kan använda valfri redigerare som du vill. Ange `sensible-editor cloud-init.txt` toocreate hello filen och visas i listan över tillgängliga redigerare. Kontrollera att filen hello hela molnet initiering kopierats korrekt, särskilt hello första raden:

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

Mer information om molnet init konfigurationsalternativ finns [moln init config exempel](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).

## <a name="create-virtual-machine"></a>Skapa en virtuell dator
Innan du kan skapa en virtuell dator, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroupAutomate* i hello *eastus* plats:

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

Nu skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create). Använd hello `--custom-data` parametern toopass i konfigurationsfilen molnet initiering. Ange hello fullständig sökväg toohello *moln init.txt* config om du har sparat filen hello utanför arbetskatalogen finns. hello följande exempel skapas en virtuell dator med namnet *myAutomatedVM*:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Det tar några minuter för hello VM toobe skapats hello paket tooinstall och hello app toostart. Det finns bakgrundsaktiviteter för att fortsätta toorun när hello Azure CLI returnerar toohello prompt. Det kan vara en annan några minuter innan du kan komma åt hello app. När du har skapat hello VM anteckna hello `publicIpAddress` visas av hello Azure CLI. Den här adressen är används tooaccess hello Node.js-appen via en webbläsare.

tooallow web tooreach trafik den virtuella datorn, öppna port 80 från hello Internet med [az vm öppna port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a>Testa webbprogram
Nu kan du öppna en webbläsare och ange *http://<publicIpAddress>*  i hello adressfältet. Ange dina egna offentliga IP-adress från hello VM skapa processen. Node.js-appen visas som i följande exempel hello:

![Visa körs NGINX-webbplats](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a>Mata in certifikat från Nyckelvalvet
Det här valfria avsnittet visar hur du säkert lagra certifikat i Azure Key Vault och mata in dem under hello distribution av Virtuella datorer. I stället för att använda en anpassad avbildning som inkluderar hello certifikat inbyggd-modulen för den här processen säkerställer att den senaste hello-certifikat är matas in tooa VM vid den första starten. Under processen hello hello certifikatet aldrig lämnar hello Azure-plattformen eller exponeras i ett skript, kommandoradsverktyget historik eller mall.

Azure Key Vault skyddar kryptografiska nycklar och hemligheter, till exempel certifikat eller lösenord. Key Vault hjälper till att effektivisera hello nyckelhanteringen och låter dig toomaintain kontrollen över nycklar som kommer åt och krypterar dina data. Det här scenariot beskriver vissa Key Vault begrepp toocreate och Använd ett certifikat, men är inte en fullständig översikt över hur toouse Key Vault.

hello följande steg visar hur du kan:

- Skapa ett Azure Key Vault
- Generera och överför ett certifikat toohello Key Vault
- Skapa en hemlighet från hello certifikat tooinject i tooa VM
- Skapa en virtuell dator och mata in hello certifikat

### <a name="create-an-azure-key-vault"></a>Skapa ett Azure Key Vault
Skapa först ett Nyckelvalv med [az keyvault skapa](/cli/azure/keyvault#create) och aktivera den för användning när du distribuerar en virtuell dator. Varje Key Vault kräver ett unikt namn och bör vara alla gemen. Ersätt *mykeyvault* i följande exempel med ditt eget unikt namn för Key Vault hello:

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a>Generera certifikat och lagra i Key Vault
För produktion, ska du importera ett giltigt certifikat som signerats av en betrodd provider med [az keyvault certifikat import](/cli/azure/keyvault/certificate#import). Den här självstudien hello följande exempel visar hur du kan skapa ett självsignerat certifikat med [az keyvault certifikat skapa](/cli/azure/keyvault/certificate#create) som använder hello standardprincipen för certifikat:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a>Förbereda certifikat för användning med VM
toouse hello certifikatet under hello VM Skapa process, hämta hello-ID för ditt certifikat med [az keyvault lista-versionerna](/cli/azure/keyvault/secret#list-versions). hello VM måste hello certifikat i en vissa format tooinject konvertera programmet på Start, så hello certifikat med [az vm format-hemlighet](/cli/azure/vm#format-secret). följande exempel hello tilldelar hello resultatet av dessa kommandon toovariables för att underlätta användning i hello nästa steg:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-toosecure-nginx"></a>Skapa moln init config toosecure NGINX
När du skapar en virtuell dator, certifikat och nycklar lagras i hello skyddade */var/lib/waagent/* directory. tooautomate att lägga till hello certifikat toohello VM och konfigurera NGINX, kan du använda en uppdaterad molnet initiering config från hello föregående exempel.

Skapa en fil med namnet *moln-init-secured.txt* och klistra in hello efter konfiguration. Om du använder hello molnet Shell skapa igen, hello molnet init-konfigurationsfilen finns det och inte på den lokala datorn. Använd `sensible-editor cloud-init-secured.txt` toocreate hello filen och visas i listan över tillgängliga redigerare. Kontrollera att filen hello hela molnet initiering kopierats korrekt, särskilt hello första raden:

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

### <a name="create-secure-vm"></a>Skapa säker virtuell dator
Nu skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create). hello certifikatdata injekteras från Nyckelvalvet med hello `--secrets` parameter. Som i föregående exempel hello du också ange hello molnet init config med hello `--custom-data` parameter:

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

Det tar några minuter för hello VM toobe skapats hello paket tooinstall och hello app toostart. Det finns bakgrundsaktiviteter för att fortsätta toorun när hello Azure CLI returnerar toohello prompt. Det kan vara en annan några minuter innan du kan komma åt hello app. När du har skapat hello VM anteckna hello `publicIpAddress` visas av hello Azure CLI. Den här adressen är används tooaccess hello Node.js-appen via en webbläsare.

tooallow skydda web trafik tooreach den virtuella datorn, öppna port 443 från hello Internet med [az vm öppna port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a>Testa säkra webbprogram
Nu kan du öppna en webbläsare och ange *https://<publicIpAddress>*  i hello adressfältet. Ange dina egna offentliga IP-adress från hello VM skapa processen. Acceptera hello Säkerhetsvarning om du använder ett självsignerat certifikat:

![Acceptera web Säkerhetsvarning för webbläsare](./media/tutorial-automate-vm-deployment/browser-warning.png)

Skyddad NGINX-platsen och Node.js app visas som i följande exempel hello:

![Visa körs säker NGINX-webbplats](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a>Nästa steg
I kursen får konfigurerat du virtuella datorer på första start med molnet initiering. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en konfigurationsfil för molnet initiering
> * Skapa en virtuell dator som använder en moln-init-fil
> * Visa en Node.js-app som körs efter hello virtuell dator skapas
> * Använda Key Vault toosecurely store certifikat
> * Automatisera säker distribution av NGINX med molnet initiering

I förväg toohello nästa självstudiekurs toolearn hur toocreate anpassade VM-avbildningar.

> [!div class="nextstepaction"]
> [Skapa anpassade avbildningar av en virtuell dator](./tutorial-custom-images.md)
