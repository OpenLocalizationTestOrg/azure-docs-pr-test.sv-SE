---
title: 'Connect Raspberry PI (C) till Azure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera Pi i Azure IoT-hubb med hjälp av Azure CLI."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: ansluta till Raspberry pi moln, pi-moln
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d7bfd8f6ae8d15dfe09f06a40a4ab415ff2e0a7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a><span data-ttu-id="6d8da-104">Skapa din IoT-hubb och registrera hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="6d8da-104">Create your IoT hub and register Raspberry Pi 3</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="6d8da-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="6d8da-105">What you will do</span></span>
* <span data-ttu-id="6d8da-106">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6d8da-106">Create a resource group.</span></span>
* <span data-ttu-id="6d8da-107">Skapa din Azure IoT-hubb i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6d8da-107">Create your Azure IoT hub in the resource group.</span></span>
* <span data-ttu-id="6d8da-108">Lägg till hallon Pi 3 Azure IoT-hubben med hjälp av Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="6d8da-108">Add Raspberry Pi 3 to the Azure IoT hub by using the Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="6d8da-109">När du använder Azure CLI för att lägga till Pi din IoT-hubb kan genererar tjänsten en nyckel för Pi att autentisera med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6d8da-109">When you use the Azure CLI to add Pi to your IoT hub, the service generates a key for Pi to authenticate with the service.</span></span> <span data-ttu-id="6d8da-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="6d8da-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6d8da-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="6d8da-111">What you will learn</span></span>
<span data-ttu-id="6d8da-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="6d8da-112">In this article, you will learn:</span></span>
* <span data-ttu-id="6d8da-113">Hur du använder Azure CLI för att skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6d8da-113">How to use the Azure CLI to create an IoT hub.</span></span>
* <span data-ttu-id="6d8da-114">Så här skapar du en enhetsidentitet för Pi i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6d8da-114">How to create a device identity for Pi in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6d8da-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="6d8da-115">What you need</span></span>
* <span data-ttu-id="6d8da-116">Ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="6d8da-116">An Azure account</span></span>
* <span data-ttu-id="6d8da-117">En Mac- eller en Windows-dator med Azure CLI installerad</span><span class="sxs-lookup"><span data-stu-id="6d8da-117">A Mac or a Windows computer with the Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="6d8da-118">Skapa din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6d8da-118">Create your IoT hub</span></span>
<span data-ttu-id="6d8da-119">Azure IoT-hubb hjälper dig att ansluta, övervaka och hantera miljontals IoT tillgångar.</span><span class="sxs-lookup"><span data-stu-id="6d8da-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="6d8da-120">Följ dessa steg för att skapa din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="6d8da-120">To create your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="6d8da-121">Logga in på ditt Azure-konto genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6d8da-121">Sign in to your Azure account by running the following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="6d8da-122">Alla tillgängliga prenumerationer visas efter en lyckad inloggning.</span><span class="sxs-lookup"><span data-stu-id="6d8da-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="6d8da-123">Ange den standard-prenumeration som du vill använda genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6d8da-123">Set the default subscription that you want to use by running the following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="6d8da-124">`subscription ID or name`hittar du i utdata från den `az login` eller `az account list` kommando.</span><span class="sxs-lookup"><span data-stu-id="6d8da-124">`subscription ID or name` can be found in the output of the `az login` or the `az account list` command.</span></span>

3. <span data-ttu-id="6d8da-125">Registrera providern genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="6d8da-125">Register the provider by running the following command.</span></span> <span data-ttu-id="6d8da-126">Resursproviders är tjänster som tillhandahåller resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="6d8da-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="6d8da-127">Du måste registrera providern innan du kan distribuera Azure-resurs som providern erbjuder.</span><span class="sxs-lookup"><span data-stu-id="6d8da-127">You must register the provider before you can deploy the Azure resource that the provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="6d8da-128">Skapa en resursgrupp med namnet iot-sample i USA, västra region genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6d8da-128">Create a resource group named iot-sample in the West US region by running the following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="6d8da-129">`westus`är den plats som du skapar din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6d8da-129">`westus` is the location you create your resource group.</span></span> <span data-ttu-id="6d8da-130">Om du vill använda en annan plats, kan du köra `az account list-locations -o table` att se alla platser har stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="6d8da-130">If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.</span></span>
 
5. <span data-ttu-id="6d8da-131">Skapa en IoT-hubb i iot-sample resursgruppen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6d8da-131">Create an IoT hub in the iot-sample resource group by running the following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   <span data-ttu-id="6d8da-132">Som standard skapar verktyget en IoT-hubb i den kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="6d8da-132">By default, the tool creates an IoT Hub in the Free pricing tier.</span></span> <span data-ttu-id="6d8da-133">Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="6d8da-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="6d8da-134">Namnet på din IoT-hubb måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="6d8da-134">The name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="6d8da-135">Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6d8da-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-pi-in-your-iot-hub"></a><span data-ttu-id="6d8da-136">Registrera Pi i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6d8da-136">Register Pi in your IoT hub</span></span>
<span data-ttu-id="6d8da-137">Varje enhet som skickar meddelanden till din IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="6d8da-137">Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="6d8da-138">Registrera Pi i din hubb genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6d8da-138">Register Pi in your hub by running following command:</span></span>

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="summary"></a><span data-ttu-id="6d8da-139">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6d8da-139">Summary</span></span>
<span data-ttu-id="6d8da-140">Du har skapat en IoT-hubb och registrerad Pi med en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6d8da-140">You've created an IoT hub and registered Pi with a device identity in your IoT hub.</span></span> <span data-ttu-id="6d8da-141">Du är redo att lära dig hur du skickar meddelanden från Pi till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6d8da-141">You're ready to learn how to send messages from Pi to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d8da-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6d8da-142">Next steps</span></span>
<span data-ttu-id="6d8da-143">[Skapa en funktionsapp i Azure-och ett Azure Storage-konto för att bearbeta och lagra IoT-hubb meddelanden](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="6d8da-143">[Create an Azure function app and an Azure Storage account to process and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span></span>

