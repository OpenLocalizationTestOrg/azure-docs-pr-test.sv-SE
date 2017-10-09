---
title: aaaCreate SQL Server-dator i Azure PowerShell (Resource Manager) | Microsoft Docs
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
ms.openlocfilehash: 2b8cb8f69ff9894a95eab617816a60c8674eeefa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a><span data-ttu-id="e4ea1-103">Etablera en virtuell dator med SQL Server med hjälp av Azure PowerShell (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="e4ea1-103">Provision a SQL Server virtual machine using Azure PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e4ea1-104">Portal</span><span class="sxs-lookup"><span data-stu-id="e4ea1-104">Portal</span></span>](virtual-machines-windows-portal-sql-server-provision.md)
> * [<span data-ttu-id="e4ea1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e4ea1-105">PowerShell</span></span>](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a><span data-ttu-id="e4ea1-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="e4ea1-106">Overview</span></span>
<span data-ttu-id="e4ea1-107">Den här kursen visar hur toocreate som en enda virtuell Azure-dator med hjälp av hello **Azure Resource Manager** distributionsmodellen med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-107">This tutorial shows you how toocreate a single Azure virtual machine using hello **Azure Resource Manager** deployment model using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="e4ea1-108">I den här självstudiekursen skapar vi en virtuell dator med en enskild disk från en avbildning i hello SQL-galleriet.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-108">In this tutorial, we will create a single virtual machine using a single disk drive from an image in hello SQL Gallery.</span></span> <span data-ttu-id="e4ea1-109">Vi skapar nya providers för hello lagring, nätverk och beräkningsresurser som ska användas av hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-109">We will create new providers for hello storage, network, and compute resources that will be used by hello virtual machine.</span></span> <span data-ttu-id="e4ea1-110">Om du har befintliga providers för någon av dessa resurser, kan du använda dessa providers i stället.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-110">If you have existing providers for any of these resources, you can use those providers instead.</span></span>

<span data-ttu-id="e4ea1-111">Om du behöver hello klassiska versionen av det här avsnittet, se [etablera en virtuell dator med SQL Server med hjälp av Azure PowerShell klassiska](../classic/ps-sql-create.md).</span><span class="sxs-lookup"><span data-stu-id="e4ea1-111">If you need hello classic version of this topic, see [Provision a SQL Server virtual machine using Azure PowerShell Classic](../classic/ps-sql-create.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4ea1-112">Krav</span><span class="sxs-lookup"><span data-stu-id="e4ea1-112">Prerequisites</span></span>
<span data-ttu-id="e4ea1-113">Den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="e4ea1-113">For this tutorial you'll need:</span></span>

* <span data-ttu-id="e4ea1-114">En Azure-konto och prenumeration innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-114">An Azure account and subscription before you start.</span></span> <span data-ttu-id="e4ea1-115">Om du inte har någon registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e4ea1-115">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e4ea1-116">[Azure PowerShell)](/powershell/azure/overview), lägsta version av 1.4.0 eller senare (den här kursen som skrivits med version 1.5.0).</span><span class="sxs-lookup"><span data-stu-id="e4ea1-116">[Azure PowerShell)](/powershell/azure/overview), minimum version of 1.4.0 or later (this tutorial written using version 1.5.0).</span></span>
  * <span data-ttu-id="e4ea1-117">tooretrieve din version, typen **Azure Get-Module - ListAvailable**.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-117">tooretrieve your version, type **Get-Module Azure -ListAvailable**.</span></span>

## <a name="configure-your-subscription"></a><span data-ttu-id="e4ea1-118">Konfigurera din prenumeration</span><span class="sxs-lookup"><span data-stu-id="e4ea1-118">Configure your subscription</span></span>
<span data-ttu-id="e4ea1-119">Öppna Windows PowerShell och upprätta åtkomst tooyour Azure-konto genom att köra följande cmdlet hello.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-119">Open Windows PowerShell and establish access tooyour Azure account by running hello following cmdlet.</span></span> <span data-ttu-id="e4ea1-120">Visas med en inloggning skärmen tooenter dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-120">You will be presented with a sign in screen tooenter your credentials.</span></span> <span data-ttu-id="e4ea1-121">Använd hello samma e-post och lösenord du använder toosign i toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-121">Use hello same email and password that you use toosign in toohello Azure portal.</span></span>

    Add-AzureRmAccount

<span data-ttu-id="e4ea1-122">När du har loggat in visas viss information på skärmen som inkluderar hello prenumerations-ID som du har loggat in.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-122">After successfully signing in you will see some information on screen that includes hello SubscriptionId with which you signed in.</span></span> <span data-ttu-id="e4ea1-123">Detta är hello prenumeration där hello resurser för den här självstudiekursen kommer att skapas om du inte ändrar tooa annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-123">This is hello subscription in which hello resources for this tutorial will be created unless you change tooa different subscription.</span></span> <span data-ttu-id="e4ea1-124">Om du har flera SubscriptionIds, kör du följande cmdlet tooreturn hello en lista över alla dina SubscriptionIds:</span><span class="sxs-lookup"><span data-stu-id="e4ea1-124">If you have multiple SubscriptionIds, run hello following cmdlet tooreturn a list of all of your SubscriptionIds:</span></span>

    Get-AzureRmSubscription

<span data-ttu-id="e4ea1-125">toochange tooanother SubscriptionID, kör hello som följer med din önskade SubscriptionId.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-125">toochange tooanother SubscriptionID, run hello following cmdlet with your desired SubscriptionId.</span></span>

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a><span data-ttu-id="e4ea1-126">Definiera variabler för avbildning</span><span class="sxs-lookup"><span data-stu-id="e4ea1-126">Define image variables</span></span>
<span data-ttu-id="e4ea1-127">toosimplify användbarhet och förståelse av hello slutförts skriptet från den här självstudiekursen kommer vi starta genom att definiera ett antal variabler.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-127">toosimplify usability and understanding of hello completed script from this tutorial, we will start by defining a number of variables.</span></span> <span data-ttu-id="e4ea1-128">Ändra hello parametervärden som du vill, men varning för namngivning begränsningar relaterade tooname längder och specialtecken när du ändrar hello värden.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-128">Change hello parameter values as you see fit, but beware of naming restrictions related tooname lengths and special characters when modifying hello values provided.</span></span>

### <a name="location-and-resource-group"></a><span data-ttu-id="e4ea1-129">Plats och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-129">Location and Resource Group</span></span>
<span data-ttu-id="e4ea1-130">Använda två variabler toodefine hello data region och hello resursgrupp som du skapar hello andra resurser för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-130">Use two variables toodefine hello data region and hello resource group into which you will create hello other resources for hello virtual machine.</span></span>

<span data-ttu-id="e4ea1-131">Ändra efter behov och kör följande cmdlets tooinitialize hello dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-131">Modify as desired and then execute hello following cmdlets tooinitialize these variables.</span></span>

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a><span data-ttu-id="e4ea1-132">Lagringsegenskaper för</span><span class="sxs-lookup"><span data-stu-id="e4ea1-132">Storage properties</span></span>
<span data-ttu-id="e4ea1-133">Använd hello följande variabler toodefine hello konto och hello lagringstyp för lagring toobe som används av hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-133">Use hello following variables toodefine hello storage account and hello type of storage toobe used by hello virtual machine.</span></span>

<span data-ttu-id="e4ea1-134">Ändra efter behov och kör följande cmdlet tooinitialize hello dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-134">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span> <span data-ttu-id="e4ea1-135">Observera att i det här exemplet använder [Premiumlagring](../../../storage/common/storage-premium-storage.md), som rekommenderas för produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-135">Note that in this example, we are using [Premium Storage](../../../storage/common/storage-premium-storage.md), which is recommended for production workloads.</span></span> <span data-ttu-id="e4ea1-136">Mer information om den här vägledningen och andra rekommendationer finns [prestandarelaterade Metodtips för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-performance.md).</span><span class="sxs-lookup"><span data-stu-id="e4ea1-136">For details on this guidance and other recommendations, see [Performance best practices for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).</span></span>

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a><span data-ttu-id="e4ea1-137">Egenskaper för nätverk</span><span class="sxs-lookup"><span data-stu-id="e4ea1-137">Network properties</span></span>
<span data-ttu-id="e4ea1-138">Använd hello efter variabler toodefine hello nätverksgränssnitt, hello TCP/IP-allokeringsmetod, hello virtuella nätverksnamnet, hello virtuella undernätsnamn, hello intervall med IP-adresser för hello virtuella nätverk, hello intervall av IP-adresser för hello-undernät och hello den offentliga domänen namnet etikett toobe används av hello nätverk i hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-138">Use hello following variables toodefine hello network interface, hello TCP/IP allocation method, hello virtual network name, hello virtual subnet name, hello range of IP addresses for hello virtual network, hello range of IP addresses for hello subnet, and hello public domain name label toobe used by hello network in hello virtual machine.</span></span>

<span data-ttu-id="e4ea1-139">Ändra efter behov och kör följande cmdlet tooinitialize hello dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-139">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span>

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"

### <a name="virtual-machine-properties"></a><span data-ttu-id="e4ea1-140">Egenskaper för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e4ea1-140">Virtual machine properties</span></span>
<span data-ttu-id="e4ea1-141">Använd hello följande variabler toodefine hello virtuellt datornamn, hello datornamn, hello storlek på virtuell dator och hello operativsystemet disknamnet för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-141">Use hello following variables toodefine hello virtual machine name, hello computer name, hello virtual machine size, and hello operating system disk name for hello virtual machine.</span></span>

<span data-ttu-id="e4ea1-142">Ändra efter behov och kör följande cmdlet tooinitialize hello dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-142">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span>

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a><span data-ttu-id="e4ea1-143">Bildegenskaper</span><span class="sxs-lookup"><span data-stu-id="e4ea1-143">Image properties</span></span>
<span data-ttu-id="e4ea1-144">Använd hello följande variabler toodefine hello avbildningen toouse för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-144">Use hello following variables toodefine hello image toouse for hello virtual machine.</span></span> <span data-ttu-id="e4ea1-145">I det här exemplet används hello SQL Server 2016 Enterprise bilden.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-145">In this example, hello SQL Server 2016 Enterprise image is used.</span></span>

<span data-ttu-id="e4ea1-146">Ändra efter behov och kör följande cmdlet tooinitialize hello dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-146">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span>

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

<span data-ttu-id="e4ea1-147">Observera att du kan få en fullständig lista över SQL Server avbildningen erbjudanden med hello Get-AzureRmVMImageOffer kommando:</span><span class="sxs-lookup"><span data-stu-id="e4ea1-147">Note that you can get a full list of SQL Server image offerings with hello Get-AzureRmVMImageOffer command:</span></span>

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

<span data-ttu-id="e4ea1-148">Och du kan se hello SKU: er som är tillgängliga för ett erbjudande med hello Get-AzureRmVMImageSku kommando.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-148">And you can see hello Skus available for an offering with hello Get-AzureRmVMImageSku command.</span></span> <span data-ttu-id="e4ea1-149">hello följande kommando visar alla SKU: er för hello **SQL2014SP1 WS2012R2** erbjuder.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-149">hello following command shows all Skus available for hello **SQL2014SP1-WS2012R2** offer.</span></span>

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a><span data-ttu-id="e4ea1-150">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e4ea1-150">Create a resource group</span></span>
<span data-ttu-id="e4ea1-151">Med hello Resource Manager-distributionsmodellen är första hello-objekt som du skapar hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-151">With hello Resource Manager deployment model, hello first object that you create is hello resource group.</span></span> <span data-ttu-id="e4ea1-152">Vi använder hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet toocreate en Azure-resursgrupp och dess resurser med hello resurs gruppera namn och plats som definierats av hello variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-152">We will use hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet toocreate an Azure resource group and its resources with hello resource group name and location defined by hello variables that you previously initialized.</span></span>

<span data-ttu-id="e4ea1-153">Kör följande cmdlet toocreate hello din nya resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-153">Execute hello following cmdlet toocreate your new resource group.</span></span>

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a><span data-ttu-id="e4ea1-154">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e4ea1-154">Create a storage account</span></span>
<span data-ttu-id="e4ea1-155">hello virtuell dator kräver lagringsresurser för hello operativsystemdisken och hello SQL Server-data och loggfiler.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-155">hello virtual machine requires storage resources for hello operating system disk and for hello SQL Server data and log files.</span></span> <span data-ttu-id="e4ea1-156">För enkelhetens skull skapar vi en enskild disk för båda.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-156">For simplicity, we will create a single disk for both.</span></span> <span data-ttu-id="e4ea1-157">Du kan koppla ytterligare diskar senare med hello [Lägg till Azure-disken](/powershell/module/azure/add-azuredisk) cmdlet i ordning tooplace SQL Server-data och loggfiler på dedikerade diskar.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-157">You can attach additional disks later using hello [Add-Azure Disk](/powershell/module/azure/add-azuredisk) cmdlet in order tooplace your SQL Server data and log files on dedicated disks.</span></span> <span data-ttu-id="e4ea1-158">Vi använder hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet toocreate en standardlagring konto i din nya resursgrupp och med hello lagringskontonamn, Sku namn och plats som definierats med hjälp av hello variabler som du tidigare har initierats.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-158">We will use hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet toocreate a standard storage account in your new resource group and with hello storage account name, storage Sku name, and location defined using hello variables that you previously initialized.</span></span>

<span data-ttu-id="e4ea1-159">Kör följande cmdlet toocreate hello det nya kontot.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-159">Execute hello following cmdlet toocreate your new storage account.</span></span>

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a><span data-ttu-id="e4ea1-160">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="e4ea1-160">Create network resources</span></span>
<span data-ttu-id="e4ea1-161">hello virtuell dator kräver ett antal nätverksresurser för nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-161">hello virtual machine requires a number of network resources for network connectivity.</span></span>

* <span data-ttu-id="e4ea1-162">Varje virtuell dator kräver ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-162">Each virtual machine requires a virtual network.</span></span>
* <span data-ttu-id="e4ea1-163">Ett virtuellt nätverk måste ha minst ett undernät som har definierats.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-163">A virtual network must have at least one subnet defined.</span></span>
* <span data-ttu-id="e4ea1-164">Ett nätverksgränssnitt måste definieras med en offentlig eller privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-164">A network interface must be defined with either a public or a private IP address.</span></span>

### <a name="create-a-virtual-network-subnet-configuration"></a><span data-ttu-id="e4ea1-165">Skapa en konfiguration av undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="e4ea1-165">Create a virtual network subnet configuration</span></span>
<span data-ttu-id="e4ea1-166">Vi startar genom att skapa en konfiguration av undernät för våra virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-166">We will start by creating a subnet configuration for our virtual network.</span></span> <span data-ttu-id="e4ea1-167">I våra självstudier ska vi skapar en standard-undernätet med hello [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-167">For our tutorial, we will create a default subnet using hello [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet.</span></span> <span data-ttu-id="e4ea1-168">Vi skapar våra undernätskonfiguration av virtuellt nätverk med hello namn och adress undernätsprefixet definieras med hjälp av hello variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-168">We will create our virtual network subnet configuration with hello subnet name and address prefix defined using hello variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="e4ea1-169">Du kan ange ytterligare egenskaper för hello virtuella undernät nätverkskonfigurationen med hjälp av denna cmdlet, men som ligger utanför hello i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-169">You can define additional properties of hello virtual network subnet configuration using this cmdlet, but that is beyond hello scope of this tutorial.</span></span>
>
>

<span data-ttu-id="e4ea1-170">Kör följande cmdlet toocreate hello konfigurationen av virtuellt undernät.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-170">Execute hello following cmdlet toocreate your virtual subnet configuration.</span></span>

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a><span data-ttu-id="e4ea1-171">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="e4ea1-171">Create a virtual network</span></span>
<span data-ttu-id="e4ea1-172">Därefter skapar vi vårt virtuellt nätverk med hello [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-172">Next, we will create our virtual network using hello [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet.</span></span> <span data-ttu-id="e4ea1-173">Vi skapar vårt virtuella nätverk i din nya resursgrupp med hello namn, plats och adressprefixet definierats hello variabler som du tidigare har initierat, samt använder hello undernätskonfiguration som du definierade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-173">We will create our virtual network in your new resource group, with hello name, location, and address prefix defined using hello variables that you previously initialized, and using hello subnet configuration that you defined in hello previous step.</span></span>

<span data-ttu-id="e4ea1-174">Kör följande cmdlet toocreate hello ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-174">Execute hello following cmdlet toocreate your virtual network.</span></span>

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-hello-public-ip-address"></a><span data-ttu-id="e4ea1-175">Skapa hello offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="e4ea1-175">Create hello public IP address</span></span>
<span data-ttu-id="e4ea1-176">Nu när vi har vårt virtuella nätverk som definierats måste tooconfigure en IP-adress för anslutning toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-176">Now that we have our virtual network defined, we need tooconfigure an IP address for connectivity toohello virtual machine.</span></span> <span data-ttu-id="e4ea1-177">Den här självstudiekursen skapar vi en offentlig IP-adress med hjälp av dynamisk IP-adressering toosupport Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-177">For this tutorial, we will create a public IP address using dynamic IP addressing toosupport Internet connectivity.</span></span> <span data-ttu-id="e4ea1-178">Vi använder hello [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet toocreate hello offentliga IP-adressen i hello resurs grupp skapat prevously och med hello namn, plats, fördelning och DNS-domännamnet anges med hjälp av hello variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-178">We will use hello [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet toocreate hello public IP address in hello resource group created prevously and with hello name, location, allocation method, and DNS domain name label defined using hello variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="e4ea1-179">Du kan ange ytterligare egenskaper för hello offentliga IP-adressen som använder denna cmdlet, men som ligger utanför den här första självstudien hello.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-179">You can define additional properties of hello public IP address using this cmdlet, but that is beyond hello scope of this initial tutorial.</span></span> <span data-ttu-id="e4ea1-180">Du kan också skapa en privat adress eller en adress med en statisk adress, men som även är utanför hello omfånget för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-180">You could also create a private address or an address with a static address, but that is also beyond hello scope of this tutorial.</span></span>
>
>

<span data-ttu-id="e4ea1-181">Kör följande cmdlet toocreate hello din offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-181">Execute hello following cmdlet toocreate your public IP address.</span></span>

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-hello-network-interface"></a><span data-ttu-id="e4ea1-182">Skapa hello nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="e4ea1-182">Create hello network interface</span></span>
<span data-ttu-id="e4ea1-183">Vi är nu redo toocreate hello nätverksgränssnittet som våra virtuella datorn ska använda.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-183">We are now ready toocreate hello network interface that our virtual machine will use.</span></span> <span data-ttu-id="e4ea1-184">Vi använder hello [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet toocreate våra nätverksgränssnitt i hello resursgrupp skapade tidigare och med hello namn, plats och undernät och offentliga IP-adressen har definierats.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-184">We will use hello [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet toocreate our network interface in hello resource group created earlier and with hello name, location, subnet and public IP address previously defined.</span></span>

<span data-ttu-id="e4ea1-185">Kör följande cmdlet toocreate hello nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-185">Execute hello following cmdlet toocreate your network interface.</span></span>

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a><span data-ttu-id="e4ea1-186">Konfigurera ett VM-objekt</span><span class="sxs-lookup"><span data-stu-id="e4ea1-186">Configure a VM object</span></span>
<span data-ttu-id="e4ea1-187">Nu när vi har lagring och nätverksresurser som definierats är vi klara toodefine beräkningsresurser för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-187">Now that we have storage and network resources defined, we are ready toodefine compute resources for hello virtual machine.</span></span> <span data-ttu-id="e4ea1-188">I våra självstudier ska vi ange hello storlek på virtuell dator och egenskaper för olika operativsystem, ange hello nätverksgränssnitt som vi skapade tidigare, definiera blob-lagring och ange hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-188">For our tutorial, we will specify hello virtual machine size and various operating system properties, specify hello network interface that we previously created, define blob storage, and then specify hello operating system disk.</span></span>

### <a name="create-hello-vm-object"></a><span data-ttu-id="e4ea1-189">Skapa hello VM-objekt</span><span class="sxs-lookup"><span data-stu-id="e4ea1-189">Create hello VM object</span></span>
<span data-ttu-id="e4ea1-190">Vi startar genom att ange hello storlek på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-190">We will start by specifying hello virtual machine size.</span></span> <span data-ttu-id="e4ea1-191">Den här självstudiekursen kommer specificerar vi en DS13.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-191">For this tutorial, we are specifying a DS13.</span></span> <span data-ttu-id="e4ea1-192">Vi använder hello [ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet toocreate en konfigurerbar virtuella datorobjektet med hello namn och storlek som definieras med hjälp av hello variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-192">We will use hello [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet toocreate a configurable virtual machine object with hello name and size defined using hello variables that you previously initialized.</span></span>

<span data-ttu-id="e4ea1-193">Köra hello följande cmdlet toocreate hello virtuella datorobjekt.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-193">Execute hello following cmdlet toocreate hello virtual machine object.</span></span>

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-toohold-hello-name-and-password-for-hello-local-administrator-credentials"></a><span data-ttu-id="e4ea1-194">Skapa en autentiseringsuppgift toohold hello namn och lösenord för hello lokal administratörsbehörighet</span><span class="sxs-lookup"><span data-stu-id="e4ea1-194">Create a credential object toohold hello name and password for hello local administrator credentials</span></span>
<span data-ttu-id="e4ea1-195">Innan vi kan ange hello operativsystemet egenskaper för hello virtuell dator, måste toosupply hello autentiseringsuppgifter för hello lokala administratörskontot som en säker sträng.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-195">Before we can set hello operating system properties for hello virtual machine, we need toosupply hello credentials for hello local administrator account as a secure string.</span></span> <span data-ttu-id="e4ea1-196">tooaccomplish, kommer vi att använda hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-196">tooaccomplish this, we will use hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

<span data-ttu-id="e4ea1-197">Kör följande cmdlet hello och Skriv hello toouse namn och lösenord för hello lokala administratörskontot på hello Windows virtuell dator i hello Windows PowerShell autentiseringsuppgifter begäran-fönstret.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-197">Execute hello following cmdlet and, in hello Windows PowerShell credential request window, type hello name and password toouse for hello local administrator account in hello Windows virtual machine.</span></span>

    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."

### <a name="set-hello-operating-system-properties-for-hello-virtual-machine"></a><span data-ttu-id="e4ea1-198">Ange hello operativsystemet egenskaper för hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e4ea1-198">Set hello operating system properties for hello virtual machine</span></span>
<span data-ttu-id="e4ea1-199">Vi är nu redo tooset hello virtuella datorns operativsystem egenskaper.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-199">Now we are ready tooset hello virtual machine's operating system properties.</span></span> <span data-ttu-id="e4ea1-200">Vi använder hello [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet tooset hello typ av operativsystem som Windows, kräver hello [virtuella datorns agent](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toobe installerat, ange den hello kan automatiskt Uppdatera och ange hello virtuellt datornamn, hello datornamn och hello autentiseringsuppgifter med hjälp av hello variabler som du tidigare har initierat.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-200">We will use hello [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet tooset hello type of operating system as Windows, require hello [virtual machine agent](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toobe installed, specify that hello cmdlet enables auto update and set hello virtual machine name, hello computer name, and hello credential using hello variables that you previously initialized.</span></span>

<span data-ttu-id="e4ea1-201">Köra hello följande cmdlet tooset hello operativsystemet egenskaper för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-201">Execute hello following cmdlet tooset hello operating system properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-hello-network-interface-toohello-virtual-machine"></a><span data-ttu-id="e4ea1-202">Lägg till hello network interface toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e4ea1-202">Add hello network interface toohello virtual machine</span></span>
<span data-ttu-id="e4ea1-203">Därefter kommer vi lägga till hello nätverksgränssnitt som vi skapade tidigare toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-203">Next, we will add hello network interface that we created previously toohello virtual machine.</span></span> <span data-ttu-id="e4ea1-204">Vi använder hello [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet tooadd hello nätverksgränssnitt med hello network interface variabel som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-204">We will use hello [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet tooadd hello network interface using hello network interface variable that you defined earlier.</span></span>

<span data-ttu-id="e4ea1-205">Köra hello följande cmdlet tooset hello nätverksgränssnitt för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-205">Execute hello following cmdlet tooset hello network interface for your virtual machine.</span></span>

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-hello-blob-storage-location-for-hello-disk-toobe-used-by-hello-virtual-machine"></a><span data-ttu-id="e4ea1-206">Ange hello blob-lagringsplats för hello disk toobe används av hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e4ea1-206">Set hello blob storage location for hello disk toobe used by hello virtual machine</span></span>
<span data-ttu-id="e4ea1-207">Vi kommer sedan att ange hello blob-lagringsplats för hello disk toobe används av hello virtuell dator med hello variabler som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-207">Next, we will set hello blob storage location for hello disk toobe used by hello virtual machine using hello variables that you defined earlier.</span></span>

<span data-ttu-id="e4ea1-208">Köra hello följande cmdlet tooset hello blob-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-208">Execute hello following cmdlet tooset hello blob storage location.</span></span>

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-hello-operating-system-disk-properties-for-hello-virtual-machine"></a><span data-ttu-id="e4ea1-209">Ange hello operativsystemet diskegenskaper för hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e4ea1-209">Set hello operating system disk properties for hello virtual machine</span></span>
<span data-ttu-id="e4ea1-210">Vi kommer därefter ange hello operativsystemet diskegenskaper för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-210">Next, we will set hello operating system disk properties for hello virtual machine.</span></span> <span data-ttu-id="e4ea1-211">Vi använder hello [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet toospecify som hello operativsystemet för hello virtuella datorn kommer från en avbildning, tooset cachelagring tooread endast (eftersom SQL Server installeras på hello samma disk) och definiera hello virtuellt datornamn och hello operativsystemdisk definieras med hjälp av hello variabler som vi definierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-211">We will use hello [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet toospecify that hello operating system for hello virtual machine will come from an image, tooset caching tooread only (because SQL Server is being installed on hello same disk) and define hello virtual machine name and hello operating system disk defined using hello variables that we defined earlier.</span></span>

<span data-ttu-id="e4ea1-212">Köra hello följande cmdlet tooset hello operativsystemet diskegenskaper för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-212">Execute hello following cmdlet tooset hello operating system disk properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-hello-platform-image-for-hello-virtual-machine"></a><span data-ttu-id="e4ea1-213">Ange hello plattformsavbildning för hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e4ea1-213">Specify hello platform image for hello virtual machine</span></span>
<span data-ttu-id="e4ea1-214">Vår senaste konfigurationssteget är toospecify hello plattformsavbildning för våra virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-214">Our last configuration step is toospecify hello platform image for our virtual machine.</span></span> <span data-ttu-id="e4ea1-215">I våra självstudier ska använder vi hello senaste SQL Server 2016 CTP-bild.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-215">For our tutorial, we are using hello latest SQL Server 2016 CTP image.</span></span> <span data-ttu-id="e4ea1-216">Vi använder hello [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet toouse avbildningen som definieras av hello variabler som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-216">We will use hello [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet toouse this image as defined by hello variables that you defined earlier.</span></span>

<span data-ttu-id="e4ea1-217">Köra hello följande cmdlet toospecify hello plattformsavbildning för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-217">Execute hello following cmdlet toospecify hello platform image for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-hello-sql-vm"></a><span data-ttu-id="e4ea1-218">Skapa hello SQL VM</span><span class="sxs-lookup"><span data-stu-id="e4ea1-218">Create hello SQL VM</span></span>
<span data-ttu-id="e4ea1-219">Nu när du är klar hello konfigurationssteg är klara toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-219">Now that you have finished hello configuration steps, you are ready toocreate hello virtual machine.</span></span> <span data-ttu-id="e4ea1-220">Vi använder hello [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet toocreate hello virtuell dator med hello variabler som vi har definierat.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-220">We will use hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet toocreate hello virtual machine using hello variables that we have defined.</span></span>

<span data-ttu-id="e4ea1-221">Kör följande cmdlet toocreate hello den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-221">Execute hello following cmdlet toocreate your virtual machine.</span></span>

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

<span data-ttu-id="e4ea1-222">hello virtuell dator skapas.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-222">hello virtual machine is created.</span></span> <span data-ttu-id="e4ea1-223">Observera att skapas ett standardlagringskonto för startdiagnostikinställningar eftersom hello har angetts för hello virtuella datorer är ett premiumlagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-223">Notice that a standard storage account is created for boot diagnostics because hello specified storage account for hello virtual machine's disk is a premium storage account.</span></span>

<span data-ttu-id="e4ea1-224">Du kan nu visa den här datorn i hello Azure Portal toosee [offentliga IP-adressen och dess fullständigt kvalificerade domännamnet](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="e4ea1-224">You can now view this machine in hello Azure Portal toosee [its public IP address and its fully qualified domain name](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

## <a name="example-script"></a><span data-ttu-id="e4ea1-225">Exempelskriptet</span><span class="sxs-lookup"><span data-stu-id="e4ea1-225">Example script</span></span>
<span data-ttu-id="e4ea1-226">hello innehåller följande skript hello fullständig PowerShell-skript för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-226">hello following script contains hello complete PowerShell script for this tutorial.</span></span> <span data-ttu-id="e4ea1-227">Det förutsätts att du redan har installationen hello Azure-prenumeration toouse med hello **Add-AzureRmAccount** och **Select-AzureRmSubscription** kommandon.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-227">It assumes that you have already setup hello Azure subscription toouse with hello **Add-AzureRmAccount** and **Select-AzureRmSubscription** commands.</span></span>

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
    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create hello VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a><span data-ttu-id="e4ea1-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4ea1-228">Next steps</span></span>
<span data-ttu-id="e4ea1-229">När hello virtuell dator har skapats är du redo tooconnect toohello virtuell dator med installation och RDP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="e4ea1-229">After hello virtual machine is created, you are ready tooconnect toohello virtual machine using RDP and setup connectivity.</span></span> <span data-ttu-id="e4ea1-230">Mer information finns i [ansluta tooa SQL Server-dator i Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).</span><span class="sxs-lookup"><span data-stu-id="e4ea1-230">For more information, see [Connect tooa SQL Server Virtual Machine on Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).</span></span>

