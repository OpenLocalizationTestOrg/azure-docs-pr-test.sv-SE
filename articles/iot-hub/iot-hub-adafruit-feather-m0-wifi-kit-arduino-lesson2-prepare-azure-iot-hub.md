---
title: 'Ansluta Arduino tooAzure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera Adafruit ludd M0 WiFi i hello Azure IoT-hubb med hjälp av hello Azure CLI."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: ansluta arduino toocloud, azure iot-hubb, internet i saker moln, azure iot-hubb skapar enhet, arduino moln
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 5edc690b-7a1d-4ebc-b011-ff27bfffe6e8
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ca362f9c143dd3a98bf47a66b63a9725a0ffc2d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-your-adafruit-feather-m0-wifi-arduino-board"></a><span data-ttu-id="37b4e-104">Skapa din IoT-hubb och registrera Adafruit ludd M0 WiFi Arduino-kort</span><span class="sxs-lookup"><span data-stu-id="37b4e-104">Create your IoT hub and register your Adafruit Feather M0 WiFi Arduino board</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="37b4e-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="37b4e-105">What you will do</span></span>
* <span data-ttu-id="37b4e-106">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="37b4e-106">Create a resource group.</span></span>
* <span data-ttu-id="37b4e-107">Skapa din Azure IoT-hubb i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="37b4e-107">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="37b4e-108">Lägga till din Arduino board toohello Azure IoT-hubb med hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="37b4e-108">Add your Arduino board toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="37b4e-109">När du använder hello Azure CLI tooadd din IoT-hubb Arduino board tooyour genererar hello tjänsten en nyckel för din Arduino board tooauthenticate med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="37b4e-109">When you use hello Azure CLI tooadd your Arduino board tooyour IoT hub, hello service generates a key for your Arduino board tooauthenticate with hello service.</span></span> <span data-ttu-id="37b4e-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshoot].</span><span class="sxs-lookup"><span data-stu-id="37b4e-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshoot].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="37b4e-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="37b4e-111">What you will learn</span></span>
<span data-ttu-id="37b4e-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="37b4e-112">In this article, you will learn:</span></span>
* <span data-ttu-id="37b4e-113">Hur toouse hello Azure CLI toocreate en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="37b4e-113">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
* <span data-ttu-id="37b4e-114">Hur board toocreate en enhetsidentitet för din Arduino i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="37b4e-114">How toocreate a device identity for your Arduino board in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="37b4e-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="37b4e-115">What you need</span></span>
* <span data-ttu-id="37b4e-116">Ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="37b4e-116">An Azure account</span></span>
* <span data-ttu-id="37b4e-117">En dator med hello Azure CLI är installerat</span><span class="sxs-lookup"><span data-stu-id="37b4e-117">A computer with hello Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="37b4e-118">Skapa din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="37b4e-118">Create your IoT hub</span></span>
<span data-ttu-id="37b4e-119">Azure IoT-hubb hjälper dig att ansluta, övervaka och hantera miljontals IoT tillgångar.</span><span class="sxs-lookup"><span data-stu-id="37b4e-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="37b4e-120">toocreate din IoT-hubb gör du följande:</span><span class="sxs-lookup"><span data-stu-id="37b4e-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="37b4e-121">Logga in tooyour Azure-konto genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="37b4e-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="37b4e-122">Alla tillgängliga prenumerationer visas efter en lyckad inloggning.</span><span class="sxs-lookup"><span data-stu-id="37b4e-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="37b4e-123">Ange hello standard prenumeration som du vill toouse genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="37b4e-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="37b4e-124">`subscription ID or name`hittar du i hello utdata från hello `az login` eller hello `az account list` kommando.</span><span class="sxs-lookup"><span data-stu-id="37b4e-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="37b4e-125">Registrera hello providern genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="37b4e-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="37b4e-126">Resursproviders är tjänster som tillhandahåller resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="37b4e-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="37b4e-127">Du måste registrera hello providern innan du kan distribuera hello Azure-resurs som hello tjänstleverantören erbjuder.</span><span class="sxs-lookup"><span data-stu-id="37b4e-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="37b4e-128">Skapa en resursgrupp med namnet iot-sample i hello västra USA region genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="37b4e-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="37b4e-129">`westus`är hello-plats som du skapar din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="37b4e-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="37b4e-130">Om du vill toouse till en annan plats, kan du köra `az account list-locations -o table` toosee alla hello platser har stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="37b4e-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>

5. <span data-ttu-id="37b4e-131">Skapa en IoT-hubb i hello iot-sample resursgruppen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="37b4e-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

<span data-ttu-id="37b4e-132">Som standard skapar hello verktyget en IoT-hubb i hello kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="37b4e-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="37b4e-133">Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="37b4e-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="37b4e-134">hello namnet på din IoT-hubb måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="37b4e-134">hello name of your IoT hub must be globally unique.</span></span>
> <span data-ttu-id="37b4e-135">Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="37b4e-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-your-arduino-board-in-your-iot-hub"></a><span data-ttu-id="37b4e-136">Registrera din Arduino för ändringar i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="37b4e-136">Register your Arduino board in your IoT hub</span></span>
<span data-ttu-id="37b4e-137">Varje enhet som skickar meddelanden tooyour IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="37b4e-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="37b4e-138">Registrera din Arduino för ändringar i din IoT-hubb genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="37b4e-138">Register your Arduino board in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id mym0wifi --hub-name {my hub name}
```

## <a name="summary"></a><span data-ttu-id="37b4e-139">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="37b4e-139">Summary</span></span>
<span data-ttu-id="37b4e-140">Du har skapat en IoT-hubb och registrerad Arduino-kort med en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="37b4e-140">You've created an IoT hub and registered your Arduino board with a device identity in your IoT hub.</span></span> <span data-ttu-id="37b4e-141">Du är klar toolearn hur toosend meddelanden från din Arduino board tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="37b4e-141">You're ready toolearn how toosend messages from your Arduino board tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37b4e-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37b4e-142">Next steps</span></span>
<span data-ttu-id="37b4e-143">[Skapa en funktionsapp i Azure-och en Azure Storage-konto tooprocess och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="37b4e-143">[Create an Azure function app and an Azure Storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>


<!-- Images and links -->

[troubleshoot]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md