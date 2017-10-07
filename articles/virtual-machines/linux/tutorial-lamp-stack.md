---
title: "aaaDeploy LAMPA på en virtuell Linux-dator i Azure | Microsoft Docs"
description: "Självstudiekurs – installera hello LYKTA stacken på en Linux-VM i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: a3d0ecb3277f15bd0a2fdc0d85b738a760e68865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a>Installera en LYKTA webbserver på en Azure VM
Den här artikeln får du veta hur toodeploy en Apache web server, MySQL och PHP (hello LYKTA stack) på en Ubuntu VM i Azure. Om du föredrar hello NGINX-webbservern finns hello [LEMP stack](tutorial-lemp-stack.md) kursen. toosee hello LYKTA server i åtgärden, kan du om du vill installera och konfigurera en WordPress-webbplats. I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa en Ubuntu VM (hello ”L” i hello LYKTA stack)
> * Öppna port 80 för webbtrafik
> * Installera Apache, MySQL och PHP
> * Verifiera installation och konfiguration
> * Installera WordPress på hello LYKTA server


Mer information om hello LYKTA stacken, inklusive rekommendationer för en produktionsmiljö finns hello [Ubuntu dokumentationen](https://help.ubuntu.com/community/ApacheMySQLPHP).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Installera Apache, MySQL och PHP

Kör följande kommando tooupdate Ubuntu paketet källor hello och installera Apache, MySQL och PHP. Observera hello hatt (^) hello slutet av hello-kommando.


```bash
sudo apt update && sudo apt install lamp-server^
```



Du kan ange tooinstall hello paket och andra beroenden. När du uppmanas, anger du ett rotlösenord för MySQL och [Retur] toocontinue. Följ hello återstående uppmaningarna. Den här processen installerar hello minsta nödvändiga PHP tillägg som behövs toouse PHP med MySQL. 

![MySQL-rot lösenordssidan][1]

## <a name="verify-installation-and-configuration"></a>Verifiera installation och konfiguration


### <a name="apache"></a>Apache

Kontrollera hello version av Apache med hello följande kommando:
```bash
apache2 -v
```

Med Apache installerad och port 80 öppna tooyour VM, kan nu komma åt hello-webbservern från hello internet. tooview hello Apache2 Ubuntu standardsida, öppna en webbläsare och ange hello offentliga IP-adress hello VM. Använd hello offentlig IP-adress som du använde tooSSH toohello VM:

![Apache standardsida][3]


### <a name="mysql"></a>MySQL

Kontrollera hello version av MySQL med hello följande kommando (Observera hello kapital `V` parametern):

```bash
msql -V
```

Vi rekommenderar att du kör skriptet toohelp säker hello installation av MySQL hello:

```bash
mysql_secure_installation
```

Ange lösenordet för roten MySQL och konfigurera hello säkerhetsinställningar för din miljö.

Om du vill toocreate en MySQL-databas, lägga till användare eller ändra konfigurationsinställningarna, inloggning tooMySQL:

```bash
mysql -u root -p
```

När du är klar avsluta hello mysql Kommandotolken genom att skriva `\q`.

### <a name="php"></a>PHP

Kontrollera hello-versionen av PHP med hello följande kommando:

```bash
php -v
```

Skapa en snabb PHP info sidan tooview i en webbläsare om du vill tootest ytterligare. hello följande kommando skapar hello PHP info-sida:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Nu kan du kontrollera hello PHP Infosidan som du skapat. Öppna en webbläsare och gå för`http://yourPublicIPAddress/info.php`. Ersätt hello offentliga IP-adressen för den virtuella datorn. Det bör se liknande toothis bild.

![Information om PHP-sida][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a>Nästa steg

I kursen får distribuerat du en LYKTA server i Azure. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en virtuell Ubuntu-dator
> * Öppna port 80 för webbtrafik
> * Installera Apache, MySQL och PHP
> * Verifiera installation och konfiguration
> * Installera WordPress på hello LYKTA server

I förväg toohello nästa självstudiekurs toolearn hur toosecure webbservrar med SSL-certifikat.

> [!div class="nextstepaction"]
> [Säker webbserver med SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png