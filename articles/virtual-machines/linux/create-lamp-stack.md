---
title: "Distribuera LYKTA på en virtuell Linux-dator i Azure | Microsoft Docs"
description: "Lär dig hur du installerar LYKTA stacken på en Linux-VM i Azure"
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
ms.openlocfilehash: ad69876bfbeba5f948a81e5c48c659fdf2265ae2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-lamp-stack-on-azure"></a>Distribuera LYKTA stacken på Azure
Den här artikeln får du veta hur du distribuerar en Apache webbservern, MySQL och PHP (LYKTA stack) på Azure. Du behöver ett Azure-konto ([skaffa en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)) och [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2). Du kan också utföra dessa steg med [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-command-summary"></a>Snabbkommando sammanfattning

1. Spara och redigera den [azuredeploy.parameters.json filen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) till dina inställningar på din lokala dator.
2. Kör följande två kommandon för att skapa en resursgrupp och distribuera mallen:

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a>Distribuera LYKTA på befintlig virtuell dator
Följande kommandon uppdaterar paket och sedan installerar Apache, MySQL och PHP:

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Distribuera LYKTA på den nya VM genomgång

1. Skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create) som innehåller den nya virtuella datorn:

```azurecli
az group create -l westus -n myResourceGroup
```
Om du vill skapa Virtuellt datorn, du kan använda ett redan skrivits Azure Resource Manager mallen [här på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

2. Spara den [azuredeploy.parameters.json filen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) till din lokala dator.
3. Redigera den **azuredeploy.parameters.json** filen till alla prioriterade indata.
4. Distribuera mallen med [az distribution skapa] refererar till hämtade json-filen:

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

Utdata ser ut ungefär så här:

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

Nu har du skapat en Linux VM med LYKTA redan installerad på den. Om du vill kan du kontrollera installationen genom att hoppa över ned till [Kontrollera LYKTA installerats](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Distribuera LYKTA på befintliga Virtuella genomgång
Om du behöver hjälp med att skapa en Linux VM, kan du gå [här om du vill veta hur du skapar en Linux VM](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli). Därefter måste du SSH i Linux VM. Om du behöver hjälp med att skapa en SSH-nyckel kan du gå [här om du vill lära dig hur du skapar en SSH-nyckel på Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Om du redan har en SSH-nyckel kan gå vidare och SSH från kommandoraden till din Linux VM med `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.

Nu när du arbetar inom din Linux VM går vi igenom installerar LYKTA stacken på Debian-baserade distributioner. De exakta kommandona kan skilja sig åt andra Linux-distributioner.

#### <a name="installing-on-debianubuntu"></a>Installera på Debian/Ubuntu
Du behöver följande paket installeras: `apache2`, `mysql-server`, `php5`, och `php5-mysql`. Du kan installera dessa paket genom att direkt ta tag paketen eller använda Tasksel.
Innan du installerar måste du hämta och uppdatera paketet listor.

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

När du kör något av föregående alternativ, uppmanas du att installera dessa paket och andra beroenden. Om du vill ange ett administratörslösenord för MySQL trycker du på ”y” och sedan på RETUR om du fortsätta och följa några andra frågor. Den här processen installerar de minsta nödvändiga PHP-tillägg för att kunna använda PHP med MySQL. 

![][1]

Kör följande kommando för att se andra PHP-tillägg som är tillgängliga som paket:

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a>Skapa info.php dokument
Du ska nu kunna kontrollera vilken version av Apache, MySQL och PHP via kommandoraden genom att skriva `apache2 -v`, `mysql -v`, eller `php -v`.

Om du vill testa ytterligare kan du skapa en snabb PHP info sida för att visa i en webbläsare. Skapa en fil med Nano textredigerare med det här kommandot:

```bash
sudo nano /var/www/html/info.php
```

Lägg till följande rader i textredigeraren GNU Nano:

```php
<?php
phpinfo();
?>
```

Sedan spara och Avsluta textredigeraren.

Starta om Apache med det här kommandot så att alla nya installerar börjar gälla.

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a>Kontrollera LAMPAN har installerats
Nu kan du kontrollera sidan PHP du skapade genom att öppna en webbläsare och gå till http://youruniqueDNS/info.php. Det bör likna den här avbildningen.

![][2]

Du kan kontrollera installationen av Apache genom att visa sidan standard Ubuntu Apache2 genom att gå till du http://youruniqueDNS/. Utdata ser ut ungefär så här:

![][3]

Grattis, du har bara installationsprogrammet en LYKTA stack på Azure VM!

## <a name="next-steps"></a>Nästa steg
Kolla Ubuntu-dokumentationen i LYKTA stacken:

* [https://help.Ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
