---
title: hantering av aaaDevice med Azure IoT Hub | Microsoft Docs
description: "Översikt över enhetshantering i Azure IoT Hub: livscykel för företagsenhet och enhetshanteringsmönster som omstart, fabriksåterställning, uppdatering av inbyggd programvara, konfiguration, enhetstvillingar, frågor, jobb."
services: iot-hub
documentationcenter: 
author: bzurcher
manager: timlt
editor: 
ms.assetid: a367e715-55f6-4593-bd68-7863cbf0eb81
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: briz
ms.openlocfilehash: 7e22fb6eb3c541a513b16a047c7c3ef557255532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-device-management-with-iot-hub"></a>Översikt över enhetshantering med IoT Hub
## <a name="introduction"></a>Introduktion
Azure IoT-hubb ger hello funktioner och en utökningsbarhetsmodell som gör att enheten och lösningar för backend-utvecklare toobuild robust enhetshantering. Enheter mellan begränsad sensorer och ett syfte mikrostyrenheter, toopowerful gateways som dirigera kommunikation för grupper av enheter.  Dessutom variera hello användningsfall och krav för IoT-operatorer avsevärt mellan branscher.  Trots denna variation ger enhetshantering med IoT-hubb hello funktioner, mönster och koden bibliotek toocater tooa mängd olika enheter och användare.

En viktig del av att skapa en lyckad enterprise IoT-lösningen är tooprovide en strategi för hur operatorer hantera hello löpande hanteringen av sina samling av enheter. IoT-operatorer kräver enkel och tillförlitlig verktyg och program som gör dem toofocus på hello mer strategiska aspekter av arbetet. Den här artikeln innehåller:

* En kort översikt över Azure IoT Hub metoden toodevice management.
* En beskrivning av gemensamma principer för hantering av enheten.
* En beskrivning av hello enhetslivscykeln.
* En översikt över vanliga mönster för hantering av enheten.

## <a name="device-management-principles"></a>Principer för enhetshantering
IoT tar med en unik uppsättning utmaningar för hantering av enheten och varje lösning i företagsklass måste uppfylla hello följande principer:

![Bild över principer för enhetshantering][img-dm_principles]

* **Skala och automatisering**: IoT-lösningar kräver enkla verktyg som kan automatisera rutinåtgärder och aktivera en relativt liten operations personal toomanage miljoner enheter. Dagliga, operatörer räknar toohandle enheten åtgärder via fjärranslutning, i grupp, och tooonly aviseras när problem uppstår som kräver deras uppmärksamhet som direkt.
* **Kompatibilitet och insyn**: hello enheten ekosystemet är ovanligt olika. Hanteringsverktyg måste vara skräddarsydda tooaccommodate en mängd olika enhetsklasser, plattformar och protokoll. Operatorer måste kunna toosupport många typer av enheter från hello mest begränsad inbäddad enkel process kretsar, toopowerful och fullständigt funktionella datorer.
* **Kontextmedvetenhet**: IoT-miljöer är dynamiska och föränderliga. Tjänstens tillförlitlighet är avgörande. Enheten hanteringsåtgärder måste ske med kontot hello följande faktorer tooensure detta Underhåll driftavbrott inte påverkar affärskritiska åtgärder eller skapa farliga villkor:
    * SLA-underhållsfönster
    * Nätverks- och krafttillstånd
    * Användningsförutsättningar
    * Enhetens geografiska plats
* **Tjänsten många roller**: stöd för hello unika arbetsflöden och processer i IoT operations-roller är avgörande. hello driftspersonal måste arbeta intresse(4) med hello anges begränsningarna för interna IT-avdelningar.  De måste också hitta hållbarhet toosurface realtid enheten operations information toosupervisors och andra ledning affärsroller.

## <a name="device-lifecycle"></a>Enhetens livscykel
Det finns en uppsättning av allmänna enhets management faser som är vanliga tooall enterprise IoT-projekt. Det finns fem faser i hello enhetslivscykeln i Azure IoT:

![Hej fem Azure IoT-enhet livscykel faser: planera, konfigurera, konfigurera, övervaka, dra tillbaka][img-device_lifecycle]

Var och en av dessa fem faser består av flera enheter operatorn krav som måste vara uppfyllda tooprovide en komplett lösning:

* **Planera**: aktivera operatörer toocreate ett schema för enheten metadata som gör dem tooeasily och korrekt fråga för och en grupp av enheter för bulk-hanteringsåtgärder som mål. Du kan använda hello enheten dubbla toostore den här enhetens metadata i hello form av taggar och egenskaper.
  
    *Ytterligare läsning*: [Kom igång med enheten twins][lnk-twins-getstarted], [förstå enheten twins][lnk-twins-devguide], [hur toouse enhetsegenskaper dubbla][lnk-twin-properties].
* **Etablera**: säkert etablera nya enheter tooIoT NAV- och aktivera operatörer tooimmediately identifiera enhetsfunktioner.  Använda hello IoT-hubb identitet registret toocreate flexibel enhet identiteter och autentiseringsuppgifter och utföra den här åtgärden i grupp med hjälp av ett jobb. Skapa enheter tooreport deras funktioner och via enhetsegenskaper i hello enheten dubbla.
  
    *Ytterligare läsning*: [Hantera identiteter för enheten][lnk-identity-registry], [Massredigera hantering av enheten identiteter][lnk-bulk-identity], [Hur toouse enheten dubbla egenskaper][lnk-twin-properties].
* **Konfigurera**: underlätta massinläsning konfigurationsändringar och inbyggd programvara uppdaterar toodevices samtidigt både hälsotillstånd och säkerhet. Utför dessa åtgärder för enhetshantering gruppvis genom att använda önskade egenskaper eller med direkta metoder och sändningsjobb.
  
    *Ytterligare läsning*: [metoder från direkt][lnk-c2d-methods], [anropa en metod som är direkt på en enhet][lnk-methods-devguide], [hur toouse enhetsegenskaper dubbla][lnk-twin-properties], [schema och broadcast jobb][lnk-jobs], [schemalägga jobb på flera enheter] [lnk-jobs-devguide].
* **Övervakaren**: övervaka hälsa för insamling av övergripande enhet, hello status för pågående åtgärder och avisering operatörer tooissues som kanske kräver deras uppmärksamhet.  Använda hello enheten dubbla tooallow enheter tooreport realtid driftsförhållanden och status för uppdateringsåtgärder. Skapa kraftfulla instrumentpanelen rapporter som mest omedelbar ytan hello-problem genom att använda enheten dubbla frågor.
  
    *Ytterligare läsning*: [hur toouse enheten dubbla egenskaper][lnk-twin-properties], [IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning] [ lnk-query-language].
* **Dra tillbaka**: ersätta eller inaktivera enheter efter ett fel, uppgradera livscykel, eller hello slutet av hello service livslängd.  Använd hello dubbla toomaintain enheten enhetsinformation om hello fysiska enheten håller på att ersättas eller arkiverade om som har återkallats. Använd hello identitetsregistret för IoT-hubb för säkert återkalla enheten identiteter och autentiseringsuppgifter.
  
    *Ytterligare läsning*: [hur toouse enheten dubbla egenskaper][lnk-twin-properties], [Hantera identiteter för enheten][lnk-identity-registry].

## <a name="device-management-patterns"></a>Enhetshanteringsmönster
IoT-hubb kan hello följande uppsättning mönster för hantering av enheten.  Hej [device management självstudier] [ lnk-get-started] visar i detalj hur tooextend dessa mönster toofit exakt scenariot och hur toodesign nya mönster baserat på dessa viktiga mallar.

* **Starta om** -hello backend-app informerar hello enheten via en direkta metoden att den har initierat en omstart.  hello enheten använder hello rapporterade egenskaper tooupdate hello omstart hello enhets status.
  
    ![Bild över omstartsmönster för enhetshantering][img-reboot_pattern]
* **Fabriksåterställning** -hello backend-app informerar hello enheten via en direkt metod har påbörjats fabriksåterställning krävs.  hello enheten använder hello rapporterade egenskaper tooupdate hello fabriksåterställa hello enhets status.
  
    ![Bilder över fabriksåterställningsmönster för enhetshantering][img-facreset_pattern]
* **Konfigurationen** -hello backend-app använder hello önskade egenskaper tooconfigure programvara körs på hello enhet.  hello enheten använder hello rapporterade egenskaper tooupdate Konfigurationsstatus för hello enhet.
  
    ![Bild över konfigurationsmönster för enhetshantering][img-config_pattern]
* **Firmware-uppdatering** -hello backend-app informerar hello enheten via en direkta metoden att den har initierat en firmware-uppdatering.  hello enheten inleder en flerstegsprocessen toodownload hello firmware-avbildning, tillämpa hello firmware-avbildning och slutligen återansluta toohello IoT-hubb-tjänsten.  I hela flerstegsprocessen för hello rapporterade hello enheten använder hello egenskaper tooupdate hello förlopp och status för hello enhet.
  
    ![Bild över mönster för uppdatering av inbyggd programvara för enhetshantering][img-fwupdate_pattern]
* **Rapportering av förlopp och status** -hello lösningens serverdel körs enheten dubbla frågor över en uppsättning enheter, tooreport på hello status och förlopp för åtgärder som körs på hello-enheter.
  
    ![Bild över mönster för uppdatering av rapporteringsprocess och status][img-report_progress_pattern]

## <a name="next-steps"></a>Nästa steg
hello-funktioner, mönster och koden bibliotek med IoT-hubb för hantering av enheter, aktivera toocreate IoT-appar som uppfyller företagskraven IoT operator i varje steg i livscykeln enhet.

Lär dig mer om hello enhetshanteringsfunktioner i IoT-hubb toocontinue finns hello [Kom igång med enhetshantering] [ lnk-get-started] kursen.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-node-node-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md
