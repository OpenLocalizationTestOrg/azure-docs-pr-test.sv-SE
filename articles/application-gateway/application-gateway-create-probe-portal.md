---
title: "aaaCreate en anpassad avsökning - Azure Application Gateway - Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad avsökning för Programgateway med hello-portalen"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33fd5564-43a7-4c54-a9ec-b1235f661f97
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 9e9309045ef33ba1010868783949b4fde31619ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-application-gateway-by-using-hello-portal"></a>Skapa en anpassad avsökningsåtgärd för Programgateway med hello-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-probe-ps.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-create-probe-classic-ps.md)

Lägg till en anpassad avsökningsåtgärd tooan befintliga Programgateway via hello Azure-portalen i den här artikeln. Anpassade avsökningar är användbara för program som har ett visst hälsotillstånd Kontrollera sidan eller för program som inte tillhandahåller ett lyckat svar på hello standardwebbprogrammet.

## <a name="before-you-begin"></a>Innan du börjar

Om du inte redan har en Programgateway finns [skapa en Programgateway](application-gateway-create-gateway-portal.md) toocreate toowork en program-gatewayen med.

## <a name="createprobe"></a>Skapa hello avsökningen

Avsökningar konfigureras i en tvåstegsprocess hello-portalen. hello första steget är toocreate hello avsökning. Lägg till hello avsökningen toohello serverdelens http-inställningar för Programgateway hello i hello andra steget.

1. Logga in toohello [Azure-portalen](https://portal.azure.com). Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/free)

1. Klicka på alla resurser i hello Azure-portalen fönstret Favoriter. Klicka på hello Programgateway i hello bladet för alla resurser. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange partners.contoso.net i hello filtrera efter namn... rutan Programgateway tooeasily åtkomst hello.

1. Klicka på **avsökningar** och klicka på hello **Lägg till** knappen tooadd en avsökning.

  ![Lägg till avsökning blad med information som har fyllt i][1]

1. På hello **Lägg till hälsoavsökningen** bladet fylla i hello krävs information för hello avsökningen och när du är klar klickar du på **OK**.

  |**Inställning** | **Värde** | **Detaljer**|
  |---|---|---|
  |**Namn**|customProbe|Det här värdet är ett eget namn toohello avsökningen som är tillgänglig i hello-portalen.|
  |**Protokoll**|HTTP eller HTTPS | hello-protokollet som hello hälsoavsökningen använder.|
  |**Värden**|engångsfaktorautentisering contoso.com|Det här värdet är hello värdnamnet som används för hello avsökningen. Gäller endast när flera platser har konfigurerats på Application Gateway, Använd annars ”127.0.0.1”. Det här värdet skiljer sig från hello virtuella datorns värdnamn.|
  |**Sökväg**|/ eller en annan sökväg|hello resten av hello fullständiga URL: en för hello anpassade avsökningen. En giltig sökväg börjar med '/'. Hello standardsökvägen http://contoso.com bara använda '/' |
  |**Intervall (sek)**|30|Hur ofta hello avsökningen körs toocheck för hälsotillstånd. Det rekommenderas inte tooset hello kortare än 30 sekunder.|
  |**Timeout (sek)**|30|hello mängden tid hello avsökningen timeout. hello timeout intervall måste toobe tillräckligt högt att ett http-anrop kan göras tooensure hello backend hälsosidan är tillgänglig.|
  |**Tröskelvärde för ohälsosamt värde**|3|Antal misslyckade försök toobe anses vara felaktigt. Ett tröskelvärde 0 innebär att om en hälsokontroll misslyckas hello backend-bestäms ohälsosamt omedelbart.|

  > [!IMPORTANT]
  > hello värdnamn är inte hello samma som servernamnet på. Det här värdet är hello namn hello virtuell värd som kör på hello-programserver. hello avsökningen skickas toohttp: / /(host name):(port from httpsetting)/urlPath

## <a name="add-probe-toohello-gateway"></a>Lägga till avsökning toohello gateway

Nu hello avsökning har skapats, är det tid tooadd den toohello gateway. Avsökningen inställningar anges på hello serverdelens http-inställningar för Programgateway hello.

1. Klicka på **HTTP-inställningar** toobring in hello configuration bladet Klicka på hello Programgateway hello aktuella serverdelens http-inställningar som anges i hello-fönstret.

  ![fönster för HTTPS-inställningar][2]

1. På hello **appGatewayBackEndHttpSettings** inställningsbladet, kontrollera hello **Använd anpassad avsökningsåtgärd** kryssrutan och välj hello-avsökningen som skapats i hello [skapa hello avsökningen](#createprobe) avsnitt på hello **anpassad avsökningen** nedrullningsbara...
När du är klar klickar du på **spara** och hello inställningar tillämpas.

hello standard avsökning kontrollerar hello åtkomst toohello standardwebbprogrammet. Nu när du har skapat en anpassad avsökning använder hello Programgateway hello anpassad sökväg som definierats toomonitor hälsa för hello backend-servrar. Hello Programgateway kontrollerar baserat på hello kriterier som har definierats, hello sökväg som anges i hello avsökning. Om hello anropa toohost:Port / sökvägen inte returnerar ett HTTP 200 399 Statussvaret tas hello server bort från rotationen efter hello ohälsosamt tröskelvärdet har uppnåtts. Avsökning av fortsätter på hello ohälsosamt instans toodetermine när den blir felfri igen. När hello instans läggs tillbaka toohealthy serverpoolen, trafiken börjar flöda tooit igen och avsöknings toohello instans fortsätter vid angivna intervall som användaren som vanligt.

## <a name="next-steps"></a>Nästa steg

hur tooconfigure SSL-avlastning med Azure Application Gateway, se toolearn [Konfigurera SSL-avlastning](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png

