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
# <a name="deploy-lamp-stack-with-hello-azure-cli-10"></a><span data-ttu-id="e7599-103">Distribuera LYKTA stacken med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e7599-103">Deploy LAMP stack with hello Azure CLI 1.0</span></span>
<span data-ttu-id="e7599-104">Den här artikeln får du veta hur toodeploy en Apache web server, MySQL och PHP (hello LYKTA stack) på Azure.</span><span class="sxs-lookup"><span data-stu-id="e7599-104">This article walks you through how toodeploy an Apache web server, MySQL, and PHP (hello LAMP stack) on Azure.</span></span> <span data-ttu-id="e7599-105">Du behöver ett Azure-konto ([skaffa en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)) och hello [Azure CLI](../../cli-install-nodejs.md) som [anslutna tooyour Azure-konto](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="e7599-105">You need an Azure Account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and hello [Azure CLI](../../cli-install-nodejs.md) that is [connected tooyour Azure account](../../xplat-cli-connect.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="e7599-106">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="e7599-106">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="e7599-107">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="e7599-107">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="e7599-108">[Azure CLI 1.0] – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="e7599-108">[Azure CLI 1.0] – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="e7599-109">[Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="e7599-109">[Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

```
# One command toocreate a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* <span data-ttu-id="e7599-110">Distribuera LYKTA på befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e7599-110">Deploy LAMP on existing VM</span></span>

```
# Two commands: one updates packages, hello other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="e7599-111">Distribuera LYKTA på den nya VM genomgång</span><span class="sxs-lookup"><span data-stu-id="e7599-111">Deploy LAMP on new VM walkthrough</span></span>
<span data-ttu-id="e7599-112">Du kan börja med att skapa en [resursgruppen](../../azure-resource-manager/resource-group-overview.md) kommer att innehålla hello ny virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="e7599-112">You can start by creating a [resource group](../../azure-resource-manager/resource-group-overview.md) that will contain hello new VM:</span></span>

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

<span data-ttu-id="e7599-113">toocreate hello Virtuella datorn kan du använda ett redan skrivits Azure Resource Manager mallen [här på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="e7599-113">toocreate hello VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

<span data-ttu-id="e7599-114">Du bör se svar uppmanar vissa fler indata:</span><span class="sxs-lookup"><span data-stu-id="e7599-114">You should see a response prompting some more inputs:</span></span>

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

<span data-ttu-id="e7599-115">Nu har du skapat en Linux VM med LYKTA redan installerad på den.</span><span class="sxs-lookup"><span data-stu-id="e7599-115">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="e7599-116">Om du vill kan du kontrollera hello installationen genom att hoppa över ned för[Kontrollera LYKTA installerats](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="e7599-116">If you wish, you can verify hello install by jumping down too[Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="e7599-117">Distribuera LYKTA på befintliga Virtuella genomgång</span><span class="sxs-lookup"><span data-stu-id="e7599-117">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="e7599-118">Om du behöver hjälp med att skapa en Linux VM, kan du gå [här toolearn hur toocreate en Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e7599-118">If you need help creating a Linux VM, you can head [here toolearn how toocreate a Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="e7599-119">Sedan måste tooSSH i hello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="e7599-119">Next, you need tooSSH into hello Linux VM.</span></span> <span data-ttu-id="e7599-120">Om du behöver hjälp med att skapa en SSH-nyckel kan du gå [här toolearn hur toocreate en SSH-nyckel på Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e7599-120">If you need help with creating an SSH key, you can head [here toolearn how toocreate an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="e7599-121">Om du redan har en SSH-nyckel kan gå vidare och SSH från kommandoraden till din Linux VM med `ssh exampleUsername@exampleDNS`.</span><span class="sxs-lookup"><span data-stu-id="e7599-121">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh exampleUsername@exampleDNS`.</span></span>

<span data-ttu-id="e7599-122">Nu när du arbetar inom din Linux VM går vi igenom installerar hello LYKTA stack på Debian-baserade distributioner.</span><span class="sxs-lookup"><span data-stu-id="e7599-122">Now that you are working within your Linux VM, we can walk through installing hello LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="e7599-123">hello kommandona kan skilja sig åt andra Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="e7599-123">hello exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="e7599-124">Installera på Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e7599-124">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="e7599-125">Du behöver hello följande paket som har installerats: `apache2`, `mysql-server`, `php5`, och `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="e7599-125">You need hello following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="e7599-126">Du kan installera dessa paket genom att direkt ta tag paketen eller använda Tasksel.</span><span class="sxs-lookup"><span data-stu-id="e7599-126">You can install these packages by directly grabbing these packages or using Tasksel.</span></span> <span data-ttu-id="e7599-127">Instruktioner för båda alternativen visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e7599-127">Instructions for both options are listed below.</span></span>
<span data-ttu-id="e7599-128">Innan du installerar du behöver toodownload och uppdatera paketet listor.</span><span class="sxs-lookup"><span data-stu-id="e7599-128">Before installing you need toodownload and update package lists.</span></span>

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a><span data-ttu-id="e7599-129">Enskilda paket</span><span class="sxs-lookup"><span data-stu-id="e7599-129">Individual packages</span></span>
<span data-ttu-id="e7599-130">Använda lgh get:</span><span class="sxs-lookup"><span data-stu-id="e7599-130">Using apt-get:</span></span>

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a><span data-ttu-id="e7599-131">Med hjälp av tasksel</span><span class="sxs-lookup"><span data-stu-id="e7599-131">Using tasksel</span></span>
<span data-ttu-id="e7599-132">Alternativt kan du hämta Tasksel, ett Debian/Ubuntu-verktyg som du installerar flera relaterade paket som en samordnad ”task” på datorn.</span><span class="sxs-lookup"><span data-stu-id="e7599-132">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

<span data-ttu-id="e7599-133">När du kör något av föregående alternativ för hello, kommer du att ange tooinstall paketen och andra beroenden.</span><span class="sxs-lookup"><span data-stu-id="e7599-133">After running either of hello previous options, you will be prompted tooinstall these packages and various other dependencies.</span></span> <span data-ttu-id="e7599-134">Tryck på 'y' och 'Enter' toocontinue och följ alla andra prompter tooset det administrativa lösenordet för MySQL.</span><span class="sxs-lookup"><span data-stu-id="e7599-134">Press 'y' and then 'Enter' toocontinue, and follow any other prompts tooset an administrative password for MySQL.</span></span> <span data-ttu-id="e7599-135">Detta installerar hello minsta nödvändiga PHP tillägg som behövs toouse PHP med MySQL.</span><span class="sxs-lookup"><span data-stu-id="e7599-135">This installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="e7599-136">Kör följande kommando toosee hello andra PHP-tillägg som är tillgängliga som paket:</span><span class="sxs-lookup"><span data-stu-id="e7599-136">Run hello following command toosee other PHP extensions that are available as packages:</span></span>

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a><span data-ttu-id="e7599-137">Skapa info.php dokument</span><span class="sxs-lookup"><span data-stu-id="e7599-137">Create info.php document</span></span>
<span data-ttu-id="e7599-138">Nu bör du kunna toocheck vilken version av Apache, MySQL och PHP du ha via hello kommandoraden genom att skriva `apache2 -v`, `mysql -v`, eller `php -v`.</span><span class="sxs-lookup"><span data-stu-id="e7599-138">You should now be able toocheck what version of Apache, MySQL, and PHP you have through hello command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="e7599-139">Om du skulle t.ex tootest ytterligare, kan du skapa en snabb PHP info sidan tooview i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="e7599-139">If you would like tootest further, you can create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="e7599-140">Skapa en fil med Nano textredigerare med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="e7599-140">Create a file with Nano text editor with this command:</span></span>

    user@ubuntu$ sudo nano /var/www/html/info.php

<span data-ttu-id="e7599-141">Lägg till hello följande rader i hello GNU Nano textredigerare:</span><span class="sxs-lookup"><span data-stu-id="e7599-141">Within hello GNU Nano text editor, add hello following lines:</span></span>

    <?php
    phpinfo();
    ?>

<span data-ttu-id="e7599-142">Sedan spara och avsluta hello textredigerare.</span><span class="sxs-lookup"><span data-stu-id="e7599-142">Then save and exit hello text editor.</span></span>

<span data-ttu-id="e7599-143">Starta om Apache med det här kommandot så att alla nya installerar börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="e7599-143">Restart Apache with this command so all new installs take effect.</span></span>

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="e7599-144">Kontrollera LAMPAN har installerats</span><span class="sxs-lookup"><span data-stu-id="e7599-144">Verify LAMP successfully installed</span></span>
<span data-ttu-id="e7599-145">Nu kan du kontrollera hello PHP Infosidan som du skapade genom att öppna en webbläsare och gå toohttp://youruniqueDNS/info.php.</span><span class="sxs-lookup"><span data-stu-id="e7599-145">Now you can check hello PHP info page you created by opening a browser and going toohttp://youruniqueDNS/info.php.</span></span> <span data-ttu-id="e7599-146">Det bör se liknande toothis bild.</span><span class="sxs-lookup"><span data-stu-id="e7599-146">It should look similar toothis image.</span></span>

![][2]

<span data-ttu-id="e7599-147">Du kan kontrollera installationen av Apache genom att visa hello Apache2 Ubuntu standardsida genom att gå tooyou http://youruniqueDNS/.</span><span class="sxs-lookup"><span data-stu-id="e7599-147">You can check your Apache installation by viewing hello Apache2 Ubuntu Default Page by going tooyou http://youruniqueDNS/.</span></span> <span data-ttu-id="e7599-148">Du bör se något som liknar den här avbildningen.</span><span class="sxs-lookup"><span data-stu-id="e7599-148">You should see something like this image.</span></span>

![][3]

<span data-ttu-id="e7599-149">Grattis, du har bara installationsprogrammet en LYKTA stack på Azure VM!</span><span class="sxs-lookup"><span data-stu-id="e7599-149">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7599-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e7599-150">Next steps</span></span>
<span data-ttu-id="e7599-151">Checka ut hello Ubuntu dokumentation om hello LYKTA stack:</span><span class="sxs-lookup"><span data-stu-id="e7599-151">Check out hello Ubuntu documentation on hello LAMP stack:</span></span>

* [<span data-ttu-id="e7599-152">https://help.Ubuntu.com/community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="e7599-152">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png