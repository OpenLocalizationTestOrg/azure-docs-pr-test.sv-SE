---
title: "Hantera DNS-poster i Azure DNS med hjälp av Azure PowerShell | Microsoft Docs"
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
ms.openlocfilehash: 2962e30e5d9c60b8e786e2ba79647cabfc5925cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a><span data-ttu-id="5a2fe-104">Hantera DNS-poster och postuppsättningar i Azure DNS med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5a2fe-104">Manage DNS records and recordsets in Azure DNS using Azure PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5a2fe-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5a2fe-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="5a2fe-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="5a2fe-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="5a2fe-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5a2fe-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="5a2fe-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5a2fe-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="5a2fe-109">Den här artikeln visar hur du hanterar DNS-posterna för DNS-zonen med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-109">This article shows you how to manage DNS records for your DNS zone by using Azure PowerShell.</span></span> <span data-ttu-id="5a2fe-110">DNS-poster kan också hanteras med hjälp av plattformsoberoende [Azure CLI](dns-operations-recordsets-cli.md) eller [Azure-portalen](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-110">DNS records can also be managed by using the cross-platform [Azure CLI](dns-operations-recordsets-cli.md) or the [Azure portal](dns-operations-recordsets-portal.md).</span></span>

<span data-ttu-id="5a2fe-111">Exemplen i den här artikeln förutsätter att du redan har [installerat Azure PowerShell loggat in, och skapa en DNS-zon](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-111">The examples in this article assume you have already [installed Azure PowerShell, signed in, and created a DNS zone](dns-operations-dnszones.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="5a2fe-112">Introduktion</span><span class="sxs-lookup"><span data-stu-id="5a2fe-112">Introduction</span></span>

<span data-ttu-id="5a2fe-113">Innan du skapar DNS-poster i Azure DNS, måste du först förstå hur Azure DNS organiserar DNS-poster i DNS-postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-113">Before creating DNS records in Azure DNS, you first need to understand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="5a2fe-114">Mer information om DNS-poster i Azure DNS finns i [DNS-zoner och poster](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-114">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>


## <a name="create-a-new-dns-record"></a><span data-ttu-id="5a2fe-115">Skapa en ny DNS-post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-115">Create a new DNS record</span></span>

<span data-ttu-id="5a2fe-116">Om din nya posten har samma namn och typ som en befintlig post, behöver du [lägga till den befintliga postuppsättning](#add-a-record-to-an-existing-record-set).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-116">If your new record has the same name and type as an existing record, you need to [add it to the existing record set](#add-a-record-to-an-existing-record-set).</span></span> <span data-ttu-id="5a2fe-117">Om din nya posten har ett annat namn och typ för alla befintliga poster, måste du skapa en ny uppsättning av poster.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-117">If your new record has a different name and type to all existing records, you need to create a new record set.</span></span> 

### <a name="create-a-records-in-a-new-record-set"></a><span data-ttu-id="5a2fe-118">Skapa ”A” poster i en ny uppsättning av poster</span><span class="sxs-lookup"><span data-stu-id="5a2fe-118">Create 'A' records in a new record set</span></span>

<span data-ttu-id="5a2fe-119">Du skapar postuppsättningar med hjälp av cmdleten `New-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-119">You create record sets by using the `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="5a2fe-120">När du skapar en uppsättning poster, måste du ange postuppsättning namn, zonen, tiden live (TTL), posttyp och posterna som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-120">When creating a record set, you need to specify the record set name, the zone, the time to live (TTL), the record type, and the records to be created.</span></span>

<span data-ttu-id="5a2fe-121">Parametrarna för att lägga till poster i en postuppsättning varierar beroende på typen av postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-121">The parameters for adding records to a record set vary depending on the type of the record set.</span></span> <span data-ttu-id="5a2fe-122">Till exempel när du använder en postuppsättning av typen ”A”, du måste ange IP-adressen med parametern `-IPv4Address`.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-122">For example, when using a record set of type 'A', you need to specify the IP address using the parameter `-IPv4Address`.</span></span> <span data-ttu-id="5a2fe-123">Andra parametrar används för andra typer av poster.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-123">Other parameters are used for other record types.</span></span> <span data-ttu-id="5a2fe-124">Se [ytterligare posttyp exempel](#additional-record-type-examples) mer information.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-124">See [Additional record type examples](#additional-record-type-examples) for details.</span></span>

<span data-ttu-id="5a2fe-125">I följande exempel skapas en postuppsättning med det relativa namnet ”www” i DNS-zonen ”contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-125">The following example creates a record set with the relative name 'www' in the DNS Zone 'contoso.com'.</span></span> <span data-ttu-id="5a2fe-126">Det fullständigt kvalificerade namnet på uppsättningen av poster är ”www.contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-126">The fully-qualified name of the record set is 'www.contoso.com'.</span></span> <span data-ttu-id="5a2fe-127">Posttypen är ”A”, och är TTL 3600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-127">The record type is 'A', and the TTL is 3600 seconds.</span></span> <span data-ttu-id="5a2fe-128">Uppsättningen poster innehåller en post med IP-adressen '1.2.3.4'.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-128">The record set contains a single record, with IP address '1.2.3.4'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="5a2fe-129">Att skapa en postuppsättning på apex för en zon (i det här fallet ”contoso.com”), använder du postuppsättningsnamnet ”@” (utan citattecken):</span><span class="sxs-lookup"><span data-stu-id="5a2fe-129">To create a record set at the 'apex' of a zone (in this case, 'contoso.com'), use the record set name '@' (excluding quotation marks):</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="5a2fe-130">Om du behöver skapa en postuppsättning innehåller fler än en post först skapa en lokal matris och lägga till poster och sedan skicka för matrisen på `New-AzureRmDnsRecordSet` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-130">If you need to create a record set containing more than one record, first create a local array and add the records, then pass the array to `New-AzureRmDnsRecordSet` as follows:</span></span>

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

<span data-ttu-id="5a2fe-131">[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan användas för att koppla programspecifika data till varje postuppsättningen, som nyckel / värde-par.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-131">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="5a2fe-132">I följande exempel visas hur du skapar en postuppsättning med två metadataposter ”Avd = ekonomi ' och ' miljö = produktion”.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-132">The following example shows how to create a record set with two metadata entries, 'dept=finance' and 'environment=production'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

<span data-ttu-id="5a2fe-133">Azure DNS stöder också 'empty-postuppsättningar som kan fungera som en platshållare för att reservera en DNS-namn innan du skapar DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-133">Azure DNS also supports 'empty' record sets, which can act as a placeholder to reserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="5a2fe-134">Tom postuppsättningar syns i Azure DNS-kontrollplan, men visas på Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-134">Empty record sets are visible in the Azure DNS control plane, but do appear on the Azure DNS name servers.</span></span> <span data-ttu-id="5a2fe-135">I följande exempel skapas en tom postuppsättning:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-135">The following example creates an empty record set:</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a><span data-ttu-id="5a2fe-136">Skapa poster för andra typer</span><span class="sxs-lookup"><span data-stu-id="5a2fe-136">Create records of other types</span></span>

<span data-ttu-id="5a2fe-137">Har sett i detalj hur du skapar ”A” poster, visar i följande exempel hur du skapar andra posttyper som stöds av Azure DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-137">Having seen in detail how to create 'A' records, the following examples show how to create records of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="5a2fe-138">I varje fall visar vi hur du skapar en postuppsättning med en enskild post.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-138">In each case, we show how to create a record set containing a single record.</span></span> <span data-ttu-id="5a2fe-139">Tidigare exemplen för ”A” poster kan anpassas att skapa postuppsättningar av andra typer som innehåller flera poster med metadata, eller skapa tom postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-139">The earlier examples for 'A' records can be adapted to create record sets of other types containing multiple records, with metadata, or to create empty record sets.</span></span>

<span data-ttu-id="5a2fe-140">Vi inte ger ett exempel för att skapa en SOA-poster, eftersom SOAs skapas och tas bort tillsammans med varje DNS-zon och kan inte skapas eller tas bort separat.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-140">We do not give an example to create an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="5a2fe-141">Dock [SOA kan ändras, som visas i en senare exempel](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-141">However, [the SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record-set-with-a-single-record"></a><span data-ttu-id="5a2fe-142">Skapa en AAAA-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-142">Create an AAAA record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a><span data-ttu-id="5a2fe-143">Skapa en CNAME-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-143">Create a CNAME record set with a single record</span></span>

> [!NOTE]
> <span data-ttu-id="5a2fe-144">DNS-standarden inte tillåter CNAME-poster på apex för en zon (`-Name '@'`), eller tillåter att postuppsättningar som innehåller fler än en post.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-144">The DNS standards do not permit CNAME records at the apex of a zone (`-Name '@'`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="5a2fe-145">Mer information finns i [CNAME-poster](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-145">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a><span data-ttu-id="5a2fe-146">Skapa en MX-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-146">Create an MX record set with a single record</span></span>

<span data-ttu-id="5a2fe-147">I det här exemplet använder vi postuppsättningsnamnet ”@” så här skapar du en MX-post på zonens apex (i det här fallet ”contoso.com”).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-147">In this example, we use the record set name '@' to create an MX record at the zone apex (in this case, 'contoso.com').</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a><span data-ttu-id="5a2fe-148">Skapa en NS-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-148">Create an NS record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a><span data-ttu-id="5a2fe-149">Skapa en PTR-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-149">Create a PTR record set with a single record</span></span>

<span data-ttu-id="5a2fe-150">I det här fallet ' min-arpa-zone.com som representerar den ARPA zon för omvänd sökning som motsvarar IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-150">In this case, 'my-arpa-zone.com' represents the ARPA reverse lookup zone representing your IP range.</span></span> <span data-ttu-id="5a2fe-151">Varje PTR-post som har angetts i den här zonen motsvarar en IP-adress i IP-intervallet.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-151">Each PTR record set in this zone corresponds to an IP address within this IP range.</span></span> <span data-ttu-id="5a2fe-152">Postnamnet ”10” är den sista oktetten i IP-adress inom den här IP-adressintervall som representeras av den här posten.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-152">The record name '10' is the last octet of the IP address within this IP range represented by this record.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a><span data-ttu-id="5a2fe-153">Skapa en SRV-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-153">Create an SRV record set with a single record</span></span>

<span data-ttu-id="5a2fe-154">När du skapar en [SRV-postuppsättning](dns-zones-records.md#srv-records), ange den  *\_service* och  *\_protokollet* i postuppsättningens namn.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-154">When creating an [SRV record set](dns-zones-records.md#srv-records), specify the *\_service* and *\_protocol* in the record set name.</span></span> <span data-ttu-id="5a2fe-155">Behöver du inte inkludera ”@” i namnet på postuppsättningen när du skapar en SRV-post på zonens apex.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-155">There is no need to include '@' in the record set name when creating an SRV record set at the zone apex.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a><span data-ttu-id="5a2fe-156">Skapa en TXT-postuppsättning med en post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-156">Create a TXT record set with a single record</span></span>

<span data-ttu-id="5a2fe-157">I följande exempel visas hur du skapar en TXT-post.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-157">The following example shows how to create a TXT record.</span></span> <span data-ttu-id="5a2fe-158">Mer information om den maximala stränglängden som stöds i TXT-poster finns [TXT-poster](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-158">For more information about the maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a><span data-ttu-id="5a2fe-159">Hämta en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="5a2fe-159">Get a record set</span></span>

<span data-ttu-id="5a2fe-160">Använd för att hämta en befintlig postuppsättning `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-160">To retrieve an existing record set, use `Get-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="5a2fe-161">Denna cmdlet returnerar ett lokalt objekt som representerar postuppsättningen i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-161">This cmdlet returns a local object that represents the record set in Azure DNS.</span></span>

<span data-ttu-id="5a2fe-162">Precis som med `New-AzureRmDnsRecordSet`, namnet på postuppsättningen anges måste vara en *relativa* namn, vilket innebär att den inte får innehålla zonnamnet.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-162">As with `New-AzureRmDnsRecordSet`, the record set name given must be a *relative* name, meaning it must exclude the zone name.</span></span> <span data-ttu-id="5a2fe-163">Du måste också ange typ av post och ange den zon som innehåller posten.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-163">You also need to specify the record type, and the zone containing the record set.</span></span>

<span data-ttu-id="5a2fe-164">I följande exempel visas hur du hämtar en postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-164">The following example shows how to retrieve a record set.</span></span> <span data-ttu-id="5a2fe-165">I det här exemplet zonen har angetts med hjälp av den `-ZoneName` och `-ResourceGroupName` parametrar.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-165">In this example, the zone is specified using the `-ZoneName` and `-ResourceGroupName` parameters.</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="5a2fe-166">Alternativt kan du också ange zonen med en zon-objekt som överförts med hjälp av den `-Zone` parameter.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-166">Alternatively, you can also specify the zone using a zone object, passed using the `-Zone` parameter.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a><span data-ttu-id="5a2fe-167">Lista över postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="5a2fe-167">List record sets</span></span>

<span data-ttu-id="5a2fe-168">Du kan också använda `Get-AzureRmDnsZone` till listan uppsättningar av poster i en zon genom att utelämna den `-Name` och/eller `-RecordType` parametrar.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-168">You can also use `Get-AzureRmDnsZone` to list record sets in a zone, by omitting the `-Name` and/or `-RecordType` parameters.</span></span>

<span data-ttu-id="5a2fe-169">I följande exempel returneras alla postuppsättningar i zonen:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-169">The following example returns all record sets in the zone:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="5a2fe-170">I följande exempel visas hur alla uppsättningar av en viss typ kan hämtas genom att ange typ av post när utelämna posten anges namn:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-170">The following example shows how all record sets of a given type can be retrieved by specifying the record type while omitting the record set name:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="5a2fe-171">Om du vill hämta alla postuppsättningar med ett angivet namn över posttyper, måste du hämta alla postuppsättningar och filtrera resultatet:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-171">To retrieve all record sets with a given name, across record types, you need to retrieve all record sets and then filter the results:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

<span data-ttu-id="5a2fe-172">I alla ovanstående exempel zonen anges antingen med hjälp av den `-ZoneName` och `-ResourceGroupName`parametrar (som visas), eller genom att ange ett objekt i zonen:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-172">In all the above examples, the zone can be specified either by using the `-ZoneName` and `-ResourceGroupName`parameters (as shown), or by specifying a zone object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-to-an-existing-record-set"></a><span data-ttu-id="5a2fe-173">Lägga till en post i en befintlig postuppsättning</span><span class="sxs-lookup"><span data-stu-id="5a2fe-173">Add a record to an existing record set</span></span>

<span data-ttu-id="5a2fe-174">Lägg till en post i en befintlig postuppsättningen, gör du följande tre steg:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-174">To add a record to an existing record set, follow the following three steps:</span></span>

1. <span data-ttu-id="5a2fe-175">Hämta uppsättningen av befintliga poster</span><span class="sxs-lookup"><span data-stu-id="5a2fe-175">Get the existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="5a2fe-176">Lägga till den nya posten i den lokala postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-176">Add the new record to the local record set.</span></span> <span data-ttu-id="5a2fe-177">Detta är en åtgärd som är offline.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-177">This is an off-line operation.</span></span>

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="5a2fe-178">Tillämpa ändringen tillbaka till Azure DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-178">Commit the change back to the Azure DNS service.</span></span> 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

<span data-ttu-id="5a2fe-179">Med hjälp av `Set-AzureRmDnsRecordSet` *ersätter* befintliga posten i Azure DNS (och alla poster som den innehåller) med uppsättningen av poster anges.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-179">Using `Set-AzureRmDnsRecordSet` *replaces* the existing record set in Azure DNS (and all records it contains) with the record set specified.</span></span> <span data-ttu-id="5a2fe-180">[ETag kontrollerar](dns-zones-records.md#etags) används för att kontrollera samtidiga ändringar inte att skrivas över.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-180">[Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="5a2fe-181">Du kan använda det valfria `-Overwrite` växel för att ignorera dessa kontroller.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-181">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

<span data-ttu-id="5a2fe-182">Den här sekvensen av åtgärder kan också vara *skickas*, vilket innebär att du skickar postuppsättning objektet genom att använda pipe i stället för att skickas som en parameter:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-182">This sequence of operations can also be *piped*, meaning you pass the record set object by using the pipe rather than passing it as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="5a2fe-183">Exemplen ovan visar hur du kan lägga till en ”A” till en uppsättning poster av typen ”A”.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-183">The examples above show how to add an 'A' record to an existing record set of type 'A'.</span></span> <span data-ttu-id="5a2fe-184">En liknande sekvens med åtgärder som används för att lägga till poster i postuppsättningar av andra typer som ersätter den `-Ipv4Address` -parametern för `Add-AzureRmDnsRecordConfig` med andra parametrar som är specifika för varje posttyp.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-184">A similar sequence of operations is used to add records to record sets of other types, substituting the `-Ipv4Address` parameter of `Add-AzureRmDnsRecordConfig` with other parameters specific to each record type.</span></span> <span data-ttu-id="5a2fe-185">Parametrarna för varje posttyp är samma som för den `New-AzureRmDnsRecordConfig` cmdlet, som visas i [ytterligare posttyp exempel](#additional-record-type-examples) ovan.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-185">The parameters for each record type are the same as for the `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>

<span data-ttu-id="5a2fe-186">Postuppsättningar av typen 'CNAME' eller 'SOA-' får inte innehålla fler än en post.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-186">Record sets of type 'CNAME' or 'SOA' cannot contain more than one record.</span></span> <span data-ttu-id="5a2fe-187">Den här begränsningen uppstår från DNS-standarden.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-187">This constraint arises from the DNS standards.</span></span> <span data-ttu-id="5a2fe-188">Det är inte en begränsning i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-188">It is not a limitation of Azure DNS.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="5a2fe-189">Ta bort en post från en befintlig post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-189">Remove a record from an existing record set</span></span>

<span data-ttu-id="5a2fe-190">Processen för att ta bort en post från en postuppsättning liknar processen att lägga till en post i en befintlig postuppsättning:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-190">The process to remove a record from a record set is similar to the process to add a record to an existing record set:</span></span>

1. <span data-ttu-id="5a2fe-191">Hämta uppsättningen av befintliga poster</span><span class="sxs-lookup"><span data-stu-id="5a2fe-191">Get the existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="5a2fe-192">Ta bort posten från lokala postuppsättning-objekt.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-192">Remove the record from the local record set object.</span></span> <span data-ttu-id="5a2fe-193">Detta är en åtgärd som är offline.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-193">This is an off-line operation.</span></span> <span data-ttu-id="5a2fe-194">Posten som tas bort måste vara en exakt matchning med en befintlig post över alla parametrar.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-194">The record that's being removed must be an exact match with an existing record across all parameters.</span></span>

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="5a2fe-195">Tillämpa ändringen tillbaka till Azure DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-195">Commit the change back to the Azure DNS service.</span></span> <span data-ttu-id="5a2fe-196">Använd det valfria `-Overwrite` växel för att undertrycka [Etag kontrollerar](dns-zones-records.md#etags) för samtidiga ändringar.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-196">Use the optional `-Overwrite` switch to suppress [Etag checks](dns-zones-records.md#etags) for concurrent changes.</span></span>

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

<span data-ttu-id="5a2fe-197">Med hjälp av sekvensen ovan för att ta bort den sista posten från en uppsättning poster tas inte bort uppsättningen av poster, i stället de lämnar en tom postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-197">Using the above sequence to remove the last record from a record set does not delete the record set, rather it leaves an empty record set.</span></span> <span data-ttu-id="5a2fe-198">Om du vill ta bort en postuppsättning helt finns [ta bort en postuppsättning](#delete-a-record-set).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-198">To remove a record set entirely, see [Delete a record set](#delete-a-record-set).</span></span>

<span data-ttu-id="5a2fe-199">På liknande sätt att lägga till poster i en postuppsättning kan sekvens med åtgärder för att ta bort en uppsättning poster också skickas:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-199">Similarly to adding records to a record set, the sequence of operations to remove a record set can also be piped:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="5a2fe-200">Olika posttyper stöds genom att skicka lämpliga parametrar för typspecifika att `Remove-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-200">Different record types are supported by passing the appropriate type-specific parameters to `Remove-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="5a2fe-201">Parametrarna för varje posttyp är samma som för den `New-AzureRmDnsRecordConfig` cmdlet, som visas i [ytterligare posttyp exempel](#additional-record-type-examples) ovan.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-201">The parameters for each record type are the same as for the `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>


## <a name="modify-an-existing-record-set"></a><span data-ttu-id="5a2fe-202">Ändra en befintlig postuppsättning</span><span class="sxs-lookup"><span data-stu-id="5a2fe-202">Modify an existing record set</span></span>

<span data-ttu-id="5a2fe-203">Stegen för att ändra en befintlig postuppsättning liknar de steg du kan vidta när du lägger till eller ta bort poster från en postuppsättning:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-203">The steps for modifying an existing record set are similar to the steps you take when adding or removing records from a record set:</span></span>

1. <span data-ttu-id="5a2fe-204">Hämta den befintliga-postuppsättning med hjälp av `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-204">Retrieve the existing record set by using `Get-AzureRmDnsRecordSet`.</span></span>
2. <span data-ttu-id="5a2fe-205">Ändra objektet lokala postuppsättningen av:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-205">Modify the local record set object by:</span></span>
    * <span data-ttu-id="5a2fe-206">Tillägg eller borttagning av poster</span><span class="sxs-lookup"><span data-stu-id="5a2fe-206">Adding or removing records</span></span>
    * <span data-ttu-id="5a2fe-207">Ändra parametrarna för befintliga poster</span><span class="sxs-lookup"><span data-stu-id="5a2fe-207">Changing the parameters of existing records</span></span>
    * <span data-ttu-id="5a2fe-208">Ändra posten ange metadata och tid till live (TTL)</span><span class="sxs-lookup"><span data-stu-id="5a2fe-208">Changing the record set metadata and time to live (TTL)</span></span>
3. <span data-ttu-id="5a2fe-209">Genomför ändringarna med hjälp av den `Set-AzureRmDnsRecordSet` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-209">Commit your changes by using the `Set-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="5a2fe-210">Detta *ersätter* befintliga posten i Azure DNS med uppsättningen av poster anges.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-210">This *replaces* the existing record set in Azure DNS with the record set specified.</span></span>

<span data-ttu-id="5a2fe-211">När du använder `Set-AzureRmDnsRecordSet`, [Etag kontrollerar](dns-zones-records.md#etags) används för att kontrollera samtidiga ändringar inte att skrivas över.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-211">When using `Set-AzureRmDnsRecordSet`, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="5a2fe-212">Du kan använda det valfria `-Overwrite` växel för att ignorera dessa kontroller.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-212">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

### <a name="to-update-a-record-in-an-existing-record-set"></a><span data-ttu-id="5a2fe-213">Att uppdatera en post i en befintlig postuppsättning</span><span class="sxs-lookup"><span data-stu-id="5a2fe-213">To update a record in an existing record set</span></span>

<span data-ttu-id="5a2fe-214">I det här exemplet ändrar vi IP-adressen för en befintlig ”A” post:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-214">In this example, we change the IP address of an existing 'A' record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-an-soa-record"></a><span data-ttu-id="5a2fe-215">Att ändra en SOA-post</span><span class="sxs-lookup"><span data-stu-id="5a2fe-215">To modify an SOA record</span></span>

<span data-ttu-id="5a2fe-216">Du kan inte lägga till eller ta bort poster från den automatiskt skapade SOA-postuppsättning på zonens apex (`-Name "@"`, inklusive citattecken).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-216">You cannot add or remove records from the automatically created SOA record set at the zone apex (`-Name "@"`, including quote marks).</span></span> <span data-ttu-id="5a2fe-217">Men du kan ändra någon av parametrarna i SOA-post (utom ”värd”) och postuppsättning TTL-värde.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-217">However, you can modify any of the parameters within the SOA record (except "Host") and the record set TTL.</span></span>

<span data-ttu-id="5a2fe-218">I följande exempel visas hur du ändrar den *e-post* -egenskapen för SOA-post:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-218">The following example shows how to change the *Email* property of the SOA record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a><span data-ttu-id="5a2fe-219">Att ändra NS-poster på zonens apex</span><span class="sxs-lookup"><span data-stu-id="5a2fe-219">To modify NS records at the zone apex</span></span>

<span data-ttu-id="5a2fe-220">NS-postuppsättning på zonens apex skapas automatiskt med varje DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-220">The NS record set at the zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="5a2fe-221">Den innehåller namnen på Azure DNS-namnservrar tilldelas zonen.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-221">It contains the names of the Azure DNS name servers assigned to the zone.</span></span>

<span data-ttu-id="5a2fe-222">Du kan lägga till ytterligare servrar till den här NS-post inställda på en värd domäner med mer än en DNS-leverantör har stöd ett namn.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-222">You can add additional name servers to this NS record set, to support co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="5a2fe-223">Du kan också ändra TTL-värde och metadata för den här postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-223">You can also modify the TTL and metadata for this record set.</span></span> <span data-ttu-id="5a2fe-224">Du kan inte ta bort eller ändra de i förväg Azure DNS-namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-224">However, you cannot remove or modify the pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="5a2fe-225">Observera att detta gäller endast NS-postuppsättning på zonens apex.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-225">Note that this applies only to the NS record set at the zone apex.</span></span> <span data-ttu-id="5a2fe-226">Andra NS postuppsättningar i zonen (som används för att delegera underordnade zoner) kan ändras utan begränsning.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-226">Other NS record sets in your zone (as used to delegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="5a2fe-227">I följande exempel visas hur du lägger till en ytterligare namnserver NS-postuppsättning på zonens apex:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-227">The following example shows how to add an additional name server to the NS record set at the zone apex:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-record-set-metadata"></a><span data-ttu-id="5a2fe-228">Att ändra postuppsättning metadata</span><span class="sxs-lookup"><span data-stu-id="5a2fe-228">To modify record set metadata</span></span>

<span data-ttu-id="5a2fe-229">[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan användas för att koppla programspecifika data till varje postuppsättningen, som nyckel / värde-par.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-229">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="5a2fe-230">I följande exempel visas hur du ändrar metadata för en befintlig postuppsättning:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-230">The following example shows how to modify the metadata of an existing record set:</span></span>

```powershell
# Get the record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a><span data-ttu-id="5a2fe-231">Ta bort en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="5a2fe-231">Delete a record set</span></span>

<span data-ttu-id="5a2fe-232">Postuppsättningar kan tas bort med hjälp av den `Remove-AzureRmDnsRecordSet` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-232">Record sets can be deleted by using the `Remove-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="5a2fe-233">En postuppsättning också tar du bort alla posterna i uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-233">Deleting a record set also deletes all records within the record set.</span></span>

> [!NOTE]
> <span data-ttu-id="5a2fe-234">Du kan inte ta bort SOA och NS-post anger på zonens apex (`-Name '@'`).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-234">You cannot delete the SOA and NS record sets at the zone apex (`-Name '@'`).</span></span>  <span data-ttu-id="5a2fe-235">Azure DNS skapas de automatiskt när zonen skapades och tar bort dem automatiskt när zonen tas bort.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-235">Azure DNS created these automatically when the zone was created, and deletes them automatically when the zone is deleted.</span></span>

<span data-ttu-id="5a2fe-236">I följande exempel visas hur du tar bort en uppsättning poster.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-236">The following example shows how to delete a record set.</span></span> <span data-ttu-id="5a2fe-237">I det här exemplet anges postuppsättningsnamnet, postuppsättning typ, zonnamnet och resursgruppen varje explicit.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-237">In this example, the record set name, record set type, zone name, and resource group are each specified explicitly.</span></span>

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="5a2fe-238">Uppsättningen av poster kan också anges med namn och typ och zonen anges med ett objekt:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-238">Alternatively, the record set can be specified by name and type, and the zone specified using an object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

<span data-ttu-id="5a2fe-239">Som ett tredje alternativ, anges postuppsättningen själva med en postuppsättning objektet:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-239">As a third option, the record set itself can be specified using a record set object:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="5a2fe-240">När du anger posten som ska tas bort med hjälp av en postuppsättning objektet [Etag kontrollerar](dns-zones-records.md#etags) används för att kontrollera samtidiga ändringar inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-240">When you specify the record set to be deleted by using a record set object, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not deleted.</span></span> <span data-ttu-id="5a2fe-241">Du kan använda det valfria `-Overwrite` växel för att ignorera dessa kontroller.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-241">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

<span data-ttu-id="5a2fe-242">Postuppsättning-objekt kan även skickas i stället för att skickas som en parameter:</span><span class="sxs-lookup"><span data-stu-id="5a2fe-242">The record set object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a><span data-ttu-id="5a2fe-243">Konfigurera meddelanden</span><span class="sxs-lookup"><span data-stu-id="5a2fe-243">Confirmation prompts</span></span>

<span data-ttu-id="5a2fe-244">Den `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, och `Remove-AzureRmDnsRecordSet` cmdlets alla stöder konfigurera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-244">The `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, and `Remove-AzureRmDnsRecordSet` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="5a2fe-245">Varje cmdlet uppmanas att bekräfta om den `$ConfirmPreference` PowerShell inställningsvariabeln har värdet `Medium` eller lägre.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-245">Each cmdlet prompts for confirmation if the `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="5a2fe-246">Eftersom standardvärdet för `$ConfirmPreference` är `High`, dessa frågor inte anges när du använder standardinställningarna för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-246">Since the default value for `$ConfirmPreference` is `High`, these prompts are not given when using the default PowerShell settings.</span></span>

<span data-ttu-id="5a2fe-247">Du kan åsidosätta aktuellt `$ConfirmPreference` inställningen med hjälp av den `-Confirm` parameter.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-247">You can override the current `$ConfirmPreference` setting using the `-Confirm` parameter.</span></span> <span data-ttu-id="5a2fe-248">Om du anger `-Confirm` eller `-Confirm:$True` , så uppmanas du att bekräfta innan den körs.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-248">If you specify `-Confirm` or `-Confirm:$True` , the cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="5a2fe-249">Om du anger `-Confirm:$False` , cmdleten visas inte efter bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-249">If you specify `-Confirm:$False` , the cmdlet does not prompt you for confirmation.</span></span> 

<span data-ttu-id="5a2fe-250">Mer information om `-Confirm` och `$ConfirmPreference`, se [om variabler för](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-250">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a2fe-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a2fe-251">Next steps</span></span>

<span data-ttu-id="5a2fe-252">Lär dig mer om [zoner och poster i Azure DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-252">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="5a2fe-253">Lär dig hur du [skydda zoner och poster](dns-protect-zones-recordsets.md) när du använder Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5a2fe-253">Learn how to [protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
<br>
<span data-ttu-id="5a2fe-254">Granska de [Azure DNS-PowerShell referensdokumentationen](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="5a2fe-254">Review the [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>
