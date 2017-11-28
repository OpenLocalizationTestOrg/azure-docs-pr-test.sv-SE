---
title: aaaUse hello Azure PowerShell tooconfigure ladda upp filen | Microsoft Docs
description: "Hur toouse hello Azure PowerShell-cmdlets tooconfigure din IoT-hubb tooenable filöverföringar från anslutna enheter. Innehåller information om hur du konfigurerar hello mål Azure storage-konto."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 9dcdc41693c09cece411921b30c91d7b3db47395
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a><span data-ttu-id="94885-104">Konfigurera IoT-hubb filöverföringar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="94885-104">Configure IoT Hub file uploads using PowerShell</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="94885-105">toouse hello [filen överför funktioner i IoT-hubb][lnk-upload], måste du först associera ett Azure storage-konto med din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="94885-105">toouse hello [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure storage account with your IoT hub.</span></span> <span data-ttu-id="94885-106">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="94885-106">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="94885-107">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="94885-107">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="94885-108">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="94885-108">An active Azure account.</span></span> <span data-ttu-id="94885-109">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="94885-109">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="94885-110">[Azure PowerShell-cmdlets][lnk-powershell-install].</span><span class="sxs-lookup"><span data-stu-id="94885-110">[Azure PowerShell cmdlets][lnk-powershell-install].</span></span>
* <span data-ttu-id="94885-111">Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="94885-111">An Azure IoT hub.</span></span> <span data-ttu-id="94885-112">Om du inte har en IoT-hubb kan du använda hello [cmdlet New-AzureRmIoTHub] [ lnk-powershell-iothub] toocreate en eller Använd hello portal för[skapar en IoT-hubb] [ lnk-portal-hub].</span><span class="sxs-lookup"><span data-stu-id="94885-112">If you don't have an IoT hub, you can use hello [New-AzureRmIoTHub cmdlet][lnk-powershell-iothub] toocreate one or use hello portal too[Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="94885-113">Ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="94885-113">An Azure storage account.</span></span> <span data-ttu-id="94885-114">Om du inte har ett Azure storage-konto kan du använda hello [Azure Storage PowerShell-cmdlets] [ lnk-powershell-storage] toocreate en eller Använd hello portal för[skapa ett lagringskonto] [ lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="94885-114">If you don't have an Azure storage account, you can use hello [Azure Storage PowerShell cmdlets][lnk-powershell-storage] toocreate one or use hello portal too[Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="94885-115">Logga in och ange ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="94885-115">Sign in and set your Azure account</span></span>

<span data-ttu-id="94885-116">Logga in tooyour Azure-konto och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="94885-116">Sign in tooyour Azure account and select your subscription.</span></span>

1. <span data-ttu-id="94885-117">Kör i PowerShell-Kommandotolken hello hello **Login-AzureRmAccount** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="94885-117">At hello PowerShell prompt, run hello **Login-AzureRmAccount** cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="94885-118">Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="94885-118">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="94885-119">Använd följande kommando toolist hello Azure-prenumerationer tillgängliga för dig toouse hello:</span><span class="sxs-lookup"><span data-stu-id="94885-119">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="94885-120">Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toomanage din IoT-hubb hello.</span><span class="sxs-lookup"><span data-stu-id="94885-120">Use hello following command tooselect subscription that you want toouse toorun hello commands toomanage your IoT hub.</span></span> <span data-ttu-id="94885-121">Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="94885-121">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="94885-122">Hämta information om ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="94885-122">Retrieve your storage account details</span></span>

<span data-ttu-id="94885-123">hello följande steg förutsätter att du har skapat ditt lagringskonto med hjälp av hello **Resource Manager** distributionsmodell och inte hello **klassiska** distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="94885-123">hello following steps assume that you created your storage account using hello **Resource Manager** deployment model, and not hello **Classic** deployment model.</span></span>

<span data-ttu-id="94885-124">tooconfigure filöverföringar från dina enheter måste du hello anslutningssträngen för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="94885-124">tooconfigure file uploads from your devices, you need hello connection string for an Azure storage account.</span></span> <span data-ttu-id="94885-125">hello storage-konto måste vara i hello samma prenumeration som din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="94885-125">hello storage account must be in hello same subscription as your IoT hub.</span></span> <span data-ttu-id="94885-126">Du måste också hello namnet på en blob-behållare i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="94885-126">You also need hello name of a blob container in hello storage account.</span></span> <span data-ttu-id="94885-127">Använd följande kommando tooretrieve hello dina nycklar för lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="94885-127">Use hello following command tooretrieve your storage account keys:</span></span>

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

<span data-ttu-id="94885-128">Anteckna hello **key1** nyckelvärdet för storage-konto.</span><span class="sxs-lookup"><span data-stu-id="94885-128">Make a note of hello **key1** storage account key value.</span></span> <span data-ttu-id="94885-129">Du behöver det i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="94885-129">You need it in hello following steps.</span></span>

<span data-ttu-id="94885-130">Du kan använda en befintlig blobbehållare för dina filöverföringar eller skapa nya:</span><span class="sxs-lookup"><span data-stu-id="94885-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="94885-131">toolist hello befintlig blob-behållare i ditt lagringskonto, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="94885-131">toolist hello existing blob containers in your storage account, use hello following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* <span data-ttu-id="94885-132">toocreate en blob-behållare i ditt lagringskonto, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="94885-132">toocreate a blob container in your storage account, use hello following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a><span data-ttu-id="94885-133">Konfigurera din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="94885-133">Configure your IoT hub</span></span>

<span data-ttu-id="94885-134">Du kan nu konfigurera din IoT-hubb tooenable [filen överför funktioner] [ lnk-upload] med information om ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="94885-134">You can now configure your IoT hub tooenable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="94885-135">hello konfigurationen kräver hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="94885-135">hello configuration requires hello following values:</span></span>

<span data-ttu-id="94885-136">**Lagringsbehållaren**: en blob-behållare i ett Azure storage-konto i din aktuella Azure-prenumeration tooassociate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="94885-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription tooassociate with your IoT hub.</span></span> <span data-ttu-id="94885-137">Du har hämtat hello information om nödvändiga lagringskonto i föregående avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="94885-137">You retrieved hello necessary storage account information in hello preceding section.</span></span> <span data-ttu-id="94885-138">IoT-hubb genererar automatiskt SAS URI: er med skriva behörigheter toothis blob-behållare för toouse enheter när de överför filer.</span><span class="sxs-lookup"><span data-stu-id="94885-138">IoT Hub automatically generates SAS URIs with write permissions toothis blob container for devices toouse when they upload files.</span></span>

<span data-ttu-id="94885-139">**Ta emot meddelanden för överförda filer**: aktivera eller inaktivera filen överför meddelanden.</span><span class="sxs-lookup"><span data-stu-id="94885-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="94885-140">**SAS TTL**: den här inställningen är hello time-to-live av hello SAS URI returneras toohello enhet av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="94885-140">**SAS TTL**: This setting is hello time-to-live of hello SAS URIs returned toohello device by IoT Hub.</span></span> <span data-ttu-id="94885-141">Ange tooone timme som standard.</span><span class="sxs-lookup"><span data-stu-id="94885-141">Set tooone hour by default.</span></span>

<span data-ttu-id="94885-142">**Filen notification inställningar Standardbegränsning**: hello time-to-live av en fil överför meddelande innan det förfaller.</span><span class="sxs-lookup"><span data-stu-id="94885-142">**File notification settings default TTL**: hello time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="94885-143">Ange tooone dagen som standard.</span><span class="sxs-lookup"><span data-stu-id="94885-143">Set tooone day by default.</span></span>

<span data-ttu-id="94885-144">**Meddelande leverans av maximalt antal filer**: hello gånger hello IoT-hubb försök toodeliver ett meddelande om överföringen.</span><span class="sxs-lookup"><span data-stu-id="94885-144">**File notification maximum delivery count**: hello number of times hello IoT Hub attempts toodeliver a file upload notification.</span></span> <span data-ttu-id="94885-145">Ange too10 som standard.</span><span class="sxs-lookup"><span data-stu-id="94885-145">Set too10 by default.</span></span>

<span data-ttu-id="94885-146">Använd följande PowerShell-cmdlet tooconfigure hello överför inställningar på din IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="94885-146">Use hello following PowerShell cmdlet tooconfigure hello file upload settings on your IoT hub:</span></span>

```powershell
Set-AzureRmIotHub `
    -ResourceGroupName "{your iot hub resource group}" `
    -Name "{your iot hub name}" `
    -FileUploadNotificationTtl "01:00:00" `
    -FileUploadSasUriTtl "01:00:00" `
    -EnableFileUploadNotifications $true `
    -FileUploadStorageConnectionString "DefaultEndpointsProtocol=https;AccountName={your storage account name};AccountKey={your storage account key};EndpointSuffix=core.windows.net" `
    -FileUploadContainerName "{your blob container name}" `
    -FileUploadNotificationMaxDeliveryCount 10
```

## <a name="next-steps"></a><span data-ttu-id="94885-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94885-147">Next steps</span></span>

<span data-ttu-id="94885-148">Mer information om funktionerna i hello filen överför IoT-hubb finns [överföra filer från en enhet][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="94885-148">For more information about hello file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="94885-149">Följ dessa länkar toolearn mer om hur du hanterar Azure IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="94885-149">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="94885-150">[Massinläsning hantera IoT-enheter][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="94885-150">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="94885-151">[IoT-hubb mått][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="94885-151">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="94885-152">[Åtgärder som övervakning][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="94885-152">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="94885-153">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="94885-153">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="94885-154">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="94885-154">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="94885-155">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="94885-155">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="94885-156">[Skydda din IoT-lösning från hello bakgrund][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="94885-156">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-powershell-storage]: https://docs.microsoft.com/powershell/module/azurerm.storage/
[lnk-powershell-iothub]: https://docs.microsoft.com/powershell/module/azurerm.iothub/new-azurermiothub
[lnk-portal-hub]: iot-hub-create-through-portal.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md