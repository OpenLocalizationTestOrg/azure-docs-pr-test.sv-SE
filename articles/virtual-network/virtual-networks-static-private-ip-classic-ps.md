---
title: "aaaConfigure privata IP-adresser för virtuella datorer (klassisk) - Azure PowerShell | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer (klassisk) med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 60c7b489-46ae-48af-a453-2b429a474afd
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99546ee9c2c4eb9aa7b67f30721d37ef9b2944f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-powershell"></a><span data-ttu-id="37be2-103">Konfigurera privat IP-adresser för en virtuell dator (klassisk) med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="37be2-103">Configure private IP addresses for a virtual machine (Classic) using PowerShell</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="37be2-104">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="37be2-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="37be2-105">Du kan också [hantera en statisk privat IP-adress i hello Resource Manager-distributionsmodellen](virtual-networks-static-private-ip-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="37be2-105">You can also [manage a static private IP address in hello Resource Manager deployment model](virtual-networks-static-private-ip-arm-ps.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="37be2-106">hello exempel PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="37be2-106">hello sample PowerShell commands below expect a simple environment already created.</span></span> <span data-ttu-id="37be2-107">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett VNet](virtual-networks-create-vnet-classic-netcfg-ps.md).</span><span class="sxs-lookup"><span data-stu-id="37be2-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [Create a VNet](virtual-networks-create-vnet-classic-netcfg-ps.md).</span></span>

## <a name="how-tooverify-if-a-specific-ip-address-is-available"></a><span data-ttu-id="37be2-108">Hur tooverify om en specifik IP-adress är tillgänglig</span><span class="sxs-lookup"><span data-stu-id="37be2-108">How tooverify if a specific IP address is available</span></span>
<span data-ttu-id="37be2-109">tooverify om hello IP-adress *192.168.1.101* finns i ett VNet med namnet *TestVNet*, kör följande PowerShell-kommando hello och verifiera hello värde för *IsAvailable*:</span><span class="sxs-lookup"><span data-stu-id="37be2-109">tooverify if hello IP address *192.168.1.101* is available in a VNet named *TestVNet*, run hello following PowerShell command and verify hello value for *IsAvailable*:</span></span>

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

<span data-ttu-id="37be2-110">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="37be2-110">Expected output:</span></span>

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="37be2-111">Hur toospecify en statisk privat IP-adress när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="37be2-111">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="37be2-112">hello PowerShell-skriptet nedan skapar en ny molntjänst med namnet *TestService*, hämtar sedan en avbildning från Azure, skapar en virtuell dator med namnet *DNS01* i hello ny molntjänst med hello Hämta bild, ställer in Hej VM toobe i ett undernät med namnet *klientdel*, och anger *192.168.1.7* som en statisk privat IP-adress för hello VM:</span><span class="sxs-lookup"><span data-stu-id="37be2-112">hello PowerShell script below creates a new cloud service named *TestService*, then retrieves an image from Azure, creates a VM named *DNS01* in hello new cloud service using hello retrieved image, sets hello VM toobe in a subnet named *FrontEnd*, and sets *192.168.1.7* as a static private IP address for hello VM:</span></span>

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

<span data-ttu-id="37be2-113">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="37be2-113">Expected output:</span></span>

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="37be2-114">Hur tooretrieve statiska privata IP-information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="37be2-114">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="37be2-115">tooview hello statisk privat IP-information för hello skapas den virtuella datorn med hello skriptet ovan, kör följande PowerShell-kommando hello och se hello värden för *IpAddress*:</span><span class="sxs-lookup"><span data-stu-id="37be2-115">tooview hello static private IP address information for hello VM created with hello script above, run hello following PowerShell command and observe hello values for *IpAddress*:</span></span>

    Get-AzureVM -Name DNS01 -ServiceName TestService

<span data-ttu-id="37be2-116">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="37be2-116">Expected output:</span></span>

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="37be2-117">Hur tooremove en statisk privat IP-adress från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="37be2-117">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="37be2-118">tooremove hello statisk privat IP-adress läggs toohello VM i hello skriptet ovan, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="37be2-118">tooremove hello static private IP address added toohello VM in hello script above, run hello following PowerShell command:</span></span>

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

<span data-ttu-id="37be2-119">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="37be2-119">Expected output:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="37be2-120">Hur tooadd en statisk privat IP-adress tooan befintliga VM</span><span class="sxs-lookup"><span data-stu-id="37be2-120">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="37be2-121">tooadd en statisk privat IP-adress toohello VM som skapats med hjälp av hello skriptet ovan runt han följande kommando:</span><span class="sxs-lookup"><span data-stu-id="37be2-121">tooadd a static private IP address toohello VM created using hello script above, runt he following command:</span></span>

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

<span data-ttu-id="37be2-122">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="37be2-122">Expected output:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a><span data-ttu-id="37be2-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37be2-123">Next steps</span></span>
* <span data-ttu-id="37be2-124">Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="37be2-124">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="37be2-125">Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="37be2-125">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="37be2-126">Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="37be2-126">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

