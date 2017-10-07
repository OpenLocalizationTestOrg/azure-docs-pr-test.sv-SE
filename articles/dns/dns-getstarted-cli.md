---
title: "aaaGet igång med Azure DNS använder Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toocreate ett DNS-zonen och posten i Azure DNS. Detta är en stegvis guide toocreate och hantera dina första DNS-zonen och posten med hello Azure CLI 2.0."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 8a894941e9910d5cc35394a1be9dbca9792613f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-20"></a>Komma igång med Azure DNS med hjälp av Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Den här artikeln vägleder dig genom hello steg toocreate din första DNS-zonen och posten med hello plattformsoberoende Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux. Du kan också utföra dessa steg med hello Azure-portalen eller Azure PowerShell.

En DNS-zon är används toohost hello DNS-poster för en viss domän. toostart som värd för din domän i Azure DNS, behöver du toocreate en DNS-zon för domännamnet. Varje DNS-post för din domän skapas sedan i den här DNS-zonen. Slutligen toopublish DNS-zonen toohello Internet, behöver du tooconfigure hello namnservrar för hello domän. Dessa steg beskrivs nedan.

Dessa instruktioner förutsätter att du redan har installerat och inloggad tooAzure CLI 2.0. Mer information finns [hur toomanage DNS zoner med hjälp av Azure CLI 2.0](dns-operations-dnszones-cli.md).

## <a name="create-hello-resource-group"></a>Skapa hello resursgrupp

Innan du skapar hello DNS-zonen skapas en resursgrupp toocontain hello DNS-zon. hello följande visar hello-kommando.

```azurecli
az group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Skapa en DNS-zon

En DNS-zon skapas med hjälp av hello `az network dns zone create` kommando. toosee hjälp för det här kommandot skriver `az network dns zone create -h`.

hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgruppen *MyResourceGroup*. Använd hello exempel toocreate en DNS-zon ersätter hello värden för din egen.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```


## <a name="create-a-dns-record"></a>Skapa en DNS-post

toocreate DNS-post och använda hello `az network dns record-set [record type] add-record` kommando. Mer information, om exempelvis A-poster, finns i `azure network dns record-set A add-record -h`.

hello följande exempel skapas en post med hello relativa namnet ”www” i hello DNS-zonen ”contoso.com” i resursgruppen ”MyResourceGroup”. hello fullständigt kvalificerade namnet på postuppsättningen hello är ”www.contoso.com”. hello-posttypen är ”A”, med IP-adress ”1.2.3.4” och en standard-TTL 3600 sekunder (1 timme) används.

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

För andra posttyper postuppsättningar med fler än en post för den alternativa TTL-värden och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hello Azure CLI 2.0](dns-operations-recordsets-cli.md).


## <a name="view-records"></a>Visa poster

toolist hello DNS-poster i zonen, Använd:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```


## <a name="update-name-servers"></a>Uppdatera namnservrar

När du är nöjd att din DNS-zonen och poster har ställts in korrekt behöver tooconfigure ditt domännamn toouse hello Azure DNS-namnservrar. Detta gör att andra användare i hello Internet toofind DNS-poster.

hello namnservrar för zonen ges av hello `az network dns zone show` kommando. servernamn toosee hello namn, Använd JSON-utdata, som visas i följande exempel hello.

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Dessa namnservrar ska konfigureras med hello domännamnsregistratorn (där du har köpt hello domännamn). Din registrator kommer att erbjuda hello alternativet tooset in hello namnservrar för hello domän. Mer information finns i [Delegera din domän tooAzure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Ta bort alla resurser
 
toodelete alla resurser skapas i den här artikeln tar hello följande steg:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Nästa steg

toolearn mer om Azure DNS finns [översikt över Azure DNS](dns-overview.md).

toolearn mer information om hur du hanterar DNS-zoner i Azure DNS finns [hantera DNS-zoner i Azure DNS använder Azure CLI 2.0](dns-operations-dnszones-cli.md).

toolearn mer information om hur du hanterar DNS-poster i Azure DNS finns [hantera DNS-poster och registrera anger i Azure DNS använder Azure CLI 2.0](dns-operations-recordsets-cli.md).
