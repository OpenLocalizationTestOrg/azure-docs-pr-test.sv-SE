---
title: "aaaHosting zoner för omvänd DNS-sökning i Azure DNS | Microsoft Docs"
description: "Lär dig hur toouse Azure DNS toohost hello zoner för omvänd DNS-sökning för IP-adressintervall"
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
ms.openlocfilehash: 24feb8ef1c75a7d91938867f348fed1190046e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a>Värd för omvänd sökning DNS-zoner i Azure DNS

Den här artikeln förklarar hur toohost hello zoner för omvänd DNS-sökning för din tilldelade IP-adressintervall i Azure DNS. hello IP-adressintervall som representeras av hello zon för omvänd sökning måste tilldelas tooyour organisation, vanligtvis av din Internetleverantör.

tooconfigure omvänd DNS för Azure-ägs IP-adress som tilldelats tooyour Azure-tjänsten, se [konfigurera hello omvänd sökning för hello IP-adresser tilldelas tooyour Azure-tjänsten](dns-reverse-dns-for-azure-services.md).

Innan du läser den här artikeln bör du vara bekant med den här [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md).

Den här artikeln vägleder dig genom hello steg toocreate din första DNS-zon för omvänd sökning och en post med hjälp av hello Azure-portalen, Azure PowerShell, Azure CLI 1.0 eller 2.0 för Azure CLI.

## <a name="create-a-reverse-lookup-dns-zone"></a>Skapa en DNS-zon för omvänd sökning

1. Logga in toohello [Azure-portalen](https://portal.azure.com)
1. Hej hubbmenyn, klicka på och klicka på **ny** > **nätverk** > och klicka sedan på **DNS-zonen** tooopen hello **skapa DNS-zonen**bladet.

   ![DNS-zon](./media/dns-reverse-dns-hosting/figure1.png)

1. På hello **skapa DNS-zonen** bladet namnge din DNS-zon. hello namnet på hello zonen är utformade på olika sätt för IPv4 och IPv6-prefix. Använd antingen hello instruktioner för [IPV4](#ipv4) eller [IPv6](#ipv6) tooname zonen. När du är klar klickar du på **skapa** toocreate hello zonen.

### <a name="ipv4"></a>IPv4

hello namnet på en IPv4 zon för omvänd sökning baseras på hello IP-adressintervall som representerar. Det bör vara i hello följande format: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Exempel finns i [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md#ipv4).

> [!NOTE]
> När du skapar classless omvänd DNS-zoner för sökning i Azure DNS, måste du använda ett bindestreck (`-`) i stället för ett snedstreck ('/ ') hello zonnamn.
>
> Till exempel för hello IP-intervallet 192.0.2.128/26 du måste använda `128-26.2.0.192.in-addr.arpa` som hello zonnamnet i stället för `128/26.2.0.192.in-addr.arpa`.
>
> Detta beror på att när båda stöds av hello DNS-standarden DNS zonen namn som innehåller hello snedstreck (`/`) tecken stöds inte i Azure DNS.

hello följande exempel visas hur toocreate en klass C omvänd DNS-zon som heter `2.0.192.in-addr.arpa` i Azure DNS via hello Azure-portalen:

 ![Skapa DNS-zon](./media/dns-reverse-dns-hosting/figure2.png)

hello 'Resursgruppsplats' definierar hello plats för resursgruppen hello och har ingen inverkan på hello DNS-zon. hello DNS-zonen plats är alltid ”globala' och visas inte.

hello följande exempel visar hur toocomplete detta uppgift med Azure PowerShell och hello Azure CLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

hello namnet på en IPv6 zon för omvänd sökning måste vara i hello följande format: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Exempel finns i [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md#ipv6).


hello följande exempel visas hur toocreate en IPv6 omvänd DNS-sökningszon med namnet `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` i Azure DNS via hello Azure-portalen:

 ![Skapa DNS-zon](./media/dns-reverse-dns-hosting/figure3.png)

hello 'Resursgruppsplats' definierar hello plats för resursgruppen hello och har ingen inverkan på hello DNS-zon. hello DNS-zonen plats är alltid ”globala' och visas inte.

hello följande exempel visar hur toocomplete detta uppgift med Azure PowerShell och hello Azure CLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>Delegera en zon för omvänd DNS-sökning

Att ha skapat en zon för omvänd DNS-sökning, måste du kontrollera hello zonen delegerad från hello överordnade zonen. DNS-delegering kan hello DNS-processen toofind hello namn namnmatchningsservrar värd för en zon för omvänd DNS-sökning. Detta gör att dessa servrar tooanswer DNS omvänd namnfrågor för hello IP-adresser i ditt-adressintervall.

För zoner för vanlig sökning hello processen att delegera en DNS-zon beskrivs i [Delegera din domän tooAzure DNS](dns-delegate-domain-azure-dns.md). Zoner för omvänd sökning-delegering fungerar hello samma sätt. hello enda skillnaden är att du behöver tooconfigure hello namnservrar med hello Internetleverantören som tillhandahåller IP-adressintervall i stället för domännamnsregistratorn.

## <a name="create-a-dns-ptr-record"></a>Skapa en DNS PTR-post

### <a name="ipv4"></a>IPv4

hello vägleder följande exempel dig genom hello processen att skapa en PTR-post i en omvänd DNS-zon i Azure DNS. Andra typer av poster och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hjälp av hello Azure-portalen](dns-operations-recordsets-portal.md).

1.  Hello överst i hello **DNS-zonen** bladet väljer **+ postuppsättningen** tooopen hello **lägga till postuppsättning** bladet.

 ![DNS-zon](./media/dns-reverse-dns-hosting/figure4.png)

1. På hello **lägga till postuppsättning** bladet. 
1. Välj **PTR** från hello posten ”**typen**” menyn.  
1. hello måste namnet på postuppsättningen hello för en PTR-post toobe hello resten av hello IPv4-adress i omvänd ordning. I det här exemplet är hello tre första oktetterna redan ifyllda som en del av namn på zon för hello (.2.0.192). Därför tillhandahålls endast hello sista oktetten i hello fältnamn. Till exempel namnger du din postuppsättning ”**15**” för en resurs vars IP-adressen är 192.0.2.15.  
1. I hello ”**domännamn**”, ange hello fullständigt kvalificerade domännamnet (FQDN) för hello resursen med hjälp av hello IP.
1. Välj **OK** längst hello hello bladet toocreate hello DNS-post.

 ![Lägg till en postuppsättning](./media/dns-reverse-dns-hosting/figure5.png)

hello följande är exempel på hur toocomplete uppgiften med PowerShell och hello AzureCLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

följande exempel hello vägleder dig genom hello processen att skapa ny 'PTR-post. Andra typer av poster och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hjälp av hello Azure-portalen](dns-operations-recordsets-portal.md).

1. Hello överst i hello **DNS-zonen bladet**väljer **+ postuppsättningen** tooopen hello **lägga till postuppsättning** bladet.

  ![DNS-zonen bladet](./media/dns-reverse-dns-hosting/figure6.png)

2. På hello **lägga till postuppsättning** bladet. 
3. Välj **PTR** från hello posten ”**typen**” menyn.  
4. hello måste namnet på postuppsättningen hello för en PTR-post toobe hello resten av hello IPv6-adress i omvänd ordning. Det får inte innehålla noll komprimering. I det här exemplet hello första 64-bitars av hello IPv6 har redan angetts som en del av namn på zon för hello (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa). Därför tillhandahålls endast hello senaste 64-bitars i hello fältnamn. hello senaste 64-bitars hello IP-adress har angetts i omvänd ordning med en period som hello avgränsare mellan varje hexadecimalt tal. Till exempel namnger du din postuppsättning ”**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**” för en resurs vars IP-adressen är 2001:0db8:abdc:0000:f524:10bc:1af9:405e.  
5. I hello ”**domännamn**”, ange hello fullständigt kvalificerade domännamnet (FQDN) för hello resursen med hjälp av hello IP.
6. Välj **OK** längst hello hello bladet toocreate hello DNS-post.

![postuppsättningen bladet Lägg till](./media/dns-reverse-dns-hosting/figure7.png)

hello följande är exempel på hur toocomplete uppgiften med PowerShell och hello AzureCLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Visa poster

tooview hello poster som du skapade navigera tooyour DNS-zonen i hello Azure-portalen. I hello lägre tillhör hello **DNS-zonen** bladet hittar du hello poster för hello DNS-zonen. Du bör se hello standard NS och SOA-poster, som skapas i varje zon, plus eventuella nya poster som du har skapat.

### <a name="ipv4"></a>IPv4

DNS-zonen bladet visar IPv4 PTR-poster:

![DNS-zonen bladet](./media/dns-reverse-dns-hosting/figure8.png)

hello följande exempel visar hur tooview hello PTR-poster med PowerShell eller hello Azure CLI:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

DNS-zonen bladet visar IPv6 PTR-poster:

![DNS-zonen bladet](./media/dns-reverse-dns-hosting/figure9.png)

hello följande är exempel på hur tooview hello poster med PowerShell och hello AzureCLI:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Kan jag vara värd zoner för omvänd DNS-sökning för min ISP-tilldelad IP-Adressblock på Azure DNS?

Ja. Värd för hello zoner för omvänd sökning (ARPA) för din egen IP-adressintervall i Azure DNS stöds fullt ut.

Skapa hello zon för omvänd sökning i Azure DNS som beskrivs i den här artikeln och sedan arbeta med Leverantören för[ombud hello zonen](dns-domain-delegation.md).  Du kan sedan hantera hello PTR-poster för varje omvänd sökning i hello samma sätt som andra posttyper.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>Hur mycket har värd min omvänd DNS-sökning zonen kostnaden?

Värd för zon för hello omvänd DNS-sökning för din ISP-tilldelad IP-Adressblock i Azure DNS debiteras med [Azure DNS standardpriser](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kan jag vara värd för omvänd DNS-sökningszoner för både IPv4 och IPv6-adresser i Azure DNS?

Ja. Den här artikeln förklarar hur toocreate både IPv4 och IPv6-zoner för omvänd DNS-sökning i Azure DNS.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Kan jag importera en befintlig omvänd DNS-sökningszon?

Ja. Du kan använda hello Azure CLI tooimport befintlig DNS-zoner i Azure DNS. Detta fungerar för både zoner för vanlig sökning och zoner för omvänd sökning.

Mer information finns i [Import- och DNS-zon filen med hello Azure CLI](dns-import-export.md).

## <a name="next-steps"></a>Nästa steg

Läs mer om omvänd DNS [omvänd DNS-sökning på Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Lär dig hur för[hantera omvänd DNS-posterna för din Azure-tjänster](dns-reverse-dns-for-azure-services.md).
