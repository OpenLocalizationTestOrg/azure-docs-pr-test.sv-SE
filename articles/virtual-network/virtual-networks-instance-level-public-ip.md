---
title: "aaaAzure instansnivå offentlig IP-adress (klassisk)-adresser | Microsoft Docs"
description: "Förstå instans nivån offentliga IP-går adresser och hur toomanage dem med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 07eef6ec-7dfe-4c4d-a2c2-be0abfb48ec5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: 832143ee6fdd80b634e1cebfddc759a8cacda583
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="instance-level-public-ip-classic-overview"></a>Instansen offentlig IP (klassisk): översikt
En instans i nivån offentliga IP-går är offentlig IP-adress som du kan tilldela direkt tooa VM eller molntjänster rollinstansen snarare än toohello molnbaserad tjänst som din Virtuella eller roll instans finns i. En går ske inte hello av hello virtuell IP-adress (VIP) som är tilldelad tooyour Molntjänsten. Det är en ytterligare IP-adress som du kan använda tooconnect direkt tooyour VM eller roll-instans.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att skapa virtuella datorer via Resource Manager. Kontrollera att du förstår hur [IP-adresser](virtual-network-ip-addresses-overview-classic.md) fungerar i Azure.

![Skillnaden mellan att går och VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Som visas i bild 1 hello Molntjänsten används med en VIP, medan hello enskilda virtuella datorer vanligtvis hämtas med hjälp av VIP:&lt;portnummer&gt;. Genom att tilldela en tooa går åt specifik VM som virtuell dator kan vara direkt med den IP-adressen.

När du skapar en molnbaserad tjänst i Azure skapas motsvarande DNS A-poster automatiskt tooallow access toohello-tjänsten via ett fullständigt kvalificerat domännamn (FQDN), istället för att använda hello faktiska VIP. hello samma process som sker för en går att tillåta åtkomst toohello VM eller roll instans av FQDN i stället för hello går. Till exempel om du skapar en molntjänst med namnet *contosoadservice*, och du konfigurerar en webbroll med namnet *contosoweb* med två instanser Azure registren hello efter en registrerar för hello instanser:

* contosoweb\_IN_0.contosoadservice.cloudapp.net
* contosoweb\_IN_1.contosoadservice.cloudapp.net 

> [!NOTE]
> Du kan tilldela en enda går för varje virtuell dator eller roll-instans. Du kan använda upp too5 ILPIPs per prenumeration. ILPIPs stöds inte för virtuella datorer som flera nätverkskort.
> 
> 

## <a name="why-would-i-request-an-ilpip"></a>Varför skulle jag för att begära en går?
Om du vill toobe kan tooconnect tooyour VM eller rollinstansen av en IP-adress tilldelas direkt tooit, i stället för via hello molnet tjänsten VIP:&lt;portnummer&gt;, begär en går för din virtuella dator eller din rollinstans.

* **Aktiva FTP** -genom att tilldela en går tooa VM, kan den ta emot trafik på alla portar. Slutpunkter är inte obligatoriska för hello VM tooreceive trafik.  Mer information på hello FTP-protokollet finns i (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [FTP-Protokollöversikt].
* **Utgående IP** – utgående trafik från hello VM är mappad toohello går som hello källa och hello går identifierar hello VM tooexternal entiteter.

> [!NOTE]
> I tidigare hello var en går adress enligt tooas en offentlig IP (PIP)-adress.
> 

## <a name="manage-an-ilpip-for-a-vm"></a>Hantera en går för en virtuell dator
hello följande aktiviteter kan du toocreate, tilldela och ta bort ILPIPs från virtuella datorer:

### <a name="how-toorequest-an-ilpip-during-vm-creation-using-powershell"></a>Hur toorequest en går under Skapa en virtuell dator med hjälp av PowerShell
hello följande PowerShell-skript skapar en molntjänst med namnet *FTPService*, hämtar en avbildning från Azure, skapar en virtuell dator med namnet *FTPInstance* med hello Hämta bild anger hello VM toouse en går och lägger till hello VM toohello ny tjänst:

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-tooretrieve-ilpip-information-for-a-vm"></a>Hur tooretrieve går information för en virtuell dator
tooview hello går informationen för hello skapas den virtuella datorn med hello föregående skript, kör följande PowerShell-kommando hello och notera hello värden för *PublicIPAddress* och *PublicIPName*:

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

Förväntad utdata:
 
    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            :   Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

### <a name="how-tooremove-an-ilpip-from-a-vm"></a>Hur tooremove en går från en virtuell dator
tooremove hello går läggas till toohello VM i hello föregående skript och köras hello följande PowerShell-kommando:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-tooadd-an-ilpip-tooan-existing-vm"></a>Hur tooadd en går tooan befintlig virtuell dator
tooadd en går toohello VM som skapats med hjälp av hello skript tidigare, kör hello följande kommando:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a>Hantera en går för en roll Cloud Services-instans

tooadd en går tooa molntjänster rollinstans, fullständig hello följande steg:

1. Hämta hello .cscfg-filen för hello molntjänst genom att slutföra hello stegen i hello [hur tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.
2. Uppdatera hello .cscfg-filen genom att lägga till hello `InstanceAddress` element. hello följande exempel lägger till en går med namnet *MyPublicIP* tooa rollinstansen med namnet *WebRole1*: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ILPIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
          <ConfigurationSettings>
        <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
          </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <InstanceAddress roleName="WebRole1">
        <PublicIPs>
          <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. Överför hello .cscfg-filen för hello molntjänst genom att slutföra hello stegen i hello [hur tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.

## <a name="next-steps"></a>Nästa steg
* Förstå hur [IP-adressering](virtual-network-ip-addresses-overview-classic.md) fungerar i hello klassiska distributionsmodellen.
* Lär dig mer om [reserverade IP-adresser](virtual-networks-reserved-public-ip.md).
