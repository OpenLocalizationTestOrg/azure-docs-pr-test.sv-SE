---
title: "aaaDeploy LAMPA på en virtuell Linux-dator med hello Azure CLI 1.0 | Microsoft Docs"
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
ms.devlang: NA
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: e78a82d388ce68710933b9b673aa1b2460bdbb14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-lamp-stack-with-hello-azure-cli-10"></a>Distribuera LYKTA stacken med hello Azure CLI 1.0
Den här artikeln får du veta hur toodeploy en Apache web server, MySQL och PHP (hello LYKTA stack) på Azure. Du behöver ett Azure-konto ([skaffa en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)) och hello [Azure CLI](../../cli-install-nodejs.md) som [anslutna tooyour Azure-konto](../../xplat-cli-connect.md).

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0] – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

```
# One command toocreate a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* Distribuera LYKTA på befintlig virtuell dator

```
# Two commands: one updates packages, hello other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Distribuera LYKTA på den nya VM genomgång
Du kan börja med att skapa en [resursgruppen](../../azure-resource-manager/resource-group-overview.md) kommer att innehålla hello ny virtuell dator:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

toocreate hello Virtuella datorn kan du använda ett redan skrivits Azure Resource Manager mallen [här på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Du bör se svar uppmanar vissa fler indata:

    info:    Executing command group deployment create
    info:    Supply values for hello following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment toocomplete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Nu har du skapat en Linux VM med LYKTA redan installerad på den. Om du vill kan du kontrollera hello installationen genom att hoppa över ned för[Kontrollera LYKTA installerats](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Distribuera LYKTA på befintliga Virtuella genomgång
Om du behöver hjälp med att skapa en Linux VM, kan du gå [här toolearn hur toocreate en Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sedan måste tooSSH i hello Linux VM. Om du behöver hjälp med att skapa en SSH-nyckel kan du gå [här toolearn hur toocreate en SSH-nyckel på Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Om du redan har en SSH-nyckel kan gå vidare och SSH från kommandoraden till din Linux VM med `ssh exampleUsername@exampleDNS`.

Nu när du arbetar inom din Linux VM går vi igenom installerar hello LYKTA stack på Debian-baserade distributioner. hello kommandona kan skilja sig åt andra Linux-distributioner.

#### <a name="installing-on-debianubuntu"></a>Installera på Debian/Ubuntu
Du behöver hello följande paket som har installerats: `apache2`, `mysql-server`, `php5`, och `php5-mysql`. Du kan installera dessa paket genom att direkt ta tag paketen eller använda Tasksel. Instruktioner för båda alternativen visas nedan.
Innan du installerar du behöver toodownload och uppdatera paketet listor.

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a>Enskilda paket
Använda lgh get:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Med hjälp av tasksel
Alternativt kan du hämta Tasksel, ett Debian/Ubuntu-verktyg som du installerar flera relaterade paket som en samordnad ”task” på datorn.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

När du kör något av föregående alternativ för hello, kommer du att ange tooinstall paketen och andra beroenden. Tryck på 'y' och 'Enter' toocontinue och följ alla andra prompter tooset det administrativa lösenordet för MySQL. Detta installerar hello minsta nödvändiga PHP tillägg som behövs toouse PHP med MySQL. 

![][1]

Kör följande kommando toosee hello andra PHP-tillägg som är tillgängliga som paket:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Skapa info.php dokument
Nu bör du kunna toocheck vilken version av Apache, MySQL och PHP du ha via hello kommandoraden genom att skriva `apache2 -v`, `mysql -v`, eller `php -v`.

Om du skulle t.ex tootest ytterligare, kan du skapa en snabb PHP info sidan tooview i en webbläsare. Skapa en fil med Nano textredigerare med det här kommandot:

    user@ubuntu$ sudo nano /var/www/html/info.php

Lägg till hello följande rader i hello GNU Nano textredigerare:

    <?php
    phpinfo();
    ?>

Sedan spara och avsluta hello textredigerare.

Starta om Apache med det här kommandot så att alla nya installerar börjar gälla.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Kontrollera LAMPAN har installerats
Nu kan du kontrollera hello PHP Infosidan som du skapade genom att öppna en webbläsare och gå toohttp://youruniqueDNS/info.php. Det bör se liknande toothis bild.

![][2]

Du kan kontrollera installationen av Apache genom att visa hello Apache2 Ubuntu standardsida genom att gå tooyou http://youruniqueDNS/. Du bör se något som liknar den här avbildningen.

![][3]

Grattis, du har bara installationsprogrammet en LYKTA stack på Azure VM!

## <a name="next-steps"></a>Nästa steg
Checka ut hello Ubuntu dokumentation om hello LYKTA stack:

* [https://help.Ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png