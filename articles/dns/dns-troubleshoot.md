---
title: "aaaAzure DNS felsökningsguiden | Microsoft Docs"
description: Hur tootroubleshoot vanliga problem med Azure DNS
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
ms.openlocfilehash: 944aa1811c980063f739268cd2c79b647b2754a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-troubleshooting-guide"></a><span data-ttu-id="7e8d4-103">Azure DNS felsökningsguiden</span><span class="sxs-lookup"><span data-stu-id="7e8d4-103">Azure DNS troubleshooting guide</span></span>

<span data-ttu-id="7e8d4-104">Den här sidan innehåller felsökningsinformation för Azure DNS-frågor.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-104">This page provides troubleshooting information for common Azure DNS questions.</span></span>

<span data-ttu-id="7e8d4-105">Om stegen inte löser problemet kan du också söka efter eller ett inlägg problemet i vår [community-support-forum på MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-105">If these steps don't resolve your issue, you can also search for or post your issue on our [community support forum on MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span></span> <span data-ttu-id="7e8d4-106">Du kan också öppna en Azure-supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-106">Alternatively, open an Azure support request.</span></span>


## <a name="i-cant-create-a-dns-zone"></a><span data-ttu-id="7e8d4-107">Jag kan inte skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="7e8d4-107">I can't create a DNS zone</span></span>

<span data-ttu-id="7e8d4-108">tooresolve vanliga problem, försök med en eller flera av följande hello:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-108">tooresolve common issues, try one or more of hello following steps:</span></span>

1.  <span data-ttu-id="7e8d4-109">Granska hello Azure DNS granska loggar toodetermine hello orsaken till felet.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-109">Review hello Azure DNS audit logs toodetermine hello failure reason.</span></span>
2.  <span data-ttu-id="7e8d4-110">Varje DNS-zonnamn måste vara unikt inom sin resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-110">Each DNS zone name must be unique within its resource group.</span></span> <span data-ttu-id="7e8d4-111">Det vill säga två DNS-zoner med hello samma namn kan inte dela en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-111">That is, two DNS zones with hello same name cannot share a resource group.</span></span> <span data-ttu-id="7e8d4-112">Försök med ett annat zonnamn eller en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-112">Try using a different zone name, or a different resource group.</span></span>
3.  <span data-ttu-id="7e8d4-113">Du kan se ett fel ”du har nått eller överskridit hello maxantalet zoner i prenumerationen {prenumerations-id}”.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-113">You may see an error "You have reached or exceeded hello maximum number of zones in subscription {subscription id}."</span></span> <span data-ttu-id="7e8d4-114">Antingen använder en annan Azure-prenumeration, ta bort vissa zoner eller kontakta Azure-supporten tooraise prenumerationsgränsen för.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-114">Either use a different Azure subscription, delete some zones, or contact Azure Support tooraise your subscription limit.</span></span>
4.  <span data-ttu-id="7e8d4-115">Du kan se ett fel ”hello-zonen {zonen name}' är inte tillgänglig”.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-115">You may see an error "hello zone '{zone name}' is not available."</span></span> <span data-ttu-id="7e8d4-116">Felet innebär att Azure DNS användes tooallocate namnservrar för denna DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-116">This error means that Azure DNS was unable tooallocate name servers for this DNS zone.</span></span> <span data-ttu-id="7e8d4-117">Försök med ett annat zonnamn.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-117">Try using a different zone name.</span></span> <span data-ttu-id="7e8d4-118">Du kan också om du är hello domain name ägare kontakta Azure-supporten, som kan allokera namnservrar för dig.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-118">Alternatively, if you are hello domain name owner, contact Azure support, who can allocate name servers for you.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="7e8d4-119">**Rekommenderade dokument**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-119">**Recommended documents**</span></span>

<span data-ttu-id="7e8d4-120">[DNS-zoner och -poster](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="7e8d4-120">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="7e8d4-121">
[Skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7e8d4-121">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>

## <a name="i-cant-create-a-dns-record"></a><span data-ttu-id="7e8d4-122">Jag kan inte skapa någon DNS-post</span><span class="sxs-lookup"><span data-stu-id="7e8d4-122">I can't create a DNS record</span></span>

<span data-ttu-id="7e8d4-123">tooresolve vanliga problem, försök med en eller flera av följande hello:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-123">tooresolve common issues, try one or more of hello following steps:</span></span>

1.  <span data-ttu-id="7e8d4-124">Granska hello Azure DNS granska loggar toodetermine hello orsaken till felet.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-124">Review hello Azure DNS audit logs toodetermine hello failure reason.</span></span>
2.  <span data-ttu-id="7e8d4-125">Hello postuppsättning finns det redan?</span><span class="sxs-lookup"><span data-stu-id="7e8d4-125">Does hello record set exist already?</span></span>  <span data-ttu-id="7e8d4-126">Azure DNS hanterar poster med hjälp av posten *anger*, som är hello uppsättning poster av hello samma namn och hello samma typ.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-126">Azure DNS manages records using record *sets*, which are hello collection of records of hello same name and hello same type.</span></span> <span data-ttu-id="7e8d4-127">Om en post med hello samma namn och Skriv redan finns tooadd en annan post du ska redigera hello befintlig post anger.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-127">If a record with hello same name and type already exists, then tooadd another such record you should edit hello existing record set.</span></span>
3.  <span data-ttu-id="7e8d4-128">Försöker toocreate en post på hello DNS-zonens apex (hello-rot' hello zonen)?</span><span class="sxs-lookup"><span data-stu-id="7e8d4-128">Are you trying toocreate a record at hello DNS zone apex (hello ‘root’ of hello zone)?</span></span> <span data-ttu-id="7e8d4-129">Om så hello DNS-konventionen är toouse hello ”@-tecknet som hello postens namn.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-129">If so, hello DNS convention is toouse hello ‘@’ character as hello record name.</span></span> <span data-ttu-id="7e8d4-130">Observera också att hello DNS-standarden inte tillåter CNAME-poster på hello zonens apex.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-130">Also note that hello DNS standards do not permit CNAME records at hello zone apex.</span></span>
4.  <span data-ttu-id="7e8d4-131">Har du en CNAME-konflikt?</span><span class="sxs-lookup"><span data-stu-id="7e8d4-131">Do you have a CNAME conflict?</span></span>  <span data-ttu-id="7e8d4-132">hello DNS-standarden tillåter inte en CNAME-post med hello samma namn som en post för andra typer.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-132">hello DNS standards do not allow a CNAME record with hello same name as a record of any other type.</span></span> <span data-ttu-id="7e8d4-133">Om du har en befintlig CNAME, skapa en post med samma namn som en annan typ misslyckas hello.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-133">If you have an existing CNAME, creating a record with hello same name of a different type fails.</span></span>  <span data-ttu-id="7e8d4-134">På samma sätt misslyckas att skapa en CNAME-post om hello namnet matchar en befintlig post av en annan typ.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-134">Likewise, creating a CNAME fails if hello name matches an existing record of a different type.</span></span> <span data-ttu-id="7e8d4-135">Ta bort hello konflikten genom att ta bort hello andra poster eller välja en annan postnamnet.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-135">Remove hello conflict by removing hello other record or choosing a different record name.</span></span>
5.  <span data-ttu-id="7e8d4-136">Har du nått hello begränsning på hello postuppsättningar som tillåts i en DNS-zon. hello aktuellt antal uppsättningar av poster och hello maximalt antal uppsättningar av poster visas i hello Azure-portalen under hello ”egenskaper” för hello zonen.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-136">Have you reached hello limit on hello number of record sets permitted in a DNS zone? hello current number of record sets and hello maximum number of record sets are shown in hello Azure portal, under hello 'Properties' for hello zone.</span></span> <span data-ttu-id="7e8d4-137">Om du har nått den här gränsen, antingen ta bort vissa postuppsättningar eller kontakta Azure-supporten tooraise postuppsättning gränsen för den här zonen, och försök sedan igen.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-137">If you have reached this limit, then either delete some record sets or contact Azure Support tooraise your record set limit for this zone, then try again.</span></span> 


### <a name="recommended-documents"></a><span data-ttu-id="7e8d4-138">**Rekommenderade dokument**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-138">**Recommended documents**</span></span>

<span data-ttu-id="7e8d4-139">[DNS-zoner och -poster](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="7e8d4-139">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="7e8d4-140">
[Skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7e8d4-140">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>



## <a name="i-cant-resolve-my-dns-record"></a><span data-ttu-id="7e8d4-141">Jag kan inte matcha min DNS-post</span><span class="sxs-lookup"><span data-stu-id="7e8d4-141">I can't resolve my DNS record</span></span>

<span data-ttu-id="7e8d4-142">DNS-namnmatchningen är en process i flera steg som kan misslyckas av flera orsaker.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-142">DNS name resolution is a multi-step process, which can fail for many reasons.</span></span> <span data-ttu-id="7e8d4-143">hello följande steg när du undersöka varför inte DNS-matchning för en DNS-post i en zon som finns i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-143">hello following steps help you investigate why DNS resolution is failing for a DNS record in a zone hosted in Azure DNS.</span></span>

1.  <span data-ttu-id="7e8d4-144">Bekräfta att hello DNS-poster har konfigurerats korrekt i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-144">Confirm that hello DNS records have been configured correctly in Azure DNS.</span></span> <span data-ttu-id="7e8d4-145">Granska hello DNS-poster i hello Azure-portalen kontrollerar att hello zonnamnet och postnamnet posttyp är korrekta.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-145">Review hello DNS records in hello Azure portal, checking that hello zone name, record name, and record type are correct.</span></span>
2.  <span data-ttu-id="7e8d4-146">Bekräfta att hello DNS-poster ska matcha på hello Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-146">Confirm that hello DNS records resolve correctly on hello Azure DNS name servers.</span></span>
    - <span data-ttu-id="7e8d4-147">Om du gör DNS-frågor från din lokala dator, kan det hända att cachelagrade resultaten inte avspeglar hello hello namnservrar aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-147">If you make DNS queries from your local PC, you may see cached results that don’t reflect hello current state of hello name servers.</span></span>  <span data-ttu-id="7e8d4-148">Företagsnätverk ofta använda DNS-proxyservrar, vilket gör att DNS-frågor inte dirigeras toospecific namnservrar.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-148">Also, corporate networks often use DNS proxy servers, which prevent DNS queries from being directed toospecific name servers.</span></span>  <span data-ttu-id="7e8d4-149">tooavoid dessa problem använder en webbaserad namnmatchningstjänst som [digwebinterface](http://digwebinterface.com).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-149">tooavoid these problems, use a web-based name resolution service such as [digwebinterface](http://digwebinterface.com).</span></span>
    - <span data-ttu-id="7e8d4-150">Vara säker på att toospecify hello rätt namnservrar för DNS-zonen som visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-150">Be sure toospecify hello correct name servers for your DNS zone, as shown in hello Azure portal.</span></span>
    - <span data-ttu-id="7e8d4-151">Kontrollera att hello DNS-namnet är rätt (du har toospecify hello fullständigt kvalificerade namn, inklusive hello zonnamnet) och hello posttyp är korrekt</span><span class="sxs-lookup"><span data-stu-id="7e8d4-151">Check that hello DNS name is correct (you have toospecify hello fully qualified name, including hello zone name) and hello record type is correct</span></span>
3.  <span data-ttu-id="7e8d4-152">Bekräfta att hello DNS-namnet har korrekt [delegerad toohello Azure DNS-namnservrar](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-152">Confirm that hello DNS domain name has been correctly [delegated toohello Azure DNS name servers](dns-domain-delegation.md).</span></span> <span data-ttu-id="7e8d4-153">Det finns [många tredjeparts webbplatser som erbjuder DNS-delegeringsverifiering](https://www.bing.com/search?q=dns+check+tool).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-153">There are a [many 3rd-party web sites that offer DNS delegation validation](https://www.bing.com/search?q=dns+check+tool).</span></span> <span data-ttu-id="7e8d4-154">Det här testet är en *zon* delegeringstest, så bör du bara ange hello DNS-zonnamnet och inte hello fullständigt kvalificerade postnamnet.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-154">This test is a *zone* delegation test, so you should only enter hello DNS zone name and not hello fully qualified record name.</span></span>
4.  <span data-ttu-id="7e8d4-155">Efter att ha utfört hello ovan, ska DNS-post nu matcha korrekt.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-155">Having completed hello above, your DNS record should now resolve correctly.</span></span> <span data-ttu-id="7e8d4-156">tooverify, kan du igen använda [digwebinterface](http://digwebinterface.com), nu med hello standardinställningar namn på servern.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-156">tooverify, you can again use [digwebinterface](http://digwebinterface.com), this time using hello default name server settings.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="7e8d4-157">**Rekommenderade dokument**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-157">**Recommended documents**</span></span>

[<span data-ttu-id="7e8d4-158">Delegera en domän tooAzure DNS</span><span class="sxs-lookup"><span data-stu-id="7e8d4-158">Delegate a domain tooAzure DNS</span></span>](dns-domain-delegation.md)



## <a name="how-do-i-specify-hello-service-and-protocol-for-an-srv-record"></a><span data-ttu-id="7e8d4-159">Hur jag ange hello ”tjänst” och ”protokoll” för en SRV-post?</span><span class="sxs-lookup"><span data-stu-id="7e8d4-159">How do I specify hello ‘service’ and ‘protocol’ for an SRV record?</span></span>

<span data-ttu-id="7e8d4-160">Azure DNS hanterar DNS-poster som postuppsättningar – hello samling poster med hello samma namn och hello samma typ.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-160">Azure DNS manages DNS records as record sets—hello collection of records with hello same name and hello same type.</span></span> <span data-ttu-id="7e8d4-161">Måste anges som del av hello postuppsättningsnamnet toobe för en SRV-postuppsättning hello ”tjänst” och ”protokoll”.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-161">For an SRV record set, hello 'service' and 'protocol' need toobe specified as part of hello record set name.</span></span> <span data-ttu-id="7e8d4-162">hello SRV parametrar ('priority', 'vikt', '-port' och 'target') anges separat för varje post i hello postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-162">hello other SRV parameters ('priority', 'weight', 'port' and 'target') are specified separately for each record in hello record set.</span></span>

<span data-ttu-id="7e8d4-163">Exempel på SRV-postnamn (tjänstnamn ”sip”, protokoll ”tcp”):</span><span class="sxs-lookup"><span data-stu-id="7e8d4-163">Example SRV record names (service name 'sip', protocol 'tcp'):</span></span>

- <span data-ttu-id="7e8d4-164">\_SIP. \_tcp (skapar en postuppsättning på zonens apex hello)</span><span class="sxs-lookup"><span data-stu-id="7e8d4-164">\_sip.\_tcp (creates a record set at hello zone apex)</span></span>
- <span data-ttu-id="7e8d4-165">\_sip.\_tcp.sipservice (skapar en postuppsättning med namnet ”sipservice”)</span><span class="sxs-lookup"><span data-stu-id="7e8d4-165">\_sip.\_tcp.sipservice (creates a record set named 'sipservice')</span></span>

### <a name="recommended-documents"></a><span data-ttu-id="7e8d4-166">**Rekommenderade dokument**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-166">**Recommended documents**</span></span>

<span data-ttu-id="7e8d4-167">[DNS-zoner och -poster](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="7e8d4-167">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="7e8d4-168">
[Skapa uppsättningar av DNS-poster och poster med hjälp av hello Azure-portalen](dns-getstarted-create-recordset-portal.md)
</span><span class="sxs-lookup"><span data-stu-id="7e8d4-168">
[Create DNS record sets and records by using hello Azure portal](dns-getstarted-create-recordset-portal.md)
</span></span><br><span data-ttu-id="7e8d4-169">
[SRV-posttyp (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span><span class="sxs-lookup"><span data-stu-id="7e8d4-169">
[SRV record type (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span></span>


## <a name="next-steps"></a><span data-ttu-id="7e8d4-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e8d4-170">Next steps</span></span>

* <span data-ttu-id="7e8d4-171">Lär dig mer om [Azure DNS-zoner och poster](dns-zones-records.md)</span><span class="sxs-lookup"><span data-stu-id="7e8d4-171">Learn about [Azure DNS zones and records](dns-zones-records.md)</span></span>
* <span data-ttu-id="7e8d4-172">toostart med hjälp av Azure DNS Lär dig hur för[skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md) och [skapa DNS-poster](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-172">toostart using Azure DNS, learn how too[create a DNS zone](dns-getstarted-create-dnszone-portal.md) and [create DNS records](dns-getstarted-create-recordset-portal.md).</span></span>
* <span data-ttu-id="7e8d4-173">toomigrate en befintlig DNS-zon Lär dig hur för[importera och exportera en DNS-zonfilen](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-173">toomigrate an existing DNS zone, learn how too[import and export a DNS zone file](dns-import-export.md).</span></span>

