---
title: "aaaManage DNS registrera uppsättningar och poster med Azure DNS | Microsoft Docs"
description: "Azure DNS ger hello kapaciteten toomanage DNS-post anger och registrerar när värd för din domän."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ed44a1-7bfe-454f-964e-922ad978264a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 2e62d017341589eaf8d1f8df2fe5db4b973381d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-record-sets-by-using-hello-azure-portal"></a>Hantera DNS-poster och postuppsättningar med hello Azure-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Den här artikeln visar hur toomanage postuppsättningar och poster för DNS-zonen med hjälp av hello Azure-portalen.

Det är viktigt toounderstand hello skillnaden mellan DNS-postuppsättningar och enskilda DNS-poster. En postuppsättning är en uppsättning poster i en zon som har hello samma namn och är hello samma skriver. Mer information finns i [skapa DNS-postuppsättningar och poster med hjälp av hello Azure-portalen](dns-getstarted-create-recordset-portal.md).

## <a name="create-a-new-record-set-and-record"></a>Skapa en ny postuppsättning och registrera

toocreate en postuppsättning i hello Azure-portalen finns [skapa DNS-poster med hjälp av hello Azure-portalen](dns-getstarted-create-recordset-portal.md).

## <a name="view-a-record-set"></a>Visa en postuppsättning

1. I hello Azure-portalen, går toohello **DNS-zonen** bladet.
2. Sök efter hello postuppsättning och markera den. Hello postuppsättning egenskaper öppnas.

    ![Sök efter en postuppsättning](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-tooa-record-set"></a>Lägg till en ny post tooa postuppsättning

Du kan lägga upp too20 poster tooany postuppsättning. En postuppsättning får inte innehålla två identiska poster. Tom postuppsättningar (med noll poster) kan skapas, men visas inte i hello Azure DNS-namnservrar. Postuppsättningar av typen CNAME kan som mest innehålla en post.

1. På hello **postuppsättning egenskaper** bladet för din DNS-zonen på hello post anges som du vill tooadd en post.

    ![Välj en uppsättning poster](./media/dns-operations-recordsets-portal/selectset500.png)

2. Ange hello postuppsättning egenskaper i hello fält.

    ![Lägga till en post](./media/dns-operations-recordsets-portal/addrecord500.png)

3. Klicka på **spara** på hello överkant hello bladet toosave dina inställningar. Stäng sedan hello-bladet.
4. I hello hörnet ser du att hello post sparas.

    ![Spara uppsättningen av poster](./media/dns-operations-recordsets-portal/saving150.png)

När hello posten har sparats, hello värden på hello **DNS-zonen** bladet visar hello nya posten.

## <a name="update-a-record"></a>Uppdatera en post

När du uppdaterar en post i en befintlig postuppsättning beroende hello fält som du kan uppdatera hello typ av post du arbetar med.

1. På hello **postuppsättning egenskaper** bladet för postuppsättningen, Sök efter hello-post.
2. Ändra hello posten. När du ändrar en post kan ändra du hello tillgängliga inställningar för hello-post. I följande exempel hello, hello **IP-adress** är markerat och hello IP-adress är i hello processen att ändras.

    ![Ändra en post](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Klicka på **spara** på hello överkant hello bladet toosave dina inställningar. I hello övre högra hörnet ser du hello-meddelande som hello posten har sparats.

    ![Uppsättningen av poster](./media/dns-operations-recordsets-portal/saved150.png)

När hello posten har sparats, hello värden för hello post ange på hello **DNS-zonen** bladet visar hello uppdatera posten.

## <a name="remove-a-record-from-a-record-set"></a>Ta bort en post från en postuppsättning

Du kan använda hello Azure portal tooremove poster från en postuppsättning. Observera att hello sista posten från en postuppsättning inte bort hello uppsättningen av poster.

1. På hello **postuppsättning egenskaper** bladet för postuppsättningen, Sök efter hello-post.
2. Klicka på hello-post som du vill tooremove. Välj sedan **ta bort**.

    ![Ta bort en post](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Klicka på **spara** på hello överkant hello bladet toosave dina inställningar.
4. När hello har tagits bort, hello värden för hello-post på hello **DNS-zonen** bladet visar hello borttagning.

## <a name="delete"></a>Ta bort en uppsättning poster

1. På hello **postuppsättning egenskaper** bladet för postuppsättningen, klickar du på **ta bort**.

    ![Ta bort en uppsättning poster](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. Ett meddelande visas som frågar om du vill toodelete hello postuppsättning.
3. Kontrollera att hello namn matchar hello post du vill toodelete och klicka sedan på **Ja**.
4. På hello **DNS-zonen** bladet Kontrollera hello postuppsättning inte längre visas.

## <a name="work-with-ns-and-soa-records"></a>Arbeta med NS och SOA-poster

NS och SOA-poster som skapas automatiskt hanteras annorlunda från andra typer av poster.

### <a name="modify-soa-records"></a>Ändra SOA-poster

Du kan inte lägga till eller ta bort poster från hello skapas automatiskt SOA-postuppsättning på zonens apex hello (namn = ”@”). Men du kan ändra någon av parametrarna hello inom hello SOA-post (utom ”värd”) och hello postuppsättning TTL-värde.

### <a name="modify-ns-records-at-hello-zone-apex"></a>Ändra NS-poster på hello zonens apex

hello NS-postuppsättning på zonens apex hello skapas automatiskt med varje DNS-zon. Den innehåller hello namnen på hello Azure DNS-namnet servrar tilldelade toohello zon.

Du kan lägga till ytterligare namn servrar toothis NS postuppsättningen, toosupport samordna värd domäner med mer än en DNS-leverantör. Du kan också ändra hello TTL-värde och metadata för den här postuppsättningen. Du kan inte ta bort eller ändra hello förifyllda Azure DNS-namnservrar.

Observera att detta gäller endast toohello NS postuppsättning på zonens apex hello. Andra NS-postuppsättningar i zonen (som används toodelegate underordnade zoner) kan ändras utan begränsning.

### <a name="delete-soa-or-ns-record-sets"></a>Ta bort SOA- eller NS postuppsättningar

Du kan inte ta bort hello SOA- och NS postuppsättningar vid hello zonens apex (namn = ”@”) som skapas automatiskt när hello zonen skapas. De tas bort automatiskt när du tar bort hello zonen.

## <a name="next-steps"></a>Nästa steg

* Mer information om Azure DNS finns hello [översikt över Azure DNS](dns-overview.md).
* Mer information om hur du automatiserar DNS finns [skapar DNS-zoner och postuppsättningar med hello .NET SDK](dns-sdk.md).
* Mer information om omvänd DNS-poster finns [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md).
