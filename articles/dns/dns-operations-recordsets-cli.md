---
title: aaaManage DNS-poster i Azure DNS med hello Azure CLI 2.0 | Microsoft Docs
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
ms.openlocfilehash: 9d6f8e74ebad55ccd2381fd84a830d2c7bbb1f30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-hello-azure-cli-20"></a><span data-ttu-id="104c7-104">Hantera DNS-poster och postuppsättningar i Azure DNS med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="104c7-104">Manage DNS records and recordsets in Azure DNS using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="104c7-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="104c7-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="104c7-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="104c7-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="104c7-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="104c7-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="104c7-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="104c7-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="104c7-109">Den här artikeln visar hur toomanage DNS-posterna för DNS-zonen med hjälp av hello plattformsoberoende Azure-kommandoradsgränssnittet (CLI) 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="104c7-109">This article shows you how toomanage DNS records for your DNS zone by using hello cross-platform Azure command-line interface (CLI) 2.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="104c7-110">Du kan också hantera DNS-posterna med [Azure PowerShell](dns-operations-recordsets.md) eller hello [Azure-portalen](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="104c7-110">You can also manage your DNS records using [Azure PowerShell](dns-operations-recordsets.md) or hello [Azure portal](dns-operations-recordsets-portal.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="104c7-111">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="104c7-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="104c7-112">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="104c7-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="104c7-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) -vår CLI för hello klassisk och resurs management distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="104c7-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="104c7-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering.</span><span class="sxs-lookup"><span data-stu-id="104c7-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

<span data-ttu-id="104c7-115">hello exemplen i den här artikeln förutsätter att du redan har [installerat hello Azure CLI 2.0 loggat in, och skapa en DNS-zon](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="104c7-115">hello examples in this article assume you have already [installed hello Azure CLI 2.0, signed in, and created a DNS zone](dns-operations-dnszones-cli.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="104c7-116">Introduktion</span><span class="sxs-lookup"><span data-stu-id="104c7-116">Introduction</span></span>

<span data-ttu-id="104c7-117">Innan du skapar DNS-poster i Azure DNS, måste du först toounderstand hur Azure DNS organiserar DNS-poster i DNS-postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="104c7-117">Before creating DNS records in Azure DNS, you first need toounderstand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="104c7-118">Mer information om DNS-poster i Azure DNS finns i [DNS-zoner och poster](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="104c7-118">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="104c7-119">Skapa en DNS-post</span><span class="sxs-lookup"><span data-stu-id="104c7-119">Create a DNS record</span></span>

<span data-ttu-id="104c7-120">toocreate DNS-post och använda hello `az network dns record-set <record-type> set-record` kommando (där `<record-type>` är hello typ av post, dvs</span><span class="sxs-lookup"><span data-stu-id="104c7-120">toocreate a DNS record, use hello `az network dns record-set <record-type> set-record` command (where `<record-type>` is hello type of record, i.e</span></span> <span data-ttu-id="104c7-121">en, srv txt, etc.) Om du vill ha hjälp, så gå till `az network dns record-set --help`.</span><span class="sxs-lookup"><span data-stu-id="104c7-121">a, srv, txt, etc.) For help, see `az network dns record-set --help`.</span></span>

<span data-ttu-id="104c7-122">När du skapar en post du toospecify hello resursgruppens namn, zonnamn, postuppsättning namn, hello posttyp och hello detaljuppgifter om hello håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="104c7-122">When creating a record, you need toospecify hello resource group name, zone name, record set name, hello record type, and hello details of hello record being created.</span></span> <span data-ttu-id="104c7-123">hello postuppsättning namn måste vara en *relativa* namn, vilket innebär att den inte får innehålla hello zonnamnet.</span><span class="sxs-lookup"><span data-stu-id="104c7-123">hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span>

<span data-ttu-id="104c7-124">Om hello postuppsättning inte redan finns, skapas den du i det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="104c7-124">If hello record set does not already exist, this command creates it for you.</span></span> <span data-ttu-id="104c7-125">Om hello-postuppsättning redan finns läggs det här kommandot hello-posten som du anger toohello befintliga postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="104c7-125">If hello record set already exists, this command adds hello record you specify toohello existing record set.</span></span>

<span data-ttu-id="104c7-126">Om en ny postuppsättning skapas, används ett standard TTL-värde (Time to Live) på 3600.</span><span class="sxs-lookup"><span data-stu-id="104c7-126">If a new record set is created, a default time-to-live (TTL) of 3600 is used.</span></span> <span data-ttu-id="104c7-127">Anvisningar för hur toouse olika TTLs Se [skapa en DNS-postuppsättning](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="104c7-127">For instructions on how toouse different TTLs, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="104c7-128">hello följande exempel skapas en A-post som kallas *www* i hello zon *contoso.com* i hello resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="104c7-128">hello following example creates an A record called *www* in hello zone *contoso.com* in hello resource group *MyResourceGroup*.</span></span> <span data-ttu-id="104c7-129">Hej IP-adressen för en post är hello *1.2.3.4*.</span><span class="sxs-lookup"><span data-stu-id="104c7-129">hello IP address of hello A record is *1.2.3.4*.</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="104c7-130">toocreate en post som i hello apex av hello zon (i det här fallet ”contoso.com”), Använd hello postnamnet ”@”, inklusive hello citattecken:</span><span class="sxs-lookup"><span data-stu-id="104c7-130">toocreate a record set in hello apex of hello zone (in this case, "contoso.com"), use hello record name "@", including hello quotation marks:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

## <a name="create-a-dns-record-set"></a><span data-ttu-id="104c7-131">Skapa en uppsättning av DNS-poster</span><span class="sxs-lookup"><span data-stu-id="104c7-131">Create a DNS record set</span></span>

<span data-ttu-id="104c7-132">I hello exemplen ovan hello DNS-posten var antingen tillagda tooan befintliga uppsättningen av poster eller hello postuppsättning skapades *implicit*.</span><span class="sxs-lookup"><span data-stu-id="104c7-132">In hello above examples, hello DNS record was either added tooan existing record set, or hello record set was created *implicitly*.</span></span> <span data-ttu-id="104c7-133">Du kan också skapa hello postuppsättning *explicit* innan du lägger till registrerar tooit.</span><span class="sxs-lookup"><span data-stu-id="104c7-133">You can also create hello record set *explicitly* before adding records tooit.</span></span> <span data-ttu-id="104c7-134">Azure DNS stöder 'empty-postuppsättningar som kan fungera som en platshållare tooreserve ett DNS-namn innan du skapar DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="104c7-134">Azure DNS supports 'empty' record sets, which can act as a placeholder tooreserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="104c7-135">Tom postuppsättningar är synliga i hello Azure DNS kontrollen plan, men visas inte i hello Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="104c7-135">Empty record sets are visible in hello Azure DNS control plane, but do not appear on hello Azure DNS name servers.</span></span>

<span data-ttu-id="104c7-136">Postuppsättningar skapas med hello `az network dns record-set <record-type> create` kommando.</span><span class="sxs-lookup"><span data-stu-id="104c7-136">Record sets are created using hello `az network dns record-set <record-type> create` command.</span></span> <span data-ttu-id="104c7-137">Om du vill ha hjälp, så gå till `az network dns record-set <record-type> create --help`.</span><span class="sxs-lookup"><span data-stu-id="104c7-137">For help, see `az network dns record-set <record-type> create --help`.</span></span>

<span data-ttu-id="104c7-138">Skapa hello-postuppsättning explicit kan du toospecify postuppsättning egenskaper, till exempel hello [Time To Live (TTL)](dns-zones-records.md#time-to-live) och metadata.</span><span class="sxs-lookup"><span data-stu-id="104c7-138">Creating hello record set explicitly allows you toospecify record set properties such as hello [Time-To-Live (TTL)](dns-zones-records.md#time-to-live) and metadata.</span></span> <span data-ttu-id="104c7-139">[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan vara används tooassociate programspecifika data med varje postuppsättningen, som nyckel / värde-par.</span><span class="sxs-lookup"><span data-stu-id="104c7-139">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="104c7-140">hello följande exempel skapar en tom postuppsättning av typen ”A” med en 60-sekunders TTL genom att använda hello `--ttl` parameter (kort form `-l`):</span><span class="sxs-lookup"><span data-stu-id="104c7-140">hello following example creates an empty record set of type 'A' with a 60-second TTL, by using hello `--ttl` parameter (short form `-l`):</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --ttl 60
```

<span data-ttu-id="104c7-141">hello följande exempel skapas en postuppsättning med två metadataposter ”Avd = ekonomi” och ”miljö = produktion”, med hjälp av hello `--metadata` parameter:</span><span class="sxs-lookup"><span data-stu-id="104c7-141">hello following example creates a record set with two metadata entries, "dept=finance" and "environment=production", by using hello `--metadata` parameter :</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --metadata "dept=finance" "environment=production"
```

<span data-ttu-id="104c7-142">Att ha skapat en tom postuppsättningen, poster kan läggas till med `azure network dns record-set <record-type> set-record` enligt beskrivningen i [skapa en DNS-post](#create-a-dns-record).</span><span class="sxs-lookup"><span data-stu-id="104c7-142">Having created an empty record set, records can be added using `azure network dns record-set <record-type> set-record` as described in [Create a DNS record](#create-a-dns-record).</span></span>

## <a name="create-records-of-other-types"></a><span data-ttu-id="104c7-143">Skapa poster för andra typer</span><span class="sxs-lookup"><span data-stu-id="104c7-143">Create records of other types</span></span>

<span data-ttu-id="104c7-144">Att ha sett i detalj hur toocreate ”A” poster, hello följande exempel visas hur toocreate post för andra typer som stöds av Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="104c7-144">Having seen in detail how toocreate 'A' records, hello following examples show how toocreate record of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="104c7-145">hello parametrar som används toospecify hello data varierar beroende på hello typ av hello-post.</span><span class="sxs-lookup"><span data-stu-id="104c7-145">hello parameters used toospecify hello record data vary depending on hello type of hello record.</span></span> <span data-ttu-id="104c7-146">Till exempel för en post av typen ”A”, du anger hello IPv4-adress med hello parametern `--ipv4-address <IPv4 address>`.</span><span class="sxs-lookup"><span data-stu-id="104c7-146">For example, for a record of type "A", you specify hello IPv4 address with hello parameter `--ipv4-address <IPv4 address>`.</span></span> <span data-ttu-id="104c7-147">Hej parametrar för varje posttyp kan visas med hjälp av `az network dns record-set <record-type> set-record --help`.</span><span class="sxs-lookup"><span data-stu-id="104c7-147">hello parameters for each record type can be listed using `az network dns record-set <record-type> set-record --help`.</span></span>

<span data-ttu-id="104c7-148">I varje fall visar vi hur toocreate en enskild post.</span><span class="sxs-lookup"><span data-stu-id="104c7-148">In each case, we show how toocreate a single record.</span></span> <span data-ttu-id="104c7-149">hello-posten är tillagda toohello befintliga uppsättningen av poster eller en uppsättning poster har skapats implicit.</span><span class="sxs-lookup"><span data-stu-id="104c7-149">hello record is added toohello existing record set, or a record set created implicitly.</span></span> <span data-ttu-id="104c7-150">Mer information om hur du skapar postuppsättningar och definierar post parametern uttryckligen finns i avsnittet [skapa en DNS-postuppsättning](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="104c7-150">For more information on creating record sets and defining record set parameter explicitly, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="104c7-151">Vi inte ger ett exempel toocreate ett SOA-postuppsättning eftersom SOAs skapas och tas bort tillsammans med varje DNS-zon och kan inte skapas eller tas bort separat.</span><span class="sxs-lookup"><span data-stu-id="104c7-151">We do not give an example toocreate an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="104c7-152">Dock [hello SOA kan ändras, som visas i en senare exempel](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="104c7-152">However, [hello SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record"></a><span data-ttu-id="104c7-153">Skapa en AAAA-post</span><span class="sxs-lookup"><span data-stu-id="104c7-153">Create an AAAA record</span></span>

```azurecli
az network dns record-set aaaa set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-aaaa --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a><span data-ttu-id="104c7-154">Skapa en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="104c7-154">Create a CNAME record</span></span>

> [!NOTE]
> <span data-ttu-id="104c7-155">hello DNS-standarden inte tillåter CNAME-poster på hello apex för en zon (`--Name "@"`), eller tillåter att postuppsättningar som innehåller fler än en post.</span><span class="sxs-lookup"><span data-stu-id="104c7-155">hello DNS standards do not permit CNAME records at hello apex of a zone (`--Name "@"`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="104c7-156">Mer information finns i [CNAME-poster](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="104c7-156">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.contoso.com
```

### <a name="create-an-mx-record"></a><span data-ttu-id="104c7-157">Skapa en MX-post</span><span class="sxs-lookup"><span data-stu-id="104c7-157">Create an MX record</span></span>

<span data-ttu-id="104c7-158">I det här exemplet använder vi hello postuppsättningsnamnet ”@” toocreate hello MX-post på hello zonens apex (i det här fallet ”contoso.com”).</span><span class="sxs-lookup"><span data-stu-id="104c7-158">In this example, we use hello record set name "@" toocreate hello MX record at hello zone apex (in this case, "contoso.com").</span></span>

```azurecli
az network dns record-set mx set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a><span data-ttu-id="104c7-159">Skapa en NS-post</span><span class="sxs-lookup"><span data-stu-id="104c7-159">Create an NS record</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-ns --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a><span data-ttu-id="104c7-160">Skapa en PTR-post</span><span class="sxs-lookup"><span data-stu-id="104c7-160">Create a PTR record</span></span>

<span data-ttu-id="104c7-161">I det här fallet ”min-arpa-zone.com' representerar hello ARPA zonen som motsvarar IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="104c7-161">In this case, 'my-arpa-zone.com' represents hello ARPA zone representing your IP range.</span></span> <span data-ttu-id="104c7-162">Varje PTR-post i den här zonen motsvarar tooan IP-adress inom IP-intervallet.</span><span class="sxs-lookup"><span data-stu-id="104c7-162">Each PTR record set in this zone corresponds tooan IP address within this IP range.</span></span>  <span data-ttu-id="104c7-163">hello-postnamnet ”10” är hello sista oktetten hello IP-adress inom den här IP-adressintervall som representeras av den här posten.</span><span class="sxs-lookup"><span data-stu-id="104c7-163">hello record name '10' is hello last octet of hello IP address within this IP range represented by this record.</span></span>

```azurecli
az network dns record-set ptr set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name my-arpa.zone.com --ptrdname myservice.contoso.com
```

### <a name="create-an-srv-record"></a><span data-ttu-id="104c7-164">Skapa en SRV-post</span><span class="sxs-lookup"><span data-stu-id="104c7-164">Create an SRV record</span></span>

<span data-ttu-id="104c7-165">När du skapar en [SRV-postuppsättning](dns-zones-records.md#srv-records), ange hello  *\_service* och  *\_protokollet* i hello postuppsättning namn.</span><span class="sxs-lookup"><span data-stu-id="104c7-165">When creating an [SRV record set](dns-zones-records.md#srv-records), specify hello *\_service* and *\_protocol* in hello record set name.</span></span> <span data-ttu-id="104c7-166">Det finns inget behov av tooinclude ”@” i hello postuppsättning namn när du skapar en SRV-post på hello zonens apex.</span><span class="sxs-lookup"><span data-stu-id="104c7-166">There is no need tooinclude "@" in hello record set name when creating an SRV record set at hello zone apex.</span></span>

```azurecli
az network dns record-set srv set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name _sip._tls --priority 10 --weight 5 --port 8080 --target sip.contoso.com
```

### <a name="create-a-txt-record"></a><span data-ttu-id="104c7-167">Skapa en TXT-post</span><span class="sxs-lookup"><span data-stu-id="104c7-167">Create a TXT record</span></span>

<span data-ttu-id="104c7-168">hello följande exempel visas hur toocreate en TXT-posten.</span><span class="sxs-lookup"><span data-stu-id="104c7-168">hello following example shows how toocreate a TXT record.</span></span> <span data-ttu-id="104c7-169">Mer information om hello stränglängden som stöds i TXT-poster finns [TXT-poster](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="104c7-169">For more information about hello maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```azurecli
az network dns record-set txt set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-txt --value "This is a TXT record"
```

## <a name="get-a-record-set"></a><span data-ttu-id="104c7-170">Hämta en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="104c7-170">Get a record set</span></span>

<span data-ttu-id="104c7-171">tooretrieve befintliga postuppsättningen, Använd `az network dns record-set <record-type> show`.</span><span class="sxs-lookup"><span data-stu-id="104c7-171">tooretrieve an existing record set, use `az network dns record-set <record-type> show`.</span></span> <span data-ttu-id="104c7-172">Om du vill ha hjälp, så gå till `az network dns record-set <record-type> show --help`.</span><span class="sxs-lookup"><span data-stu-id="104c7-172">For help, see `az network dns record-set <record-type> show --help`.</span></span>

<span data-ttu-id="104c7-173">Som när du skapar en post eller en postuppsättning hello post anger namn måste vara en *relativa* namn, vilket innebär att den inte får innehålla hello zonnamnet.</span><span class="sxs-lookup"><span data-stu-id="104c7-173">As when creating a record or record set, hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span> <span data-ttu-id="104c7-174">Du måste också toospecify hello-posttypen, hello zon som innehåller hello post ange och hello resursgruppen som innehåller hello zonen.</span><span class="sxs-lookup"><span data-stu-id="104c7-174">You also need toospecify hello record type, hello zone containing hello record set, and hello resource group containing hello zone.</span></span>

<span data-ttu-id="104c7-175">hello följande exempel hämtas hello post *www* av typen A från zonen *contoso.com* i resursgruppen *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="104c7-175">hello following example retrieves hello record *www* of type A from zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a show --resource-group myresourcegroup --zone-name contoso.com --name www
```

## <a name="list-record-sets"></a><span data-ttu-id="104c7-176">Lista över postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="104c7-176">List record sets</span></span>

<span data-ttu-id="104c7-177">Du kan visa alla poster i en DNS-zon med hjälp av hello `az network dns record-set list` kommando.</span><span class="sxs-lookup"><span data-stu-id="104c7-177">You can list all records in a DNS zone by using hello `az network dns record-set list` command.</span></span> <span data-ttu-id="104c7-178">Om du vill ha hjälp, så gå till `az network dns record-set list --help`.</span><span class="sxs-lookup"><span data-stu-id="104c7-178">For help, see `az network dns record-set list --help`.</span></span>

<span data-ttu-id="104c7-179">Det här exemplet returnerar alla postuppsättningar i zonen hello *contoso.com*, i resursgrupp *MyResourceGroup*, oavsett namn eller posttyp:</span><span class="sxs-lookup"><span data-stu-id="104c7-179">This example returns all record sets in hello zone *contoso.com*, in resource group *MyResourceGroup*, regardless of name or record type:</span></span>

```azurecli
az network dns record-set list --resource-group myresourcegroup --zone-name contoso.com
```

<span data-ttu-id="104c7-180">Det här exemplet returnerar alla uppsättningar av poster som matchar hello angivna posttypen (i det här fallet ”A” poster):</span><span class="sxs-lookup"><span data-stu-id="104c7-180">This example returns all record sets that match hello given record type (in this case, 'A' records):</span></span>

```azurecli
az network dns record-set a list --resource-group myresourcegroup --zone-name contoso.com 
```

## <a name="add-a-record-tooan-existing-record-set"></a><span data-ttu-id="104c7-181">Lägg till en post tooan befintliga uppsättningen av poster</span><span class="sxs-lookup"><span data-stu-id="104c7-181">Add a record tooan existing record set</span></span>

<span data-ttu-id="104c7-182">Du kan använda `az network dns record-set <record-type> set-record` både toocreate en post i en ny post, eller tooadd en befintlig post tooan-post.</span><span class="sxs-lookup"><span data-stu-id="104c7-182">You can use `az network dns record-set <record-type> set-record` both toocreate a record in a new record set, or tooadd a record tooan existing record set.</span></span>

<span data-ttu-id="104c7-183">Mer information finns i [skapa en DNS-post](#create-a-dns-record) och [skapa poster för andra typer](#create-records-of-other-types) ovan.</span><span class="sxs-lookup"><span data-stu-id="104c7-183">For more information, see [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="104c7-184">Ta bort en post från en befintlig post.</span><span class="sxs-lookup"><span data-stu-id="104c7-184">Remove a record from an existing record set.</span></span>

<span data-ttu-id="104c7-185">tooremove ett DNS-posten från en befintlig postuppsättningen, Använd `az network dns record-set <record-type> remove-record`.</span><span class="sxs-lookup"><span data-stu-id="104c7-185">tooremove a DNS record from an existing record set, use `az network dns record-set <record-type> remove-record`.</span></span> <span data-ttu-id="104c7-186">Om du vill ha hjälp, så gå till `az network dns record-set <record-type> remove-record -h`.</span><span class="sxs-lookup"><span data-stu-id="104c7-186">For help, see `az network dns record-set <record-type> remove-record -h`.</span></span>

<span data-ttu-id="104c7-187">Det här kommandot tar bort en DNS-post från en postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="104c7-187">This command deletes a DNS record from a record set.</span></span> <span data-ttu-id="104c7-188">Om hello sista posten i en postuppsättning tas bort raderas även hello-postuppsättning sig själv.</span><span class="sxs-lookup"><span data-stu-id="104c7-188">If hello last record in a record set is deleted, hello record set itself is also deleted.</span></span> <span data-ttu-id="104c7-189">tookeep hello tom uppsättningen av poster i stället använda hello `--keep-empty-record-set` alternativet.</span><span class="sxs-lookup"><span data-stu-id="104c7-189">tookeep hello empty record set instead, use hello `--keep-empty-record-set` option.</span></span>

<span data-ttu-id="104c7-190">Du behöver toospecify hello poster toobe bort och hello zon den ska tas bort från, med hjälp av hello samma parametrar som när du skapar en post med hjälp av `az network dns record-set <record-type> set-record`.</span><span class="sxs-lookup"><span data-stu-id="104c7-190">You need toospecify hello record toobe deleted and hello zone it should be deleted from, using hello same parameters as when creating a record using `az network dns record-set <record-type> set-record`.</span></span> <span data-ttu-id="104c7-191">Dessa parametrar beskrivs i [skapa en DNS-post](#create-a-dns-record) och [skapa poster för andra typer](#create-records-of-other-types) ovan.</span><span class="sxs-lookup"><span data-stu-id="104c7-191">These parameters are described in [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

<span data-ttu-id="104c7-192">följande exempel tar bort hello hello post med värdet '1.2.3.4' från hello post uppsättning med namnet *www* i hello zon *contoso.com*, i resursgrupp hello *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="104c7-192">hello following example deletes hello A record with value '1.2.3.4' from hello record set named *www* in hello zone *contoso.com*, in hello resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "www" --ipv4-address 1.2.3.4
```

## <a name="modify-an-existing-record-set"></a><span data-ttu-id="104c7-193">Ändra en befintlig postuppsättning</span><span class="sxs-lookup"><span data-stu-id="104c7-193">Modify an existing record set</span></span>

<span data-ttu-id="104c7-194">Varje post innehåller en [time to live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), och DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="104c7-194">Each record set contains a [time-to-live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), and DNS records.</span></span> <span data-ttu-id="104c7-195">hello följande avsnitt beskrivs hur toomodify av de här egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="104c7-195">hello following sections explain how toomodify each of these properties.</span></span>

### <a name="toomodify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a><span data-ttu-id="104c7-196">toomodify A, AAAA, MX, NS, PTR, SRV och TXT-post</span><span class="sxs-lookup"><span data-stu-id="104c7-196">toomodify an A, AAAA, MX, NS, PTR, SRV, or TXT record</span></span>

<span data-ttu-id="104c7-197">toomodify en befintlig post av typen A, AAAA, MX, NS, PTR, SRV och TXT, du måste först lägga till en ny post och delete hello befintlig post.</span><span class="sxs-lookup"><span data-stu-id="104c7-197">toomodify an existing record of type A, AAAA, MX, NS, PTR, SRV, or TXT, you should first add a new record and then delete hello existing record.</span></span> <span data-ttu-id="104c7-198">Detaljerade anvisningar om hur toodelete och lägga till poster, se hello tidigare avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="104c7-198">For detailed instructions on how toodelete and add records, see hello earlier sections of this article.</span></span>

<span data-ttu-id="104c7-199">hello som följande exempel visar hur toomodify en ”A”-post från IP-adress: 1.2.3.4 tooIP adressen 5.6.7.8:</span><span class="sxs-lookup"><span data-stu-id="104c7-199">hello following example shows how toomodify an 'A' record, from IP address 1.2.3.4 tooIP address 5.6.7.8:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 5.6.7.8
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="104c7-200">Det går inte att lägga till, ta bort eller ändra hello poster i hello skapas automatiskt NS-postuppsättning på zonens apex hello (`--Name "@"`, inklusive citattecken).</span><span class="sxs-lookup"><span data-stu-id="104c7-200">You cannot add, remove, or modify hello records in hello automatically created NS record set at hello zone apex (`--Name "@"`, including quote marks).</span></span> <span data-ttu-id="104c7-201">För den här postuppsättningen är hello endast ändringar tillåts toomodify hello postuppsättning TTL-värde och metadata.</span><span class="sxs-lookup"><span data-stu-id="104c7-201">For this record set, hello only changes permitted are toomodify hello record set TTL and metadata.</span></span>

### <a name="toomodify-a-cname-record"></a><span data-ttu-id="104c7-202">toomodify en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="104c7-202">toomodify a CNAME record</span></span>

<span data-ttu-id="104c7-203">Till skillnad från de flesta andra typer av poster, får en CNAME-postuppsättning endast innehålla en enskild post.</span><span class="sxs-lookup"><span data-stu-id="104c7-203">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="104c7-204">Därför kan du ersätta hello aktuellt värde genom att lägga till en ny post och ta bort hello befintlig post som för andra typer av poster.</span><span class="sxs-lookup"><span data-stu-id="104c7-204">Therefore, you cannot replace hello current value by adding a new record and removing hello existing record, as for other record types.</span></span>

<span data-ttu-id="104c7-205">Toomodify en CNAME-post, Använd i stället `az network dns record-set cname set-record`.</span><span class="sxs-lookup"><span data-stu-id="104c7-205">Instead, toomodify a CNAME record, use `az network dns record-set cname set-record`.</span></span> <span data-ttu-id="104c7-206">Hjälp finns i`az network dns record-set cname set-record --help`</span><span class="sxs-lookup"><span data-stu-id="104c7-206">For help, see `az network dns record-set cname set-record --help`</span></span>

<span data-ttu-id="104c7-207">hello exempel ändrar hello CNAME-postuppsättning *www* i hello zon *contoso.com*, i resursgrupp *MyResourceGroup*, toopoint för 'www.fabrikam.net' i stället för dess befintliga värdet:</span><span class="sxs-lookup"><span data-stu-id="104c7-207">hello example modifies hello CNAME record set *www* in hello zone *contoso.com*, in resource group *MyResourceGroup*, toopoint too'www.fabrikam.net' instead of its existing value:</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.fabrikam.net
``` 

### <a name="toomodify-an-soa-record"></a><span data-ttu-id="104c7-208">toomodify en SOA-post</span><span class="sxs-lookup"><span data-stu-id="104c7-208">toomodify an SOA record</span></span>

<span data-ttu-id="104c7-209">Till skillnad från de flesta andra typer av poster, får en CNAME-postuppsättning endast innehålla en enskild post.</span><span class="sxs-lookup"><span data-stu-id="104c7-209">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="104c7-210">Därför kan du ersätta hello aktuellt värde genom att lägga till en ny post och ta bort hello befintlig post som för andra typer av poster.</span><span class="sxs-lookup"><span data-stu-id="104c7-210">Therefore, you cannot replace hello current value by adding a new record and removing hello existing record, as for other record types.</span></span>

<span data-ttu-id="104c7-211">Toomodify hello SOA-post, Använd i stället `az network dns record-set soa update`.</span><span class="sxs-lookup"><span data-stu-id="104c7-211">Instead, toomodify hello SOA record, use `az network dns record-set soa update`.</span></span> <span data-ttu-id="104c7-212">Om du vill ha hjälp, så gå till `az network dns record-set soa update --help`.</span><span class="sxs-lookup"><span data-stu-id="104c7-212">For help, see `az network dns record-set soa update --help`.</span></span>

<span data-ttu-id="104c7-213">hello följande exempel visas hur tooset hello e-post-egenskapen för hello SOA-posten för hello zonen *contoso.com* i hello resursgruppen *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="104c7-213">hello following example shows how tooset hello 'email' property of hello SOA record for hello zone *contoso.com* in hello resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set soa update --resource-group myresourcegroup --zone-name contoso.com --email admin.contoso.com
```

### <a name="toomodify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="104c7-214">toomodify NS-poster på hello zonens apex</span><span class="sxs-lookup"><span data-stu-id="104c7-214">toomodify NS records at hello zone apex</span></span>

<span data-ttu-id="104c7-215">hello NS-postuppsättning på zonens apex hello skapas automatiskt med varje DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="104c7-215">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="104c7-216">Den innehåller hello namnen på hello Azure DNS-namnet servrar tilldelade toohello zon.</span><span class="sxs-lookup"><span data-stu-id="104c7-216">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="104c7-217">Du kan lägga till ytterligare namn servrar toothis NS postuppsättningen, toosupport samordna värd domäner med mer än en DNS-leverantör.</span><span class="sxs-lookup"><span data-stu-id="104c7-217">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="104c7-218">Du kan också ändra hello TTL-värde och metadata för den här postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="104c7-218">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="104c7-219">Du kan inte ta bort eller ändra hello förifyllda Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="104c7-219">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="104c7-220">Observera att detta gäller endast toohello NS postuppsättning på zonens apex hello.</span><span class="sxs-lookup"><span data-stu-id="104c7-220">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="104c7-221">Andra NS-postuppsättningar i zonen (som används toodelegate underordnade zoner) kan ändras utan begränsning.</span><span class="sxs-lookup"><span data-stu-id="104c7-221">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="104c7-222">hello som följande exempel visar hur tooadd en ytterligare toohello NS namnserverposten ange på hello zonens apex:</span><span class="sxs-lookup"><span data-stu-id="104c7-222">hello following example shows how tooadd an additional name server toohello NS record set at hello zone apex:</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="toomodify-hello-ttl-of-an-existing-record-set"></a><span data-ttu-id="104c7-223">Ange toomodify hello TTL-värde på en befintlig post</span><span class="sxs-lookup"><span data-stu-id="104c7-223">toomodify hello TTL of an existing record set</span></span>

<span data-ttu-id="104c7-224">Ange toomodify hello TTL-värde på en befintlig post använder `azure network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="104c7-224">toomodify hello TTL of an existing record set, use `azure network dns record-set <record-type> update`.</span></span> <span data-ttu-id="104c7-225">Om du vill ha hjälp, så gå till `azure network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="104c7-225">For help, see `azure network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="104c7-226">hello som följande exempel visar hur toomodify en postuppsättning TTL-värde, i detta fall too60 sekunder:</span><span class="sxs-lookup"><span data-stu-id="104c7-226">hello following example shows how toomodify a record set TTL, in this case too60 seconds:</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set ttl=60
```

### <a name="toomodify-hello-metadata-of-an-existing-record-set"></a><span data-ttu-id="104c7-227">Ange toomodify hello metadata för en befintlig post</span><span class="sxs-lookup"><span data-stu-id="104c7-227">toomodify hello metadata of an existing record set</span></span>

<span data-ttu-id="104c7-228">[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan vara används tooassociate programspecifika data med varje postuppsättningen, som nyckel / värde-par.</span><span class="sxs-lookup"><span data-stu-id="104c7-228">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="104c7-229">toomodify hello metadata för en befintlig post angetts använder `az network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="104c7-229">toomodify hello metadata of an existing record set, use `az network dns record-set <record-type> update`.</span></span> <span data-ttu-id="104c7-230">Om du vill ha hjälp, så gå till `az network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="104c7-230">For help, see `az network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="104c7-231">hello följande exempel visas hur toomodify en postuppsättning med två metadataposter ”Avd = ekonomi” och ”miljö = produktion”.</span><span class="sxs-lookup"><span data-stu-id="104c7-231">hello following example shows how toomodify a record set with two metadata entries, "dept=finance" and "environment=production".</span></span> <span data-ttu-id="104c7-232">Observera att alla befintliga metadata är *ersättas* av hello värdena.</span><span class="sxs-lookup"><span data-stu-id="104c7-232">Note that any existing metadata is *replaced* by hello values given.</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set metadata.dept=finance metadata.environment=production
```

## <a name="delete-a-record-set"></a><span data-ttu-id="104c7-233">Ta bort en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="104c7-233">Delete a record set</span></span>

<span data-ttu-id="104c7-234">Postuppsättningar kan tas bort med hjälp av hello `az network dns record-set <record-type> delete` kommando.</span><span class="sxs-lookup"><span data-stu-id="104c7-234">Record sets can be deleted by using hello `az network dns record-set <record-type> delete` command.</span></span> <span data-ttu-id="104c7-235">Om du vill ha hjälp, så gå till `azure network dns record-set <record-type> delete --help`.</span><span class="sxs-lookup"><span data-stu-id="104c7-235">For help, see `azure network dns record-set <record-type> delete --help`.</span></span> <span data-ttu-id="104c7-236">En postuppsättning också tar bort alla poster inom hello uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="104c7-236">Deleting a record set also deletes all records within hello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="104c7-237">Du kan inte ta bort hello SOA- och NS postuppsättningar vid basdomänen hello (`--name "@"`).</span><span class="sxs-lookup"><span data-stu-id="104c7-237">You cannot delete hello SOA and NS record sets at hello zone apex (`--name "@"`).</span></span>  <span data-ttu-id="104c7-238">Dessa skapas automatiskt när hello zonen skapades och tas bort automatiskt när hello zon tas bort.</span><span class="sxs-lookup"><span data-stu-id="104c7-238">These are created automatically when hello zone was created, and are deleted automatically when hello zone is deleted.</span></span>

<span data-ttu-id="104c7-239">hello följande exempel tar bort hello-postuppsättning med namnet *www* av typen A från hello zon *contoso.com* i resursgruppen *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="104c7-239">hello following example deletes hello record set named *www* of type A from hello zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a delete --resource-group myresourcegroup --zone-name contoso.com --name www
```

<span data-ttu-id="104c7-240">Du kan ange tooconfirm hello borttagningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="104c7-240">You are prompted tooconfirm hello delete operation.</span></span> <span data-ttu-id="104c7-241">toosuppress detta kommandotolk, använda hello `--yes` växla.</span><span class="sxs-lookup"><span data-stu-id="104c7-241">toosuppress this prompt, use hello `--yes` switch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="104c7-242">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="104c7-242">Next steps</span></span>

<span data-ttu-id="104c7-243">Lär dig mer om [zoner och poster i Azure DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="104c7-243">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="104c7-244">Lär dig hur för[skydda zoner och poster](dns-protect-zones-recordsets.md) när du använder Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="104c7-244">Learn how too[protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
