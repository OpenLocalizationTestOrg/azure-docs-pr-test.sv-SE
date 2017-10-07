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
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a><span data-ttu-id="4bcfa-103">Installera en LEMP webbserver på en Azure VM</span><span class="sxs-lookup"><span data-stu-id="4bcfa-103">Install a LEMP web server on an Azure VM</span></span>
<span data-ttu-id="4bcfa-104">Den här artikeln får du veta hur toodeploy en NGINX web server, MySQL och PHP (hello LEMP stack) på en Ubuntu VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-104">This article walks you through how toodeploy an NGINX web server, MySQL, and PHP (hello LEMP stack) on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="4bcfa-105">Hej LEMP stacken är ett populärt alternativ toohello [LYKTA stack](tutorial-lamp-stack.md), där du kan också installera i Azure.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-105">hello LEMP stack is an alternative toohello popular [LAMP stack](tutorial-lamp-stack.md), which you can also install in Azure.</span></span> <span data-ttu-id="4bcfa-106">toosee hello LEMP server i åtgärden, kan du om du vill installera och konfigurera en WordPress-webbplats.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-106">toosee hello LEMP server in action, you can optionally install and configure a WordPress site.</span></span> <span data-ttu-id="4bcfa-107">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4bcfa-108">Skapa en Ubuntu VM (hello ”L” i hello LEMP stack)</span><span class="sxs-lookup"><span data-stu-id="4bcfa-108">Create an Ubuntu VM (hello 'L' in hello LEMP stack)</span></span>
> * <span data-ttu-id="4bcfa-109">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="4bcfa-109">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="4bcfa-110">Installera NGINX, MySQL och PHP</span><span class="sxs-lookup"><span data-stu-id="4bcfa-110">Install NGINX, MySQL, and PHP</span></span>
> * <span data-ttu-id="4bcfa-111">Verifiera installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="4bcfa-111">Verify installation and configuration</span></span>
> * <span data-ttu-id="4bcfa-112">Installera WordPress på hello LEMP server</span><span class="sxs-lookup"><span data-stu-id="4bcfa-112">Install WordPress on hello LEMP server</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4bcfa-113">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="4bcfa-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="4bcfa-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4bcfa-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a><span data-ttu-id="4bcfa-116">Installera NGINX, MySQL och PHP</span><span class="sxs-lookup"><span data-stu-id="4bcfa-116">Install NGINX, MySQL, and PHP</span></span>

<span data-ttu-id="4bcfa-117">Kör följande kommando tooupdate Ubuntu paketet källor hello och installera NGINX, MySQL och PHP.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-117">Run hello following command tooupdate Ubuntu package sources and install NGINX, MySQL, and PHP.</span></span> 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

<span data-ttu-id="4bcfa-118">Du kan ange tooinstall hello paket och andra beroenden.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-118">You are prompted tooinstall hello packages and other dependencies.</span></span> <span data-ttu-id="4bcfa-119">När du uppmanas, anger du ett rotlösenord för MySQL och [Retur] toocontinue.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-119">When prompted, set a root password for MySQL, and then [Enter] toocontinue.</span></span> <span data-ttu-id="4bcfa-120">Följ hello återstående uppmaningarna.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-120">Follow hello remaining prompts.</span></span> <span data-ttu-id="4bcfa-121">Den här processen installerar hello minsta nödvändiga PHP tillägg som behövs toouse PHP med MySQL.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-121">This process installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![MySQL-rot lösenordssidan][1]

## <a name="verify-installation-and-configuration"></a><span data-ttu-id="4bcfa-123">Verifiera installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="4bcfa-123">Verify installation and configuration</span></span>


### <a name="nginx"></a><span data-ttu-id="4bcfa-124">NGINX</span><span class="sxs-lookup"><span data-stu-id="4bcfa-124">NGINX</span></span>

<span data-ttu-id="4bcfa-125">Kontrollera hello version av NGINX med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-125">Check hello version of NGINX with hello following command:</span></span>
```bash
nginx -v
```

<span data-ttu-id="4bcfa-126">Med NGINX installerad och port 80 öppna tooyour VM, kan nu komma åt hello-webbservern från hello internet.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-126">With NGINX installed, and port 80 open tooyour VM, hello web server can now be accessed from hello internet.</span></span> <span data-ttu-id="4bcfa-127">tooview hello NGINX välkomstsidan öppna en webbläsare och ange hello offentliga IP-adress hello VM.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-127">tooview hello NGINX welcome page, open a web browser, and enter hello public IP address of hello VM.</span></span> <span data-ttu-id="4bcfa-128">Använd hello offentlig IP-adress som du använde tooSSH toohello VM:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-128">Use hello public IP address you used tooSSH toohello VM:</span></span>

![NGINX standardsida][3]


### <a name="mysql"></a><span data-ttu-id="4bcfa-130">MySQL</span><span class="sxs-lookup"><span data-stu-id="4bcfa-130">MySQL</span></span>

<span data-ttu-id="4bcfa-131">Kontrollera hello version av MySQL med hello följande kommando (Observera hello kapital `V` parametern):</span><span class="sxs-lookup"><span data-stu-id="4bcfa-131">Check hello version of MySQL with hello following command (note hello capital `V` parameter):</span></span>

```bash
msql -V
```

<span data-ttu-id="4bcfa-132">Vi rekommenderar att du kör skriptet toohelp säker hello installation av MySQL hello:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-132">We recommend running hello following script toohelp secure hello installation of MySQL:</span></span>

```bash
mysql_secure_installation
```

<span data-ttu-id="4bcfa-133">Ange lösenordet för roten MySQL och konfigurera hello säkerhetsinställningar för din miljö.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-133">Enter your MySQL root password, and configure hello security settings for your environment.</span></span>

<span data-ttu-id="4bcfa-134">Om du vill toocreate en MySQL-databas, lägga till användare eller ändra konfigurationsinställningarna, inloggning tooMySQL:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-134">If you want toocreate a MySQL database, add users, or change configuration settings, login tooMySQL:</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="4bcfa-135">När du är klar avsluta hello mysql Kommandotolken genom att skriva `\q`.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-135">When done, exit hello mysql prompt by typing `\q`.</span></span>

### <a name="php"></a><span data-ttu-id="4bcfa-136">PHP</span><span class="sxs-lookup"><span data-stu-id="4bcfa-136">PHP</span></span>

<span data-ttu-id="4bcfa-137">Kontrollera hello-versionen av PHP med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-137">Check hello version of PHP with hello following command:</span></span>

```bash
php -v
```

<span data-ttu-id="4bcfa-138">Konfigurera NGINX toouse hello PHP FastCGI Process Manager (PHP-: FPM).</span><span class="sxs-lookup"><span data-stu-id="4bcfa-138">Configure NGINX toouse hello PHP FastCGI Process Manager (PHP-FPM).</span></span> <span data-ttu-id="4bcfa-139">Kör följande kommandon tooback hello ursprungliga NGINX Server blockera config-fil och sedan redigera hello originalfilen i ett redigeringsprogram hello:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-139">Run hello following commands tooback up hello original NGINX server block config file and then edit hello original file in an editor of your choice:</span></span>

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

<span data-ttu-id="4bcfa-140">I Redigeraren för hello ersätter hello innehållet i `/etc/nginx/sites-available/default` med hello följande.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-140">In hello editor, replace hello contents of `/etc/nginx/sites-available/default` with hello following.</span></span> <span data-ttu-id="4bcfa-141">Se hello kommentarer förklaring av hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-141">See hello comments for explanation of hello settings.</span></span> <span data-ttu-id="4bcfa-142">Ersätt hello offentliga IP-adressen för den virtuella datorn för *yourPublicIPAddress*, och lämna hello återstående inställningar.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-142">Substitute hello public IP address of your VM for *yourPublicIPAddress*, and leave hello remaining settings.</span></span> <span data-ttu-id="4bcfa-143">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-143">Then save hello file.</span></span>

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

<span data-ttu-id="4bcfa-144">Kontrollera hello NGINX-konfigurationen för Syntaxfel:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-144">Check hello NGINX configuration for syntax errors:</span></span>

```bash
sudo nginx -t
```

<span data-ttu-id="4bcfa-145">Om hello syntax är korrekt kan du starta om NGINX med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-145">If hello syntax is correct, restart NGINX with hello following command:</span></span>

```bash
sudo service nginx restart
```

<span data-ttu-id="4bcfa-146">Skapa en snabb PHP info sidan tooview i en webbläsare om du vill tootest ytterligare.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-146">If you want tootest further, create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="4bcfa-147">hello följande kommando skapar hello PHP info-sida:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-147">hello following command creates hello PHP info page:</span></span>

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



<span data-ttu-id="4bcfa-148">Nu kan du kontrollera hello PHP Infosidan som du skapat.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-148">Now you can check hello PHP info page you created.</span></span> <span data-ttu-id="4bcfa-149">Öppna en webbläsare och gå för`http://yourPublicIPAddress/info.php`.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-149">Open a browser and go too`http://yourPublicIPAddress/info.php`.</span></span> <span data-ttu-id="4bcfa-150">Ersätt hello offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-150">Substitute hello public IP address of your VM.</span></span> <span data-ttu-id="4bcfa-151">Det bör se liknande toothis bild.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-151">It should look similar toothis image.</span></span>

![Information om PHP-sida][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a><span data-ttu-id="4bcfa-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4bcfa-153">Next steps</span></span>

<span data-ttu-id="4bcfa-154">I kursen får du har distribuerat en LEMP server i Azure.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-154">In this tutorial, you deployed a LEMP server in Azure.</span></span> <span data-ttu-id="4bcfa-155">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="4bcfa-155">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4bcfa-156">Skapa en virtuell Ubuntu-dator</span><span class="sxs-lookup"><span data-stu-id="4bcfa-156">Create an Ubuntu VM</span></span>
> * <span data-ttu-id="4bcfa-157">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="4bcfa-157">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="4bcfa-158">Installera NGINX, MySQL och PHP</span><span class="sxs-lookup"><span data-stu-id="4bcfa-158">Install NGINX, MySQL, and PHP</span></span>
> * <span data-ttu-id="4bcfa-159">Verifiera installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="4bcfa-159">Verify installation and configuration</span></span>
> * <span data-ttu-id="4bcfa-160">Installera WordPress hello LEMP stacken</span><span class="sxs-lookup"><span data-stu-id="4bcfa-160">Install WordPress on hello LEMP stack</span></span>

<span data-ttu-id="4bcfa-161">I förväg toohello nästa självstudiekurs toolearn hur toosecure webbservrar med SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="4bcfa-161">Advance toohello next tutorial toolearn how toosecure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4bcfa-162">Säker webbserver med SSL</span><span class="sxs-lookup"><span data-stu-id="4bcfa-162">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
