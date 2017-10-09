---
title: "aaaUse hello CustomScript-tillägg på en Linux-VM | Microsoft Docs"
description: "Lär dig hur du skapar toouse hello CustomScript-tillägg toodeploy program på Linux-datorer i Azure med hjälp av hello klassiska distributionsmodellen."
editor: tysonn
manager: timlt
documentationcenter: 
services: virtual-machines-linux
author: gbowerman
tags: azure-service-management
ms.assetid: e535241d-feca-4412-b07a-67c936ba88a0
ms.service: virtual-machines-linux
ms.workload: multiple
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: guybo
ms.openlocfilehash: 864a586e70093eefbabc065a3c05e1cf9e315704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-lamp-app-using-hello-azure-customscript-extension-for-linux"></a>Distribuera en LYKTA-app med hello Azure CustomScript-tillägg för Linux
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Information om hur du distribuerar en LYKTA-stacken använder hello Resource Manager-modellen finns [här](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

hello Microsoft Azure CustomScript-tillägg för Linux innehåller en sätt toocustomize dina virtuella datorer (VM) genom att köra skadlig kod som skrivs i valfritt skriptspråk som stöds av hello VM (till exempel Python och Bash). Detta ger en mycket flexibelt sätt tooautomate programmet distribution toomultiple datorer.

Du kan distribuera hello CustomScript-tillägg med hello Azure-portalen, Windows PowerShell eller hello Azure-kommandoradsgränssnittet (Azure CLI).

I den här artikeln använder vi skapade hello Azure CLI toodeploy en enkel LYKTA programmet tooan Ubuntu VM med hello klassiska distributionsmodellen.

## <a name="prerequisites"></a>Krav
För det här exemplet skapar du två virtuella Azure-datorer kör Ubuntu 14.04 eller senare. hello VMs kallas *skript-vm* och *lykta vm*. Använd ett unikt namn när du skapar hello virtuella datorer. En används toorun hello CLI-kommandona och ett är används toodeploy hello LYKTA app.

Du måste också ett Azure Storage-konto och en nyckel tooaccess it (du kan hämta det från hello Azure-portalen).

Om du behöver hjälp med att skapa virtuella Linux-datorer i Azure Se för[skapa en virtuell dator kör Linux](createportal.md).

hello installera kommandon förutsätter Ubuntu men du kan anpassa hello installation för alla stöder Linux distro.

hello måste skript vm VM toohave Azure CLI installerats med en fungerande anslutning tooAzure. Hjälp med det här finns för[installera och konfigurera hello Azure-kommandoradsgränssnittet](../../../cli-install-nodejs.md).

## <a name="upload-a-script"></a>Ladda upp ett skript
Vi använder hello CustomScript-tillägg toorun ett skript på en fjärransluten VM tooinstall hello LYKTA stack och skapa en PHP-sida. I ordning tooaccess hello skript från valfri plats ska vi skicka den som en Azure blob.

### <a name="script-overview"></a>Översikt över skript
exempel på skript hello installerar en LYKTA stack tooUbuntu (inklusive hur du konfigurerar en tyst installation av MySQL), skriver en enkel PHP-filen och startar Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install hello LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Överför skript
Spara hello skript som en textfil, till exempel *install_lamp.sh*, och sedan ladda upp den tooAzure lagring. Du kan göra det enkelt med Azure CLI. hello filöverföringar följande exempel hello filen tooa storage-behållare med namnet ”skript”. Om hello-behållaren inte finns behöver du toocreate den första.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Även skapa en JSON-fil som beskriver hur toodownload hello skriptet från Azure Storage. Spara den som *public_config.json* (i stället för ”mystorage” hello namnet på ditt lagringskonto):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-hello-extension"></a>Distribuera hello-tillägg
Nu kan du använda hello nästa kommando toodeploy hello Linux CustomScript-tillägg toohello remote hello VM med hjälp av Azure CLI.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

hello föregående kommando hämtar och kör hello *install_lamp.sh* skriptet på hello VM kallas *lykta vm*.

Eftersom hello appen innehåller en webbserver, kan du komma ihåg tooopen HTTP lyssningsport på hello remote virtuell dator med hello nästa kommando.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Övervakning och felsökning
Du kan kontrollera hur väl hello anpassat skript körs genom att titta på hello loggfilen hello remote VM. SSH för*lykta vm* och pilslut hello loggfil med hello nästa kommando.

    cd /var/log/azure/customscript
    tail -f handler.log

När du har kört hello CustomScript-tillägg kan du bläddra toohello PHP-sida som du skapade för information. hello PHP sidan till hello exempel i den här artikeln är *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Ytterligare resurser
Du kan använda samma grundläggande steg toodeploy hello mer komplexa appar. I det här exemplet sparades hello installationsskriptet som en offentlig blob i Azure Storage. Ett säkrare alternativ skulle vara toostore hello installationsskriptet som en säker blob med en [säker Åtkomstsignatur](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

Ytterligare resurser för Azure CLI, Linux och hello CustomScript-tillägg visas bredvid.

[Automatisera anpassningsuppgifter i virtuella Linux-datorer med CustomScript-tillägg](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux-tillägg (GitHub)](https://github.com/Azure/azure-linux-extensions)