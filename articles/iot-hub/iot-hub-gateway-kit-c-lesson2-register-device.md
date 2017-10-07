---
title: 'SensorTag enhet & Azure IoT-Gateway - lektion 2: registrera enhet | Microsoft Docs'
description: 
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure iot-hubb internet av saker moln, azure iot-hubb skapar enhet, ti sensortag, ti tivera
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 2c18f5ae-e39a-48ae-a9fe-04bb595740a0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5d2322268aa18f52f60c2833778323773ac4eec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a><span data-ttu-id="189f6-103">Skapa din Azure IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="189f6-103">Create your Azure IoT hub and register your device</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="189f6-104">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="189f6-104">What you will do</span></span>

- <span data-ttu-id="189f6-105">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="189f6-105">Create a resource group</span></span>
- <span data-ttu-id="189f6-106">Skapa din första IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="189f6-106">Create your first IoT hub</span></span>
- <span data-ttu-id="189f6-107">Registrera din enhet i din IoT-hubb med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="189f6-107">Register your device in your IoT hub by using hello Azure CLI.</span></span> 

<span data-ttu-id="189f6-108">När du registrerar din enhet i din IoT-hubb genererar hello Azure IoT Hub service en nyckel för din enhet toouse tooauthenticate med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="189f6-108">When you register your device in your IoT hub, hello Azure IoT Hub service generates a key for your device toouse tooauthenticate with hello service.</span></span> 

<span data-ttu-id="189f6-109">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="189f6-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="189f6-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="189f6-110">What you will learn</span></span>

<span data-ttu-id="189f6-111">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="189f6-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="189f6-112">Hur toouse hello Azure CLI toocreate en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="189f6-112">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
- <span data-ttu-id="189f6-113">Hur tooregister en enhet i en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="189f6-113">How tooregister a device in an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="189f6-114">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="189f6-114">What you need</span></span>

- <span data-ttu-id="189f6-115">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="189f6-115">An active Azure subscription.</span></span> <span data-ttu-id="189f6-116">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="189f6-116">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
- <span data-ttu-id="189f6-117">Du bör ha hello Azure CLI installerad.</span><span class="sxs-lookup"><span data-stu-id="189f6-117">You should have hello Azure CLI installed.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="189f6-118">Skapa en IoT Hub</span><span class="sxs-lookup"><span data-stu-id="189f6-118">Create an IoT hub</span></span>

<span data-ttu-id="189f6-119">toocreate en IoT-hubb så här:</span><span class="sxs-lookup"><span data-stu-id="189f6-119">toocreate an IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="189f6-120">Logga in tooyour Azure-konto genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="189f6-120">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="189f6-121">Alla tillgängliga prenumerationer visas efter en lyckad inloggning.</span><span class="sxs-lookup"><span data-stu-id="189f6-121">All your available subscriptions will be listed after a successful sign-in.</span></span>

2. <span data-ttu-id="189f6-122">Ange standard hello Azure-prenumeration som du vill toouse genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="189f6-122">Set hello default Azure subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="189f6-123">`subscription ID or name`hittar du i hello utdata från hello `az login` eller hello `az account list` kommando.</span><span class="sxs-lookup"><span data-stu-id="189f6-123">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="189f6-124">Registrera hello providern genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="189f6-124">Register hello provider by running hello following command.</span></span> <span data-ttu-id="189f6-125">Resursproviders är tjänster som tillhandahåller resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="189f6-125">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="189f6-126">Du måste registrera hello providern innan du kan distribuera hello Azure-resurs som hello tjänstleverantören erbjuder.</span><span class="sxs-lookup"><span data-stu-id="189f6-126">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. <span data-ttu-id="189f6-127">Skapa en resursgrupp med namnet `iot-gateway` i hello västra USA region genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="189f6-127">Create a resource group named `iot-gateway` in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   <span data-ttu-id="189f6-128">`westus`är hello-plats som du skapar din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="189f6-128">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="189f6-129">Om du vill toouse till en annan plats, kan du köra `az account list-locations -o table` toosee alla hello platser har stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="189f6-129">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>

5. <span data-ttu-id="189f6-130">Skapa en IoT-hubb i hello `iot-gateway` resursgruppen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="189f6-130">Create an IoT hub in hello `iot-gateway` resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

<span data-ttu-id="189f6-131">Som standard skapar hello verktyget en IoT-hubb i hello kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="189f6-131">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="189f6-132">Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="189f6-132">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="189f6-133">hello namnet på din IoT-hubb måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="189f6-133">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="189f6-134">Du kan skapa en enda F1-versionen av Azure Iot Hub under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="189f6-134">You can create only one F1 edition of Azure Iot Hub under your Azure subscription.</span></span>

## <a name="register-your-device-in-your-iot-hub"></a><span data-ttu-id="189f6-135">Registrera din enhet i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="189f6-135">Register your device in your IoT hub</span></span>

<span data-ttu-id="189f6-136">Varje enhet som skickar meddelanden tooyour IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="189f6-136">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>
<span data-ttu-id="189f6-137">Registrera din enhet i din IoT-hubb genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="189f6-137">Register your device in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a><span data-ttu-id="189f6-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="189f6-138">Summary</span></span>

<span data-ttu-id="189f6-139">Du har skapat en IoT-hubb och registrerat din logiska enhet med en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="189f6-139">You've created an IoT hub and registered your logical device with a device identity in your IoT hub.</span></span> <span data-ttu-id="189f6-140">Du är klar toolearn hur tooconfigure och kör en gateway exempel toosend programdata från fysisk enhet tooyour IoT-hubb i hello cloud.</span><span class="sxs-lookup"><span data-stu-id="189f6-140">You're ready toolearn how tooconfigure and run a gateway sample application toosend data from your physical device tooyour IoT hub in hello cloud.</span></span>

## <a name="next-steps"></a><span data-ttu-id="189f6-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="189f6-141">Next steps</span></span>
[<span data-ttu-id="189f6-142">Konfigurera och köra en ort exempelapp</span><span class="sxs-lookup"><span data-stu-id="189f6-142">Configure and run a BLE sample app</span></span>](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)