---
title: "aaaHosting zoner för omvänd DNS-sökning i Azure DNS | Microsoft Docs"
description: "Lär dig hur toouse Azure DNS toohost hello zoner för omvänd DNS-sökning för IP-adressintervall"
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
ms.openlocfilehash: 24feb8ef1c75a7d91938867f348fed1190046e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a><span data-ttu-id="118ad-103">Värd för omvänd sökning DNS-zoner i Azure DNS</span><span class="sxs-lookup"><span data-stu-id="118ad-103">Hosting reverse DNS lookup zones in Azure DNS</span></span>

<span data-ttu-id="118ad-104">Den här artikeln förklarar hur toohost hello zoner för omvänd DNS-sökning för din tilldelade IP-adressintervall i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="118ad-104">This article explains how toohost hello reverse DNS lookup zones for your assigned IP ranges in Azure DNS.</span></span> <span data-ttu-id="118ad-105">hello IP-adressintervall som representeras av hello zon för omvänd sökning måste tilldelas tooyour organisation, vanligtvis av din Internetleverantör.</span><span class="sxs-lookup"><span data-stu-id="118ad-105">hello IP ranges represented by hello reverse lookup zone must be assigned tooyour organization, typically by your ISP.</span></span>

<span data-ttu-id="118ad-106">tooconfigure omvänd DNS för Azure-ägs IP-adress som tilldelats tooyour Azure-tjänsten, se [konfigurera hello omvänd sökning för hello IP-adresser tilldelas tooyour Azure-tjänsten](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="118ad-106">tooconfigure reverse DNS for Azure-owned IP address assigned tooyour Azure service, see [configure hello reverse lookup for hello IP addresses allocated tooyour Azure service](dns-reverse-dns-for-azure-services.md).</span></span>

<span data-ttu-id="118ad-107">Innan du läser den här artikeln bör du vara bekant med den här [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="118ad-107">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="118ad-108">Den här artikeln vägleder dig genom hello steg toocreate din första DNS-zon för omvänd sökning och en post med hjälp av hello Azure-portalen, Azure PowerShell, Azure CLI 1.0 eller 2.0 för Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="118ad-108">This article walks you through hello steps toocreate your first reverse lookup DNS zone and record using hello Azure portal, Azure PowerShell, Azure CLI 1.0 or Azure CLI 2.0.</span></span>

## <a name="create-a-reverse-lookup-dns-zone"></a><span data-ttu-id="118ad-109">Skapa en DNS-zon för omvänd sökning</span><span class="sxs-lookup"><span data-stu-id="118ad-109">Create a reverse lookup DNS zone</span></span>

1. <span data-ttu-id="118ad-110">Logga in toohello [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="118ad-110">Sign in toohello [Azure portal](https://portal.azure.com)</span></span>
1. <span data-ttu-id="118ad-111">Hej hubbmenyn, klicka på och klicka på **ny** > **nätverk** > och klicka sedan på **DNS-zonen** tooopen hello **skapa DNS-zonen**bladet.</span><span class="sxs-lookup"><span data-stu-id="118ad-111">On hello Hub menu, click and click **New** > **Networking** > and then click **DNS zone** tooopen hello **Create DNS zone** blade.</span></span>

   ![DNS-zon](./media/dns-reverse-dns-hosting/figure1.png)

1. <span data-ttu-id="118ad-113">På hello **skapa DNS-zonen** bladet namnge din DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="118ad-113">On hello **Create DNS zone** blade, name your DNS zone.</span></span> <span data-ttu-id="118ad-114">hello namnet på hello zonen är utformade på olika sätt för IPv4 och IPv6-prefix.</span><span class="sxs-lookup"><span data-stu-id="118ad-114">hello name of hello zone is crafted differently for IPv4 and IPv6 prefixes.</span></span> <span data-ttu-id="118ad-115">Använd antingen hello instruktioner för [IPV4](#ipv4) eller [IPv6](#ipv6) tooname zonen.</span><span class="sxs-lookup"><span data-stu-id="118ad-115">Use either hello instructions for [IPV4](#ipv4) or [IPv6](#ipv6) tooname your zone.</span></span> <span data-ttu-id="118ad-116">När du är klar klickar du på **skapa** toocreate hello zonen.</span><span class="sxs-lookup"><span data-stu-id="118ad-116">When complete click **Create** toocreate hello zone.</span></span>

### <a name="ipv4"></a><span data-ttu-id="118ad-117">IPv4</span><span class="sxs-lookup"><span data-stu-id="118ad-117">IPv4</span></span>

<span data-ttu-id="118ad-118">hello namnet på en IPv4 zon för omvänd sökning baseras på hello IP-adressintervall som representerar.</span><span class="sxs-lookup"><span data-stu-id="118ad-118">hello name of an IPv4 reverse lookup zone is based on hello IP range it represents.</span></span> <span data-ttu-id="118ad-119">Det bör vara i hello följande format: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="118ad-119">It should be in hello following format: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span></span> <span data-ttu-id="118ad-120">Exempel finns i [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md#ipv4).</span><span class="sxs-lookup"><span data-stu-id="118ad-120">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv4).</span></span>

> [!NOTE]
> <span data-ttu-id="118ad-121">När du skapar classless omvänd DNS-zoner för sökning i Azure DNS, måste du använda ett bindestreck (`-`) i stället för ett snedstreck ('/ ') hello zonnamn.</span><span class="sxs-lookup"><span data-stu-id="118ad-121">When creating classless reverse DNS lookup zones in Azure DNS, you must use a hyphen (`-`) rather than a forward slash ('/') in hello zone name.</span></span>
>
> <span data-ttu-id="118ad-122">Till exempel för hello IP-intervallet 192.0.2.128/26 du måste använda `128-26.2.0.192.in-addr.arpa` som hello zonnamnet i stället för `128/26.2.0.192.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="118ad-122">For example, for hello IP range 192.0.2.128/26, you must use `128-26.2.0.192.in-addr.arpa` as hello zone name instead of `128/26.2.0.192.in-addr.arpa`.</span></span>
>
> <span data-ttu-id="118ad-123">Detta beror på att när båda stöds av hello DNS-standarden DNS zonen namn som innehåller hello snedstreck (`/`) tecken stöds inte i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="118ad-123">This is because, while both are supported by hello DNS standards, DNS zone names containing hello forward slash (`/`) character are not supported in Azure DNS.</span></span>

<span data-ttu-id="118ad-124">hello följande exempel visas hur toocreate en klass C omvänd DNS-zon som heter `2.0.192.in-addr.arpa` i Azure DNS via hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="118ad-124">hello following example shows how toocreate a 'Class C' reverse DNS zone named `2.0.192.in-addr.arpa` in Azure DNS via hello Azure portal:</span></span>

 ![Skapa DNS-zon](./media/dns-reverse-dns-hosting/figure2.png)

<span data-ttu-id="118ad-126">hello 'Resursgruppsplats' definierar hello plats för resursgruppen hello och har ingen inverkan på hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="118ad-126">hello 'Resource group location' defines hello location for hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="118ad-127">hello DNS-zonen plats är alltid ”globala' och visas inte.</span><span class="sxs-lookup"><span data-stu-id="118ad-127">hello DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="118ad-128">hello följande exempel visar hur toocomplete detta uppgift med Azure PowerShell och hello Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="118ad-128">hello following examples show how toocomplete this task with Azure PowerShell and hello Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="118ad-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="118ad-129">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="118ad-130">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="118ad-130">Azure CLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="118ad-131">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="118ad-131">Azure CLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="118ad-132">IPv6</span><span class="sxs-lookup"><span data-stu-id="118ad-132">IPv6</span></span>

<span data-ttu-id="118ad-133">hello namnet på en IPv6 zon för omvänd sökning måste vara i hello följande format: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span><span class="sxs-lookup"><span data-stu-id="118ad-133">hello name of an IPv6 reverse lookup zone should be in hello following form: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span></span>  <span data-ttu-id="118ad-134">Exempel finns i [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md#ipv6).</span><span class="sxs-lookup"><span data-stu-id="118ad-134">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv6).</span></span>


<span data-ttu-id="118ad-135">hello följande exempel visas hur toocreate en IPv6 omvänd DNS-sökningszon med namnet `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` i Azure DNS via hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="118ad-135">hello following example shows how toocreate an IPv6 reverse DNS lookup zone named `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS via hello Azure portal:</span></span>

 ![Skapa DNS-zon](./media/dns-reverse-dns-hosting/figure3.png)

<span data-ttu-id="118ad-137">hello 'Resursgruppsplats' definierar hello plats för resursgruppen hello och har ingen inverkan på hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="118ad-137">hello 'Resource group location' defines hello location for hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="118ad-138">hello DNS-zonen plats är alltid ”globala' och visas inte.</span><span class="sxs-lookup"><span data-stu-id="118ad-138">hello DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="118ad-139">hello följande exempel visar hur toocomplete detta uppgift med Azure PowerShell och hello Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="118ad-139">hello following examples show how toocomplete this task with Azure PowerShell and hello Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="118ad-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="118ad-140">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a><span data-ttu-id="118ad-141">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="118ad-141">AzureCLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a><span data-ttu-id="118ad-142">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="118ad-142">AzureCLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a><span data-ttu-id="118ad-143">Delegera en zon för omvänd DNS-sökning</span><span class="sxs-lookup"><span data-stu-id="118ad-143">Delegate a reverse DNS lookup zone</span></span>

<span data-ttu-id="118ad-144">Att ha skapat en zon för omvänd DNS-sökning, måste du kontrollera hello zonen delegerad från hello överordnade zonen.</span><span class="sxs-lookup"><span data-stu-id="118ad-144">Having created your reverse DNS lookup zone, you must ensure that hello zone is delegated from hello parent zone.</span></span> <span data-ttu-id="118ad-145">DNS-delegering kan hello DNS-processen toofind hello namn namnmatchningsservrar värd för en zon för omvänd DNS-sökning.</span><span class="sxs-lookup"><span data-stu-id="118ad-145">DNS delegation enables hello DNS name resolution process toofind hello name servers hosting your reverse DNS lookup zone.</span></span> <span data-ttu-id="118ad-146">Detta gör att dessa servrar tooanswer DNS omvänd namnfrågor för hello IP-adresser i ditt-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="118ad-146">This enables those name servers tooanswer DNS reverse queries for hello IP addresses in your address range.</span></span>

<span data-ttu-id="118ad-147">För zoner för vanlig sökning hello processen att delegera en DNS-zon beskrivs i [Delegera din domän tooAzure DNS](dns-delegate-domain-azure-dns.md).</span><span class="sxs-lookup"><span data-stu-id="118ad-147">For forward lookup zones, hello process of delegating a DNS zone is described in [Delegate your domain tooAzure DNS](dns-delegate-domain-azure-dns.md).</span></span> <span data-ttu-id="118ad-148">Zoner för omvänd sökning-delegering fungerar hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="118ad-148">Delegation for reverse lookup zones works hello same way.</span></span> <span data-ttu-id="118ad-149">hello enda skillnaden är att du behöver tooconfigure hello namnservrar med hello Internetleverantören som tillhandahåller IP-adressintervall i stället för domännamnsregistratorn.</span><span class="sxs-lookup"><span data-stu-id="118ad-149">hello only difference is that you need tooconfigure hello name servers with hello ISP who provided your IP range, rather than your domain name registrar.</span></span>

## <a name="create-a-dns-ptr-record"></a><span data-ttu-id="118ad-150">Skapa en DNS PTR-post</span><span class="sxs-lookup"><span data-stu-id="118ad-150">Create a DNS PTR record</span></span>

### <a name="ipv4"></a><span data-ttu-id="118ad-151">IPv4</span><span class="sxs-lookup"><span data-stu-id="118ad-151">IPv4</span></span>

<span data-ttu-id="118ad-152">hello vägleder följande exempel dig genom hello processen att skapa en PTR-post i en omvänd DNS-zon i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="118ad-152">hello following example walks you through hello process of creating a PTR record in a reverse DNS zone in Azure DNS.</span></span> <span data-ttu-id="118ad-153">Andra typer av poster och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hjälp av hello Azure-portalen](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="118ad-153">For other record types and toomodify existing records, see [Manage DNS records and record sets by using hello Azure portal](dns-operations-recordsets-portal.md).</span></span>

1.  <span data-ttu-id="118ad-154">Hello överst i hello **DNS-zonen** bladet väljer **+ postuppsättningen** tooopen hello **lägga till postuppsättning** bladet.</span><span class="sxs-lookup"><span data-stu-id="118ad-154">At hello top of hello **DNS zone** blade, select **+ Record set** tooopen hello **Add record set** blade.</span></span>

 ![DNS-zon](./media/dns-reverse-dns-hosting/figure4.png)

1. <span data-ttu-id="118ad-156">På hello **lägga till postuppsättning** bladet.</span><span class="sxs-lookup"><span data-stu-id="118ad-156">On hello **Add record set** blade.</span></span> 
1. <span data-ttu-id="118ad-157">Välj **PTR** från hello posten ”**typen**” menyn.</span><span class="sxs-lookup"><span data-stu-id="118ad-157">Select **PTR** from hello record "**Type**" menu.</span></span>  
1. <span data-ttu-id="118ad-158">hello måste namnet på postuppsättningen hello för en PTR-post toobe hello resten av hello IPv4-adress i omvänd ordning.</span><span class="sxs-lookup"><span data-stu-id="118ad-158">hello name of hello record set for a PTR record needs toobe hello rest of hello IPv4 address in reverse order.</span></span> <span data-ttu-id="118ad-159">I det här exemplet är hello tre första oktetterna redan ifyllda som en del av namn på zon för hello (.2.0.192).</span><span class="sxs-lookup"><span data-stu-id="118ad-159">In this example, hello first three octets are already populated as part of hello zone name (.2.0.192).</span></span> <span data-ttu-id="118ad-160">Därför tillhandahålls endast hello sista oktetten i hello fältnamn.</span><span class="sxs-lookup"><span data-stu-id="118ad-160">Therefore, only hello last octet is supplied in hello name field.</span></span> <span data-ttu-id="118ad-161">Till exempel namnger du din postuppsättning ”**15**” för en resurs vars IP-adressen är 192.0.2.15.</span><span class="sxs-lookup"><span data-stu-id="118ad-161">For example, you could name your record set "**15**" for a resource whose IP address is 192.0.2.15.</span></span>  
1. <span data-ttu-id="118ad-162">I hello ”**domännamn**”, ange hello fullständigt kvalificerade domännamnet (FQDN) för hello resursen med hjälp av hello IP.</span><span class="sxs-lookup"><span data-stu-id="118ad-162">In hello "**Domain Name**" field, enter hello fully qualified domain name (FQDN) of hello resource using hello IP.</span></span>
1. <span data-ttu-id="118ad-163">Välj **OK** längst hello hello bladet toocreate hello DNS-post.</span><span class="sxs-lookup"><span data-stu-id="118ad-163">Select **OK** at hello bottom of hello blade toocreate hello DNS record.</span></span>

 ![Lägg till en postuppsättning](./media/dns-reverse-dns-hosting/figure5.png)

<span data-ttu-id="118ad-165">hello följande är exempel på hur toocomplete uppgiften med PowerShell och hello AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="118ad-165">hello following are examples on how toocomplete this task with PowerShell and hello AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="118ad-166">PowerShell</span><span class="sxs-lookup"><span data-stu-id="118ad-166">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a><span data-ttu-id="118ad-167">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="118ad-167">AzureCLI 1.0</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a><span data-ttu-id="118ad-168">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="118ad-168">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a><span data-ttu-id="118ad-169">IPv6</span><span class="sxs-lookup"><span data-stu-id="118ad-169">IPv6</span></span>

<span data-ttu-id="118ad-170">följande exempel hello vägleder dig genom hello processen att skapa ny 'PTR-post.</span><span class="sxs-lookup"><span data-stu-id="118ad-170">hello following example walks you through hello process of creating new 'PTR' record.</span></span> <span data-ttu-id="118ad-171">Andra typer av poster och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hjälp av hello Azure-portalen](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="118ad-171">For other record types and toomodify existing records, see [Manage DNS records and record sets by using hello Azure portal](dns-operations-recordsets-portal.md).</span></span>

1. <span data-ttu-id="118ad-172">Hello överst i hello **DNS-zonen bladet**väljer **+ postuppsättningen** tooopen hello **lägga till postuppsättning** bladet.</span><span class="sxs-lookup"><span data-stu-id="118ad-172">At hello top of hello **DNS zone blade**, select **+ Record set** tooopen hello **Add record set** blade.</span></span>

  ![DNS-zonen bladet](./media/dns-reverse-dns-hosting/figure6.png)

2. <span data-ttu-id="118ad-174">På hello **lägga till postuppsättning** bladet.</span><span class="sxs-lookup"><span data-stu-id="118ad-174">On hello **Add record set** blade.</span></span> 
3. <span data-ttu-id="118ad-175">Välj **PTR** från hello posten ”**typen**” menyn.</span><span class="sxs-lookup"><span data-stu-id="118ad-175">Select **PTR** from hello record "**Type**" menu.</span></span>  
4. <span data-ttu-id="118ad-176">hello måste namnet på postuppsättningen hello för en PTR-post toobe hello resten av hello IPv6-adress i omvänd ordning.</span><span class="sxs-lookup"><span data-stu-id="118ad-176">hello name of hello record set for a PTR record needs toobe hello rest of hello IPv6 address in reverse order.</span></span> <span data-ttu-id="118ad-177">Det får inte innehålla noll komprimering.</span><span class="sxs-lookup"><span data-stu-id="118ad-177">It must not include any zero compression.</span></span> <span data-ttu-id="118ad-178">I det här exemplet hello första 64-bitars av hello IPv6 har redan angetts som en del av namn på zon för hello (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span><span class="sxs-lookup"><span data-stu-id="118ad-178">In this example, hello first 64 bits of hello IPv6 are already populated as part of hello zone name (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span></span> <span data-ttu-id="118ad-179">Därför tillhandahålls endast hello senaste 64-bitars i hello fältnamn.</span><span class="sxs-lookup"><span data-stu-id="118ad-179">Therefore, only hello last 64 bits are supplied in hello name field.</span></span> <span data-ttu-id="118ad-180">hello senaste 64-bitars hello IP-adress har angetts i omvänd ordning med en period som hello avgränsare mellan varje hexadecimalt tal.</span><span class="sxs-lookup"><span data-stu-id="118ad-180">hello last 64 bits of hello IP address are entered in reverse order, using a period as hello delimiter between each hexadecimal number.</span></span> <span data-ttu-id="118ad-181">Till exempel namnger du din postuppsättning ”**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**” för en resurs vars IP-adressen är 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span><span class="sxs-lookup"><span data-stu-id="118ad-181">For example, you could name your record set "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" for a resource whose IP address is 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span></span>  
5. <span data-ttu-id="118ad-182">I hello ”**domännamn**”, ange hello fullständigt kvalificerade domännamnet (FQDN) för hello resursen med hjälp av hello IP.</span><span class="sxs-lookup"><span data-stu-id="118ad-182">In hello "**Domain Name**" field, enter hello fully qualified domain name (FQDN) of hello resource using hello IP.</span></span>
6. <span data-ttu-id="118ad-183">Välj **OK** längst hello hello bladet toocreate hello DNS-post.</span><span class="sxs-lookup"><span data-stu-id="118ad-183">Select **OK** at hello bottom of hello blade toocreate hello DNS record.</span></span>

![postuppsättningen bladet Lägg till](./media/dns-reverse-dns-hosting/figure7.png)

<span data-ttu-id="118ad-185">hello följande är exempel på hur toocomplete uppgiften med PowerShell och hello AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="118ad-185">hello following are examples on how toocomplete this task with PowerShell and hello AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="118ad-186">PowerShell</span><span class="sxs-lookup"><span data-stu-id="118ad-186">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a><span data-ttu-id="118ad-187">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="118ad-187">AzureCLI 1.0</span></span>

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a><span data-ttu-id="118ad-188">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="118ad-188">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a><span data-ttu-id="118ad-189">Visa poster</span><span class="sxs-lookup"><span data-stu-id="118ad-189">View Records</span></span>

<span data-ttu-id="118ad-190">tooview hello poster som du skapade navigera tooyour DNS-zonen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="118ad-190">tooview hello records you created, navigate tooyour DNS zone in hello Azure portal.</span></span> <span data-ttu-id="118ad-191">I hello lägre tillhör hello **DNS-zonen** bladet hittar du hello poster för hello DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="118ad-191">In hello lower part of hello **DNS zone** blade, you can see hello records for hello DNS zone.</span></span> <span data-ttu-id="118ad-192">Du bör se hello standard NS och SOA-poster, som skapas i varje zon, plus eventuella nya poster som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="118ad-192">You should see hello default NS and SOA records, which are created in every zone, plus any new records you have created.</span></span>

### <a name="ipv4"></a><span data-ttu-id="118ad-193">IPv4</span><span class="sxs-lookup"><span data-stu-id="118ad-193">IPv4</span></span>

<span data-ttu-id="118ad-194">DNS-zonen bladet visar IPv4 PTR-poster:</span><span class="sxs-lookup"><span data-stu-id="118ad-194">DNS zone blade, showing IPv4 PTR records:</span></span>

![DNS-zonen bladet](./media/dns-reverse-dns-hosting/figure8.png)

<span data-ttu-id="118ad-196">hello följande exempel visar hur tooview hello PTR-poster med PowerShell eller hello Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="118ad-196">hello following examples show how tooview hello PTR records using PowerShell or hello Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="118ad-197">PowerShell</span><span class="sxs-lookup"><span data-stu-id="118ad-197">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="118ad-198">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="118ad-198">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="118ad-199">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="118ad-199">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="118ad-200">IPv6</span><span class="sxs-lookup"><span data-stu-id="118ad-200">IPv6</span></span>

<span data-ttu-id="118ad-201">DNS-zonen bladet visar IPv6 PTR-poster:</span><span class="sxs-lookup"><span data-stu-id="118ad-201">DNS zone blade, showing IPv6 PTR records:</span></span>

![DNS-zonen bladet](./media/dns-reverse-dns-hosting/figure9.png)

<span data-ttu-id="118ad-203">hello följande är exempel på hur tooview hello poster med PowerShell och hello AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="118ad-203">hello following are examples on how tooview hello records with PowerShell and hello AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="118ad-204">PowerShell</span><span class="sxs-lookup"><span data-stu-id="118ad-204">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="118ad-205">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="118ad-205">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="118ad-206">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="118ad-206">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a><span data-ttu-id="118ad-207">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="118ad-207">FAQ</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a><span data-ttu-id="118ad-208">Kan jag vara värd zoner för omvänd DNS-sökning för min ISP-tilldelad IP-Adressblock på Azure DNS?</span><span class="sxs-lookup"><span data-stu-id="118ad-208">Can I host reverse DNS lookup zones for my ISP-assigned IP blocks on Azure DNS?</span></span>

<span data-ttu-id="118ad-209">Ja.</span><span class="sxs-lookup"><span data-stu-id="118ad-209">Yes.</span></span> <span data-ttu-id="118ad-210">Värd för hello zoner för omvänd sökning (ARPA) för din egen IP-adressintervall i Azure DNS stöds fullt ut.</span><span class="sxs-lookup"><span data-stu-id="118ad-210">Hosting hello reverse lookup (ARPA) zones for your own IP ranges in Azure DNS is fully supported.</span></span>

<span data-ttu-id="118ad-211">Skapa hello zon för omvänd sökning i Azure DNS som beskrivs i den här artikeln och sedan arbeta med Leverantören för[ombud hello zonen](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="118ad-211">Create hello reverse lookup zone in Azure DNS as explained in this article, then work with your ISP too[delegate hello zone](dns-domain-delegation.md).</span></span>  <span data-ttu-id="118ad-212">Du kan sedan hantera hello PTR-poster för varje omvänd sökning i hello samma sätt som andra posttyper.</span><span class="sxs-lookup"><span data-stu-id="118ad-212">You can then manage hello PTR records for each reverse lookup in hello same way as other record types.</span></span>

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a><span data-ttu-id="118ad-213">Hur mycket har värd min omvänd DNS-sökning zonen kostnaden?</span><span class="sxs-lookup"><span data-stu-id="118ad-213">How much does hosting my reverse DNS lookup zone cost?</span></span>

<span data-ttu-id="118ad-214">Värd för zon för hello omvänd DNS-sökning för din ISP-tilldelad IP-Adressblock i Azure DNS debiteras med [Azure DNS standardpriser](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="118ad-214">Hosting hello reverse DNS lookup zone for your ISP-assigned IP block in Azure DNS is charged at [standard Azure DNS rates](https://azure.microsoft.com/pricing/details/dns/).</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a><span data-ttu-id="118ad-215">Kan jag vara värd för omvänd DNS-sökningszoner för både IPv4 och IPv6-adresser i Azure DNS?</span><span class="sxs-lookup"><span data-stu-id="118ad-215">Can I host reverse DNS lookup zones for both IPv4 and IPv6 addresses in Azure DNS?</span></span>

<span data-ttu-id="118ad-216">Ja.</span><span class="sxs-lookup"><span data-stu-id="118ad-216">Yes.</span></span> <span data-ttu-id="118ad-217">Den här artikeln förklarar hur toocreate både IPv4 och IPv6-zoner för omvänd DNS-sökning i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="118ad-217">This article explains how toocreate both IPv4 and IPv6 reverse DNS lookup zones in Azure DNS.</span></span>

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a><span data-ttu-id="118ad-218">Kan jag importera en befintlig omvänd DNS-sökningszon?</span><span class="sxs-lookup"><span data-stu-id="118ad-218">Can I import an existing reverse DNS lookup zone?</span></span>

<span data-ttu-id="118ad-219">Ja.</span><span class="sxs-lookup"><span data-stu-id="118ad-219">Yes.</span></span> <span data-ttu-id="118ad-220">Du kan använda hello Azure CLI tooimport befintlig DNS-zoner i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="118ad-220">You can use hello Azure CLI tooimport existing DNS zones into Azure DNS.</span></span> <span data-ttu-id="118ad-221">Detta fungerar för både zoner för vanlig sökning och zoner för omvänd sökning.</span><span class="sxs-lookup"><span data-stu-id="118ad-221">This works for both forward lookup zones and reverse lookup zones.</span></span>

<span data-ttu-id="118ad-222">Mer information finns i [Import- och DNS-zon filen med hello Azure CLI](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="118ad-222">For more information, see [Import and export a DNS zone file using hello Azure CLI](dns-import-export.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="118ad-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="118ad-223">Next steps</span></span>

<span data-ttu-id="118ad-224">Läs mer om omvänd DNS [omvänd DNS-sökning på Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="118ad-224">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="118ad-225">Lär dig hur för[hantera omvänd DNS-posterna för din Azure-tjänster](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="118ad-225">Learn how too[manage reverse DNS records for your Azure services](dns-reverse-dns-for-azure-services.md).</span></span>
