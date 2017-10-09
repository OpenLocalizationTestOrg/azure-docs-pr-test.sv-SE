---
title: "aaaIoT säkerhetsmetoder | Microsoft Docs"
description: "Rekommenderade säkerhetsmetoder för att skydda din IoT-infrastruktur"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 24e7bda2-5f7b-44e3-b8af-761abd3276ff
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: yurid
ms.openlocfilehash: 3a546c67978a519446ab6c83917a0789675f1b52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-best-practices"></a>Sakernas Internet säkerhetsmetoder
toosecure en Sakernas Internet (IoT)-infrastruktur kräver en rigorösa security-strategi. Den här strategin kräver toosecure data i molnet hello kan skydda dataintegriteten som överförs över hello offentliga internet och etablera enheter på ett säkert sätt. Varje lager bygger större säkerhet säkerhet i hello hela infrastrukturen.

## <a name="secure-an-iot-infrastructure"></a>Skydda en IoT-infrastruktur
Den här strategin för säkerhet på djupet kan utvecklas och utförs med aktiv medverkan av olika aktörer med hello tillverkning, utveckling och distribution av IoT-enheter och infrastrukturen. Nedan följer en beskrivning av dessa spelare.  

* **IoT maskinvara tillverkare/integrator**: dessa normalt hello tillverkare av IoT-maskinvara som distribueras i dietetisk montering maskinvara från olika tillverkare och leverantörer som tillhandahåller maskinvara för en IoT-distribution som har tillverkats eller integrerad med andra leverantörer.
* **IoT-lösningen developer**: hello utvecklingen av en IoT-lösning bör göras av en för lösningsutvecklare. Den här utvecklare kan en del av en interna team eller en systemintegreraren (SI) som har specialiserat sig på den här aktiviteten. Hej IoT-lösningen utvecklare kan utveckla olika komponenter i hello IoT-lösningen från början, integrera olika standardprogram eller öppen källkod komponenter eller anta förkonfigurerade lösningar med mindre anpassning.
* **IoT-lösningen deployer**: efter IoT-lösningen har utvecklats, måste toobe som distribuerats i hello-fältet. Detta innefattar distribution av maskinvara, sammankoppling av enheter och distribution av lösningar i maskinvaruenheter eller hello molnet.
* **IoT-lösningen operatorn**: efter hello IoT-lösningen har distribuerats, kräver långsiktig driften, övervakningen, uppgraderingar och underhåll. Detta kan göras av en intern team som omfattar information technology specialister, maskinvara åtgärder och underhåll team och domän specialister som övervakar hello korrekt beteende av övergripande IoT-infrastruktur.

hello avsnitten som följer ger bästa praxis för var och en av dessa spelare toohelp utveckla, distribuera och driva en säker IoT-infrastruktur.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT maskinvara tillverkare/integrator
hello följande är hello bästa praxis för IoT maskinvarutillverkare och maskinvara dietetisk.

* **Omfång toominimum maskinvarukrav**: hello maskinvara design ska innehålla minsta hello funktioner som krävs för driften av hello maskinvara och inget mer. Ett exempel är tooinclude USB-portar endast om det behövs för hello hello enheten ska fungera. Dessa ytterligare funktioner öppna hello enhet för oönskade angreppsmetoder som bör undvikas.
* **Kontrollera maskinvara säkerhetsförsluten bevis**: skapa i mekanismer toodetect fysiska manipulering, till exempel öppning av luckan till hello eller ta bort en del av hello enhet. Dessa säkerhetsförsluten signaler kan vara en del av hello data dataströmmen har överförts toohello moln, vilket kan varna operatorerna för dessa händelser.
* **Skapa runt säker maskinvara**: om kostnad för sålda varor tillåter, skapa säkerhetsfunktioner, till exempel säker och krypterad lagring eller starta funktioner som bygger på Trusted Platform Module (TPM). Dessa funktioner göra enheter mer säkra och skydda hello övergripande IoT-infrastruktur.
* **Göra uppgraderingar säker**: Firmware-uppgraderingar under hello livstid hello enhet är ofrånkomligt. Skapa enheter med säker sökvägar för uppgraderingar och kryptografiska försäkran för versioner av inbyggd tillåter hello enheten toobe säker under och efter uppgraderingar.

## <a name="iot-solution-developer"></a>IoT-lösningen utvecklare
hello följande är hello bästa praxis för utvecklare av IoT-lösningar:

* **Följ säker software development metoder**: utveckling av säker programvara kräver grunden som du tänker på säkerheten från hello starten av hello projekt alla hello sätt tooits implementering, testning och distribution. hello val av plattformar och språk verktyg påverkas med denna metod. hello Microsoft Security Development Lifecycle innehåller en stegvis toobuilding säker programvara.
* **Välj med öppen källkod med försiktighet**: med öppen källkod ger en möjlighet tooquickly utveckla lösningar. När du väljer med öppen källkod, Överväg hello aktivitetsnivån i hello community för varje komponent för öppen källkod. En aktiv community garanterar att programvaran stöds och att problem identifieras och hanteras. Du kan också en otydligt och inaktiva med öppen källkod stöds inte och problem kommer troligen inte identifieras.
* **Integrera med försiktighet**: många programvara säkerhetsbrister finns på hello gränsen för bibliotek och API: er. Funktioner som inte kanske behövs för hello nuvarande distributionen kan fortfarande vara tillgänglig via ett API-lager. tooensure övergripande säkerheten och se till att toocheck alla komponenter integreras för säkerhetsbrister-gränssnitt.      

## <a name="iot-solution-deployer"></a>Distribueraren för IoT-lösning
hello följande är bästa praxis för IoT-lösningen distributörer:

* **Distribuera maskinvara på ett säkert sätt**: IoT-distributioner kan kräva maskinvara toobe distribuerats i osäkra platser, exempelvis offentliga blanksteg eller oövervakade språk. Kontrollera att maskinvaran distribution är förfalska toohello största utsträckning i sådana situationer. Om USB- eller andra portar finns på hello maskinvara, se till att de omfattas på ett säkert sätt. Många angreppsmetoder kan använda dessa som startpunkter.
* **Skydda autentiseringsnycklar**: under distribution varje enhet kräver enhets-ID och tillhörande autentiseringsnycklar som genererats av hello-Molntjänsten. Skydda de här nycklarna fysiskt även efter hello-distribution. Alla avslöjade nyckeln kan användas av en obehörig enhet toomasquerade som en befintlig enhet.

## <a name="iot-solution-operator"></a>IoT-lösningen operator
hello följande är hello bästa praxis för ansvariga för IoT-lösningen:

* **Behåll hello system toodate**: Kontrollera att enhetens operativsystem och alla enhetsdrivrutiner är uppgraderade toohello senaste versionerna. Om du aktiverar automatiska uppdateringar i Windows 10 (IoT eller andra SKU: er), håller Microsoft den in toodate, vilket ger ett säkert operativsystem för IoT-enheter. Att hålla andra operativsystem (till exempel Linux) in toodate säkerställer att de också skyddas mot skadliga attacker.
* **Skydda mot skadlig aktivitet**: om hello operativsystemet tillåter, installera hello senaste antivirus eller antimalware funktionerna på varje enhets operativsystem. Detta kan hjälpa till att minska de flesta externa hot. Du kan skydda de flesta moderna operativsystem mot hot genom att vidta lämpliga åtgärder.
* **Granska ofta**: granskning IoT infrastruktur för säkerhetsrelaterade problem är nyckel när svarar toosecurity incidenter. De flesta operativsystem innehåller inbyggda händelseloggning som ska granskas ofta toomake att inga säkerhetsintrång har uppstått. Granskningsinformation kan skickas som en separat telemetri dataströmmen toohello molnbaserad tjänst där den kan analyseras.
* **Fysiskt skydda hello IoT infrastruktur**: hello sämsta attacker mot IoT infrastruktur startas med fysisk åtkomst toodevices. En är viktig säkerhet tooprotect mot skadlig användning av USB-portar och andra fysisk åtkomst. En nyckel toouncovering överträdelser som har inträffat loggning av fysisk åtkomst till exempel USB-port används. Windows 10 (IoT och andra SKU: er) kan igen och detaljerad loggning av dessa händelser.
* **Skydda molnet autentiseringsuppgifter**: molnet autentiseringsuppgifter som används för att konfigurera och använda en IoT-distribution är eventuellt hello enklaste sättet toogain åtkomst och IoT-dator. Skydda hello autentiseringsuppgifter genom att ändra hello lösenord ofta och avstå från att använda dessa autentiseringsuppgifter på offentliga datorer.

Funktionerna i olika IoT-enheter varierar. Vissa enheter kan vara datorer som kör vanliga skrivbord och vissa enheter med mycket inaktiverad operativsystem. hello säkerhetsmetoder beskrivs kan tidigare vara tillämpliga toothese enheter i olika grader. Om ytterligare säkerhets- och bästa praxis från hello tillverkare av dessa enheter ska följas.

Vissa äldre och begränsad enheter kan inte ha har utformats speciellt för IoT-distribution. Dessa enheter kan saknar hello kapaciteten tooencrypt data, ansluta till hello Internet, eller ange avancerade granskning. I dessa fall kan en modern och säker gateway aggregera data från äldre enheter och ge hello säkerhet som krävs för att ansluta enheterna över hello Internet. Fältet gateways kan tillhandahålla säker autentisering, förhandling krypterade sessioner, kan ta emot kommandon från hello molnet och många andra säkerhetsfunktioner.

## <a name="see-also"></a>Se även
toolearn mer om att skydda din IoT-lösningen, se:

* [IoT-säkerhetsarkitekturen][lnk-security-architecture]
* [Säkra din IoT-distribution][lnk-security-deployment]

Du kan även utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:

* [Förutsägande Underhåll förkonfigurerade lösning: översikt][lnk-predictive-overview]
* [Vanliga frågor om Azure IoT Suite][lnk-faq]

Du kan läsa om IoT-hubb säkerhet i [kontroll åtkomst tooIoT hubb] [ lnk-devguide-security] i hello utvecklarhandboken för IoT-hubb.

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md

[lnk-security-architecture]: iot-security-architecture.md
[lnk-security-deployment]: iot-suite-security-deployment.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md
