---
title: "Använd Freebsds paketfilter för att skapa en brandvägg i Azure | Microsoft Docs"
description: "Lär dig hur du distribuerar en NAT-brandvägg med Freebsd's PF i Azure."
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: cd777291a1321eabf4efe0d7b9b101f932d9398b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a><span data-ttu-id="356ac-103">Hur du använder Freebsds paketfilter för att skapa en säker brandvägg i Azure</span><span class="sxs-lookup"><span data-stu-id="356ac-103">How to use FreeBSD's Packet Filter to create a secure firewall in Azure</span></span>
<span data-ttu-id="356ac-104">Den här artikeln beskriver hur du distribuerar en NAT-brandvägg med Freebsds förpackaren Filter via Azure Resource Manager-mall för web server ovanligt.</span><span class="sxs-lookup"><span data-stu-id="356ac-104">This article introduces how to deploy a NAT firewall using FreeBSD’s Packer Filter through Azure Resource Manager template for common web server scenario.</span></span>

## <a name="what-is-pf"></a><span data-ttu-id="356ac-105">Vad är PF?</span><span class="sxs-lookup"><span data-stu-id="356ac-105">What is PF?</span></span>
<span data-ttu-id="356ac-106">PF (paketfilter, också skrivs pf) är ett BSD licensierade tillståndskänslig paketfilter, en central del av programvaran för firewalling.</span><span class="sxs-lookup"><span data-stu-id="356ac-106">PF (Packet Filter, also written pf) is a BSD licensed stateful packet filter, a central piece of software for firewalling.</span></span> <span data-ttu-id="356ac-107">PF sedan har utvecklats snabbt och nu har flera fördelar jämfört med andra tillgängliga brandväggar.</span><span class="sxs-lookup"><span data-stu-id="356ac-107">PF has since evolved quickly and now has several advantages over other available firewalls.</span></span> <span data-ttu-id="356ac-108">NAT (Network Address Translation) är i PF ända sedan paketschemaläggaren och aktiva köhantering har integrerats i PF, genom att integrera ALTQ och göra den konfigureras via PFS konfiguration.</span><span class="sxs-lookup"><span data-stu-id="356ac-108">Network Address Translation (NAT) is in PF since day one, then packet scheduler and active queue management have been integrated into PF, by integrating the ALTQ and making it configurable through PF's configuration.</span></span> <span data-ttu-id="356ac-109">Funktioner som pfsync och KARP för växling vid fel och redundans, authpf för sessionsautentisering och ftp-proxy för att underlätta firewalling svårt FTP-protokollet har också utökat PF.</span><span class="sxs-lookup"><span data-stu-id="356ac-109">Features such as pfsync and CARP for failover and redundancy, authpf for session authentication, and ftp-proxy to ease firewalling the difficult FTP protocol, have also extended PF.</span></span> <span data-ttu-id="356ac-110">Kort sagt: PF är en kraftfull och funktionsrika brandvägg.</span><span class="sxs-lookup"><span data-stu-id="356ac-110">In short, PF is a powerful and feature-rich firewall.</span></span> 

## <a name="get-started"></a><span data-ttu-id="356ac-111">Kom igång</span><span class="sxs-lookup"><span data-stu-id="356ac-111">Get started</span></span>
<span data-ttu-id="356ac-112">Om du är intresserad av att skapa en säker brandvägg i molnet för webbservrar, nu sätter vi igång.</span><span class="sxs-lookup"><span data-stu-id="356ac-112">If you are interested in setting up a secure firewall in the cloud for your web servers, then let’s get started.</span></span> <span data-ttu-id="356ac-113">Du kan också använda de skript som används i den här Azure Resource Manager-mallen för att konfigurera din nätverkstopologin.</span><span class="sxs-lookup"><span data-stu-id="356ac-113">You can also apply the scripts used in this Azure Resource Manager template to set up your networking topology.</span></span>
<span data-ttu-id="356ac-114">Azure Resource Manager-mallen ställa in en FreeBSD virtuell dator som utför NAT /redirection med PF och två FreeBSD-virtuella datorer med webbservern Nginx installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="356ac-114">The Azure Resource Manager template set up a FreeBSD virtual machine that performs NAT /redirection using PF and two FreeBSD virtual machines with the Nginx web server installed and configured.</span></span> <span data-ttu-id="356ac-115">Förutom att utföra NAT för de två servrarna utgång webbtrafik virtuella NAT-omdirigering spärras HTTP-begäranden och omdirigerar dem till två webbservrar på resursallokering sätt.</span><span class="sxs-lookup"><span data-stu-id="356ac-115">In addition to performing NAT for the two web servers egress traffic, the NAT/redirection virtual machine intercepts HTTP requests and redirect them to the two web servers in round-robin fashion.</span></span> <span data-ttu-id="356ac-116">VNet använder den privata icke-dirigerbara IP-adressutrymme 10.0.0.2/24 och du kan ändra parametrarna för mallen.</span><span class="sxs-lookup"><span data-stu-id="356ac-116">The VNet uses the private non-routable IP address space 10.0.0.2/24 and you can modify the parameters of the template.</span></span> <span data-ttu-id="356ac-117">Azure Resource Manager-mall definierar också en routningstabell för hela VNet, som är en samling individuella vägare som används för att åsidosätta Azure standardvägar baserat på mål-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="356ac-117">The Azure Resource Manager template also defines a route table for the whole VNet, which is a collection of individual routes used to override Azure default routes based on the destination IP address.</span></span> 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a><span data-ttu-id="356ac-119">Distribuera via Azure CLI</span><span class="sxs-lookup"><span data-stu-id="356ac-119">Deploy through Azure CLI</span></span>
<span data-ttu-id="356ac-120">Du behöver senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="356ac-120">You need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="356ac-121">Skapa en resursgrupp med [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="356ac-121">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="356ac-122">I följande exempel skapas ett Resursgruppsnamn `myResourceGroup` i den `West US` plats.</span><span class="sxs-lookup"><span data-stu-id="356ac-122">The following example creates a resource group name `myResourceGroup` in the `West US` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="356ac-123">Distribuera mallen [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) med [az distribution skapa](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="356ac-123">Next, deploy the template [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="356ac-124">Hämta [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) under samma sökväg och definiera en egen resurs-värden som `adminPassword`, `networkPrefix`, och `domainNamePrefix`.</span><span class="sxs-lookup"><span data-stu-id="356ac-124">Download [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) under the same path and define your own resource values, such as `adminPassword`, `networkPrefix`, and `domainNamePrefix`.</span></span> 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

<span data-ttu-id="356ac-125">Efter cirka fem minuter visas information för `"provisioningState": "Succeeded"`.</span><span class="sxs-lookup"><span data-stu-id="356ac-125">After about five minutes, you will get the information of `"provisioningState": "Succeeded"`.</span></span> <span data-ttu-id="356ac-126">Sedan kan du ssh till klientdel VM (NAT) eller åtkomst Nginx webbserver i en webbläsare som använder offentliga IP-adress eller FQDN för klientdel VM (NAT).</span><span class="sxs-lookup"><span data-stu-id="356ac-126">Then you can ssh to the frontend VM (NAT) or access Nginx web server in a browser using the public IP address or FQDN of the frontend VM (NAT).</span></span> <span data-ttu-id="356ac-127">I följande exempel visar FQDN och offentliga IP-adress som tilldelats klientdel VM (NAT) i den `myResourceGroup` resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="356ac-127">The following example lists FQDN and public IP address that assigned to the frontend VM (NAT) in the `myResourceGroup` resource group.</span></span> 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a><span data-ttu-id="356ac-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="356ac-128">Next steps</span></span>
<span data-ttu-id="356ac-129">Vill du konfigurera din egen NAT i Azure?</span><span class="sxs-lookup"><span data-stu-id="356ac-129">Do you want to set up your own NAT in Azure?</span></span> <span data-ttu-id="356ac-130">Öppna datakällan, kostnadsfritt men kraftfulla?</span><span class="sxs-lookup"><span data-stu-id="356ac-130">Open Source, free but powerful?</span></span> <span data-ttu-id="356ac-131">Sedan är PF ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="356ac-131">Then PF is a good choice.</span></span> <span data-ttu-id="356ac-132">Med hjälp av mallen [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), behöver du bara fem minuter har PF i Azure för web server ovanligt att konfigurera en NAT-brandvägg med belastningsutjämning för resursallokering med FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="356ac-132">By using the template [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), you only need five minutes to set up a NAT firewall with round-robin load balancing using FreeBSD's PF in Azure for common web server scenario.</span></span> 

<span data-ttu-id="356ac-133">Om du vill lära dig att erbjuda FreeBSD i Azure finns [introduktion till FreeBSD på Azure](freebsd-intro-on-azure.md).</span><span class="sxs-lookup"><span data-stu-id="356ac-133">If you want to learn the offering of FreeBSD in Azure, refer to [introduction to FreeBSD on Azure](freebsd-intro-on-azure.md).</span></span>

<span data-ttu-id="356ac-134">Om du vill veta mer om PF avser [FreeBSD handboken](https://www.freebsd.org/doc/handbook/firewalls-pf.html) eller [PF-Användarhandbok](https://www.freebsd.org/doc/handbook/firewalls-pf.html).</span><span class="sxs-lookup"><span data-stu-id="356ac-134">If you want to know more about PF, refer to [FreeBSD handbook](https://www.freebsd.org/doc/handbook/firewalls-pf.html) or [PF-User's Guide](https://www.freebsd.org/doc/handbook/firewalls-pf.html).</span></span>
