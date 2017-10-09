---
title: "aaaGet igång med förkonfigurerade lösningar | Microsoft Docs"
description: "Följ den här självstudiekursen toolearn hur toodeploy ett Azure IoT Suite förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: a7f46023d26b08de2e8ed48c34c5066a43e3fa38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-preconfigured-solutions"></a>Kom igång med hello förkonfigurerade lösningar

Azure IoT Suite [förkonfigurerade lösningar] [ lnk-preconfigured-solutions] kombinera flera Azure IoT services toodeliver slutpunkt till slutpunkt-lösningar som implementerar vanliga scenarier för IoT-företag. Hej *fjärrövervaknings* förkonfigurerade lösningen ansluter tooand övervakar dina enheter. Du kan använda hello lösning tooanalyze hello dataström från dina enheter och tooimprove affärsresultatet genom att göra processer svara automatiskt toothat dataström.

Den här kursen visar hur tooprovision hello fjärrövervaknings förkonfigurerade lösningen. Även vägleder dig genom hello grundläggande funktioner i hello förkonfigurerade lösningen. Du kan komma åt många av dessa funktioner från hello lösning *instrumentpanelen* som distribuerar som en del av hello förkonfigurerade lösningen:

![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-dashboard]

toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.

> [!NOTE]
> Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a>Scenarioöversikt

När du distribuerar hello remote förkonfigurerade övervakningslösning förinställd den med resurser som gör att toostep via ett vanligt scenario för fjärråtkomst övervakning. I det här scenariot lösning för flera enheter anslutna toohello rapporterar oväntat temperatur värden. hello följande avsnitt visar hur till:

* Identifiera hello enheter skickar oväntat temperatur värden.
* Konfigurera de här enheterna toosend mer detaljerad telemetri.
* Korrigera hello problemet genom att uppdatera hello inbyggd programvara på enheterna.
* Kontrollera att åtgärden har löst problemet hello.

En nyckelfunktion i det här scenariot är att du kan utföra dessa åtgärder via fjärranslutning från hello lösning instrumentpanelen. Du behöver inte fysisk åtkomst toohello enheter.

## <a name="view-hello-solution-dashboard"></a>Visa hello lösning instrumentpanelen

hello lösning instrumentpanelen kan du toomanage hello distribueras lösning. Du kan till exempel visa telemetri, lägga till enheter och konfigurera regler.

1. När hello etableringen har slutförts och hello panelen för din förkonfigurerade lösning anger **klar**, Välj **starta** tooopen fjärråtkomst övervakning lösning portalen på en ny flik.

    ![Starta hello förkonfigurerade lösningen][img-launch-solution]

1. Som standard visar hello lösning portal hello *instrumentpanelen*. Du kan navigera tooother områden i hello lösning portal med hello menyn hello vänster hello-sidan.

    ![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-menu]

hello instrumentpanelen visar hello följande information:

* En karta som visar hello platsen för varje enhet ansluten toohello lösning. När du först kör hello lösning finns 25 simulerade enheter. hello simulerade enheterna implementeras som Azure WebJobs och hello lösningen använder hello Bing Maps API tooplot information på hello karta. Se hello [vanliga frågor och svar] [ lnk-faq] toolearn hur toomake hello kartan dynamiska.
* Panelen **Telemetrihistorik** ritar upp fuktighets- och temperaturtelemetri från en vald enhet nästan i realtid och visar aggregerade data, till exempel högsta, lägsta och genomsnittlig fuktighet.
* Panelen **Larmhistorik** visar de senaste larmhändelserna när ett telemetrivärde har överskridit ett tröskelvärde. Du kan definiera dina egna larm i tillägg toohello exempel som skapats av hello förkonfigurerade lösningen.
* På panelen **Jobb** visas information om schemalagda jobb. Du kan schemalägga egna jobb på sidan **Hanteringsjobb**.

## <a name="view-alarms"></a>Visa larm

panelen för hello larm händelser visar att fem enheter rapporterar högre än förväntade telemetri värden.

![TODO larm historik på instrumentpanelen för hello-lösning][img-alarms]

> [!NOTE]
> Dessa larm genereras av en regel som ingår i hello förkonfigurerade lösningen. Den här regeln genererar en avisering när hello temperatur värdet som skickas av en enhet överstiger 60. Du kan definiera dina egna regler och åtgärder genom att välja [regler](#add-a-rule) och [åtgärder](#add-an-action) hello vänstra menyn.

## <a name="view-devices"></a>Visa enheter

Hej *enheter* listan visas alla hello registrerade enheter i hello-lösning. Lägg till eller ta bort enheter och anropa metoder i enheter hello enheten listan som du kan visa och redigera enhetens metadata. Du kan filtrera och sortera hello lista över enheter i listan över hello-enheter. Du kan också anpassa hello kolumner som visas i listan över hello-enheter.

1. Välj **enheter** tooshow hello listan över enheter för den här lösningen.

   ![Visa hello listan över enheter i hello lösning portal][img-devicelist]

1. hello enheten i listan visas först 25 simulerade enheter som skapats av hello etableringsprocessen. Du kan lägga till ytterligare simulerade och fysiska enheter toohello lösning.

1. tooview hello information om en enhet, Välj en enhet i listan över hello-enheter.

   ![Visa information om hello enhet i hello lösning portal][img-devicedetails]

Hej **enhetsinformation** panelen innehåller sex avsnitt:

* En samling länkar som aktiverar du toocustomize hello ikon, inaktivera hello enheten, lägga till en regel, anropa en metod eller skicka ett kommando. En jämförelse av kommandon (meddelanden från enheten till molnet) och metoder (direkta metoder) finns i [Cloud-to-device communications guidance][lnk-c2d-guidance] (Vägledning för kommunikation från moln till enhet).
* Hej **enheten dubbla - taggar** avsnittet kan du tooedit taggvärden för hello enhet. Du kan visa värden i listan över enheter hello och använda taggen värden toofilter hello listan över enheter.
* Hej **enheten dubbla - önskade egenskaper** avsnittet kan du tooset egenskapen värden toobe skickas toohello enhet.
* Hej **enheten dubbla - rapporterade egenskaper** avsnittet visar egenskapsvärden som skickas från hello enheten.
* Hej **enhetsegenskaper** avsnittet visar information från hello identitetsregistret till exempel hello enhet id- och autentiseringsnycklarna.
* Hej **senaste jobb** avsnittet visar information om alla jobb som har nyligen mål för den här enheten.

## <a name="filter-hello-device-list"></a>Filtrera listan över hello-enheter

Du kan använda ett filter toodisplay endast de enheter som skickar oväntat temperatur värden. hello fjärråtkomst övervakning förkonfigurerade lösningen innehåller hello **ohälsosamt enheter** filtrera tooshow enheter med ett medelvärde temperatur-värde som är större än 60. Du kan även [skapa egna filter](#add-a-filter).

1. Välj **öppna sparade filter** toodisplay en lista över tillgängliga filtren. Välj **ohälsosamt enheter** tooapply hello filter:

    ![Visa hello listan över filter][img-unhealthy-filter]

1. listan över hello-enheter visas nu endast enheter med ett medelvärde temperatur värde större än 60.

    ![Visa hello filtreras listan över enheter visar ohälsosamt][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a>Uppdatera önskade egenskaper

Nu har du identifierat en uppsättning enheter som kan behöva repareras. Men du bestämmer dig för hello data frekvens på 15 sekunder inte är tillräcklig för en tydlig diagnos av hello problemet. Ändra hello telemetri frekvens toofive sekunder tooprovide diagnostisera du med mer datapunkter toobetter hello problemet. Du kan skicka den här konfigurationen ändras tooyour fjärranslutna enheter från hello lösning portal. Du kan göra hello ändra en gång, utvärdera hello inverkan och sedan vidta åtgärder för hello resultat.

Följ dessa steg toorun ett jobb som ändrar hello **TelemetryInterval** önskad egenskap för hello berörda enheter. När hello enheter får nya hello **TelemetryInterval** egenskapsvärde, de ändra sin konfiguration toosend telemetri varje fem sekunder i stället för var 15: e sekund:

1. När du visar hello lista över enheter som ohälsosamt i listan över enheter hello väljer **jobbschemat**, sedan **redigera enheten dubbla**.

1. Anropa hello jobbet **intervall mellan lösenordsbyte telemetri**.

1. Ändra hello värdet på hello **önskade egenskapen** namn **önskade. Config.TelemetryInterval** toofive sekunder.

1. Välj **Schema**.

    ![Ändra hello TelemetryInterval egenskapen toofive sekunder][img-change-interval]

1. Du kan övervaka hello hello jobb på hello **Management jobb** sidan hello-portalen.

> [!NOTE]
> Om du vill toochange önskade egenskapsvärde för en enskild enhet använder hello **egenskaper** avsnitt i hello **enhetsinformation** panelen i stället för att köra ett jobb.

Det här jobbet anger hello-värdet för hello **TelemetryInterval** önskad egenskap i hello enheten identiska för alla hello enheter som valts av hello filter. hello enheter hämtar värdet från hello enheten dubbla och uppdatera deras beteende. När en enhet hämtar och bearbetar en önskad egenskap från en enhet dubbla, anger hello motsvarande rapporterade value-egenskap.

## <a name="invoke-methods"></a>Anropningsmetoder

När hello jobbet körs du upptäcker i hello lista över enheter som ohälsosamt att alla dessa enheter har gamla (mindre än version 1.6) programvaran versioner.

![Visa hello rapporterade version på inbyggd programvara för hello ohälsosamt enheter][img-old-firmware]

Den här versionen av inbyggd programvara kan vara hello grundorsaken till hello oväntat temperatur värden eftersom du vet att andra felfri enheter har nyligen uppdaterat tooversion 2.0. Du kan använda inbyggda hello **gamla firmware enheter** filtrera tooidentify alla enheter med äldre versioner av inbyggd programvara. Från hello-portalen kan du via fjärranslutning uppdatera alla hello-enheter som fortfarande kör äldre versioner av inbyggd programvara:

1. Välj **öppna sparade filter** toodisplay en lista över tillgängliga filtren. Välj **gamla firmware enheter** tooapply hello filter:

    ![Visa hello listan över filter][img-old-filter]

1. listan över hello-enheter visas nu endast enheter med äldre versioner av inbyggd programvara. Den här listan innehåller hello fem enheter som identifieras av hello **ohälsosamt enheter** filter och tre ytterligare enheter:

    ![Visa hello filtreras listan över enheter visar gamla enheter][img-filtered-old-list]

1. Välj **Jobbschema**, sedan **Anropa metod**.

1. Ange **jobbnamn** för**Firmware-uppdatering tooversion 2.0**.

1. Välj **InitiateFirmwareUpdate** som hello **metoden**.

1. Ange hello **FwPackageUri** parameter för**https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.

1. Välj **Schema**. hello standardvärdet är nu för hello jobbet toorun.

    ![Skapa jobbet tooupdate hello inbyggda hello markerade enheter][img-method-update]

> [!NOTE]
> Om du vill tooinvoke en metod i en enskild enhet väljer **metoder** i hello **enhetsinformation** panelen i stället för att köra ett jobb.

Det här jobbet anropar hello **InitiateFirmwareUpdate** direkt metod på alla hello-enheter som valts av hello filter. Enheter svara direkt tooIoT hubb och startar sedan hello firmware uppdateringsprocessen asynkront. hello enheter visar statusinformation om hello firmware uppdateringsprocessen via rapporterade egenskapsvärden som visas i följande skärmdumpar hello. Välj hello **uppdatera** ikonen tooupdate hello information enheten och jobbet i hello:

![Jobblistan som visar hello firmware uppdatera listan körs][img-update-1]
![listan över enheter visar status för uppdateringar av inbyggd programvara][img-update-2]
![lista visar hello firmware update jobblistan klar][img-update-3]

> [!NOTE]
> I en produktionsmiljö kan du schemalägga jobb toorun under en angiven underhållsperiod.

## <a name="scenario-review"></a>Scenariogranskning

I det här scenariot identifiera potentiella problem med en del av din fjärranslutna enheter med hello larm historik på hello instrumentpanelen och ett filter. Konfigurera hello enheter tooprovide du använt hello filter och sedan ett jobb tooremotely mer information toohelp diagnostisera hello problemet. Slutligen använde du ett filter och ett jobb tooschedule Underhåll på hello berörda enheter. Om du återgår toohello instrumentpanelen kan kontrollera du att det inte längre några larm som kommer från enheter i din lösning. Du kan använda ett filter tooverify som hello inbyggd programvara är uppdaterad på alla hello enheter i din lösning och att det finns inga fler ohälsosamt enheter:

![Filter som visar att alla enheter som har uppdaterad inbyggd programvara][img-updated]

![Filter som visar att alla enheter är felfria][img-healthy]

## <a name="other-features"></a>Andra funktioner

hello beskrivs följande avsnitt några ytterligare funktioner hello remote förkonfigurerade övervakningslösning som inte beskrivs som en del av hello föregående scenario.

### <a name="customize-columns"></a>Anpassa kolumner

Du kan anpassa hello informationen som visas i listan över hello-enheter genom att välja **kolumnen editor**. Du kan lägga till och ta bort kolumner som visar rapporterade egenskap- och taggvärden. Du kan även ändra ordningen och byta namn på kolumner:

   ![Kolumnen redigeraren med hello listan över enheter][img-columneditor]

### <a name="customize-hello-device-icon"></a>Anpassa hello ikon

Du kan anpassa hello ikon visas i listan över enheter hello från hello **enhetsinformation** panelen på följande sätt:

1. Välj hello penna ikonen tooopen hello **Redigera bild** för en enhet:

   ![Öppna redigeringsprogrammet för enhetsavbildningar][img-startimageedit]

1. Ladda upp en ny avbildning eller använda en befintlig hello-avbildningar och väljer sedan **spara**:

   ![Redigera redigeringsprogrammet för enhetsavbildningar][img-imageedit]

1. hello valda nu visas i hello **ikonen** kolumnen för hello enhet.

> [!NOTE]
> hello avbildningen lagras i blob storage. En tagg i hello enheten dubbla innehåller en länk toohello avbildning i blob storage.

### <a name="add-a-device"></a>Lägg till en enhet

När du distribuerar hello förkonfigurerade lösningen kan etablera du automatiskt 25 exempel enheter som du kan se i listan över hello-enheter. Dessa enheter är *simulerade enheter* som körs i ett Azure-webbjobb. Simulerade enheter gör det enkelt för dig tooexperiment med hello förkonfigurerade lösningen utan hello måste toodeploy verkliga fysiska enheter. Om du vill tooconnect en lösning för verklig enheter toohello, se hello [ansluta din enhet toohello remote förkonfigurerade övervakningslösning] [ lnk-connect-rm] kursen.

hello följande steg visar hur tooadd en simulerad enhet toohello lösning:

1. Gå tillbaka toohello enhetslistan.

1. tooadd en enhet, Välj **+ Lägg till en enhet** i hello nedre vänstra hörnet.

   ![Lägg till en enhet toohello förkonfigurerade lösning][img-adddevice]

1. Välj **Lägg till ny** på hello **simulerade enheten** panelen.

   ![Ange information om den nya enheten på instrumentpanelen][img-addnew]

   Tillägg toocreating en ny simulerade enhet, du kan också lägga till en fysisk enhet om du väljer toocreate en **anpassad enhet**. toolearn mer information om hur du ansluter fysiska enheter toohello lösning finns [ansluta din enhet toohello IoT Suite remote förkonfigurerade övervakningslösning][lnk-connect-rm].

1. Välj **Låt mig ange mitt eget enhets-ID** och ange ett unikt enhets-ID, t.ex. **mydevice_01**.

1. Välj **Skapa**.

   ![Spara den nya enheten][img-definedevice]

1. I steg 3 i **lägger till en simulerad enhet**, Välj **klar** tooreturn toohello enhetslistan.

1. Du kan visa din enhet **kör** i listan över hello-enheter.

    ![Visa den nya enheten i enhetslistan][img-runningnew]

1. Du kan också visa hello simulerade telemetri från den nya enheten på hello instrumentpanelen:

    ![Visa telemetri från den nya enheten][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a>Inaktivera och ta bort en enhet

Du kan inaktivera en enhet och när den har inaktiverats kan du ta bort den:

![Inaktivera och ta bort en enhet][img-disable]

### <a name="add-a-rule"></a>Lägg till en regel

Det finns inga regler för hello ny enhet som du just lagt till. I det här avsnittet kan du lägga till en regel som utlöser ett larm när hello temperatur som rapporterats av hello ny enhet överskrider 47 grader. Innan du börjar, Lägg märke till att hello telemetri historik för hello ny enhet på hello instrumentpanelen visar hello enheten temperatur aldrig överskrider 45 grader.

1. Gå tillbaka toohello enhetslistan.

1. tooadd en regel för hello enheten, Välj den nya enheten i hello **enhetslistan**, och välj sedan **Lägg till regel**.

1. Skapa en regel som använder **temperatur** som hello datafält och använder **AlarmTemp** som hello utdata när hello temperatur överskrider 47 grader:

    ![Lägga till en enhetsregel][img-adddevicerule]

1. toosave ändringarna, Välj **spara och visa regler**.

1. Välj **kommandon** i hello informationspenelen för hello ny enhet.

    ![Lägga till en enhetsregel][img-adddevicerule2]

1. Välj **ChangeSetPointTemp** hello Kommandolistan och ange **SetPointTemp** too45. Välj sedan **kommandot Skicka**:

    ![Lägga till en enhetsregel][img-adddevicerule3]

1. Gå tillbaka toohello instrumentpanelen. Efter en kort tid ser en ny post i hello **larm historik** när hello temperatur som rapporterats av den nya enheten fönster överskrider hello 47 graders tröskeln:

    ![Lägga till en enhetsregel][img-adddevicerule4]

1. Du kan granska och redigera alla regler på hello **regler** sidan hello instrumentpanelen:

    ![Visa en lista över enhetsregler][img-rules]

1. Du kan granska och redigera alla hello-åtgärder som kan vidtas i svaret tooa regel på hello **åtgärder** sidan hello instrumentpanelen:

    ![Visa en lista över enhetsåtgärder][img-actions]

> [!NOTE]
> Det är möjligt toodefine åtgärder som kan skicka ett e-postmeddelande eller SMS i svaret tooa regel eller integrera med en line-of-business-systemet via en [Logikapp][lnk-logic-apps]. Mer information finns i hello [ansluta Logikapp tooyour Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen][lnk-logicapptutorial].

### <a name="manage-filters"></a>Hantera filter

I listan över enheter hello kan du skapa, spara och Läs in filter toodisplay en egen lista över enheter anslutna tooyour hubb. toocreate ett filter:

1. Välj hello redigera filterikonen ovan hello lista över enheter:

    ![Öppna hello filter redigeraren][img-editfiltericon]

1. I hello **Filter editor**, lägga till hello fält, operatorer och värden toofilter hello enhetslistan. Du kan lägga till flera satser toorefine filtret. Välj **Filter** tooapply hello filter:

    ![Skapa ett filter][img-filtereditor]

1. I det här exemplet filtreras hello listan efter tillverkare och modell tal:

    ![Filtrerad lista][img-filterelist]

1. toosave filtret med ett anpassat namn väljer hello **Spara som** ikon:

    ![Spara ett filter][img-savefilter]

1. tooreapply ett filter som du sparade tidigare, Välj hello **öppna sparade filter** ikon:

    ![Öppna ett filter][img-openfilter]

Du kan skapa filter som baseras på enhets-id, enhetstillstånd, önskade egenskaper, rapporterade egenskaper och taggar. Du lägger till dina egna anpassade taggar tooa enhet i hello **taggar** avsnitt i hello **enhetsinformation** Kontrollpanelen eller köra ett jobb tooupdate taggar på flera enheter.

> [!NOTE]
> I hello **Filter editor**, du kan använda hello **Avancerat läge** tooedit hello frågetexten direkt.

### <a name="commands"></a>Kommandon

Från hello **enhetsinformation** Kontrollpanelen, kan du skicka kommandon toohello enhet. När en enhet startar, skickar information om hello kommandon stöder toohello lösning. En beskrivning av hello skillnaderna mellan kommandon och metoder finns [alternativ för Azure IoT Hub-moln till enhet][lnk-c2d-guidance].

1. Välj **kommandon** i hello **enhetsinformation** panelen för hello vald enhet:

   ![Enhetskommandon på instrumentpanelen][img-devicecommands]

1. Välj **PingDevice** hello kommandot listan.

1. Klicka på **Skicka kommando**.

1. Du kan se hello status för hello-kommandot i historiken för hello-kommando.

   ![Kommandostatus på instrumentpanelen][img-pingcommand]

hello lösning spårar hello status för varje kommando som skickas. Ursprungligen hello resultatet är **väntande**. När hello enheten rapporterar att den har körts hello-kommandot, hello är resultatet för**lyckade**.

## <a name="behind-hello-scenes"></a>Hello bakgrunden

När du distribuerar en förkonfigurerade lösning skapar hello distributionsprocessen flera resurser i hello Azure-prenumeration du valt. Du kan visa dessa resurser i hello Azure [portal][lnk-portal]. hello distributionsprocessen skapar en **resursgruppen** med ett namn baserat på hello namn du väljer för din förkonfigurerade lösning:

![Förkonfigurerade lösning i hello Azure-portalen][img-portal]

Du kan visa hello inställningarna för varje resurs som väljs i hello listan över resurser i hello resursgruppen.

Du kan också visa hello källkoden för hello förkonfigurerade lösningen. fjärråtkomst övervakning förkonfigurerade lösningen källkoden hello är i hello [azure iot-fjärr-övervakning] [ lnk-rmgithub] GitHub-lagringsplatsen:

* Hej **DeviceAdministration** mappen innehåller hello källkoden för hello instrumentpanelen.
* Hej **Simulator** mappen innehåller hello källkoden för hello simulerade enheten.
* Hej **EventProcessor** mappen innehåller hello källkoden för hello backend-processen som hanterar hello inkommande telemetri.

När du är klar kan du ta bort hello förkonfigurerade lösningen från din Azure-prenumeration på hello [azureiotsuite.com] [ lnk-azureiotsuite] plats. Den här webbplatsen kan tooeasily ta bort alla hello resurser som etablerades när du skapade hello förkonfigurerade lösningen.

> [!NOTE]
> tooensure att du tar bort allt relaterade toohello förkonfigurerade lösningen kan ta bort den på hello [azureiotsuite.com] [ lnk-azureiotsuite] plats och ta inte bort hello resursgrupp i hello-portalen.

## <a name="next-steps"></a>Nästa steg

Nu när du har distribuerat en fungerande förkonfigurerade lösning kan fortsätta du komma igång med IoT Suite genom att läsa hello följande artiklar:

* [Genomgång av den förkonfigurerade lösningen för fjärrövervakning][lnk-rm-walkthrough]
* [Ansluta din enhet toohello remote förkonfigurerade övervakningslösning][lnk-connect-rm]
* [Behörigheter för hello azureiotsuite.com plats][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-faq.md