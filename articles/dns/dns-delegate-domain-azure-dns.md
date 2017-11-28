---
title: "Delegera din domän till Azure DNS | Microsoft Docs"
description: "Lär dig hur du ändrar domändelegering och använder Azure DNS-namnservrar för att tillhandahålla domänvärdtjänster."
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
ms.openlocfilehash: 33b3ec24432ff1268860b9a2e9d5098600a8dedc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="delegate-a-domain-to-azure-dns"></a><span data-ttu-id="78b12-103">Delegera en domän till Azure DNS</span><span class="sxs-lookup"><span data-stu-id="78b12-103">Delegate a domain to Azure DNS</span></span>

<span data-ttu-id="78b12-104">Med Azure DNS kan du vara värd för en DNS-zon och hantera DNS-posterna för en domän i Azure.</span><span class="sxs-lookup"><span data-stu-id="78b12-104">Azure DNS allows you to host a DNS zone and manage the DNS records for a domain in Azure.</span></span> <span data-ttu-id="78b12-105">För att DNS-frågor för en domän ska nå Azure DNS måste domänen delegeras till Azure DNS från den överordnade domänen.</span><span class="sxs-lookup"><span data-stu-id="78b12-105">In order for DNS queries for a domain to reach Azure DNS, the domain has to be delegated to Azure DNS from the parent domain.</span></span> <span data-ttu-id="78b12-106">Tänk på att Azure DNS inte är domänregistratorn.</span><span class="sxs-lookup"><span data-stu-id="78b12-106">Keep in mind Azure DNS is not the domain registrar.</span></span> <span data-ttu-id="78b12-107">I artikeln förklaras hur du delegerar din domän till Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="78b12-107">This article explains how to delegate your domain to Azure DNS.</span></span>

<span data-ttu-id="78b12-108">Om domänerna köpts från en registrator kan registratorn konfigurera dessa NS-poster.</span><span class="sxs-lookup"><span data-stu-id="78b12-108">For domains purchased from a registrar, your registrar offers the option to set up these NS records.</span></span> <span data-ttu-id="78b12-109">Du behöver inte äga en domän för att kunna skapa en DNS-zon med det domännamnet i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="78b12-109">You do not have to own a domain to create a DNS zone with that domain name in Azure DNS.</span></span> <span data-ttu-id="78b12-110">Du behöver äga domänen för att konfigurera delegeringen till Azure DNS med registratorn.</span><span class="sxs-lookup"><span data-stu-id="78b12-110">However, you do need to own the domain to set up the delegation to Azure DNS with the registrar.</span></span>

<span data-ttu-id="78b12-111">Anta exempelvis att du köper domänen ”contoso.net” och skapar en zon med namnet ”contoso.net” i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="78b12-111">For example, suppose you purchase the domain 'contoso.net' and create a zone with the name 'contoso.net' in Azure DNS.</span></span> <span data-ttu-id="78b12-112">Som ägare till domänen erbjuder sig registratorn att konfigurera namnserveradresserna (det vill säga NS-posterna) för din domän.</span><span class="sxs-lookup"><span data-stu-id="78b12-112">As the owner of the domain, your registrar offers you the option to configure the name server addresses (that is, the NS records) for your domain.</span></span> <span data-ttu-id="78b12-113">Registratorn lagrar dessa NS-poster i den överordnade domänen, i detta fall ”.net”.</span><span class="sxs-lookup"><span data-stu-id="78b12-113">The registrar stores these NS records in the parent domain, in this case '.net'.</span></span> <span data-ttu-id="78b12-114">Klienter över hela världen kan sedan omdirigeras till din domän i Azure DNS-zonen när du försöker matcha DNS-poster i ”contoso.net”.</span><span class="sxs-lookup"><span data-stu-id="78b12-114">Clients around the world can then be directed to your domain in Azure DNS zone when trying to resolve DNS records in 'contoso.net'.</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="78b12-115">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="78b12-115">Create a DNS zone</span></span>

1. <span data-ttu-id="78b12-116">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="78b12-116">Sign in to the Azure portal</span></span>
1. <span data-ttu-id="78b12-117">Klicka på **Nytt > Nätverk >** på navmenyn och klicka sedan på **DNS-zon** för att öppna bladet Skapa DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="78b12-117">On the Hub menu, click and click **New > Networking >** and then click **DNS zone** to open the Create DNS zone blade.</span></span>

    ![DNS-zon](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="78b12-119">På bladet **Skapa DNS-zon** anger du följande värden och klickar sedan på **Skapa**:</span><span class="sxs-lookup"><span data-stu-id="78b12-119">On the **Create DNS zone** blade enter the following values, then click **Create**:</span></span>

   | <span data-ttu-id="78b12-120">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="78b12-120">**Setting**</span></span> | <span data-ttu-id="78b12-121">**Värde**</span><span class="sxs-lookup"><span data-stu-id="78b12-121">**Value**</span></span> | <span data-ttu-id="78b12-122">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="78b12-122">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="78b12-123">**Namn**</span><span class="sxs-lookup"><span data-stu-id="78b12-123">**Name**</span></span>|<span data-ttu-id="78b12-124">contoso.net</span><span class="sxs-lookup"><span data-stu-id="78b12-124">contoso.net</span></span>|<span data-ttu-id="78b12-125">Namnet på DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="78b12-125">The name of the DNS zone</span></span>|
   |<span data-ttu-id="78b12-126">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="78b12-126">**Subscription**</span></span>|<span data-ttu-id="78b12-127">[Din prenumeration]</span><span class="sxs-lookup"><span data-stu-id="78b12-127">[Your subscription]</span></span>|<span data-ttu-id="78b12-128">Välj den prenumeration där du vill skapa programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="78b12-128">Select a subscription to create the application gateway in.</span></span>|
   |<span data-ttu-id="78b12-129">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="78b12-129">**Resource group**</span></span>|<span data-ttu-id="78b12-130">**Skapa ny:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="78b12-130">**Create new:** contosoRG</span></span>|<span data-ttu-id="78b12-131">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="78b12-131">Create a resource group.</span></span> <span data-ttu-id="78b12-132">Resursgruppens namn måste vara unikt inom den prenumeration du valde.</span><span class="sxs-lookup"><span data-stu-id="78b12-132">The resource group name must be unique within the subscription you selected.</span></span> <span data-ttu-id="78b12-133">Mer information om resursgrupper finns i [översikten över Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="78b12-133">To learn more about resource groups, read the [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="78b12-134">**Plats**</span><span class="sxs-lookup"><span data-stu-id="78b12-134">**Location**</span></span>|<span data-ttu-id="78b12-135">Västra USA</span><span class="sxs-lookup"><span data-stu-id="78b12-135">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="78b12-136">Resursgruppen refererar till platsen för resursgruppen och har ingen inverkan på DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="78b12-136">The resource group refers to the location of the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="78b12-137">Platsen för DNS-zonen är alltid "global" och visas inte.</span><span class="sxs-lookup"><span data-stu-id="78b12-137">The DNS zone location is always "global", and is not shown.</span></span>

## <a name="retrieve-name-servers"></a><span data-ttu-id="78b12-138">Hämta namnservrar</span><span class="sxs-lookup"><span data-stu-id="78b12-138">Retrieve name servers</span></span>

<span data-ttu-id="78b12-139">Innan du kan delegera din DNS-zon till Azure DNS måste du känna till namnservernamnen för din zon.</span><span class="sxs-lookup"><span data-stu-id="78b12-139">Before you can delegate your DNS zone to Azure DNS, you first need to know the name server names for your zone.</span></span> <span data-ttu-id="78b12-140">Azure DNS allokerar namnservrar från en pool varje gång en zon skapas.</span><span class="sxs-lookup"><span data-stu-id="78b12-140">Azure DNS allocates name servers from a pool each time a zone is created.</span></span>

1. <span data-ttu-id="78b12-141">I Azure Portal klickar du på **Alla resurser** i rutan **Favoriter** för den DNS-zon du skapade.</span><span class="sxs-lookup"><span data-stu-id="78b12-141">With the DNS zone created, in the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="78b12-142">Klicka på DNS-zonen **contoso.net** på bladet **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="78b12-142">Click the **contoso.net** DNS zone in the **All resources** blade.</span></span> <span data-ttu-id="78b12-143">Om den prenumeration du valde redan har flera resurser kan du ange **contoso.net** i rutan Filtrera efter namn...</span><span class="sxs-lookup"><span data-stu-id="78b12-143">If the subscription you selected already has several resources in it, you can enter **contoso.net** in the Filter by name…</span></span> <span data-ttu-id="78b12-144">för att enkelt få åtkomst till Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="78b12-144">box to easily access the application gateway.</span></span> 

1. <span data-ttu-id="78b12-145">Hämta namnservrarna på bladet DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="78b12-145">Retrieve the name servers from the DNS zone blade.</span></span> <span data-ttu-id="78b12-146">I det här exemplet har zonen ”contoso.net” tilldelats namnservrarna ”ns1-01.azure-dns.com”, ”ns2-01.azure-dns.net”, ”ns3-01.azure-dns.org” och ”ns4-01.azure-dns.info”:</span><span class="sxs-lookup"><span data-stu-id="78b12-146">In this example, the zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![DNS-namnserver](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="78b12-148">Azure DNS skapar automatiskt auktoritativa NS-poster i din zon som innehåller de tilldelade namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="78b12-148">Azure DNS automatically creates authoritative NS records in your zone containing the assigned name servers.</span></span>  <span data-ttu-id="78b12-149">Om du vill se namnservernamnen via Azure PowerShell eller Azure CLI behöver du bara hämta dessa poster.</span><span class="sxs-lookup"><span data-stu-id="78b12-149">To see the name server names via Azure PowerShell or Azure CLI, you simply need to retrieve these records.</span></span>

<span data-ttu-id="78b12-150">I följande exempel finns exempel på hur du kan hämta namnservrarna för en zon i Azure DNS med PowerShell och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="78b12-150">The following examples also provide the steps to retrieve the name servers for a zone in Azure DNS with PowerShell and Azure CLI.</span></span>

### <a name="powershell"></a><span data-ttu-id="78b12-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="78b12-151">PowerShell</span></span>

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

<span data-ttu-id="78b12-152">Följande exempel är svaret.</span><span class="sxs-lookup"><span data-stu-id="78b12-152">The following example is the response.</span></span>

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

### <a name="azure-cli"></a><span data-ttu-id="78b12-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="78b12-153">Azure CLI</span></span>

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

<span data-ttu-id="78b12-154">Följande exempel är svaret.</span><span class="sxs-lookup"><span data-stu-id="78b12-154">The following example is the response.</span></span>

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

## <a name="delegate-the-domain"></a><span data-ttu-id="78b12-155">Delegera domänen</span><span class="sxs-lookup"><span data-stu-id="78b12-155">Delegate the domain</span></span>

<span data-ttu-id="78b12-156">Nu när du har namnservrar och DNS-zonen är skapad måste den överordnade domänen uppdateras med Azure DNS-namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="78b12-156">Now that the DNS zone is created and you have the name servers, the parent domain needs to be updated with the Azure DNS name servers.</span></span> <span data-ttu-id="78b12-157">Varje registrator har sina egna DNS-hanteringsverktyg för att ändra namnserverposterna för en domän.</span><span class="sxs-lookup"><span data-stu-id="78b12-157">Each registrar has their own DNS management tools to change the name server records for a domain.</span></span> <span data-ttu-id="78b12-158">På registratorns DNS-hanteringssida redigerar du NS-posterna och ersätter NS-posterna med dem som Azure DNS skapat.</span><span class="sxs-lookup"><span data-stu-id="78b12-158">In the registrar's DNS management page, edit the NS records and replace the NS records with the ones Azure DNS created.</span></span>

<span data-ttu-id="78b12-159">När du delegerar en domän till Azure DNS måste du använda namnservernamnen som tillhandahålls av Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="78b12-159">When delegating a domain to Azure DNS, you must use the name server names provided by Azure DNS.</span></span> <span data-ttu-id="78b12-160">Vi rekommenderar att du använder alla fyra namnservernamn, oavsett vilket namn din domän har.</span><span class="sxs-lookup"><span data-stu-id="78b12-160">It is recommended to use all four name server names, regardless of the name of your domain.</span></span> <span data-ttu-id="78b12-161">Domändelegering kräver inte att namnservernamnet använder samma toppnivådomän som din domän.</span><span class="sxs-lookup"><span data-stu-id="78b12-161">Domain delegation does not require the name server name to use the same top-level domain as your domain.</span></span>

<span data-ttu-id="78b12-162">Använd inte ”fästposter” för att peka på IP-adresser för Azure DNS-namnservrar eftersom dessa IP-adresser kan komma att ändras i framtida.</span><span class="sxs-lookup"><span data-stu-id="78b12-162">You should not use 'glue records' to point to the Azure DNS name server IP addresses, since these IP addresses may change in future.</span></span> <span data-ttu-id="78b12-163">Delegering med hjälp av namnservernamn i din egen zon, även kallat ”vanity name servers”, stöds inte i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="78b12-163">Delegations using name server names in your own zone, sometimes called 'vanity name servers', are not currently supported in Azure DNS.</span></span>

## <a name="verify-name-resolution-is-working"></a><span data-ttu-id="78b12-164">Kontrollera att namnmatchningen fungerar</span><span class="sxs-lookup"><span data-stu-id="78b12-164">Verify name resolution is working</span></span>

<span data-ttu-id="78b12-165">När du har slutfört delegeringen kan du kontrollera att namnmatchningen fungerar med hjälp av ett verktyg som till exempel ”nslookup” för att fråga efter SOA-posten för din zon (som skapas automatiskt när zonen skapas).</span><span class="sxs-lookup"><span data-stu-id="78b12-165">After completing the delegation, you can verify that name resolution is working by using a tool such as 'nslookup' to query the SOA record for your zone (which is also automatically created when the zone is created).</span></span>

<span data-ttu-id="78b12-166">Du behöver inte ange Azure DNS-namnservrarna om delegeringen har konfigurerats korrekt. Den normala DNS-matchningsprocessen hittar namnservrarna automatiskt.</span><span class="sxs-lookup"><span data-stu-id="78b12-166">You do not have to specify the Azure DNS name servers, if the delegation has been set up correctly, the normal DNS resolution process finds the name servers automatically.</span></span>

```
nslookup -type=SOA contoso.com
```

<span data-ttu-id="78b12-167">Här följer ett exempel på ett svar från föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="78b12-167">The following is an example response from the preceding command:</span></span>

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

## <a name="delegate-sub-domains-in-azure-dns"></a><span data-ttu-id="78b12-168">Delegera underdomäner i Azure DNS</span><span class="sxs-lookup"><span data-stu-id="78b12-168">Delegate sub-domains in Azure DNS</span></span>

<span data-ttu-id="78b12-169">Om du vill konfigurera en separat underordnad zon kan du delegera en underdomän i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="78b12-169">If you want to set up a separate child zone, you can delegate a sub-domain in Azure DNS.</span></span> <span data-ttu-id="78b12-170">Anta till exempel att du har konfigurerat och delegerat ”contoso.net” i Azure DNS och att du vill konfigurera en separat underordnad zon, ”partners.contoso.net”.</span><span class="sxs-lookup"><span data-stu-id="78b12-170">For example, having set up and delegated 'contoso.net' in Azure DNS, suppose you would like to set up a separate child zone, 'partners.contoso.net'.</span></span>

1. <span data-ttu-id="78b12-171">Skapa den underordnade zonen ”partners.contoso.net” i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="78b12-171">Create the child zone 'partners.contoso.net' in Azure DNS.</span></span>
2. <span data-ttu-id="78b12-172">Leta upp de auktoritativa NS-posterna i den underordnade zonen  för att hämta namnservrarna som är värdar för den underordnade zonen i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="78b12-172">Look up the authoritative NS records in the child zone to obtain the name servers hosting the child zone in Azure DNS.</span></span>
3. <span data-ttu-id="78b12-173">Delegera den underordnade zonen genom att konfigurera NS-poster i den överordnade zonen som pekar på den underordnade zonen.</span><span class="sxs-lookup"><span data-stu-id="78b12-173">Delegate the child zone by configuring NS records in the parent zone pointing to the child zone.</span></span>

### <a name="create-a-dns-zone"></a><span data-ttu-id="78b12-174">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="78b12-174">Create a DNS zone</span></span>

1. <span data-ttu-id="78b12-175">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="78b12-175">Sign in to the Azure portal</span></span>
1. <span data-ttu-id="78b12-176">Klicka på **Nytt > Nätverk >** på navmenyn och klicka sedan på **DNS-zon** för att öppna bladet Skapa DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="78b12-176">On the Hub menu, click and click **New > Networking >** and then click **DNS zone** to open the Create DNS zone blade.</span></span>

    ![DNS-zon](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="78b12-178">På bladet **Skapa DNS-zon** anger du följande värden och klickar sedan på **Skapa**:</span><span class="sxs-lookup"><span data-stu-id="78b12-178">On the **Create DNS zone** blade enter the following values, then click **Create**:</span></span>

   | <span data-ttu-id="78b12-179">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="78b12-179">**Setting**</span></span> | <span data-ttu-id="78b12-180">**Värde**</span><span class="sxs-lookup"><span data-stu-id="78b12-180">**Value**</span></span> | <span data-ttu-id="78b12-181">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="78b12-181">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="78b12-182">**Namn**</span><span class="sxs-lookup"><span data-stu-id="78b12-182">**Name**</span></span>|<span data-ttu-id="78b12-183">partners.contoso.net</span><span class="sxs-lookup"><span data-stu-id="78b12-183">partners.contoso.net</span></span>|<span data-ttu-id="78b12-184">Namnet på DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="78b12-184">The name of the DNS zone</span></span>|
   |<span data-ttu-id="78b12-185">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="78b12-185">**Subscription**</span></span>|<span data-ttu-id="78b12-186">[Din prenumeration]</span><span class="sxs-lookup"><span data-stu-id="78b12-186">[Your subscription]</span></span>|<span data-ttu-id="78b12-187">Välj den prenumeration där du vill skapa programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="78b12-187">Select a subscription to create the application gateway in.</span></span>|
   |<span data-ttu-id="78b12-188">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="78b12-188">**Resource group**</span></span>|<span data-ttu-id="78b12-189">**Använd befintlig:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="78b12-189">**Use Existing:** contosoRG</span></span>|<span data-ttu-id="78b12-190">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="78b12-190">Create a resource group.</span></span> <span data-ttu-id="78b12-191">Resursgruppens namn måste vara unikt inom den prenumeration du valde.</span><span class="sxs-lookup"><span data-stu-id="78b12-191">The resource group name must be unique within the subscription you selected.</span></span> <span data-ttu-id="78b12-192">Mer information om resursgrupper finns i [översikten över Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="78b12-192">To learn more about resource groups, read the [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="78b12-193">**Plats**</span><span class="sxs-lookup"><span data-stu-id="78b12-193">**Location**</span></span>|<span data-ttu-id="78b12-194">Västra USA</span><span class="sxs-lookup"><span data-stu-id="78b12-194">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="78b12-195">Resursgruppen refererar till platsen för resursgruppen och har ingen inverkan på DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="78b12-195">The resource group refers to the location of the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="78b12-196">Platsen för DNS-zonen är alltid "global" och visas inte.</span><span class="sxs-lookup"><span data-stu-id="78b12-196">The DNS zone location is always "global", and is not shown.</span></span>

### <a name="retrieve-name-servers"></a><span data-ttu-id="78b12-197">Hämta namnservrar</span><span class="sxs-lookup"><span data-stu-id="78b12-197">Retrieve name servers</span></span>

1. <span data-ttu-id="78b12-198">I Azure Portal klickar du på **Alla resurser** i rutan **Favoriter** för den DNS-zon du skapade.</span><span class="sxs-lookup"><span data-stu-id="78b12-198">With the DNS zone created, in the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="78b12-199">Klicka på DNS-zonen **partners.contoso.net** på bladet **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="78b12-199">Click the **partners.contoso.net** DNS zone in the **All resources** blade.</span></span> <span data-ttu-id="78b12-200">Om den prenumeration du valde redan har flera resurser kan du ange **partners.contoso.net** i rutan Filtrera efter namn...</span><span class="sxs-lookup"><span data-stu-id="78b12-200">If the subscription you selected already has several resources in it, you can enter **partners.contoso.net** in the Filter by name…</span></span> <span data-ttu-id="78b12-201">när du ska hitta din DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="78b12-201">box to easily access the DNS zone.</span></span>

1. <span data-ttu-id="78b12-202">Hämta namnservrarna på bladet DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="78b12-202">Retrieve the name servers from the DNS zone blade.</span></span> <span data-ttu-id="78b12-203">I det här exemplet har zonen ”contoso.net” tilldelats namnservrarna ”ns1-01.azure-dns.com”, ”ns2-01.azure-dns.net”, ”ns3-01.azure-dns.org” och ”ns4-01.azure-dns.info”:</span><span class="sxs-lookup"><span data-stu-id="78b12-203">In this example, the zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![DNS-namnserver](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="78b12-205">Azure DNS skapar automatiskt auktoritativa NS-poster i din zon som innehåller de tilldelade namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="78b12-205">Azure DNS automatically creates authoritative NS records in your zone containing the assigned name servers.</span></span>  <span data-ttu-id="78b12-206">Om du vill se namnservernamnen via Azure PowerShell eller Azure CLI behöver du bara hämta dessa poster.</span><span class="sxs-lookup"><span data-stu-id="78b12-206">To see the name server names via Azure PowerShell or Azure CLI, you simply need to retrieve these records.</span></span>

### <a name="create-name-server-record-in-parent-zone"></a><span data-ttu-id="78b12-207">Skapa namnserverpost i överordnad zon</span><span class="sxs-lookup"><span data-stu-id="78b12-207">Create name server record in parent zone</span></span>

1. <span data-ttu-id="78b12-208">Navigera till DNS-zonen **contoso.net** i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="78b12-208">Navigate to the **contoso.net** DNS zone in the Azure portal.</span></span>
1. <span data-ttu-id="78b12-209">Klicka på **+ Postuppsättning**</span><span class="sxs-lookup"><span data-stu-id="78b12-209">Click **+ Record set**</span></span>
1. <span data-ttu-id="78b12-210">Ange följande värden på bladet **Lägg till uppsättning av poster** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="78b12-210">On the **Add record set** blade, enter the following values, then click **OK**:</span></span>

   | <span data-ttu-id="78b12-211">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="78b12-211">**Setting**</span></span> | <span data-ttu-id="78b12-212">**Värde**</span><span class="sxs-lookup"><span data-stu-id="78b12-212">**Value**</span></span> | <span data-ttu-id="78b12-213">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="78b12-213">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="78b12-214">**Namn**</span><span class="sxs-lookup"><span data-stu-id="78b12-214">**Name**</span></span>|<span data-ttu-id="78b12-215">partner</span><span class="sxs-lookup"><span data-stu-id="78b12-215">partners</span></span>|<span data-ttu-id="78b12-216">Namnet på den underordnade DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="78b12-216">The name of the child DNS zone</span></span>|
   |<span data-ttu-id="78b12-217">**Typ**</span><span class="sxs-lookup"><span data-stu-id="78b12-217">**Type**</span></span>|<span data-ttu-id="78b12-218">NS</span><span class="sxs-lookup"><span data-stu-id="78b12-218">NS</span></span>|<span data-ttu-id="78b12-219">Använd NS för namnserverposter.</span><span class="sxs-lookup"><span data-stu-id="78b12-219">Use NS for name server records.</span></span>|
   |<span data-ttu-id="78b12-220">**TTL**</span><span class="sxs-lookup"><span data-stu-id="78b12-220">**TTL**</span></span>|<span data-ttu-id="78b12-221">1</span><span class="sxs-lookup"><span data-stu-id="78b12-221">1</span></span>|<span data-ttu-id="78b12-222">Time to live.</span><span class="sxs-lookup"><span data-stu-id="78b12-222">Time to live.</span></span>|
   |<span data-ttu-id="78b12-223">**TTL-enhet**</span><span class="sxs-lookup"><span data-stu-id="78b12-223">**TTL unit**</span></span>|<span data-ttu-id="78b12-224">Timmar</span><span class="sxs-lookup"><span data-stu-id="78b12-224">Hours</span></span>|<span data-ttu-id="78b12-225">anger timmar som enheter för time to live</span><span class="sxs-lookup"><span data-stu-id="78b12-225">sets time to live unit to hours</span></span>|
   |<span data-ttu-id="78b12-226">**NAMNSERVER**</span><span class="sxs-lookup"><span data-stu-id="78b12-226">**NAME SERVER**</span></span>|<span data-ttu-id="78b12-227">{namnservrar från zonen partners.contoso.net}</span><span class="sxs-lookup"><span data-stu-id="78b12-227">{name servers from partners.contoso.net zone}</span></span>|<span data-ttu-id="78b12-228">Ange alla 4 namnservrarna från partners.contoso.net.zone.</span><span class="sxs-lookup"><span data-stu-id="78b12-228">Enter all 4 of the name servers from partners.contoso.net zone.</span></span> |

   ![DNS-namnserver](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a><span data-ttu-id="78b12-230">Delegera underdomäner i Azure DNS med andra verktyg</span><span class="sxs-lookup"><span data-stu-id="78b12-230">Delegating sub-domains in Azure DNS with other tools</span></span>

<span data-ttu-id="78b12-231">I följande exempel får du anvisningar för hur du delegerar underdomäner i Azure DNS med PowerShell och CLI:</span><span class="sxs-lookup"><span data-stu-id="78b12-231">The following examples provide the steps to delegate sub-domains in Azure DNS with PowerShell and CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="78b12-232">PowerShell</span><span class="sxs-lookup"><span data-stu-id="78b12-232">PowerShell</span></span>

<span data-ttu-id="78b12-233">Följande PowerShell-exempel demonstrerar hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="78b12-233">The following PowerShell example demonstrates how this works.</span></span> <span data-ttu-id="78b12-234">Du kan utföra samma steg från Azure Portal, eller via plattformsoberoende Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="78b12-234">The same steps can be executed via the Azure portal, or via the cross-platform Azure CLI.</span></span>

```powershell
# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

<span data-ttu-id="78b12-235">Använd `nslookup` för att kontrollera att allt är korrekt konfigurerat genom att leta upp SOA-posten för den underordnade zonen.</span><span class="sxs-lookup"><span data-stu-id="78b12-235">Use `nslookup` to verify that everything is set up correctly by looking up the SOA record of the child zone.</span></span>

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

#### <a name="azure-cli"></a><span data-ttu-id="78b12-236">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="78b12-236">Azure CLI</span></span>

```azurecli
#!/bin/bash

# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

<span data-ttu-id="78b12-237">Hämta namnservrarna för `partners.contoso.net`-zonen från utdata.</span><span class="sxs-lookup"><span data-stu-id="78b12-237">Retrieve the name servers for the `partners.contoso.net` zone from the output.</span></span>

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

<span data-ttu-id="78b12-238">Skapa postuppsättningen och NS-poster för varje namnserver.</span><span class="sxs-lookup"><span data-stu-id="78b12-238">Create the record set and NS records for each name server.</span></span>

```azurecli
#!/bin/bash

# Create the record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a><span data-ttu-id="78b12-239">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="78b12-239">Delete all resources</span></span>

<span data-ttu-id="78b12-240">Så här tar du bort alla resurser som skapats i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="78b12-240">To delete all resources created in this article, complete the following steps:</span></span>

1. <span data-ttu-id="78b12-241">Klicka på **Alla resurser** i rutan **Favoriter** i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="78b12-241">In the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="78b12-242">Klicka på resursgruppen **contosorg** på bladet Alla resurser.</span><span class="sxs-lookup"><span data-stu-id="78b12-242">Click the **contosorg** resource group in the All resources blade.</span></span> <span data-ttu-id="78b12-243">Om den prenumeration du valde redan har flera resurser kan du ange **contosorg** i rutan **Filtrera efter namn...**</span><span class="sxs-lookup"><span data-stu-id="78b12-243">If the subscription you selected already has several resources in it, you can enter **contosorg** in the **Filter by name…**</span></span> <span data-ttu-id="78b12-244">när du ska hitta resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="78b12-244">box to easily access the resource group.</span></span>
1. <span data-ttu-id="78b12-245">Klicka på knappen **Ta bort** i bladet **contosorg**.</span><span class="sxs-lookup"><span data-stu-id="78b12-245">In the **contosorg** blade, click the **Delete** button.</span></span>
1. <span data-ttu-id="78b12-246">Du måste ange namnet på resursgruppen i portalen som bekräftelse på att du vill ta bort den.</span><span class="sxs-lookup"><span data-stu-id="78b12-246">The portal requires you to type the name of the resource group to confirm that you want to delete it.</span></span> <span data-ttu-id="78b12-247">Skriv *contosorg* som resursgruppnamn och klicka sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="78b12-247">Type *contosorg* for the resource group name, then click **Delete**.</span></span> <span data-ttu-id="78b12-248">När du tar bort en resursgrupp så tas alla resurser i resursgruppen bort, så du måste alltid kontrollera innehållet i en resursgrupp innan du tar bort den.</span><span class="sxs-lookup"><span data-stu-id="78b12-248">Deleting a resource group deletes all resources within the resource group, so always be sure to confirm the contents of a resource group before deleting it.</span></span> <span data-ttu-id="78b12-249">Portalen tar bort alla resurser som finns i resursgruppen och sedan tas själva resursgruppen bort.</span><span class="sxs-lookup"><span data-stu-id="78b12-249">The portal deletes all resources contained within the resource group, then deletes the resource group itself.</span></span> <span data-ttu-id="78b12-250">Den här processen tar flera minuter.</span><span class="sxs-lookup"><span data-stu-id="78b12-250">This process takes several minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="78b12-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="78b12-251">Next steps</span></span>

[<span data-ttu-id="78b12-252">Hantera DNS-zoner</span><span class="sxs-lookup"><span data-stu-id="78b12-252">Manage DNS zones</span></span>](dns-operations-dnszones.md)

[<span data-ttu-id="78b12-253">Hantera DNS-poster</span><span class="sxs-lookup"><span data-stu-id="78b12-253">Manage DNS records</span></span>](dns-operations-recordsets.md)
