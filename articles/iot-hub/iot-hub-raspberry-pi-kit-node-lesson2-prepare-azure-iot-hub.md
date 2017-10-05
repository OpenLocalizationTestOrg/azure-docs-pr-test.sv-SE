---
featureFlags: usabilla
title: 'Ansluta hallon Pi (nod) till Azure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera Pi i IoT-hubb identitetsregistret med hjälp av Azure CLI."
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
ms.openlocfilehash: 774f9356d7a11b2c61905ada75bed92d44e5fc0c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a><span data-ttu-id="32b14-104">Skapa din IoT-hubb och registrera hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="32b14-104">Create your IoT hub and register Raspberry Pi 3</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="32b14-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="32b14-105">What you will do</span></span>
* <span data-ttu-id="32b14-106">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="32b14-106">Create a resource group.</span></span>
* <span data-ttu-id="32b14-107">Skapa din Azure IoT-hubb i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="32b14-107">Create your Azure IoT hub in the resource group.</span></span>
* <span data-ttu-id="32b14-108">Lägg till hallon Pi 3 Azure IoT-hubben med hjälp av Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="32b14-108">Add Raspberry Pi 3 to the Azure IoT hub by using the Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="32b14-109">När du använder Azure CLI för att lägga till Pi din IoT-hubb kan genererar tjänsten en nyckel för Pi att autentisera med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="32b14-109">When you use Azure CLI to add Pi to your IoT hub, the service generates a key for Pi to authenticate with the service.</span></span> <span data-ttu-id="32b14-110">Om du har några problem, söka efter lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="32b14-110">If you have any problems, seek solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="32b14-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="32b14-111">What you will learn</span></span>
<span data-ttu-id="32b14-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="32b14-112">In this article, you will learn:</span></span>
* <span data-ttu-id="32b14-113">Hur du använder Azure CLI för att skapa en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="32b14-113">How to use Azure CLI to create an IoT hub</span></span>
* <span data-ttu-id="32b14-114">Så här skapar du en enhetsidentitet för Pi i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="32b14-114">How to create a device identity for Pi in your IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="32b14-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="32b14-115">What you need</span></span>
* <span data-ttu-id="32b14-116">Ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="32b14-116">An Azure account</span></span>
* <span data-ttu-id="32b14-117">En Mac- eller en Windows-dator med Azure CLI installerad</span><span class="sxs-lookup"><span data-stu-id="32b14-117">A Mac or a Windows computer with the Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="32b14-118">Skapa din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="32b14-118">Create your IoT hub</span></span>
<span data-ttu-id="32b14-119">Azure IoT-hubb hjälper dig att ansluta, övervaka och hantera miljontals IoT tillgångar.</span><span class="sxs-lookup"><span data-stu-id="32b14-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="32b14-120">Följ dessa steg för att skapa din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="32b14-120">To create your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="32b14-121">Logga in på ditt Azure-konto genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="32b14-121">Sign in to your Azure account by running the following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="32b14-122">Alla tillgängliga prenumerationer visas efter en lyckad inloggning.</span><span class="sxs-lookup"><span data-stu-id="32b14-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="32b14-123">Ange den standard-prenumeration som du vill använda genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="32b14-123">Set the default subscription that you want to use by running the following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="32b14-124">`subscription ID or name`hittar du i utdata från den `az login` eller `az account list` kommando.</span><span class="sxs-lookup"><span data-stu-id="32b14-124">`subscription ID or name` can be found in the output of the `az login` or the `az account list` command.</span></span>

3. <span data-ttu-id="32b14-125">Registrera providern genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="32b14-125">Register the provider by running the following command.</span></span> <span data-ttu-id="32b14-126">Resursproviders är tjänster som tillhandahåller resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="32b14-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="32b14-127">Du måste registrera providern innan du kan distribuera Azure-resurs som providern erbjuder.</span><span class="sxs-lookup"><span data-stu-id="32b14-127">You must register the provider before you can deploy the Azure resource that the provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="32b14-128">Skapa en resursgrupp med namnet iot-sample i USA, västra region genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="32b14-128">Create a resource group named iot-sample in the West US region by running the following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="32b14-129">`westus`är den plats som du skapar din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="32b14-129">`westus` is the location you create your resource group.</span></span> <span data-ttu-id="32b14-130">Om du vill använda en annan plats, kan du köra `az account list-locations -o table` att se alla platser har stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="32b14-130">If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.</span></span>
 
5. <span data-ttu-id="32b14-131">Skapa en IoT-hubb i iot-sample resursgruppen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="32b14-131">Create an IoT hub in the iot-sample resource group by running the following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   <span data-ttu-id="32b14-132">Som standard skapar verktyget en IoT-hubb i den kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="32b14-132">By default, the tool creates an IoT Hub in the Free pricing tier.</span></span> <span data-ttu-id="32b14-133">Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="32b14-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE] 
> <span data-ttu-id="32b14-134">Namnet på din IoT-hubb måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="32b14-134">The name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="32b14-135">Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="32b14-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-pi-in-your-iot-hub"></a><span data-ttu-id="32b14-136">Registrera Pi i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="32b14-136">Register Pi in your IoT hub</span></span>
<span data-ttu-id="32b14-137">Varje enhet som skickar meddelanden till din IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="32b14-137">Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span> <span data-ttu-id="32b14-138">Du använder Azure CLI för att registrera din Pi och skapa ett självsignerat X.509-certifikat för autentisering.</span><span class="sxs-lookup"><span data-stu-id="32b14-138">You will use Azure CLI to register your Pi and create a self-signed X.509 certificate for device authentication.</span></span>

<span data-ttu-id="32b14-139">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="32b14-139">Run the following command:</span></span>

```bash
# For Windows command prompt
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir %USERPROFILE%\.iot-hub-getting-started
 
# For macOS or Ubuntu
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir ~/.iot-hub-getting-started
```

## <a name="summary"></a><span data-ttu-id="32b14-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="32b14-140">Summary</span></span>
<span data-ttu-id="32b14-141">Du har skapat en IoT-hubb och registrerad Pi med en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="32b14-141">You've created an IoT hub and registered Pi with a device identity in your IoT hub.</span></span> <span data-ttu-id="32b14-142">Du är redo att lära dig hur du skickar meddelanden från Pi till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="32b14-142">You're ready to learn how to send messages from Pi to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32b14-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32b14-143">Next steps</span></span>
[<span data-ttu-id="32b14-144">Skapa en funktionsapp i Azure-och ett Azure storage-konto för att bearbeta och lagra meddelanden för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="32b14-144">Create an Azure function app and an Azure storage account to process and store IoT hub messages</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

