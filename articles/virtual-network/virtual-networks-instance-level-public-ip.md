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
# <a name="instance-level-public-ip-classic-overview"></a><span data-ttu-id="0ff83-103">Instansen offentlig IP (klassisk): översikt</span><span class="sxs-lookup"><span data-stu-id="0ff83-103">Instance level public IP (Classic) overview</span></span>
<span data-ttu-id="0ff83-104">En instans i nivån offentliga IP-går är offentlig IP-adress som du kan tilldela direkt tooa VM eller molntjänster rollinstansen snarare än toohello molnbaserad tjänst som din Virtuella eller roll instans finns i.</span><span class="sxs-lookup"><span data-stu-id="0ff83-104">An instance level public IP (ILPIP) is a public IP address that you can assign directly tooa VM or Cloud Services role instance, rather than toohello cloud service that your VM or role instance reside in.</span></span> <span data-ttu-id="0ff83-105">En går ske inte hello av hello virtuell IP-adress (VIP) som är tilldelad tooyour Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ff83-105">An ILPIP doesn’t take hello place of hello virtual IP (VIP) that is assigned tooyour cloud service.</span></span> <span data-ttu-id="0ff83-106">Det är en ytterligare IP-adress som du kan använda tooconnect direkt tooyour VM eller roll-instans.</span><span class="sxs-lookup"><span data-stu-id="0ff83-106">Rather, it’s an additional IP address that you can use tooconnect directly tooyour VM or role instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0ff83-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0ff83-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="0ff83-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="0ff83-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="0ff83-109">Microsoft rekommenderar att skapa virtuella datorer via Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0ff83-109">Microsoft recommends creating VMs through Resource Manager.</span></span> <span data-ttu-id="0ff83-110">Kontrollera att du förstår hur [IP-adresser](virtual-network-ip-addresses-overview-classic.md) fungerar i Azure.</span><span class="sxs-lookup"><span data-stu-id="0ff83-110">Make sure you understand how [IP addresses](virtual-network-ip-addresses-overview-classic.md) work in Azure.</span></span>

![Skillnaden mellan att går och VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

<span data-ttu-id="0ff83-112">Som visas i bild 1 hello Molntjänsten används med en VIP, medan hello enskilda virtuella datorer vanligtvis hämtas med hjälp av VIP:&lt;portnummer&gt;.</span><span class="sxs-lookup"><span data-stu-id="0ff83-112">As shown in Figure 1, hello cloud service is accessed using a VIP, while hello individual VMs are normally accessed using VIP:&lt;port number&gt;.</span></span> <span data-ttu-id="0ff83-113">Genom att tilldela en tooa går åt specifik VM som virtuell dator kan vara direkt med den IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="0ff83-113">By assigning an ILPIP tooa specific VM, that VM can be accessed directly using that IP address.</span></span>

<span data-ttu-id="0ff83-114">När du skapar en molnbaserad tjänst i Azure skapas motsvarande DNS A-poster automatiskt tooallow access toohello-tjänsten via ett fullständigt kvalificerat domännamn (FQDN), istället för att använda hello faktiska VIP.</span><span class="sxs-lookup"><span data-stu-id="0ff83-114">When you create a cloud service in Azure, corresponding DNS A records are created automatically tooallow access toohello service through a fully qualified domain name (FQDN), instead of using hello actual VIP.</span></span> <span data-ttu-id="0ff83-115">hello samma process som sker för en går att tillåta åtkomst toohello VM eller roll instans av FQDN i stället för hello går.</span><span class="sxs-lookup"><span data-stu-id="0ff83-115">hello same process happens for an ILPIP, allowing access toohello VM or role instance by FQDN instead of hello ILPIP.</span></span> <span data-ttu-id="0ff83-116">Till exempel om du skapar en molntjänst med namnet *contosoadservice*, och du konfigurerar en webbroll med namnet *contosoweb* med två instanser Azure registren hello efter en registrerar för hello instanser:</span><span class="sxs-lookup"><span data-stu-id="0ff83-116">For instance, if you create a cloud service named *contosoadservice*, and you configure a web role named *contosoweb* with two instances, Azure registers hello following A records for hello instances:</span></span>

* <span data-ttu-id="0ff83-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="0ff83-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span></span>
* <span data-ttu-id="0ff83-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="0ff83-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span></span> 

> [!NOTE]
> <span data-ttu-id="0ff83-119">Du kan tilldela en enda går för varje virtuell dator eller roll-instans.</span><span class="sxs-lookup"><span data-stu-id="0ff83-119">You can assign only one ILPIP for each VM or role instance.</span></span> <span data-ttu-id="0ff83-120">Du kan använda upp too5 ILPIPs per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0ff83-120">You can use up too5 ILPIPs per subscription.</span></span> <span data-ttu-id="0ff83-121">ILPIPs stöds inte för virtuella datorer som flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="0ff83-121">ILPIPs are not supported for multi-NIC VMs.</span></span>
> 
> 

## <a name="why-would-i-request-an-ilpip"></a><span data-ttu-id="0ff83-122">Varför skulle jag för att begära en går?</span><span class="sxs-lookup"><span data-stu-id="0ff83-122">Why would I request an ILPIP?</span></span>
<span data-ttu-id="0ff83-123">Om du vill toobe kan tooconnect tooyour VM eller rollinstansen av en IP-adress tilldelas direkt tooit, i stället för via hello molnet tjänsten VIP:&lt;portnummer&gt;, begär en går för din virtuella dator eller din rollinstans.</span><span class="sxs-lookup"><span data-stu-id="0ff83-123">If you want toobe able tooconnect tooyour VM or role instance by an IP address assigned directly tooit, rather than using hello cloud service VIP:&lt;port number&gt;, request an ILPIP for your VM or your role instance.</span></span>

* <span data-ttu-id="0ff83-124">**Aktiva FTP** -genom att tilldela en går tooa VM, kan den ta emot trafik på alla portar.</span><span class="sxs-lookup"><span data-stu-id="0ff83-124">**Active FTP** - By assigning an ILPIP tooa VM, it can receive traffic on any port.</span></span> <span data-ttu-id="0ff83-125">Slutpunkter är inte obligatoriska för hello VM tooreceive trafik.</span><span class="sxs-lookup"><span data-stu-id="0ff83-125">Endpoints are not required for hello VM tooreceive traffic.</span></span>  <span data-ttu-id="0ff83-126">Mer information på hello FTP-protokollet finns i (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [FTP-Protokollöversikt].</span><span class="sxs-lookup"><span data-stu-id="0ff83-126">See (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview)[FTP Protocol Overview] for details on hello FTP protocol.</span></span>
* <span data-ttu-id="0ff83-127">**Utgående IP** – utgående trafik från hello VM är mappad toohello går som hello källa och hello går identifierar hello VM tooexternal entiteter.</span><span class="sxs-lookup"><span data-stu-id="0ff83-127">**Outbound IP** - Outbound traffic originating from hello VM is mapped toohello ILPIP as hello source and hello ILPIP uniquely identifies hello VM tooexternal entities.</span></span>

> [!NOTE]
> <span data-ttu-id="0ff83-128">I tidigare hello var en går adress enligt tooas en offentlig IP (PIP)-adress.</span><span class="sxs-lookup"><span data-stu-id="0ff83-128">In hello past, an ILPIP address was referred tooas a public IP (PIP) address.</span></span>
> 

## <a name="manage-an-ilpip-for-a-vm"></a><span data-ttu-id="0ff83-129">Hantera en går för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0ff83-129">Manage an ILPIP for a VM</span></span>
<span data-ttu-id="0ff83-130">hello följande aktiviteter kan du toocreate, tilldela och ta bort ILPIPs från virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="0ff83-130">hello following tasks enable you toocreate, assign, and remove ILPIPs from VMs:</span></span>

### <a name="how-toorequest-an-ilpip-during-vm-creation-using-powershell"></a><span data-ttu-id="0ff83-131">Hur toorequest en går under Skapa en virtuell dator med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="0ff83-131">How toorequest an ILPIP during VM creation using PowerShell</span></span>
<span data-ttu-id="0ff83-132">hello följande PowerShell-skript skapar en molntjänst med namnet *FTPService*, hämtar en avbildning från Azure, skapar en virtuell dator med namnet *FTPInstance* med hello Hämta bild anger hello VM toouse en går och lägger till hello VM toohello ny tjänst:</span><span class="sxs-lookup"><span data-stu-id="0ff83-132">hello following PowerShell script creates a cloud service named *FTPService*, retrieves an image from Azure, creates a VM named *FTPInstance* using hello retrieved image, sets hello VM toouse an ILPIP, and adds hello VM toohello new service:</span></span>

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-tooretrieve-ilpip-information-for-a-vm"></a><span data-ttu-id="0ff83-133">Hur tooretrieve går information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0ff83-133">How tooretrieve ILPIP information for a VM</span></span>
<span data-ttu-id="0ff83-134">tooview hello går informationen för hello skapas den virtuella datorn med hello föregående skript, kör följande PowerShell-kommando hello och notera hello värden för *PublicIPAddress* och *PublicIPName*:</span><span class="sxs-lookup"><span data-stu-id="0ff83-134">tooview hello ILPIP information for hello VM created with hello previous script, run hello following PowerShell command and observe hello values for *PublicIPAddress* and *PublicIPName*:</span></span>

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

<span data-ttu-id="0ff83-135">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="0ff83-135">Expected output:</span></span>
 
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

### <a name="how-tooremove-an-ilpip-from-a-vm"></a><span data-ttu-id="0ff83-136">Hur tooremove en går från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0ff83-136">How tooremove an ILPIP from a VM</span></span>
<span data-ttu-id="0ff83-137">tooremove hello går läggas till toohello VM i hello föregående skript och köras hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="0ff83-137">tooremove hello ILPIP added toohello VM in hello previous script, run hello following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-tooadd-an-ilpip-tooan-existing-vm"></a><span data-ttu-id="0ff83-138">Hur tooadd en går tooan befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0ff83-138">How tooadd an ILPIP tooan existing VM</span></span>
<span data-ttu-id="0ff83-139">tooadd en går toohello VM som skapats med hjälp av hello skript tidigare, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ff83-139">tooadd an ILPIP toohello VM created using hello script previous, run hello following command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a><span data-ttu-id="0ff83-140">Hantera en går för en roll Cloud Services-instans</span><span class="sxs-lookup"><span data-stu-id="0ff83-140">Manage an ILPIP for a Cloud Services role instance</span></span>

<span data-ttu-id="0ff83-141">tooadd en går tooa molntjänster rollinstans, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0ff83-141">tooadd an ILPIP tooa Cloud Services role instance, complete hello following steps:</span></span>

1. <span data-ttu-id="0ff83-142">Hämta hello .cscfg-filen för hello molntjänst genom att slutföra hello stegen i hello [hur tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.</span><span class="sxs-lookup"><span data-stu-id="0ff83-142">Download hello .cscfg file for hello cloud service by completing hello steps in hello [How tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>
2. <span data-ttu-id="0ff83-143">Uppdatera hello .cscfg-filen genom att lägga till hello `InstanceAddress` element.</span><span class="sxs-lookup"><span data-stu-id="0ff83-143">Update hello .cscfg file by adding hello `InstanceAddress` element.</span></span> <span data-ttu-id="0ff83-144">hello följande exempel lägger till en går med namnet *MyPublicIP* tooa rollinstansen med namnet *WebRole1*:</span><span class="sxs-lookup"><span data-stu-id="0ff83-144">hello following sample adds an ILPIP named *MyPublicIP* tooa role instance named *WebRole1*:</span></span> 

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
3. <span data-ttu-id="0ff83-145">Överför hello .cscfg-filen för hello molntjänst genom att slutföra hello stegen i hello [hur tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.</span><span class="sxs-lookup"><span data-stu-id="0ff83-145">Upload hello .cscfg file for hello cloud service by completing hello steps in hello [How tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ff83-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ff83-146">Next steps</span></span>
* <span data-ttu-id="0ff83-147">Förstå hur [IP-adressering](virtual-network-ip-addresses-overview-classic.md) fungerar i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="0ff83-147">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in hello classic deployment model.</span></span>
* <span data-ttu-id="0ff83-148">Lär dig mer om [reserverade IP-adresser](virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="0ff83-148">Learn about [Reserved IPs](virtual-networks-reserved-public-ip.md).</span></span>
