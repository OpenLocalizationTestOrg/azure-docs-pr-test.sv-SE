---
title: aaaManage DNS-poster i Azure DNS med hello Azure CLI 1.0 | Microsoft Docs
description: "Hantera DNS-postuppsättningar och på Azure DNS-poster när värd för din domän på Azure DNS. Alla CLI 1.0-kommandon för åtgärder på uppsättningar av poster och poster."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2016
ms.author: jonatul
ms.openlocfilehash: 1f01450b0839f712cb1d96be318766bac581fea1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-in-azure-dns-using-hello-azure-cli-10"></a>Hantera DNS-poster i Azure DNS med hello Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Den här artikeln visar hur toomanage DNS-posterna för DNS-zonen med hjälp av hello plattformsoberoende Azure-kommandoradsgränssnittet (CLI) som är tillgänglig för Windows, Mac och Linux. Du kan också hantera DNS-posterna med [Azure PowerShell](dns-operations-recordsets.md) eller hello [Azure-portalen](dns-operations-recordsets-portal.md).

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet

Du kan göra hello med hjälp av något av följande versioner av CLI hello:

* [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) -vår CLI för hello klassisk och resurs management distributionsmodeller.
* [Azure CLI 2.0](dns-operations-recordsets-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering.

hello exemplen i den här artikeln förutsätter att du redan har [installerat hello Azure CLI version 1.0, loggat in, och skapa en DNS-zon](dns-operations-dnszones-cli-nodejs.md).

## <a name="introduction"></a>Introduktion

Innan du skapar DNS-poster i Azure DNS, måste du först toounderstand hur Azure DNS organiserar DNS-poster i DNS-postuppsättningar.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Mer information om DNS-poster i Azure DNS finns i [DNS-zoner och poster](dns-zones-records.md).

## <a name="create-a-dns-record"></a>Skapa en DNS-post

toocreate DNS-post och använda hello `azure network dns record-set add-record` kommando. Om du vill ha hjälp, så gå till `azure network dns record-set add-record -h`.

När du skapar en post du toospecify hello resursgruppens namn, zonnamn, postuppsättning namn, hello posttyp och hello detaljuppgifter om hello håller på att skapas. hello postuppsättning namn måste vara en *relativa* namn, vilket innebär att den inte får innehålla hello zonnamnet.

Om hello postuppsättning inte redan finns, skapas den du i det här kommandot. Om det redan finns hello postuppsättning hello det här kommandot adda posten som du anger toohello befintliga postuppsättning.

Om en ny postuppsättning skapas, används ett standard TTL-värde (Time to Live) på 3600. Anvisningar för hur toouse olika TTLs Se [skapa en DNS-postuppsättning](#create-a-dns-record-set).

hello följande exempel skapas en A-post som kallas *www* i hello zon *contoso.com* i hello resursgruppen *MyResourceGroup*. Hej IP-adressen för en post är hello *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

toocreate en post i hello apex av hello zon (i det här fallet ”contoso.com”), Använd hello postnamnet ”@”, inklusive hello citattecken:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>Skapa en uppsättning av DNS-poster

I hello exemplen ovan hello DNS-posten var antingen tillagda tooan befintliga uppsättningen av poster eller hello postuppsättning skapades *implicit*. Du kan också skapa hello postuppsättning *explicit* innan du lägger till registrerar tooit. Azure DNS stöder 'empty-postuppsättningar som kan fungera som en platshållare tooreserve ett DNS-namn innan du skapar DNS-poster. Tom postuppsättningar är synliga i hello Azure DNS kontrollen plan, men visas inte i hello Azure DNS-namnservrar.

Postuppsättningar skapas med hello `azure network dns record-set create` kommando. Om du vill ha hjälp, så gå till `azure network dns record-set create -h`.

Skapa hello-postuppsättning explicit kan du toospecify postuppsättning egenskaper, till exempel hello [Time To Live (TTL)](dns-zones-records.md#time-to-live) och metadata. [Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan vara används tooassociate programspecifika data med varje postuppsättningen, som nyckel / värde-par.

hello följande exempel skapar en tom postuppsättning med en 60-sekunders TTL med hjälp av hello `--ttl` parameter (kort form `-l`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --ttl 60
```

hello följande exempel skapas en postuppsättning med två metadataposter ”Avd = ekonomi” och ”miljö = produktion”, med hjälp av hello `--metadata` parameter (kort form `-m`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

Att ha skapat en tom postuppsättningen, poster kan läggas till med `azure network dns record-set add-record` enligt beskrivningen i [skapa en DNS-post](#create-a-dns-record).

## <a name="create-records-of-other-types"></a>Skapa poster för andra typer

Att ha sett i detalj hur toocreate ”A” poster, hello följande exempel visas hur toocreate post för andra typer som stöds av Azure DNS.

hello parametrar som används toospecify hello data varierar beroende på hello typ av hello-post. Till exempel för en post av typen ”A”, du anger hello IPv4-adress med hello parametern `-a <IPv4 address>`. Hej parametrar för varje posttyp kan visas med hjälp av `azure network dns record-set add-record -h`.

I varje fall visar vi hur toocreate en enskild post. hello-posten är tillagda toohello befintliga uppsättningen av poster eller en uppsättning poster har skapats implicit. Mer information om hur du skapar postuppsättningar och definierar post parametern uttryckligen finns i avsnittet [skapa en DNS-postuppsättning](#create-a-dns-record-set).

Vi inte ger ett exempel toocreate ett SOA-postuppsättning eftersom SOAs skapas och tas bort tillsammans med varje DNS-zon och kan inte skapas eller tas bort separat. Dock [hello SOA kan ändras, som visas i en senare exempel](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record"></a>Skapa en AAAA-post

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-aaaa AAAA --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a>Skapa en CNAME-post

> [!NOTE]
> hello DNS-standarden inte tillåter CNAME-poster på hello apex för en zon (`-Name "@"`), eller tillåter att postuppsättningar som innehåller fler än en post.
> 
> Mer information finns i [CNAME-poster](dns-zones-records.md#cname-records).

```azurecli
azure network dns record-set add-record  MyResourceGroup contoso.com  test-cname CNAME --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>Skapa en MX-post

I det här exemplet använder vi hello postuppsättningsnamnet ”@” toocreate hello MX-post på hello zonens apex (i det här fallet ”contoso.com”).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "@" MX --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>Skapa en NS-post

```azurecli
azure network dns record-set add-record MyResourceGroup  contoso.com  test-ns NS --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>Skapa en PTR-post

I det här fallet ”min-arpa-zone.com' representerar hello ARPA zonen som motsvarar IP-adressintervall. Varje PTR-post i den här zonen motsvarar tooan IP-adress inom IP-intervallet.  hello-postnamnet ”10” är hello sista oktetten hello IP-adress inom den här IP-adressintervall som representeras av den här posten.

```azurecli
azure network dns record-set add-record MyResourceGroup my-arpa-zone.com "10" PTR --ptrdname "myservice.contoso.com"
```

### <a name="create-an-srv-record"></a>Skapa en SRV-post

När du skapar en [SRV-postuppsättning](dns-zones-records.md#srv-records), ange hello  *\_service* och  *\_protokollet* i hello postuppsättning namn. Det finns inget behov av tooinclude ”@” i hello postuppsättning namn när du skapar en SRV-post på hello zonens apex.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "_sip._tls" SRV --priority 10 --weight 5 --port 8080 --target "sip.contoso.com"
```

### <a name="create-a-txt-record"></a>Skapa en TXT-post

hello följande exempel visas hur toocreate en TXT-posten. Mer information om hello stränglängden som stöds i TXT-poster finns [TXT-poster](dns-zones-records.md#txt-records).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-txt TXT --text "This is a TXT record"
```

## <a name="get-a-record-set"></a>Hämta en uppsättning poster

tooretrieve befintliga postuppsättningen, Använd `azure network dns record-set show`. Om du vill ha hjälp, så gå till `azure network dns record-set show -h`.

Som när du skapar en post eller en postuppsättning hello post anger namn måste vara en *relativa* namn, vilket innebär att den inte får innehålla hello zonnamnet. Du måste också toospecify hello-posttypen, hello zon som innehåller hello post ange och hello resursgruppen som innehåller hello zonen.

hello följande exempel hämtas hello post *www* av typen A från zonen *contoso.com* i resursgruppen *MyResourceGroup*:

```azurecli
azure network dns record-set show MyResourceGroup contoso.com www A
```

## <a name="list-record-sets"></a>Lista över postuppsättningar

Du kan visa alla poster i en DNS-zon med hjälp av hello `azure network dns record-set list` kommando. Om du vill ha hjälp, så gå till `azure network dns record-set list -h`.

Det här exemplet returnerar alla postuppsättningar i zonen hello *contoso.com*, i resursgrupp *MyResourceGroup*, oavsett namn eller posttyp:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```

Det här exemplet returnerar alla uppsättningar av poster som matchar hello angivna posttypen (i det här fallet ”A” poster):

```azurecli
azure network dns record-set list MyResourceGroup contoso.com --type A
```

## <a name="add-a-record-tooan-existing-record-set"></a>Lägg till en post tooan befintliga uppsättningen av poster

Du kan använda `azure network dns record-set add-record` både toocreate en post i en ny post, eller tooadd en befintlig post tooan-post.

Mer information finns i [skapa en DNS-post](#create-a-dns-record) och [skapa poster för andra typer](#create-records-of-other-types) ovan.

## <a name="remove-a-record-from-an-existing-record-set"></a>Ta bort en post från en befintlig post.

tooremove ett DNS-posten från en befintlig postuppsättningen, Använd `azure network dns record-set delete-record`. Om du vill ha hjälp, så gå till `azure network dns record-set delete-record -h`.

Det här kommandot tar bort en DNS-post från en postuppsättning. Om hello sista posten i en postuppsättning raderas hello-postuppsättning själva är **inte** tas bort. I stället lämnas en tom postuppsättning. toodelete hello-postuppsättning i stället Se [ta bort en postuppsättning](#delete-a-record-set).

Du behöver toospecify hello poster toobe bort och hello zon den ska tas bort från, med hjälp av hello samma parametrar som när du skapar en post med hjälp av `azure network dns record-set add-record`. Dessa parametrar beskrivs i [skapa en DNS-post](#create-a-dns-record) och [skapa poster för andra typer](#create-records-of-other-types) ovan.

Det här kommandot uppmanas att bekräfta. Den här uppmaningen kan undertryckas med hjälp av hello `--quiet` växla (kort form `-q`).

följande exempel tar bort hello hello post med värdet '1.2.3.4' från hello post uppsättning med namnet *www* i hello zon *contoso.com*, i resursgrupp hello *MyResourceGroup*. hello bekräftelse ignoreras.

```azurecli
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4 --quiet
```

## <a name="modify-an-existing-record-set"></a>Ändra en befintlig postuppsättning

Varje post innehåller en [time to live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), och DNS-poster. hello följande avsnitt beskrivs hur toomodify av de här egenskaperna.

### <a name="toomodify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a>toomodify A, AAAA, MX, NS, PTR, SRV och TXT-post

toomodify en befintlig post av typen A, AAAA, MX, NS, PTR, SRV och TXT, du måste först lägga till en ny post och delete hello befintlig post. Detaljerade anvisningar om hur toodelete och lägga till poster, se hello tidigare avsnitt i den här artikeln.

hello som följande exempel visar hur toomodify en ”A”-post från IP-adress: 1.2.3.4 tooIP adressen 5.6.7.8:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 5.6.7.8
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

### <a name="toomodify-a-cname-record"></a>toomodify en CNAME-post

toomodify en CNAME-post använder `azure network dns record-set add-record` tooadd hello nya poster värdet. Till skillnad från andra typer av poster, får en CNAME-postuppsättning endast innehålla en enskild post. Hello befintlig post är därför *ersättas* när hello ny post läggs och behöver inte toobe bort separat.  Du kommer att tillfrågas tooaccept denna ersättning.

hello exempel ändrar hello CNAME-postuppsättning *www* i hello zon *contoso.com*, i resursgrupp *MyResourceGroup*, toopoint för 'www.fabrikam.net' i stället för dess befintliga värdet:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www CNAME --cname www.fabrikam.net
``` 

### <a name="toomodify-an-soa-record"></a>toomodify en SOA-post

Använd `azure network dns record-set set-soa-record` toomodify hello SOA för en viss DNS-zon. Om du vill ha hjälp, så gå till `azure network dns record-set set-soa-record -h`.

hello följande exempel visas hur tooset hello e-post-egenskapen för hello SOA-posten för hello zonen *contoso.com* i hello resursgruppen *MyResourceGroup*:

```azurecli
azure network dns record-set set-soa-record rg1 contoso.com --email admin.contoso.com
```


### <a name="toomodify-ns-records-at-hello-zone-apex"></a>toomodify NS-poster på hello zonens apex

hello NS-postuppsättning på zonens apex hello skapas automatiskt med varje DNS-zon. Den innehåller hello namnen på hello Azure DNS-namnet servrar tilldelade toohello zon.

Du kan lägga till ytterligare namn servrar toothis NS postuppsättningen, toosupport samordna värd domäner med mer än en DNS-leverantör. Du kan också ändra hello TTL-värde och metadata för den här postuppsättningen. Du kan inte ta bort eller ändra hello förifyllda Azure DNS-namnservrar.

Observera att detta gäller endast toohello NS postuppsättning på zonens apex hello. Andra NS-postuppsättningar i zonen (som används toodelegate underordnade zoner) kan ändras utan begränsning.

hello som följande exempel visar hur tooadd en ytterligare toohello NS namnserverposten ange på hello zonens apex:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="toomodify-hello-ttl-of-an-existing-record-set"></a>Ange toomodify hello TTL-värde på en befintlig post

Ange toomodify hello TTL-värde på en befintlig post använder `azure network dns record-set set`. Om du vill ha hjälp, så gå till `azure network dns record-set set -h`.

hello som följande exempel visar hur toomodify en postuppsättning TTL-värde, i detta fall too60 sekunder:

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --ttl 60
```

### <a name="toomodify-hello-metadata-of-an-existing-record-set"></a>Ange toomodify hello metadata för en befintlig post

[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan vara används tooassociate programspecifika data med varje postuppsättningen, som nyckel / värde-par. toomodify hello metadata för en befintlig post angetts använder `azure network dns record-set set`. Om du vill ha hjälp, så gå till `azure network dns record-set set -h`.

hello följande exempel visas hur toomodify en postuppsättning med två metadataposter ”Avd = ekonomi” och ”miljö = produktion”, med hjälp av hello `--metadata` parameter (kort form `-m`). Observera att alla befintliga metadata är *ersättas* av hello värdena.

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

## <a name="delete-a-record-set"></a>Ta bort en uppsättning poster

Postuppsättningar kan tas bort med hjälp av hello `azure network dns record-set delete` kommando. Om du vill ha hjälp, så gå till `azure network dns record-set delete -h`. En postuppsättning också tar bort alla poster inom hello uppsättningen av poster.

> [!NOTE]
> Du kan inte ta bort hello SOA- och NS postuppsättningar vid basdomänen hello (`-Name "@"`).  Dessa skapas automatiskt när hello zonen skapades och tas bort automatiskt när hello zon tas bort.

hello följande exempel tar bort hello-postuppsättning med namnet *www* av typen A från hello zon *contoso.com* i resursgruppen *MyResourceGroup*:

```azurecli
azure network dns record-set delete MyResourceGroup contoso.com www A
```

Du kan ange tooconfirm hello borttagningsåtgärd. toosuppress detta kommandotolk, använda hello `--quiet` växla (kort form `-q`).

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [zoner och poster i Azure DNS](dns-zones-records.md).
<br>
Lär dig hur för[skydda zoner och poster](dns-protect-zones-recordsets.md) när du använder Azure DNS.
