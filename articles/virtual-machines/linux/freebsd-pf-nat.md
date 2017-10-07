---
title: "aaaUse FreeBSD paketfilter toocreate en brandvägg i Azure | Microsoft Docs"
description: "Lär dig hur toodeploy en NAT-brandväggen med hjälp av Freebsd's PF i Azure."
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
ms.openlocfilehash: 3d3a5dde2ca03ba6fc384581c786f5eb746e6d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-freebsds-packet-filter-toocreate-a-secure-firewall-in-azure"></a><span data-ttu-id="07b34-103">Hur toouse FreeBSD paketfilter toocreate en säker brandvägg i Azure</span><span class="sxs-lookup"><span data-stu-id="07b34-103">How toouse FreeBSD's Packet Filter toocreate a secure firewall in Azure</span></span>
<span data-ttu-id="07b34-104">Den här artikeln beskriver hur toodeploy NAT-brandvägg med Freebsds förpackaren Filter via Azure Resource Manager-mall för web server ovanligt.</span><span class="sxs-lookup"><span data-stu-id="07b34-104">This article introduces how toodeploy a NAT firewall using FreeBSD’s Packer Filter through Azure Resource Manager template for common web server scenario.</span></span>

## <a name="what-is-pf"></a><span data-ttu-id="07b34-105">Vad är PF?</span><span class="sxs-lookup"><span data-stu-id="07b34-105">What is PF?</span></span>
<span data-ttu-id="07b34-106">PF (paketfilter, också skrivs pf) är ett BSD licensierade tillståndskänslig paketfilter, en central del av programvaran för firewalling.</span><span class="sxs-lookup"><span data-stu-id="07b34-106">PF (Packet Filter, also written pf) is a BSD licensed stateful packet filter, a central piece of software for firewalling.</span></span> <span data-ttu-id="07b34-107">PF sedan har utvecklats snabbt och nu har flera fördelar jämfört med andra tillgängliga brandväggar.</span><span class="sxs-lookup"><span data-stu-id="07b34-107">PF has since evolved quickly and now has several advantages over other available firewalls.</span></span> <span data-ttu-id="07b34-108">NAT (Network Address Translation) är i PF ända sedan paketschemaläggaren och aktiva köhantering har integrerats i PF, genom att integrera hello ALTQ och göra den konfigureras via PFS konfiguration.</span><span class="sxs-lookup"><span data-stu-id="07b34-108">Network Address Translation (NAT) is in PF since day one, then packet scheduler and active queue management have been integrated into PF, by integrating hello ALTQ and making it configurable through PF's configuration.</span></span> <span data-ttu-id="07b34-109">Funktioner som pfsync och KARP för växling vid fel och redundans, authpf för sessionsautentisering och ftp-proxy tooease firewalling hello svårt FTP-protokollet har också utökat PF.</span><span class="sxs-lookup"><span data-stu-id="07b34-109">Features such as pfsync and CARP for failover and redundancy, authpf for session authentication, and ftp-proxy tooease firewalling hello difficult FTP protocol, have also extended PF.</span></span> <span data-ttu-id="07b34-110">Kort sagt: PF är en kraftfull och funktionsrika brandvägg.</span><span class="sxs-lookup"><span data-stu-id="07b34-110">In short, PF is a powerful and feature-rich firewall.</span></span> 

## <a name="get-started"></a><span data-ttu-id="07b34-111">Kom igång</span><span class="sxs-lookup"><span data-stu-id="07b34-111">Get started</span></span>
<span data-ttu-id="07b34-112">Om du är intresserad av att skapa en säker brandvägg i hello molnet för webbservrar, nu sätter vi igång.</span><span class="sxs-lookup"><span data-stu-id="07b34-112">If you are interested in setting up a secure firewall in hello cloud for your web servers, then let’s get started.</span></span> <span data-ttu-id="07b34-113">Du kan också använda hello-skript som används i den här Azure Resource Manager-mall tooset upp din nätverkstopologin.</span><span class="sxs-lookup"><span data-stu-id="07b34-113">You can also apply hello scripts used in this Azure Resource Manager template tooset up your networking topology.</span></span>
<span data-ttu-id="07b34-114">hello Azure Resource Manager-mall som ställer in en FreeBSD virtuell dator som utför NAT /redirection med PF och två FreeBSD-virtuella datorer med hello Nginx-webbservern installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="07b34-114">hello Azure Resource Manager template set up a FreeBSD virtual machine that performs NAT /redirection using PF and two FreeBSD virtual machines with hello Nginx web server installed and configured.</span></span> <span data-ttu-id="07b34-115">Dessutom tooperforming NAT för hello två webbservrar utgående trafik, hello NAT/omdirigering av virtuell dator fångar upp HTTP-begäranden och omdirigerar dem toohello två webbservrar på resursallokering sätt.</span><span class="sxs-lookup"><span data-stu-id="07b34-115">In addition tooperforming NAT for hello two web servers egress traffic, hello NAT/redirection virtual machine intercepts HTTP requests and redirect them toohello two web servers in round-robin fashion.</span></span> <span data-ttu-id="07b34-116">Hej VNet använder hello privata icke-dirigerbara IP-adressutrymme 10.0.0.2/24 och du kan ändra hello parametrarna för hello mallen.</span><span class="sxs-lookup"><span data-stu-id="07b34-116">hello VNet uses hello private non-routable IP address space 10.0.0.2/24 and you can modify hello parameters of hello template.</span></span> <span data-ttu-id="07b34-117">hello Azure Resource Manager-mall definierar också en routningstabell för hello hela VNet, som är en samling individuella vägare som används för toooverride Azure standardvägar baserat på hello mål-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="07b34-117">hello Azure Resource Manager template also defines a route table for hello whole VNet, which is a collection of individual routes used toooverride Azure default routes based on hello destination IP address.</span></span> 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a><span data-ttu-id="07b34-119">Distribuera via Azure CLI</span><span class="sxs-lookup"><span data-stu-id="07b34-119">Deploy through Azure CLI</span></span>
<span data-ttu-id="07b34-120">Du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="07b34-120">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="07b34-121">Skapa en resursgrupp med [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="07b34-121">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="07b34-122">hello följande exempel skapas ett Resursgruppsnamn `myResourceGroup` i hello `West US` plats.</span><span class="sxs-lookup"><span data-stu-id="07b34-122">hello following example creates a resource group name `myResourceGroup` in hello `West US` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="07b34-123">Distribuera mallen hello [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) med [az distribution skapa](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="07b34-123">Next, deploy hello template [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="07b34-124">Hämta [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) under hello samma sökväg och definiera en egen resurs-värden som `adminPassword`, `networkPrefix`, och `domainNamePrefix`.</span><span class="sxs-lookup"><span data-stu-id="07b34-124">Download [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) under hello same path and define your own resource values, such as `adminPassword`, `networkPrefix`, and `domainNamePrefix`.</span></span> 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

<span data-ttu-id="07b34-125">Efter cirka fem minuter kommer du få hello information för `"provisioningState": "Succeeded"`.</span><span class="sxs-lookup"><span data-stu-id="07b34-125">After about five minutes, you will get hello information of `"provisioningState": "Succeeded"`.</span></span> <span data-ttu-id="07b34-126">Sedan kan du ssh toohello klientdel VM (NAT) eller öppna Nginx webbserver i en webbläsare som använder hello offentlig IP-adress eller fullständigt domännamn för hello klientdel VM (NAT).</span><span class="sxs-lookup"><span data-stu-id="07b34-126">Then you can ssh toohello frontend VM (NAT) or access Nginx web server in a browser using hello public IP address or FQDN of hello frontend VM (NAT).</span></span> <span data-ttu-id="07b34-127">hello följande exempel visar FQDN och offentliga IP-adress som tilldelats toohello klientdel VM (NAT) i hello `myResourceGroup` resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="07b34-127">hello following example lists FQDN and public IP address that assigned toohello frontend VM (NAT) in hello `myResourceGroup` resource group.</span></span> 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a><span data-ttu-id="07b34-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="07b34-128">Next steps</span></span>
<span data-ttu-id="07b34-129">Vill du tooset upp egna NAT i Azure?</span><span class="sxs-lookup"><span data-stu-id="07b34-129">Do you want tooset up your own NAT in Azure?</span></span> <span data-ttu-id="07b34-130">Öppna datakällan, kostnadsfritt men kraftfulla?</span><span class="sxs-lookup"><span data-stu-id="07b34-130">Open Source, free but powerful?</span></span> <span data-ttu-id="07b34-131">Sedan är PF ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="07b34-131">Then PF is a good choice.</span></span> <span data-ttu-id="07b34-132">Med hjälp av mallen hello [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), behöver du bara fem minuter tooset upp en NAT-brandvägg med belastningsutjämning för resursallokering med Freebsd's PF i Azure för web server ovanligt.</span><span class="sxs-lookup"><span data-stu-id="07b34-132">By using hello template [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), you only need five minutes tooset up a NAT firewall with round-robin load balancing using FreeBSD's PF in Azure for common web server scenario.</span></span> 

<span data-ttu-id="07b34-133">Om du vill toolearn hello erbjuds FreeBSD i Azure finns för[introduktion tooFreeBSD på Azure](freebsd-intro-on-azure.md).</span><span class="sxs-lookup"><span data-stu-id="07b34-133">If you want toolearn hello offering of FreeBSD in Azure, refer too[introduction tooFreeBSD on Azure](freebsd-intro-on-azure.md).</span></span>

<span data-ttu-id="07b34-134">Om du vill tooknow mer om PF finns för[FreeBSD handboken](https://www.freebsd.org/doc/handbook/firewalls-pf.html) eller [PF-Användarhandbok](https://www.freebsd.org/doc/handbook/firewalls-pf.html).</span><span class="sxs-lookup"><span data-stu-id="07b34-134">If you want tooknow more about PF, refer too[FreeBSD handbook](https://www.freebsd.org/doc/handbook/firewalls-pf.html) or [PF-User's Guide](https://www.freebsd.org/doc/handbook/firewalls-pf.html).</span></span>
