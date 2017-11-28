---
title: 'SensorTag enhet & Azure IoT-Gateway - Lektion 4: Table storage | Microsoft Docs'
description: "Spara meddelanden från Intel NUC tooyour IoT-hubb, skrivs tooAzure Table storage och läses sedan från hello molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Hämta data från molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 8ca78045-ad92-4a6a-90f1-05f9668e6f0e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 29525b084eb4d6e6dfcb16d9b34f78f075d30b7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="d3442-104">Läs meddelandena kvar i Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="d3442-104">Read messages persisted in Azure Table storage</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="d3442-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="d3442-105">What you will do</span></span>

- <span data-ttu-id="d3442-106">Kör exempelprogrammet i hello gateway på din gateway som skickar meddelanden tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d3442-106">Run hello gateway sample application on your gateway that sends messages tooyour IoT hub.</span></span>
- <span data-ttu-id="d3442-107">Kör sedan en exempelkod på din värd datorn tooread hello meddelanden i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="d3442-107">Then run a sample code on your host computer tooread hello messages in your Azure Table storage.</span></span> 

<span data-ttu-id="d3442-108">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="d3442-108">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="d3442-109">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="d3442-109">What you will learn</span></span>

<span data-ttu-id="d3442-110">Hur gulp toouse hello verktyget toorun exempel kod tooread hälsningsmeddelande i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="d3442-110">How toouse hello gulp tool toorun hello sample code tooread messages in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="d3442-111">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="d3442-111">What you need</span></span>

<span data-ttu-id="d3442-112">Du har nu hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="d3442-112">You have have successfully done hello following tasks:</span></span>

- <span data-ttu-id="d3442-113">[Skapat hello Azure funktionsapp och hello Azure storage-konto](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="d3442-113">[Created hello Azure function app and hello Azure storage account](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md).</span></span>
- <span data-ttu-id="d3442-114">[Kör exempelprogrammet i hello gateway](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md).</span><span class="sxs-lookup"><span data-stu-id="d3442-114">[Run hello gateway sample application](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md).</span></span>
- <span data-ttu-id="d3442-115">[Läsa meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md).</span><span class="sxs-lookup"><span data-stu-id="d3442-115">[Read messages from your IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md).</span></span>

## <a name="get-your-azure-storage-connection-strings"></a><span data-ttu-id="d3442-116">Hämta ditt Azure storage-anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="d3442-116">Get your Azure storage connection strings</span></span>

<span data-ttu-id="d3442-117">Tidigt i kursen skapat du ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d3442-117">Early in this lesson, you successfully created an Azure storage account.</span></span> <span data-ttu-id="d3442-118">tooget hello anslutningssträngen för hello Azure storage-konto som kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="d3442-118">tooget hello connection string of hello Azure storage account, run hello following commands:</span></span>

* <span data-ttu-id="d3442-119">Visa en lista med alla lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="d3442-119">List all your storage accounts.</span></span>

```bash
az storage account list -g iot-gateway --query [].name
```

* <span data-ttu-id="d3442-120">Hämta anslutningssträngen för azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="d3442-120">Get azure storage connection string.</span></span>

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

<span data-ttu-id="d3442-121">Använd iot-gateway som hello värde för `{resource group name}` om du inte ändrar hello värdet i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="d3442-121">Use iot-gateway as hello value of `{resource group name}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="d3442-122">Konfigurera hello enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="d3442-122">Configure hello device connection</span></span>

<span data-ttu-id="d3442-123">Uppdatera hello `config-azure.json` filen så att hello exempelkod som körs på värddatorn för hello kan läsa meddelandet i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="d3442-123">Update hello `config-azure.json` file so that hello sample code that runs on hello host computer can read message in your Azure Table storage.</span></span> <span data-ttu-id="d3442-124">tooconfigure Hej enhetsanslutning, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="d3442-124">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="d3442-125">Öppna hello enheten konfigurationsfilen `config-azure.json` genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="d3442-125">Open hello device configuration file `config-azure.json` by running hello following commands:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfiguration](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. <span data-ttu-id="d3442-127">Ersätt `[Azure storage connection string]` med hello Azure storage-anslutningssträngen som du fick.</span><span class="sxs-lookup"><span data-stu-id="d3442-127">Replace `[Azure storage connection string]` with hello Azure storage connection string that you obtained.</span></span>

   <span data-ttu-id="d3442-128">`[IoT hub connection string]`redan ska ersättas i avsnittet [läsa meddelanden från Azure IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md) i lektion3.</span><span class="sxs-lookup"><span data-stu-id="d3442-128">`[IoT hub connection string]` should already be replaced in section [Read messages from Azure IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md) in Lesson3.</span></span>

## <a name="read-messages-in-your-azure-table-storage"></a><span data-ttu-id="d3442-129">Läsa meddelanden i Azure-tabellagring</span><span class="sxs-lookup"><span data-stu-id="d3442-129">Read messages in your Azure Table storage</span></span>

<span data-ttu-id="d3442-130">Kör exempelprogrammet i hello gateway och läsa Azure Table storage meddelanden hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d3442-130">Run hello gateway sample application and read Azure Table storage messages by hello following command:</span></span>

```bash
gulp run --table-storage
```

<span data-ttu-id="d3442-131">Din IoT-hubb startar din Azure-funktion toosave-meddelande till din Azure Table storage när nytt meddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="d3442-131">Your IoT hub triggers your Azure Function application toosave message into your Azure Table storage when new message arrives.</span></span>
<span data-ttu-id="d3442-132">Hej `gulp run` körs gateway exempelprogram som skickar meddelanden tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d3442-132">hello `gulp run` command runs gateway sample application that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="d3442-133">Med `table-storage` parameter, den också skapas en underordnad process tooreceive hello sparas meddelandet i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="d3442-133">With `table-storage` parameter, it also spawns a child process tooreceive hello saved message in your Azure Table storage.</span></span>

<span data-ttu-id="d3442-134">hello-meddelanden som skickas och tas emot är alla visas direkt på hello samma konsolfönstret i hello värddatorn.</span><span class="sxs-lookup"><span data-stu-id="d3442-134">hello messages that are being sent and received are all displayed instantly on hello same console window in hello host machine.</span></span> <span data-ttu-id="d3442-135">hello exempel programinstansen avslutas automatiskt i 40 sekunder.</span><span class="sxs-lookup"><span data-stu-id="d3442-135">hello sample application instance will terminate automatically in 40 seconds.</span></span>

   ![gulp Läs](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table.png)


## <a name="summary"></a><span data-ttu-id="d3442-137">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d3442-137">Summary</span></span>

<span data-ttu-id="d3442-138">Du har kört hello exempel tooread hello meddelanden i din Azure Table storage sparas av ditt program för Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="d3442-138">You've run hello sample code tooread hello messages in your Azure Table storage saved by your Azure Function application.</span></span>
