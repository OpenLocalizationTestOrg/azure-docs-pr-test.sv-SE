---
title: aaaCustomize Azure IoT Suite anslutna factory | Microsoft Docs
description: "En beskrivning av hur toocustomize hello funktionssätt hello anslutna factory förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 53f2fef7a76b5d8e6ad023945a7812dc7fabd12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-how-hello-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Anpassa hur hello anslutna fabriksinställningarna lösning visar data från dina OPC UA-servrar

## <a name="introduction"></a>Introduktion

hello anslutna factory lösning sammanställer och visar data från hello OPC UA servrar anslutna toohello lösning. Du kan bläddra och skicka kommandon toohello OPC UA servrar i din lösning. Mer information om OPC UA finns hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md).

Exempel på sammanställda data i hello lösningen är hello övergripande utrustning effektivitet (OEE) och nyckeln (nyckeltal) som du kan visa i hello instrumentpanelen på hello fabriken, rad och station nivåer. hello följande skärmbild visar hello OEE och KPI-värden för hello **sammansättningen** station på **produktion rad 1**, i hello **München** fabriken:

![Exempel på OEE och KPI-värden i hello-lösning][img-oee-kpi]

hello lösning aktiverar du tooview detaljerad information från specifika dataobjekt från hello OPC UA servrar, kallade *stationer*. hello visar följande skärmbild områden hello antalet tillverkade artiklar från en specifik station:

![Områden av antal tillverkade artiklar][img-manufactured-items]

Om du klickar på en av hello diagram, kan du utforska hello data ytterligare med tiden serien insikter (TSD):

![Utforska data med hjälp av tid serien insikter][img-tsi]

Den här artikeln beskrivs:

- Hur görs hello data tillgängliga toohello olika vyer i hello lösning.
- Hur du kan anpassa hello sätt hello lösning visar hello data.

## <a name="data-sources"></a>Datakällor

Hej anslutna fabriksinställningarna lösning visar data från hello OPC UA anslutna servrar toohello lösning. hello standardinstallationen omfattar flera OPC UA-servrar som kör en fabrik simulering. Du kan lägga till egna OPC UA-servrar som [ansluta via en gateway] [ lnk-connect-cf] tooyour lösning.

Du kan bläddra hello dataobjekt att en anslutna OPC UA-servern kan skicka tooyour lösning i hello instrumentpanelen:

1. Navigera toohello **väljer du en OPC UA server** vy:

    ![Navigera toohello Välj vyn OPC UA server][img-select-server]

1. Välj en server och klicka på **Anslut**. Klicka på **Fortsätt** när hello säkerhetsvarning visas.

    > [!NOTE]
    > Den här varningen visas en gång för varje server endast och upprättar en förtroenderelation mellan hello lösning instrumentpanelen och hello-servern.

1. Nu kan du bläddra hello dataobjekt som hello servern kan skicka toohello lösning. Objekt som skickas toohello lösning har en grön bock:

    ![Publicerade objekt][img-published]

1. Om du är en *administratör* i hello-lösning kan du välja toopublish toomake en data-objekt den tillgängliga i hello ansluten factory lösning. Som administratör kan du också ändra hello värdet på dataobjekt och anropa metoder i hello OPC UA server.

## <a name="map-hello-data"></a>Mappa hello data

hello anslutna factory lösning maps och mängder hello publicerade dataobjekt från hello OPC UA server toohello olika vyer i hello-lösning. hello distribuerar anslutna factory lösning tooyour Azure-konto när du etablerar hello lösning. Mappningsinformationen lagras i en JSON-fil i hello Visual Studio anslutna factory-lösning. Du kan visa och ändra den här JSON-konfigurationsfil i hello anslutna factory Visual Studio-lösning. Du kan distribuera hello lösning när du har gjort en ändring.

Du kan använda hello-konfigurationsfil:

- Redigera hello befintliga simulerade fabriker och produktion rader stationer.
- Mappa data från verkliga OPC UA-servrar som du ansluter toohello lösning.

tooclone en kopia av hello ansluten factory Visual Studio-lösning, Använd hello följande git-kommando:

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

hello filen **ContosoTopologyDescription.json** definierar hello mappning från hello OPC UA serverdata objekt toohello vyer i hello anslutna factory lösning instrumentpanelen. Du hittar den här konfigurationsfilen i hello **Contoso\Topology** mapp i hello **WebApp** projekt i hello Visual Studio-lösning.

hello innehållet hello JSON-filen har ordnats som en hierarki av fabriken, produktionen och station noder. Den här hierarkin definierar hello Navigeringshierarki i hello anslutna factory instrumentpanelen. Värden på varje nod i hierarkin hello avgör hello informationen som visas i hello instrumentpanelen. Hello JSON-fil innehåller till exempel följande värden för hello München factory hello:

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

hello namn, beskrivning och plats visas på den här vyn i instrumentpanelen för hello:

![München data i hello instrumentpanelen][img-munich]

Varje fabriken, produktionen och station har ett image-egenskapen. Du hittar dessa JPEG-filer i hello **Content\img** mapp i hello **WebApp** projekt. Dessa bildfiler visas hello anslutna factory instrumentpanelen.

Varje station innehåller flera detaljerade egenskaper som definierar hello mappning från hello OPC UA dataobjekt. De här egenskaperna beskrivs i följande avsnitt hello:

### <a name="opcuri"></a>OpcUri

Hej **OpcUri** värdet är hello OPC UA programmet URI som unikt identifierar hello OPC UA server. Till exempel hello **OpcUri** värde hello sammansättningen station på produktion rad 1 i München ser ut som följande hello: **urn: scada2194:ua:munich:productionline0:assemblystation**.

Du kan visa hello URI: er hello anslutna OPC UA servrar i instrumentpanelen för hello-lösningen:

![Visa OPC UA server URI: er][img-server-uris]

### <a name="simulation"></a>Simulering

Hej informationen i hello **simuleringen** noden är särskilda toohello OPC UA simuleringen som körs i hello OPC UA servrar som tillhandahålls som standard. Den används inte för en verklig OPC UA-server.

### <a name="kpi1-and-kpi2"></a>Kpi1 och Kpi2

Dessa noder beskrivs hur data från hello station bidrar toohello två KPI-värden i hello instrumentpanelen. Dessa KPI-värden stöds i en standarddistribution, enheter per timme och kWh per timme. hello lösning beräknas KPI vales på en station hello nivå och sammanställer dem på fabriken nivå och hello produktionsrad.

Varje KPI har en lägsta, högsta och målvärdet. Varje KPI-värdet kan också definiera aviseringsåtgärder för hello anslutna factory lösning tooperform. hello följande utdrag visar hello KPI definitioner för hello sammansättningen station produktion rad 1 i München:

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

hello följande skärmbild visar hello KPI data i hello instrumentpanelen.

![KPI-information i hello-instrumentpanelen][lnk-kpi]

### <a name="opcnodes"></a>OpcNodes

Hej **OpcNodes** noder identifiera hello publicerade dataobjekt från hello OPC UA server och ange hur tooprocess data.

Hej **NodeId** värdet identifierar hello specifika OPC UA NodeID från hello OPC UA server. hello första noden i hello sammansättningen station för produktion rad 1 i München har värdet **ns = 2, i = 385**. En **NodeId** värdet anger hello data objektet tooread från hello OPC UA server och hello **SymbolicName** innehåller ett användarvänligt namn toouse hello instrumentpanelen för dessa data.

Andra värden som är kopplade till varje nod sammanfattas i följande tabell hello:

| Värde | Beskrivning |
| ----- | ----------- |
| Relevans  | hello KPI och OEE värden informationen bidrar till. |
| OpCode     | Hur hello informationen sammanställs. |
| Enheter      | hello enheter toouse hello instrumentpanelen.  |
| Synliga    | Om toodisplay detta värde i hello instrumentpanelen. Vissa värden beräkningar men visas inte.  |
| Maximalt    | hello maximalt värde som utlöser en avisering i hello instrumentpanelen. |
| MaximumAlertActions | En åtgärd tootake i svaret tooan avisering. Till exempel skicka en kommandot tooa station. |
| ConstValue | Ett konstantvärde som används i en beräkning. |

## <a name="deploy-hello-changes"></a>Distribuera hello ändringar

När du är klar med ändringarna toohello **ContosoTopologyDescription.json** -fil, måste du distribuera hello anslutna factory lösning tooyour Azure-konto.

Hej **azure iot-ansluten-fabrik** databasen innehåller en **build.ps1** PowerShell-skript som du kan använda toorebuild och distribuera hello lösning.

## <a name="next-steps"></a>Nästa steg

Läs mer om hello anslutna factory förkonfigurerade lösningen genom att läsa hello följande artiklar:

* [Genomgång av den förkonfigurerade lösningen Ansluten fabrik][lnk-rm-walkthrough]
* [Distribuera en gateway för anslutna factory][lnk-connect-cf]
* [Behörigheter för hello azureiotsuite.com plats][lnk-permissions]
* [Vanliga frågor och svar om ansluten fabrik](iot-suite-faq-cf.md)
* [VANLIGA FRÅGOR OCH SVAR][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md