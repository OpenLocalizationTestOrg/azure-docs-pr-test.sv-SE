---
title: "Kom igång med testa i produktion för Web Apps"
description: "Läs mer om Test i produktion (TiP)-funktionen i Azure App Service Web Apps."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 4623468d-886e-4203-8012-8f86deb2790b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: cephalin
ms.openlocfilehash: 9f38b635140cacf0513c75385bce3c110a930969
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-test-in-production-for-web-apps"></a>Kom igång med testa i produktion för Web Apps
Testa i produktion eller live-testa ditt webbprogram med levande kunden trafik, är en test-strategi som appen utvecklare integrera allt i sina [flexibel utveckling](https://en.wikipedia.org/wiki/Agile_software_development) metoder. På så sätt kan du testa kvaliteten på dina appar med levande användaraktiviteten i produktionsmiljön, till skillnad från syntetiskt data i en testmiljö. Genom att exponera din nya app till verkliga användare, kan du få information om verkliga problem som din app kan stöta på när den har distribuerats. Du kan kontrollera funktionalitet, prestanda och värdet för din app-uppdateringar mot volymen, hastighet och antal användartrafiken, som du kan aldrig ungefärliga i en testmiljö.

## <a name="traffic-routing-in-app-service-web-apps"></a>Trafik routning i App Service Web Apps
Med funktionen routning av nätverkstrafik i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), du kan dirigera om en del av live användartrafik till en eller flera [distributionsfack](web-sites-staged-publishing.md), och analysera din app med [Azure-program Insikter](/services/application-insights/) eller [Azure HDInsight](/services/hdinsight/), eller ett verktyg från tredje part som [New Relic](/marketplace/partners/newrelic/newrelic/) validera din ändring. Du kan exempelvis implementera följande scenarier med App Service:

* Identifiera funktionella buggar eller identifiera flaskhalsar i dina uppdateringar före distributionen av hela platsen
* Utföra ”kontrollerade test flygplan” dina ändringar genom att mäta användbarhet mått på beta-app
* Gradvis snabbt skala upp till en ny uppdatering och smidigt tillbaka till den aktuella versionen om ett fel inträffar 
* Optimera verksamhetsresultat för din app genom att köra [A / B testar](https://en.wikipedia.org/wiki/A/B_testing) eller [multivariate testerna](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) i flera distributionsplatser

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Krav för att använda Routning av nätverkstrafik i Web Apps
* Ditt webbprogram måste köras i **Standard** eller **Premium** nivån, eftersom det krävs för flera distributionsplatser.
* Trafikroutning kräver cookies är aktiverade i användarnas webbläsare för att fungera korrekt. Trafik Routning använder cookies för att fästa en klientwebbläsare till en distributionsplats för livslängd klientsessionen.
* Trafik routning stöd för avancerade scenarier tips via Azure PowerShell-cmdlets.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Väg trafik segmentet till en distributionsplats
På nivån basic i varje scenario tips vidarebefordra fördefinierade procent av din live trafik till en icke-produktion distributionsplats. Följ stegen nedan om du vill göra detta:

> [!NOTE]
> De här stegen förutsätter att du redan har en [icke-produktion distributionsplatsen](web-sites-staged-publishing.md) och som önskade app webbinnehållet redan [distribueras](web-sites-deploy.md) till den.
> 
> 

1. Logga in på den [Azure-portalen](https://portal.azure.com/).
2. I din webbapps blad klickar du på **inställningar** > **Trafikroutning**.
   ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Välj den plats som du vill dirigera trafik till och procentandelen av den totala trafiken som du vill ha och klickar sedan **spara**.
   
    ![](./media/app-service-web-test-in-production/02-select-slot.png)
4. Gå till bladet för den distributionsplatsen. Du bör nu se live trafik dirigeras till den.
   
    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

När du har konfigurerat routning av nätverkstrafik dirigeras den angivna procentandelen av klienter slumpmässigt till din produktionsplatsen. Det är dock viktigt att Observera att när en klient omdirigeras automatiskt till en viss plats, den ”Fäst” till platsen för den klientsessionen. Detta har gjort med hjälp av en cookie fästa användarsessionen. Om du granska den HTTP-begäranden, hittar du en `TipMix` cookie i varje efterföljande begäran.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Tvinga klientbegäranden till en specifik plats
Förutom automatisk trafikroutning kan Apptjänst väg begäranden till en specifik plats. Detta är användbart när du vill att användarna ska kunna välja i eller CEIP beta-app. Om du vill göra detta måste du använda den `x-ms-routing-name` Frågeparametern.

Att omdirigera användare till en specifik plats med hjälp av `x-ms-routing-name`, måste du se till att platsen har redan lagts till i listan för routning av nätverkstrafik. Eftersom du vill vidarebefordra till en plats uttryckligen faktiska routning procentandelen som du ställer in spelar ingen roll. Om du vill kan utforma du en ”länk av beta” som användarna kan klicka på för att komma åt appen beta.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Välja användare utanför beta-appen
Så att användarna kan välja bort appen beta, exempelvis kan du placera den här länken på din webbsida:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

Strängen `x-ms-routing-name=self` anger produktionsplatsen. När klientens webbläsare åtkomst till länken, inte bara omdirigeras till produktionsplatsen, men varje efterföljande begäran innehåller den `x-ms-routing-name=self` cookie som PIN-session till produktionsplatsen.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Välja användare i beta-appen
Så att användare kan välja att beta-app genom att ange samma Frågeparametern namnet på icke-produktionsplatsen, till exempel:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Fler resurser
* [Skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md)
* [Distribuera ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md)
* [Flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md)
* [Använda DevOps miljöer effektivt för ditt webbprogram](app-service-web-staged-publishing-realworld-scenarios.md)

