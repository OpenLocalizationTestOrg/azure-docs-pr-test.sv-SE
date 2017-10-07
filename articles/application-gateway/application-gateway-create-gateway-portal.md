---
title: aaaCreate en Programgateway - Azure-portalen | Microsoft Docs
description: "Lär dig hur hello toocreate en Programgateway med hjälp av portalen"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 54dffe95-d802-4f86-9e2e-293f49bd1e06
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 24c1d5701eae372cd233162ceb58dea36a3b6a39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-hello-portal"></a>Skapa en Programgateway med hello-portalen

[Programgateway](application-gateway-introduction.md) är en dedikerad virtuell installation som ger programmet leverans domänkontrollant (ADC) som en tjänst som erbjuder olika layer 7 belastningsutjämning för ditt program. Den här artikeln tar dig igenom hello steg toocreate en Programgateway använda hello Azure-portalen och lägga till en befintlig server som en backend-medlem.

## <a name="log-in-tooazure"></a>Logga in tooAzure

Logga in toohello Azure-portalen på [http://portal.azure.com](http://portal.azure.com)

## <a name="create-application-gateway"></a>Skapa Programgateway

toocreate en Programgateway fullständig hello steg som följer. Programgateway kräver sin egen undernät. När du skapar ett virtuellt nätverk, se till att du lämnar tillräckligt med adressutrymme toohave flera undernät. När hello Programgateway är distribuerade tooa undernät, kan andra programgatewayer läggas tooit.

1. I hello Favoriter hello portalen klickar **ny**
1. I hello **ny** bladet, klickar du på **nätverk**. I hello **nätverk** bladet, klickar du på **Programgateway**som visas i följande bild hello:

    ![Skapa Programgateway][1]

1. I hello **grunderna** bladet som visas, ange hello följande värden, och klicka sedan på **OK**:

   | **Inställning** | **Värde** | **Detaljer**|
   |---|---|---|
   |**Namn**|AdatumAppGateway|hello namnet på hello Programgateway|
   |**Nivå**|Standard|Tillgängliga värden är Standard och Brandvägg. Besök [Brandvägg för webbaserade program](application-gateway-web-application-firewall-overview.md) toolearn mer om Brandvägg.|
   |**SKU-storleken**|Medel|Alternativ när du väljer standardnivån är små, medelstora och stora. När du väljer Brandvägg nivå är alternativen medel och stor bara.|
   |**Instansantal**|2|Antal instanser av hello Programgateway för hög tillgänglighet. Antalet instanser av 1 bör endast användas för testning.|
   |**Prenumeration**|[Din prenumeration]|Välj en prenumeration toocreate hello Programgateway i.|
   |**Resursgrupp**|**Skapa en ny:** AdatumAppGatewayRG|Skapa en resursgrupp. hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt. Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups) översiktsartikel.|
   |**Plats**|Västra USA||

1. I hello **inställningar** bladet som visas under **för virtuella nätverk**, klickar du på **Välj ett virtuellt nätverk**. Hej **Välj virtuellt nätverk** blad öppnas.  Klicka på **Skapa nytt** tooopen hello **skapa virtuellt nätverk** bladet.

   ![Välj ett virtuellt nätverk][2]

1. På hello **skapa virtuellt nätverk-bladet** ange hello följande värden och klicka sedan på **OK**. Hej **skapa virtuellt nätverk** och **Välj virtuellt nätverk** blad stängs. Det här steget fyller hello **undernät** på hello **inställningar** bladet med hello undernät valt.

   | **Inställning** | **Värde** | **Detaljer**|
   |---|---|---|
   |**Namn**|AdatumAppGatewayVNET|Namnet på hello Programgateway|
   |**Adressutrymmet**|10.0.0.0/16|Detta är hello adressutrymmet för hello virtuellt nätverk|
   |**Namn på undernät**|AppGatewaySubnet|Namnet på hello undernät för hello Programgateway|
   |**Adressintervall för undernätet**|10.0.0.0/28|Det här undernätet tillåter flera undernät i hello virtuellt nätverk för medlemmar i serverdelen|

1. På hello **inställningar** bladet under **Frontend-IP-konfiguration**, Välj **offentliga** som hello **IP-adresstyp**

1. På hello **inställningar** bladet under **offentliga IP-adressen** klickar du på **Välj en offentlig IP-adress**, hello **Välj offentlig IP-adress** blad öppnas , klickar du på **Skapa nytt**.

   ![Välj offentlig IP-adress][3]

1. På hello **skapa offentlig IP-adress** bladet acceptera standardvärdet för hello och på **OK**. hello blad stängs och fyller hello **offentliga IP-adressen** med hello offentlig IP-adress som väljs.

1. På hello **inställningar** bladet under **Lyssnarkonfigurationen**, klickar du på **HTTP** under **protokollet**. Ange hello port toouse i hello **Port** fältet.

2. Klicka på **OK** på hello **inställningar** bladet toocontinue.

1. Granska inställningarna för hello på hello **sammanfattning** bladet och klicka på **OK** toostart skapandet av hello Programgateway. Skapa en Programgateway är en tidskrävande uppgift och tar tid toocomplete.

## <a name="add-servers-toobackend-pools"></a>Lägg till servrar toobackend pooler

När du har skapat hello Programgateway måste hello system där hello programmet toobe belastningsutjämnade toobe tillagda toohello Programgateway. hello IP-adresser, FQDN eller nätverkskort av servrarna läggs toohello serverdelsadresspooler.

### <a name="ip-address-or-fqdn"></a>IP-adress eller FQDN

1. Med hello Programgateway som skapats i hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**. Klicka på hello **AdatumAppGateway** Programgateway i hello bladet för alla resurser. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **AdatumAppGateway** i hello **filtrera efter namn...** rutan Programgateway tooeasily åtkomst hello.

1. hello Programgateway som du har skapat visas. Klicka på **serverdelspooler**, och välj hello aktuella serverdelspool **appGatewayBackendPool**, hello **appGatewayBackendPool** blad öppnas.

   ![Programpooler Gateway backend][4]

1. Klicka på **lägga till målet** tooadd IP-adresser till FQDN-värden. Välj **IP-adress eller FQDN** som hello **typen** och din IP-adress eller FQDN i hello fältet. Upprepa det här steget för ytterligare backend poolmedlemmar. När du är klar klickar du på **spara**.

### <a name="virtual-machine-and-nic"></a>Den virtuella datorn och nätverkskortet

Du kan också lägga till virtuella nätverkskort som medlemmar i serverdelen. Endast virtuella datorer i samma virtuella nätverk som hello Programgateway är tillgängliga via hello hello listrutan.

1. Med hello Programgateway som skapats i hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**. Klicka på hello **AdatumAppGateway** Programgateway i hello bladet för alla resurser. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **AdatumAppGateway** i hello **filtrera efter namn...** rutan Programgateway tooeasily åtkomst hello.

1. hello Programgateway som du har skapat visas. Klicka på **serverdelspooler**, och välj hello aktuella serverdelspool **appGatewayBackendPool**, hello **appGatewayBackendPool** blad öppnas.

   ![Programpooler Gateway backend][5]

1. Klicka på **lägga till målet** tooadd IP-adresser till FQDN-värden. Välj **virtuella** som hello **typen** och välj hello virtuella datorn och nätverkskortet toouse. När du är klar klickar du på **spara**

   > [!NOTE]
   > Endast virtuella datorer i hello samma virtuella nätverk som hello Programgateway är tillgängliga i hello listrutan.

## <a name="clean-up-resources"></a>Rensa resurser

Ta bort hello resursgrupp, Programgateway och alla relaterade resurser när de inte längre behövs. Välj toodo därför hello resursgruppen från hello programmet nätverksgateway-bladet och klicka på **ta bort**.

## <a name="next-steps"></a>Nästa steg

I detta scenario distribueras en Programgateway och lägga till en server toohello serverdel. hello nästa steg är tooconfigure hello Programgateway genom att ändra inställningarna och justera regler i hello gateway. De här stegen finns genom att besöka hello följande artiklar:

Lär dig hur toocreate anpassade hälsoavsökning genom att besöka [skapa en anpassad hälsoavsökningen](application-gateway-create-probe-portal.md)

Lär dig hur tooconfigure SSL-avlastning och vidta hello kostsamma SSL dekryptering av webbservrar genom att besöka [Konfigurera SSL-avlastning](application-gateway-ssl-portal.md)

Lär dig hur tooprotect dina program med [Brandvägg för webbaserade program](application-gateway-webapplicationfirewall-overview.md) en funktion i Programgateway.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
