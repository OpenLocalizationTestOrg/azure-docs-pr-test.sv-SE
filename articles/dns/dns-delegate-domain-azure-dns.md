---
title: "aaaDelegate din domän tooAzure DNS | Microsoft Docs"
description: "Förstå hur toochange domän delegering och Använd Azure DNS namn servrar tooprovide domän värdar."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.openlocfilehash: f780bdaa416150e5e3afe6c6845dc75ba54b6203
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-a-domain-tooazure-dns"></a><span data-ttu-id="5c982-103">Delegera en domän tooAzure DNS</span><span class="sxs-lookup"><span data-stu-id="5c982-103">Delegate a domain tooAzure DNS</span></span>

<span data-ttu-id="5c982-104">Azure DNS kan du toohost en DNS-zon och hantera hello DNS-poster för en domän i Azure.</span><span class="sxs-lookup"><span data-stu-id="5c982-104">Azure DNS allows you toohost a DNS zone and manage hello DNS records for a domain in Azure.</span></span> <span data-ttu-id="5c982-105">För DNS-frågor för en domän tooreach Azure DNS hello domän har toobe delegerad tooAzure DNS från hello överordnad domän.</span><span class="sxs-lookup"><span data-stu-id="5c982-105">In order for DNS queries for a domain tooreach Azure DNS, hello domain has toobe delegated tooAzure DNS from hello parent domain.</span></span> <span data-ttu-id="5c982-106">Kom ihåg Azure DNS är inte hello domänregistrator.</span><span class="sxs-lookup"><span data-stu-id="5c982-106">Keep in mind Azure DNS is not hello domain registrar.</span></span> <span data-ttu-id="5c982-107">Den här artikeln förklarar hur toodelegate din domän tooAzure DNS.</span><span class="sxs-lookup"><span data-stu-id="5c982-107">This article explains how toodelegate your domain tooAzure DNS.</span></span>

<span data-ttu-id="5c982-108">För domäner som har köpt från ett register, erbjuder din registrator hello alternativet tooset upp dessa NS-poster.</span><span class="sxs-lookup"><span data-stu-id="5c982-108">For domains purchased from a registrar, your registrar offers hello option tooset up these NS records.</span></span> <span data-ttu-id="5c982-109">Du har inte tooown domän-toocreate en DNS-zon med det domännamnet i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5c982-109">You do not have tooown a domain toocreate a DNS zone with that domain name in Azure DNS.</span></span> <span data-ttu-id="5c982-110">Du behöver dock tooown hello domän tooset in hello delegering tooAzure DNS med hello register.</span><span class="sxs-lookup"><span data-stu-id="5c982-110">However, you do need tooown hello domain tooset up hello delegation tooAzure DNS with hello registrar.</span></span>

<span data-ttu-id="5c982-111">Anta att du köper hello domän 'contoso.net' och skapa en zon med hello name 'contoso.net' i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5c982-111">For example, suppose you purchase hello domain 'contoso.net' and create a zone with hello name 'contoso.net' in Azure DNS.</span></span> <span data-ttu-id="5c982-112">Hello ägare av hello domän erbjuder din registrator du hello alternativet tooconfigure hello adresser (det vill säga hello NS-poster) för din domän.</span><span class="sxs-lookup"><span data-stu-id="5c982-112">As hello owner of hello domain, your registrar offers you hello option tooconfigure hello name server addresses (that is, hello NS records) for your domain.</span></span> <span data-ttu-id="5c982-113">hello registrator lagrar dessa NS-poster i hello överordnade domänen i det här fallet '.net'.</span><span class="sxs-lookup"><span data-stu-id="5c982-113">hello registrar stores these NS records in hello parent domain, in this case '.net'.</span></span> <span data-ttu-id="5c982-114">Klienter runt hello world kan sedan vara riktad tooyour domän i Azure DNS-zonen när tooresolve DNS-poster i 'contoso.net'.</span><span class="sxs-lookup"><span data-stu-id="5c982-114">Clients around hello world can then be directed tooyour domain in Azure DNS zone when trying tooresolve DNS records in 'contoso.net'.</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="5c982-115">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="5c982-115">Create a DNS zone</span></span>

1. <span data-ttu-id="5c982-116">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5c982-116">Sign in toohello Azure portal</span></span>
1. <span data-ttu-id="5c982-117">Hej hubbmenyn, klicka på och klicka på **New > nätverk >** och klicka sedan på **DNS-zonen** tooopen hello skapa DNS-zonen bladet.</span><span class="sxs-lookup"><span data-stu-id="5c982-117">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![DNS-zon](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="5c982-119">På hello **skapa DNS-zonen** bladet ange hello följande värden, och klicka sedan på **skapa**:</span><span class="sxs-lookup"><span data-stu-id="5c982-119">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>

   | <span data-ttu-id="5c982-120">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="5c982-120">**Setting**</span></span> | <span data-ttu-id="5c982-121">**Värde**</span><span class="sxs-lookup"><span data-stu-id="5c982-121">**Value**</span></span> | <span data-ttu-id="5c982-122">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="5c982-122">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="5c982-123">**Namn**</span><span class="sxs-lookup"><span data-stu-id="5c982-123">**Name**</span></span>|<span data-ttu-id="5c982-124">contoso.net</span><span class="sxs-lookup"><span data-stu-id="5c982-124">contoso.net</span></span>|<span data-ttu-id="5c982-125">hello namnet på hello DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="5c982-125">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="5c982-126">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="5c982-126">**Subscription**</span></span>|<span data-ttu-id="5c982-127">[Din prenumeration]</span><span class="sxs-lookup"><span data-stu-id="5c982-127">[Your subscription]</span></span>|<span data-ttu-id="5c982-128">Välj en prenumeration toocreate hello Programgateway i.</span><span class="sxs-lookup"><span data-stu-id="5c982-128">Select a subscription toocreate hello application gateway in.</span></span>|
   |<span data-ttu-id="5c982-129">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="5c982-129">**Resource group**</span></span>|<span data-ttu-id="5c982-130">**Skapa ny:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="5c982-130">**Create new:** contosoRG</span></span>|<span data-ttu-id="5c982-131">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="5c982-131">Create a resource group.</span></span> <span data-ttu-id="5c982-132">hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt.</span><span class="sxs-lookup"><span data-stu-id="5c982-132">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="5c982-133">Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) översiktsartikel.</span><span class="sxs-lookup"><span data-stu-id="5c982-133">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="5c982-134">**Plats**</span><span class="sxs-lookup"><span data-stu-id="5c982-134">**Location**</span></span>|<span data-ttu-id="5c982-135">Västra USA</span><span class="sxs-lookup"><span data-stu-id="5c982-135">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="5c982-136">hello resursgruppen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="5c982-136">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="5c982-137">hello DNS-zonen plats är alltid ”globala” och visas inte.</span><span class="sxs-lookup"><span data-stu-id="5c982-137">hello DNS zone location is always "global", and is not shown.</span></span>

## <a name="retrieve-name-servers"></a><span data-ttu-id="5c982-138">Hämta namnservrar</span><span class="sxs-lookup"><span data-stu-id="5c982-138">Retrieve name servers</span></span>

<span data-ttu-id="5c982-139">Innan du kan delegera din DNS-zonen tooAzure DNS, måste du först tooknow hello servernamn för zonen.</span><span class="sxs-lookup"><span data-stu-id="5c982-139">Before you can delegate your DNS zone tooAzure DNS, you first need tooknow hello name server names for your zone.</span></span> <span data-ttu-id="5c982-140">Azure DNS allokerar namnservrar från en pool varje gång en zon skapas.</span><span class="sxs-lookup"><span data-stu-id="5c982-140">Azure DNS allocates name servers from a pool each time a zone is created.</span></span>

1. <span data-ttu-id="5c982-141">Med hello DNS-zon som skapats i hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="5c982-141">With hello DNS zone created, in hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="5c982-142">Klicka på hello **contoso.net** DNS-zonen i hello **alla resurser** bladet.</span><span class="sxs-lookup"><span data-stu-id="5c982-142">Click hello **contoso.net** DNS zone in hello **All resources** blade.</span></span> <span data-ttu-id="5c982-143">Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **contoso.net** i hello filtrera efter namn...</span><span class="sxs-lookup"><span data-stu-id="5c982-143">If hello subscription you selected already has several resources in it, you can enter **contoso.net** in hello Filter by name…</span></span> <span data-ttu-id="5c982-144">rutan Programgateway tooeasily åtkomst hello.</span><span class="sxs-lookup"><span data-stu-id="5c982-144">box tooeasily access hello application gateway.</span></span> 

1. <span data-ttu-id="5c982-145">Hämta hello namnservrar från hello DNS-zonen bladet.</span><span class="sxs-lookup"><span data-stu-id="5c982-145">Retrieve hello name servers from hello DNS zone blade.</span></span> <span data-ttu-id="5c982-146">I det här exemplet hello zonen 'contoso.net' har tilldelats namnservrar ' ns1-01.azure-dns.com', 'ns2-01.azure-DNS-.net', ' ns3-01.azure-dns.org', och ' ns4-01.azure-dns.info':</span><span class="sxs-lookup"><span data-stu-id="5c982-146">In this example, hello zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![DNS-namnserver](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="5c982-148">Azure DNS skapas automatiskt auktoritära NS-poster i zonen som innehåller hello tilldelade namnservrar.</span><span class="sxs-lookup"><span data-stu-id="5c982-148">Azure DNS automatically creates authoritative NS records in your zone containing hello assigned name servers.</span></span>  <span data-ttu-id="5c982-149">toosee hello namnserver namn via Azure PowerShell eller Azure CLI, du behöver tooretrieve dessa poster.</span><span class="sxs-lookup"><span data-stu-id="5c982-149">toosee hello name server names via Azure PowerShell or Azure CLI, you simply need tooretrieve these records.</span></span>

<span data-ttu-id="5c982-150">hello dessutom följande exempel hello steg tooretrieve hello namnservrar för en zon i Azure DNS med PowerShell och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="5c982-150">hello following examples also provide hello steps tooretrieve hello name servers for a zone in Azure DNS with PowerShell and Azure CLI.</span></span>

### <a name="powershell"></a><span data-ttu-id="5c982-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5c982-151">PowerShell</span></span>

```powershell
# hello record name "@" is used toorefer toorecords at hello top of hello zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

<span data-ttu-id="5c982-152">följande exempel hello är hello svar.</span><span class="sxs-lookup"><span data-stu-id="5c982-152">hello following example is hello response.</span></span>

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a><span data-ttu-id="5c982-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5c982-153">Azure CLI</span></span>

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

<span data-ttu-id="5c982-154">följande exempel hello är hello svar.</span><span class="sxs-lookup"><span data-stu-id="5c982-154">hello following example is hello response.</span></span>

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-hello-domain"></a><span data-ttu-id="5c982-155">Delegera hello domän</span><span class="sxs-lookup"><span data-stu-id="5c982-155">Delegate hello domain</span></span>

<span data-ttu-id="5c982-156">Nu när du har hello namnservrar och hello DNS-zonen skapas måste toobe uppdateras med hello Azure DNS-namnservrar hello överordnade domänen.</span><span class="sxs-lookup"><span data-stu-id="5c982-156">Now that hello DNS zone is created and you have hello name servers, hello parent domain needs toobe updated with hello Azure DNS name servers.</span></span> <span data-ttu-id="5c982-157">Varje register har sina egna DNS management tools toochange hello namnserverposter för en domän.</span><span class="sxs-lookup"><span data-stu-id="5c982-157">Each registrar has their own DNS management tools toochange hello name server records for a domain.</span></span> <span data-ttu-id="5c982-158">Hej domänregistrators DNS-hantering på sidan Redigera hello NS-poster och Ersätt hello NS-poster med hello som skapats i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5c982-158">In hello registrar's DNS management page, edit hello NS records and replace hello NS records with hello ones Azure DNS created.</span></span>

<span data-ttu-id="5c982-159">När delegera en domän tooAzure DNS, måste du använda hello servernamn tillhandahålls av Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5c982-159">When delegating a domain tooAzure DNS, you must use hello name server names provided by Azure DNS.</span></span> <span data-ttu-id="5c982-160">Det rekommenderas toouse alla fyra namn servernamn, oavsett hello namnet på din domän.</span><span class="sxs-lookup"><span data-stu-id="5c982-160">It is recommended toouse all four name server names, regardless of hello name of your domain.</span></span> <span data-ttu-id="5c982-161">Domän-delegering kräver inte hello name server name toouse hello samma toppnivådomänen som din domän.</span><span class="sxs-lookup"><span data-stu-id="5c982-161">Domain delegation does not require hello name server name toouse hello same top-level domain as your domain.</span></span>

<span data-ttu-id="5c982-162">Du bör inte använda ”sammanlänkande poster' toopoint toohello Azure DNS-namnet IP-adresser, eftersom dessa IP-adresser kan ändras i framtiden.</span><span class="sxs-lookup"><span data-stu-id="5c982-162">You should not use 'glue records' toopoint toohello Azure DNS name server IP addresses, since these IP addresses may change in future.</span></span> <span data-ttu-id="5c982-163">Delegering med hjälp av namnservernamn i din egen zon, även kallat ”vanity name servers”, stöds inte i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5c982-163">Delegations using name server names in your own zone, sometimes called 'vanity name servers', are not currently supported in Azure DNS.</span></span>

## <a name="verify-name-resolution-is-working"></a><span data-ttu-id="5c982-164">Kontrollera att namnmatchningen fungerar</span><span class="sxs-lookup"><span data-stu-id="5c982-164">Verify name resolution is working</span></span>

<span data-ttu-id="5c982-165">När du har slutfört hello delegering kan du kontrollera att namnmatchning fungerar genom att använda ett verktyg som till exempel ”nslookup” tooquery hello SOA-post för zonen (som skapas automatiskt när hello zon skapas).</span><span class="sxs-lookup"><span data-stu-id="5c982-165">After completing hello delegation, you can verify that name resolution is working by using a tool such as 'nslookup' tooquery hello SOA record for your zone (which is also automatically created when hello zone is created).</span></span>

<span data-ttu-id="5c982-166">Du har inte toospecify hello Azure DNS-namnservrar hittar om hello delegering har ställts in korrekt hello normal DNS lösningsprocessen hello namnservrar automatiskt.</span><span class="sxs-lookup"><span data-stu-id="5c982-166">You do not have toospecify hello Azure DNS name servers, if hello delegation has been set up correctly, hello normal DNS resolution process finds hello name servers automatically.</span></span>

```
nslookup -type=SOA contoso.com
```

<span data-ttu-id="5c982-167">hello följande är ett exempelsvar från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="5c982-167">hello following is an example response from hello preceding command:</span></span>

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-sub-domains-in-azure-dns"></a><span data-ttu-id="5c982-168">Delegera underdomäner i Azure DNS</span><span class="sxs-lookup"><span data-stu-id="5c982-168">Delegate sub-domains in Azure DNS</span></span>

<span data-ttu-id="5c982-169">Om du vill tooset in en separat underordnad zon kan delegera du en underordnad domän i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5c982-169">If you want tooset up a separate child zone, you can delegate a sub-domain in Azure DNS.</span></span> <span data-ttu-id="5c982-170">Till exempel att ställa in och delegerad 'contoso.net' i Azure DNS du anta att du vill tooset in en separat underordnad zon 'partners.contoso.net'.</span><span class="sxs-lookup"><span data-stu-id="5c982-170">For example, having set up and delegated 'contoso.net' in Azure DNS, suppose you would like tooset up a separate child zone, 'partners.contoso.net'.</span></span>

1. <span data-ttu-id="5c982-171">Skapa partners.contoso.net' hello underordnade zonen' i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5c982-171">Create hello child zone 'partners.contoso.net' in Azure DNS.</span></span>
2. <span data-ttu-id="5c982-172">Leta upp hello auktoritära NS-poster i hello underordnade zonen tooobtain hello namnservrar värd hello underordnade zonen i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="5c982-172">Look up hello authoritative NS records in hello child zone tooobtain hello name servers hosting hello child zone in Azure DNS.</span></span>
3. <span data-ttu-id="5c982-173">Delegera hello underordnade zonen genom att konfigurera NS-poster i hello överordnade zonen pekar toohello underordnad zon.</span><span class="sxs-lookup"><span data-stu-id="5c982-173">Delegate hello child zone by configuring NS records in hello parent zone pointing toohello child zone.</span></span>

### <a name="create-a-dns-zone"></a><span data-ttu-id="5c982-174">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="5c982-174">Create a DNS zone</span></span>

1. <span data-ttu-id="5c982-175">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5c982-175">Sign in toohello Azure portal</span></span>
1. <span data-ttu-id="5c982-176">Hej hubbmenyn, klicka på och klicka på **New > nätverk >** och klicka sedan på **DNS-zonen** tooopen hello skapa DNS-zonen bladet.</span><span class="sxs-lookup"><span data-stu-id="5c982-176">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![DNS-zon](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="5c982-178">På hello **skapa DNS-zonen** bladet ange hello följande värden, och klicka sedan på **skapa**:</span><span class="sxs-lookup"><span data-stu-id="5c982-178">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>

   | <span data-ttu-id="5c982-179">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="5c982-179">**Setting**</span></span> | <span data-ttu-id="5c982-180">**Värde**</span><span class="sxs-lookup"><span data-stu-id="5c982-180">**Value**</span></span> | <span data-ttu-id="5c982-181">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="5c982-181">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="5c982-182">**Namn**</span><span class="sxs-lookup"><span data-stu-id="5c982-182">**Name**</span></span>|<span data-ttu-id="5c982-183">partners.contoso.net</span><span class="sxs-lookup"><span data-stu-id="5c982-183">partners.contoso.net</span></span>|<span data-ttu-id="5c982-184">hello namnet på hello DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="5c982-184">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="5c982-185">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="5c982-185">**Subscription**</span></span>|<span data-ttu-id="5c982-186">[Din prenumeration]</span><span class="sxs-lookup"><span data-stu-id="5c982-186">[Your subscription]</span></span>|<span data-ttu-id="5c982-187">Välj en prenumeration toocreate hello Programgateway i.</span><span class="sxs-lookup"><span data-stu-id="5c982-187">Select a subscription toocreate hello application gateway in.</span></span>|
   |<span data-ttu-id="5c982-188">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="5c982-188">**Resource group**</span></span>|<span data-ttu-id="5c982-189">**Använd befintlig:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="5c982-189">**Use Existing:** contosoRG</span></span>|<span data-ttu-id="5c982-190">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="5c982-190">Create a resource group.</span></span> <span data-ttu-id="5c982-191">hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt.</span><span class="sxs-lookup"><span data-stu-id="5c982-191">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="5c982-192">Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) översiktsartikel.</span><span class="sxs-lookup"><span data-stu-id="5c982-192">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="5c982-193">**Plats**</span><span class="sxs-lookup"><span data-stu-id="5c982-193">**Location**</span></span>|<span data-ttu-id="5c982-194">Västra USA</span><span class="sxs-lookup"><span data-stu-id="5c982-194">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="5c982-195">hello resursgruppen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="5c982-195">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="5c982-196">hello DNS-zonen plats är alltid ”globala” och visas inte.</span><span class="sxs-lookup"><span data-stu-id="5c982-196">hello DNS zone location is always "global", and is not shown.</span></span>

### <a name="retrieve-name-servers"></a><span data-ttu-id="5c982-197">Hämta namnservrar</span><span class="sxs-lookup"><span data-stu-id="5c982-197">Retrieve name servers</span></span>

1. <span data-ttu-id="5c982-198">Med hello DNS-zon som skapats i hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="5c982-198">With hello DNS zone created, in hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="5c982-199">Klicka på hello **partners.contoso.net** DNS-zonen i hello **alla resurser** bladet.</span><span class="sxs-lookup"><span data-stu-id="5c982-199">Click hello **partners.contoso.net** DNS zone in hello **All resources** blade.</span></span> <span data-ttu-id="5c982-200">Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **partners.contoso.net** i hello filtrera efter namn...</span><span class="sxs-lookup"><span data-stu-id="5c982-200">If hello subscription you selected already has several resources in it, you can enter **partners.contoso.net** in hello Filter by name…</span></span> <span data-ttu-id="5c982-201">rutan tooeasily åtkomst hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="5c982-201">box tooeasily access hello DNS zone.</span></span>

1. <span data-ttu-id="5c982-202">Hämta hello namnservrar från hello DNS-zonen bladet.</span><span class="sxs-lookup"><span data-stu-id="5c982-202">Retrieve hello name servers from hello DNS zone blade.</span></span> <span data-ttu-id="5c982-203">I det här exemplet hello zonen 'contoso.net' har tilldelats namnservrar ' ns1-01.azure-dns.com', 'ns2-01.azure-DNS-.net', ' ns3-01.azure-dns.org', och ' ns4-01.azure-dns.info':</span><span class="sxs-lookup"><span data-stu-id="5c982-203">In this example, hello zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![DNS-namnserver](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="5c982-205">Azure DNS skapas automatiskt auktoritära NS-poster i zonen som innehåller hello tilldelade namnservrar.</span><span class="sxs-lookup"><span data-stu-id="5c982-205">Azure DNS automatically creates authoritative NS records in your zone containing hello assigned name servers.</span></span>  <span data-ttu-id="5c982-206">toosee hello namnserver namn via Azure PowerShell eller Azure CLI, du behöver tooretrieve dessa poster.</span><span class="sxs-lookup"><span data-stu-id="5c982-206">toosee hello name server names via Azure PowerShell or Azure CLI, you simply need tooretrieve these records.</span></span>

### <a name="create-name-server-record-in-parent-zone"></a><span data-ttu-id="5c982-207">Skapa namnserverpost i överordnad zon</span><span class="sxs-lookup"><span data-stu-id="5c982-207">Create name server record in parent zone</span></span>

1. <span data-ttu-id="5c982-208">Navigera toohello **contoso.net** DNS-zonen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5c982-208">Navigate toohello **contoso.net** DNS zone in hello Azure portal.</span></span>
1. <span data-ttu-id="5c982-209">Klicka på **+ Postuppsättning**</span><span class="sxs-lookup"><span data-stu-id="5c982-209">Click **+ Record set**</span></span>
1. <span data-ttu-id="5c982-210">På hello **lägga till postuppsättning** bladet ange hello följande värden, och klicka sedan på **OK**:</span><span class="sxs-lookup"><span data-stu-id="5c982-210">On hello **Add record set** blade, enter hello following values, then click **OK**:</span></span>

   | <span data-ttu-id="5c982-211">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="5c982-211">**Setting**</span></span> | <span data-ttu-id="5c982-212">**Värde**</span><span class="sxs-lookup"><span data-stu-id="5c982-212">**Value**</span></span> | <span data-ttu-id="5c982-213">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="5c982-213">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="5c982-214">**Namn**</span><span class="sxs-lookup"><span data-stu-id="5c982-214">**Name**</span></span>|<span data-ttu-id="5c982-215">partner</span><span class="sxs-lookup"><span data-stu-id="5c982-215">partners</span></span>|<span data-ttu-id="5c982-216">hello namnet på hello underordnade DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="5c982-216">hello name of hello child DNS zone</span></span>|
   |<span data-ttu-id="5c982-217">**Typ**</span><span class="sxs-lookup"><span data-stu-id="5c982-217">**Type**</span></span>|<span data-ttu-id="5c982-218">NS</span><span class="sxs-lookup"><span data-stu-id="5c982-218">NS</span></span>|<span data-ttu-id="5c982-219">Använd NS för namnserverposter.</span><span class="sxs-lookup"><span data-stu-id="5c982-219">Use NS for name server records.</span></span>|
   |<span data-ttu-id="5c982-220">**TTL**</span><span class="sxs-lookup"><span data-stu-id="5c982-220">**TTL**</span></span>|<span data-ttu-id="5c982-221">1</span><span class="sxs-lookup"><span data-stu-id="5c982-221">1</span></span>|<span data-ttu-id="5c982-222">Tid toolive.</span><span class="sxs-lookup"><span data-stu-id="5c982-222">Time toolive.</span></span>|
   |<span data-ttu-id="5c982-223">**TTL-enhet**</span><span class="sxs-lookup"><span data-stu-id="5c982-223">**TTL unit**</span></span>|<span data-ttu-id="5c982-224">Timmar</span><span class="sxs-lookup"><span data-stu-id="5c982-224">Hours</span></span>|<span data-ttu-id="5c982-225">Anger tid toolive enhet toohours</span><span class="sxs-lookup"><span data-stu-id="5c982-225">sets time toolive unit toohours</span></span>|
   |<span data-ttu-id="5c982-226">**NAMNSERVER**</span><span class="sxs-lookup"><span data-stu-id="5c982-226">**NAME SERVER**</span></span>|<span data-ttu-id="5c982-227">{namnservrar från zonen partners.contoso.net}</span><span class="sxs-lookup"><span data-stu-id="5c982-227">{name servers from partners.contoso.net zone}</span></span>|<span data-ttu-id="5c982-228">Ange alla 4 i hello namnservrar från partners.contoso.net zon.</span><span class="sxs-lookup"><span data-stu-id="5c982-228">Enter all 4 of hello name servers from partners.contoso.net zone.</span></span> |

   ![DNS-namnserver](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a><span data-ttu-id="5c982-230">Delegera underdomäner i Azure DNS med andra verktyg</span><span class="sxs-lookup"><span data-stu-id="5c982-230">Delegating sub-domains in Azure DNS with other tools</span></span>

<span data-ttu-id="5c982-231">hello innehåller följande exempel hello steg toodelegate underdomäner i Azure DNS med PowerShell och CLI:</span><span class="sxs-lookup"><span data-stu-id="5c982-231">hello following examples provide hello steps toodelegate sub-domains in Azure DNS with PowerShell and CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="5c982-232">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5c982-232">PowerShell</span></span>

<span data-ttu-id="5c982-233">hello följande PowerShell-exempel visar hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="5c982-233">hello following PowerShell example demonstrates how this works.</span></span> <span data-ttu-id="5c982-234">hello samma steg kan utföras via hello Azure-portalen eller via hello plattformsoberoende Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="5c982-234">hello same steps can be executed via hello Azure portal, or via hello cross-platform Azure CLI.</span></span>

```powershell
# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve hello authoritative NS records from hello child zone as shown in hello next example. This contains hello name servers assigned toohello child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create hello corresponding NS record set in hello parent zone toocomplete hello delegation. hello record set name in hello parent zone matches hello child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

<span data-ttu-id="5c982-235">Använd `nslookup` tooverify allt har konfigurerats korrekt genom att leta upp hello SOA-post på hello underordnad zon.</span><span class="sxs-lookup"><span data-stu-id="5c982-235">Use `nslookup` tooverify that everything is set up correctly by looking up hello SOA record of hello child zone.</span></span>

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a><span data-ttu-id="5c982-236">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5c982-236">Azure CLI</span></span>

```azurecli
#!/bin/bash

# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

<span data-ttu-id="5c982-237">Hämta hello namnservrar för hello `partners.contoso.net` zon från hello-utdata.</span><span class="sxs-lookup"><span data-stu-id="5c982-237">Retrieve hello name servers for hello `partners.contoso.net` zone from hello output.</span></span>

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

<span data-ttu-id="5c982-238">Skapa hello postuppsättning och NS-poster för varje namnserver.</span><span class="sxs-lookup"><span data-stu-id="5c982-238">Create hello record set and NS records for each name server.</span></span>

```azurecli
#!/bin/bash

# Create hello record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a><span data-ttu-id="5c982-239">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="5c982-239">Delete all resources</span></span>

<span data-ttu-id="5c982-240">toodelete alla resurser skapas i den här artikeln, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c982-240">toodelete all resources created in this article, complete hello following steps:</span></span>

1. <span data-ttu-id="5c982-241">I hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="5c982-241">In hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="5c982-242">Klicka på hello **contosorg** resursgrupp i hello bladet för alla resurser.</span><span class="sxs-lookup"><span data-stu-id="5c982-242">Click hello **contosorg** resource group in hello All resources blade.</span></span> <span data-ttu-id="5c982-243">Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **contosorg** i hello **filtrera efter namn...**</span><span class="sxs-lookup"><span data-stu-id="5c982-243">If hello subscription you selected already has several resources in it, you can enter **contosorg** in hello **Filter by name…**</span></span> <span data-ttu-id="5c982-244">rutan tooeasily åtkomst hello resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="5c982-244">box tooeasily access hello resource group.</span></span>
1. <span data-ttu-id="5c982-245">I hello **contosorg** bladet, klickar du på hello **ta bort** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c982-245">In hello **contosorg** blade, click hello **Delete** button.</span></span>
1. <span data-ttu-id="5c982-246">hello portalen måste du tootype hello namnet på hello resurs grupp tooconfirm som du vill toodelete den.</span><span class="sxs-lookup"><span data-stu-id="5c982-246">hello portal requires you tootype hello name of hello resource group tooconfirm that you want toodelete it.</span></span> <span data-ttu-id="5c982-247">Typen *contosorg* hello resursgruppens namn, sedan klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5c982-247">Type *contosorg* for hello resource group name, then click **Delete**.</span></span> <span data-ttu-id="5c982-248">Tar bort en resursgrupp alla resurser inom hello resursgrupp, så alltid att tooconfirm hello innehållet i en resursgrupp innan den tas bort.</span><span class="sxs-lookup"><span data-stu-id="5c982-248">Deleting a resource group deletes all resources within hello resource group, so always be sure tooconfirm hello contents of a resource group before deleting it.</span></span> <span data-ttu-id="5c982-249">hello portal tar bort alla resurser som ingår i hello resursgrupp och sedan tar bort hello resursgruppen sig själv.</span><span class="sxs-lookup"><span data-stu-id="5c982-249">hello portal deletes all resources contained within hello resource group, then deletes hello resource group itself.</span></span> <span data-ttu-id="5c982-250">Den här processen tar flera minuter.</span><span class="sxs-lookup"><span data-stu-id="5c982-250">This process takes several minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c982-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c982-251">Next steps</span></span>

[<span data-ttu-id="5c982-252">Hantera DNS-zoner</span><span class="sxs-lookup"><span data-stu-id="5c982-252">Manage DNS zones</span></span>](dns-operations-dnszones.md)

[<span data-ttu-id="5c982-253">Hantera DNS-poster</span><span class="sxs-lookup"><span data-stu-id="5c982-253">Manage DNS records</span></span>](dns-operations-recordsets.md)
