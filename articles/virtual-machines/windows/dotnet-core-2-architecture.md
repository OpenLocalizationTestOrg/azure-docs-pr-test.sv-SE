---
title: "Distribuera Windows beräkningsresurser med Azure Resource Manager-mallar | Microsoft Docs"
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
ms.openlocfilehash: 8a8b888195e52ea9669922a6a00a873025f3c375
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="application-architecture-with-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="345ce-103">Programarkitektur med Azure Resource Manager-mallar för virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="345ce-103">Application architecture with Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="345ce-104">När du utvecklar en Azure Resource Manager-distribution för att beräkna krav måste mappas till Azure-resurser och tjänster.</span><span class="sxs-lookup"><span data-stu-id="345ce-104">When developing an Azure Resource Manager deployment, compute requirements need to be mapped to Azure resources and services.</span></span> <span data-ttu-id="345ce-105">Om ett program består av flera http-slutpunkter, en databas och en data cachelagring service, Azure-resurser som är värdar för varje för dessa komponenter måste vara rationaliserad.</span><span class="sxs-lookup"><span data-stu-id="345ce-105">If an application consists of several http endpoints, a database, and a data caching service, the Azure resources that host each of these components needs to be rationalized.</span></span> <span data-ttu-id="345ce-106">Musik Store exempelprogrammet innehåller exempelvis ett webbprogram som är värd för en virtuell dator och en SQL-databas, som finns i Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="345ce-106">For instance, the sample Music Store application includes a web application that is hosted on a virtual machine, and a SQL database, which is hosted in Azure SQL database.</span></span> 

<span data-ttu-id="345ce-107">Det här dokumentet beskriver hur beräkningsresurser musik Store har konfigurerats i Azure Resource Manager exempelmall.</span><span class="sxs-lookup"><span data-stu-id="345ce-107">This document details how the Music Store compute resources are configured in the sample Azure Resource Manager template.</span></span> <span data-ttu-id="345ce-108">Alla beroenden och unika konfigurationer är markerade.</span><span class="sxs-lookup"><span data-stu-id="345ce-108">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="345ce-109">Distribuera en instans av lösningen till din Azure-prenumeration och fungerar tillsammans med Azure Resource Manager-mall för bästa resultat.</span><span class="sxs-lookup"><span data-stu-id="345ce-109">For the best experience, pre-deploy an instance of the solution to your Azure subscription and work along with the Azure Resource Manager template.</span></span> <span data-ttu-id="345ce-110">Fullständig mallen hittar du här – [musik Store distributionen på Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="345ce-110">The complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="virtual-machine"></a><span data-ttu-id="345ce-111">Virtuell dator</span><span class="sxs-lookup"><span data-stu-id="345ce-111">Virtual Machine</span></span>
<span data-ttu-id="345ce-112">Musik Store-programmet innehåller ett webbprogram där kunder kan bläddra bland och köpa musik.</span><span class="sxs-lookup"><span data-stu-id="345ce-112">The Music Store application includes a web application where customers can browse and purchase music.</span></span> <span data-ttu-id="345ce-113">Det finns flera Azure-tjänster som kan vara värd för webbprogram i det här exemplet används en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="345ce-113">While there are several Azure services that can host web applications, for this example, a Virtual Machine is used.</span></span> <span data-ttu-id="345ce-114">Med mallen musik Store exempel kan en virtuell dator distribueras, installera en webbserver och musik Store webbplats installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="345ce-114">Using the sample Music Store template, a virtual machine is deployed, a web server install, and the Music Store website installed and configured.</span></span> <span data-ttu-id="345ce-115">För den här artikeln beskrivs bara distributionen av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="345ce-115">For the sake of this article, only the virtual machine deployment is detailed.</span></span> <span data-ttu-id="345ce-116">Konfigurationen av webbservern och programmet beskrivs i en senare artikel.</span><span class="sxs-lookup"><span data-stu-id="345ce-116">The configuration of the web server and the application is detailed in a later article.</span></span>

<span data-ttu-id="345ce-117">En virtuell dator kan läggas till en mall med hjälp av Visual Studio Lägg till ny resurs-guiden eller genom att infoga giltig JSON i mallen för distribution.</span><span class="sxs-lookup"><span data-stu-id="345ce-117">A virtual machine can be added to a template using the Visual Studio Add New Resource wizard, or by inserting valid JSON into the deployment template.</span></span> <span data-ttu-id="345ce-118">När du distribuerar en virtuell dator krävs också flera relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="345ce-118">When deploying a virtual machine, several related resources are also needed.</span></span> <span data-ttu-id="345ce-119">Om du använder Visual Studio för att skapa mallen, är dessa resurser skapas.</span><span class="sxs-lookup"><span data-stu-id="345ce-119">If using Visual Studio to create the template, these resources are created for you.</span></span> <span data-ttu-id="345ce-120">Om hur du manuellt skapar mallen, måste dessa resurser infogas och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="345ce-120">If manually constructing the template, these resources need to be inserted and configured.</span></span>

<span data-ttu-id="345ce-121">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).</span><span class="sxs-lookup"><span data-stu-id="345ce-121">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).</span></span>

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

<span data-ttu-id="345ce-122">Distribution kan kan egenskaperna för virtuella datorn ses i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="345ce-122">Once deployed, the virtual machine properties can be seen in the Azure portal.</span></span>

![Virtuell dator](./media/dotnet-core-2-architecture/vm-win.png)

## <a name="storage-account"></a><span data-ttu-id="345ce-124">Storage-konto</span><span class="sxs-lookup"><span data-stu-id="345ce-124">Storage Account</span></span>
<span data-ttu-id="345ce-125">Storage-konton har många lagringsalternativ och funktioner.</span><span class="sxs-lookup"><span data-stu-id="345ce-125">Storage accounts have many storage options and capabilities.</span></span> <span data-ttu-id="345ce-126">Ett lagringskonto innehåller virtuella hårddiskar på den virtuella datorn och eventuella ytterligare hårddiskar i kontexten av Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="345ce-126">For the context of Azure Virtual machines, a storage account holds the virtual hard drives of the virtual machine and any additional data disks.</span></span> <span data-ttu-id="345ce-127">Musik Store-exempel innehåller ett lagringskonto för att lagra den virtuella hårddisken på varje virtuell dator i distributionen.</span><span class="sxs-lookup"><span data-stu-id="345ce-127">The Music Store sample includes one storage account to hold the virtual hard drive of each virtual machine in the deployment.</span></span> 

<span data-ttu-id="345ce-128">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Lagringskonto](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).</span><span class="sxs-lookup"><span data-stu-id="345ce-128">Follow this link to see the JSON sample within the Resource Manager template – [Storage Account](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).</span></span>

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

<span data-ttu-id="345ce-129">Ett lagringskonto är associerat med en virtuell dator i Resource Manager mallen deklarationen av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="345ce-129">A storage account is associate with a virtual machine inside the Resource Manager template declaration of the virtual machine.</span></span> 

<span data-ttu-id="345ce-130">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella datorn och Storage-konto associationen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).</span><span class="sxs-lookup"><span data-stu-id="345ce-130">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine and Storage Account association](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).</span></span>

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

<span data-ttu-id="345ce-131">Efter distributionen kan storage-konto visas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="345ce-131">After deployment, the storage account can be viewed in the Azure portal.</span></span>

![Storage-konto](./media/dotnet-core-2-architecture/storacct-win.png)

<span data-ttu-id="345ce-133">Klicka i lagringsblobbehållare för kontot visas virtuell hårddiskfil för varje virtuell dator distribueras med hjälp av mallen.</span><span class="sxs-lookup"><span data-stu-id="345ce-133">Clicking into the storage account blob container, the virtual hard drive file for each virtual machine deployed with the template can be seen.</span></span>

![Virtuella hårddiskar](./media/dotnet-core-2-architecture/vhd-win.png)

<span data-ttu-id="345ce-135">Mer information om Azure Storage finns [Azure Storage-dokumentationen](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="345ce-135">For more information on Azure Storage, see [Azure Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>

## <a name="virtual-network"></a><span data-ttu-id="345ce-136">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="345ce-136">Virtual Network</span></span>
<span data-ttu-id="345ce-137">Om en virtuell dator kräver interna nätverk, till exempel möjligheten att kommunicera med andra virtuella datorer och Azure-resurser, krävs ett Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="345ce-137">If a virtual machine requires internal networking such as the ability to communicate with other virtual machines and Azure resources, an Azure Virtual Network is required.</span></span>  <span data-ttu-id="345ce-138">Ett virtuellt nätverk gör inte den virtuella datorn tillgänglig via internet.</span><span class="sxs-lookup"><span data-stu-id="345ce-138">A virtual network does not make the virtual machine accessible over the internet.</span></span> <span data-ttu-id="345ce-139">Anslutning för offentliga kräver en offentlig IP-adress som beskrivs senare i den här serien.</span><span class="sxs-lookup"><span data-stu-id="345ce-139">Public connectivity requires a public IP address, which is detailed later in this series.</span></span>

<span data-ttu-id="345ce-140">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella nätverk och undernät](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).</span><span class="sxs-lookup"><span data-stu-id="345ce-140">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Network and Subnets](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).</span></span>

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

<span data-ttu-id="345ce-141">Från Azure-portalen virtuellt nätverk ser ut som följande bild.</span><span class="sxs-lookup"><span data-stu-id="345ce-141">From the Azure portal, the virtual network looks like the following image.</span></span> <span data-ttu-id="345ce-142">Observera att alla virtuella datorer som distribueras med hjälp av mallen är anslutna till det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="345ce-142">Notice that all virtual machines deployed with the template are attached to the virtual network.</span></span>

![Virtual Network](./media/dotnet-core-2-architecture/vnet-win.png)

## <a name="network-interface"></a><span data-ttu-id="345ce-144">Nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="345ce-144">Network Interface</span></span>
 <span data-ttu-id="345ce-145">Ett nätverksgränssnitt ansluter en virtuell dator till ett virtuellt nätverk, mer specifikt till ett undernät som har definierats i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="345ce-145">A network interface connects a virtual machine to a virtual network, more specifically to a subnet that has been defined in the virtual network.</span></span> 

 <span data-ttu-id="345ce-146">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [nätverksgränssnittet](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).</span><span class="sxs-lookup"><span data-stu-id="345ce-146">Follow this link to see the JSON sample within the Resource Manager template – [Network Interface](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).</span></span>

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

<span data-ttu-id="345ce-147">Varje virtuell datorresurs innehåller en nätverksprofil.</span><span class="sxs-lookup"><span data-stu-id="345ce-147">Each virtual machine resource includes a network profile.</span></span> <span data-ttu-id="345ce-148">Nätverksgränssnittet är kopplat till den virtuella datorn i den här profilen.</span><span class="sxs-lookup"><span data-stu-id="345ce-148">The network interface is associated with the virtual machine in this profile.</span></span>  

<span data-ttu-id="345ce-149">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella nätverksprofil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).</span><span class="sxs-lookup"><span data-stu-id="345ce-149">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine Network Profile](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).</span></span>

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

<span data-ttu-id="345ce-150">Från Azure-portalen nätverksgränssnittet ser ut som följande bild.</span><span class="sxs-lookup"><span data-stu-id="345ce-150">From the Azure portal, the network interface looks like the following image.</span></span> <span data-ttu-id="345ce-151">Den interna IP-adressen och virtuella associationen kan ses på gränssnittet nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="345ce-151">The internal IP address and the virtual machine association can be seen on the network interface resource.</span></span>

![Nätverksgränssnitt](./media/dotnet-core-2-architecture/nic-win.png)

<span data-ttu-id="345ce-153">Mer information om virtuella Azure-nätverk finns [dokumentation för Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="345ce-153">For more information on Azure Virtual Networks, see [Azure Virtual Network documentation](https://azure.microsoft.com/documentation/services/virtual-network/).</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="345ce-154">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="345ce-154">Azure SQL Database</span></span>
<span data-ttu-id="345ce-155">Förutom en virtuell dator som värd för webbplatsen musik Store distribueras en Azure SQL Database som värd för musik Store-databasen.</span><span class="sxs-lookup"><span data-stu-id="345ce-155">In addition to a virtual machine hosting the Music Store website, an Azure SQL Database is deployed to host the Music Store database.</span></span> <span data-ttu-id="345ce-156">Fördelen med att använda Azure SQL Database här är att en annan uppsättning virtuella datorer är inte obligatoriska och tillgänglighet och skala är inbyggd i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="345ce-156">The advantage of using Azure SQL Database here is that a second set of virtual machines is not required, and scale and availability is built into the service.</span></span>

<span data-ttu-id="345ce-157">En Azure SQL database kan läggas till med Visual Studio lägga till nya resursen guiden, eller genom att infoga giltig JSON i en mall.</span><span class="sxs-lookup"><span data-stu-id="345ce-157">An Azure SQL database can be added using the Visual Studio Add New Resource wizard, or by inserting valid JSON into a template.</span></span> <span data-ttu-id="345ce-158">SQL Server-resurs innehåller ett användarnamn och lösenord som har beviljats administratörsbehörighet på SQL-instansen.</span><span class="sxs-lookup"><span data-stu-id="345ce-158">The SQL Server resource includes a user name and password that is granted administrative rights on the SQL instance.</span></span> <span data-ttu-id="345ce-159">Dessutom läggs en resurs för SQL-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="345ce-159">Also, a SQL firewall resource is added.</span></span> <span data-ttu-id="345ce-160">Som standard är program som finns i Azure kan ansluta till SQL-instansen.</span><span class="sxs-lookup"><span data-stu-id="345ce-160">By default, applications hosted in Azure are able to connect with the SQL instance.</span></span> <span data-ttu-id="345ce-161">Sådana en SQL Server Management studio att ansluta till SQL-instansen brandväggen måste konfigureras för att tillåta externa program.</span><span class="sxs-lookup"><span data-stu-id="345ce-161">To allow external application such a SQL Server Management studio to connect to the SQL instance, the firewall needs to be configured.</span></span> <span data-ttu-id="345ce-162">Standardkonfigurationen är bra för enkelhetens musik Store-demonstrationen.</span><span class="sxs-lookup"><span data-stu-id="345ce-162">For the sake of the Music Store demo, the default configuration is fine.</span></span> 

<span data-ttu-id="345ce-163">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).</span><span class="sxs-lookup"><span data-stu-id="345ce-163">Follow this link to see the JSON sample within the Resource Manager template – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).</span></span>

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

<span data-ttu-id="345ce-164">En vy av SQLServer och MusicStore databas som visas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="345ce-164">A view of the SQL server and MusicStore database as seen in the Azure portal.</span></span>

![SQL Server](./media/dotnet-core-2-architecture/sql-win.png)

<span data-ttu-id="345ce-166">Mer information om hur du distribuerar Azure SQL Database finns [dokumentation för Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="345ce-166">For more information on deploying Azure SQL Database, see [Azure SQL Database documentation](https://azure.microsoft.com/documentation/services/sql-database/).</span></span>

## <a name="next-step"></a><span data-ttu-id="345ce-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="345ce-167">Next step</span></span>
<hr>

[<span data-ttu-id="345ce-168">Steg 2 – åtkomst och säkerhet i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="345ce-168">Step 2 - Access and Security in Azure Resource Manager Templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

