---
title: "aaaGet igång med test i produktion för webbprogram"
description: "Läs mer om hello Test i produktion (TiP)-funktionen i Azure App Service Web Apps."
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
ms.openlocfilehash: 2ddbd532ffe2a4f3e07fd386d9741a3fde3639ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-test-in-production-for-web-apps"></a>Kom igång med testa i produktion för Web Apps
Testa i produktion eller live-testa ditt webbprogram med levande kunden trafik, är en test-strategi som appen utvecklare integrera allt i sina [flexibel utveckling](https://en.wikipedia.org/wiki/Agile_software_development) metoder. Den låter dig tootest hello kvaliteten på dina appar med levande användaraktiviteten i produktionsmiljön, som skillnad från toosynthesized data i en testmiljö. Genom att exponera dina nya app tooreal användare, kan du få information om hello verkliga problem med din app kan stöta på när den har distribuerats. Du kan kontrollera hello funktionalitet, prestanda och värdet för din app-uppdateringar mot hello volym, hastighet och antal användartrafiken, som du kan aldrig ungefärliga i en testmiljö.

## <a name="traffic-routing-in-app-service-web-apps"></a>Trafik routning i App Service Web Apps
Med hello routning av nätverkstrafik funktion i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), du kan dirigera om en del av live användaren trafik tooone eller flera [distributionsfack](web-sites-staged-publishing.md), och analysera din app med [Azure-program Insikter](/services/application-insights/) eller [Azure HDInsight](/services/hdinsight/), eller ett verktyg från tredje part som [New Relic](/marketplace/partners/newrelic/newrelic/) toovalidate ändringen. Du kan exempelvis implementera hello följande scenarier med App Service:

* Identifiera funktionella buggar eller identifiera flaskhalsar i uppdateringar tidigare toosite hela distributionen
* Utföra ”kontrollerade test flygplan” dina ändringar genom att mäta användbarhet mått på hello beta-app
* Ökar gradvis tooa ny uppdatering och smidigt tillbaka ned toohello aktuell version om ett fel inträffar 
* Optimera verksamhetsresultat för din app genom att köra [A / B testar](https://en.wikipedia.org/wiki/A/B_testing) eller [multivariate testerna](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) i flera distributionsplatser

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Krav för att använda Routning av nätverkstrafik i Web Apps
* Ditt webbprogram måste köras i **Standard** eller **Premium** nivån, eftersom det krävs för flera distributionsplatser.
* I ordning toowork korrekt trafikroutning cookies toobe aktiverat i hello användarnas webbläsare. Trafik Routning använder cookies toopin en distributionsplats för klientens webbläsare tooa för hello livslängd hello klientsessionen.
* Trafik routning stöd för avancerade scenarier tips via Azure PowerShell-cmdlets.

## <a name="route-traffic-segment-tooa-deployment-slot"></a>Väg trafik segment tooa distributionsplatsen
På hello grundläggande nivå i varje scenario tips vidarebefordra fördefinierade procent av din live trafik tooa icke-produktion distributionsplatsen. toodo detta, följ hello stegen nedan:

> [!NOTE]
> hello här stegen förutsätter att du redan har en [icke-produktion distributionsplatsen](web-sites-staged-publishing.md) och den hello önskat app webbinnehåll är redan [distribueras](web-sites-deploy.md) tooit.
> 
> 

1. Logga in på hello [Azure Portal](https://portal.azure.com/).
2. I din webbapps blad klickar du på **inställningar** > **Trafikroutning**.
   ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Välj hello-plats som du vill tooroute trafik tooand hello procentandelen hello totala trafiken du vill ha och klickar sedan **spara**.
   
    ![](./media/app-service-web-test-in-production/02-select-slot.png)
4. Gå toohello distribution fack-bladet. Du bör nu se live trafik som dirigeras tooit.
   
    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

När du har konfigurerat routning av nätverkstrafik angetts hello procentandelen klienter kommer att slumpmässigt routade tooyour icke-produktionsplatsen. Det är dock viktigt toonote att när en klient är automatiskt routade tooa specifika plats, blir ”fasta” toothat plats för hello livslängd session som klienten. Detta har gjort med hjälp av en cookie toopin hello användarsession. Om du inspektera hello HTTP-begäranden, hittar du en `TipMix` cookie i varje efterföljande begäran.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-tooa-specific-slot"></a>Tvinga klienten begäranden tooa specifik plats
Apptjänst är i tillägg tooautomatic routning av nätverkstrafik, kan tooroute begäranden tooa specifik plats. Detta är användbart när du vill att dina användare toobe kan tooopt-till eller CEIP beta-app. toodo kan du använda hello `x-ms-routing-name` Frågeparametern.

tooreroute användare tooa specifika plats använder `x-ms-routing-name`, måste du kontrollera hello platsen har redan lagts till toohello routning av nätverkstrafik lista. Eftersom du uttryckligen vill tooroute tooa fack hello faktiska routning procentandel du anger spelar ingen roll. Om du vill kan du skapa en ”länk av beta” som användarna kan klicka på tooaccess hello beta-app.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Välja användare utanför beta-appen
toolet användarna välja utanför beta-appen, till exempel kan du placera den här länken på din webbsida:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back tooproduction app</a>

Hej sträng `x-ms-routing-name=self` anger hello produktionsplatsen. När hello klientens webbläsare åtkomst hello länken är inte bara den omdirigerade toohello produktionsplatsen, men varje efterföljande begäran innehåller hello `x-ms-routing-name=self` cookie som PIN-hello session toohello produktionsplatsen.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-toobeta-app"></a>Välja användare i toobeta app
toolet användarna välja i tooyour beta app, ange hello samma fråga toohello parameternamn för hello icke-produktionsplatsen, till exempel:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Fler resurser
* [Skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md)
* [Distribuera ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md)
* [Flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md)
* [Använda DevOps miljöer effektivt för ditt webbprogram](app-service-web-staged-publishing-realworld-scenarios.md)

