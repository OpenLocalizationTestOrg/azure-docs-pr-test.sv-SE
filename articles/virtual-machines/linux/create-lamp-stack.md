---
title: "aaaDeploy LAMPA på en virtuell Linux-dator i Azure | Microsoft Docs"
description: "Lär dig hur tooinstall hello LYKTA stacken på en Linux-VM i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jluk
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: 42d887bb9f78becc02505e336be25fdaaf78df70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-lamp-stack-on-azure"></a>Distribuera LYKTA stacken på Azure
Den här artikeln får du veta hur toodeploy en Apache web server, MySQL och PHP (hello LYKTA stack) på Azure. Du behöver ett Azure-konto ([skaffa en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)) och hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2). Du kan också utföra dessa steg med hello [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-command-summary"></a>Snabbkommando sammanfattning

1. Spara och redigera hello [azuredeploy.parameters.json filen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour inställningar på din lokala dator.
2. Kör följande två kommandon toocreate hello en resursgrupp och distribuera mallen:

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a>Distribuera LYKTA på befintlig virtuell dator
hello följande kommandon uppdateringar paket och sedan installerar Apache, MySQL och PHP:

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Distribuera LYKTA på den nya VM genomgång

1. Skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create) toocontain hello ny virtuell dator:

```azurecli
az group create -l westus -n myResourceGroup
```
toocreate hello Virtuella datorn kan du använda ett redan skrivits Azure Resource Manager mallen [här på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

2. Spara hello [azuredeploy.parameters.json filen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour lokal dator.
3. Redigera hello **azuredeploy.parameters.json** filen tooyour önskade indata.
4. Distribuera hello mallen med [az distribution skapa] refererar till hello hämtas json-fil:

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

hello utdata är liknande toohello följande exempel:

```json
{
"id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Resources/deployments/azuredeploy",
"name": "azuredeploy",
"properties": {
    "correlationId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "debugSetting": null,
}
...
"provisioningState": "Succeeded",
"template": null,
"templateLink": {
    "contentVersion": "1.0.0.0",
    "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json"
    },
    "timestamp": "2017-02-22T00:05:51.860411+00:00"
},
"resourceGroup": "myResourceGroup"
}
```

Nu har du skapat en Linux VM med LYKTA redan installerad på den. Om du vill kan du kontrollera hello installationen genom att hoppa över ned för[Kontrollera LYKTA installerats](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Distribuera LYKTA på befintliga Virtuella genomgång
Om du behöver hjälp med att skapa en Linux VM, kan du gå [här toolearn hur toocreate en Linux VM](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli). Sedan måste tooSSH i hello Linux VM. Om du behöver hjälp med att skapa en SSH-nyckel kan du gå [här toolearn hur toocreate en SSH-nyckel på Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Om du redan har en SSH-nyckel kan gå vidare och SSH från kommandoraden till din Linux VM med `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.

Nu när du arbetar inom din Linux VM går vi igenom installerar hello LYKTA stack på Debian-baserade distributioner. hello kommandona kan skilja sig åt andra Linux-distributioner.

#### <a name="installing-on-debianubuntu"></a>Installera på Debian/Ubuntu
Du behöver hello följande paket som har installerats: `apache2`, `mysql-server`, `php5`, och `php5-mysql`. Du kan installera dessa paket genom att direkt ta tag paketen eller använda Tasksel.
Innan du installerar du behöver toodownload och uppdatera paketet listor.

```bash
sudo apt-get update
```

##### <a name="individual-packages"></a>Enskilda paket
Använda lgh get:

```bash
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

##### <a name="using-tasksel"></a>Med hjälp av tasksel
Alternativt kan du hämta Tasksel, ett Debian/Ubuntu-verktyg som du installerar flera relaterade paket som en samordnad ”task” på datorn.

```bash
sudo apt-get install tasksel
sudo tasksel install lamp-server
```

När du kör något av föregående alternativ för hello, kommer du att ange tooinstall paketen och andra beroenden. tooset ett administratörslösenord för MySQL, tryck på 'y' och 'Enter' toocontinue och följer några andra frågor. Den här processen installerar hello minsta nödvändiga PHP tillägg som behövs toouse PHP med MySQL. 

![][1]

Kör följande kommando toosee hello andra PHP-tillägg som är tillgängliga som paket:

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a>Skapa info.php dokument
Nu bör du kunna toocheck vilken version av Apache, MySQL och PHP du ha via hello kommandoraden genom att skriva `apache2 -v`, `mysql -v`, eller `php -v`.

Om du skulle t.ex tootest ytterligare, kan du skapa en snabb PHP info sidan tooview i en webbläsare. Skapa en fil med Nano textredigerare med det här kommandot:

```bash
sudo nano /var/www/html/info.php
```

Lägg till hello följande rader i hello GNU Nano textredigerare:

```php
<?php
phpinfo();
?>
```

Sedan spara och avsluta hello textredigerare.

Starta om Apache med det här kommandot så att alla nya installerar börjar gälla.

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a>Kontrollera LAMPAN har installerats
Nu kan du kontrollera hello PHP Infosidan som du skapade genom att öppna en webbläsare och gå toohttp://youruniqueDNS/info.php. Det bör se liknande toothis bild.

![][2]

Du kan kontrollera installationen av Apache genom att visa hello Apache2 Ubuntu standardsida genom att gå tooyou http://youruniqueDNS/. hello utdata är liknande toohello följande exempel:

![][3]

Grattis, du har bara installationsprogrammet en LYKTA stack på Azure VM!

## <a name="next-steps"></a>Nästa steg
Checka ut hello Ubuntu dokumentation om hello LYKTA stack:

* [https://help.Ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
