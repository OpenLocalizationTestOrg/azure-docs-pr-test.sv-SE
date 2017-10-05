---
title: 'Simulerade enhet & Azure IoT Gateway - lektionen 4: Table storage | Microsoft Docs'
description: "Spara meddelanden från Intel NUC till din IoT-hubb, skriva dem till Azure Table storage och läses sedan från molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Hämta data från molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 78e4b6ea-968d-401e-a7dc-8f9acdb3ec1a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: de5fae794c195132e2a487c0095845c756aa28e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="671b1-104">Läs meddelandena kvar i Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="671b1-104">Read messages persisted in Azure Table storage</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="671b1-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="671b1-105">What you will do</span></span>

- <span data-ttu-id="671b1-106">Kör exempelprogrammet gateway på din gateway som skickar meddelanden till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="671b1-106">Run the gateway sample application on your gateway that sends messages to your IoT hub.</span></span>
- <span data-ttu-id="671b1-107">Köra exempelkod på värddatorn för att läsa meddelanden i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="671b1-107">Run sample code on your host computer to read messages in your Azure Table storage.</span></span>

<span data-ttu-id="671b1-108">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="671b1-108">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="671b1-109">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="671b1-109">What you will learn</span></span>

<span data-ttu-id="671b1-110">Hur du använder verktyget gulp köra exempelkod för att läsa meddelanden i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="671b1-110">How to use the gulp tool to run the sample code to read messages in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="671b1-111">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="671b1-111">What you need</span></span>

<span data-ttu-id="671b1-112">Du har nu gjort följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="671b1-112">You have have successfully done the following tasks:</span></span>

- <span data-ttu-id="671b1-113">[Skapade appen Azure-funktion och Azure-lagringskontot](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="671b1-113">[Created the Azure function app and the Azure storage account](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).</span></span>
- <span data-ttu-id="671b1-114">[Kör exempelprogrammet gateway](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span><span class="sxs-lookup"><span data-stu-id="671b1-114">[Run the gateway sample application](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span></span>
- <span data-ttu-id="671b1-115">[Läsa meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).</span><span class="sxs-lookup"><span data-stu-id="671b1-115">[Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).</span></span>

## <a name="get-your-azure-storage-connection-strings"></a><span data-ttu-id="671b1-116">Hämta ditt Azure storage-anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="671b1-116">Get your Azure storage connection strings</span></span>

<span data-ttu-id="671b1-117">Tidigt i kursen skapat du ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="671b1-117">Early in this lesson, you successfully created an Azure storage account.</span></span> <span data-ttu-id="671b1-118">Kör följande kommandon för att hämta anslutningssträngen för Azure storage-konto:</span><span class="sxs-lookup"><span data-stu-id="671b1-118">To get the connection string of the Azure storage account, run the following commands:</span></span>

* <span data-ttu-id="671b1-119">Visa en lista med alla lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="671b1-119">List all your storage accounts.</span></span>

```bash
az storage account list -g iot-gateway --query [].name
```

* <span data-ttu-id="671b1-120">Hämta anslutningssträngen för azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="671b1-120">Get azure storage connection string.</span></span>

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

<span data-ttu-id="671b1-121">Använd `iot-gateway` som värde för `{resource group name}` om du inte ändra värdet i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="671b1-121">Use `iot-gateway` as the value of `{resource group name}` if you didn't change the value in Lesson 2.</span></span>

## <a name="configure-the-device-connection"></a><span data-ttu-id="671b1-122">Konfigurera enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="671b1-122">Configure the device connection</span></span>

<span data-ttu-id="671b1-123">Uppdatera den `config-azure.json` filen så att exempelkoden som körs på värddatorn kan läsa meddelandet i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="671b1-123">Update the `config-azure.json` file so that the sample code that runs on the host computer can read message in your Azure Table storage.</span></span> <span data-ttu-id="671b1-124">Följ dessa steg för att konfigurera enhetsanslutningen:</span><span class="sxs-lookup"><span data-stu-id="671b1-124">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="671b1-125">Öppna konfigurationsfilen enheten `config-azure.json` genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="671b1-125">Open the device configuration file `config-azure.json` by running the following commands:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfiguration](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. <span data-ttu-id="671b1-127">Ersätt `[Azure storage connection string]` med Azure storage-anslutningssträngen som du fick.</span><span class="sxs-lookup"><span data-stu-id="671b1-127">Replace `[Azure storage connection string]` with the Azure storage connection string that you obtained.</span></span>

   <span data-ttu-id="671b1-128">`[IoT hub connection string]`redan ska ersättas i avsnittet [läsa meddelanden från Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) i lektion3.</span><span class="sxs-lookup"><span data-stu-id="671b1-128">`[IoT hub connection string]` should already be replaced in section [Read messages from Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) in Lesson3.</span></span>

## <a name="read-messages-in-your-azure-table-storage"></a><span data-ttu-id="671b1-129">Läsa meddelanden i Azure-tabellagring</span><span class="sxs-lookup"><span data-stu-id="671b1-129">Read messages in your Azure Table storage</span></span>

<span data-ttu-id="671b1-130">Kör exempelprogrammet gateway och läsa Azure Table storage meddelanden med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="671b1-130">Run the gateway sample application and read Azure Table storage messages by the following command:</span></span>

```bash
gulp run --table-storage
```

<span data-ttu-id="671b1-131">IoT-hubb utlöser tillämpningsprogrammet Azure-funktion för att spara meddelanden i Azure Table storage när nytt meddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="671b1-131">Your IoT hub triggers your Azure Function application to save message into your Azure Table storage when new message arrives.</span></span>
<span data-ttu-id="671b1-132">Den `gulp run` körs gateway exempelprogram som skickar meddelanden till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="671b1-132">The `gulp run` command runs gateway sample application that sends messages to your IoT hub.</span></span> <span data-ttu-id="671b1-133">Med `table-storage` parameter, den också skapas en underordnad process för att ta emot meddelandet sparade i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="671b1-133">With `table-storage` parameter, it also spawns a child process to receive the saved message in your Azure Table storage.</span></span>

<span data-ttu-id="671b1-134">De meddelanden som skickas och tas emot är alla visas direkt på samma konsolfönstret i värddatorn.</span><span class="sxs-lookup"><span data-stu-id="671b1-134">The messages that are being sent and received are all displayed instantly on the same console window in the host machine.</span></span> <span data-ttu-id="671b1-135">Exempel programinstansen avslutas automatiskt i 40 sekunder.</span><span class="sxs-lookup"><span data-stu-id="671b1-135">The sample application instance will terminate automatically in 40 seconds.</span></span>

   ![gulp Läs](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table_simudev.png)


## <a name="summary"></a><span data-ttu-id="671b1-137">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="671b1-137">Summary</span></span>

<span data-ttu-id="671b1-138">Du har kört exempelkod för att läsa meddelanden i din Azure Table storage sparas av ditt program för Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="671b1-138">You've run the sample code to read the messages in your Azure Table storage saved by your Azure Function application.</span></span>
