---
title: aaaManage DNS-zoner i Azure DNS - Azure CLI 1.0 | Microsoft Docs
description: "Du kan hantera DNS-zoner som använder Azure CLI 1.0. Den här artikeln visar hur tooupdate, ta bort och skapa DNS-zoner på Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: cb9790cc46626ef7f38a43edb57511104fe6057e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-10"></a>Hur toomanage DNS-zoner i Azure DNS med hello Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Den här guiden visar hur toomanage DNS-zoner med hjälp av hello plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux. Du kan också hantera DNS-zoner med [Azure PowerShell](dns-operations-dnszones.md) eller hello Azure-portalen.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet

Du kan göra hello med hjälp av något av följande versioner av CLI hello:

* [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -vår CLI för hello klassisk och resurs management distributionsmodeller.
* [Azure CLI 2.0](dns-operations-dnszones-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering.

## <a name="introduction"></a>Introduktion

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a>Få hjälp

Alla CLI 1.0-kommandon som rör tooAzure DNS börja med `azure network dns`. Hjälp är tillgänglig för varje kommando med hello `--help` alternativet (kort form `-h`).  Exempel:

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a>Skapa en DNS-zon

En DNS-zon skapas med hjälp av hello `azure network dns zone create` kommando. Om du vill ha hjälp, så gå till `azure network dns zone create -h`.

hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgrupp med namnet *MyResourceGroup*:

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a>toocreate en DNS-zon med taggar

hello följande exempel visas hur toocreate ett DNS zonen med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*, med hjälp av hello `--tags` Parametern (kort form `-t`):

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a>Hämta en DNS-zon

Använd tooretrieve en DNS-zon `azure network dns zone show`. Om du vill ha hjälp, så gå till `azure network dns zone show -h`.

hello följande exempel returnerar hello DNS-zonen *contoso.com* och dess associerade data från resursgruppen *MyResourceGroup*. 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

följande exempel hello är hello svar.

```
info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

Observera att DNS-poster inte returneras av `azure network dns zone show`. toolist DNS-poster, Använd `azure network dns record-set list`.


## <a name="list-dns-zones"></a>Lista över DNS-zoner

tooenumerate DNS-zoner, Använd `azure network dns zone list`. Om du vill ha hjälp, så gå till `azure network dns zone list -h`.

Resursgruppen för att ange hello visas zonerna inom hello resursgrupp:

```azurecli
azure network dns zone list MyResourceGroup
```

Utelämna hello resursgrupp visar en lista över alla zoner i prenumerationen hello:

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a>Uppdatera en DNS-zon

Ändringar tooa DNS-zonresurs kan göras med hjälp av `azure network dns zone set`. Om du vill ha hjälp, så gå till `azure network dns zone set -h`.

Det här kommandot uppdaterar inte någon hello DNS uppsättningar av poster i zonen hello (se [hur tooManage DNS-poster](dns-operations-recordsets-cli-nodejs.md)). Det är bara använda tooupdate egenskaper för hello zonresurs sig själv. Dessa egenskaper är för närvarande begränsad toohello [Azure Resource Manager ”-taggar'](dns-zones-records.md#tags) för hello zonen resurs.

hello följande exempel visas hur tooupdate hello taggar på en DNS-zon. hello befintliga taggar ersätts med hello-värde som angetts.

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a>Ta bort en DNS-zon

DNS-zoner kan tas bort med `azure network dns zone delete`. Om du vill ha hjälp, så gå till `azure network dns zone delete -h`.

> [!NOTE]
> En DNS-zon också tar du bort DNS-poster i zonen hello. Den här åtgärden kan inte ångras. Om hello DNS-zon används, misslyckas tjänster med hjälp av hello zon när hello zon tas bort.
>
>tooprotect mot oavsiktlig zonen radering finns [hur tooprotect DNS zoner och registrerar](dns-protect-zones-recordsets.md).

Det här kommandot uppmanas att bekräfta. hello valfria `--quiet` växla (kort form `-q`) förhindrar det här meddelandet.

hello följande exempel visas hur toodelete hello zonen *contoso.com* från resursgruppen *MyResourceGroup*.

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a>Nästa steg

Lär dig hur för[hantera uppsättningar av poster och poster](dns-getstarted-create-recordset-cli-nodejs.md) i DNS-zonen.

Lär dig hur för[Delegera din domän tooAzure DNS](dns-domain-delegation.md).

