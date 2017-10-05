---
title: "Azure DNS felsökningsguiden | Microsoft Docs"
description: "Felsökning av vanliga problem med Azure DNS"
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: 95b01dc3-ee69-4575-a259-4227131e4f9c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/20/2017
ms.author: jonatul
ms.openlocfilehash: 1d9bb681a864bdc3e5a2f9c9a531d9566b16ada4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-dns-troubleshooting-guide"></a><span data-ttu-id="90814-103">Azure DNS felsökningsguiden</span><span class="sxs-lookup"><span data-stu-id="90814-103">Azure DNS troubleshooting guide</span></span>

<span data-ttu-id="90814-104">Den här sidan innehåller felsökningsinformation för Azure DNS-frågor.</span><span class="sxs-lookup"><span data-stu-id="90814-104">This page provides troubleshooting information for common Azure DNS questions.</span></span>

<span data-ttu-id="90814-105">Om stegen inte löser problemet kan du också söka efter eller ett inlägg problemet i vår [community-support-forum på MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span><span class="sxs-lookup"><span data-stu-id="90814-105">If these steps don't resolve your issue, you can also search for or post your issue on our [community support forum on MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span></span> <span data-ttu-id="90814-106">Du kan också öppna en Azure-supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="90814-106">Alternatively, open an Azure support request.</span></span>


## <a name="i-cant-create-a-dns-zone"></a><span data-ttu-id="90814-107">Jag kan inte skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="90814-107">I can't create a DNS zone</span></span>

<span data-ttu-id="90814-108">Prova ett eller flera av följande steg för att lösa vanliga problem:</span><span class="sxs-lookup"><span data-stu-id="90814-108">To resolve common issues, try one or more of the following steps:</span></span>

1.  <span data-ttu-id="90814-109">Gå igenom granskningsloggar Azure DNS för att ta reda på orsaken till felet.</span><span class="sxs-lookup"><span data-stu-id="90814-109">Review the Azure DNS audit logs to determine the failure reason.</span></span>
2.  <span data-ttu-id="90814-110">Varje DNS-zonnamn måste vara unikt inom sin resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="90814-110">Each DNS zone name must be unique within its resource group.</span></span> <span data-ttu-id="90814-111">Det innebär att två DNS-zoner med samma namn inte kan dela en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="90814-111">That is, two DNS zones with the same name cannot share a resource group.</span></span> <span data-ttu-id="90814-112">Försök med ett annat zonnamn eller en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="90814-112">Try using a different zone name, or a different resource group.</span></span>
3.  <span data-ttu-id="90814-113">Ett felmeddelande om att ”Du har nått eller överskridit maxantalet zoner i prenumerationen {subscription id}” kanske visas.</span><span class="sxs-lookup"><span data-stu-id="90814-113">You may see an error "You have reached or exceeded the maximum number of zones in subscription {subscription id}."</span></span> <span data-ttu-id="90814-114">Då kan du antingen använda en annan Azure-prenumeration, ta bort vissa zoner eller kontakta Azure-supporten för att öka prenumerationsgränsen.</span><span class="sxs-lookup"><span data-stu-id="90814-114">Either use a different Azure subscription, delete some zones, or contact Azure Support to raise your subscription limit.</span></span>
4.  <span data-ttu-id="90814-115">Ett felmeddelande om att ”Zonen {zone name} inte är tillgänglig” kanske visas.</span><span class="sxs-lookup"><span data-stu-id="90814-115">You may see an error "The zone '{zone name}' is not available."</span></span> <span data-ttu-id="90814-116">Detta fel innebär att Azure DNS inte kunde allokera namnservrar för den här DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="90814-116">This error means that Azure DNS was unable to allocate name servers for this DNS zone.</span></span> <span data-ttu-id="90814-117">Försök med ett annat zonnamn.</span><span class="sxs-lookup"><span data-stu-id="90814-117">Try using a different zone name.</span></span> <span data-ttu-id="90814-118">Om du är ägare till domännamnet kan du även kontakta Azure-supporten som kan allokera namnservrar åt dig.</span><span class="sxs-lookup"><span data-stu-id="90814-118">Alternatively, if you are the domain name owner, contact Azure support, who can allocate name servers for you.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="90814-119">**Rekommenderade dokument**</span><span class="sxs-lookup"><span data-stu-id="90814-119">**Recommended documents**</span></span>

<span data-ttu-id="90814-120">[DNS-zoner och -poster](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="90814-120">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="90814-121">
[Skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="90814-121">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>

## <a name="i-cant-create-a-dns-record"></a><span data-ttu-id="90814-122">Jag kan inte skapa någon DNS-post</span><span class="sxs-lookup"><span data-stu-id="90814-122">I can't create a DNS record</span></span>

<span data-ttu-id="90814-123">Prova ett eller flera av följande steg för att lösa vanliga problem:</span><span class="sxs-lookup"><span data-stu-id="90814-123">To resolve common issues, try one or more of the following steps:</span></span>

1.  <span data-ttu-id="90814-124">Gå igenom granskningsloggar Azure DNS för att ta reda på orsaken till felet.</span><span class="sxs-lookup"><span data-stu-id="90814-124">Review the Azure DNS audit logs to determine the failure reason.</span></span>
2.  <span data-ttu-id="90814-125">Finns postuppsättningen redan?</span><span class="sxs-lookup"><span data-stu-id="90814-125">Does the record set exist already?</span></span>  <span data-ttu-id="90814-126">Azure DNS hanterar poster med post*uppsättningar*, vilket är en samling av poster med samma namn och av samma typ.</span><span class="sxs-lookup"><span data-stu-id="90814-126">Azure DNS manages records using record *sets*, which are the collection of records of the same name and the same type.</span></span> <span data-ttu-id="90814-127">Om det redan finns en post med samma namn och typ, redigerar du den befintliga postuppsättningen om du vill lägga till ännu en post.</span><span class="sxs-lookup"><span data-stu-id="90814-127">If a record with the same name and type already exists, then to add another such record you should edit the existing record set.</span></span>
3.  <span data-ttu-id="90814-128">Försöker du skapa en post i DNS-basdomänen (zonens ”rot”)?</span><span class="sxs-lookup"><span data-stu-id="90814-128">Are you trying to create a record at the DNS zone apex (the ‘root’ of the zone)?</span></span> <span data-ttu-id="90814-129">Om så är fallet använder DNS-konventionen tecknet ”@” som postens namn.</span><span class="sxs-lookup"><span data-stu-id="90814-129">If so, the DNS convention is to use the ‘@’ character as the record name.</span></span> <span data-ttu-id="90814-130">Observera också att DNS-standarden inte tillåter CNAME-poster i basdomänen.</span><span class="sxs-lookup"><span data-stu-id="90814-130">Also note that the DNS standards do not permit CNAME records at the zone apex.</span></span>
4.  <span data-ttu-id="90814-131">Har du en CNAME-konflikt?</span><span class="sxs-lookup"><span data-stu-id="90814-131">Do you have a CNAME conflict?</span></span>  <span data-ttu-id="90814-132">DNS-standarden tillåter inte en CNAME-post med samma namn som en post av någon annan typ.</span><span class="sxs-lookup"><span data-stu-id="90814-132">The DNS standards do not allow a CNAME record with the same name as a record of any other type.</span></span> <span data-ttu-id="90814-133">Om du har ett befintligt CNAME kommer du att misslyckas om du försöker skapa en post med samma namn och av en annan typ.</span><span class="sxs-lookup"><span data-stu-id="90814-133">If you have an existing CNAME, creating a record with the same name of a different type fails.</span></span>  <span data-ttu-id="90814-134">På samma sätt går det inte att skapa ett CNAME om namnet matchar en befintlig post av en annan typ.</span><span class="sxs-lookup"><span data-stu-id="90814-134">Likewise, creating a CNAME fails if the name matches an existing record of a different type.</span></span> <span data-ttu-id="90814-135">Lös konflikten genom att ta bort den andra posten eller välja ett annat postnamn.</span><span class="sxs-lookup"><span data-stu-id="90814-135">Remove the conflict by removing the other record or choosing a different record name.</span></span>
5.  <span data-ttu-id="90814-136">Har du nått gränsen för antal postuppsättningar som tillåts i en DNS-zon?</span><span class="sxs-lookup"><span data-stu-id="90814-136">Have you reached the limit on the number of record sets permitted in a DNS zone?</span></span> <span data-ttu-id="90814-137">Det aktuella antalet postuppsättningar och det maximala antalet postuppsättningar visas i Azure Portal under ”Egenskaper” för zonen.</span><span class="sxs-lookup"><span data-stu-id="90814-137">The current number of record sets and the maximum number of record sets are shown in the Azure portal, under the 'Properties' for the zone.</span></span> <span data-ttu-id="90814-138">Om du har uppnått gränsen kan du antingen ta bort vissa postuppsättningar eller kontakta Azure-supporten om du vill öka gränsen för den här zonen. Försök sedan igen.</span><span class="sxs-lookup"><span data-stu-id="90814-138">If you have reached this limit, then either delete some record sets or contact Azure Support to raise your record set limit for this zone, then try again.</span></span> 


### <a name="recommended-documents"></a><span data-ttu-id="90814-139">**Rekommenderade dokument**</span><span class="sxs-lookup"><span data-stu-id="90814-139">**Recommended documents**</span></span>

<span data-ttu-id="90814-140">[DNS-zoner och -poster](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="90814-140">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="90814-141">
[Skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="90814-141">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>



## <a name="i-cant-resolve-my-dns-record"></a><span data-ttu-id="90814-142">Jag kan inte matcha min DNS-post</span><span class="sxs-lookup"><span data-stu-id="90814-142">I can't resolve my DNS record</span></span>

<span data-ttu-id="90814-143">DNS-namnmatchningen är en process i flera steg som kan misslyckas av flera orsaker.</span><span class="sxs-lookup"><span data-stu-id="90814-143">DNS name resolution is a multi-step process, which can fail for many reasons.</span></span> <span data-ttu-id="90814-144">Följande steg hjälper dig att undersöka varför DNS-matchningen inte fungerar för en DNS-post i en zon som finns i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="90814-144">The following steps help you investigate why DNS resolution is failing for a DNS record in a zone hosted in Azure DNS.</span></span>

1.  <span data-ttu-id="90814-145">Kontrollera att DNS-posterna har konfigurerats korrekt i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="90814-145">Confirm that the DNS records have been configured correctly in Azure DNS.</span></span> <span data-ttu-id="90814-146">Granska DNS-posterna i Azure Portal och kontrollera att zonnamn, postnamn samt posttyp är korrekta.</span><span class="sxs-lookup"><span data-stu-id="90814-146">Review the DNS records in the Azure portal, checking that the zone name, record name, and record type are correct.</span></span>
2.  <span data-ttu-id="90814-147">Kontrollera att DNS-posterna matchas korrekt på Azure DNS-namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="90814-147">Confirm that the DNS records resolve correctly on the Azure DNS name servers.</span></span>
    - <span data-ttu-id="90814-148">Om du skapar DNS-frågor från din lokala dator, kan cachelagrade resultat visas som inte motsvarar det aktuella tillståndet för namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="90814-148">If you make DNS queries from your local PC, you may see cached results that don’t reflect the current state of the name servers.</span></span>  <span data-ttu-id="90814-149">Företagsnätverk använder dessutom ofta DNS-proxyservrar, vilket förhindrar att DNS-frågor dirigeras till specifika namnservrar.</span><span class="sxs-lookup"><span data-stu-id="90814-149">Also, corporate networks often use DNS proxy servers, which prevent DNS queries from being directed to specific name servers.</span></span>  <span data-ttu-id="90814-150">Undvik dessa problem genom att använda en webbaserad namnmatchningstjänst som t.ex. [digwebinterface](http://digwebinterface.com).</span><span class="sxs-lookup"><span data-stu-id="90814-150">To avoid these problems, use a web-based name resolution service such as [digwebinterface](http://digwebinterface.com).</span></span>
    - <span data-ttu-id="90814-151">Se till att ange rätt namnservrar för DNS-zonen, enligt det som visas i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="90814-151">Be sure to specify the correct name servers for your DNS zone, as shown in the Azure portal.</span></span>
    - <span data-ttu-id="90814-152">Kontrollera att DNS-namnet är korrekt (du måste ange det fullständiga namnet, inklusive zonnamnet) och att posttypen stämmer</span><span class="sxs-lookup"><span data-stu-id="90814-152">Check that the DNS name is correct (you have to specify the fully qualified name, including the zone name) and the record type is correct</span></span>
3.  <span data-ttu-id="90814-153">Kontrollera att DNS-domännamnet har [delegerats till Azure DNS-namnservrarna](dns-domain-delegation.md) på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="90814-153">Confirm that the DNS domain name has been correctly [delegated to the Azure DNS name servers](dns-domain-delegation.md).</span></span> <span data-ttu-id="90814-154">Det finns [många tredjeparts webbplatser som erbjuder DNS-delegeringsverifiering](https://www.bing.com/search?q=dns+check+tool).</span><span class="sxs-lookup"><span data-stu-id="90814-154">There are a [many 3rd-party web sites that offer DNS delegation validation](https://www.bing.com/search?q=dns+check+tool).</span></span> <span data-ttu-id="90814-155">Det här testet är ett *zon*delegeringstest, så du bör bara ange DNS-zonens namn och inte det fullständiga postnamnet.</span><span class="sxs-lookup"><span data-stu-id="90814-155">This test is a *zone* delegation test, so you should only enter the DNS zone name and not the fully qualified record name.</span></span>
4.  <span data-ttu-id="90814-156">När du har utfört ovanstående bör DNS-posten matchas korrekt.</span><span class="sxs-lookup"><span data-stu-id="90814-156">Having completed the above, your DNS record should now resolve correctly.</span></span> <span data-ttu-id="90814-157">Om du vill kontrollera detta använder du [digwebinterface](http://digwebinterface.com) igen, men nu med standardinställningarna för namnservern.</span><span class="sxs-lookup"><span data-stu-id="90814-157">To verify, you can again use [digwebinterface](http://digwebinterface.com), this time using the default name server settings.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="90814-158">**Rekommenderade dokument**</span><span class="sxs-lookup"><span data-stu-id="90814-158">**Recommended documents**</span></span>

[<span data-ttu-id="90814-159">Delegera en domän till Azure DNS</span><span class="sxs-lookup"><span data-stu-id="90814-159">Delegate a domain to Azure DNS</span></span>](dns-domain-delegation.md)



## <a name="how-do-i-specify-the-service-and-protocol-for-an-srv-record"></a><span data-ttu-id="90814-160">Hur anger jag ”tjänst” och ”protokoll” för en SRV-post?</span><span class="sxs-lookup"><span data-stu-id="90814-160">How do I specify the ‘service’ and ‘protocol’ for an SRV record?</span></span>

<span data-ttu-id="90814-161">Azure DNS hanterar DNS-poster som postuppsättningar, vilket är en samling av poster med samma namn och av samma typ.</span><span class="sxs-lookup"><span data-stu-id="90814-161">Azure DNS manages DNS records as record sets—the collection of records with the same name and the same type.</span></span> <span data-ttu-id="90814-162">För en SRV-postuppsättning måste ”tjänst” och ”protokoll” anges som en del av namnet på postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="90814-162">For an SRV record set, the 'service' and 'protocol' need to be specified as part of the record set name.</span></span> <span data-ttu-id="90814-163">Andra SRV-parametrar (”prioritet”, ”vikt”, ”port” och ”mål”) anges separat för varje post i postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="90814-163">The other SRV parameters ('priority', 'weight', 'port' and 'target') are specified separately for each record in the record set.</span></span>

<span data-ttu-id="90814-164">Exempel på SRV-postnamn (tjänstnamn ”sip”, protokoll ”tcp”):</span><span class="sxs-lookup"><span data-stu-id="90814-164">Example SRV record names (service name 'sip', protocol 'tcp'):</span></span>

- <span data-ttu-id="90814-165">\_sip.\_tcp (skapar en postuppsättning i basdomänen)</span><span class="sxs-lookup"><span data-stu-id="90814-165">\_sip.\_tcp (creates a record set at the zone apex)</span></span>
- <span data-ttu-id="90814-166">\_sip.\_tcp.sipservice (skapar en postuppsättning med namnet ”sipservice”)</span><span class="sxs-lookup"><span data-stu-id="90814-166">\_sip.\_tcp.sipservice (creates a record set named 'sipservice')</span></span>

### <a name="recommended-documents"></a><span data-ttu-id="90814-167">**Rekommenderade dokument**</span><span class="sxs-lookup"><span data-stu-id="90814-167">**Recommended documents**</span></span>

<span data-ttu-id="90814-168">[DNS-zoner och -poster](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="90814-168">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="90814-169">
[Skapa DNS-postuppsättningar och poster med hjälp av Azure-portalen](dns-getstarted-create-recordset-portal.md)
</span><span class="sxs-lookup"><span data-stu-id="90814-169">
[Create DNS record sets and records by using the Azure portal](dns-getstarted-create-recordset-portal.md)
</span></span><br><span data-ttu-id="90814-170">
[SRV-posttyp (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span><span class="sxs-lookup"><span data-stu-id="90814-170">
[SRV record type (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span></span>


## <a name="next-steps"></a><span data-ttu-id="90814-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="90814-171">Next steps</span></span>

* <span data-ttu-id="90814-172">Lär dig mer om [Azure DNS-zoner och poster](dns-zones-records.md)</span><span class="sxs-lookup"><span data-stu-id="90814-172">Learn about [Azure DNS zones and records](dns-zones-records.md)</span></span>
* <span data-ttu-id="90814-173">Om du vill börja använda Azure DNS, lär du dig hur du [skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md) och [skapa DNS-poster](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="90814-173">To start using Azure DNS, learn how to [create a DNS zone](dns-getstarted-create-dnszone-portal.md) and [create DNS records](dns-getstarted-create-recordset-portal.md).</span></span>
* <span data-ttu-id="90814-174">Om du vill migrera en befintlig DNS-zon, lär du dig hur du [importera och exportera en DNS-zonfilen](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="90814-174">To migrate an existing DNS zone, learn how to [import and export a DNS zone file](dns-import-export.md).</span></span>

