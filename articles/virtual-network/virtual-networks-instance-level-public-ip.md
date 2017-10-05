---
title: "Azure instansnivå offentlig IP-adress (klassisk)-adresser | Microsoft Docs"
description: "Förstå instans nivån offentliga IP-går adresser och hantera dem med hjälp av PowerShell."
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
ms.openlocfilehash: 773043f2841ec7539b0d49357dec6bcb9f4f78a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="instance-level-public-ip-classic-overview"></a><span data-ttu-id="5caa9-103">Instansen offentlig IP (klassisk): översikt</span><span class="sxs-lookup"><span data-stu-id="5caa9-103">Instance level public IP (Classic) overview</span></span>
<span data-ttu-id="5caa9-104">En instans nivån offentliga IP-går är en offentlig IP-adress som kan tilldelas direkt till en virtuell dator eller molntjänster rollinstans i stället för till Molntjänsten som din Virtuella eller roll instans finns i.</span><span class="sxs-lookup"><span data-stu-id="5caa9-104">An instance level public IP (ILPIP) is a public IP address that you can assign directly to a VM or Cloud Services role instance, rather than to the cloud service that your VM or role instance reside in.</span></span> <span data-ttu-id="5caa9-105">En går äga inte rum för den virtuella IP (VIP) som är tilldelad till Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="5caa9-105">An ILPIP doesn’t take the place of the virtual IP (VIP) that is assigned to your cloud service.</span></span> <span data-ttu-id="5caa9-106">Det är en ytterligare IP-adress som du kan använda för att ansluta direkt till din Virtuella eller roll-instans.</span><span class="sxs-lookup"><span data-stu-id="5caa9-106">Rather, it’s an additional IP address that you can use to connect directly to your VM or role instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5caa9-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5caa9-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="5caa9-108">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="5caa9-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="5caa9-109">Microsoft rekommenderar att skapa virtuella datorer via Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5caa9-109">Microsoft recommends creating VMs through Resource Manager.</span></span> <span data-ttu-id="5caa9-110">Kontrollera att du förstår hur [IP-adresser](virtual-network-ip-addresses-overview-classic.md) fungerar i Azure.</span><span class="sxs-lookup"><span data-stu-id="5caa9-110">Make sure you understand how [IP addresses](virtual-network-ip-addresses-overview-classic.md) work in Azure.</span></span>

![Skillnaden mellan att går och VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

<span data-ttu-id="5caa9-112">I bild 1 visas Molntjänsten används med en VIP samtidigt som de enskilda virtuella datorerna används normalt med VIP:&lt;portnummer&gt;.</span><span class="sxs-lookup"><span data-stu-id="5caa9-112">As shown in Figure 1, the cloud service is accessed using a VIP, while the individual VMs are normally accessed using VIP:&lt;port number&gt;.</span></span> <span data-ttu-id="5caa9-113">Genom att tilldela en går till en specifik virtuell dator kan kan den virtuella datorn nås direkt med den IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="5caa9-113">By assigning an ILPIP to a specific VM, that VM can be accessed directly using that IP address.</span></span>

<span data-ttu-id="5caa9-114">När du skapar en molnbaserad tjänst i Azure skapas motsvarande DNS A-poster automatiskt för att tillåta åtkomst till tjänsten via ett fullständigt kvalificerat domännamn (FQDN), i stället för verkliga VIP.</span><span class="sxs-lookup"><span data-stu-id="5caa9-114">When you create a cloud service in Azure, corresponding DNS A records are created automatically to allow access to the service through a fully qualified domain name (FQDN), instead of using the actual VIP.</span></span> <span data-ttu-id="5caa9-115">Samma process som sker för en går att tillåta åtkomst till virtuell dator eller roll-instansen av FQDN i stället för i går.</span><span class="sxs-lookup"><span data-stu-id="5caa9-115">The same process happens for an ILPIP, allowing access to the VM or role instance by FQDN instead of the ILPIP.</span></span> <span data-ttu-id="5caa9-116">Till exempel om du skapar en molntjänst med namnet *contosoadservice*, och du konfigurerar en webbroll med namnet *contosoweb* med två instanser Azure registrerar följande A-poster för instanserna:</span><span class="sxs-lookup"><span data-stu-id="5caa9-116">For instance, if you create a cloud service named *contosoadservice*, and you configure a web role named *contosoweb* with two instances, Azure registers the following A records for the instances:</span></span>

* <span data-ttu-id="5caa9-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="5caa9-117">contosoweb\_IN_0.contosoadservice.cloudapp.net</span></span>
* <span data-ttu-id="5caa9-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="5caa9-118">contosoweb\_IN_1.contosoadservice.cloudapp.net</span></span> 

> [!NOTE]
> <span data-ttu-id="5caa9-119">Du kan tilldela en enda går för varje virtuell dator eller roll-instans.</span><span class="sxs-lookup"><span data-stu-id="5caa9-119">You can assign only one ILPIP for each VM or role instance.</span></span> <span data-ttu-id="5caa9-120">Du kan använda upp till 5 ILPIPs per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5caa9-120">You can use up to 5 ILPIPs per subscription.</span></span> <span data-ttu-id="5caa9-121">ILPIPs stöds inte för virtuella datorer som flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="5caa9-121">ILPIPs are not supported for multi-NIC VMs.</span></span>
> 
> 

## <a name="why-would-i-request-an-ilpip"></a><span data-ttu-id="5caa9-122">Varför skulle jag för att begära en går?</span><span class="sxs-lookup"><span data-stu-id="5caa9-122">Why would I request an ILPIP?</span></span>
<span data-ttu-id="5caa9-123">Om du vill kunna ansluta till din Virtuella eller roll-instans med en IP-adress som tilldelats till den i stället för att använda molnet tjänsten VIP:&lt;portnummer&gt;, begär en går för din virtuella dator eller din rollinstans.</span><span class="sxs-lookup"><span data-stu-id="5caa9-123">If you want to be able to connect to your VM or role instance by an IP address assigned directly to it, rather than using the cloud service VIP:&lt;port number&gt;, request an ILPIP for your VM or your role instance.</span></span>

* <span data-ttu-id="5caa9-124">**Aktiva FTP** -genom att tilldela en går till en virtuell dator kan den ta emot trafik på alla portar.</span><span class="sxs-lookup"><span data-stu-id="5caa9-124">**Active FTP** - By assigning an ILPIP to a VM, it can receive traffic on any port.</span></span> <span data-ttu-id="5caa9-125">Slutpunkter krävs inte för den virtuella datorn tar emot trafik.</span><span class="sxs-lookup"><span data-stu-id="5caa9-125">Endpoints are not required for the VM to receive traffic.</span></span>  <span data-ttu-id="5caa9-126">Information om FTP-protokollet finns i (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [FTP-Protokollöversikt].</span><span class="sxs-lookup"><span data-stu-id="5caa9-126">See (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview)[FTP Protocol Overview] for details on the FTP protocol.</span></span>
* <span data-ttu-id="5caa9-127">**Utgående IP** – utgående trafik från den virtuella datorn är mappad till går som källa och går identifierar den virtuella datorn till externa enheter.</span><span class="sxs-lookup"><span data-stu-id="5caa9-127">**Outbound IP** - Outbound traffic originating from the VM is mapped to the ILPIP as the source and the ILPIP uniquely identifies the VM to external entities.</span></span>

> [!NOTE]
> <span data-ttu-id="5caa9-128">Tidigare som går-adress kallas en offentlig IP (PIP)-adress.</span><span class="sxs-lookup"><span data-stu-id="5caa9-128">In the past, an ILPIP address was referred to as a public IP (PIP) address.</span></span>
> 

## <a name="manage-an-ilpip-for-a-vm"></a><span data-ttu-id="5caa9-129">Hantera en går för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5caa9-129">Manage an ILPIP for a VM</span></span>
<span data-ttu-id="5caa9-130">Följande aktiviteter kan du skapa, tilldela och ta bort ILPIPs från virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="5caa9-130">The following tasks enable you to create, assign, and remove ILPIPs from VMs:</span></span>

### <a name="how-to-request-an-ilpip-during-vm-creation-using-powershell"></a><span data-ttu-id="5caa9-131">Hur du begär ett går under Skapa en virtuell dator med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="5caa9-131">How to request an ILPIP during VM creation using PowerShell</span></span>
<span data-ttu-id="5caa9-132">Följande PowerShell-skript skapar en molntjänst med namnet *FTPService*, hämtar en avbildning från Azure, skapar en virtuell dator med namnet *FTPInstance* använder den hämtade avbildningen anger den virtuella datorn att använda en går och lägger till den virtuella datorn till den nya tjänsten:</span><span class="sxs-lookup"><span data-stu-id="5caa9-132">The following PowerShell script creates a cloud service named *FTPService*, retrieves an image from Azure, creates a VM named *FTPInstance* using the retrieved image, sets the VM to use an ILPIP, and adds the VM to the new service:</span></span>

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-to-retrieve-ilpip-information-for-a-vm"></a><span data-ttu-id="5caa9-133">Hur du hämtar går information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5caa9-133">How to retrieve ILPIP information for a VM</span></span>
<span data-ttu-id="5caa9-134">Kör följande PowerShell-kommando för att visa går informationen för den virtuella datorn som skapats med föregående skript, och notera att värdena för *PublicIPAddress* och *PublicIPName*:</span><span class="sxs-lookup"><span data-stu-id="5caa9-134">To view the ILPIP information for the VM created with the previous script, run the following PowerShell command and observe the values for *PublicIPAddress* and *PublicIPName*:</span></span>

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

<span data-ttu-id="5caa9-135">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="5caa9-135">Expected output:</span></span>
 
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

### <a name="how-to-remove-an-ilpip-from-a-vm"></a><span data-ttu-id="5caa9-136">Ta bort en går från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5caa9-136">How to remove an ILPIP from a VM</span></span>
<span data-ttu-id="5caa9-137">Kör följande PowerShell-kommando för att ta bort går till den virtuella datorn i föregående skript:</span><span class="sxs-lookup"><span data-stu-id="5caa9-137">To remove the ILPIP added to the VM in the previous script, run the following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-to-add-an-ilpip-to-an-existing-vm"></a><span data-ttu-id="5caa9-138">Hur du lägger till en går till en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5caa9-138">How to add an ILPIP to an existing VM</span></span>
<span data-ttu-id="5caa9-139">Om du vill lägga till en går till den virtuella datorn skapas med hjälp av skript som tidigare, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5caa9-139">To add an ILPIP to the VM created using the script previous, run the following command:</span></span>

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a><span data-ttu-id="5caa9-140">Hantera en går för en roll Cloud Services-instans</span><span class="sxs-lookup"><span data-stu-id="5caa9-140">Manage an ILPIP for a Cloud Services role instance</span></span>

<span data-ttu-id="5caa9-141">Utför följande steg för att lägga till en går till en roll Cloud Services-instans:</span><span class="sxs-lookup"><span data-stu-id="5caa9-141">To add an ILPIP to a Cloud Services role instance, complete the following steps:</span></span>

1. <span data-ttu-id="5caa9-142">Hämta .cscfg-filen för Molntjänsten genom att slutföra stegen i den [hur du konfigurerar molntjänster](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.</span><span class="sxs-lookup"><span data-stu-id="5caa9-142">Download the .cscfg file for the cloud service by completing the steps in the [How to Configure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>
2. <span data-ttu-id="5caa9-143">Uppdatera .cscfg-filen genom att lägga till den `InstanceAddress` element.</span><span class="sxs-lookup"><span data-stu-id="5caa9-143">Update the .cscfg file by adding the `InstanceAddress` element.</span></span> <span data-ttu-id="5caa9-144">I följande exempel lägger till en går med namnet *MyPublicIP* till en roll med namnet *WebRole1*:</span><span class="sxs-lookup"><span data-stu-id="5caa9-144">The following sample adds an ILPIP named *MyPublicIP* to a role instance named *WebRole1*:</span></span> 

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
3. <span data-ttu-id="5caa9-145">Överför .cscfg-filen för Molntjänsten genom att slutföra stegen i den [hur du konfigurerar molntjänster](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.</span><span class="sxs-lookup"><span data-stu-id="5caa9-145">Upload the .cscfg file for the cloud service by completing the steps in the [How to Configure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5caa9-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5caa9-146">Next steps</span></span>
* <span data-ttu-id="5caa9-147">Förstå hur [IP-adressering](virtual-network-ip-addresses-overview-classic.md) fungerar i den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="5caa9-147">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in the classic deployment model.</span></span>
* <span data-ttu-id="5caa9-148">Lär dig mer om [reserverade IP-adresser](virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="5caa9-148">Learn about [Reserved IPs](virtual-networks-reserved-public-ip.md).</span></span>
