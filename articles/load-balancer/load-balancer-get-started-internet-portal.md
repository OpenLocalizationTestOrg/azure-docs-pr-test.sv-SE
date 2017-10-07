---
title: "belastningsutjämnaren aaaCreate mot en Internet - Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate en Internetriktade belastningsutjämnare i Resource Manager med hjälp av hello Azure-portalen"
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: annahar
ms.openlocfilehash: 390ba8ec1474c54cf2c0022c4a3c219d21b1a659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-hello-azure-portal"></a>Skapa en Internetriktade belastningsutjämnare använder hello Azure-portalen

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Mall](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello Resource Manager-modellen. Du kan också [Lär dig hur toocreate en Internetriktade belastningsutjämnare använder klassisk distribution](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Detta omfattar hello aktivitetssekvensen enskilda som har utfärdat toobe toocreate en belastningsutjämnare och förklarar i detalj vad görs tooaccomplish hello målet.

## <a name="what-is-required-toocreate-an-internet-facing-load-balancer"></a>Vad är obligatoriska toocreate en Internetriktade belastningsutjämnare?

Du behöver toocreate och konfigurera hello följande objekt toodeploy belastningsutjämning.

* IP-konfiguration på klientsidan – innehåller offentliga IP-adresser för inkommande nätverkstrafik.
* Backend-adresspool - innehåller nätverksgränssnitt (NIC) för virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren.
* Belastningsutjämning regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooport i hello backend-adresspool.
* Inkommande NAT-regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool.
* Avsökningar - innehåller hälsa-avsökningar som används toocheck tillgängligheten för virtuella datorer instanser i hello backend-adresspool.

Mer information om belastningsutjämningskomponenter för Azure Resource Manager finns i [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Belastningsutjämningsstöd i Azure Resource Manager).

## <a name="set-up-a-load-balancer-in-azure-portal"></a>Konfigurera en belastningsutjämnare på Azure Portal

> [!IMPORTANT]
> I det här exemplet förutsätter vi att du har ett virtuellt nätverk som heter **myVNet**. Se för[skapa virtuellt nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) toodo detta. Det förutsätts även att det finns ett undernät i **myVNet** kallas **LB undernät vara** och två virtuella datorer kallas **web1** och **web2** respektive i Hej samma tillgänglighetsuppsättning kallas **myAvailSet** i **myVNet**. Se för[länken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toocreate virtuella datorer.

1. Från en webbläsare navigerar toohello Azure-portalen: [http://portal.azure.com](http://portal.azure.com) och logga in med ditt Azure-konto.
2. Välj på hello översta vänstra sidan av hello-skärmen **ny** > **nätverk** > **belastningsutjämnaren.**
3. I hello **skapa belastningsutjämnaren** bladet, ange ett namn för din belastningsutjämnare. I vårt exempel använder vi **myLoadBalancer**.
4. Under **Typ** väljer du **Offentlig**.
5. Under **Offentlig IP-adress** skapar du en ny offentlig IP-adress med namnet **myPublicIP**.
6. Under Resursgrupp väljer du **myRG**. Välj en lämplig **plats** och klicka sedan på **OK**. hello belastningsutjämnaren startar sedan toodeploy och tar några minuter toosuccessfully slutförd distribution.

    ![Uppdatera resursgruppen för belastningsutjämnaren](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)

## <a name="create-a-back-end-address-pool"></a>Skapa en backend-adresspool

1. När belastningsutjämnaren har distribuerats väljer du den från dina resurser. Välj Serverdelspooler under Inställningar. Skriv ett namn för poolen. Klicka på hello **Lägg till** knappen i hello övre delen av hello bladet som visas.
2. Klicka på **lägga till en virtuell dator** i hello **lägga till serverdelspoolen** bladet.  Välj **Välj en tillgänglighetsuppsättning** under **Tillgänglighetsuppsättning** och välj **myAvailSet**. Välj därefter **Välj hello virtuella datorerna** under hello delen med virtuella datorer i hello bladet och klicka på **web1** och **web2**, hello två virtuella datorer som skapades för belastningsutjämning. Se till att både blå markeringar toohello lämnas enligt hello bilden nedan. Klicka på **Välj** i det bladet följt av OK i hello **Välj virtuella datorer** bladet och sedan **OK** i hello **lägga till serverdelspoolen** bladet.

    ![Lägga till toohello serverdelsadresspool- ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Kontrollera att meddelanden listrutan toomake har en uppdatering angående sparar hello belastningsutjämnarens serverdelspool i tillägget tooupdating hello nätverksgränssnittet för både hello VMs **web1** och **web2**.

## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Skapa en avsökning, LB-regel och NAT-regler

1. Skapa en hälsoavsökning.

    Välj Avsökningar under Inställningar för belastningsutjämnaren. Klicka på **Lägg till** finns hello överst i hello-bladet.

    Det finns två sätt tooconfigure en avsökning: HTTP eller TCP. I det här exemplet använder vi HTTP, men TCP kan konfigureras på liknande sätt.
    Uppdatera hello nödvändig information. Som vi redan nämnt belastningsutjämnar **myLoadBalancer** trafik på port 80. hello markerade sökvägen är HealthProbe.aspx, är 15 sekunder och tröskelvärde för ohälsosamt värde är 2. När du är klar klickar du på **OK** toocreate hello avsökning.

    Du muspekaren över hello ”i” ikonen toolearn mer om dessa enskilda konfigurationer och hur de kan vara ändrade toocater tooyour krav.

    ![Lägga till en avsökning](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Skapa en belastningsutjämningsregel.

    Klicka på belastningsutjämningsregler i hello inställningar av din belastningsutjämnare. I nya hello-bladet klickar du på **Lägg till**. Ge regeln ett namn. Här använder vi HTTP. Välj hello klientdel porten och serverdelsporten. Här har vi valt 80 för båda. Välj **LB backend** som serverdelspool och hello skapat **HealthProbe** som hello avsökning. Du kan ange andra konfigurationer enligt tooyour krav. Klicka sedan på OK toosave hello belastningsutjämningsregeln.

    ![Lägga till en belastningsutjämningsregel](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Skapa NAT-regler för inkommande trafik

    Klicka på inkommande NAT-regler under hello inställningar av din belastningsutjämnare. I hello nytt blad med klickar du på **Lägg till**. Ge NAT-regeln för inkommande trafik ett namn. Här använder vi **inboundNATrule1**. hello-målet måste vara hello offentlig IP skapade tidigare. Välj Anpassad under tjänst och välj hello-protokoll som du vill att toouse. Här har vi valt TCP. Ange hello port 3441 och hello målporten i det här fallet 3389. Klicka sedan på OK toosave den här regeln.

    Upprepa det här steget för hello andra inkommande NAT-regeln anropas inboundNATrule2 från port 3442 tooTarget port 3389 När hello första regeln har skapats.

    ![Lägga till en NAT-regel för inkommande trafik](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Ta bort en belastningsutjämnare

toodelete en belastningsutjämnare väljer hello belastningsutjämnare som du vill tooremove. I hello *belastningsutjämnaren* bladet, klickar du på **ta bort** finns hello överst i hello-bladet. Välj **Ja** när du uppmanas att göra det.

## <a name="next-steps"></a>Nästa steg

[Komma igång med att konfigurera en intern belastningsutjämnare](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
