---
title: "Skapa en virtuell dator för SQL Server i Azure PowerShell (Resource Manager) | Microsoft Docs"
description: "Innehåller steg och PowerShell-skript för att skapa en virtuell Azure-dator med SQL Server virtuella galleriavbildningar."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/17/2017
ms.author: jroth
ms.openlocfilehash: aa90a1d017af5f477407ab33f0580904472f412b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a><span data-ttu-id="9643d-103">Etablera en virtuell dator med SQL Server med hjälp av Azure PowerShell (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="9643d-103">Provision a SQL Server virtual machine using Azure PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9643d-104">Portal</span><span class="sxs-lookup"><span data-stu-id="9643d-104">Portal</span></span>](virtual-machines-windows-portal-sql-server-provision.md)
> * [<span data-ttu-id="9643d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9643d-105">PowerShell</span></span>](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a><span data-ttu-id="9643d-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="9643d-106">Overview</span></span>
<span data-ttu-id="9643d-107">Den här kursen visar hur du skapar en enda Azure virtuell dator med hjälp av den **Azure Resource Manager** distributionsmodellen med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="9643d-107">This tutorial shows you how to create a single Azure virtual machine using the **Azure Resource Manager** deployment model using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="9643d-108">I den här självstudiekursen skapar vi en virtuell dator med hjälp av en enskild disk från en avbildning i SQL-galleriet.</span><span class="sxs-lookup"><span data-stu-id="9643d-108">In this tutorial, we will create a single virtual machine using a single disk drive from an image in the SQL Gallery.</span></span> <span data-ttu-id="9643d-109">Vi skapar nya providers för lagring, nätverk och beräkningsresurser som ska användas av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-109">We will create new providers for the storage, network, and compute resources that will be used by the virtual machine.</span></span> <span data-ttu-id="9643d-110">Om du har befintliga providers för någon av dessa resurser, kan du använda dessa providers i stället.</span><span class="sxs-lookup"><span data-stu-id="9643d-110">If you have existing providers for any of these resources, you can use those providers instead.</span></span>

<span data-ttu-id="9643d-111">Om du behöver den klassiska versionen av det här avsnittet finns [etablera en virtuell dator med SQL Server med hjälp av Azure PowerShell klassiska](../classic/ps-sql-create.md).</span><span class="sxs-lookup"><span data-stu-id="9643d-111">If you need the classic version of this topic, see [Provision a SQL Server virtual machine using Azure PowerShell Classic](../classic/ps-sql-create.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9643d-112">Krav</span><span class="sxs-lookup"><span data-stu-id="9643d-112">Prerequisites</span></span>
<span data-ttu-id="9643d-113">Den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="9643d-113">For this tutorial you'll need:</span></span>

* <span data-ttu-id="9643d-114">En Azure-konto och prenumeration innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="9643d-114">An Azure account and subscription before you start.</span></span> <span data-ttu-id="9643d-115">Om du inte har någon registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9643d-115">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="9643d-116">[Azure PowerShell)](/powershell/azure/overview), lägsta version av 1.4.0 eller senare (den här kursen som skrivits med version 1.5.0).</span><span class="sxs-lookup"><span data-stu-id="9643d-116">[Azure PowerShell)](/powershell/azure/overview), minimum version of 1.4.0 or later (this tutorial written using version 1.5.0).</span></span>
  * <span data-ttu-id="9643d-117">Om du vill hämta din version skriver **Azure Get-Module - ListAvailable**.</span><span class="sxs-lookup"><span data-stu-id="9643d-117">To retrieve your version, type **Get-Module Azure -ListAvailable**.</span></span>

## <a name="configure-your-subscription"></a><span data-ttu-id="9643d-118">Konfigurera din prenumeration</span><span class="sxs-lookup"><span data-stu-id="9643d-118">Configure your subscription</span></span>
<span data-ttu-id="9643d-119">Öppna Windows PowerShell och upprätta åtkomst till ditt Azure-konto genom att köra följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9643d-119">Open Windows PowerShell and establish access to your Azure account by running the following cmdlet.</span></span> <span data-ttu-id="9643d-120">Visas med ett tecken på skärmen för att ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="9643d-120">You will be presented with a sign in screen to enter your credentials.</span></span> <span data-ttu-id="9643d-121">Använd samma e-post och lösenord som du använder för att logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9643d-121">Use the same email and password that you use to sign in to the Azure portal.</span></span>

    Add-AzureRmAccount

<span data-ttu-id="9643d-122">När du har loggat in visas viss information på skärmen som inkluderar prenumerations-ID som du har loggat in.</span><span class="sxs-lookup"><span data-stu-id="9643d-122">After successfully signing in you will see some information on screen that includes the SubscriptionId with which you signed in.</span></span> <span data-ttu-id="9643d-123">Det här är den prenumeration där resurser för den här självstudiekursen kommer att skapas om du inte ändrar till en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9643d-123">This is the subscription in which the resources for this tutorial will be created unless you change to a different subscription.</span></span> <span data-ttu-id="9643d-124">Om du har flera SubscriptionIds, kör du följande cmdlet för att returnera en lista över alla dina SubscriptionIds:</span><span class="sxs-lookup"><span data-stu-id="9643d-124">If you have multiple SubscriptionIds, run the following cmdlet to return a list of all of your SubscriptionIds:</span></span>

    Get-AzureRmSubscription

<span data-ttu-id="9643d-125">Kör följande cmdlet för att ändra till ett annat prenumerations-ID med önskade prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="9643d-125">To change to another SubscriptionID, run the following cmdlet with your desired SubscriptionId.</span></span>

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a><span data-ttu-id="9643d-126">Definiera variabler för avbildning</span><span class="sxs-lookup"><span data-stu-id="9643d-126">Define image variables</span></span>
<span data-ttu-id="9643d-127">För att förenkla användbarhet och förståelse av skriptet slutförda från den här självstudiekursen kommer vi börjar genom att definiera ett antal variabler.</span><span class="sxs-lookup"><span data-stu-id="9643d-127">To simplify usability and understanding of the completed script from this tutorial, we will start by defining a number of variables.</span></span> <span data-ttu-id="9643d-128">Ändra parametervärden som du vill, men varning för namngivning av restriktioner relaterade till namnet längder och specialtecken när du ändrar de värden som anges.</span><span class="sxs-lookup"><span data-stu-id="9643d-128">Change the parameter values as you see fit, but beware of naming restrictions related to name lengths and special characters when modifying the values provided.</span></span>

### <a name="location-and-resource-group"></a><span data-ttu-id="9643d-129">Plats och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9643d-129">Location and Resource Group</span></span>
<span data-ttu-id="9643d-130">Du kan använda två variabler för att definiera dataområdet och den resursgrupp som du vill skapa de andra resurserna för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-130">Use two variables to define the data region and the resource group into which you will create the other resources for the virtual machine.</span></span>

<span data-ttu-id="9643d-131">Ändra efter behov och kör följande cmdlets för att initiera dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="9643d-131">Modify as desired and then execute the following cmdlets to initialize these variables.</span></span>

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a><span data-ttu-id="9643d-132">Lagringsegenskaper för</span><span class="sxs-lookup"><span data-stu-id="9643d-132">Storage properties</span></span>
<span data-ttu-id="9643d-133">Du kan använda följande variabler för att definiera storage-konto och lagringstyp som ska användas av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-133">Use the following variables to define the storage account and the type of storage to be used by the virtual machine.</span></span>

<span data-ttu-id="9643d-134">Ändra efter behov och kör följande cmdlet för att initiera dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="9643d-134">Modify as desired and then execute the following cmdlet to initialize these variables.</span></span> <span data-ttu-id="9643d-135">Observera att i det här exemplet använder [Premiumlagring](../../../storage/common/storage-premium-storage.md), som rekommenderas för produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="9643d-135">Note that in this example, we are using [Premium Storage](../../../storage/common/storage-premium-storage.md), which is recommended for production workloads.</span></span> <span data-ttu-id="9643d-136">Mer information om den här vägledningen och andra rekommendationer finns [prestandarelaterade Metodtips för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-performance.md).</span><span class="sxs-lookup"><span data-stu-id="9643d-136">For details on this guidance and other recommendations, see [Performance best practices for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).</span></span>

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a><span data-ttu-id="9643d-137">Egenskaper för nätverk</span><span class="sxs-lookup"><span data-stu-id="9643d-137">Network properties</span></span>
<span data-ttu-id="9643d-138">Du kan använda följande variabler för att definiera nätverksgränssnittet, TCP/IP-allokeringsmetod, det virtuella nätverksnamnet, virtuella undernätsnamn, IP-adressintervall för det virtuella nätverket, IP-adressintervall för undernätet och offentliga domännamnet som ska användas av nätverket i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-138">Use the following variables to define the network interface, the TCP/IP allocation method, the virtual network name, the virtual subnet name, the range of IP addresses for the virtual network, the range of IP addresses for the subnet, and the public domain name label to be used by the network in the virtual machine.</span></span>

<span data-ttu-id="9643d-139">Ändra efter behov och kör följande cmdlet för att initiera dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="9643d-139">Modify as desired and then execute the following cmdlet to initialize these variables.</span></span>

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"

### <a name="virtual-machine-properties"></a><span data-ttu-id="9643d-140">Egenskaper för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9643d-140">Virtual machine properties</span></span>
<span data-ttu-id="9643d-141">Du kan använda följande variabler för att definiera namnet på virtuella datorn, namnet på datorn, storleken på virtuella datorn och operativsystemet diskens namn för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-141">Use the following variables to define the virtual machine name, the computer name, the virtual machine size, and the operating system disk name for the virtual machine.</span></span>

<span data-ttu-id="9643d-142">Ändra efter behov och kör följande cmdlet för att initiera dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="9643d-142">Modify as desired and then execute the following cmdlet to initialize these variables.</span></span>

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a><span data-ttu-id="9643d-143">Bildegenskaper</span><span class="sxs-lookup"><span data-stu-id="9643d-143">Image properties</span></span>
<span data-ttu-id="9643d-144">Du kan använda följande variabler för att definiera avbildningen som ska användas för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-144">Use the following variables to define the image to use for the virtual machine.</span></span> <span data-ttu-id="9643d-145">I det här exemplet används SQL Server 2016 Enterprise-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="9643d-145">In this example, the SQL Server 2016 Enterprise image is used.</span></span>

<span data-ttu-id="9643d-146">Ändra efter behov och kör följande cmdlet för att initiera dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="9643d-146">Modify as desired and then execute the following cmdlet to initialize these variables.</span></span>

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

<span data-ttu-id="9643d-147">Observera att du kan få en fullständig lista över erbjudanden för SQL Server-avbildning med kommandot Get-AzureRmVMImageOffer:</span><span class="sxs-lookup"><span data-stu-id="9643d-147">Note that you can get a full list of SQL Server image offerings with the Get-AzureRmVMImageOffer command:</span></span>

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

<span data-ttu-id="9643d-148">Och du kan se tillgängliga för ett erbjudande med kommandot Get-AzureRmVMImageSku SKU: er.</span><span class="sxs-lookup"><span data-stu-id="9643d-148">And you can see the Skus available for an offering with the Get-AzureRmVMImageSku command.</span></span> <span data-ttu-id="9643d-149">Följande kommando visar alla SKU: er för den **SQL2014SP1 WS2012R2** erbjuder.</span><span class="sxs-lookup"><span data-stu-id="9643d-149">The following command shows all Skus available for the **SQL2014SP1-WS2012R2** offer.</span></span>

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a><span data-ttu-id="9643d-150">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="9643d-150">Create a resource group</span></span>
<span data-ttu-id="9643d-151">Med Resource Manager-distributionsmodellen är det första objektet som du skapar resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9643d-151">With the Resource Manager deployment model, the first object that you create is the resource group.</span></span> <span data-ttu-id="9643d-152">Vi använder den [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) för att skapa en Azure-resursgrupp och dess resurser med resursgruppens namn och plats som anges av de variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="9643d-152">We will use the [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet to create an Azure resource group and its resources with the resource group name and location defined by the variables that you previously initialized.</span></span>

<span data-ttu-id="9643d-153">Kör följande cmdlet för att skapa din nya resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9643d-153">Execute the following cmdlet to create your new resource group.</span></span>

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a><span data-ttu-id="9643d-154">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="9643d-154">Create a storage account</span></span>
<span data-ttu-id="9643d-155">Den virtuella datorn krävs lagringsresurser för operativsystemets disk och för de SQL Server data- och loggfiler.</span><span class="sxs-lookup"><span data-stu-id="9643d-155">The virtual machine requires storage resources for the operating system disk and for the SQL Server data and log files.</span></span> <span data-ttu-id="9643d-156">För enkelhetens skull skapar vi en enskild disk för båda.</span><span class="sxs-lookup"><span data-stu-id="9643d-156">For simplicity, we will create a single disk for both.</span></span> <span data-ttu-id="9643d-157">Du kan koppla ytterligare diskar senare med den [Lägg till Azure-disken](/powershell/module/azure/add-azuredisk) cmdlet för att placera din SQL Server-data och loggfilen filer på dedikerade diskar.</span><span class="sxs-lookup"><span data-stu-id="9643d-157">You can attach additional disks later using the [Add-Azure Disk](/powershell/module/azure/add-azuredisk) cmdlet in order to place your SQL Server data and log files on dedicated disks.</span></span> <span data-ttu-id="9643d-158">Vi använder den [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) för att skapa ett standardlagringskonto i din nya resursgrupp och med lagringskontonamn, Sku namn och plats som definierats med hjälp av de variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="9643d-158">We will use the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet to create a standard storage account in your new resource group and with the storage account name, storage Sku name, and location defined using the variables that you previously initialized.</span></span>

<span data-ttu-id="9643d-159">Kör följande cmdlet för att skapa det nya kontot.</span><span class="sxs-lookup"><span data-stu-id="9643d-159">Execute the following cmdlet to create your new storage account.</span></span>

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a><span data-ttu-id="9643d-160">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="9643d-160">Create network resources</span></span>
<span data-ttu-id="9643d-161">Den virtuella datorn kräver ett antal nätverksresurser för nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="9643d-161">The virtual machine requires a number of network resources for network connectivity.</span></span>

* <span data-ttu-id="9643d-162">Varje virtuell dator kräver ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="9643d-162">Each virtual machine requires a virtual network.</span></span>
* <span data-ttu-id="9643d-163">Ett virtuellt nätverk måste ha minst ett undernät som har definierats.</span><span class="sxs-lookup"><span data-stu-id="9643d-163">A virtual network must have at least one subnet defined.</span></span>
* <span data-ttu-id="9643d-164">Ett nätverksgränssnitt måste definieras med en offentlig eller privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="9643d-164">A network interface must be defined with either a public or a private IP address.</span></span>

### <a name="create-a-virtual-network-subnet-configuration"></a><span data-ttu-id="9643d-165">Skapa en konfiguration av undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="9643d-165">Create a virtual network subnet configuration</span></span>
<span data-ttu-id="9643d-166">Vi startar genom att skapa en konfiguration av undernät för våra virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="9643d-166">We will start by creating a subnet configuration for our virtual network.</span></span> <span data-ttu-id="9643d-167">I våra självstudier ska vi skapar en standard undernätet med hjälp av den [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9643d-167">For our tutorial, we will create a default subnet using the [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet.</span></span> <span data-ttu-id="9643d-168">Vi skapar våra undernätskonfiguration av virtuellt nätverk med namn och adress undernätsprefixet definieras med hjälp av de variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="9643d-168">We will create our virtual network subnet configuration with the subnet name and address prefix defined using the variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="9643d-169">Du kan ange ytterligare egenskaper för virtuellt nätverk undernätskonfiguration använder denna cmdlet, men som ligger utanför omfånget för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="9643d-169">You can define additional properties of the virtual network subnet configuration using this cmdlet, but that is beyond the scope of this tutorial.</span></span>
>
>

<span data-ttu-id="9643d-170">Kör följande cmdlet för att skapa konfigurationen för virtuella undernät.</span><span class="sxs-lookup"><span data-stu-id="9643d-170">Execute the following cmdlet to create your virtual subnet configuration.</span></span>

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a><span data-ttu-id="9643d-171">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="9643d-171">Create a virtual network</span></span>
<span data-ttu-id="9643d-172">Därefter skapar vi vårt virtuella datornätverk som använder den [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9643d-172">Next, we will create our virtual network using the [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet.</span></span> <span data-ttu-id="9643d-173">Vi skapar vårt virtuella nätverk i din nya resursgrupp, med namn, plats och adress prefix definieras de variabler som du tidigare har initierat, samt använder undernätskonfiguration som du definierade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="9643d-173">We will create our virtual network in your new resource group, with the name, location, and address prefix defined using the variables that you previously initialized, and using the subnet configuration that you defined in the previous step.</span></span>

<span data-ttu-id="9643d-174">Kör följande cmdlet för att skapa det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="9643d-174">Execute the following cmdlet to create your virtual network.</span></span>

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a><span data-ttu-id="9643d-175">Skapa offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="9643d-175">Create the public IP address</span></span>
<span data-ttu-id="9643d-176">Nu när vi har vårt virtuella nätverk som definierats som vi behöver konfigurera en IP-adress för anslutning till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-176">Now that we have our virtual network defined, we need to configure an IP address for connectivity to the virtual machine.</span></span> <span data-ttu-id="9643d-177">Den här självstudiekursen skapar vi en offentlig IP-adress med hjälp av dynamisk IP-adresser för att stödja Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="9643d-177">For this tutorial, we will create a public IP address using dynamic IP addressing to support Internet connectivity.</span></span> <span data-ttu-id="9643d-178">Vi använder den [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) för att skapa den offentliga IP-adressen i resursen grupp skapat prevously och med namn, plats, fördelning och DNS-domännamnet anges med hjälp av de variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="9643d-178">We will use the [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet to create the public IP address in the resource group created prevously and with the name, location, allocation method, and DNS domain name label defined using the variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="9643d-179">Du kan ange ytterligare egenskaper för den offentliga IP-adressen som använder denna cmdlet, men som ligger utanför omfånget för den här första självstudien.</span><span class="sxs-lookup"><span data-stu-id="9643d-179">You can define additional properties of the public IP address using this cmdlet, but that is beyond the scope of this initial tutorial.</span></span> <span data-ttu-id="9643d-180">Du kan också skapa en privat adress eller en adress med en statisk adress, men som även är utanför omfattningen för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="9643d-180">You could also create a private address or an address with a static address, but that is also beyond the scope of this tutorial.</span></span>
>
>

<span data-ttu-id="9643d-181">Kör följande cmdlet för att skapa din offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="9643d-181">Execute the following cmdlet to create your public IP address.</span></span>

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a><span data-ttu-id="9643d-182">Skapa nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="9643d-182">Create the network interface</span></span>
<span data-ttu-id="9643d-183">Vi är nu redo att skapa nätverksgränssnittet som våra virtuella datorn ska använda.</span><span class="sxs-lookup"><span data-stu-id="9643d-183">We are now ready to create the network interface that our virtual machine will use.</span></span> <span data-ttu-id="9643d-184">Vi använder den [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) för att skapa våra nätverksgränssnitt i resursgruppen som du skapade tidigare och med namn, plats, undernät och offentliga IP-adressen har definierats.</span><span class="sxs-lookup"><span data-stu-id="9643d-184">We will use the [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet to create our network interface in the resource group created earlier and with the name, location, subnet and public IP address previously defined.</span></span>

<span data-ttu-id="9643d-185">Kör följande cmdlet för att skapa nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="9643d-185">Execute the following cmdlet to create your network interface.</span></span>

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a><span data-ttu-id="9643d-186">Konfigurera ett VM-objekt</span><span class="sxs-lookup"><span data-stu-id="9643d-186">Configure a VM object</span></span>
<span data-ttu-id="9643d-187">Nu när vi har lagring och nätverksresurser som definierats är vi redo att definiera beräkningsresurser för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-187">Now that we have storage and network resources defined, we are ready to define compute resources for the virtual machine.</span></span> <span data-ttu-id="9643d-188">I våra självstudier ska vi ange storleken på virtuella datorn och egenskaper för olika operativsystem, ange det nätverksgränssnitt som vi skapade tidigare, definiera blob-lagring och sedan ange operativsystemets disk.</span><span class="sxs-lookup"><span data-stu-id="9643d-188">For our tutorial, we will specify the virtual machine size and various operating system properties, specify the network interface that we previously created, define blob storage, and then specify the operating system disk.</span></span>

### <a name="create-the-vm-object"></a><span data-ttu-id="9643d-189">Skapa VM-objekt</span><span class="sxs-lookup"><span data-stu-id="9643d-189">Create the VM object</span></span>
<span data-ttu-id="9643d-190">Vi startar genom att ange storleken på virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-190">We will start by specifying the virtual machine size.</span></span> <span data-ttu-id="9643d-191">Den här självstudiekursen kommer specificerar vi en DS13.</span><span class="sxs-lookup"><span data-stu-id="9643d-191">For this tutorial, we are specifying a DS13.</span></span> <span data-ttu-id="9643d-192">Vi använder den [ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) för att skapa en konfigurerbar virtuella datorobjektet med namnet och storleken som definieras med hjälp av de variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="9643d-192">We will use the [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet to create a configurable virtual machine object with the name and size defined using the variables that you previously initialized.</span></span>

<span data-ttu-id="9643d-193">Kör följande cmdlet för att skapa det virtuella datorobjektet.</span><span class="sxs-lookup"><span data-stu-id="9643d-193">Execute the following cmdlet to create the virtual machine object.</span></span>

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a><span data-ttu-id="9643d-194">Skapa ett autentiseringsuppgiftobjekt för namn och lösenord för autentiseringsuppgifter för lokal administratör</span><span class="sxs-lookup"><span data-stu-id="9643d-194">Create a credential object to hold the name and password for the local administrator credentials</span></span>
<span data-ttu-id="9643d-195">Innan vi kan ange egenskaperna för operativsystemet för den virtuella datorn måste behöver vi ange autentiseringsuppgifter för det lokala administratörskontot som en säker sträng.</span><span class="sxs-lookup"><span data-stu-id="9643d-195">Before we can set the operating system properties for the virtual machine, we need to supply the credentials for the local administrator account as a secure string.</span></span> <span data-ttu-id="9643d-196">För att åstadkomma detta, kommer vi att använda den [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9643d-196">To accomplish this, we will use the [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

<span data-ttu-id="9643d-197">Kör följande cmdlet och i fönstret Windows PowerShell autentiseringsuppgifter, skriver du namnet och lösenordet för det lokala administratörskontot på den virtuella datorn i Windows.</span><span class="sxs-lookup"><span data-stu-id="9643d-197">Execute the following cmdlet and, in the Windows PowerShell credential request window, type the name and password to use for the local administrator account in the Windows virtual machine.</span></span>

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a><span data-ttu-id="9643d-198">Ange egenskaperna för operativsystemet för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9643d-198">Set the operating system properties for the virtual machine</span></span>
<span data-ttu-id="9643d-199">Vi är nu redo att ange egenskaper för den virtuella datorns operativsystem.</span><span class="sxs-lookup"><span data-stu-id="9643d-199">Now we are ready to set the virtual machine's operating system properties.</span></span> <span data-ttu-id="9643d-200">Vi använder den [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) för att ange vilken typ av operativsystem som Windows, kräver den [virtuella datorns agent](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) anger att cmdleten aktiverar automatisk uppdatering är installerad, och ange namnet på virtuella datorn, datornamnet och autentiseringsuppgifter med hjälp av de variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="9643d-200">We will use the [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet to set the type of operating system as Windows, require the [virtual machine agent](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) to be installed, specify that the cmdlet enables auto update and set the virtual machine name, the computer name, and the credential using the variables that you previously initialized.</span></span>

<span data-ttu-id="9643d-201">Kör följande cmdlet för att ange egenskaperna för operativsystemet för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-201">Execute the following cmdlet to set the operating system properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a><span data-ttu-id="9643d-202">Lägg till nätverksgränssnittet till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9643d-202">Add the network interface to the virtual machine</span></span>
<span data-ttu-id="9643d-203">Nu ska vi lägger till nätverksgränssnittet som vi skapade tidigare till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-203">Next, we will add the network interface that we created previously to the virtual machine.</span></span> <span data-ttu-id="9643d-204">Vi använder den [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) för att lägga till nätverksgränssnittet som använder nätverket gränssnittet variabeln som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="9643d-204">We will use the [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet to add the network interface using the network interface variable that you defined earlier.</span></span>

<span data-ttu-id="9643d-205">Kör följande cmdlet om du vill ange nätverksgränssnittet för din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="9643d-205">Execute the following cmdlet to set the network interface for your virtual machine.</span></span>

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a><span data-ttu-id="9643d-206">Ange blob-lagringsplats för disken som ska användas av den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9643d-206">Set the blob storage location for the disk to be used by the virtual machine</span></span>
<span data-ttu-id="9643d-207">Vi kommer sedan att ange blob-lagringsplats för disken som ska användas av den virtuella datorn med de variabler som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="9643d-207">Next, we will set the blob storage location for the disk to be used by the virtual machine using the variables that you defined earlier.</span></span>

<span data-ttu-id="9643d-208">Kör följande cmdlet för att ange platsen för blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="9643d-208">Execute the following cmdlet to set the blob storage location.</span></span>

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a><span data-ttu-id="9643d-209">Ange operativsystemet diskegenskaper för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9643d-209">Set the operating system disk properties for the virtual machine</span></span>
<span data-ttu-id="9643d-210">Vi kommer därefter ange operativsystemet diskegenskaper för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-210">Next, we will set the operating system disk properties for the virtual machine.</span></span> <span data-ttu-id="9643d-211">Vi använder den [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) för att ange att operativsystemet för den virtuella datorn ska hämtas från en bild för att definiera cachelagring för att skrivskyddat (eftersom SQL Server installeras på samma disk) och ange namnet på virtuella datorn och operativsystemdisken definieras med hjälp av de variabler som vi definierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9643d-211">We will use the [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet to specify that the operating system for the virtual machine will come from an image, to set caching to read only (because SQL Server is being installed on the same disk) and define the virtual machine name and the operating system disk defined using the variables that we defined earlier.</span></span>

<span data-ttu-id="9643d-212">Kör följande cmdlet om du vill ange operativsystemet diskegenskaper för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-212">Execute the following cmdlet to set the operating system disk properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a><span data-ttu-id="9643d-213">Ange plattformsavbildningen som för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9643d-213">Specify the platform image for the virtual machine</span></span>
<span data-ttu-id="9643d-214">Vår senaste konfigurationssteget är att ange plattformsavbildning för våra virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-214">Our last configuration step is to specify the platform image for our virtual machine.</span></span> <span data-ttu-id="9643d-215">I våra självstudier ska använder vi den senaste SQL Server 2016 CTP-bilden.</span><span class="sxs-lookup"><span data-stu-id="9643d-215">For our tutorial, we are using the latest SQL Server 2016 CTP image.</span></span> <span data-ttu-id="9643d-216">Vi använder den [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) för att använda den här avbildningen som definieras av de variabler som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="9643d-216">We will use the [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet to use this image as defined by the variables that you defined earlier.</span></span>

<span data-ttu-id="9643d-217">Kör följande cmdlet för att ange plattformsavbildningen som för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-217">Execute the following cmdlet to specify the platform image for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a><span data-ttu-id="9643d-218">Skapa den virtuella SQL-datorn</span><span class="sxs-lookup"><span data-stu-id="9643d-218">Create the SQL VM</span></span>
<span data-ttu-id="9643d-219">Nu när du är klar konfigurationsstegen är du redo att skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-219">Now that you have finished the configuration steps, you are ready to create the virtual machine.</span></span> <span data-ttu-id="9643d-220">Vi använder den [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) för att skapa den virtuella datorn med de variabler som vi har definierat.</span><span class="sxs-lookup"><span data-stu-id="9643d-220">We will use the [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet to create the virtual machine using the variables that we have defined.</span></span>

<span data-ttu-id="9643d-221">Kör följande cmdlet om du vill skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-221">Execute the following cmdlet to create your virtual machine.</span></span>

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

<span data-ttu-id="9643d-222">Den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="9643d-222">The virtual machine is created.</span></span> <span data-ttu-id="9643d-223">Observera att skapas ett standardlagringskonto för startdiagnostikinställningar eftersom det angivna lagringskontot för den virtuella disken är ett premiumlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="9643d-223">Notice that a standard storage account is created for boot diagnostics because the specified storage account for the virtual machine's disk is a premium storage account.</span></span>

<span data-ttu-id="9643d-224">Nu kan du visa den här datorn i Azure-portalen för att se [offentliga IP-adressen och dess fullständigt kvalificerade domännamnet](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="9643d-224">You can now view this machine in the Azure Portal to see [its public IP address and its fully qualified domain name](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

## <a name="example-script"></a><span data-ttu-id="9643d-225">Exempelskriptet</span><span class="sxs-lookup"><span data-stu-id="9643d-225">Example script</span></span>
<span data-ttu-id="9643d-226">Följande skript innehåller fullständig PowerShell-skriptet för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="9643d-226">The following script contains the complete PowerShell script for this tutorial.</span></span> <span data-ttu-id="9643d-227">Det förutsätts att du har redan installerat Azure-prenumerationen för användning med den **Add-AzureRmAccount** och **Select-AzureRmSubscription** kommandon.</span><span class="sxs-lookup"><span data-stu-id="9643d-227">It assumes that you have already setup the Azure subscription to use with the **Add-AzureRmAccount** and **Select-AzureRmSubscription** commands.</span></span>

    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a><span data-ttu-id="9643d-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9643d-228">Next steps</span></span>
<span data-ttu-id="9643d-229">Du är redo att ansluta till den virtuella datorn med installation och RDP-anslutning när du har skapat den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9643d-229">After the virtual machine is created, you are ready to connect to the virtual machine using RDP and setup connectivity.</span></span> <span data-ttu-id="9643d-230">Mer information finns i [Anslut till en SQL Server-dator i Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).</span><span class="sxs-lookup"><span data-stu-id="9643d-230">For more information, see [Connect to a SQL Server Virtual Machine on Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).</span></span>

