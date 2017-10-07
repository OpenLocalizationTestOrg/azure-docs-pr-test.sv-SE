---
title: "aaaCreate en Azure IoT-hubb med hjälp av en PowerShell-cmdlet | Microsoft Docs"
description: 'Hur toouse toocreate en PowerShell-cmdlet: en IoT-hubb.'
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 005cd8d48eb39d2e8b1bfb9ef84330d4aae4658f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-new-azurermiothub-cmdlet"></a><span data-ttu-id="03b36-103">Skapa en IoT-hubb med hello ny AzureRmIotHub cmdlet</span><span class="sxs-lookup"><span data-stu-id="03b36-103">Create an IoT hub using hello New-AzureRmIotHub cmdlet</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="03b36-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="03b36-104">Introduction</span></span>

<span data-ttu-id="03b36-105">Du kan använda Azure PowerShell-cmdlets toocreate och hantera Azure IoT-hubbar.</span><span class="sxs-lookup"><span data-stu-id="03b36-105">You can use Azure PowerShell cmdlets toocreate and manage Azure IoT hubs.</span></span> <span data-ttu-id="03b36-106">De här självstudierna visar hur toocreate en IoT-hubb med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="03b36-106">This tutorial shows you how toocreate an IoT hub with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="03b36-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="03b36-107">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="03b36-108">Den här artikeln täcker hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="03b36-108">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="03b36-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="03b36-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="03b36-110">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="03b36-110">An active Azure account.</span></span> <br/><span data-ttu-id="03b36-111">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="03b36-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="03b36-112">[Azure PowerShell-cmdlets][lnk-powershell-install].</span><span class="sxs-lookup"><span data-stu-id="03b36-112">[Azure PowerShell cmdlets][lnk-powershell-install].</span></span>

## <a name="connect-tooyour-azure-subscription"></a><span data-ttu-id="03b36-113">Ansluta tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="03b36-113">Connect tooyour Azure subscription</span></span>
<span data-ttu-id="03b36-114">Ange hello efter kommandot toosign i tooyour Azure-prenumeration i PowerShell-Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="03b36-114">In a PowerShell command prompt, enter hello following command toosign in tooyour Azure subscription:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="03b36-115">Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="03b36-115">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="03b36-116">Använd följande kommando toolist hello Azure-prenumerationer tillgängliga för dig toouse hello:</span><span class="sxs-lookup"><span data-stu-id="03b36-116">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="03b36-117">Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toocreate din IoT-hubb hello.</span><span class="sxs-lookup"><span data-stu-id="03b36-117">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="03b36-118">Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="03b36-118">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

## <a name="create-resource-group"></a><span data-ttu-id="03b36-119">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="03b36-119">Create resource group</span></span>

<span data-ttu-id="03b36-120">Du behöver en resurs grupp toodeploy en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="03b36-120">You need a resource group toodeploy an IoT hub.</span></span> <span data-ttu-id="03b36-121">Du kan använda en befintlig resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="03b36-121">You can use an existing resource group or create a new one.</span></span>

<span data-ttu-id="03b36-122">Du kan använda följande kommando toodiscover hello platser där du kan distribuera en IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="03b36-122">You can use hello following command toodiscover hello locations where you can deploy an IoT hub:</span></span>

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

<span data-ttu-id="03b36-123">toocreate en resursgrupp för din IoT-hubb hello stöds platser för IoT-hubb, Använd hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="03b36-123">toocreate a resource group for your IoT hub in one of hello supported locations for IoT Hub, use hello following command.</span></span> <span data-ttu-id="03b36-124">Det här exemplet skapar en resursgrupp med namnet **MyIoTRG1** i hello **östra USA** region:</span><span class="sxs-lookup"><span data-stu-id="03b36-124">This example creates a resource group called **MyIoTRG1** in hello **East US** region:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a><span data-ttu-id="03b36-125">Skapa en IoT Hub</span><span class="sxs-lookup"><span data-stu-id="03b36-125">Create an IoT hub</span></span>

<span data-ttu-id="03b36-126">toocreate en IoT-hubb i hello resursgrupp som du skapade i hello föregående steg, Använd hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="03b36-126">toocreate an IoT hub in hello resource group you created in hello previous step, use hello following command.</span></span> <span data-ttu-id="03b36-127">Det här exemplet skapas en **S1** hubb kallas **MyTestIoTHub** i hello **östra USA** region:</span><span class="sxs-lookup"><span data-stu-id="03b36-127">This example creates an **S1** hub called **MyTestIoTHub** in hello **East US** region:</span></span>

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

<span data-ttu-id="03b36-128">Hej IoT-hubb hello namn måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="03b36-128">hello name of hello IoT hub must be unique.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


<span data-ttu-id="03b36-129">Du kan ange alla hello IoT-hubbar i din prenumeration med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="03b36-129">You can list all hello IoT hubs in your subscription using hello following command:</span></span>

```powershell
Get-AzureRmIotHub
```

<span data-ttu-id="03b36-130">hello föregående exempel läggs en S1 Standard IoT-hubb du debiteras.</span><span class="sxs-lookup"><span data-stu-id="03b36-130">hello previous example adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="03b36-131">Du kan ta bort hello IoT-hubb med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="03b36-131">You can delete hello IoT hub using hello following command:</span></span>

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

<span data-ttu-id="03b36-132">Alternativt kan du ta bort en resursgrupp och alla hello resurser som den innehåller med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="03b36-132">Alternatively, you can remove a resource group and all hello resources it contains using hello following command:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a><span data-ttu-id="03b36-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03b36-133">Next steps</span></span>

<span data-ttu-id="03b36-134">Nu du har distribuerat en IoT-hubb med hjälp av en PowerShell-cmdlet kan behöva du ytterligare tooexplore:</span><span class="sxs-lookup"><span data-stu-id="03b36-134">Now you have deployed an IoT hub using a PowerShell cmdlet, you may want tooexplore further:</span></span>

* <span data-ttu-id="03b36-135">Identifiera andra [PowerShell-cmdlets för att arbeta med din IoT-hubb][lnk-iothub-cmdlets].</span><span class="sxs-lookup"><span data-stu-id="03b36-135">Discover other [PowerShell cmdlets for working with your IoT hub][lnk-iothub-cmdlets].</span></span>
* <span data-ttu-id="03b36-136">Läs mer om hello funktionerna i hello [IoT-hubb resursprovidern REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="03b36-136">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>

<span data-ttu-id="03b36-137">toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="03b36-137">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="03b36-138">[Introduktion tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="03b36-138">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="03b36-139">[Azure IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="03b36-139">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="03b36-140">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="03b36-140">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="03b36-141">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="03b36-141">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
