---
title: "aaaAzure DNS-vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor och svar om Azure DNS"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: jonatul
ms.openlocfilehash: 55006e9d8b311f1e94678eb9d35ce3448b936488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-faq"></a>Azure DNS vanliga frågor och svar

## <a name="about-azure-dns"></a>Om Azure DNS

### <a name="what-is-azure-dns"></a>Vad är Azure DNS?

hello Domain Name System eller DNS är översätter (eller lösa) en webbplats eller tjänst name tooits IP-adress. Azure DNS är värdtjänsten för DNS-domäner som tillhandahåller namnmatchning med hjälp av Microsoft Azure-infrastrukturen. Värd för dina domäner i Azure, kan du hantera din DNS hello poster med samma autentiseringsuppgifter, API: er, verktyg och fakturering som andra Azure-tjänster.

DNS-domäner i Azure DNS finns på Azures globalt nätverk av DNS-namnservrar. Vi använder Anycast nätverk så att varje DNS-frågan har besvarats av hello närmaste tillgängliga DNS-server. Detta ger både snabb prestanda och hög tillgänglighet för din domän.

hello Azure DNS-tjänsten är baserad på Azure Resource Manager. Därför fördelar från Resource Manager-funktioner, till exempel rollbaserad åtkomstkontroll, granskningsloggar och låsa resurs. Dina domäner och poster kan hanteras via hello Azure-portalen, Azure PowerShell-cmdlets och hello plattformsoberoende Azure CLI. Program som kräver automatisk DNS-hantering kan integreras med hello-tjänsten via hello REST-API och SDK: er.

### <a name="how-much-does-azure-dns-cost"></a>Hur mycket kostar Azure DNS?

hello Azure DNS faktureringsmodell som tillämpas baseras på hello antal DNS-zoner i Azure DNS och hello antal DNS-frågor som de får. Rabatter anges baserat på användning.

Mer information finns i hello [Azure DNS sida med priser](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-hello-sla-for-azure-dns"></a>Vad är hello SLA för Azure DNS?

Vi garanterar att giltiga DNS-förfrågningar får ett svar från minst en server i Azure DNS-namnet minst 99,99% hello tid.

Mer information finns i hello [Azure-serviceavtalet för DNS-sidan](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-hello-same-as-a-dns-domain"></a>Vad är en DNS-zon? Är den hello samma som en DNS-domän? 

En domän är ett unikt namn i hello domännamnssystemet, till exempel ”contoso.com”.

En DNS-zon är används toohost hello DNS-poster för en viss domän. Hello domänen ”contoso.com” kan till exempel innehålla flera DNS-poster, till exempel ”mail.contoso.com” (för en e-postserver) och ”www.contoso.com” (för en webbplats). Dessa finns i hello DNS-zonen ”contoso.com”.

Ett domännamn *bara ett namn*, medan en DNS-zon är en Dataresurs som innehåller hello DNS-poster för ett domännamn. Azure DNS kan du toohost en DNS-zon och hantera hello DNS-poster för en domän i Azure. Det ger också DNS-namnservrar tooanswer DNS-frågor från hello Internet.

### <a name="do-i-need-toopurchase-a-dns-domain-name-toouse-azure-dns"></a>Behöver jag toopurchase en DNS-domänen namnet toouse Azure DNS? 

Inte nödvändigtvis.

Du behöver inte toopurchase domän-toohost en DNS-zon i Azure DNS. Du kan skapa en DNS-zon när som helst utan äger hello domännamn. DNS-frågor för den här zonen löser endast om de är riktade toohello Azure DNS-namnservrar tilldelade toohello zon.

Du behöver toopurchase hello domännamnet om du vill toolink DNS-zonen i hello global DNS-hierarki – det här kan DNS-frågor från var som helst i hello world toofind DNS-zon och besvaras med DNS-poster.

## <a name="azure-dns-features"></a>Azure DNS-funktioner

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Azure DNS som har stöd för DNS-baserade routning eller slutpunkt trafikredundans?

DNS-baserade Routning och slutpunkten trafikredundans tillhandahålls av Azure Traffic Manager. Detta är en separat Azure-tjänst som kan användas tillsammans med Azure DNS. Mer information finns i hello [Traffic Manager-översikt](../traffic-manager/traffic-manager-overview.md).

Azure DNS stöder endast värd 'statiska' DNS-domäner, där varje DNS-fråga för att DNS-posten får hello alltid samma DNS-svar.

### <a name="does-azure-dns-support-domain-name-registration"></a>Stöder Azure DNS domännamnsregistrering?

Nej. Azure DNS stöder för närvarande inte köp av domännamn. Om du vill toopurchase domäner, måste toouse från tredje part domännamnsregistratorn. hello registrator debiterar vanligtvis en liten årlig avgift. hello domäner kan finnas i Azure DNS för för hantering av DNS-poster. Se [delegera en domän tooAzure DNS](dns-domain-delegation.md) mer information.

Detta är en funktion som vi följer upp på vår eftersläpning. Du kan använda webbplatsen feedback för[registrera ditt stöd för den här funktionen](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-private-domains"></a>Stöder Azure DNS ”privat” domäner?

Nej. Azure DNS stöder för närvarande endast Internet-riktade domäner.

Detta är en funktion som vi följer upp på vår eftersläpning. Du kan använda webbplatsen feedback för[registrera ditt stöd för den här funktionen](https://feedback.azure.com/forums/217313-networking/suggestions/10737696-enable-split-dns-for-providing-both-public-and-int).

Mer information om interna DNS-alternativ i Azure finns [namnmatchning för virtuella datorer och Rollinstanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="does-azure-dns-support-dnssec"></a>Stöder Azure DNS DNSSEC?

Nej. Azure DNS stöder för närvarande inte DNSSEC.

Detta är en funktion som vi följer upp på vår eftersläpning. Du kan använda webbplatsen feedback för[registrera ditt stöd för den här funktionen](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Stöder Azure DNS zonöverföringar (AXFR/IXFR)?

Nej. Azure DNS stöder för närvarande inte zonöverföringar. DNS-zoner kan vara [importeras till Azure DNS med hello Azure CLI](dns-import-export.md). DNS-poster kan sedan hanteras via hello [Azure DNS-hanteringsportalen](dns-operations-recordsets-portal.md), vår [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md), eller [ CLI verktyget](dns-operations-recordsets-cli.md).

Detta är en funktion som vi följer upp på vår eftersläpning. Du kan använda webbplatsen feedback för[registrera ditt stöd för den här funktionen](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Stöder Azure DNS URL omdirigeringar?

Nej. URL: en omdirigering tjänster är inte faktiskt en DNS-tjänst - de fungerar med hello http-nivå, i stället för hello DNS-nivå. Vissa DNS-providers toobundle en URL: en omdirigering tjänst som en del av deras övergripande erbjudande. Detta stöds inte för närvarande av Azure DNS.

Den här funktionen spåras på vår eftersläpning. Du kan använda webbplatsen feedback för[registrera ditt stöd för den här funktionen](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

## <a name="using-azure-dns"></a>Med hjälp av Azure DNS

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Kan jag Samkör en domän med hjälp av Azure DNS och en annan DNS-leverantör?

Ja. Azure DNS stöder samtidigt värd domäner med andra DNS-tjänster.

toodo så du behöver toomodify hello NS-poster för hello domän (som styr vilka providers får DNS-frågor för hello domän) toopoint toohello namnservrar för båda providers. Dessa NS-poster måste toobe ändras på 3 platser: hello annan provider i Azure DNS i och i hello överordnade zonen (normalt konfigureras via hello domännamnsregistratorn). Mer information om DNS-delegering finns [DNS-delegering i domänen](dns-domain-delegation.md).

Du måste dessutom tooensure att hello DNS-poster för hello domän är synkroniserade mellan både DNS-providers. Azure DNS stöder för närvarande inte DNS-zonöverföring. Du behöver toosynchronize DNS-poster med hjälp av antingen hello [Azure DNS-hanteringsportalen](dns-operations-recordsets-portal.md), vår [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md) , eller [CLI verktyget](dns-operations-recordsets-cli.md).

### <a name="do-i-have-toodelegate-my-domain-tooall-4-azure-dns-name-servers"></a>Måste jag toodelegate min domän tooall 4 Azure DNS-namnservrar?

Ja. Azure DNS tilldelar 4 namnservrar tooeach DNS-zonen för isolering av fel och ökad återhämtning. tooqualify för hello Azure DNS-SLA måste du toodelegate namnservrar för tooall 4 din domän.

### <a name="what-are-hello-usage-limits-for-azure-dns"></a>Vad är hello användningsbegränsningar för Azure DNS?

hello följer standardgränser som gäller när du använder Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Kan jag flytta en Azure DNS-zon mellan resursgrupper eller mellan prenumerationer?

Ja. DNS-zoner kan flyttas mellan resursgrupper eller mellan prenumerationer.

Det finns ingen inverkan på DNS-frågor när du flyttar en DNS-zon. hello namnservrar tilldelade toohello zon förblir hello bearbetas samma och DNS-frågor som vanligt i hela.

Mer information och anvisningar om hur toomove DNS-zoner finns [flytta resurser tooa ny resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-tootake-effect"></a>Hur lång tid tar det för DNS-ändringarna tootake effekt?

Den nya DNS-zoner och DNS-poster vanligtvis återspeglas i hello Azure DNS-namnservrar snabbt inom några sekunder.

Ändringar tooexisting DNS-poster kan ta lite längre tid, men fortfarande visas på hello Azure DNS-namnservrar inom 60 sekunder. DNS-cachelagring utanför Azure DNS (genom DNS-klienter och rekursiva DNS-matchare) kan då också påverka hello tid det tar för en DNS-ändringen toobe effektiva. Cachevaraktigheten kan kontrolleras med hello Time To Live (TTL)-egenskapen för varje uppsättning av poster.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Hur kan jag skydda DNS-zoner mot oavsiktlig borttagning?

Azure DNS hanteras med Azure Resource Manager och fördelar från hello åtkomst till kontrollfunktioner som Azure Resource Manager tillhandahåller. Rollbaserad åtkomstkontroll kan vara används toocontrol vilka användare som har läs- eller skrivbehörighet tooDNS zoner och postuppsättningar. Resurslås kan vara tillämpade tooprevent oavsiktlig ändring eller borttagning av DNS-zoner och postuppsättningar.

Mer information finns i [Skydda DNS-zoner och poster](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Hur ställer jag in SPF-poster i Azure DNS?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Hur ställer jag in ett internationellt domännamn (IDN) i Azure DNS?

Internationella domännamn (IDN) fungerar genom att koda varje DNS-namn med hjälp av '[punycode](https://en.wikipedia.org/wiki/Punycode)'. DNS-frågor görs med hjälp av namnen punycode-kodat.

Du kan konfigurera internationella domännamn (IDN) i Azure DNS med första konvertering hello zonnamnet eller postuppsättning namnet toopunycode. Azure DNS stöder för närvarande inte inbyggda konvertering till/från punycode.

## <a name="next-steps"></a>Nästa steg

[Lär dig mer om Azure DNS](dns-overview.md)
<br>
[Mer information om DNS-zoner och poster](dns-zones-records.md)
<br>
[Kom igång med Azure DNS](dns-getstarted-portal.md)

