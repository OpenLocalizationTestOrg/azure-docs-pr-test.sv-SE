---
title: "aaaReverse DNS för Azure-tjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure omvänd DNS-sökning för tjänster i Azure"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: c6fe1d80232f124be86dd7fc57abc20699be7eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a><span data-ttu-id="44864-103">Konfigurera omvänd DNS för tjänster i Azure</span><span class="sxs-lookup"><span data-stu-id="44864-103">Configure reverse DNS for services hosted in Azure</span></span>

<span data-ttu-id="44864-104">Den här artikeln förklarar hur tooconfigure omvänd DNS-sökning för tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="44864-104">This article explains how tooconfigure reverse DNS lookups for services hosted in Azure.</span></span>

<span data-ttu-id="44864-105">Tjänster i Azure använda IP-adresser tilldelas av Azure och som ägs av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="44864-105">Services in Azure use IP addresses assigned by Azure and owned by Microsoft.</span></span> <span data-ttu-id="44864-106">Dessa omvänd DNS-poster (PTR-poster) måste skapas i hello motsvarande ägs av Microsoft omvänd DNS-zoner för sökning.</span><span class="sxs-lookup"><span data-stu-id="44864-106">These reverse DNS records (PTR records) must be created in hello corresponding Microsoft-owned reverse DNS lookup zones.</span></span> <span data-ttu-id="44864-107">Den här artikeln förklarar hur toodo detta.</span><span class="sxs-lookup"><span data-stu-id="44864-107">This article explains how toodo this.</span></span>

<span data-ttu-id="44864-108">Det här scenariot ska inte förväxlas med hello möjligheten för[värd hello omvänd DNS-sökningszoner för din tilldelade IP-adressintervall i Azure DNS](dns-reverse-dns-hosting.md).</span><span class="sxs-lookup"><span data-stu-id="44864-108">This scenario should not be confused with hello ability too[host hello reverse DNS lookup zones for your assigned IP ranges in Azure DNS](dns-reverse-dns-hosting.md).</span></span> <span data-ttu-id="44864-109">I det här fallet måste hello IP-adressintervall som representeras av hello zon för omvänd sökning tilldelas tooyour organisation, vanligtvis av din Internetleverantör.</span><span class="sxs-lookup"><span data-stu-id="44864-109">In this case, hello IP ranges represented by hello reverse lookup zone must be assigned tooyour organization, typically by your ISP.</span></span>

<span data-ttu-id="44864-110">Innan du läser den här artikeln bör du vara bekant med den här [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44864-110">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="44864-111">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="44864-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
* <span data-ttu-id="44864-112">I hello Resource Manager-distributionsmodellen exponeras beräkningsresurser (till exempel virtuella datorer, virtuella datorer eller Service Fabric-kluster) via en PublicIpAddress-resurs.</span><span class="sxs-lookup"><span data-stu-id="44864-112">In hello Resource Manager deployment model, compute resources (such as virtual machines, virtual machine scale sets, or Service Fabric clusters) are exposed via a PublicIpAddress resource.</span></span> <span data-ttu-id="44864-113">Omvänd DNS-sökning konfigureras med hello 'ReverseFqdn'-egenskapen för hello PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="44864-113">Reverse DNS lookups are configured using hello 'ReverseFqdn' property of hello PublicIpAddress.</span></span>
* <span data-ttu-id="44864-114">I hello klassiska distributionsmodellen exponeras beräkningsresurser med molntjänster.</span><span class="sxs-lookup"><span data-stu-id="44864-114">In hello Classic deployment model, compute resources are exposed using Cloud Services.</span></span> <span data-ttu-id="44864-115">Omvänd DNS-sökning konfigureras med hello 'ReverseDnsFqdn'-egenskapen för hello tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="44864-115">Reverse DNS lookups are configured using hello 'ReverseDnsFqdn' property of hello Cloud Service.</span></span>

<span data-ttu-id="44864-116">Omvänd DNS stöds inte för hello Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="44864-116">Reverse DNS is not currently supported for hello Azure App Service.</span></span>

## <a name="validation-of-reverse-dns-records"></a><span data-ttu-id="44864-117">Validering av omvänd DNS-poster</span><span class="sxs-lookup"><span data-stu-id="44864-117">Validation of reverse DNS records</span></span>

<span data-ttu-id="44864-118">En tredje part får inte vara kan toocreate omvänd DNS-poster för sina Azure-tjänsten mappning tooyour DNS-domäner.</span><span class="sxs-lookup"><span data-stu-id="44864-118">A third party should not be able toocreate reverse DNS records for their Azure service mapping tooyour DNS domains.</span></span> <span data-ttu-id="44864-119">tooprevent, Azure kan bara hello skapandet av en omvänd DNS-post där domännamnet som anges i hello omvänd DNS-posten är hello samma som eller matchar, hello DNS-namn eller IP-adressen för en PublicIpAddress eller moln-tjänsten i hello samma Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="44864-119">tooprevent this, Azure only allows hello creation of a reverse DNS record where domain name specified in hello reverse DNS record is hello same as, or resolves to, hello DNS name or IP address of a PublicIpAddress or Cloud Service in hello same Azure subscription.</span></span>

<span data-ttu-id="44864-120">Verifieringen utförs endast när hello omvänd DNS-post anges eller ändras.</span><span class="sxs-lookup"><span data-stu-id="44864-120">This validation is only performed when hello reverse DNS record is set or modified.</span></span> <span data-ttu-id="44864-121">Periodiska ny verifiering utförs inte.</span><span class="sxs-lookup"><span data-stu-id="44864-121">Periodic re-validation is not performed.</span></span>

<span data-ttu-id="44864-122">Exempel: Anta att hello PublicIpAddress resurs har contosoapp1.northus.cloudapp.azure.com för hello DNS-namn och IP-adress 23.96.52.53.</span><span class="sxs-lookup"><span data-stu-id="44864-122">For example: suppose hello PublicIpAddress resource has hello DNS name contosoapp1.northus.cloudapp.azure.com and IP address 23.96.52.53.</span></span> <span data-ttu-id="44864-123">Hej ReverseFqdn för hello PublicIpAddress kan anges som:</span><span class="sxs-lookup"><span data-stu-id="44864-123">hello ReverseFqdn for hello PublicIpAddress can be specified as:</span></span>
* <span data-ttu-id="44864-124">hello DNS-namn för hello PublicIpAddress, contosoapp1.northus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="44864-124">hello DNS name for hello PublicIpAddress, contosoapp1.northus.cloudapp.azure.com</span></span>
* <span data-ttu-id="44864-125">hello DNS-namn för en annan PublicIpAddress i hello samma prenumeration, till exempel contosoapp2.westus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="44864-125">hello DNS name for a different PublicIpAddress in hello same subscription, such as contosoapp2.westus.cloudapp.azure.com</span></span>
* <span data-ttu-id="44864-126">En alternativa DNS-namn, till exempel app1.contoso.com, så länge det här namnet är *första* som konfigurerats som en CNAME-toocontosoapp1.northus.cloudapp.azure.com eller tooa olika PublicIpAddress i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="44864-126">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as a CNAME toocontosoapp1.northus.cloudapp.azure.com, or tooa different PublicIpAddress in hello same subscription.</span></span>
* <span data-ttu-id="44864-127">En alternativa DNS-namn, till exempel app1.contoso.com, så länge det här namnet är *första* som konfigurerats som en A-poster toohello IP-adress 23.96.52.53 eller toohello IP-adressen för en annan PublicIpAddress i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="44864-127">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as an A record toohello IP address 23.96.52.53, or toohello IP address of a different PublicIpAddress in hello same subscription.</span></span>

<span data-ttu-id="44864-128">hello samma begränsningar gäller tooreverse DNS för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="44864-128">hello same constraints apply tooreverse DNS for Cloud Services.</span></span>


## <a name="reverse-dns-for-publicipaddress-resources"></a><span data-ttu-id="44864-129">Omvänd DNS för PublicIpAddress-resurser</span><span class="sxs-lookup"><span data-stu-id="44864-129">Reverse DNS for PublicIpAddress resources</span></span>

<span data-ttu-id="44864-130">Det här avsnittet innehåller detaljerade anvisningar om hur tooconfigure omvänd DNS för PublicIpAddress resurser i hello Resource Manager-distributionsmodellen, med hjälp av Azure PowerShell, Azure CLI 1.0 eller 2.0 för Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="44864-130">This section provides detailed instructions for how tooconfigure reverse DNS for PublicIpAddress resources in hello Resource Manager deployment model, using either Azure PowerShell, Azure CLI 1.0, or Azure CLI 2.0.</span></span> <span data-ttu-id="44864-131">Konfigurera omvänd DNS för PublicIpAddress resurser stöds inte för närvarande via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="44864-131">Configuring reverse DNS for PublicIpAddress resources is not currently supported via hello Azure portal.</span></span>

<span data-ttu-id="44864-132">Azure för närvarande har stöd för omvänd DNS endast för IPv4 PublicIpAddress resurser.</span><span class="sxs-lookup"><span data-stu-id="44864-132">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources.</span></span> <span data-ttu-id="44864-133">Det finns inte stöd för IPv6.</span><span class="sxs-lookup"><span data-stu-id="44864-133">It is not supported for IPv6.</span></span>

### <a name="add-reverse-dns-tooan-existing-publicipaddresses"></a><span data-ttu-id="44864-134">Lägg till omvänd DNS-tooan befintliga PublicIpAddresses</span><span class="sxs-lookup"><span data-stu-id="44864-134">Add reverse DNS tooan existing PublicIpAddresses</span></span>

#### <a name="powershell"></a><span data-ttu-id="44864-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="44864-135">PowerShell</span></span>

<span data-ttu-id="44864-136">tooadd omvänd DNS-tooan befintliga PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="44864-136">tooadd reverse DNS tooan existing PublicIpAddress:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

<span data-ttu-id="44864-137">tooadd omvänd DNS-tooan befintliga PublicIpAddress som inte redan har ett DNS-namn, måste du också ange ett DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="44864-137">tooadd reverse DNS tooan existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="44864-138">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="44864-138">Azure CLI 1.0</span></span>

<span data-ttu-id="44864-139">tooadd omvänd DNS-tooan befintliga PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="44864-139">tooadd reverse DNS tooan existing PublicIpAddress:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="44864-140">tooadd omvänd DNS-tooan befintliga PublicIpAddress som inte redan har ett DNS-namn, måste du också ange ett DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="44864-140">tooadd reverse DNS tooan existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="44864-141">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="44864-141">Azure CLI 2.0</span></span>

<span data-ttu-id="44864-142">tooadd omvänd DNS-tooan befintliga PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="44864-142">tooadd reverse DNS tooan existing PublicIpAddress:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="44864-143">tooadd omvänd DNS-tooan befintliga PublicIpAddress som inte redan har ett DNS-namn, måste du också ange ett DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="44864-143">tooadd reverse DNS tooan existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a><span data-ttu-id="44864-144">Skapa en offentlig IP-adress med omvänd DNS</span><span class="sxs-lookup"><span data-stu-id="44864-144">Create a Public IP Address with reverse DNS</span></span>

<span data-ttu-id="44864-145">toocreate en ny PublicIpAddress hello omvänd DNS-egenskapen har redan angetts:</span><span class="sxs-lookup"><span data-stu-id="44864-145">toocreate a new PublicIpAddress with hello reverse DNS property already specified:</span></span>

#### <a name="powershell"></a><span data-ttu-id="44864-146">PowerShell</span><span class="sxs-lookup"><span data-stu-id="44864-146">PowerShell</span></span>

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a><span data-ttu-id="44864-147">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="44864-147">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="44864-148">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="44864-148">Azure CLI 2.0</span></span>

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a><span data-ttu-id="44864-149">Visa omvända DNS för en befintlig PublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="44864-149">View reverse DNS for an existing PublicIpAddress</span></span>

<span data-ttu-id="44864-150">tooview hello konfigurerat värde för en befintlig PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="44864-150">tooview hello configured value for an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="44864-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="44864-151">PowerShell</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a><span data-ttu-id="44864-152">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="44864-152">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a><span data-ttu-id="44864-153">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="44864-153">Azure CLI 2.0</span></span>

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a><span data-ttu-id="44864-154">Ta bort omvänd DNS från befintliga offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="44864-154">Remove reverse DNS from existing Public IP Addresses</span></span>

<span data-ttu-id="44864-155">tooremove en omvänd DNS-egenskap från en befintlig PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="44864-155">tooremove a reverse DNS property from an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="44864-156">PowerShell</span><span class="sxs-lookup"><span data-stu-id="44864-156">PowerShell</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="44864-157">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="44864-157">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a><span data-ttu-id="44864-158">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="44864-158">Azure CLI 2.0</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a><span data-ttu-id="44864-159">Konfigurera omvänd DNS för molntjänster</span><span class="sxs-lookup"><span data-stu-id="44864-159">Configure reverse DNS for Cloud Services</span></span>

<span data-ttu-id="44864-160">Det här avsnittet innehåller detaljerade anvisningar om hur tooconfigure omvänd DNS för molntjänster i hello klassiska distributionsmodellen, med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="44864-160">This section provides detailed instructions for how tooconfigure reverse DNS for Cloud Services in hello Classic deployment model, using Azure PowerShell.</span></span> <span data-ttu-id="44864-161">Konfigurera omvänd DNS för molntjänster stöds inte via hello Azure-portalen, Azure CLI 1.0 eller 2.0 för Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="44864-161">Configuring reverse DNS for Cloud Services is not supported via hello Azure portal, Azure CLI 1.0, or Azure CLI 2.0.</span></span>

### <a name="add-reverse-dns-tooexisting-cloud-services"></a><span data-ttu-id="44864-162">Lägg till omvänd DNS-tooexisting molntjänster</span><span class="sxs-lookup"><span data-stu-id="44864-162">Add reverse DNS tooexisting Cloud Services</span></span>

<span data-ttu-id="44864-163">tooadd en omvänd DNS registrera tooan befintlig tjänst i molnet:</span><span class="sxs-lookup"><span data-stu-id="44864-163">tooadd a reverse DNS record tooan existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a><span data-ttu-id="44864-164">Skapa en molntjänst med omvänd DNS</span><span class="sxs-lookup"><span data-stu-id="44864-164">Create a Cloud Service with reverse DNS</span></span>

<span data-ttu-id="44864-165">toocreate en ny molntjänst hello omvänd DNS-egenskapen har redan angetts:</span><span class="sxs-lookup"><span data-stu-id="44864-165">toocreate a new Cloud Service with hello reverse DNS property already specified:</span></span>

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a><span data-ttu-id="44864-166">Visa omvända DNS för befintliga molntjänster</span><span class="sxs-lookup"><span data-stu-id="44864-166">View reverse DNS for existing Cloud Services</span></span>

<span data-ttu-id="44864-167">tooview hello omvänd DNS-egenskapen för en befintlig molntjänst:</span><span class="sxs-lookup"><span data-stu-id="44864-167">tooview hello reverse DNS property for an existing Cloud Service:</span></span>

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a><span data-ttu-id="44864-168">Ta bort omvänd DNS från befintliga molntjänster</span><span class="sxs-lookup"><span data-stu-id="44864-168">Remove reverse DNS from existing Cloud Services</span></span>

<span data-ttu-id="44864-169">tooremove en omvänd DNS-egenskap från en befintlig molntjänst:</span><span class="sxs-lookup"><span data-stu-id="44864-169">tooremove a reverse DNS property from an existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a><span data-ttu-id="44864-170">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="44864-170">FAQ</span></span>

### <a name="how-much-do-reverse-dns-records-cost"></a><span data-ttu-id="44864-171">Hur mycket omvänd DNS-poster kostnaden?</span><span class="sxs-lookup"><span data-stu-id="44864-171">How much do reverse DNS records cost?</span></span>

<span data-ttu-id="44864-172">De är kostnadsfritt!</span><span class="sxs-lookup"><span data-stu-id="44864-172">They're free!</span></span>  <span data-ttu-id="44864-173">Det finns utan extra kostnad för omvänd DNS-poster eller frågor.</span><span class="sxs-lookup"><span data-stu-id="44864-173">There is no additional cost for reverse DNS records or queries.</span></span>

### <a name="will-my-reverse-dns-records-resolve-from-hello-internet"></a><span data-ttu-id="44864-174">Kommer min omvänd DNS-poster Lös från hello internet?</span><span class="sxs-lookup"><span data-stu-id="44864-174">Will my reverse DNS records resolve from hello internet?</span></span>

<span data-ttu-id="44864-175">Ja.</span><span class="sxs-lookup"><span data-stu-id="44864-175">Yes.</span></span> <span data-ttu-id="44864-176">När du har angett hello omvänd DNS-egenskapen för din Azure-tjänst Azure hanterar alla hello DNS-delegeringar och DNS-zoner krävs tooensure som omvänd DNS-post matchas för alla användare på Internet.</span><span class="sxs-lookup"><span data-stu-id="44864-176">Once you set hello reverse DNS property for your Azure service, Azure manages all hello DNS delegations and DNS zones required tooensure that reverse DNS record resolves for all Internet users.</span></span>

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a><span data-ttu-id="44864-177">Skapas standard omvänd DNS-poster för min Azure-tjänster?</span><span class="sxs-lookup"><span data-stu-id="44864-177">Are default reverse DNS records created for my Azure services?</span></span>

<span data-ttu-id="44864-178">Nej.</span><span class="sxs-lookup"><span data-stu-id="44864-178">No.</span></span> <span data-ttu-id="44864-179">Omvänd DNS är en opt-funktion.</span><span class="sxs-lookup"><span data-stu-id="44864-179">Reverse DNS is an opt-in feature.</span></span> <span data-ttu-id="44864-180">Ingen standard omvänd DNS-poster skapas om du väljer att inte tooconfigure dem.</span><span class="sxs-lookup"><span data-stu-id="44864-180">No default reverse DNS records are created if you choose not tooconfigure them.</span></span>

### <a name="what-is-hello-format-for-hello-fully-qualified-domain-name-fqdn"></a><span data-ttu-id="44864-181">Vad är hello format för hello fullständigt kvalificerat domännamn (FQDN)?</span><span class="sxs-lookup"><span data-stu-id="44864-181">What is hello format for hello fully-qualified domain name (FQDN)?</span></span>

<span data-ttu-id="44864-182">FQDN anges i vanlig ordning och måste avslutas med en punkt (till exempel ”app1.contoso.com”.).</span><span class="sxs-lookup"><span data-stu-id="44864-182">FQDNs are specified in forward order, and must be terminated by a dot (for example, "app1.contoso.com.").</span></span>

### <a name="what-happens-if-hello-validation-check-for-hello-reverse-dns-ive-specified-fails"></a><span data-ttu-id="44864-183">Vad händer om hello valideringskontrollen för hello omvänd DNS som jag har angett misslyckas?</span><span class="sxs-lookup"><span data-stu-id="44864-183">What happens if hello validation check for hello reverse DNS I've specified fails?</span></span>

<span data-ttu-id="44864-184">Om hello omvänd DNS-verifieringen misslyckas, misslyckas hello åtgärden tooconfigure hello omvänd DNS-post.</span><span class="sxs-lookup"><span data-stu-id="44864-184">Where hello reverse DNS validation check fails, hello operation tooconfigure hello reverse DNS record fails.</span></span> <span data-ttu-id="44864-185">Korrigera hello omvänd DNS-värde som krävs och försök igen.</span><span class="sxs-lookup"><span data-stu-id="44864-185">Correct hello reverse DNS value as required, and retry.</span></span>

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a><span data-ttu-id="44864-186">Kan jag konfigurera omvänd DNS för Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="44864-186">Can I configure reverse DNS for Azure App Service?</span></span>

<span data-ttu-id="44864-187">Nej.</span><span class="sxs-lookup"><span data-stu-id="44864-187">No.</span></span> <span data-ttu-id="44864-188">Omvänd DNS stöds inte för hello Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="44864-188">Reverse DNS is not supported for hello Azure App Service.</span></span>

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a><span data-ttu-id="44864-189">Kan jag konfigurera flera omvänd DNS-poster för min Azure-tjänsten?</span><span class="sxs-lookup"><span data-stu-id="44864-189">Can I configure multiple reverse DNS records for my Azure service?</span></span>

<span data-ttu-id="44864-190">Nej.</span><span class="sxs-lookup"><span data-stu-id="44864-190">No.</span></span> <span data-ttu-id="44864-191">Azure stöder en omvänd DNS-post för varje Azure-molntjänst eller en PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="44864-191">Azure supports a single reverse DNS record for each Azure Cloud Service or PublicIpAddress.</span></span>

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a><span data-ttu-id="44864-192">Kan jag konfigurera omvänd DNS för IPv6-PublicIpAddress resurser?</span><span class="sxs-lookup"><span data-stu-id="44864-192">Can I configure reverse DNS for IPv6 PublicIpAddress resources?</span></span>

<span data-ttu-id="44864-193">Nej.</span><span class="sxs-lookup"><span data-stu-id="44864-193">No.</span></span> <span data-ttu-id="44864-194">Azure för närvarande har stöd för omvänd DNS endast för IPv4 PublicIpAddress resurser och molntjänster.</span><span class="sxs-lookup"><span data-stu-id="44864-194">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources and Cloud Services.</span></span>

### <a name="can-i-send-emails-tooexternal-domains-from-my-azure-compute-services"></a><span data-ttu-id="44864-195">Kan jag skicka e-postmeddelanden tooexternal domäner från min Azure Compute-tjänster?</span><span class="sxs-lookup"><span data-stu-id="44864-195">Can I send emails tooexternal domains from my Azure Compute services?</span></span>

<span data-ttu-id="44864-196">Nej.</span><span class="sxs-lookup"><span data-stu-id="44864-196">No.</span></span> [<span data-ttu-id="44864-197">Skicka e-postmeddelanden tooexternal domäner har inte stöd för Azure Compute-tjänster</span><span class="sxs-lookup"><span data-stu-id="44864-197">Azure Compute services do not support sending emails tooexternal domains</span></span>](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a><span data-ttu-id="44864-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44864-198">Next steps</span></span>

<span data-ttu-id="44864-199">Läs mer om omvänd DNS [omvänd DNS-sökning på Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="44864-199">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="44864-200">Lär dig hur för[värden hello zon för omvänd sökning för din ISP-tilldelad IP-adressintervall i Azure DNS](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="44864-200">Learn how too[host hello reverse lookup zone for your ISP-assigned IP range in Azure DNS](dns-reverse-dns-for-azure-services.md).</span></span>

