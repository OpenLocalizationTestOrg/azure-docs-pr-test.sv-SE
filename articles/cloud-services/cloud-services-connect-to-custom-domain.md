---
title: "aaaConnect en molntjänst tooa anpassade domänkontrollanten | Microsoft Docs"
description: "Lär dig hur tooconnect egna web/worker roller tooa AD-domän med PowerShell och tillägg för AD-domän"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1e2d7c87-d254-4e7a-a832-67f84411ec95
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 9540190ccf17c03e55159c6c68429eee29e0a558
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-azure-cloud-services-roles-tooa-custom-ad-domain-controller-hosted-in-azure"></a>Ansluta Azure Cloud Services-roller tooa anpassad AD-domänkontrollant finns i Azure
Vi kommer först ställa in ett virtuellt nätverk (VNet) i Azure. Vi lägger sedan till en Active Directory-domänkontrollant (som finns på en virtuell dator i Azure) toohello VNet. Vi kommer därefter lägga till tjänsten roller av befintliga cloud-toohello som skapats i förväg VNet sedan ansluter dem toohello domänkontrollant.

Innan vi börjar några av saker tookeep i åtanke:

1. Den här kursen använder PowerShell, så kontrollera att du har Azure PowerShell installerad och klar toogo. tooget hjälp med att konfigurera Azure PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
2. AD-domänkontrollant och roll för Web/Worker-instanser måste toobe i hello virtuella nätverk.

Följ den här stegvisa guiden och om du stöter på problem oss lämna en kommentar hello slutet av hello artikel. Någon får tillbaka tooyou (Ja, kan vi läsa kommentarer).

hello-nätverk som refereras av hello Molntjänsten måste vara en **klassiskt virtuellt nätverk**.

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk
Du kan skapa ett virtuellt nätverk i Azure med hjälp av hello Azure-portalen eller PowerShell. Den här kursen ska vi använda PowerShell. ett virtuellt nätverk som använder toocreate hello Azure portal, se [skapa virtuellt nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Skapa en virtuell dator
När du har slutfört konfigurera hello virtuella nätverk måste toocreate en AD-domänkontrollant. Den här självstudiekursen kommer ska vi kunna installera en AD-domänkontrollant på en virtuell dator i Azure.

toodo, skapa en virtuell dator med hjälp av PowerShell med hjälp av hello följande kommandon:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it toohello Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-tooa-domain-controller"></a>Uppgradera din virtuella tooa domänkontrollant
tooconfigure hello virtuell dator som en AD-domänkontrollant, kommer du behöver toolog i toohello VM och konfigurera den.

toolog i toohello VM, du kan få hello RDP-filen via PowerShell, Använd hello följande kommandon:

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

När du har registrerat i toohello VM, konfigurera den virtuella datorn som en AD-domänkontrollant genom följande steg för steg-guide för hello på [hur tooset kunden AD-domänkontrollant](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-toohello-virtual-network"></a>Lägg till din molntjänst toohello virtuellt nätverk
Sedan måste tooadd din cloud service distribution toohello nya VNet. toodo kan ändra din cloud service cscfg genom att lägga till hello relevanta avsnitt tooyour cscfg med Visual Studio eller hello redigeringsprogram.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Därefter bygga projektet cloud services och distribuera den tooAzure. tooget hjälp med att distribuera din cloud services paketet tooAzure finns [hur tooCreate och distribuera en tjänst i molnet](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)

## <a name="connect-your-webworker-roles-toohello-domain"></a>Ansluta din web/worker roller toohello domän
När ditt molntjänstprojekt har distribuerats på Azure, kan du ansluta din roll instanser toohello anpassade AD-domän med hjälp av hello AD-domänen tillägget. tooadd hello AD-domänen tillägget tooyour befintlig cloud services distribution och koppling hello anpassad domän, kör du följande kommandon i PowerShell hello:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension toohello cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

Och det är den.

Dina molntjänster ska vara domänansluten tooyour anpassade domänkontrollant. Om du vill läsa mer om hello olika alternativ som är tillgängliga för toolearn hur tooconfigure AD-domänen tillägg, Använd hello PowerShell. Några exempel följande:

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
