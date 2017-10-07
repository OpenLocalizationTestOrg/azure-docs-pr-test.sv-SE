---
title: "aaaAzure IoT Suite anslutna factory översikt | Microsoft Docs"
description: "En beskrivning av hello Azure IoT Suite ansluten factory förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 929b5ed41ef7d82c9b4480d02aa3f0db38931919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-connected-factory-preconfigured-solution"></a>Kom igång med hello anslutna factory förkonfigurerade lösningen

Azure IoT Suite [förkonfigurerade lösningar] [ lnk-preconfigured-solutions] kombinera flera Azure IoT services toodeliver slutpunkt till slutpunkt-lösningar som implementerar vanliga scenarier för IoT-företag. Hej *anslutna factory* förkonfigurerade lösningen ansluter tooand Övervakare industriella enheter. Du kan använda hello lösning tooanalyze hello dataström från dina enheter och toodrive operativa produktivitet och lönsamhet.

Den här kursen visar hur tooprovision hello anslutna factory förkonfigurerade lösningen. Även vägleder dig genom hello grundläggande funktioner i hello förkonfigurerade lösningen. Du kan komma åt många av dessa funktioner från hello lösning *instrumentpanelen* som distribuerar som en del av hello förkonfigurerade lösningen:

![instrumentpanel för den förkonfigurerade lösningen ansluten fabrik][img-cf-home]

toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.

> [!NOTE]
> Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].
> 
> 

## <a name="provision-hello-solution"></a>Etablera hello lösning

1. Logga in med dina inloggningsuppgifter för Azure-konto tooazureiotsuite.com och klicka på ”**+**” toocreate en lösning.
2. Klicka på **Välj** på hello **anslutna factory** panelen.
3. Ange ett **lösningsnamn** för den förkonfigurerade anslutna fabriken för fjärrövervakning.
4. Välj hello **prenumeration** och **Region** du vill toouse tooprovision hello-lösningen.
5. Klicka på **skapa lösningen** toobegin hello etableringsprocessen. Denna process brukar ta flera minuter toorun.

### <a name="while-you-wait-for-hello-provisioning-process-toocomplete"></a>Medan du väntar hello etablering processen toocomplete

1. Klicka på hello panelen för din lösning med **etablering** status.
2. Meddelande hello **etablering tillstånd** som Azure-tjänster har distribuerats i din Azure-prenumeration.
3. När etableringen är klar hello status ändras för**klar**.
4. Klicka på panelen hello toosee hello information i lösningen i hello högra rutan.

> [!NOTE]
> Om du får problem som distribuerar hello förkonfigurerade lösningen granska [behörigheter på hello azureiotsuite.com platsen] [ lnk-permissions] och hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md). Om hello problem kvarstår, skapa en tjänstbiljett på hello [portal][lnk-portal].

Finns det information som du förväntar dig toosee som inte visas för lösningen? Lämna förslag på funktioner i [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## <a name="scenario-overview"></a>Scenarioöversikt

När du distribuerar hello anslutna factory förkonfigurerade lösningen, förinställd med resurser som gör att du toostep via ett vanligt scenario för industriella. I det här scenariot flera fabriker anslutna toohello lösning rapporten hello värden krävs toocompute övergripande utrustning effektivitet (OEE) och nyckeltal (KPI: er). hello följande avsnitt visar hur till:

* Övervaka fabrik, produktionslinjer, station OEE och KPI-värden
* Analysera hello telemetridata som genereras från dessa enheter med hjälp av Azure tid serien Insights
* Arbeta med aviseringar toofix problem

En nyckelfunktion i det här scenariot är att du kan utföra dessa åtgärder via fjärranslutning från hello lösning instrumentpanelen. Du behöver inte fysisk åtkomst toohello enheter.

## <a name="view-hello-solution-dashboard"></a>Visa hello lösning instrumentpanelen

hello lösning instrumentpanelen kan du toomanage hello distribueras lösning. Det här är en hierarkisk representation av en global fabrikskonfiguration. Du kan till exempel se OEE och KPI:er, publicera nya noder för telemetri och åtgärdsaviseringar.

1. När hello etableringen har slutförts och hello panelen för din förkonfigurerade lösning anger **klar**, Välj **starta** tooopen anslutna factory lösning portalen på en ny flik.

    ![Starta hello förkonfigurerade lösningen][img-launch-solution]

1. Som standard visar hello lösning portal hello *instrumentpanelen*. toonavigate tooother områden av hello portal använder hello-menyn i hello vänster sida av hello-sidan.

    ![Instrumentpanel för den förkonfigurerade lösningen Ansluten fabrik][cf-img-menu]

hello instrumentpanelen visar hello följande information:

* En **Factory listan** panelen som visar hello status, plats och den aktuella produktion konfigurationen i hello-lösning. När du kör först hello lösning, finns det ett antal simulerade enheter. hello produktionen simuleringen består av tre verkliga OPC UA servrar per rad produktion som utför simulerade aktiviteter och dela data. Mer information om OPC UA finns hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md).
* En **kartan** att visar hello platsen för varje enhet ansluten toohello lösning. hello-lösning kan använda hello Bing Maps API tooplot information på hello karta. Om Bing Maps Enterprise API är aktiverat för din prenumeration används den här funktionen automatiskt. Om inte, finns hello [vanliga frågor och svar] [ lnk-faq] toolearn hur toomake hello kartan dynamiska.
* En panel för **aviseringar** som visar aviseringar som genereras när ett telemetri- eller OEE/KPI-värde överskrider ett visst tröskelvärde.
* En **utrustning effektiviteten** panelen som visar hello OEE värden för hello hela företaget eller hello factory/produktion rad/station du visar. Det här värdet sammanställs från hello station visa toohello företagsnivå. Hej OEE bild och dess ingående element kan analyseras ytterligare.
* **Prestationsindikatorer** panelen som visar hello antalet enheter som producerats och energi som används med hello hela företaget eller hello factory/produktion rad/station du visar. Dessa värden sammanställs från en station visa toohello enterprise-nivå.

## <a name="view-factories"></a>Visa fabriker

Hej *fabriker* panelen visar du hello geografiska placeringen för alla hello fabriker i hello lösning, status och aktuell konfiguration för produktion. Du kan navigera andra nivåer i hierarkin för hello lösning toohello hello platser listan. hello rader i hello listan är hyperlänkar som länkar information om hello produktion rader på den platsen. Är det möjligt toodrill hello produktionen detaljer och ned toohello station nivån vyn. Du kan också använda en toohello filterlistan.

![Fabriker för den förkonfigurerade lösningen Ansluten fabrik][cf-img-factories] 

1. Hej **Factory panelen** visar hello factory listan för den här lösningen.

2. hello factory i listan visas först sex fabriker som skapats av hello etableringsprocessen. Du kan lägga till ytterligare simulerade och fysiska enheter toohello lösning.

3. tooview hello detaljer för en fabrik Klicka hello rad i hello factory lista.

4. tooview hello information om en rad för produktion, klicka på hello rad i hello lista.

5. tooview hello publicerade OPC UA noder i en station hello produktion rad, klicka på hello rad i hello lista.

6. tooview information på en viss nod i hello station, klicka på hello rad i hello lista. Den här åtgärden startar hello kontexten panel med tiden serien insikter visualiseringar. Klicka på de här diagrammen toodo ytterligare analys i hello tid serien insikter explorer miljö.

## <a name="view-map"></a>Visa karta

Om din prenumeration har åtkomst toohello Bing Maps API, hello *fabriker* karta visar hello geografisk plats och status för alla hello fabriker i hello lösning. toodrill hello plats detaljer Klicka hello platser som visas på kartan hello.

![Karta för den förkonfigurerade lösningen Ansluten fabrik][cf-img-map]

## <a name="view-alerts"></a>Visa aviseringar

Hej **avisering** panelen visas aviseringar på grund av tooa rapporterade värde eller ett beräknat OEE/KPI-värde som överskrider den konfigurerade tröskeln. Den här panelen visas aviseringar på varje nivå på hello hierarkin från hello station nivå toohello globala vyn. hello aviseringar innehåller en beskrivning av hello avisering, datum, tid, plats och antalet förekomster. Du kan få insyn i toohello data som orsakade hello avisering med hello tid serien Insights-data. hello tid serien insikter data visualiseras i hello aviseringar om tillämpligt. Om du är administratör kan koppla du standardåtgärder på hello aviseringar som:

* Stäng hello avisering.
* Bekräfta hello avisering.

Du kan också kan du utföra mer komplexa åtgärder. För hello trycket OPC UA nod i hello sammansättningen kan du till exempel:

* Visa mer information på en webbsida i ett nytt webbläsarfönster.
* Minska hello orsaken till hello avisering genom att anropa en metod för OPC UA på hello enhet.
* Utelämna hello tillgängligheten för hello standardåtgärder.

    ![Aviseringar för den förkonfigurerade lösningen Ansluten fabrik][cf-img-alerts]

> [!NOTE]
> De här aviseringarna genereras av regler som har angetts i en konfigurationsfil i hello förkonfigurerade lösningen. De här reglerna kan generera aviseringar när hello OEE eller KPI siffror eller OPC UA nod värden överskrider den konfigurerade tröskeln.

1. Hej **aviseringar panelen** visar hello aviseringarna som skapats i den här lösningen.

2. tooview hello information om en avisering på hello avisering hello aviseringar Kontrollpanelen.

3. toofurther analysera hello aviseringsdata, klicka på hello diagram i hello avisering panelen tooopen hello tid serien insikter explorer miljö.

4. tooaddress Hej avisering, flera åtgärder är tillgängliga i hello avisering panelen. Välj hello lämpligt alternativ för dig och på hello köra åtgärden kommandoknapp.

## <a name="view-overall-equipment-efficiency"></a>Visa OEE (Overall Equipment Efficiency)

OEE priser hello effektiviteten för hello produktionsprocess med hjälp av en nyckel produktions-relaterade parametrar. OEE är en branschstandard som standard mått beräknas genom att multiplicera hello tillgänglighet hastighet, hastighet för prestanda och kvalitet snabbt: OEE = tillgänglighet x kvaliteten x.

![OEE för den förkonfigurerade lösningen Ansluten fabrik][cf-img-oee]

1. tooview OEE för en nivå i hierarkin hello, navigera toohello specifik vy som du behöver. Hej OEE för vyn visar hello Kontrollpanelen tillsammans med var och en av hello-element som utgör hello OEE procent.

2. toofurther analysera hello OEE för en nivå i hello hierarkidata, klicka på hello OEE, tillgänglighet, prestanda och kvalitet procent. En kontext panel visas med tiden serien insikter driven visualiseringar som visar data från hello senaste timmen, senaste 24 timmarna och senaste 7 dagarna.

    ![TSI-visualisering för den förkonfigurerade lösningen Ansluten fabrik][cf-img-tsi-visualization]

3. toofurther analysera hello aviseringsdata, klicka på hello diagrammet hello avisering Kontrollpanelen. Den här åtgärden öppnar hello tid serien insikter explorer miljö.

    ![TSI-utforskaren för den förkonfigurerade lösningen Ansluten fabrik][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a>Visa KPI: er

hello lösningen ger två nyckeltal *enheter per timme* och *energi som används i kWh*.

![KPI för den förkonfigurerade lösningen Ansluten fabrik][cf-img-kpi]

1. tooview enheter per timme eller energi som används för alla nivåer i hierarkin hello, navigera toohello specifik vy som du behöver. hello-enheter per timme och energi som används visas hello-panelen.

2. tooanalyze enheter per timme eller energi som används för varje nivå i hierarkin för hello ytterligare, klicka på hello mätaren i hello **prestationsindikatorer** panelen. En kontext panel visas med tid serien insikter påslagen visualiseringar som gör att du tooview data från hello senaste timmen hello senaste 24 timmarna och senaste 7 dagarna.

## <a name="scenario-review"></a>Scenariogranskning

I det här scenariot övervakade din fabriker OEE och KPI: er värden i hello instrumentpanelen. Du sedan för tid serien insikter tooprovide mer information toohelp detaljerat ytterligare hello telemetridata för OEE och KPI: er toohelp med upptäcka avvikelser. Du också används hello avisering panelen tooview problem med din fabriker och du använde hello åtgärder tillgängliga tooyou tooresolve hello avisering.

## <a name="other-features"></a>Andra funktioner

hello följande avsnitt beskrivs några ytterligare funktionerna i hello anslutna factory lösning som inte beskrivs i föregående hello-scenariot.

## <a name="apply-filters"></a>Använda filter

1. Klicka på hello **ikonen** toodisplay en lista över tillgängliga filter på antingen hello factory platser panelen eller hello aviseringar.

2. hello filterpanelen visas för dig. 

    ![Filter för den förkonfigurerade lösningen Ansluten fabrik][cf-img-alert-filter]

3. Välj hello filter som du behöver. Det är också möjligt tootype fritext i hello filter-fält.

4. hello-filter används sedan för dig. hello Filterläge visas också i hello instrumentpanel via en Trattens som visar hello fabriker och aviseringar tabeller.

    ![Filter för den förkonfigurerade lösningen Ansluten fabrik][cf-img-alert-filter-funnel]

    > [!NOTE]
    > Ett aktivt filter påverkar inte hello visas OEE och KPI värden endast filtrerar hello lista innehåll.

5. tooclear ett filter, klicka på hello Trattens och filter i hello filterpanel kontext. Hej text **alla** visas i hello fabriker och aviseringar tabeller.

## <a name="browse-an-opc-ua-server"></a>Navigera i en OPC UA-serverläsare

När du distribuerar hello förkonfigurerade lösningen kan etablera du automatiskt simulerade OPC UA-servrar som du kan bläddra via hello lösning webbläsare. Dessa servrar är *simulerade OPC UA-servrar*. Simulerade servrar gör det enkelt för dig tooexperiment med hello förkonfigurerade lösningen utan hello måste toodeploy verkliga fysiska servrar. Om du vill tooconnect en verklig OPC UA toohello serverlösning, se hello [ansluta OPC UA enheten toohello anslutna factory förkonfigurerade lösningen] [ lnk-connect-cf] kursen.

1. Klicka på hello **factory ikonen** i navigeringsfältet i hello instrumentpanelen.

    ![Serverläsare för den förkonfigurerade lösningen Ansluten fabrik][cf-img-server-browser]

2. Välj en av servrarna hello hello förkonfigurerade listan. Den här listan visar hello-servrar som distribueras i hello förkonfigurerade lösningen.

    ![Serverurval för den förkonfigurerade lösningen Ansluten fabrik][cf-img-server-choice]

3. Klicka på **Connect** (Anslut) för att visa en dialogruta för säkerhet. Hello simuleringen, är det säkra tooclick **Fortsätt**

4. tooexpand någon av hello noderna i trädet för hello server klickar du på den. Noder som publicerar telemetri har ett skalstreck bredvid sig.

    ![Serverträd för den förkonfigurerade lösningen Ansluten fabrik][cf-img-server-tree]

5. Högerklicka på ett objekt tooread, skriva, publicera eller anropa noden. hello åtgärder tillgängliga tooyou beror på dina behörigheter och hello attribut för hello-nod. hello läsa alternativet toodisplays en kontext panel visar hello värdet hello specifik nod. hello skriva alternativet visar en kontext panel där du kan ange ett nytt värde. Hej samtalsalternativet visar en nod där du kan ange hello parametrar för hello-anrop.

## <a name="publish-a-node"></a>Publicera en nod

När du bläddrar en *simulerade OPC UA server*, du kan också välja toopublish nya noder. Du kan analysera hello telemetri från dessa noder i hello-lösning. Dessa *simulerade OPC UA servrar* gör det enkelt tooexperiment med hello förkonfigurerade lösningen utan att distribuera verkliga fysiska enheter.

1. Bläddra tooa nod i hello OPC UA webbläsare serverträdet toopublish gärna.

2. Högerklicka på hello-nod.

3. Välj **Publish** (Publicera).

    ![Ansluten fabrik publicerar nod][cf-img-publish-node]

4. En kontext ruta visas som talar om att hello publicera har slutförts. hello noden visas på hello station nivån med en bock bredvid den.

    ![Slutförd publicering i den förkonfigurerade lösningen Ansluten fabrik][cf-img-publish-success]

## <a name="command-and-control"></a>Kommando och kontroll

hello anslutna fabriken kan du kommandot och kontrollera dina branschen enheter direkt från hello molnet. Du kan använda den här funktionen toorespond tooalerts som genererats av hello enhet. Du kan till exempel skicka en kommandot toohello enhet från hello molnet. Du hittar hello tillgängliga kommandon i hello **StationCommands** nod i hello OPC UA servrar webbläsarträdet. Öppna en trycket versionen används på hello sammansättningen station av en rad för produktion i München i det här scenariot. toouse hello-kommando- och funktioner, måste du vara i hello **administratör** roll för hello förkonfigurerade lösningsdistribution.

1. Bläddra toohello **StationCommands** nod i hello OPC UA serverträdet webbläsare.

2. Välj hello-kommandot som du vill använda.

3. Högerklicka på hello-nod.

4. Välj **Call** (Anropa).

    ![Anropskommando för den förkonfigurerade lösningen Ansluten fabrik][cf-img-call-command]

5. En kontext ruta visas information om vilken metod du toocall och eventuella parameterinformation gäller.

6. Välj **Call** (Anropa).

    ![Kontextpanel för anrop för den förkonfigurerade lösningen Ansluten fabrik][cf-img-call-context]

7. hello kontexten panelen är uppdaterade tooinform att hello-metodanrop lyckades. Du kan kontrollera hello anropet lyckades genom att läsa hello värdet för hello trycket nod som har uppdaterats på grund av hello-anrop.

    ![Slutfört anrop för den förkonfigurerade lösningen Ansluten fabrik][cf-img-call-success]


## <a name="behind-hello-scenes"></a>Hello bakgrunden

När du distribuerar en förkonfigurerade lösning skapar hello distributionsprocessen flera resurser i hello Azure-prenumeration du valt. Du kan visa dessa resurser i hello Azure [portal][lnk-portal]. hello distributionsprocessen skapar en **resursgruppen** med ett namn baserat på hello namn du väljer för din förkonfigurerade lösning:

![Förkonfigurerade lösning i hello Azure-portalen][img-cf-portal]

Du kan visa hello inställningarna för varje resurs som väljs i hello listan över resurser i hello resursgruppen.

Du kan också visa hello källkoden för hello förkonfigurerade lösningen. hello källkoden för anslutna factory förkonfigurerade lösningen finns i hello [azure iot-ansluten-fabrik] [ lnk-cfgithub] GitHub-lagringsplatsen:

När du är klar kan du ta bort hello förkonfigurerade lösningen från din Azure-prenumeration på hello [azureiotsuite.com] [ lnk-azureiotsuite] plats. Den här webbplatsen kan tooeasily ta bort alla hello resurser som etablerades när du skapade hello förkonfigurerade lösningen.

> [!NOTE]
> tooensure att du tar bort allt relaterade toohello förkonfigurerade lösningen kan ta bort den på hello [azureiotsuite.com] [ lnk-azureiotsuite] plats. Ta inte bort hello resursgrupp i hello-portalen.

## <a name="next-steps"></a>Nästa steg

Nu när du har distribuerat en fungerande förkonfigurerade lösning kan fortsätta du komma igång med IoT Suite genom att läsa hello följande artiklar:

* [Genomgång av den förkonfigurerade lösningen Ansluten fabrik][lnk-rm-walkthrough]
* [Ansluta enheten toohello anslutna factory förkonfigurerade lösningen][lnk-connect-cf]
* [Behörigheter för hello azureiotsuite.com plats][lnk-permissions]

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md