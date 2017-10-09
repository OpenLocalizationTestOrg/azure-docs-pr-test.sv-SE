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
# <a name="deploy-a-lamp-app-using-hello-azure-customscript-extension-for-linux"></a><span data-ttu-id="79e5c-103">Distribuera en LYKTA-app med hello Azure CustomScript-tillägg för Linux</span><span class="sxs-lookup"><span data-stu-id="79e5c-103">Deploy a LAMP app using hello Azure CustomScript Extension for Linux</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="79e5c-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="79e5c-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="79e5c-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="79e5c-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="79e5c-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="79e5c-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="79e5c-107">Information om hur du distribuerar en LYKTA-stacken använder hello Resource Manager-modellen finns [här](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="79e5c-107">For information about deploying a LAMP stack using hello Resource Manager model, see [here](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="79e5c-108">hello Microsoft Azure CustomScript-tillägg för Linux innehåller en sätt toocustomize dina virtuella datorer (VM) genom att köra skadlig kod som skrivs i valfritt skriptspråk som stöds av hello VM (till exempel Python och Bash).</span><span class="sxs-lookup"><span data-stu-id="79e5c-108">hello Microsoft Azure CustomScript Extension for Linux provides a way toocustomize your virtual machines (VMs) by running arbitrary code written in any scripting language supported by hello VM (for example, Python, and Bash).</span></span> <span data-ttu-id="79e5c-109">Detta ger en mycket flexibelt sätt tooautomate programmet distribution toomultiple datorer.</span><span class="sxs-lookup"><span data-stu-id="79e5c-109">This provides a very flexible way tooautomate application deployment toomultiple machines.</span></span>

<span data-ttu-id="79e5c-110">Du kan distribuera hello CustomScript-tillägg med hello Azure-portalen, Windows PowerShell eller hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="79e5c-110">You can deploy hello CustomScript Extension using hello Azure portal, Windows PowerShell, or hello Azure Command-Line Interface (Azure CLI).</span></span>

<span data-ttu-id="79e5c-111">I den här artikeln använder vi skapade hello Azure CLI toodeploy en enkel LYKTA programmet tooan Ubuntu VM med hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="79e5c-111">In this article we'll use hello Azure CLI toodeploy a simple LAMP application tooan Ubuntu VM created using hello classic deployment model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79e5c-112">Krav</span><span class="sxs-lookup"><span data-stu-id="79e5c-112">Prerequisites</span></span>
<span data-ttu-id="79e5c-113">För det här exemplet skapar du två virtuella Azure-datorer kör Ubuntu 14.04 eller senare.</span><span class="sxs-lookup"><span data-stu-id="79e5c-113">For this example, first create two Azure VMs running Ubuntu 14.04 or later.</span></span> <span data-ttu-id="79e5c-114">hello VMs kallas *skript-vm* och *lykta vm*.</span><span class="sxs-lookup"><span data-stu-id="79e5c-114">hello VMs are called *script-vm* and *lamp-vm*.</span></span> <span data-ttu-id="79e5c-115">Använd ett unikt namn när du skapar hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="79e5c-115">Use unique names when you create hello VMs.</span></span> <span data-ttu-id="79e5c-116">En används toorun hello CLI-kommandona och ett är används toodeploy hello LYKTA app.</span><span class="sxs-lookup"><span data-stu-id="79e5c-116">One is used toorun hello CLI commands and one is used toodeploy hello LAMP app.</span></span>

<span data-ttu-id="79e5c-117">Du måste också ett Azure Storage-konto och en nyckel tooaccess it (du kan hämta det från hello Azure-portalen).</span><span class="sxs-lookup"><span data-stu-id="79e5c-117">You also need an Azure Storage account and a key tooaccess it (you can get this from hello Azure portal).</span></span>

<span data-ttu-id="79e5c-118">Om du behöver hjälp med att skapa virtuella Linux-datorer i Azure Se för[skapa en virtuell dator kör Linux](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="79e5c-118">If you need help creating Linux VMs on Azure refer too[Create a Virtual Machine Running Linux](createportal.md).</span></span>

<span data-ttu-id="79e5c-119">hello installera kommandon förutsätter Ubuntu men du kan anpassa hello installation för alla stöder Linux distro.</span><span class="sxs-lookup"><span data-stu-id="79e5c-119">hello install commands assume Ubuntu, but you can adapt hello installation for any supported Linux distro.</span></span>

<span data-ttu-id="79e5c-120">hello måste skript vm VM toohave Azure CLI installerats med en fungerande anslutning tooAzure.</span><span class="sxs-lookup"><span data-stu-id="79e5c-120">hello script-vm VM needs toohave Azure CLI installed, with a working connection tooAzure.</span></span> <span data-ttu-id="79e5c-121">Hjälp med det här finns för[installera och konfigurera hello Azure-kommandoradsgränssnittet](../../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="79e5c-121">For help with this refer too[Install and Configure hello Azure Command-Line Interface](../../../cli-install-nodejs.md).</span></span>

## <a name="upload-a-script"></a><span data-ttu-id="79e5c-122">Ladda upp ett skript</span><span class="sxs-lookup"><span data-stu-id="79e5c-122">Upload a script</span></span>
<span data-ttu-id="79e5c-123">Vi använder hello CustomScript-tillägg toorun ett skript på en fjärransluten VM tooinstall hello LYKTA stack och skapa en PHP-sida.</span><span class="sxs-lookup"><span data-stu-id="79e5c-123">We'll use hello CustomScript Extension toorun a script on a remote VM tooinstall hello LAMP stack and create a PHP page.</span></span> <span data-ttu-id="79e5c-124">I ordning tooaccess hello skript från valfri plats ska vi skicka den som en Azure blob.</span><span class="sxs-lookup"><span data-stu-id="79e5c-124">In order tooaccess hello script from anywhere we'll upload it as an Azure blob.</span></span>

### <a name="script-overview"></a><span data-ttu-id="79e5c-125">Översikt över skript</span><span class="sxs-lookup"><span data-stu-id="79e5c-125">Script overview</span></span>
<span data-ttu-id="79e5c-126">exempel på skript hello installerar en LYKTA stack tooUbuntu (inklusive hur du konfigurerar en tyst installation av MySQL), skriver en enkel PHP-filen och startar Apache.</span><span class="sxs-lookup"><span data-stu-id="79e5c-126">hello script example installs a LAMP stack tooUbuntu (including setting up a silent install of MySQL), writes a simple PHP file, and starts Apache.</span></span>

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

### <a name="upload-script"></a><span data-ttu-id="79e5c-127">Överför skript</span><span class="sxs-lookup"><span data-stu-id="79e5c-127">Upload script</span></span>
<span data-ttu-id="79e5c-128">Spara hello skript som en textfil, till exempel *install_lamp.sh*, och sedan ladda upp den tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="79e5c-128">Save hello script as a text file, for example *install_lamp.sh*, and then upload it tooAzure Storage.</span></span> <span data-ttu-id="79e5c-129">Du kan göra det enkelt med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="79e5c-129">You can do this easily with Azure CLI.</span></span> <span data-ttu-id="79e5c-130">hello filöverföringar följande exempel hello filen tooa storage-behållare med namnet ”skript”.</span><span class="sxs-lookup"><span data-stu-id="79e5c-130">hello following example uploads hello file tooa storage container named "scripts".</span></span> <span data-ttu-id="79e5c-131">Om hello-behållaren inte finns behöver du toocreate den första.</span><span class="sxs-lookup"><span data-stu-id="79e5c-131">If hello container doesn't exist you'll need toocreate it first.</span></span>

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

<span data-ttu-id="79e5c-132">Även skapa en JSON-fil som beskriver hur toodownload hello skriptet från Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="79e5c-132">Also create a JSON file that describes how toodownload hello script from Azure Storage.</span></span> <span data-ttu-id="79e5c-133">Spara den som *public_config.json* (i stället för ”mystorage” hello namnet på ditt lagringskonto):</span><span class="sxs-lookup"><span data-stu-id="79e5c-133">Save this as *public_config.json* (replacing "mystorage" with hello name of your storage account):</span></span>

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-hello-extension"></a><span data-ttu-id="79e5c-134">Distribuera hello-tillägg</span><span class="sxs-lookup"><span data-stu-id="79e5c-134">Deploy hello extension</span></span>
<span data-ttu-id="79e5c-135">Nu kan du använda hello nästa kommando toodeploy hello Linux CustomScript-tillägg toohello remote hello VM med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="79e5c-135">Now you can use hello next command toodeploy hello Linux CustomScript Extension toohello remote VM using hello Azure CLI.</span></span>

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

<span data-ttu-id="79e5c-136">hello föregående kommando hämtar och kör hello *install_lamp.sh* skriptet på hello VM kallas *lykta vm*.</span><span class="sxs-lookup"><span data-stu-id="79e5c-136">hello previous command downloads and runs hello *install_lamp.sh* script on hello VM called *lamp-vm*.</span></span>

<span data-ttu-id="79e5c-137">Eftersom hello appen innehåller en webbserver, kan du komma ihåg tooopen HTTP lyssningsport på hello remote virtuell dator med hello nästa kommando.</span><span class="sxs-lookup"><span data-stu-id="79e5c-137">Since hello app includes a web server, remember tooopen an HTTP listening port on hello remote VM with hello next command.</span></span>

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="79e5c-138">Övervakning och felsökning</span><span class="sxs-lookup"><span data-stu-id="79e5c-138">Monitoring and troubleshooting</span></span>
<span data-ttu-id="79e5c-139">Du kan kontrollera hur väl hello anpassat skript körs genom att titta på hello loggfilen hello remote VM.</span><span class="sxs-lookup"><span data-stu-id="79e5c-139">You can check on how well hello custom script runs by looking at hello log file on hello remote VM.</span></span> <span data-ttu-id="79e5c-140">SSH för*lykta vm* och pilslut hello loggfil med hello nästa kommando.</span><span class="sxs-lookup"><span data-stu-id="79e5c-140">SSH too*lamp-vm* and tail hello log file with hello next command.</span></span>

    cd /var/log/azure/customscript
    tail -f handler.log

<span data-ttu-id="79e5c-141">När du har kört hello CustomScript-tillägg kan du bläddra toohello PHP-sida som du skapade för information.</span><span class="sxs-lookup"><span data-stu-id="79e5c-141">After you run hello CustomScript Extension, you can browse toohello PHP page you created for information.</span></span> <span data-ttu-id="79e5c-142">hello PHP sidan till hello exempel i den här artikeln är *http://lamp-vm.cloudapp.net/phpinfo.php*.</span><span class="sxs-lookup"><span data-stu-id="79e5c-142">hello PHP page for hello example in this article is *http://lamp-vm.cloudapp.net/phpinfo.php*.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79e5c-143">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="79e5c-143">Additional resources</span></span>
<span data-ttu-id="79e5c-144">Du kan använda samma grundläggande steg toodeploy hello mer komplexa appar.</span><span class="sxs-lookup"><span data-stu-id="79e5c-144">You can use hello same basic steps toodeploy more complex apps.</span></span> <span data-ttu-id="79e5c-145">I det här exemplet sparades hello installationsskriptet som en offentlig blob i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="79e5c-145">In this example hello install script was saved as a public blob in Azure Storage.</span></span> <span data-ttu-id="79e5c-146">Ett säkrare alternativ skulle vara toostore hello installationsskriptet som en säker blob med en [säker Åtkomstsignatur](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).</span><span class="sxs-lookup"><span data-stu-id="79e5c-146">A more secure option would be toostore hello install script as a secure blob with a [Secure Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).</span></span>

<span data-ttu-id="79e5c-147">Ytterligare resurser för Azure CLI, Linux och hello CustomScript-tillägg visas bredvid.</span><span class="sxs-lookup"><span data-stu-id="79e5c-147">Additional resources for Azure CLI, Linux and hello CustomScript Extension are listed next.</span></span>

[<span data-ttu-id="79e5c-148">Automatisera anpassningsuppgifter i virtuella Linux-datorer med CustomScript-tillägg</span><span class="sxs-lookup"><span data-stu-id="79e5c-148">Automate Linux VM Customization Tasks Using CustomScript Extension</span></span>](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[<span data-ttu-id="79e5c-149">Azure Linux-tillägg (GitHub)</span><span class="sxs-lookup"><span data-stu-id="79e5c-149">Azure Linux Extensions (GitHub)</span></span>](https://github.com/Azure/azure-linux-extensions)