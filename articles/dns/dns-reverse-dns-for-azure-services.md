---
title: "Omvänd DNS för Azure-tjänster | Microsoft Docs"
description: "Lär dig hur du konfigurerar omvänd DNS-sökning för tjänster i Azure"
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
ms.openlocfilehash: 63701e1ce0c1c6dcf2ce02ebce272b8280395e7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a><span data-ttu-id="7da85-103">Konfigurera omvänd DNS för tjänster i Azure</span><span class="sxs-lookup"><span data-stu-id="7da85-103">Configure reverse DNS for services hosted in Azure</span></span>

<span data-ttu-id="7da85-104">Den här artikeln förklarar hur du konfigurerar omvänd DNS-sökning för tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="7da85-104">This article explains how to configure reverse DNS lookups for services hosted in Azure.</span></span>

<span data-ttu-id="7da85-105">Tjänster i Azure använda IP-adresser tilldelas av Azure och som ägs av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7da85-105">Services in Azure use IP addresses assigned by Azure and owned by Microsoft.</span></span> <span data-ttu-id="7da85-106">Dessa omvänd DNS-poster (PTR-poster) måste skapas i de motsvarande ägs av Microsoft omvänd DNS-zonerna för sökning.</span><span class="sxs-lookup"><span data-stu-id="7da85-106">These reverse DNS records (PTR records) must be created in the corresponding Microsoft-owned reverse DNS lookup zones.</span></span> <span data-ttu-id="7da85-107">Den här artikeln förklarar hur du gör detta.</span><span class="sxs-lookup"><span data-stu-id="7da85-107">This article explains how to do this.</span></span>

<span data-ttu-id="7da85-108">Det här scenariot ska inte förväxlas med möjlighet att [värd zoner omvänd DNS-sökning för din tilldelade IP-adressintervall i Azure DNS](dns-reverse-dns-hosting.md).</span><span class="sxs-lookup"><span data-stu-id="7da85-108">This scenario should not be confused with the ability to [host the reverse DNS lookup zones for your assigned IP ranges in Azure DNS](dns-reverse-dns-hosting.md).</span></span> <span data-ttu-id="7da85-109">I det här fallet måste IP-adressintervall som representeras av zon för omvänd sökning tilldelas för din organisation, vanligtvis av din Internetleverantör.</span><span class="sxs-lookup"><span data-stu-id="7da85-109">In this case, the IP ranges represented by the reverse lookup zone must be assigned to your organization, typically by your ISP.</span></span>

<span data-ttu-id="7da85-110">Innan du läser den här artikeln bör du vara bekant med den här [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7da85-110">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="7da85-111">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7da85-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
* <span data-ttu-id="7da85-112">I Resource Manager-distributionsmodellen exponeras beräkningsresurser (till exempel virtuella datorer, virtuella datorer eller Service Fabric-kluster) via en PublicIpAddress-resurs.</span><span class="sxs-lookup"><span data-stu-id="7da85-112">In the Resource Manager deployment model, compute resources (such as virtual machines, virtual machine scale sets, or Service Fabric clusters) are exposed via a PublicIpAddress resource.</span></span> <span data-ttu-id="7da85-113">Omvänd DNS-sökningar har konfigurerats med hjälp av egenskapen 'ReverseFqdn' för PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="7da85-113">Reverse DNS lookups are configured using the 'ReverseFqdn' property of the PublicIpAddress.</span></span>
* <span data-ttu-id="7da85-114">I den klassiska distributionsmodellen exponeras beräkningsresurser med molntjänster.</span><span class="sxs-lookup"><span data-stu-id="7da85-114">In the Classic deployment model, compute resources are exposed using Cloud Services.</span></span> <span data-ttu-id="7da85-115">Omvänd DNS-sökningar har konfigurerats med hjälp av egenskapen 'ReverseDnsFqdn' för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="7da85-115">Reverse DNS lookups are configured using the 'ReverseDnsFqdn' property of the Cloud Service.</span></span>

<span data-ttu-id="7da85-116">Omvänd DNS stöds inte för Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7da85-116">Reverse DNS is not currently supported for the Azure App Service.</span></span>

## <a name="validation-of-reverse-dns-records"></a><span data-ttu-id="7da85-117">Validering av omvänd DNS-poster</span><span class="sxs-lookup"><span data-stu-id="7da85-117">Validation of reverse DNS records</span></span>

<span data-ttu-id="7da85-118">En tredje part ska inte skapa omvänd DNS-poster för sina Azure-mappning till DNS-domäner.</span><span class="sxs-lookup"><span data-stu-id="7da85-118">A third party should not be able to create reverse DNS records for their Azure service mapping to your DNS domains.</span></span> <span data-ttu-id="7da85-119">Om du vill förhindra detta Azure bara kan skapas en omvänd DNS-post där domännamnet som anges i omvänd DNS-posten är samma som eller matchas till DNS-namn eller IP-adressen för en molntjänst eller en PublicIpAddress i samma Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7da85-119">To prevent this, Azure only allows the creation of a reverse DNS record where domain name specified in the reverse DNS record is the same as, or resolves to, the DNS name or IP address of a PublicIpAddress or Cloud Service in the same Azure subscription.</span></span>

<span data-ttu-id="7da85-120">Verifieringen utförs endast när omvänd DNS-posten anges eller ändras.</span><span class="sxs-lookup"><span data-stu-id="7da85-120">This validation is only performed when the reverse DNS record is set or modified.</span></span> <span data-ttu-id="7da85-121">Periodiska ny verifiering utförs inte.</span><span class="sxs-lookup"><span data-stu-id="7da85-121">Periodic re-validation is not performed.</span></span>

<span data-ttu-id="7da85-122">Exempel: Anta att PublicIpAddress-resursen har contosoapp1.northus.cloudapp.azure.com för DNS-namn och IP-adress 23.96.52.53.</span><span class="sxs-lookup"><span data-stu-id="7da85-122">For example: suppose the PublicIpAddress resource has the DNS name contosoapp1.northus.cloudapp.azure.com and IP address 23.96.52.53.</span></span> <span data-ttu-id="7da85-123">ReverseFqdn för PublicIpAddress kan anges som:</span><span class="sxs-lookup"><span data-stu-id="7da85-123">The ReverseFqdn for the PublicIpAddress can be specified as:</span></span>
* <span data-ttu-id="7da85-124">DNS-namn för PublicIpAddress contosoapp1.northus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="7da85-124">The DNS name for the PublicIpAddress, contosoapp1.northus.cloudapp.azure.com</span></span>
* <span data-ttu-id="7da85-125">DNS-namn för en annan PublicIpAddress med samma prenumeration, till exempel contosoapp2.westus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="7da85-125">The DNS name for a different PublicIpAddress in the same subscription, such as contosoapp2.westus.cloudapp.azure.com</span></span>
* <span data-ttu-id="7da85-126">En alternativa DNS-namn, till exempel app1.contoso.com, så länge det här namnet är *första* konfigurerad som en CNAME-post till contosoapp1.northus.cloudapp.azure.com eller till en annan PublicIpAddress med samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7da85-126">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as a CNAME to contosoapp1.northus.cloudapp.azure.com, or to a different PublicIpAddress in the same subscription.</span></span>
* <span data-ttu-id="7da85-127">En alternativa DNS-namn, till exempel app1.contoso.com, så länge det här namnet är *första* konfigurerad som en A-post till IP-adressen 23.96.52.53 eller IP-adressen för en annan PublicIpAddress med samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7da85-127">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as an A record to the IP address 23.96.52.53, or to the IP address of a different PublicIpAddress in the same subscription.</span></span>

<span data-ttu-id="7da85-128">Samma begränsningar gäller för omvänd DNS för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="7da85-128">The same constraints apply to reverse DNS for Cloud Services.</span></span>


## <a name="reverse-dns-for-publicipaddress-resources"></a><span data-ttu-id="7da85-129">Omvänd DNS för PublicIpAddress-resurser</span><span class="sxs-lookup"><span data-stu-id="7da85-129">Reverse DNS for PublicIpAddress resources</span></span>

<span data-ttu-id="7da85-130">Det här avsnittet innehåller detaljerade anvisningar om hur du konfigurerar omvänd DNS för PublicIpAddress resurser i Resource Manager-distributionsmodellen, med hjälp av Azure PowerShell, Azure CLI 1.0 eller 2.0 för Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="7da85-130">This section provides detailed instructions for how to configure reverse DNS for PublicIpAddress resources in the Resource Manager deployment model, using either Azure PowerShell, Azure CLI 1.0, or Azure CLI 2.0.</span></span> <span data-ttu-id="7da85-131">Konfigurera omvänd DNS för PublicIpAddress resurser stöds inte för närvarande via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="7da85-131">Configuring reverse DNS for PublicIpAddress resources is not currently supported via the Azure portal.</span></span>

<span data-ttu-id="7da85-132">Azure för närvarande har stöd för omvänd DNS endast för IPv4 PublicIpAddress resurser.</span><span class="sxs-lookup"><span data-stu-id="7da85-132">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources.</span></span> <span data-ttu-id="7da85-133">Det finns inte stöd för IPv6.</span><span class="sxs-lookup"><span data-stu-id="7da85-133">It is not supported for IPv6.</span></span>

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a><span data-ttu-id="7da85-134">Lägg till omvänd DNS i en befintlig PublicIpAddresses</span><span class="sxs-lookup"><span data-stu-id="7da85-134">Add reverse DNS to an existing PublicIpAddresses</span></span>

#### <a name="powershell"></a><span data-ttu-id="7da85-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7da85-135">PowerShell</span></span>

<span data-ttu-id="7da85-136">Lägga till omvänd DNS i en befintlig PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="7da85-136">To add reverse DNS to an existing PublicIpAddress:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

<span data-ttu-id="7da85-137">Lägg till omvänd DNS i en befintlig PublicIpAddress som inte redan har ett DNS-namn, måste du också ange ett DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="7da85-137">To add reverse DNS to an existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="7da85-138">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7da85-138">Azure CLI 1.0</span></span>

<span data-ttu-id="7da85-139">Lägga till omvänd DNS i en befintlig PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="7da85-139">To add reverse DNS to an existing PublicIpAddress:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="7da85-140">Lägg till omvänd DNS i en befintlig PublicIpAddress som inte redan har ett DNS-namn, måste du också ange ett DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="7da85-140">To add reverse DNS to an existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="7da85-141">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7da85-141">Azure CLI 2.0</span></span>

<span data-ttu-id="7da85-142">Lägga till omvänd DNS i en befintlig PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="7da85-142">To add reverse DNS to an existing PublicIpAddress:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="7da85-143">Lägg till omvänd DNS i en befintlig PublicIpAddress som inte redan har ett DNS-namn, måste du också ange ett DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="7da85-143">To add reverse DNS to an existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a><span data-ttu-id="7da85-144">Skapa en offentlig IP-adress med omvänd DNS</span><span class="sxs-lookup"><span data-stu-id="7da85-144">Create a Public IP Address with reverse DNS</span></span>

<span data-ttu-id="7da85-145">Skapa en ny PublicIpAddress med omvänd DNS-egenskapen har redan angetts:</span><span class="sxs-lookup"><span data-stu-id="7da85-145">To create a new PublicIpAddress with the reverse DNS property already specified:</span></span>

#### <a name="powershell"></a><span data-ttu-id="7da85-146">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7da85-146">PowerShell</span></span>

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a><span data-ttu-id="7da85-147">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7da85-147">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="7da85-148">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7da85-148">Azure CLI 2.0</span></span>

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a><span data-ttu-id="7da85-149">Visa omvända DNS för en befintlig PublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="7da85-149">View reverse DNS for an existing PublicIpAddress</span></span>

<span data-ttu-id="7da85-150">Visa det konfigurerade värdet för en befintlig PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="7da85-150">To view the configured value for an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="7da85-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7da85-151">PowerShell</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a><span data-ttu-id="7da85-152">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7da85-152">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a><span data-ttu-id="7da85-153">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7da85-153">Azure CLI 2.0</span></span>

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a><span data-ttu-id="7da85-154">Ta bort omvänd DNS från befintliga offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="7da85-154">Remove reverse DNS from existing Public IP Addresses</span></span>

<span data-ttu-id="7da85-155">Ta bort en omvänd DNS-egenskap från en befintlig PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="7da85-155">To remove a reverse DNS property from an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="7da85-156">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7da85-156">PowerShell</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="7da85-157">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7da85-157">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a><span data-ttu-id="7da85-158">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7da85-158">Azure CLI 2.0</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a><span data-ttu-id="7da85-159">Konfigurera omvänd DNS för molntjänster</span><span class="sxs-lookup"><span data-stu-id="7da85-159">Configure reverse DNS for Cloud Services</span></span>

<span data-ttu-id="7da85-160">Det här avsnittet innehåller detaljerade anvisningar om hur du konfigurerar omvänd DNS för molntjänster i den klassiska distributionsmodellen med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7da85-160">This section provides detailed instructions for how to configure reverse DNS for Cloud Services in the Classic deployment model, using Azure PowerShell.</span></span> <span data-ttu-id="7da85-161">Konfigurera omvänd DNS för molntjänster stöds inte via Azure portal, Azure CLI 1.0 eller 2.0 för Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="7da85-161">Configuring reverse DNS for Cloud Services is not supported via the Azure portal, Azure CLI 1.0, or Azure CLI 2.0.</span></span>

### <a name="add-reverse-dns-to-existing-cloud-services"></a><span data-ttu-id="7da85-162">Lägg till omvänd DNS i befintliga molntjänster</span><span class="sxs-lookup"><span data-stu-id="7da85-162">Add reverse DNS to existing Cloud Services</span></span>

<span data-ttu-id="7da85-163">Lägga till en omvänd DNS-post i en befintlig molntjänst:</span><span class="sxs-lookup"><span data-stu-id="7da85-163">To add a reverse DNS record to an existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a><span data-ttu-id="7da85-164">Skapa en molntjänst med omvänd DNS</span><span class="sxs-lookup"><span data-stu-id="7da85-164">Create a Cloud Service with reverse DNS</span></span>

<span data-ttu-id="7da85-165">Skapa en ny molntjänst med omvänd DNS-egenskapen har redan angetts:</span><span class="sxs-lookup"><span data-stu-id="7da85-165">To create a new Cloud Service with the reverse DNS property already specified:</span></span>

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a><span data-ttu-id="7da85-166">Visa omvända DNS för befintliga molntjänster</span><span class="sxs-lookup"><span data-stu-id="7da85-166">View reverse DNS for existing Cloud Services</span></span>

<span data-ttu-id="7da85-167">Visa omvända DNS-egenskapen för en befintlig molntjänst:</span><span class="sxs-lookup"><span data-stu-id="7da85-167">To view the reverse DNS property for an existing Cloud Service:</span></span>

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a><span data-ttu-id="7da85-168">Ta bort omvänd DNS från befintliga molntjänster</span><span class="sxs-lookup"><span data-stu-id="7da85-168">Remove reverse DNS from existing Cloud Services</span></span>

<span data-ttu-id="7da85-169">Ta bort en omvänd DNS-egenskap från en befintlig molntjänst:</span><span class="sxs-lookup"><span data-stu-id="7da85-169">To remove a reverse DNS property from an existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a><span data-ttu-id="7da85-170">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="7da85-170">FAQ</span></span>

### <a name="how-much-do-reverse-dns-records-cost"></a><span data-ttu-id="7da85-171">Hur mycket omvänd DNS-poster kostnaden?</span><span class="sxs-lookup"><span data-stu-id="7da85-171">How much do reverse DNS records cost?</span></span>

<span data-ttu-id="7da85-172">De är kostnadsfritt!</span><span class="sxs-lookup"><span data-stu-id="7da85-172">They're free!</span></span>  <span data-ttu-id="7da85-173">Det finns utan extra kostnad för omvänd DNS-poster eller frågor.</span><span class="sxs-lookup"><span data-stu-id="7da85-173">There is no additional cost for reverse DNS records or queries.</span></span>

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a><span data-ttu-id="7da85-174">Löser omvänd DNS-poster från internet?</span><span class="sxs-lookup"><span data-stu-id="7da85-174">Will my reverse DNS records resolve from the internet?</span></span>

<span data-ttu-id="7da85-175">Ja.</span><span class="sxs-lookup"><span data-stu-id="7da85-175">Yes.</span></span> <span data-ttu-id="7da85-176">När du har angett egenskapen DNS för din Azure-tjänst, hanterar Azure alla DNS-delegeringar och DNS-zoner som krävs för att säkerställa att omvänd DNS-post matchas för alla användare på Internet.</span><span class="sxs-lookup"><span data-stu-id="7da85-176">Once you set the reverse DNS property for your Azure service, Azure manages all the DNS delegations and DNS zones required to ensure that reverse DNS record resolves for all Internet users.</span></span>

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a><span data-ttu-id="7da85-177">Skapas standard omvänd DNS-poster för min Azure-tjänster?</span><span class="sxs-lookup"><span data-stu-id="7da85-177">Are default reverse DNS records created for my Azure services?</span></span>

<span data-ttu-id="7da85-178">Nej.</span><span class="sxs-lookup"><span data-stu-id="7da85-178">No.</span></span> <span data-ttu-id="7da85-179">Omvänd DNS är en opt-funktion.</span><span class="sxs-lookup"><span data-stu-id="7da85-179">Reverse DNS is an opt-in feature.</span></span> <span data-ttu-id="7da85-180">Ingen standard omvänd DNS-poster skapas om du inte väljer att konfigurera dem.</span><span class="sxs-lookup"><span data-stu-id="7da85-180">No default reverse DNS records are created if you choose not to configure them.</span></span>

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a><span data-ttu-id="7da85-181">Vad är formatet för det fullständigt kvalificerade domännamnet (FQDN)?</span><span class="sxs-lookup"><span data-stu-id="7da85-181">What is the format for the fully-qualified domain name (FQDN)?</span></span>

<span data-ttu-id="7da85-182">FQDN anges i vanlig ordning och måste avslutas med en punkt (till exempel ”app1.contoso.com”.).</span><span class="sxs-lookup"><span data-stu-id="7da85-182">FQDNs are specified in forward order, and must be terminated by a dot (for example, "app1.contoso.com.").</span></span>

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a><span data-ttu-id="7da85-183">Vad händer om valideringskontrollen för omvänd DNS jag har angett misslyckas?</span><span class="sxs-lookup"><span data-stu-id="7da85-183">What happens if the validation check for the reverse DNS I've specified fails?</span></span>

<span data-ttu-id="7da85-184">Om omvänd DNS-verifieringen misslyckas, misslyckas åtgärden att konfigurera omvänd DNS-posten.</span><span class="sxs-lookup"><span data-stu-id="7da85-184">Where the reverse DNS validation check fails, the operation to configure the reverse DNS record fails.</span></span> <span data-ttu-id="7da85-185">Korrigera omvänd DNS-värde som krävs och försök igen.</span><span class="sxs-lookup"><span data-stu-id="7da85-185">Correct the reverse DNS value as required, and retry.</span></span>

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a><span data-ttu-id="7da85-186">Kan jag konfigurera omvänd DNS för Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="7da85-186">Can I configure reverse DNS for Azure App Service?</span></span>

<span data-ttu-id="7da85-187">Nej.</span><span class="sxs-lookup"><span data-stu-id="7da85-187">No.</span></span> <span data-ttu-id="7da85-188">Omvänd DNS stöds inte för Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7da85-188">Reverse DNS is not supported for the Azure App Service.</span></span>

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a><span data-ttu-id="7da85-189">Kan jag konfigurera flera omvänd DNS-poster för min Azure-tjänsten?</span><span class="sxs-lookup"><span data-stu-id="7da85-189">Can I configure multiple reverse DNS records for my Azure service?</span></span>

<span data-ttu-id="7da85-190">Nej.</span><span class="sxs-lookup"><span data-stu-id="7da85-190">No.</span></span> <span data-ttu-id="7da85-191">Azure stöder en omvänd DNS-post för varje Azure-molntjänst eller en PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="7da85-191">Azure supports a single reverse DNS record for each Azure Cloud Service or PublicIpAddress.</span></span>

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a><span data-ttu-id="7da85-192">Kan jag konfigurera omvänd DNS för IPv6-PublicIpAddress resurser?</span><span class="sxs-lookup"><span data-stu-id="7da85-192">Can I configure reverse DNS for IPv6 PublicIpAddress resources?</span></span>

<span data-ttu-id="7da85-193">Nej.</span><span class="sxs-lookup"><span data-stu-id="7da85-193">No.</span></span> <span data-ttu-id="7da85-194">Azure för närvarande har stöd för omvänd DNS endast för IPv4 PublicIpAddress resurser och molntjänster.</span><span class="sxs-lookup"><span data-stu-id="7da85-194">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources and Cloud Services.</span></span>

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a><span data-ttu-id="7da85-195">Kan jag skicka e-post till externa domäner från min Azure Compute-tjänster?</span><span class="sxs-lookup"><span data-stu-id="7da85-195">Can I send emails to external domains from my Azure Compute services?</span></span>

<span data-ttu-id="7da85-196">Nej.</span><span class="sxs-lookup"><span data-stu-id="7da85-196">No.</span></span> [<span data-ttu-id="7da85-197">Skicka e-post till externa domäner har inte stöd för Azure Compute-tjänster</span><span class="sxs-lookup"><span data-stu-id="7da85-197">Azure Compute services do not support sending emails to external domains</span></span>](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a><span data-ttu-id="7da85-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7da85-198">Next steps</span></span>

<span data-ttu-id="7da85-199">Läs mer om omvänd DNS [omvänd DNS-sökning på Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="7da85-199">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="7da85-200">Lär dig hur du [värd zon för omvänd sökning för din ISP-tilldelad IP-adressintervall i Azure DNS](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="7da85-200">Learn how to [host the reverse lookup zone for your ISP-assigned IP range in Azure DNS](dns-reverse-dns-for-azure-services.md).</span></span>

