---
title: ladda upp aaaUse hello Azure portal tooconfigure filen | Microsoft Docs
description: "Hur toouse hello Azure portal tooconfigure filöverföringar din IoT-hubb tooenable från anslutna enheter. Innehåller information om hur du konfigurerar hello mål Azure storage-konto."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: b90c3fbed47b4eb144d3cb7480068b7cfc776ba6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-hello-azure-portal"></a>Konfigurera IoT-hubb filöverföringar med hello Azure-portalen

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Ladda upp filen

toouse hello [filen överför funktioner i IoT-hubb][lnk-upload], måste du först associera ett Azure Storage-konto med hubben. Välj **filuppladdning** toodisplay en lista över egenskaper för filöverföring för hello IoT-hubb som ändras.

![Visa inställningarna i hello portal för IoT-hubb filöverföring][13]

**Lagringsbehållaren**: Använd hello Azure portal tooselect en blob-behållare i ett Azure Storage-konto i din aktuella Azure-prenumeration tooassociate med IoT-hubben. Om nödvändigt, du kan skapa ett Azure Storage-konto på hello **lagringskonton** bladet och blob-behållare på hello **behållare** bladet. IoT-hubb genererar automatiskt SAS URI: er med skriva behörigheter toothis blob-behållare för toouse enheter när de överför filer.

![Visa behållare för lagring för filöverföring i hello portal][14]

**Ta emot meddelanden för överförda filer**: aktivera eller inaktivera filen överför meddelanden via hello växla.

**SAS TTL**: den här inställningen är hello time-to-live av hello SAS URI returneras toohello enhet av IoT-hubb. Som standard tooone timme men kan vara anpassade tooother värden med hjälp av hello skjutreglaget.

**Filen notification inställningar Standardbegränsning**: hello time-to-live av en fil överför meddelande innan det förfaller. Ange tooone dagen som standard men kan vara anpassade tooother värden med hjälp av hello skjutreglaget.

**Meddelande leverans av maximalt antal filer**: hello gånger hello IoT-hubb försök toodeliver ett meddelande om överföringen. Som standard too10 men kan vara anpassade tooother värden med hjälp av hello skjutreglaget.

![Konfigurera filuppladdning för IoT-hubb i hello-portalen][15]

## <a name="next-steps"></a>Nästa steg

Mer information om funktionerna i hello filen överför IoT-hubb finns [överföra filer från en enhet] [ lnk-upload] i hello utvecklarhandboken för IoT-hubb.

Följ dessa länkar toolearn mer om hur du hanterar Azure IoT-hubb:

* [Massinläsning hantera IoT-enheter][lnk-bulk]
* [IoT-hubb mått][lnk-metrics]
* [Åtgärder som övervakning][lnk-monitor]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Simulera en enhet med IoT kant][lnk-iotedge]
* [Skydda din IoT-lösning från hello bakgrund][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
