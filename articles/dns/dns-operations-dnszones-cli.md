---
title: aaaManage DNS-zoner i Azure DNS - Azure CLI 2.0 | Microsoft Docs
description: "Du kan hantera DNS-zoner som använder Azure CLI 2.0. Den här artikeln visar hur tooupdate, ta bort och skapa DNS-zoner på Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
ms.openlocfilehash: 3945a558b2db3490e50678d8395a47e55a85c8fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-20"></a>Hur toomanage DNS-zoner i Azure DNS med hello Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)


Den här guiden visar hur toomanage DNS-zoner med hjälp av hello plattformsoberoende Azure CLI, som är tillgänglig för Windows, Mac och Linux. Du kan också hantera DNS-zoner med [Azure PowerShell](dns-operations-dnszones.md) eller hello Azure-portalen.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet

Du kan göra hello med hjälp av något av följande versioner av CLI hello:

* [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -vår CLI för hello klassisk och resurs management distributionsmodeller.
* [Azure CLI 2.0](dns-operations-dnszones-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering.

## <a name="introduction"></a>Introduktion

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Konfigurera Azure CLI 2.0 för Azure DNS

### <a name="before-you-begin"></a>Innan du börjar

Kontrollera att du har hello följande objekt innan du börjar din konfiguration.

* En Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).

* Installera hello senaste versionen av hello Azure CLI 2.0, tillgänglig för Windows, Linux och MAC. Mer information finns på [installera hello Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

### <a name="sign-in-tooyour-azure-account"></a>Logga in tooyour Azure-konto

Öppna ett konsolfönster och autentisera med dina autentiseringsuppgifter. Mer information finns i loggen i tooAzure från hello Azure CLI

```
az login
```

### <a name="select-hello-subscription"></a>Välj hello-prenumeration

Kontrollera hello prenumerationer för hello-kontot.

```
az account list
```

Välj vilka av dina Azure-prenumerationer toouse.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Azure Resource Manager kräver att alla resursgrupper anger en plats. Detta används som hello standardplatsen för resurser i resursgruppen. Men eftersom alla DNS-resurser är globala, inte regional, har hello valet av resursgruppens plats ingen inverkan på Azure DNS.

Du kan hoppa över det här steget om du använder en befintlig resursgrupp.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>Få hjälp

Alla CLI 2.0-kommandon som rör tooAzure DNS börja med `az network dns`. Hjälp är tillgänglig för varje kommando med hello `--help` alternativet (kort form `-h`).  Exempel:

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>Skapa en DNS-zon

En DNS-zon skapas med hjälp av hello `az network dns zone create` kommando. Om du vill ha hjälp, så gå till `az network dns zone create -h`.

hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgrupp med namnet *MyResourceGroup*:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a>toocreate en DNS-zon med taggar

hello följande exempel visas hur toocreate ett DNS zonen med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*, med hjälp av hello `--tags` Parametern (kort form `-t`):

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>Hämta en DNS-zon

Använd tooretrieve en DNS-zon `az network dns zone show`. Om du vill ha hjälp, så gå till `az network dns zone show --help`.

hello följande exempel returnerar hello DNS-zonen *contoso.com* och dess associerade data från resursgruppen *MyResourceGroup*. 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

följande exempel hello är hello svar.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Observera att DNS-poster inte returneras av `az network dns zone show`. toolist DNS-poster, Använd `az network dns record-set list`.


## <a name="list-dns-zones"></a>Lista över DNS-zoner

tooenumerate DNS-zoner, Använd `az network dns zone list`. Om du vill ha hjälp, så gå till `az network dns zone list --help`.

Resursgruppen för att ange hello visas zonerna inom hello resursgrupp:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Utelämna hello resursgrupp visar en lista över alla zoner i prenumerationen hello:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>Uppdatera en DNS-zon

Ändringar tooa DNS-zonresurs kan göras med hjälp av `az network dns zone update`. Om du vill ha hjälp, så gå till `az network dns zone update --help`.

Det här kommandot uppdaterar inte någon hello DNS uppsättningar av poster i zonen hello (se [hur tooManage DNS-poster](dns-operations-recordsets-cli.md)). Det är bara använda tooupdate egenskaper för hello zonresurs sig själv. Dessa egenskaper är för närvarande begränsad toohello [Azure Resource Manager ”-taggar'](dns-zones-records.md#tags) för hello zonen resurs.

hello följande exempel visas hur tooupdate hello taggar på en DNS-zon. hello befintliga taggar ersätts med hello-värde som angetts.

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>Ta bort en DNS-zon

DNS-zoner kan tas bort med `az network dns zone delete`. Om du vill ha hjälp, så gå till `az network dns zone delete --help`.

> [!NOTE]
> En DNS-zon också tar du bort DNS-poster i zonen hello. Den här åtgärden kan inte ångras. Om hello DNS-zon används, misslyckas tjänster med hjälp av hello zon när hello zon tas bort.
>
>tooprotect mot oavsiktlig zonen radering finns [hur tooprotect DNS zoner och registrerar](dns-protect-zones-recordsets.md).

Det här kommandot uppmanas att bekräfta. hello valfria `--yes` växeln förhindrar det här meddelandet.

hello följande exempel visas hur toodelete hello zonen *contoso.com* från resursgruppen *MyResourceGroup*.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>Nästa steg

Lär dig hur för[hantera uppsättningar av poster och poster](dns-getstarted-create-recordset-cli.md) i DNS-zonen.

Lär dig hur för[Delegera din domän tooAzure DNS](dns-domain-delegation.md).

