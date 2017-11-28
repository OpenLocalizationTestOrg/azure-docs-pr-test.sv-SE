---
title: aaaStatic interna privata IP - Azure VM - klassisk
description: "Förstå statiska interna IP-adresser (korta) och hur toomanage dem."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 93444c6f-af1b-41f8-a035-77f5c0302bf0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.openlocfilehash: 5abe1c59f2f3ed19bcf56c269dfe57ac32d4f601
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-a-static-internal-private-ip-address-using-powershell-classic"></a><span data-ttu-id="332c8-103">Hur tooset en intern statisk privat IP-adress med hjälp av PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="332c8-103">How tooset a static internal private IP address using PowerShell (Classic)</span></span>
<span data-ttu-id="332c8-104">I de flesta fall behöver du inte toospecify statiska interna IP-adress för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="332c8-104">In most cases, you won’t need toospecify a static internal IP address for your virtual machine.</span></span> <span data-ttu-id="332c8-105">Virtuella datorer i ett virtuellt nätverk får automatiskt en intern IP-adress från det intervall som du anger.</span><span class="sxs-lookup"><span data-stu-id="332c8-105">VMs in a virtual network will automatically receive an internal IP address from a range that you specify.</span></span> <span data-ttu-id="332c8-106">Men i vissa fall, ange en statisk IP-adress för en viss virtuell dator är meningsfullt.</span><span class="sxs-lookup"><span data-stu-id="332c8-106">But in certain cases, specifying a static IP address for a particular VM makes sense.</span></span> <span data-ttu-id="332c8-107">Till exempel om den virtuella datorn är pågående toorun DNS eller kommer att vara en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="332c8-107">For example, if your VM is going toorun DNS or will be a domain controller.</span></span> <span data-ttu-id="332c8-108">En statiska interna IP-adressen förblir med hello VM även via ett stoppa/avetablering tillstånd.</span><span class="sxs-lookup"><span data-stu-id="332c8-108">A static internal IP address stays with hello VM even through a stop/deprovision state.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="332c8-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="332c8-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="332c8-110">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="332c8-110">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="332c8-111">Microsoft rekommenderar att de flesta nya distributioner använder hello [Resource Manager-distributionsmodellen](virtual-networks-static-private-ip-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="332c8-111">Microsoft recommends that most new deployments use hello [Resource Manager deployment model](virtual-networks-static-private-ip-arm-ps.md).</span></span>
> 
> 

## <a name="how-tooverify-if-a-specific-ip-address-is-available"></a><span data-ttu-id="332c8-112">Hur tooverify om en specifik IP-adress är tillgänglig</span><span class="sxs-lookup"><span data-stu-id="332c8-112">How tooverify if a specific IP address is available</span></span>
<span data-ttu-id="332c8-113">tooverify om hello IP-adress *10.0.0.7* finns i ett vnet med namnet *TestVnet*, kör följande PowerShell-kommando hello och verifiera hello värde för *IsAvailable*:</span><span class="sxs-lookup"><span data-stu-id="332c8-113">tooverify if hello IP address *10.0.0.7* is available in a vnet named *TestVnet*, run hello following PowerShell command and verify hello value for *IsAvailable*:</span></span>

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> <span data-ttu-id="332c8-114">Om du vill tootest hello kommandot ovan i en säker miljö följer hello riktlinjerna i [skapa ett virtuellt nätverk (klassiska)](virtual-networks-create-vnet-classic-pportal.md) toocreate ett vnet med namnet *TestVnet* och se till att den använder hello  *10.0.0.0/8* adressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="332c8-114">If you want tootest hello command above in a safe environment follow hello guidelines in [Create a virtual network (classic)](virtual-networks-create-vnet-classic-pportal.md) toocreate a vnet named *TestVnet* and ensure it uses hello *10.0.0.0/8* address space.</span></span>
> 
> 

## <a name="how-toospecify-a-static-internal-ip-when-creating-a-vm"></a><span data-ttu-id="332c8-115">Hur toospecify en statiska interna IP-adress när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="332c8-115">How toospecify a static internal IP when creating a VM</span></span>
<span data-ttu-id="332c8-116">hello PowerShell-skriptet nedan skapar en ny molntjänst med namnet *TestService*, hämtar en avbildning från Azure sedan skapar en virtuell dator med namnet *TestVM* i hello ny molntjänst med hello Hämta bild Anger hello VM toobe i ett undernät med namnet *undernät 1*, och anger *10.0.0.7* som en intern statisk IP för hello VM:</span><span class="sxs-lookup"><span data-stu-id="332c8-116">hello PowerShell script below creates a new cloud service named *TestService*, then retrieves an image from Azure, then creates a VM named *TestVM* in hello new cloud service using hello retrieved image, sets hello VM toobe in a subnet named *Subnet-1*, and sets *10.0.0.7* as a static internal IP for hello VM:</span></span>

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-tooretrieve-static-internal-ip-information-for-a-vm"></a><span data-ttu-id="332c8-117">Hur tooretrieve statiska interna IP-information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="332c8-117">How tooretrieve static internal IP information for a VM</span></span>
<span data-ttu-id="332c8-118">tooview hello statiska interna IP-information för hello skapas den virtuella datorn med hello skriptet ovan, kör följande PowerShell-kommando hello och notera hello värden för *IpAddress*:</span><span class="sxs-lookup"><span data-stu-id="332c8-118">tooview hello static internal IP information for hello VM created with hello script above, run hello following PowerShell command and observe hello values for *IpAddress*:</span></span>

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-tooremove-a-static-internal-ip-from-a-vm"></a><span data-ttu-id="332c8-119">Hur tooremove en statiska interna IP-adress från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="332c8-119">How tooremove a static internal IP from a VM</span></span>
<span data-ttu-id="332c8-120">tooremove hello statiska interna IP lades till toohello VM i hello skriptet ovan, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="332c8-120">tooremove hello static internal IP added toohello VM in hello script above, run hello following PowerShell command:</span></span>

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-tooadd-a-static-internal-ip-tooan-existing-vm"></a><span data-ttu-id="332c8-121">Hur tooadd en statiska interna IP-tooan befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="332c8-121">How tooadd a static internal IP tooan existing VM</span></span>
<span data-ttu-id="332c8-122">tooadd en statiska interna IP-toohello VM som skapats med hjälp av hello skriptet ovan runt han följande kommando:</span><span class="sxs-lookup"><span data-stu-id="332c8-122">tooadd a static internal IP toohello VM created using hello script above, runt he following command:</span></span>

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="332c8-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="332c8-123">Next steps</span></span>
[<span data-ttu-id="332c8-124">Reserverad IP</span><span class="sxs-lookup"><span data-stu-id="332c8-124">Reserved IP</span></span>](virtual-networks-reserved-public-ip.md)

[<span data-ttu-id="332c8-125">Offentlig IP på instansnivå (går)</span><span class="sxs-lookup"><span data-stu-id="332c8-125">Instance-Level Public IP (ILPIP)</span></span>](virtual-networks-instance-level-public-ip.md)

[<span data-ttu-id="332c8-126">Reserverad IP REST API: er</span><span class="sxs-lookup"><span data-stu-id="332c8-126">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)

