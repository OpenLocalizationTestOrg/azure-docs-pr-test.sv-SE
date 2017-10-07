---
title: aaaManage DNS-zoner i Azure DNS - Azure-portalen | Microsoft Docs
description: "Du kan hantera DNS-zoner med hello Azure-portalen. Den här artikeln beskriver hur tooupdate, ta bort och skapa DNS-zoner på Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: 0d8ce302bb7126dfe8077a6f3e33418e16fcea64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-hello-azure-portal"></a>Hur toomanage DNS-zoner i hello Azure-portalen

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Den här artikeln visar hur toomanage DNS-zoner med hjälp av hello Azure-portalen. Du kan också hantera DNS-zoner med hello plattformsoberoende [Azure CLI](dns-operations-dnszones-cli.md) eller hello Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>Skapa en DNS-zon

1. Logga in toohello Azure-portalen
2. Hej hubbmenyn, klicka på och klicka på **New > nätverk >** och klicka sedan på **DNS-zonen** tooopen hello skapa DNS-zonen bladet.

    ![DNS-zon](./media/dns-operations-dnszones-portal/openzone650.png)

4. På hello **skapa DNS-zonen** bladet ange hello följande värden, och klicka sedan på **skapa**:


   | **Inställning** | **Värde** | **Detaljer** |
   |---|---|---|
   |**Namn**|contoso.com|hello namnet på hello DNS-zonen|
   |**Prenumeration**|[Din prenumeration]|Välj en prenumeration toocreate hello DNS-zonen i.|
   |**Resursgrupp**|**Skapa ny:** contosoDNSRG|Skapa en resursgrupp. hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt. Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) översiktsartikel.|
   |**Plats**|Västra USA||

> [!NOTE]
> hello resursgruppen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello DNS-zon. hello DNS-zonen plats är alltid ”globala” och visas inte.

## <a name="list-dns-zones"></a>Lista över DNS-zoner

I hello Azure portal, navigerar för**fler tjänster** > **nätverk** > **DNS-zoner**. Varje DNS-zon är det egna resurs, information, till exempel antal post och namnservrar kan visas i den här vyn. hello kolumnen **NAMNSERVRAR** finns inte i hello standardvyn, tooadd det klickar du på **kolumner**väljer **namnservrar** och på **klar**.

![Visar en lista över DNS-zoner](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Ta bort en DNS-zon

Navigera tooa DNS-zonen i hello-portalen. På hello **DNS-zonen** bladet, klickar du på **ta bort zon**. Du kan ange tooconfirm du förskjutning toodelete hello DNS-zon. En DNS-zon också tar du bort alla hello-poster som ingår i hello zon.

## <a name="next-steps"></a>Nästa steg

Lär dig hur toowork med DNS-zon och poster genom att besöka [Kom igång med Azure DNS med hello Azure-portalen](dns-getstarted-portal.md).
