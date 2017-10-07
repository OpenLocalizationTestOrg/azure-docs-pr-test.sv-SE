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
# <a name="connecting-azure-cloud-services-roles-tooa-custom-ad-domain-controller-hosted-in-azure"></a><span data-ttu-id="7c2b6-103">Ansluta Azure Cloud Services-roller tooa anpassad AD-domänkontrollant finns i Azure</span><span class="sxs-lookup"><span data-stu-id="7c2b6-103">Connecting Azure Cloud Services Roles tooa custom AD Domain Controller hosted in Azure</span></span>
<span data-ttu-id="7c2b6-104">Vi kommer först ställa in ett virtuellt nätverk (VNet) i Azure.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-104">We will first set up a Virtual Network (VNet) in Azure.</span></span> <span data-ttu-id="7c2b6-105">Vi lägger sedan till en Active Directory-domänkontrollant (som finns på en virtuell dator i Azure) toohello VNet.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-105">We will then add an Active Directory Domain Controller (hosted on an Azure Virtual Machine) toohello VNet.</span></span> <span data-ttu-id="7c2b6-106">Vi kommer därefter lägga till tjänsten roller av befintliga cloud-toohello som skapats i förväg VNet sedan ansluter dem toohello domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-106">Next, we will add existing cloud service roles toohello pre-created VNet, then connect them toohello Domain Controller.</span></span>

<span data-ttu-id="7c2b6-107">Innan vi börjar några av saker tookeep i åtanke:</span><span class="sxs-lookup"><span data-stu-id="7c2b6-107">Before we get started, couple of things tookeep in mind:</span></span>

1. <span data-ttu-id="7c2b6-108">Den här kursen använder PowerShell, så kontrollera att du har Azure PowerShell installerad och klar toogo.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-108">This tutorial uses PowerShell, so make sure you have Azure PowerShell installed and ready toogo.</span></span> <span data-ttu-id="7c2b6-109">tooget hjälp med att konfigurera Azure PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7c2b6-109">tooget help with setting up Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="7c2b6-110">AD-domänkontrollant och roll för Web/Worker-instanser måste toobe i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-110">Your AD Domain Controller and Web/Worker Role instances need toobe in hello VNet.</span></span>

<span data-ttu-id="7c2b6-111">Följ den här stegvisa guiden och om du stöter på problem oss lämna en kommentar hello slutet av hello artikel.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-111">Follow this step-by-step guide and if you run into any issues, leave us a comment at hello end of hello article.</span></span> <span data-ttu-id="7c2b6-112">Någon får tillbaka tooyou (Ja, kan vi läsa kommentarer).</span><span class="sxs-lookup"><span data-stu-id="7c2b6-112">Someone will get back tooyou (yes, we do read comments).</span></span>

<span data-ttu-id="7c2b6-113">hello-nätverk som refereras av hello Molntjänsten måste vara en **klassiskt virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-113">hello network that is referenced by hello cloud service must be a **classic virtual network**.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="7c2b6-114">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="7c2b6-114">Create a Virtual Network</span></span>
<span data-ttu-id="7c2b6-115">Du kan skapa ett virtuellt nätverk i Azure med hjälp av hello Azure-portalen eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-115">You can create a Virtual Network in Azure using hello Azure portal or PowerShell.</span></span> <span data-ttu-id="7c2b6-116">Den här kursen ska vi använda PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-116">For this tutorial, we will use PowerShell.</span></span> <span data-ttu-id="7c2b6-117">ett virtuellt nätverk som använder toocreate hello Azure portal, se [skapa virtuellt nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="7c2b6-117">toocreate a Virtual Network using hello Azure portal, see [Create Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>

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

## <a name="create-a-virtual-machine"></a><span data-ttu-id="7c2b6-118">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="7c2b6-118">Create a Virtual Machine</span></span>
<span data-ttu-id="7c2b6-119">När du har slutfört konfigurera hello virtuella nätverk måste toocreate en AD-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-119">Once you have completed setting up hello Virtual Network, you will need toocreate an AD Domain Controller.</span></span> <span data-ttu-id="7c2b6-120">Den här självstudiekursen kommer ska vi kunna installera en AD-domänkontrollant på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-120">For this tutorial, we will be setting up an AD Domain Controller on an Azure Virtual Machine.</span></span>

<span data-ttu-id="7c2b6-121">toodo, skapa en virtuell dator med hjälp av PowerShell med hjälp av hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="7c2b6-121">toodo this, create a virtual machine through PowerShell using hello following commands:</span></span>

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

## <a name="promote-your-virtual-machine-tooa-domain-controller"></a><span data-ttu-id="7c2b6-122">Uppgradera din virtuella tooa domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="7c2b6-122">Promote your Virtual Machine tooa Domain Controller</span></span>
<span data-ttu-id="7c2b6-123">tooconfigure hello virtuell dator som en AD-domänkontrollant, kommer du behöver toolog i toohello VM och konfigurera den.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-123">tooconfigure hello Virtual Machine as an AD Domain Controller, you will need toolog in toohello VM and configure it.</span></span>

<span data-ttu-id="7c2b6-124">toolog i toohello VM, du kan få hello RDP-filen via PowerShell, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="7c2b6-124">toolog in toohello VM, you can get hello RDP file through PowerShell, use hello following commands:</span></span>

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

<span data-ttu-id="7c2b6-125">När du har registrerat i toohello VM, konfigurera den virtuella datorn som en AD-domänkontrollant genom följande steg för steg-guide för hello på [hur tooset kunden AD-domänkontrollant](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c2b6-125">Once you are signed in toohello VM, set up your Virtual Machine as an AD Domain Controller by following hello step-by-step guide on [How tooset up your customer AD Domain Controller](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).</span></span>

## <a name="add-your-cloud-service-toohello-virtual-network"></a><span data-ttu-id="7c2b6-126">Lägg till din molntjänst toohello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="7c2b6-126">Add your Cloud Service toohello Virtual Network</span></span>
<span data-ttu-id="7c2b6-127">Sedan måste tooadd din cloud service distribution toohello nya VNet.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-127">Next, you need tooadd your cloud service deployment toohello new VNet.</span></span> <span data-ttu-id="7c2b6-128">toodo kan ändra din cloud service cscfg genom att lägga till hello relevanta avsnitt tooyour cscfg med Visual Studio eller hello redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-128">toodo this, modify your cloud service cscfg by adding hello relevant sections tooyour cscfg using Visual Studio or hello editor of your choice.</span></span>

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

<span data-ttu-id="7c2b6-129">Därefter bygga projektet cloud services och distribuera den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-129">Next build your cloud services project and deploy it tooAzure.</span></span> <span data-ttu-id="7c2b6-130">tooget hjälp med att distribuera din cloud services paketet tooAzure finns [hur tooCreate och distribuera en tjänst i molnet](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)</span><span class="sxs-lookup"><span data-stu-id="7c2b6-130">tooget help with deploying your cloud services package tooAzure, see [How tooCreate and Deploy a Cloud Service](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)</span></span>

## <a name="connect-your-webworker-roles-toohello-domain"></a><span data-ttu-id="7c2b6-131">Ansluta din web/worker roller toohello domän</span><span class="sxs-lookup"><span data-stu-id="7c2b6-131">Connect your web/worker roles toohello domain</span></span>
<span data-ttu-id="7c2b6-132">När ditt molntjänstprojekt har distribuerats på Azure, kan du ansluta din roll instanser toohello anpassade AD-domän med hjälp av hello AD-domänen tillägget.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-132">Once your cloud service project is deployed on Azure, connect your role instances toohello custom AD domain using hello AD Domain Extension.</span></span> <span data-ttu-id="7c2b6-133">tooadd hello AD-domänen tillägget tooyour befintlig cloud services distribution och koppling hello anpassad domän, kör du följande kommandon i PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="7c2b6-133">tooadd hello AD Domain Extension tooyour existing cloud services deployment and join hello custom domain, execute hello following commands in PowerShell:</span></span>

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

<span data-ttu-id="7c2b6-134">Och det är den.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-134">And that's it.</span></span>

<span data-ttu-id="7c2b6-135">Dina molntjänster ska vara domänansluten tooyour anpassade domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-135">Your cloud services should be joined tooyour custom domain controller.</span></span> <span data-ttu-id="7c2b6-136">Om du vill läsa mer om hello olika alternativ som är tillgängliga för toolearn hur tooconfigure AD-domänen tillägg, Använd hello PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7c2b6-136">If you would like toolearn more about hello different options available for how tooconfigure AD Domain Extension, use hello PowerShell help.</span></span> <span data-ttu-id="7c2b6-137">Några exempel följande:</span><span class="sxs-lookup"><span data-stu-id="7c2b6-137">A couple of examples follow:</span></span>

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
