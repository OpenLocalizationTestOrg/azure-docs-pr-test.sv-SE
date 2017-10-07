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
# <a name="how-tooset-a-static-internal-private-ip-address-using-powershell-classic"></a>Hur tooset en intern statisk privat IP-adress med hjälp av PowerShell (klassisk)
I de flesta fall behöver du inte toospecify statiska interna IP-adress för den virtuella datorn. Virtuella datorer i ett virtuellt nätverk får automatiskt en intern IP-adress från det intervall som du anger. Men i vissa fall, ange en statisk IP-adress för en viss virtuell dator är meningsfullt. Till exempel om den virtuella datorn är pågående toorun DNS eller kommer att vara en domänkontrollant. En statiska interna IP-adressen förblir med hello VM även via ett stoppa/avetablering tillstånd. 

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello [Resource Manager-distributionsmodellen](virtual-networks-static-private-ip-arm-ps.md).
> 
> 

## <a name="how-tooverify-if-a-specific-ip-address-is-available"></a>Hur tooverify om en specifik IP-adress är tillgänglig
tooverify om hello IP-adress *10.0.0.7* finns i ett vnet med namnet *TestVnet*, kör följande PowerShell-kommando hello och verifiera hello värde för *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> Om du vill tootest hello kommandot ovan i en säker miljö följer hello riktlinjerna i [skapa ett virtuellt nätverk (klassiska)](virtual-networks-create-vnet-classic-pportal.md) toocreate ett vnet med namnet *TestVnet* och se till att den använder hello  *10.0.0.0/8* adressutrymmet.
> 
> 

## <a name="how-toospecify-a-static-internal-ip-when-creating-a-vm"></a>Hur toospecify en statiska interna IP-adress när du skapar en virtuell dator
hello PowerShell-skriptet nedan skapar en ny molntjänst med namnet *TestService*, hämtar en avbildning från Azure sedan skapar en virtuell dator med namnet *TestVM* i hello ny molntjänst med hello Hämta bild Anger hello VM toobe i ett undernät med namnet *undernät 1*, och anger *10.0.0.7* som en intern statisk IP för hello VM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-tooretrieve-static-internal-ip-information-for-a-vm"></a>Hur tooretrieve statiska interna IP-information för en virtuell dator
tooview hello statiska interna IP-information för hello skapas den virtuella datorn med hello skriptet ovan, kör följande PowerShell-kommando hello och notera hello värden för *IpAddress*:

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

## <a name="how-tooremove-a-static-internal-ip-from-a-vm"></a>Hur tooremove en statiska interna IP-adress från en virtuell dator
tooremove hello statiska interna IP lades till toohello VM i hello skriptet ovan, kör hello följande PowerShell-kommando:

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-tooadd-a-static-internal-ip-tooan-existing-vm"></a>Hur tooadd en statiska interna IP-tooan befintlig virtuell dator
tooadd en statiska interna IP-toohello VM som skapats med hjälp av hello skriptet ovan runt han följande kommando:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a>Nästa steg
[Reserverad IP](virtual-networks-reserved-public-ip.md)

[Offentlig IP på instansnivå (går)](virtual-networks-instance-level-public-ip.md)

[Reserverad IP REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx)

