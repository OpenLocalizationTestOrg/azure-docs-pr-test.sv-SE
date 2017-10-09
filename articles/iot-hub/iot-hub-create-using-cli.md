---
title: aaaCreate en IoT-hubb med Azure CLI (az.py) | Microsoft Docs
description: Hur en Azure IoT-hubb med toocreate hello plattformsoberoende Azure CLI 2.0 (az.py).
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 9c9639235c2ac343e6ceb9578291dafaea26ea24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-cli-20"></a><span data-ttu-id="5cc37-103">Skapa en IoT-hubb med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5cc37-103">Create an IoT hub using hello Azure CLI 2.0</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="5cc37-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="5cc37-104">Introduction</span></span>

<span data-ttu-id="5cc37-105">Du kan använda Azure CLI 2.0 (az.py) toocreate och hantera Azure IoT-hubbar programmässigt.</span><span class="sxs-lookup"><span data-stu-id="5cc37-105">You can use Azure CLI 2.0 (az.py) toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="5cc37-106">Den här artikeln visar hur hello toouse Azure CLI 2.0 (az.py) toocreate en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5cc37-106">This article shows you how toouse hello Azure CLI 2.0 (az.py) toocreate an IoT hub.</span></span>

<span data-ttu-id="5cc37-107">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="5cc37-107">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="5cc37-108">[Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – hello CLI för hello klassisk och resurs management distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="5cc37-108">[Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – hello CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="5cc37-109">Azure CLI 2.0 (az.py) - hello nästa generations CLI för hello management resursdistributionsmodell som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5cc37-109">Azure CLI 2.0 (az.py) - hello next generation CLI for hello resource management deployment model as described in this article.</span></span>

<span data-ttu-id="5cc37-110">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="5cc37-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="5cc37-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="5cc37-111">An active Azure account.</span></span> <span data-ttu-id="5cc37-112">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="5cc37-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="5cc37-113">[Azure CLI 2.0][lnk-CLI-install].</span><span class="sxs-lookup"><span data-stu-id="5cc37-113">[Azure CLI 2.0][lnk-CLI-install].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="5cc37-114">Logga in och ange ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="5cc37-114">Sign in and set your Azure account</span></span>

<span data-ttu-id="5cc37-115">Logga in tooyour Azure-konto och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5cc37-115">Sign in tooyour Azure account and select your subscription.</span></span>

1. <span data-ttu-id="5cc37-116">Kör hello Kommandotolken hello [inloggningen kommandot][lnk-login-command]:</span><span class="sxs-lookup"><span data-stu-id="5cc37-116">At hello command prompt, run hello [login command][lnk-login-command]:</span></span>
    
    ```azurecli
    az login
    ```

    <span data-ttu-id="5cc37-117">Följ hello instruktioner tooauthenticate med hello kod och logga in tooyour Azure-konto via en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="5cc37-117">Follow hello instructions tooauthenticate using hello code and sign in tooyour Azure account through a web browser.</span></span>

2. <span data-ttu-id="5cc37-118">Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-konton som är associerade med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="5cc37-118">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure accounts associated with your credentials.</span></span> <span data-ttu-id="5cc37-119">Använder följande hello [kommandot toolist hello Azure konton] [ lnk-az-account-command] tillgängliga för du toouse:</span><span class="sxs-lookup"><span data-stu-id="5cc37-119">Use hello following [command toolist hello Azure accounts][lnk-az-account-command] available for you toouse:</span></span>
    
    ```azurecli
    az account list 
    ```

    <span data-ttu-id="5cc37-120">Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toocreate din IoT-hubb hello.</span><span class="sxs-lookup"><span data-stu-id="5cc37-120">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="5cc37-121">Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="5cc37-121">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a><span data-ttu-id="5cc37-122">Skapa en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="5cc37-122">Create an IoT Hub</span></span>

<span data-ttu-id="5cc37-123">Använd hello Azure CLI toocreate en resursgrupp och Lägg sedan till en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5cc37-123">Use hello Azure CLI toocreate a resource group and then add an IoT hub.</span></span>

1. <span data-ttu-id="5cc37-124">När du skapar en IoT-hubb skapar du den i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="5cc37-124">When you create an IoT hub, you must create it in a resource group.</span></span> <span data-ttu-id="5cc37-125">Använd en befintlig resursgrupp eller kör följande hello [kommandot toocreate en resursgrupp][lnk-az-resource-command]:</span><span class="sxs-lookup"><span data-stu-id="5cc37-125">Either use an existing resource group, or run hello following [command toocreate a resource group][lnk-az-resource-command]:</span></span>
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > <span data-ttu-id="5cc37-126">hello föregående exempel skapar hello resursgrupp i hello västra USA plats.</span><span class="sxs-lookup"><span data-stu-id="5cc37-126">hello previous example creates hello resource group in hello West US location.</span></span> <span data-ttu-id="5cc37-127">Du kan visa en lista över tillgängliga platser genom att köra kommandot hello `az account list-locations -o table`.</span><span class="sxs-lookup"><span data-stu-id="5cc37-127">You can view a list of available locations by running hello command `az account list-locations -o table`.</span></span>
    >
    >

2. <span data-ttu-id="5cc37-128">Kör följande hello [kommandot toocreate en IoT-hubb] [ lnk-az-iot-command] med hjälp av ett globalt unikt namn för din IoT-hubb i din resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="5cc37-128">Run hello following [command toocreate an IoT hub][lnk-az-iot-command] in your resource group, using a globally unique name for your IoT hub:</span></span>
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> <span data-ttu-id="5cc37-129">hello föregående kommando skapar en IoT-hubb i hello S1 prisnivå du debiteras.</span><span class="sxs-lookup"><span data-stu-id="5cc37-129">hello previous command creates an IoT hub in hello S1 pricing tier for which you are billed.</span></span> <span data-ttu-id="5cc37-130">Mer information finns i [Azure IoT Hub-priser][lnk-iot-pricing].</span><span class="sxs-lookup"><span data-stu-id="5cc37-130">For more information, see [Azure IoT Hub pricing][lnk-iot-pricing].</span></span>
>
>

## <a name="remove-an-iot-hub"></a><span data-ttu-id="5cc37-131">Ta bort en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="5cc37-131">Remove an IoT Hub</span></span>

<span data-ttu-id="5cc37-132">Du kan använda hello Azure CLI för[ta bort en enskild resurs][lnk-az-resource-command], till exempel en IoT-hubb eller ta bort en resursgrupp och alla dess resurser, inklusive alla IoT-hubbar.</span><span class="sxs-lookup"><span data-stu-id="5cc37-132">You can use hello Azure CLI too[delete an individual resource][lnk-az-resource-command], such as an IoT hub, or delete a resource group and all its resources, including any IoT hubs.</span></span>

<span data-ttu-id="5cc37-133">toodelete en IoT-hubb kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5cc37-133">toodelete an IoT hub, run hello following command:</span></span>

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

<span data-ttu-id="5cc37-134">toodelete en resursgrupp och alla dess resurser, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5cc37-134">toodelete a resource group and all its resources, run hello following command:</span></span>

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a><span data-ttu-id="5cc37-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5cc37-135">Next steps</span></span>
<span data-ttu-id="5cc37-136">toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="5cc37-136">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="5cc37-137">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="5cc37-137">[IoT Hub developer guide][lnk-devguide]</span></span>

<span data-ttu-id="5cc37-138">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="5cc37-138">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="5cc37-139">[Med hjälp av hello Azure portal toomanage IoT-hubb][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="5cc37-139">[Using hello Azure portal toomanage IoT Hub][lnk-portal]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
