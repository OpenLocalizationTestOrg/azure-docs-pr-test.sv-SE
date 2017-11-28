---
title: "Hantera DNS-poster i Azure DNS använder Azure CLI 2.0 | Microsoft Docs"
description: "Hantera DNS-postuppsättningar och på Azure DNS-poster när värd för din domän på Azure DNS. Alla CLI 2.0-kommandon för åtgärder på uppsättningar av poster och poster."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: jonatul
ms.openlocfilehash: 9543759d7ba88c7c5068021cebbeec6b8d63633e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-the-azure-cli-20"></a><span data-ttu-id="f3a9b-104">Hantera DNS-poster och postuppsättningar i Azure DNS använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f3a9b-104">Manage DNS records and recordsets in Azure DNS using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f3a9b-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f3a9b-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="f3a9b-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f3a9b-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="f3a9b-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f3a9b-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="f3a9b-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f3a9b-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="f3a9b-109">Den här artikeln visar hur du hanterar DNS-posterna för DNS-zonen med hjälp av plattformsoberoende Azure-kommandoradsgränssnittet (CLI) 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-109">This article shows you how to manage DNS records for your DNS zone by using the cross-platform Azure command-line interface (CLI) 2.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="f3a9b-110">Du kan också hantera DNS-posterna med [Azure PowerShell](dns-operations-recordsets.md) eller [Azure-portalen](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-110">You can also manage your DNS records using [Azure PowerShell](dns-operations-recordsets.md) or the [Azure portal](dns-operations-recordsets-portal.md).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="f3a9b-111">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="f3a9b-111">CLI versions to complete the task</span></span>

<span data-ttu-id="f3a9b-112">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="f3a9b-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) – vår CLI för distributionsmodellerna klassisk och resurshantering.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="f3a9b-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) – vår nästa generations CLI för distributionsmodellen resurshantering.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) - our next generation CLI for the resource management deployment model.</span></span>

<span data-ttu-id="f3a9b-115">Exemplen i den här artikeln förutsätter att du redan har [installerat Azure CLI 2.0 loggat in, och skapa en DNS-zon](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-115">The examples in this article assume you have already [installed the Azure CLI 2.0, signed in, and created a DNS zone](dns-operations-dnszones-cli.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="f3a9b-116">Introduktion</span><span class="sxs-lookup"><span data-stu-id="f3a9b-116">Introduction</span></span>

<span data-ttu-id="f3a9b-117">Innan du skapar DNS-poster i Azure DNS, måste du först förstå hur Azure DNS organiserar DNS-poster i DNS-postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-117">Before creating DNS records in Azure DNS, you first need to understand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="f3a9b-118">Mer information om DNS-poster i Azure DNS finns i [DNS-zoner och poster](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-118">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="f3a9b-119">Skapa en DNS-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-119">Create a DNS record</span></span>

<span data-ttu-id="f3a9b-120">Du kan skapa en DNS-post med den `az network dns record-set <record-type> set-record` kommando (där `<record-type>` är typ av post engångsfaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="f3a9b-120">To create a DNS record, use the `az network dns record-set <record-type> set-record` command (where `<record-type>` is the type of record, i.e</span></span> <span data-ttu-id="f3a9b-121">en, srv txt, etc.) Om du vill ha hjälp, så gå till `az network dns record-set --help`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-121">a, srv, txt, etc.) For help, see `az network dns record-set --help`.</span></span>

<span data-ttu-id="f3a9b-122">När du skapar en post måste du ange resursgruppsnamn, zonnamn, postuppsättningsnamn, posttyp och information om posten som skapas.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-122">When creating a record, you need to specify the resource group name, zone name, record set name, the record type, and the details of the record being created.</span></span> <span data-ttu-id="f3a9b-123">Namnet på postuppsättningen anges måste vara en *relativa* namn, vilket innebär att den inte får innehålla zonnamnet.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-123">The record set name given must be a *relative* name, meaning it must exclude the zone name.</span></span>

<span data-ttu-id="f3a9b-124">Om postuppsättningen inte redan finns, skapar det här kommandot den åt dig.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-124">If the record set does not already exist, this command creates it for you.</span></span> <span data-ttu-id="f3a9b-125">Om postuppsättningen redan finns, lägger det här kommandot till posten som du anger till den befintliga postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-125">If the record set already exists, this command adds the record you specify to the existing record set.</span></span>

<span data-ttu-id="f3a9b-126">Om en ny postuppsättning skapas, används ett standard TTL-värde (Time to Live) på 3600.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-126">If a new record set is created, a default time-to-live (TTL) of 3600 is used.</span></span> <span data-ttu-id="f3a9b-127">Instruktioner om hur du använder olika TTLs finns [skapa en DNS-postuppsättning](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-127">For instructions on how to use different TTLs, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="f3a9b-128">Följande exempel skapar en A-post som heter *www* i zonen *contoso.com* i resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-128">The following example creates an A record called *www* in the zone *contoso.com* in the resource group *MyResourceGroup*.</span></span> <span data-ttu-id="f3a9b-129">IP-adressen för A-posten är *1.2.3.4*.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-129">The IP address of the A record is *1.2.3.4*.</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="f3a9b-130">Skapa en post överst i zonen (i det här fallet ”contoso.com”) genom att använda postnamnet "@", inklusive citattecknen.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-130">To create a record set in the apex of the zone (in this case, "contoso.com"), use the record name "@", including the quotation marks:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

## <a name="create-a-dns-record-set"></a><span data-ttu-id="f3a9b-131">Skapa en uppsättning av DNS-poster</span><span class="sxs-lookup"><span data-stu-id="f3a9b-131">Create a DNS record set</span></span>

<span data-ttu-id="f3a9b-132">I ovanstående exempel DNS-posten antingen har lagts till i en befintlig postuppsättning eller uppsättningen av poster har skapats *implicit*.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-132">In the above examples, the DNS record was either added to an existing record set, or the record set was created *implicitly*.</span></span> <span data-ttu-id="f3a9b-133">Du kan också skapa uppsättningen av poster *explicit* innan du lägger till poster.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-133">You can also create the record set *explicitly* before adding records to it.</span></span> <span data-ttu-id="f3a9b-134">Azure DNS stöder 'empty-postuppsättningar som kan fungera som en platshållare för att reservera en DNS-namn innan du skapar DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-134">Azure DNS supports 'empty' record sets, which can act as a placeholder to reserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="f3a9b-135">Tom postuppsättningar syns i Azure DNS-kontrollplan, men visas inte i Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-135">Empty record sets are visible in the Azure DNS control plane, but do not appear on the Azure DNS name servers.</span></span>

<span data-ttu-id="f3a9b-136">Postuppsättningar skapas med hjälp av den `az network dns record-set <record-type> create` kommando.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-136">Record sets are created using the `az network dns record-set <record-type> create` command.</span></span> <span data-ttu-id="f3a9b-137">Om du vill ha hjälp, så gå till `az network dns record-set <record-type> create --help`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-137">For help, see `az network dns record-set <record-type> create --help`.</span></span>

<span data-ttu-id="f3a9b-138">Skapar posten explicit kan du ange postuppsättning egenskaper som den [Time To Live (TTL)](dns-zones-records.md#time-to-live) och metadata.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-138">Creating the record set explicitly allows you to specify record set properties such as the [Time-To-Live (TTL)](dns-zones-records.md#time-to-live) and metadata.</span></span> <span data-ttu-id="f3a9b-139">[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan användas för att koppla programspecifika data till varje postuppsättningen, som nyckel / värde-par.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-139">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="f3a9b-140">I följande exempel skapas en tom postuppsättningen av typen ”A” med en 60-sekunders TTL med hjälp av den `--ttl` parameter (kort form `-l`):</span><span class="sxs-lookup"><span data-stu-id="f3a9b-140">The following example creates an empty record set of type 'A' with a 60-second TTL, by using the `--ttl` parameter (short form `-l`):</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --ttl 60
```

<span data-ttu-id="f3a9b-141">I följande exempel skapas en postuppsättning med två metadataposter ”Avd = ekonomi” och ”miljö = produktion”, med hjälp av den `--metadata` parameter:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-141">The following example creates a record set with two metadata entries, "dept=finance" and "environment=production", by using the `--metadata` parameter :</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --metadata "dept=finance" "environment=production"
```

<span data-ttu-id="f3a9b-142">Att ha skapat en tom postuppsättningen, poster kan läggas till med `azure network dns record-set <record-type> set-record` enligt beskrivningen i [skapa en DNS-post](#create-a-dns-record).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-142">Having created an empty record set, records can be added using `azure network dns record-set <record-type> set-record` as described in [Create a DNS record](#create-a-dns-record).</span></span>

## <a name="create-records-of-other-types"></a><span data-ttu-id="f3a9b-143">Skapa poster för andra typer</span><span class="sxs-lookup"><span data-stu-id="f3a9b-143">Create records of other types</span></span>

<span data-ttu-id="f3a9b-144">Har sett i detalj hur du skapar ”A” poster, visar i följande exempel hur du skapar andra posttyper som stöds av Azure DNS-post.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-144">Having seen in detail how to create 'A' records, the following examples show how to create record of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="f3a9b-145">Parametrarna som används för att ange postdata varierar beroende på posttypen.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-145">The parameters used to specify the record data vary depending on the type of the record.</span></span> <span data-ttu-id="f3a9b-146">För en post av typen "A", anger du exempelvis IPv4-adressen med parametern `--ipv4-address <IPv4 address>`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-146">For example, for a record of type "A", you specify the IPv4 address with the parameter `--ipv4-address <IPv4 address>`.</span></span> <span data-ttu-id="f3a9b-147">Parametrarna för varje posttyp kan visas med hjälp av `az network dns record-set <record-type> set-record --help`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-147">The parameters for each record type can be listed using `az network dns record-set <record-type> set-record --help`.</span></span>

<span data-ttu-id="f3a9b-148">I varje fall visar vi hur du skapar en enskild post.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-148">In each case, we show how to create a single record.</span></span> <span data-ttu-id="f3a9b-149">Posten läggs till den befintliga uppsättningen av poster eller en uppsättning av poster som skapats implicit.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-149">The record is added to the existing record set, or a record set created implicitly.</span></span> <span data-ttu-id="f3a9b-150">Mer information om hur du skapar postuppsättningar och definierar post parametern uttryckligen finns i avsnittet [skapa en DNS-postuppsättning](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-150">For more information on creating record sets and defining record set parameter explicitly, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="f3a9b-151">Vi inte ger ett exempel för att skapa en SOA-poster, eftersom SOAs skapas och tas bort tillsammans med varje DNS-zon och kan inte skapas eller tas bort separat.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-151">We do not give an example to create an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="f3a9b-152">Dock [SOA kan ändras, som visas i en senare exempel](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-152">However, [the SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record"></a><span data-ttu-id="f3a9b-153">Skapa en AAAA-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-153">Create an AAAA record</span></span>

```azurecli
az network dns record-set aaaa set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-aaaa --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a><span data-ttu-id="f3a9b-154">Skapa en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-154">Create a CNAME record</span></span>

> [!NOTE]
> <span data-ttu-id="f3a9b-155">DNS-standarden inte tillåter CNAME-poster på apex för en zon (`--Name "@"`), eller tillåter att postuppsättningar som innehåller fler än en post.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-155">The DNS standards do not permit CNAME records at the apex of a zone (`--Name "@"`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="f3a9b-156">Mer information finns i [CNAME-poster](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-156">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.contoso.com
```

### <a name="create-an-mx-record"></a><span data-ttu-id="f3a9b-157">Skapa en MX-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-157">Create an MX record</span></span>

<span data-ttu-id="f3a9b-158">I det här exemplet använder vi postuppsättningsnamnet "@" för att skapa MX-posten i basdomänen (i det här fallet "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="f3a9b-158">In this example, we use the record set name "@" to create the MX record at the zone apex (in this case, "contoso.com").</span></span>

```azurecli
az network dns record-set mx set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a><span data-ttu-id="f3a9b-159">Skapa en NS-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-159">Create an NS record</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-ns --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a><span data-ttu-id="f3a9b-160">Skapa en PTR-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-160">Create a PTR record</span></span>

<span data-ttu-id="f3a9b-161">I det här fallet ”min-arpa-zone.com' representerar ARPA zonen som motsvarar IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-161">In this case, 'my-arpa-zone.com' represents the ARPA zone representing your IP range.</span></span> <span data-ttu-id="f3a9b-162">Varje PTR-post som har angetts i den här zonen motsvarar en IP-adress i IP-intervallet.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-162">Each PTR record set in this zone corresponds to an IP address within this IP range.</span></span>  <span data-ttu-id="f3a9b-163">Postnamnet ”10” är den sista oktetten i IP-adress inom den här IP-adressintervall som representeras av den här posten.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-163">The record name '10' is the last octet of the IP address within this IP range represented by this record.</span></span>

```azurecli
az network dns record-set ptr set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name my-arpa.zone.com --ptrdname myservice.contoso.com
```

### <a name="create-an-srv-record"></a><span data-ttu-id="f3a9b-164">Skapa en SRV-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-164">Create an SRV record</span></span>

<span data-ttu-id="f3a9b-165">När du skapar en [SRV-postuppsättning](dns-zones-records.md#srv-records), ange den  *\_service* och  *\_protokollet* i postuppsättningens namn.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-165">When creating an [SRV record set](dns-zones-records.md#srv-records), specify the *\_service* and *\_protocol* in the record set name.</span></span> <span data-ttu-id="f3a9b-166">Det finns inget behov av att inkludera ”@” i namnet på postuppsättningen när du skapar en SRV-post på zonens apex.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-166">There is no need to include "@" in the record set name when creating an SRV record set at the zone apex.</span></span>

```azurecli
az network dns record-set srv set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name _sip._tls --priority 10 --weight 5 --port 8080 --target sip.contoso.com
```

### <a name="create-a-txt-record"></a><span data-ttu-id="f3a9b-167">Skapa en TXT-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-167">Create a TXT record</span></span>

<span data-ttu-id="f3a9b-168">I följande exempel visas hur du skapar en TXT-post.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-168">The following example shows how to create a TXT record.</span></span> <span data-ttu-id="f3a9b-169">Mer information om den maximala stränglängden som stöds i TXT-poster finns [TXT-poster](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-169">For more information about the maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```azurecli
az network dns record-set txt set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-txt --value "This is a TXT record"
```

## <a name="get-a-record-set"></a><span data-ttu-id="f3a9b-170">Hämta en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="f3a9b-170">Get a record set</span></span>

<span data-ttu-id="f3a9b-171">Använd för att hämta en befintlig postuppsättning `az network dns record-set <record-type> show`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-171">To retrieve an existing record set, use `az network dns record-set <record-type> show`.</span></span> <span data-ttu-id="f3a9b-172">Om du vill ha hjälp, så gå till `az network dns record-set <record-type> show --help`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-172">For help, see `az network dns record-set <record-type> show --help`.</span></span>

<span data-ttu-id="f3a9b-173">När du skapar en post eller en postuppsättning postuppsättningsnamnet måste vara en *relativa* namn, vilket innebär att den inte får innehålla zonnamnet.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-173">As when creating a record or record set, the record set name given must be a *relative* name, meaning it must exclude the zone name.</span></span> <span data-ttu-id="f3a9b-174">Du måste också ange posttypen zonen som innehåller uppsättningen av poster och resursgruppen som innehåller zonen.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-174">You also need to specify the record type, the zone containing the record set, and the resource group containing the zone.</span></span>

<span data-ttu-id="f3a9b-175">I följande exempel hämtas posten *www* av typen A från zonen *contoso.com* i resursgruppen *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-175">The following example retrieves the record *www* of type A from zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a show --resource-group myresourcegroup --zone-name contoso.com --name www
```

## <a name="list-record-sets"></a><span data-ttu-id="f3a9b-176">Lista över postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="f3a9b-176">List record sets</span></span>

<span data-ttu-id="f3a9b-177">Du kan visa alla poster i en DNS-zon med hjälp av den `az network dns record-set list` kommando.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-177">You can list all records in a DNS zone by using the `az network dns record-set list` command.</span></span> <span data-ttu-id="f3a9b-178">Om du vill ha hjälp, så gå till `az network dns record-set list --help`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-178">For help, see `az network dns record-set list --help`.</span></span>

<span data-ttu-id="f3a9b-179">Det här exemplet returnerar alla postuppsättningar i zonen *contoso.com*, i resursgrupp *MyResourceGroup*, oavsett namn eller posttyp:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-179">This example returns all record sets in the zone *contoso.com*, in resource group *MyResourceGroup*, regardless of name or record type:</span></span>

```azurecli
az network dns record-set list --resource-group myresourcegroup --zone-name contoso.com
```

<span data-ttu-id="f3a9b-180">Det här exemplet returnerar alla uppsättningar av poster som matchar den givna posttypen (i det här fallet ”A” poster):</span><span class="sxs-lookup"><span data-stu-id="f3a9b-180">This example returns all record sets that match the given record type (in this case, 'A' records):</span></span>

```azurecli
az network dns record-set a list --resource-group myresourcegroup --zone-name contoso.com 
```

## <a name="add-a-record-to-an-existing-record-set"></a><span data-ttu-id="f3a9b-181">Lägga till en post i en befintlig postuppsättning</span><span class="sxs-lookup"><span data-stu-id="f3a9b-181">Add a record to an existing record set</span></span>

<span data-ttu-id="f3a9b-182">Du kan använda `az network dns record-set <record-type> set-record` att skapa en post i en ny postuppsättning eller lägga till en post i en befintlig postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-182">You can use `az network dns record-set <record-type> set-record` both to create a record in a new record set, or to add a record to an existing record set.</span></span>

<span data-ttu-id="f3a9b-183">Mer information finns i [skapa en DNS-post](#create-a-dns-record) och [skapa poster för andra typer](#create-records-of-other-types) ovan.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-183">For more information, see [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="f3a9b-184">Ta bort en post från en befintlig post.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-184">Remove a record from an existing record set.</span></span>

<span data-ttu-id="f3a9b-185">Ta bort en DNS-post från en befintlig post med `az network dns record-set <record-type> remove-record`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-185">To remove a DNS record from an existing record set, use `az network dns record-set <record-type> remove-record`.</span></span> <span data-ttu-id="f3a9b-186">Om du vill ha hjälp, så gå till `az network dns record-set <record-type> remove-record -h`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-186">For help, see `az network dns record-set <record-type> remove-record -h`.</span></span>

<span data-ttu-id="f3a9b-187">Det här kommandot tar bort en DNS-post från en postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-187">This command deletes a DNS record from a record set.</span></span> <span data-ttu-id="f3a9b-188">Om den sista posten i en postuppsättning tas bort raderas även postuppsättningen sig själv.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-188">If the last record in a record set is deleted, the record set itself is also deleted.</span></span> <span data-ttu-id="f3a9b-189">Om du vill behålla tom postuppsättningen i stället använda den `--keep-empty-record-set` alternativet.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-189">To keep the empty record set instead, use the `--keep-empty-record-set` option.</span></span>

<span data-ttu-id="f3a9b-190">Du måste ange posten som ska tas bort och zonen den bör tas bort från, med samma parametrar som när du skapar en post med hjälp av `az network dns record-set <record-type> set-record`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-190">You need to specify the record to be deleted and the zone it should be deleted from, using the same parameters as when creating a record using `az network dns record-set <record-type> set-record`.</span></span> <span data-ttu-id="f3a9b-191">Dessa parametrar beskrivs i [skapa en DNS-post](#create-a-dns-record) och [skapa poster för andra typer](#create-records-of-other-types) ovan.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-191">These parameters are described in [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

<span data-ttu-id="f3a9b-192">I följande exempel tar bort en post med värdet '1.2.3.4 ”från posten uppsättning med namnet *www* i zonen *contoso.com*, i resursgrupp *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-192">The following example deletes the A record with value '1.2.3.4' from the record set named *www* in the zone *contoso.com*, in the resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "www" --ipv4-address 1.2.3.4
```

## <a name="modify-an-existing-record-set"></a><span data-ttu-id="f3a9b-193">Ändra en befintlig postuppsättning</span><span class="sxs-lookup"><span data-stu-id="f3a9b-193">Modify an existing record set</span></span>

<span data-ttu-id="f3a9b-194">Varje post innehåller en [time to live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), och DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-194">Each record set contains a [time-to-live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), and DNS records.</span></span> <span data-ttu-id="f3a9b-195">I följande avsnitt beskrivs hur du ändrar var och en av dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-195">The following sections explain how to modify each of these properties.</span></span>

### <a name="to-modify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a><span data-ttu-id="f3a9b-196">Att ändra en A, AAAA, MX, NS, PTR, SRV och TXT-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-196">To modify an A, AAAA, MX, NS, PTR, SRV, or TXT record</span></span>

<span data-ttu-id="f3a9b-197">Om du vill ändra en befintlig post av typen A, AAAA, MX, NS, PTR, SRV och TXT, bör du först lägga till en ny post och ta sedan bort den befintliga posten.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-197">To modify an existing record of type A, AAAA, MX, NS, PTR, SRV, or TXT, you should first add a new record and then delete the existing record.</span></span> <span data-ttu-id="f3a9b-198">För detaljerade anvisningar om hur du tar bort och lägga till poster i avsnitten tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-198">For detailed instructions on how to delete and add records, see the earlier sections of this article.</span></span>

<span data-ttu-id="f3a9b-199">I följande exempel visas hur du ändrar en ”A” post från IP-adress: 1.2.3.4 till IP-adressen 5.6.7.8:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-199">The following example shows how to modify an 'A' record, from IP address 1.2.3.4 to IP address 5.6.7.8:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 5.6.7.8
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="f3a9b-200">Det går inte att lägga till, ta bort eller ändra poster i den automatiskt skapade NS-postuppsättning på zonens apex (`--Name "@"`, inklusive citattecken).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-200">You cannot add, remove, or modify the records in the automatically created NS record set at the zone apex (`--Name "@"`, including quote marks).</span></span> <span data-ttu-id="f3a9b-201">För den här postuppsättningen har endast ändringar tillåts att ändra posten anger TTL-värde och metadata.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-201">For this record set, the only changes permitted are to modify the record set TTL and metadata.</span></span>

### <a name="to-modify-a-cname-record"></a><span data-ttu-id="f3a9b-202">Så här ändrar du en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-202">To modify a CNAME record</span></span>

<span data-ttu-id="f3a9b-203">Till skillnad från de flesta andra typer av poster, får en CNAME-postuppsättning endast innehålla en enskild post.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-203">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="f3a9b-204">Därför kan du ersätta det aktuella värdet genom att lägga till en ny post och ta bort den befintliga posten, som för andra typer av poster.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-204">Therefore, you cannot replace the current value by adding a new record and removing the existing record, as for other record types.</span></span>

<span data-ttu-id="f3a9b-205">Använd i stället för att ändra en CNAME-post, `az network dns record-set cname set-record`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-205">Instead, to modify a CNAME record, use `az network dns record-set cname set-record`.</span></span> <span data-ttu-id="f3a9b-206">Hjälp finns i`az network dns record-set cname set-record --help`</span><span class="sxs-lookup"><span data-stu-id="f3a9b-206">For help, see `az network dns record-set cname set-record --help`</span></span>

<span data-ttu-id="f3a9b-207">Exemplet ändrar CNAME-postuppsättning *www* i zonen *contoso.com*, i resursgrupp *MyResourceGroup*, peka på 'www.fabrikam.net' i stället för det befintliga värdet:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-207">The example modifies the CNAME record set *www* in the zone *contoso.com*, in resource group *MyResourceGroup*, to point to 'www.fabrikam.net' instead of its existing value:</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a><span data-ttu-id="f3a9b-208">Att ändra en SOA-post</span><span class="sxs-lookup"><span data-stu-id="f3a9b-208">To modify an SOA record</span></span>

<span data-ttu-id="f3a9b-209">Till skillnad från de flesta andra typer av poster, får en CNAME-postuppsättning endast innehålla en enskild post.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-209">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="f3a9b-210">Därför kan du ersätta det aktuella värdet genom att lägga till en ny post och ta bort den befintliga posten, som för andra typer av poster.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-210">Therefore, you cannot replace the current value by adding a new record and removing the existing record, as for other record types.</span></span>

<span data-ttu-id="f3a9b-211">Använd i stället för att ändra SOA-post, `az network dns record-set soa update`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-211">Instead, to modify the SOA record, use `az network dns record-set soa update`.</span></span> <span data-ttu-id="f3a9b-212">Om du vill ha hjälp, så gå till `az network dns record-set soa update --help`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-212">For help, see `az network dns record-set soa update --help`.</span></span>

<span data-ttu-id="f3a9b-213">I följande exempel visas hur du ställer in egenskapen 'e-post för SOA-post för zonen *contoso.com* i resursgruppen *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-213">The following example shows how to set the 'email' property of the SOA record for the zone *contoso.com* in the resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set soa update --resource-group myresourcegroup --zone-name contoso.com --email admin.contoso.com
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a><span data-ttu-id="f3a9b-214">Att ändra NS-poster på zonens apex</span><span class="sxs-lookup"><span data-stu-id="f3a9b-214">To modify NS records at the zone apex</span></span>

<span data-ttu-id="f3a9b-215">NS-postuppsättning på zonens apex skapas automatiskt med varje DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-215">The NS record set at the zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="f3a9b-216">Den innehåller namnen på Azure DNS-namnservrar tilldelas zonen.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-216">It contains the names of the Azure DNS name servers assigned to the zone.</span></span>

<span data-ttu-id="f3a9b-217">Du kan lägga till ytterligare servrar till den här NS-post inställda på en värd domäner med mer än en DNS-leverantör har stöd ett namn.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-217">You can add additional name servers to this NS record set, to support co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="f3a9b-218">Du kan också ändra TTL-värde och metadata för den här postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-218">You can also modify the TTL and metadata for this record set.</span></span> <span data-ttu-id="f3a9b-219">Du kan inte ta bort eller ändra de i förväg Azure DNS-namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-219">However, you cannot remove or modify the pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="f3a9b-220">Observera att detta gäller endast NS-postuppsättning på zonens apex.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-220">Note that this applies only to the NS record set at the zone apex.</span></span> <span data-ttu-id="f3a9b-221">Andra NS postuppsättningar i zonen (som används för att delegera underordnade zoner) kan ändras utan begränsning.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-221">Other NS record sets in your zone (as used to delegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="f3a9b-222">I följande exempel visas hur du lägger till en ytterligare namnserver NS-postuppsättning på zonens apex:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-222">The following example shows how to add an additional name server to the NS record set at the zone apex:</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a><span data-ttu-id="f3a9b-223">Att ändra TTL-värde på en befintlig postuppsättning</span><span class="sxs-lookup"><span data-stu-id="f3a9b-223">To modify the TTL of an existing record set</span></span>

<span data-ttu-id="f3a9b-224">Om du vill ändra TTL-värde på en befintlig postuppsättningen, Använd `azure network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-224">To modify the TTL of an existing record set, use `azure network dns record-set <record-type> update`.</span></span> <span data-ttu-id="f3a9b-225">Om du vill ha hjälp, så gå till `azure network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-225">For help, see `azure network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="f3a9b-226">I följande exempel visas hur du ändrar en postuppsättning TTL-värde, i det här fallet till 60 sekunder:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-226">The following example shows how to modify a record set TTL, in this case to 60 seconds:</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set ttl=60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a><span data-ttu-id="f3a9b-227">Ändra metadata för en befintlig postuppsättning</span><span class="sxs-lookup"><span data-stu-id="f3a9b-227">To modify the metadata of an existing record set</span></span>

<span data-ttu-id="f3a9b-228">[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan användas för att koppla programspecifika data till varje postuppsättningen, som nyckel / värde-par.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-228">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="f3a9b-229">Om du vill ändra metadata för en befintlig postuppsättningen, Använd `az network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-229">To modify the metadata of an existing record set, use `az network dns record-set <record-type> update`.</span></span> <span data-ttu-id="f3a9b-230">Om du vill ha hjälp, så gå till `az network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-230">For help, see `az network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="f3a9b-231">I följande exempel visas hur du ändrar en postuppsättning med två metadataposter ”Avd = ekonomi” och ”miljö = produktion”.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-231">The following example shows how to modify a record set with two metadata entries, "dept=finance" and "environment=production".</span></span> <span data-ttu-id="f3a9b-232">Observera att alla befintliga metadata är *ersättas* med angivna värden.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-232">Note that any existing metadata is *replaced* by the values given.</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set metadata.dept=finance metadata.environment=production
```

## <a name="delete-a-record-set"></a><span data-ttu-id="f3a9b-233">Ta bort en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="f3a9b-233">Delete a record set</span></span>

<span data-ttu-id="f3a9b-234">Postuppsättningar kan tas bort med hjälp av den `az network dns record-set <record-type> delete` kommando.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-234">Record sets can be deleted by using the `az network dns record-set <record-type> delete` command.</span></span> <span data-ttu-id="f3a9b-235">Om du vill ha hjälp, så gå till `azure network dns record-set <record-type> delete --help`.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-235">For help, see `azure network dns record-set <record-type> delete --help`.</span></span> <span data-ttu-id="f3a9b-236">En postuppsättning också tar du bort alla posterna i uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-236">Deleting a record set also deletes all records within the record set.</span></span>

> [!NOTE]
> <span data-ttu-id="f3a9b-237">Du kan inte ta bort SOA och NS-post anger på zonens apex (`--name "@"`).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-237">You cannot delete the SOA and NS record sets at the zone apex (`--name "@"`).</span></span>  <span data-ttu-id="f3a9b-238">Dessa skapas automatiskt när zonen skapades och tas bort automatiskt när zonen tas bort.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-238">These are created automatically when the zone was created, and are deleted automatically when the zone is deleted.</span></span>

<span data-ttu-id="f3a9b-239">I följande exempel tar bort posten namngivna *www* av typen A från zonen *contoso.com* i resursgruppen *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="f3a9b-239">The following example deletes the record set named *www* of type A from the zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a delete --resource-group myresourcegroup --zone-name contoso.com --name www
```

<span data-ttu-id="f3a9b-240">Du uppmanas att bekräfta borttagningen.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-240">You are prompted to confirm the delete operation.</span></span> <span data-ttu-id="f3a9b-241">Om du vill ignorera den här uppmaningen använder den `--yes` växla.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-241">To suppress this prompt, use the `--yes` switch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3a9b-242">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f3a9b-242">Next steps</span></span>

<span data-ttu-id="f3a9b-243">Lär dig mer om [zoner och poster i Azure DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="f3a9b-243">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="f3a9b-244">Lär dig hur du [skydda zoner och poster](dns-protect-zones-recordsets.md) när du använder Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f3a9b-244">Learn how to [protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
