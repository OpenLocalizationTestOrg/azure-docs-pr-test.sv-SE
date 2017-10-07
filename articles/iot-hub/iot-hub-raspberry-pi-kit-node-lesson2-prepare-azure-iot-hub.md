---
featureFlags: usabilla
title: 'Ansluta hallon Pi (nod) tooAzure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera Pi i hello IoT-hubb identitetsregistret med hjälp av Azure CLI."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: ansluta till Raspberry pi moln, pi-moln
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 736215b6-e7e4-46f9-af30-0ded9ffa5204
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 97533298d52d1187c49a4c35ddda922d6e45c87d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a><span data-ttu-id="f2c57-104">Skapa din IoT-hubb och registrera hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="f2c57-104">Create your IoT hub and register Raspberry Pi 3</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="f2c57-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="f2c57-105">What you will do</span></span>
* <span data-ttu-id="f2c57-106">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f2c57-106">Create a resource group.</span></span>
* <span data-ttu-id="f2c57-107">Skapa din Azure IoT-hubb i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f2c57-107">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="f2c57-108">Lägga till hallon Pi 3 toohello Azure IoT-hubb med hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="f2c57-108">Add Raspberry Pi 3 toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="f2c57-109">När du använder Azure CLI tooadd Pi tooyour IoT-hubb genererar hello tjänsten en nyckel för Pi tooauthenticate med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f2c57-109">When you use Azure CLI tooadd Pi tooyour IoT hub, hello service generates a key for Pi tooauthenticate with hello service.</span></span> <span data-ttu-id="f2c57-110">Om du har några problem med sökning lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f2c57-110">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f2c57-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="f2c57-111">What you will learn</span></span>
<span data-ttu-id="f2c57-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="f2c57-112">In this article, you will learn:</span></span>
* <span data-ttu-id="f2c57-113">Hur toouse Azure CLI toocreate en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="f2c57-113">How toouse Azure CLI toocreate an IoT hub</span></span>
* <span data-ttu-id="f2c57-114">Hur toocreate enhetsidentitet för Pi i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="f2c57-114">How toocreate a device identity for Pi in your IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f2c57-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="f2c57-115">What you need</span></span>
* <span data-ttu-id="f2c57-116">Ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="f2c57-116">An Azure account</span></span>
* <span data-ttu-id="f2c57-117">En Mac- eller en Windows-dator med hello Azure CLI installerat</span><span class="sxs-lookup"><span data-stu-id="f2c57-117">A Mac or a Windows computer with hello Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="f2c57-118">Skapa din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="f2c57-118">Create your IoT hub</span></span>
<span data-ttu-id="f2c57-119">Azure IoT-hubb hjälper dig att ansluta, övervaka och hantera miljontals IoT tillgångar.</span><span class="sxs-lookup"><span data-stu-id="f2c57-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="f2c57-120">toocreate din IoT-hubb gör du följande:</span><span class="sxs-lookup"><span data-stu-id="f2c57-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="f2c57-121">Logga in tooyour Azure-konto genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f2c57-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="f2c57-122">Alla tillgängliga prenumerationer visas efter en lyckad inloggning.</span><span class="sxs-lookup"><span data-stu-id="f2c57-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="f2c57-123">Ange hello standard prenumeration som du vill toouse genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f2c57-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="f2c57-124">`subscription ID or name`hittar du i hello utdata från hello `az login` eller hello `az account list` kommando.</span><span class="sxs-lookup"><span data-stu-id="f2c57-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="f2c57-125">Registrera hello providern genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2c57-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="f2c57-126">Resursproviders är tjänster som tillhandahåller resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="f2c57-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="f2c57-127">Du måste registrera hello providern innan du kan distribuera hello Azure-resurs som hello tjänstleverantören erbjuder.</span><span class="sxs-lookup"><span data-stu-id="f2c57-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="f2c57-128">Skapa en resursgrupp med namnet iot-sample i hello västra USA region genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f2c57-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="f2c57-129">`westus`är hello-plats som du skapar din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f2c57-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="f2c57-130">Om du vill toouse till en annan plats, kan du köra `az account list-locations -o table` toosee alla hello platser har stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="f2c57-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>
 
5. <span data-ttu-id="f2c57-131">Skapa en IoT-hubb i hello iot-sample resursgruppen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f2c57-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   <span data-ttu-id="f2c57-132">Som standard skapar hello verktyget en IoT-hubb i hello kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="f2c57-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="f2c57-133">Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="f2c57-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE] 
> <span data-ttu-id="f2c57-134">hello namnet på din IoT-hubb måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="f2c57-134">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="f2c57-135">Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f2c57-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-pi-in-your-iot-hub"></a><span data-ttu-id="f2c57-136">Registrera Pi i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="f2c57-136">Register Pi in your IoT hub</span></span>
<span data-ttu-id="f2c57-137">Varje enhet som skickar meddelanden tooyour IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="f2c57-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span> <span data-ttu-id="f2c57-138">Du kommer att använda Azure CLI tooregister din Pi och skapa ett självsignerat X.509-certifikat för autentisering.</span><span class="sxs-lookup"><span data-stu-id="f2c57-138">You will use Azure CLI tooregister your Pi and create a self-signed X.509 certificate for device authentication.</span></span>

<span data-ttu-id="f2c57-139">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f2c57-139">Run hello following command:</span></span>

```bash
# For Windows command prompt
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir %USERPROFILE%\.iot-hub-getting-started
 
# For macOS or Ubuntu
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir ~/.iot-hub-getting-started
```

## <a name="summary"></a><span data-ttu-id="f2c57-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f2c57-140">Summary</span></span>
<span data-ttu-id="f2c57-141">Du har skapat en IoT-hubb och registrerad Pi med en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f2c57-141">You've created an IoT hub and registered Pi with a device identity in your IoT hub.</span></span> <span data-ttu-id="f2c57-142">Du är klar toolearn hur toosend meddelanden från Pi tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f2c57-142">You're ready toolearn how toosend messages from Pi tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2c57-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f2c57-143">Next steps</span></span>
[<span data-ttu-id="f2c57-144">Skapa en funktionsapp i Azure-och ett Azure storage-konto tooprocess och lagra IoT-hubb meddelanden</span><span class="sxs-lookup"><span data-stu-id="f2c57-144">Create an Azure function app and an Azure storage account tooprocess and store IoT hub messages</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

