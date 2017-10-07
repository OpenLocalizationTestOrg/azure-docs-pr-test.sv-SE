---
title: "aaaPredictive Underhåll förkonfigurerade lösningen | Microsoft Docs"
description: "En beskrivning av hello Azure IoT Suite förutsägande Underhåll förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 2d09801467d33db6b7d6333fa071aea2bf573f20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Översikt över förkonfigurerad lösning för förebyggande underhåll

Hej *förutsägande Underhåll* [förkonfigurerade lösningen] [ lnk_preconfigured_solutions] är en av hello [Microsoft Azure IoT Suite] [ lnk_iot_suite] förkonfigurerade lösningar. Den här lösningen integrerar realtidsinsamling av enhetstelemetri med en förebyggande modell skapad med [Azure Machine Learning][lnk-machine-learning].

Med Azure IoT Suite du snabbt och enkelt ansluta tooand övervakaren tillgångar och analysera telemetri i realtid i instrumentpaneler och visualiseringar. I hello förutsägande underhållslösningen ger hello instrumentpaneler och visualiseringar nya intelligence som kan enheten effektivitet och förbättra intäkter dataströmmar.

## <a name="hello-scenario"></a>hello Scenario

Fabrikam är ett regionalt flygbolag som fokuserar på bra kundupplevelser till konkurrenskraftiga priser. En orsak till flygförseningar är underhållsproblem, och flygplansmotorernas underhåll är särskilt krävande. Fabrikam måste undvika motorn fel under svarta på alla vis så att den regelbundet kontrollerar dess motorer och schemalägger underhåll enligt tooa plan. Dock hello flygplan motorer inte bär alltid densamma. En del onödigt underhåll utförs på motorerna. Dessutom kan det uppstå problem som gör att planet blir stående tills underhåll har utförts. Om ett flygplan är på en plats hello där rätt tekniker eller delar är inte tillgänglig, dessa problem kan vara särskilt kostsamma.

hello motorer i Fabrikams flygplan instrumenterats med sensorer som övervakar motorn villkor under svarta. Fabrikam använder hello förutsägande Underhåll lösningen toocollect hello sensordata som samlas in under hello svarta. När kan sparas års motorn operativa och fel data har Fabrikams datavetare modelleras en sätt toopredict hello återstående livslängd (RUL) ett flygplan-motorn. hello modellen använder en korrelation mellan data från fyra hello motorn sensorer och motorn förslitning som leder tooeventual fel. Medan Fabrikam fortsätter tooperform regelbundna kontroller tooensure säkerhet, kan nu använda hello modeller toocompute hello RUL för varje motor efter varje svarta. hello modellen använder hello telemetri som samlats in från hello motorer under hello svarta. Fabrikam kan nu förutsäga framtida tidpunkter för potentiella fel och planera för underhåll och reparation i förväg.

> [!NOTE]
> hello lösning modellen använder motorns verkliga förslitning data.

Fabrikam kan optimera sina åtgärder tooreduce kostnader genom att förutsäga hello punkt när Underhåll krävs.

Underhållskoordinatorer arbetar tillsammans med schemaläggare för att:

- Planera Underhåll toocoincide med ett flygplan stoppas på en viss plats.
- Se till att tillräckligt med tid är tillgänglig för hello flygplan toobe ur funktion utan att orsaka avbrott i schemat.
- tooschedule tekniker tooensure att flygplan går effektivt utan väntetid.

Inventeringskontrollchefer får underhållsplaner så att de kan optimera beställningsprocessen och reservdelslagret.

Dessa aktiviteter aktivera Fabrikam toominimize flygplan grunden tid och minska kostnaderna samtidigt hello passagerare och säkerhet teamet.

toounderstand hur [Azure IoT Suite] [ lnk_iot_suite] ger hello funktioner kunder måste toorealize hello potentiella förutsägande Underhåll igenom detta [infographic] [lnk_infographic].

## <a name="how-hello-predictive-maintenance-solution-is-built"></a>Hur hello förutsägande underhållslösningen byggs

hello lösningen använder en befintlig Azure Machine Learning-modell tillgänglig som en mall tooshow dessa funktioner fungerar från enheten telemetri som samlats in via IoT Suite-tjänster. Microsoft har skapat en [regressionsmodell] [ lnk_regression_model] av flygplan motor baserat på offentligt tillgängliga data<sup>\[1\]</sup>, och stegvisa information om hur toouse hello modellen.

hello Azure IoT förutsägande underhållslösningen används hello regressionsmodell som skapas utifrån mallen. hello-modellen har distribuerats till din Azure-prenumeration och exponeras via en automatiskt genererad API. hello lösningen innehåller en delmängd av hello testa data som representerar 4 (i 100 totala) motorer och hello 4 (av totalt 21) sensor dataströmmar. Dessa data är tillräckligt tooprovide en korrekt resultat från hello tränad modell.

*\[1\] A. Saxena och K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*

## <a name="get-started-with-predictive-maintenance"></a>Kom igång med förebyggande underhåll

Den här kursen visar hur tooprovision hello förutsägande Underhåll lösning. Även vägleder dig genom hello grundläggande funktioner i hello förutsägande Underhåll lösning. Du kan komma åt många av dessa funktioner via hello lösning instrumentpanelen som distribuerar tillsammans med hello förkonfigurerade lösningen.

toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.

> [!NOTE]
> Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].

1. Logga in för[azureiotsuite.com] [ lnk-azureiotsuite] med din Azure kontoautentiseringsuppgifter och klicka på ** + ** toocreate en lösning.
1. Klicka på **Välj** hello **förutsägande Underhåll** panelen.
1. Ange ett **lösningsnamn** för din förkonfigurerade lösning för förebyggande underhåll.
1. Välj hello **Region** och **prenumeration** du vill toouse tooprovision hello-lösningen.
1. Klicka på **skapa lösningen** toobegin hello etableringsprocessen. Denna process brukar ta flera minuter toorun.

### <a name="wait-for-hello-provisioning-process-toocomplete"></a>Vänta tills hello etablering processen toocomplete

1. Klicka på hello panelen för din lösning med **etablering** status.
1. Meddelande hello **etablering tillstånd** som Azure-tjänster har distribuerats i din Azure-prenumeration.
1. När etableringen är klar hello status ändras för**klar**.
1. Klicka på panelen hello toosee hello information i lösningen i hello högra rutan. Från det här fönstret kan du starta hello lösning instrumentpanel och åtkomst hello Machine Learning-arbetsytan.

> [!NOTE]
> Om du får problem som distribuerar hello förkonfigurerade lösningen granska [behörigheter på hello azureiotsuite.com platsen] [ lnk-permissions] och hello [vanliga frågor och svar] [ lnk-faq]. Om hello problem kvarstår, skapa en tjänstbiljett på hello [portal][lnk-portal].

Finns det information som du förväntar dig toosee som inte visas för lösningen? Lämna förslag på funktioner i [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## <a name="view-hello-solution"></a>Visa hello lösning

Det här avsnittet hjälper dig att hello lösning Användargränssnittet.

### <a name="predictive-maintenance-dashboard"></a>Instrumentpanel för förebyggande underhåll

Den här sidan i hello webbprogram använder PowerBI JavaScript-kontroller (se hello [PowerBI-visuell information databasen][lnk-powerbi]) toovisualize:

* hello utdata från hello Stream Analytics-jobb i blob storage.
* Hej RUL och cykel antal per flygplan motorn.

### <a name="observing-hello-behavior-of-hello-cloud-solution"></a>Sett hello funktionssätt hello molnlösning

I hello Azure portal, navigerar toohello resursgrupp med hello lösningens namn du har valt tooview etablerade resurserna.

![][img-resource-group]

När du etablerar hello förkonfigurerade lösningen kan du få ett e-postmeddelande med en länk toohello Machine Learning-arbetsytan. Du kan också navigera toohello Machine Learning-arbetsytan från hello [azureiotsuite.com] [ lnk-azureiotsuite] sidan för etablerade lösningen. En panel är tillgänglig på den här sidan när hello lösningen är i hello **klar** tillstånd.

![][img-machine-learning]

Du kan se hello urvalet har etablerats med fyra simulerade enheter toorepresent två flygplan med två motorer per flygplan, var och en med fyra sensorer i hello lösning-portalen. När du först navigerar toohello lösning portal har hello simulering stoppats.

![][img-simulation-stopped]

Klicka på **starta simuleringen** toobegin hello simuleringen. Hej sensor historik, RUL, cykler och RUL historik fylla hello instrumentpanelen.

![][img-simulation-running]

När RUL är mindre än 160 (en godtycklig tröskel valt), visas hello lösning en varning symbolen nästa toohello RUL visas. hello lösning portal visar också hello flygplan motorn i gult. Observera hur hello RUL värden ha en allmän nedåtgående trend övergripande men tenderar toobounce uppåt och nedåt. Det här beteendet fås hello varierande cykel längder och hello modellen noggrannhet.

![][img-simulation-warning]

hello fullständig simuleringen tar cirka 35 minuter toocomplete 148 cykler. hello 160 RUL tröskelvärdet har uppnåtts för hello första gången vid cirka 5 minuter och båda motorer träffar hello tröskelvärdet cirka åtta minuter.

hello simuleringen körs via hello fullständiga datauppsättningen för 148 cykler och reglerar på slutvärdena RUL och cykel.

Du kan stoppa hello simuleringen vid någon plats, men att klicka på **starta simuleringen** repetitioner hello simulering från hello start av hello dataset.

## <a name="next-steps"></a>Nästa steg

Mer information om hur Azure IoT möjliggör scenarier med förutsägande Underhåll läsa toolearn [avbilda värde från hello Sakernas Internet][lnk_capture_value].

Ta en [genomgången] [ lnk-predictive-walkthrough] hello förutsägande Underhåll lösning.

Du kan även utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:

* [Vanliga frågor och svar om IoT Suite][lnk-faq]
* [IoT-säkerhet från hello bakgrund][lnk-security-groundup]

[img-resource-group]: media/iot-suite-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-suite-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-suite-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-permissions]: iot-suite-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/