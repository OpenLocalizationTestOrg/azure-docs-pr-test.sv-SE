---
title: "aaaManage DNS-poster i Azure DNS med hjälp av Azure PowerShell | Microsoft Docs"
description: "Hantera DNS-postuppsättningar och på Azure DNS-poster när värd för din domän på Azure DNS. Alla PowerShell-kommandon för åtgärder på uppsättningar av poster och poster."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: bfdf116e174d06db0514abdc0ec3f4fc4ee0a079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a><span data-ttu-id="7e226-104">Hantera DNS-poster och postuppsättningar i Azure DNS med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e226-104">Manage DNS records and recordsets in Azure DNS using Azure PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7e226-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7e226-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="7e226-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7e226-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="7e226-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7e226-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="7e226-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e226-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="7e226-109">Den här artikeln visar hur toomanage DNS poster för DNS-zonen med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7e226-109">This article shows you how toomanage DNS records for your DNS zone by using Azure PowerShell.</span></span> <span data-ttu-id="7e226-110">DNS-poster kan också hanteras med hjälp av hello plattformsoberoende [Azure CLI](dns-operations-recordsets-cli.md) eller hello [Azure-portalen](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7e226-110">DNS records can also be managed by using hello cross-platform [Azure CLI](dns-operations-recordsets-cli.md) or hello [Azure portal](dns-operations-recordsets-portal.md).</span></span>

<span data-ttu-id="7e226-111">hello exemplen i den här artikeln förutsätter att du redan har [installerat Azure PowerShell loggat in, och skapa en DNS-zon](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="7e226-111">hello examples in this article assume you have already [installed Azure PowerShell, signed in, and created a DNS zone](dns-operations-dnszones.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="7e226-112">Introduktion</span><span class="sxs-lookup"><span data-stu-id="7e226-112">Introduction</span></span>

<span data-ttu-id="7e226-113">Innan du skapar DNS-poster i Azure DNS, måste du först toounderstand hur Azure DNS organiserar DNS-poster i DNS-postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="7e226-113">Before creating DNS records in Azure DNS, you first need toounderstand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="7e226-114">Mer information om DNS-poster i Azure DNS finns i [DNS-zoner och poster](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="7e226-114">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>


## <a name="create-a-new-dns-record"></a><span data-ttu-id="7e226-115">Skapa en ny DNS-post</span><span class="sxs-lookup"><span data-stu-id="7e226-115">Create a new DNS record</span></span>

<span data-ttu-id="7e226-116">Om din nya posten har hello samma namn och som en befintlig post, du behöver för[lägga till den befintliga postuppsättning för toohello](#add-a-record-to-an-existing-record-set).</span><span class="sxs-lookup"><span data-stu-id="7e226-116">If your new record has hello same name and type as an existing record, you need too[add it toohello existing record set](#add-a-record-to-an-existing-record-set).</span></span> <span data-ttu-id="7e226-117">Om din nya posten har ett annat namn och typ tooall befintliga poster, måste toocreate en ny uppsättning av poster.</span><span class="sxs-lookup"><span data-stu-id="7e226-117">If your new record has a different name and type tooall existing records, you need toocreate a new record set.</span></span> 

### <a name="create-a-records-in-a-new-record-set"></a><span data-ttu-id="7e226-118">Skapa ”A” poster i en ny uppsättning av poster</span><span class="sxs-lookup"><span data-stu-id="7e226-118">Create 'A' records in a new record set</span></span>

<span data-ttu-id="7e226-119">Du skapar postuppsättningar med hello `New-AzureRmDnsRecordSet` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7e226-119">You create record sets by using hello `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="7e226-120">När du skapar en postuppsättning måste toospecify hello postuppsättningens namn, hello zonen, hello tid toolive (TTL), hello posttyp och hello poster toobe skapades.</span><span class="sxs-lookup"><span data-stu-id="7e226-120">When creating a record set, you need toospecify hello record set name, hello zone, hello time toolive (TTL), hello record type, and hello records toobe created.</span></span>

<span data-ttu-id="7e226-121">hello parametrar för att lägga till poster tooa postuppsättning varierar beroende på hello typ av hello uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="7e226-121">hello parameters for adding records tooa record set vary depending on hello type of hello record set.</span></span> <span data-ttu-id="7e226-122">Till exempel när du använder en postuppsättning av typen ”A”, måste toospecify hello IP-adressen med hello parametern `-IPv4Address`.</span><span class="sxs-lookup"><span data-stu-id="7e226-122">For example, when using a record set of type 'A', you need toospecify hello IP address using hello parameter `-IPv4Address`.</span></span> <span data-ttu-id="7e226-123">Andra parametrar används för andra typer av poster.</span><span class="sxs-lookup"><span data-stu-id="7e226-123">Other parameters are used for other record types.</span></span> <span data-ttu-id="7e226-124">Se [ytterligare posttyp exempel](#additional-record-type-examples) mer information.</span><span class="sxs-lookup"><span data-stu-id="7e226-124">See [Additional record type examples](#additional-record-type-examples) for details.</span></span>

<span data-ttu-id="7e226-125">hello skapas följande exempel en postuppsättning med hello relativa namnet www om du i hello DNS-zonen ”contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="7e226-125">hello following example creates a record set with hello relative name 'www' in hello DNS Zone 'contoso.com'.</span></span> <span data-ttu-id="7e226-126">hello fullständigt kvalificerade namnet på postuppsättningen hello är ”www.contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="7e226-126">hello fully-qualified name of hello record set is 'www.contoso.com'.</span></span> <span data-ttu-id="7e226-127">hello-posttypen är ”A” och hello TTL-värde är 3 600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="7e226-127">hello record type is 'A', and hello TTL is 3600 seconds.</span></span> <span data-ttu-id="7e226-128">hello postuppsättning innehåller en post med IP-adressen '1.2.3.4'.</span><span class="sxs-lookup"><span data-stu-id="7e226-128">hello record set contains a single record, with IP address '1.2.3.4'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="7e226-129">toocreate en postuppsättning på hello apex om du för en zon (i det här fallet ”contoso.com”), hello används postuppsättningsnamnet ”@” (utan citattecken):</span><span class="sxs-lookup"><span data-stu-id="7e226-129">toocreate a record set at hello 'apex' of a zone (in this case, 'contoso.com'), use hello record set name '@' (excluding quotation marks):</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="7e226-130">Om du behöver en post som innehåller fler än en post toocreate först skapa en lokal matris och lägga till hello poster och sedan skicka hello matris för`New-AzureRmDnsRecordSet` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7e226-130">If you need toocreate a record set containing more than one record, first create a local array and add hello records, then pass hello array too`New-AzureRmDnsRecordSet` as follows:</span></span>

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

<span data-ttu-id="7e226-131">[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan vara används tooassociate programspecifika data med varje postuppsättningen, som nyckel / värde-par.</span><span class="sxs-lookup"><span data-stu-id="7e226-131">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="7e226-132">hello följande exempel visas hur toocreate en postuppsättning med två metadataposter ”Avd = ekonomi ' och ' miljö = produktion”.</span><span class="sxs-lookup"><span data-stu-id="7e226-132">hello following example shows how toocreate a record set with two metadata entries, 'dept=finance' and 'environment=production'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

<span data-ttu-id="7e226-133">Azure DNS stöder också 'empty-postuppsättningar som kan fungera som en platshållare tooreserve ett DNS-namn innan du skapar DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="7e226-133">Azure DNS also supports 'empty' record sets, which can act as a placeholder tooreserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="7e226-134">Tom postuppsättningar är synliga i hello Azure DNS-kontrollplan, men visas på hello Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="7e226-134">Empty record sets are visible in hello Azure DNS control plane, but do appear on hello Azure DNS name servers.</span></span> <span data-ttu-id="7e226-135">hello följande exempel skapar en tom postuppsättning:</span><span class="sxs-lookup"><span data-stu-id="7e226-135">hello following example creates an empty record set:</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a><span data-ttu-id="7e226-136">Skapa poster för andra typer</span><span class="sxs-lookup"><span data-stu-id="7e226-136">Create records of other types</span></span>

<span data-ttu-id="7e226-137">Att ha sett i detalj hur toocreate ”A” poster, hello följande exempel visas hur toocreate poster av andra post typer som stöds av Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="7e226-137">Having seen in detail how toocreate 'A' records, hello following examples show how toocreate records of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="7e226-138">I varje fall visar vi hur toocreate en post som innehåller en post.</span><span class="sxs-lookup"><span data-stu-id="7e226-138">In each case, we show how toocreate a record set containing a single record.</span></span> <span data-ttu-id="7e226-139">hello tidigare exempel ”A” poster kan vara anpassats toocreate postuppsättningar av andra typer som innehåller flera poster med metadata, eller toocreate tom postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="7e226-139">hello earlier examples for 'A' records can be adapted toocreate record sets of other types containing multiple records, with metadata, or toocreate empty record sets.</span></span>

<span data-ttu-id="7e226-140">Vi inte ger ett exempel toocreate ett SOA-postuppsättning eftersom SOAs skapas och tas bort tillsammans med varje DNS-zon och kan inte skapas eller tas bort separat.</span><span class="sxs-lookup"><span data-stu-id="7e226-140">We do not give an example toocreate an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="7e226-141">Dock [hello SOA kan ändras, som visas i en senare exempel](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="7e226-141">However, [hello SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record-set-with-a-single-record"></a><span data-ttu-id="7e226-142">Skapa en AAAA-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="7e226-142">Create an AAAA record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a><span data-ttu-id="7e226-143">Skapa en CNAME-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="7e226-143">Create a CNAME record set with a single record</span></span>

> [!NOTE]
> <span data-ttu-id="7e226-144">hello DNS-standarden inte tillåter CNAME-poster på hello apex för en zon (`-Name '@'`), eller tillåter att postuppsättningar som innehåller fler än en post.</span><span class="sxs-lookup"><span data-stu-id="7e226-144">hello DNS standards do not permit CNAME records at hello apex of a zone (`-Name '@'`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="7e226-145">Mer information finns i [CNAME-poster](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="7e226-145">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a><span data-ttu-id="7e226-146">Skapa en MX-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="7e226-146">Create an MX record set with a single record</span></span>

<span data-ttu-id="7e226-147">I det här exemplet använder vi hello postuppsättningsnamnet ”@” toocreate en MX-post vid hello zonens apex (i det här fallet ”contoso.com”).</span><span class="sxs-lookup"><span data-stu-id="7e226-147">In this example, we use hello record set name '@' toocreate an MX record at hello zone apex (in this case, 'contoso.com').</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a><span data-ttu-id="7e226-148">Skapa en NS-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="7e226-148">Create an NS record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a><span data-ttu-id="7e226-149">Skapa en PTR-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="7e226-149">Create a PTR record set with a single record</span></span>

<span data-ttu-id="7e226-150">I det här fallet ”min-arpa-zone.com' representerar hello ARPA zon för omvänd sökning som motsvarar IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="7e226-150">In this case, 'my-arpa-zone.com' represents hello ARPA reverse lookup zone representing your IP range.</span></span> <span data-ttu-id="7e226-151">Varje PTR-post i den här zonen motsvarar tooan IP-adress inom IP-intervallet.</span><span class="sxs-lookup"><span data-stu-id="7e226-151">Each PTR record set in this zone corresponds tooan IP address within this IP range.</span></span> <span data-ttu-id="7e226-152">hello-postnamnet ”10” är hello sista oktetten hello IP-adress inom den här IP-adressintervall som representeras av den här posten.</span><span class="sxs-lookup"><span data-stu-id="7e226-152">hello record name '10' is hello last octet of hello IP address within this IP range represented by this record.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a><span data-ttu-id="7e226-153">Skapa en SRV-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="7e226-153">Create an SRV record set with a single record</span></span>

<span data-ttu-id="7e226-154">När du skapar en [SRV-postuppsättning](dns-zones-records.md#srv-records), ange hello  *\_service* och  *\_protokollet* i hello postuppsättning namn.</span><span class="sxs-lookup"><span data-stu-id="7e226-154">When creating an [SRV record set](dns-zones-records.md#srv-records), specify hello *\_service* and *\_protocol* in hello record set name.</span></span> <span data-ttu-id="7e226-155">Det finns inget behov av tooinclude ”@” i hello postuppsättning namn när du skapar en SRV-post på hello zonens apex.</span><span class="sxs-lookup"><span data-stu-id="7e226-155">There is no need tooinclude '@' in hello record set name when creating an SRV record set at hello zone apex.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a><span data-ttu-id="7e226-156">Skapa en TXT-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="7e226-156">Create a TXT record set with a single record</span></span>

<span data-ttu-id="7e226-157">hello följande exempel visas hur toocreate en TXT-posten.</span><span class="sxs-lookup"><span data-stu-id="7e226-157">hello following example shows how toocreate a TXT record.</span></span> <span data-ttu-id="7e226-158">Mer information om hello stränglängden som stöds i TXT-poster finns [TXT-poster](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="7e226-158">For more information about hello maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a><span data-ttu-id="7e226-159">Hämta en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="7e226-159">Get a record set</span></span>

<span data-ttu-id="7e226-160">tooretrieve befintliga postuppsättningen, Använd `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="7e226-160">tooretrieve an existing record set, use `Get-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="7e226-161">Denna cmdlet returnerar ett lokalt objekt som representerar hello postuppsättningen i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="7e226-161">This cmdlet returns a local object that represents hello record set in Azure DNS.</span></span>

<span data-ttu-id="7e226-162">Precis som med `New-AzureRmDnsRecordSet`, hello postuppsättning namn måste vara en *relativa* namn, vilket innebär att den inte får innehålla hello zonnamnet.</span><span class="sxs-lookup"><span data-stu-id="7e226-162">As with `New-AzureRmDnsRecordSet`, hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span> <span data-ttu-id="7e226-163">Du måste också toospecify hello posttyp och hello zon som innehåller hello uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="7e226-163">You also need toospecify hello record type, and hello zone containing hello record set.</span></span>

<span data-ttu-id="7e226-164">hello följande exempel visas hur tooretrieve en postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="7e226-164">hello following example shows how tooretrieve a record set.</span></span> <span data-ttu-id="7e226-165">I det här exemplet hello zon anges med hjälp av hello `-ZoneName` och `-ResourceGroupName` parametrar.</span><span class="sxs-lookup"><span data-stu-id="7e226-165">In this example, hello zone is specified using hello `-ZoneName` and `-ResourceGroupName` parameters.</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="7e226-166">Du kan också också ange hello zon med hjälp av en zon-objekt som överförts med hello `-Zone` parameter.</span><span class="sxs-lookup"><span data-stu-id="7e226-166">Alternatively, you can also specify hello zone using a zone object, passed using hello `-Zone` parameter.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a><span data-ttu-id="7e226-167">Lista över postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="7e226-167">List record sets</span></span>

<span data-ttu-id="7e226-168">Du kan också använda `Get-AzureRmDnsZone` toolist postuppsättningar i en zon för genom att utelämna hello `-Name` och/eller `-RecordType` parametrar.</span><span class="sxs-lookup"><span data-stu-id="7e226-168">You can also use `Get-AzureRmDnsZone` toolist record sets in a zone, by omitting hello `-Name` and/or `-RecordType` parameters.</span></span>

<span data-ttu-id="7e226-169">hello följande exempel returneras alla postuppsättningar i zonen hello:</span><span class="sxs-lookup"><span data-stu-id="7e226-169">hello following example returns all record sets in hello zone:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="7e226-170">hello följande exempel visar hur alla uppsättningar av en viss typ kan hämtas genom att ange hello posttyp när utelämna hello post anges namn:</span><span class="sxs-lookup"><span data-stu-id="7e226-170">hello following example shows how all record sets of a given type can be retrieved by specifying hello record type while omitting hello record set name:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="7e226-171">tooretrieve alla postuppsättningar med ett angivet namn över posttyper, behöver du tooretrieve alla postuppsättningar och sedan hello filterresultaten:</span><span class="sxs-lookup"><span data-stu-id="7e226-171">tooretrieve all record sets with a given name, across record types, you need tooretrieve all record sets and then filter hello results:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

<span data-ttu-id="7e226-172">I alla hello exemplen ovan hello zon kan vara anges antingen med hjälp av hello `-ZoneName` och `-ResourceGroupName`parametrar (som visas), eller genom att ange ett objekt i zonen:</span><span class="sxs-lookup"><span data-stu-id="7e226-172">In all hello above examples, hello zone can be specified either by using hello `-ZoneName` and `-ResourceGroupName`parameters (as shown), or by specifying a zone object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-tooan-existing-record-set"></a><span data-ttu-id="7e226-173">Lägg till en post tooan befintliga uppsättningen av poster</span><span class="sxs-lookup"><span data-stu-id="7e226-173">Add a record tooan existing record set</span></span>

<span data-ttu-id="7e226-174">tooadd en befintlig post poster tooan ange, följ hello följande tre steg:</span><span class="sxs-lookup"><span data-stu-id="7e226-174">tooadd a record tooan existing record set, follow hello following three steps:</span></span>

1. <span data-ttu-id="7e226-175">Hämta hello befintliga postuppsättning</span><span class="sxs-lookup"><span data-stu-id="7e226-175">Get hello existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="7e226-176">Lägg till hello nya poster toohello lokala postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="7e226-176">Add hello new record toohello local record set.</span></span> <span data-ttu-id="7e226-177">Detta är en åtgärd som är offline.</span><span class="sxs-lookup"><span data-stu-id="7e226-177">This is an off-line operation.</span></span>

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="7e226-178">Bekräfta hello ändra tillbaka toohello Azure DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7e226-178">Commit hello change back toohello Azure DNS service.</span></span> 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

<span data-ttu-id="7e226-179">Med hjälp av `Set-AzureRmDnsRecordSet` *ersätter* hello befintliga posten i Azure DNS (och alla poster som den innehåller) med hello postuppsättning anges.</span><span class="sxs-lookup"><span data-stu-id="7e226-179">Using `Set-AzureRmDnsRecordSet` *replaces* hello existing record set in Azure DNS (and all records it contains) with hello record set specified.</span></span> <span data-ttu-id="7e226-180">[ETag kontrollerar](dns-zones-records.md#etags) används tooensure samtidiga ändringar inte att skrivas över.</span><span class="sxs-lookup"><span data-stu-id="7e226-180">[Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="7e226-181">Du kan använda valfri hello `-Overwrite` växla toosuppress kontrollerna.</span><span class="sxs-lookup"><span data-stu-id="7e226-181">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

<span data-ttu-id="7e226-182">Den här sekvensen av åtgärder kan också vara *skickas*, vilket innebär att du skickar hello postuppsättning objekt genom att använda hello pipe i stället för att skicka som en parameter:</span><span class="sxs-lookup"><span data-stu-id="7e226-182">This sequence of operations can also be *piped*, meaning you pass hello record set object by using hello pipe rather than passing it as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="7e226-183">hello exemplen ovan visar hur tooadd en ”A” poster tooan befintlig post inställd av typen ”A”.</span><span class="sxs-lookup"><span data-stu-id="7e226-183">hello examples above show how tooadd an 'A' record tooan existing record set of type 'A'.</span></span> <span data-ttu-id="7e226-184">En liknande sekvens av åtgärder är används tooadd poster toorecord uppsättningar av andra typer som ersätter hello `-Ipv4Address` -parametern för `Add-AzureRmDnsRecordConfig` med andra parametrar specifika tooeach posttyp.</span><span class="sxs-lookup"><span data-stu-id="7e226-184">A similar sequence of operations is used tooadd records toorecord sets of other types, substituting hello `-Ipv4Address` parameter of `Add-AzureRmDnsRecordConfig` with other parameters specific tooeach record type.</span></span> <span data-ttu-id="7e226-185">hello parametrar för varje posttyp är hello samma som för hello `New-AzureRmDnsRecordConfig` cmdlet, som visas i [ytterligare posttyp exempel](#additional-record-type-examples) ovan.</span><span class="sxs-lookup"><span data-stu-id="7e226-185">hello parameters for each record type are hello same as for hello `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>

<span data-ttu-id="7e226-186">Postuppsättningar av typen 'CNAME' eller 'SOA-' får inte innehålla fler än en post.</span><span class="sxs-lookup"><span data-stu-id="7e226-186">Record sets of type 'CNAME' or 'SOA' cannot contain more than one record.</span></span> <span data-ttu-id="7e226-187">Den här begränsningen uppstår från hello DNS-standarden.</span><span class="sxs-lookup"><span data-stu-id="7e226-187">This constraint arises from hello DNS standards.</span></span> <span data-ttu-id="7e226-188">Det är inte en begränsning i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="7e226-188">It is not a limitation of Azure DNS.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="7e226-189">Ta bort en post från en befintlig post</span><span class="sxs-lookup"><span data-stu-id="7e226-189">Remove a record from an existing record set</span></span>

<span data-ttu-id="7e226-190">hello processen tooremove en post från en postuppsättning är liknande toohello processen tooadd en post tooan befintliga postuppsättningen:</span><span class="sxs-lookup"><span data-stu-id="7e226-190">hello process tooremove a record from a record set is similar toohello process tooadd a record tooan existing record set:</span></span>

1. <span data-ttu-id="7e226-191">Hämta hello befintliga postuppsättning</span><span class="sxs-lookup"><span data-stu-id="7e226-191">Get hello existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="7e226-192">Ta bort hello-posten från hello lokala postuppsättning-objekt.</span><span class="sxs-lookup"><span data-stu-id="7e226-192">Remove hello record from hello local record set object.</span></span> <span data-ttu-id="7e226-193">Detta är en åtgärd som är offline.</span><span class="sxs-lookup"><span data-stu-id="7e226-193">This is an off-line operation.</span></span> <span data-ttu-id="7e226-194">hello-post som tas bort måste vara en exakt matchning med en befintlig post över alla parametrar.</span><span class="sxs-lookup"><span data-stu-id="7e226-194">hello record that's being removed must be an exact match with an existing record across all parameters.</span></span>

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="7e226-195">Bekräfta hello ändra tillbaka toohello Azure DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7e226-195">Commit hello change back toohello Azure DNS service.</span></span> <span data-ttu-id="7e226-196">Använd hello valfria `-Overwrite` växla toosuppress [Etag kontrollerar](dns-zones-records.md#etags) för samtidiga ändringar.</span><span class="sxs-lookup"><span data-stu-id="7e226-196">Use hello optional `-Overwrite` switch toosuppress [Etag checks](dns-zones-records.md#etags) for concurrent changes.</span></span>

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

<span data-ttu-id="7e226-197">Med hello ovan sekvens tooremove hello sista posten från en uppsättning poster tas inte bort hello postuppsättningen, i stället de lämnar en tom postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="7e226-197">Using hello above sequence tooremove hello last record from a record set does not delete hello record set, rather it leaves an empty record set.</span></span> <span data-ttu-id="7e226-198">tooremove en postuppsättning helt, se [ta bort en postuppsättning](#delete-a-record-set).</span><span class="sxs-lookup"><span data-stu-id="7e226-198">tooremove a record set entirely, see [Delete a record set](#delete-a-record-set).</span></span>

<span data-ttu-id="7e226-199">På liknande sätt postuppsättning tooadding poster tooa, hello sekvens med åtgärder tooremove en postuppsättning kan också skickas:</span><span class="sxs-lookup"><span data-stu-id="7e226-199">Similarly tooadding records tooa record set, hello sequence of operations tooremove a record set can also be piped:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="7e226-200">Olika posttyper stöds genom att ange hello lämpliga typspecifika parametrar för`Remove-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="7e226-200">Different record types are supported by passing hello appropriate type-specific parameters too`Remove-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="7e226-201">hello parametrar för varje posttyp är hello samma som för hello `New-AzureRmDnsRecordConfig` cmdlet, som visas i [ytterligare posttyp exempel](#additional-record-type-examples) ovan.</span><span class="sxs-lookup"><span data-stu-id="7e226-201">hello parameters for each record type are hello same as for hello `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>


## <a name="modify-an-existing-record-set"></a><span data-ttu-id="7e226-202">Ändra en befintlig postuppsättning</span><span class="sxs-lookup"><span data-stu-id="7e226-202">Modify an existing record set</span></span>

<span data-ttu-id="7e226-203">hello stegen för att ändra en befintlig postuppsättning är liknande toohello steg du ta när du lägger till eller ta bort poster från en postuppsättning:</span><span class="sxs-lookup"><span data-stu-id="7e226-203">hello steps for modifying an existing record set are similar toohello steps you take when adding or removing records from a record set:</span></span>

1. <span data-ttu-id="7e226-204">Hämta hello befintliga-postuppsättning med hjälp av `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="7e226-204">Retrieve hello existing record set by using `Get-AzureRmDnsRecordSet`.</span></span>
2. <span data-ttu-id="7e226-205">Ändra hello lokala postuppsättning objekt genom att:</span><span class="sxs-lookup"><span data-stu-id="7e226-205">Modify hello local record set object by:</span></span>
    * <span data-ttu-id="7e226-206">Tillägg eller borttagning av poster</span><span class="sxs-lookup"><span data-stu-id="7e226-206">Adding or removing records</span></span>
    * <span data-ttu-id="7e226-207">Ändra hello parametrar av befintliga poster</span><span class="sxs-lookup"><span data-stu-id="7e226-207">Changing hello parameters of existing records</span></span>
    * <span data-ttu-id="7e226-208">Ändra hello post ange metadata och toolive TTL (time)</span><span class="sxs-lookup"><span data-stu-id="7e226-208">Changing hello record set metadata and time toolive (TTL)</span></span>
3. <span data-ttu-id="7e226-209">Genomför ändringarna med hjälp av hello `Set-AzureRmDnsRecordSet` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7e226-209">Commit your changes by using hello `Set-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="7e226-210">Detta *ersätter* hello befintliga posten i Azure DNS med hello postuppsättning anges.</span><span class="sxs-lookup"><span data-stu-id="7e226-210">This *replaces* hello existing record set in Azure DNS with hello record set specified.</span></span>

<span data-ttu-id="7e226-211">När du använder `Set-AzureRmDnsRecordSet`, [Etag kontrollerar](dns-zones-records.md#etags) används tooensure samtidiga ändringar inte att skrivas över.</span><span class="sxs-lookup"><span data-stu-id="7e226-211">When using `Set-AzureRmDnsRecordSet`, [Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="7e226-212">Du kan använda valfri hello `-Overwrite` växla toosuppress kontrollerna.</span><span class="sxs-lookup"><span data-stu-id="7e226-212">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

### <a name="tooupdate-a-record-in-an-existing-record-set"></a><span data-ttu-id="7e226-213">Ange tooupdate en post i en befintlig post</span><span class="sxs-lookup"><span data-stu-id="7e226-213">tooupdate a record in an existing record set</span></span>

<span data-ttu-id="7e226-214">I det här exemplet ändrar vi hello IP-adressen för en befintlig ”A” post:</span><span class="sxs-lookup"><span data-stu-id="7e226-214">In this example, we change hello IP address of an existing 'A' record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-an-soa-record"></a><span data-ttu-id="7e226-215">toomodify en SOA-post</span><span class="sxs-lookup"><span data-stu-id="7e226-215">toomodify an SOA record</span></span>

<span data-ttu-id="7e226-216">Du kan inte lägga till eller ta bort poster från hello skapas automatiskt SOA-postuppsättning på zonens apex hello (`-Name "@"`, inklusive citattecken).</span><span class="sxs-lookup"><span data-stu-id="7e226-216">You cannot add or remove records from hello automatically created SOA record set at hello zone apex (`-Name "@"`, including quote marks).</span></span> <span data-ttu-id="7e226-217">Men du kan ändra någon av parametrarna hello inom hello SOA-post (utom ”värd”) och hello postuppsättning TTL-värde.</span><span class="sxs-lookup"><span data-stu-id="7e226-217">However, you can modify any of hello parameters within hello SOA record (except "Host") and hello record set TTL.</span></span>

<span data-ttu-id="7e226-218">följande exempel visar hur hello toochange hello *e-post* -egenskapen för hello SOA-post:</span><span class="sxs-lookup"><span data-stu-id="7e226-218">hello following example shows how toochange hello *Email* property of hello SOA record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="7e226-219">toomodify NS-poster på hello zonens apex</span><span class="sxs-lookup"><span data-stu-id="7e226-219">toomodify NS records at hello zone apex</span></span>

<span data-ttu-id="7e226-220">hello NS-postuppsättning på zonens apex hello skapas automatiskt med varje DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="7e226-220">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="7e226-221">Den innehåller hello namnen på hello Azure DNS-namnet servrar tilldelade toohello zon.</span><span class="sxs-lookup"><span data-stu-id="7e226-221">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="7e226-222">Du kan lägga till ytterligare namn servrar toothis NS postuppsättningen, toosupport samordna värd domäner med mer än en DNS-leverantör.</span><span class="sxs-lookup"><span data-stu-id="7e226-222">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="7e226-223">Du kan också ändra hello TTL-värde och metadata för den här postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="7e226-223">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="7e226-224">Du kan inte ta bort eller ändra hello förifyllda Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="7e226-224">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="7e226-225">Observera att detta gäller endast toohello NS postuppsättning på zonens apex hello.</span><span class="sxs-lookup"><span data-stu-id="7e226-225">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="7e226-226">Andra NS-postuppsättningar i zonen (som används toodelegate underordnade zoner) kan ändras utan begränsning.</span><span class="sxs-lookup"><span data-stu-id="7e226-226">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="7e226-227">hello som följande exempel visar hur tooadd en ytterligare toohello NS namnserverposten ange på hello zonens apex:</span><span class="sxs-lookup"><span data-stu-id="7e226-227">hello following example shows how tooadd an additional name server toohello NS record set at hello zone apex:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-record-set-metadata"></a><span data-ttu-id="7e226-228">toomodify postuppsättning metadata</span><span class="sxs-lookup"><span data-stu-id="7e226-228">toomodify record set metadata</span></span>

<span data-ttu-id="7e226-229">[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan vara används tooassociate programspecifika data med varje postuppsättningen, som nyckel / värde-par.</span><span class="sxs-lookup"><span data-stu-id="7e226-229">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="7e226-230">hello följande exempel visas hur toomodify hello metadata för en befintlig post angetts:</span><span class="sxs-lookup"><span data-stu-id="7e226-230">hello following example shows how toomodify hello metadata of an existing record set:</span></span>

```powershell
# Get hello record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a><span data-ttu-id="7e226-231">Ta bort en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="7e226-231">Delete a record set</span></span>

<span data-ttu-id="7e226-232">Postuppsättningar kan tas bort med hjälp av hello `Remove-AzureRmDnsRecordSet` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7e226-232">Record sets can be deleted by using hello `Remove-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="7e226-233">En postuppsättning också tar bort alla poster inom hello uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="7e226-233">Deleting a record set also deletes all records within hello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="7e226-234">Du kan inte ta bort hello SOA- och NS postuppsättningar vid basdomänen hello (`-Name '@'`).</span><span class="sxs-lookup"><span data-stu-id="7e226-234">You cannot delete hello SOA and NS record sets at hello zone apex (`-Name '@'`).</span></span>  <span data-ttu-id="7e226-235">Azure DNS skapas de automatiskt när hello zonen skapades och tar bort dem automatiskt när hello zon tas bort.</span><span class="sxs-lookup"><span data-stu-id="7e226-235">Azure DNS created these automatically when hello zone was created, and deletes them automatically when hello zone is deleted.</span></span>

<span data-ttu-id="7e226-236">hello följande exempel visas hur toodelete en postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="7e226-236">hello following example shows how toodelete a record set.</span></span> <span data-ttu-id="7e226-237">I det här exemplet anges hello postuppsättningens namn, postuppsättning typ, zonnamnet och resursgruppen varje explicit.</span><span class="sxs-lookup"><span data-stu-id="7e226-237">In this example, hello record set name, record set type, zone name, and resource group are each specified explicitly.</span></span>

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="7e226-238">Du kan också hello uppsättningen av poster kan anges med namn och typ och hello zonen angivna med hjälp av ett objekt:</span><span class="sxs-lookup"><span data-stu-id="7e226-238">Alternatively, hello record set can be specified by name and type, and hello zone specified using an object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

<span data-ttu-id="7e226-239">Som ett tredje alternativ, kan du ange hello-postuppsättning sig själv med en postuppsättning-objekt:</span><span class="sxs-lookup"><span data-stu-id="7e226-239">As a third option, hello record set itself can be specified using a record set object:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="7e226-240">När du anger hello postuppsättning toobe tas bort med hjälp av en postuppsättning objektet [Etag kontrollerar](dns-zones-records.md#etags) används tooensure samtidiga ändringar tas inte bort.</span><span class="sxs-lookup"><span data-stu-id="7e226-240">When you specify hello record set toobe deleted by using a record set object, [Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not deleted.</span></span> <span data-ttu-id="7e226-241">Du kan använda valfri hello `-Overwrite` växla toosuppress kontrollerna.</span><span class="sxs-lookup"><span data-stu-id="7e226-241">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

<span data-ttu-id="7e226-242">hello postuppsättning objekt kan även skickas i stället för att skickas som en parameter:</span><span class="sxs-lookup"><span data-stu-id="7e226-242">hello record set object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a><span data-ttu-id="7e226-243">Konfigurera meddelanden</span><span class="sxs-lookup"><span data-stu-id="7e226-243">Confirmation prompts</span></span>

<span data-ttu-id="7e226-244">Hej `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, och `Remove-AzureRmDnsRecordSet` cmdlets alla stöder konfigurera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="7e226-244">hello `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, and `Remove-AzureRmDnsRecordSet` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="7e226-245">Varje cmdlet uppmanas att bekräfta om hello `$ConfirmPreference` PowerShell inställningsvariabeln har värdet `Medium` eller lägre.</span><span class="sxs-lookup"><span data-stu-id="7e226-245">Each cmdlet prompts for confirmation if hello `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="7e226-246">Eftersom hello standardvärdet för `$ConfirmPreference` är `High`, dessa frågor inte anges när du använder hello standardinställningarna för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7e226-246">Since hello default value for `$ConfirmPreference` is `High`, these prompts are not given when using hello default PowerShell settings.</span></span>

<span data-ttu-id="7e226-247">Du kan åsidosätta hello aktuella `$ConfirmPreference` inställningen med hello `-Confirm` parameter.</span><span class="sxs-lookup"><span data-stu-id="7e226-247">You can override hello current `$ConfirmPreference` setting using hello `-Confirm` parameter.</span></span> <span data-ttu-id="7e226-248">Om du anger `-Confirm` eller `-Confirm:$True` , hello cmdlet uppmanar dig att bekräfta innan den körs.</span><span class="sxs-lookup"><span data-stu-id="7e226-248">If you specify `-Confirm` or `-Confirm:$True` , hello cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="7e226-249">Om du anger `-Confirm:$False` , hello cmdleten visas inte efter bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="7e226-249">If you specify `-Confirm:$False` , hello cmdlet does not prompt you for confirmation.</span></span> 

<span data-ttu-id="7e226-250">Mer information om `-Confirm` och `$ConfirmPreference`, se [om variabler för](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="7e226-250">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e226-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e226-251">Next steps</span></span>

<span data-ttu-id="7e226-252">Lär dig mer om [zoner och poster i Azure DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="7e226-252">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="7e226-253">Lär dig hur för[skydda zoner och poster](dns-protect-zones-recordsets.md) när du använder Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="7e226-253">Learn how too[protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
<br>
<span data-ttu-id="7e226-254">Granska hello [Azure DNS-PowerShell referensdokumentationen](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="7e226-254">Review hello [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>
