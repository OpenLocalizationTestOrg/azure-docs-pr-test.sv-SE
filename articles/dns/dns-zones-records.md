---
title: "aaaDNS zoner och registrerar översikt – Azure DNS | Microsoft Docs"
description: "Översikt över stöd för värd för DNS-zoner och poster i Microsoft Azure DNS."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: jonatul
ms.openlocfilehash: f214c3e2e810a80a000281820acd35f0aaf5a7e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-dns-zones-and-records"></a>Översikt över DNS-zoner och poster

Den här sidan förklarar hello nyckelbegreppen i domäner, DNS-zoner och DNS-poster och postuppsättningar och hur de stöds i Azure DNS.

## <a name="domain-names"></a>Domännamn

hello Domain Name System är en hierarki av domäner. hello hierarki börjar från hello '' rotdomänen, vars namn är helt enkelt ”**.**'.  Under detta kommer toppdomänerna, till exempel ”com”, ”net”, ”org”, ”se” eller ”uk”.  Under dessa finns domänerna på den andra nivån, till exempel ”org.se” eller ”co.uk”. hello domäner i hello DNS-hierarkin distribueras globalt, DNS-namnservrar runt hälsningsmeddelande som värd.

En domännamnsregistratorn är en organisation som gör att du toopurchase ett domännamn, till exempel ”contoso.com”.  Köp en domän med namnet ger du hello rätt toocontrol hello DNS-hierarkin med det, till exempel tillåta att du toodirect hello namnet ”www.contoso.com” tooyour företagets webbplats. hello register kan vara värd för hello domän i sin egen namnservrar för din räkning, eller att du toospecify alternativt namn för servrar.

Azure DNS ger en serverinfrastruktur för namn på distribuerade globalt, hög tillgänglighet som du kan använda toohost din domän. Värd för dina domäner i Azure DNS, kan du hantera din DNS-poster med hello samma autentiseringsuppgifter, API: er, verktyg, fakturering och support i andra Azure-tjänster.

Azure DNS stöder för närvarande inte köp av domännamn. Om du vill toopurchase ett domännamn måste toouse från tredje part domännamnsregistratorn. hello registrator debiterar vanligtvis en liten årlig avgift. hello domäner kan finnas i Azure DNS för för hantering av DNS-poster. Se [delegera en domän tooAzure DNS](dns-domain-delegation.md) mer information.

## <a name="dns-zones"></a>DNS-zoner

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>DNS-poster

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Time to live

hello tid toolive, eller TTL, anger du hur länge varje post ska cachelagras av klienter innan efterfrågas på nytt. I ovanstående exempel hello är hello TTL 3600 sekunder eller 1 timme.

I Azure DNS hello TTL-värde har angetts för hello postuppsättningen, inte för varje post så samma värde används för alla poster i posten hello inställd.  Du kan ange ett TTL-värde mellan 1 och 2 147 483 647 sekunder.

### <a name="wildcard-records"></a>Poster med jokertecken

Azure DNS stöder [poster med jokertecken](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Jokertecken poster returneras i svaret tooany frågan med ett matchande namn (om det finns en bättre matchning från en icke-jokertecken postuppsättning). Azure DNS stöder postuppsättningar med jokertecken för alla postuppsättningar utom NS och SOA.

toocreate en post med jokertecken ange använder hello postuppsättningsnamnet ”\*'. Alternativt kan du också använda ett namn med ”\*'som dess vänstra etiketten, till exempel'\*.foo'.

### <a name="cname-records"></a>CNAME-poster

CNAME-postuppsättningar kan inte samexistera med andra postuppsättningar med hello samma namn. T.ex, du kan inte skapa en CNAME-postuppsättning med hello relativa namnet ”www” och en A registrera med hello relativa namnet ”www” på hello samma tid.

Eftersom hello zonens apex (namn = ”@”) alltid innehåller NS- och SOA-post i hello mängder som skapades när hello zonen skapades, du kan inte skapa en CNAME-postuppsättning på zonens apex hello.

Dessa villkor uppkommer hello DNS-standarderna och är inte begränsningar i Azure DNS.

### <a name="ns-records"></a>NS-poster

hello NS-postuppsättning på zonens apex hello (namnet ”@”) skapas automatiskt med varje DNS-zon och tas bort automatiskt när hello zonen raderas (det kan inte tas bort separat).

Den här postuppsättning innehåller hello namnen på hello Azure DNS-namnet servrar tilldelade toohello zon. Du kan lägga till ytterligare namn servrar toothis NS postuppsättningen, toosupport samordna värd domäner med mer än en DNS-leverantör. Du kan också ändra hello TTL-värde och metadata för den här postuppsättningen. Du kan inte ta bort eller ändra hello förifyllda Azure DNS-namnservrar. 

Observera att detta gäller endast toohello NS postuppsättning på zonens apex hello. Andra NS-postuppsättningar i zonen (som används toodelegate underordnade zoner) kan skapas, ändras och tas bort utan begränsning.

### <a name="soa-records"></a>SOA-poster

En uppsättning för SOA-poster skapas automatiskt vid hello apex för varje zon (namn = ”@”), och tas bort automatiskt när hello zon tas bort.  SOA-poster kan inte skapas eller tas bort separat.

Du kan ändra alla egenskaper av hello SOA-poster utom hello 'host'-egenskap som är förkonfigurerad toorefer toohello primära namn servernamn som tillhandahålls av Azure DNS.

### <a name="spf-records"></a>SPF-poster

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>SRV-poster

[SRV-poster](https://en.wikipedia.org/wiki/SRV_record) används av olika tjänster toospecify serverplatser. När du anger en SRV-post i Azure DNS:

* Hej *service* och *protokollet* måste anges som en del av hello postuppsättningens namn, med understreck prefixet.  Till exempel '\_sip.\_ TCP.Name'.  För en post på hello zonens apex finns inget behov av toospecify ”@” i postnamnet hello, bara använda hello-tjänst och protokoll, till exempel '\_sip.\_ TCP'.
* Hej *prioritet*, *vikt*, *port*, och *mål* anges som parametrar i varje post i hello uppsättningen av poster.

### <a name="txt-records"></a>TXT-poster

TXT-poster är används toomap domän namn tooarbitrary textsträngar. De används i flera program, särskilt relaterade tooemail konfigurering, till exempel hello [avsändaren princip Framework (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) och [DomainKeys identifieras e-post (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail).

hello DNS-standarden tillåter en enda post TXT toocontain flera strängar, som kan vara upp too254 tecken. När flera strängar används kan de användas i sammanslagningen av klienter och behandlas som en sträng.

När du anropar hello Azure DNS-REST API, måste toospecify varje TXT-sträng separat.  När du använder hello Azure-portalen, PowerShell eller CLI gränssnitt bör du ange en enskild textsträng per post som automatiskt är uppdelad i 254 tecken segment om det behövs.

hello flera strängar i en DNS-post inte ska förväxlas med hello flera TXT-poster i en TXT-postuppsättning.  En TXT-postuppsättning kan innehålla flera poster, *som* kan innehålla flera strängar.  Azure DNS stöder en totala stränglängden för in too1024 tecken i varje TXT-postuppsättning (för alla poster som kombineras).

## <a name="tags-and-metadata"></a>Taggar och metadata

### <a name="tags"></a>Taggar

Taggar är en lista över namn / värde-par och används av Azure Resource Manager toolabel resurser.  Azure Resource Manager använder taggar tooenable filtrerade vyer av fakturan Azure och du kan också tooset en princip på vilka taggar som krävs. Mer information om taggar finns [med hjälp av taggar tooorganize resurserna i Azure](../azure-resource-manager/resource-group-using-tags.md).

Azure DNS stöder med hjälp av Azure Resource Manager taggar på DNS-zonen resurser.  Det stöder inte taggar på DNS-postuppsättningar, även om som ett alternativ 'metadata' stöds på DNS-post anger som beskrivs nedan.

### <a name="metadata"></a>Metadata

Som ett alternativ toorecord ange taggar, stöder Azure DNS kommentera postuppsättningar med 'metadata'.  Liknande tootags metadata kan du tooassociate namn / värde-par med varje uppsättning av poster.  Detta kan vara användbart, till exempel ange toorecord hello syftet med varje post.  Till skillnad från taggar, metadata kan inte använda tooprovide en filtrerad vy av fakturan Azure och kan inte anges i en Azure Resource Manager-princip.

## <a name="etags"></a>Etags

Anta att två personer eller två processer försök toomodify en DNS-posten på hello samma tid. Vilken wins? Och hello vinnaren känner att de har över ändringar som skapats av någon annan?

Azure DNS använder Etags toohandle samtidiga ändringar toohello samma resurs på ett säkert sätt. Etags skiljer sig från [Azure Resource Manager ”-taggar'](#tags). Varje DNS-resursposter (zonen eller postuppsättning) har en Etag som är kopplade till den. När en resurs hämtas, hämtas även dess Etag. När du uppdaterar en resurs måste välja du toopass tillbaka hello Etag så att Azure DNS kan kontrollera att hello Etag på hello server matchar. Eftersom varje uppdatering tooa resurs resulterar i hello Etag återskapas, anger ett Etag-Typfel samtidiga har ändrats. Etags kan också användas när du skapar en ny resurs tooensure som hello resursen inte finns redan.

Som standard använder Azure DNS-PowerShell Etags tooblock samtidiga ändringar toozones och postuppsättningar. hello valfria *-skriva över* växel kan vara används toosuppress Etag-kontroller, i vilket fall alla samtidiga ändringarna som skett skrivs över.

På hello nivå av hello Azure DNS-REST API anges Etags med HTTP-huvuden.  Deras beteende ges i följande tabell hello:

| Huvudet | Beteende |
| --- | --- |
| Ingen |PLACERA lyckas alltid (inga Etag-kontroller) |
| IF-match<etag> |PLACERA lyckas bara om resursen finns och Etag matchar |
| IF-match * |PLACERA lyckas bara om resursen finns |
| IF-none-match * |PLACERA lyckas bara om resursen inte finns |


## <a name="limits"></a>Begränsningar

hello följer standardgränser som gäller när du använder Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Nästa steg

* toostart med hjälp av Azure DNS Lär dig hur för[skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md) och [skapa DNS-poster](dns-getstarted-create-recordset-portal.md).
* toomigrate en befintlig DNS-zon Lär dig hur för[importera och exportera en DNS-zonfilen](dns-import-export.md).
