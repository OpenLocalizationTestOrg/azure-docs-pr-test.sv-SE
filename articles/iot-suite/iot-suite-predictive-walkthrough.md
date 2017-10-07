---
title: "aaaPredictive Underhåll genomgången | Microsoft Docs"
description: "En genomgång av hello Azure IoT förutsägande Underhåll förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 900d6351019489a8e2f4b98908364e3bd14975c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Genomgång av den förkonfigurerade lösningen för förebyggande underhåll

hello förutsägande Underhåll förkonfigurerade lösning som är en slutpunkt till slutpunkt för ett scenario för företag som beräknar hello punkt då ett fel är sannolikt toooccur. Du kan använda denna förkonfigurerade lösning proaktivt för aktiviteter, till exempel för att optimera underhåll. hello lösningen kombinerar viktiga Azure IoT Suite-tjänster, till exempel IoT Hub, Stream analytics och en [Azure Machine Learning] [ lnk-machine-learning] arbetsytan. Den här arbetsytan innehåller en modell, baserat på en offentlig exempel datauppsättning toopredict hello återstående livslängd (RUL) ett flygplan-motorn. hello lösningen fullständigt implementerar hello affärsscenario för IoT som en startpunkt för du tooplan och implementera en lösning som uppfyller dina egna specifika affärsbehov.

## <a name="logical-architecture"></a>Logisk arkitektur

följande diagram hello beskrivs hello logiska komponenter av hello förkonfigurerade lösningen:

![][img-architecture]

hello blå objekt är etablerad på hello region där du har distribuerat hello förkonfigurerade lösningen Azure-tjänster. hello listan över regioner där du kan distribuera hello förkonfigurerade lösningen visar på hello [etablering sidan][lnk-azureiotsuite].

hello grön objektet är en simulerad enhet som representerar ett flygplan-motorn. Mer information om enheterna simulerade i hello efter avsnittet.

hello grå representerar objekten komponenter som implementerar *enhetshantering* funktioner. hello aktuella versionen av hello förutsägande Underhåll förkonfigurerade lösningen inte att etablera dessa resurser. toolearn mer om hantering av enheter, se toohello [remote förkonfigurerade övervakningslösning][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simulerade enheter

I hello förkonfigurerade lösningen representerar en simulerad enhet ett flygplan-motorn. hello lösning etableras med två motorer som mappar tooa enda flygplan. Varje motor avger fyra typer av telemetri: Sensor 9, Sensor 11, Sensor 14 och 15 Sensor ger hello uppgifter som behövs för hello Machine Learning modellen toocalculate hello RUL hello-motorn. Varje simulerade enheten skickar hello följande telemetri meddelanden tooIoT hubb:

*Antal cykler*. En cykel representerar en slutförd flygning med varierande längd på mellan 2–10 timmar. Under hello svarta avbildas telemetridata varje halvtimme.

*Telemetri*. Det finns fyra sensorer som representerar motorattribut. hello sensorer är allmänt märkta Sensor 9, Sensor 11, Sensor 14 och 15 Sensor. Dessa fyra sensorer representerar telemetri tillräcklig tooobtain användbara resultat från hello RUL modell. hello-modell som används i hello förkonfigurerade lösningen har skapats från en offentlig datauppsättning som innehåller verkliga motorn sensordata. Mer information om hur hello modellen skapades från hello ursprungliga data finns hello [Cortana Intelligence Gallery förutsägande Underhåll mallen][lnk-cortana-analytics].

hello simulerade enheter kan hantera hello följande kommandon som skickas från hello IoT-hubb i hello lösningen:

| Kommando | Beskrivning |
| --- | --- |
| StartTelemetry |Kontroller hello tillståndet för hello simuleringen.<br/>Startar hello enhet som skickar telemetri |
| StopTelemetry |Kontroller hello tillståndet för hello simuleringen.<br/>Stoppar hello enhet som skickar telemetri |

IoT Hub bekräftar enhetskommandona.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics-jobb

**Jobbet: Telemetri** fungerar på hello inkommande enheten telemetri dataström med hjälp av två instruktioner:

* hello först väljer all telemetri från hello enheter och skickar den här tooblob datalagring. Här är den visualiseras i hello webbapp.
* hello andra beräknar genomsnittligt sensor värden över ett skjutfönster i två minuter och skickar dem via hello Event hub tooan **Händelseprocessorn**.

## <a name="event-processor"></a>Händelseprocessor
Hej **värd för händelsebearbetning** körs i ett Azure Web-jobb. Hej **Händelseprocessorn** tar hello genomsnittlig sensor värden för en slutförd cykel. Den skickar sedan dessa värden tooan API som exponerar tränade modellen toocalculate hello RUL för en motor. hello API exponeras av Machine Learning-arbetsytan som har etablerats som en del av hello lösning.

## <a name="machine-learning"></a>Machine Learning
hello Machine Learning komponenten använder en modell som härletts från data som samlas in från verkliga flygplan motorer. Du kan navigera toohello Machine Learning-arbetsytan från hello-panelen på hello [azureiotsuite.com] [ lnk-azureiotsuite] sidan för etablerade lösningen. hello rutan är tillgänglig när hello lösning hello **klar** tillstånd.


## <a name="next-steps"></a>Nästa steg
Nu du har sett hello viktiga komponenter av hello förutsägande Underhåll förkonfigurerade lösningen du toocustomize den. Se [Vägledning för anpassning av förkonfigurerade lösningar][lnk-customize].

Du kan även utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:

* [Vanliga frågor och svar om IoT Suite][lnk-faq]
* [IoT-säkerhet från hello bakgrund][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/