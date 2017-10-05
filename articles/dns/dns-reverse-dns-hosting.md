---
title: "Värd för zoner för omvänd DNS-sökning i Azure DNS | Microsoft Docs"
description: "Lär dig hur du använder Azure DNS som värd för omvänd DNS-sökningszoner för IP-adressintervall"
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
ms.openlocfilehash: 3e10b25d2f9b91c96af2958fef6dc6a4fdbff301
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a><span data-ttu-id="45ea6-103">Värd för omvänd sökning DNS-zoner i Azure DNS</span><span class="sxs-lookup"><span data-stu-id="45ea6-103">Hosting reverse DNS lookup zones in Azure DNS</span></span>

<span data-ttu-id="45ea6-104">Den här artikeln beskriver hur du värd zoner omvänd DNS-sökning för din tilldelade IP-adressintervall i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="45ea6-104">This article explains how to host the reverse DNS lookup zones for your assigned IP ranges in Azure DNS.</span></span> <span data-ttu-id="45ea6-105">IP-adressintervall som representeras av zon för omvänd sökning måste tilldelas för din organisation, vanligtvis av din Internetleverantör.</span><span class="sxs-lookup"><span data-stu-id="45ea6-105">The IP ranges represented by the reverse lookup zone must be assigned to your organization, typically by your ISP.</span></span>

<span data-ttu-id="45ea6-106">Om du vill konfigurera omvänd DNS för Azure-ägs IP-adressen till din Azure-tjänst finns [konfigurera omvänd sökning för IP-adresser som allokerats till Azure-tjänstens](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="45ea6-106">To configure reverse DNS for Azure-owned IP address assigned to your Azure service, see [configure the reverse lookup for the IP addresses allocated to your Azure service](dns-reverse-dns-for-azure-services.md).</span></span>

<span data-ttu-id="45ea6-107">Innan du läser den här artikeln bör du vara bekant med den här [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="45ea6-107">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="45ea6-108">Den här artikeln vägleder dig genom stegen för att skapa din första omvänd sökning DNS-zon och registrera med Azure-portalen, Azure PowerShell, Azure CLI 1.0 eller 2.0 för Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="45ea6-108">This article walks you through the steps to create your first reverse lookup DNS zone and record using the Azure portal, Azure PowerShell, Azure CLI 1.0 or Azure CLI 2.0.</span></span>

## <a name="create-a-reverse-lookup-dns-zone"></a><span data-ttu-id="45ea6-109">Skapa en DNS-zon för omvänd sökning</span><span class="sxs-lookup"><span data-stu-id="45ea6-109">Create a reverse lookup DNS zone</span></span>

1. <span data-ttu-id="45ea6-110">Logga in på den [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="45ea6-110">Sign in to the [Azure portal](https://portal.azure.com)</span></span>
1. <span data-ttu-id="45ea6-111">På navmenyn klickar du på och klicka på **ny** > **nätverk** > och klicka sedan på **DNS-zonen** att öppna den **skapa DNS-zonen** bladet.</span><span class="sxs-lookup"><span data-stu-id="45ea6-111">On the Hub menu, click and click **New** > **Networking** > and then click **DNS zone** to open the **Create DNS zone** blade.</span></span>

   ![DNS-zon](./media/dns-reverse-dns-hosting/figure1.png)

1. <span data-ttu-id="45ea6-113">På den **skapa DNS-zonen** bladet namnge din DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="45ea6-113">On the **Create DNS zone** blade, name your DNS zone.</span></span> <span data-ttu-id="45ea6-114">Namnet på zonen är utformade på olika sätt för IPv4 och IPv6-prefix.</span><span class="sxs-lookup"><span data-stu-id="45ea6-114">The name of the zone is crafted differently for IPv4 and IPv6 prefixes.</span></span> <span data-ttu-id="45ea6-115">Följ instruktionerna för [IPV4](#ipv4) eller [IPv6](#ipv6) att namnge din tidszon.</span><span class="sxs-lookup"><span data-stu-id="45ea6-115">Use either the instructions for [IPV4](#ipv4) or [IPv6](#ipv6) to name your zone.</span></span> <span data-ttu-id="45ea6-116">När du är klar klickar du på **skapa** att skapa zonen.</span><span class="sxs-lookup"><span data-stu-id="45ea6-116">When complete click **Create** to create the zone.</span></span>

### <a name="ipv4"></a><span data-ttu-id="45ea6-117">IPv4</span><span class="sxs-lookup"><span data-stu-id="45ea6-117">IPv4</span></span>

<span data-ttu-id="45ea6-118">Namnet på en IPv4 zon för omvänd sökning baseras på IP-adressintervall som representerar.</span><span class="sxs-lookup"><span data-stu-id="45ea6-118">The name of an IPv4 reverse lookup zone is based on the IP range it represents.</span></span> <span data-ttu-id="45ea6-119">Det bör vara i följande format: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="45ea6-119">It should be in the following format: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span></span> <span data-ttu-id="45ea6-120">Exempel finns i [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md#ipv4).</span><span class="sxs-lookup"><span data-stu-id="45ea6-120">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv4).</span></span>

> [!NOTE]
> <span data-ttu-id="45ea6-121">När du skapar classless omvänd DNS-zoner för sökning i Azure DNS, måste du använda ett bindestreck (`-`) i stället för ett snedstreck ('/ ') i zonnamnet.</span><span class="sxs-lookup"><span data-stu-id="45ea6-121">When creating classless reverse DNS lookup zones in Azure DNS, you must use a hyphen (`-`) rather than a forward slash ('/') in the zone name.</span></span>
>
> <span data-ttu-id="45ea6-122">Till exempel för IP-intervallet 192.0.2.128/26, du måste använda `128-26.2.0.192.in-addr.arpa` som zonnamnet i stället för `128/26.2.0.192.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="45ea6-122">For example, for the IP range 192.0.2.128/26, you must use `128-26.2.0.192.in-addr.arpa` as the zone name instead of `128/26.2.0.192.in-addr.arpa`.</span></span>
>
> <span data-ttu-id="45ea6-123">Detta beror på att när båda stöds av DNS-standarden DNS zonen namn som innehåller snedstreck (`/`) tecken stöds inte i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="45ea6-123">This is because, while both are supported by the DNS standards, DNS zone names containing the forward slash (`/`) character are not supported in Azure DNS.</span></span>

<span data-ttu-id="45ea6-124">I följande exempel visas hur du skapar en klass C omvänd DNS-zon som heter `2.0.192.in-addr.arpa` i Azure DNS via Azure portal:</span><span class="sxs-lookup"><span data-stu-id="45ea6-124">The following example shows how to create a 'Class C' reverse DNS zone named `2.0.192.in-addr.arpa` in Azure DNS via the Azure portal:</span></span>

 ![Skapa DNS-zon](./media/dns-reverse-dns-hosting/figure2.png)

<span data-ttu-id="45ea6-126">'Resursgruppens plats' definierar plats för resursgruppen och har ingen inverkan på DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="45ea6-126">The 'Resource group location' defines the location for the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="45ea6-127">Plats för DNS-zonen är alltid ”globala' och visas inte.</span><span class="sxs-lookup"><span data-stu-id="45ea6-127">The DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="45ea6-128">Följande exempel visar hur du utför den här uppgiften med Azure PowerShell och Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="45ea6-128">The following examples show how to complete this task with Azure PowerShell and the Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="45ea6-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="45ea6-129">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="45ea6-130">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-130">Azure CLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="45ea6-131">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-131">Azure CLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="45ea6-132">IPv6</span><span class="sxs-lookup"><span data-stu-id="45ea6-132">IPv6</span></span>

<span data-ttu-id="45ea6-133">Namnet på en zon för omvänd sökning IPv6 bör vara i följande format: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span><span class="sxs-lookup"><span data-stu-id="45ea6-133">The name of an IPv6 reverse lookup zone should be in the following form: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span></span>  <span data-ttu-id="45ea6-134">Exempel finns i [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md#ipv6).</span><span class="sxs-lookup"><span data-stu-id="45ea6-134">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv6).</span></span>


<span data-ttu-id="45ea6-135">I följande exempel visas hur du skapar en IPv6 omvänd DNS-sökningszon med namnet `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` i Azure DNS via Azure portal:</span><span class="sxs-lookup"><span data-stu-id="45ea6-135">The following example shows how to create an IPv6 reverse DNS lookup zone named `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS via the Azure portal:</span></span>

 ![Skapa DNS-zon](./media/dns-reverse-dns-hosting/figure3.png)

<span data-ttu-id="45ea6-137">'Resursgruppens plats' definierar plats för resursgruppen och har ingen inverkan på DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="45ea6-137">The 'Resource group location' defines the location for the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="45ea6-138">Plats för DNS-zonen är alltid ”globala' och visas inte.</span><span class="sxs-lookup"><span data-stu-id="45ea6-138">The DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="45ea6-139">Följande exempel visar hur du utför den här uppgiften med Azure PowerShell och Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="45ea6-139">The following examples show how to complete this task with Azure PowerShell and the Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="45ea6-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="45ea6-140">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a><span data-ttu-id="45ea6-141">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-141">AzureCLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a><span data-ttu-id="45ea6-142">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-142">AzureCLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a><span data-ttu-id="45ea6-143">Delegera en zon för omvänd DNS-sökning</span><span class="sxs-lookup"><span data-stu-id="45ea6-143">Delegate a reverse DNS lookup zone</span></span>

<span data-ttu-id="45ea6-144">Att ha skapat en zon för omvänd DNS-sökning, måste du kontrollera att zonen delegeras från den överordnade zonen.</span><span class="sxs-lookup"><span data-stu-id="45ea6-144">Having created your reverse DNS lookup zone, you must ensure that the zone is delegated from the parent zone.</span></span> <span data-ttu-id="45ea6-145">DNS-delegering kan DNS-namnmatchningen att hitta namnservrar som värd för en zon för omvänd DNS-sökning.</span><span class="sxs-lookup"><span data-stu-id="45ea6-145">DNS delegation enables the DNS name resolution process to find the name servers hosting your reverse DNS lookup zone.</span></span> <span data-ttu-id="45ea6-146">Detta gör att servrarna namn att besvara omvänd DNS-frågor för IP-adresser i ditt-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="45ea6-146">This enables those name servers to answer DNS reverse queries for the IP addresses in your address range.</span></span>

<span data-ttu-id="45ea6-147">För zoner för vanlig sökning beskrivs processen för att delegera en DNS-zon i [Delegera din domän till Azure DNS](dns-delegate-domain-azure-dns.md).</span><span class="sxs-lookup"><span data-stu-id="45ea6-147">For forward lookup zones, the process of delegating a DNS zone is described in [Delegate your domain to Azure DNS](dns-delegate-domain-azure-dns.md).</span></span> <span data-ttu-id="45ea6-148">Delegering av zoner för omvänd sökning fungerar på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="45ea6-148">Delegation for reverse lookup zones works the same way.</span></span> <span data-ttu-id="45ea6-149">Den enda skillnaden är att du måste konfigurera namnservrarna med Internetleverantören som tillhandahåller IP-adressintervall i stället för domännamnsregistratorn.</span><span class="sxs-lookup"><span data-stu-id="45ea6-149">The only difference is that you need to configure the name servers with the ISP who provided your IP range, rather than your domain name registrar.</span></span>

## <a name="create-a-dns-ptr-record"></a><span data-ttu-id="45ea6-150">Skapa en DNS PTR-post</span><span class="sxs-lookup"><span data-stu-id="45ea6-150">Create a DNS PTR record</span></span>

### <a name="ipv4"></a><span data-ttu-id="45ea6-151">IPv4</span><span class="sxs-lookup"><span data-stu-id="45ea6-151">IPv4</span></span>

<span data-ttu-id="45ea6-152">I följande exempel vägleder dig genom processen att skapa en PTR-post i en omvänd DNS-zon i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="45ea6-152">The following example walks you through the process of creating a PTR record in a reverse DNS zone in Azure DNS.</span></span> <span data-ttu-id="45ea6-153">Information om andra posttyper och hur du ändrar befintliga poster finns i [Hantera DNS-poster och postuppsättningar med Azure Portal](dns-operations-recordsets-portal.md) (på engelska).</span><span class="sxs-lookup"><span data-stu-id="45ea6-153">For other record types and to modify existing records, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span>

1.  <span data-ttu-id="45ea6-154">Välj **+ Postuppsättning** längst upp på bladet **DNS-zon** för att öppna bladet **Lägg till uppsättning av poster**.</span><span class="sxs-lookup"><span data-stu-id="45ea6-154">At the top of the **DNS zone** blade, select **+ Record set** to open the **Add record set** blade.</span></span>

 ![DNS-zon](./media/dns-reverse-dns-hosting/figure4.png)

1. <span data-ttu-id="45ea6-156">På den **lägga till postuppsättning** bladet.</span><span class="sxs-lookup"><span data-stu-id="45ea6-156">On the **Add record set** blade.</span></span> 
1. <span data-ttu-id="45ea6-157">Välj **PTR** från posten ”**typen**” menyn.</span><span class="sxs-lookup"><span data-stu-id="45ea6-157">Select **PTR** from the record "**Type**" menu.</span></span>  
1. <span data-ttu-id="45ea6-158">Namnet på posten för en PTR-post måste vara resten av IPv4-adressen i omvänd ordning.</span><span class="sxs-lookup"><span data-stu-id="45ea6-158">The name of the record set for a PTR record needs to be the rest of the IPv4 address in reverse order.</span></span> <span data-ttu-id="45ea6-159">I det här exemplet fylls de tre första oktetterna redan som en del av zonnamnet (.2.0.192).</span><span class="sxs-lookup"><span data-stu-id="45ea6-159">In this example, the first three octets are already populated as part of the zone name (.2.0.192).</span></span> <span data-ttu-id="45ea6-160">Därför anges bara den sista oktetten i namnfältet.</span><span class="sxs-lookup"><span data-stu-id="45ea6-160">Therefore, only the last octet is supplied in the name field.</span></span> <span data-ttu-id="45ea6-161">Till exempel namnger du din postuppsättning ”**15**” för en resurs vars IP-adressen är 192.0.2.15.</span><span class="sxs-lookup"><span data-stu-id="45ea6-161">For example, you could name your record set "**15**" for a resource whose IP address is 192.0.2.15.</span></span>  
1. <span data-ttu-id="45ea6-162">I den ”**domännamn**”, ange det fullständigt kvalificerade domännamnet (FQDN) på resursen med hjälp av IP-Adressen.</span><span class="sxs-lookup"><span data-stu-id="45ea6-162">In the "**Domain Name**" field, enter the fully qualified domain name (FQDN) of the resource using the IP.</span></span>
1. <span data-ttu-id="45ea6-163">Klicka på **OK** längst ned på bladet för att skapa DNS-posten.</span><span class="sxs-lookup"><span data-stu-id="45ea6-163">Select **OK** at the bottom of the blade to create the DNS record.</span></span>

 ![Lägg till en postuppsättning](./media/dns-reverse-dns-hosting/figure5.png)

<span data-ttu-id="45ea6-165">Här följer några exempel på hur du utför den här uppgiften med PowerShell och AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="45ea6-165">The following are examples on how to complete this task with PowerShell and the AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="45ea6-166">PowerShell</span><span class="sxs-lookup"><span data-stu-id="45ea6-166">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a><span data-ttu-id="45ea6-167">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-167">AzureCLI 1.0</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a><span data-ttu-id="45ea6-168">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-168">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a><span data-ttu-id="45ea6-169">IPv6</span><span class="sxs-lookup"><span data-stu-id="45ea6-169">IPv6</span></span>

<span data-ttu-id="45ea6-170">I följande exempel vägleder dig genom processen att skapa ny 'PTR-post.</span><span class="sxs-lookup"><span data-stu-id="45ea6-170">The following example walks you through the process of creating new 'PTR' record.</span></span> <span data-ttu-id="45ea6-171">Information om andra posttyper och hur du ändrar befintliga poster finns i [Hantera DNS-poster och postuppsättningar med Azure Portal](dns-operations-recordsets-portal.md) (på engelska).</span><span class="sxs-lookup"><span data-stu-id="45ea6-171">For other record types and to modify existing records, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span>

1. <span data-ttu-id="45ea6-172">Längst upp i den **DNS-zonen bladet**väljer **+ postuppsättningen** att öppna den **lägga till postuppsättning** bladet.</span><span class="sxs-lookup"><span data-stu-id="45ea6-172">At the top of the **DNS zone blade**, select **+ Record set** to open the **Add record set** blade.</span></span>

  ![DNS-zonen bladet](./media/dns-reverse-dns-hosting/figure6.png)

2. <span data-ttu-id="45ea6-174">På den **lägga till postuppsättning** bladet.</span><span class="sxs-lookup"><span data-stu-id="45ea6-174">On the **Add record set** blade.</span></span> 
3. <span data-ttu-id="45ea6-175">Välj **PTR** från posten ”**typen**” menyn.</span><span class="sxs-lookup"><span data-stu-id="45ea6-175">Select **PTR** from the record "**Type**" menu.</span></span>  
4. <span data-ttu-id="45ea6-176">Namnet på posten för en PTR-post måste vara resten av IPv6-adressen i omvänd ordning.</span><span class="sxs-lookup"><span data-stu-id="45ea6-176">The name of the record set for a PTR record needs to be the rest of the IPv6 address in reverse order.</span></span> <span data-ttu-id="45ea6-177">Det får inte innehålla noll komprimering.</span><span class="sxs-lookup"><span data-stu-id="45ea6-177">It must not include any zero compression.</span></span> <span data-ttu-id="45ea6-178">I det här exemplet fylls redan första 64 bitarna i IPv6 som en del av zonnamnet (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span><span class="sxs-lookup"><span data-stu-id="45ea6-178">In this example, the first 64 bits of the IPv6 are already populated as part of the zone name (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span></span> <span data-ttu-id="45ea6-179">Därför tillhandahålls bara de senaste 64-bitarna i namnfältet.</span><span class="sxs-lookup"><span data-stu-id="45ea6-179">Therefore, only the last 64 bits are supplied in the name field.</span></span> <span data-ttu-id="45ea6-180">Senaste 64-bitars av IP-adressen anges i omvänd ordning med en period som avgränsare mellan varje hexadecimalt tal.</span><span class="sxs-lookup"><span data-stu-id="45ea6-180">The last 64 bits of the IP address are entered in reverse order, using a period as the delimiter between each hexadecimal number.</span></span> <span data-ttu-id="45ea6-181">Till exempel namnger du din postuppsättning ”**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**” för en resurs vars IP-adressen är 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span><span class="sxs-lookup"><span data-stu-id="45ea6-181">For example, you could name your record set "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" for a resource whose IP address is 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span></span>  
5. <span data-ttu-id="45ea6-182">I den ”**domännamn**”, ange det fullständigt kvalificerade domännamnet (FQDN) på resursen med hjälp av IP-Adressen.</span><span class="sxs-lookup"><span data-stu-id="45ea6-182">In the "**Domain Name**" field, enter the fully qualified domain name (FQDN) of the resource using the IP.</span></span>
6. <span data-ttu-id="45ea6-183">Klicka på **OK** längst ned på bladet för att skapa DNS-posten.</span><span class="sxs-lookup"><span data-stu-id="45ea6-183">Select **OK** at the bottom of the blade to create the DNS record.</span></span>

![postuppsättningen bladet Lägg till](./media/dns-reverse-dns-hosting/figure7.png)

<span data-ttu-id="45ea6-185">Här följer några exempel på hur du utför den här uppgiften med PowerShell och AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="45ea6-185">The following are examples on how to complete this task with PowerShell and the AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="45ea6-186">PowerShell</span><span class="sxs-lookup"><span data-stu-id="45ea6-186">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a><span data-ttu-id="45ea6-187">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-187">AzureCLI 1.0</span></span>

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a><span data-ttu-id="45ea6-188">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-188">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a><span data-ttu-id="45ea6-189">Visa poster</span><span class="sxs-lookup"><span data-stu-id="45ea6-189">View Records</span></span>

<span data-ttu-id="45ea6-190">Gå till din DNS-zonen i Azure portal om du vill visa de poster som du skapade.</span><span class="sxs-lookup"><span data-stu-id="45ea6-190">To view the records you created, navigate to your DNS zone in the Azure portal.</span></span> <span data-ttu-id="45ea6-191">I den nedre delen av den **DNS-zonen** bladet hittar du på posterna för DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="45ea6-191">In the lower part of the **DNS zone** blade, you can see the records for the DNS zone.</span></span> <span data-ttu-id="45ea6-192">Du bör se NS- och SOA-standardposterna (dessa skapas i varje zon) plus eventuella nya poster som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="45ea6-192">You should see the default NS and SOA records, which are created in every zone, plus any new records you have created.</span></span>

### <a name="ipv4"></a><span data-ttu-id="45ea6-193">IPv4</span><span class="sxs-lookup"><span data-stu-id="45ea6-193">IPv4</span></span>

<span data-ttu-id="45ea6-194">DNS-zonen bladet visar IPv4 PTR-poster:</span><span class="sxs-lookup"><span data-stu-id="45ea6-194">DNS zone blade, showing IPv4 PTR records:</span></span>

![DNS-zonen bladet](./media/dns-reverse-dns-hosting/figure8.png)

<span data-ttu-id="45ea6-196">Följande exempel visar hur du visar PTR-poster med PowerShell eller Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="45ea6-196">The following examples show how to view the PTR records using PowerShell or the Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="45ea6-197">PowerShell</span><span class="sxs-lookup"><span data-stu-id="45ea6-197">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="45ea6-198">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-198">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="45ea6-199">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-199">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="45ea6-200">IPv6</span><span class="sxs-lookup"><span data-stu-id="45ea6-200">IPv6</span></span>

<span data-ttu-id="45ea6-201">DNS-zonen bladet visar IPv6 PTR-poster:</span><span class="sxs-lookup"><span data-stu-id="45ea6-201">DNS zone blade, showing IPv6 PTR records:</span></span>

![DNS-zonen bladet](./media/dns-reverse-dns-hosting/figure9.png)

<span data-ttu-id="45ea6-203">Här följer några exempel på hur du visar posterna med PowerShell och AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="45ea6-203">The following are examples on how to view the records with PowerShell and the AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="45ea6-204">PowerShell</span><span class="sxs-lookup"><span data-stu-id="45ea6-204">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="45ea6-205">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-205">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="45ea6-206">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="45ea6-206">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a><span data-ttu-id="45ea6-207">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="45ea6-207">FAQ</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a><span data-ttu-id="45ea6-208">Kan jag vara värd zoner för omvänd DNS-sökning för min ISP-tilldelad IP-Adressblock på Azure DNS?</span><span class="sxs-lookup"><span data-stu-id="45ea6-208">Can I host reverse DNS lookup zones for my ISP-assigned IP blocks on Azure DNS?</span></span>

<span data-ttu-id="45ea6-209">Ja.</span><span class="sxs-lookup"><span data-stu-id="45ea6-209">Yes.</span></span> <span data-ttu-id="45ea6-210">Värd för zoner för omvänd sökning (ARPA) för din egen IP-adressintervall i Azure DNS stöds fullt ut.</span><span class="sxs-lookup"><span data-stu-id="45ea6-210">Hosting the reverse lookup (ARPA) zones for your own IP ranges in Azure DNS is fully supported.</span></span>

<span data-ttu-id="45ea6-211">Skapa en zon för omvänd sökning i DNS-Azure som beskrivs i den här artikeln och sedan arbeta med Leverantören [delegera zonen](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="45ea6-211">Create the reverse lookup zone in Azure DNS as explained in this article, then work with your ISP to [delegate the zone](dns-domain-delegation.md).</span></span>  <span data-ttu-id="45ea6-212">Du kan sedan hantera PTR-poster för varje omvänd sökning på samma sätt som andra typer av poster.</span><span class="sxs-lookup"><span data-stu-id="45ea6-212">You can then manage the PTR records for each reverse lookup in the same way as other record types.</span></span>

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a><span data-ttu-id="45ea6-213">Hur mycket har värd min omvänd DNS-sökning zonen kostnaden?</span><span class="sxs-lookup"><span data-stu-id="45ea6-213">How much does hosting my reverse DNS lookup zone cost?</span></span>

<span data-ttu-id="45ea6-214">Värd för DNS-zonen för omvänd sökning för din ISP-tilldelad IP-Adressblock i Azure DNS debiteras med [Azure DNS standardpriser](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="45ea6-214">Hosting the reverse DNS lookup zone for your ISP-assigned IP block in Azure DNS is charged at [standard Azure DNS rates](https://azure.microsoft.com/pricing/details/dns/).</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a><span data-ttu-id="45ea6-215">Kan jag vara värd för omvänd DNS-sökningszoner för både IPv4 och IPv6-adresser i Azure DNS?</span><span class="sxs-lookup"><span data-stu-id="45ea6-215">Can I host reverse DNS lookup zones for both IPv4 and IPv6 addresses in Azure DNS?</span></span>

<span data-ttu-id="45ea6-216">Ja.</span><span class="sxs-lookup"><span data-stu-id="45ea6-216">Yes.</span></span> <span data-ttu-id="45ea6-217">Den här artikeln beskriver hur du skapar både IPv4 och IPv6 omvänd DNS-sökning i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="45ea6-217">This article explains how to create both IPv4 and IPv6 reverse DNS lookup zones in Azure DNS.</span></span>

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a><span data-ttu-id="45ea6-218">Kan jag importera en befintlig omvänd DNS-sökningszon?</span><span class="sxs-lookup"><span data-stu-id="45ea6-218">Can I import an existing reverse DNS lookup zone?</span></span>

<span data-ttu-id="45ea6-219">Ja.</span><span class="sxs-lookup"><span data-stu-id="45ea6-219">Yes.</span></span> <span data-ttu-id="45ea6-220">Du kan använda Azure CLI för att importera befintliga DNS-zoner i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="45ea6-220">You can use the Azure CLI to import existing DNS zones into Azure DNS.</span></span> <span data-ttu-id="45ea6-221">Detta fungerar för både zoner för vanlig sökning och zoner för omvänd sökning.</span><span class="sxs-lookup"><span data-stu-id="45ea6-221">This works for both forward lookup zones and reverse lookup zones.</span></span>

<span data-ttu-id="45ea6-222">Mer information finns i [importera och exportera en DNS-zonfilen med hjälp av Azure CLI](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="45ea6-222">For more information, see [Import and export a DNS zone file using the Azure CLI](dns-import-export.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="45ea6-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="45ea6-223">Next steps</span></span>

<span data-ttu-id="45ea6-224">Läs mer om omvänd DNS [omvänd DNS-sökning på Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="45ea6-224">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="45ea6-225">Lär dig hur du [hantera omvänd DNS-posterna för din Azure-tjänster](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="45ea6-225">Learn how to [manage reverse DNS records for your Azure services](dns-reverse-dns-for-azure-services.md).</span></span>
