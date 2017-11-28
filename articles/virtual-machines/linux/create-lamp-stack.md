---
title: "aaaDeploy LAMPA på en virtuell Linux-dator i Azure | Microsoft Docs"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: 42d887bb9f78becc02505e336be25fdaaf78df70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-lamp-stack-on-azure"></a><span data-ttu-id="3ba67-103">Distribuera LYKTA stacken på Azure</span><span class="sxs-lookup"><span data-stu-id="3ba67-103">Deploy LAMP stack on Azure</span></span>
<span data-ttu-id="3ba67-104">Den här artikeln får du veta hur toodeploy en Apache web server, MySQL och PHP (hello LYKTA stack) på Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba67-104">This article walks you through how toodeploy an Apache web server, MySQL, and PHP (hello LAMP stack) on Azure.</span></span> <span data-ttu-id="3ba67-105">Du behöver ett Azure-konto ([skaffa en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)) och hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="3ba67-105">You need an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span> <span data-ttu-id="3ba67-106">Du kan också utföra dessa steg med hello [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3ba67-106">You can also perform these steps with hello [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="3ba67-107">Snabbkommando sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3ba67-107">Quick command summary</span></span>

1. <span data-ttu-id="3ba67-108">Spara och redigera hello [azuredeploy.parameters.json filen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour inställningar på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="3ba67-108">Save and edit hello [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour preference on your local machine.</span></span>
2. <span data-ttu-id="3ba67-109">Kör följande två kommandon toocreate hello en resursgrupp och distribuera mallen:</span><span class="sxs-lookup"><span data-stu-id="3ba67-109">Run hello following two commands toocreate a resource group and then deploy your template:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a><span data-ttu-id="3ba67-110">Distribuera LYKTA på befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3ba67-110">Deploy LAMP on existing VM</span></span>
<span data-ttu-id="3ba67-111">hello följande kommandon uppdateringar paket och sedan installerar Apache, MySQL och PHP:</span><span class="sxs-lookup"><span data-stu-id="3ba67-111">hello following commands updates packages, then installs Apache, MySQL, and PHP:</span></span>

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="3ba67-112">Distribuera LYKTA på den nya VM genomgång</span><span class="sxs-lookup"><span data-stu-id="3ba67-112">Deploy LAMP on new VM walkthrough</span></span>

1. <span data-ttu-id="3ba67-113">Skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create) toocontain hello ny virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="3ba67-113">Create a resource group with [az group create](/cli/azure/group#create) toocontain hello new VM:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
```
<span data-ttu-id="3ba67-114">toocreate hello Virtuella datorn kan du använda ett redan skrivits Azure Resource Manager mallen [här på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="3ba67-114">toocreate hello VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

2. <span data-ttu-id="3ba67-115">Spara hello [azuredeploy.parameters.json filen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="3ba67-115">Save hello [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour local machine.</span></span>
3. <span data-ttu-id="3ba67-116">Redigera hello **azuredeploy.parameters.json** filen tooyour önskade indata.</span><span class="sxs-lookup"><span data-stu-id="3ba67-116">Edit hello **azuredeploy.parameters.json** file tooyour preferred inputs.</span></span>
4. <span data-ttu-id="3ba67-117">Distribuera hello mallen med [az distribution skapa] refererar till hello hämtas json-fil:</span><span class="sxs-lookup"><span data-stu-id="3ba67-117">Deploy hello template with [az group deployment create] referencing hello downloaded json file:</span></span>

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

<span data-ttu-id="3ba67-118">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3ba67-118">hello output is similar toohello following example:</span></span>

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

<span data-ttu-id="3ba67-119">Nu har du skapat en Linux VM med LYKTA redan installerad på den.</span><span class="sxs-lookup"><span data-stu-id="3ba67-119">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="3ba67-120">Om du vill kan du kontrollera hello installationen genom att hoppa över ned för[Kontrollera LYKTA installerats](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="3ba67-120">If you wish, you can verify hello install by jumping down too[Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="3ba67-121">Distribuera LYKTA på befintliga Virtuella genomgång</span><span class="sxs-lookup"><span data-stu-id="3ba67-121">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="3ba67-122">Om du behöver hjälp med att skapa en Linux VM, kan du gå [här toolearn hur toocreate en Linux VM](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span><span class="sxs-lookup"><span data-stu-id="3ba67-122">If you need help creating a Linux VM, you can head [here toolearn how toocreate a Linux VM](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span></span> <span data-ttu-id="3ba67-123">Sedan måste tooSSH i hello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="3ba67-123">Next, you need tooSSH into hello Linux VM.</span></span> <span data-ttu-id="3ba67-124">Om du behöver hjälp med att skapa en SSH-nyckel kan du gå [här toolearn hur toocreate en SSH-nyckel på Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3ba67-124">If you need help with creating an SSH key, you can head [here toolearn how toocreate an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="3ba67-125">Om du redan har en SSH-nyckel kan gå vidare och SSH från kommandoraden till din Linux VM med `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="3ba67-125">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="3ba67-126">Nu när du arbetar inom din Linux VM går vi igenom installerar hello LYKTA stack på Debian-baserade distributioner.</span><span class="sxs-lookup"><span data-stu-id="3ba67-126">Now that you are working within your Linux VM, we can walk through installing hello LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="3ba67-127">hello kommandona kan skilja sig åt andra Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="3ba67-127">hello exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="3ba67-128">Installera på Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="3ba67-128">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="3ba67-129">Du behöver hello följande paket som har installerats: `apache2`, `mysql-server`, `php5`, och `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="3ba67-129">You need hello following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="3ba67-130">Du kan installera dessa paket genom att direkt ta tag paketen eller använda Tasksel.</span><span class="sxs-lookup"><span data-stu-id="3ba67-130">You can install these packages by directly grabbing these packages or using Tasksel.</span></span>
<span data-ttu-id="3ba67-131">Innan du installerar du behöver toodownload och uppdatera paketet listor.</span><span class="sxs-lookup"><span data-stu-id="3ba67-131">Before installing you need toodownload and update package lists.</span></span>

```bash
sudo apt-get update
```

##### <a name="individual-packages"></a><span data-ttu-id="3ba67-132">Enskilda paket</span><span class="sxs-lookup"><span data-stu-id="3ba67-132">Individual packages</span></span>
<span data-ttu-id="3ba67-133">Använda lgh get:</span><span class="sxs-lookup"><span data-stu-id="3ba67-133">Using apt-get:</span></span>

```bash
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

##### <a name="using-tasksel"></a><span data-ttu-id="3ba67-134">Med hjälp av tasksel</span><span class="sxs-lookup"><span data-stu-id="3ba67-134">Using tasksel</span></span>
<span data-ttu-id="3ba67-135">Alternativt kan du hämta Tasksel, ett Debian/Ubuntu-verktyg som du installerar flera relaterade paket som en samordnad ”task” på datorn.</span><span class="sxs-lookup"><span data-stu-id="3ba67-135">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

```bash
sudo apt-get install tasksel
sudo tasksel install lamp-server
```

<span data-ttu-id="3ba67-136">När du kör något av föregående alternativ för hello, kommer du att ange tooinstall paketen och andra beroenden.</span><span class="sxs-lookup"><span data-stu-id="3ba67-136">After running either of hello previous options, you will be prompted tooinstall these packages and various other dependencies.</span></span> <span data-ttu-id="3ba67-137">tooset ett administratörslösenord för MySQL, tryck på 'y' och 'Enter' toocontinue och följer några andra frågor.</span><span class="sxs-lookup"><span data-stu-id="3ba67-137">tooset an administrative password for MySQL, press 'y' and then 'Enter' toocontinue, and follow any other prompts.</span></span> <span data-ttu-id="3ba67-138">Den här processen installerar hello minsta nödvändiga PHP tillägg som behövs toouse PHP med MySQL.</span><span class="sxs-lookup"><span data-stu-id="3ba67-138">This process installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="3ba67-139">Kör följande kommando toosee hello andra PHP-tillägg som är tillgängliga som paket:</span><span class="sxs-lookup"><span data-stu-id="3ba67-139">Run hello following command toosee other PHP extensions that are available as packages:</span></span>

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a><span data-ttu-id="3ba67-140">Skapa info.php dokument</span><span class="sxs-lookup"><span data-stu-id="3ba67-140">Create info.php document</span></span>
<span data-ttu-id="3ba67-141">Nu bör du kunna toocheck vilken version av Apache, MySQL och PHP du ha via hello kommandoraden genom att skriva `apache2 -v`, `mysql -v`, eller `php -v`.</span><span class="sxs-lookup"><span data-stu-id="3ba67-141">You should now be able toocheck what version of Apache, MySQL, and PHP you have through hello command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="3ba67-142">Om du skulle t.ex tootest ytterligare, kan du skapa en snabb PHP info sidan tooview i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="3ba67-142">If you would like tootest further, you can create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="3ba67-143">Skapa en fil med Nano textredigerare med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="3ba67-143">Create a file with Nano text editor with this command:</span></span>

```bash
sudo nano /var/www/html/info.php
```

<span data-ttu-id="3ba67-144">Lägg till hello följande rader i hello GNU Nano textredigerare:</span><span class="sxs-lookup"><span data-stu-id="3ba67-144">Within hello GNU Nano text editor, add hello following lines:</span></span>

```php
<?php
phpinfo();
?>
```

<span data-ttu-id="3ba67-145">Sedan spara och avsluta hello textredigerare.</span><span class="sxs-lookup"><span data-stu-id="3ba67-145">Then save and exit hello text editor.</span></span>

<span data-ttu-id="3ba67-146">Starta om Apache med det här kommandot så att alla nya installerar börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="3ba67-146">Restart Apache with this command so all new installs take effect.</span></span>

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="3ba67-147">Kontrollera LAMPAN har installerats</span><span class="sxs-lookup"><span data-stu-id="3ba67-147">Verify LAMP successfully installed</span></span>
<span data-ttu-id="3ba67-148">Nu kan du kontrollera hello PHP Infosidan som du skapade genom att öppna en webbläsare och gå toohttp://youruniqueDNS/info.php.</span><span class="sxs-lookup"><span data-stu-id="3ba67-148">Now you can check hello PHP info page you created by opening a browser and going toohttp://youruniqueDNS/info.php.</span></span> <span data-ttu-id="3ba67-149">Det bör se liknande toothis bild.</span><span class="sxs-lookup"><span data-stu-id="3ba67-149">It should look similar toothis image.</span></span>

![][2]

<span data-ttu-id="3ba67-150">Du kan kontrollera installationen av Apache genom att visa hello Apache2 Ubuntu standardsida genom att gå tooyou http://youruniqueDNS/.</span><span class="sxs-lookup"><span data-stu-id="3ba67-150">You can check your Apache installation by viewing hello Apache2 Ubuntu Default Page by going tooyou http://youruniqueDNS/.</span></span> <span data-ttu-id="3ba67-151">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3ba67-151">hello output is similar toohello following example:</span></span>

![][3]

<span data-ttu-id="3ba67-152">Grattis, du har bara installationsprogrammet en LYKTA stack på Azure VM!</span><span class="sxs-lookup"><span data-stu-id="3ba67-152">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ba67-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3ba67-153">Next steps</span></span>
<span data-ttu-id="3ba67-154">Checka ut hello Ubuntu dokumentation om hello LYKTA stack:</span><span class="sxs-lookup"><span data-stu-id="3ba67-154">Check out hello Ubuntu documentation on hello LAMP stack:</span></span>

* [<span data-ttu-id="3ba67-155">https://help.Ubuntu.com/community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="3ba67-155">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
