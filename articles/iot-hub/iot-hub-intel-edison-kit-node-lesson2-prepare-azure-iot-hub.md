---
title: 'Ansluta Intel modern (nod) till Azure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera modern i Azure IoT-hubb med hjälp av Azure CLI."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: c1465cc2-d0d9-4326-967c-64d95b61dd54
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 81584c1b7314c4331ac059b8e3ed782970588ae2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-intel-edison"></a><span data-ttu-id="4e19d-103">Skapa din IoT-hubb och registrera Intel modern</span><span class="sxs-lookup"><span data-stu-id="4e19d-103">Create your IoT hub and register Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="4e19d-104">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="4e19d-104">What you will do</span></span>
* <span data-ttu-id="4e19d-105">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="4e19d-105">Create a resource group.</span></span>
* <span data-ttu-id="4e19d-106">Skapa din Azure IoT-hubb i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="4e19d-106">Create your Azure IoT hub in the resource group.</span></span>
* <span data-ttu-id="4e19d-107">Lägg till Intel modern Azure IoT-hubben med hjälp av Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="4e19d-107">Add Intel Edison to the Azure IoT hub by using the Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="4e19d-108">När du använder Azure CLI för att lägga till modern din IoT-hubb kan genererar tjänsten en nyckel för modern att autentisera med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4e19d-108">When you use the Azure CLI to add Edison to your IoT hub, the service generates a key for Edison to authenticate with the service.</span></span> <span data-ttu-id="4e19d-109">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="4e19d-109">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4e19d-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="4e19d-110">What you will learn</span></span>
<span data-ttu-id="4e19d-111">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="4e19d-111">In this article, you will learn:</span></span>
* <span data-ttu-id="4e19d-112">Hur du använder Azure CLI för att skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4e19d-112">How to use the Azure CLI to create an IoT hub.</span></span>
* <span data-ttu-id="4e19d-113">Så här skapar du en enhetsidentitet för modern i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4e19d-113">How to create a device identity for Edison in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4e19d-114">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="4e19d-114">What you need</span></span>
* <span data-ttu-id="4e19d-115">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="4e19d-115">An Azure account.</span></span> <span data-ttu-id="4e19d-116">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="4e19d-116">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
* <span data-ttu-id="4e19d-117">Du bör ha Azure CLI installerad.</span><span class="sxs-lookup"><span data-stu-id="4e19d-117">You should have the Azure CLI installed.</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="4e19d-118">Skapa din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="4e19d-118">Create your IoT hub</span></span>
<span data-ttu-id="4e19d-119">Azure IoT-hubb hjälper dig att ansluta, övervaka och hantera miljontals IoT tillgångar.</span><span class="sxs-lookup"><span data-stu-id="4e19d-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="4e19d-120">Följ dessa steg för att skapa din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="4e19d-120">To create your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="4e19d-121">Logga in på ditt Azure-konto genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4e19d-121">Sign in to your Azure account by running the following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="4e19d-122">Alla tillgängliga prenumerationer visas efter en lyckad inloggning.</span><span class="sxs-lookup"><span data-stu-id="4e19d-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="4e19d-123">Ange den standard-prenumeration som du vill använda genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4e19d-123">Set the default subscription that you want to use by running the following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="4e19d-124">`subscription ID or name`hittar du i utdata från den `az login` eller `az account list` kommando.</span><span class="sxs-lookup"><span data-stu-id="4e19d-124">`subscription ID or name` can be found in the output of the `az login` or the `az account list` command.</span></span>

3. <span data-ttu-id="4e19d-125">Registrera providern genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="4e19d-125">Register the provider by running the following command.</span></span> <span data-ttu-id="4e19d-126">Resursproviders är tjänster som tillhandahåller resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="4e19d-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="4e19d-127">Du måste registrera providern innan du kan distribuera Azure-resurs som providern erbjuder.</span><span class="sxs-lookup"><span data-stu-id="4e19d-127">You must register the provider before you can deploy the Azure resource that the provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="4e19d-128">Skapa en resursgrupp med namnet iot-sample i USA, västra region genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4e19d-128">Create a resource group named iot-sample in the West US region by running the following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="4e19d-129">`westus`är den plats som du skapar din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="4e19d-129">`westus` is the location you create your resource group.</span></span> <span data-ttu-id="4e19d-130">Om du vill använda en annan plats, kan du köra `az account list-locations -o table` att se alla platser har stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="4e19d-130">If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.</span></span>

5. <span data-ttu-id="4e19d-131">Skapa en IoT-hubb i iot-sample resursgruppen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4e19d-131">Create an IoT hub in the iot-sample resource group by running the following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

<span data-ttu-id="4e19d-132">Som standard skapar verktyget en IoT-hubb i den kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="4e19d-132">By default, the tool creates an IoT Hub in the Free pricing tier.</span></span> <span data-ttu-id="4e19d-133">Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="4e19d-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE] 
> <span data-ttu-id="4e19d-134">Namnet på din IoT-hubb måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="4e19d-134">The name of your IoT hub must be globally unique.</span></span>
> <span data-ttu-id="4e19d-135">Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4e19d-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>


## <a name="register-edison-in-your-iot-hub"></a><span data-ttu-id="4e19d-136">Registrera modern i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="4e19d-136">Register Edison in your IoT hub</span></span>
<span data-ttu-id="4e19d-137">Varje enhet som skickar meddelanden till din IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="4e19d-137">Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="4e19d-138">Registrera modern i din IoT-hubb genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4e19d-138">Register Edison in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a><span data-ttu-id="4e19d-139">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="4e19d-139">Summary</span></span>
<span data-ttu-id="4e19d-140">Du har skapat en IoT-hubb och registrerad modern med en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4e19d-140">You've created an IoT hub and registered Edison with a device identity in your IoT hub.</span></span> <span data-ttu-id="4e19d-141">Du är redo att lära dig hur du skickar meddelanden från modern till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4e19d-141">You're ready to learn how to send messages from Edison to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e19d-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e19d-142">Next steps</span></span>
<span data-ttu-id="4e19d-143">[Skapa en funktionsapp i Azure-och ett Azure Storage-konto för att bearbeta och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="4e19d-143">[Create an Azure function app and an Azure Storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md