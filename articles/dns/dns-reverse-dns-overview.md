---
title: "aaaOverview av omvänd DNS i Azure | Microsoft Docs"
description: "Lär dig hur omvänd DNS fungerar och hur den kan användas i Azure"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 687663fb83469ab8e696bb714649d0856915bad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-reverse-dns-and-support-in-azure"></a>Översikt över omvänd DNS- och support i Azure

Den här artikeln ger en översikt över hur omvänd DNS fungerar och hello omvänd DNS-scenarier som stöds i Azure.

## <a name="what-is-reverse-dns"></a>Vad är omvänd DNS?

Vanliga DNS-poster aktiverar mappning från en DNS-namnet (t.ex 'www.contoso.com) tooan IP-adress (till exempel 64.4.6.100).  Omvänd DNS gör hello översättning av en IP-adress (64.4.6.100) tillbaka tooa namnet ('www.contoso.com).

Omvänd DNS-poster används i en mängd olika situationer. Omvänd DNS-poster används ofta för att bekämpa skräppost e-post genom att verifiera hello avsändaren av ett e-postmeddelande.  hello mottagande e-server hämtar hello omvänd DNS-post för hello skickar serverns IP-adress och verifierar om som värd är auktoriserade toosend e-post från hello ursprung domän. 

## <a name="how-reverse-dns-works"></a>Hur omvänd DNS fungerar

Omvänd DNS-poster finns i särskilda DNS-zoner, kallas ”ARPA-zoner.  Dessa zoner bildar en separat DNS parallellt med hello vanliga hierarkin som är värd för domäner, till exempel ”contoso.com”.

Till exempel implementeras hello DNS-posten ”www.contoso.com” med en post i DNS ”A” med hello namnet www om du i hello zonen ”contoso.com”.  Den här A-posten pekar toohello motsvarande IP-adress i det här fallet 64.4.6.100.  hello omvänd sökning implementeras separat, med hjälp av en 'PTR-post med namnet '100' i hello zon '6.4.64.in-addr.arpa' (Observera att IP-adresser återförs i ARPA-zoner.)  Den här PTR-post pekar om den har konfigurerats korrekt toohello namnet ”www.contoso.com”.

När en organisation tilldelas ett IP-Adressblock måste hämta de också hello rätt toomanage hello motsvarande ARPA zon. Hej ARPA-zoner som motsvarande toohello IP-adress-block som används av Azure på och hanteras av Microsoft. Leverantören kan vara värd för hello ARPA zon för din egen IP-adresser som du eller tillåta tooyou värden hello ARPA zon i en DNS-tjänsten du väljer, till exempel Azure DNS.

> [!NOTE]
> Vanliga DNS-sökningar och omvänd DNS-sökning implementeras i separata, parallella DNS-hierarkier. hello omvänd sökning för ”www.contoso.com” är **inte** finns i hello zonen ”contoso.com”, i stället den finns i hello ARPA zon för hello motsvarande IP-adressintervall. Separata zoner används för IPv4 och IPv6-Adressblock.

### <a name="ipv4"></a>IPv4

hello namnet på en IPv4 zon för omvänd sökning måste vara i hello följande format: `<IPv4 network prefix in reverse order>.in-addr.arpa`.

Till exempel när du skapar en zon för omvänd toohost poster för värdar med IP-adresser som finns i hello 192.0.2.0/24 prefix, skapas hello zonnamnet genom att isolera hello nätverksprefixet av hello adress (192.0.2) och sedan återföra hello ordning (2.0.192) och lägger till hello suffix `.in-addr.arpa`.

|Undernät-klass|Nätverksprefixet  |Inverterad nätverksprefixet  |Standard-suffix  |Zonnamn på för omvänd |
|-------|----------------|------------|-----------------|---------------------------|
|Klass A|203.0.0.0/8     | 203        | .in-addr.arpa   | `203.in-addr.arpa`        |
|Klass B|198.51.0.0/16   | 51.198     | .in-addr.arpa   | `51.198.in-addr.arpa`     |
|Klass C|192.0.2.0/24    | 2.0.192    | .in-addr.arpa   | `2.0.192.in-addr.arpa`    |

### <a name="classless-ipv4-delegation"></a>Classless IPv4-delegering

I vissa fall hello IP-adressintervallet allokerade tooan organisation är mindre än en klass C (/ 24) intervall. I det här fallet hello IP-adressintervall hamnar inte på en zon gräns inom hello `.in-addr.arpa` zonen hierarki och därför kan inte delegeras som en underordnad zon.

I stället en annan mekanism används tootransfer kontroll av enskilda omvänd sökning (PTR) poster tooa dedikerad DNS-zon. Den här mekanismen delegerar en underordnad zon för varje IP-adressintervall och maps varje IP-adressen i hello vara individuellt toothat underordnade zonen med CNAME-poster.

Anta exempelvis att en organisation beviljas hello IP-intervallet 192.0.2.128/26 av dess Internetleverantör. Detta representerar 64 IP-adresser från 192.0.2.128 too192.0.2.191. Omvänd DNS för det här intervallet implementeras på följande sätt:
- hello organisation skapar en zon för omvänd sökning som kallas 128-26.2.0.192.in-addr.arpa. hello prefixet ' 128-26' representerar hello nätverket segment tilldelat toohello organisation inom hello klass C (/ 24) intervall.
- hello ISP skapar NS-poster tooset in hello DNS-delegering för hello ovan zonen från hello klass C överordnade zonen. Det skapar också CNAME-poster i hello överordnad (klass C) zon för omvänd sökning mappning varje IP-adress i hello IP-intervallet toohello nya zonen som skapas av hello organisation:

```
$ORIGIN 2.0.192.in-addr.arpa
; Delegate child zone
128-26    NS       <name server 1 for 128-26.2.0.192.in-addr.arpa>
128-26    NS       <name server 2 for 128-26.2.0.192.in-addr.arpa>
; CNAME records for each IP address
129       CNAME    129.128-26.2.0.192.in-addr.arpa
130       CNAME    130.128-26.2.0.192.in-addr.arpa
131       CNAME    131.128-26.2.0.192.in-addr.arpa
; etc
```
- sedan hello organisation hanterar hello enskilda PTR-poster i sina underordnade zonen.

```
$ORIGIN 128-26.2.0.192.in-addr.arpa
; PTR records for each UIP address. Names match CNAME targets in parent zone
129      PTR    www.contoso.com
130      PTR    mail.contoso.com
131      PTR    partners.contoso.com
; etc
```
En omvänd sökning för hello IP-adressen '192.0.2.129' för en PTR-post med namnet '129.2.0.192.in-addr.arpa'. Den här frågan matchas via hello CNAME i hello överordnade zonen toohello PTR-post i hello underordnad zon.

### <a name="ipv6"></a>IPv6

hello namnet på en zon för omvänd sökning av IPv6-bör vara i hello följande format:`<IPv6 network prefix in reverse order>.ip6.arpa`

Till exempel. När du skapar en zon för omvänd toohost poster för värdar med IP-adresser som finns i hello 2001:db8:1000:abdc:: / 64 prefix hello zonnamnet skapas genom att isolera hello nätverksprefixet hello adress (2001:db8:abdc::). Expandera bredvid hello IPv6 nätverket prefixet tooremove [noll komprimering](https://technet.microsoft.com/library/cc781672(v=ws.10).aspx), om den har använt tooshorten hello IPv6-adressprefix (2001:0db8:abdc:0000::). Omvänd hello ordning, med en period som hello mellan varje hexadecimalt värde hello prefix, toobuild hello omvänd nätverksprefixet (`0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2`) och lägga till suffix för hello `.ip6.arpa`.


|Nätverksprefixet  |Utökade och inverterad nätverksprefixet |Standard-suffix |Zonnamn på för omvänd  |
|---------|---------|---------|---------|
|2001:db8:abdc:: / 64    | 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2        | . ip6.arpa        | `0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa`       |
|2001:db8:1000:9102:: / 64    | 2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2        | . ip6.arpa        | `2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2.ip6.arpa`        |


## <a name="azure-support-for-reverse-dns"></a>Azure-stöd för omvänd DNS

Azure stöder två separata scenarier som rör tooreverse DNS:

**Värd för hello omvänd sökning zonen motsvarande tooyour IP-adressintervall.**
Azure DNS kan användas för[värd zoner för omvänd sökning och hantera hello PTR-poster för varje omvänd DNS-sökning](dns-reverse-dns-hosting.md), för både IPv4 och IPv6.  Hej skapar hello (ARPA) zon för omvänd sökning, ställa in hello delegering och konfigurera PTR poster är hello samma som för vanliga DNS-zoner.  hello är bara skillnaderna att hello delegering måste vara konfigurerat via din Internetleverantör i stället för DNS-registratorns och endast hello PTR posttyp ska användas.

**Konfigurera hello omvänd DNS-post för hello IP-adress som tilldelats tooyour Azure-tjänsten.** Azure gör det möjligt för[konfigurera hello omvänd sökning för hello IP-adresser tilldelas tooyour Azure-tjänsten](dns-reverse-dns-for-azure-services.md).  Den här omvänd sökning har konfigurerats i Azure som en PTR-post i hello motsvarande ARPA zon.  Dessa ARPA-zoner, motsvarande tooall hello IP-adressintervall som används av Azure, hanteras av Microsoft

## <a name="next-steps"></a>Nästa steg

Läs mer om omvänd DNS [omvänd DNS-sökning på Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Lär dig hur för[värden hello zon för omvänd sökning för din ISP-tilldelad IP-adressintervall i Azure DNS](dns-reverse-dns-for-azure-services.md).
<br>
Lär dig hur för[hantera omvänd DNS-posterna för din Azure-tjänster](dns-reverse-dns-for-azure-services.md).

