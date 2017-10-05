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
ms.openlocfilehash: 685a479583f5f11f57bef22dc5881285bb1f70d0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a><span data-ttu-id="22ba4-103">Skapa din Azure IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="22ba4-103">Create your Azure IoT hub and register your device</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="22ba4-104">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="22ba4-104">What you will do</span></span>

- <span data-ttu-id="22ba4-105">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="22ba4-105">Create a resource group</span></span>
- <span data-ttu-id="22ba4-106">Skapa din första IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="22ba4-106">Create your first IoT hub</span></span>
- <span data-ttu-id="22ba4-107">Registrera din enhet i din IoT-hubb med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="22ba4-107">Register your device in your IoT hub by using the Azure CLI.</span></span> 

<span data-ttu-id="22ba4-108">När du registrerar din enhet i din IoT-hubb skapar en nyckel för att enheten ska använda för att autentisera med tjänsten Azure IoT Hub-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="22ba4-108">When you register your device in your IoT hub, the Azure IoT Hub service generates a key for your device to use to authenticate with the service.</span></span> 

<span data-ttu-id="22ba4-109">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="22ba4-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="22ba4-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="22ba4-110">What you will learn</span></span>

<span data-ttu-id="22ba4-111">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="22ba4-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="22ba4-112">Hur du använder Azure CLI för att skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="22ba4-112">How to use the Azure CLI to create an IoT hub.</span></span>
- <span data-ttu-id="22ba4-113">Hur du registrerar en enhet i en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="22ba4-113">How to register a device in an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="22ba4-114">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="22ba4-114">What you need</span></span>

- <span data-ttu-id="22ba4-115">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="22ba4-115">An active Azure subscription.</span></span> <span data-ttu-id="22ba4-116">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="22ba4-116">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
- <span data-ttu-id="22ba4-117">Du bör ha Azure CLI installerad.</span><span class="sxs-lookup"><span data-stu-id="22ba4-117">You should have the Azure CLI installed.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="22ba4-118">Skapa en IoT Hub</span><span class="sxs-lookup"><span data-stu-id="22ba4-118">Create an IoT hub</span></span>

<span data-ttu-id="22ba4-119">Följ dessa steg om du vill skapa en IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="22ba4-119">To create an IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="22ba4-120">Logga in på ditt Azure-konto genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="22ba4-120">Sign in to your Azure account by running the following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="22ba4-121">Alla tillgängliga prenumerationer visas efter en lyckad inloggning.</span><span class="sxs-lookup"><span data-stu-id="22ba4-121">All your available subscriptions will be listed after a successful sign-in.</span></span>

2. <span data-ttu-id="22ba4-122">Ange Azure-prenumeration som du vill använda genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="22ba4-122">Set the default Azure subscription that you want to use by running the following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="22ba4-123">`subscription ID or name`hittar du i utdata från den `az login` eller `az account list` kommando.</span><span class="sxs-lookup"><span data-stu-id="22ba4-123">`subscription ID or name` can be found in the output of the `az login` or the `az account list` command.</span></span>

3. <span data-ttu-id="22ba4-124">Registrera providern genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="22ba4-124">Register the provider by running the following command.</span></span> <span data-ttu-id="22ba4-125">Resursproviders är tjänster som tillhandahåller resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="22ba4-125">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="22ba4-126">Du måste registrera providern innan du kan distribuera Azure-resurs som providern erbjuder.</span><span class="sxs-lookup"><span data-stu-id="22ba4-126">You must register the provider before you can deploy the Azure resource that the provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. <span data-ttu-id="22ba4-127">Skapa en resursgrupp med namnet `iot-gateway` i USA, västra region genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="22ba4-127">Create a resource group named `iot-gateway` in the West US region by running the following command:</span></span>

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   <span data-ttu-id="22ba4-128">`westus`är den plats som du skapar din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="22ba4-128">`westus` is the location you create your resource group.</span></span> <span data-ttu-id="22ba4-129">Om du vill använda en annan plats, kan du köra `az account list-locations -o table` att se alla platser har stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="22ba4-129">If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.</span></span>

5. <span data-ttu-id="22ba4-130">Skapa en IoT-hubb i den `iot-gateway` resursgruppen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="22ba4-130">Create an IoT hub in the `iot-gateway` resource group by running the following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

<span data-ttu-id="22ba4-131">Som standard skapar verktyget en IoT-hubb i den kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="22ba4-131">By default, the tool creates an IoT Hub in the Free pricing tier.</span></span> <span data-ttu-id="22ba4-132">Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="22ba4-132">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="22ba4-133">Namnet på din IoT-hubb måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="22ba4-133">The name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="22ba4-134">Du kan skapa en enda F1-versionen av Azure Iot Hub under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="22ba4-134">You can create only one F1 edition of Azure Iot Hub under your Azure subscription.</span></span>

## <a name="register-your-device-in-your-iot-hub"></a><span data-ttu-id="22ba4-135">Registrera din enhet i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="22ba4-135">Register your device in your IoT hub</span></span>

<span data-ttu-id="22ba4-136">Varje enhet som skickar meddelanden till din IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="22ba4-136">Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>
<span data-ttu-id="22ba4-137">Registrera din enhet i din IoT-hubb genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="22ba4-137">Register your device in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a><span data-ttu-id="22ba4-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="22ba4-138">Summary</span></span>

<span data-ttu-id="22ba4-139">Du har skapat en IoT-hubb och registrerat din logiska enhet med en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="22ba4-139">You've created an IoT hub and registered your logical device with a device identity in your IoT hub.</span></span> <span data-ttu-id="22ba4-140">Du är redo att lära dig hur du konfigurerar och kör ett exempelprogram för gateway för att skicka data från den fysiska enheten till din IoT-hubb i molnet.</span><span class="sxs-lookup"><span data-stu-id="22ba4-140">You're ready to learn how to configure and run a gateway sample application to send data from your physical device to your IoT hub in the cloud.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22ba4-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="22ba4-141">Next steps</span></span>
[<span data-ttu-id="22ba4-142">Konfigurera och köra en ort exempelapp</span><span class="sxs-lookup"><span data-stu-id="22ba4-142">Configure and run a BLE sample app</span></span>](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)