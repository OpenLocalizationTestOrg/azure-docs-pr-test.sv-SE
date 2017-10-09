---
title: aaaDeploying Windows Compute resurser med Azure Resource Manager-mallar | Microsoft Docs
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b026fe81-1bc1-4899-ac32-886091671498
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dee228a492b08053713829e156e5b5ba304d7588
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-architecture-with-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="5b29b-103">Programarkitektur med Azure Resource Manager-mallar för virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="5b29b-103">Application architecture with Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="5b29b-104">När du utvecklar en Azure Resource Manager-distribution måste behov toobe mappas tooAzure resurser och tjänster.</span><span class="sxs-lookup"><span data-stu-id="5b29b-104">When developing an Azure Resource Manager deployment, compute requirements need toobe mapped tooAzure resources and services.</span></span> <span data-ttu-id="5b29b-105">Om ett program består av flera http-slutpunkter, en databas och en data cachelagring service, hello Azure-resurser som är värdar för var och en av dessa komponenter måste toobe rationaliserad.</span><span class="sxs-lookup"><span data-stu-id="5b29b-105">If an application consists of several http endpoints, a database, and a data caching service, hello Azure resources that host each of these components needs toobe rationalized.</span></span> <span data-ttu-id="5b29b-106">Hello musik Store exempelprogrammet innehåller exempelvis ett webbprogram som är värd för en virtuell dator och en SQL-databas, som finns i Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5b29b-106">For instance, hello sample Music Store application includes a web application that is hosted on a virtual machine, and a SQL database, which is hosted in Azure SQL database.</span></span> 

<span data-ttu-id="5b29b-107">Det här dokumentet beskriver hur hello musik Store beräkningsresurser konfigureras i hello Azure Resource Manager exempelmall.</span><span class="sxs-lookup"><span data-stu-id="5b29b-107">This document details how hello Music Store compute resources are configured in hello sample Azure Resource Manager template.</span></span> <span data-ttu-id="5b29b-108">Alla beroenden och unika konfigurationer är markerade.</span><span class="sxs-lookup"><span data-stu-id="5b29b-108">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="5b29b-109">Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat.</span><span class="sxs-lookup"><span data-stu-id="5b29b-109">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="5b29b-110">hello fullständig mallen hittar du här – [musik Store distributionen på Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="5b29b-110">hello complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="virtual-machine"></a><span data-ttu-id="5b29b-111">Virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5b29b-111">Virtual Machine</span></span>
<span data-ttu-id="5b29b-112">hello musik Store-programmet innehåller ett webbprogram där kunder kan bläddra bland och köpa musik.</span><span class="sxs-lookup"><span data-stu-id="5b29b-112">hello Music Store application includes a web application where customers can browse and purchase music.</span></span> <span data-ttu-id="5b29b-113">Det finns flera Azure-tjänster som kan vara värd för webbprogram i det här exemplet används en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5b29b-113">While there are several Azure services that can host web applications, for this example, a Virtual Machine is used.</span></span> <span data-ttu-id="5b29b-114">Använder hello exempelmall musik Store, en virtuell dator distribueras, installera en webbserver och hello musik Store webbplats installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="5b29b-114">Using hello sample Music Store template, a virtual machine is deployed, a web server install, and hello Music Store website installed and configured.</span></span> <span data-ttu-id="5b29b-115">För hello ut i den här artikeln beskrivs bara distributionen av hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5b29b-115">For hello sake of this article, only hello virtual machine deployment is detailed.</span></span> <span data-ttu-id="5b29b-116">hello konfigurationen av hello webbservern och hello program beskrivs i en senare artikel.</span><span class="sxs-lookup"><span data-stu-id="5b29b-116">hello configuration of hello web server and hello application is detailed in a later article.</span></span>

<span data-ttu-id="5b29b-117">En virtuell dator kan läggas till tooa mallen med hjälp av hello Visual Studio Lägg till ny resurs guiden, eller genom att infoga giltig JSON i hello Distributionsmall.</span><span class="sxs-lookup"><span data-stu-id="5b29b-117">A virtual machine can be added tooa template using hello Visual Studio Add New Resource wizard, or by inserting valid JSON into hello deployment template.</span></span> <span data-ttu-id="5b29b-118">När du distribuerar en virtuell dator krävs också flera relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="5b29b-118">When deploying a virtual machine, several related resources are also needed.</span></span> <span data-ttu-id="5b29b-119">Om du använder Visual Studio toocreate hello mall skapas dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="5b29b-119">If using Visual Studio toocreate hello template, these resources are created for you.</span></span> <span data-ttu-id="5b29b-120">Om manuellt konstruera hello mall, måste dessa resurser toobe infogas och konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="5b29b-120">If manually constructing hello template, these resources need toobe inserted and configured.</span></span>

<span data-ttu-id="5b29b-121">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).</span><span class="sxs-lookup"><span data-stu-id="5b29b-121">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).</span></span>

```json
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

<span data-ttu-id="5b29b-122">När har distribuerats, kan hello egenskaper för virtuell dator ses i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5b29b-122">Once deployed, hello virtual machine properties can be seen in hello Azure portal.</span></span>

![Virtuell dator](./media/dotnet-core-2-architecture/vm-win.png)

## <a name="storage-account"></a><span data-ttu-id="5b29b-124">Lagringskonto</span><span class="sxs-lookup"><span data-stu-id="5b29b-124">Storage Account</span></span>
<span data-ttu-id="5b29b-125">Storage-konton har många lagringsalternativ och funktioner.</span><span class="sxs-lookup"><span data-stu-id="5b29b-125">Storage accounts have many storage options and capabilities.</span></span> <span data-ttu-id="5b29b-126">Hello-kontexten av Azure virtuella datorer innehåller ett lagringskonto hello virtuella hårddiskar för hello virtuell dator och eventuella ytterligare hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="5b29b-126">For hello context of Azure Virtual machines, a storage account holds hello virtual hard drives of hello virtual machine and any additional data disks.</span></span> <span data-ttu-id="5b29b-127">hello musik Store exempel innehåller en lagring konto toohold hello virtuella hårddisken på varje virtuell dator i hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="5b29b-127">hello Music Store sample includes one storage account toohold hello virtual hard drive of each virtual machine in hello deployment.</span></span> 

<span data-ttu-id="5b29b-128">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Lagringskonto](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).</span><span class="sxs-lookup"><span data-stu-id="5b29b-128">Follow this link toosee hello JSON sample within hello Resource Manager template – [Storage Account](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

<span data-ttu-id="5b29b-129">Ett lagringskonto är associerat med en virtuell dator i hello Resource Manager-mall deklarationen av hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5b29b-129">A storage account is associate with a virtual machine inside hello Resource Manager template declaration of hello virtual machine.</span></span> 

<span data-ttu-id="5b29b-130">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella datorn och Storage-konto associationen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).</span><span class="sxs-lookup"><span data-stu-id="5b29b-130">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine and Storage Account association](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).</span></span>

```json
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

<span data-ttu-id="5b29b-131">Efter distributionen kan hello storage-konto visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5b29b-131">After deployment, hello storage account can be viewed in hello Azure portal.</span></span>

![Lagringskonto](./media/dotnet-core-2-architecture/storacct-win.png)

<span data-ttu-id="5b29b-133">Klicka i hello lagringsblobbehållare för kontot visas hello virtuell hårddiskfil för varje virtuell dator distribueras med hello mallen.</span><span class="sxs-lookup"><span data-stu-id="5b29b-133">Clicking into hello storage account blob container, hello virtual hard drive file for each virtual machine deployed with hello template can be seen.</span></span>

![Virtuella hårddiskar](./media/dotnet-core-2-architecture/vhd-win.png)

<span data-ttu-id="5b29b-135">Mer information om Azure Storage finns [Azure Storage-dokumentationen](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="5b29b-135">For more information on Azure Storage, see [Azure Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>

## <a name="virtual-network"></a><span data-ttu-id="5b29b-136">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="5b29b-136">Virtual Network</span></span>
<span data-ttu-id="5b29b-137">Om en virtuell dator kräver interna nätverk, till exempel hello möjlighet toocommunicate med andra virtuella datorer och Azure-resurser, krävs ett Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="5b29b-137">If a virtual machine requires internal networking such as hello ability toocommunicate with other virtual machines and Azure resources, an Azure Virtual Network is required.</span></span>  <span data-ttu-id="5b29b-138">Ett virtuellt nätverk gör inte hello virtuella datorn nås över hello internet.</span><span class="sxs-lookup"><span data-stu-id="5b29b-138">A virtual network does not make hello virtual machine accessible over hello internet.</span></span> <span data-ttu-id="5b29b-139">Anslutning för offentliga kräver en offentlig IP-adress som beskrivs senare i den här serien.</span><span class="sxs-lookup"><span data-stu-id="5b29b-139">Public connectivity requires a public IP address, which is detailed later in this series.</span></span>

<span data-ttu-id="5b29b-140">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella nätverk och undernät](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).</span><span class="sxs-lookup"><span data-stu-id="5b29b-140">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Network and Subnets](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

<span data-ttu-id="5b29b-141">Från hello Azure-portalen, hello virtuellt nätverk ser ut som hello följande bild.</span><span class="sxs-lookup"><span data-stu-id="5b29b-141">From hello Azure portal, hello virtual network looks like hello following image.</span></span> <span data-ttu-id="5b29b-142">Observera att alla virtuella datorer som distribueras med hello mallen är anslutna toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="5b29b-142">Notice that all virtual machines deployed with hello template are attached toohello virtual network.</span></span>

![Virtual Network](./media/dotnet-core-2-architecture/vnet-win.png)

## <a name="network-interface"></a><span data-ttu-id="5b29b-144">Nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="5b29b-144">Network Interface</span></span>
 <span data-ttu-id="5b29b-145">Ett nätverksgränssnitt ansluter en virtuell dator tooa virtuellt nätverk, mer specifikt tooa undernät som har definierats i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="5b29b-145">A network interface connects a virtual machine tooa virtual network, more specifically tooa subnet that has been defined in hello virtual network.</span></span> 

 <span data-ttu-id="5b29b-146">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [nätverksgränssnittet](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).</span><span class="sxs-lookup"><span data-stu-id="5b29b-146">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Interface](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

<span data-ttu-id="5b29b-147">Varje virtuell datorresurs innehåller en nätverksprofil.</span><span class="sxs-lookup"><span data-stu-id="5b29b-147">Each virtual machine resource includes a network profile.</span></span> <span data-ttu-id="5b29b-148">hello nätverksgränssnittet är kopplat till hello virtuell dator i den här profilen.</span><span class="sxs-lookup"><span data-stu-id="5b29b-148">hello network interface is associated with hello virtual machine in this profile.</span></span>  

<span data-ttu-id="5b29b-149">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella nätverksprofil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).</span><span class="sxs-lookup"><span data-stu-id="5b29b-149">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine Network Profile](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).</span></span>

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

<span data-ttu-id="5b29b-150">Från hello Azure-portalen, hello nätverksgränssnittet ser ut som hello följande bild.</span><span class="sxs-lookup"><span data-stu-id="5b29b-150">From hello Azure portal, hello network interface looks like hello following image.</span></span> <span data-ttu-id="5b29b-151">hello interna IP-adress och hello virtuella koppling kan ses på hello nätverksresurs gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="5b29b-151">hello internal IP address and hello virtual machine association can be seen on hello network interface resource.</span></span>

![Nätverksgränssnitt](./media/dotnet-core-2-architecture/nic-win.png)

<span data-ttu-id="5b29b-153">Mer information om virtuella Azure-nätverk finns [dokumentation för Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="5b29b-153">For more information on Azure Virtual Networks, see [Azure Virtual Network documentation](https://azure.microsoft.com/documentation/services/virtual-network/).</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="5b29b-154">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5b29b-154">Azure SQL Database</span></span>
<span data-ttu-id="5b29b-155">Dessutom är tooa virtuella datorn är värd för hello musik Store webbplats, Azure SQL-databas distribuerade toohost hello musik Store-databas.</span><span class="sxs-lookup"><span data-stu-id="5b29b-155">In addition tooa virtual machine hosting hello Music Store website, an Azure SQL Database is deployed toohost hello Music Store database.</span></span> <span data-ttu-id="5b29b-156">hello fördelen med att använda Azure SQL Database här är att en annan uppsättning virtuella datorer är inte obligatoriska och tillgänglighet och skala är inbyggd i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5b29b-156">hello advantage of using Azure SQL Database here is that a second set of virtual machines is not required, and scale and availability is built into hello service.</span></span>

<span data-ttu-id="5b29b-157">En Azure SQL database kan läggas till med hello Visual Studio Lägg till ny resurs guiden, eller genom att infoga giltig JSON i en mall.</span><span class="sxs-lookup"><span data-stu-id="5b29b-157">An Azure SQL database can be added using hello Visual Studio Add New Resource wizard, or by inserting valid JSON into a template.</span></span> <span data-ttu-id="5b29b-158">hello SQL Server-resurs innehåller ett användarnamn och lösenord som har beviljats administratörsbehörighet på hello SQL-instansen.</span><span class="sxs-lookup"><span data-stu-id="5b29b-158">hello SQL Server resource includes a user name and password that is granted administrative rights on hello SQL instance.</span></span> <span data-ttu-id="5b29b-159">Dessutom läggs en resurs för SQL-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="5b29b-159">Also, a SQL firewall resource is added.</span></span> <span data-ttu-id="5b29b-160">Som standard är program som finns i Azure kan tooconnect med hello SQL-instans.</span><span class="sxs-lookup"><span data-stu-id="5b29b-160">By default, applications hosted in Azure are able tooconnect with hello SQL instance.</span></span> <span data-ttu-id="5b29b-161">tooallow externt program sådana en SQL Server Management studio tooconnect toohello SQL-instans, hello brandvägg måste toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="5b29b-161">tooallow external application such a SQL Server Management studio tooconnect toohello SQL instance, hello firewall needs toobe configured.</span></span> <span data-ttu-id="5b29b-162">Hello standardkonfigurationen är bra för hello musik Store demo hello ut.</span><span class="sxs-lookup"><span data-stu-id="5b29b-162">For hello sake of hello Music Store demo, hello default configuration is fine.</span></span> 

<span data-ttu-id="5b29b-163">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).</span><span class="sxs-lookup"><span data-stu-id="5b29b-163">Follow this link toosee hello JSON sample within hello Resource Manager template – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).</span></span>

```json
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

<span data-ttu-id="5b29b-164">En vy över hello SQLServer- och MusicStore som visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5b29b-164">A view of hello SQL server and MusicStore database as seen in hello Azure portal.</span></span>

![SQL Server](./media/dotnet-core-2-architecture/sql-win.png)

<span data-ttu-id="5b29b-166">Mer information om hur du distribuerar Azure SQL Database finns [dokumentation för Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="5b29b-166">For more information on deploying Azure SQL Database, see [Azure SQL Database documentation](https://azure.microsoft.com/documentation/services/sql-database/).</span></span>

## <a name="next-step"></a><span data-ttu-id="5b29b-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b29b-167">Next step</span></span>
<hr>

[<span data-ttu-id="5b29b-168">Steg 2 – åtkomst och säkerhet i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="5b29b-168">Step 2 - Access and Security in Azure Resource Manager Templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

