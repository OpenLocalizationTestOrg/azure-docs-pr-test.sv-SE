---
title: "aaaGet igång med Azure DNS med hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate ett DNS-zonen och posten i Azure DNS. Detta är en stegvis guide toocreate och hantera dina första DNS-zonen och posten med hello Azure-portalen."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 5cea01d840d794001cccac64defed8b329d948db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-hello-azure-portal"></a>Kom igång med Azure DNS med hello Azure-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Den här artikeln vägleder dig genom hello steg toocreate din första DNS-zonen och posten med hello Azure-portalen. Du kan också göra detta med hjälp av Azure PowerShell eller hello plattformsoberoende Azure CLI.

En DNS-zon är används toohost hello DNS-poster för en viss domän. toostart som värd för din domän i Azure DNS, behöver du toocreate en DNS-zon för domännamnet. Varje DNS-post för din domän skapas sedan i den här DNS-zonen. Slutligen toopublish DNS-zonen toohello Internet, behöver du tooconfigure hello namnservrar för hello domän. Var och en av dessa steg beskrivs i följande hello.

## <a name="create-a-dns-zone"></a>Skapa en DNS-zon

1. Logga in toohello Azure-portalen
2. Hej hubbmenyn, klicka på och klicka på **New > nätverk >** och klicka sedan på **DNS-zonen** tooopen hello skapa DNS-zonen bladet.

    ![DNS-zon](./media/dns-getstarted-portal/openzone650.png)

4. På hello **skapa DNS-zonen** bladet ange hello följande värden, och klicka sedan på **skapa**:


   | **Inställning** | **Värde** | **Detaljer** |
   |---|---|---|
   |**Namn**|contoso.com|hello namnet på hello DNS-zonen|
   |**Prenumeration**|[Din prenumeration]|Välj en prenumeration toocreate hello DNS-zonen i.|
   |**Resursgrupp**|**Skapa ny:** contosoDNSRG|Skapa en resursgrupp. hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt. Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) översiktsartikel.|
   |**Plats**|Västra USA||

> [!NOTE]
> hello resursgruppen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello DNS-zon. hello DNS-zonen plats är alltid ”globala” och visas inte.

## <a name="create-a-dns-record"></a>Skapa en DNS-post

hello vägleder följande exempel dig genom hello processen att skapa nya ”A” post. Andra typer av poster och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hjälp av hello Azure-portalen](dns-operations-recordsets-portal.md). 

1. Med hello skapade DNS-zonen i hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**. Klicka på hello **contoso.com** DNS-zonen i hello bladet för alla resurser. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **contoso.com** i hello **filtrera efter namn...** rutan tooeasily åtkomst hello DNS-zon.

1. Hello överst i hello **DNS-zonen** bladet väljer **+ postuppsättningen** tooopen hello **lägga till postuppsättning** bladet.

1. På hello **lägga till postuppsättning** bladet anger hello följande värden och klickar på **OK**. I det här exemplet skapar du en A-post.

   |**Inställning** | **Värde** | **Detaljer** |
   |---|---|---|
   |**Namn**|www|Namnet på hello-post|
   |**Typ**|A| Typ av DNS-poster toocreate, giltiga värden är A, AAAA, CNAME, MX, NS, SRV, TXT och PTR.  Mer information om posttyper finns i [Översikt över DNS-zoner och poster](dns-zones-records.md)|
   |**TTL**|1|Time-to-live för hello DNS-begäran.|
   |**TTL-enhet**|Timmar|Tidsmått för TTL-värde.|
   |**IP-adress**|ipAddressValue| Det här värdet är hello IP-adress som löser hello DNS-post.|

## <a name="view-records"></a>Visa poster

I hello längst ned på bladet för hello DNS-zonen ser du hello poster för hello DNS-zonen. Du bör se hello standard DNS- och SOA-poster, som skapas i varje zon, plus eventuella nya poster som du har skapat.

![zon](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Uppdatera namnservrar

När du är nöjd att din DNS-zonen och poster har ställts in korrekt behöver tooconfigure ditt domännamn toouse hello Azure DNS-namnservrar. Detta gör att andra användare i hello Internet toofind DNS-poster.

hello namnservrar för zonen anges i hello Azure-portalen:

![zon](./media/dns-getstarted-portal/viewzonens500.png)

Dessa namnservrar ska konfigureras med hello domännamnsregistratorn (där du har köpt hello domännamn). Din registrator erbjuder hello alternativet tooset in hello namnservrar för hello domän. Mer information finns i [Delegera din domän tooAzure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Ta bort alla resurser

toodelete alla resurser skapas i den här artikeln, fullständig hello följande steg:

1. I hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**. Klicka på hello **MyResourceGroup** resursgrupp i hello bladet för alla resurser. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **MyResourceGroup** i hello **filtrera efter namn...** rutan tooeasily åtkomst hello resursgruppens namn.
1. I hello **MyResourceGroup** bladet, klickar du på hello **ta bort** knappen.
1. hello portalen måste du tootype hello namnet på hello resurs grupp tooconfirm som du vill toodelete den. Klicka på **ta bort**, typen *MyResourceGroup* hello resursgruppens namn, sedan klickar du på **ta bort**. Tar bort en resursgrupp alla resurser inom hello resursgrupp, så alltid att tooconfirm hello innehållet i en resursgrupp innan den tas bort. hello portal tar bort alla resurser som ingår i hello resursgrupp och sedan tar bort hello resursgruppen sig själv. Den här processen tar flera minuter.


## <a name="next-steps"></a>Nästa steg

toolearn mer om Azure DNS finns [översikt över Azure DNS](dns-overview.md).

toolearn mer information om hur du hanterar DNS-poster i Azure DNS finns [hantera DNS-poster och postuppsättningar med hjälp av hello Azure-portalen](dns-operations-recordsets-portal.md).

