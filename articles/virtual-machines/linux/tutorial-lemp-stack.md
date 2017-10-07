---
title: "aaaDeploy LEMP på en Linux-dator i Azure | Microsoft Docs"
description: "Självstudiekurs – installera hello LEMP stacken på en Linux-VM i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: d8f9d84c5e9c0df4e9e985c10fe10f63a2f88214
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a>Installera en LEMP webbserver på en Azure VM
Den här artikeln får du veta hur toodeploy en NGINX web server, MySQL och PHP (hello LEMP stack) på en Ubuntu VM i Azure. Hej LEMP stacken är ett populärt alternativ toohello [LYKTA stack](tutorial-lamp-stack.md), där du kan också installera i Azure. toosee hello LEMP server i åtgärden, kan du om du vill installera och konfigurera en WordPress-webbplats. I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa en Ubuntu VM (hello ”L” i hello LEMP stack)
> * Öppna port 80 för webbtrafik
> * Installera NGINX, MySQL och PHP
> * Verifiera installation och konfiguration
> * Installera WordPress på hello LEMP server


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>Installera NGINX, MySQL och PHP

Kör följande kommando tooupdate Ubuntu paketet källor hello och installera NGINX, MySQL och PHP. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

Du kan ange tooinstall hello paket och andra beroenden. När du uppmanas, anger du ett rotlösenord för MySQL och [Retur] toocontinue. Följ hello återstående uppmaningarna. Den här processen installerar hello minsta nödvändiga PHP tillägg som behövs toouse PHP med MySQL. 

![MySQL-rot lösenordssidan][1]

## <a name="verify-installation-and-configuration"></a>Verifiera installation och konfiguration


### <a name="nginx"></a>NGINX

Kontrollera hello version av NGINX med hello följande kommando:
```bash
nginx -v
```

Med NGINX installerad och port 80 öppna tooyour VM, kan nu komma åt hello-webbservern från hello internet. tooview hello NGINX välkomstsidan öppna en webbläsare och ange hello offentliga IP-adress hello VM. Använd hello offentlig IP-adress som du använde tooSSH toohello VM:

![NGINX standardsida][3]


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

Konfigurera NGINX toouse hello PHP FastCGI Process Manager (PHP-: FPM). Kör följande kommandon tooback hello ursprungliga NGINX Server blockera config-fil och sedan redigera hello originalfilen i ett redigeringsprogram hello:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

I Redigeraren för hello ersätter hello innehållet i `/etc/nginx/sites-available/default` med hello följande. Se hello kommentarer förklaring av hello inställningar. Ersätt hello offentliga IP-adressen för den virtuella datorn för *yourPublicIPAddress*, och lämna hello återstående inställningar. Spara hello-filen.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

Kontrollera hello NGINX-konfigurationen för Syntaxfel:

```bash
sudo nginx -t
```

Om hello syntax är korrekt kan du starta om NGINX med hello följande kommando:

```bash
sudo service nginx restart
```

Skapa en snabb PHP info sidan tooview i en webbläsare om du vill tootest ytterligare. hello följande kommando skapar hello PHP info-sida:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Nu kan du kontrollera hello PHP Infosidan som du skapat. Öppna en webbläsare och gå för`http://yourPublicIPAddress/info.php`. Ersätt hello offentliga IP-adressen för den virtuella datorn. Det bör se liknande toothis bild.

![Information om PHP-sida][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Nästa steg

I kursen får du har distribuerat en LEMP server i Azure. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en virtuell Ubuntu-dator
> * Öppna port 80 för webbtrafik
> * Installera NGINX, MySQL och PHP
> * Verifiera installation och konfiguration
> * Installera WordPress hello LEMP stacken

I förväg toohello nästa självstudiekurs toolearn hur toosecure webbservrar med SSL-certifikat.

> [!div class="nextstepaction"]
> [Säker webbserver med SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
