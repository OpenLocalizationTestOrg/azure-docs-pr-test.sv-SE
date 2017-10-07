---
title: "Översikt över aaaAzure DNS-delegering | Microsoft Docs"
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
ms.openlocfilehash: eaf2d2e345004b4d631e8d81d548b8ca23277d05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delegation-of-dns-zones-with-azure-dns"></a>Delegering av DNS-zoner med Azure DNS

Azure DNS kan du toohost en DNS-zon och hantera hello DNS-poster för en domän i Azure. För DNS-frågor för en domän tooreach Azure DNS hello domän har toobe delegerad tooAzure DNS från hello överordnad domän. Kom ihåg Azure DNS är inte hello domänregistrator. Den här artikeln förklarar hur domän-delegering fungerar och hur toodelegate domäner tooAzure DNS.

## <a name="how-dns-delegation-works"></a>Så här fungerar DNS-delegering

### <a name="domains-and-zones"></a>Domäner och zoner

hello Domain Name System är en hierarki av domäner. hello hierarki börjar från hello '' rotdomänen, vars namn är helt enkelt ”**.**'.  Under detta kommer toppdomänerna, till exempel ”com”, ”net”, ”org”, ”se” eller ”uk”.  Under dessa toppnivådomäner finns domäner på den andra nivån, till exempel ”org.se” eller ”co.uk”.  Och så vidare. hello domäner i hello DNS-hierarki finns med hjälp av separata DNS-zoner. Dessa zoner distribueras globalt, DNS-namnservrar runt hälsningsmeddelande som värd.

**DNS-zonen** -en domän är ett unikt namn i hello Domain Name System, till exempel ”contoso.com”. En DNS-zon är används toohost hello DNS-poster för en viss domän. Hello domänen ”contoso.com” kan till exempel innehålla flera DNS-poster, till exempel ”mail.contoso.com” (för en e-postserver) och ”www.contoso.com” (för en webbplats).

**Domänregistrator** – En domänregistrator är ett företag som kan tillhandahålla Internetdomännamn. De kontrollera om hello Internet-domän som du vill toouse är tillgänglig och att du toopurchase den. När hello domännamn är registrerad, är du hello juridiska ägare för hello domännamn. Om du redan har en Internet-domän, använder du hello aktuella domän registrator toodelegate tooAzure DNS.

toofind mer information om vem som äger ett visst domännamn eller information om hur toobuy en domän finns [Internet-domänhantering i Azure AD](https://msdn.microsoft.com/library/azure/hh969248.aspx).

### <a name="resolution-and-delegation"></a>Matchning och delegering

Det finns två typer av DNS-servrar:

* En *auktoritativ* DNS-server är värd för DNS-zoner. Den svarar på DNS-frågor om poster i just dessa zoner.
* En *rekursiv* DNS-server är inte värd för DNS-zoner. Alla DNS-frågor svarar genom att anropa auktoritära DNS-servrar toogather hello data som behövs.

Azure DNS tillhandahåller en auktoritativ DNS-tjänst.  Någon rekursiv DNS-tjänst tillhandahålls inte. Molntjänster och virtuella datorer i Azure är automatiskt konfigurerade toouse en rekursiv DNS-tjänst som tillhandahålls separat som en del av Azures infrastruktur. Mer information om hur toochange dessa DNS-inställningar, se [namnmatchning i Azure](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

DNS-klienter på datorer eller mobila enheter anropa vanligtvis en rekursiv DNS-server tooperform DNS-frågor måste hello klientprogram.

När en rekursiv DNS-server tar emot en fråga för en DNS-post, till exempel ”www.contoso.com”, måste det först toofind hello name server värd hello zon för hello ”contoso.com” domän. toofind hello name server startas på hello rotservrar namn, och därifrån hittar hello namnservrar värd hello 'com-zon. Sedan frågar hello 'com-namnet toofind hello namnservrar där hello ”contoso.com” zonen.  Slutligen är det kan tooquery dessa namnservrar för ”www.contoso.com”.

Den här proceduren kallas lösa hello DNS-namn. Strikt sett DNS-matchning inkluderar ytterligare steg, till exempel följande CNAME-resursposter, men som inte är viktiga toounderstanding hur DNS-delegering fungerar.

Hur en överordnad zon 'pekar' toohello namnservrar för en underordnad zon? Detta sker med hjälp av en särskild typ av DNS-post kallad en NS-post (NS står för namnserver). Hej rotzon innehåller NS-poster för 'com- och visar hello namnservrar för hello 'com-zon. I sin tur innehåller hello 'com-zonen NS-poster för ”contoso.com”, som visar hello namnservrar för hello ”contoso.com” zonen. Konfigurera hello NS-poster för en underordnad zon i en överordnad zon kallas delegerat hello-domän.

hello följande bild visar ett exempel DNS-fråga. Hej contoso.net och partners.contoso.net är Azure DNS-zoner.

![DNS-namnserver](./media/dns-domain-delegation/image1.png)

1. Hej klientbegäranden `www.partners.contoso.net` från sina lokala DNS-servern.
1. hello lokala DNS-servern har inte hello post så att den gör en begäran tootheir namn rotserver.
1. hello rotserver namn inte har hello post men vet hello-adressen för hello `.net` namnserver, ger adress toohello DNS-servern
1. hello DNS skickar hello begäran toohello `.net` namnserver, den har inte hello post men vet hello adressen hello contoso.net namn på servern. I det här fallet är det en DNS-zon i Azure DNS.
1. hello zonen `contoso.net` inte har hello post men vet hello namnserver för `partners.contoso.net` och svarar med. I det här fallet är det en DNS-zon i Azure DNS.
1. begär hello DNS-servern hello IP-adressen för `partners.contoso.net` från hello `partners.contoso.net` zon. Det innehåller hello A-post och svarar med hello IP-adress.
1. hello DNS-servern ger hello IP-adress toohello klient
1. hello klienten ansluter toohello webbplats `www.partners.contoso.net`.

Varje delegation verkligen har två kopior av hello NS-poster. en i hello överordnade zonen pekar toohello underordnade och en annan i hello underordnade zonen sig själv. hello 'contoso.net' zonen innehåller hello NS-poster för 'contoso.net' (i tillägget toohello NS-poster i ”net'). Dessa poster kallas auktoritära NS-poster och de sitter vid hello apex av hello underordnad zon.

## <a name="next-steps"></a>Nästa steg

Lär dig hur för[Delegera din domän tooAzure DNS](dns-delegate-domain-azure-dns.md)

