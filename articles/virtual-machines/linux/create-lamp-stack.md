---
title: "Distribuera LYKTA på en virtuell Linux-dator i Azure | Microsoft Docs"
description: "Lär dig hur du installerar LYKTA stacken på en Linux-VM i Azure"
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
ms.openlocfilehash: ad69876bfbeba5f948a81e5c48c659fdf2265ae2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-lamp-stack-on-azure"></a><span data-ttu-id="940b7-103">Distribuera LYKTA stacken på Azure</span><span class="sxs-lookup"><span data-stu-id="940b7-103">Deploy LAMP stack on Azure</span></span>
<span data-ttu-id="940b7-104">Den här artikeln får du veta hur du distribuerar en Apache webbservern, MySQL och PHP (LYKTA stack) på Azure.</span><span class="sxs-lookup"><span data-stu-id="940b7-104">This article walks you through how to deploy an Apache web server, MySQL, and PHP (the LAMP stack) on Azure.</span></span> <span data-ttu-id="940b7-105">Du behöver ett Azure-konto ([skaffa en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)) och [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="940b7-105">You need an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and the [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span> <span data-ttu-id="940b7-106">Du kan också utföra dessa steg med [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="940b7-106">You can also perform these steps with the [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="940b7-107">Snabbkommando sammanfattning</span><span class="sxs-lookup"><span data-stu-id="940b7-107">Quick command summary</span></span>

1. <span data-ttu-id="940b7-108">Spara och redigera den [azuredeploy.parameters.json filen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) till dina inställningar på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="940b7-108">Save and edit the [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) to your preference on your local machine.</span></span>
2. <span data-ttu-id="940b7-109">Kör följande två kommandon för att skapa en resursgrupp och distribuera mallen:</span><span class="sxs-lookup"><span data-stu-id="940b7-109">Run the following two commands to create a resource group and then deploy your template:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a><span data-ttu-id="940b7-110">Distribuera LYKTA på befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="940b7-110">Deploy LAMP on existing VM</span></span>
<span data-ttu-id="940b7-111">Följande kommandon uppdaterar paket och sedan installerar Apache, MySQL och PHP:</span><span class="sxs-lookup"><span data-stu-id="940b7-111">The following commands updates packages, then installs Apache, MySQL, and PHP:</span></span>

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="940b7-112">Distribuera LYKTA på den nya VM genomgång</span><span class="sxs-lookup"><span data-stu-id="940b7-112">Deploy LAMP on new VM walkthrough</span></span>

1. <span data-ttu-id="940b7-113">Skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create) som innehåller den nya virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="940b7-113">Create a resource group with [az group create](/cli/azure/group#create) to contain the new VM:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
```
<span data-ttu-id="940b7-114">Om du vill skapa Virtuellt datorn, du kan använda ett redan skrivits Azure Resource Manager mallen [här på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="940b7-114">To create the VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

2. <span data-ttu-id="940b7-115">Spara den [azuredeploy.parameters.json filen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) till din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="940b7-115">Save the [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) to your local machine.</span></span>
3. <span data-ttu-id="940b7-116">Redigera den **azuredeploy.parameters.json** filen till alla prioriterade indata.</span><span class="sxs-lookup"><span data-stu-id="940b7-116">Edit the **azuredeploy.parameters.json** file to your preferred inputs.</span></span>
4. <span data-ttu-id="940b7-117">Distribuera mallen med [az distribution skapa] refererar till hämtade json-filen:</span><span class="sxs-lookup"><span data-stu-id="940b7-117">Deploy the template with [az group deployment create] referencing the downloaded json file:</span></span>

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

<span data-ttu-id="940b7-118">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="940b7-118">The output is similar to the following example:</span></span>

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

<span data-ttu-id="940b7-119">Nu har du skapat en Linux VM med LYKTA redan installerad på den.</span><span class="sxs-lookup"><span data-stu-id="940b7-119">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="940b7-120">Om du vill kan du kontrollera installationen genom att hoppa över ned till [Kontrollera LYKTA installerats](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="940b7-120">If you wish, you can verify the install by jumping down to [Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="940b7-121">Distribuera LYKTA på befintliga Virtuella genomgång</span><span class="sxs-lookup"><span data-stu-id="940b7-121">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="940b7-122">Om du behöver hjälp med att skapa en Linux VM, kan du gå [här om du vill veta hur du skapar en Linux VM](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span><span class="sxs-lookup"><span data-stu-id="940b7-122">If you need help creating a Linux VM, you can head [here to learn how to create a Linux VM](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span></span> <span data-ttu-id="940b7-123">Därefter måste du SSH i Linux VM.</span><span class="sxs-lookup"><span data-stu-id="940b7-123">Next, you need to SSH into the Linux VM.</span></span> <span data-ttu-id="940b7-124">Om du behöver hjälp med att skapa en SSH-nyckel kan du gå [här om du vill lära dig hur du skapar en SSH-nyckel på Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="940b7-124">If you need help with creating an SSH key, you can head [here to learn how to create an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="940b7-125">Om du redan har en SSH-nyckel kan gå vidare och SSH från kommandoraden till din Linux VM med `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="940b7-125">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="940b7-126">Nu när du arbetar inom din Linux VM går vi igenom installerar LYKTA stacken på Debian-baserade distributioner.</span><span class="sxs-lookup"><span data-stu-id="940b7-126">Now that you are working within your Linux VM, we can walk through installing the LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="940b7-127">De exakta kommandona kan skilja sig åt andra Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="940b7-127">The exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="940b7-128">Installera på Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="940b7-128">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="940b7-129">Du behöver följande paket installeras: `apache2`, `mysql-server`, `php5`, och `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="940b7-129">You need the following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="940b7-130">Du kan installera dessa paket genom att direkt ta tag paketen eller använda Tasksel.</span><span class="sxs-lookup"><span data-stu-id="940b7-130">You can install these packages by directly grabbing these packages or using Tasksel.</span></span>
<span data-ttu-id="940b7-131">Innan du installerar måste du hämta och uppdatera paketet listor.</span><span class="sxs-lookup"><span data-stu-id="940b7-131">Before installing you need to download and update package lists.</span></span>

```bash
sudo apt-get update
```

##### <a name="individual-packages"></a><span data-ttu-id="940b7-132">Enskilda paket</span><span class="sxs-lookup"><span data-stu-id="940b7-132">Individual packages</span></span>
<span data-ttu-id="940b7-133">Använda lgh get:</span><span class="sxs-lookup"><span data-stu-id="940b7-133">Using apt-get:</span></span>

```bash
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

##### <a name="using-tasksel"></a><span data-ttu-id="940b7-134">Med hjälp av tasksel</span><span class="sxs-lookup"><span data-stu-id="940b7-134">Using tasksel</span></span>
<span data-ttu-id="940b7-135">Alternativt kan du hämta Tasksel, ett Debian/Ubuntu-verktyg som du installerar flera relaterade paket som en samordnad ”task” på datorn.</span><span class="sxs-lookup"><span data-stu-id="940b7-135">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

```bash
sudo apt-get install tasksel
sudo tasksel install lamp-server
```

<span data-ttu-id="940b7-136">När du kör något av föregående alternativ, uppmanas du att installera dessa paket och andra beroenden.</span><span class="sxs-lookup"><span data-stu-id="940b7-136">After running either of the previous options, you will be prompted to install these packages and various other dependencies.</span></span> <span data-ttu-id="940b7-137">Om du vill ange ett administratörslösenord för MySQL trycker du på ”y” och sedan på RETUR om du fortsätta och följa några andra frågor.</span><span class="sxs-lookup"><span data-stu-id="940b7-137">To set an administrative password for MySQL, press 'y' and then 'Enter' to continue, and follow any other prompts.</span></span> <span data-ttu-id="940b7-138">Den här processen installerar de minsta nödvändiga PHP-tillägg för att kunna använda PHP med MySQL.</span><span class="sxs-lookup"><span data-stu-id="940b7-138">This process installs the minimum required PHP extensions needed to use PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="940b7-139">Kör följande kommando för att se andra PHP-tillägg som är tillgängliga som paket:</span><span class="sxs-lookup"><span data-stu-id="940b7-139">Run the following command to see other PHP extensions that are available as packages:</span></span>

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a><span data-ttu-id="940b7-140">Skapa info.php dokument</span><span class="sxs-lookup"><span data-stu-id="940b7-140">Create info.php document</span></span>
<span data-ttu-id="940b7-141">Du ska nu kunna kontrollera vilken version av Apache, MySQL och PHP via kommandoraden genom att skriva `apache2 -v`, `mysql -v`, eller `php -v`.</span><span class="sxs-lookup"><span data-stu-id="940b7-141">You should now be able to check what version of Apache, MySQL, and PHP you have through the command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="940b7-142">Om du vill testa ytterligare kan du skapa en snabb PHP info sida för att visa i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="940b7-142">If you would like to test further, you can create a quick PHP info page to view in a browser.</span></span> <span data-ttu-id="940b7-143">Skapa en fil med Nano textredigerare med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="940b7-143">Create a file with Nano text editor with this command:</span></span>

```bash
sudo nano /var/www/html/info.php
```

<span data-ttu-id="940b7-144">Lägg till följande rader i textredigeraren GNU Nano:</span><span class="sxs-lookup"><span data-stu-id="940b7-144">Within the GNU Nano text editor, add the following lines:</span></span>

```php
<?php
phpinfo();
?>
```

<span data-ttu-id="940b7-145">Sedan spara och Avsluta textredigeraren.</span><span class="sxs-lookup"><span data-stu-id="940b7-145">Then save and exit the text editor.</span></span>

<span data-ttu-id="940b7-146">Starta om Apache med det här kommandot så att alla nya installerar börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="940b7-146">Restart Apache with this command so all new installs take effect.</span></span>

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="940b7-147">Kontrollera LAMPAN har installerats</span><span class="sxs-lookup"><span data-stu-id="940b7-147">Verify LAMP successfully installed</span></span>
<span data-ttu-id="940b7-148">Nu kan du kontrollera sidan PHP du skapade genom att öppna en webbläsare och gå till http://youruniqueDNS/info.php.</span><span class="sxs-lookup"><span data-stu-id="940b7-148">Now you can check the PHP info page you created by opening a browser and going to http://youruniqueDNS/info.php.</span></span> <span data-ttu-id="940b7-149">Det bör likna den här avbildningen.</span><span class="sxs-lookup"><span data-stu-id="940b7-149">It should look similar to this image.</span></span>

![][2]

<span data-ttu-id="940b7-150">Du kan kontrollera installationen av Apache genom att visa sidan standard Ubuntu Apache2 genom att gå till du http://youruniqueDNS/.</span><span class="sxs-lookup"><span data-stu-id="940b7-150">You can check your Apache installation by viewing the Apache2 Ubuntu Default Page by going to you http://youruniqueDNS/.</span></span> <span data-ttu-id="940b7-151">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="940b7-151">The output is similar to the following example:</span></span>

![][3]

<span data-ttu-id="940b7-152">Grattis, du har bara installationsprogrammet en LYKTA stack på Azure VM!</span><span class="sxs-lookup"><span data-stu-id="940b7-152">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="940b7-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="940b7-153">Next steps</span></span>
<span data-ttu-id="940b7-154">Kolla Ubuntu-dokumentationen i LYKTA stacken:</span><span class="sxs-lookup"><span data-stu-id="940b7-154">Check out the Ubuntu documentation on the LAMP stack:</span></span>

* [<span data-ttu-id="940b7-155">https://help.Ubuntu.com/community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="940b7-155">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
