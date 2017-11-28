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
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a><span data-ttu-id="75563-103">Installera en LYKTA webbserver på en Azure VM</span><span class="sxs-lookup"><span data-stu-id="75563-103">Install a LAMP web server on an Azure VM</span></span>
<span data-ttu-id="75563-104">Den här artikeln får du veta hur toodeploy en Apache web server, MySQL och PHP (hello LYKTA stack) på en Ubuntu VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="75563-104">This article walks you through how toodeploy an Apache web server, MySQL, and PHP (hello LAMP stack) on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="75563-105">Om du föredrar hello NGINX-webbservern finns hello [LEMP stack](tutorial-lemp-stack.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="75563-105">If you prefer hello NGINX web server, see hello [LEMP stack](tutorial-lemp-stack.md) tutorial.</span></span> <span data-ttu-id="75563-106">toosee hello LYKTA server i åtgärden, kan du om du vill installera och konfigurera en WordPress-webbplats.</span><span class="sxs-lookup"><span data-stu-id="75563-106">toosee hello LAMP server in action, you can optionally install and configure a WordPress site.</span></span> <span data-ttu-id="75563-107">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="75563-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="75563-108">Skapa en Ubuntu VM (hello ”L” i hello LYKTA stack)</span><span class="sxs-lookup"><span data-stu-id="75563-108">Create an Ubuntu VM (hello 'L' in hello LAMP stack)</span></span>
> * <span data-ttu-id="75563-109">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="75563-109">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="75563-110">Installera Apache, MySQL och PHP</span><span class="sxs-lookup"><span data-stu-id="75563-110">Install Apache, MySQL, and PHP</span></span>
> * <span data-ttu-id="75563-111">Verifiera installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="75563-111">Verify installation and configuration</span></span>
> * <span data-ttu-id="75563-112">Installera WordPress på hello LYKTA server</span><span class="sxs-lookup"><span data-stu-id="75563-112">Install WordPress on hello LAMP server</span></span>


<span data-ttu-id="75563-113">Mer information om hello LYKTA stacken, inklusive rekommendationer för en produktionsmiljö finns hello [Ubuntu dokumentationen](https://help.ubuntu.com/community/ApacheMySQLPHP).</span><span class="sxs-lookup"><span data-stu-id="75563-113">For more on hello LAMP stack, including recommendations for a production environment, see hello [Ubuntu documentation](https://help.ubuntu.com/community/ApacheMySQLPHP).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="75563-114">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="75563-114">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="75563-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="75563-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="75563-116">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="75563-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a><span data-ttu-id="75563-117">Installera Apache, MySQL och PHP</span><span class="sxs-lookup"><span data-stu-id="75563-117">Install Apache, MySQL, and PHP</span></span>

<span data-ttu-id="75563-118">Kör följande kommando tooupdate Ubuntu paketet källor hello och installera Apache, MySQL och PHP.</span><span class="sxs-lookup"><span data-stu-id="75563-118">Run hello following command tooupdate Ubuntu package sources and install Apache, MySQL, and PHP.</span></span> <span data-ttu-id="75563-119">Observera hello hatt (^) hello slutet av hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="75563-119">Note hello caret (^) at hello end of hello command.</span></span>


```bash
sudo apt update && sudo apt install lamp-server^
```



<span data-ttu-id="75563-120">Du kan ange tooinstall hello paket och andra beroenden.</span><span class="sxs-lookup"><span data-stu-id="75563-120">You are prompted tooinstall hello packages and other dependencies.</span></span> <span data-ttu-id="75563-121">När du uppmanas, anger du ett rotlösenord för MySQL och [Retur] toocontinue.</span><span class="sxs-lookup"><span data-stu-id="75563-121">When prompted, set a root password for MySQL, and then [Enter] toocontinue.</span></span> <span data-ttu-id="75563-122">Följ hello återstående uppmaningarna.</span><span class="sxs-lookup"><span data-stu-id="75563-122">Follow hello remaining prompts.</span></span> <span data-ttu-id="75563-123">Den här processen installerar hello minsta nödvändiga PHP tillägg som behövs toouse PHP med MySQL.</span><span class="sxs-lookup"><span data-stu-id="75563-123">This process installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![MySQL-rot lösenordssidan][1]

## <a name="verify-installation-and-configuration"></a><span data-ttu-id="75563-125">Verifiera installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="75563-125">Verify installation and configuration</span></span>


### <a name="apache"></a><span data-ttu-id="75563-126">Apache</span><span class="sxs-lookup"><span data-stu-id="75563-126">Apache</span></span>

<span data-ttu-id="75563-127">Kontrollera hello version av Apache med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75563-127">Check hello version of Apache with hello following command:</span></span>
```bash
apache2 -v
```

<span data-ttu-id="75563-128">Med Apache installerad och port 80 öppna tooyour VM, kan nu komma åt hello-webbservern från hello internet.</span><span class="sxs-lookup"><span data-stu-id="75563-128">With Apache installed, and port 80 open tooyour VM, hello web server can now be accessed from hello internet.</span></span> <span data-ttu-id="75563-129">tooview hello Apache2 Ubuntu standardsida, öppna en webbläsare och ange hello offentliga IP-adress hello VM.</span><span class="sxs-lookup"><span data-stu-id="75563-129">tooview hello Apache2 Ubuntu Default Page, open a web browser, and enter hello public IP address of hello VM.</span></span> <span data-ttu-id="75563-130">Använd hello offentlig IP-adress som du använde tooSSH toohello VM:</span><span class="sxs-lookup"><span data-stu-id="75563-130">Use hello public IP address you used tooSSH toohello VM:</span></span>

![Apache standardsida][3]


### <a name="mysql"></a><span data-ttu-id="75563-132">MySQL</span><span class="sxs-lookup"><span data-stu-id="75563-132">MySQL</span></span>

<span data-ttu-id="75563-133">Kontrollera hello version av MySQL med hello följande kommando (Observera hello kapital `V` parametern):</span><span class="sxs-lookup"><span data-stu-id="75563-133">Check hello version of MySQL with hello following command (note hello capital `V` parameter):</span></span>

```bash
msql -V
```

<span data-ttu-id="75563-134">Vi rekommenderar att du kör skriptet toohelp säker hello installation av MySQL hello:</span><span class="sxs-lookup"><span data-stu-id="75563-134">We recommend running hello following script toohelp secure hello installation of MySQL:</span></span>

```bash
mysql_secure_installation
```

<span data-ttu-id="75563-135">Ange lösenordet för roten MySQL och konfigurera hello säkerhetsinställningar för din miljö.</span><span class="sxs-lookup"><span data-stu-id="75563-135">Enter your MySQL root password, and configure hello security settings for your environment.</span></span>

<span data-ttu-id="75563-136">Om du vill toocreate en MySQL-databas, lägga till användare eller ändra konfigurationsinställningarna, inloggning tooMySQL:</span><span class="sxs-lookup"><span data-stu-id="75563-136">If you want toocreate a MySQL database, add users, or change configuration settings, login tooMySQL:</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="75563-137">När du är klar avsluta hello mysql Kommandotolken genom att skriva `\q`.</span><span class="sxs-lookup"><span data-stu-id="75563-137">When done, exit hello mysql prompt by typing `\q`.</span></span>

### <a name="php"></a><span data-ttu-id="75563-138">PHP</span><span class="sxs-lookup"><span data-stu-id="75563-138">PHP</span></span>

<span data-ttu-id="75563-139">Kontrollera hello-versionen av PHP med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75563-139">Check hello version of PHP with hello following command:</span></span>

```bash
php -v
```

<span data-ttu-id="75563-140">Skapa en snabb PHP info sidan tooview i en webbläsare om du vill tootest ytterligare.</span><span class="sxs-lookup"><span data-stu-id="75563-140">If you want tootest further, create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="75563-141">hello följande kommando skapar hello PHP info-sida:</span><span class="sxs-lookup"><span data-stu-id="75563-141">hello following command creates hello PHP info page:</span></span>

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

<span data-ttu-id="75563-142">Nu kan du kontrollera hello PHP Infosidan som du skapat.</span><span class="sxs-lookup"><span data-stu-id="75563-142">Now you can check hello PHP info page you created.</span></span> <span data-ttu-id="75563-143">Öppna en webbläsare och gå för`http://yourPublicIPAddress/info.php`.</span><span class="sxs-lookup"><span data-stu-id="75563-143">Open a browser and go too`http://yourPublicIPAddress/info.php`.</span></span> <span data-ttu-id="75563-144">Ersätt hello offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="75563-144">Substitute hello public IP address of your VM.</span></span> <span data-ttu-id="75563-145">Det bör se liknande toothis bild.</span><span class="sxs-lookup"><span data-stu-id="75563-145">It should look similar toothis image.</span></span>

![Information om PHP-sida][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a><span data-ttu-id="75563-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75563-147">Next steps</span></span>

<span data-ttu-id="75563-148">I kursen får distribuerat du en LYKTA server i Azure.</span><span class="sxs-lookup"><span data-stu-id="75563-148">In this tutorial, you deployed a LAMP server in Azure.</span></span> <span data-ttu-id="75563-149">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="75563-149">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="75563-150">Skapa en virtuell Ubuntu-dator</span><span class="sxs-lookup"><span data-stu-id="75563-150">Create an Ubuntu VM</span></span>
> * <span data-ttu-id="75563-151">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="75563-151">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="75563-152">Installera Apache, MySQL och PHP</span><span class="sxs-lookup"><span data-stu-id="75563-152">Install Apache, MySQL, and PHP</span></span>
> * <span data-ttu-id="75563-153">Verifiera installation och konfiguration</span><span class="sxs-lookup"><span data-stu-id="75563-153">Verify installation and configuration</span></span>
> * <span data-ttu-id="75563-154">Installera WordPress på hello LYKTA server</span><span class="sxs-lookup"><span data-stu-id="75563-154">Install WordPress on hello LAMP server</span></span>

<span data-ttu-id="75563-155">I förväg toohello nästa självstudiekurs toolearn hur toosecure webbservrar med SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="75563-155">Advance toohello next tutorial toolearn how toosecure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="75563-156">Säker webbserver med SSL</span><span class="sxs-lookup"><span data-stu-id="75563-156">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png