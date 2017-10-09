---
title: aaaAzure IoT-hubb IP-anslutningsfilter | Microsoft Docs
description: "Hur toouse IP filtrering tooblock anslutningar från särskilda IP-adresser för tooyour Azure IoT-hubb. Du kan blockera anslutningar från enskilda eller intervall med IP-adresser."
services: iot-hub
documentationcenter: 
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: f833eac3-5b5f-46a7-a47b-f4f6fc927f3f
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: boltean
ms.openlocfilehash: 45e5906a494561b6108895d86d6a04fc3b52b8fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-ip-filters"></a>IP-filter

Säkerhet är en viktig del av alla IoT-lösning baserad på Azure IoT Hub. Ibland behöver tooexplicitly ange hello IP-adresser från vilka enheter kan anslutas som en del av din konfiguration. Hej _IP-filter_ funktionen kan du tooconfigure regler för avvisa eller ta emot trafik från specifika IPv4-adresser.

## <a name="when-toouse"></a>När toouse

Det finns två specifika användningsfall när det är användbart tooblock hello IoT-hubbslutpunkter för vissa IP-adresser:

- IoT-hubb bör endast ta emot trafik från ett angivet IP-adresser och avvisa allt annat. Till exempel du använder din IoT-hubb med [Azure Express Route] toocreate privata anslutningar mellan en IoT-hubb och lokal infrastruktur.
- Du måste tooreject trafik från IP-adresser som har identifierats som misstänkt av hello IoT hub-administratören.

## <a name="how-filter-rules-are-applied"></a>Hur filter regler

hello IP-filterregler tillämpas på hello IoT-hubb servicenivå. Hello IP-filterregler gäller därför tooall anslutningar från enheter och backend-appar som använder alla protokoll som stöds.

Alla anslutningsförsök från en IP-adress som matchar en rejecting IP-regel i din IoT-hubb tar emot en obehörig 401 statuskoden och en beskrivning. hello svarsmeddelande nämner inte hello IP-regeln.

## <a name="default-setting"></a>Standardinställningen

Som standard hello **IP-Filter** rutnät i hello portal för IoT-hubben är tom. Den här standardinställningen innebär att din hubb accepterar anslutningar IP-adresser. Den här standardinställningen är likvärdiga tooa regel som accepterar hello 0.0.0.0/0 IP-adressintervall.

![IoT-hubb standardinställningarna IP-filter][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a>Lägg till eller redigera en regel för IP-filter

När du lägger till en regel för IP-filter efterfrågas hello följande värden:

- En **Regelnamn för IP-filter** som måste vara en unik, skiftlägeskänsliga, alfanumeriska sträng in too128 tecken. Endast hello ASCII-7-bitars alfanumeriska tecken plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` accepteras.
- Välj en **avvisa** eller **acceptera** som hello **åtgärd** för hello IP-filterregeln.
- Ange en IPv4-adress eller ett block av IP-adresser i CIDR-notation. Till exempel i CIDR notation 192.168.100.0/22 representerar hello 1024 IPv4-adresser från adressen 192.168.100.0 too192.168.103.255.

![Lägg till en IP-filter regeln tooan IoT-hubb][img-ip-filter-add-rule]

När du har sparat hello regeln visas en varning som meddelar dig om att hello uppdatering pågår.

![Meddelande om att spara en regel för IP-filter][img-ip-filter-save-new-rule]

Hej **Lägg till** alternativet inaktiveras när du når hello högst 10 IP-filterregler.

Du kan redigera en befintlig regel genom att dubbelklicka på hello rad som innehåller hello regeln.

> [!NOTE]
> Neka IP adresser kan förhindra att andra Azure-tjänster (till exempel Azure Stream Analytics, virtuella datorer i Azure eller hello enheten Explorer i hello portal) interagerar med hello IoT-hubb.

> [!WARNING]
> Om du använder Azure Stream Analytics (ASA) tooread meddelanden från en IoT-hubb med IP-filtrering, Använd hello Event Hub-kompatibelt namn och slutpunkten för din IoT-hubb i hello ASA anslutningssträngen.

## <a name="delete-an-ip-filter-rule"></a>Ta bort en regel för IP-filter

toodelete en regel för IP-filter, Välj en eller flera regler i hello rutnätet och klicka på **ta bort**.

![Ta bort en regel för IoT-hubb IP-filter][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a>IP-filter regeln utvärdering

IP-filterregler tillämpas i ordning och hello första regeln att matchar hello IP-adressen bestämmer hello godkänna eller avvisa åtgärd.

Om du vill tooaccept adresser i hello intervallet 192.168.100.0/22 och avvisa allt annat ska hello första regeln i rutnätet hello acceptera hello adressintervallet 192.168.100.0/22. hello nästa regel bör Ignorera alla adresser med hjälp av hello intervallet 0.0.0.0/0.

Du kan ändra hello ordning av IP-filter reglerna i hello rutnätet genom att klicka tre lodräta punkter hello hello början av en rad med dra och släpp.

toosave din nya IP-filtrera regel ordning, klickar du på **spara**.

![Ändra din IoT-hubb IP filterregler hello ordning][img-ip-filter-rule-order]

## <a name="next-steps"></a>Nästa steg

toofurther utforska hello funktionerna i IoT Hub, se:

- [Åtgärder som övervakning][lnk-monitor]
- [IoT-hubb mått][lnk-metrics]

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure Express Route]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md