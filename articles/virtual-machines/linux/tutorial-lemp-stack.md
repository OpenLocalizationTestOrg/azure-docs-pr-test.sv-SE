---
title: "Distribuera LEMP på en Linux-dator i Azure | Microsoft Docs"
description: "Självstudiekurs – installera LEMP stacken på en Linux-VM i Azure"
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
ms.openlocfilehash: 653af144eb12cacf955f96a5442efd73add38e88
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a><span data-ttu-id="2854e-103">Installera en LEMP webbserver på en Azure VM</span><span class="sxs-lookup"><span data-stu-id="2854e-103">Install a LEMP web server on an Azure VM</span></span>
<span data-ttu-id="2854e-104">Den här artikeln får du veta hur du distribuerar en NGINX-webbservern, MySQL och PHP (LEMP stack) på en Ubuntu VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="2854e-104">This article walks you through how to deploy an NGINX web server, MySQL, and PHP (the LEMP stack) on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="2854e-105">LEMP-stacken är ett alternativ till att den populära [LYKTA stack](tutorial-lamp-stack.md), där du kan också installera i Azure.</span><span class="sxs-lookup"><span data-stu-id="2854e-105">The LEMP stack is an alternative to the popular [LAMP stack](tutorial-lamp-stack.md), which you can also install in Azure.</span></span> <span data-ttu-id="2854e-106">Om du vill se LEMP servern i praktiken du om du vill installera och konfigurera en WordPress-webbplats.</span><span class="sxs-lookup"><span data-stu-id="2854e-106">To see the LEMP server in action, you can optionally install and configure a WordPress site.</span></span> <span data-ttu-id="2854e-107">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="2854e-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2854e-108">Skapa en Ubuntu VM (den ”L” i LEMP stack)</span><span class="sxs-lookup"><span data-stu-id="2854e-108">Create an Ubuntu VM (the 'L' in the LEMP stack)</span></span>
> * <span data-ttu-id="2854e-109">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="2854e-109">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="2854e-110">Installera NGINX, MySQL och PHP</span><span class="sxs-lookup"><span data-stu-id="2854e-110">Install NGINX, MySQL, and PHP</span></span>
> * <span data-ttu-id="2854e-111">Verifiera installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="2854e-111">Verify installation and configuration</span></span>
> * <span data-ttu-id="2854e-112">Installera WordPress på servern LEMP</span><span class="sxs-lookup"><span data-stu-id="2854e-112">Install WordPress on the LEMP server</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="2854e-113">Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2854e-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="2854e-114">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="2854e-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="2854e-115">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2854e-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a><span data-ttu-id="2854e-116">Installera NGINX, MySQL och PHP</span><span class="sxs-lookup"><span data-stu-id="2854e-116">Install NGINX, MySQL, and PHP</span></span>

<span data-ttu-id="2854e-117">Kör följande kommando för att uppdatera Ubuntu paketet källor och installera NGINX, MySQL och PHP.</span><span class="sxs-lookup"><span data-stu-id="2854e-117">Run the following command to update Ubuntu package sources and install NGINX, MySQL, and PHP.</span></span> 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

<span data-ttu-id="2854e-118">Du uppmanas att installera paket och andra beroenden.</span><span class="sxs-lookup"><span data-stu-id="2854e-118">You are prompted to install the packages and other dependencies.</span></span> <span data-ttu-id="2854e-119">När du uppmanas, anger du ett rotlösenord för MySQL, och sedan [Retur] för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="2854e-119">When prompted, set a root password for MySQL, and then [Enter] to continue.</span></span> <span data-ttu-id="2854e-120">Följ anvisningarna för återstående.</span><span class="sxs-lookup"><span data-stu-id="2854e-120">Follow the remaining prompts.</span></span> <span data-ttu-id="2854e-121">Den här processen installerar de minsta nödvändiga PHP-tillägg för att kunna använda PHP med MySQL.</span><span class="sxs-lookup"><span data-stu-id="2854e-121">This process installs the minimum required PHP extensions needed to use PHP with MySQL.</span></span> 

![MySQL-rot lösenordssidan][1]

## <a name="verify-installation-and-configuration"></a><span data-ttu-id="2854e-123">Verifiera installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="2854e-123">Verify installation and configuration</span></span>


### <a name="nginx"></a><span data-ttu-id="2854e-124">NGINX</span><span class="sxs-lookup"><span data-stu-id="2854e-124">NGINX</span></span>

<span data-ttu-id="2854e-125">Kontrollera vilken version av NGINX med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2854e-125">Check the version of NGINX with the following command:</span></span>
```bash
nginx -v
```

<span data-ttu-id="2854e-126">Med NGINX installerat, och port 80 är öppen för den virtuella datorn, kan du nu åt webbservern från internet.</span><span class="sxs-lookup"><span data-stu-id="2854e-126">With NGINX installed, and port 80 open to your VM, the web server can now be accessed from the internet.</span></span> <span data-ttu-id="2854e-127">Om du vill visa NGINX välkomstsidan, öppna en webbläsare och ange offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2854e-127">To view the NGINX welcome page, open a web browser, and enter the public IP address of the VM.</span></span> <span data-ttu-id="2854e-128">Använd den offentliga IP-adressen som du använde för att SSH till den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="2854e-128">Use the public IP address you used to SSH to the VM:</span></span>

![NGINX standardsida][3]


### <a name="mysql"></a><span data-ttu-id="2854e-130">MySQL</span><span class="sxs-lookup"><span data-stu-id="2854e-130">MySQL</span></span>

<span data-ttu-id="2854e-131">Kontrollera vilken version av MySQL med följande kommando (Observera kapital `V` parametern):</span><span class="sxs-lookup"><span data-stu-id="2854e-131">Check the version of MySQL with the following command (note the capital `V` parameter):</span></span>

```bash
msql -V
```

<span data-ttu-id="2854e-132">Vi rekommenderar att du kör följande skript om du vill skydda installationen av MySQL:</span><span class="sxs-lookup"><span data-stu-id="2854e-132">We recommend running the following script to help secure the installation of MySQL:</span></span>

```bash
mysql_secure_installation
```

<span data-ttu-id="2854e-133">Ange lösenordet för roten MySQL och konfigurera säkerhetsinställningarna för din miljö.</span><span class="sxs-lookup"><span data-stu-id="2854e-133">Enter your MySQL root password, and configure the security settings for your environment.</span></span>

<span data-ttu-id="2854e-134">Om du vill skapa en MySQL-databas, lägga till användare eller ändra konfigurationsinställningarna, logga in på MySQL:</span><span class="sxs-lookup"><span data-stu-id="2854e-134">If you want to create a MySQL database, add users, or change configuration settings, login to MySQL:</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="2854e-135">När du är klar avsluta mysql-Kommandotolken genom att skriva `\q`.</span><span class="sxs-lookup"><span data-stu-id="2854e-135">When done, exit the mysql prompt by typing `\q`.</span></span>

### <a name="php"></a><span data-ttu-id="2854e-136">PHP</span><span class="sxs-lookup"><span data-stu-id="2854e-136">PHP</span></span>

<span data-ttu-id="2854e-137">Kontrollera versionen av PHP med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2854e-137">Check the version of PHP with the following command:</span></span>

```bash
php -v
```

<span data-ttu-id="2854e-138">Konfigurera NGINX om du vill använda PHP FastCGI Process Manager (PHP-: FPM).</span><span class="sxs-lookup"><span data-stu-id="2854e-138">Configure NGINX to use the PHP FastCGI Process Manager (PHP-FPM).</span></span> <span data-ttu-id="2854e-139">Kör följande kommandon för att säkerhetskopiera den ursprungliga NGINX server block config-fil och sedan redigera den ursprungliga filen i ett redigeringsprogram:</span><span class="sxs-lookup"><span data-stu-id="2854e-139">Run the following commands to back up the original NGINX server block config file and then edit the original file in an editor of your choice:</span></span>

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

<span data-ttu-id="2854e-140">I redigeraren, ersätter du innehållet i `/etc/nginx/sites-available/default` med följande.</span><span class="sxs-lookup"><span data-stu-id="2854e-140">In the editor, replace the contents of `/etc/nginx/sites-available/default` with the following.</span></span> <span data-ttu-id="2854e-141">Visa kommentarerna förklaring av inställningarna.</span><span class="sxs-lookup"><span data-stu-id="2854e-141">See the comments for explanation of the settings.</span></span> <span data-ttu-id="2854e-142">Ersätt offentliga IP-adressen för den virtuella datorn för *yourPublicIPAddress*, och lämna de återstående inställningarna.</span><span class="sxs-lookup"><span data-stu-id="2854e-142">Substitute the public IP address of your VM for *yourPublicIPAddress*, and leave the remaining settings.</span></span> <span data-ttu-id="2854e-143">Spara sedan filen.</span><span class="sxs-lookup"><span data-stu-id="2854e-143">Then save the file.</span></span>

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

<span data-ttu-id="2854e-144">Kontrollera NGINX-konfigurationen för Syntaxfel:</span><span class="sxs-lookup"><span data-stu-id="2854e-144">Check the NGINX configuration for syntax errors:</span></span>

```bash
sudo nginx -t
```

<span data-ttu-id="2854e-145">Om syntax är korrekt kan du starta om NGINX med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2854e-145">If the syntax is correct, restart NGINX with the following command:</span></span>

```bash
sudo service nginx restart
```

<span data-ttu-id="2854e-146">Om du vill testa ytterligare skapar du en snabb PHP info sida för att visa i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="2854e-146">If you want to test further, create a quick PHP info page to view in a browser.</span></span> <span data-ttu-id="2854e-147">Följande kommando skapar sidan PHP:</span><span class="sxs-lookup"><span data-stu-id="2854e-147">The following command creates the PHP info page:</span></span>

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



<span data-ttu-id="2854e-148">Nu kan du kontrollera PHP Infosidan som du skapat.</span><span class="sxs-lookup"><span data-stu-id="2854e-148">Now you can check the PHP info page you created.</span></span> <span data-ttu-id="2854e-149">Öppna en webbläsare och gå till `http://yourPublicIPAddress/info.php`.</span><span class="sxs-lookup"><span data-stu-id="2854e-149">Open a browser and go to `http://yourPublicIPAddress/info.php`.</span></span> <span data-ttu-id="2854e-150">Ersätt offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2854e-150">Substitute the public IP address of your VM.</span></span> <span data-ttu-id="2854e-151">Det bör likna den här avbildningen.</span><span class="sxs-lookup"><span data-stu-id="2854e-151">It should look similar to this image.</span></span>

![Information om PHP-sida][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a><span data-ttu-id="2854e-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2854e-153">Next steps</span></span>

<span data-ttu-id="2854e-154">I kursen får du har distribuerat en LEMP server i Azure.</span><span class="sxs-lookup"><span data-stu-id="2854e-154">In this tutorial, you deployed a LEMP server in Azure.</span></span> <span data-ttu-id="2854e-155">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="2854e-155">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2854e-156">Skapa en virtuell Ubuntu-dator</span><span class="sxs-lookup"><span data-stu-id="2854e-156">Create an Ubuntu VM</span></span>
> * <span data-ttu-id="2854e-157">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="2854e-157">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="2854e-158">Installera NGINX, MySQL och PHP</span><span class="sxs-lookup"><span data-stu-id="2854e-158">Install NGINX, MySQL, and PHP</span></span>
> * <span data-ttu-id="2854e-159">Verifiera installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="2854e-159">Verify installation and configuration</span></span>
> * <span data-ttu-id="2854e-160">Installera WordPress LEMP-stacken</span><span class="sxs-lookup"><span data-stu-id="2854e-160">Install WordPress on the LEMP stack</span></span>

<span data-ttu-id="2854e-161">Gå vidare till nästa kurs att lära dig säker webbserver med SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="2854e-161">Advance to the next tutorial to learn how to secure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2854e-162">Säker webbserver med SSL</span><span class="sxs-lookup"><span data-stu-id="2854e-162">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
