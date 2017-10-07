---
title: "aaaCreate eller uppdatera en Azure Programgateway med Brandvägg för webbaserade program | Microsoft Docs"
description: "Lär dig hur hello toocreate en Programgateway med Brandvägg för webbaserade program med hjälp av portalen"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b561a210-ed99-4ab4-be06-b49215e3255a
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.openlocfilehash: 68d140fef14499da654ea251d1208e6a800f55a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-web-application-firewall-by-using-hello-portal"></a>Skapa en Programgateway med Brandvägg för webbaserade program med hjälp av hello-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Lär dig hur toocreate Brandvägg för webbaserade program aktiverat Programgateway.

hello Brandvägg för webbaserade program (Brandvägg) i Azure Application Gateway skyddar webbprogram från vanliga webbaserade attacker som SQL injection, webbplatser scripting attacker och sessionen hijacks. Webbprogrammet skyddar mot många hello OWASP översta 10 vanliga web säkerhetsproblem.

## <a name="scenarios"></a>Scenarier

Det finns två scenarier i den här artikeln:

I hello första scenariot lär du dig för[skapar en Programgateway med Brandvägg för webbaserade program](#create-an-application-gateway-with-web-application-firewall)

I hello andra fallet kan du lära dig för[lägga till web application brandväggen tooan befintliga Programgateway](#add-web-application-firewall-to-an-existing-application-gateway).

![Exempel på ett scenario][scenario]

> [!NOTE]
> Ytterligare konfiguration av hello Programgateway, inklusive anpassade hälsa avsökningar, backend-adresser för poolen och ytterligare regler konfigureras när hello Programgateway har konfigurerats och inte under första distributionen.

## <a name="before-you-begin"></a>Innan du börjar

Azure Application Gateway kräver sin egen undernät. När du skapar ett virtuellt nätverk, se till att du lämnar tillräckligt med adressutrymme toohave flera undernät. När du distribuerar ett program tooa gatewayundernät endast ytterligare programgatewayer är kan toobe läggs toohello undernät.

##<a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Lägg till web application brandväggen tooan befintliga Programgateway

Det här exemplet uppdaterar ett befintligt program gateway toosupport Brandvägg för webbaserade program i förebyggande läge.

1. I hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**. Klicka på hello befintliga Programgateway i hello **alla resurser** bladet. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange hello namn i hello **filtrera efter namn...** rutan tooeasily åtkomst hello DNS-zon.

   ![Skapa Programgateway][1]

1. Klicka på **Brandvägg för webbaserade program** och uppdatera hello programinställningar för gateway. När du är klar klickar du på **spara**

    hello inställningar tooupdate ett befintligt program gateway toosupport Brandvägg för webbaserade program är:

   | **Inställning** | **Värde** | **Detaljer**
   |---|---|---|
   |**Uppgradera tooWAF nivå**| Markerad | Detta anger hello nivå av hello programmet gateway toohello Brandvägg nivå.|
   |**Status för brandväggen**| Enabled | Den här inställningen aktiverar hello brandväggen på hello Brandvägg.|
   |**Brandväggsläge** | Prevention (Skydd) | Den här inställningen är hur Brandvägg för webbaserade program behandlar skadlig trafik. **Identifiering av** läge loggar endast hello händelser där **förebyggande** hello händelser loggas i läge och stoppar hello skadlig trafik.|
   |**Regeluppsättning**|3.0|Den här inställningen avgör hello [core regeluppsättning](application-gateway-web-application-firewall-overview.md#core-rule-sets) poolmedlemmar används tooprotect som hello backend.|
   |**Konfigurera inaktiverade regler**|Det varierar|tooprevent möjliga falska positiva identifieringar, den här inställningen kan du toodisable vissa [regler och regelgrupper](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > När du uppgraderar en befintlig programmet gateway toohello Brandvägg SKU hello SKU storlek ändringar för**medel**. Detta kan ändras när konfigurationen är klar.

    ![bladet visar grundläggande inställningar][2-1]

    > [!NOTE]
    > måste vara aktiverat tooview programmet brandväggen webbloggar, diagnostik och ApplicationGatewayFirewallLog markerad. Du kan välja ett instansantal på 1 för testning. Det är viktigt att valfri instans antalet under två instanser tooknow omfattas inte av hello SLA och därför rekommenderas inte. Liten gateways är inte tillgängliga när du använder Brandvägg för webbaserade program.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Skapa en Programgateway med Brandvägg för webbaserade program

Det här scenariot kommer:

* Skapa en medelhög web application brandväggen Programgateway med två instanser.
* Skapa ett virtuellt nätverk med namnet AdatumAppGatewayVNET med en 10.0.0.0/16 reserverade CIDR-blocket.
* Skapa undernätet Appgatewaysubnet som använder 10.0.0.0/28 som dess CIDR-block.
* Konfigurera ett certifikat för SSL-avlastning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com). Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/free)
1. I hello Favoriter hello portalen klickar **ny**
1. I hello **ny** bladet, klickar du på **nätverk**. I hello **nätverk** bladet, klickar du på **Programgateway**som visas i följande bild hello:
1. Navigera toohello Azure-portalen klickar du på **ny** > **nätverk** > **Programgateway**

    ![Skapa Programgateway][1]

1. I hello **grunderna** bladet som visas, ange hello följande värden, och klicka sedan på **OK**:

   | **Inställning** | **Värde** | **Detaljer**
   |---|---|---|
   |**Namn**|AdatumAppGateway|hello namnet på hello Programgateway|
   |**Nivå**|BRANDVÄGG|Tillgängliga värden är Standard och Brandvägg. Besök [Brandvägg för webbaserade program](application-gateway-web-application-firewall-overview.md) toolearn mer om Brandvägg.|
   |**SKU-storleken**|Medel|Alternativ när du väljer standardnivån är små, medelstora och stora. När du väljer Brandvägg nivå är alternativen medel och stor bara.|
   |**Instansantal**|2|Antal instanser av hello Programgateway för hög tillgänglighet. Antalet instanser av 1 bör endast användas för testning.|
   |**Prenumeration**|[Din prenumeration]|Välj en prenumeration toocreate hello Programgateway i.|
   |**Resursgrupp**|**Skapa en ny:** AdatumAppGatewayRG|Skapa en resursgrupp. hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt. Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups) översiktsartikel.|
   |**Plats**|Västra USA||

   ![bladet visar grundläggande inställningar][2-2]

1. I hello **inställningar** bladet som visas under **för virtuella nätverk**, klickar du på **Välj ett virtuellt nätverk**. Det här steget öppnas ange hello **Välj virtuellt nätverk** bladet.  Klicka på **Skapa nytt** tooopen hello **skapa virtuellt nätverk** bladet.

   ![Välj ett virtuellt nätverk][2]

1. På hello **skapa virtuellt nätverk-bladet** ange hello följande värden och klicka sedan på **OK**. Det här steget stängs hello **skapa virtuellt nätverk** och **Välj virtuellt nätverk** blad. Hello fylls **undernät** på hello **inställningar** bladet med hello undernät valt.

   |**Inställning** | **Värde** | **Detaljer** |
   |---|---|---|
   |**Namn**|AdatumAppGatewayVNET|Namnet på hello Programgateway|
   |**Adressutrymmet**|10.0.0.0/16| Det här värdet är hello adressutrymmet för hello virtuellt nätverk|
   |**Namn på undernät**|AppGatewaySubnet|Namnet på hello undernät för hello Programgateway|
   |**Adressintervall för undernätet**|10.0.0.0/28 | Det här undernätet tillåter flera undernät i hello virtuellt nätverk för medlemmar i serverdelen|

1. På hello **inställningar** bladet under **Frontend-IP-konfiguration**, Välj **offentliga** som hello **IP-adresstyp**

1. På hello **inställningar** bladet under **offentliga IP-adressen**, klickar du på **Välj en offentlig IP-adress**, det här steget öppnar hello **Välj offentlig IP-adress**bladet, klickar du på **Skapa nytt**.

   ![Välj offentlig IP-adress][3]

1. På hello **skapa offentlig IP-adress** bladet acceptera standardvärdet för hello och på **OK**. Det här steget stängs hello **Välj offentlig IP-adress** bladet, hello **skapa offentlig IP-adress** bladet och fylla **offentliga IP-adressen** med hello offentlig IP-adress som väljs.

1. På hello **inställningar** bladet under **Lyssnarkonfigurationen**, klickar du på **HTTP** under **protokollet**. toouse **https**, krävs ett certifikat. hello privat nyckel för hello certifikat krävs så att en PFX-export av hello certifikat måste toobe tillhandahålls och hello hello-filens lösenord.

1. Konfigurera hello **Brandvägg** specifika inställningar.

   |**Inställning** | **Värde** | **Detaljer** |
   |---|---|---|
   |**Status för brandväggen**| Enabled| Den här inställningen inaktiverar Brandvägg eller inaktivera.|
   |**Brandväggsläge** | Prevention (Skydd)| Den här inställningen avgör hello åtgärder Brandvägg tar på sig skadlig trafik. Om **identifiering** är valt, loggas endast trafik.  Om **förebyggande** väljs trafik är inloggad och stoppats med ett 403 obehörig svar.|


1. Granska hello sammanfattningssidan och klicka på **OK**.  Hello Programgateway är nu i kö och skapas.

1. Navigera tooit i hello portal toocontinue konfigurationen av hello Programgateway när hello Programgateway har skapats.

    ![Programvy Gateway resurs][10]

Dessa steg kan du skapa en basic-Programgateway med standardinställningar för hello lyssnare, serverdelspool, serverdelens http-inställningar och regler. Du kan ändra dessa inställningar toosuit distributionen när hello etablering har lyckats

> [!NOTE]
> Programgatewayer som skapats med konfiguration av hello grundläggande webbprogram brandväggen konfigureras med CR 3.0 för skydd.

## <a name="next-steps"></a>Nästa steg

Därefter kan du lära dig hur tooconfigure en anpassad domän alias för hello [offentliga IP-adressen](../dns/dns-custom-domain.md#public-ip-address) med hjälp av Azure DNS eller en annan DNS-leverantör.

Lär dig hur tooconfigure diagnostikloggning, toolog hello händelser som har identifierats eller förhindras med Brandvägg för webbaserade program genom att besöka [Gateway Programdiagnostik](application-gateway-diagnostics.md)

Lär dig hur toocreate anpassade hälsoavsökning genom att besöka [skapa en anpassad hälsoavsökningen](application-gateway-create-probe-portal.md)

Lär dig hur tooconfigure SSL-avlastning och vidta hello kostsamma SSL dekryptering av webbservrar genom att besöka [Konfigurera SSL-avlastning](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
