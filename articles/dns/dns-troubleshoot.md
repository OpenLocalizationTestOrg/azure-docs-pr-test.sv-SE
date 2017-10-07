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
# <a name="azure-dns-troubleshooting-guide"></a>Azure DNS felsökningsguiden

Den här sidan innehåller felsökningsinformation för Azure DNS-frågor.

Om stegen inte löser problemet kan du också söka efter eller ett inlägg problemet i vår [community-support-forum på MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork). Du kan också öppna en Azure-supportbegäran.


## <a name="i-cant-create-a-dns-zone"></a>Jag kan inte skapa en DNS-zon

tooresolve vanliga problem, försök med en eller flera av följande hello:

1.  Granska hello Azure DNS granska loggar toodetermine hello orsaken till felet.
2.  Varje DNS-zonnamn måste vara unikt inom sin resursgrupp. Det vill säga två DNS-zoner med hello samma namn kan inte dela en resursgrupp. Försök med ett annat zonnamn eller en annan resursgrupp.
3.  Du kan se ett fel ”du har nått eller överskridit hello maxantalet zoner i prenumerationen {prenumerations-id}”. Antingen använder en annan Azure-prenumeration, ta bort vissa zoner eller kontakta Azure-supporten tooraise prenumerationsgränsen för.
4.  Du kan se ett fel ”hello-zonen {zonen name}' är inte tillgänglig”. Felet innebär att Azure DNS användes tooallocate namnservrar för denna DNS-zon. Försök med ett annat zonnamn. Du kan också om du är hello domain name ägare kontakta Azure-supporten, som kan allokera namnservrar för dig.


### <a name="recommended-documents"></a>**Rekommenderade dokument**

[DNS-zoner och -poster](dns-zones-records.md)
<br>
[Skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md)

## <a name="i-cant-create-a-dns-record"></a>Jag kan inte skapa någon DNS-post

tooresolve vanliga problem, försök med en eller flera av följande hello:

1.  Granska hello Azure DNS granska loggar toodetermine hello orsaken till felet.
2.  Hello postuppsättning finns det redan?  Azure DNS hanterar poster med hjälp av posten *anger*, som är hello uppsättning poster av hello samma namn och hello samma typ. Om en post med hello samma namn och Skriv redan finns tooadd en annan post du ska redigera hello befintlig post anger.
3.  Försöker toocreate en post på hello DNS-zonens apex (hello-rot' hello zonen)? Om så hello DNS-konventionen är toouse hello ”@-tecknet som hello postens namn. Observera också att hello DNS-standarden inte tillåter CNAME-poster på hello zonens apex.
4.  Har du en CNAME-konflikt?  hello DNS-standarden tillåter inte en CNAME-post med hello samma namn som en post för andra typer. Om du har en befintlig CNAME, skapa en post med samma namn som en annan typ misslyckas hello.  På samma sätt misslyckas att skapa en CNAME-post om hello namnet matchar en befintlig post av en annan typ. Ta bort hello konflikten genom att ta bort hello andra poster eller välja en annan postnamnet.
5.  Har du nått hello begränsning på hello postuppsättningar som tillåts i en DNS-zon. hello aktuellt antal uppsättningar av poster och hello maximalt antal uppsättningar av poster visas i hello Azure-portalen under hello ”egenskaper” för hello zonen. Om du har nått den här gränsen, antingen ta bort vissa postuppsättningar eller kontakta Azure-supporten tooraise postuppsättning gränsen för den här zonen, och försök sedan igen. 


### <a name="recommended-documents"></a>**Rekommenderade dokument**

[DNS-zoner och -poster](dns-zones-records.md)
<br>
[Skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md)



## <a name="i-cant-resolve-my-dns-record"></a>Jag kan inte matcha min DNS-post

DNS-namnmatchningen är en process i flera steg som kan misslyckas av flera orsaker. hello följande steg när du undersöka varför inte DNS-matchning för en DNS-post i en zon som finns i Azure DNS.

1.  Bekräfta att hello DNS-poster har konfigurerats korrekt i Azure DNS. Granska hello DNS-poster i hello Azure-portalen kontrollerar att hello zonnamnet och postnamnet posttyp är korrekta.
2.  Bekräfta att hello DNS-poster ska matcha på hello Azure DNS-namnservrar.
    - Om du gör DNS-frågor från din lokala dator, kan det hända att cachelagrade resultaten inte avspeglar hello hello namnservrar aktuella tillstånd.  Företagsnätverk ofta använda DNS-proxyservrar, vilket gör att DNS-frågor inte dirigeras toospecific namnservrar.  tooavoid dessa problem använder en webbaserad namnmatchningstjänst som [digwebinterface](http://digwebinterface.com).
    - Vara säker på att toospecify hello rätt namnservrar för DNS-zonen som visas i hello Azure-portalen.
    - Kontrollera att hello DNS-namnet är rätt (du har toospecify hello fullständigt kvalificerade namn, inklusive hello zonnamnet) och hello posttyp är korrekt
3.  Bekräfta att hello DNS-namnet har korrekt [delegerad toohello Azure DNS-namnservrar](dns-domain-delegation.md). Det finns [många tredjeparts webbplatser som erbjuder DNS-delegeringsverifiering](https://www.bing.com/search?q=dns+check+tool). Det här testet är en *zon* delegeringstest, så bör du bara ange hello DNS-zonnamnet och inte hello fullständigt kvalificerade postnamnet.
4.  Efter att ha utfört hello ovan, ska DNS-post nu matcha korrekt. tooverify, kan du igen använda [digwebinterface](http://digwebinterface.com), nu med hello standardinställningar namn på servern.


### <a name="recommended-documents"></a>**Rekommenderade dokument**

[Delegera en domän tooAzure DNS](dns-domain-delegation.md)



## <a name="how-do-i-specify-hello-service-and-protocol-for-an-srv-record"></a>Hur jag ange hello ”tjänst” och ”protokoll” för en SRV-post?

Azure DNS hanterar DNS-poster som postuppsättningar – hello samling poster med hello samma namn och hello samma typ. Måste anges som del av hello postuppsättningsnamnet toobe för en SRV-postuppsättning hello ”tjänst” och ”protokoll”. hello SRV parametrar ('priority', 'vikt', '-port' och 'target') anges separat för varje post i hello postuppsättning.

Exempel på SRV-postnamn (tjänstnamn ”sip”, protokoll ”tcp”):

- \_SIP. \_tcp (skapar en postuppsättning på zonens apex hello)
- \_sip.\_tcp.sipservice (skapar en postuppsättning med namnet ”sipservice”)

### <a name="recommended-documents"></a>**Rekommenderade dokument**

[DNS-zoner och -poster](dns-zones-records.md)
<br>
[Skapa uppsättningar av DNS-poster och poster med hjälp av hello Azure-portalen](dns-getstarted-create-recordset-portal.md)
<br>
[SRV-posttyp (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)


## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [Azure DNS-zoner och poster](dns-zones-records.md)
* toostart med hjälp av Azure DNS Lär dig hur för[skapa en DNS-zon](dns-getstarted-create-dnszone-portal.md) och [skapa DNS-poster](dns-getstarted-create-recordset-portal.md).
* toomigrate en befintlig DNS-zon Lär dig hur för[importera och exportera en DNS-zonfilen](dns-import-export.md).

