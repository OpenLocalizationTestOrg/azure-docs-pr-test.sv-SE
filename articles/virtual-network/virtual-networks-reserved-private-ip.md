---
title: Statiska interna privata IP - Azure VM - klassisk
description: "Förstå statiska interna IP-adresser (korta) och hur du hanterar dem."
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
ms.openlocfilehash: cf9ee59ca4e44ed01836c2efb1f4df5f073bf6e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a><span data-ttu-id="67961-103">Hur du ställer in en statisk internt privat IP-adress med hjälp av PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="67961-103">How to set a static internal private IP address using PowerShell (Classic)</span></span>
<span data-ttu-id="67961-104">I de flesta fall behöver du inte ange en statisk interna IP-adress för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="67961-104">In most cases, you won’t need to specify a static internal IP address for your virtual machine.</span></span> <span data-ttu-id="67961-105">Virtuella datorer i ett virtuellt nätverk får automatiskt en intern IP-adress från det intervall som du anger.</span><span class="sxs-lookup"><span data-stu-id="67961-105">VMs in a virtual network will automatically receive an internal IP address from a range that you specify.</span></span> <span data-ttu-id="67961-106">Men i vissa fall, ange en statisk IP-adress för en viss virtuell dator är meningsfullt.</span><span class="sxs-lookup"><span data-stu-id="67961-106">But in certain cases, specifying a static IP address for a particular VM makes sense.</span></span> <span data-ttu-id="67961-107">Till exempel om den virtuella datorn kommer att köra DNS eller om en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="67961-107">For example, if your VM is going to run DNS or will be a domain controller.</span></span> <span data-ttu-id="67961-108">En statiska interna IP-adressen förblir med den virtuella datorn även via ett stoppa/avetablering tillstånd.</span><span class="sxs-lookup"><span data-stu-id="67961-108">A static internal IP address stays with the VM even through a stop/deprovision state.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="67961-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="67961-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="67961-110">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="67961-110">This article covers using the classic deployment model.</span></span> <span data-ttu-id="67961-111">Microsoft rekommenderar att de flesta nya distributioner använder den [Resource Manager-distributionsmodellen](virtual-networks-static-private-ip-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="67961-111">Microsoft recommends that most new deployments use the [Resource Manager deployment model](virtual-networks-static-private-ip-arm-ps.md).</span></span>
> 
> 

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a><span data-ttu-id="67961-112">Så här kontrollerar du om det finns en specifik IP-adress</span><span class="sxs-lookup"><span data-stu-id="67961-112">How to verify if a specific IP address is available</span></span>
<span data-ttu-id="67961-113">Kontrollera om IP-adressen *10.0.0.7* finns i ett vnet med namnet *TestVnet*, kör följande PowerShell-kommandot och kontrollera värdet för *IsAvailable*:</span><span class="sxs-lookup"><span data-stu-id="67961-113">To verify if the IP address *10.0.0.7* is available in a vnet named *TestVnet*, run the following PowerShell command and verify the value for *IsAvailable*:</span></span>

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> <span data-ttu-id="67961-114">Om du vill testa kommandot ovan i en säker miljö följa riktlinjerna i [skapa ett virtuellt nätverk (klassiska)](virtual-networks-create-vnet-classic-pportal.md) skapa ett vnet med namnet *TestVnet* och se till att den använder den *10.0.0.0/8*  adressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="67961-114">If you want to test the command above in a safe environment follow the guidelines in [Create a virtual network (classic)](virtual-networks-create-vnet-classic-pportal.md) to create a vnet named *TestVnet* and ensure it uses the *10.0.0.0/8* address space.</span></span>
> 
> 

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a><span data-ttu-id="67961-115">Så här anger du en statiska interna IP-adress när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="67961-115">How to specify a static internal IP when creating a VM</span></span>
<span data-ttu-id="67961-116">PowerShell-skriptet nedan skapar en ny molntjänst med namnet *TestService*, hämtar en avbildning från Azure sedan skapar en virtuell dator med namnet *TestVM* i nya Molntjänsten med hämtade bild, ställer in den Virtuell dator i ett undernät med namnet *undernät 1*, och anger *10.0.0.7* som en statiska interna IP-adress för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="67961-116">The PowerShell script below creates a new cloud service named *TestService*, then retrieves an image from Azure, then creates a VM named *TestVM* in the new cloud service using the retrieved image, sets the VM to be in a subnet named *Subnet-1*, and sets *10.0.0.7* as a static internal IP for the VM:</span></span>

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a><span data-ttu-id="67961-117">Hur du hämtar statiska interna IP-information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="67961-117">How to retrieve static internal IP information for a VM</span></span>
<span data-ttu-id="67961-118">Kör följande PowerShell-kommando för att visa statiska interna IP-information för den virtuella datorn skapas med skriptet ovan, och notera att värdena för *IpAddress*:</span><span class="sxs-lookup"><span data-stu-id="67961-118">To view the static internal IP information for the VM created with the script above, run the following PowerShell command and observe the values for *IpAddress*:</span></span>

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

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a><span data-ttu-id="67961-119">Ta bort en statiska interna IP-adress från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="67961-119">How to remove a static internal IP from a VM</span></span>
<span data-ttu-id="67961-120">Kör följande PowerShell-kommando för att ta bort statiska interna IP-Adressen till den virtuella datorn i skriptet ovan:</span><span class="sxs-lookup"><span data-stu-id="67961-120">To remove the static internal IP added to the VM in the script above, run the following PowerShell command:</span></span>

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a><span data-ttu-id="67961-121">Lägga till en intern statisk IP på en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="67961-121">How to add a static internal IP to an existing VM</span></span>
<span data-ttu-id="67961-122">Om du vill lägga till en intern statisk skapas IP-adress med den virtuella datorn med skriptet ovan runt han följande kommando:</span><span class="sxs-lookup"><span data-stu-id="67961-122">To add a static internal IP to the VM created using the script above, runt he following command:</span></span>

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="67961-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="67961-123">Next steps</span></span>
[<span data-ttu-id="67961-124">Reserverad IP</span><span class="sxs-lookup"><span data-stu-id="67961-124">Reserved IP</span></span>](virtual-networks-reserved-public-ip.md)

[<span data-ttu-id="67961-125">Offentlig IP på instansnivå (går)</span><span class="sxs-lookup"><span data-stu-id="67961-125">Instance-Level Public IP (ILPIP)</span></span>](virtual-networks-instance-level-public-ip.md)

[<span data-ttu-id="67961-126">Reserverad IP REST API: er</span><span class="sxs-lookup"><span data-stu-id="67961-126">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)

