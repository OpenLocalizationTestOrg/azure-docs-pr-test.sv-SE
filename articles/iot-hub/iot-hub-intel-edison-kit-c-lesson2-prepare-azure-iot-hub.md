---
title: 'Connect Intel EDISON (C) tooAzure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera modern i hello Azure IoT-hubb med hjälp av hello Azure CLI."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 80bfc3cd-a1fc-4775-8994-d8033381dd3d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9635e916425883d65793d0ed46843ab49b3f35ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-intel-edison"></a><span data-ttu-id="41600-103">Skapa din IoT-hubb och registrera Intel modern</span><span class="sxs-lookup"><span data-stu-id="41600-103">Create your IoT hub and register Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="41600-104">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="41600-104">What you will do</span></span>
* <span data-ttu-id="41600-105">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="41600-105">Create a resource group.</span></span>
* <span data-ttu-id="41600-106">Skapa din Azure IoT-hubb i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="41600-106">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="41600-107">Lägga till Intel modern toohello Azure IoT-hubb med hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="41600-107">Add Intel Edison toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="41600-108">När du använder hello Azure CLI tooadd modern tooyour IoT-hubb genererar hello tjänsten en nyckel för modern tooauthenticate med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="41600-108">When you use hello Azure CLI tooadd Edison tooyour IoT hub, hello service generates a key for Edison tooauthenticate with hello service.</span></span> <span data-ttu-id="41600-109">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="41600-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="41600-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="41600-110">What you will learn</span></span>
<span data-ttu-id="41600-111">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="41600-111">In this article, you will learn:</span></span>
* <span data-ttu-id="41600-112">Hur toouse hello Azure CLI toocreate en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="41600-112">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
* <span data-ttu-id="41600-113">Hur toocreate enhetsidentitet för modern i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="41600-113">How toocreate a device identity for Edison in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="41600-114">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="41600-114">What you need</span></span>
* <span data-ttu-id="41600-115">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="41600-115">An Azure account.</span></span> <span data-ttu-id="41600-116">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="41600-116">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
* <span data-ttu-id="41600-117">Du bör ha hello Azure CLI installerad.</span><span class="sxs-lookup"><span data-stu-id="41600-117">You should have hello Azure CLI installed.</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="41600-118">Skapa din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="41600-118">Create your IoT hub</span></span>
<span data-ttu-id="41600-119">Azure IoT-hubb hjälper dig att ansluta, övervaka och hantera miljontals IoT tillgångar.</span><span class="sxs-lookup"><span data-stu-id="41600-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="41600-120">toocreate din IoT-hubb gör du följande:</span><span class="sxs-lookup"><span data-stu-id="41600-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="41600-121">Logga in tooyour Azure-konto genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="41600-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="41600-122">Alla tillgängliga prenumerationer visas efter en lyckad inloggning.</span><span class="sxs-lookup"><span data-stu-id="41600-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="41600-123">Ange hello standard prenumeration som du vill toouse genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="41600-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="41600-124">`subscription ID or name`hittar du i hello utdata från hello `az login` eller hello `az account list` kommando.</span><span class="sxs-lookup"><span data-stu-id="41600-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="41600-125">Registrera hello providern genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="41600-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="41600-126">Resursproviders är tjänster som tillhandahåller resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="41600-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="41600-127">Du måste registrera hello providern innan du kan distribuera hello Azure-resurs som hello tjänstleverantören erbjuder.</span><span class="sxs-lookup"><span data-stu-id="41600-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="41600-128">Skapa en resursgrupp med namnet iot-sample i hello västra USA region genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="41600-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="41600-129">`westus`är hello-plats som du skapar din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="41600-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="41600-130">Om du vill toouse till en annan plats, kan du köra `az account list-locations -o table` toosee alla hello platser har stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="41600-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>

5. <span data-ttu-id="41600-131">Skapa en IoT-hubb i hello iot-sample resursgruppen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="41600-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

<span data-ttu-id="41600-132">Som standard skapar hello verktyget en IoT-hubb i hello kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="41600-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="41600-133">Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="41600-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE] 
> <span data-ttu-id="41600-134">hello namnet på din IoT-hubb måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="41600-134">hello name of your IoT hub must be globally unique.</span></span>
> <span data-ttu-id="41600-135">Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="41600-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>


## <a name="register-edison-in-your-iot-hub"></a><span data-ttu-id="41600-136">Registrera modern i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="41600-136">Register Edison in your IoT hub</span></span>
<span data-ttu-id="41600-137">Varje enhet som skickar meddelanden tooyour IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="41600-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="41600-138">Registrera modern i din IoT-hubb genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="41600-138">Register Edison in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a><span data-ttu-id="41600-139">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="41600-139">Summary</span></span>
<span data-ttu-id="41600-140">Du har skapat en IoT-hubb och registrerad modern med en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="41600-140">You've created an IoT hub and registered Edison with a device identity in your IoT hub.</span></span> <span data-ttu-id="41600-141">Du är klar toolearn hur toosend meddelanden från modern tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="41600-141">You're ready toolearn how toosend messages from Edison tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="41600-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="41600-142">Next steps</span></span>
<span data-ttu-id="41600-143">[Skapa en funktionsapp i Azure-och en Azure Storage-konto tooprocess och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="41600-143">[Create an Azure function app and an Azure Storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md