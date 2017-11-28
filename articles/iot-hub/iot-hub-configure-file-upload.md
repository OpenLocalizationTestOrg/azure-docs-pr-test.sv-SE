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
# <a name="configure-iot-hub-file-uploads-using-hello-azure-portal"></a><span data-ttu-id="b86a5-104">Konfigurera IoT-hubb filöverföringar med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b86a5-104">Configure IoT Hub file uploads using hello Azure portal</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a><span data-ttu-id="b86a5-105">Ladda upp filen</span><span class="sxs-lookup"><span data-stu-id="b86a5-105">File upload</span></span>

<span data-ttu-id="b86a5-106">toouse hello [filen överför funktioner i IoT-hubb][lnk-upload], måste du först associera ett Azure Storage-konto med hubben.</span><span class="sxs-lookup"><span data-stu-id="b86a5-106">toouse hello [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your hub.</span></span> <span data-ttu-id="b86a5-107">Välj **filuppladdning** toodisplay en lista över egenskaper för filöverföring för hello IoT-hubb som ändras.</span><span class="sxs-lookup"><span data-stu-id="b86a5-107">Select **File upload** toodisplay a list of file upload properties for hello IoT hub that is being modified.</span></span>

![Visa inställningarna i hello portal för IoT-hubb filöverföring][13]

<span data-ttu-id="b86a5-109">**Lagringsbehållaren**: Använd hello Azure portal tooselect en blob-behållare i ett Azure Storage-konto i din aktuella Azure-prenumeration tooassociate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="b86a5-109">**Storage container**: Use hello Azure portal tooselect a blob container in an Azure Storage account in your current Azure subscription tooassociate with your IoT Hub.</span></span> <span data-ttu-id="b86a5-110">Om nödvändigt, du kan skapa ett Azure Storage-konto på hello **lagringskonton** bladet och blob-behållare på hello **behållare** bladet.</span><span class="sxs-lookup"><span data-stu-id="b86a5-110">If necessary, you can create an Azure Storage account on hello **Storage accounts** blade and blob container on hello **Containers** blade.</span></span> <span data-ttu-id="b86a5-111">IoT-hubb genererar automatiskt SAS URI: er med skriva behörigheter toothis blob-behållare för toouse enheter när de överför filer.</span><span class="sxs-lookup"><span data-stu-id="b86a5-111">IoT Hub automatically generates SAS URIs with write permissions toothis blob container for devices toouse when they upload files.</span></span>

![Visa behållare för lagring för filöverföring i hello portal][14]

<span data-ttu-id="b86a5-113">**Ta emot meddelanden för överförda filer**: aktivera eller inaktivera filen överför meddelanden via hello växla.</span><span class="sxs-lookup"><span data-stu-id="b86a5-113">**Receive notifications for uploaded files**: Enable or disable file upload notifications via hello toggle.</span></span>

<span data-ttu-id="b86a5-114">**SAS TTL**: den här inställningen är hello time-to-live av hello SAS URI returneras toohello enhet av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b86a5-114">**SAS TTL**: This setting is hello time-to-live of hello SAS URIs returned toohello device by IoT Hub.</span></span> <span data-ttu-id="b86a5-115">Som standard tooone timme men kan vara anpassade tooother värden med hjälp av hello skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="b86a5-115">Set tooone hour by default but can be customized tooother values using hello slider.</span></span>

<span data-ttu-id="b86a5-116">**Filen notification inställningar Standardbegränsning**: hello time-to-live av en fil överför meddelande innan det förfaller.</span><span class="sxs-lookup"><span data-stu-id="b86a5-116">**File notification settings default TTL**: hello time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="b86a5-117">Ange tooone dagen som standard men kan vara anpassade tooother värden med hjälp av hello skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="b86a5-117">Set tooone day by default but can be customized tooother values using hello slider.</span></span>

<span data-ttu-id="b86a5-118">**Meddelande leverans av maximalt antal filer**: hello gånger hello IoT-hubb försök toodeliver ett meddelande om överföringen.</span><span class="sxs-lookup"><span data-stu-id="b86a5-118">**File notification maximum delivery count**: hello number of times hello IoT Hub attempts toodeliver a file upload notification.</span></span> <span data-ttu-id="b86a5-119">Som standard too10 men kan vara anpassade tooother värden med hjälp av hello skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="b86a5-119">Set too10 by default but can be customized tooother values using hello slider.</span></span>

![Konfigurera filuppladdning för IoT-hubb i hello-portalen][15]

## <a name="next-steps"></a><span data-ttu-id="b86a5-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b86a5-121">Next steps</span></span>

<span data-ttu-id="b86a5-122">Mer information om funktionerna i hello filen överför IoT-hubb finns [överföra filer från en enhet] [ lnk-upload] i hello utvecklarhandboken för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b86a5-122">For more information about hello file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload] in hello IoT Hub developer guide.</span></span>

<span data-ttu-id="b86a5-123">Följ dessa länkar toolearn mer om hur du hanterar Azure IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="b86a5-123">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="b86a5-124">[Massinläsning hantera IoT-enheter][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="b86a5-124">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="b86a5-125">[IoT-hubb mått][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="b86a5-125">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="b86a5-126">[Åtgärder som övervakning][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="b86a5-126">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="b86a5-127">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="b86a5-127">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="b86a5-128">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="b86a5-128">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="b86a5-129">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="b86a5-129">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="b86a5-130">[Skydda din IoT-lösning från hello bakgrund][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="b86a5-130">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

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
