---
title: aaaCreate en Azure IoT-hubb med en mall (PowerShell) | Microsoft Docs
description: "Hur toouse toocreate en Azure Resource Manager-mall för en IoT-hubb med PowerShell."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7eade855-c289-4ffb-b5ef-02be8c5f670f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e98ff5e898200cd727b9326fb3df393e43b021e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a><span data-ttu-id="0c05b-103">Skapa en IoT-hubb med hjälp av Azure Resource Manager-mall (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="0c05b-103">Create an IoT hub using Azure Resource Manager template (PowerShell)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="0c05b-104">Du kan använda Azure Resource Manager toocreate och hantera Azure IoT-hubbar programmässigt.</span><span class="sxs-lookup"><span data-stu-id="0c05b-104">You can use Azure Resource Manager toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="0c05b-105">De här självstudierna visar hur toouse toocreate en Azure Resource Manager-mall för en IoT-hubb med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0c05b-105">This tutorial shows you how toouse an Azure Resource Manager template toocreate an IoT hub with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="0c05b-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0c05b-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="0c05b-107">Den här artikeln täcker hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="0c05b-107">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="0c05b-108">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c05b-108">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="0c05b-109">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="0c05b-109">An active Azure account.</span></span> <br/><span data-ttu-id="0c05b-110">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="0c05b-110">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="0c05b-111">[Azure PowerShell 1.0] [ lnk-powershell-install] eller senare.</span><span class="sxs-lookup"><span data-stu-id="0c05b-111">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

> [!TIP]
> <span data-ttu-id="0c05b-112">hello artikel [med hjälp av Azure PowerShell med Azure Resource Manager] [ lnk-powershell-arm] ger mer information om hur toouse PowerShell och Azure Resource Manager-mallar toocreate Azure resurser.</span><span class="sxs-lookup"><span data-stu-id="0c05b-112">hello article [Using Azure PowerShell with Azure Resource Manager][lnk-powershell-arm] provides more information about how toouse PowerShell and Azure Resource Manager templates toocreate Azure resources.</span></span>

## <a name="connect-tooyour-azure-subscription"></a><span data-ttu-id="0c05b-113">Ansluta tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0c05b-113">Connect tooyour Azure subscription</span></span>

<span data-ttu-id="0c05b-114">Ange hello efter kommandot toosign i tooyour Azure-prenumeration i PowerShell-Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="0c05b-114">In a PowerShell command prompt, enter hello following command toosign in tooyour Azure subscription:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="0c05b-115">Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0c05b-115">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="0c05b-116">Använd följande kommando toolist hello Azure-prenumerationer tillgängliga för dig toouse hello:</span><span class="sxs-lookup"><span data-stu-id="0c05b-116">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="0c05b-117">Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toocreate din IoT-hubb hello.</span><span class="sxs-lookup"><span data-stu-id="0c05b-117">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="0c05b-118">Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="0c05b-118">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

<span data-ttu-id="0c05b-119">Du kan använda följande kommandon toodiscover där du kan distribuera en IoT-hubb och hello API-versioner som stöds för närvarande hello:</span><span class="sxs-lookup"><span data-stu-id="0c05b-119">You can use hello following commands toodiscover where you can deploy an IoT hub and hello currently supported API versions:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

<span data-ttu-id="0c05b-120">Skapa en resurs grupp toocontain din IoT-hubb med hjälp av följande kommando på någon av platserna hello stöds för IoT-hubb hello.</span><span class="sxs-lookup"><span data-stu-id="0c05b-120">Create a resource group toocontain your IoT hub using hello following command in one of hello supported locations for IoT Hub.</span></span> <span data-ttu-id="0c05b-121">Det här exemplet skapar en resursgrupp med namnet **MyIoTRG1**:</span><span class="sxs-lookup"><span data-stu-id="0c05b-121">This example creates a resource group called **MyIoTRG1**:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-toocreate-an-iot-hub"></a><span data-ttu-id="0c05b-122">Skicka en mall toocreate en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="0c05b-122">Submit a template toocreate an IoT hub</span></span>

<span data-ttu-id="0c05b-123">Använd en IoT-hubb för toocreate en JSON-mall i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0c05b-123">Use a JSON template toocreate an IoT hub in your resource group.</span></span> <span data-ttu-id="0c05b-124">Du kan också använda en Azure Resource Manager mallen toomake ändringar tooan befintliga IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0c05b-124">You can also use an Azure Resource Manager template toomake changes tooan existing IoT hub.</span></span>

1. <span data-ttu-id="0c05b-125">Använd en text editor toocreate som kallas för en Azure Resource Manager-mall **template.json** med hello följande resource definition toocreate en ny standard IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0c05b-125">Use a text editor toocreate an Azure Resource Manager template called **template.json** with hello following resource definition toocreate a new standard IoT hub.</span></span> <span data-ttu-id="0c05b-126">Det här exemplet lägger till hello IoT-hubb i hello **östra USA** region, skapar två konsumentgrupper (**cg1** och **cg2**) på hello Event Hub-kompatibel slutpunkt och använder hello **2016-02-03** API-versionen.</span><span class="sxs-lookup"><span data-stu-id="0c05b-126">This example adds hello IoT Hub in hello **East US** region, creates two consumer groups (**cg1** and **cg2**) on hello Event Hub-compatible endpoint, and uses hello **2016-02-03** API version.</span></span> <span data-ttu-id="0c05b-127">Den här mallen också förväntas du toopass i hello IoT hub-namn som en parameter med namnet **hubName**.</span><span class="sxs-lookup"><span data-stu-id="0c05b-127">This template also expects you toopass in hello IoT hub name as a parameter called **hubName**.</span></span> <span data-ttu-id="0c05b-128">Hello aktuella listan över platser som har stöd för IoT-hubb finns [Azure Status][lnk-status].</span><span class="sxs-lookup"><span data-stu-id="0c05b-128">For hello current list of locations that support IoT Hub see [Azure Status][lnk-status].</span></span>

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. <span data-ttu-id="0c05b-129">Spara filen med hello Azure Resource Manager mallen på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="0c05b-129">Save hello Azure Resource Manager template file on your local machine.</span></span> <span data-ttu-id="0c05b-130">Det här exemplet förutsätter att du sparar den i en mapp med namnet **c:\templates**.</span><span class="sxs-lookup"><span data-stu-id="0c05b-130">This example assumes you save it in a folder called **c:\templates**.</span></span>

3. <span data-ttu-id="0c05b-131">Kör följande kommando toodeploy hello din nya IoT-hubb som skickar hello namnet på din IoT-hubb som en parameter.</span><span class="sxs-lookup"><span data-stu-id="0c05b-131">Run hello following command toodeploy your new IoT hub, passing hello name of your IoT hub as a parameter.</span></span> <span data-ttu-id="0c05b-132">I det här exemplet hello hello IoT-hubb heter `abcmyiothub`.</span><span class="sxs-lookup"><span data-stu-id="0c05b-132">In this example, hello name of hello IoT hub is `abcmyiothub`.</span></span> <span data-ttu-id="0c05b-133">hello namnet på din IoT-hubb måste vara globalt unika:</span><span class="sxs-lookup"><span data-stu-id="0c05b-133">hello name of your IoT hub must be globally unique:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. <span data-ttu-id="0c05b-134">hello utdata visar hello nycklar för hello IoT hub du skapat.</span><span class="sxs-lookup"><span data-stu-id="0c05b-134">hello output displays hello keys for hello IoT hub you created.</span></span>

5. <span data-ttu-id="0c05b-135">tooverify lägga till din programmet hello ny IoT-hubb, besök hello [Azure-portalen] [ lnk-azure-portal] och visa en lista över resurser.</span><span class="sxs-lookup"><span data-stu-id="0c05b-135">tooverify your application added hello new IoT hub, visit hello [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="0c05b-136">Du kan också använda hello **Get-AzureRmResource** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0c05b-136">Alternatively, use hello **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="0c05b-137">Det här exempelprogrammet lägger till en S1 Standard IoT-hubb du debiteras.</span><span class="sxs-lookup"><span data-stu-id="0c05b-137">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="0c05b-138">Du kan ta bort hello IoT-hubb via hello [Azure-portalen] [ lnk-azure-portal] eller genom att använda hello **ta bort AzureRmResource** PowerShell-cmdlet när du är klar.</span><span class="sxs-lookup"><span data-stu-id="0c05b-138">You can delete hello IoT hub through hello [Azure portal][lnk-azure-portal] or by using hello **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c05b-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c05b-139">Next steps</span></span>

<span data-ttu-id="0c05b-140">Nu du har distribuerat en IoT-hubb med en Azure Resource Manager-mall med PowerShell kan behöva du ytterligare tooexplore:</span><span class="sxs-lookup"><span data-stu-id="0c05b-140">Now you have deployed an IoT hub using an Azure Resource Manager template with PowerShell, you may want tooexplore further:</span></span>

* <span data-ttu-id="0c05b-141">Läs mer om hello funktionerna i hello [IoT-hubb resursprovidern REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="0c05b-141">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="0c05b-142">Läs [översikt över Azure Resource Manager] [ lnk-azure-rm-overview] toolearn mer om hello funktioner i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0c05b-142">Read [Azure Resource Manager overview][lnk-azure-rm-overview] toolearn more about hello capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="0c05b-143">toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="0c05b-143">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="0c05b-144">[Introduktion tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="0c05b-144">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="0c05b-145">[Azure IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="0c05b-145">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="0c05b-146">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="0c05b-146">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="0c05b-147">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="0c05b-147">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
