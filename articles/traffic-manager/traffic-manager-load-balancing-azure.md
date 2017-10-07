---
title: "aaaUsing belastningsutjämning tjänster i Azure | Microsoft Docs"
description: "Den här kursen visar hur toocreate ett scenario med hjälp av hello Azure belastningsutjämning portfölj: Traffic Manager Application Gateway och belastningsutjämnare."
services: traffic-manager
documentationcenter: 
author: liumichelle
manager: vitinnan
editor: 
ms.assetid: f89be3be-a16f-4d47-bcae-db2ab72ade17
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: limichel
ms.openlocfilehash: d217047102d8c4828250dc0733e8ec9ee1e84b3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-load-balancing-services-in-azure"></a>Med belastningsutjämning tjänster i Azure

## <a name="introduction"></a>Introduktion

Microsoft Azure tillhandahåller flera tjänster för att hantera hur nätverkstrafik distribueras och Utjämning av nätverksbelastning. Du kan använda dessa tjänster individuellt eller kombinera deras metoder, beroende på dina behov, toobuild hello optimal lösning.

I den här självstudiekursen kommer vi först definiera kunden användningsfall och se hur det kan göras mer robusta och performant genom att använda följande Azure belastningsutjämning portfölj hello: Traffic Manager Application Gateway och belastningsutjämnare. Vi ger sedan stegvisa instruktioner för hur du skapar en distribution som är geografiskt redundant, distribuerar trafik tooVMs och hjälper dig att hantera olika typer av begäranden.

På en konceptuell nivå spelar var och en av dessa tjänster en specifik roll i hello belastningsutjämning hierarki.

* **Traffic Manager** innehåller globala DNS belastningsutjämning. Det ser ut på DNS-förfrågningar och svarar med en felfri slutpunkt, i enlighet med hello routning princip hello kunden har valts. Alternativ för routningsmetoder är:
  * Prestanda routning toosend hello begärande toohello närmaste slutpunkt med avseende på latens.
  * Prioritet och routning toodirect alla trafik tooan slutpunkt med andra slutpunkter som säkerhetskopia.
  * Viktad resursallokering routning, som distribuerar trafik utifrån hello-värde som är tilldelad tooeach slutpunkt.

  hello klienten ansluter direkt toothat slutpunkt. Azure Traffic Manager identifierar när en slutpunkt har dåligt hälsotillstånd och sedan omdirigerar hello klienter tooanother felfri instans. Se för[Azure Traffic Manager-dokumentationen](traffic-manager-overview.md) toolearn mer om hello-tjänsten.
* **Programgateway** ger programmet leverans domänkontrollant (ADC) som en tjänst som erbjuder olika Layer 7 belastningsutjämning funktioner för ditt program. Det gör att kunder toooptimize web servergruppen produktivitet genom att avlasta processorintensiva SSL-avslutning toohello Programgateway. Andra lager 7 routningsfunktionerna inkluderar resursallokering distribution av inkommande trafik, cookie-baserad session tillhörighet, URL-sökväg-baserad Routning och hello möjlighet toohost flera webbplatser bakom en enda Programgateway. Programgateway kan konfigureras som en mot Internet-gateway, en endast internt-gateway eller en kombination av båda. Programgateway är fullständigt Azure hanterade, skalbara och hög tillgänglighet. För att få en bättre hantering ingår en omfattande uppsättning diagnostik- och loggningsfunktioner.
* **Belastningsutjämnaren** är en integrerad del av hello Azure SDN stacken, hög prestanda, låg latens lager 4 belastningsutjämning tjänster för alla UDP och TCP-protokoll. Den hanterar inkommande och utgående anslutningar. Du kan konfigurera offentliga och interna belastningsutjämnade slutpunkter och definiera regler toomap inkommande anslutningar tooback slutpunkt pool mål med hjälp av TCP- och HTTP-avsökning av hälsotillstånd alternativ toomanage tjänsttillgänglighet.

## <a name="scenario"></a>Scenario

I det här exemplet använder vi en enkel webbplats som har två typer av innehåll: bilder och dynamiskt återgivna webbsidor. hello webbplats måste vara geografiskt redundant och den bör hantera sina användare från hello närmaste (lägsta svarstid) plats toothem. hello programutvecklaren har beslutat att hello eventuella webbadresser som matchar mönstret/bilder / * hanteras från en dedikerad pool för virtuella datorer som skiljer sig från hello resten av hello webbservergrupp.

Dessutom måste hello VM Standardpool betjänar hello dynamiskt innehåll tootalk tooa backend-databas som finns på ett kluster med hög tillgänglighet. hello hela distributionen har ställts in via Azure Resource Manager.

Med hjälp av Traffic Manager Application Gateway och belastningsutjämnare kan den här webbplatsen tooachieve dessa utforma mål:

* **Flera geo-redundans**: om en region kraschar Traffic Manager vägar sömlöst trafik toohello närmaste region utan någon åtgärd från hello programmet ägare.
* **Kortare svarstider**: eftersom Traffic Manager dirigerar automatiskt hello kunden toohello närmaste region, hello kund får kortare svarstid när du begär hello webbsidan innehållet.
* **Oberoende skalbarhet**: eftersom hello web application arbetsbelastning avgränsas av typ av innehåll, hello programmet ägare kan skala hello begäran arbetsbelastningar är oberoende av varandra. Programgateway garanterar att hello trafik dirigeras toohello rätt poolerna baserat på angivna hello hello programmet hello hälsa och regler.
* **Intern belastningsutjämning**: eftersom belastningsutjämnare är framför hello kluster med hög tillgänglighet, hello aktiva och felfri slutpunkt för en databas är utsatta toohello program. Dessutom kan en databasadministratör optimera hello arbetsbelastning genom att distribuera aktiva och passiva repliker över hello kluster oberoende av hello klientprogrammet. Belastningsutjämnaren ger anslutningar toohello kluster med hög tillgänglighet och säkerställer att endast felfri databaser tar emot anslutningsbegäranden.

hello visar följande diagram hello arkitekturen i det här scenariot:

![Diagram över belastningsutjämning arkitektur](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> Det här exemplet är endast ett av många möjliga konfigurationer hello belastningsutjämning tjänster som Azure erbjuder. Traffic Manager Application Gateway och belastningsutjämnare kan vara mixed och matchade toobest passar dina behov för belastningsutjämning. Till exempel om SSL-avlastning eller Layer 7 bearbetning behövs inte kan belastningsutjämnaren användas i stället för Programgateway.

## <a name="setting-up-hello-load-balancing-stack"></a>Konfigurera hello belastningsutjämning stack

### <a name="step-1-create-a-traffic-manager-profile"></a>Steg 1: Skapa en trafikhanterarprofil

1. I hello Azure-portalen klickar du på **ny**, och sedan söka hello marketplace för ”trafikhanterarprofil”.
2. På hello **skapa Traffic Manager-profilen** bladet ange hello följande grundläggande information:

  * **Namnet**: ge din Traffic Manager-profil en DNS-Prefixnamn.
  * **Routningsmetoden**: Välj hello princip för routning av nätverkstrafik metod. Mer information om hello metoder finns [om Traffic Manager-trafikroutningsmetoder](traffic-manager-routing-methods.md).
  * **Prenumerationen**: Välj hello-prenumeration som innehåller hello-profil.
  * **Resursgruppen**: Välj hello resursgruppen som innehåller hello-profil. Det kan vara en ny eller befintlig resursgrupp.
  * **Resursgruppens plats**: Traffic Manager-tjänsten är global och inte knuten tooa plats. Du måste dock ange en region för hello grupp där hello metadata som associeras med hello Traffic Manager-profilen finns. Den här platsen påverkar inte hello runtime tillgängligheten för hello profil.

3. Klicka på **skapa** toogenerate hello Traffic Manager-profilen.

  ![”Skapa Traffic Manager” bladet](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-hello-application-gateways"></a>Steg 2: Skapa hello programgatewayer

1. Klicka i hello Azure-portalen hello vänster **ny** > **nätverk** > **Programgateway**.
2. Ange följande grundläggande information om hello Programgateway hello:

  * **Namnet**: hello namnet på hello Programgateway.
  * **SKU-storleken**: hello storleken på hello Programgateway, tillgänglig som små, medelstora eller stora.
  * **Instansen antal**: hello antalet instanser, ett värde mellan 2 och 10.
  * **Resursgruppen**: hello resursgruppen som innehåller hello Programgateway. Det kan vara en befintlig resursgrupp eller en ny.
  * **Plats**: hello region för hello Programgateway, vilket är hello samma plats som hello resursgruppen. hello platsen är viktig, eftersom hello virtuella nätverk och offentliga IP-Adressen måste vara i hello samma plats som hello gateway.
3. Klicka på **OK**.
4. Definiera hello virtuellt nätverk, undernät, IP frontend- och lyssnare konfigurationer för hello Programgateway. I det här scenariot hello frontend IP-adressen är **offentliga**, vilket gör att det toobe som lagts till som en slutpunkt toohello trafikhanterarprofil vid ett senare tillfälle.
5. Konfigurera hello lyssnare med något av följande alternativ för hello:
    * Om du använder HTTP, det finns inget tooconfigure. Klicka på **OK**.
    * Om du använder HTTPS ytterligare krävs konfiguration. Se för[skapa en Programgateway](../application-gateway/application-gateway-create-gateway-portal.md)början från steg 9. När du har slutfört hello konfiguration, klickar du på **OK**.

#### <a name="configure-url-routing-for-application-gateways"></a>Konfigurera URL routning för programgatewayer

När du väljer en backend-adresspool tar en Programgateway som är konfigurerad med en sökväg-baserade regel en sökvägar för hello URL: en i tillägg tooround robin-distribution. I det här scenariot vi lägger till en sökväg-baserade regler toodirect alla URL: en med ”/images/\*” toohello bild serverpoolen. Mer information om hur du konfigurerar URL sökväg-baserade routning för en Programgateway finns för[skapa en sökväg-baserade regel för en Programgateway](../application-gateway/application-gateway-create-url-route-portal.md).

![Diagram över webbnivå Gateway](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. Gå toohello instans av hello Programgateway som du skapade i föregående avsnitt hello från din resursgrupp.
2. Under **inställningar**väljer **serverdelspooler**, och välj sedan **Lägg till** tooadd hello virtuella datorer som du vill tooassociate med hello webbnivå backend-pooler.
3. På hello **lägga till serverdelspoolen** bladet anger hello namnet på hello backend-poolen och alla hello IP-adresser hello-datorer som finns i hello pool. I det här scenariot ansluter vi två backend-server-pooler med virtuella datorer.

  ![Programmet Gateway ”Lägg till serverdelspoolen” bladet](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. Under **inställningar** hello Programgateway väljer **regler**, och klicka sedan på hello **sökväg baserat** knappen tooadd en regel.

  ![Programmet Gateway regler ”sökväg baserad” knappen](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. På hello **Lägg till sökväg-baserade regler** bladet konfigurera hello regel genom att tillhandahålla hello följande information.

   Grundläggande inställningar:

   + **Namnet**: hello eget namn för hello-regel som är tillgänglig i hello-portalen.
   + **Lyssnare**: hello lyssnare som används för hello regeln.
   + **Standard serverdelspool**: hello backend-adresspool toobe används med hello Standardregeln.
   + **Standardinställningarna för HTTP-**: hello HTTP inställningar toobe används med hello Standardregeln.

   Sökväg-baserade regler:

   + **Namnet**: hello eget namn för hello sökväg-baserade regler.
   + **Sökvägar**: hello sökvägsregeln som används för att vidarebefordra trafik.
   + **Serverdelspool**: hello backend-adresspool toobe används med den här regeln.
   + **HTTP-inställningen**: hello HTTP inställningar toobe används med den här regeln.

   > [!IMPORTANT]
   > Sökvägar: Giltiga sökvägar måste inledas med ”/”. Hej jokertecknet ”\*” tillåts endast i slutet av hello. Giltiga exempel är /xyz /xyz\*, eller /xyz/\*.

   ![Programmet Gateway ”Lägg till sökväg-baserade regeln” bladet](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-toohello-traffic-manager-endpoints"></a>Steg 3: Lägga till programslutpunkter gateways toohello Traffic Manager

I det här scenariot är Traffic Manager anslutna tooapplication gateways (som konfigurerats i hello föregående steg) som finns i olika regioner. Hej programgatewayer är konfigurerat hello nästa steg är tooconnect dem tooyour Traffic Manager-profilen.

1. Öppna Traffic Manager-profilen. toodo alltså se i resursgruppen eller Sök efter hello namnet på hello Traffic Manager-profilen från **alla resurser**.
2. I hello vänster och välj **slutpunkter**, och klicka sedan på **Lägg till** tooadd en slutpunkt.

  ![Traffic Manager slutpunkter ”Lägg till”-knappen](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. På hello **lägga till slutpunkten** bladet, skapa en slutpunkt genom att ange hello följande information:

  * **Typen**: Ange hello endpoint tooload-saldot. I det här scenariot väljer **Azure endpoint** eftersom vi ansluter den toohello programmet gateway-instanser som har konfigurerats tidigare.
  * **Namnet**: Ange hello namnet på hello slutpunkt.
  * **Rikta resurstypen**: Välj **offentliga IP-adressen** och under **mål resurs**, Välj hello offentliga IP-Adressen för hello Programgateway som konfigurerats tidigare.

   ![Traffic Manager ”Lägg till slutpunkten” bladet](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Nu kan du testa din konfiguration genom att öppna den med hello DNS för din Traffic Manager-profil (i det här exemplet: TrafficManagerScenario.trafficmanager.net). Du kan skicka begäranden, registreringspunkter eller avslutar virtuella datorer och servrar som har skapats i olika regioner och ändra hello Traffic Manager-profilen inställningar tootest din konfiguration.

### <a name="step-4-create-a-load-balancer"></a>Steg 4: Skapa en belastningsutjämnare

I det här scenariot distribuerar belastningsutjämnare anslutningar från hello web nivå toohello databaser inom ett kluster med hög tillgänglighet.

Om ditt kluster med hög tillgänglighet databasen använder SQL Server AlwaysOn, se för[konfigurera en eller flera alltid på tillgänglighet Tillgänglighetsgruppslyssnarnas](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) stegvisa instruktioner.

Mer information om hur du konfigurerar en intern belastningsutjämnare finns [skapa en intern belastningsutjämnare i hello Azure-portalen](../load-balancer/load-balancer-get-started-ilb-arm-portal.md).

1. Klicka i hello Azure-portalen hello vänster **ny** > **nätverk** > **belastningsutjämnaren**.
2. På hello **skapa belastningsutjämnaren** bladet Välj ett namn för din belastningsutjämnare.
3. Ange hello **typen** för**internt**, och välj hello lämpliga virtuella nätverk och undernät för hello load balancer tooreside i.
4. Under **IP-adresstilldelning**, väljer du antingen **dynamiska** eller **statiska**.
5. Under **resursgruppen**, väljer hello resursgrupp för hello belastningsutjämnaren.
6. Under **plats**, Välj hello rätt region för hello belastningsutjämnaren.
7. Klicka på **skapa** toogenerate hello belastningsutjämnaren.

#### <a name="connect-a-back-end-database-tier-toohello-load-balancer"></a>Ansluta en backend-databasens nivå toohello belastningsutjämnare

1. Hitta hello belastningsutjämnare som skapades i föregående steg i hello från din resursgrupp.
2. Under **inställningar**, klickar du på **serverdelspooler**, och klicka sedan på **Lägg till** tooadd en backend-adresspool.

  ![Läsa in belastningsutjämning ”Lägg till serverdelspoolen” bladet](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

3. På hello **lägga till serverdelspoolen** bladet anger hello namnet på hello backend-adresspool.
4. Lägga till enskilda datorer eller en tillgänglighet set toohello backend-adresspool.

#### <a name="configure-a-probe"></a>Konfigurera en avsökning

1. I din belastningsutjämnare under **inställningar**väljer **avsökningar**, och klicka sedan på **Lägg till** tooadd en avsökning.

 ![Läsa in belastningsutjämning ”Lägg till avsökning” bladet](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

2. På hello **Lägg till avsökning** bladet ange hello namn för hello avsökningen.
3. Välj hello **protokollet** för hello avsökningen. Du kanske vill en TCP-avsökning i stället för en HTTP-avsökningen för en databas. toolearn mer om belastningsutjämnare avsökningar, se för[förstå load balancer avsökningar](../load-balancer/load-balancer-custom-probe-overview.md).
4. Ange hello **Port** för din databas toobe som används för att komma åt hello avsökning.
5. Under **intervall**, ange hur ofta tooprobe hello program.
6. Under **tröskelvärde för ohälsosamt värde**, ange hello antal kontinuerlig avsökningen fel som måste ske för hello backend-VM toobe anses vara felaktigt.
7. Klicka på **OK** toocreate hello avsökning.

#### <a name="configure-hello-load-balancing-rules"></a>Konfigurera regler för belastningsutjämning av hello

1. Under **inställningar** din belastningsutjämnare väljer **belastningsutjämningsregler**, och klicka sedan på **Lägg till** toocreate en regel.
2. På hello **Lägg till regel för belastningsutjämning** bladet ange hello **namn** för hello belastningsutjämning regeln.
3. Välj hello **Klientdelens IP-adress** av hello belastningsutjämnare **protokollet**, och **Port**.
4. Under **serverdelsport**, ange hello port toobe används i hello backend-adresspool.
5. Välj hello **serverdelspool** och hello **avsökning** som har skapats i hello föregående steg tooapply hello regeln ska.
6. Under **sessionspersistence**, väljer du hur hello sessioner toopersist.
7. Under **timeout vid inaktivitet**, ange hello antalet minuter innan en timeout för inaktivitet.
8. Under **flytande IP**, väljer du antingen **inaktiverad** eller **aktiverad**.
9. Klicka på **OK** toocreate hello regeln.

### <a name="step-5-connect-web-tier-vms-toohello-load-balancer"></a>Steg 5: Anslut webbnivå VMs toohello belastningsutjämnare

Vi nu konfigurera hello IP-adress och belastningsutjämnare frontend-port i hello-program som körs på din webbnivå virtuella datorer för alla anslutningar. Den här konfigurationen är särskilda toohello program som körs på dessa virtuella datorer. tooconfigure hello mål-IP-adress och port, referera toohello dokumentationen. toofind hello IP-adressen för hello klientdelen i hello Azure-portalen går toohello frontend IP-adresspool på hello **inställningar för belastningsutjämnare** bladet.

![Läsa in belastningsutjämning ”Frontend IP-pool” navigeringsfönstret](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Nästa steg

* [Översikt över Traffic Manager](traffic-manager-overview.md)
* [Översikt över Gateway](../application-gateway/application-gateway-introduction.md)
* [Översikt över Azure Load Balancer](../load-balancer/load-balancer-overview.md)
